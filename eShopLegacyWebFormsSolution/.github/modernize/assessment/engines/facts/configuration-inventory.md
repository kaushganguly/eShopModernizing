# Configuration & Externalized Settings Inventory

The application uses a small set of XML configuration files and MSBuild project/package files. Runtime behavior is controlled primarily by two application settings; the default is mock catalog data with no required database credentials.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web.config | Runtime configuration | `src/eShopLegacyWebForms/Web.config` | Connection string, application settings, ASP.NET settings, modules, and binding redirects |
| Web.Debug.config | Transform | `src/eShopLegacyWebForms/Web.Debug.config` | Build-time connection string transform |
| Web.Release.config | Transform | `src/eShopLegacyWebForms/Web.Release.config` | Build-time connection string transform |
| ApplicationInsights.config | Telemetry configuration | `src/eShopLegacyWebForms/ApplicationInsights.config` | Application Insights settings |
| Bundle.config | Browser asset configuration | `src/eShopLegacyWebForms/Bundle.config` | Client bundle definitions |
| eShopLegacyWebForms.csproj | Build configuration | `src/eShopLegacyWebForms/eShopLegacyWebForms.csproj` | Legacy MSBuild project, .NET Framework target, Debug/Release |
| packages.config | Dependency configuration | `src/eShopLegacyWebForms/packages.config` | NuGet package versions |

No external configuration server or secret-store integration was identified.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Development compilation and debugging | Legacy MSBuild and Web Application targets |
| Release | MSBuild `Release` configuration | Release compilation and packaging | Web.config transform support |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default | Web.config | `Web.config` | `UseMockData=true`, `UseCustomizationData=false`; LocalDB configured but not used in mock mode |
| Debug transform | Build configuration | `Web.Debug.config` | Replaces the connection string during transform |
| Release transform | Build configuration | `Web.Release.config` | Replaces the connection string during transform |

## Properties Inventory

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `UseMockData` | `true` | Default | Web.config |
| `UseCustomizationData` | `false` | Default | Web.config |
| `CatalogDBContext` connection string | LocalDB `MSSQLLocalDB`; integrated security | Default | Web.config |
| `compilation.debug` | `true` | Default | Web.config |
| `compilation.targetFramework` | `4.7.2` | Default | Web.config |
| `httpRuntime.targetFramework` | `4.6.1` | Default | Web.config |
| `requestValidationMode` | `2.0` | Default | Web.config |

No environment-variable placeholders were identified in the application configuration.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopLegacyWebForms | ASP.NET hosted on IIS; no custom startup options found | Not specified | Not specified |

## Startup Dependency Chain

1. ASP.NET application startup registers routes and bundles.
2. Autofac is configured and chooses the mock or database catalog service.
3. If mock data is disabled, EF6 is configured to initialize the catalog database when first used; no explicit wait or health-check mechanism is configured.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| None detected | — | The configured LocalDB connection uses integrated security; no password or API secret was found. |

### Secrets Provisioning Workflow

No application secret-provisioning workflow, managed identity, vault, or credential binding was found. Database access in the checked-in connection string relies on Windows integrated security.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | Web.config `appSettings` |
| `UseCustomizationData` | `false` | Web.config `appSettings` |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework target | 4.7.2 | `eShopLegacyWebForms.csproj` |
| ASP.NET runtime target | 4.6.1 | Web.config |
| Entity Framework | 6.2.0 | packages.config |
| Autofac | 4.9.1 | packages.config |
| Application Insights | 2.9.1 | packages.config |
| MSBuild project tools | 12.0 legacy project format | `eShopLegacyWebForms.csproj` |
