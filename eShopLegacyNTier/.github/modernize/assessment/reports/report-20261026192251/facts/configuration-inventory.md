# Configuration & Externalized Settings Inventory

The scoped project contains two active application configuration files, two web transforms, desktop settings/manifest metadata, and build/service-reference configuration. One environment-based database override is observed; no secret-store workflow is established.

All citations are relative to `eShopLegacyNTier`; defaults below are checked-in values, not verified deployed settings. Credential-bearing connection values are masked.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Server base config | XML | `src\eShopWCFService\Web.config:3-64` | Framework, hosting, WCF, EF provider and named connection. |
| Debug / Release transforms | XDT XML | `src\eShopWCFService\Web.Debug.config:5-30`; `src\eShopWCFService\Web.Release.config:5-31` | Debug contains examples only; Release removes compilation debug attribute. |
| Desktop config | XML | `src\eShopWinForms\App.config:6-30` | CLR target and WCF client configuration. |
| Environment | Process environment | `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:10-18` | `ConnectionString` overrides named connection fallback. |
| Build / packaging | MSBuild / solution | `eShopLegacyNTier.sln:11-23`; `src\eShopWinForms\eShopWinForms.csproj:4-48`; `src\eShopWCFService\eShopWCFService.csproj:3-42` | Targets, Debug/Release, IIS Express and publishing defaults. |
| Desktop settings | Settings XML | `src\eShopWinForms\Properties\Settings.settings:2-6` | Default profile exists but settings collection is empty. |
| Desktop manifest | Windows XML manifest | `src\eShopWinForms\app.manifest:19,43` | asInvoker, uiAccess false; Windows 10 compatibility declaration. |
| Service reference | WCF generation metadata | `src\eShopWinForms\eShopWinForms.csproj:258-277` | Generated proxy and cached metadata; not a separate runtime environment. |
| Package declarations | NuGet packages.config | `src\eShopWCFService\packages.config:3`; `src\eShopWinForms\packages.config:3-5` | Fixed versions; see `dependency-map.md`. |

