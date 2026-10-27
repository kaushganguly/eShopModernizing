# Configuration & Externalized Settings Inventory

The scoped application has XML application configuration, Debug/Release transforms, generated client configuration, and root Compose deployment overrides. Secrets are supplied directly in checked-in deployment definitions rather than through an evidenced secret-store workflow; all credential values are intentionally masked here [C1-C8].

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Service Web.config | Runtime XML | Absolute path in [C1] | EF provider, named connection, WCF behavior, hosting settings |
| Web.Debug.config / Web.Release.config | Publish transforms | [C2] | Debug has examples/comments only; Release removes compilation debug attribute |
| Desktop App.config | Legacy runtime XML | [C3] | Framework startup declaration and WCF endpoint are legacy; selected SDK client constructor uses generated binding/address |
| Generated Reference.cs | Executable configuration | [C4] | Active default client binding and address are code, not environment-aware configuration |
| ConnectedService.json | Design-time tooling | [C5] | Input service address, arrays, namespace mapping, synchronous methods, historical target setting |
| CatalogConfiguration | Runtime environment | [C6] | Reads process environment before named connection fallback |
| Root docker-compose.yml / override | Deployment YAML | [C7] | Base build/dependency topology; override ports/environment/SQL readiness/network |
| docker-compose.dcproj / Dockerfile / FolderProfile.pubxml | Build and publish | [C8] | Windows target, WCF image, file-system publish output |
| temp Compose files | Alternate deployment artifacts | [C9] | Not selected by root Docker project; base build context differs from root topology |
| Project files | Build configuration | [C10-C12] | Selected SDK desktop; classic service; alternate desktop Framework project |

No appsettings/environment-specific JSON, Key Vault/Vault references, remote configuration service or secret-store binding is evidenced by these sources. No files outside the requested application scope were assessed.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug / Any CPU | Solution configuration, Debug default in classic service | Symbols, no optimization, DEBUG and TRACE | Classic MSBuild 15 project; WebApplication targets [C10, C13] |
| Release / Any CPU | Solution selection or MSBuild configuration | Optimization, TRACE, pdbonly | Release web transform removes debug compilation attribute [C2, C10] |
| SDK desktop Debug / Release | Solution selection | Windows executable; SDK defaults unless overridden | Microsoft.NET.Sdk, WinForms and WPF; no custom conditional groups [C11, C13] |
| FileSystem publish | Select FolderProfile | Release / Any CPU recorded; output Docker publish directory | `WebPublishMethod=FileSystem`, `ExcludeApp_Data=False`, `DeleteExistingFiles=False` [C8] |
| Alternate desktop classic project | Must explicitly build `.fx.csproj` | Framework 4.7.1 desktop / ClickOnce settings | Not referenced by selected solution; installation from Disk, updates disabled [C12, C13] |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Named-connection fallback | ConnectionString environment variable absent | Web.config | LocalDB eShopDatabase [C1, C6] |
| Root Compose deployment | Explicit Compose launch with root override | Root YAML | SQL container connection, published ports, external nat network [C7] |
| Mock alternative | No active runtime selector | ApplicationModule code scaffold | `useMockData` stored but registrations commented; startup constructs WCF proxy directly [C14] |

These are execution modes inferred from wiring, not ASPNETCORE_ENVIRONMENT profiles. Debug/Release are build/publish configurations, not independent runtime profiles.

## Properties Inventory

