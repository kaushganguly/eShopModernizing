# Configuration & Externalized Settings Inventory

The scoped project has two runtime XML configuration files, two web build-transform files, an empty desktop settings definition and a desktop application manifest, plus two MSBuild project files. A process environment variable can override the database connection; no secret-store integration or named environment-profile system is declared.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Service configuration | Runtime XML | `src\eShopWCFService\Web.config:1-65` | App setting, ASP.NET/WCF hosting, EF provider and named connection |
| Desktop configuration | Runtime XML | `src\eShopWinForms\App.config:1-31` | CLR selection, empty Forms configuration section, WCF client |
| Web transforms | Build/publish XML | `src\eShopWCFService\Web.Debug.config:5-30`; `Web.Release.config:5-31` | Debug has no effective overrides; Release removes compilation debug attribute |
| Desktop settings | Generated settings input | `src\eShopWinForms\Properties\Settings.settings:1-7` | Default profile only; no settings entries |
| Application manifest | Windows process metadata | `src\eShopWinForms\app.manifest:3-43` | Runs as invoking user; Windows 10 compatibility declaration |
| WCF activation directive | Service-host metadata | `src\eShopWCFService\CatalogService.svc:1` | Selects hosted service class; includes a debug directive |
| Build and packaging | MSBuild | `src\eShopWCFService\eShopWCFService.csproj`; `src\eShopWinForms\eShopWinForms.csproj` | Framework targets, build configurations, hosting and ClickOnce-related settings |
| Connection override | Process environment | `ConnectionString` read in `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:10-18` | Environment value wins over `name=EntityModel`; unset, not empty, triggers fallback |
| Publish-profile reference | MSBuild `None` entry | `src\eShopWCFService\eShopWCFService.csproj:100` | `Properties\PublishProfiles\FolderProfile.pubxml` is referenced but absent from the inspected scoped tree; contents unknown |
| Service-reference metadata | Design-time WCF generation input | `src\eShopWinForms\eShopWinForms.csproj:122-134,258-265` | WSDL, XSD, svcmap and svcinfo files; not independent runtime environment profiles |

