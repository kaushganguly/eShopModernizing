# Dependency Map

The `eShopLegacyMVCSolution` project files declare 63 package references, representing 59 distinct package/version coordinates across the legacy MVC application and its ASP.NET Core port. The legacy application's `packages.config` also records its pinned package inventory.

## Dependencies

```mermaid
flowchart LR
    App["eShopLegacyMVCSolution"]
    subgraph Web["Web Frameworks"]
        Mvc["ASP.NET MVC 5.2.7"]
        Razor["ASP.NET Razor 3.2.7"]
        Pages["ASP.NET WebPages 3.2.7"]
        WebApi["ASP.NET Web API 5.2.7"]
        WebApiClient["Web API Client 5.2.7"]
        WebApiCore["Web API Core 5.2.7"]
        WebApiHost["Web API WebHost 5.2.7"]
        Session["ASP.NET SessionState Module 1.1.0"]
        TelemetryCorrelation["ASP.NET TelemetryCorrelation 1.0.5"]
        Optimization["ASP.NET Web Optimization 1.1.3"]
        AspNetCore["ASP.NET Core 2.2.0"]
        CoreMvc["ASP.NET Core MVC 2.2.0"]
        StaticFiles["ASP.NET Core StaticFiles 2.2.0"]
        AutofacMvc["Autofac MVC5 4.0.2"]
        AutofacDI["Autofac Extensions DependencyInjection 4.4.0"]
    end
    subgraph Data["Database and ORM"]
        Ef6["Entity Framework 6.2.0"]
        EfCore["EF Core 2.2.6"]
        EfRelational["EF Core Relational 2.2.6"]
        EfSql["EF Core SQL Server 2.2.6"]
        EfDesign["EF Core Design 2.2.6"]
    end
    subgraph Observability["Observability"]
        AppInsights["Application Insights 2.9.1"]
        AiAgent["AI Agent Intercept 2.4.0"]
        AiDependency["AI Dependency Collector 2.9.1"]
        AiPerf["AI Performance Counter Collector 2.9.1"]
        AiWeb["AI Web 2.9.1"]
        AiServer["AI Windows Server 2.9.1"]
        AiChannel["AI Windows Server Telemetry Channel 2.9.1"]
    end
    subgraph Logging["Logging"]
        Log4net["log4net 2.0.10"]
    end
    subgraph Utils["Utilities"]
        Other["29 additional UI, JSON, compiler, and system utility packages"]
        Autofac49["Autofac 4.9.1"]
        Autofac61["Autofac 6.1.0"]
    end

    App -->|"web frameworks"| Web
    App -->|"persistence"| Data
    App -->|"telemetry"| Observability
    App -->|"logging"| Logging
    App -->|"utilities"| Utils
```

### Dependency Summary

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web Frameworks | 15 | ASP.NET MVC 5.2.7, ASP.NET Web API 5.2.7, ASP.NET Core MVC 2.2.0 | Classic MVC and the separate ASP.NET Core port are both present |
| Database / ORM | 5 | Entity Framework 6.2.0, EF Core 2.2.6, EF Core SQL Server 2.2.6 | Separate persistence stacks are used by the two applications |
| Observability | 7 | Application Insights 2.9.1 family | The legacy application has multiple Application Insights components |
| Logging | 1 | log4net 2.0.10 | Declared in both applications |
| Utilities | 31 | Autofac 4.9.1 / 6.1.0, Newtonsoft.Json 12.0.1 / 13.0.2 | Includes UI, compatibility, compiler, and system support packages |

### Version & Compatibility Risks

The port targets .NET Framework 4.6.1 while using ASP.NET Core 2.2 and EF Core 2.2, and the legacy application targets .NET Framework 4.7.2 with EF 6.2. These framework and package generations are old and require compatibility review for supported .NET targets. ASP.NET Core 2.2 and EF Core 2.2 are end-of-support; the solution also carries version skew in Newtonsoft.Json across projects.

### Notable Observations

- The solution contains two distinct web stacks and ORM generations, so modernization work should preserve each application's boundaries.
- Autofac and log4net are shared across both applications, while the port references the legacy `Autofac.Mvc5` package despite using ASP.NET Core MVC.
- The legacy package inventory includes numerous system and UI support packages; the port separately declares old ASP.NET Core and EF Core packages.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| — | — | No test-scoped dependencies detected |

Total test-scope dependencies: 0

No test projects or test-scoped package references were detected in this solution folder.
