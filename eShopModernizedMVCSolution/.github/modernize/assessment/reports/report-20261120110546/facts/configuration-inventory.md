# Configuration & Externalized Settings Inventory

The project uses Web.config and its build transforms, with Docker Compose overrides for container settings. Secrets builders are configured, but the Compose override also contains literal SQL credentials; sensitive values are omitted here.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web.config | Runtime settings | `src/eShopModernizedMVC/Web.config` | Uses Secrets, Environment, and optional Azure Key Vault configuration builders |
| Web.Debug.config | Build transform | `src/eShopModernizedMVC/Web.Debug.config` | Debug-specific connection string transform |
| Web.Release.config | Build transform | `src/eShopModernizedMVC/Web.Release.config` | Release-specific connection string transform |
| Docker Compose | Container topology | `docker-compose.yml` | Declares MVC application and SQL Server dependency |
| Docker Compose override | Container environment | `docker-compose.override.yml` | Overrides app settings and SQL connection; contains hard-coded SQL credentials (redacted) |
| Application Insights | Telemetry settings | `src/eShopModernizedMVC/ApplicationInsights.config` | Telemetry configuration |
| Project/package files | Build settings | `.csproj`, `packages.config` | Legacy MSBuild project and NuGet package declarations |
| Dockerfile and publish profile | Deployment settings | `src/eShopModernizedMVC/Dockerfile`, `Properties/PublishProfiles/FolderProfile.pubxml` | Windows IIS container and folder publishing |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Local debugging and debug transform | .NET Framework reference assemblies, MVC build packages |
| Release | MSBuild `Release` configuration | Release compilation and release transform | Same package set with release build properties |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Plain .NET | Default configuration | Web.config | Mock catalog enabled; SQL LocalDB; Azure storage and Azure AD disabled |
| Windows container | Compose service environment | `docker-compose.override.yml` | SQL container endpoint and app environment settings; SQL credentials are literal in the override |
| Azure storage | `UseAzureStorage` setting | Web.config or external builder values | Selects Blob Storage implementation |
| Azure AD | `UseAzureActiveDirectory` setting | Web.config or external builder values | Selects OpenID Connect authentication |

## Properties Inventory

### eShopModernizedMVC

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `UseMockData` | `true` | Plain .NET | Web.config |
| `UseCustomizationData` | `false` | Plain .NET | Web.config |
| `UseAzureManagedIdentity` | `false` | Plain .NET | Web.config |
| `UseAzureStorage` | `false` | Plain .NET | Web.config |
| `UseAzureActiveDirectory` | `false` | Plain .NET | Web.config |
| `StorageConnectionString` | Placeholder | Plain .NET / external override | Web.config configuration builders |
| `KeyVaultName` | Empty | Optional Key Vault | Web.config configuration builders |
| `AzureActiveDirectoryClientId` | Setup placeholder | Optional Azure AD | Web.config configuration builders |
| `AzureActiveDirectoryTenant` | Setup placeholder | Optional Azure AD | Web.config configuration builders |
| `AzureActiveDirectoryInstance` | Microsoft login URL template | Optional Azure AD | Web.config |
| `PostLogoutRedirectUri` | Local HTTPS URL | Optional Azure AD | Web.config |
| `CatalogDBContext` connection | LocalDB with integrated authentication | Plain .NET | Web.config |

Values marked as sensitive or supplied by container settings are not reproduced. The config builders include user secrets, environment variables, and optional Key Vault lookup.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopModernizedMVC | .NET Framework 4.7.2 under IIS; no explicit startup options found | No explicit limit found | No explicit count found |
| sql.data | SQL Server Windows container | No explicit limit found | One Compose service |

The application container exposes port 80. No CPU or memory limits were identified in Compose.

## Startup Dependency Chain

1. Compose starts the SQL service and declares it as an MVC service dependency; no health-based readiness wait is configured.
2. IIS starts the MVC application.
3. `Application_Start` registers Autofac, routes, filters, and bundles.
4. The application initializes the database when mock data is disabled, initializes images, and registers telemetry.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| SQL login credentials | Database credential | Literal in `docker-compose.override.yml` (values redacted) |
| Storage connection string | Azure storage credential | Placeholder in Web.config; container values may be environment-supplied |
| Azure AD client and tenant settings | Identity configuration | Setup placeholders or externalized settings |
| User secrets and Key Vault configuration | Secret provider references | Config builders configured; no secret values included |

### Secrets Provisioning Workflow

Web.config applies the user-secrets, environment, and optional Azure Key Vault builders to connection strings and app settings. Container deployments can supply values through Compose environment settings. The Compose override includes literal SQL credentials; no managed-identity permissions or secret-injection pipeline was found in the assessed workspace.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | Web.config / external builders |
| `UseCustomizationData` | `false` | Web.config / external builders |
| `UseAzureManagedIdentity` | `false` | Web.config / external builders |
| `UseAzureStorage` | `false` | Web.config / external builders |
| `UseAzureActiveDirectory` | `false` | Web.config / external builders |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework | 4.7.2 | `eShopModernizedMVC.csproj` |
| ASP.NET MVC | 5.2.7 | `packages.config` |
| Entity Framework | 6.3.0 | `packages.config` |
| Autofac | 4.9.4 | `packages.config` |
| OWIN Security | 4.0.1 | `packages.config` |
| log4net | 2.0.12 | `packages.config` |
| Application Insights | 2.11.0 | `packages.config` |
| Container base | .NET Framework ASP.NET 4.7.2 Windows image | `Dockerfile` |
| Build system | Legacy MSBuild project format | `.csproj` and `.sln` |
