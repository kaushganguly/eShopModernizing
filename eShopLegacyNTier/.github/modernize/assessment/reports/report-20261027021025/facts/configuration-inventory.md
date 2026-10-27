# Configuration & Externalized Settings Inventory

The scoped workspace contains four application configuration files, two project files, two package manifests and an empty desktop settings definition. A single process environment variable can override the database connection; no secret-store integration is evidenced.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Service base | XML | `src\eShopWCFService\Web.config:1-65` | Runtime, WCF, EF and named connection |
| Service Debug | XDT XML | `src\eShopWCFService\Web.Debug.config:5-30` | Examples are comments; no effective override |
| Service Release | XDT XML | `src\eShopWCFService\Web.Release.config:17-18` | Removes compilation debug attribute |
| Desktop base | XML | `src\eShopWinForms\App.config:6-30` | Runtime and WCF client binding |
| Desktop settings | XML | `src\eShopWinForms\Properties\Settings.settings:2-6` | Default profile, empty settings collection |
| Build and packaging | MSBuild | `src\eShopWCFService\eShopWCFService.csproj:1-43,108-123`; `src\eShopWinForms\eShopWinForms.csproj:2-49` | Frameworks, Debug/Release, development host and desktop deployment metadata |
| Package versions | NuGet XML | `src\eShopWCFService\packages.config:3`; `src\eShopWinForms\packages.config:3-5` | Pinned package versions |
| Connection override | Process environment | `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:10-17` | Environment first, named config fallback |
| Service metadata | Generated metadata | `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.svcmap:14-22` | Generation settings and development metadata source, not a runtime config service |
| Local image location | Hard-coded source path | `src\eShopWinForms\Views\CatalogView.cs:55-59` | Relative to process working directory, not externally configured |

No scoped Docker/Kubernetes configuration, launchSettings, appsettings, external configuration repository or vault binding was found. The server project references `Properties\PublishProfiles\FolderProfile.pubxml`, but that file was absent in the scoped checkout (`src\eShopWCFService\eShopWCFService.csproj:100`).

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug / AnyCPU | Default configuration; selectable through MSBuild Configuration | Symbols full, optimization off, DEBUG and TRACE | Same package set; server bin output; desktop bin Debug (`src\eShopWCFService\eShopWCFService.csproj:4,27-34`; `src\eShopWinForms\eShopWinForms.csproj:5,31-39`) |
| Release / AnyCPU | Explicit Release configuration | pdbonly, optimization on, TRACE | Same package set; Release transform removes service compilation debug attribute (`src\eShopWCFService\eShopWCFService.csproj:36-42`; `src\eShopWinForms\eShopWinForms.csproj:41-48`; `src\eShopWCFService\Web.Release.config:18`) |

The service uses CSharp and Visual Studio WebApplication targets (`src\eShopWCFService\eShopWCFService.csproj:112-114`); desktop uses CSharp targets (`src\eShopWinForms\eShopWinForms.csproj:280`). Transform application depends on the publishing workflow; a Release build alone is not proof of deployed transformed settings.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Service base | Hosted WCF application configuration | Web.config | Default runtime settings and named connection |
| Service connection override | Set process environment variable before context creation | No separate file | Replaces named connection lookup |
| Desktop base | Executable configuration | App.config at build/deployment | Fixed development WCF target |

Evidence: `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:12-17`; `src\eShopWinForms\App.config:6-29`. Debug/Release are build configurations, not separate runtime environment profiles. No ASPNETCORE_ENVIRONMENT or equivalent profile-composition mechanism is evidenced.

## Properties Inventory

