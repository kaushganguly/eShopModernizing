# Configuration & Externalized Settings Inventory

The solution has a small configuration surface: two legacy project files, WinForms `App.config`, WCF `Web.config`, and debug/release transforms. No secret store, app configuration service, or runtime feature-flag framework was identified.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| WinForms application config | XML configuration | `src/eShopWinForms/App.config` | Framework runtime and local WCF client endpoint |
| WCF service config | XML configuration | `src/eShopWCFService/Web.config` | Service binding, metadata behavior, EF provider, and connection string |
| WCF transforms | XML transforms | `src/eShopWCFService/Web.Debug.config`, `Web.Release.config` | Transform the named connection string for deployment configurations |
| Project files | MSBuild | `src/eShopWinForms/eShopWinForms.csproj`, `src/eShopWCFService/eShopWCFService.csproj` | Legacy project format and Debug/Release configuration |
| NuGet package files | NuGet | `src/eShopWinForms/packages.config`, `src/eShopWCFService/packages.config` | Package versions |
| Connection override | Environment variable | `ConnectionString` | `CatalogConfiguration` prefers this value before the named config connection |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Configuration=Debug` | Debug symbols and unoptimized builds | No profile-specific package changes detected |
| Release | MSBuild `Configuration=Release` | Optimized builds; WCF XML transform applies when published | No profile-specific package changes detected |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default | .NET Framework application configuration | `App.config`, `Web.config` | Local WCF endpoint and LocalDB connection |
| WCF Debug / Release | MSBuild publish transform | `Web.Debug.config`, `Web.Release.config` | Transformed named connection string; actual environment selection depends on deployment |

## Properties Inventory

### eShopWinForms

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| WCF client endpoint address | `http://localhost:62314/CatalogService.svc` | Default | `App.config` |
| WCF client binding | `basicHttpBinding` | Default | `App.config` |
| Supported runtime | .NET Framework 4.7 | Default | `App.config` and `.csproj` |

### eShopWCFService

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `EntityModel` connection string | LocalDB `MSSQLLocalDB`, database `eShopDatabase` | Default | `Web.config` |
| `ConnectionString` | Unset; when set, overrides the named connection | Any process environment | `CatalogConfiguration.cs` |
| Entity Framework provider | `System.Data.SqlClient` | Default | `Web.config` |
| Service binding | `basicHttpBinding` | Default | `Web.config` |
| HTTP service metadata | Enabled | Default | `Web.config` |
| Directory browsing | Enabled | Default | `Web.config` |
| Compilation/runtime target | .NET Framework 4.6.1 | Default | `Web.config` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopWinForms | .NET Framework 4.7; no explicit startup parameters found | Not specified | Desktop process |
| eShopWCFService | .NET Framework 4.6.1; IIS/IIS Express hosting | Not specified | Hosting configuration dependent |

## Startup Dependency Chain

1. The WCF service host must be available before the WinForms client can complete service calls.
2. The WCF service creates `EntityModel` and depends on the configured SQL Server LocalDB connection for database operations.
3. No explicit health-check wait, retry loop, or readiness probe was found.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| None detected | — | No password, API key, or cryptographic key was found in the inspected configuration |

### Secrets Provisioning Workflow

No secret manager or provisioning workflow was identified. The connection configuration uses a local named connection, with an optional process environment variable override; no credentials are embedded in the listed connection strings.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| None detected | — | No feature-flag framework or application toggle was found |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| WinForms client | .NET Framework 4.7 | `eShopWinForms.csproj` |
| WCF service | .NET Framework 4.6.1 | `eShopWCFService.csproj` |
| Entity Framework | 6.1.3 | `packages.config` |
| Newtonsoft.Json | 6.0.4 | `packages.config` |
| Microsoft ASP.NET Web API Client | 5.2.3 | `packages.config` |
| Build format | Legacy MSBuild project files / Visual Studio solution | `.sln` and `.csproj` files |
