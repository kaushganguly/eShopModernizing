# Configuration & Externalized Settings Inventory

The scoped workspace uses XML application configuration, MSBuild properties, root Compose overrides, and three alternate Compose files under `temp`. Connection selection has one environment override; sensitive literals are represented only as `[MASKED]`, and no external secret store is configured in the inspected sources.

All evidence paths are relative to `eShopModernizedNTier`. This is a static inventory, not validation of deployed configuration.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Server application / hosting | XML | `src\eShopWCFService\Web.config:3-64` | ASP.NET, WCF, EF provider, named connection |
| Server transforms | XML XDT | `src\eShopWCFService\Web.Debug.config:5-30`; `Web.Release.config:5-31` in same directory | Debug has no active overrides; Release removes compilation debug attribute |
| Desktop application | XML | `src\eShopWinForms\App.config:3-41` | Legacy runtime declaration, DPI and WCF/EF settings; generated client programmatic defaults take precedence in the actual construction path |
| Desktop user settings | XML | `src\eShopWinForms\Properties\Settings.settings:2-7` | Default profile, no declared settings |
| Client-generated settings | C# / JSON | `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs:448-477,591-627`; `ConnectedService.json:2-20` in same directory | Programmatic binding/address; service-reference generation metadata |
| Environment resolution | C# | `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:10-17` | Environment `ConnectionString` or named configuration fallback |
| Main orchestration | YAML | `docker-compose.yml:1-13`; `docker-compose.override.yml:1-25` | Windows service and SQL configuration; override carries environment/ports/network |
| Alternate orchestration artifacts | YAML / MSBuild | `temp\docker-compose.yml:1-14`; `temp\docker-compose.override.yml:1-21`; `temp\docker-compose.ci.build.yml:1-22`; `temp\docker-compose.dcproj:2-16` | Separate artifacts, not included by root dcproj; differing relative build context and Compose version |
| Build configuration | MSBuild | `src\eShopWCFService\eShopWCFService.csproj:1-42,112-117`; `src\eShopWinForms\eShopWinForms.csproj:1-34`; `src\eShopWinForms\eShopWinForms.fx.csproj:1-54` | Active SDK desktop, classic server, alternate classic desktop |
| Publish configuration | MSBuild | `src\eShopWCFService\Properties\PublishProfiles\FolderProfile.pubxml:8-16` | File-system Release publishing |
| Container image build | Dockerfile | `src\eShopWCFService\Dockerfile:1-5` | Runtime image and publish-output copy; no compilation stage |
| Docker IDE configuration | MSBuild | `docker-compose.dcproj:2-16` | Windows target; service launch configuration |
| Windows application manifest | XML | `src\eShopWinForms\app.manifest:3,19,43` | Application identity, asInvoker, Windows 10 compatibility declaration; explicitly selected by legacy project at `eShopWinForms.fx.csproj:54` |

