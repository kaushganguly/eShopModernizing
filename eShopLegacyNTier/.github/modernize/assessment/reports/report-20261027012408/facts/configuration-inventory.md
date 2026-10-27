# Configuration & Externalized Settings Inventory

The scope contains four runtime/XML transform files, two package manifests, two project files, a solution and desktop manifest/settings resources. Runtime customization consists chiefly of static XML settings and one environment-variable override; no external configuration or secret store integration is evidenced.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Server base settings | Runtime XML | `src\eShopWCFService\Web.config:1-65` | Hosting, WCF, EF provider and connection |
| Debug transform | Publish/build transform | `src\eShopWCFService\Web.Debug.config:5-30` | No active setting overrides; examples are comments |
| Release transform | Publish/build transform | `src\eShopWCFService\Web.Release.config:17-18` | Removes compilation debug attribute |
| Desktop base settings | Runtime XML | `src\eShopWinForms\App.config:6-30` | CLR target and WCF binding/client address |
| Connection override | Process environment | `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:10-18` | Reads ConnectionString; falls back to named EntityModel |
| Build and publish settings | MSBuild | Both project files | Classic project settings, Debug/Release, references and packaging |
| Solution settings | Visual Studio solution | `eShopLegacyNTier.sln:1-9` and configuration sections | Two projects; no test project listed |
| Package versions | NuGet XML | `src\eShopWCFService\packages.config:3`; `src\eShopWinForms\packages.config:3-5` | Fixed package versions |
| Desktop application settings | Generated settings source | `src\eShopWinForms\Properties\Settings.settings:2-6` | Default profile has no settings |
| Desktop manifest | Windows manifest | `src\eShopWinForms\app.manifest:3-19,42-43` | asInvoker; Windows 10 compatibility declaration |
| Generated service metadata | Design-time connected service | `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.svcmap:21-23` | Static metadata source for regeneration, not dynamic discovery |
| Desktop resources | Embedded assets | `src\eShopWinForms\eShopWinForms.csproj:104-115` | Resource files and form localization/assets, not environment property stores |
| Publish profile reference | Build input reference | `src\eShopWCFService\eShopWCFService.csproj:100` | FolderProfile.pubxml is referenced but absent from the scoped files |

No scoped Dockerfile, Compose, Kubernetes, Helm, IaC, appsettings, .env, config server or secret-store declaration was found. No external repository was assessed.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug / AnyCPU | Default when Configuration/Platform unset; or MSBuild properties | DEBUG and TRACE; full debug symbols; optimization off | Same package set as Release; server bin output, desktop bin\Debug |
| Release / AnyCPU | MSBuild Configuration=Release | TRACE only; optimization on; pdbonly | Same packages; desktop bin\Release; server bin; publish transform removes compilation debug attribute |

Evidence: `src\eShopWCFService\eShopWCFService.csproj:4-5,27-42`; `src\eShopWinForms\eShopWinForms.csproj:5-6,31-48`; `src\eShopWCFService\Web.Release.config:17-18`. Both configurations set ErrorReport=prompt and WarningLevel=4. Server uses Microsoft.CSharp.targets and WebApplication targets; desktop uses Microsoft.CSharp.targets (`src\eShopWCFService\eShopWCFService.csproj:108-114`; `src\eShopWinForms\eShopWinForms.csproj:280`). Transforms are build/publish settings, not named runtime environments.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default server | Normal IIS/WCF startup | Web.config | Base settings |
| Default desktop | Normal executable startup | App.config becomes executable config | Static CLR/client settings |
| Connection override, not a named profile | ConnectionString environment variable present | Environment plus CatalogConfiguration.cs | Replaces named connection lookup |

No ASPNETCORE_ENVIRONMENT, named Development/Production configurations, runtime profile composition or runtime config-server loading exists in scope. Debug/Release transforms do not introduce separate database settings; connection-string examples in those files are commented.

## Properties Inventory

