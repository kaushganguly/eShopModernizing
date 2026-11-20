# Configuration & Externalized Settings Inventory

Configuration is split across project files, WCF and WinForms XML files, and Docker Compose definitions. Local development defaults to SQL Server LocalDB, while Compose supplies a container database connection; SQL administrator credentials are embedded in Compose configuration and are intentionally masked here.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Solution | Visual Studio solution | `eShopModernizedNTier.sln` | Includes WCF service and SDK-style WinForms project |
| WCF service settings | .NET XML configuration | `src/eShopWCFService/Web.config` | WCF service, EF provider, connection string, metadata and runtime settings |
| WCF transforms | MSBuild XML transforms | `src/eShopWCFService/Web.Debug.config`, `Web.Release.config` | Environment-specific sample connection-string transforms |
| WinForms settings | .NET XML configuration | `src/eShopWinForms/App.config` | WCF client endpoint, binding, supported runtime, EF provider |
| Build projects | MSBuild project files | `src/eShopWCFService/eShopWCFService.csproj`, `src/eShopWinForms/eShopWinForms.csproj`, `src/eShopWinForms/eShopWinForms.fx.csproj` | Framework targets, build configurations, and package declarations |
| NuGet packages | Legacy package manifest | `src/eShopWCFService/packages.config` | Entity Framework package version |
| Docker Compose | Container configuration | `docker-compose.yml`, `docker-compose.override.yml` | Service definitions, ports, environment overrides, SQL settings |
| Compose build templates | Container configuration | `temp/docker-compose.yml`, `temp/docker-compose.override.yml`, `temp/docker-compose.ci.build.yml` | CI and local container build variants |
| WCF container | Dockerfile | `src/eShopWCFService/Dockerfile` | Uses the .NET Framework WCF base image |
| WCF client metadata | Connected service | `src/eShopWinForms/Connected Services/eShopServiceReference/ConnectedService.json` | Generated WCF client configuration metadata |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Development build; WCF transform uses the debug configuration | No profile-specific packages identified |
| Release | MSBuild `Release` configuration | Release build; WCF transform uses the release configuration | No profile-specific packages identified |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Local development | Default WCF configuration | `Web.config` | LocalDB connection; `ConnectionString` environment variable can override |
| Debug | WCF config transform | `Web.Debug.config` | Sample `ReleaseSQLServer` / `MyReleaseDB` connection transform |
| Release | WCF config transform | `Web.Release.config` | Sample `ReleaseSQLServer` / `MyReleaseDB` connection transform |
| Docker Compose | Compose environment | `docker-compose.override.yml` and CI variant | Connection string points to `sql.data`; SQL Server administrator password is masked |
| WinForms client | App configuration | `src/eShopWinForms/App.config` | WCF proxy endpoint is configured at `http://localhost:5113/CatalogService.svc` |

## Properties Inventory

### `eShopWCFService`

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `ConnectionString` | `name=EntityModel` when the environment variable is absent | Environment variable overrides default | `CatalogConfiguration.ConnectionString` |
| `EntityModel` | LocalDB `MSSQLLocalDB`, database `eShopDatabase` | Default local configuration | `Web.config` |
| `aspnet:UseTaskFriendlySynchronizationContext` | `true` | Default | `Web.config` |
| `system.web/compilation/@debug` | `true` | Default | `Web.config` |
| `serviceMetadata/httpGetEnabled` | `true` | Default | `Web.config` |
| `serviceMetadata/httpsGetEnabled` | `true` | Default | `Web.config` |
| `serviceDebug/includeExceptionDetailInFaults` | `false` | Default | `Web.config` |
| `system.webServer/directoryBrowse/@enabled` | `true` | Default | `Web.config` |

### `eShopWinForms`

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| WCF client endpoint | `http://localhost:5113/CatalogService.svc` | Default | `App.config` |
| WCF binding | `basicHttpBinding` | Default | `App.config` |
| Supported runtime | `.NETFramework,Version=v4.7.1` in legacy configuration | Legacy project | `App.config` |
| DPI awareness | `PerMonitorV2` | Default | `App.config` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| `eShopWCFService` | .NET Framework WCF base image; no explicit startup arguments identified | Not specified | Not specified |
| `eShopWinForms` | Windows desktop application; no explicit startup arguments identified | Not specified | One interactive client per user |
| `sql.data` | SQL Server container defaults | Not specified | One Compose service |

## Startup Dependency Chain

1. Docker Compose starts `sql.data` before `eshopwcfservice` using `depends_on`; no database health check or readiness wait is configured.
2. The WinForms client starts independently and expects the WCF endpoint at its configured localhost port.
3. Compose maps the WCF container port 80 to host port 5113 and SQL Server port 1433 to host port 5433.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| SQL Server administrator password | Database credential | Literal value in Compose environment configuration: `[MASKED]` |
| Application SQL connection string | Database connection | Compose environment override includes credential material: `[MASKED]` |

### Secrets Provisioning Workflow

No Key Vault, Vault, AWS Secrets Manager, encrypted configuration, or managed identity integration was found. Compose passes database settings into containers and currently contains literal credential material. No credential rotation or external secrets provisioning workflow is configured. Values are intentionally omitted from this report.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| Mock data selection | Not active | `useMockData` is present in `ApplicationModule`, but its registration logic is commented out |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| WCF service target | .NET Framework 4.6.1 | `eShopWCFService.csproj` |
| WCF container base | .NET Framework WCF 4.7.2 | `src/eShopWCFService/Dockerfile` |
| SDK-style WinForms project | .NET 6.0-windows | `eShopWinForms.csproj` |
| Legacy WinForms project | .NET Framework 4.7.1 | `eShopWinForms.fx.csproj` |
| Entity Framework | NuGet 6.1.3 and 6.4.4 | `packages.config` and project package references |
| WCF client packages | 4.8.0 | `eShopWinForms.csproj` |
| Web API Client | 5.2.3 and 5.2.7 | WinForms project files |
| SQL Server container | Developer image, no explicit tag | `docker-compose.yml` |
