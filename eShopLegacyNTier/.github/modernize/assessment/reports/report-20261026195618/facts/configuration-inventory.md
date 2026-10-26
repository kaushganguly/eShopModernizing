# Configuration & Externalized Settings Inventory

The scope contains **four active runtime/build-transform XML configurations** (desktop App.config; service Web.config, Web.Debug.config and Web.Release.config), plus project settings, an empty user-settings definition and saved service-reference metadata. There are no named application runtime profiles or secret-store integrations; a single environment variable can override the service connection string.

All evidence paths below are relative to `D:\a\eShopModernizing\eShopModernizing\eShopLegacyNTier`. Commented examples and generated metadata are not treated as effective runtime overrides.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Desktop runtime | XML | `src\eShopWinForms\App.config:6-30` | Framework runtime and WCF binding/client settings |
| Service runtime | XML | `src\eShopWCFService\Web.config:3-64` | ASP.NET, WCF, EF and database configuration |
| Debug transform | XDT XML | `src\eShopWCFService\Web.Debug.config:5-30` | No active changes; connection/custom-error examples are comments |
| Release transform | XDT XML | `src\eShopWCFService\Web.Release.config:17-18` | Removes compilation debug attribute when transform is applied |
| Connection override | Process environment / code | `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:10-18` | Reads `ConnectionString`; null falls back to named configuration |
| Build / development host | MSBuild projects and solution | `eShopLegacyNTier.sln`; `src\eShopWCFService\eShopWCFService.csproj`; `src\eShopWinForms\eShopWinForms.csproj` | Build variants, targets, references, IIS development settings and desktop publishing properties |
| Service activation | ASP.NET directive | `src\eShopWCFService\CatalogService.svc:1` | CatalogService activation; Debug=true |
| Desktop manifest | XML | `src\eShopWinForms\app.manifest:19,43` | asInvoker privilege and Windows 10 compatibility declaration |
| User settings | XML | `src\eShopWinForms\Properties\Settings.settings:2-6` | Default profile with no settings |
| Generated reference settings | Saved metadata | `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.svcmap:3-30`, `configuration.svcinfo`, `configuration91.svcinfo`, WSDL/XSD/discovery files | Generation-time snapshot, not runtime environment configuration |
| Package declarations | NuGet XML | `src\eShopWCFService\packages.config`, `src\eShopWinForms\packages.config` | Versions in `dependency-map.md` |
| Referenced publishing profile | MSBuild None item | `src\eShopWCFService\eShopWCFService.csproj:100` | References `Properties\PublishProfiles\FolderProfile.pubxml`; file was not present in scoped file inventory |

No appsettings, .env, config-server URI, cloud configuration service, container/orchestration definition, or secret-store reference was found in scope. No `.github\agents` content was accessed.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug / AnyCPU | Default Configuration when unset, or MSBuild Configuration=Debug | Full symbols, optimization off, DEBUG and TRACE; desktop output bin\Debug, service bin | Same declared references in both configurations |
| Release / AnyCPU | MSBuild Configuration=Release | pdbonly, optimization on, TRACE; desktop output bin\Release, service bin | Service XDT removes compilation debug on transform application |
| Web application toolchain | Project import evaluation | Build service as legacy web application | ToolsVersion 15.0, Microsoft.CSharp.targets and Visual Studio WebApplications targets |
| Desktop publishing properties | MSBuild publishing | Disk-oriented publish configuration | PublishUrl=publish\, bootstrapper enabled, updates disabled; not proof of a published installation |

Evidence: service project `src\eShopWCFService\eShopWCFService.csproj:1-43,108-114`; desktop project `src\eShopWinForms\eShopWinForms.csproj:2-49,280`. Building Release alone is not evidence that a transformed file has been deployed.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default desktop | Executable configuration | App.config | Fixed WCF client configuration; no environment-specific file |
| Default service | Hosted Web.config | Web.config | LocalDB named connection and WCF settings |
| Database override | Process environment presence | CatalogConfiguration code | `ConnectionString` replaces named connection selection; empty string is not treated as absent |
| Debug / Release publication | Build/publish transform, not runtime profile | Web.Debug.config / Web.Release.config | Only active Release change is removal of debug |

No ASPNETCORE_ENVIRONMENT or profile composition is implemented. The empty `(Default)` Settings.settings profile has no application values.

## Properties Inventory

