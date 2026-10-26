# Configuration & Externalized Settings Inventory

The scoped configuration is XML/MSBuild plus Compose, Docker and generated proxy settings, with no named application-environment profile system identified. Connection strings and credential literals are intentionally replaced by `[REDACTED]`; the source includes plaintext deployment secrets.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Service config | XML | Absolute source [C1] | ASP.NET, WCF, EF, named connection |
| Debug / Release transforms | XDT XML | Absolute sources [C2] | Debug contains only example comments; Release removes compilation debug attribute |
| Client config | XML | Absolute source [C3] | Legacy runtime declaration, DPI, WCF and EF settings |
| Generated proxy | C# configuration | Absolute source [C4] | Actual default constructor uses programmatic binding/address |
| Proxy generator input | JSON | Absolute source [C5] | Regeneration metadata, not runtime environment config |
| Compose base / override | YAML | Absolute sources [C6] | Service wiring, environment, network, ports and SQL healthcheck |
| Alternate Compose files | YAML / MSBuild | Absolute sources [C7] | Existing `temp` tree is assessed as alternate material, not assumed active |
| Build / publish | MSBuild, Dockerfile | Absolute sources [C8–C10] | Targets, build modes, publish location, runtime image |
| Desktop settings | XML | Absolute source [C11] | Empty settings collection |
| External configuration / secret store | Not found | Scope only | No Key Vault, Vault, remote configuration server or managed identity binding identified |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug / Any CPU | Solution Debug selection; server defaults to Debug when unset | Server full debug symbols, no optimization, DEBUG and TRACE | Existing package graph unchanged [C8] |
| Release / Any CPU | Solution Release selection | Server optimization, pdbonly, TRACE; Release web transform removes debug | Existing package graph unchanged [C2, C8] |
| Active desktop SDK build | Solution-selected csproj | net6.0-windows, WinExe, WinForms and WPF, GenerateAssemblyInfo=false | Microsoft.NET.Sdk; no custom Debug/Release package conditions [C9] |
| Alternate desktop framework build | Explicitly choose `.fx.csproj`; not selected by solution | .NET Framework 4.7.1, AnyCPU, DEBUG/TRACE vs TRACE | Older EF and WebApi.Client versions; see dependency-map.md [C9] |
| FolderProfile publish | Select profile explicitly | FileSystem; last-used Release / Any CPU | Output under MSBuild docker_publish_root plus obj\Docker\publish; retain existing files [C10] |
| Docker orchestration | Compose project / tools | Windows target, browser launch enabled | Microsoft.Docker.Sdk; root project version 2.1 [C10] |

Build configurations are not equivalent to runtime environment profiles.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Default/local service | Environment connection override absent | Web.config | Named EntityModel connection fallback |
| Compose deployment | Merge root base and override | Root Compose files | ConnectionString and SQL credentials injected as environment literals |
| Alternate deployment definitions | Explicit selection; activation not established | Existing temp Compose files | Similar ports/network/secret entries; older Compose version |
| Desktop | Start executable | Generated default proxy; legacy App.config also present | No environment-based endpoint selection found |

No appsettings environment files, ASPNETCORE_ENVIRONMENT use or profile composition mechanism was identified. The generated connection metadata says netcoreapp3.0 while the active project says net6.0-windows; this is generation history, not a second active runtime [C5, C9].

## Properties Inventory

### Service

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| ConnectionString environment variable | Absent; fallback to named EntityModel connection | Compose supplies `[REDACTED]` | [C6, C12] |
| connectionStrings / EntityModel | `[REDACTED]` | Default/local; no active XDT override | [C1] |
| aspnet:UseTaskFriendlySynchronizationContext | true | All | [C1] |
| system.web / compilation.debug | true | Removed by Release transform | [C1, C2] |
| compilation.targetFramework / httpRuntime.targetFramework | 4.6.1 / 4.6.1 | All | [C1] |
| serviceMetadata.httpGetEnabled / httpsGetEnabled | true / true | All | [C1] |
| serviceDebug.includeExceptionDetailInFaults | false | All | [C1] |
| protocolMapping HTTPS binding | basicHttpsBinding | All; does not prove an HTTPS listener | [C1] |
| service endpoint binding / bindingConfiguration | basicHttpBinding / empty | All | [C1] |
| metadata endpoint binding | mexHttpBinding | All | [C1] |
| serviceHostingEnvironment.aspNetCompatibilityEnabled / multipleSiteBindingsEnabled | true / true | All | [C1] |
| modules.runAllManagedModulesForAllRequests | true | All | [C1] |
| directoryBrowse.enabled | true | All | [C1] |
| EF defaultConnectionFactory / parameter | LocalDbConnectionFactory / mssqllocaldb | Default | [C1] |
| EF provider invariant / implementation | System.Data.SqlClient / EF SqlServer provider | All | [C1] |

