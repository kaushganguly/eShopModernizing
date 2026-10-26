# Configuration & Externalized Settings Inventory

The scoped configuration comprises service/desktop XML settings, two web transforms, three project files, a publish profile, generated client settings, and root/alternate Compose artifacts. Only the database connection has a discovered runtime environment override; sensitive credentials are represented as `[MASKED]`, never reproduced.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Service settings | XML | `src\eShopWCFService\Web.config` | WCF/IIS, EF provider and named database connection |
| Service transforms | XML XDT | `src\eShopWCFService\Web.Debug.config`, `Web.Release.config` | Debug contains no active changes; Release removes compilation debug attribute |
| Desktop settings | XML | `src\eShopWinForms\App.config` | Legacy runtime/DPI, WCF and EF settings; do not assume generated .NET 6 proxy reads endpoint from this file |
| Generated WCF settings | C# defaults | `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs` | Active default constructor chooses binding/address in code |
| WCF regeneration settings | JSON | `src\eShopWinForms\Connected Services\eShopServiceReference\ConnectedService.json` | Input metadata location, collection options, generated namespace and older generator target |
| User/application settings scaffold | XML | `src\eShopWinForms\Properties\Settings.settings` | Empty settings collection; no configured user values |
| Runtime override adapter | Environment and C# | `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs` | `ConnectionString` environment value precedes named XML connection |
| Root deployment | YAML / MSBuild | `docker-compose.yml`, `docker-compose.override.yml`, `docker-compose.dcproj` | Solution-selected Windows container definition |
| Alternate deployment artifacts | YAML / MSBuild | `temp\docker-compose.yml`, `temp\docker-compose.override.yml`, `temp\docker-compose.ci.build.yml`, `temp\docker-compose.dcproj` | Retained alternate files; not referenced by root solution; relative build context differs |
| Container recipe | Dockerfile | `src\eShopWCFService\Dockerfile` | Prepublished content copied into WCF image |
| Publish settings | MSBuild XML | `src\eShopWCFService\Properties\PublishProfiles\FolderProfile.pubxml` | Filesystem publish into Docker build input |
| Build configuration | Project XML | Three scoped `.csproj` files and solution | Two selected projects plus retained legacy desktop project |
| Desktop compatibility | XML | `src\eShopWinForms\app.manifest` | Windows 10 compatibility and as-invoker UAC; DPI example is commented out |

No scoped `.env`, `appsettings*.json`, `launchSettings.json`, remote config repository, Azure App Configuration, Key Vault/Vault/Secrets Manager reference or Kubernetes/Helm settings were found. This inventory does not inspect configuration outside the requested subtree.

Evidence: [service settings][web], [desktop settings][app], [proxy defaults][proxy], [connection adapter][connection], [root deployment][compose], [alternate deployment][alternate], [publish profile][publish].

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug / Any CPU | Solution default or MSBuild `/p:Configuration=Debug` | Legacy service: full symbols, optimization disabled, `DEBUG;TRACE` | Same service package references; no profile-specific additions |
| Release / Any CPU | MSBuild `/p:Configuration=Release` | Legacy service: pdb-only symbols, optimization enabled, `TRACE`; publish transform removes debug attribute | Same packages; WebApplication targets |
| Selected desktop Debug / Release | SDK configuration conventions / solution selection | SDK-style build; no explicit profile-specific property groups in selected file | Microsoft.NET.Sdk; WinForms and WPF enabled |
| Retained `.fx.csproj` Debug / Release | Explicit build of legacy file, not selected by solution | Legacy 4.7.1 desktop; full/pdb-only symbols and optimization switches | Older EF/WebApi.Client versions |
| FolderProfile | Publish profile selection | Filesystem Release publish to Docker input; retains existing output files | Web publish tooling |
| Docker project | Solution's Docker integration | Windows target; launches configured service in browser | Microsoft.Docker.Sdk, ToolsVersion 15.0 |

Evidence: [service profiles][serviceproject], [desktop][desktop], [legacy profiles][legacy], [solution][solution], [publish][publish], [Docker project][dcproj].

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default/local service | Environment override absent | Web.config | Named LocalDB connection |
| Root Compose | Compose root plus override file | Root YAML files | Environment database connection, host port mappings, external network |
| Alternate Compose artifacts | Explicit selection by an operator | `temp` YAML files | Same conceptual environment bindings; schema 3 rather than 3.4; alternate base build context |
| Desktop default | `Program.Main` constructs parameterless generated client | Generated Reference.cs | Hardcoded HTTP remote address and binding defaults |
| Legacy desktop configuration | Explicit legacy project/runtime | App.config | CLR v4/.NET Framework 4.7.1 declaration and XML client settings |