### Service and persistence

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `ConnectionString` process environment | Absent; falls back to `name=EntityModel` | Compose supplies a string for server `sql.data`, database `eShopDatabase`; credential portion `[MASKED]`; checked-in entry contains placeholder-like tail, not a verified working connection | C6, C7 |
| `connectionStrings/EntityModel` | `Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=eShopDatabase;Persist Security Info=True;` | No active transform override | C1:62-64 |
| `providerName` / provider invariant | System.Data.SqlClient | All service modes | C1:58-63 |
| `entityFramework/defaultConnectionFactory` | LocalDbConnectionFactory; parameter mssqllocaldb | Base XML | C1:52-60 |
| `aspnet:UseTaskFriendlySynchronizationContext` | Boolean true | Base XML | C1:7-9 |
| `system.web/compilation` | debug true, targetFramework 4.6.1 | Release removes debug attribute | C1:18-20; C2 |
| `system.web/httpRuntime/targetFramework` | 4.6.1 | Base XML | C1:20 |
| `serviceMetadata` | httpGetEnabled true; httpsGetEnabled true | Base XML | C1:25-29 |
| `serviceDebug/includeExceptionDetailInFaults` | false | Base XML | C1:29 |
| `protocolMapping` | HTTPS scheme maps to basicHttpsBinding | Base XML; not proof of deployed listener | C1:33-35 |
| Service binding / metadata binding | basicHttpBinding / mexHttpBinding | Explicit service configuration | C1:37-41 |
| `serviceHostingEnvironment` | aspNetCompatibilityEnabled true; multipleSiteBindingsEnabled true | Base XML | C1:36 |
| `system.webServer/modules` | runAllManagedModulesForAllRequests true | Base XML | C1:44-45 |
| `directoryBrowse/enabled` | true | Base XML | C1:50 |

### Desktop, proxy and generation

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `supportedRuntime` | CLR v4.0, Framework 4.7.1 | Legacy App.config; not selected SDK target | C3:7-9 |
| `DpiAwareness` | PerMonitorV2 | Legacy App.config | C3:10-12 |
| XML WCF binding | BasicHttpBinding_ICatalogService; otherwise defaults | Legacy XML, not generated default constructor's configuration source | C3:13-30; C4 |
| Generated binding limits | MaxBufferSize and MaxReceivedMessageSize int.MaxValue; ReaderQuotas Max; AllowCookies true | Active generated client | C4:591-600 |
| Generated remote address | Localhost HTTP, host port 5113 | Compiled default; overloads allow caller-supplied address but startup does not use them | C4:448-476,605-621 |
| ConnectedService `ProviderId` / `Version` | Microsoft.VisualStudio.ConnectedService.Wcf / 15.0.30423.811 | Design time | C5:2-3 |
| `collectionTypes` / `namespaceMappings` | System.Array, Dictionary; eShopWinForms.eShopServiceReference | Design time | C5:11-17 |
| `sync` / `targetFramework` / `typeReuseMode` | true / netcoreapp3.0 / None | Historical generation settings, not actual selected project target | C5:18-20 |
| Desktop EF provider/factory | System.Data.SqlClient / LocalDbConnectionFactory mssqllocaldb | Legacy XML; not evidence of active desktop DB use | C3:32-40 |

### Deployment settings

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| Compose schema version | 3.4 | Root YAML; alternates use 3 | C7, C9 |
| `eshopwcfservice.image` / build | eshop/wcfservice; source context src\eShopWCFService, Dockerfile | Root base | C7 |
| `sql.data.image` | microsoft/mssql-server-windows-developer; no explicit tag | Root base | C7 |
| `SA_PASSWORD` | `[MASKED]` | SQL container environment | C7 |
| `ACCEPT_EULA` | Y | SQL container environment | C7 |
| Published ports | WCF 5113 to 80; SQL 5433 to 1433 | Root override | C7 |
| SQL healthcheck | sqlcmd command; credentials `[MASKED]`; interval 1s; retries 20 | Root override | C7 |
| Default network | External network named nat | Root override | C7 |
| Dockerfile `source` build argument | Fallback obj/Docker/publish | Build argument; image copy source | C8 |
| `DockerTargetOS` / `DockerServiceName` / `DockerLaunchBrowser` | Windows / eshopwcfservice / true | Docker project; project version 2.1 | C8 |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopWinForms | Windows GUI, STAThread; SDK target net6.0-windows | No minimum/limit declared | No explicit scale setting |
| eShopWCFService | IIS WCF image; listens on container port 80; runtime connection environment | No CPU/memory requests or limits declared | No replicas declaration |
| sql.data | Windows SQL image; credential/EULA environment | No CPU/memory/disk quota declared | No replicas declaration |

No JVM parameters apply. Root Compose declares no named volume or bind mount for database durability. Requirements such as minimum RAM, supported Windows host/container build, and production instance counts cannot be inferred [C7, C8, C11, C14].

## Startup Dependency Chain

