# Configuration & Externalized Settings Inventory

The solution uses XML configuration and transforms for the legacy MVC app, plus JSON configuration for the ported app. Runtime toggles select mock data and customization-data seeding; no external secret store integration was found.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web.config | Application XML | `src/eShopLegacyMVC/Web.config` | SQL connection and application settings |
| Web.Debug.config | Transform | `src/eShopLegacyMVC/Web.Debug.config` | Debug connection-string transform |
| Web.Release.config | Transform | `src/eShopLegacyMVC/Web.Release.config` | Release connection-string transform |
| appsettings.json | Application JSON | `eShopPorted/appsettings.json` | Default connection, logging, hosts, and feature toggles |
| appsettings.Development.json | Environment JSON | `eShopPorted/appsettings.Development.json` | Development logging overrides |
| launchSettings.json | Local launch settings | `eShopPorted/Properties/launchSettings.json` | Local run profiles |
| app.config | Application XML | `eShopPorted/app.config` | Legacy-compatible app configuration |
| log4Net.xml | Logging XML | `src/eShopLegacyMVC/log4Net.xml` | Log4net configuration |
| ApplicationInsights.config | Telemetry XML | `src/eShopLegacyMVC/ApplicationInsights.config` | Application Insights configuration |

No external configuration repository, Key Vault, Vault, or cloud secret provider reference was found in the scoped solution.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild configuration | Development compilation and debug symbols | No profile-specific package additions identified |
| Release | MSBuild configuration | Optimized compilation and release transforms | Web.Release.config applies connection transform |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Legacy default | IIS / Web.config | `Web.config` | `UseMockData=false`, `UseCustomizationData=false` |
| Legacy Debug | Web.Debug.config transform | `Web.config`, `Web.Debug.config` | Connection string transform |
| Legacy Release | Web.Release.config transform | `Web.config`, `Web.Release.config` | Connection string transform |
| Ported default | ASP.NET Core host environment | `appsettings.json` | `UseMockData=true`, `UseCustomizationData=true` |
| Ported Development | ASP.NET Core Development environment | `appsettings.Development.json` plus base JSON | Logging overrides |

## Properties Inventory

### eShopLegacyMVC

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `connectionStrings:CatalogDBContext` | Configured value (not reproduced) | Debug / Release transforms | `Web.config` and transforms |
| `appSettings:UseMockData` | `false` | Base config | `Web.config` |
| `appSettings:UseCustomizationData` | `false` | Base config | `Web.config` |
| `compilation.debug` | `true` | Base config | `Web.config` |
| `compilation.targetFramework` | `4.7.2` | Base config | `Web.config` |
| `httpRuntime.targetFramework` | `4.7` | Base config | `Web.config` |

### eShopPorted

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `ConnectionStrings:DefaultConnection` | LocalDB connection | Base config | `appsettings.json` |
| `Logging:LogLevel:Default` | `Warning` | Base config | `appsettings.json` |
| `AllowedHosts` | `*` | Base config | `appsettings.json` |
| `UseMockData` | `true` | Base config | `appsettings.json` |
| `UseCustomizationData` | `true` | Base config | `appsettings.json` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopLegacyMVC | IIS-hosted .NET Framework; no explicit startup options found | Not specified | Not specified |
| eShopPorted | ASP.NET Core host; environment set by host/launch profile | Not specified | Not specified |

No container resource limits, explicit memory allocation, or instance scaling settings were found within the scoped solution.

## Startup Dependency Chain

1. The legacy application initializes Autofac and registers MVC/Web API controllers.
2. With mock data disabled, the legacy application configures its EF6 database initializer; the application needs the configured SQL Server to serve persistent catalog operations.
3. The ported application configures MVC and Autofac; with mock mode disabled, it configures its EF Core SQL Server DbContext.
4. No readiness probes, health-check wait mechanisms, or explicit database startup timeout settings were identified.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` connection string | Database connection | Configured by Web.config and transforms; no password field identified |
| `DefaultConnection` | Database connection | LocalDB trusted connection in appsettings; no password field identified |

### Secrets Provisioning Workflow

No Key Vault, managed identity, Vault, or cloud secret provisioning flow was found. The legacy connection is supplied through XML configuration/transforms; the ported app reads its JSON connection setting. No actual credential values are reproduced here.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `false` in legacy app; `true` in ported app | Web.config / appsettings |
| `UseCustomizationData` | `false` in legacy app; `true` in ported app | Web.config / appsettings |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Legacy MVC app target | .NET Framework 4.7.2 | `eShopLegacyMVC.csproj` |
| Legacy Web API | 5.2.7 | `packages.config` |
| Ported app target | net461 | `eShopPorted.csproj` |
| ASP.NET Core MVC packages | 2.2.0 | `eShopPorted.csproj` |
| Entity Framework | 6.2.0 | `packages.config` |
| Entity Framework Core | 2.2.6 | `eShopPorted.csproj` |
| .NET SDK used for assessment | 10.0.402 | Environment |
