# Configuration & Externalized Settings Inventory

The project has Web.config as its primary runtime configuration and uses configuration builders for environment variables, user secrets, and optional Azure Key Vault resolution. Runtime toggles select mock data, storage, managed identity, and Azure Active Directory.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web.config | Application configuration | `src/eShopModernizedMVC/Web.config` | Connection strings, app settings, config builders, compilation, session state, and EF settings |
| Web.Debug.config | Transform | `src/eShopModernizedMVC/Web.Debug.config` | Debug deployment transform |
| Web.Release.config | Transform | `src/eShopModernizedMVC/Web.Release.config` | Release deployment transform |
| .env | Environment input | `eShopModernizedMVCSolution/.env` | Defines `ESHOP_EXTERNAL_DNS_NAME_OR_IP`; values are externally supplied |
| Docker Compose files | Deployment configuration | `docker-compose*.yml` in workspace root | Configure the MVC application and SQL Server container, ports, environment and dependencies |
| log4net.xml | Logging configuration | `src/eShopModernizedMVC/log4Net.xml` | Logging appenders and destinations |
| ApplicationInsights.config | Telemetry configuration | `src/eShopModernizedMVC/ApplicationInsights.config` | Application Insights modules and telemetry |
| Configuration builders | External secret/config providers | Web.config | Environment, User Secrets, and optional Azure Key Vault providers |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Local/debug build with symbols and no optimization | Legacy MSBuild web application targets |
| Release | MSBuild `Release` configuration | Optimized deployment build | Legacy MSBuild web application targets |

The project uses a non-SDK-style project file and targets .NET Framework 4.7.2.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Plain .NET/local | Web.config defaults | Web.config | Mock data enabled; Azure storage and Azure AD disabled; SQL LocalDB connection |
| Compose/container | Compose environment overrides | `docker-compose*.yml`, `.env` | SQL Server service connection and application port mapping |
| Debug/Release transforms | Web Deploy transform | Web.Debug.config / Web.Release.config | Database connection transform entries |

No `appsettings.{Environment}.json` files or ASP.NET Core environment profiles were found.

## Properties Inventory

### eShopModernizedMVC

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `CatalogDBContext` | LocalDB with trusted connection; no embedded password | Plain .NET; overridden externally for container/deployment | Web.config connection strings; Secrets, Environment, and AzureKeyVault builders |
| `KeyVaultName` | Empty | Optional Azure Key Vault | Web.config |
| `UseMockData` | `true` | Plain .NET; override supported | Web.config / environment / Key Vault |
| `UseCustomizationData` | `false` | Plain .NET; override supported | Web.config / environment / Key Vault |
| `StorageConnectionString` | Placeholder, not a usable credential | Azure storage when enabled | Web.config / environment / Key Vault |
| `UseAzureManagedIdentity` | `false` | Azure deployment override | Web.config / environment / Key Vault |
| `UseAzureStorage` | `false` | Azure storage deployment override | Web.config / environment / Key Vault |
| `UseAzureActiveDirectory` | `false` | Authentication deployment override | Web.config / environment / Key Vault |
| `AzureActiveDirectoryClientId` | Placeholder | Azure AD/OpenID Connect | Web.config / environment / Key Vault |
| `AzureActiveDirectoryTenant` | Placeholder | Azure AD/OpenID Connect | Web.config / environment / Key Vault |
| `AzureActiveDirectoryInstance` | `https://login.microsoftonline.com/{0}` | Azure AD/OpenID Connect | Web.config |
| `PostLogoutRedirectUri` | `https://localhost:44321/` | Local development; deployment override | Web.config |
| `webpages:Version` | `3.0.0.0` | All | Web.config |
| `webpages:Enabled` | `false` | All | Web.config |
| `ClientValidationEnabled` | `true` | All | Web.config |
| `UnobtrusiveJavaScriptEnabled` | `true` | All | Web.config |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopModernizedMVC | IIS/IIS Express-hosted .NET Framework; no JVM options | Not specified | Not specified |
| SQL Server | SQL Server Windows container in Compose configuration | Not specified | One service in Compose |

Compose maps the web application to host port 5115 and SQL Server to 1433 in the supplied override. No explicit CPU or memory limits were found.

## Startup Dependency Chain

1. The MVC application initializes Autofac, routes, filters, and bundles.
2. With mock data disabled, EF6 uses the configured SQL Server connection and database initializer.
3. Image service initialization runs during application startup; the Azure implementation accesses the configured blob container.
4. OWIN cookie and OpenID Connect middleware are initialized for authentication.

Compose declares a dependency from the MVC container to SQL Server; readiness behavior varies by Compose file. No general application readiness endpoint was found.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` | Database connection | Trusted LocalDB connection by default; deployment value supplied externally |
| `StorageConnectionString` | Azure Storage connection | Placeholder in Web.config; expected to be externally configured |
| `AzureActiveDirectoryClientId` / `AzureActiveDirectoryTenant` | Identity configuration | Placeholder values; expected to be supplied by deployment |
| Redis `accessKey` | Session-store credential | Example placeholder in commented configuration |

### Secrets Provisioning Workflow

The connection strings and app settings declare `Secrets`, `Environment`, and `AzureKeyVault` configuration builders. Deployments may supply values through environment variables or Azure Key Vault, and Azure SQL access can use the `AzureServiceTokenProvider` when managed identity is enabled. No actual password or key is embedded in the documented defaults.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | Web.config / environment / Key Vault |
| `UseCustomizationData` | `false` | Web.config / environment / Key Vault |
| `UseAzureManagedIdentity` | `false` | Web.config / environment / Key Vault |
| `UseAzureStorage` | `false` | Web.config / environment / Key Vault |
| `UseAzureActiveDirectory` | `false` | Web.config / environment / Key Vault |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework | 4.7.2 | `eShopModernizedMVC.csproj` |
| ASP.NET MVC | 5.2.7 | `packages.config` |
| Entity Framework | 6.3.0 | `packages.config` |
| Autofac MVC | 4.0.2 | `packages.config` |
| log4net | 2.0.12 | `packages.config` |
| Application Insights | 2.11.x | `packages.config` |
| MSBuild project format | Legacy non-SDK | `eShopModernizedMVC.csproj` |