### Service

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `ConnectionString` environment variable | Not assigned in repository | Any process environment | `Models\Infrastructure\CatalogConfiguration.cs:16-18` |
| Connection fallback selector | `name=EntityModel` | Default | Same file:10,17 |
| `appSettings/aspnet:UseTaskFriendlySynchronizationContext` | true (bool) | Base config | `src\eShopWCFService\Web.config:8` |
| `system.web/compilation@debug` | true (bool) | Removed by Release transform | Web.config:19; Web.Release.config:18 |
| `system.web/compilation@targetFramework` | 4.6.1 | Base | Web.config:19 |
| `system.web/httpRuntime@targetFramework` | 4.6.1 | Base | Web.config:20 |
| `serviceMetadata@httpGetEnabled`, `@httpsGetEnabled` | true, true (bools) | Base | Web.config:27 |
| `serviceDebug@includeExceptionDetailInFaults` | false (bool) | Base | Web.config:29 |
| `protocolMapping/add@binding`, `@scheme` | basicHttpsBinding, https | Base | Web.config:34 |
| `serviceHostingEnvironment@aspNetCompatibilityEnabled`, `@multipleSiteBindingsEnabled` | true, true | Base | Web.config:36 |
| Application endpoint binding / configuration / contract | basicHttpBinding / empty / eShopWCFService.ICatalogService | Base | Web.config:38-40; HTTP paths intentionally cataloged only in api-service-contracts.md |
| Metadata endpoint binding / contract | mexHttpBinding / IMetadataExchange | Base | Web.config:40 |
| `system.webServer/modules@runAllManagedModulesForAllRequests` | true | Base | Web.config:45 |
| `system.webServer/directoryBrowse@enabled` | true | Base | Web.config:50 |
| `entityFramework/defaultConnectionFactory@type` | LocalDbConnectionFactory | Base | Web.config:53 |
| Factory parameter | mssqllocaldb | Base | Web.config:55 |
| `entityFramework/providers/provider@invariantName` | System.Data.SqlClient | Base | Web.config:59 |
| Provider service type | System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer | Base | Web.config:59 |
| `connectionStrings/EntityModel` | `Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=eShopDatabase;Persist Security Info=True;` | Bypassed by environment override | Web.config:63; no credential value in checked-in string |
| `connectionStrings/EntityModel@providerName` | System.Data.SqlClient | Base | Web.config:63 |
| EF config-section registration | EntityFrameworkSection, assembly Version=6.0.0.0; requirePermission=false | Base | Web.config:5; assembly version is not NuGet package version |

For shorthand entries above, `Models\Infrastructure` is under `src\eShopWCFService`.

### Desktop and generated reference

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `startup/supportedRuntime@version`, `@sku` | v4.0 / .NETFramework,Version=v4.7 | Default | `src\eShopWinForms\App.config:7` |
| Named WCF binding | BasicHttpBinding_ICatalogService; basicHttpBinding, empty body | Default; no explicit timeout/security overrides | App.config:20-23 |
| Client endpoint address | Fixed HTTP localhost address on port 62314; full path in API inventory | Default | App.config:26-28 |
| Client contract / endpoint name | eShopServiceReference.ICatalogService / BasicHttpBinding_ICatalogService | Default | App.config:28 |
| Windows Forms compatibility switches | No active entries | Default | App.config:9-18; all examples commented |
| User settings | Empty | `(Default)` | `src\eShopWinForms\Properties\Settings.settings:2-6` |
| GenerateAsynchronousMethods / GenerateTaskBasedAsynchronousMethod | false / true | Generation only | `Reference.svcmap:4-5` |
| EnableDataBinding / GenerateSerializableTypes | true / true | Generation only | Reference.svcmap:6,13 |
| ImportXmlTypes / GenerateInternalTypes / GenerateMessageContracts | false / false / false | Generation only | Reference.svcmap:8-10 |
| Serializer / UseSerializerForFaults / ReferenceAllAssemblies | Auto / true / true | Generation only | Reference.svcmap:14-16 |
| ExcludedTypes / NamespaceMappings / CollectionMappings / ReferencedAssemblies / ReferencedDataContractTypes / ServiceContractMappings | Empty | Generation only | Reference.svcmap:7,11-12,17-19 |

`Reference.svcmap` shorthand denotes `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.svcmap`. Its metadata source and saved `.svcinfo`/WSDL/XSD settings are a design-time snapshot. Nullable catalog filters in the snapshot differ from current server source; see `api-service-contracts.md`. They must not be mistaken for additional live configuration sources.

### Build and packaging settings

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| Configuration / Platform | Debug / AnyCPU | Both | Both project files:4-6 (desktop:5-6) |
| OutputType / TargetFrameworkVersion | Library / v4.6.1 service; WinExe / v4.7 desktop | Both | Service project:11-15; desktop:8-11 |
| Desktop AutoGenerateBindingRedirects | true | Both | Desktop project:13 |
| DebugSymbols / DebugType / Optimize / DefineConstants | Debug: true / full / false / DEBUG;TRACE; Release: pdbonly / true / TRACE | Both | Service project:27-42; desktop:31-48 |
| ErrorReport / WarningLevel | prompt / 4 | Both configurations | Service project:33-34,41-42; desktop:38-39,47-48 |
| Desktop PublishUrl / Install / InstallFrom | publish\ / true / Disk | Publishing | Desktop project:15-17 |
| Desktop UpdateEnabled / UpdateMode / UpdateInterval / UpdateIntervalUnits | false / Foreground / 7 / Days | Publishing | Desktop project:18-21 |
| Desktop UpdatePeriodically / UpdateRequired | false / false | Publishing | Desktop project:22-23 |
| Desktop MapFileExtensions / ApplicationRevision / ApplicationVersion | true / 0 / 1.0.0.%2a | Publishing | Desktop project:24-26 |
| Desktop IsWebBootstrapper / UseApplicationTrust / BootstrapperEnabled | false / false / true | Publishing | Desktop project:27-29 |
| Service UseIISExpress / UseIIS / AutoAssignPort | true / True / True | Development | Service project:18,119-120 |
| Service DevelopmentServerPort / virtual directory | 62314 / root | Development | Service project:121-122 |
| Service NTLMAuthentication / UseCustomServer / SaveServerSettingsInUserFile | False / False / False | Development | Service project:124-128 |
| Service SSL port and IIS Express authentication properties | Unset | Development | Service project:19-24 |

