# Configuration & Externalized Settings Inventory

The solution has separate XML-based configuration for the legacy application and JSON/launch-profile configuration for the ported application. Local SQL Server LocalDB connection strings are declared in source configuration; no external secret store or environment-variable secret workflow was identified.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web.config | Runtime configuration | `src/eShopLegacyMVC/Web.config` | Connection strings, application settings, runtime bindings, and ASP.NET settings |
| Web transforms | Environment/build transforms | `src/eShopLegacyMVC/Web.Debug.config`, `Web.Release.config` | Web deployment configuration transforms |
| Application Insights config | Telemetry configuration | `src/eShopLegacyMVC/ApplicationInsights.config` | Application Insights settings |
| log4net config | Logging configuration | `src/eShopLegacyMVC/log4Net.xml` | Logging configuration file |
| appsettings.json | Runtime configuration | `eShopPorted/appsettings.json` | Default settings and LocalDB connection |
| appsettings.Development.json | Environment configuration | `eShopPorted/appsettings.Development.json` | Development logging overrides |
| launchSettings.json | Local launch profiles | `eShopPorted/Properties/launchSettings.json` | IIS Express and project URLs, development environment |
| app.config | Runtime configuration | `eShopPorted/app.config` | Legacy-style application configuration file |
| Project files | Build/package configuration | Three `.csproj` files | Target frameworks, dependencies, build properties |
| Solution file | Build configuration | `eShopLegacyMVC.sln` | Debug and Release solution configurations |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration; default for the legacy project | Debug symbols and unoptimized build | `DEBUG;TRACE` constants |
| Release | MSBuild `Release` configuration | Release build configuration | Optimized build settings |

No additional build profiles or conditional dependency sets were identified.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Legacy default | ASP.NET application startup | `src/eShopLegacyMVC/Web.config` | `UseMockData=false`, `UseCustomizationData=false`; LocalDB connection |
| Ported Development | `ASPNETCORE_ENVIRONMENT=Development` in launch profiles | `eShopPorted/appsettings.json`, `appsettings.Development.json` | Development logging levels; default settings remain applicable |

No production-specific appsettings file or runtime profile composition was found.

## Properties Inventory

### eShopLegacyMVC

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `connectionStrings.CatalogDBContext` | LocalDB catalog connection; no password | Default | `Web.config` |
| `UseMockData` | `false` | Default | `Web.config` |
| `UseCustomizationData` | `false` | Default | `Web.config` |
| `webpages:Version` | `3.0.0.0` | Default | `Web.config` |
| `webpages:Enabled` | `false` | Default | `Web.config` |
| `ClientValidationEnabled` | `true` | Default | `Web.config` |
| `UnobtrusiveJavaScriptEnabled` | `true` | Default | `Web.config` |
| `system.web.compilation.debug` | `true` | Default | `Web.config` |
| `system.web.compilation.targetFramework` | `4.7.2` | Default | `Web.config` |
| `system.web.httpRuntime.targetFramework` | `4.6.1` | Default | `Web.config` |
| `system.web.sessionState.mode` | `InProc` | Default | `Web.config` |

### eShopPorted

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `ConnectionStrings:DefaultConnection` | LocalDB eShopPorted connection; no password | Default | `appsettings.json` |
| `Logging:LogLevel:Default` | `Warning` | Default | `appsettings.json` |
| `Logging:LogLevel:Default` | `Debug` | Development | `appsettings.Development.json` |
| `Logging:LogLevel:System` | `Information` | Development | `appsettings.Development.json` |
| `Logging:LogLevel:Microsoft` | `Information` | Development | `appsettings.Development.json` |
| `AllowedHosts` | `*` | Default | `appsettings.json` |
| `UseMockData` | `true` | Default | `appsettings.json` |
| `UseCustomizationData` | `true` | Default | `appsettings.json` |
| `ASPNETCORE_ENVIRONMENT` | `Development` in launch profiles | Local launch profiles | `launchSettings.json` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopLegacyMVC | .NET Framework / IIS or IIS Express | Not specified | Not specified |
| eShopPorted | ASP.NET Core web host; launch URLs are defined for local development | Not specified | Not specified |

No JVM settings, container resource limits, or instance scaling configuration were found.

## Startup Dependency Chain

1. The ASP.NET host starts and loads its application configuration.
2. The legacy app registers its Autofac container and routes; when mock data is disabled, the EF6 initializer is configured.
3. The ported app builds its service container and request pipeline; the SQL Server context is registered when mock data is disabled.

No external service readiness waits, health-check gates, or startup timeout mechanisms were identified.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` | Database connection string | LocalDB with integrated/trusted authentication; no password value |
| `ConnectionStrings:DefaultConnection` | Database connection string | LocalDB with trusted connection; no password value |

No API keys, credentials, encrypted values, or references to Key Vault, Vault, or cloud secret stores were identified.

### Secrets Provisioning Workflow

No separate secrets provisioning workflow is present in the inspected solution. The configured database connections use local trusted authentication; no secret source, managed identity, or secret binding process was found.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `false` in legacy app; `true` in ported app | `Web.config` / `appsettings.json` |
| `UseCustomizationData` | `false` in legacy app; `true` in ported app | `Web.config` / `appsettings.json` |

These settings select mock versus database-backed catalog service behavior and customized seed data behavior.

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Legacy MVC target framework | .NET Framework 4.7.2 | `src/eShopLegacyMVC/eShopLegacyMVC.csproj` |
| Legacy ASP.NET MVC | 5.2.7 | `src/eShopLegacyMVC/packages.config` |
| Legacy Entity Framework | 6.2.0 | `src/eShopLegacyMVC/packages.config` |
| Ported target framework | .NET Framework 4.6.1 | `eShopPorted/eShopPorted.csproj` |
| Ported ASP.NET Core | 2.2.0 | `eShopPorted/eShopPorted.csproj` |
| Ported Entity Framework Core | 2.2.6 | `eShopPorted/eShopPorted.csproj` |
| Autofac | 4.9.1 / 6.1.0 | Project dependency declarations |
| .NET SDK used for assessment | 10.0.402 | Assessment environment |
