# Dependency Map

The eShopModernizedMVC project declares 146 NuGet packages through `packages.config`. The diagram highlights key dependencies; minor framework-support packages are aggregated for readability.

## Dependencies

```mermaid
flowchart LR
    App["eShopModernizedMVC"]
    subgraph Web["Web Framework and UI"]
        MVC["Microsoft.AspNet.Mvc 5.2.7"]
        Razor["Microsoft.AspNet.Razor 3.2.7"]
        Optimization["Microsoft.AspNet.Web.Optimization 1.1.3"]
        AutofacMvc["Autofac.Mvc5 4.0.2"]
        WebOther["12 additional web and UI packages"]
    end
    subgraph DB["Database and ORM"]
        EF["EntityFramework 6.3.0"]
        DbOther["4 additional data packages"]
    end
    subgraph Messaging["Messaging"]
        ServiceBus["WindowsAzure.ServiceBus 6.0.0"]
    end
    subgraph Cache["Caching"]
        Redis["StackExchange.Redis 2.0.601"]
        RedisSession["Microsoft.Web.RedisSessionStateProvider 4.0.1"]
    end
    subgraph Logging["Logging"]
        Log4Net["log4net 2.0.12"]
        AzureAppender["log4net.Appender.Azure 1.4.3.0"]
        InsightsAppender["Microsoft.ApplicationInsights.Log4NetAppender 2.11.0"]
    end
    subgraph Observability["Observability"]
        Insights["Microsoft.ApplicationInsights 2.11.0"]
        InsightsWeb["Microsoft.ApplicationInsights.Web 2.11.2"]
        InsightsOther["8 additional telemetry packages"]
    end
    subgraph Security["Security and Identity"]
        SecurityDeps["22 security and identity packages including Azure Key Vault, OWIN, and IdentityModel"]
    end
    subgraph Utility["Utilities and Runtime"]
        Support["87 runtime and utility packages"]
    end

    App -->|"web"| Web
    App -->|"persistence"| DB
    App -->|"messaging"| Messaging
    App -->|"session cache"| Cache
    App -->|"logging"| Logging
    App -->|"telemetry"| Observability
    App -->|"identity and secrets"| Security
    App -->|"runtime support"| Utility
```

### Dependency Summary

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web Framework and UI | 16 | ASP.NET MVC, Razor, Web Optimization, Autofac.Mvc5 | ASP.NET MVC 5 application |
| Database and ORM | 5 | Entity Framework 6.3.0 | SQL Server persistence |
| Messaging | 1 | WindowsAzure.ServiceBus | Declared messaging library; no message flow found in application code |
| Caching | 2 | StackExchange.Redis, Redis Session State Provider | Redis is used for optional session state |
| Logging | 3 | log4net, Azure appender, Application Insights log appender | Logging configuration is externalized in log4net.xml |
| Observability | 10 | Application Insights and telemetry components | Classic Application Insights integration |
| Security and Identity | 22 | Azure Key Vault, Azure App Authentication, OWIN, IdentityModel | Optional OIDC and secrets configuration |
| Utilities and Runtime | 87 | System.* and other support libraries | Includes framework compatibility packages |

### Version & Compatibility Risks

The project targets .NET Framework 4.7.2 and uses the classic ASP.NET MVC 5 and Entity Framework 6 stack. Several dependency families are older, including IdentityModel 5.6.0, Newtonsoft.Json 13.0.2, and log4net 2.0.12; the security assessment also identified medium-severity advisories for IdentityModel token packages and log4net.

### Notable Observations

- `packages.config` is the dependency source; the project does not use central package management or a lock file.
- Azure Key Vault and configuration-builder packages are present, but the application enables those integrations through configuration.
- Redis is declared for optional session storage; no general-purpose application cache was identified.
- No test-scoped packages or separate test project were found in the target solution.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| None detected | — | No test framework package or test project was found in this solution. |

Total test-scope dependencies: 0

The target solution contains no declared test dependencies; automated test coverage could not be identified from its project structure.
