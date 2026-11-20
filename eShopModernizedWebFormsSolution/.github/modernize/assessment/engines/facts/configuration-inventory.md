# Configuration & Externalized Settings Inventory

Configuration is primarily held in `Web.config`, with Debug and Release transforms, Compose environment overrides, and optional environment, user-secrets, and Azure Key Vault builders. Sensitive values are referenced or masked here rather than reproduced.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Main application configuration | XML | `src/eShopModernizedWebForms/Web.config` | App settings, connection string, authentication, EF, session, and binding redirects |
| Build transforms | XML | `src/eShopModernizedWebForms/Web.Debug.config`, `Web.Release.config` | Environment-specific Web.config transforms |
| Application telemetry | XML | `src/eShopModernizedWebForms/ApplicationInsights.config` | Request, dependency, and exception telemetry modules |
| Assembly logging | XML | `src/eShopModernizedWebForms/log4Net.xml` | log4net configuration |
| Compose service overrides | YAML | `docker-compose.override.yml` | Runtime environment overrides and local service ports; secret-like values not reproduced |
| Compose service definitions | YAML | `docker-compose.yml`, `docker-compose.nobuild.yml` | Web Forms and SQL Server containers |
| External configuration | Configuration builders | Environment, User Secrets, optional Azure Key Vault | Applied to `appSettings` and `connectionStrings` |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug|AnyCPU` | Debug symbols and `DEBUG;TRACE` compilation constants | Legacy .NET Framework Web Application targets |
| Release | MSBuild `Release|AnyCPU` | Release compilation and PDB-only symbols | Legacy .NET Framework Web Application targets |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default/IIS | Web.config defaults | `Web.config` | Mock catalog enabled; Azure Storage, managed identity, and Azure AD disabled |
| Compose | Docker Compose environment injection | `docker-compose.override.yml` | Sets SQL connection override, mock-data mode, optional telemetry/AAD values, and HTTP port mapping |
| Debug/Release transforms | Build configuration | `Web.Debug.config`, `Web.Release.config` | Transform-based settings; no separate ASP.NET Core-style environment profiles |

## Properties Inventory

| Property Key | Default or Reference | Profiles | Source |
|---|---|---|---|
| `UseMockData` | `true` | Default and Compose | `Web.config`; Compose override |
| `UseCustomizationData` | `false` | Default and Compose | `Web.config`; Compose override |
| `UseAzureManagedIdentity` | `false` | Default | `Web.config` |
| `UseAzureStorage` | `false` | Default and Compose | `Web.config`; Compose override |
| `UseAzureActiveDirectory` | `false` | Default and Compose | `Web.config`; Compose override |
| `StorageConnectionString` | Placeholder; masked | Environment/config builder or Compose variable | `Web.config`; `docker-compose.override.yml` |
| `CatalogDBContext` | Externalized SQL Server connection; masked | Environment, User Secrets, Key Vault, or Compose | `Web.config`; `docker-compose.override.yml` |
| `KeyVaultName` | Empty placeholder | Key Vault configuration | `Web.config` |
| `AzureActiveDirectoryClientId` | Setup placeholder | Compose/environment override | `Web.config`; `docker-compose.override.yml` |
| `AzureActiveDirectoryTenant` | Setup placeholder | Compose/environment override | `Web.config`; `docker-compose.override.yml` |
| `AzureActiveDirectoryInstance` | Microsoft identity authority URL template | Default | `Web.config` |
| `PostLogoutRedirectUri` | Local HTTPS URL | Compose override available | `Web.config`; `docker-compose.override.yml` |
| `AppInsightsInstrumentationKey` | Externalized; not set as a literal in the inspected appSettings | Compose/environment override | `CatalogConfiguration.cs`; `docker-compose.override.yml` |
| EF default connection factory | LocalDB factory | Default | `Web.config` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopModernizedWebForms | IIS-hosted .NET Framework 4.7.2; no additional startup flags identified | Not specified | Not specified |
| SQL Server | Container runtime settings in Compose; secret-like sample values omitted | Not specified | One service instance in Compose |

## Startup Dependency Chain

1. The Web Forms app requires a reachable SQL Server when mock mode is disabled.
2. Compose defines a SQL Server health check, but no explicit `depends_on` ordering/readiness gate for the web app was identified.
3. Optional Azure Blob Storage, Key Vault, OpenID Connect, and Application Insights integrations depend on their corresponding external configuration and services.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` connection string | SQL credentials / connection information | User Secrets, environment builder, Key Vault, or Compose; value masked |
| `StorageConnectionString` | Azure Storage credential | User Secrets, environment builder, Key Vault, or Compose; value masked |
| `AppInsightsInstrumentationKey` | Telemetry key | Environment / Compose variable; value masked |
| Azure AD client and tenant settings | Identity configuration | Configuration placeholders or environment variables |

### Secrets Provisioning Workflow

`Web.config` registers User Secrets, Environment, and optional Azure Key Vault configuration builders for app settings and connection strings. Environment-specific values can be supplied by Compose. The code can use a managed-identity SQL connection factory when enabled; the default configuration selects the app-settings factory. No repository-backed production credential provisioning sequence or explicit Key Vault access policy is documented.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | `Web.config` and environment override |
| `UseCustomizationData` | `false` | `Web.config` |
| `UseAzureManagedIdentity` | `false` | `Web.config` |
| `UseAzureStorage` | `false` | `Web.config` and environment override |
| `UseAzureActiveDirectory` | `false` | `Web.config` and environment override |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Target framework | .NET Framework 4.7.2 | Project file and Web.config |
| ASP.NET | Web Forms / System.Web | Project file |
| Entity Framework | 6.3.0 | `packages.config` |
| Autofac | 4.9.4 | `packages.config` |
| Application Insights | 2.11.x | `packages.config` |
| SQL provider | System.Data.SqlClient | Web.config |
| Build configurations | Debug and Release, Any CPU | Solution and project files |
