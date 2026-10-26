# Configuration & Externalized Settings Inventory

Configuration is spread across the legacy Web.config model, environment-specific transforms, and Docker Compose overrides. Environment variables, optional Azure Key Vault configuration, and user-secrets builders are available for external settings; the local compose configuration also contains a fixed development SQL credential.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web.config | Runtime configuration | `src/eShopModernizedWebForms/Web.config` | Application settings, connection-string reference, and configuration builders |
| Web.Debug.config | Build transform | `src/eShopModernizedWebForms/Web.Debug.config` | Debug-specific configuration transform |
| Web.Release.config | Build transform | `src/eShopModernizedWebForms/Web.Release.config` | Release-specific configuration transform |
| Docker Compose | Container configuration | `docker-compose.yml`, `docker-compose.override.yml` | Web and SQL services, port mapping, environment overrides, and SQL health check |
| Environment file | Environment-variable template | `.env` | Example Azure settings are commented; values are not supplied there |
| Application Insights | Telemetry configuration | `src/eShopModernizedWebForms/ApplicationInsights.config` | Telemetry SDK configuration |
| log4net | Logging configuration | `src/eShopModernizedWebForms/log4Net.xml` | Logging appenders and levels |
| Bundle configuration | UI asset configuration | `src/eShopModernizedWebForms/Bundle.config` | Client-side asset bundles |
| Azure Key Vault | External secret source | Web.config configuration builder | Optional Key Vault builder; no URI or value is reproduced here |
| Environment and user secrets | External configuration sources | Web.config configuration builders | Environment and user-secrets builders are registered |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug|AnyCPU` configuration | Development build with `DEBUG;TRACE` symbols | Legacy Web Application targets |
| Release | MSBuild `Release|AnyCPU` configuration | Release build with `TRACE` symbol and release transform | Legacy Web Application targets |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default/local | Web.config defaults | `Web.config` | Mock data enabled; Azure storage, managed identity, and Azure AD disabled |
| Docker Compose | Compose environment variables | `docker-compose.yml`, `docker-compose.override.yml` | Local SQL connection and web port; mock-data and storage/auth flags overridden |
| Debug | Build transform | `Web.Debug.config` | Development connection settings |
| Release | Build transform | `Web.Release.config` | Release connection settings |

## Properties Inventory

Sensitive values are intentionally omitted. External references and setting names are retained.

### eShopModernizedWebForms

| Property Key | Default / Value | Profiles | Source |
|---|---|---|---|
| `UseMockData` | `true` | Local; overridden in Compose | Web.config / environment |
| `UseCustomizationData` | `false` | Local; overridden in Compose | Web.config / environment |
| `UseAzureStorage` | `false` | Local; overridden in Compose | Web.config / environment |
| `UseAzureManagedIdentity` | `false` | Local | Web.config / environment |
| `UseAzureActiveDirectory` | `false` | Local; overridden in Compose | Web.config / environment |
| `ConnectionString` | Externalized; value omitted | Compose / deployment | Environment or connection-string configuration builder |
| `StorageConnectionString` | Externalized; value omitted | Azure storage profile | Environment / Key Vault |
| `AppInsightsInstrumentationKey` | Externalized; value omitted | Telemetry profile | Environment |
| `AzureActiveDirectoryClientId` | Externalized; value omitted | Azure AD profile | Environment |
| `AzureActiveDirectoryTenant` | Externalized; value omitted | Azure AD profile | Environment |
| `AzureActiveDirectoryInstance` | Microsoft identity authority template | All profiles | Web.config |
| `PostLogoutRedirectUri` | Localhost in Web.config | Web.config / Compose | Configuration value; Compose supplies a host-based URI |
| `KeyVaultName` | Externalized; value omitted | Key Vault profile | Web.config configuration builder |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopModernizedWebForms | IIS-hosted .NET Framework application; container listens on port 80 | Not specified | Not specified |
| SQL Server | SQL Server container runtime | Not specified | One compose service |

No explicit CPU, memory limit, or application instance scaling setting was found.

## Startup Dependency Chain

1. Docker Compose starts the SQL Server service and defines a `sqlcmd` health check.
2. The Web Forms application uses the configured SQL connection when accessing catalog data.
3. No Compose `depends_on` readiness condition or application-level wait-for-database mechanism was identified.
4. Azure Blob Storage, Azure Active Directory, and Application Insights are external and are used only when their corresponding configuration is enabled.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| Catalog database connection | Connection string | Environment/configuration builder; values omitted |
| Azure Storage connection | Storage credential | Environment or optional Key Vault; values omitted |
| Application Insights instrumentation key | Telemetry credential | Environment; value omitted |
| Azure identity client settings | Identity configuration | Environment; values omitted |
| SQL Server local compose credential | Database password | Fixed development value in Compose; value omitted |

### Secrets Provisioning Workflow

For deployment, Web.config can resolve settings from environment variables, Azure Key Vault, or user secrets. Azure managed identity is an optional SQL connection mechanism. The local compose setup supplies a development SQL password directly; no production provisioning pipeline or role assignment was identified in this subproject.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | Web.config / environment |
| `UseCustomizationData` | `false` | Web.config / environment |
| `UseAzureStorage` | `false` | Web.config / environment |
| `UseAzureManagedIdentity` | `false` | Web.config / environment |
| `UseAzureActiveDirectory` | `false` | Web.config / environment |

No dedicated feature-flag framework was identified.

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework | 4.7.2 | `eShopModernizedWebForms.csproj` |
| ASP.NET Web Forms / IIS | .NET Framework 4.7.2 | Project configuration |
| Entity Framework | 6.3.0 | `packages.config` |
| OWIN | 4.2.2 | `packages.config` |
| Autofac | 4.9.4 | `packages.config` |
| Azure Storage SDK | 9.3.3 | `packages.config` |
| Application Insights | 2.11.x | `packages.config` |
| log4net | 2.0.10 | `packages.config` |
| MSBuild Web Application targets | Visual Studio tooling dependency | Legacy project import |
| Container base image | Windows IIS image | `src/eShopModernizedWebForms/Dockerfile` |
