# Configuration & Externalized Settings Inventory

Configuration is primarily held in ASP.NET XML files, with Debug and Release web.config transforms. Two app settings control whether mock data and customization data are enabled; no external secret store or environment-variable binding was identified.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| ASP.NET application settings and connection strings | Runtime configuration | `src/eShopLegacyWebForms/Web.config` | Includes LocalDB, framework, session, and feature settings |
| Debug transform | Build/deployment transform | `src/eShopLegacyWebForms/Web.Debug.config` | Applies Debug-specific connection configuration |
| Release transform | Build/deployment transform | `src/eShopLegacyWebForms/Web.Release.config` | Applies Release-specific connection configuration |
| Application Insights | Telemetry configuration | `src/eShopLegacyWebForms/ApplicationInsights.config` | Web telemetry configuration; no populated instrumentation key found |
| log4net | Logging configuration | `src/eShopLegacyWebForms/log4Net.xml` | Logging appenders and levels |
| ASP.NET bundles | UI configuration | `src/eShopLegacyWebForms/Bundle.config`, `App_Start/BundleConfig.cs` | Script and style bundle registration |
| ASP.NET routes | Application configuration | `src/eShopLegacyWebForms/App_Start/RouteConfig.cs` | Friendly page routes |
| Project and package manifests | Build/dependency configuration | `src/eShopLegacyWebForms/eShopLegacyWebForms.csproj`, `packages.config` | Legacy MSBuild project and NuGet package versions |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Development compilation and debugging | Debug symbols and web.config transform |
| Release | MSBuild `Release` configuration | Optimized deployment build | Release web.config transform |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default | ASP.NET application startup | `Web.config` | `UseMockData=true`; `UseCustomizationData=false` |
| Debug | Build/deployment transform | `Web.Debug.config` | Debug-specific connection string transform |
| Release | Build/deployment transform | `Web.Release.config` | Release-specific connection string transform |

## Properties Inventory

### eShopLegacyWebForms

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `UseMockData` | `true` | Default | `Web.config` |
| `UseCustomizationData` | `false` | Default | `Web.config` |
| `CatalogDBContext` connection string | LocalDB, Windows integrated security | Debug/Release transforms may replace it | `Web.config`, `Web.Debug.config`, `Web.Release.config` |
| `system.web/compilation` target framework | `4.7.2` | Default | `Web.config` |
| `system.web/httpRuntime` target framework | `4.6.1` | Default | `Web.config` |
| `system.web/sessionState` mode | `InProc` | Default | `Web.config` |
| `system.web/httpRuntime` request validation mode | `2.0` | Default | `Web.config` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopLegacyWebForms | ASP.NET on .NET Framework under IIS; no explicit startup parameters found | Not specified | Not specified |

No container resource limits or JVM options were found in the target workspace.

## Startup Dependency Chain

1. ASP.NET application startup registers routes and bundles and configures the Autofac container.
2. With mock mode enabled, the catalog service uses the in-memory mock and does not require the database.
3. With mock mode disabled, EF6 uses the configured LocalDB connection; the database initializer seeds or initializes catalog data as required.
4. Application Insights and log4net provide telemetry and logging configuration.

No explicit service health wait or startup timeout mechanism was found.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` | Database connection | LocalDB with integrated security; no password present |
| Application Insights instrumentation key | Telemetry credential | No populated key found |

### Secrets Provisioning Workflow

No Key Vault, Vault, AWS Secrets Manager, encrypted settings, or deployment secret-provisioning workflow was identified. The configured database connection relies on Windows integrated security; no hard-coded database password or application credential was found.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | ASP.NET `appSettings` |
| `UseCustomizationData` | `false` | ASP.NET `appSettings` |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework target | 4.7.2 | Project and `Web.config` |
| ASP.NET HTTP runtime target | 4.6.1 | `Web.config` |
| Entity Framework | 6.2.0 | `packages.config` |
| Autofac | 4.9.1 | `packages.config` |
| ASP.NET Web Forms | .NET Framework `System.Web` | Project and `Web.config` |
| Application Insights | 2.9.1 | `packages.config` |
| log4net | 2.0.10 | `packages.config` |
