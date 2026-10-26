# Configuration & Externalized Settings Inventory

The scoped configuration landscape combines XML application settings, one environment connection override, a generated client configuration path, build/publish settings and two sets of Compose artifacts. Credentials are represented only as **[MASKED]**; no deployed secrets store, identity provisioning or runtime environment was verified. [C1–C12]

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Service Web.config | Application XML | Absolute path [C1] | Web runtime, WCF behavior, EF provider and named connection. |
| Web.Debug.config / Web.Release.config | Publish transforms | Absolute paths [C2, C3] | Debug contains examples/comments only; Release removes compilation debug attribute. |
| CatalogConfiguration | Environment reader | Absolute path [C4] | Environment ConnectionString if non-null, otherwise named EntityModel connection. Empty string does not trigger fallback. |
| Desktop App.config | Legacy-style application XML | Absolute path [C5] | Runtime declaration, DPI and WCF/EF settings; not proof all settings are consumed by .NET 6. |
| Generated Reference.cs | Code-defined binding/address | Absolute path [C6] | Parameterless generated client selects binding and address in code. |
| ConnectedService.json | Design-time tooling | Absolute path [C7] | Proxy generation input/settings; not a runtime environment profile. |
| Settings.settings | Desktop settings designer | Absolute path [C8] | Default profile with no settings entries. |
| app.manifest | Desktop manifest | Absolute path [C9] | asInvoker, no UI access; Windows 10 compatibility declaration. Legacy project explicitly selects this manifest; SDK project does not explicitly select it. [C14, C15] |
| Root Compose files / dcproj | Deployment and VS tooling | Absolute paths [C10–C12] | Windows target; base services plus environment/port/network override. |
| temp Compose files / dcproj | Alternative deployment artifacts | Absolute paths [C13] | Existing scoped files were read only; not treated as active configuration or written to. Build context differs from root Compose. |
| FolderProfile.pubxml / Dockerfile | Publish/package and image | Absolute paths [C16, C17] | Filesystem publication then copy into prebuilt WCF image. |
| Application and legacy projects / solution | MSBuild | Absolute paths [C14, C15, C18, C19] | SDK desktop selected by solution; legacy .fx project not selected. |

No appsettings/environment-specific JSON, config-server repository, Key Vault/Vault/App Configuration binding or Kubernetes manifest was found in the scoped inventory. This is a scoped negative finding, not evidence about external deployment systems; the implemented external resolution source is the process environment. [C4, C10–C13]

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Solution Debug / Release, Any CPU | Visual Studio/MSBuild Configuration selection | Selects matching configurations for service, SDK desktop and Compose project | No profile-specific package changes declared. [C18] |
| WCF Debug | Default when Configuration unset; Debug/AnyCPU | Full debug symbols, no optimization, DEBUG and TRACE, bin output, warning level 4, error report prompt | Framework 4.6.1; EF package references shared. [C19] |
| WCF Release | Release/AnyCPU | pdbonly, optimization, TRACE, same bin output and warning/error settings | Release XML publish transform separately removes debug compilation attribute. [C3, C19] |
| SDK desktop | SDK defaults plus solution Debug/Release | WinExe, net6.0-windows, WinForms and WPF enabled, assembly-info generation disabled | Six PackageReferences, no conditional dependency groups in file. [C14, C18] |
| Legacy desktop Debug / Release | Explicitly building .fx.csproj, not selected by solution | Framework 4.7.1, AnyCPU, full vs pdbonly symbols, optimize false vs true, DEBUG/TRACE vs TRACE, separate output folders | Framework references, manifest, ClickOnce-style publication metadata. [C15, C18] |
| FolderProfile | PublishProfile selection | FileSystem provider/method, last-used Release/Any CPU, include App_Data, do not delete existing files | Output `$(docker_publish_root)obj\Docker\publish`; docker_publish_root is not defined by this file. [C16] |

Service project enables WCF configuration validation, binding redirects and IIS Express; it declares unversioned imported CSharp and WebApplication targets. Legacy desktop enables binding redirects, disk install, disabled update checking, seven-day interval metadata and bootstrapper settings. These are build/publication declarations, not active runtime feature flags. [C15, C19]

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Service fallback | ConnectionString environment variable absent/null | Web.config | Named EntityModel connection. [C1, C4] |
| Service environment override | ConnectionString process environment variable exists | CatalogConfiguration plus root Compose override if supplied | Replaces entire connection value; no validation or empty-string fallback in reader. [C4, C11] |
| Root container deployment | Compose base with override through tooling/conventional Compose use | Root Compose files | Environment, host ports and external nat network; actual invocation unverified. [C10–C12] |
| Alternative temp deployment | Explicit selection of alternate artifact set | temp Compose files | Compose version 3, different build context; CI artifact resembles override, not proof CI runs it. [C13] |
| Desktop generated defaults | Program constructs parameterless generated client | Reference.cs | Code-defined binding and remote address; no ASPNETCORE_ENVIRONMENT-style profile selection. [C6, C20] |

