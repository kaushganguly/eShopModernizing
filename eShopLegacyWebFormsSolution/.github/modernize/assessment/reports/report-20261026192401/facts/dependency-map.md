# Dependency Map

The eShopLegacyWebForms project declares 44 NuGet dependencies in `packages.config`, all targeting .NET Framework 4.7.2. The diagram highlights the framework and infrastructure packages; compact groups keep the complete dependency map readable.

## Dependencies

```mermaid
flowchart LR
    App["eShopLegacyWebForms"]
    subgraph Web["Web Frameworks"]
        WebForms["ASP.NET Web Forms infrastructure"]
        Script["ASP.NET ScriptManager packages"]
        Friendly["Microsoft ASP.NET FriendlyUrls 1.0.2"]
        Optimization["ASP.NET Web Optimization 1.1.3"]
        Session["ASP.NET SessionState 1.1.0"]
    end
    subgraph DB["Database and ORM"]
        EF["Entity Framework 6.2.0"]
        Sql["System.Data.SqlClient framework provider"]
    end
    subgraph Obs["Observability"]
        AppInsights["Microsoft Application Insights 2.9.1"]
        AiPackages["Application Insights web, dependency, Windows Server, and channel packages 2.9.1"]
        AiAgent["Application Insights Agent Intercept 2.4.0"]
        Correlation["ASP.NET Telemetry Correlation 1.0.5"]
    end
    subgraph Log["Logging"]
        Log4Net["log4net 2.0.10"]
    end
    subgraph Util["Utilities"]
        Autofac["Autofac 4.9.1 and Autofac.Web 4.0.0"]
        Json["Newtonsoft.Json 12.0.1"]
        Compiler["Microsoft.CodeDom and Microsoft.Net.Compilers 2.0.1 and 2.10.0"]
        WebAssets["Bootstrap 4.3.1, jQuery 3.5.0, Popper.js 1.14.3, Modernizr 2.8.3, Respond 1.4.2"]
        Support["Antlr, Microsoft.Web.Infrastructure, WebGrease, Pipelines.Sockets.Unofficial"]
        Runtime["System.Buffers, DiagnosticSource, PerformanceCounter, Compression, Pipelines, Memory, Numerics, Unsafe, Channels, and Tasks.Extensions"]
    end

    App -->|"presentation"| Web
    App -->|"persistence"| DB
    App -->|"telemetry"| Obs
    App -->|"logging"| Log
    App -->|"supporting libraries"| Util
    EF -->|"SQL provider"| Sql
    AppInsights -.->|"web instrumentation"| AiPackages
```

### Dependency Summary

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web Frameworks | 14 | ASP.NET Web Forms, FriendlyUrls 1.0.2, Web Optimization 1.1.3, SessionState 1.1.0 | Classic System.Web application and client asset integration |
| Database / ORM | 1 | Entity Framework 6.2.0 | Uses the .NET Framework SQL Server provider |
| Observability | 8 | Application Insights 2.9.1, Telemetry Correlation 1.0.5 | Multiple Application Insights collection modules |
| Logging | 1 | log4net 2.0.10 | Application logging |
| Utilities | 20 | Autofac 4.9.1, Newtonsoft.Json 12.0.1, Bootstrap 4.3.1, jQuery 3.5.0 | Includes UI assets, compiler support, and System.* compatibility libraries |

### Version & Compatibility Risks

The application targets .NET Framework 4.7.2 and uses classic ASP.NET Web Forms, which constrains hosting and modernization options. Entity Framework 6.2.0, Autofac 4.x, Newtonsoft.Json 12.0.1, jQuery 3.5.0, and the Application Insights 2.9.1 family are older package versions; compatibility should be checked before any framework or platform migration.

### Notable Observations

- The application uses legacy `packages.config` dependency management rather than SDK-style package references or central package management.
- Application Insights is split across several independently versioned instrumentation packages.
- The declared package set contains no explicit test-scoped packages.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|

Total test-scope dependencies: 0

No test dependencies are declared in `packages.config`; no test project was found in the target solution.