### Desktop and proxy generation

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| supportedRuntime version / sku | v4.0 / .NET Framework 4.7.1 | Legacy App.config; not authoritative active SDK target | [C3] |
| DpiAwareness | PerMonitorV2 | App.config | [C3] |
| client binding name / type | BasicHttpBinding_ICatalogService / basicHttpBinding | XML; same logical name as generated proxy | [C3] |
| client endpoint address | Local HTTP address, port 5113 | XML and generated code; generated constructor wins in active path | [C3, C4] |
| generated MaxBufferSize / MaxReceivedMessageSize | int.MaxValue / int.MaxValue | Programmatic binding | [C4] |
| generated ReaderQuotas / AllowCookies | XmlDictionaryReaderQuotas.Max / true | Programmatic binding | [C4] |
| EF defaultConnectionFactory / parameter / provider | LocalDbConnectionFactory / mssqllocaldb / System.Data.SqlClient | XML; no active client DB call found | [C3] |
| ConnectedService ProviderId / Version | Microsoft.VisualStudio.ConnectedService.Wcf / 15.0.30423.811 | Generation only | [C5] |
| collectionTypes | System.Array, System.Collections.Generic.Dictionary | Generation only | [C5] |
| namespaceMappings / sync / targetFramework / typeReuseMode | eShopWinForms.eShopServiceReference / true / netcoreapp3.0 / None | Generation only | [C5] |
| Properties.Settings settings | Empty | Default | [C11] |

### Containers and publication

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| eshopwcfservice.image / build context | eshop/wcfservice / scoped service directory | Root Compose | [C6] |
| sql.data.image | microsoft/mssql-server-windows-developer; untagged | Root and alternate Compose | [C6, C7] |
| SA_PASSWORD | `[REDACTED]` | SQL container | [C6, C7] |
| ACCEPT_EULA | Y | SQL container | [C6, C7] |
| service ports / SQL ports | 5113:80 / 5433:1433 | Compose overrides | [C6, C7] |
| default network external name | nat | Overrides | [C6, C7] |
| SQL healthcheck | sqlcmd select 1; password argument `[REDACTED]` | Root override: interval 1s, retries 20; alternate override: interval 1s, retries unspecified; alternate CI: interval 1s, retries 20 | [C6, C7] |
| Docker ARG source | obj/Docker/publish fallback | Image build | [C10] |
| publish method / provider / configuration / platform | FileSystem / FileSystem / Release / Any CPU | FolderProfile | [C10] |
| publishUrl | MSBuild docker_publish_root plus obj\Docker\publish | FolderProfile | [C10] |
| LaunchSiteAfterPublish / ExcludeApp_Data / DeleteExistingFiles | true / false / false | FolderProfile | [C10] |
| DockerTargetOS / DockerLaunchBrowser | Windows / true | Compose project | [C10] |

No explicit timeout, CPU, memory, scaling or application pool tuning values were found. MSBuild resolves publish variables at build time; effective values are unverified. The alternate Compose base uses a different relative build context and is not assumed equivalent to the solution-selected root base [C6, C7].

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopWinForms | Windows GUI, STA entry point; synchronous initial data load before modal display | Not specified | Not specified |
| eShopWCFService | IIS-hosted WCF on Windows image; ConnectionString environment injection | No explicit limit/reservation | No replicas specified |
| sql.data | Windows SQL Developer image; SA_PASSWORD and ACCEPT_EULA injected | No explicit limit/reservation | No replicas specified |

There are no JVM options. Dockerfile copies previously published binaries rather than compiling source; publication is a prerequisite [C10]. Windows containers and the external nat network are assumptions, not verified host capabilities.

## Startup Dependency Chain