Internal GUIDs, assembly paths, resource lists and empty MSBuild placeholders are not external runtime properties. No additional property placeholders or remote property-resolution chain was found.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopWinForms | CLR v4; STAThread; visual styles; asInvoker; AnyCPU | Unspecified | No scaling declaration; one form per process in source |
| eShopWCFService | IIS/IIS Express hosting; .NET Framework 4.6.1; optional process environment connection override | Unspecified | No worker/replica count declared |
| LocalDB / alternative SQL Server | Installation and permissions external to this scope | Unspecified | Unspecified |

No JVM options apply, and no CPU limits, container images, memory allocations, autoscaling or startup command-line arguments are declared. Evidence: `src\eShopWinForms\Program.cs:16-27`, `src\eShopWinForms\app.manifest:19`, service project:15-24.

## Startup Dependency Chain

1. Host/environment and SQL Server availability/permissions must exist before successful service data access. WCF host activates CatalogService, whose default constructor creates EntityModel; EF initializer runs when initialization/data access is triggered (`src\eShopWCFService\CatalogService.svc.cs:18-20`, `src\eShopWCFService\EntityModel.cs:12-15`).
2. Desktop startup constructs the generated client, then calls `LoadView` **before** `ShowDialog` (`src\eShopWinForms\Program.cs:22-27`).
3. `LoadView` performs discount/catalog/lookups/shipment-list requests; the service and database must respond for normal startup completion (`src\eShopWinForms\Controllers\CatalogController.cs:150-157`).
4. Catalog rendering requires local image files at a working-directory-relative asset path (`src\eShopWinForms\Views\CatalogView.cs:55-59`).

No automatic service orchestration, readiness probe, wait-for-TCP mechanism, startup retry, health gate or configured startup timeout exists in scoped source. Network timeouts remain WCF defaults, not explicit source values.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| Process environment `ConnectionString` | Potential credential-bearing DB connection | `[MASKED if supplied]`; repository supplies no value |
| Named `EntityModel` connection | DB location / security-sensitive configuration | Checked-in LocalDB string contains no password or username; no secret value reproduced |
| WCF/IIS credentials or TLS certificates | None configured in inspected settings | External hosting settings unknown |

No actual credential value, API key, Key Vault reference, DPAPI-protected section or encrypted property is defined in the inspected runtime settings.

### Secrets Provisioning Workflow

No provisioning pipeline or managed-identity/service-principal/RBAC binding is defined in scope. The only implemented external-input flow is: operator or hosting environment supplies process `ConnectionString` → static reader retrieves it → EntityModel receives it; otherwise the named Web.config connection is used. Access identity and database credentials depend on that supplied string and host configuration, and cannot be established from this source. Environment variables are not a dedicated secret-management system.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| Business feature flags | None detected | No feature-management framework or business toggle reader found |
| Operational debug / metadata / directory listing settings | Listed above | Web.config and Release transform; not rollout flags |

The `CatalogServiceMock` class is not selected by a configuration flag: activation points to CatalogService (`src\eShopWCFService\CatalogService.svc:1`).

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Desktop .NET Framework | 4.7 target / CLR v4.0 | Desktop project:11; App.config:7 |
| Service .NET Framework / ASP.NET targets | 4.6.1 | Service project:15; Web.config:19-20 |
| EF | NuGet 6.1.3; assembly 6.0.0.0 | Both packages.config:3; service project:45-49 |
| Microsoft.AspNet.WebApi.Client | 5.2.3 | Desktop packages.config:4 |
| Newtonsoft.Json | 6.0.4 | Desktop packages.config:5 |
| WCF / Windows Forms / ADO.NET | Framework supplied; no independent package version | Project assembly references |
| Visual Studio / solution | VS15 declaration, 15.0.26730.16; solution format 12.00 | `eShopLegacyNTier.sln:2-5` |
| MSBuild | ToolsVersion 15.0 declaration; installed version unknown | Both project opening elements |
| SQL Server / IIS | Exact installed versions unknown | LocalDB factory and IIS development settings are not version evidence |
| Docker base image | None declared | No scoped Dockerfile |

**Limitations:** no build/tests, deployment, external environment lookup or service interrogation. This is source-defined configuration, not a claim that committed development values are used in production.
