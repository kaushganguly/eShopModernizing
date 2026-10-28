# Configuration & Externalized Settings Inventory

The scoped tree contains **11 runtime/development configuration files**, plus three project manifests, a solution and legacy package metadata. Configuration is checked-in XML/JSON with legacy build transforms and Core host defaults; no scoped external configuration repository, secret-store binding, or deployment secret-provisioning workflow was found.

Scope: `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution` only. All three projects are solution members; extra scoped projects: **none**. No repository-parent configuration or `.github\agents` content was inspected.

## Configuration Sources

All paths below are absolute. Configuration citations refer to the numbered evidence list.

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Legacy application | XML | `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\Web.config` | Connection, app settings, IIS modules/handlers, runtime redirects, compiler settings [C1] |
| Legacy Debug transform | XDT XML | `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\Web.Debug.config` | No active value changes; illustrative database/error settings are comments [C2] |
| Legacy Release transform | XDT XML | `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\Web.Release.config` | Removes compilation debug attribute; database examples are comments [C3] |
| Legacy views | XML | `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\Views\Web.config` | MVC/Razor legacy view configuration [C4] |
| Legacy telemetry | XML | `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\ApplicationInsights.config` | Collectors, initializers, sampling; no active instrumentation key [C5] |
| Legacy logging | XML | `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\log4Net.xml` | Rolling-file settings; AssemblyInfo requests `log4net.xml` [C6] |
| Ported base | JSON | `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\eShopPorted\appsettings.json` | Connection, logging and flags [C7] |
| Ported development | JSON | `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\eShopPorted\appsettings.Development.json` | Logging overrides only [C8] |
| Ported launch | JSON | `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\eShopPorted\Properties\launchSettings.json` | Development process environment and local bindings; not production deployment config [C9] |
| Ported runtime | XML | `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\eShopPorted\app.config` | Server GC enabled [C10] |
| Ported views | XML | `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\eShopPorted\Views\Web.config` | Retained System.Web MVC/Razor settings, not Core configuration [C11] |
| Build and dependency manifests | MSBuild/NuGet/solution | Three scoped `.csproj` files, solution, legacy `packages.config` | See framework versions and `dependency-map.md` [C12–C15] |
| Programmatic configuration | C# | Ported Program/Startup; legacy Global.asax and App_Start | Host defaults, DI, error handling, routing and bundles [C16], [C17] |

No scoped `.env`, container manifests, Kubernetes ConfigMap/Secret, Key Vault/App Configuration client registration, publish profiles, or independent configuration-server references were found.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug Any CPU | Solution configuration or MSBuild Configuration=Debug; legacy/library default | Debug symbols; DEBUG and TRACE; optimization off in legacy/library | Same packages; legacy full PDB; utility writes bin Debug |
| Release Any CPU | Solution selection or MSBuild Configuration=Release | TRACE; optimization on in legacy/library; legacy PDB-only | Same packages; legacy release publish transform removes debug |
| Ported SDK configurations | SDK defaults / solution configuration | Web SDK build targeting net461 | No project-specific Debug/Release overrides |
| Legacy optional view compilation | MSBuild MvcBuildViews=true; default false | AspNetCompiler after build | Old WebApplication targets; not an environment profile |

No package references are conditioned by configuration. Legacy uses ToolsVersion 15.0, installed Visual Studio web targets and optional TypeScript 3.0 target imports; the utility is old-style MSBuild, while the port uses `Microsoft.NET.Sdk.Web`. No scoped global.json pins an SDK. [C12–C15]

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Legacy default | IIS application configuration | Web.config | Database mode, customization off; InProc session |
| Legacy published Debug/Release | Publishing transforms, not runtime environment selection | Web.Debug.config / Web.Release.config | Release removes debug; no active environment database override |
| Ported Development | ASPNETCORE_ENVIRONMENT=Development in both launch profiles | Base plus appsettings.Development.json | Debug default logging; System/Microsoft Information; developer exception page |
| Ported nondevelopment | Host environment not Development | Base; optional environment file from host defaults, none other checked in | Exception-handler target configured in Startup; no explicit production JSON |
| Utility | None | None | Uses caller process runtime configuration |

Ported `WebHost.CreateDefaultBuilder(args)` supplies normal host configuration precedence: base JSON, environment JSON, development user secrets when applicable, environment variables, then command-line arguments. No UserSecretsId is declared and no explicit secret binding is visible. Environment overrides can use nested keys such as `ConnectionStrings__DefaultConnection`; these are supported by host defaults, **not repository-provisioned values**. Legacy directly reads ConfigurationManager and does not implement a custom environment-variable substitution layer. [C16], [C17]

