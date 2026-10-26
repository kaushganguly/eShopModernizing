# Configuration & Externalized Settings Inventory

Configuration is split across the classic ASP.NET application configuration, environment-driven container overrides, and telemetry/logging files. Runtime switches select mock data, storage, managed identity, and optional Azure AD authentication; secrets are intended to be supplied externally, although the development Compose override contains a fixed database credential.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| ASP.NET configuration | Application settings and connection strings | `src/eShopModernizedMVC/Web.config` | Uses Environment, Azure Key Vault, and Secrets configuration builders |
| Build transforms | Debug and Release transforms | `src/eShopModernizedMVC/Web.Debug.config`, `Web.Release.config` | Environment-specific Web.config transforms |
| Telemetry | Application Insights configuration | `src/eShopModernizedMVC/ApplicationInsights.config` | Telemetry identifier configured; value intentionally omitted |
| Logging | log4net XML | `src/eShopModernizedMVC/log4Net.xml` | Logging appenders and levels |
| Container deployment | Docker Compose | `docker-compose.yml`, `docker-compose.override.yml`, `docker-compose.override.nosql.yml`, `docker-compose.nobuild.yml`, `docker-compose.nobuild.nosql.yml` | Defines MVC and SQL Server containers and runtime environment overrides |
| Environment template | Environment variables | `.env` | Storage and telemetry variable examples are commented out |
| Container image | Dockerfile | `src/eShopModernizedMVC/Dockerfile` | Windows ASP.NET Framework 4.7.2 image; exposes port 80 |
| External secret/config sources | Azure Key Vault and environment configuration builders | Configured by `Web.config` | Optional Key Vault builder is implemented by `OptionalKeyVaultConfigurationBuilder` |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Development build with `DEBUG;TRACE` symbols | No profile-specific package additions identified |
| Release | MSBuild `Release` configuration | Optimized deployment build with `TRACE` symbol | No profile-specific package additions identified |
| Docker Compose variants | Select Compose file/override | Standard SQL-backed container setup or no-build/mock variants | Windows Docker SDK project; no profile-specific NuGet packages |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| ASP.NET default | Web.config defaults | `Web.config` | Mock catalog defaults to enabled; Azure AD and managed identity default to disabled |
| Debug | Build/deployment transform | `Web.Debug.config` | Overrides are applied during Debug packaging |
| Release | Build/deployment transform | `Web.Release.config` | Overrides are applied during Release packaging |
| Compose SQL/mock | Compose environment | `docker-compose.override.yml` | Enables mock catalog and supplies external service settings |
| Compose SQL-backed | Compose environment | `docker-compose.override.nosql.yml` | Sets mock data off; SQL is the intended catalog store |

The Compose file named `override.nosql.yml` is a no-SQL/mock-oriented variant; it does not define the SQL Server service.

## Properties Inventory

### eShopModernizedMVC

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `UseMockData` | `true` | Compose can override | Web.config / environment |
| `UseCustomizationData` | `false` | Compose can override | Web.config / environment |
| `UseAzureStorage` | `false` | Compose can override | Web.config / environment |
| `UseAzureManagedIdentity` | `false` | Environment or Web.config | Web.config / environment |
| `UseAzureActiveDirectory` | `false` | Environment or Web.config | Web.config / environment |
| `StorageConnectionString` | Empty/external | Compose environment variable | Environment/Key Vault configuration builders |
| `AppInsightsInstrumentationKey` | External | Compose environment variable | Environment/Key Vault configuration builders |
| `AzureActiveDirectoryClientId` | Placeholder | Compose environment variable | Web.config / environment |
| `AzureActiveDirectoryTenant` | Placeholder | Compose environment variable | Web.config / environment |
| `AzureActiveDirectoryInstance` | Microsoft identity login URL template | Web.config | Application setting |
| `PostLogoutRedirectUri` | Local HTTPS URL | Compose external host override | Web.config / environment |
| `CatalogDBContext` | External connection string | Compose SQL profile | Web.config connection string and environment builder |
| `KeyVaultName` | Empty | External configuration | Web.config |
| `webpages:Version` | `3.0.0.0` | All | Web.config |
| `webpages:Enabled` | `false` | All | Web.config |
| `ClientValidationEnabled` | `true` | All | Web.config |
| `UnobtrusiveJavaScriptEnabled` | `true` | All | Web.config |

Sensitive values and connection-string contents are not reproduced. Compose references external storage, telemetry, and Azure AD variables; the SQL Server development override contains a literal SA credential.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopModernizedMVC | .NET Framework 4.7.2 in IIS; container port 80 | Not specified | Not specified |
| sql.data | SQL Server Windows developer image | Not specified | One Compose service |

No JVM options, container CPU/memory limits, or horizontal instance configuration were found. The Compose override maps host port 5115 to application port 80 and host port 5433 to SQL Server port 1433.

## Startup Dependency Chain

1. SQL Server container starts before the MVC container according to `depends_on`.
2. The SQL health check uses `sqlcmd` in the Compose override, but the application dependency does not wait on a healthy condition.
3. MVC `Application_Start` registers dependencies and routes, initializes SQL schema/seed data when mock mode is off, initializes image storage, and configures telemetry.
4. Azure Blob Storage, Azure AD, and Application Insights are external services and are not started by Compose.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| SQL Server SA credential | Database password | Fixed development value in `docker-compose.override.yml`; value omitted |
| `StorageConnectionString` | Azure Storage credential | Environment/configuration-builder reference; value omitted |
| `AppInsightsInstrumentationKey` | Telemetry identifier | Environment/configuration-builder reference; value omitted |
| Azure AD client and tenant settings | Identity configuration | Environment/configuration-builder references; values omitted |

### Secrets Provisioning Workflow

`Web.config` enables Environment, Azure Key Vault, and Secrets configuration builders. `CatalogConfiguration` reads application settings, and the optional Key Vault builder tolerates initialization failures when configured as optional. SQL can use a connection string or an Azure managed identity token provider when `UseAzureManagedIdentity` is enabled. Compose supplies external variable references for storage, telemetry, and Azure AD; no deployment identity/RBAC provisioning workflow is defined in this subdirectory. The Compose development override also contains a hard-coded SQL Server SA password.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | Web.config / environment |
| `UseCustomizationData` | `false` | Web.config / environment |
| `UseAzureStorage` | `false` | Web.config / environment |
| `UseAzureManagedIdentity` | `false` | Web.config / environment |
| `UseAzureActiveDirectory` | `false` | Web.config / environment |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework | 4.7.2 | Project file and Dockerfile |
| ASP.NET MVC | 5.2.7 | `packages.config` |
| Entity Framework | 6.3.0 | `packages.config` |
| Autofac | 4.9.4 | `packages.config` |
| OWIN / OpenID Connect | 4.2.2 / 4.0.1 | `packages.config` |
| Application Insights | 2.11.x | `packages.config` |
| SQL Server | Windows Developer image | Compose files |
| Container base image | `mcr.microsoft.com/dotnet/framework/aspnet:4.7.2` | Dockerfile |
| MSBuild project format | Classic non-SDK project | `.csproj` |
