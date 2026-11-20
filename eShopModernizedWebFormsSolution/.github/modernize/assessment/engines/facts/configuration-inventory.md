# Configuration & Externalized Settings Inventory

Configuration comes from classic .NET Framework XML files, Docker Compose, and environment-variable-backed configuration builders. Secrets are intended to be supplied through user secrets, environment variables, or optional Azure Key Vault; sensitive values in checked-in configuration are masked in this inventory.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web application configuration | XML | `src/eShopModernizedWebForms/Web.config` | Connection strings and app settings use Secrets, Environment, and optional Azure Key Vault builders |
| Debug and Release transforms | XML transforms | `src/eShopModernizedWebForms/Web.Debug.config`, `Web.Release.config` | Release removes the debug compilation attribute; Debug is otherwise a template |
| Local container configuration | Docker Compose | `docker-compose.yml`, `docker-compose.override.yml` | Defines Web Forms and SQL Server services, port mappings, environment overrides, and SQL health check |
| Environment file | Environment variables | `.env` | Provides the external DNS/IP substitution; Compose also references Azure Storage, Application Insights, and Azure AD variables |
| Project and package manifests | MSBuild / NuGet | `src/eShopModernizedWebForms/eShopModernizedWebForms.csproj`, `packages.config` | Legacy project format, target framework, build references, and package versions |
| Telemetry and logging | XML | `src/eShopModernizedWebForms/ApplicationInsights.config`, `log4Net.xml` | Application Insights and log4net behavior |
| Bundling | XML | `src/eShopModernizedWebForms/Bundle.config` | Browser asset bundling configuration |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | Visual Studio/MSBuild `Debug` configuration | Local development build | Debug symbols and Web Forms build targets |
| Release | Visual Studio/MSBuild `Release` configuration | Release packaging; removes the debug compilation attribute | Same package set, release transform |

The project uses a legacy non-SDK `.csproj` format and targets .NET Framework 4.7.2.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default/local | `Web.config` defaults and Compose override | `Web.config`, `docker-compose.override.yml` | Mock catalog is enabled by default; Azure identity and Azure image storage are disabled by default |
| Customized seed data | `UseCustomizationData` setting | `Web.config` plus `Setup` CSV and image files | Loads catalog brands, types, items, and images from local files |
| Azure integrations | Feature settings and environment/secret sources | `Web.config`, Compose environment | Can enable Azure Blob Storage, managed identity, Azure AD, and telemetry |
| Debug / Release | MSBuild configuration | `Web.Debug.config`, `Web.Release.config` | Release removes the debug compilation setting |

No `appsettings.{Environment}.json` or ASP.NET Core environment profile was detected.

## Properties Inventory

### `eShopModernizedWebForms` properties

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `CatalogDBContext` connection string | Local SQL Server endpoint; credentials masked | Compose/local or externally overridden | `Web.config`, Compose `ConnectionString`, configuration builders |
| `UseMockData` | `true` | Default/local; can be overridden | `Web.config`, environment |
| `UseCustomizationData` | `false` | Default/local | `Web.config`, environment |
| `UseAzureManagedIdentity` | `false` | Azure SQL mode | `Web.config`, environment |
| `UseAzureStorage` | `false` | Azure image mode | `Web.config`, environment |
| `StorageConnectionString` | `[MASKED/PLACEHOLDER]` | Azure image mode | `Web.config`, `ESHOP_AZURE_STORAGE_CONNECTION_STRING` |
| `UseAzureActiveDirectory` | `false` | Optional OIDC authentication | `Web.config`, environment |
| `AzureActiveDirectoryClientId` / `AzureActiveDirectoryTenant` | Placeholder values | Azure AD mode | `Web.config`, Compose environment |
| `AzureActiveDirectoryInstance` | Microsoft login endpoint template | Azure AD mode | `Web.config` |
| `AppInsightsInstrumentationKey` | Not set in the base app settings | Optional telemetry | Compose environment `APP_INSIGHTS_INSTRUMENTATION_KEY` |
| `PostLogoutRedirectUri` | Local HTTPS URL | Azure AD mode | `Web.config`, Compose external host substitution |
| `KeyVaultName` | Empty | Optional Key Vault builder | `Web.config` and environment substitution |

The Compose file references `APP_INSIGHTS_INSTRUMENTATION_KEY`, `AZURE_ACTIVE_DIRECTORY_CLIENT_ID`, `AZURE_ACTIVE_DIRECTORY_TENANT`, `ESHOP_AZURE_STORAGE_CONNECTION_STRING`, and `ESHOP_EXTERNAL_DNS_NAME_OR_IP`.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| `eShopModernizedWebForms` | ASP.NET on .NET Framework 4.7.2; container exposes port 80 | No explicit limit found | No explicit count found |
| `sql.data` | SQL Server Windows Developer container | No explicit limit found | One Compose service |

Compose maps host port 5114 to Web Forms port 80 and host port 5433 to SQL Server port 1433. No CPU or memory quota was identified.

## Startup Dependency Chain

1. The Web Forms app initializes routes, bundles, Autofac, database configuration, image handling, and telemetry during `Application_Start`.
2. With mock data disabled, application data operations require the SQL Server service.
3. Compose configures a SQL `sqlcmd` health check, but no explicit `depends_on` readiness gate for the Web Forms service was identified.
4. Azure Blob Storage and Azure AD are used only when their respective options are enabled.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| SQL Server connection credentials | Database credential | `Web.config` / Compose setting `[MASKED]` |
| SQL Server container administrator credential | Database credential | Compose environment `[MASKED]` |
| Azure Storage connection string | Cloud credential | `ESHOP_AZURE_STORAGE_CONNECTION_STRING` |
| Application Insights instrumentation key | Telemetry credential | `APP_INSIGHTS_INSTRUMENTATION_KEY` |
| Azure AD client and tenant identifiers | Identity configuration | Compose environment; client identifier is externally supplied |
| Key Vault name | Secret-store selector | `${KeyVaultName}`; optional builder |

### Secrets Provisioning Workflow

The `Secrets`, `Environment`, and optional `AzureKeyVault` configuration builders resolve connection and application settings. Compose forwards local overrides and environment values. For Azure deployment, the application can use the Key Vault builder or managed identity for database access; no deployment-time secret provisioning or role-assignment workflow was found in the assessed workspace. A fixed local SQL credential is present in Compose configuration; its value is intentionally not reproduced here.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | Web.config / environment |
| `UseCustomizationData` | `false` | Web.config / environment |
| `UseAzureManagedIdentity` | `false` | Web.config / environment |
| `UseAzureStorage` | `false` | Web.config / environment |
| `UseAzureActiveDirectory` | `false` | Web.config / environment |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework | 4.7.2 | Project file and Web.config |
| ASP.NET Web Forms | System.Web | Project file and Web.config |
| Entity Framework | 6.3.0 | `packages.config` |
| Autofac | 4.9.4 | `packages.config` |
| OWIN / OpenID Connect | Package line 4.0.1 | `packages.config` |
| Application Insights | 2.11.0 | `packages.config` |
| Docker base image | `mcr.microsoft.com/dotnet/framework/aspnet:4.7.2` | `src/eShopModernizedWebForms/Dockerfile` |
| Build tool | MSBuild / Visual Studio legacy Web Application targets | `.sln` and `.csproj` |