### Service

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| appSettings / aspnet:UseTaskFriendlySynchronizationContext | true, boolean | Base | `src\eShopWCFService\Web.config:8` |
| system.web / compilation.debug | true, boolean | Removed by Release publish transform | `src\eShopWCFService\Web.config:19`; `src\eShopWCFService\Web.Release.config:18` |
| compilation.targetFramework; httpRuntime.targetFramework | 4.6.1 | Base | `src\eShopWCFService\Web.config:19-20` |
| serviceMetadata.httpGetEnabled; httpsGetEnabled | true; true | Base | `src\eShopWCFService\Web.config:27` |
| serviceDebug.includeExceptionDetailInFaults | false | Base | `src\eShopWCFService\Web.config:29` |
| protocolMapping HTTPS binding | basicHttpsBinding | Base | `src\eShopWCFService\Web.config:33-35` |
| serviceHostingEnvironment.aspNetCompatibilityEnabled; multipleSiteBindingsEnabled | true; true | Base | `src\eShopWCFService\Web.config:36` |
| service endpoint binding; bindingConfiguration | basicHttpBinding; empty | Base | `src\eShopWCFService\Web.config:38-40` |
| metadata exchange binding | mexHttpBinding | Base | `src\eShopWCFService\Web.config:40` |
| modules.runAllManagedModulesForAllRequests | true | Base | `src\eShopWCFService\Web.config:45` |
| directoryBrowse.enabled | true | Base | `src\eShopWCFService\Web.config:50` |
| entityFramework.defaultConnectionFactory | LocalDbConnectionFactory; parameter mssqllocaldb | Base | `src\eShopWCFService\Web.config:53-56` |
| entityFramework provider | System.Data.SqlClient / EF SQL Server provider | Base | `src\eShopWCFService\Web.config:59` |
| connectionStrings / EntityModel | [MASKED connection string]; provider System.Data.SqlClient | Base | `src\eShopWCFService\Web.config:63` |
| ConnectionString | Unset by default; string supplied by deployment | Environment override | `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:16-17` |
| configConnectionName | EntityModel, internal fallback name | Base | `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:10` |
| ServiceHost.Debug | true | Host directive | `src\eShopWCFService\CatalogService.svc:1` |

Resolution is process `ConnectionString` value if non-null, otherwise `name=EntityModel`. An empty environment string is not treated as absent by the null-coalescing expression (`src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:16-17`).

### Desktop and build/deployment metadata

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| supportedRuntime.version; sku | v4.0; .NETFramework,Version=v4.7 | Desktop base | `src\eShopWinForms\App.config:6-7` |
| basicHttpBinding name | BasicHttpBinding_ICatalogService; no custom binding attributes | Desktop base | `src\eShopWinForms\App.config:21-22` |
| client.endpoint.address | Development localhost HTTP address, port 62314; route in API document | Desktop base | `src\eShopWinForms\App.config:26` |
| client binding / bindingConfiguration / name | basicHttpBinding / BasicHttpBinding_ICatalogService / BasicHttpBinding_ICatalogService | Desktop base | `src\eShopWinForms\App.config:27-28` |
| client contract | eShopServiceReference.ICatalogService | Desktop base | `src\eShopWinForms\App.config:28` |
| Windows Forms DPI compatibility switches | No active entries; examples commented out | Desktop base | `src\eShopWinForms\App.config:9-18` |
| PublishUrl; Install; InstallFrom | publish directory; true; Disk | Build metadata | `src\eShopWinForms\eShopWinForms.csproj:15-17` |
| UpdateEnabled; UpdateMode; UpdateInterval / Units | false; Foreground; 7 / Days | Build metadata | `src\eShopWinForms\eShopWinForms.csproj:18-21` |
| UpdatePeriodically; UpdateRequired | false; false | Build metadata | `src\eShopWinForms\eShopWinForms.csproj:22-23` |
| MapFileExtensions; ApplicationRevision; ApplicationVersion | true; 0; 1.0.0.* | Build metadata | `src\eShopWinForms\eShopWinForms.csproj:24-26` |
| IsWebBootstrapper; UseApplicationTrust; BootstrapperEnabled | false; false; true | Build metadata | `src\eShopWinForms\eShopWinForms.csproj:27-29` |
| AutoGenerateBindingRedirects | true | Build metadata | `src\eShopWinForms\eShopWinForms.csproj:13` |
| UseIISExpress; DevelopmentServerPort | true; 62314 | Development host metadata | `src\eShopWCFService\eShopWCFService.csproj:18,121` |