No appsettings JSON, `.env`, Kubernetes manifests, cloud configuration repository, Key Vault, Vault, or managed secret-store reference was found in the scoped inventory. HTTP endpoint details are intentionally owned by `api-service-contracts.md`.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug / Any CPU | Solution configuration; classic projects default to Debug / AnyCPU | Server and legacy client: full symbols, no optimization, DEBUG and TRACE; server output bin, legacy client bin\Debug | Same package sets; server classic web targets (`eShopModernizedNTier.sln:13-29`; `src\eShopWCFService\eShopWCFService.csproj:4-5,27-34`; `src\eShopWinForms\eShopWinForms.fx.csproj:31-39`) |
| Release / Any CPU | Solution selection / MSBuild Configuration | Server and legacy client: optimized, TRACE, pdbonly; Release XDT removes server debug attribute | Same packages; web transform/publish (`src\eShopWCFService\eShopWCFService.csproj:36-42`; `src\eShopWinForms\eShopWinForms.fx.csproj:41-48`; `src\eShopWCFService\Web.Release.config:18`) |
| SDK desktop | Solution selects `eShopWinForms.csproj`; Debug/Release use SDK defaults | WinExe; WinForms and WPF enabled; assembly info generation disabled | Microsoft.NET.Sdk; no custom conditional profile groups (`src\eShopWinForms\eShopWinForms.csproj:1-9`; `eShopModernizedNTier.sln:10,26-29`) |
| FolderProfile | PublishProfile selection | FileSystem publish, last-used Release / Any CPU | publishUrl `$(docker_publish_root)obj\Docker\publish`; no new application dependencies (`src\eShopWCFService\Properties\PublishProfiles\FolderProfile.pubxml:8-16`) |
| Alternate legacy desktop | Explicitly build `.fx.csproj`; not the solution-selected client | .NET Framework desktop and ClickOnce metadata | Classic CSharp targets; older package versions (`src\eShopWinForms\eShopWinForms.fx.csproj:11,15-29,265-272`) |
| Docker tooling | Root dcproj build / IDE tooling | Windows container orchestration | Microsoft.Docker.Sdk, version not pinned (`docker-compose.dcproj:2-9`) |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default local server | No `ConnectionString` environment variable | `Web.config` | Named LocalDB connection (`src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:16-17`; `Web.config:63`) |
| Container server | Root Compose plus override | `docker-compose.yml`; `docker-compose.override.yml` | Environment connection, published ports, external nat network (`docker-compose.override.yml:5-25`) |
| Alternate container definitions | Explicit selection of files under `temp` | Three temp YAML files | Compose v3 and different build context; not formal production/dev profiles (`temp\docker-compose.yml:1-9`) |
| Desktop default | Program uses default generated client constructor | Generated `Reference.cs` | Programmatic HTTP destination and binding; not an environment-selected profile (`src\eShopWinForms\Program.cs:23`; generated `Reference.cs:448-452,591-621`) |

No `ASPNETCORE_ENVIRONMENT` or formal runtime profile composition was found. Debug/Release XML transforms are build/publish transformations, not independent runtime profiles. Alternate files under `temp` are existing source artifacts and were read only.

## Properties Inventory

### eShopWCFService

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `ConnectionString` | Unset → `name=EntityModel`; string | Environment override | `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:10-17` |
| `connectionStrings.EntityModel` | `Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=eShopDatabase;Persist Security Info=True;` | Local fallback | `src\eShopWCFService\Web.config:63` |
| `connectionStrings.EntityModel.providerName` | `System.Data.SqlClient` | Local | `Web.config:63` |
| `aspnet:UseTaskFriendlySynchronizationContext` | `true`; boolean | All | `Web.config:8` |
| `system.web.compilation.debug` | `true`; removed by Release transform | Debug / Release publish | `Web.config:19`; `Web.Release.config:18` |
| `compilation.targetFramework`, `httpRuntime.targetFramework` | `4.6.1` | All | `Web.config:19-20` |
| `serviceMetadata.httpGetEnabled`, `httpsGetEnabled` | Both `true` | All | `Web.config:27` |
| `serviceDebug.includeExceptionDetailInFaults` | `false` | All | `Web.config:29` |
| `protocolMapping` | HTTPS scheme → basicHttpsBinding | All | `Web.config:33-35` |
| `serviceHostingEnvironment.aspNetCompatibilityEnabled`, `multipleSiteBindingsEnabled` | Both `true` | All | `Web.config:36` |
| `services.service.name`, endpoint bindings/contracts | CatalogService; basicHttpBinding and mexHttpBinding | All; address details in API document | `Web.config:38-40` |
| `system.webServer.modules.runAllManagedModulesForAllRequests` | `true` | All | `Web.config:45` |
| `directoryBrowse.enabled` | `true` | All | `Web.config:50` |
| `entityFramework.defaultConnectionFactory` / parameter | LocalDbConnectionFactory / `mssqllocaldb` | All | `Web.config:53-56` |
| `entityFramework.providers` | System.Data.SqlClient → SqlProviderServices | All | `Web.config:58-60` |
| `UseIISExpress`, `WcfConfigValidationEnabled`, `AutoGenerateBindingRedirects` | All `true` | Build / development | `src\eShopWCFService\eShopWCFService.csproj:16-18` |
| Development host settings | UseIIS true; AutoAssignPort true; DevelopmentServerPort 62314; NTLMAuthentication false; UseCustomServer false; SaveServerSettingsInUserFile false | IDE development; service address in API document | `eShopWCFService.csproj:122-131` |
| Publish settings | WebPublishMethod / PublishProvider FileSystem; LaunchSiteAfterPublish true; ExcludeApp_Data false; DeleteExistingFiles false | FolderProfile | `src\eShopWCFService\Properties\PublishProfiles\FolderProfile.pubxml:8-16` |

