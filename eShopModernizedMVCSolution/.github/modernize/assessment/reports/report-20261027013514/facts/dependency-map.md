# Dependency Map

The eShopModernizedMVC project declares or resolves 146 NuGet package entries in `packages.config`. The diagram highlights primary libraries; the remaining entries are mainly framework support and transitive packages.

## Dependencies

```mermaid
flowchart LR
    App["eShopModernizedMVC"]
    subgraph Web["Web Frameworks"]
        MVC["Microsoft.AspNet.Mvc 5.2.7"]
        Razor["Microsoft.AspNet.Razor 3.2.7"]
        WebPages["Microsoft.AspNet.WebPages 3.2.7"]
        Owin["Microsoft.Owin 4.2.2"]
    end
    subgraph DB["Database and ORM"]
        EF["EntityFramework 6.3.0"]
        Sql["System.Data.SqlClient framework provider"]
    end
    subgraph Security["Security"]
        Oidc["Microsoft.Owin.Security.OpenIdConnect 4.0.1"]
        Identity["Microsoft.IdentityModel.Tokens 5.6.0"]
        Vault["Microsoft.Azure.KeyVault 3.0.4"]
    end
    subgraph Obs["Observability"]
        AI["Microsoft.ApplicationInsights 2.11.0"]
        Log["log4net 2.0.12"]
    end
    subgraph Util["Utilities and Storage"]
        Autofac["Autofac 4.9.4"]
        Json["Newtonsoft.Json 13.0.2"]
        Storage["WindowsAzure.Storage 9.3.3"]
        Redis["StackExchange.Redis 2.0.601"]
    end

    App -->|"web"| MVC
    App -->|"web views"| Razor
    App -->|"web support"| WebPages
    App -->|"middleware"| Owin
    App -->|"persistence"| EF
    App -->|"authentication"| Oidc
    Oidc -->|"token handling"| Identity
    App -->|"secret provider integration"| Vault
    App -->|"telemetry"| AI
    App -->|"logging"| Log
    App -->|"dependency injection"| Autofac
    App -->|"serialization support"| Json
    App -->|"image storage"| Storage
    App -.->|"optional session package"| Redis
```

### Dependency Summary

Counts below are the representative packages shown in the diagram; the full dependency inventory includes 146 entries.

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web Frameworks | 4 | ASP.NET MVC, Razor, WebPages, OWIN | Classic ASP.NET MVC 5 application |
| Database and ORM | 1 | Entity Framework 6.3.0 | SQL Server provider is provided by the .NET Framework |
| Security | 3 | OWIN OpenID Connect, IdentityModel, Azure Key Vault | Optional Azure Active Directory and Key Vault integration |
| Observability | 2 | Application Insights, log4net | Application telemetry and logging |
| Utilities and Storage | 4 | Autofac, Newtonsoft.Json, WindowsAzure.Storage, StackExchange.Redis | Redis package is present; Redis session setup is commented out |

### Version & Compatibility Risks

The project targets .NET Framework 4.7.2 and uses ASP.NET MVC 5 and Entity Framework 6, so modernization involves a legacy web stack rather than a current .NET platform. Several Azure libraries and supporting packages are older versions. The NuGet CLI dependency listing could not load Visual Studio WebApplication targets in this environment, so this inventory uses the resolved package entries in `packages.config`.

### Notable Observations

- `packages.config` contains 146 package entries, including many `System.*` and Microsoft.Extensions support packages.
- `Microsoft.Net.Compilers` is marked as a development dependency, but no test framework packages or test project were found in this solution.
- Azure Key Vault, managed identity, Blob Storage, and Application Insights integrations are configured as optional application capabilities.
- GitHub Advisory checks for the 146 package entries returned no matching advisories; this does not establish that packages are current or risk-free.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| None detected | — | No test-scoped packages or test project are declared in the solution |

Total test-scope dependencies: 0

No test dependencies were detected in `packages.config`.