## Properties Inventory

### Legacy application settings and XML runtime settings

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| connectionStrings / CatalogDBContext | `Data Source=(localdb)\MSSQLLocalDB; Initial Catalog=Microsoft.eShopOnContainers.Services.CatalogDb; Integrated Security=True; MultipleActiveResultSets=True` | No active scoped overrides | Web.config:11-13 [C1] |
| connectionStrings / providerName | System.Data.SqlClient | All | Web.config:12 |
| webpages:Version / webpages:Enabled | 3.0.0.0 / false | All | Web.config:15-16 |
| ClientValidationEnabled / UnobtrusiveJavaScriptEnabled | true / true | All | Web.config:17-18 |
| UseMockData / UseCustomizationData | false / false, parsed as Boolean | All unless host edits XML | Web.config:19-20 |
| system.web / compilation | debug=true; targetFramework=4.7.2 | Release transform removes debug attribute | Web.config:31; Web.Release.config:18 |
| system.web / httpRuntime / targetFramework | 4.6.1 | All | Web.config:32 |
| sessionState / mode | InProc | All | Web.config:33 |
| globalization / culture and uiCulture | en-US / en-US | All | Web.config:38 |
| httpModules and integrated modules | TelemetryCorrelation and ApplicationInsights; async session module replaces Session | All | Web.config:34-37,86-92 |
| validateIntegratedModeConfiguration | false | All | Web.config:85 |
| ExtensionlessUrlHandler variants | ISAPI 32/64-bit and integrated handlers; GET,HEAD,POST,DEBUG,PUT,DELETE,PATCH,OPTIONS | All | Web.config:94-100 |
| ISAPI scriptProcessor paths | Framework / Framework64 v4.0.30319 under `%windir%` | All | Web.config:98-99 |
| EF defaultConnectionFactory / parameter | LocalDbConnectionFactory / mssqllocaldb | All | Web.config:103-110 |
| EF provider | System.Data.SqlClient / EF SQL Server provider | All | Web.config:109-110 |
| C# CodeDOM | Provider 2.0.1.0; warningLevel 4; langversion default; suppress 1659,1699,1701 | All | Web.config:115 |
| VB CodeDOM | Provider 2.0.1.0; warningLevel 4; langversion default; suppress 41008; Web define; optionInfer enabled | All | Web.config:116 |
| Binding redirect target versions | Newtonsoft.Json 12.0.0.0; Optimization 1.1.0.0; WebGrease 1.6.5135.21930; Autofac 6.1.0.0; Antlr3.Runtime 3.5.0.2; DiagnosticSource 4.0.3.1; WebPages 3.0.0.0; MVC/Web.Http 5.2.7.0 | All | Web.config:41-81 |

Redirect old-version ranges are retained in the source; these are **assembly** versions, not necessarily NuGet package versions.

### Ported application

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| ConnectionStrings:DefaultConnection | `Server=(localdb)\mssqllocaldb;Database=eShopPorted;Trusted_Connection=True;MultipleActiveResultSets=true` | No checked-in development override | appsettings.json:2-4 [C7] |
| Logging:LogLevel:Default | Warning | Development Debug | appsettings.json:5-8; development JSON:3-5 |
| Logging:LogLevel:System | Not explicitly set in base | Development Information | Development JSON:5 [C8] |
| Logging:LogLevel:Microsoft | Not explicitly set in base | Development Information | Development JSON:6 |
| AllowedHosts | `*` | No checked-in override | appsettings.json:10 |
| UseMockData | `"true"` string, converted to Boolean | No checked-in override | appsettings.json:11; Startup:28 |
| UseCustomizationData | `"true"` string | No reader found in ported source | appsettings.json:12 |
| runtime / gcServer / enabled | true | Runtime XML | app.config:3-4 [C10] |

### View and logging configuration

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| Razor host factory / view base | System.Web MVC factory assembly 5.2.3.0 / WebViewPage | Both Views XML files | Views Web.config:11-13 [C4], [C11] |
| Razor namespaces | MVC, Ajax, Html, Optimization, Routing and app namespace | Per application | Views Web.config:14-20 |
| webpages:Enabled | false | Both view folders | Views Web.config:26 |
| BlockViewHandler | HttpNotFoundHandler for all view-folder requests | Legacy IIS; retained port config not Core equivalent | Views Web.config:29-32 |
| View compilation assembly | System.Web.Mvc 5.2.3.0 | Both view XML files | Views Web.config:36-39 |
| log4net root / appender | ALL / RollingFileAppender named file | Legacy | log4Net.xml:2-7 [C6] |
| log file / appendToFile | `logFiles\myapp.log` / true | Legacy | log4Net.xml:7-8 |
| rollingStyle / maxSizeRollBackups / maximumFileSize | Size / 5 / 10MB | Legacy | log4Net.xml:9-11 |
| staticLogFileName / layout | true / PatternLayout with date, thread, activity, level, logger, requestinfo and message | Legacy | log4Net.xml:12-14 |

