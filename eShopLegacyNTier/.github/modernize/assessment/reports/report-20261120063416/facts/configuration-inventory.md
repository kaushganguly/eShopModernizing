# Configuration & Externalized Settings Inventory

Configuration is held in two runtime configuration files, two WCF environment-transform files, project build properties, and NuGet manifests. No secret store or active feature-flag system was detected.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| WCF service configuration | XML | `src/eShopWCFService/Web.config` | Service hosting, WCF binding, Entity Framework provider, and database connection |
| WCF Debug transform | XML transform | `src/eShopWCFService/Web.Debug.config` | Contains sample transformation content in comments; no active override |
| WCF Release transform | XML transform | `src/eShopWCFService/Web.Release.config` | Contains sample transformation content in comments; no active override |
| Windows Forms configuration | XML | `src/eShopWinForms/App.config` | Runtime version and WCF client endpoint |
| Project build configuration | MSBuild | `src/eShopWCFService/eShopWCFService.csproj`, `src/eShopWinForms/eShopWinForms.csproj` | Framework target, references, and Debug/Release build configurations |
| NuGet package manifests | XML | Both projects' `packages.config` | Declared package versions |
| Environment variable | Process environment | `ConnectionString` | Optional WCF service connection override |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild configuration | Development build; WCF Debug transform contains only commented examples | Same declared packages as Release |
| Release | MSBuild configuration | Release build; removes the debug compilation attribute | Same declared packages as Debug |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default | No profile selector detected | WCF `Web.config`; client `App.config` | LocalDB connection and loopback WCF endpoint |
| Environment override | `ConnectionString` environment variable | WCF `Web.config` plus process environment | Environment value replaces the named EF connection configuration |

The Debug and Release transform files contain commented examples and do not currently define active database overrides.

## Properties Inventory

### eShopWCFService

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `aspnet:UseTaskFriendlySynchronizationContext` | `true` | Default | `Web.config` |
| `system.web/compilation/@targetFramework` | `4.6.1` | Default | `Web.config` |
| `system.web/httpRuntime/@targetFramework` | `4.6.1` | Default | `Web.config` |
| `serviceMetadata/@httpGetEnabled` | `true` | Default | `Web.config` |
| `serviceMetadata/@httpsGetEnabled` | `true` | Default | `Web.config` |
| `serviceDebug/@includeExceptionDetailInFaults` | `false` | Default | `Web.config` |
| `directoryBrowse/@enabled` | `true` | Default | `Web.config` |
| `entityFramework/defaultConnectionFactory/parameters/parameter` | `mssqllocaldb` | Default | `Web.config` |
| `connectionStrings/EntityModel` | SQL Server LocalDB database | Default | `Web.config` |
| `ConnectionString` | Unset | Environment override | Process environment; takes precedence in `CatalogConfiguration` |

### eShopWinForms

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `startup/supportedRuntime/@sku` | `.NETFramework,Version=v4.7` | Default | `App.config` |
| `system.serviceModel/client/endpoint/@address` | Loopback WCF service address | Default | `App.config` |
| `system.serviceModel/client/endpoint/@binding` | `basicHttpBinding` | Default | `App.config` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopWinForms | .NET Framework 4.7 runtime; no custom startup arguments detected | Not specified | User-launched desktop process |
| eShopWCFService | .NET Framework 4.6.1; hosted by IIS/IIS Express | Not specified | Host-defined |

No Docker, Kubernetes, JVM, or explicit CPU/memory allocation settings were found.

## Startup Dependency Chain

1. The WCF service must be hosted and reachable at the client-configured endpoint before the Windows Forms client makes service calls.
2. The WCF service initializes Entity Framework and uses the configured SQL Server endpoint when a request accesses data.

No readiness probe, service wait mechanism, or startup timeout was detected.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `ConnectionString` | Database connection configuration | Environment variable; no credential value is committed |
| `EntityModel` | Database connection string | LocalDB configured with integrated local context; no password is present |

### Secrets Provisioning Workflow

No secret-management integration, encryption, or provisioning workflow was detected. The service optionally reads its database connection string from the process environment; deployment is responsible for setting a suitable value.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| None detected | — | No feature-flag framework or application toggle was identified |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| eShopWCFService target framework | .NET Framework 4.6.1 | `eShopWCFService.csproj` |
| eShopWinForms target framework | .NET Framework 4.7 | `eShopWinForms.csproj` |
| Entity Framework | 6.1.3 | `packages.config` |
| Microsoft.AspNet.WebApi.Client | 5.2.3 | `packages.config` |
| Newtonsoft.Json | 6.0.4 | `packages.config` |
| Build configurations | Debug, Release | MSBuild project files |
