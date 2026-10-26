# Dependency Map

The eShopModernizedMVC project declares 146 NuGet package entries in `src/eShopModernizedMVC/packages.config`; the legacy packages.config format also lists installed transitive packages.

## Dependencies

```mermaid
flowchart LR
    App["eShopModernizedMVC"]
    subgraph Web["Web Frameworks"]
        Mvc["ASP.NET MVC 5.2.7"]
        Razor["ASP.NET Razor and WebPages 3.2.7"]
        WebOpt["Web Optimization 1.1.3"]
        Js["jQuery 3.5.0 and Bootstrap 4.3.1"]
        Ui["UI support packages 11"]
    end
    subgraph Data["Database and ORM"]
        Ef["Entity Framework 6.3.0"]
        OData["Microsoft Data OData 5.8.4"]
        DbSupport["Database compatibility libraries 3"]
    end
    subgraph Cache["Caching"]
        RedisSession["Redis Session State Provider 4.0.1"]
        RedisClient["StackExchange.Redis 2.0.601"]
    end
    subgraph Log["Logging and Observability"]
        Log4Net["log4net 2.0.12"]
        AzureAppender["log4net Azure Appender 1.4.3"]
        AppInsights["Application Insights packages 10"]
    end
    subgraph Security["Security and Identity"]
        Owin["OWIN authentication packages 6"]
        Identity["Identity Model packages 8"]
        KeyVault["Azure Key Vault packages 4"]
    end
    subgraph Messaging["Messaging"]
        ServiceBus["Windows Azure Service Bus 6.0.0"]
    end
    subgraph Util["Utilities"]
        SystemCompat["System compatibility packages 50 plus"]
        Other["Remaining libraries and configuration packages"]
    end

    App -->|"presentation"| Web
    App -->|"persistence"| Data
    App -->|"optional session provider"| Cache
    App -->|"logging and telemetry"| Log
    App -->|"optional authentication and secrets"| Security
    App -->|"messaging library"| Messaging
    App -->|"support libraries"| Util
```

### Dependency Summary

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web Frameworks | 16 | ASP.NET MVC, Razor, Web Optimization, jQuery, Bootstrap | Legacy server-rendered MVC stack |
| Database / ORM | 5 | Entity Framework, Microsoft.Data.OData, Microsoft.Data.Edm | SQL Server access is through EF6 |
| Caching | 2 | Redis Session State Provider, StackExchange.Redis | Packages are declared; active Redis session configuration was not found |
| Logging / Observability | 12 | log4net, Application Insights | Application Insights and Azure appender integrations |
| Security | 17 | OWIN, IdentityModel, Azure Key Vault, Azure App Authentication | Optional OIDC and configuration-builder integration |
| Messaging | 1 | Windows Azure Service Bus | No message-broker usage was identified in the application flow |
| Utilities | 92 | System compatibility packages and other libraries | Includes many framework-support packages from packages.config |

### Version & Compatibility Risks

The application targets .NET Framework 4.7.2 and ASP.NET MVC 5.2.7, with several older integration packages. The package set includes beta configuration-builder packages and legacy System.* support libraries; moving to a newer .NET runtime will require compatibility review rather than only changing a target framework.

### Notable Observations

- The project uses classic `packages.config`, and its 146 entries include framework support and transitive packages as well as application-facing libraries.
- The ASP.NET Redis session provider and Redis client are present, but no active Redis session configuration was found.
- Messaging and blob libraries are declared although the app's primary business path is catalog management.
- The selected solution contains no separate test project or test-scoped package set.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| None detected | — | No test project or test-only dependency was found in the selected solution. |

Total test-scope dependencies: 0

No test dependencies were detected in the selected project.