1. Published service artifacts and external nat network must exist before image/container startup [C6, C10].
2. Compose starts `sql.data` before `eshopwcfservice` via short-form depends_on. The SQL container has a query-based healthcheck, but no service_healthy condition or explicit wait loop connects readiness to service startup [C6].
3. Catalog operations require SQL connectivity. First database access can initialize and seed a missing database; there is no explicit startup probe or application retry [C12].
4. The desktop constructs the proxy and performs initial requests before displaying the form; unavailable service/database can prevent initial loading. No degraded startup branch is implemented [C13].

No application health endpoint, readiness timeout or deployment orchestrator beyond Compose was identified. Effective IIS settings and platform health defaults are unknown.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| connectionStrings / EntityModel | Full connection string | Web.config `[REDACTED]` |
| ConnectionString | Full deployment connection string | Root and alternate Compose plaintext literal `[REDACTED]` |
| SA_PASSWORD | SQL administrative password | Root and alternate Compose plaintext literal `[REDACTED]` |
| SQL healthcheck password argument | Credential | Root and alternate Compose command argument `[REDACTED]` |

### Secrets Provisioning Workflow

Repository Compose literals → Docker environment → service Environment.GetEnvironmentVariable lookup → EF connection. A missing environment value falls back to the named XML connection [C12]. The SQL container separately consumes its password environment entry, while healthchecks embed a password argument [C6, C7]. No vault lookup, secret-file mount, encrypted configuration, managed identity, scoped deployment identity or automated secret rotation workflow was found in the assessed subtree. Configured deployment uses SQL administrative credentials; exact credential values are deliberately omitted. External provisioning and host-side access control are unverified.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| No active business feature flag found | Not applicable | No feature-management integration identified |
| useMockData | Constructor parameter; no effective switch | ApplicationModule registration body is commented out and not the Program composition path [C13] |

Debug compilation, metadata exposure and directory browsing are configuration switches, not business feature flags.

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Active desktop | net6.0-windows | [C9] |
| Alternate desktop | .NET Framework 4.7.1 | [C9] |
| WCF service target / XML runtime target | .NET Framework 4.6.1 | [C1, C8] |
| WCF Windows container base | 4.7.2 | [C10] |
| EF server / active client | 6.1.3 / 6.4.4 | [C8, C9] |
| WCF client packages | 4.8.0 | [C9] |
| WebApi.Client active / alternate client | 5.2.7 / 5.2.3 | [C9] |
| SQL Server image | Untagged; engine version unknown | [C6] |
| Compose schema root / alternate | 3.4 / 3 | [C6, C7] |
| Service MSBuild ToolsVersion | 15.0; installed version not assessed | [C8] |
| Solution Visual Studio format/version | Format 12.00, VS 15.0.26430.16; historical metadata | [C8] |
| Proxy generator | Svcutil 2.0.1-preview-30422-0661 | Generated attribute [C4] |
| .NET SDK | Unpinned in scoped project; installed version not assessed | [C9] |

**Evidence (all paths absolute):**

- [C1] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Web.config:3-64`.
- [C2] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Web.Debug.config:5-30`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Web.Release.config:5-31`.
- [C3] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\App.config:3-41`.
- [C4] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs:1-18,448-452,591-621` (generated).
- [C5] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Connected Services\eShopServiceReference\ConnectedService.json:1-22`.
- [C6] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\docker-compose.yml:1-13`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\docker-compose.override.yml:1-25`.
- [C7] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\temp\docker-compose.yml:1-14`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\temp\docker-compose.override.yml:1-21`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\temp\docker-compose.ci.build.yml:1-22`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\temp\docker-compose.dcproj:2-16`.
- [C8] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\eShopModernizedNTier.sln:2-29`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\eShopWCFService.csproj:1-70`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\packages.config:3`.
- [C9] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\eShopWinForms.csproj:1-34`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\eShopWinForms.fx.csproj:11,20-40,265-270`.
- [C10] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Dockerfile:1-5`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Properties\PublishProfiles\FolderProfile.pubxml:6-18`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\docker-compose.dcproj:2-16`.
- [C11] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Properties\Settings.settings:1-7`.
- [C12] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\Models\Infrastructure\CatalogConfiguration.cs:10-18`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWCFService\EntityModel.cs:12-15`.
- [C13] `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Program.cs:16-27`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Controllers\CatalogController.cs:150-157`; `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier\src\eShopWinForms\Modules\ApplicationModule.cs:8-34`.
