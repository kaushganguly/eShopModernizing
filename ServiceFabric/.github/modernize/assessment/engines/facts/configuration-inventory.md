# Configuration & Externalized Settings Inventory

The ServiceFabric subtree includes application/service manifests, local and cloud parameter profiles, publish profiles, and ARM templates. Sensitive values are masked below; application source configuration is outside this workspace.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Service Fabric solution | Build configuration | `D:\a\eShopModernizing\eShopModernizing\ServiceFabric\eShopModernizedMVC_ServiceFabricApp.sln` | Three `.sfproj` deployment projects; Debug/Release x64 configurations |
| Application manifests | Service Fabric deployment configuration | `D:\a\eShopModernizing\eShopModernizing\ServiceFabric\eShopModernizedMVC-ServiceFabricApp\ApplicationPackageRoot\ApplicationManifest.xml`; `D:\a\eShopModernizing\eShopModernizing\ServiceFabric\eShopModernizedSQL_ServiceFabricApp\ApplicationPackageRoot\ApplicationManifest.xml`; `D:\a\eShopModernizing\eShopModernizing\ServiceFabric\eShopModernizedWebForms-ServiceApp\ApplicationPackageRoot\ApplicationManifest.xml` | Application parameters, environment-variable bindings, services and instance counts |
| Service manifests | Container runtime configuration | `D:\a\eShopModernizing\eShopModernizing\ServiceFabric\eShopModernizedMVC-ServiceFabricApp\ApplicationPackageRoot\MVC_ContainerPkg\ServiceManifest.xml`; `D:\a\eShopModernizing\eShopModernizing\ServiceFabric\eShopModernizedSQL_ServiceFabricApp\ApplicationPackageRoot\SQL_ContainerPkg\ServiceManifest.xml`; `D:\a\eShopModernizing\eShopModernizing\ServiceFabric\eShopModernizedWebForms-ServiceApp\ApplicationPackageRoot\WebForms_ContainerPkg\ServiceManifest.xml` | Image names, environment variables, and HTTP/TCP endpoints |
| Settings files | Service Fabric settings | `D:\a\eShopModernizing\eShopModernizing\ServiceFabric\<application>\ApplicationPackageRoot\<container>\Config\Settings.xml` | Present for all three container packages; no application-specific settings identified |
| Application parameters | Runtime environment profiles | `D:\a\eShopModernizing\eShopModernizing\ServiceFabric\<application>\ApplicationParameters\Cloud.xml`, `Local.1Node.xml`, `Local.5Node.xml` | Cloud and local deployment overrides |
| Publish profiles | Publish/deployment profiles | `D:\a\eShopModernizing\eShopModernizing\ServiceFabric\<application>\PublishProfiles\Cloud.xml`, `Local.1Node.xml`, `Local.5Node.xml` | Present for each application |
| NuGet package manifests | Build dependencies | `D:\a\eShopModernizing\eShopModernizing\ServiceFabric\<application>\packages.config` | Service Fabric MSBuild package 1.6.1 |
| ARM templates and parameter files | Cluster infrastructure | `D:\a\eShopModernizing\eShopModernizing\ServiceFabric\ServiceFabric-ARM-Templates-Scripts\` | Includes cluster configuration and masked admin-password parameter values; some templates reference Key Vault certificates |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | Visual Studio/MSBuild solution configuration, x64 | Development build/deploy configuration | Service Fabric MSBuild 1.6.1 |
| Release | Visual Studio/MSBuild solution configuration, x64 | Release build/deploy configuration | Service Fabric MSBuild 1.6.1 |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Cloud | Publish profile selection | `ApplicationParameters\Cloud.xml`, `PublishProfiles\Cloud.xml` under each application | Cloud deployment parameters; sensitive values masked |
| Local.1Node | Publish profile selection | `ApplicationParameters\Local.1Node.xml`, `PublishProfiles\Local.1Node.xml` under each application | Single-node local cluster settings |
| Local.5Node | Publish profile selection | `ApplicationParameters\Local.5Node.xml`, `PublishProfiles\Local.5Node.xml` under each application | Five-node local cluster settings |

## Properties Inventory

| Service | Property | Default/Value | Source |
|---|---|---|---|
| MVC container | `UseMockData` | `False` | `MVC_ContainerPkg\ServiceManifest.xml` |
| MVC container | `UseCustomizationData` | `False` | `MVC_ContainerPkg\ServiceManifest.xml` |
| MVC container | `UseAzureStorage` | `False` | Application and service manifests |
| MVC container | `ConnectionString` | `[MASKED: parameterized]` | `MVC_ContainerPkg\ServiceManifest.xml` |
| MVC container | `StorageConnectionString` | Not set | `MVC_ContainerPkg\ServiceManifest.xml` |
| MVC container | `UseAzureActiveDirectory` | `False` | Application and service manifests |
| MVC container | `AppInsightsInstrumentationKey` | Not set by default | Application and service manifests |
| MVC container | `PostLogoutRedirectUri` | `10.0.75.1:5115` | `MVC_ContainerPkg\ServiceManifest.xml` |
| Web Forms container | `UseMockData` | `False` | `WebForms_ContainerPkg\ServiceManifest.xml` |
| Web Forms container | `UseCustomizationData` | `False` | `WebForms_ContainerPkg\ServiceManifest.xml` |
| Web Forms container | `UseAzureStorage` | `False` | Application and service manifests |
| Web Forms container | `ConnectionString` | `[MASKED: parameterized]` | `WebForms_ContainerPkg\ServiceManifest.xml` |
| Web Forms container | `StorageConnectionString` | Not set | `WebForms_ContainerPkg\ServiceManifest.xml` |
| Web Forms container | `UseAzureActiveDirectory` | `False` | Application and service manifests |
| Web Forms container | `AppInsightsInstrumentationKey` | Not set by default | Application and service manifests |
| Web Forms container | `PostLogoutRedirectUri` | `10.0.75.1:5114` | `WebForms_ContainerPkg\ServiceManifest.xml` |
| SQL container | `SA_PASSWORD` | `[MASKED: literal value present]` | `SQL_ContainerPkg\ServiceManifest.xml` |
| SQL container | `ACCEPT_EULA` | `Y` | `SQL_ContainerPkg\ServiceManifest.xml` |

## Startup Parameters & Resource Requirements

| Service | Runtime Options | Memory | Instance Count |
|---|---|---|---|
| MVC container | HTTP endpoint 5115 | Not specified | `-1` (all nodes) by default |
| Web Forms container | HTTP endpoint 5114 | Not specified | `-1` (all nodes) by default |
| SQL container | TCP endpoint 1433 | Not specified | `1` by default |

## Startup Dependency Chain

The MVC and Web Forms services are configured with database connection strings for the SQL service. No explicit readiness probe, startup wait, or retry policy is identified in the Service Fabric manifests. The deployment artifacts do not establish a guaranteed startup order.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| SQL container `SA_PASSWORD` | SQL administrator password | Literal value in `SQL_ContainerPkg\ServiceManifest.xml`; `[MASKED]` |
| ARM `adminPassword` | Cluster VM administrator password | Non-empty literal values in checked-in ARM parameter files; `[MASKED]` |
| MVC/Web Forms database connection strings | Database credentials/configuration | Bound through application parameters; actual value not reproduced |
| Cluster certificates | Certificate secrets | ARM templates include Key Vault secret references; no secret values reproduced |
| Deployment helper service principal fields | Client credentials | Placeholder strings in helper source; no usable credential identified |

### Secrets Provisioning Workflow

The SQL container manifest and several ARM parameter files contain literal sensitive configuration, so the current deployment artifacts do not demonstrate an end-to-end secret-injection mechanism for those values. ARM templates include Key Vault references for certificate material, but the selected Service Fabric app manifests do not show managed identity or a Key Vault binding for the SQL password. Values are intentionally masked.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `False` | Service/application parameters |
| `UseCustomizationData` | `False` | Service/application parameters |
| `UseAzureStorage` | `False` | Application parameters |
| `UseAzureActiveDirectory` | `False` | Application parameters |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| MVC deployment package target framework | .NET Framework 4.6.1 | `eShopModernizedMVC-ServiceFabricApp\packages.config` |
| SQL deployment package target framework | .NET Framework 4.6.1 | `eShopModernizedSQL_ServiceFabricApp\packages.config` |
| Web Forms deployment package target framework | .NET Framework 4.6.2 | `eShopModernizedWebForms-ServiceApp\packages.config` |
| Service Fabric MSBuild package | 1.6.1 | Three `packages.config` files |
| SQL Server Windows Developer container | 2017 | `SQL_ContainerPkg\ServiceManifest.xml` |
| MVC/Web Forms container application runtime | Not specified | Application source and image build files are outside this workspace |
