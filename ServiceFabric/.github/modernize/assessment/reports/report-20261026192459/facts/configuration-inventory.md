# Configuration & Externalized Settings Inventory

The workspace contains Service Fabric manifests, application parameters, publish profiles, and ARM deployment templates. Environment-specific values are partly parameterized; sensitive values are redacted in this inventory.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Application and service manifests | Service Fabric XML | `eShopModernizedMVC-ServiceFabricApp/ApplicationPackageRoot/` | Defines MVC service, endpoints, environment variables, and optional integration parameters |
| Application and service manifests | Service Fabric XML | `eShopModernizedSQL_ServiceFabricApp/ApplicationPackageRoot/` | Defines SQL container, endpoint, and required environment variables |
| Application and service manifests | Service Fabric XML | `eShopModernizedWebForms-ServiceApp/ApplicationPackageRoot/` | Defines WebForms service, endpoints, and environment variables |
| Application parameters | XML | Each application's `ApplicationParameters/` directory | Includes Cloud, Local.1Node, and Local.5Node deployment settings |
| Publish profiles | XML | Each application's `PublishProfiles/` directory | Contains local and cloud publish configurations |
| Cluster deployment | ARM JSON | `ServiceFabric-ARM-Templates-Scripts/` | Defines cluster resources, security settings, and Key Vault certificate references |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | Visual Studio/MSBuild configuration | Local development and deployment | Service Fabric packaging |
| Release | Visual Studio/MSBuild configuration | Release packaging and deployment | Service Fabric packaging |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Cloud | Publish profile and application parameter file | `ApplicationParameters/Cloud.xml`, `PublishProfiles/Cloud.xml` | Cloud endpoint, parameterized storage, telemetry, identity, and instance-count settings |
| Local.1Node | Local publish profile | `ApplicationParameters/Local.1Node.xml`, `PublishProfiles/Local.1Node.xml` | Local cluster deployment; defaults are used where parameters are omitted |
| Local.5Node | Local publish profile | `ApplicationParameters/Local.5Node.xml`, `PublishProfiles/Local.5Node.xml` | Local multi-node deployment; defaults are used where parameters are omitted |

## Properties Inventory

| Service | Property | Default/Source |
|---|---|---|
| MVC and WebForms | ConnectionString | Configured in the service manifest; credential value redacted |
| MVC and WebForms | UseMockData, UseCustomizationData | Manifest values |
| MVC and WebForms | UseAzureStorage, UseAzureActiveDirectory | Manifest defaults with cloud parameter overrides |
| MVC and WebForms | StorageConnectionString, InstrumentationKey | Cloud application parameters; values are placeholders or redacted |
| MVC and WebForms | AzureActiveDirectoryClientId, AzureActiveDirectoryTenant, PostLogoutRedirectUri | Application parameters |
| SQL | SA_PASSWORD | Sensitive manifest value; redacted |
| SQL | ACCEPT_EULA | Service manifest |
| Cluster deployment | adminPassword | ARM parameter; sensitive default value redacted |
| Cluster deployment | certificate source and thumbprint | ARM parameters; certificate is referenced through Key Vault |

## Startup Parameters & Resource Requirements

| Service | Runtime Options | Memory | Instance Count |
|---|---|---|---|
| MVC | HTTP container endpoint 5115 | Not specified | Configured for dynamic placement |
| WebForms | HTTP container endpoint 5114 | Not specified | Configured for dynamic placement |
| SQL | TCP container endpoint 1433 | Not specified | Fixed at one instance |

## Startup Dependency Chain

1. The SQL container provides the database endpoint used by the MVC and WebForms services.
2. The MVC and WebForms containers connect to SQL using its Service Fabric DNS name.
3. The manifests do not define an explicit readiness wait or database health dependency; application startup behavior could not be confirmed without the container source.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage |
|---|---|---|
| SQL service password | Database credential | Literal in service manifest; value redacted |
| MVC and WebForms database connection string | Database credential reference | Service manifests; credential value redacted |
| Cluster administrator password | Infrastructure credential | ARM parameter file; value redacted |
| Service principal client secret | Deployment credential field | Example helper contains placeholder text |
| Cluster certificate | Certificate reference | ARM template references a Key Vault secret; no private key is reproduced here |

### Secrets Provisioning Workflow

Cloud application parameters use placeholders for storage, telemetry, and identity settings. The ARM templates reference a Key Vault certificate for cluster security, but the manifests also contain sensitive SQL and cluster-password configuration. The workspace does not document a complete workflow for supplying those credentials from a managed secret store. All sensitive values are intentionally omitted from this report.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| UseMockData | Manifest value | Service manifest |
| UseCustomizationData | Manifest value | Service manifest |
| UseAzureStorage | Disabled by default in manifest | Cloud application parameter |
| UseAzureActiveDirectory | Disabled by default in manifest | Cloud application parameter |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| MVC Service Fabric project package target | .NET Framework 4.6.1 | `eShopModernizedMVC-ServiceFabricApp/packages.config` |
| SQL Service Fabric project package target | .NET Framework 4.6.1 | `eShopModernizedSQL_ServiceFabricApp/packages.config` |
| WebForms Service Fabric project package target | .NET Framework 4.6.2 | `eShopModernizedWebForms-ServiceApp/packages.config` |
| Microsoft.VisualStudio.Azure.Fabric.MSBuild | 1.6.1 | Each application's `packages.config` |
| SQL Server container | 2017 Windows Developer image | SQL service manifest |
| Service Fabric runtime | Not specified by a resolvable project build | Application and service manifests |
