# Configuration & Externalized Settings Inventory

Configuration is split between `Web.config`, environment-specific transforms, telemetry/logging files, and Docker Compose overrides. Secrets can be supplied through configuration builders for user secrets, environment variables, or optional Azure Key Vault integration.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web configuration | XML application settings and connection strings | `src/eShopModernizedMVC/Web.config` | Environment, user-secret, and optional Key Vault builders are configured |
| Debug transform | XML transform | `src/eShopModernizedMVC/Web.Debug.config` | Contains only example transform content |
| Release transform | XML transform | `src/eShopModernizedMVC/Web.Release.config` | Removes the `debug` compilation attribute |
| Logging | XML | `src/eShopModernizedMVC/log4Net.xml` | Configures log4net appenders and layouts |
| Telemetry | XML | `src/eShopModernizedMVC/ApplicationInsights.config` | Includes an Application Insights instrumentation key |
| Container deployment | Docker Compose YAML | `docker-compose.yml`, `docker-compose.override.yml`, `docker-compose.override.nosql.yml` | Supplies environment overrides and SQL Server configuration |
| Container image | Dockerfile | `src/eShopModernizedMVC/Dockerfile` | .NET Framework ASP.NET base image 4.7.2; exposes port 80 |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Local development compilation | Debug symbols enabled; `DEBUG` and `TRACE` constants |
| Release | MSBuild `Release` configuration | Release compilation | Optimized build; `TRACE` constant; Web.Release transform removes debug setting |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Local / Web.config | Default when running without Compose | `Web.config` | Mock data and local image storage defaults; LocalDB connection |
| SQL database mode | Compose environment values | `docker-compose.override.yml` | Database mode enabled; SQL Server connection is injected |
| Mock/no-SQL mode | Compose environment values | `docker-compose.override.nosql.yml` | Mock catalog enabled; Azure storage disabled |
| Azure integrations | Application settings or environment | `Web.config`, Compose overrides | Azure storage, managed identity, and Azure AD toggles can be enabled |

## Properties Inventory

| Property Key | Default / Source | Profiles | Purpose |
|---|---|---|---|
| `UseMockData` | `true` | Local and mock/no-SQL; false in SQL Compose mode | Selects in-memory catalog implementation |
| `UseCustomizationData` | `false` | Local and Compose | Selects bundled customization/CSV seed data |
| `UseAzureStorage` | `false` | Local and Compose | Selects Azure image storage |
| `UseAzureManagedIdentity` | `false` | Local or environment override | Selects managed-identity database connection factory |
| `UseAzureActiveDirectory` | `false` | Local and Compose | Enables OWIN OpenID Connect authentication |
| `CatalogDBContext` | LocalDB connection in `Web.config`; replaced by environment in Compose | Local/database | SQL Server catalog connection |
| `StorageConnectionString` | Placeholder in `Web.config`; environment-provided in Compose | Azure storage | Azure Blob Storage connection |
| `AppInsightsInstrumentationKey` | Externalized through environment or config | All | Application Insights telemetry destination |
| `KeyVaultName` | Empty in `Web.config` | Optional Azure Key Vault | Selects Key Vault config builder |
| `AzureActiveDirectoryClientId` | Placeholder | Optional Azure AD | OIDC application identifier |
| `AzureActiveDirectoryTenant` | Placeholder | Optional Azure AD | OIDC tenant |
| `AzureActiveDirectoryInstance` | Microsoft identity authority template | Optional Azure AD | OIDC authority base |
| `PostLogoutRedirectUri` | Local HTTPS URL; Compose host override | Local and Compose | OIDC redirect target |

## Startup Parameters & Resource Requirements

| Service | Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopModernizedMVC | ASP.NET on .NET Framework 4.7.2, container port 80 | Not specified | Not specified |
| SQL Server | SQL Server Windows Developer container; environment supplied | Not specified | One Compose service |

No explicit JVM settings, container CPU limits, memory limits, or scaling counts were found.

## Startup Dependency Chain

1. Compose starts the `sql.data` service before the web app through `depends_on`; no readiness wait is declared in the base Compose file.
2. The MVC application starts and registers Autofac, routes, filters, and bundles.
3. When database mode is active, Entity Framework's database initializer creates the database/sequences and seeds catalog data.
4. Catalog image initialization runs before the OWIN pipeline is initialized; Azure storage may be contacted when that mode is enabled.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` | Database connection | Environment/configuration builder; local uses integrated authentication |
| `StorageConnectionString` | Storage credential | Environment/configuration builder or Key Vault; placeholder in file |
| `AppInsightsInstrumentationKey` | Telemetry identifier | Environment/configuration builder |
| `AzureActiveDirectoryClientId` and tenant | Identity configuration | Environment/configuration builder; placeholders in file |
| SQL Server container administrator credential | Database password | Compose override contains a development-only literal; value omitted |

### Secrets Provisioning Workflow

`Web.config` chains user secrets, environment variables, and an optional Azure Key Vault builder for connection strings and application settings. Compose injects environment variables for database and cloud settings. Managed identity can be selected for SQL access; otherwise the configured connection string is used. No actual credential values are reproduced in this inventory.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` locally | App setting or environment |
| `UseCustomizationData` | `false` | App setting or environment |
| `UseAzureStorage` | `false` | App setting or environment |
| `UseAzureManagedIdentity` | `false` | App setting or environment |
| `UseAzureActiveDirectory` | `false` | App setting or environment |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework | 4.7.2 | `eShopModernizedMVC.csproj` and Dockerfile |
| ASP.NET MVC | 5.2.7 | `packages.config` |
| Entity Framework | 6.3.0 | `packages.config` |
| Autofac MVC integration | 4.9.4 / 4.0.2 | `packages.config` |
| OWIN OpenID Connect | 4.0.1 | `packages.config` |
| WindowsAzure.Storage | 9.3.3 | `packages.config` |
| log4net | 2.0.12 | `packages.config` |
| Application Insights | 2.11.x | `packages.config` |
| Container base image | `mcr.microsoft.com/dotnet/framework/aspnet:4.7.2` | Dockerfile |
| Build format | Legacy MSBuild project | `.csproj` and solution |
