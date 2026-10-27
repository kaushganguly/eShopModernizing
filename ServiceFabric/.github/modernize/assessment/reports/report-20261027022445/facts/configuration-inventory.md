# Configuration & Externalized Settings Inventory

Configuration is primarily stored in Service Fabric application/service manifests and environment-specific application parameter and publish-profile XML files. The scope contains three deployment projects, with no application-level configuration files or secret-store integration for the SQL container password.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Application manifests | Service Fabric XML | Each project's `ApplicationPackageRoot/ApplicationManifest.xml` | Declares service DNS names, instance counts, port bindings, and environment overrides |
| Service manifests | Service Fabric XML | Each project's `ApplicationPackageRoot/*/ServiceManifest.xml` | Declares container images, endpoint ports, and runtime environment variables |
| Settings | Service Fabric XML | Each project's `ApplicationPackageRoot/*/Config/Settings.xml` | Empty placeholder settings files |
| Application parameters | Environment XML | Each project's `ApplicationParameters/Local.1Node.xml`, `Local.5Node.xml`, `Cloud.xml` | Local profiles set service counts; Cloud profile supplies deployment placeholders for optional cloud integrations |
| Publish profiles | Service Fabric XML | Each project's `PublishProfiles/Local.1Node.xml`, `Local.5Node.xml`, `Cloud.xml` | Select cluster connection and parameter file |
| Build dependencies | NuGet XML | Each project's `packages.config` | Declares Service Fabric MSBuild package 1.6.1 |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| `Debug|x64` | Visual Studio/MSBuild configuration | Debug packaging/deployment | Microsoft.VisualStudio.Azure.Fabric.MSBuild 1.6.1 |
| `Release|x64` | Visual Studio/MSBuild configuration | Release packaging/deployment | Microsoft.VisualStudio.Azure.Fabric.MSBuild 1.6.1 |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Local.1Node | Local publish profile | `ApplicationParameters/Local.1Node.xml` | Local cluster; MVC/Web Forms default instance counts are -1; SQL defaults to 1 |
| Local.5Node | Local publish profile | `ApplicationParameters/Local.5Node.xml` | Local multi-node cluster profile; values are profile-specific |
| Cloud | Cloud publish profile | `ApplicationParameters/Cloud.xml` | Cluster endpoint and parameterized storage, telemetry, and AAD values |

## Properties Inventory

| Service | Property Key | Default/Configured Value | Profiles | Source |
|---|---|---|---|---|
| MVC | UseMockData | False | Service manifest | MVC `ServiceManifest.xml` |
| MVC | UseCustomizationData | False | Service manifest | MVC `ServiceManifest.xml` |
| MVC | UseAzureStorage | False | Service manifest; Cloud parameter | MVC manifest and `Cloud.xml` |
| MVC | StorageConnectionString | Empty; Cloud placeholder | Service manifest; Cloud parameter | MVC manifest and `Cloud.xml` |
| MVC | AppInsightsInstrumentationKey | Empty; Cloud placeholder | Service manifest; Cloud parameter | MVC manifest and `Cloud.xml` |
| MVC | UseAzureActiveDirectory | False; Cloud placeholder | Service manifest; Cloud parameter | MVC manifest and `Cloud.xml` |
| Web Forms | UseMockData | False | Service manifest | Web Forms `ServiceManifest.xml` |
| Web Forms | UseCustomizationData | False | Service manifest | Web Forms `ServiceManifest.xml` |
| Web Forms | UseAzureStorage | False; Cloud parameter | Service manifest and `Cloud.xml` | Web Forms manifest and parameters |
| Web Forms | UseAzureActiveDirectory | False; Cloud parameter | Service manifest and `Cloud.xml` | Web Forms manifest and parameters |
| SQL | SA_PASSWORD | [MASKED] | Service manifest | SQL `ServiceManifest.xml` |
| SQL | ACCEPT_EULA | Y | Service manifest | SQL `ServiceManifest.xml` |

The MVC and Web Forms manifests also declare connection-string and identity/redirect parameters. Secret values are intentionally masked; see the security assessment for the hard-coded password findings.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| MVC | Not specified | Not specified | -1 (all available nodes) |
| Web Forms | Not specified | Not specified | -1 (all available nodes) |
| SQL | Not specified | Not specified | 1 |

| Service | Endpoint | Protocol | Container Port |
|---|---|---|---:|
| MVC | MVC_ContainerTypeEndpoint | HTTP | 80, mapped to host 5115 |
| Web Forms | WebForms_ContainerTypeEndpoint | HTTP | 80, mapped to host 5114 |
| SQL | SqlData_ContainerTypeEndpoint | TCP | 1433 |

## Startup Dependency Chain

1. Service Fabric deploys the SQL container as a separate application; its Service Fabric DNS name is `eshopmodernized.sqldata`.
2. MVC and Web Forms containers use that DNS name and TCP port 1433 in their configured SQL connection strings.
3. No explicit health-check or wait-for-ready mechanism is declared in the scoped manifests.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| SQL container SA password | Password | Literal in SQL service manifest; value redacted |
| ARM deployment administrator password | Password | Literal in an ARM test parameter file; value redacted |
| Service principal client secret | Placeholder | Placeholder text in deployment helper; not a usable credential |
| Cluster certificate | Key Vault reference | URI/reference in secured ARM parameters; no private key value in this scope |

### Secrets Provisioning Workflow

The SQL password is supplied directly as a Service Fabric container environment variable, and a test ARM parameter file contains a literal administrator password. Secured ARM templates use Key Vault certificate references, but no managed identity or secret-injection workflow for the SQL password is declared in the deployment scope. No credential values are reproduced here.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| UseMockData | False | MVC and Web Forms service-manifest environment variables |
| UseCustomizationData | False | MVC and Web Forms service-manifest environment variables |
| UseAzureStorage | False | Service manifest and application parameter overrides |
| UseAzureActiveDirectory | False | Service manifest and application parameter overrides |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| MVC Service Fabric deployment project | .NET Framework 4.6.1 | `.sfproj` |
| SQL Service Fabric deployment project | .NET Framework 4.6.1 | `.sfproj` |
| Web Forms Service Fabric deployment project | .NET Framework 4.6.2 | `.sfproj` |
| Microsoft.VisualStudio.Azure.Fabric.MSBuild | 1.6.1 | `packages.config` |
| SQL Server Windows Developer container | 2017 | SQL `ServiceManifest.xml` |
| MVC and Web Forms containers | Not specified | `ServiceManifest.xml` image names |


