# Configuration & Externalized Settings Inventory

Configuration is split between classic ASP.NET XML configuration and JSON/launch-profile settings for the ported application. Both define local SQL Server LocalDB usage and a mock-data toggle; no external secret store or deployment resource limits were identified.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| ASP.NET application settings and connection strings | XML | `src/eShopLegacyMVC/Web.config` | Includes LocalDB connection, framework behavior, session mode, and mock/customization toggles |
| Debug transform | XML transform | `src/eShopLegacyMVC/Web.Debug.config` | Debug-specific connection-string transformation |
| Release transform | XML transform | `src/eShopLegacyMVC/Web.Release.config` | Release-specific connection-string transformation |
| Telemetry configuration | XML | `src/eShopLegacyMVC/ApplicationInsights.config` | Application Insights module/configuration |
| Logging configuration | XML | `src/eShopLegacyMVC/log4Net.xml` | log4net appenders and logging behavior |
| Application configuration | XML | `eShopPorted/app.config` | .NET application configuration source |
| Default settings | JSON | `eShopPorted/appsettings.json` | LocalDB, logging defaults, allowed hosts, and feature toggles |
| Development overrides | JSON | `eShopPorted/appsettings.Development.json` | Development logging overrides |
| Local run profiles | JSON | `eShopPorted/Properties/launchSettings.json` | IIS Express and project URL settings; sets `ASPNETCORE_ENVIRONMENT=Development` |
| Package and framework declarations | XML | Project `.csproj` and `packages.config` files | Target frameworks and NuGet dependency versions |

No remote configuration server or Key Vault/Vault/AWS Secrets Manager reference was identified.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | Visual Studio/MSBuild configuration | Development compilation and debug symbols | Legacy project defines DEBUG and TRACE |
| Release | Visual Studio/MSBuild configuration | Optimized release output | Legacy project enables optimization and emits PDB-only debug information |
| eShopPorted project | SDK-style MSBuild project | Builds ASP.NET Core application targeting net461 | ASP.NET Core 2.2 and EF Core 2.2.6 |

No custom package-selection build profile or conditional compilation profile was identified.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Legacy default | ASP.NET application startup | `Web.config` | Mock data and customization data disabled; LocalDB context initialization is enabled |
| Ported default | ASP.NET Core configuration defaults | `appsettings.json` | Mock data and customization data enabled; LocalDB connection is configured |
| Development | `ASPNETCORE_ENVIRONMENT=Development` in launch profiles | `appsettings.Development.json`, `launchSettings.json` | Increases Microsoft/System log levels; IIS Express and project URL profiles |

## Properties Inventory

### eShopLegacyMVC

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `ConnectionStrings.CatalogDBContext` | LocalDB catalog database with integrated authentication | Debug/Release transforms can replace connection information | `Web.config`, `Web.Debug.config`, `Web.Release.config` |
| `UseMockData` | `false` | Default | `Web.config` |
| `UseCustomizationData` | `false` | Default | `Web.config` |
| `webpages:Enabled` | `false` | Default | `Web.config` |
| `ClientValidationEnabled` | `true` | Default | `Web.config` |
| `UnobtrusiveJavaScriptEnabled` | `true` | Default | `Web.config` |
| `compilation.debug` | `true` | Default | `Web.config` |
| `sessionState.mode` | `InProc` | Default | `Web.config` |
| `globalization.culture` / `uiCulture` | `en-US` | Default | `Web.config` |

### eShopPorted

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `ConnectionStrings.DefaultConnection` | LocalDB database `eShopPorted`, integrated authentication | Development inherits default | `appsettings.json` |
| `UseMockData` | `true` | Default | `appsettings.json` |
| `UseCustomizationData` | `true` | Default | `appsettings.json` |
| `AllowedHosts` | `*` | Default | `appsettings.json` |
| `Logging.LogLevel.Default` | `Warning` | Development overrides to `Debug` | `appsettings.json`, `appsettings.Development.json` |
| `Logging.LogLevel.System` | Not explicitly set | Development: `Information` | `appsettings.Development.json` |
| `Logging.LogLevel.Microsoft` | Not explicitly set | Development: `Information` | `appsettings.Development.json` |
| `ASPNETCORE_ENVIRONMENT` | `Development` in local launch profiles | IIS Express and eShopPorted profiles | `Properties/launchSettings.json` |

## Startup Parameters & Resource Requirements

| Service | Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopLegacyMVC | IIS/IIS Express; classic ASP.NET configuration | Not specified | Not specified |
| eShopPorted | Local project profile exposes HTTP 5000 and HTTPS 5001; IIS Express profile uses HTTP 52637 and SSL 44337 | Not specified | Not specified |

No explicit JVM/runtime heap, CPU, memory, or replica limits were found.

## Startup Dependency Chain

1. `eShopLegacyMVC` registers Autofac, routes, filters, and bundles during `Application_Start`; when `UseMockData=false`, it configures the EF6 catalog initializer and subsequently depends on LocalDB.
2. `eShopPorted` configures MVC and Autofac at startup; it registers `CatalogDBContext` only when `UseMockData=false`, after which catalog operations depend on LocalDB.
3. Neither application declares a database readiness probe, retry/wait mechanism, health-check endpoint, or external service startup dependency.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| LocalDB connection settings | Database connection | Uses integrated Windows authentication; no password value |
| Application Insights configuration | Telemetry configuration | Configuration source exists; no credential value is reproduced here |

No password, API key, or external secret-store reference was identified in the application configuration. Connection details are local development settings, not secret values.

### Secrets Provisioning Workflow

No secret-provisioning workflow, managed identity, service principal, Key Vault binding, or deployment secret injection was identified. SQL access uses integrated Windows authentication for the configured LocalDB instances.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `false` in legacy; `true` in ported | `Web.config` / `appsettings.json` |
| `UseCustomizationData` | `false` in legacy; `true` in ported | `Web.config` / `appsettings.json` |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Legacy target framework | .NET Framework 4.7.2 | `eShopLegacyMVC.csproj` |
| Ported target framework | .NET Framework 4.6.1 | `eShopPorted.csproj` |
| Legacy ASP.NET MVC | 5.2.7 | NuGet package declarations |
| Legacy ASP.NET Web API | 5.2.7 | NuGet package declarations |
| Ported ASP.NET Core MVC | 2.2.0 | `eShopPorted.csproj` |
| Legacy Entity Framework | 6.2.0 | NuGet package declarations |
| Ported Entity Framework Core | 2.2.6 | `eShopPorted.csproj` |
| Autofac | 6.1.0 legacy; 4.9.1 ported | Project package declarations |
| SQL provider | System.Data.SqlClient / EF Core SQL Server 2.2.6 | Project configuration and package declarations |
| Build format | Legacy MSBuild and SDK-style project | Project files |
