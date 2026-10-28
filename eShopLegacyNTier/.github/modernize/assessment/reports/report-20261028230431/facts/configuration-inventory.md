# Configuration & Externalized Settings Inventory

The scoped workspace has four host/client XML configuration files, two package manifests, two project files, an empty desktop settings file and service-reference metadata. Debug/Release are build configurations; the only explicit runtime externalization is a process environment connection-string override. All references are relative to `eShopLegacyNTier`; sensitive connection values are masked.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Service host | Runtime XML | `src\eShopWCFService\Web.config:3-64` | System.Web, WCF, EF and named connection. |
| Debug transform | Build-time XDT | `src\eShopWCFService\Web.Debug.config:5-30` | No active override; sample replacements are comments, not configuration. |
| Release transform | Build-time XDT | `src\eShopWCFService\Web.Release.config:17-18` | Removes compilation debug attribute on transformation; does not replace connection. |
| Desktop client | Runtime XML | `src\eShopWinForms\App.config:6-30` | CLR target and default WCF binding. High-DPI options are commented out. |
| Process environment | Runtime accessor | `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:10-18` | Reads ConnectionString, otherwise named EntityModel. |
| Project build / publish settings | MSBuild | `src\eShopWCFService\eShopWCFService.csproj:1-43,108-129`; `src\eShopWinForms\eShopWinForms.csproj:2-49` | Framework targets, build flags, IIS Express and desktop publish defaults. |
| Dependency manifests | NuGet XML | `src\eShopWCFService\packages.config:1-4`; `src\eShopWinForms\packages.config:1-6` | Exact package versions. |
| Desktop settings | Settings XML and generated accessor | `src\eShopWinForms\Properties\Settings.settings:2-6` | Default profile exists but Settings element is empty; not a populated runtime profile. |
| Desktop manifest | Windows execution manifest | `src\eShopWinForms\app.manifest:19,43` | asInvoker, uiAccess false; Windows 10 compatibility declaration. |
| Service-reference generation | XML metadata / WSDL / XSD | `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.svcmap:3-35` | Generation settings and metadata source; design-time artifacts, not another runtime config server. |
| Publish profile reference | MSBuild None item | `src\eShopWCFService\eShopWCFService.csproj:100` | Refers to Properties\PublishProfiles\FolderProfile.pubxml, but that file is absent in the scoped source tree. No publish destination inferred. |

No scoped `.env`, appsettings, container/orchestrator configuration, external configuration repository, Key Vault/Vault reference or remote configuration client was found. Other repository folders were not assessed.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug / AnyCPU | Default if MSBuild Configuration/Platform unset; or explicit `/p:Configuration=Debug` | Full debug symbols, no optimization, DEBUG and TRACE, warning level 4, ErrorReport prompt | Same references as Release; classic CSharp targets; service adds WebApplication targets. |
| Release / AnyCPU | Explicit `/p:Configuration=Release` | pdbonly symbols, optimization, TRACE, warning level 4, ErrorReport prompt | Same references; service Release XDT removes host compilation debug setting when transformed. |