### eShopWCFService

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| aspnet:UseTaskFriendlySynchronizationContext | true, Boolean | All | `src\eShopWCFService\Web.config:8` |
| system.web/compilation.debug | true, Boolean | Release publish removes attribute | `src\eShopWCFService\Web.config:19`; `src\eShopWCFService\Web.Release.config:18` |
| system.web/compilation.targetFramework | 4.6.1 | All | `src\eShopWCFService\Web.config:19` |
| system.web/httpRuntime.targetFramework | 4.6.1 | All | `src\eShopWCFService\Web.config:20` |
| serviceMetadata.httpGetEnabled / httpsGetEnabled | true / true | All | `src\eShopWCFService\Web.config:27` |
| serviceDebug.includeExceptionDetailInFaults | false | All | `src\eShopWCFService\Web.config:29` |
| protocolMapping scheme / binding | https / basicHttpsBinding | All | `src\eShopWCFService\Web.config:34` |
| serviceHostingEnvironment.aspNetCompatibilityEnabled | true | All | `src\eShopWCFService\Web.config:36` |
| serviceHostingEnvironment.multipleSiteBindingsEnabled | true | All | `src\eShopWCFService\Web.config:36` |
| services/service.name | eShopWCFService.CatalogService | All | `src\eShopWCFService\Web.config:38` |
| application endpoint binding / bindingConfiguration / contract | basicHttpBinding / empty / eShopWCFService.ICatalogService | All | `src\eShopWCFService\Web.config:39` |
| metadata endpoint binding / contract | mexHttpBinding / IMetadataExchange | All | `src\eShopWCFService\Web.config:40` |
| modules.runAllManagedModulesForAllRequests | true | All | `src\eShopWCFService\Web.config:45` |
| directoryBrowse.enabled | true | All | `src\eShopWCFService\Web.config:50` |
| entityFramework section type / requirePermission | EF section assembly version 6.0.0.0 / false | All | `src\eShopWCFService\Web.config:5` |
| defaultConnectionFactory.type | System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework | All | `src\eShopWCFService\Web.config:53` |
| defaultConnectionFactory parameter | mssqllocaldb | All | `src\eShopWCFService\Web.config:55` |
| provider.invariantName / provider.type | System.Data.SqlClient / System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer | All | `src\eShopWCFService\Web.config:59` |
| connectionStrings/EntityModel | Data Source=(localdb)\MSSQLLocalDB; Initial Catalog=eShopDatabase; Persist Security Info=True; | Default | `src\eShopWCFService\Web.config:63` |
| connectionStrings/EntityModel.providerName | System.Data.SqlClient | Default | `src\eShopWCFService\Web.config:63` |
| ConnectionString, environment string | Unset; fallback name=EntityModel | Environment override | `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:10-18` |
| ServiceHost Language / Debug / Service | C# / true / eShopWCFService.CatalogService | All; no directive transform found | `src\eShopWCFService\CatalogService.svc:1` |

Connection resolution is `ConnectionString environment value ?? "name=EntityModel"`, passed to DbContext; an empty-but-present environment string is not filtered out (`src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:16-17`; `src\eShopWCFService\EntityModel.cs:12-15`). No variable substitution syntax or nested secret-reference expansion is implemented. Endpoint addresses are deliberately inventoried in `api-service-contracts.md`, not repeated as route entries here.

### eShopWinForms

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| startup/supportedRuntime.version | v4.0 | All | `src\eShopWinForms\App.config:7` |
| startup/supportedRuntime.sku | .NETFramework,Version=v4.7 | All | `src\eShopWinForms\App.config:7` |
| basicHttpBinding/binding.name | BasicHttpBinding_ICatalogService; otherwise empty binding | All | `src\eShopWinForms\App.config:21-22` |
| client endpoint binding / bindingConfiguration | basicHttpBinding / BasicHttpBinding_ICatalogService | All | `src\eShopWinForms\App.config:26-28` |
| client endpoint contract / name | eShopServiceReference.ICatalogService / BasicHttpBinding_ICatalogService | All | `src\eShopWinForms\App.config:28` |
| Windows Forms DPI compatibility switches | No active values; examples commented | All | `src\eShopWinForms\App.config:9-18` |
| Settings profile | (Default), no settings | All | `src\eShopWinForms\Properties\Settings.settings:2-6` |
| requestedExecutionLevel / uiAccess | asInvoker / false | All | `src\eShopWinForms\app.manifest:19` |
| Manifest identity version / name | 1.0.0.0 / MyApplication.app | All | `src\eShopWinForms\app.manifest:3` |
| Windows compatibility | Windows 10 supportedOS entry | All | `src\eShopWinForms\app.manifest:42-43` |
| Catalog asset location, code constant | Current directory plus ..\..\Assets\Images\Catalog\ and filename | All | `src\eShopWinForms\Views\CatalogView.cs:55-58` |