1. An existing Windows-compatible external `nat` network is referenced, not created by root Compose [C7].
2. `eshopwcfservice` declares `depends_on: sql.data`. SQL has a healthcheck at 1-second interval with 20 retries; **there is no service_healthy condition or application retry loop**, so start ordering is not proof SQL is ready. Timeout/start-period values are not explicitly set [C7].
3. EF initialization is registered in the DbContext constructor and runs on database initialization/access, creating and seeding only a missing database [C6, C15].
4. Desktop startup calls `LoadView` before showing its modal dialog, so service/data/image failures can prevent a usable initial view. No explicit wait/readiness loop is present [C14].

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| Root override `SA_PASSWORD` | SQL administrative credential | Checked-in environment literal `[MASKED]` [C7] |
| Root override SQL healthcheck password argument | SQL credential | Checked-in command argument `[MASKED]` [C7] |
| Root override `ConnectionString` | Connection configuration, potentially credential-bearing | Checked-in environment value, sensitive portion `[MASKED]`; not verified operational [C7] |
| temp override / CI build YAML equivalents | Alternate deployment credentials | Checked-in environment/command entries `[MASKED]` [C9] |
| XML transform example connection strings | Commented examples, not active provisioning | Credential-like example text deliberately omitted [C2] |

### Secrets Provisioning Workflow

The observed root flow is deployment YAML → process environment in SQL/WCF containers → service connection selector → EF connection. SQL healthcheck also embeds its credential argument. No managed identity, service principal, RBAC grant, encrypted secret provider, or automated rotation/provisioning sequence is evidenced in this scope. The LocalDB fallback uses its named XML connection without an explicit password. Database setup privileges and identity behavior outside these files are not verified [C1, C6, C7].

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `useMockData` | Constructor argument, no active default selected | Inactive ApplicationModule; registration body commented out |
| No operational feature flag framework detected | Not applicable | Active startup directly constructs generated proxy |

ConnectionString selects a connection, not mock mode. Debug compilation and metadata/debug behavior switches are configuration toggles, not business feature rollout controls [C1, C6, C14].

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Selected desktop target | net6.0-windows | C11 |
| Alternate desktop target | .NET Framework 4.7.1 | C12 |
| Service compilation / web runtime target | .NET Framework 4.6.1 | C1, C10 |
| WCF container base | mcr.microsoft.com/dotnet/framework/wcf:4.7.2 | C8 |
| Server EntityFramework | Package 6.1.3; assembly identity 6.0.0.0 | C10 |
| Desktop EntityFramework / WebApi.Client | 6.4.4 / 5.2.7 | C11 |
| Desktop WCF client packages | 4.8.0 for Duplex, Http, NetTcp, Security | C11 |
| SQL container | Untagged Windows developer image; engine version unverified | C7 |
| Classic project ToolsVersion / Visual Studio solution | 15.0 / Visual Studio 15 header | C10, C13 |
| SDK/compiler installation | Not pinned by these scoped declarations | C11 |

### Source evidence

- **[C1]** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Web.config:3-64`
- **[C2]** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Web.Debug.config:5-30`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Web.Release.config:5-30`
- **[C3]** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\App.config:3-41`
- **[C4]** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs:448-476,591-621`
- **[C5]** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Connected Services\eShopServiceReference\ConnectedService.json:1-22`
- **[C6]** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:10-18`
- **[C7]** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\docker-compose.yml:1-13`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\docker-compose.override.yml:1-25`
- **[C8]** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\docker-compose.dcproj:2-15`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Dockerfile:1-5`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Properties\PublishProfiles\FolderProfile.pubxml:6-17`
- **[C9]** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\temp\docker-compose.yml:1-14`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\temp\docker-compose.override.yml:1-21`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\temp\docker-compose.ci.build.yml:1-22`
- **[C10]** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\eShopWCFService.csproj:1-49,111-127`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\packages.config:3`
- **[C11]** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\eShopWinForms.csproj:1-34`
- **[C12]** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\eShopWinForms.fx.csproj:4-48`
- **[C13]** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\eShopModernizedNTier.sln:2-29`
- **[C14]** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Program.cs:16-27`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Modules\ApplicationModule.cs:6-34`
- **[C15]** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\EntityModel.cs:12-16`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Models\Infrastructure\CatalogDBInitializer.cs:11-79`
