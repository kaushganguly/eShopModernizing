# Configuration & Externalized Settings Inventory

Configuration is split across classic ASP.NET configuration files, Docker Compose, and environment/secret providers. Runtime flags select mock versus external integrations; the checked-in Compose override contains a plaintext development database credential, which is intentionally masked in this inventory.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| ASP.NET configuration | XML | `src/eShopModernizedMVC/Web.config` | App settings, connection string, OWIN, logging, and config builders |
| Build transforms | XML transforms | `Web.Debug.config`, `Web.Release.config` | Environment-specific transform files |
| Application telemetry | XML | `ApplicationInsights.config` | Application Insights initializer and telemetry modules; key omitted |
| Docker Compose | YAML | `docker-compose.yml`, `docker-compose.override.yml` | Web/SQL services, environment settings, ports, and SQL health check |
| NuGet package inventory | XML | `packages.config` | Legacy package versions |
| Container build | Dockerfile | `src/eShopModernizedMVC/Dockerfile` | ASP.NET Framework 4.7.2 container image |
| User secrets | Configuration builder | `Web.config` | UserSecrets builder is registered |
| Environment variables | Configuration builder | `Web.config` / Compose | `EnvironmentConfigBuilder` resolves external settings |
| Azure Key Vault | Optional config builder | `Web.config` | Optional Key Vault builder uses the configured vault name |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug|AnyCPU` | Local development build; symbols enabled, optimization disabled | Legacy MSBuild, CodeDom compiler |
| Release | MSBuild `Release|AnyCPU` | Optimized deployment build | Same framework/package references; PDB-only debug information |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Local development | `Web.config` defaults | `Web.config` | Mock catalog on; cloud storage, managed identity, and Azure AD off |
| Docker Compose | Compose environment variables | `docker-compose.yml`, `docker-compose.override.yml` | SQL container connection, externalized storage/App Insights/AAD values, mapped HTTP port |
| Release transform | MSBuild/Web.config transform | `Web.Release.config` | Transformed deployment connection configuration |
| Azure-integrated deployment | Environment/config-builder values | `Web.config` and deployment environment | Optional Key Vault, managed identity, Azure Blob Storage, and Azure AD |

## Properties Inventory

| Property Key | Default / Source | Profiles | Source |
|---|---|---|---|
| `ConnectionStrings:CatalogDBContext` | LocalDB trusted connection; Compose overrides with SQL service connection (password masked) | Local / Compose / deployment | `Web.config`, Compose |
| `UseMockData` | `true` | Local default; Compose configurable | `Web.config`, environment |
| `UseCustomizationData` | `false` | Local / Compose | `Web.config`, environment |
| `UseAzureStorage` | `false` | Local / Compose / deployment | `Web.config`, environment |
| `StorageConnectionString` | Placeholder; Compose reads `ESHOP_AZURE_STORAGE_CONNECTION_STRING` | Azure storage | `Web.config`, Compose |
| `UseAzureManagedIdentity` | `false` | Local / Azure | `Web.config`, environment |
| `UseAzureActiveDirectory` | `false` | Local / Azure | `Web.config`, Compose |
| `AzureActiveDirectoryClientId` | Placeholder / environment variable | Azure AD | `Web.config`, Compose |
| `AzureActiveDirectoryTenant` | Placeholder / environment variable | Azure AD | `Web.config`, Compose |
| `AzureActiveDirectoryInstance` | Microsoft login endpoint template | Azure AD | `Web.config` |
| `PostLogoutRedirectUri` | Local HTTPS URL; Compose host-based HTTP URL | Local / Compose | `Web.config`, Compose |
| `KeyVaultName` | Empty by default; optional | Azure | `Web.config` |
| `AppInsightsInstrumentationKey` | Empty in app settings; Compose environment placeholder; telemetry config has an instrumentation identifier | Telemetry | `Web.config`, Compose, `ApplicationInsights.config` |
| `ClientValidationEnabled` | `true` | All | `Web.config` |
| `UnobtrusiveJavaScriptEnabled` | `true` | All | `Web.config` |
| `webpages:Enabled` | `false` | All | `Web.config` |
| `webpages:Version` | `3.0.0.0` | All | `Web.config` |

## Startup Parameters & Resource Requirements

| Service | Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopModernizedMVC | ASP.NET on .NET Framework 4.7.2; container exposes port 80 | Not specified | Not specified |
| sql.data | SQL Server Windows Developer container | Not specified | One Compose service |

No JVM settings, container memory limits, CPU requests, or scaling counts were found in the target Compose configuration.

## Startup Dependency Chain

1. `sql.data` starts as the SQL Server container; Compose defines a `sqlcmd` health check.
2. `eshop.modernized.mvc` declares `depends_on: sql.data`; Compose does not specify a healthy-condition dependency.
3. During application startup, Autofac registrations are created, MVC routes/filters/bundles are registered, EF initialization is configured, images are initialized, and telemetry is configured.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| SQL Server SA password | Database credential | Plaintext development value in `docker-compose.override.yml`; value masked |
| Catalog SQL connection string | Database credential | Config builder chain can source User Secrets, environment, and optional Key Vault; local default uses trusted LocalDB |
| Azure Storage connection string | Storage credential | Environment placeholder in Compose; app config uses a placeholder |
| Azure AD client/tenant values | Identity configuration | Environment placeholders / app settings |
| Application Insights instrumentation identifier | Telemetry configuration | Externalized setting; value omitted |

### Secrets Provisioning Workflow

The ASP.NET `connectionStrings` and `appSettings` sections enable User Secrets, Environment, and Azure Key Vault configuration builders. Compose can inject storage, telemetry, and identity values from environment variables. The SQL access-token provider uses `AzureServiceTokenProvider` when managed identity is enabled. No deployment RBAC assignments or secret-creation pipeline were found in this workspace. The Compose SQL credential is a development-only literal and is not reproduced here.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | App setting / environment |
| `UseCustomizationData` | `false` | App setting / environment |
| `UseAzureStorage` | `false` | App setting / environment |
| `UseAzureManagedIdentity` | `false` | App setting / environment |
| `UseAzureActiveDirectory` | `false` | App setting / environment |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework | 4.7.2 | Project file and Dockerfile |
| ASP.NET MVC | 5.2.7 | `packages.config` |
| Entity Framework | 6.3.0 | `packages.config` |
| Autofac | 4.9.4 | `packages.config` |
| OWIN / OpenID Connect | 4.2.2 / 4.0.1 | `packages.config` |
| log4net | 2.0.12 | `packages.config` |
| Application Insights | 2.11.0 | `packages.config` |
| Azure Storage SDK | 9.3.3 | `packages.config` |
| ASP.NET container image | 4.7.2 | Dockerfile |
| Docker Compose format | 3.4 | Compose files |
| TypeScript tools | 3.0 | Project file |
