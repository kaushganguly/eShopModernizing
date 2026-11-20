# Dependency Map

The `eShopModernizedWebForms` project has 147 package entries in `packages.config`. The legacy project cannot resolve a complete dependency graph in this environment, so this map reports the declared package inventory and groups less central packages.

## Dependencies

```mermaid
flowchart LR
    App["eShopModernizedWebForms"]
    subgraph Web["Web and UI"]
        WebStack["ASP.NET Web Forms and optimization packages"]
        UI["Bootstrap 4.3.1, jQuery 3.5.0, Modernizr 2.8.3"]
        DI["Autofac 4.9.4 and Autofac.Web 4.0.0"]
    end
    subgraph Data["Database, Cloud, and Messaging"]
        EF["EntityFramework 6.3.0"]
        Azure["Azure Key Vault, Storage, and App Authentication packages"]
        Redis["StackExchange.Redis 2.0.601"]
        Bus["WindowsAzure.ServiceBus 6.0.0"]
    end
    subgraph Security["Identity and Security"]
        Owin["OWIN Security and OpenID Connect packages"]
        Identity["Microsoft IdentityModel 5.x packages"]
    end
    subgraph Telemetry["Observability and Logging"]
        Insights["Application Insights 2.11.x packages"]
        Log4Net["log4net 2.0.10 and Azure appender 1.4.3.0"]
    end
    subgraph Utilities["Utilities and Framework Packages"]
        Other["85 other packages, including System and Microsoft.Extensions packages"]
    end
    App -->|"presentation"| Web
    App -->|"data and integrations"| Data
    App -->|"authentication"| Security
    App -->|"telemetry"| Telemetry
    App -->|"supporting libraries"| Utilities
```

### Dependency Summary

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web and UI | 21 | ASP.NET Web Forms, Autofac, Bootstrap, jQuery | Includes web UI and framework package entries |
| Database, cloud, and messaging | 13 | Entity Framework, Azure Storage, Key Vault, Redis, Windows Azure Service Bus | Optional services are present in configuration/packages |
| Identity and security | 15 | OWIN Security, OpenID Connect, Microsoft IdentityModel | Supports optional Azure AD authentication |
| Observability and logging | 12 | Application Insights, log4net | Telemetry and logging integrations |
| Utilities and framework | 86 | System packages, Microsoft.Extensions, compiler and supporting libraries | Aggregated for readability; exact entries remain in `packages.config` |

### Version & Compatibility Risks

The application targets .NET Framework 4.7.2 and uses the legacy packages.config format. Entity Framework 6.3.0, OWIN, and several Azure integration packages are older generations; compatibility and support should be checked when upgrading the application platform.

### Notable Observations

- `dotnet list package --include-transitive --format json` could not evaluate the project because Visual Studio WebApplication MSBuild targets are unavailable; versions and counts here come from `packages.config`, not a resolved transitive graph.
- The package inventory includes Azure Blob Storage, Key Vault, Redis session-state, and Service Bus packages, while not all are enabled in the default runtime configuration.
- No test-scoped packages or test project were identified.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| None detected | — | No test project or test framework package was found in the solution |

Total test-scope dependencies: 0

No test dependencies detected; automated test infrastructure is not present in the assessed solution.
