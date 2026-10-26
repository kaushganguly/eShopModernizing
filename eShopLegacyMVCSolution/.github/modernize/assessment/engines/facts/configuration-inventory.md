# Configuration & Externalized Settings Inventory

Configuration is split across legacy XML configuration and the ported application's JSON settings and launch profiles. No external configuration service or managed secret store was identified.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Application configuration | XML | `src/eShopLegacyMVC/Web.config` | Connection string, app settings, compilation, framework configuration |
| Environment transforms | XML transforms | `src/eShopLegacyMVC/Web.Debug.config`, `Web.Release.config` | Build/deployment transforms |
| Views configuration | XML | `src/eShopLegacyMVC/Views/Web.config` | MVC view compilation and Razor settings |
| Telemetry configuration | XML | `src/eShopLegacyMVC/ApplicationInsights.config` | Application Insights settings |
| Ported app settings | JSON | `eShopPorted/appsettings.json` | Connection string, logging, mock/customization settings |
| Ported development settings | JSON | `eShopPorted/appsettings.Development.json` | Development logging overrides |
| Launch profiles | JSON | `eShopPorted/Properties/launchSettings.json` | Local ports and `ASPNETCORE_ENVIRONMENT` |
| Library configuration | XML | `eShopPorted/app.config` | Legacy application configuration file |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Local development and debugging | Debug symbols; optimization disabled |
| Release | MSBuild `Release` configuration | Optimized build | Optimizations enabled; PDB-only debug symbols |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Legacy default | IIS / ASP.NET configuration | `Web.config` | Uses SQL Server LocalDB; `UseMockData=false` |
| Ported Development | `ASPNETCORE_ENVIRONMENT=Development` in launch settings | `appsettings.json`, `appsettings.Development.json` | More verbose logging; localhost ports 5000 and 5001 |
| Ported non-development | Host environment | `appsettings.json` | Base logging level is Warning; no environment-specific production file found |

## Properties Inventory

### eShopLegacyMVC

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `CatalogDBContext` connection | LocalDB catalog database; integrated authentication | Default | `Web.config` |
| `UseMockData` | `false` | Default | `Web.config` |
| `UseCustomizationData` | `false` | Default | `Web.config` |
| `webpages:Enabled` | `false` | Default | `Web.config` |
| `ClientValidationEnabled` | `true` | Default | `Web.config` |
| `UnobtrusiveJavaScriptEnabled` | `true` | Default | `Web.config` |
| Compilation target | .NET Framework 4.7.2 | Debug / Release transforms | `Web.config` |

### eShopPorted

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `ConnectionStrings:DefaultConnection` | LocalDB `eShopPorted`; trusted connection | Base | `appsettings.json` |
| `UseMockData` | `true` | Base | `appsettings.json` |
| `UseCustomizationData` | `true` | Base | `appsettings.json` |
| `Logging:LogLevel:Default` | `Warning` | Base | `appsettings.json` |
| `Logging:LogLevel:Default` | `Debug` | Development | `appsettings.Development.json` |
| `Logging:LogLevel:System` | `Information` | Development | `appsettings.Development.json` |
| `Logging:LogLevel:Microsoft` | `Information` | Development | `appsettings.Development.json` |
| `AllowedHosts` | `*` | Base | `appsettings.json` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopLegacyMVC | IIS-hosted ASP.NET; no explicit process parameters found | Not specified | Not specified |
| eShopPorted | ASP.NET Core host; development URLs HTTP 5000 and HTTPS 5001 | Not specified | Not specified |

## Startup Dependency Chain

1. The legacy ASP.NET application initializes its MVC, Web API, dependency injection, and EF6 components in the application startup path.
2. The ported host builds the ASP.NET Core pipeline and registers Autofac and MVC services.
3. If ported mock data is disabled, the application configures `CatalogDBContext` using `DefaultConnection`; no explicit database readiness wait was identified.

No container health checks, startup wait scripts, or explicit startup timeout settings were found.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| LocalDB connection strings | Database connection | Integrated/trusted authentication; no password stored |
| Application Insights configuration | Telemetry | No credential value reported here |

No hard-coded password, API key, or cryptographic key was identified in the reviewed configuration.

### Secrets Provisioning Workflow

No Key Vault, Vault, AWS Secrets Manager, managed identity, or deployment-time secret binding was found. The checked-in LocalDB connection strings use trusted authentication and contain no password.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `false` in legacy; `true` in ported | `Web.config` or `appsettings.json` |
| `UseCustomizationData` | `false` in legacy; `true` in ported | `Web.config` or `appsettings.json` |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| eShopLegacyMVC target framework | .NET Framework 4.6.1; Web.config compilation target 4.7.2 | `eShopLegacyMVC.csproj`, `Web.config` |
| eShopPorted target framework | .NET Framework 4.6.1 | `eShopPorted.csproj` |
| eShopLegacy.Utilities target framework | .NET Framework 4.6.1 | `eShopLegacy.Utilities.csproj` |
| ASP.NET MVC | 5.2.7 | `packages.config` |
| ASP.NET Core / MVC | 2.2.0 | `eShopPorted.csproj` |
| Entity Framework / EF Core | 6.2.0 / 2.2.6 | Package manifests |
| Autofac | 4.9.1 | Package manifests |
| Build configurations | Debug and Release | Project files |