No named ASP.NET Core environment profiles or composable framework runtime profiles appear. Debug/Release web transforms are **publish/build transforms**, not runtime environment selectors. The root solution does not select the alternate Docker project.

## Properties Inventory

Only configuration values relevant to the application/build are listed; XML schema declarations, assembly public-key identifiers and designer resource values are metadata, not externalized properties. HTTP addresses/routes are cataloged in `api-service-contracts.md`, not repeated here.

### eShopWCFService

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| Environment `ConnectionString` | Unset; entire non-null environment value wins | Compose overrides; otherwise local | CatalogConfiguration.cs |
| `configConnectionName` | `EntityModel` | All | CatalogConfiguration.cs |
| `connectionStrings/EntityModel` | `Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=eShopDatabase;Persist Security Info=True;` | Default | Web.config |
| Connection provider | `System.Data.SqlClient` | All | Web.config |
| Compose `ConnectionString` | `Server=sql.data;Database=eShopDatabase;User Id=[MASKED];[MASKED]` | Root and alternate overrides | YAML; credential-bearing remainder redacted |
| `aspnet:UseTaskFriendlySynchronizationContext` | Boolean `true` | All | Web.config |
| `compilation/debug` | Boolean `true` | Release publish removes attribute | Web.config, Web.Release.config |
| `compilation/targetFramework`; `httpRuntime/targetFramework` | `4.6.1` | All | Web.config |
| `serviceMetadata/httpGetEnabled`; `httpsGetEnabled` | Both boolean `true` | All | Web.config |
| `serviceDebug/includeExceptionDetailInFaults` | Boolean `false` | All | Web.config |
| `protocolMapping/https` | `basicHttpsBinding` | All | Web.config |
| Service binding; metadata binding | `basicHttpBinding`; `mexHttpBinding` | All | Web.config |
| `serviceHostingEnvironment/aspNetCompatibilityEnabled`; `multipleSiteBindingsEnabled` | Both boolean `true` | All | Web.config |
| `modules/runAllManagedModulesForAllRequests` | Boolean `true` | All | Web.config |
| `directoryBrowse/enabled` | Boolean `true` | All | Web.config |
| EF default connection factory parameter | `mssqllocaldb` | Default | Web.config |
| EF provider | `System.Data.Entity.SqlServer.SqlProviderServices` | All | Web.config |

There is no interpolation chain beyond the environment-or-named-connection choice. An empty but defined environment string does not trigger the null-coalescing fallback. No project-defined connection-pool size or SQL command timeout override was found.

### eShopWinForms and generated client

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `startup/supportedRuntime` | `v4.0`; SKU .NETFramework 4.7.1 | Legacy XML artifact; selected project targets .NET 6 | App.config |
| `DpiAwareness` | `PerMonitorV2` | XML configuration | App.config |
| XML client binding name | `BasicHttpBinding_ICatalogService` | Legacy XML client | App.config |
| XML EF connection factory/provider | LocalDB factory with `mssqllocaldb`; SQL Server provider | XML configuration; not proof of active desktop DB use | App.config |
| Generated binding | New default `BasicHttpBinding` | Actual selected desktop startup | Reference.cs |
| `MaxBufferSize`; `MaxReceivedMessageSize` | `int.MaxValue` (2147483647) | Generated proxy | Reference.cs |
| `ReaderQuotas` | `XmlDictionaryReaderQuotas.Max` | Generated proxy | Reference.cs |
| `AllowCookies` | Boolean `true` | Generated proxy | Reference.cs |
| Binding timeouts | No override; WCF defaults | Generated proxy | Reference.cs |
| Connected service `ProviderId`; `Version` | Microsoft.VisualStudio.ConnectedService.Wcf; 15.0.30423.811 | Proxy regeneration | ConnectedService.json |
| `collectionTypes` | `System.Array`; generic Dictionary | Proxy regeneration | ConnectedService.json |
| `namespaceMappings`; `sync` | `eShopWinForms.eShopServiceReference`; boolean `true` | Proxy regeneration | ConnectedService.json |
| `targetFramework`; `typeReuseMode` | `netcoreapp3.0`; `None` | Historical generation input, not current project target | ConnectedService.json |
| User settings | Empty | Default | Settings.settings |
| Manifest `requestedExecutionLevel`; `uiAccess` | `asInvoker`; boolean `false` | Explicitly referenced by retained legacy project; selected SDK project does not explicitly select this manifest | app.manifest |
| Manifest compatibility | Windows 10 supported-OS declaration | Same manifest-selection caveat | app.manifest |

