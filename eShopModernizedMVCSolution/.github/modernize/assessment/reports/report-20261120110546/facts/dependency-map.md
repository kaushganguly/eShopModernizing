# Dependency Map

The eShopModernizedMVC project has 146 package entries in `packages.config`, including framework-support and transitive packages. The diagram shows the major modernization-relevant libraries and aggregates the long tail.

## Dependencies

```mermaid
flowchart LR
    D_App["eShopModernizedMVC"]
    subgraph D_Web["Web Frameworks"]
        D_MVC["ASP.NET MVC 5.2.7"]
        D_Razor["ASP.NET Razor 3.2.7"]
        D_Bootstrap["Bootstrap 4.3.1"]
        D_JQuery["jQuery 3.5.0"]
    end
    subgraph D_Data["Database and ORM"]
        D_EF["Entity Framework 6.3.0"]
        D_SQL["System Data SqlClient framework provider"]
    end
    subgraph D_Messaging["Messaging"]
        D_ServiceBus["Windows Azure Service Bus 6.0.0"]
    end
    subgraph D_Cache["Caching"]
        D_Redis["StackExchange.Redis 2.0.601"]
        D_RedisSession["Redis Session State Provider 4.0.1"]
    end
    subgraph D_Logging["Logging"]
        D_Log4net["log4net 2.0.12"]
        D_LogAppender["Azure log4net Appender 1.4.3"]
    end
    subgraph D_Security["Security"]
        D_Owin["OWIN OpenID Connect 4.0.1"]
        D_Identity["Microsoft Identity Model 5.6.0"]
        D_KeyVault["Azure Key Vault 3.0.4"]
    end
    subgraph D_Observability["Observability"]
        D_AppInsights["Application Insights 2.11.0"]
    end
    subgraph D_Utilities["Utilities"]
        D_Other["91 framework and utility packages"]
    end

    D_App -->|"web"| D_Web
    D_App -->|"persistence"| D_Data
    D_App -->|"messaging client declared"| D_Messaging
    D_App -->|"optional session cache"| D_Cache
    D_App -->|"logging"| D_Logging
    D_App -->|"identity and secrets"| D_Security
    D_App -->|"telemetry"| D_Observability
    D_App -->|"support libraries"| D_Utilities
```

### Dependency Summary

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web Frameworks | 15 | ASP.NET MVC, Razor, Bootstrap, jQuery | Legacy server-rendered MVC application |
| Database and ORM | 5 | Entity Framework, Microsoft.Data client libraries, System.Spatial | SQL Server persistence |
| Messaging | 1 | Windows Azure Service Bus | Package declared; no active messaging flow was identified |
| Caching | 2 | StackExchange.Redis, Redis Session State Provider | Redis session configuration is commented out |
| Logging | 2 | log4net, Azure log4net Appender | Application logging |
| Security | 19 | OWIN Security, Identity Model, Azure Key Vault | Optional identity and configuration integrations |
| Observability | 11 | Application Insights packages | Includes collectors and framework integration packages |
| Utilities | 91 | System.* packages, Azure client libraries, serialization and build support | Aggregated for readability |

### Version & Compatibility Risks

The project targets .NET Framework 4.7.2 and ASP.NET MVC 5, rather than modern .NET. Several package families are legacy versions, including Microsoft.Extensions 3.0.0, older Azure client libraries, and the 5.6.0 Identity Model packages. Moving to a modern runtime will require replacing or upgrading framework-specific packages and reviewing binding redirects.

### Notable Observations

- The project uses a legacy `packages.config` dependency model and contains 146 package entries.
- Redis packages are present, but the Redis session-state configuration is commented out.
- Azure Service Bus is declared but no application-level message publishing or receiving path was identified.
- Application Insights is represented by multiple integration and collector packages.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| None detected | — | No test project or test-scoped package entries are present in the solution. |

Total test-scope dependencies: 0

No test dependencies were detected in the solution package manifests.
