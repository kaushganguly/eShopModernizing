# Dependency Map

The eShopLegacyWebForms application declares 44 NuGet packages in `src/eShopLegacyWebForms/packages.config`.

## Dependencies

```mermaid
flowchart LR
    App["eShopLegacyWebForms"]
    subgraph Web["Web Frameworks and UI"]
        WebPkgs["AspNet.ScriptManager.bootstrap 4.3.1<br/>AspNet.ScriptManager.jQuery 3.3.1<br/>bootstrap 4.3.1<br/>jQuery 3.5.0<br/>Microsoft.AspNet.FriendlyUrls 1.0.2<br/>Microsoft.AspNet.FriendlyUrls.Core 1.0.2<br/>Microsoft.AspNet.ScriptManager.MSAjax 5.0.0<br/>Microsoft.AspNet.ScriptManager.WebForms 5.0.0<br/>Microsoft.AspNet.SessionState.SessionStateModule 1.1.0<br/>Microsoft.AspNet.TelemetryCorrelation 1.0.5<br/>Microsoft.AspNet.Web.Optimization 1.1.3<br/>Microsoft.AspNet.Web.Optimization.WebForms 1.1.3<br/>Modernizr 2.8.3<br/>popper.js 1.14.3<br/>Respond 1.4.2"]
    end
    subgraph Data["Database and ORM"]
        EF["EntityFramework 6.2.0"]
    end
    subgraph Logging["Logging"]
        Log4Net["log4net 2.0.10"]
    end
    subgraph Observability["Observability"]
        AI["Microsoft.ApplicationInsights 2.9.1<br/>Microsoft.ApplicationInsights.Agent.Intercept 2.4.0<br/>Microsoft.ApplicationInsights.DependencyCollector 2.9.1<br/>Microsoft.ApplicationInsights.PerfCounterCollector 2.9.1<br/>Microsoft.ApplicationInsights.Web 2.9.1<br/>Microsoft.ApplicationInsights.WindowsServer 2.9.1<br/>Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel 2.9.1"]
    end
    subgraph Utilities["Utilities and Runtime Support"]
        Utils["Antlr 3.5.0.2<br/>Autofac 4.9.1<br/>Autofac.Web 4.0.0<br/>Microsoft.CodeDom.Providers.DotNetCompilerPlatform 2.0.1<br/>Microsoft.Net.Compilers 2.10.0<br/>Microsoft.Web.Infrastructure 1.0.0.0<br/>Newtonsoft.Json 12.0.1<br/>Pipelines.Sockets.Unofficial 1.0.7<br/>System.Buffers 4.4.0<br/>System.Diagnostics.DiagnosticSource 4.5.1<br/>System.Diagnostics.PerformanceCounter 4.5.0<br/>System.IO.Compression 4.3.0<br/>System.IO.Compression.ZipFile 4.3.0<br/>System.IO.Pipelines 4.5.1<br/>System.Memory 4.5.1<br/>System.Numerics.Vectors 4.4.0<br/>System.Runtime.CompilerServices.Unsafe 4.5.0<br/>System.Threading.Channels 4.5.0<br/>System.Threading.Tasks.Extensions 4.5.1<br/>WebGrease 1.6.0"]
    end
    App -->|"presentation"| WebPkgs
    App -->|"persistence"| EF
    App -->|"logging"| Log4Net
    App -->|"telemetry"| AI
    App -->|"support"| Utils
```

### Dependency Summary

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web Frameworks and UI | 15 | ASP.NET Web Forms packages, Bootstrap 4.3.1, jQuery 3.5.0 | Legacy server-side UI and bundled browser libraries |
| Database and ORM | 1 | EntityFramework 6.2.0 | EF6 SQL Server persistence |
| Logging | 1 | log4net 2.0.10 | Application logging |
| Observability | 7 | Application Insights 2.9.1 components | Web, dependency, performance-counter, and Windows telemetry |
| Utilities and Runtime Support | 20 | Autofac 4.9.1, Newtonsoft.Json 12.0.1, .NET support packages | Includes compiler/build and runtime compatibility packages |
| **Total** | **44** |  | Declared package count from `packages.config` |

### Version & Compatibility Risks

The project targets .NET Framework 4.7.2 and depends on older Web Forms, EF6, Bootstrap, and telemetry packages. Newtonsoft.Json 12.0.1 is affected by high-severity CVE-2024-21907; the patched version is 13.0.1 or later. NuGet CLI dependency resolution could not load the legacy WebApplication targets, so transitive dependencies were not enumerated.

### Notable Observations

- `packages.config` lists 44 direct dependencies and no test-scoped packages.
- The project uses both ASP.NET Web Forms UI libraries and Autofac's older Web Forms integration.
- AppCAT identified the checked-in connection string as a configuration concern; the connection uses LocalDB and integrated security rather than an embedded password.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| None detected | — | No test-scoped packages or test project found in the assessed workspace. |

Total test-scope dependencies: 0

No test dependencies were declared in the inspected project package configuration.