### Deployment and publishing

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| Compose `version` | `3.4` root; `3` alternate | Corresponding YAML files | Compose manifests |
| Service `image` | `eshop/wcfservice` | Root/alternate | Compose base files |
| Service `build.context`; `dockerfile` | `./src/eShopWCFService`; `Dockerfile` | Root; alternate has `./eShopWCFService` | Compose base files |
| SQL `image` | `microsoft/mssql-server-windows-developer`, no tag | Root/alternate | Compose base files |
| SQL `SA_PASSWORD` | `[MASKED]` | Override and alternate CI files | YAML environment |
| SQL `ACCEPT_EULA` | `Y` | Override and alternate CI files | YAML environment |
| `networks/default/external/name` | `nat` | Override and alternate CI files | YAML |
| Docker `ARG source` | Unset; COPY uses `obj/Docker/publish` fallback | Image build | Dockerfile |
| Docker `WORKDIR` | `/inetpub/wwwroot` | Image | Dockerfile |
| `DockerTargetOS`; `DockerServiceName` | `Windows`; `eshopwcfservice` | Docker tooling | dcproj |
| `DockerLaunchBrowser`; `ProjectVersion` | `True`; `2.1` root, `2.0` alternate | Docker tooling | dcproj |
| `WebPublishMethod`; `PublishProvider` | `FileSystem`; `FileSystem` | FolderProfile | pubxml |
| `LastUsedBuildConfiguration`; `LastUsedPlatform` | `Release`; `Any CPU` | FolderProfile | pubxml |
| `LaunchSiteAfterPublish`; `ExcludeApp_Data`; `DeleteExistingFiles` | `True`; `False`; `False` | FolderProfile | pubxml |
| `publishUrl` | `$(docker_publish_root)obj\Docker\publish` | FolderProfile | pubxml; MSBuild property expansion |

Evidence: [Web.config][web], [Release transform][release], [App.config][app], [proxy][proxy], [generator JSON][generator], [manifest][manifest], [root override][override], [Dockerfile][docker], [publish][publish]. Port mappings and readiness settings belong to the following startup sections.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopWCFService | IIS/WCF image; container port 80 → host 5113; database connection passed as environment variable | No explicit memory/CPU reservation or limit | No explicit replica/scaling setting |
| eShopWinForms | Windows desktop executable; manual proxy construction; local image-path dependency | No declared limits | One desktop process per launch; no scaling configuration |
| sql.data | SQL Server Windows container; 1433 → host 5433; EULA and masked password environment entries | No explicit memory/CPU reservation or limit | No explicit replica/scaling setting |

No JVM applies. No ASPNETCORE_ENVIRONMENT, managed-heap tuning, cloud instance SKU, Kubernetes resources or autoscaling policy is defined. Development service project contains IIS Express enablement, auto port assignment and development-server port 62314; its IIS development address uses port 5200, distinct from Compose and the generated client's 5113 ([service development settings][iis]).

## Startup Dependency Chain

1. **External Windows `nat` network must exist** before Compose can attach the containers; these YAML files reference it rather than create it.
2. **sql.data → WCF container start ordering:** root Compose declares `depends_on: sql.data`. This is not a health-conditioned dependency in the shown version-3.4 definition.
3. **SQL readiness observation:** root override runs `sqlcmd` with a masked credential and `select 1`; interval 1 second, retries 20. No explicit start period or healthcheck timeout is set. The alternate CI file also specifies 20 retries; the alternate override leaves retries implicit. No explicit wait/retry code connects this health result to WCF request handling.
4. **WCF → database on use:** EF initialization can create/seed a missing database during data access. No startup migration job, dockerize loop or service readiness endpoint is provided.
5. **Desktop → WCF before dialog display:** `Program.Main` loads initial data synchronously before showing the form. There is no readiness wait or exception-recovery flow; the service and database need to be usable for initial loading.

Evidence: [Compose dependency][compose], [network and healthcheck][override], [alternate CI healthcheck][ci], [initializer registration][context], [desktop startup][program]. No health/management HTTP path is duplicated here.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| Compose service `ConnectionString` | Credential-bearing DB connection | Literal YAML environment entry; credential content `[MASKED]` |
| Compose SQL `SA_PASSWORD` | Database administrator password | Literal YAML environment entry `[MASKED]` |
| SQL healthcheck `sqlcmd -P` argument | Password in command array | Literal YAML argument `[MASKED]` |
| Alternate override and CI equivalents | Duplicate credential-bearing configuration | Same categories, all values `[MASKED]` |
| LocalDB named connection | Connection configuration | No explicit password in shown fallback; infrastructure access controls unspecified |

