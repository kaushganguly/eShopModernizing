# Configuration & Externalized Settings Inventory

Configuration is primarily stored in Web.config, with Debug and Release transforms, package/build files, and separate logging and telemetry configuration. The inspected configuration uses local integrated SQL authentication and contains no secret-store integration.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web.config | Runtime configuration | `src/eShopLegacyWebForms/Web.config` | Connection string, mock/customization flags, framework settings, session, EF and modules |
| Web.Debug.config | Build transform | `src/eShopLegacyWebForms/Web.Debug.config` | No active transformation beyond example templates |
| Web.Release.config | Build transform | `src/eShopLegacyWebForms/Web.Release.config` | Removes the debug compilation attribute |
| ApplicationInsights.config | Telemetry configuration | `src/eShopLegacyWebForms/ApplicationInsights.config` | Application Insights web tracking; no instrumentation key or endpoint is checked in |
| log4Net.xml | Logging configuration | `src/eShopLegacyWebForms/log4Net.xml` | Rolling file appender to `logFiles\myapp.log` |
| Bundle.config | Web asset configuration | `src/eShopLegacyWebForms/Bundle.config` | Legacy bundling configuration file |
| packages.config | Dependency configuration | `src/eShopLegacyWebForms/packages.config` | 44 NuGet package declarations |
| `.csproj` / `.sln` | Build configuration | Project and solution root | Legacy Web Application project targeting .NET Framework 4.7.2 |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | Visual Studio/MSBuild `Debug` configuration | Development build | Web.Debug.config transform; no additional package group identified |
| Release | Visual Studio/MSBuild `Release` configuration | Release build | Removes the debug compilation attribute through Web.Release.config |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default | ASP.NET runtime; no explicit environment selector found | Web.config | `UseMockData=true`, `UseCustomizationData=false`, LocalDB connection |
| Debug | Web.config transform selected by build configuration | Web.Debug.config | No active application setting override detected |
| Release | Web.config transform selected by build configuration | Web.Release.config | Removes `compilation debug`; connection-string sample is commented out |

## Properties Inventory

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `CatalogDBContext` | LocalDB catalog connection with integrated security | Default | Web.config connectionStrings |
| `UseMockData` | `true` | Default | Web.config appSettings |
| `UseCustomizationData` | `false` | Default | Web.config appSettings |
| `system.web/compilation@debug` | `true` | Release transform removes the attribute | Web.config / Web.Release.config |
| `system.web/compilation@targetFramework` | `4.7.2` | Default | Web.config |
| `system.web/httpRuntime@targetFramework` | `4.6.1` | Default | Web.config |
| `system.web/sessionState@mode` | `InProc` | Default | Web.config |
| `runtime/assemblyBinding` | Newtonsoft.Json 12.0.0 binding redirect ceiling and other redirects | Default | Web.config |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopLegacyWebForms | ASP.NET hosted by IIS/IIS Express; no project-defined startup flags | Not specified | Not specified |

## Startup Dependency Chain

1. IIS initializes the ASP.NET application.
2. `Application_Start` registers routes and bundles, configures Autofac, then configures EF database initialization when mock data is disabled.
3. In database mode, EF uses the configured SQL Server LocalDB connection; no explicit readiness wait or startup timeout is configured.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` | Database connection | LocalDB integrated security; no embedded password |
| Application Insights instrumentation key | Telemetry credential | Not configured in the checked-in file |

### Secrets Provisioning Workflow

No Key Vault, Vault, cloud secrets manager, encrypted configuration, or service-principal provisioning workflow was found. The checked-in database connection uses Windows integrated security; deployment-specific credentials and transport protection, if needed, must be supplied by the hosting environment.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | Web.config appSettings; selects mock versus EF-backed service |
| `UseCustomizationData` | `false` | Web.config appSettings; selects local CSV and picture seed data |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework target | 4.7.2 | `.csproj` and Web.config |
| ASP.NET HTTP runtime target | 4.6.1 | Web.config |
| Entity Framework | 6.2.0 | packages.config |
| Autofac | 4.9.1 | packages.config |
| log4net | 2.0.10 | packages.config |
| Microsoft Application Insights | 2.9.1 | packages.config |
| Newtonsoft.Json | 12.0.1 | packages.config |
| Visual Studio project format | Legacy Web Application Project; VS 2017-era solution | `.csproj` and `.sln` |