Debug/Release are build configurations, not evidence of Development/Production runtime profiles. No named application runtime profiles compose in the inspected source. [C4, C14, C18, C19]

## Properties Inventory

Values below are declared defaults/settings, not observed effective production values. HTTP route inventories are intentionally left to `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\.github\modernize\assessment\engines\facts\api-service-contracts.md`.

### WCF service application

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| aspnet:UseTaskFriendlySynchronizationContext | Boolean true | Base XML | [C1:7-9] |
| system.web/compilation.debug | Boolean true | Removed by Release publish transform | [C1:18-20], [C3:17-18] |
| compilation.targetFramework; httpRuntime.targetFramework | 4.6.1; 4.6.1 | Base XML | [C1:18-20] |
| serviceMetadata.httpGetEnabled; httpsGetEnabled | true; true | Base XML | [C1:25-29] |
| serviceDebug.includeExceptionDetailInFaults | false | Base XML | [C1:29] |
| protocolMapping HTTPS binding | basicHttpsBinding | Base XML; listener not established here | [C1:33-35] |
| serviceHostingEnvironment.aspNetCompatibilityEnabled; multipleSiteBindingsEnabled | true; true | Base XML | [C1:36] |
| service.name; endpoint binding / contract | eShopWCFService.CatalogService; basicHttpBinding / eShopWCFService.ICatalogService; metadata exchange uses mexHttpBinding | Base XML | [C1:37-41] |
| modules.runAllManagedModulesForAllRequests | true | Base XML | [C1:44-45] |
| directoryBrowse.enabled | true | Base XML | [C1:50] |
| entityFramework.defaultConnectionFactory; parameter | LocalDbConnectionFactory; mssqllocaldb | Base XML | [C1:52-57] |
| entityFramework provider | System.Data.SqlClient → EntityFramework.SqlServer.SqlProviderServices | Base XML | [C1:58-60] |
| connectionStrings.EntityModel | Data Source=(localdb)\MSSQLLocalDB; Initial Catalog=eShopDatabase; Persist Security Info=True; provider System.Data.SqlClient | Used on environment fallback | [C1:62-64], [C4] |
| ConnectionString | Full connection string, no source-code default | Environment wins when non-null; otherwise name=EntityModel | [C4:10-17] |

### Desktop application and generation settings

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| startup.supportedRuntime.version / sku | v4.0 / .NETFramework,Version=v4.7.1 | Legacy XML; conflicts with SDK target declaration | [C5:7-9], [C14] |
| DpiAwareness | PerMonitorV2 | XML application setting; effective .NET 6 consumption unverified | [C5:10-12] |
| basicHttpBinding name | BasicHttpBinding_ICatalogService; no custom limits/timeouts here | XML client settings | [C5:13-17] |
| client endpoint binding / contract / name | basicHttpBinding / eShopServiceReference.ICatalogService / BasicHttpBinding_ICatalogService | XML; generated default constructor uses code instead | [C5:19-30], [C6] |
| client remote address | HTTP localhost port 5113; service route documented in API contracts | Code default; matching XML value exists | [C6:605-621], [C5:25] |
| generated MaxBufferSize / MaxReceivedMessageSize | int.MaxValue, 2147483647 bytes each | Code default binding | [C6:591-600] |
| generated ReaderQuotas / AllowCookies | XmlDictionaryReaderQuotas.Max / true | Code default binding | [C6:597-599] |
| generated timeouts / security customization | Not explicitly assigned | Framework defaults not measured | [C6:591-600] |
| EF default factory / parameter / provider | LocalDbConnectionFactory / mssqllocaldb / System.Data.SqlClient | XML only; no traced client DB use | [C5:32-40], [C20] |
| Settings profiles / values | Default / no values | Designer only | [C8] |
| ConnectedService ProviderId / Version | Microsoft.VisualStudio.ConnectedService.Wcf / 15.0.30423.811 | Design-time | [C7:2-3] |
| ConnectedService inputs | Local HTTP service metadata address, port 5113 | Design-time only | [C7:8-10] |
| collectionTypes / namespaceMappings | System.Array and Dictionary; service types mapped to client namespace | Design-time | [C7:11-17] |
| sync / targetFramework / typeReuseMode | true / netcoreapp3.0 / None | Design-time; not the net6.0-windows runtime target | [C7:18-20] |
| requestedExecutionLevel / uiAccess / supportedOS | asInvoker / false / Windows 10 declaration | Manifest; selection differs by project file | [C9:19-43], [C15:53-55], [C14] |

