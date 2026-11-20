# Dependency Map

The `eShopModernizedWebForms` solution uses a legacy .NET Framework package manifest. Its `packages.config` contains 147 package entries, including platform and transitive package entries; key modernization dependencies are shown below.

## Dependencies

```mermaid
flowchart LR
    App["eShopModernizedWebForms"]
    subgraph Web["Web Frameworks"]
        WebForms["ASP.NET Web Forms .NET Framework 4.7.2"]
        Friendly["Microsoft ASP.NET FriendlyUrls 1.0.2"]
        Optimization["Web Optimization WebForms 1.1.3"]
    end
    subgraph DB["Database / ORM"]
        EF["Entity Framework 6.3.0"]
    end
    subgraph Security["Security"]
        Owin["Microsoft OWIN OpenID Connect 4.0.1"]
        Identity["Microsoft IdentityModel Tokens 5.6.0"]
        KeyVault["Azure Key Vault 3.0.4"]
    end
    subgraph Cache["Caching"]
        RedisSession["Redis Session State Provider 4.0.1"]
        RedisClient["StackExchange.Redis 2.0.601"]
    end
    subgraph Cloud["Storage and Messaging"]
        Blob["WindowsAzure.Storage 9.3.3"]
        Bus["WindowsAzure.ServiceBus 6.0.0"]
    end
    subgraph Telemetry["Logging and Observability"]
        Log4Net["log4net 2.0.10"]
        AppInsights["Application Insights 2.11.0"]
    end
    subgraph UI["Web UI"]
        Bootstrap["Bootstrap 4.3.1"]
        JQuery["jQuery 3.5.0"]
    end
    subgraph Util["Utilities"]
        Json["Newtonsoft.Json 13.0.2"]
        Config["Configuration Builders 2.0.0 beta"]
        Other["97 platform and utility packages"]
    end

    App -->|"web"| Web
    App -->|"persistence"| DB
    App -->|"identity and secrets"| Security
    App -->|"optional session support"| Cache
    App -->|"optional cloud integrations"| Cloud
    App -->|"diagnostics"| Telemetry
    App -->|"browser UI"| UI
    App -->|"configuration and runtime"| Util
```

### Dependency Summary

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web Frameworks | 16 | ASP.NET Web Forms, FriendlyUrls, Web Optimization WebForms | Legacy server-rendered web stack |
| Database / ORM | 1 | Entity Framework 6.3.0 | SQL Server provider is part of the EF package |
| Authentication / Security | 18 | OWIN, IdentityModel, Azure Key Vault | Optional OpenID Connect and externalized secrets |
| Caching / Session State | 2 | Redis Session State Provider, StackExchange.Redis | Session provider configuration is commented out |
| Storage / Messaging | 2 | WindowsAzure.Storage, WindowsAzure.ServiceBus | Azure Storage is used; no service-bus workflow was identified |
| Logging / Observability | 11 | log4net, Application Insights | Legacy telemetry libraries |
| Utilities / Platform | 97 | Newtonsoft.Json, configuration builders, System libraries | Includes many framework and platform-support packages |

### Version & Compatibility Risks

The application targets .NET Framework 4.7.2 and depends on EF6, OWIN, older IdentityModel packages, and a beta generation of configuration builders. The CVE scan matched the declared `log4net` 2.0.10 and Microsoft IdentityModel token packages to medium-severity advisories. The dependency-list CLI could not evaluate the legacy project because `Microsoft.WebApplication.targets` is absent, so the vulnerability scan used direct package-manifest entries rather than a resolved transitive tree.

### Notable Observations

- `packages.config` contains 147 entries; the project has no separate test project or test-scoped package set.
- The project declares Redis and Azure Service Bus packages, but no active cache or message-bus flow was identified in the application source/configuration.
- Application Insights, Azure Storage, Azure AD, and Key Vault integrations are optional and controlled by configuration.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| None detected | — | No test project or test-scoped packages were found in the assessed solution |

Total test-scope dependencies: 0

No test dependencies or automated test project were detected in this workspace.
