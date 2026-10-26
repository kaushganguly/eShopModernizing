# Configuration & Externalized Settings Inventory

Configuration is split between Web.config, environment-specific Web.config transforms, container settings, and optional external configuration builders. No application password or key values are reproduced here.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web.config | Application and runtime configuration | `src/eShopModernizedWebForms/Web.config` | Base settings, connection string, configuration builders, runtime and session configuration |
| Web.Debug.config | Build/deployment transform | `src/eShopModernizedWebForms/Web.Debug.config` | Debug transform; custom error behavior is replaced for remote deployments |
| Web.Release.config | Build/deployment transform | `src/eShopModernizedWebForms/Web.Release.config` | Release transform; removes the debug compilation attribute and applies remote-only errors |
| Application Insights | Telemetry configuration | `src/eShopModernizedWebForms/ApplicationInsights.config` | Application Insights module and telemetry settings |
| log4net | Logging configuration | `src/eShopModernizedWebForms/log4Net.xml` | Logging appenders and diagnostics |
| Docker Compose | Container configuration | Workspace-root `docker-compose.yml`, `docker-compose.override.yml`, `docker-compose.nobuild.yml` | Web Forms and SQL Server services; environment overrides and health check |
| Compose environment file | Environment defaults | Workspace-root `.env` | External DNS value; cloud credential examples are comments/placeholders |
| Configuration builders | Externalized settings | Web.config | Environment, user-secrets, and optional Azure Key Vault builders |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Development build; `Web.Debug.config` transform | .NET Framework Web Application targets |
| Release | MSBuild `Release` configuration | Release build; `Web.Release.config` transform | .NET Framework Web Application targets |
| Docker Compose | Compose invocation | Runs the web application and SQL Server container | Microsoft Docker SDK project |

No Maven/Gradle-style build profiles or centralized package management file were found.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Base application | Default Web.config | `Web.config` | Mock catalog data enabled; Azure storage, managed identity, customization data, and Azure AD disabled in the sample defaults |
| Debug | Web deployment transform | `Web.Debug.config` | Debug-oriented custom error transform |
| Release | Web deployment transform | `Web.Release.config` | Removes the `debug` compilation attribute and configures remote-only custom errors |
| Container | Docker Compose environment | `docker-compose.override.yml` and `.env` | Overrides connection and storage settings, feature switches, and redirect URL |

## Properties Inventory

| Service | Property Key | Default or Source | Profiles | Source |
|---|---|---|---|---|
| Web Forms | `UseMockData` | `true` | Base; Compose override | Web.config / environment |
| Web Forms | `UseCustomizationData` | `false` | Base; Compose override | Web.config / environment |
| Web Forms | `UseAzureManagedIdentity` | `false` | Base | Web.config / environment |
| Web Forms | `UseAzureStorage` | `false` | Base; Compose override | Web.config / environment |
| Web Forms | `UseAzureActiveDirectory` | `false` | Base; Compose override | Web.config / environment |
| Web Forms | `StorageConnectionString` | Secret/external setting; value omitted | All | Environment, user secrets, or optional Key Vault |
| Web Forms | `CatalogDBContext` connection string | External setting; value omitted | Base; Compose override | Web.config connection string and configuration builders |
| Web Forms | Azure AD client ID and tenant | External settings; values omitted | When Azure AD is enabled | Web.config, environment, user secrets, or Key Vault |
| Web Forms | `AzureActiveDirectoryInstance` | Microsoft identity authority URL template | Base | Web.config |
| Web Forms | `PostLogoutRedirectUri` | Local development redirect URI | Base; Compose override | Web.config / environment |
| Web Forms | Application Insights instrumentation key | External setting; value omitted | Optional | Web.config / environment |
| Compose | `ESHOP_EXTERNAL_DNS_NAME_OR_IP` | Local network address | Container | `.env` |
| SQL Server container | `SA_PASSWORD` | Supplied as a Compose environment value; not repeated here | Container | `docker-compose.override.yml` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| Web Forms | .NET Framework 4.7.2 under IIS; no explicit heap/runtime tuning found | Not specified | Not specified |
| SQL Server | SQL Server Developer container | Not specified | One service in Compose |

Compose exposes the web service on host port 5114 and SQL Server on host port 5433. No CPU or memory limits were found.

## Startup Dependency Chain

1. IIS or the web container starts the application and reads Web.config.
2. Configuration builders resolve environment, user-secrets, and optional Key Vault values.
3. OWIN configures OpenID Connect when Azure AD is enabled; otherwise the configured fallback middleware is used.
4. `Global.Application_Start` registers routes and bundles, builds the Autofac container, configures the database initializer, initializes images, and installs telemetry.
5. In database-backed mode, the application requires the configured SQL Server connection when database initialization or a catalog request occurs; Azure Blob Storage is required only when enabled.

Docker Compose defines a SQL Server health check but no explicit `depends_on` or application wait-for-ready mechanism.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` connection string | Database credential | Configuration builders / environment; value omitted |
| `StorageConnectionString` | Azure Storage credential | Configuration builders / environment; value omitted |
| Azure AD client and tenant settings | Identity configuration | Configuration builders / environment; values omitted |
| `SA_PASSWORD` | SQL Server container credential | Docker Compose override; literal sample value intentionally omitted |

### Secrets Provisioning Workflow

The application can resolve values through environment variables, user secrets, and an optional Azure Key Vault configuration builder. Managed identity is available for Azure SQL access when selected. The Compose sample supplies a literal SQL Server password for its local service and uses the same value in the SQL health check. No identity permissions or deployment-time secret provisioning workflow is defined in this workspace.

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
| .NET Framework target | 4.7.2 | Project file and Dockerfile |
| ASP.NET Web Forms | Full .NET Framework | Project file |
| Entity Framework | 6.3.0 | `packages.config` |
| Autofac | 4.9.4 | `packages.config` |
| OWIN OpenID Connect | 4.0.1 | `packages.config` |
| Application Insights | 2.11.0 | `packages.config` |
| Azure Storage | 9.3.3 | `packages.config` |
| Docker base image | `mcr.microsoft.com/dotnet/framework/aspnet:4.7.2` | `src/eShopModernizedWebForms/Dockerfile` |
| Build format | Legacy MSBuild Web Application project | `.csproj` and solution |
