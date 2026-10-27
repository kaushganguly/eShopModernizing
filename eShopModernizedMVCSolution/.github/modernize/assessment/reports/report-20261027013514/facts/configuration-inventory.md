# Configuration & Externalized Settings Inventory

Configuration is split between Web.config, environment-specific Web.config transforms, a local environment file, and Docker Compose overrides. Environment variables, user secrets, and an optional Azure Key Vault configuration builder provide secret/configuration sources; actual credential values are intentionally not reproduced here.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Application configuration | XML | `src/eShopModernizedMVC/Web.config` | Connection string, app settings, config builders, runtime and telemetry configuration |
| Debug transform | XML transform | `src/eShopModernizedMVC/Web.Debug.config` | No active override beyond the template |
| Release transform | XML transform | `src/eShopModernizedMVC/Web.Release.config` | Removes the `debug` compilation attribute |
| Environment variables | Configuration builder | Environment / Docker Compose | Overrides settings in Web.config; secret values omitted |
| User secrets | Configuration builder | Web.config `Secrets` builder | Identified by a user-secrets ID |
| Azure Key Vault | Configuration builder | Web.config `AzureKeyVault` builder | Optional; vault name is supplied through configuration |
| Local environment file | dotenv-style input | `.env` | Defines `ESHOP_EXTERNAL_DNS_NAME_OR_IP` for Compose substitution |
| Compose configuration | YAML | `docker-compose.yml`, `docker-compose.override.yml` | Application and SQL Server services, settings, ports, and SQL health check |
| Alternate Compose files | YAML | `docker-compose.override.nosql.yml`, `docker-compose.nobuild*.yml` | Additional local run modes; the primary configured database remains SQL Server |
| Container image | Dockerfile | `src/eShopModernizedMVC/Dockerfile` | Uses the .NET Framework ASP.NET 4.7.2 image |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | Visual Studio/MSBuild `Debug` configuration | Development build with symbols and `DEBUG;TRACE` constants | .NET Framework MSBuild and compiler packages |
| Release | Visual Studio/MSBuild `Release` configuration | Optimized build with `TRACE`; Release transform removes debug setting | Same package set as Debug |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Plain .NET / LocalDB | Run outside the application container | Web.config | LocalDB connection; mock data, mock images, and non-Azure auth defaults |
| Docker Compose | Compose service configuration | Web.config plus Compose override | SQL Server connection, host/container ports, mock-data and feature flags, external DNS |
| Azure integration | Explicit feature flags and deployment environment | Web.config plus environment/Key Vault configuration | Managed identity, Azure Blob Storage, Application Insights, and optional Azure AD |

There are no ASP.NET Core environment-specific `appsettings` files or `ASPNETCORE_ENVIRONMENT` profiles.

## Properties Inventory

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `CatalogDBContext` connection string | LocalDB | Plain .NET; replaced by Compose environment configuration | Web.config connection string and config builders |
| `UseMockData` | `true` | Compose may override | Web.config / environment |
| `UseCustomizationData` | `false` | Compose may override | Web.config / environment |
| `UseAzureStorage` | `false` | Compose may override | Web.config / environment |
| `UseAzureManagedIdentity` | `false` | Deployment-specific | Web.config / environment |
| `UseAzureActiveDirectory` | `false` | Deployment-specific | Web.config / environment |
| `StorageConnectionString` | Placeholder, masked | Azure storage profile | Web.config / environment / Key Vault |
| `AppInsightsInstrumentationKey` | Empty unless supplied | Telemetry profile | Web.config / environment |
| `AzureActiveDirectoryClientId` | Placeholder | Azure AD profile | Web.config / environment |
| `AzureActiveDirectoryTenant` | Placeholder | Azure AD profile | Web.config / environment |
| `AzureActiveDirectoryInstance` | Microsoft login URL template | Azure AD profile | Web.config |
| `PostLogoutRedirectUri` | Local HTTPS URL | Plain .NET; Compose override | Web.config / environment |
| `KeyVaultName` | Empty | Azure Key Vault profile | Web.config / environment |
| `ESHOP_EXTERNAL_DNS_NAME_OR_IP` | Compose default is used if unset | Docker Compose | `.env` / shell environment |
| SQL Server administrator password | Supplied by Compose configuration; masked | Docker Compose | Compose override; literal omitted |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopModernizedMVC | ASP.NET on .NET Framework 4.7.2; listens on container port 80 | Not specified | One Compose service instance by default |
| sql.data | SQL Server Windows Developer container | Not specified | One Compose service instance by default |

No explicit JVM options, container memory limits, CPU limits, or scaling configuration were found.

## Startup Dependency Chain

1. Docker Compose starts `sql.data` before `eshop.modernized.mvc` through `depends_on`.
2. The SQL service has a Compose health check in the override file, but the application dependency does not wait on a health condition.
3. If mock data is disabled, application startup configures the EF initializer, seeds catalog records, and creates SQL sequences.
4. Application startup initializes image storage and telemetry. Azure services are required only when their corresponding features are enabled.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| SQL Server administrator password | Database credential | Compose environment; value masked |
| `StorageConnectionString` | Azure Storage credential | Web.config placeholder / environment or Key Vault; value masked |
| Application Insights instrumentation key | Telemetry credential | Environment/configuration source; value masked |
| Azure AD client and tenant identifiers | Identity configuration | Web.config placeholders / environment; values not repeated |
| Optional Azure Key Vault name | Secret-store reference | Web.config placeholder / environment |

### Secrets Provisioning Workflow

The Web.config configuration builders can source values from user secrets, environment variables, and optional Azure Key Vault. Docker Compose supplies environment overrides for local runs. When managed identity is enabled, the application obtains a SQL access token through `AzureServiceTokenProvider`; no service-principal secret is specified in source. The repository also contains a static SQL administrator credential in a Compose override, which is not reproduced here.

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
| .NET Framework target | 4.7.2 | Application project file |
| ASP.NET MVC | 5.2.7 | `packages.config` |
| Entity Framework | 6.3.0 | `packages.config` |
| OWIN | 4.2.2 | `packages.config` |
| Autofac | 4.9.4 | `packages.config` |
| Application Insights | 2.11.0 | `packages.config` |
| Windows Azure Storage | 9.3.3 | `packages.config` |
| Docker base image | `mcr.microsoft.com/dotnet/framework/aspnet:4.7.2` | Dockerfile |
| Docker Compose file format | 3.4 | Compose files |
