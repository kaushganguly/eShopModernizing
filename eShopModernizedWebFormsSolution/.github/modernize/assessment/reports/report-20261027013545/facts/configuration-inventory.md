# Configuration & Externalized Settings Inventory

Configuration is primarily XML-based, with Docker Compose environment overrides and optional configuration builders for environment variables, user secrets, and Azure Key Vault. No separate application profile files or active deployment-specific Web.config transforms were identified.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web application settings and connection strings | XML | `src/eShopModernizedWebForms/Web.config` | Environment, user-secrets, and Key Vault config builders are registered |
| Debug/Release transforms | XML transforms | `src/eShopModernizedWebForms/Web.Debug.config`, `Web.Release.config` | Mostly template examples; Debug transform removes the `debug` attribute |
| Telemetry | XML | `src/eShopModernizedWebForms/ApplicationInsights.config` | Application Insights modules, initializers, and sampling |
| Bundles | XML | `src/eShopModernizedWebForms/Bundle.config` | Client bundle configuration |
| Package versions | NuGet XML | `src/eShopModernizedWebForms/packages.config` | Legacy NuGet package inventory |
| Container service definitions | Docker Compose | `docker-compose.yml`, `docker-compose.override.yml` | Web Forms and SQL Server services; override maps ports and environment |
| Container runtime | Dockerfile | `src/eShopModernizedWebForms/Dockerfile` | ASP.NET Framework 4.7.2 image |
| Local Compose variables | Environment file | `.env` | Defines external DNS/IP setting; value omitted |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Development symbols and unoptimized output | `DEBUG;TRACE`; Web.Debug transform removes `debug` attribute |
| Release | MSBuild `Release` configuration | Optimized output with PDB-only symbols | `TRACE`; Release transform is a template and defines no active overrides |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Local default | `Web.config` appSettings | `Web.config` | Mock catalog, local SQL connection template; feature toggles default to the configured values |
| Docker Compose | Compose environment | `docker-compose.override.yml` plus `Web.config` config builders | Enables mock data, disables Azure storage, supplies SQL connection setting and externalized Azure values |
| Azure identity/storage | Application settings | `Web.config` plus environment/Key Vault builders | Optional managed identity, Azure Blob Storage, and Azure AD |

No ASP.NET Core `ASPNETCORE_ENVIRONMENT` profile files were found; this is a classic ASP.NET application.

## Properties Inventory

### eShopModernizedWebForms

| Property Key | Default / Value | Profiles | Source |
|---|---|---|---|
| `UseMockData` | `true` | Compose: `true`; set false for SQL-backed data | `Web.config`, Compose environment |
| `UseCustomizationData` | `false` | Compose: `false` | `Web.config`, Compose environment |
| `UseAzureManagedIdentity` | `false` | Set true for managed identity SQL token | `Web.config`, Compose environment |
| `UseAzureStorage` | `false` | Compose: `false` | `Web.config`, Compose environment |
| `UseAzureActiveDirectory` | `false` | Set true to enable OWIN OpenID Connect | `Web.config`, Compose environment |
| `CatalogDBContext` | Connection string includes a literal fallback credential `[MASKED]` | Compose overrides with service SQL connection | `Web.config` connection string and Compose environment |
| `StorageConnectionString` | Placeholder; actual value externalized | Compose references `ESHOP_AZURE_STORAGE_CONNECTION_STRING` | `Web.config`, Compose environment |
| `AppInsightsInstrumentationKey` | Empty unless supplied | Compose references `APP_INSIGHTS_INSTRUMENTATION_KEY` | `Web.config`, Compose environment |
| `KeyVaultName` | Empty placeholder | Environment/Key Vault use | `Web.config` |
| `AzureActiveDirectoryClientId` | Setup placeholder | Azure AD profile | `Web.config`, Compose environment |
| `AzureActiveDirectoryTenant` | Setup placeholder | Azure AD profile | `Web.config`, Compose environment |
| `AzureActiveDirectoryInstance` | `https://login.microsoftonline.com/{0}` | Azure AD profile | `Web.config` |
| `PostLogoutRedirectUri` | `https://localhost:44322/` | Compose uses external DNS/IP and port 5114 | `Web.config`, Compose environment |

The Compose SQL service defines an `SA_PASSWORD` value in the development override; its value is intentionally not reproduced here.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopModernizedWebForms | ASP.NET Framework 4.7.2 / IIS container, port 80 | Not specified | One Compose service |
| sql.data | SQL Server container, port 1433 | Not specified | One Compose service |

No JVM settings, CPU limits, container memory limits, or scale counts were found.

## Startup Dependency Chain

1. IIS starts the Web Forms application; `Application_Start` registers routes and bundles, builds Autofac, configures the database initializer when mock data is disabled, initializes images, and sets up telemetry.
2. The SQL service has a Compose health check, but the web service has no `depends_on` health gate in the inspected Compose files.
3. When Azure AD, Azure Key Vault, managed identity, or Azure Blob Storage is enabled, those external services/identities must be reachable for their respective application functions.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` SQL authentication | Database credential | Literal fallback in `Web.config`, `[MASKED]`; Compose also supplies a development SQL credential |
| `StorageConnectionString` | Azure storage credential | External environment variable / Key Vault configuration; placeholder in source |
| `SA_PASSWORD` | SQL Server container password | Compose override value not reproduced |
| Azure AD client settings | Identity configuration | Client ID and tenant are placeholders/environment-supplied; no client secret identified |
| `AppInsightsInstrumentationKey` | Telemetry key | Environment-supplied placeholder |

### Secrets Provisioning Workflow

The configuration pipeline can resolve settings through the `Secrets`, `Environment`, and optional `AzureKeyVault` config builders. Docker Compose passes sensitive settings as environment values, while Azure managed identity can obtain a SQL access token when enabled. No deployment pipeline or explicit Key Vault RBAC provisioning steps were found in this workspace; the configured application identity and required Key Vault permissions are deployment responsibilities.

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
| .NET Framework | 4.7.2 | `.csproj`, Dockerfile |
| ASP.NET Web Forms | .NET Framework 4.7.2 | `.csproj`, Web.config |
| Entity Framework | 6.3.0 | `packages.config` |
| Autofac | 4.9.4 | `packages.config` |
| OWIN OpenID Connect | 4.0.1 | `packages.config` |
| Windows Azure Storage | 9.3.3 | `packages.config` |
| Application Insights | 2.11.0 | `packages.config` |
| TypeScript tools | 3.0 | `.csproj` |
| Container base image | `mcr.microsoft.com/dotnet/framework/aspnet:4.7.2` | Dockerfile |
| Build | Legacy MSBuild project; Visual Studio WebApplication targets required | `.csproj`, `.sln` |
