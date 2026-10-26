# Dependency Map

The solution has two web applications and one shared library. The legacy application declares 43 packages in `packages.config`; the ported application declares 14 package references and the shared library has no external package declarations.

## Dependencies

```mermaid
flowchart LR
    App["eShopLegacyMVC solution"]
    subgraph Web["Web Frameworks"]
        MVC["ASP.NET MVC 5.2.7"]
        WebApi["ASP.NET Web API 5.2.7"]
        AspNetCore["ASP.NET Core 2.2.0"]
        CoreMvc["ASP.NET Core MVC 2.2.0"]
    end
    subgraph Data["Database and ORM"]
        EF6["Entity Framework 6.2.0"]
        EFCore["Entity Framework Core 2.2.6"]
        SqlServer["EF Core SQL Server 2.2.6"]
        SqlClient["System.Data.SqlClient 4.6.1"]
    end
    subgraph Logging["Logging and Observability"]
        Log4Net["log4net 2.0.10"]
        AppInsights["Application Insights 2.9.1"]
    end
    subgraph Injection["Dependency Injection"]
        Autofac["Autofac 4.9.1"]
        AutofacCore["Autofac.Extensions.DependencyInjection 4.4.0"]
        AutofacMvc["Autofac.Mvc5 4.0.2"]
    end
    subgraph Utilities["Utilities and Web Assets"]
        JsonNet["Newtonsoft.Json 12.0.1 and 13.0.2"]
        Bootstrap["Bootstrap 4.3.1"]
        JQuery["jQuery 3.5.0"]
        WebOpt["Microsoft.AspNet.Web.Optimization 1.1.3"]
    end
    App -->|"legacy presentation"| MVC
    App -->|"legacy API"| WebApi
    App -->|"ported application"| AspNetCore
    App -->|"ported persistence"| Data
    App -->|"logging"| Logging
    App -->|"dependency registration"| Injection
    App -->|"serialization and assets"| Utilities
```

### Dependency Summary

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web Frameworks | 4 | ASP.NET MVC 5, Web API 2, ASP.NET Core 2.2 | Two generations of the web stack coexist |
| Database / ORM | 4 | Entity Framework 6, EF Core 2.2, SQL Server provider | Separate persistence implementations |
| Logging / Observability | 8 | log4net, Application Insights | Legacy telemetry packages and logging |
| Dependency Injection | 3 | Autofac, Autofac MVC integration | Autofac is shared across app implementations |
| Utilities / UI | 40 | Newtonsoft.Json, Bootstrap, jQuery, WebGrease | Includes framework support and frontend packages |

### Version & Compatibility Risks

The projects target .NET Framework 4.6.1, while the web configuration mentions 4.7.2. ASP.NET Core 2.2 and EF Core 2.2 are out of support, and many legacy packages are several major versions behind current releases. AppCAT's full package compatibility and vulnerability details are in the versioned report and security assessment.

### Notable Observations

- The legacy application uses EF6 while the ported application uses EF Core 2.2.
- Newtonsoft.Json is declared at different versions in the two applications.
- No centralized package version management is present.
- No test-scoped packages or test projects were identified in this solution.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| None detected | — | No test dependencies or test projects were identified |

Total test-scope dependencies: 0

No test dependencies detected.
