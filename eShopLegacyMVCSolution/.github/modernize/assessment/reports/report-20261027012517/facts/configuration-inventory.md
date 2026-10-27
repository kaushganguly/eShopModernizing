# Configuration & Externalized Settings Inventory

The solution uses XML configuration for the legacy MVC application and JSON/environment configuration for the ported application. Runtime switches control mock data and customization data; no external secret store or production configuration profile was found.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Legacy application configuration | XML | `src/eShopLegacyMVC/Web.config` | App settings, LocalDB connection, runtime and web settings |
| Legacy transforms | XML transforms | `src/eShopLegacyMVC/Web.Debug.config`, `Web.Release.config` | Connection-string transforms for build configurations |
| Legacy logging | XML | `src/eShopLegacyMVC/log4Net.xml` | Rolling file appender |
| Legacy telemetry | XML | `src/eShopLegacyMVC/ApplicationInsights.config` | Application Insights modules and initializers |
| Ported default settings | JSON | `eShopPorted/appsettings.json` | Connection string, log levels, host filtering, feature switches |
| Ported development settings | JSON | `eShopPorted/appsettings.Development.json` | Development log levels |
| Ported launch profiles | JSON | `eShopPorted/Properties/launchSettings.json` | Local IIS Express/project URLs and Development environment |
| Ported runtime configuration | XML | `eShopPorted/app.config` | Server GC setting |
| Solution/project build settings | MSBuild | `.sln` and three `.csproj` files | Targets and Debug/Release configuration |

No config server, Key Vault, Vault, AWS Secrets Manager, Kubernetes ConfigMap, or external configuration repository was identified.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Development symbols and local debugging | No profile-specific dependency set identified |
| Release | MSBuild `Release` configuration | Optimized legacy build and release transform | `Web.Release.config` transforms the connection string |
| Ported development | `ASPNETCORE_ENVIRONMENT=Development` in launch profiles | Local development settings and developer exception page | Uses `appsettings.Development.json` |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Legacy default | ASP.NET configuration | `Web.config` | Mock data and customization data disabled; LocalDB selected |
| Legacy Debug/Release | MSBuild web transforms | `Web.Debug.config`, `Web.Release.config` | Transforms the connection string |
| Ported default | ASP.NET Core host | `appsettings.json` | Mock data and customization data enabled |
| Ported Development | `ASPNETCORE_ENVIRONMENT` | `appsettings.Development.json` and launch settings | More verbose logging; developer exception page |

## Properties Inventory

### `eShopLegacyMVC`

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `UseMockData` | `false` | Default | `Web.config` |
| `UseCustomizationData` | `false` | Default | `Web.config` |
| `CatalogDBContext` | LocalDB database with integrated security | Default; transformed for Debug/Release | `Web.config` |
| `webpages:Enabled` | `false` | Default | `Web.config` |
| `ClientValidationEnabled` | `true` | Default | `Web.config` |
| `UnobtrusiveJavaScriptEnabled` | `true` | Default | `Web.config` |

### `eShopPorted`

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `ConnectionStrings:DefaultConnection` | LocalDB connection using trusted connection | Default | `appsettings.json` |
| `Logging:LogLevel:Default` | `Warning` | Default | `appsettings.json` |
| `Logging:LogLevel:*` | `Information` for System and Microsoft | Default | `appsettings.json` |
| `AllowedHosts` | `*` | Default | `appsettings.json` |
| `UseMockData` | `true` | Default | `appsettings.json` |
| `UseCustomizationData` | `true` | Default | `appsettings.json`; no use of this setting was found in the ported startup path |
| `Logging:LogLevel:Default` | `Debug` | Development | `appsettings.Development.json` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| `eShopLegacyMVC` | IIS/IIS Express; no custom heap options found | Not specified | Not specified |
| `eShopPorted` | ASP.NET Core host; local profile selects Development | Not specified | Not specified |
| `eShopLegacy.Utilities` | Loaded as a project library | Not applicable | Not applicable |

No explicit CPU, memory, or instance allocation was found.

## Startup Dependency Chain

1. The web host reads application configuration and initializes MVC/Web API routing.
2. Autofac registers the mock service or database-backed catalog service according to `UseMockData`.
3. In `eShopPorted`, EF Core SQL Server is registered only when mock data is disabled; in the checked-in default mode, no database connection is needed.
4. The legacy database initializer seeds the database and creates sequence objects when it initializes the database.

No readiness probes, service wait scripts, or inter-service startup dependencies were identified.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| Legacy `CatalogDBContext` connection | Database connection | LocalDB with integrated security; no password value |
| Ported `DefaultConnection` | Database connection | LocalDB trusted connection; no password value |

No hard-coded application password, API key, or cryptographic key was identified. No secret store, connection-string encryption mechanism, or managed identity configuration was found.

### Secrets Provisioning Workflow

No secret provisioning workflow is configured. Both connection strings use local database settings and integrated/trusted security; no secrets are referenced from an external provider.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `false` legacy; `true` ported | `Web.config` or `appsettings.json`; selects mock versus database service |
| `UseCustomizationData` | `false` legacy; `true` ported | `Web.config` selects CSV/custom versus preconfigured seed data; ported value appears unused |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Legacy MVC app | .NET Framework 4.7.2 | `src/eShopLegacyMVC/eShopLegacyMVC.csproj` |
| Ported app | `net461`, ASP.NET Core 2.2 | `eShopPorted/eShopPorted.csproj` and package references |
| Utilities library | .NET Framework 4.6.1 | `eShopLegacy.Utilities/eShopLegacy.Utilities.csproj` |
| Entity Framework | 6.2.0 | Legacy project references |
| Entity Framework Core | 2.2.6 | Ported project references |
| Autofac | 6.1.0 legacy; 4.9.1 ported | Project references |
| log4net | 2.0.10 | Project references |
| Application Insights | 2.9.1 | Legacy package manifest |
| Build system | MSBuild / SDK-style project for ported app | Solution and project files |
