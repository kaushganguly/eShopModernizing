# Configuration & Externalized Settings Inventory

The project uses Web.config, two configuration transforms, an Application Insights XML file, and legacy MSBuild project settings. The default runtime uses mock catalog data; no external secret store or environment-variable-based configuration workflow was identified.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web.config | Runtime XML configuration | `src/eShopLegacyWebForms/Web.config` | Main application settings, connection string, ASP.NET runtime and modules |
| Web.Debug.config | XML transform | `src/eShopLegacyWebForms/Web.Debug.config` | No effective transform entries identified |
| Web.Release.config | XML transform | `src/eShopLegacyWebForms/Web.Release.config` | Removes the compilation `debug` attribute |
| ApplicationInsights.config | Telemetry XML configuration | `src/eShopLegacyWebForms/ApplicationInsights.config` | Configures telemetry modules and processors; no instrumentation key or connection string found |
| eShopLegacyWebForms.csproj | Legacy MSBuild project | `src/eShopLegacyWebForms/eShopLegacyWebForms.csproj` | Target framework, Debug/Release properties, and package imports |
| packages.config | NuGet dependency configuration | `src/eShopLegacyWebForms/packages.config` | Declares 44 packages for .NET Framework 4.7.2 |
| eShopLegacyWebForms.sln | Solution configuration | `eShopLegacyWebForms.sln` | Contains the Web Forms project and solution configurations |

No appsettings files, external configuration server, Key Vault, Vault, AWS Secrets Manager, Docker Compose, or Kubernetes configuration is present within the target workspace.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Unoptimized local build; defines `DEBUG;TRACE` | Imports CodeDom and Microsoft.Net.Compilers package targets when installed |
| Release | MSBuild `Release` configuration | Optimized build; defines `TRACE`; Web.Release transform removes compilation debug setting | Same project package references as Debug |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default | IIS/Web Forms application startup; no ASP.NET environment selector identified | Web.config | Mock data enabled; customization data disabled; LocalDB connection string |
| Debug | Web.config transform at build/publish time | Web.Debug.config | No effective override identified |
| Release | Web.config transform at build/publish time | Web.Release.config | Removes the compilation `debug` attribute; no effective database override |

## Properties Inventory

### eShopLegacyWebForms

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `UseMockData` | `true` | Default | `Web.config` appSettings |
| `UseCustomizationData` | `false` | Default | `Web.config` appSettings |
| `CatalogDBContext` | LocalDB `MSSQLLocalDB`, catalog `Microsoft.eShopOnContainers.Services.CatalogDb`, integrated security, multiple active result sets | Default; commented sample transform only | `Web.config` connectionStrings |
| `system.web/compilation@debug` | `true` | Removed for Release | `Web.config`; `Web.Release.config` |
| `system.web/compilation@targetFramework` | `4.7.2` | Default | `Web.config` |
| `system.web/httpRuntime@targetFramework` | `4.6.1` | Default | `Web.config` |
| `system.web/httpRuntime@requestValidationMode` | `2.0` | Default | `Web.config` |
| `system.web/sessionState@mode` | `InProc` | Default | `Web.config` |
| `system.web/globalization@uiCulture` | `en-US` | Default | `Web.config` |
| `system.web/globalization@culture` | `en-US` | Default | `Web.config` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopLegacyWebForms | IIS-hosted .NET Framework 4.7.2; no explicit process flags or environment overrides identified | Not specified | Not specified |

No container or cloud resource allocations are declared in the target workspace.

## Startup Dependency Chain

1. IIS starts the application and invokes `Application_Start`.
2. The application registers page routes and bundles, then constructs the Autofac container and registers the configured catalog service.
3. When mock data is disabled, the application registers `CatalogDBInitializer` for `CatalogDBContext`; the initializer creates and seeds the database when needed.
4. No explicit readiness wait, health check, or startup timeout is configured.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| None identified | No configured secret values or secret-provider references found | Not applicable |

The checked-in database connection uses integrated security and contains no password. No telemetry instrumentation key or credentials are configured in the identified telemetry file.

### Secrets Provisioning Workflow

No secret store, environment-variable secret binding, managed identity, or deployment-time secret provisioning workflow was found in the target project. The LocalDB connection uses integrated security; any production credentials would need to be supplied through hosting configuration not present here.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | Web.config appSettings; selects the mock or database-backed catalog service |
| `UseCustomizationData` | `false` | Web.config appSettings; selects built-in or local customization seed data |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework target | 4.7.2 | `eShopLegacyWebForms.csproj` |
| ASP.NET runtime target | 4.6.1 | `Web.config` `httpRuntime` |
| Entity Framework | 6.2.0 | `packages.config` |
| Autofac | 4.9.1 | `packages.config` |
| ASP.NET Web Forms | System.Web on .NET Framework | Project references and `Web.config` |
| Application Insights | 2.9.1 | `packages.config` and `ApplicationInsights.config` |
| MSBuild configuration | Legacy non-SDK project; Debug and Release | `eShopLegacyWebForms.csproj` |