### Deployment and tooling settings

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| Compose version | Root 3.4; temp 3 | Artifact-specific | [C10:1], [C11:1], [C13] |
| eshopwcfservice.image | eshop/wcfservice | Root and alternate | [C10:4-8], [C13] |
| build.context / dockerfile | Root ./src/eShopWCFService / Dockerfile; alternate ./eShopWCFService / Dockerfile | Alternate relative context does not match root layout as written; execution directory/resolution unverified | [C10:6-8], [C13] |
| service ConnectionString environment declaration | SQL server sql.data, database eShopDatabase, privileged SQL login; credential portion [MASKED] | Root and temp overrides; supplied value ends in an incomplete placeholder | [C11:5-7], [C13] |
| sql.data.image | microsoft/mssql-server-windows-developer, no explicit tag | Root and alternate | [C10:12-13], [C13] |
| SA_PASSWORD / ACCEPT_EULA | [MASKED] / Y | SQL container | [C11:11-14], [C13] |
| service port mapping / SQL mapping | 5113:80 / 5433:1433 | Deployment override | [C11:8-16], [C13] |
| networks.default.external.name | nat | Root and temp | [C11:22-25], [C13] |
| DockerLaunchBrowser / DockerServiceName / DockerTargetOS / ProjectVersion | true / eshopwcfservice / Windows / 2.1; temp ProjectVersion 2.0 | VS Docker project metadata | [C12:3-9], [C13] |
| DockerServiceUrl | HTTP container address template; route intentionally not repeated here | Root and alternate differ | [C12:6], [C13] |

All secret-bearing environment entries above are masked even where sample values are checked in. No reference syntax to an external secret store is used in those entries. [C11, C13]

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| WCF service | Windows WCF image tag 4.7.2; exposes 80; WORKDIR /inetpub/wwwroot; ARG source defaults COPY input to obj/Docker/publish | No memory or CPU allocation in inspected Docker/Compose files | Not explicitly set; no scaling policy declared. [C10, C11, C17] |
| SQL container | SQL developer image; SA_PASSWORD [MASKED], ACCEPT_EULA=Y | No memory/CPU limit declared | Not explicitly set. [C10, C11] |
| Desktop | net6.0-windows WinExe; STAThread entry; visual styles enabled, compatible text rendering false | No memory/CPU limits declared in project/startup | Process instances are not controlled by scoped deployment files. [C14, C20] |

No JVM settings apply. No .NET GC/environment tuning, readiness restart budget, storage allocation or minimum hardware requirements are stated in the inspected manifests. The SQL image is not version-pinned; resource sizing requires external deployment evidence. [C10, C11, C17]

## Startup Dependency Chain

1. **External nat network → Compose containers:** declared external network must already be available; no network provisioning workflow is provided in these manifests. [C11:22-25]
2. **sql.data → eshopwcfservice:** base Compose lists depends_on with a service name only; no service_healthy condition or application retry/wait command is declared. A SQL healthcheck runs sqlcmd with credentials [MASKED] and `select 1`, interval 1s, retries 20 in root override. Alternate override lacks an explicit retry count; alternate CI artifact sets 20. No explicit healthcheck timeout/start period is supplied. This is a container SQL check, not WCF readiness. [C10:9-10], [C11:17-20], [C13]
3. **Published service files → WCF image:** Dockerfile only copies prepublished output. FolderProfile provides a matching publication directory; no build execution inside the image is declared. [C16, C17]
4. **Reachable WCF service and database → desktop initial display:** Program synchronously invokes LoadView before ShowDialog; that path makes remote calls. No desktop startup timeout/retry or fallback is implemented there. [C20, C21]

Actual startup ordering/readiness was not tested. The incomplete service connection override means these files alone do not demonstrate a working SQL connection. [C4, C11]

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| sql.data environment SA_PASSWORD | Database administrator credential | Literal in root/alternate Compose artifacts: [MASKED]. [C11, C13] |
| sql.data healthcheck sqlcmd password argument | Duplicate database credential | Literal command argument: [MASKED]. [C11, C13] |
| eshopwcfservice environment ConnectionString | Credential-bearing connection configuration | SQL login and incomplete credential placeholder; credential material [MASKED]. [C11, C13] |
| Web.config connectionStrings.EntityModel | Local connection metadata | No explicit password present in named fallback; no authentication success inferred. [C1:62-64] |