Evidence: `src\eShopWCFService\eShopWCFService.csproj:4-5,27-42,112-114`; `src\eShopWinForms\eShopWinForms.csproj:5-6,31-48,280`; `src\eShopWCFService\Web.Release.config:17-18`. Service outputs both configurations to `bin\`; desktop uses `bin\Debug\` / `bin\Release\`. No dependency conditionals are declared.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default service | IIS-hosted process reads deployed Web.config | `src\eShopWCFService\Web.config` | Local named connection and host defaults. |
| Service external connection | Set process environment ConnectionString before context construction | Accessor `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:16-18` | Replaces named connection resolution entirely; no named environment/profile. |
| Desktop default | Launch executable with deployed application configuration | `src\eShopWinForms\App.config:6-30` | Fixed WCF client endpoint/binding. |

No ASPNETCORE_ENVIRONMENT, environment-specific runtime config files or multi-profile composition is implemented. Debug/Release XDT files are deployment/build transformations, not runtime-selectable environments.

## Properties Inventory

### WCF service

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| appSettings/aspnet:UseTaskFriendlySynchronizationContext | true (boolean) | Base; no override | `src\eShopWCFService\Web.config:7-8` |
| system.web/compilation debug | true (boolean) | Removed by Release XDT when applied | `src\eShopWCFService\Web.config:19`; `Web.Release.config:18` |
| system.web/compilation targetFramework | 4.6.1 (framework identifier) | Base | `src\eShopWCFService\Web.config:19` |
| system.web/httpRuntime targetFramework | 4.6.1 | Base | `src\eShopWCFService\Web.config:20` |
| serviceMetadata httpGetEnabled / httpsGetEnabled | true / true | Base | `src\eShopWCFService\Web.config:27` |
| serviceDebug includeExceptionDetailInFaults | false | Base | `src\eShopWCFService\Web.config:29` |
| protocolMapping https binding | basicHttpsBinding | Base | `src\eShopWCFService\Web.config:33-34` |
| serviceHostingEnvironment aspNetCompatibilityEnabled / multipleSiteBindingsEnabled | true / true | Base | `src\eShopWCFService\Web.config:36` |
| service name / business endpoint binding / bindingConfiguration | eShopWCFService.CatalogService / basicHttpBinding / empty string | Base | `src\eShopWCFService\Web.config:38-39` |
| metadata endpoint binding / contract | mexHttpBinding / IMetadataExchange | Base | `src\eShopWCFService\Web.config:40` |
| modules runAllManagedModulesForAllRequests | true | Base | `src\eShopWCFService\Web.config:45` |
| directoryBrowse enabled | true | Base | `src\eShopWCFService\Web.config:50` |
| entityFramework section type / requirePermission | EF section assembly version 6.0.0.0 / false | Base | `src\eShopWCFService\Web.config:5` |
| entityFramework/defaultConnectionFactory | System.Data.Entity.Infrastructure.LocalDbConnectionFactory | Base | `src\eShopWCFService\Web.config:53` |
| defaultConnectionFactory parameter | mssqllocaldb (instance name) | Base | `src\eShopWCFService\Web.config:55` |
| providers invariantName / type | System.Data.SqlClient / System.Data.Entity.SqlServer.SqlProviderServices | Base | `src\eShopWCFService\Web.config:59` |
| connectionStrings/EntityModel connectionString | [MASKED]; non-secret target is LocalDB, database eShopDatabase; Persist Security Info=True | Replaced by environment value if set | `src\eShopWCFService\Web.config:63` |
| connectionStrings/EntityModel providerName | System.Data.SqlClient | Base | `src\eShopWCFService\Web.config:63` |
| ConnectionString environment variable | Unset by checked-in files; arbitrary string | Process environment override | `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:16-18` |
| configConnectionName fallback | EntityModel; resolved as name=EntityModel | Only if environment variable is null | `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:10,17` |

Booleans are true/false; targetFramework/provider/binding names are framework identifiers. No application-enforced numeric range is defined for these entries. An empty environment string is not treated as absent because resolution uses null coalescing. No placeholder expansion or nested remote-provider resolution is implemented.

### Desktop and build/publish settings

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| startup/supportedRuntime version / sku | v4.0 / .NETFramework,Version=v4.7 | Desktop default | `src\eShopWinForms\App.config:6-7` |
| basicHttpBinding name | BasicHttpBinding_ICatalogService; no custom binding attributes | Desktop default | `src\eShopWinForms\App.config:21-22` |
| client binding / bindingConfiguration / contract / name | basicHttpBinding / BasicHttpBinding_ICatalogService / eShopServiceReference.ICatalogService / BasicHttpBinding_ICatalogService | Desktop default | `src\eShopWinForms\App.config:27-28` |
| client endpoint address | Local-development HTTP destination, port 62314; route listed only in API contract document | Desktop default; no environment substitution | `src\eShopWinForms\App.config:26` |
| Properties.Settings values | None | Default | `src\eShopWinForms\Properties\Settings.settings:2-6` |
| AutoGenerateBindingRedirects | true in both projects | All builds | `src\eShopWCFService\eShopWCFService.csproj:17`; `src\eShopWinForms\eShopWinForms.csproj:13` |
| WcfConfigValidationEnabled / UseIISExpress | True / true | Service builds | `src\eShopWCFService\eShopWCFService.csproj:16,18` |
| IISExpressSSLPort / anonymous and Windows authentication / classic pipeline / global host file | Empty/unset, not explicit false or enabled | Local service tooling | `src\eShopWCFService\eShopWCFService.csproj:19-24` |
| UseIIS / AutoAssignPort / DevelopmentServerPort / DevelopmentServerVPath | True / True / 62314 / root | Local service tooling | `src\eShopWCFService\eShopWCFService.csproj:118-123` |
| PublishUrl / Install / InstallFrom | publish\ / true / Disk | Desktop publishing | `src\eShopWinForms\eShopWinForms.csproj:15-17` |
| UpdateEnabled / UpdateMode / UpdateInterval / UpdateIntervalUnits | false / Foreground / 7 / Days | Desktop publishing | `src\eShopWinForms\eShopWinForms.csproj:18-21` |
| UpdatePeriodically / UpdateRequired / MapFileExtensions | false / false / true | Desktop publishing | `src\eShopWinForms\eShopWinForms.csproj:22-24` |
| ApplicationRevision / ApplicationVersion | 0 / 1.0.0.%2a | Desktop publishing | `src\eShopWinForms\eShopWinForms.csproj:25-26` |
| IsWebBootstrapper / UseApplicationTrust / BootstrapperEnabled | false / false / true | Desktop publishing | `src\eShopWinForms\eShopWinForms.csproj:27-29` |
| requestedExecutionLevel / uiAccess | asInvoker / false | Desktop manifest | `src\eShopWinForms\app.manifest:19` |
| GenerateTaskBasedAsynchronousMethod / GenerateAsynchronousMethods | true / false | Proxy generation only | `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.svcmap:4-5` |
| EnableDataBinding / GenerateSerializableTypes / Serializer / UseSerializerForFaults / ReferenceAllAssemblies | true / true / Auto / true / true | Proxy generation only | `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.svcmap:6-16` |
| ImportXmlTypes / GenerateInternalTypes / GenerateMessageContracts | false / false / false | Proxy generation only | `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.svcmap:8-10` |

No active high-DPI switches are set (`src\eShopWinForms\App.config:9-18`). Metadata generation maps/collections are empty (`Reference.svcmap:7,11-12,17-19`). Framework and MSBuild targets are listed below; compile flags are captured in Build Profiles. Business endpoint routes are intentionally not duplicated here.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| CatalogService | IIS Express enabled; .NET Framework 4.6.1 target; ConnectionString may be injected into host process | Not specified | Not specified |
| eShopWinForms | STAThread entry point; CLR v4 / .NET Framework 4.7; Windows execution asInvoker | Not specified | Not specified |

Evidence: `src\eShopWCFService\eShopWCFService.csproj:15-24`; `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:16`; `src\eShopWinForms\Program.cs:16-20`; `src\eShopWinForms\App.config:7`; `src\eShopWinForms\app.manifest:19`. No JVM, container environment declarations, CPU quotas, autoscaling rules or explicit process startup arguments are supplied. AnyCPU is a compilation target, not a CPU allocation.

## Startup Dependency Chain

1. SQL Server/LocalDB must be accessible to the WCF host. Context registers create-if-absent initialization; seed data is applied when EF initializes the missing database (`src\eShopWCFService\EntityModel.cs:12-16`; `src\eShopWCFService\Models\Infrastructure\CatalogDBInitializer.cs:11-20`).
2. IIS activates CatalogService and its default constructor creates the context (`src\eShopWCFService\CatalogService.svc:1`; `src\eShopWCFService\CatalogService.svc.cs:18-20`).
3. Desktop constructs proxy/controller and performs initial remote reads before showing the window (`src\eShopWinForms\Program.cs:22-27`; `src\eShopWinForms\Controllers\CatalogController.cs:150-157`).

This is a logical dependency order, not an orchestration script. No explicit wait loop, startup timeout, readiness probe or health-check mechanism is present. Availability of IIS, SQL and image assets is not verified by a deployment definition.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| Web.config connectionStrings/EntityModel | Sensitive connection configuration; no username/password literal in checked-in default | [MASKED] in repository XML; `src\eShopWCFService\Web.config:63` |
| Process environment ConnectionString | Potential credential-bearing database connection | [MASKED]; provisioned externally, value absent from source; `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:16-18` |

No API key, credential vault URI, encrypted configuration section, DPAPI provisioning, managed identity or service-principal workflow is declared in scope.

### Secrets Provisioning Workflow

Checked-in fallback XML or externally set process environment → `CatalogConfiguration.ConnectionString` → EF context constructor. Deployment must provide host/database access; no scoped pipeline specifies the identity, access policy, secret creation, rotation or injection procedure. Do not infer cloud managed identity or production database credentials from this sample. The desktop receives only its service destination, not a declared database secret.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| No business feature flags detected | Not applicable | No feature-management provider or application toggle branch is declared in the scoped configuration. |

Debug compilation, metadata exposure and desktop update switches are operational settings, not business feature flags. The mock service is not selected by a configuration toggle.

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Service target .NET Framework | 4.6.1 | `src\eShopWCFService\eShopWCFService.csproj:15`; `Web.config:19-20` |
| Desktop target .NET Framework | 4.7, CLR v4 | `src\eShopWinForms\eShopWinForms.csproj:11`; `App.config:7` |
| EntityFramework | 6.1.3 package; 6.0.0.0 assembly identity | `src\eShopWCFService\packages.config:3`; service project `:45-49` |
| Microsoft.AspNet.WebApi.Client | 5.2.3 | `src\eShopWinForms\packages.config:4` |
| Newtonsoft.Json | 6.0.4 | `src\eShopWinForms\packages.config:5` |
| WCF / WinForms | Framework-provided; no separate package pin | `src\eShopWCFService\eShopWCFService.csproj:64-68`; `src\eShopWinForms\eShopWinForms.csproj:77,85` |
| MSBuild project ToolsVersion | 15.0 declared; installed version not determined | Both csproj opening Project element |
| Visual Studio solution | Format 12.00; VisualStudioVersion 15.0.26730.16 | `eShopLegacyNTier.sln:2-5` |
| SQL Server LocalDB | mssqllocaldb instance name; engine version not pinned | `src\eShopWCFService\Web.config:53-55` |
| Container base image | None declared in assessed workspace | No scoped Dockerfile or container definition present |

No runtime host, restored package graph or publish transform execution was tested. Missing referenced publish profile and deployment-owned IIS/database settings limit production conclusions.