No application setting values are defined in the desktop Settings collection (`src\eShopWinForms\Properties\Settings.settings:6`). Package versions are cataloged in `dependency-map.md`; compiler profile values are above.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| Desktop | CLR v4, .NET Framework 4.7; STA entry point | Not declared | Not declared |
| WCF service | .NET Framework 4.6.1; IIS/IIS Express; optional process connection override | Not declared | Not declared |

Evidence: `src\eShopWinForms\App.config:7`; `src\eShopWinForms\Program.cs:16-17`; `src\eShopWCFService\eShopWCFService.csproj:15,18`; `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:16`. CPU, scaling limits, container resource allocations and explicit transport timeouts are not declared within scope. No JVM is involved.

## Startup Dependency Chain

1. SQL Server connection must be reachable and permit required access; context registers create-if-not-exists initialization and seeding (`src\eShopWCFService\EntityModel.cs:12-15`; `src\eShopWCFService\Models\Infrastructure\CatalogDBInitializer.cs:11-19`). EF initialization is triggered by database use, not a proven separate startup job.
2. IIS-hosted WCF application must be reachable before desktop initial service calls (`src\eShopWCFService\CatalogService.svc:1`; `src\eShopWinForms\Program.cs:23-27`).
3. Desktop calls LoadView before showing its dialog; local catalog image files must be present for rendering (`src\eShopWinForms\Program.cs:26-27`; `src\eShopWinForms\Views\CatalogView.cs:55-59`).

No automated dependency wait, readiness probe, startup retry loop or startup timeout override was found. WCF metadata publication is not a database readiness guarantee.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| connectionStrings / EntityModel | Database connection material | XML configuration, [MASKED] (`src\eShopWCFService\Web.config:63`) |
| Process environment / ConnectionString | Optional complete database connection override | Process environment, [MASKED if supplied] (`src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:16-17`) |

Connection values and database identifiers are intentionally withheld regardless of whether they contain credentials. No encrypted configuration section, vault reference, managed identity or secret-provider SDK binding is evidenced.

### Secrets Provisioning Workflow

The only evidenced flow is deployment/operator supplies a process environment connection value, or leaves the base named XML connection in place; `CatalogConfiguration` selects it and `EntityModel` consumes it (`src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:12-17`; `src\eShopWCFService\EntityModel.cs:12-15`). How deployment authenticates, provisions credentials, grants DB permissions or rotates secrets is not defined in this workspace. Do not infer a production identity/access model from the development connection.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| No business feature flags detected | — | No feature-management library or active business toggle evidenced |
| Desktop update enablement, operational only | false | Project packaging metadata (`src\eShopWinForms\eShopWinForms.csproj:18`) |

Commented DPI settings and Debug/Release symbols are compatibility/build controls, not evidence of runtime business feature rollout.

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Desktop target / CLR | .NET Framework 4.7 / v4.0 | `src\eShopWinForms\eShopWinForms.csproj:11`; `src\eShopWinForms\App.config:7` |
| Server target | .NET Framework 4.6.1 | `src\eShopWCFService\eShopWCFService.csproj:15` |
| EF | 6.1.3 package | `src\eShopWCFService\packages.config:3` |
| Web API Client | 5.2.3 package | `src\eShopWinForms\packages.config:4` |
| Newtonsoft.Json | 6.0.4 package | `src\eShopWinForms\packages.config:5` |
| MSBuild project ToolsVersion | 15.0 declared | Both project files, first Project element |
| Visual Studio solution metadata | 15.0.26730.16 | `eShopLegacyNTier.sln:3-5` |
| WCF, Windows Forms, System.Web | Framework-provided | `src\eShopWCFService\eShopWCFService.csproj:64-68`; `src\eShopWinForms\eShopWinForms.csproj:77,85` |
| SQL Server / IIS installed versions | Not pinned | Default database/host references do not establish installed versions |
| Container image | None declared in scope | No scoped container definition found |

Limitations: static configuration review only; production overrides, installed runtimes and missing publish-profile contents were unavailable. No build, test or deployment execution was performed.
