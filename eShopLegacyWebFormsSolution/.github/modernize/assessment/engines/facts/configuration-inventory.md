# Configuration & Externalized Settings Inventory

The project has a small, file-based configuration surface centered on `Web.config`, with build-time Debug and Release configurations and XML transforms. Two application settings select mock data and customization data; no external secret store or environment-variable binding was identified.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web.config | Runtime application configuration | `src/eShopLegacyWebForms/Web.config` | Connection string, application settings, ASP.NET runtime/session settings, modules, and Entity Framework provider |
| Web.Debug.config | Build/publish transform | `src/eShopLegacyWebForms/Web.Debug.config` | Contains template examples; no active configuration transform |
| Web.Release.config | Build/publish transform | `src/eShopLegacyWebForms/Web.Release.config` | Removes the `debug` compilation attribute; connection-string transform is only a commented example |
| ApplicationInsights.config | Telemetry configuration | `src/eShopLegacyWebForms/ApplicationInsights.config` | Configures telemetry initializers, modules, processors, and channel; no instrumentation key is present |
| packages.config | Dependency configuration | `src/eShopLegacyWebForms/packages.config` | Declares NuGet package versions for .NET Framework 4.7.2 |
| eShopLegacyWebForms.csproj | Build configuration | `src/eShopLegacyWebForms/eShopLegacyWebForms.csproj` | Legacy MSBuild project with Debug/Release groups and .NET Framework target |
| .vsconfig | Development environment configuration | `eShopLegacyWebFormsSolution/.vsconfig` | Visual Studio component selection; not runtime configuration |
| Customization CSV files | Optional seed data | `src/eShopLegacyWebForms/Setup/` | Catalog customization files are read when customization data is enabled |

No external configuration server, container configuration, or secret-store reference was identified in the target solution.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild configuration `Debug|AnyCPU` | Development compilation with debug symbols and `DEBUG;TRACE` constants | .NET Framework 4.7.2; CodeDOM compiler provider |
| Release | MSBuild configuration `Release|AnyCPU` | Optimized compilation with `TRACE`; Release transform removes the compilation `debug` attribute | Same project dependencies as Debug |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default | No environment-specific profile mechanism identified | `Web.config` | `UseMockData=true`; `UseCustomizationData=false`; ASP.NET compilation has debugging enabled |
| Debug publish | Web project Debug transform | `Web.Debug.config` | No active overrides |
| Release publish | Web project Release transform | `Web.Release.config` | Removes the `debug` attribute from the compilation element |

## Properties Inventory

| Service | Property Key | Default | Profiles | Source |
|---|---|---|---|---|
| eShopLegacyWebForms | `connectionStrings:CatalogDBContext` | SQL Server LocalDB connection using integrated security; credentials are not embedded | Default; no active transform override | `Web.config` |
| eShopLegacyWebForms | `appSettings:UseMockData` | `true` | Default | `Web.config` |
| eShopLegacyWebForms | `appSettings:UseCustomizationData` | `false` | Default | `Web.config` |
| eShopLegacyWebForms | `system.web/compilation@debug` | `true` | Removed by Release transform | `Web.config`, `Web.Release.config` |
| eShopLegacyWebForms | `system.web/compilation@targetFramework` | `4.7.2` | Default | `Web.config` |
| eShopLegacyWebForms | `system.web/httpRuntime@targetFramework` | `4.6.1` | Default | `Web.config` |
| eShopLegacyWebForms | `system.web/httpRuntime@requestValidationMode` | `2.0` | Default | `Web.config` |
| eShopLegacyWebForms | `system.web/sessionState@mode` | `InProc` | Default | `Web.config` |
| eShopLegacyWebForms | `system.web/globalization@uiCulture` | `en-US` | Default | `Web.config` |
| eShopLegacyWebForms | `system.web/globalization@culture` | `en-US` | Default | `Web.config` |
| eShopLegacyWebForms | `system.web/entityFramework/defaultConnectionFactory` | Entity Framework LocalDB connection factory | Default | `Web.config` |

No `${ENV_VAR}` or `%ENV_VAR%` configuration placeholders were identified.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopLegacyWebForms | IIS/IIS Express-hosted .NET Framework application; no explicit startup parameters identified | Not specified | Not specified |

No container, CPU, memory, or scaling limits were found in the target solution.

## Startup Dependency Chain

1. ASP.NET application startup registers URL routes and bundles, configures the Autofac container, and checks whether database initialization is required.
2. With mock data enabled, the catalog service uses the mock implementation and does not require the database for catalog operations.
3. With mock data disabled, the application configures the Entity Framework initializer for the catalog database.

No external service readiness checks, health probes, or startup wait mechanisms were identified.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` connection string | Database connection | LocalDB and integrated authentication; no password or credential value is configured |
| Application Insights instrumentation key | Telemetry credential | Not present in the inspected configuration; no value emitted |

No encrypted configuration, Key Vault, Vault, AWS Secrets Manager, or other secret provisioning integration was found.

### Secrets Provisioning Workflow

No secrets provisioning workflow is configured in the inspected project. The database connection uses integrated authentication without an embedded password, and the Application Insights configuration contains telemetry modules but no instrumentation key. No managed identity or secret-store access policy was identified.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | `appSettings` in `Web.config`; selects the in-memory catalog service or Entity Framework-backed service |
| `UseCustomizationData` | `false` | `appSettings` in `Web.config`; selects optional customization seed data during database initialization |

No feature-flag framework or remotely managed toggles were identified.

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework target | 4.7.2 | `eShopLegacyWebForms.csproj` |
| ASP.NET runtime target setting | 4.6.1 | `Web.config` |
| Entity Framework | 6.2.0 | `packages.config` |
| Autofac | 4.9.1 | `packages.config` |
| Application Insights | 2.9.1 | `packages.config` |
| log4net | 2.0.10 | `packages.config` |
| MSBuild project format | Legacy non-SDK style | `eShopLegacyWebForms.csproj` |
| NuGet package management | `packages.config` | `packages.config` |
| Visual Studio solution format | Version 12.00 | `eShopLegacyWebForms.sln` |