Ported log4net is declared and called, but no ported log4net configuration initialization was found. Legacy AssemblyInfo references `log4net.xml` whereas the checked-in filename is `log4Net.xml`; these match on normal Windows case-insensitive filesystems but are not portable casing. [C6]

### Legacy telemetry configuration

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| TelemetryInitializers | Dependency parsing, Azure role/web role, build info, web test, synthetic user-agent, client IP, operation/correlation, user/authenticated user/account/session | Legacy | ApplicationInsights.config:3-20 [C5] |
| SyntheticUserAgentTelemetryInitializer / Filters | `search\|spider\|crawl\|Bot\|Monitor\|AlwaysOn` | Legacy | ApplicationInsights.config:12 |
| ExcludeComponentCorrelationHttpHeadersOnDomains | core.windows.net; core.chinacloudapi.cn; core.cloudapi.de; core.usgovcloudapi.net | Legacy | ApplicationInsights.config:24-34 |
| IncludeDiagnosticSourceActivities | Microsoft.Azure.EventHubs; Microsoft.Azure.ServiceBus | Legacy | ApplicationInsights.config:35-38 |
| TelemetryModules | Dependency/performance collection, QuickPulse, app-service heartbeat, Azure metadata, debugger developer mode, unhandled/unobserved exceptions, request/exception/ASP.NET diagnostics | Legacy | ApplicationInsights.config:22-114 |
| RequestTrackingTelemetryModule / Handlers | Eight handler entries for PageInspector, static/resource/bundle/script/trace/discovery/debug handlers | Legacy | ApplicationInsights.config:96-110 |
| ApplicationIdProvider | ApplicationInsightsApplicationIdProvider | Legacy | ApplicationInsights.config:116 |
| Default sink / processors / channel | QuickPulse; autocollected metrics; ServerTelemetryChannel | Legacy | ApplicationInsights.config:118-131 |
| Adaptive sampling / MaxTelemetryItemsPerSecond | 5 for non-Event stream and 5 for Event stream | Legacy | ApplicationInsights.config:122-128 |
| InstrumentationKey | Not actively configured; placeholder only in a comment | Legacy | ApplicationInsights.config:134-139 |

Telemetry activity names for Event Hubs/Service Bus are SDK collection hooks, **not evidence that the application uses messaging**. Commented performance-counter and heartbeat-exclusion examples are not active settings.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| Legacy | IIS Express enabled; URL port 52429; alternate development port metadata 53134; NTLMAuthentication false | No CPU/memory allocations declared | Not declared |
| Ported Project profile | ASPNETCORE_ENVIRONMENT=Development; local HTTP 5000 and HTTPS 5001; browser launch enabled | Server GC true; no memory/CPU limit | Not declared |
| Ported IIS Express profile | Development; local HTTP 52637, SSL 44337; windowsAuthentication false, anonymousAuthentication true | No allocations declared | Not declared |
| Utility | Runs in caller process; .NET Framework 4.6.1 target | Shared caller allocation | Not a separately scaled service |

Sources: [C9], [C10], [C12], [C14]. No scoped containers, base images, replicas, autoscaling rules, startup timeout values, or production hosting resource definitions were found. No JVM settings apply.

## Startup Dependency Chain

1. **Legacy IIS startup → DI and routes/filters/bundles → database initializer registration.** Database creation/seed is triggered by EF when first needed, not explicitly forced as a readiness step. Database mode requires available SQL Server and setup scripts/files where customization is enabled. No health-wait/retry loop is declared. [C17]
2. **Ported default host → Startup → mock selection or SQL context registration → MVC/static-file pipeline.** Default mock mode has no SQL dependency. Database mode needs a separately prepared schema: Program/Startup do not call Migrate/EnsureCreated. [C16]
3. **No ordering between the two web applications or separate library startup.** No Compose depends_on, Kubernetes readiness gate, custom readiness indicator, or startup timeout configuration exists in the assessed tree.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| Legacy CatalogDBContext connection setting | Database access configuration | Checked-in integrated-identity connection; no password literal present |
| Ported DefaultConnection setting | Database access configuration | Checked-in trusted-connection setting; no password literal present |
| Application Insights InstrumentationKey | Telemetry ingestion identifier | No active value; placeholder comment only |
| Potential deployment overrides | Unknown | Not represented by scoped deployment artifacts; any supplied credential value must be masked |

