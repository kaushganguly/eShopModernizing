# Dependency Map

The application declares 147 NuGet package entries in `src/eShopModernizedWebForms/packages.config`; the map highlights the key runtime libraries and groups the remaining packages as supporting utilities and framework compatibility packages.

## Dependencies

```mermaid
flowchart LR
    App["eShopModernizedWebForms"]
    subgraph Web["Web Frameworks"]
        WebForms["ASP.NET Web Forms and System.Web"]
        Optimization["Microsoft.AspNet.Web.Optimization 1.1.3"]
        Friendly["Microsoft.AspNet.FriendlyUrls 1.0.2"]
        Autofac["Autofac 4.9.4 and Autofac.Web 4.0.0"]
    end
    subgraph Data["Database and ORM"]
        EF["EntityFramework 6.3.0"]
        Sql["System.Data.SqlClient provider"]
    end
    subgraph Cloud["Cloud and Storage"]
        AzureStorage["WindowsAzure.Storage 9.3.3"]
        KeyVault["Microsoft.Azure.KeyVault 3.0.4"]
        ManagedIdentity["Microsoft.Azure.Services.AppAuthentication 1.3.1"]
        Redis["Microsoft.Web.RedisSessionStateProvider 4.0.1"]
    end
    subgraph Security["Security"]
        Owin["Microsoft.Owin.Security and OpenIdConnect 4.0.1"]
        IdentityModel["Microsoft.IdentityModel packages 5.6.0"]
    end
    subgraph Observability["Logging and Observability"]
        Log4Net["log4net 2.0.10"]
        Insights["Microsoft.ApplicationInsights 2.11.x"]
    end
    subgraph Frontend["Frontend and Utilities"]
        JQuery["jQuery 3.5.0"]
        Bootstrap["Bootstrap 4.3.1"]
        Utils["Remaining framework and utility packages"]
    end
    App -->|"web"| Web
    App -->|"persistence"| Data
    App -->|"cloud integrations"| Cloud
    App -->|"authentication"| Security
    App -->|"telemetry"| Observability
    App -->|"client assets and support"| Frontend
```

### Dependency Summary

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web Frameworks | 4 key packages | System.Web, Web Optimization, FriendlyUrls, Autofac | ASP.NET Web Forms on .NET Framework |
| Database / ORM | 1 key package | Entity Framework 6.3.0 | SQL Server access via configured connection factory |
| Cloud / Storage / Session | 4 key packages | Azure Storage, Key Vault, App Authentication, Redis provider | Optional services selected by configuration |
| Security | 2 key groups | OWIN OpenID Connect, IdentityModel | OIDC and token handling |
| Logging / Observability | 2 key groups | log4net, Application Insights | Application logging and telemetry |
| Frontend / Utilities | 134 supporting entries | jQuery, Bootstrap, Antlr, System.* packages | Includes framework support and transitive-style package entries from `packages.config` |

### Version & Compatibility Risks

The application targets .NET Framework 4.7.2 and uses legacy Web Forms, OWIN, and `packages.config` package management. The dependency set includes older versions of Entity Framework, Azure libraries, Application Insights, and authentication components; migration to modern .NET requires a framework and application-model transition rather than a simple package update.

### Notable Observations

- The package manifest includes many `System.*` and Microsoft.Extensions compatibility packages, increasing upgrade coordination effort.
- Redis session and Azure storage are optional runtime integrations; their packages are declared even when mock/local modes are configured.
- WindowsAzure.ServiceBus is declared, but no active application use was identified in the inspected source.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| None detected | — | No test-scoped dependencies are declared in the application `packages.config`. |

Total test-scope dependencies: 0

No test framework or integration-test package is declared in this project manifest.
