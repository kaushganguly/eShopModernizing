# Configuration & Externalized Settings Inventory

Configuration is sourced from `Web.config`, its build transforms, Docker Compose, and environment variables, with optional configuration builders for user secrets, environment settings, and Azure Key Vault. The application has Debug and Release build configurations and feature flags for mock data, storage, customization data, managed identity, and OpenID Connect.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web application configuration | XML | `src/eShopModernizedWebForms/Web.config` | Connection string, app settings, ASP.NET settings, framework integrations, and configuration builders |
| Debug transform | XML transform | `src/eShopModernizedWebForms/Web.Debug.config` | No active overrides identified |
| Release transform | XML transform | `src/eShopModernizedWebForms/Web.Release.config` | Removes the `debug` compilation attribute |
| Container services | Docker Compose | `docker-compose.yml`, `docker-compose.override.yml` | Web app and SQL Server services; override sets development values and host ports |
| Environment file | Environment variables | `.env` | Defines `ESHOP_EXTERNAL_DNS_NAME_OR_IP`; values are not copied here |
| Secret providers | Configuration builders | `Web.config` | User Secrets, environment configuration builder, optional Azure Key Vault |
| Runtime image | Dockerfile | `src/eShopModernizedWebForms/Dockerfile` | ASP.NET Framework 4.7.2 base image, port 80 |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug|AnyCPU` | Debug compilation with `DEBUG;TRACE` symbols | No profile-specific package changes identified |
| Release | MSBuild `Release|AnyCPU` | Release compilation with `TRACE`; transform removes debug setting | No profile-specific package changes identified |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default/local | `Web.config` app settings; Compose override when containerized | `Web.config`, `docker-compose.override.yml` | Mock catalog data enabled by default; Azure storage and Entra ID disabled by default |
| SQL-backed | `UseMockData=false` | `Web.config` or environment/secret providers | Enables database initializer and EF-backed catalog service |
| Azure storage | `UseAzureStorage=true` | `Web.config` or environment/secret providers | Selects Azure Blob Storage image service |
| Managed identity | `UseAzureManagedIdentity=true` | `Web.config` or environment/secret providers | SQL connection factory obtains a database access token |
| Entra ID sign-in | `UseAzureActiveDirectory=true` | `Web.config` or environment/secret providers | OWIN OpenID Connect middleware is configured |

## Properties Inventory

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `CatalogDBContext` | Connection string is configured; credentials are supplied through secret/environment builders | SQL-backed | `Web.config` connection string |
| `KeyVaultName` | Empty | Optional Key Vault | `Web.config`; environment substitution |
| `UseMockData` | `true` | Default/local | `Web.config`; Compose override |
| `UseCustomizationData` | `false` | Default/local | `Web.config`; Compose override |
| `StorageConnectionString` | Placeholder | Azure storage | `Web.config`; Compose environment variable |
| `UseAzureManagedIdentity` | `false` | Managed identity | `Web.config` |
| `UseAzureStorage` | `false` | Azure storage | `Web.config`; Compose override |
| `UseAzureActiveDirectory` | `false` | Entra ID sign-in | `Web.config`; Compose override |
| `AzureActiveDirectoryClientId` | Portal placeholder | Entra ID sign-in | `Web.config`; environment/secret builder |
| `AzureActiveDirectoryTenant` | Tenant placeholder | Entra ID sign-in | `Web.config`; environment/secret builder |
| `AzureActiveDirectoryInstance` | Microsoft login URL template | Entra ID sign-in | `Web.config` |
| `PostLogoutRedirectUri` | `https://localhost:44322/` | Entra ID sign-in | `Web.config`; Compose override |
| `AppInsightsInstrumentationKey` | Not set in `Web.config` | Telemetry | `CatalogConfiguration`; environment/secret builder |
| `ESHOP_EXTERNAL_DNS_NAME_OR_IP` | Compose fallback address | Container networking | `.env` and Compose interpolation |
| SQL Server `SA_PASSWORD` | A literal development credential is present in Compose override; redacted | Local SQL container | `docker-compose.override.yml`; do not use as a production secret |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eshop.modernized.webforms | .NET Framework 4.7.2 ASP.NET image; exposes container port 80 | Not specified | Not specified |
| sql.data | SQL Server Windows Developer container | Not specified | Not specified |

The web host port is 5114 in the supplied Compose override; SQL Server is exposed on host port 5433 and container port 1433. No CPU or memory limits were found.

## Startup Dependency Chain

1. Docker Compose starts the Web Forms and SQL Server containers; no explicit `depends_on` relationship or application wait-for-health condition is configured.
2. The SQL container has a Compose health check.
3. On application startup, `Global.Application_Start` registers routes and bundles, configures Autofac, initializes the database when mock mode is disabled, initializes images, and configures telemetry.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` | SQL connection credentials | Supplied through `Secrets`, environment, or optional Key Vault configuration builders; value redacted |
| `StorageConnectionString` | Azure Storage credential | Placeholder in source configuration; environment/secret substitution supported |
| `AzureActiveDirectoryClientId` and tenant | Identity configuration | Placeholder values in source configuration; environment/secret substitution supported |
| SQL Server `SA_PASSWORD` in Compose override | Database password | Literal development credential committed in `docker-compose.override.yml`; value redacted |
| `AppInsightsInstrumentationKey` | Telemetry instrumentation key | Read from app settings; external value expected |

### Secrets Provisioning Workflow

The connection string and app settings are configured to use the User Secrets, Environment, and optional Azure Key Vault builders. Compose can supply values as environment variables. When managed identity is enabled, `ManagedIdentitySqlConnectionFactory` obtains a SQL access token via `AzureServiceTokenProvider`; otherwise the configured SQL connection string is used. The Compose override also contains a hard-coded local SQL Server credential, which is not appropriate for production use.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | `Web.config` or environment configuration |
| `UseCustomizationData` | `false` | `Web.config` or environment configuration |
| `UseAzureManagedIdentity` | `false` | `Web.config` or environment configuration |
| `UseAzureStorage` | `false` | `Web.config` or environment configuration |
| `UseAzureActiveDirectory` | `false` | `Web.config` or environment configuration |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Target framework | .NET Framework 4.7.2 | Project file and `Web.config` |
| ASP.NET container image | `mcr.microsoft.com/dotnet/framework/aspnet:4.7.2` | Dockerfile |
| Entity Framework | 6.3.0 | `packages.config` |
| Autofac | 4.9.4 | `packages.config` |
| OWIN | 4.2.2 | `packages.config` |
| Application Insights | 2.11.x | `packages.config` |
| log4net | 2.0.10 | `packages.config` |
| Build configurations | Debug and Release | Project file |
