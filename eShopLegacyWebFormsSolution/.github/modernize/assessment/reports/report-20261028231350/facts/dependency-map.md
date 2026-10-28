# Dependency Map

The eShopLegacyWebForms application declares 44 NuGet packages in `packages.config`, all targeting .NET Framework 4.7.2. There are no test-scoped packages in the project manifest.

## Dependencies

```mermaid
flowchart LR
    App["eShopLegacyWebForms"]
    subgraph Web["Web Frameworks and UI"]
        WebStack["ASP.NET Web Forms platform; FriendlyUrls 1.0.2 and Core 1.0.2; Web Optimization 1.1.3; Web Optimization WebForms 1.1.3; SessionState Module 1.1.0; TelemetryCorrelation 1.0.5"]
        UiStack["Bootstrap 4.3.1; ScriptManager bootstrap 4.3.1; ScriptManager jQuery 3.3.1; jQuery 3.5.0; Popper.js 1.14.3"]
        BrowserStack["ScriptManager MSAjax 5.0.0; ScriptManager WebForms 5.0.0; Modernizr 2.8.3; Respond 1.4.2"]
    end
    subgraph Data["Database and ORM"]
        EntityFramework["EntityFramework 6.2.0"]
    end
    subgraph Logging["Logging"]
        Log4net["log4net 2.0.10"]
    end
    subgraph Observe["Observability"]
        Insights["ApplicationInsights 2.9.1; Agent.Intercept 2.4.0; DependencyCollector 2.9.1; PerfCounterCollector 2.9.1"]
        InsightsWeb["ApplicationInsights.Web 2.9.1; WindowsServer 2.9.1; WindowsServer.TelemetryChannel 2.9.1"]
    end
    subgraph Utilities["Utilities and Build Support"]
        Build["Antlr 3.5.0.2; CodeDom.Providers.DotNetCompilerPlatform 2.0.1; Microsoft.Net.Compilers 2.10.0"]
        Json["Newtonsoft.Json 12.0.1"]
        Infrastructure["Microsoft.Web.Infrastructure 1.0.0.0; WebGrease 1.6.0; Autofac 4.9.1; Autofac.Web 4.0.0"]
        SystemPackages["Pipelines.Sockets.Unofficial 1.0.7; System.Buffers 4.4.0; DiagnosticSource 4.5.1; PerformanceCounter 4.5.0"]
        RuntimePackages["IO.Compression 4.3.0; IO.Compression.ZipFile 4.3.0; IO.Pipelines 4.5.1; System.Memory 4.5.1"]
        AsyncPackages["Numerics.Vectors 4.4.0; Runtime.CompilerServices.Unsafe 4.5.0; Threading.Channels 4.5.0; Tasks.Extensions 4.5.1"]
    end

    App -->|"presentation"| Web
    App -->|"persistence"| Data
    App -->|"logging"| Logging
    App -->|"telemetry"| Observe
    App -->|"support libraries"| Utilities
```

### Dependency Summary

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web Frameworks and UI | 15 | ASP.NET Web Forms, Bootstrap 4.3.1, jQuery 3.5.0 | Legacy server-rendered UI and browser assets |
| Database / ORM | 1 | EntityFramework 6.2.0 | Entity Framework 6, using the configured SQL Server provider |
| Logging | 1 | log4net 2.0.10 | Application logging |
| Observability | 7 | Application Insights 2.9.1 and its collectors | Web, dependency, performance-counter, and server telemetry |
| Utilities | 20 | Autofac 4.9.1, Newtonsoft.Json 12.0.1, System.* libraries | Includes build support and framework compatibility packages |
| **Total** | **44** |  | All packages target net472 |

### Version & Compatibility Risks

The application targets .NET Framework 4.7.2 and uses legacy ASP.NET Web Forms, Entity Framework 6.2.0, and older UI and telemetry packages. These dependencies require compatibility review for modernization; the existing Web Forms application model is tied to .NET Framework, and moving to a newer .NET runtime is not a simple target-framework change.

### Notable Observations

- NuGet dependencies are managed through the legacy `packages.config` format rather than SDK-style project package references.
- The package inventory includes numerous .NET Framework support libraries and a compiler package, increasing the migration surface.
- No security-specific package or dedicated caching, messaging, or test framework dependency is declared.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| None detected | — | No packages are declared as test dependencies in `packages.config`. |

Total test-scope dependencies: 0

No test dependencies were detected in the project package manifest.
