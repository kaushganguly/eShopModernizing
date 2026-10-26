# Configuration & Externalized Settings Inventory

Configuration is centered on `Web.config` and its Debug/Release transforms, with configuration builders supporting user secrets, environment variables, and optional Azure Key Vault values. No secret values are reproduced in this inventory.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web application settings | .NET Framework configuration | `src/eShopModernizedWebForms/Web.config` | Connection strings and app settings use `Secrets`, `Environment`, and `AzureKeyVault` builders |
| Debug transform | Build transform | `src/eShopModernizedWebForms/Web.Debug.config` | Overrides selected app settings for Debug builds |
| Release transform | Build transform | `src/eShopModernizedWebForms/Web.Release.config` | Overrides selected app settings and removes the compilation debug attribute |
| Logging | XML configuration | `src/eShopModernizedWebForms/log4Net.xml` | log4net configuration |
| Telemetry | XML configuration | `src/eShopModernizedWebForms/ApplicationInsights.config` | Application Insights configuration |
| Client bundles | XML configuration | `src/eShopModernizedWebForms/Bundle.config` | Static asset bundling/minification |
| Package references | NuGet manifest | `src/eShopModernizedWebForms/packages.config` | 147 declared package entries |
| User secrets | .NET configuration builder | User Secrets provider | Configured for connection strings and app settings |
| Environment variables | .NET configuration builder | `Environment` builder | Supplies external overrides |
| Azure Key Vault | Custom configuration builder | Optional Key Vault builder | Vault name is supplied by configuration; builder is marked optional |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Local/debug build; applies `Web.Debug.config` | Legacy MSBuild Web Application project targets |
| Release | MSBuild `Release` configuration | Release packaging; applies `Web.Release.config` | Legacy MSBuild Web Application project targets |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Mock/local | App settings; currently the default configuration | `Web.config` | Mock catalog and local image behavior are selected by feature settings |
| Database-backed | App settings | `Web.config` plus external configuration providers | SQL Server catalog context; connection string or managed-identity token |
| Azure storage | App settings | `Web.config` plus external configuration providers | Azure Blob image service |
| Azure identity | App settings | `Web.config` plus external configuration providers | OpenID Connect sign-in and managed identity options |
| Debug/Release deployment | Build transform | `Web.Debug.config`, `Web.Release.config` | Environment-specific settings; Release removes the debug compilation attribute |

No `appsettings.{Environment}.json` or separate runtime profile files were identified.

## Properties Inventory

The following non-secret app settings are defined in `Web.config`; the values shown are booleans. Credentials and connection strings are masked.

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `UseMockData` | `true` | Web.config and transforms | `Web.config` |
| `UseCustomizationData` | `false` | Web.config and transforms | `Web.config` |
| `UseAzureManagedIdentity` | `false` | Web.config and transforms | `Web.config` |
| `UseAzureStorage` | `false` | Web.config and transforms | `Web.config` |
| `UseAzureActiveDirectory` | `false` | Web.config and transforms | `Web.config` |
| `KeyVaultName` | `[externalized]` | Configuration providers | `Web.config` |
| `StorageConnectionString` | `[MASKED/externalized]` | Configuration providers | `Web.config` |
| `CatalogDBContext` connection string | `[MASKED/externalized]` | Configuration providers | `Web.config` |
| Azure AD client ID, tenant, instance, and post-logout URI | `[configured]` | Configuration providers | `Web.config` |
| Application Insights instrumentation key | `[externalized]` | Configuration providers | Application settings |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopModernizedWebForms | IIS-hosted .NET Framework application; no explicit runtime flags found | Not specified | Not specified |
| Redis, SQL Server, Azure Storage, identity, telemetry | Managed externally | Not specified | Not specified |

## Startup Dependency Chain

1. IIS starts the Web Forms application.
2. `Application_Start` registers routes and bundles, builds the Autofac container, configures the database initializer, initializes catalog images, and configures telemetry.
3. Database access requires a reachable SQL Server when mock mode is disabled.
4. Azure image initialization requires Blob Storage when Azure storage is enabled; Azure identity or Key Vault access is required only when those options are selected.
5. No explicit service wait-for mechanism, health-check readiness probe, or startup timeout was identified.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` | SQL Server connection information | `[MASKED; configuration builders]` |
| `StorageConnectionString` | Azure Storage connection information | `[MASKED; configuration builders]` |
| Azure AD / Key Vault settings | Identity and endpoint configuration | References only; no secret values included |
| Application Insights instrumentation key | Telemetry configuration | `[MASKED/externalized]` |

### Secrets Provisioning Workflow

`Web.config` applies User Secrets, Environment, and Azure Key Vault configuration builders to connection strings and app settings. An optional custom Key Vault builder obtains its vault name from configuration. When managed identity is selected, `ManagedIdentitySqlConnectionFactory` requests a token for Azure SQL and uses it on the SQL connection. The repository does not document the deployment identity’s precise access assignments or secret-provisioning automation.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | App setting |
| `UseCustomizationData` | `false` | App setting |
| `UseAzureManagedIdentity` | `false` | App setting |
| `UseAzureStorage` | `false` | App setting |
| `UseAzureActiveDirectory` | `false` | App setting |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework | 4.7.2 | Project file and Web.config |
| ASP.NET Web Forms | System.Web on .NET Framework 4.7.2 | Project and Web.config |
| Entity Framework | 6.3.0 | `packages.config` |
| Autofac | 4.9.4; Autofac.Web 4.0.0 | `packages.config` |
| OWIN / OpenID Connect | Microsoft.Owin 4.2.2; authentication package set includes 4.0.1 | `packages.config` |
| Azure Storage | WindowsAzure.Storage 9.3.3 | `packages.config` |
| Redis session provider | 4.0.1 | `packages.config` |
| Application Insights | 2.11.x | `packages.config` |
| Build format | Legacy MSBuild Web Application project | `.csproj` |
