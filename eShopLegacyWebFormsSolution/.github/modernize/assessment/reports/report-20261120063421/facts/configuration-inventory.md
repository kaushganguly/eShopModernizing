# Configuration & Externalized Settings Inventory

The application has a small configuration surface centered on Web.config and its Debug/Release transforms. It selects mock versus database-backed catalog access through application settings; the configured SQL connection uses integrated authentication.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web.config | ASP.NET application configuration | `src/eShopLegacyWebForms/Web.config` | Connection string, app settings, runtime, Entity Framework, and assembly redirects |
| Web.Debug.config | Build transform | `src/eShopLegacyWebForms/Web.Debug.config` | No active overrides; contains sample transform guidance |
| Web.Release.config | Build transform | `src/eShopLegacyWebForms/Web.Release.config` | Removes the compilation `debug` attribute |
| ApplicationInsights.config | Telemetry configuration | `src/eShopLegacyWebForms/ApplicationInsights.config` | Telemetry modules and sampling; no instrumentation key or connection string |
| log4Net.xml | Logging configuration | `src/eShopLegacyWebForms/log4Net.xml` | Rolling-file appender and log layout |
| Bundle.config | Asset bundling configuration | `src/eShopLegacyWebForms/Bundle.config` | CSS bundle includes Bootstrap and site styles |
| eShopLegacyWebForms.csproj | Build configuration | `src/eShopLegacyWebForms/eShopLegacyWebForms.csproj` | Legacy Web Application project targeting .NET Framework 4.7.2 |
| packages.config | Dependency configuration | `src/eShopLegacyWebForms/packages.config` | Declares NuGet package versions |
| .vsconfig | IDE workload manifest | `eShopLegacyWebFormsSolution/.vsconfig` | Visual Studio setup metadata; not runtime configuration |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild/Visual Studio `Debug` configuration | Development compilation and debugging | Web Forms project build targets |
| Release | MSBuild/Visual Studio `Release` configuration | Release compilation; transform removes the debug setting | Same package set as Debug |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default | Base ASP.NET application configuration | `Web.config` | `UseMockData=true`; `UseCustomizationData=false` |
| Debug | Web application publish/build transform | `Web.config`, `Web.Debug.config` | No active transform override detected |
| Release | Web application publish/build transform | `Web.config`, `Web.Release.config` | Removes the compilation debug attribute |

## Properties Inventory

### Application

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `UseMockData` | `true` | Default | `Web.config` |
| `UseCustomizationData` | `false` | Default | `Web.config` |
| `connectionStrings:CatalogDBContext` | SQL Server LocalDB catalog database; integrated authentication; credentials absent | Default, when mock data is disabled | `Web.config` |
| `system.web/compilation/@debug` | `true` | Removed by Release transform | `Web.config`, `Web.Release.config` |
| `system.web/compilation/@targetFramework` | `4.7.2` | Default | `Web.config` |
| `system.web/httpRuntime/@targetFramework` | `4.6.1` | Default | `Web.config` |
| `system.web/httpRuntime/@requestValidationMode` | `2.0` | Default | `Web.config` |
| `system.web/sessionState/@mode` | `InProc` | Default | `Web.config` |
| `system.web/globalization/@uiCulture` | `en-US` | Default | `Web.config` |
| `system.web/globalization/@culture` | `en-US` | Default | `Web.config` |
| `entityFramework/defaultConnectionFactory/parameters/parameter` | `mssqllocaldb` | Default | `Web.config` |

### Logging and telemetry

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `log4net/root/level` | `ALL` | Default | `log4Net.xml` |
| `log4net/appender/file` | `logFiles\myapp.log` | Default | `log4Net.xml` |
| `log4net/appender/maximumFileSize` | `10MB` | Default | `log4Net.xml` |
| `log4net/appender/maxSizeRollBackups` | `5` | Default | `log4Net.xml` |
| `ApplicationInsights/TelemetrySinks/default/TelemetryProcessors/*/MaxTelemetryItemsPerSecond` | `5` | Default | `ApplicationInsights.config` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopLegacyWebForms | ASP.NET on .NET Framework under IIS/IIS Express; no explicit startup flags found | Not specified | Not specified |

No container resource limits, JVM options, or scaling configuration were found in the assessed workspace.

## Startup Dependency Chain

1. ASP.NET application startup registers page routes and asset bundles.
2. Autofac is configured from `UseMockData` and registers the corresponding catalog service.
3. If mock mode is disabled, the application registers the Entity Framework database initializer, which creates and seeds catalog data on database use.

No external service readiness checks or startup wait mechanisms were found.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` connection string | Database connection | LocalDB and integrated authentication; no username/password present |
| Application Insights instrumentation key / connection string | Telemetry credential | Not configured |

### Secrets Provisioning Workflow

No Key Vault, Vault, AWS Secrets Manager, or other secret store integration was found. The configured database connection relies on the host's integrated identity; there is no SQL password to provision. Application Insights libraries are installed, but the inspected configuration does not provide an instrumentation key or connection string.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | `Web.config` app setting |
| `UseCustomizationData` | `false` | `Web.config` app setting |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework | 4.7.2 | Project file and Web.config compilation setting |
| ASP.NET Web Forms | System.Web framework; configured httpRuntime target 4.6.1 | Web.config |
| Entity Framework | 6.2.0 | packages.config |
| Autofac | 4.9.1 | packages.config |
| log4net | 2.0.10 | packages.config |
| Application Insights | 2.9.1 family | packages.config |
| Visual Studio solution format | 12.00 | Solution file |