In this table, shortened `Web.config`, `Web.Release.config`, and `eShopWCFService.csproj` references are under `src\eShopWCFService`.

### eShopWinForms

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `startup.supportedRuntime` | CLR v4.0 / .NET Framework 4.7.1 | Legacy App.config declaration, not active SDK target | `src\eShopWinForms\App.config:7-8` |
| `DpiAwareness` | `PerMonitorV2` | App.config | `App.config:10-11` |
| `basicHttpBinding` name | `BasicHttpBinding_ICatalogService`; no explicit timeout/security values | App.config | `App.config:14-17` |
| Client contract/name | eShopServiceReference.ICatalogService / BasicHttpBinding_ICatalogService | App.config; actual generated construction is code-based | `App.config:25`; `src\eShopWinForms\Program.cs:23` |
| EF default factory / parameter / provider | LocalDbConnectionFactory / mssqllocaldb / System.Data.SqlClient | App.config; no active direct DB use proven | `App.config:32-40` |
| Generated binding MaxBufferSize / MaxReceivedMessageSize | `int.MaxValue` = 2147483647 bytes | Default generated client | `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs:595-598` |
| Generated ReaderQuotas / AllowCookies | XmlDictionaryReaderQuotas.Max / `true` | Default generated client | `Reference.cs:597-599` in same directory |
| Generated endpoint | Hardcoded loopback HTTP address; alternate constructor accepts supplied address | Default constructor | `Reference.cs:448-477,605-621` in same directory; full address in API document |
| Service-reference collectionTypes | System.Array and generic Dictionary | Generation metadata only | `ConnectedService.json:11-14` in same directory |
| Service-reference namespaceMappings / sync / typeReuseMode / targetFramework | eShopWinForms.eShopServiceReference / true / None / netcoreapp3.0 | Generation metadata; target is stale relative to net6.0-windows | `ConnectedService.json:15-20` in same directory |
| Windows manifest identity / privileges | MyApplication.app version 1.0.0.0; asInvoker; uiAccess false; Windows 10 compatibility | Explicitly selected by legacy project only | `src\eShopWinForms\app.manifest:3,19,43`; `src\eShopWinForms\eShopWinForms.fx.csproj:54` |
| Settings profile | Default; empty Settings | Desktop settings | `src\eShopWinForms\Properties\Settings.settings:2-7` |
| Legacy ClickOnce | PublishUrl publish\; Install true; InstallFrom Disk; UpdateEnabled false; UpdateMode Foreground; interval 7 Days; UpdatePeriodically false; UpdateRequired false | Alternate .fx project | `src\eShopWinForms\eShopWinForms.fx.csproj:15-23` |
| Legacy ClickOnce metadata | MapFileExtensions true; ApplicationRevision 0; ApplicationVersion `1.0.0.%2a`; IsWebBootstrapper false; UseApplicationTrust false; BootstrapperEnabled true | Alternate .fx project | `eShopWinForms.fx.csproj:24-29` in same directory |
| Image asset path | Current directory plus `..\..\..\Assets\Images\Catalog\` and stored filename | Hardcoded, no setting override | `src\eShopWinForms\Views\CatalogView.cs:55-58` |

In this table, shortened `App.config` references are under `src\eShopWinForms`. No settings property redirects the generated proxy's default address; changing the legacy XML endpoint alone does not establish a change to the actual default constructor path.

### Compose and image configuration

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| Compose `version` | `3.4` root; `3` alternate files | Root / temp | `docker-compose.yml:1`; `temp\docker-compose.yml:1` |
| `eshopwcfservice.image` | `eshop/wcfservice` | Root / temp | `docker-compose.yml:5`; `temp\docker-compose.yml:6` |
| Build context / dockerfile | `.\src\eShopWCFService` / Dockerfile; alternate context `.\eShopWCFService` relative to temp Compose location | Root / temp | `docker-compose.yml:7-8`; `temp\docker-compose.yml:8-9` |
| `sql.data.image` | `microsoft/mssql-server-windows-developer`; no version tag | Root / temp | `docker-compose.yml:13`; `temp\docker-compose.yml:14` |
| Connection environment | `Server=sql.data;Database=eShopDatabase;[MASKED]` — credential segment omitted | Root override and temp overrides | `docker-compose.override.yml:7`; `temp\docker-compose.override.yml:6`; `temp\docker-compose.ci.build.yml:6` |
| `SA_PASSWORD` | `[MASKED]` | SQL container | `docker-compose.override.yml:13`; temp override/CI files:11 |
| `ACCEPT_EULA` | `Y` | SQL container | `docker-compose.override.yml:14`; temp override/CI files:12 |
| `ports` | Service 5113:80; SQL 5433:1433 | Override files | `docker-compose.override.yml:9,16`; `temp\docker-compose.override.yml:8,14`; `temp\docker-compose.ci.build.yml:8,14` |
| Default network | External network named `nat` | Override files | `docker-compose.override.yml:22-25`; `temp\docker-compose.override.yml:18-21`; `temp\docker-compose.ci.build.yml:19-22` |
| Dockerfile `source` build argument | Unset; copy fallback `obj/Docker/publish` | Image build | `src\eShopWCFService\Dockerfile:3-5` |
| Dockerfile WORKDIR / EXPOSE | IIS wwwroot / 80 | Image runtime | `src\eShopWCFService\Dockerfile:2-4` |
| Docker IDE launch settings | DockerLaunchBrowser true; DockerServiceName eshopwcfservice; DockerTargetOS Windows; ProjectVersion 2.1 root, 2.0 temp | IDE tooling; URLs omitted here | `docker-compose.dcproj:5-9`; `temp\docker-compose.dcproj:5-9` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopWCFService | Windows IIS/WCF image defaults; connection environment override; no custom entrypoint or GC options | Not specified | One service declaration; no replica/scaling setting |
| sql.data | Windows SQL image; credential/EULA environment inputs | Not specified | One service declaration |
| Desktop | STAThread; visual styles; compatible text rendering false; loads data before ShowDialog | Not specified | User-launched process; no scale setting |

Evidence: `src\eShopWCFService\Dockerfile:1-5`; `docker-compose.yml:3-13`; `docker-compose.override.yml:5-20`; `src\eShopWinForms\Program.cs:16-27`. No CPU limits, resource requests, container volume, memory limits, or JVM settings are declared. SQL data durability beyond the container filesystem is not configured in these Compose files.

## Startup Dependency Chain

1. External Windows network `nat` must already exist for the root override (`docker-compose.override.yml:22-25`).
2. `eshopwcfservice` → depends on → `sql.data`, using simple Compose `depends_on`, **not** a `service_healthy` condition (`docker-compose.yml:9-10`).
3. SQL healthcheck runs `sqlcmd` with masked credential arguments and `select 1`; root interval is 1 second with 20 retries. Temp CI also specifies 20 retries; temp override specifies interval only. No explicit healthcheck timeout/start period is supplied (`docker-compose.override.yml:17-20`; `temp\docker-compose.ci.build.yml:15-18`; `temp\docker-compose.override.yml:15-17`). These settings do not create a service-readiness gate for the simple depends_on.
4. First server data access can create/seed a missing database; no custom connection retry/wait loop was found (`src\eShopWCFService\EntityModel.cs:12-15`; `src\eShopWCFService\Models\Infrastructure\CatalogDBInitializer.cs:11-20`).
5. Desktop `LoadView` calls the service before showing its modal UI; no readiness retry/fallback wraps this initialization (`src\eShopWinForms\Program.cs:26-27`; `src\eShopWinForms\Controllers\CatalogController.cs:150-157`).

Alternate temp build context points to a directory not present under `temp` in the scoped inventory. Its usability depends on external tooling/layout and was not assumed.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| Root override service ConnectionString | Credential-bearing SQL connection | Committed YAML literal; user/password `[MASKED]` (`docker-compose.override.yml:7`) |
| Root SQL SA_PASSWORD | Database administrator password | Committed YAML literal `[MASKED]` (`docker-compose.override.yml:13`) |
| Root SQL healthcheck credential arguments | SQL login/password | Committed command arguments `[MASKED]` (`docker-compose.override.yml:18`) |
| Temp override and CI equivalents | Connection and healthcheck credentials | Committed YAML literals `[MASKED]` (`temp\docker-compose.override.yml:6,11,16`; `temp\docker-compose.ci.build.yml:6,11,16`) |
| Local named connection | Database location / connection metadata | No explicit password/user entry in the local connection shown in Web.config (`src\eShopWCFService\Web.config:63`) |

### Secrets Provisioning Workflow

Observed flow: checked-in Compose literals → container environment → server's `Environment.GetEnvironmentVariable` → EF connection; SQL receives its password independently via its environment, and healthchecks embed their own credential arguments (`docker-compose.override.yml:5-18`; `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:16-17`). Operators would need to keep these inputs consistent; no automatic secret binding/rotation workflow is present.

No scoped deployment workflow establishes managed identity, service-principal authentication, secret-store RBAC, encrypted configuration, or runtime vault retrieval. This document does not reproduce or validate existing credential values.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| No active feature flag found | Not applicable | No feature-management framework declared |
| `useMockData` (inactive code only) | Constructor supplied, not wired | `ApplicationModule` stores the bool, but registration block is commented and Program instantiates a real proxy (`src\eShopWinForms\Modules\ApplicationModule.cs:6-34`; `src\eShopWinForms\Program.cs:23`) |

Debug compilation and connection-environment selection are build/configuration choices, not business feature flags.

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Active desktop target / SDK | net6.0-windows / Microsoft.NET.Sdk version unpinned | `src\eShopWinForms\eShopWinForms.csproj:1-7` |
| Server target / MSBuild ToolsVersion | .NET Framework 4.6.1 / 15.0 project declaration | `src\eShopWCFService\eShopWCFService.csproj:1,15` |
| Alternate desktop target | .NET Framework 4.7.1 | `src\eShopWinForms\eShopWinForms.fx.csproj:11` |
| WCF Windows base image | `mcr.microsoft.com/dotnet/framework/wcf:4.7.2` | `src\eShopWCFService\Dockerfile:1` |
| SQL Server Windows image | Developer image, unpinned tag | `docker-compose.yml:13` |
| Entity Framework | Server/legacy 6.1.3; active desktop 6.4.4 | `src\eShopWCFService\packages.config:3`; `src\eShopWinForms\eShopWinForms.fx.csproj:265-267`; `src\eShopWinForms\eShopWinForms.csproj:22` |
| Microsoft.AspNet.WebApi.Client | Active 5.2.7; alternate 5.2.3 | `src\eShopWinForms\eShopWinForms.csproj:23`; `src\eShopWinForms\eShopWinForms.fx.csproj:268-270` |
| System.ServiceModel Duplex / Http / NetTcp / Security | 4.8.0 | `src\eShopWinForms\eShopWinForms.csproj:24-27` |
| WCF connected-service provider / generator | 15.0.30423.811 / Svcutil 2.0.1-preview-30422-0661 | `src\eShopWinForms\Connected Services\eShopServiceReference\ConnectedService.json:2-3`; `Reference.cs:437` in same directory |
| Solution format / recorded Visual Studio | 12.00 / 15.0.26430.16 | `eShopModernizedNTier.sln:2-5` |
| Docker SDK / actual installed tools | SDK version and installed tool versions not established | `docker-compose.dcproj:2`; no execution performed |

The generated reference's netcoreapp3.0 metadata and App.config's Framework 4.7.1 declaration do not change the active net6.0-windows target. Target, hosting-image, and installed-tool versions are distinct; runtime compatibility was not tested.
