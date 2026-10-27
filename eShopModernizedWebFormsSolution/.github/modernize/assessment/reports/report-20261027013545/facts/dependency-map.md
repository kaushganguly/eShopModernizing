# Dependency Map

The eShopModernizedWebForms project has 147 package entries in `packages.config` (including framework and transitive package entries). The diagram groups the principal dependencies by function and omits low-level framework packages for readability.

## Dependencies

```mermaid
flowchart LR
    App["eShopModernizedWebForms"]
    subgraph Web["Web Frameworks"]
        WebForms["ASP.NET Web Forms .NET Framework 4.7.2"]
        OWIN["OWIN 4.0.1 to 4.2.2"]
        WebAssets["Bootstrap 4.3.1 jQuery 3.5.0"]
    end
    subgraph Database["Database and ORM"]
        EF["Entity Framework 6.3.0"]
        SQL["System.Data.SqlClient Framework provider"]
        DataServices["Microsoft.Data OData and EDM 5.8.4"]
    end
    subgraph Cloud["Cloud and Storage"]
        Blob["WindowsAzure.Storage 9.3.3"]
        Bus["WindowsAzure.ServiceBus 6.0.0"]
        Redis["StackExchange.Redis 2.0.601"]
        KeyVault["Microsoft.Azure.KeyVault 3.0.4"]
    end
    subgraph Observability["Logging and Observability"]
        Insights["Application Insights 2.11.0"]
        Log4Net["log4net 2.0.10"]
    end
    subgraph Security["Identity and Configuration"]
        Identity["Microsoft.IdentityModel 5.6.0"]
        Auth["Microsoft.Owin.Security.OpenIdConnect 4.0.1"]
        Config["Configuration Builders 2.0.0 beta"]
    end
    subgraph Utilities["Utilities"]
        Newtonsoft["Newtonsoft.Json 13.0.2"]
        Framework["94 System and BCL package entries"]
    end
    App -->|"presentation"| Web
    App -->|"data access"| Database
    App -->|"optional integrations"| Cloud
    App -->|"telemetry"| Observability
    App -->|"authentication and secrets"| Security
    App -->|"serialization and runtime support"| Utilities
```

### Dependency Summary

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web Frameworks | 8 | ASP.NET Web Forms, OWIN, Bootstrap, jQuery | Legacy server-rendered framework and browser assets |
| Database / ORM | 5 | Entity Framework 6.3.0, Microsoft.Data EDM/OData | SQL Server provider is part of the target framework |
| Cloud / Storage | 8 | Azure Storage, Key Vault, Service Bus, Redis | Several are optional or not currently enabled |
| Logging / Observability | 11 | Application Insights, log4net | Includes telemetry modules and collectors |
| Security / Identity | 21 | OWIN authentication, IdentityModel, configuration builders | Includes Azure AD and Key Vault integration packages |
| Utilities | 94 | Newtonsoft.Json and System/BCL packages | Primarily framework support and transitive packages |

### Version & Compatibility Risks

The application targets .NET Framework 4.7.2 and uses legacy Web Forms, Entity Framework 6, OWIN, and older Azure SDK packages. The package list contains multiple older framework-support packages and a prerelease 2.0.0-beta Configuration Builders family; compatibility should be checked when modernizing the target runtime or Azure integrations.

### Notable Observations

- `packages.config` lists installed packages, including transitive dependencies; the 147 entries are not all direct references.
- Azure Blob Storage, Azure AD, Redis session support, and Service Bus packages are present, but not all are enabled or used by the current default configuration.
- Entity Framework 6.3.0 is the only ORM package family; no EF Core dependency is declared.
- No test-scoped packages or test project were found in this solution.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| None detected | — | No test framework packages or test project found |

Total test-scope dependencies: 0

No test dependencies were detected in the solution package inventory.
