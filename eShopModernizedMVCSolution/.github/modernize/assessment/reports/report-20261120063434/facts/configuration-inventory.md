# Configuration & Externalized Settings Inventory

The application’s runtime configuration is centered on `Web.config`, Docker Compose overrides, and build-time Web.config transforms. Configuration builders support user secrets, environment variables, and optional Azure Key Vault; the checked-in Docker override also contains a literal development SQL credential.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Application configuration | XML | `src/eShopModernizedMVC/Web.config` | App settings, connection string, config builders, authentication, and runtime defaults |
| View configuration | XML | `src/eShopModernizedMVC/Views/Web.config` | Razor view compilation settings |
| Build transforms | XML transforms | `src/eShopModernizedMVC/Web.Debug.config`, `Web.Release.config` | Environment-specific connection-string transforms |
| Telemetry | XML | `src/eShopModernizedMVC/ApplicationInsights.config` | Application Insights configuration |
| Container deployment | Compose YAML | `docker-compose.yml`, `docker-compose.override.yml`, `docker-compose.nobuild.yml`, `docker-compose.nobuild.nosql.yml`, `docker-compose.override.nosql.yml` | Service composition, environment overrides, ports, SQL health check |
| Container image | Dockerfile | `src/eShopModernizedMVC/Dockerfile` | Windows ASP.NET Framework image; exposes port 80 |
| Package manifest | NuGet XML | `src/eShopModernizedMVC/packages.config` | Framework and application dependencies |

`Web.config` declares `Secrets`, `Environment`, and optional `AzureKeyVault` configuration builders for connection strings and app settings. The Azure Key Vault name is a runtime selector; no external configuration repository was identified.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | Visual Studio/MSBuild `Debug` configuration | Development build and transform | `Web.Debug.config` transform |
| Release | Visual Studio/MSBuild `Release` configuration | Release packaging and transform | `Web.Release.config` transform |
| Container build | Docker Compose build | Publishes the web project into an IIS ASP.NET Framework image | Existing MSBuild/Docker build configuration |

The project is a legacy non-SDK-style ASP.NET Web Application targeting .NET Framework 4.7.2. No conditional package profile or alternate target framework was found.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Local/default | Application startup | `Web.config` | Mock data on; Azure Storage, managed identity, customization data, and Entra ID off |
| Docker SQL compose | Compose override | `docker-compose.override.yml` | SQL service connection environment; mock data setting and host port mappings |
| Alternate compose override | Explicit selection of `docker-compose.override.nosql.yml` | `docker-compose.override.nosql.yml` | Alternate mock/SQL settings; no distinct NoSQL provider was identified in application code |

No `ASPNETCORE_ENVIRONMENT` profile system is used; the application runs on classic ASP.NET and IIS.

## Properties Inventory

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `UseMockData` | `true` | Default and compose-overridden | `Web.config`, Docker Compose |
| `UseCustomizationData` | `false` | Default | `Web.config` |
| `UseAzureManagedIdentity` | `false` | Default | `Web.config` |
| `UseAzureStorage` | `false` | Default | `Web.config` |
| `UseAzureActiveDirectory` | `false` | Default | `Web.config` |
| `StorageConnectionString` | Placeholder; expected from external configuration | Environment, user secrets, or Key Vault | `Web.config` config builders |
| `KeyVaultName` | Empty | Deployment-specific | `Web.config` |
| `AzureActiveDirectoryClientId` | Setup placeholder | Entra ID deployment | `Web.config` |
| `AzureActiveDirectoryTenant` | Setup placeholder | Entra ID deployment | `Web.config` |
| `AzureActiveDirectoryInstance` | Microsoft login authority template | Entra ID deployment | `Web.config` |
| `PostLogoutRedirectUri` | Local HTTPS URL | Environment-specific | `Web.config` |
| `AppInsightsInstrumentationKey` | Not configured by default | Environment-specific | App settings and `ApplicationInsights.config` |
| `CatalogDBContext` connection string | LocalDB with trusted connection | Local or Compose SQL environment | `Web.config` and config builders |

Secret values are intentionally omitted. Database and storage values should be supplied through the configured externalization mechanisms; the Docker override files contain a checked-in development SQL credential.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopModernizedMVC | IIS / .NET Framework 4.7.2; container port 80 | Not specified | Compose defines one application container |
| SQL Server | SQL Server Windows Developer image; container port 1433 | Not specified | Compose defines one SQL container |

Docker Compose maps the MVC container to host port 5115 and SQL Server to host port 5433. No explicit CPU, memory, or scale limits were found.

## Startup Dependency Chain

1. The MVC container depends on the SQL Server service in the SQL-backed Compose configuration.
2. The SQL Server service has a Compose health check; the web application can start after its configured dependency.
3. During application startup, Autofac registers services, MVC routes and filters are configured, and the database initializer is configured only when mock data is disabled.
4. Image initialization runs during startup; telemetry initialization is added before request processing.

No separate discovery server, configuration server, or external startup wait utility was identified.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` SQL connection | Database credential | Local trusted connection by default; Docker override contains a development credential |
| `StorageConnectionString` | Storage account credential | Placeholder/externalized through `Secrets`, `Environment`, or optional Key Vault builders |
| `AppInsightsInstrumentationKey` | Telemetry credential | Environment/configuration value; not present as a concrete key in the default configuration |
| Entra ID client and tenant selectors | Identity configuration | Deployment placeholders in `Web.config` |

### Secrets Provisioning Workflow

The configuration-builder chain can read user secrets, environment variables, and optional Azure Key Vault values for application settings and connection strings. Managed identity is an opt-in SQL connection mode using Azure Service Authentication; no specific managed identity assignment or Key Vault RBAC policy is defined in this repository. Local Docker examples include a development SQL credential and should not be treated as production secret provisioning.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | `Web.config` or Compose environment |
| `UseCustomizationData` | `false` | `Web.config` |
| `UseAzureManagedIdentity` | `false` | `Web.config` |
| `UseAzureStorage` | `false` | `Web.config` |
| `UseAzureActiveDirectory` | `false` | `Web.config` |
| Optional Azure Key Vault builder | Enabled as optional builder | `KeyVaultName` setting |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework | 4.7.2 | Project file and Dockerfile |
| ASP.NET MVC | 5.2.7 | `packages.config` |
| Entity Framework | 6.3.0 | `packages.config` |
| Autofac | 4.9.4 | `packages.config` |
| OWIN / OpenID Connect | 4.2.x / 4.0.1 | `packages.config` |
| SQL Server container | Windows Developer image; tag not pinned in Compose | `docker-compose.yml` |
| ASP.NET container base | `mcr.microsoft.com/dotnet/framework/aspnet:4.7.2` | Dockerfile |