No Docker, Kubernetes, external configuration repository, Key Vault, Vault or remote config-server file/reference was found in the scoped project tree. The UWP-style `Helpers\SettingsStorageExtensions.cs` file is not in the desktop compile list and is not an active settings source (`src\eShopWinForms\eShopWinForms.csproj:89-103,109-113,155-159`).

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug / Any CPU | Default when `Configuration` is unset, or MSBuild configuration selection | Both projects: full debug information, symbols enabled, optimization off, `DEBUG;TRACE`, warning level 4, error report `prompt` | Same dependencies; service output `bin\`, client `bin\Debug\` |
| Release / Any CPU | Explicit configuration selection | Both projects: `pdbonly`, optimization on, `TRACE`, warning level 4, error report `prompt`; web publish transform removes compilation `debug` | Same dependencies; service output `bin\`, client `bin\Release\` |

Evidence: `eShopLegacyNTier.sln:11-23`; `src\eShopWCFService\eShopWCFService.csproj:4-5,27-42`; `src\eShopWinForms\eShopWinForms.csproj:5-6,31-48`; `src\eShopWCFService\Web.Release.config:17-18`. Transforms are build/publish inputs, not runtime profile selection.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default | Normal application startup | `Web.config`, `App.config` | Local service configuration and named database connection |
| Environment connection override | Set process variable before starting service host | `CatalogConfiguration.cs:16-17` | Only connection selection; no named profile activation |
| Desktop settings `(Default)` | Generated settings infrastructure | `Properties\Settings.settings:2-6` | No settings or overrides defined |

No `ASPNETCORE_ENVIRONMENT`, named development/production JSON files, runtime profile composition or runtime Debug/Release configuration branch is declared. Project build configurations must not be presented as environment profiles.

## Properties Inventory

### eShopWCFService

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `appSettings/aspnet:UseTaskFriendlySynchronizationContext` | Boolean `true` | Default | `src\eShopWCFService\Web.config:7-8` |
| `system.web/compilation/@debug` | Boolean `true`; removed by Release web transform | Build-dependent publish output | `Web.config:19`; `Web.Release.config:18` |
| `system.web/compilation/@targetFramework`; `httpRuntime/@targetFramework` | `4.6.1` for both | Default | `Web.config:19-20` |
| `serviceMetadata/@httpGetEnabled`; `@httpsGetEnabled` | Boolean `true` for both | Default | `Web.config:27` |
| `serviceDebug/@includeExceptionDetailInFaults` | Boolean `false` | Default | `Web.config:29` |
| `protocolMapping/add/@scheme`; `@binding` | `https`; `basicHttpsBinding` | Default | `Web.config:33-35` |
| `serviceHostingEnvironment/@aspNetCompatibilityEnabled`; `@multipleSiteBindingsEnabled` | Boolean `true` for both | Default | `Web.config:36` |
| `services/service/@name` | `eShopWCFService.CatalogService` | Default | `Web.config:38` |
| Application endpoint `@binding`; `@bindingConfiguration`; `@contract` | `basicHttpBinding`; empty; `eShopWCFService.ICatalogService` | Default | `Web.config:39`; address inventory is in `api-service-contracts.md` |
| Metadata endpoint `@binding`; `@contract` | `mexHttpBinding`; `IMetadataExchange` | Default | `Web.config:40` |
| `modules/@runAllManagedModulesForAllRequests` | Boolean `true` | Default | `Web.config:45` |
| `directoryBrowse/@enabled` | Boolean `true` | Default | `Web.config:50` |
| `entityFramework/defaultConnectionFactory/@type` | `System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework` | Default | `Web.config:53` |
| Default connection factory parameter | `mssqllocaldb` | Default | `Web.config:55` |
| `providers/provider/@invariantName`; `@type` | `System.Data.SqlClient`; `System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer` | Default | `Web.config:59` |
| `connectionStrings/EntityModel` | `Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=eShopDatabase;Persist Security Info=True;` — no credential fields present | Default | `Web.config:63` |
| `connectionStrings/EntityModel/@providerName` | `System.Data.SqlClient` | Default | `Web.config:63` |
| Process environment `ConnectionString` | Unset; resolver returns `name=EntityModel` | Environment override | `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:10-18` |
| `.svc` `ServiceHost/Service`; `Debug`; `CodeBehind` | `eShopWCFService.CatalogService`; `true`; `CatalogService.svc.cs` | Default host activation | `src\eShopWCFService\CatalogService.svc:1` |

### eShopWinForms

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `startup/supportedRuntime/@version`; `@sku` | `v4.0`; `.NETFramework,Version=v4.7` | Default | `src\eShopWinForms\App.config:6-7` |
| `System.Windows.Forms.ApplicationConfigurationSection` | Empty; DPI switches in comments are inactive | Default | `App.config:9-18` |
| `basicHttpBinding/binding/@name` | `BasicHttpBinding_ICatalogService`; no custom attributes | Default | `App.config:20-23` |
| Client endpoint `@binding`; `@bindingConfiguration`; `@name` | `basicHttpBinding`; `BasicHttpBinding_ICatalogService`; same name | Default | `App.config:26-28`; address inventory is in `api-service-contracts.md` |
| Client endpoint `@contract` | `eShopServiceReference.ICatalogService` | Default | `App.config:28` |
| User settings | No entries | `(Default)` | `src\eShopWinForms\Properties\Settings.settings:2-6` |
| `requestedExecutionLevel/@level`; `@uiAccess` | `asInvoker`; Boolean `false` | Default | `src\eShopWinForms\app.manifest:19` |
| `supportedOS` | Windows 10 compatibility declaration | Default | `app.manifest:42-43` |
| Image asset directory | Hardcoded `Environment.CurrentDirectory` plus `\..\..\Assets\Images\Catalog\` | All | `src\eShopWinForms\Views\CatalogView.cs:55-58`; source-level dependency, not an externalized setting |

### Build, hosting and packaging settings

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| Both projects: `AutoGenerateBindingRedirects` | `true` | All builds | `src\eShopWCFService\eShopWCFService.csproj:17`; `src\eShopWinForms\eShopWinForms.csproj:13` |
| Service: `OutputType`; `RootNamespace`; `AssemblyName` | `Library`; `WcfService1`; `WcfService1` | All builds | `src\eShopWCFService\eShopWCFService.csproj:11-15`; project display name differs from declared assembly name |
| Desktop: `OutputType`; `RootNamespace`; `AssemblyName`; `FileAlignment` | `WinExe`; `eShopWinForms`; `eShopWinForms`; `512` | All builds | `src\eShopWinForms\eShopWinForms.csproj:8-12` |
| Desktop: `ApplicationManifest` | `app.manifest` | All builds | `src\eShopWinForms\eShopWinForms.csproj:53-55` |
| Service: `WcfConfigValidationEnabled`; `UseIISExpress` | `True`; `true` | Development tooling | `src\eShopWCFService\eShopWCFService.csproj:16-18` |
| Service: `Use64BitIISExpress`, SSL/authentication/pipeline settings | Empty values; not explicit enablement | Development tooling | `eShopWCFService.csproj:19-24` |
| Service: `UseIIS`; `AutoAssignPort`; `DevelopmentServerPort`; `DevelopmentServerVPath` | `True`; `True`; `62314`; `/` | Development tooling | `eShopWCFService.csproj:119-123` |
| Service: `NTLMAuthentication`; `UseCustomServer`; `CustomServerUrl`; `SaveServerSettingsInUserFile` | `False`; `False`; empty; `False` | Development tooling | `eShopWCFService.csproj:124-128` |
| Desktop: `PublishUrl`; `Install`; `InstallFrom` | `publish\`; `true`; `Disk` | Packaging | `src\eShopWinForms\eShopWinForms.csproj:15-17` |
| Desktop: `UpdateEnabled`; `UpdateMode`; `UpdateInterval`; `UpdateIntervalUnits` | `false`; `Foreground`; `7`; `Days` | Packaging | `eShopWinForms.csproj:18-21` |
| Desktop: `UpdatePeriodically`; `UpdateRequired`; `MapFileExtensions` | `false`; `false`; `true` | Packaging | `eShopWinForms.csproj:22-24` |
| Desktop: `ApplicationRevision`; `ApplicationVersion` | `0`; `1.0.0.*` | Packaging | `eShopWinForms.csproj:25-26` |
| Desktop: `IsWebBootstrapper`; `UseApplicationTrust`; `BootstrapperEnabled` | `false`; `false`; `true` | Packaging | `eShopWinForms.csproj:27-29` |
| Desktop bootstrapper `.NET Framework 3.5 SP1/Install` | `false` | Packaging; inactive prerequisite | `eShopWinForms.csproj:271-276` |

MSBuild resolves imports from framework/tooling properties such as `MSBuildExtensionsPath`, `MSBuildToolsVersion`, `MSBuildBinPath` and `VSToolsPath`; the service falls back to `VisualStudioVersion=10.0` only when unset (`src\eShopWCFService\eShopWCFService.csproj:2,108-114`). No application property-placeholder engine is declared. Database resolution is process environment value → named connection if null → EF provider; an empty environment string is not filtered out.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopWinForms | CLR v4, .NET Framework 4.7 target; STA thread; Windows GUI; `asInvoker`; no command-line options parsed | No explicit memory or CPU allocation | Not specified |
| eShopWCFService | .NET Framework 4.6.1 target; IIS/IIS Express activation; development port 62314; optional process `ConnectionString` | No explicit memory, CPU or worker-process limits | Not specified |
| LocalDB | Installed SQL Server LocalDB instance expected for default connection | Not specified | No scaling declaration |

Evidence: `src\eShopWinForms\Program.cs:16-27`; `src\eShopWinForms\App.config:6-7`; `src\eShopWinForms\app.manifest:19`; `src\eShopWCFService\eShopWCFService.csproj:15-24,119-124`; `src\eShopWCFService\Web.config:53-64`. There are no JVM options, container resource limits or autoscaling settings in scope.

## Startup Dependency Chain

1. **Database availability → service data access:** default LocalDB installation/instance must be usable; context initialization registers missing-database creation and seed data. No explicit readiness probe or wait loop is declared (`src\eShopWCFService\EntityModel.cs:12-16`; `src\eShopWCFService\Models\Infrastructure\CatalogDBInitializer.cs:11-20`).
2. **IIS/IIS Express → WCF service activation:** `.svc` activates `CatalogService`, whose default constructor creates the context. No custom host-start orchestration is checked in (`src\eShopWCFService\CatalogService.svc:1`; `src\eShopWCFService\CatalogService.svc.cs:18-21`).
3. **Service availability → desktop initial display:** startup invokes `LoadView` before showing the modal catalog window. Initial SOAP requests require the service/database; there is no startup retry, custom timeout or health-based gating (`src\eShopWinForms\Program.cs:22-27`; `src\eShopWinForms\Controllers\CatalogController.cs:150-157`).
4. **Local assets → catalog rendering:** image files must exist at the working-directory-relative folder; the code has no missing-image fallback (`src\eShopWinForms\Views\CatalogView.cs:55-61`).

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `ConnectionString` process variable | Potential credential-bearing database connection | Runtime value unknown; `[MASKED]` if populated |
| Named `EntityModel` connection | Operationally sensitive database location | Checked-in XML; no username/password present; do not assume production credentials |
| External secret-store reference | None detected | No Key Vault, Vault, managed-identity or encrypted-section binding declared |

Evidence: `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:16-17`; `src\eShopWCFService\Web.config:62-64`. No actual credentials or connection passwords are reproduced.

### Secrets Provisioning Workflow

The checked-in workflow is simply **operator/host-provided process environment → `CatalogConfiguration` → EF connection selection → service database access**. If the variable is absent, configuration falls back to the XML named connection. No deployment script establishing the variable, secret retrieval identity, RBAC grant, rotation mechanism, DPAPI-protected section or managed secret-store provisioning was found. The default local connection has no explicit credential fields; actual database authentication and permissions require host-side verification rather than inventing a provisioning process.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| Business feature-flag framework | None detected | No active feature-management dependency or conditional registration |
| Desktop automatic update | `UpdateEnabled=false` | Packaging property; operational switch, not a business feature flag (`src\eShopWinForms\eShopWinForms.csproj:18`) |
| High-DPI workaround switches | Inactive comments only | `src\eShopWinForms\App.config:9-16`; not enabled feature flags |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Service .NET Framework target | 4.6.1 | `src\eShopWCFService\eShopWCFService.csproj:15`; `Web.config:19-20` |
| Desktop .NET Framework target | 4.7 | `src\eShopWinForms\eShopWinForms.csproj:11`; `App.config:7` |
| CLR configuration | v4.0 | `src\eShopWinForms\App.config:7`; not proof of installed runtime patch |
| Entity Framework | NuGet 6.1.3, assembly references 6.0.0.0 | Both package manifests; `src\eShopWCFService\eShopWCFService.csproj:45-49` |
| Newtonsoft.Json | NuGet 6.0.4, assembly reference 6.0.0.0 | `src\eShopWinForms\packages.config:5`; `eShopWinForms.csproj:63-64` |
| Microsoft.AspNet.WebApi.Client | 5.2.3 | `src\eShopWinForms\packages.config:4` |
| Project tooling schema | MSBuild ToolsVersion 15.0 | Both `.csproj` opening elements; not installed MSBuild version |
| Solution tooling metadata | Visual Studio 15.0.26730.16, solution format 12.00 | `eShopLegacyNTier.sln:2-5` |
| WCF / Windows Forms | Supplied by .NET Framework | Project framework assembly references; not separate pinned package versions |
| SQL Server LocalDB / IIS Express | Version not pinned | `src\eShopWCFService\Web.config:53-55`; `eShopWCFService.csproj:18` |
| Container base image | None declared | No Dockerfile in scoped tree |
