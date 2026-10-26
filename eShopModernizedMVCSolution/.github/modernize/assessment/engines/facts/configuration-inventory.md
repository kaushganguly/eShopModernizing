# Configuration & Externalized Settings Inventory

The application configuration is centered on `Web.config`, with build transforms and Compose overrides for local/container execution. Environment configuration builders and an optional Azure Key Vault builder externalize selected settings; sensitive values are not reproduced here.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web application settings | XML runtime configuration | `src/eShopModernizedMVC/Web.config` | App settings, SQL connection string, environment/secret configuration builders, and framework settings |
| Build transforms | XML transforms | `Web.Debug.config`, `Web.Release.config` | Environment-specific Web.config transformations |
| Docker Compose | YAML deployment configuration | `docker-compose*.yml` | App and SQL container definitions, environment overrides, ports, and SQL health check |
| Environment values | Environment variables | Compose and root `.env` | Compose supplies feature flags and externalized secrets; `.env` supplies the external DNS name |
| Application Insights | XML telemetry configuration | `src/eShopModernizedMVC/ApplicationInsights.config` | Telemetry configuration and instrumentation-key reference |
| Logging | XML logging configuration | `src/eShopModernizedMVC/log4Net.xml` | log4net appenders and logging behavior |
| Project and package manifests | MSBuild and NuGet XML | `eShopModernizedMVC.csproj`, `packages.config` | Target framework, build configurations, references, and package versions |
| Optional secret store | Azure Key Vault configuration builder | `Web.config` `AzureKeyVault` builder | Optional; configured through `KeyVaultName`, which is a placeholder in the checked-in configuration |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | `Debug|AnyCPU` MSBuild configuration | Local/debug build with symbols and optimization disabled | Same package references as Release |
| Release | `Release|AnyCPU` MSBuild configuration | Optimized deployment build | Same package references as Debug |
| Container build | Docker Compose build file | Build and run the MVC image with companion infrastructure | Uses the application Dockerfile; no separate NuGet profile was found |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Plain local development | Web.config defaults | `Web.config` | Mock catalog on; Azure storage, managed identity, and Azure AD off |
| Standard Compose | Compose environment overrides | `docker-compose.yml`, `docker-compose.override.yml` | Mock catalog on; host app port 5115 and SQL port 5433 |
| Alternate Compose override | Explicit alternate override file | `docker-compose.override.nosql.yml` | Mock catalog off; database and cloud settings supplied externally |
| Azure-integrated deployment | Environment/configuration-builder values | Web.config plus deployment environment | Can enable Azure Blob Storage, Azure AD, managed identity, and optional Key Vault resolution |

There are no `appsettings.{Environment}.json` files or ASP.NET Core environment profiles in this .NET Framework application.

## Properties Inventory

### `eShopModernizedMVC`

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `UseMockData` | `true` | Compose can override; alternate override sets false | Web.config appSettings |
| `UseCustomizationData` | `false` | Compose override | Web.config appSettings |
| `UseAzureStorage` | `false` | Compose override or Azure deployment | Web.config appSettings |
| `UseAzureManagedIdentity` | `false` | Compose/deployment override | Web.config appSettings |
| `UseAzureActiveDirectory` | `false` | Compose/deployment override | Web.config appSettings |
| `StorageConnectionString` | Externalized; value omitted | Environment/Secrets/optional Key Vault | Web.config appSettings |
| `AppInsightsInstrumentationKey` | Externalized; value omitted | Environment/Secrets/optional Key Vault | Web.config appSettings and Compose |
| `AzureActiveDirectoryClientId` | Portal-provided placeholder | Environment/Key Vault as configured | Web.config appSettings |
| `AzureActiveDirectoryTenant` | Tenant-name placeholder | Environment/Key Vault as configured | Web.config appSettings |
| `AzureActiveDirectoryInstance` | `https://login.microsoftonline.com/{0}` | Web.config | Web.config appSettings |
| `PostLogoutRedirectUri` | `https://localhost:44321/` | Web.config/Compose override | Web.config appSettings |
| `KeyVaultName` | Empty placeholder | Environment/deployment | Web.config appSettings |
| `CatalogDBContext` connection string | LocalDB with trusted connection | Compose or managed deployment override | Web.config connectionStrings; sensitive deployment values omitted |

The `Environment`, `Secrets`, and `AzureKeyVault` configuration builders are configured for connection strings and app settings. The Redis session-state provider block is commented out.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| `eshop.modernized.mvc` | ASP.NET on .NET Framework 4.7.2; container port 80 | Not specified | Not specified |
| `sql.data` | SQL Server container; port 1433 | Not specified | Not specified |

No CPU limits, memory limits, explicit runtime heap settings, or replica counts were identified.

## Startup Dependency Chain

1. In Compose SQL mode, the MVC container depends on `sql.data`; the SQL Compose override defines a health check, but startup is not configured to wait on a health-conditioned dependency.
2. With mock data enabled, the catalog service does not require SQL. When SQL mode is enabled, the application configures `CatalogDBInitializer`, which creates the database if needed and seeds catalog data.
3. When Azure image storage is enabled, application startup initializes the image container and uploads the configured local seed images.
4. Azure AD and Key Vault are optional external dependencies for their corresponding authentication and configuration paths.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` deployment connection string | Database credential | Externalized through Compose/environment configuration; literal values omitted |
| SQL container password | Database credential | Present in Compose development overrides; value omitted |
| `StorageConnectionString` | Azure storage credential | Environment/Secrets/optional Key Vault; value omitted |
| `AppInsightsInstrumentationKey` | Telemetry identifier | Environment/Secrets/optional Key Vault; value omitted |
| Azure AD client/tenant settings | Identity configuration | Placeholder/configuration-builder values; no secret value listed |

### Secrets Provisioning Workflow

The `Environment` and `Secrets` configuration builders resolve environment-provided settings; an optional Azure Key Vault builder can retrieve settings when `KeyVaultName` is configured. The repository does not document the deployment identity, Key Vault access policy/RBAC grants, or a provisioning pipeline. Compose contains development database credentials; their values are intentionally not repeated in this inventory.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | Web.config appSettings; Compose may override |
| `UseCustomizationData` | `false` | Web.config appSettings and Compose |
| `UseAzureStorage` | `false` | Web.config appSettings and Compose |
| `UseAzureManagedIdentity` | `false` | Web.config appSettings and deployment environment |
| `UseAzureActiveDirectory` | `false` | Web.config appSettings and Compose |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework | 4.7.2 | `eShopModernizedMVC.csproj` |
| ASP.NET MVC | 5.2.7 | `packages.config` |
| Entity Framework | 6.3.0 | `packages.config` |
| Autofac | 4.9.4 | `packages.config` |
| OWIN | 4.2.2 | `packages.config` |
| Azure Storage SDK | 9.3.3 | `packages.config` |
| Application Insights | 2.11.0 | `packages.config` |
| log4net | 2.0.12 | `packages.config` |
| MSBuild project format | ToolsVersion 15.0, legacy non-SDK project | `eShopModernizedMVC.csproj` |
