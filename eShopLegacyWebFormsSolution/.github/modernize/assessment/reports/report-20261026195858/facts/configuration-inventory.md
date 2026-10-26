# Configuration & Externalized Settings Inventory

Configuration is concentrated in the Web Forms project’s XML files, with two application settings controlling mock data and seed-data customization. No runtime environment profiles, external configuration service, or secret-store integration were detected.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web configuration | Runtime settings and connection string | `src/eShopLegacyWebForms/Web.config` | Contains app settings, session, compilation, globalization, binding redirects, EF provider, and SQL LocalDB connection |
| Debug transform | Environment transform | `src/eShopLegacyWebForms/Web.Debug.config` | Web.config transform |
| Release transform | Environment transform | `src/eShopLegacyWebForms/Web.Release.config` | Web.config transform |
| Application Insights | Telemetry configuration | `src/eShopLegacyWebForms/ApplicationInsights.config` | Application Insights SDK configuration |
| Bundle configuration | Client asset registration | `src/eShopLegacyWebForms/Bundle.config` and `App_Start/BundleConfig.cs` | Browser asset bundles |
| Logging | log4net configuration | `src/eShopLegacyWebForms/log4Net.xml` | Logging appenders and levels |
| Package manifest | NuGet package versions | `src/eShopLegacyWebForms/packages.config` | 44 declared packages |
| Project file | Build and framework settings | `src/eShopLegacyWebForms/eShopLegacyWebForms.csproj` | Legacy MSBuild project targeting .NET Framework 4.7.2 |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | Visual Studio/MSBuild Debug configuration | Development build | Defines `DEBUG;TRACE`; Web.config has `debug="true"` |
| Release | Visual Studio/MSBuild Release configuration | Release build | Defines `TRACE`; uses Web.Release.config transform |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default | IIS/Web Forms application startup | `Web.config` | `UseMockData=true`, `UseCustomizationData=false` |
| Debug | Web.Debug.config transform | Web.config plus transform | No additional runtime profile settings identified |
| Release | Web.Release.config transform | Web.config plus transform | No additional runtime profile settings identified |

## Properties Inventory

| Service | Property Key | Default | Profiles | Source |
|---|---|---|---|---|
| eShopLegacyWebForms | `UseMockData` | `true` | Default | `Web.config` appSettings |
| eShopLegacyWebForms | `UseCustomizationData` | `false` | Default | `Web.config` appSettings |
| eShopLegacyWebForms | `CatalogDBContext` connection | LocalDB; integrated security | Default database mode | `Web.config` connectionStrings |
| eShopLegacyWebForms | `compilation.debug` | `true` | Default; build transforms | `Web.config` |
| eShopLegacyWebForms | `compilation.targetFramework` | `4.7.2` | Default | `Web.config` |
| eShopLegacyWebForms | `httpRuntime.targetFramework` | `4.6.1` | Default | `Web.config` |
| eShopLegacyWebForms | `httpRuntime.requestValidationMode` | `2.0` | Default | `Web.config` |
| eShopLegacyWebForms | `sessionState.mode` | `InProc` | Default | `Web.config` |
| eShopLegacyWebForms | `globalization.uiCulture` / `culture` | `en-US` | Default | `Web.config` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopLegacyWebForms | IIS-hosted .NET Framework; no explicit startup arguments found | Not specified | Not specified |

## Startup Dependency Chain

1. IIS starts the ASP.NET application and invokes `Application_Start`.
2. The application registers routes and bundles, builds the Autofac container, then configures database initialization.
3. When `UseMockData` is false, the EF initializer creates/seeds the catalog database on first use; otherwise database initialization is skipped.
4. No explicit readiness probe, startup wait, or external config service dependency was found.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| None detected | — | The configured LocalDB connection uses integrated security and contains no password |

### Secrets Provisioning Workflow

No external secret provisioning workflow, key vault, credential injection, or cryptographic secret configuration was found in the project files.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| UseMockData | `true` | `Web.config` appSettings |
| UseCustomizationData | `false` | `Web.config` appSettings |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework | 4.7.2 | Project file and Web.config |
| ASP.NET Web Forms | System.Web on .NET Framework 4.7.2 | Project references |
| Entity Framework | 6.2.0 | `packages.config` |
| Autofac | 4.9.1 | `packages.config` |
| Application Insights | 2.9.1; Agent Intercept 2.4.0 | `packages.config` |
| log4net | 2.0.10 | `packages.config` |
| Bootstrap | 4.3.1 | `packages.config` |
| jQuery | 3.3.1 | `packages.config` |
