# Configuration & Externalized Settings Inventory

The scoped solution externalizes deployment parameters through Service Fabric application manifests, parameter files, and publish profiles. It also contains a literal SQL administrator password in a service manifest; the value is intentionally redacted in this inventory.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Application manifests | Service Fabric configuration | `*/ApplicationPackageRoot/ApplicationManifest.xml` | Service references, instance counts, and parameter defaults |
| Service manifests | Container configuration | `*/ApplicationPackageRoot/*/ServiceManifest.xml` | Images, ports, and environment variables |
| Service settings | Service Fabric settings | `*/ApplicationPackageRoot/*/Config/Settings.xml` | Present but no settings entries identified |
| Application parameter files | Environment/deployment parameters | `*/ApplicationParameters/{Cloud,Local.1Node,Local.5Node}.xml` | Values vary by deployment profile |
| Publish profiles | Deployment connection and upgrade settings | `*/PublishProfiles/{Cloud,Local.1Node,Local.5Node}.xml` | Service Fabric cluster deployment profiles |
| Deployment scripts | PowerShell | `*/Scripts/Deploy-FabricApplication.ps1` | Publishes packaged application to a cluster |
| ARM templates | Infrastructure deployment | `ServiceFabric-ARM-Templates-Scripts/` | Cluster template and parameter files |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug x64 | MSBuild configuration | Development packaging | Service Fabric project targets |
| Release x64 | MSBuild configuration | Release packaging | Service Fabric project targets |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Cloud | Publish profile and application parameter file | `PublishProfiles/Cloud.xml`, `ApplicationParameters/Cloud.xml` | Cluster connection and service parameters |
| Local 1-node | Publish profile and application parameter file | `PublishProfiles/Local.1Node.xml`, `ApplicationParameters/Local.1Node.xml` | Local cluster and instance settings |
| Local 5-node | Publish profile and application parameter file | `PublishProfiles/Local.5Node.xml`, `ApplicationParameters/Local.5Node.xml` | Local cluster and instance settings |

## Properties Inventory

| Service | Property Key | Default/Configured Value | Profiles | Source |
|---|---|---|---|---|
| MVC | `UseMockData` | `False` | Manifest default | MVC service manifest |
| MVC | `UseCustomizationData` | `False` | Manifest default | MVC service manifest |
| MVC | `UseAzureStorage` | `False` | Manifest default | MVC service manifest |
| MVC | `UseAzureActiveDirectory` | `False` | Manifest default | MVC service manifest |
| MVC | `ConnectionString` | SQL service reference; credentials `[MASKED]` | Manifest default | MVC service manifest |
| Web Forms | `UseMockData` | `False` | Manifest default | Web Forms service manifest |
| Web Forms | `UseCustomizationData` | `False` | Manifest default | Web Forms service manifest |
| Web Forms | `UseAzureStorage` | `False` | Manifest default | Web Forms service manifest |
| Web Forms | `UseAzureActiveDirectory` | `False` | Manifest default | Web Forms service manifest |
| Web Forms | `ConnectionString` | SQL service reference; credentials `[MASKED]` | Manifest default | Web Forms service manifest |
| SQL | `SA_PASSWORD` | `[MASKED]` | Manifest default | SQL service manifest |
| SQL | `ACCEPT_EULA` | `Y` | Manifest default | SQL service manifest |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| MVC container | No startup options found | Not specified | `-1` (manifest default) |
| Web Forms container | No startup options found | Not specified | `-1` (manifest default) |
| SQL container | No startup options found | Not specified | `1` |

## Startup Dependency Chain

1. The SQL container must be available for database-backed MVC and Web Forms operations.
2. No explicit readiness probe, startup wait, or retry configuration is declared in the scoped manifests.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| SQL Server SA password | Database credential | Literal environment variable in SQL service manifest; `[MASKED]` |
| MVC and Web Forms connection strings | Database connection string | Manifest environment variables; credentials `[MASKED]` |

### Secrets Provisioning Workflow

The scoped manifests configure the SQL administrator password and application database connection strings directly. No Key Vault, managed identity, encrypted secret reference, or other external secret provisioning workflow is visible in this workspace. The password value is omitted.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `False` | Service manifest environment variable |
| `UseCustomizationData` | `False` | Service manifest environment variable |
| `UseAzureStorage` | `False` | Service manifest and application parameters |
| `UseAzureActiveDirectory` | `False` | Service manifest and application parameters |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| MVC Service Fabric deployment project | .NET Framework 4.6.1 metadata | `.sfproj` |
| SQL Service Fabric deployment project | .NET Framework 4.6.1 metadata | `.sfproj` |
| Web Forms Service Fabric deployment project | .NET Framework 4.6.2 metadata | `.sfproj` |
| Service Fabric MSBuild package | 1.6.1 | `packages.config` |
| SQL Server Developer container | 2017 | SQL service manifest |
| MVC and Web Forms container images | Version not specified | Service manifests |
