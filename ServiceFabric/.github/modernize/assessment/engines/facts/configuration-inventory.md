# Configuration & Externalized Settings Inventory

The ServiceFabric directory contains application/service manifests, environment parameter files, publish profiles, and ARM cluster templates. Runtime settings include manifest-defined defaults, deployment-time parameter substitution, and several sensitive values that must not be committed in clear text.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Service Fabric solution and projects | Build/deployment | `eShopModernizedMVC_ServiceFabricApp.sln`; three `*.sfproj` files | Three application packages; Debug and Release configurations |
| Service and application manifests | Runtime/deployment | Each project's `ApplicationPackageRoot/` | Container images, DNS names, ports, environment settings, singleton partitions |
| Service settings | Runtime | `ApplicationPackageRoot/*_ContainerPkg/Config/Settings.xml` | Config packages are present; no application-specific settings are defined there |
| Application parameter files | Environment | `ApplicationParameters/Cloud.xml`, `Local.1Node.xml`, `Local.5Node.xml` per app | Cloud settings contain deployment placeholders; local files use manifest defaults |
| Publish profiles | Deployment | `PublishProfiles/Cloud.xml`, `Local.1Node.xml`, `Local.5Node.xml` | Service Fabric deployment profiles |
| ARM templates and parameter files | Infrastructure | `ServiceFabric-ARM-Templates-Scripts/` | Cluster, certificate, networking, storage, and administrator parameters |
| NuGet package manifests | Build | Each project's `packages.config` | Microsoft.VisualStudio.Azure.Fabric.MSBuild 1.6.1 |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Local build/deployment | Service Fabric MSBuild 1.6.1 |
| Release | MSBuild `Release` configuration | Release packaging/deployment | Service Fabric MSBuild 1.6.1 |
| MVC and SQL deployment projects | Project metadata | Service Fabric project schema 1.7; target .NET Framework 4.6.1 | Service Fabric MSBuild 1.6.1 |
| Web Forms deployment project | Project metadata | Service Fabric project schema 1.7; target .NET Framework 4.6.2 | Service Fabric MSBuild 1.6.1 |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Cloud | Publish with Cloud profile and matching application parameters | `PublishProfiles/Cloud.xml`, `ApplicationParameters/Cloud.xml` | App instance count, storage, telemetry, and Azure AD values; cloud parameters include tokenized placeholders |
| Local one-node | Local Service Fabric publish profile | `PublishProfiles/Local.1Node.xml`, `ApplicationParameters/Local.1Node.xml` | Uses application manifest defaults |
| Local five-node | Local Service Fabric publish profile | `PublishProfiles/Local.5Node.xml`, `ApplicationParameters/Local.5Node.xml` | Uses application manifest defaults |

## Properties Inventory

| Service/Module | Property | Default or Source | Profiles |
|---|---|---|---|
| MVC | `ConnectionString` | Static SQL connection string; credential redacted | Service manifest |
| MVC | `UseMockData`, `UseCustomizationData`, `UseAzureStorage`, `UseAzureActiveDirectory` | `False` | Service manifest |
| MVC | `StorageConnectionString`, `AppInsightsInstrumentationKey`, Azure AD client/tenant fields | Empty in service manifest; Cloud values are external placeholders | Service manifest and Cloud parameter file |
| Web Forms | `ConnectionString` | Static SQL connection string; credential redacted | Service manifest |
| Web Forms | `UseMockData`, `UseCustomizationData`, `UseAzureStorage`, `UseAzureActiveDirectory` | `False` | Service manifest |
| Web Forms | `StorageConnectionString`, `AppInsightsInstrumentationKey`, Azure AD client/tenant fields | Empty in service manifest; Cloud values are external placeholders | Service manifest and Cloud parameter file |
| SQL container | `SA_PASSWORD` | Static value in service manifest; redacted | Service manifest |
| SQL container | `ACCEPT_EULA` | `Y` | Service manifest |
| ARM cluster | `adminPassword` | Supplied through ARM parameter file/deployment | ARM template and parameter files; treat as secret |
| ARM cluster | certificate URL, thumbprint, source vault | Supplied through parameters | ARM template and parameter files |

## Startup Parameters & Resource Requirements

| Service | Runtime Options | Memory | Instance Count |
|---|---|---|---|
| MVC container | Container port 80 mapped to service port 5115 | Not specified | `-1` (Service Fabric default across nodes) |
| Web Forms container | Container port 80 mapped to service port 5114 | Not specified | `-1` (Service Fabric default across nodes) |
| SQL container | TCP port 1433 | Not specified | Cloud parameter file sets 1; manifest otherwise uses its parameter |

No CPU or memory limits are declared in the application manifests reviewed.

## Startup Dependency Chain

1. Provision the Service Fabric cluster and its network/storage/certificate resources through ARM deployment.
2. Deploy the SQL application so `eshopmodernized.sqldata:1433` is available.
3. Deploy the MVC and Web Forms applications, which reference that SQL DNS name.

The application manifests do not declare a SQL startup dependency, health-check wait, or retry chain. The deployment scripts and cluster-level readiness behavior are outside the application manifests.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| SQL service administrator password | Database credential | Static `SA_PASSWORD` environment setting in SQL service manifest; value omitted |
| MVC and Web Forms SQL connection strings | Database credential | Static manifest connection strings; credential omitted |
| ARM `adminPassword` | Cluster administrator credential | ARM parameter supplied at deployment; do not store in plain-text parameter files |
| Certificate URL and source vault parameters | Certificate reference | ARM deployment parameters reference certificate material; no certificate value reproduced |
| Storage and Azure AD values | External integration credentials/settings | Parameterized for Cloud deployment; values omitted |

### Secrets Provisioning Workflow

Cluster administrator credentials and certificate references are inputs to ARM deployment; the templates expose source-vault and certificate URL parameters. Cloud application parameters substitute external storage, telemetry, and Azure AD values into environment variables. No managed identity permissions or automated secret retrieval workflow are specified in this directory. The checked-in SQL service manifest contains a static database password, so secret provisioning is not fully externalized for that container.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `False` | MVC and Web Forms service manifests |
| `UseCustomizationData` | `False` | MVC and Web Forms service manifests |
| `UseAzureStorage` | `False` | Service manifest and Cloud application parameter |
| `UseAzureActiveDirectory` | `False` | Service manifest and Cloud application parameter |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| MVC Service Fabric deployment project | .NET Framework 4.6.1 | `.sfproj` |
| SQL Service Fabric deployment project | .NET Framework 4.6.1 | `.sfproj` |
| Web Forms Service Fabric deployment project | .NET Framework 4.6.2 | `.sfproj` |
| Service Fabric project schema | 1.7 | `.sfproj` |
| Service Fabric MSBuild package | 1.6.1 | `packages.config` |
| SQL Server container | 2017 Windows Developer | SQL service manifest |
| MVC and Web Forms images | No tag specified | Service manifests |