### Secrets Provisioning Workflow

The only evidenced flow is **Compose literal environment declaration → container process environment → CatalogConfiguration → EntityModel**. SQL container setup and its healthcheck receive credentials from their manifest entries. There is no referenced managed identity, service principal, RBAC assignment, secret-store retrieval, encrypted configuration provider or rotation pipeline in these scoped files. External provisioning, SQL access controls and encryption at rest remain unknown. [C4, C10, C11, C13, C22]

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| useMockData | Constructor-supplied Boolean; no active default selected by startup | ApplicationModule stores it, but registration logic is commented out. It is not an operational runtime flag. [C23, C20] |
| Operational feature-management flags | None identified in inspected application configuration | No remote flag provider or runtime flag configuration demonstrated. [C1, C4, C5, C14] |

Build DEBUG/TRACE constants and metadata/debug booleans are configuration controls, not business feature-rollout mechanisms. [C1, C19]

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| SDK desktop target | net6.0-windows; SDK version not pinned in project | [C14:1-8] |
| Legacy desktop target / App.config | .NET Framework 4.7.1 / CLR v4.0 declaration | [C15:11], [C5:7-9] |
| WCF service target / web runtime declaration | .NET Framework 4.6.1 | [C19:15], [C1:18-20] |
| WCF container base | mcr.microsoft.com/dotnet/framework/wcf:4.7.2 | [C17:1] |
| SQL image | microsoft/mssql-server-windows-developer, no tag/digest pin | [C10:13] |
| EF | Service 6.1.3; desktop 6.4.4 | [C24], [C14:22] |
| WebApi.Client | 5.2.7 desktop | [C14:23] |
| System.ServiceModel Duplex / Http / NetTcp / Security | 4.8.0 each | [C14:24-27] |
| Connected Service generator | Microsoft.Tools.ServiceModel.Svcutil 2.0.1-preview-30422-0661; design target netcoreapp3.0 | [C6:16], [C7:19] |
| Visual Studio solution metadata | VS 15, 15.0.26430.16 | [C18:2-5] |
| MSBuild project metadata | Service/legacy/Docker ToolsVersion 15.0; publish profile ToolsVersion 4.0 | [C19:1], [C15:2], [C12:2], [C16:6] |
| Docker SDK | Microsoft.Docker.Sdk, version unpinned | [C12:2] |

**Gaps:** static declarations do not establish installed SDK/MSBuild/Docker/SQL versions, actual deployment profile, external identities, successful build, TLS certificate configuration, effective .NET 6 handling of legacy XML settings or resource requirements. No build, container startup or secret provisioning was run.

### Evidence

All locations are absolute and remain inside the requested scope. Property-table labels such as `[C1:7-9]` select that line range from the full source path below.

- [C1] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Web.config:1-65`
- [C2] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Web.Debug.config:5-30`
- [C3] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Web.Release.config:5-31`
- [C4] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:10-17`
- [C5] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\App.config:1-42`
- [C6] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs:16-17`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs:448-479`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs:591-621`
- [C7] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Connected Services\eShopServiceReference\ConnectedService.json:1-22`
- [C8] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Properties\Settings.settings:1-7`
- [C9] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\app.manifest:1-76`
- [C10] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\docker-compose.yml:1-13`
- [C11] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\docker-compose.override.yml:1-25`
- [C12] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\docker-compose.dcproj:1-17`
- [C13] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\temp\docker-compose.yml:1-14`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\temp\docker-compose.override.yml:1-21`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\temp\docker-compose.ci.build.yml:1-22`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\temp\docker-compose.dcproj:1-18`
- [C14] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\eShopWinForms.csproj:1-34`
- [C15] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\eShopWinForms.fx.csproj:1-74`
- [C16] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Properties\PublishProfiles\FolderProfile.pubxml:6-18`
- [C17] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Dockerfile:1-5`
- [C18] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\eShopModernizedNTier.sln:2-29`
- [C19] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\eShopWCFService.csproj:1-71`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\eShopWCFService.csproj:111-131`
- [C20] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Program.cs:16-27`
- [C21] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Controllers\CatalogController.cs:60-158`
- [C22] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\EntityModel.cs:10-16`
- [C23] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Modules\ApplicationModule.cs:6-34`
- [C24] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\packages.config:3`
