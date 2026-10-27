# Configuration & Externalized Settings Inventory

The application uses a small set of XML configuration files and build-time Web.config transforms. Mock catalog mode is enabled by default; no external secret store or environment-specific application settings were identified.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web.config | Runtime configuration | `src/eShopLegacyWebForms/Web.config` | Connection string, mock/customization flags, session, framework and HTTP module settings |
| Web.Debug.config | Build transform | `src/eShopLegacyWebForms/Web.Debug.config` | Debug transform file; no application-specific overrides found |
| Web.Release.config | Build transform | `src/eShopLegacyWebForms/Web.Release.config` | Removes the compilation `debug` attribute |
| log4net.xml | Logging configuration | `src/eShopLegacyWebForms/log4Net.xml` | Rolling-file appender and logging format |
| ApplicationInsights.config | Telemetry configuration | `src/eShopLegacyWebForms/ApplicationInsights.config` | Telemetry modules, initializers, and sampling; no instrumentation key configured |
| Bundle.config | Asset configuration | `src/eShopLegacyWebForms/Bundle.config` | Bundled CSS and JavaScript assets |
| packages.config | Dependency configuration | `src/eShopLegacyWebForms/packages.config` | NuGet package versions for the legacy project |
| eShopLegacyWebForms.csproj | Build and IIS Express settings | `src/eShopLegacyWebForms/eShopLegacyWebForms.csproj` | .NET Framework target, Debug/Release configuration, local IIS Express port |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Development compilation and debugging | CodeDom compiler; Debug Web.config transform |
| Release | MSBuild `Release` configuration | Release packaging; removes the compilation debug attribute | Same declared packages; Release Web.config transform |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default | IIS/IIS Express; no ASP.NET environment variable profile found | `Web.config` | `UseMockData=true`; `UseCustomizationData=false` |
| Release transform | Applied during Release packaging | `Web.config`, `Web.Release.config` | Removes compilation debug setting; no connection-string or app-setting replacement is active |

## Properties Inventory

### eShopLegacyWebForms

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `UseMockData` | `true` | Default | `Web.config`; selects in-memory mock service; when false, enables EF database mode |
| `UseCustomizationData` | `false` | Default | `Web.config`; when enabled, seed data is read from setup files |
| `CatalogDBContext` connection string | LocalDB `MSSQLLocalDB`, catalog `Microsoft.eShopOnContainers.Services.CatalogDb`, integrated security | Default configuration; used only when mock mode is disabled | `Web.config`; no password value |
| `system.web/compilation/@debug` | `true` | Removed by Release transform | `Web.config`, `Web.Release.config` |
| `system.web/compilation/@targetFramework` | `4.7.2` | Default | `Web.config` |
| `system.web/httpRuntime/@targetFramework` | `4.6.1` | Default | `Web.config` |
| `system.web/sessionState/@mode` | `InProc` | Default | `Web.config` |
| `system.web/globalization/@culture` and `@uiCulture` | `en-US` | Default | `Web.config` |
| `entityFramework/defaultConnectionFactory` | `mssqllocaldb` | Default; only relevant to database mode | `Web.config` |
| Log file | `logFiles\myapp.log` | Default | `log4Net.xml`; rolling file output |
| Application Insights sampling | Adaptive sampling, configured at 5 items/second | Default | `ApplicationInsights.config` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopLegacyWebForms | IIS Express port `50586`; no JVM settings | Not specified | Not specified |

No container CPU/memory limits, replica count, or explicit application heap settings were found.

## Startup Dependency Chain

1. IIS/IIS Express starts the ASP.NET application.
2. `Application_Start` registers routes and bundles and configures the Autofac container.
3. If `UseMockData=false`, EF6 is configured to initialize the LocalDB-backed catalog. With the default `UseMockData=true`, no database startup dependency is required.
4. Application Insights and Autofac HTTP modules participate in request processing.

No readiness probe, external configuration server, or startup wait mechanism was found.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` | Database connection string | `Web.config`; LocalDB uses integrated security; no password is present |
| Application Insights instrumentation key | Telemetry credential | No configured key found |

### Secrets Provisioning Workflow

No Key Vault, Vault, AWS Secrets Manager, service principal, or secret-provisioning workflow was identified. The configured LocalDB connection uses integrated security and contains no embedded password. The Application Insights configuration has no instrumentation key.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | `Web.config`; chooses mock or EF-backed catalog service |
| `UseCustomizationData` | `false` | `Web.config`; chooses CSV/setup data or preconfigured seed data |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework target | 4.7.2 | `eShopLegacyWebForms.csproj` and `Web.config` |
| ASP.NET `httpRuntime` target | 4.6.1 | `Web.config` |
| Entity Framework | 6.2.0 | `packages.config` |
| Autofac | 4.9.1 | `packages.config` |
| log4net | 2.0.10 | `packages.config` |
| Application Insights | 2.9.1 | `packages.config` |
| Bootstrap | 4.3.1 | `packages.config` |
| jQuery | 3.5.0 | `packages.config` |
| Newtonsoft.Json | 12.0.1 | `packages.config` |
| Build system | Legacy MSBuild project; Visual Studio WebApplication targets required | `.csproj` and `.sln` |