No actual credentials are reproduced in this report. Connection strings shown above contain no password/user-secret tokens. No protected-configuration encryption, DPAPI section encryption, secret-store client, managed-identity binding, or credential rotation setup was found. [C1], [C5], [C7], [C16]

### Secrets Provisioning Workflow

The repository does **not** implement a cloud secret provisioning sequence in scope. Default database access uses the operating-system identity of the executing process; production identity permissions are not specified. Core host defaults permit operators to supply connection settings through environment variables/arguments, but no checked-in deployment binds such values. Application Insights requires separate operator provisioning before a live destination is established. No specific service-principal, managed-identity role, Key Vault permission, or deployment automation is evidenced.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| Legacy UseMockData | false | Web.config; selects singleton mock versus scoped database service |
| Legacy UseCustomizationData | false | Web.config; read by initializer for CSV/ZIP customization |
| Ported UseMockData | true | JSON plus Core configuration overrides; parsed in Startup |
| Ported UseCustomizationData | true but apparently unused | JSON; no corresponding ported initialization workflow found |

Sources: [C1], [C7], [C16–C18]. These are startup configuration switches, not remote feature-management or automatic failure recovery; no A/B rollout service was identified.

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Legacy target | .NET Framework 4.7.2 | Legacy csproj:19 [C12] |
| Legacy runtime compatibility setting | httpRuntime 4.6.1 versus compilation 4.7.2 | Web.config:31-32 [C1] |
| Ported / utility target | .NET Framework 4.6.1 | Ported csproj:3; utility csproj:12 [C13], [C14] |
| Legacy MVC / Web API / Razor | 5.2.7 / 5.2.7 / 3.2.7 | Legacy csproj package declarations [C12] |
| Ported ASP.NET Core / EF Core | 2.2.0 / 2.2.6 | Ported csproj:7,16-22 [C13] |
| EF6 / Autofac | 6.2.0 / legacy 6.1.0, ported 4.9.1 | Project manifests; legacy packages.config still says Autofac 4.9.1 |
| log4net / Json.NET | 2.0.10 / legacy 12.0.1, ported 13.0.2 | Project manifests |
| Application Insights | 2.9.1 family; Intercept 2.4.0 | Legacy project manifest |
| View XML MVC references | 5.2.3.0 assembly version | Both view configuration files; differs from MVC package 5.2.7 |
| MSBuild / Visual Studio metadata | ToolsVersion 15.0 for old projects; solution VS16; installed tool versions not observed | csproj roots; solution:1-5 [C12], [C14], [C15] |
| TypeScript tools / C# language | TypeScript tools 3.0; C# default | Legacy csproj:31,41 |
| Ported SDK / Docker image / SQL Server version | SDK not pinned in scope; no image; DB engine version unspecified | No matching scoped manifests; connection settings |

These are static declarations, not proof of successful restore/build or current deployed versions. No application code was changed and no tests/builds were run.

### Source Evidence

- [C1] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\Web.config:11-118`.
- [C2] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\Web.Debug.config:5-30`.
- [C3] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\Web.Release.config:5-31`.
- [C4] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\Views\Web.config:4-39`.
- [C5] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\ApplicationInsights.config:3-139`.
- [C6] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\log4Net.xml:1-17`; `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\Properties\AssemblyInfo.cs:37`.
- [C7] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\eShopPorted\appsettings.json:1-13`.
- [C8] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\eShopPorted\appsettings.Development.json:1-9`.
- [C9] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\eShopPorted\Properties\launchSettings.json:2-25`.
- [C10] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\eShopPorted\app.config:2-6`.
- [C11] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\eShopPorted\Views\Web.config:4-39`.
- [C12] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\eShopLegacyMVC.csproj:2-54,90-104,242-259,432-465,473-570`.
- [C13] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\eShopPorted\eShopPorted.csproj:1-32`.
- [C14] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\eShopLegacy.Utilities\eShopLegacy.Utilities.csproj:2-47`.
- [C15] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\eShopLegacyMVC.sln:1-18`; `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\packages.config:3-44`.
- [C16] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\eShopPorted\Program.cs:8-15`; `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\eShopPorted\Startup.cs:25-65`.
- [C17] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\Global.asax.cs:27-35,60-90`.
- [C18] `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs:26-42,339-353`; `D:\a\eShopModernizing\eShopModernizing\eShopLegacyMVCSolution\eShopPorted\Modules\ApplicationModule.cs:16-28`.
