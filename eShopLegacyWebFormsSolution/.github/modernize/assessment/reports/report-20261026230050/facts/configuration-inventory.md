# Configuration & Externalized Settings Inventory

Configuration is primarily XML-based, with one application configuration file and Debug/Release transforms. The checked-in mock-data switch defaults to enabled; no secret store or external configuration service is referenced.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| ASP.NET application settings and connection strings | Runtime configuration | `src/eShopLegacyWebForms/Web.config` | Contains LocalDB connection, mock/customization switches, ASP.NET settings, EF provider, and binding redirects |
| Debug transform | Environment transform | `src/eShopLegacyWebForms/Web.Debug.config` | Web Application project transform |
| Release transform | Environment transform | `src/eShopLegacyWebForms/Web.Release.config` | Web Application project transform |
| Application Insights | Telemetry configuration | `src/eShopLegacyWebForms/ApplicationInsights.config` | Configures telemetry initializers, modules, processors, and sinks |
| Bundling | Runtime asset configuration | `src/eShopLegacyWebForms/Bundle.config`, `App_Start/BundleConfig.cs` | Defines/registers browser asset bundles |
| Routes | Runtime route configuration | `src/eShopLegacyWebForms/App_Start/RouteConfig.cs` | Maps catalog pages |
| Package versions | Build/package configuration | `src/eShopLegacyWebForms/packages.config` | Declares 44 NuGet packages |
| Project build configuration | MSBuild | `src/eShopLegacyWebForms/eShopLegacyWebForms.csproj` | Legacy Web Application project targeting .NET Framework 4.7.2 |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | Visual Studio/MSBuild `Debug` configuration | Development build | Debug symbols and project-defined compilation settings |
| Release | Visual Studio/MSBuild `Release` configuration | Optimized deployment build | Release build settings; Web.Release.config transform |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default | No ASP.NET environment profile; `Web.config` applies | `Web.config` | `UseMockData=true`; `UseCustomizationData=false`; LocalDB configured |
| Debug | Web.Debug.config transform at deployment/build | `Web.Debug.config` | No distinct runtime profile was identified |
| Release | Web.Release.config transform at deployment/build | `Web.Release.config` | No distinct runtime profile was identified |

## Properties Inventory

### `eShopLegacyWebForms`

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `connectionStrings:CatalogDBContext` | `Data Source=(localdb)\MSSQLLocalDB; Initial Catalog=Microsoft.eShopOnContainers.Services.CatalogDb; Integrated Security=True; MultipleActiveResultSets=True;` | Default | `Web.config` |
| `appSettings:UseMockData` | `true` | Default | `Web.config` |
| `appSettings:UseCustomizationData` | `false` | Default | `Web.config` |
| `system.web/compilation@debug` | `true` | Default; transforms may change it | `Web.config` |
| `system.web/compilation@targetFramework` | `4.7.2` | Default | `Web.config` |
| `system.web/httpRuntime@targetFramework` | `4.6.1` | Default | `Web.config` |
| `system.web/httpRuntime@requestValidationMode` | `2.0` | Default | `Web.config` |
| `system.web/sessionState@mode` | `InProc` | Default | `Web.config` |
| `globalization@uiCulture` / `@culture` | `en-US` | Default | `Web.config` |
| `EntityFramework defaultConnectionFactory` | LocalDB factory using `mssqllocaldb` | Default | `Web.config` |
| Newtonsoft.Json binding redirect | `0.0.0.0-12.0.0.0` redirected to `12.0.0.0` | Default | `Web.config` |
| Application Insights instrumentation key | Not present | All | No key/reference found in `ApplicationInsights.config` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| `eShopLegacyWebForms` | ASP.NET on .NET Framework 4.7.2 hosted by IIS/IIS Express; no JVM or explicit runtime flags found | Not specified | Not specified |
| SQL Server LocalDB | LocalDB instance referenced by the application connection string | Not specified | Local development database |

## Startup Dependency Chain

1. ASP.NET application startup registers routes and bundles.
2. Autofac is configured and chooses the catalog implementation from `UseMockData`.
3. If mock mode is disabled, EF6 configures the database initializer; LocalDB must be available when database operations start.
4. No readiness probes, external config server, or explicit database wait/retry mechanism was found.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` connection | Database connection | LocalDB with integrated Windows authentication; no password is stored |
| Application Insights instrumentation key | Telemetry credential | No key or external reference found |

### Secrets Provisioning Workflow

No Key Vault, Vault, AWS Secrets Manager, encrypted settings, or provisioning workflow is configured in the selected application. The database connection uses integrated Windows authentication. Any production identity or deployment-level secret provisioning is outside the checked-in project configuration.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | `Web.config` app setting; selects mock or EF-backed service |
| `UseCustomizationData` | `false` | `Web.config` app setting; controls CSV/ZIP catalog initialization |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework | 4.7.2 project target | `.csproj` and `Web.config` |
| ASP.NET Web Forms | System.Web framework | Project type and page files |
| Entity Framework | 6.2.0 | `packages.config` |
| Autofac | 4.9.1 | `packages.config` |
| Autofac.Web | 4.0.0 | `packages.config` |
| log4net | 2.0.10 | `packages.config` |
| Application Insights | 2.9.1 | `packages.config` |
| Newtonsoft.Json | 12.0.1 | `packages.config` |
| MSBuild project format | Legacy Web Application project, ToolsVersion 12.0 | `.csproj` |
