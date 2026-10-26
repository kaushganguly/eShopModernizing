# Dependency Map

The `eShopModernizedMVC` project has 146 package entries in `packages.config`, including the legacy ASP.NET stack and Azure integrations. The list contains no detected test-framework dependencies.

## Dependencies

```mermaid
flowchart LR
    App["eShopModernizedMVC"]
    subgraph Web["Web Frameworks"]
        Mvc["Microsoft.AspNet.Mvc 5.2.7"]
        Razor["ASP.NET Razor 3.2.7"]
        WebPages["ASP.NET WebPages 3.2.7"]
        Optimization["Web Optimization 1.1.3"]
    end
    subgraph DB["Database and ORM"]
        EF["Entity Framework 6.3.0"]
        SQL["System.Data.SqlClient framework provider"]
    end
    subgraph Storage["Storage and Messaging"]
        AzureStorage["WindowsAzure.Storage 9.3.3"]
        ServiceBus["WindowsAzure.ServiceBus 6.0.0"]
    end
    subgraph Security["Security and Identity"]
        Owin["Microsoft.Owin 4.2.2"]
        Oidc["Microsoft.Owin.Security.OpenIdConnect 4.0.1"]
        KeyVault["Microsoft.Azure.KeyVault 3.0.4"]
        AuthLib["Microsoft.Azure.Services.AppAuthentication 1.3.1"]
    end
    subgraph Observability["Logging and Observability"]
        Log4Net["log4net 2.0.12"]
        AppInsights["Microsoft.ApplicationInsights 2.11.0"]
        AiWeb["Microsoft.ApplicationInsights.Web 2.11.2"]
    end
    subgraph Cache["Caching"]
        RedisProvider["Microsoft.Web.RedisSessionStateProvider 4.0.1"]
        RedisClient["StackExchange.Redis 2.0.601"]
    end
    subgraph Utilities["Utilities"]
        Autofac["Autofac 4.9.4"]
        Json["Newtonsoft.Json 13.0.2"]
        Compiler["Microsoft.Net.Compilers 3.3.1"]
    end

    App -->|"web rendering"| Web
    App -->|"persistence"| DB
    App -->|"external integrations"| Storage
    App -->|"authentication and secrets"| Security
    App -->|"telemetry and logging"| Observability
    App -->|"optional session support"| Cache
    App -->|"dependency injection and utilities"| Utilities
```

### Dependency Summary

| Category | Count shown | Key Libraries | Notes |
|---|---:|---|---|
| Web Frameworks | 4 | ASP.NET MVC 5.2.7, Razor 3.2.7, WebPages 3.2.7 | Classic `System.Web` MVC application |
| Database and ORM | 2 | Entity Framework 6.3.0, SQL client provider | SQL Server persistence is optional when mock data is enabled |
| Storage and Messaging | 2 | WindowsAzure.Storage 9.3.3, WindowsAzure.ServiceBus 6.0.0 | Blob storage is used by the Azure image service; no service-bus use was identified in application code |
| Security and Identity | 4 | OWIN 4.2.2, OWIN OpenID Connect 4.0.1, Azure Key Vault 3.0.4 | Azure AD and Key Vault support are configuration-dependent |
| Logging and Observability | 3 | log4net 2.0.12, Application Insights 2.11.0 | Includes Application Insights web and listener integrations |
| Caching | 2 | Redis session provider 4.0.1, StackExchange.Redis 2.0.601 | Redis session configuration in `Web.config` is commented out |
| Utilities | 3 | Autofac 4.9.4, Newtonsoft.Json 13.0.2, Microsoft.Net.Compilers 3.3.1 | The remaining package entries include framework support libraries |

### Version & Compatibility Risks

The application targets .NET Framework 4.7.2 and uses ASP.NET MVC 5, EF6, and older Azure client libraries. These versions are tied to the classic `System.Web` hosting model, so moving to modern .NET requires framework and package replacements rather than a target-framework-only change.

### Notable Observations

- `packages.config` contains 146 entries; most do not represent separately used application features.
- Redis-related packages are present, but the Redis session provider configuration is commented out.
- `WindowsAzure.ServiceBus` appears in the package manifest, but no service-bus usage was found in the application source.
- No test project or test-specific package was found in the solution scope.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| None detected | — | No test project or test-framework package was found |

Total test-scope dependencies: 0

No test dependencies or test infrastructure were detected in the solution.