No Docker/Kubernetes, external configuration repository, Key Vault/Vault, appsettings profile files or cloud provisioning manifest was found within the assessed subtree. A folder publish profile is referenced by the server project, but the referenced file is not present in the scoped file inventory (`src\eShopWCFService\eShopWCFService.csproj:100`).

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug / AnyCPU | Default if Configuration omitted; select through MSBuild/IDE | Symbols/full debug; no optimization; DEBUG and TRACE | Same declared packages; desktop `bin\Debug\`, server `bin\` (`src\eShopWinForms\eShopWinForms.csproj:5-6,31-39`; `src\eShopWCFService\eShopWCFService.csproj:4-5,27-34`). |
| Release / AnyCPU | Explicit Configuration=Release / solution selection | Optimization; pdbonly; TRACE | Same packages; desktop `bin\Release\`, server `bin\`; web publish transform removes debug (`src\eShopWinForms\eShopWinForms.csproj:41-48`; `src\eShopWCFService\eShopWCFService.csproj:36-42`; `src\eShopWCFService\Web.Release.config:18`). |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Base server / desktop | Normal launch/hosting | Web.config / App.config | Checked-in defaults; no named runtime environments detected. |
| Server connection override | Host sets process `ConnectionString` | Environment read in CatalogConfiguration | Non-null environment value wins over `name=EntityModel`; empty string is not rejected (`src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:16-17`). |
| Published Release configuration | Publish transform, not runtime profile selection | Web.Release.config | Only compilation debug attribute removed; no active DB override (`src\eShopWCFService\Web.Release.config:6-18`). |

## Properties Inventory

### Server

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `appSettings/aspnet:UseTaskFriendlySynchronizationContext` | Boolean `true` | Base | `src\eShopWCFService\Web.config:7-9` |
| `system.web/compilation` | `debug=true`, `targetFramework=4.6.1` | Release removes debug | `src\eShopWCFService\Web.config:19`; `src\eShopWCFService\Web.Release.config:18` |
| `system.web/httpRuntime/targetFramework` | `4.6.1` | Base | `src\eShopWCFService\Web.config:20` |
| `serviceMetadata` | `httpGetEnabled=true`, `httpsGetEnabled=true` | Base | `src\eShopWCFService\Web.config:27` |
| `serviceDebug/includeExceptionDetailInFaults` | Boolean `false` | Base | `src\eShopWCFService\Web.config:29` |
| `protocolMapping` | `scheme=https`, `binding=basicHttpsBinding` | Base | `src\eShopWCFService\Web.config:33-35` |
| `serviceHostingEnvironment` | `aspNetCompatibilityEnabled=true`, `multipleSiteBindingsEnabled=true` | Base | `src\eShopWCFService\Web.config:36` |
| Service endpoint binding / contract | `basicHttpBinding` / `eShopWCFService.ICatalogService`; default binding configuration | Base | `src\eShopWCFService\Web.config:38-40`; paths documented in `api-service-contracts.md` |
| Metadata binding / contract | `mexHttpBinding` / `IMetadataExchange` | Base | `src\eShopWCFService\Web.config:40` |
| `system.webServer/modules/runAllManagedModulesForAllRequests` | Boolean `true` | Base | `src\eShopWCFService\Web.config:45` |
| `system.webServer/directoryBrowse/enabled` | Boolean `true` | Base | `src\eShopWCFService\Web.config:50` |
| `entityFramework/defaultConnectionFactory` | `LocalDbConnectionFactory`; parameter `mssqllocaldb` | Base | `src\eShopWCFService\Web.config:53-57` |
| `entityFramework/providers` | `System.Data.SqlClient` / EF SQL Server provider | Base | `src\eShopWCFService\Web.config:58-60` |
| `connectionStrings/EntityModel` | `Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=eShopDatabase;Persist Security Info=True;` (no password/user ID present) | Named fallback | `src\eShopWCFService\Web.config:63` |
| Environment `ConnectionString` | Unset; value `[MASKED]` when provided | Host override | `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:16-17` |
| `configConnectionName` | String `EntityModel`; resolves to `name=EntityModel` | Code fallback | `src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:10-17` |

The checked-in connection does not explicitly specify Integrated Security or credentials. Authentication success and effective deployment identity cannot be inferred.

### Desktop and build-host metadata

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `startup/supportedRuntime` | CLR `v4.0`, SKU `.NETFramework,Version=v4.7` | Base | `src\eShopWinForms\App.config:6-8` |
| Client binding configuration | `BasicHttpBinding_ICatalogService`, empty binding definition | Base | `src\eShopWinForms\App.config:19-28` |
| Client address / contract | Local development HTTP address; contract `eShopServiceReference.ICatalogService` | Base | `src\eShopWinForms\App.config:26-28`; exact address in `api-service-contracts.md` |
| Windows Forms DPI switches | None active; examples commented out | Base | `src\eShopWinForms\App.config:9-18` |
| IIS Express settings | `UseIISExpress=true`; SSL/authentication fields empty; development port `62314`; NTLM false | Development build | `src\eShopWCFService\eShopWCFService.csproj:18-24,119-128` |
| Desktop publish | `PublishUrl=publish\`; Install true; InstallFrom Disk; updates disabled; version `1.0.0.*`, revision 0 | Packaging | `src\eShopWinForms\eShopWinForms.csproj:15-29` |
| Desktop update metadata | Foreground, interval 7 Days; periodic/required false; file-extension mapping true; web bootstrapper false; application trust false; bootstrapper true | Packaging, update disabled | `src\eShopWinForms\eShopWinForms.csproj:18-29` |
| Manifest / settings | asInvoker; uiAccess false; no settings values | Base | `src\eShopWinForms\app.manifest:19`; `src\eShopWinForms\Properties\Settings.settings:2-6` |
| Image location | Current directory plus `..\..\Assets\Images\Catalog\` and stored filename; code-derived, not externalized | Desktop | `src\eShopWinForms\Views\CatalogView.cs:55-58` |

No explicit custom binding timeouts, quotas, pooling limits, environment interpolation chain or remote property provider is declared. Commented template examples are not active configuration.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| CatalogService | .NET Framework 4.6.1; IIS/IIS Express-hosted; process environment may override DB connection | No explicit memory/CPU limits observed | Unknown; no scaling configuration found |
| eShopWinForms | .NET Framework 4.7; STAThread; Windows desktop process; asInvoker | No explicit memory/CPU limits observed | One process per invocation; deployment count unknown |

Evidence: `src\eShopWCFService\eShopWCFService.csproj:15-24`; `src\eShopWinForms\App.config:6-8`; `src\eShopWinForms\Program.cs:16-27`; `src\eShopWinForms\app.manifest:19`. JVM parameters do not apply.

## Startup Dependency Chain

1. Hosting environment supplies optional `ConnectionString` before server context construction; otherwise the named config is used (`src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:12-18`).
2. CatalogService creates EntityModel and registers its initializer; database access requires the configured SQL instance and permissions. Initialization creates/seeds a missing database, not a separate readiness service (`src\eShopWCFService\CatalogService.svc.cs:18-20`; `src\eShopWCFService\EntityModel.cs:12-15`).
3. Desktop constructs proxy/controller and invokes `LoadView` **before** showing the dialog; initial service/data failures therefore affect startup (`src\eShopWinForms\Program.cs:22-27`; `src\eShopWinForms\Controllers\CatalogController.cs:150-157`).
4. Local image assets must resolve for rendering (`src\eShopWinForms\Views\CatalogView.cs:55-59`).

No explicit wait-for-port, readiness probe, startup retry, or custom startup timeout is observed. Default WCF timeouts are not explicitly configured.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| Process environment `ConnectionString` | Potentially credential-bearing connection string | `[MASKED]`; supplied externally, actual value unknown (`src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:16-17`). |
| `connectionStrings/EntityModel` | Connection metadata | Plaintext Web.config; local instance/database only, no explicit password/user ID in source (`src\eShopWCFService\Web.config:63`). |

No API keys/password literals or encrypted configuration provider were detected in the assessed configuration. This does not establish deployment secret hygiene.

### Secrets Provisioning Workflow

Observed resolution is **host process environment → CatalogConfiguration → EntityModel**, falling back to the named Web.config entry. No provisioning script, managed identity, service-principal flow, Key Vault binding, RBAC grant, or credential rotation process is evidenced within scope. The host operator and SQL login identity are unknown; the desktop has no configured service credentials (`src\eShopWinForms\App.config:19-29`).

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| No business feature flags detected | Not applicable | No flag provider or rollout configuration observed. |
| Desktop update enablement (packaging switch, not business flag) | `false` | MSBuild project (`src\eShopWinForms\eShopWinForms.csproj:18-23`). |

Debug, metadata exposure, directory browsing and DPI comments are technical settings, not evidence of a feature-management system.

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Desktop .NET Framework | 4.7 | `src\eShopWinForms\eShopWinForms.csproj:11`; `src\eShopWinForms\App.config:7` |
| Server .NET Framework | 4.6.1 | `src\eShopWCFService\eShopWCFService.csproj:15`; `src\eShopWCFService\Web.config:19-20` |
| EntityFramework | 6.1.3, assembly identity 6.0.0.0 | `src\eShopWCFService\packages.config:3`; `src\eShopWCFService\eShopWCFService.csproj:45-49` |
| Microsoft.AspNet.WebApi.Client | 5.2.3 | `src\eShopWinForms\packages.config:4` |
| Newtonsoft.Json | 6.0.4 | `src\eShopWinForms\packages.config:5` |
| MSBuild project ToolsVersion | 15.0 declared; installed tool version unknown | Both `.csproj` files, lines 1-2 |
| Visual Studio solution metadata | 15.0.26730.16 | `eShopLegacyNTier.sln:3-5` |
| SQL Server LocalDB / IIS Express | Version unknown | `src\eShopWCFService\Web.config:53-63`; `src\eShopWCFService\eShopWCFService.csproj:18` |
| Container base image | None found in scope | No Docker build declaration observed. |