Neither client nor server binding declares explicit timeout, security mode, reader quota, buffer-size or message-size overrides. Framework defaults are not treated as application-declared values.

### Build, packaging and host defaults

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| ToolsVersion | 15.0 | Both projects | Both project files, opening Project element |
| Server TargetFrameworkVersion / OutputType / AssemblyName / RootNamespace | v4.6.1 / Library / WcfService1 / WcfService1 | Both configurations | `src\eShopWCFService\eShopWCFService.csproj:11-15` |
| WcfConfigValidationEnabled / AutoGenerateBindingRedirects | true / true | Server | `src\eShopWCFService\eShopWCFService.csproj:16-17` |
| UseIISExpress / UseIIS / AutoAssignPort | true / true / true | Server development host | `src\eShopWCFService\eShopWCFService.csproj:18,119-120` |
| DevelopmentServerPort / NTLMAuthentication / UseCustomServer | 62314 / false / false | Server development host | `src\eShopWCFService\eShopWCFService.csproj:121-125` |
| DevelopmentServerVPath / SaveServerSettingsInUserFile | / / false | Server development host | `src\eShopWCFService\eShopWCFService.csproj:122,128` |
| IISExpressSSLPort, authentication flags, 64-bit flag, classic pipeline flag | Empty; no explicit values | Server development host | `src\eShopWCFService\eShopWCFService.csproj:19-24` |
| Desktop TargetFrameworkVersion / OutputType / AssemblyName / RootNamespace | v4.7 / WinExe / eShopWinForms / eShopWinForms | Both configurations | `src\eShopWinForms\eShopWinForms.csproj:8-11` |
| FileAlignment / AutoGenerateBindingRedirects | 512 / true | Desktop | `src\eShopWinForms\eShopWinForms.csproj:12-13` |
| PublishUrl / Install / InstallFrom | publish\ / true / Disk | Desktop | `src\eShopWinForms\eShopWinForms.csproj:15-17` |
| UpdateEnabled / UpdateMode | false / Foreground | Desktop | `src\eShopWinForms\eShopWinForms.csproj:18-19` |
| UpdateInterval / UpdateIntervalUnits | 7 / Days | Desktop | `src\eShopWinForms\eShopWinForms.csproj:20-21` |
| UpdatePeriodically / UpdateRequired | false / false | Desktop | `src\eShopWinForms\eShopWinForms.csproj:22-23` |
| MapFileExtensions / ApplicationRevision / ApplicationVersion | true / 0 / 1.0.0.%2a | Desktop | `src\eShopWinForms\eShopWinForms.csproj:24-26` |
| IsWebBootstrapper / UseApplicationTrust / BootstrapperEnabled | false / false / true | Desktop | `src\eShopWinForms\eShopWinForms.csproj:27-29` |
| ApplicationManifest | app.manifest | Desktop | `src\eShopWinForms\eShopWinForms.csproj:54` |
| Microsoft.Net.Framework.3.5.SP1 bootstrapper Install / Visible | false / false | Desktop | `src\eShopWinForms\eShopWinForms.csproj:270-274` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopWCFService | IIS/IIS Express hosted Framework library; optional ConnectionString process environment | No declared allocation or limit | No count or scaling configuration |
| eShopWinForms | CLR v4.0, Framework 4.7; STA entry point; Windows Forms visual styles | No declared allocation or limit | No count or scaling configuration |
| SQL Server | LocalDB default; engine startup/limits not declared | Not declared | Not declared |