The service connection literal contains a visibly masked credential remainder in source; its real deployment-time value is not established. SQL password/healthcheck literals are sensitive even when used as sample credentials. They are intentionally not reproduced. No encryption wrapper, protected-configuration provider, managed identity or secret-store binding was discovered.

### Secrets Provisioning Workflow

The only evidenced workflow is **checked-in Compose settings → container environment / healthcheck command → service environment read → EF connection**. SQL Server independently receives its password through its environment. No scoped pipeline retrieves secrets, replaces the masked connection placeholder, provisions identity/RBAC, or synchronizes the connection credentials with SQL's administrator secret. Consequently successful credential alignment is not verified. The desktop does not provision database credentials and sends no discovered API authentication credential. Secret storage and rotation outside this subtree are unknown.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| No operational feature flag discovered | Not applicable | No feature-management framework or runtime flag provider |
| `ApplicationModule.useMockData` | Constructor argument, no active configuration source | Registration body is commented out; startup constructs real generated proxy directly |

Connection selection and Debug/Release transforms are configuration choices, not a gradual-rollout mechanism. The dormant mock-selection code is not an enabled test mode ([module][module], [startup][program]).

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Selected desktop target | `net6.0-windows` | [eShopWinForms.csproj][desktop] |
| Selected service target | .NET Framework 4.6.1 | [Service project][serviceproject], [Web.config][web] |
| Retained desktop target / runtime declaration | .NET Framework 4.7.1 / CLR v4 | [Legacy project][legacy], [App.config][app] |
| WCF container runtime image | `mcr.microsoft.com/dotnet/framework/wcf:4.7.2` | [Dockerfile][docker] |
| Service EF package | 6.1.3 | [packages.config][packages] |
| Selected desktop EF package | 6.4.4 | [Desktop manifest][desktop] |
| Selected desktop WebApi.Client | 5.2.7 | [Desktop manifest][desktop] |
| Selected desktop WCF packages | 4.8.0 | [Desktop manifest][desktop] |
| Retained legacy EF / WebApi.Client | 6.1.3 / 5.2.3 | [Legacy package section][legacypackages] |
| SQL Server engine | Unspecified; image has no explicit tag or digest | [Compose][compose] |
| Legacy MSBuild project format | ToolsVersion 15.0; publish profile ToolsVersion 4.0 | [Service project][serviceproject], [publish][publish] |
| Solution Visual Studio metadata | 15.0.26430.16; minimum 10.0.40219.1 | [Solution][solution] |
| .NET SDK / installed MSBuild / Docker engine / Compose binary | Not pinned or established in scope | No scoped global.json or engine-version requirement found |

Manifest targets, image tags, generator history and locally installed tools are different facts; none is substituted for another.

[web]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWCFService/Web.config#L1-L65
[app]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWinForms/App.config#L1-L42
[proxy]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWinForms/Connected%20Services/eShopServiceReference/Reference.cs#L591-L621
[connection]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWCFService/Models/Infrastructure/CatalogConfiguration.cs#L10-L18
[compose]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/docker-compose.yml#L1-L13
[alternate]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/temp/docker-compose.yml#L1-L14
[publish]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWCFService/Properties/PublishProfiles/FolderProfile.pubxml#L6-L17
[release]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWCFService/Web.Release.config#L17-L18
[serviceproject]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWCFService/eShopWCFService.csproj#L1-L48
[desktop]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWinForms/eShopWinForms.csproj#L1-L28
[legacy]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWinForms/eShopWinForms.fx.csproj#L1-L47
[solution]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/eShopModernizedNTier.sln#L1-L29
[dcproj]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/docker-compose.dcproj#L1-L17
[generator]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWinForms/Connected%20Services/eShopServiceReference/ConnectedService.json#L1-L22
[override]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/docker-compose.override.yml#L1-L25
[docker]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWCFService/Dockerfile#L1-L5
[iis]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWCFService/eShopWCFService.csproj#L121-L127
[ci]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/temp/docker-compose.ci.build.yml#L9-L22
[context]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWCFService/EntityModel.cs#L10-L16
[program]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWinForms/Program.cs#L17-L27
[module]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWinForms/Modules/ApplicationModule.cs#L6-L34
[packages]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWCFService/packages.config#L1-L4
[legacypackages]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWinForms/eShopWinForms.fx.csproj#L264-L270
[manifest]: https://github.com/kaushganguly/eShopModernizing/blob/1732cb32a4e3e096341ca098b8fb54e2e8e9f08f/eShopModernizedNTier/src/eShopWinForms/app.manifest#L19-L58
