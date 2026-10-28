# Architecture Diagram

The application is a server-rendered catalog management site built with ASP.NET Web Forms on .NET Framework 4.7.2. Its catalog service can use either an in-memory mock or Entity Framework 6 backed by SQL Server LocalDB.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Browser"]
    end
    subgraph App["Application Layer - ASP.NET Web Forms"]
        Pages["Web Forms Pages"]
        Services["Catalog Services"]
        DI["Autofac Dependency Injection"]
        Telemetry["Application Insights and log4net"]
    end
    subgraph Data["Data Layer"]
        EF["Entity Framework 6"]
        LocalDB[("SQL Server LocalDB")]
        Mock["In-memory Mock Catalog"]
    end

    Browser -->|"HTTP requests"| Pages
    Pages -->|"catalog operations"| Services
    DI -.->|"registers"| Services
    Services -->|"when mock data is disabled"| EF
    EF -->|"SQL queries"| LocalDB
    Services -->|"when mock data is enabled"| Mock
    Pages -.->|"request telemetry and logs"| Telemetry
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---|---|
| Presentation | ASP.NET Web Forms | .NET Framework 4.7.2 | Server-rendered catalog pages |
| Business Logic | C# catalog services | .NET Framework 4.7.2 | Catalog queries and create, update, and delete operations |
| Dependency Injection | Autofac | 4.9.1 | Registers catalog services and data components |
| Data Access | Entity Framework | 6.2.0 | Object-relational mapping and database operations |
| Database | SQL Server LocalDB | LocalDB | Catalog item, brand, and type storage |
| Telemetry and Logging | Application Insights and log4net | Application Insights 2.9.1; log4net 2.0.10 | Request telemetry and application logging |

### Data Storage & External Services

The configured database is SQL Server LocalDB, with Entity Framework mapping catalog items, brands, and types. When `UseMockData` is enabled, catalog operations instead use an in-memory mock service; the application also includes Application Insights instrumentation. No remote service endpoint is configured in the inspected application settings.

### Key Architectural Decisions

- ASP.NET Web Forms pages access catalog operations through the `ICatalogService` abstraction.
- Autofac selects an in-memory mock or Entity Framework-backed service using the `UseMockData` setting.
- Entity Framework 6 maps catalog items, brands, and types through `CatalogDBContext`.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        CatalogPages["Catalog Web Forms Pages"]
        SitePages["Shared and informational Web Forms Pages"]
        Routes["RouteConfig and BundleConfig"]
    end
    subgraph Business["Business Logic"]
        Interface["ICatalogService"]
        CatalogService["CatalogService"]
        MockService["CatalogServiceMock"]
    end
    subgraph DataAccess["Data Access"]
        Context["CatalogDBContext"]
        Entities["CatalogItem, CatalogBrand, CatalogType"]
        Generator["CatalogItemHiLoGenerator"]
        Database[("SQL Server LocalDB")]
        MockStore["In-memory catalog data"]
    end
    subgraph Infrastructure["Infrastructure"]
        Global["Global application startup"]
        Autofac["ApplicationModule and Autofac"]
        Initializer["CatalogDBInitializer"]
        Telemetry["Application Insights and log4net"]
    end

    CatalogPages -->|"uses"| Interface
    SitePages -->|"uses shared page infrastructure"| Routes
    Interface -.->|"implemented by"| CatalogService
    Interface -.->|"implemented by"| MockService
    CatalogService -->|"queries and persists"| Context
    CatalogService -->|"generates catalog IDs"| Generator
    Context -->|"maps"| Entities
    Context -->|"executes SQL"| Database
    MockService -->|"reads and updates"| MockStore
    Global -->|"registers routes, bundles, and container"| Routes
    Global -->|"configures"| Autofac
    Autofac -->|"selects implementation by UseMockData"| Interface
    Autofac -->|"registers"| Context
    Global -->|"initializes database when mock data is disabled"| Initializer
    Global -.->|"records request telemetry and logs"| Telemetry
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| Catalog Web Forms Pages | Presentation | ASP.NET Web Forms pages | Browse and manage catalog items |
| Shared and informational Web Forms Pages | Presentation | ASP.NET Web Forms pages | Provide shared site and informational views |
| RouteConfig and BundleConfig | Presentation | Application configuration | Register URL routes and client-side bundles |
| ICatalogService | Business Logic | Service interface | Defines catalog operations used by pages |
| CatalogService | Business Logic | Service implementation | Performs catalog operations through Entity Framework |
| CatalogServiceMock | Business Logic | Mock service implementation | Provides in-memory catalog behavior |
| CatalogDBContext | Data Access | Entity Framework DbContext | Maps and queries catalog entities |
| CatalogItem, CatalogBrand, CatalogType | Data Access | Entity models | Represent catalog data |
| CatalogItemHiLoGenerator | Data Access | ID generator | Allocates catalog item identifiers |
| Global | Infrastructure | ASP.NET application class | Configures startup, dependency injection, database initialization, and request telemetry |
| ApplicationModule and Autofac | Infrastructure | Dependency injection module | Registers services and data components |
| CatalogDBInitializer | Infrastructure | Database initializer | Initializes catalog database data |
| Application Insights and log4net | Infrastructure | Telemetry and logging | Captures application telemetry and logs |