Evidence: `src\eShopWCFService\eShopWCFService.csproj:11-18`; `src\eShopWinForms\App.config:6-8`; `src\eShopWinForms\Program.cs:16-20`. No JVM settings, container memory/CPU requests, cloud instance counts or horizontal scaling configuration apply or are declared.

## Startup Dependency Chain

1. SQL Server connection/database must be available to the service's EF data access. EntityModel registers create-if-not-exists initialization; initialization can occur on first EF access, not necessarily at host process start (`src\eShopWCFService\EntityModel.cs:12-16`; `src\eShopWCFService\Models\Infrastructure\CatalogDBInitializer.cs:11-20`).
2. IIS/WCF service must be reachable before the desktop's first data load. No dockerize, health probe, wait loop or startup timeout override is present (`src\eShopWCFService\CatalogService.svc:1`; `src\eShopWinForms\Program.cs:22-27`).
3. Program creates view, proxy and controller, invokes LoadView, then shows the dialog. LoadView performs remote discount/catalog/classification calls (`src\eShopWinForms\Controllers\CatalogController.cs:150-157`).
4. Local image files must exist when products render; there is no asset readiness probe or recovery wrapper (`src\eShopWinForms\Views\CatalogView.cs:55-59`).

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| ConnectionString environment variable | Potential credential-bearing database connection | Runtime value unknown; **[MASKED]** if supplied |
| Web.config connectionStrings/EntityModel | Database connection metadata | Checked-in local connection; no password/token in the declared value |
| WCF credentials | No explicit credential configuration | No secret reference found |

### Secrets Provisioning Workflow

The only evidenced path is **externally set process environment → CatalogConfiguration → EntityModel**. No repository-local provisioning script, managed identity, service principal, RBAC policy, Key Vault/Vault binding, DPAPI section encryption or rotation workflow is declared. The service needs database access, while the desktop configuration contains a static service address and no credentials. The local connection does not explicitly declare Integrated Security or a SQL login; successful authentication and host identity cannot be asserted from this source (`src\eShopWCFService\Web.config:63`; `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:16-17`).

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| No business feature flags detected | Not applicable | No feature-management provider or conditional feature activation found |
| Desktop update enablement, deployment switch only | UpdateEnabled=false | Project publish settings, `src\eShopWinForms\eShopWinForms.csproj:18-23` |
| Desktop DPI compatibility examples | Commented; inactive | App.config comments, `src\eShopWinForms\App.config:9-18` |

CatalogServiceMock is not a feature-flag-selectable fallback: Program selects the generated remote client directly (`src\eShopWinForms\Program.cs:23`).

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Desktop .NET Framework | 4.7; CLR v4.0 | `src\eShopWinForms\eShopWinForms.csproj:11`; `src\eShopWinForms\App.config:7` |
| Server .NET Framework / ASP.NET target | 4.6.1 | `src\eShopWCFService\eShopWCFService.csproj:15`; `src\eShopWCFService\Web.config:19-20` |
| EntityFramework | 6.1.3 | Both package manifests, line 3 |
| Microsoft.AspNet.WebApi.Client | 5.2.3 | `src\eShopWinForms\packages.config:4` |
| Newtonsoft.Json | 6.0.4 | `src\eShopWinForms\packages.config:5` |
| Classic project ToolsVersion | 15.0, declared; installed MSBuild not inspected | Both project files, opening Project element |
| Solution format / Visual Studio metadata | 12.00 / 15.0.26730.16 | `eShopLegacyNTier.sln:2-5` |
| C# language version | Not explicitly pinned | Project files |
| IIS, LocalDB engine, NuGet executable | Not pinned | No explicit version declarations found |
| Container base images | None | No scoped Dockerfile found |

Comments mentioning other framework versions or transform example credentials are not active settings. No tests, builds, linters or deployment verification were performed.
