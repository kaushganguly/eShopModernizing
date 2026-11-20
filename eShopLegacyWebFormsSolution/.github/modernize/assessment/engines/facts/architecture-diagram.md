# Architecture Diagram

This application presents the eShop catalog through server-rendered Web Forms pages. Catalog operations use either an in-memory mock service or Entity Framework backed by SQL Server LocalDB.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Browser"]
    end
    subgraph Presentation["Presentation Layer"]
        Pages["ASP.NET Web Forms Pages"]
        Routing["ASP.NET Routing"]
    end
    subgraph Application["Application Layer - .NET Framework 4.7.2"]
        ServiceContract["ICatalogService"]
        RealService["CatalogService"]
        MockService["CatalogServiceMock"]
        DependencyInjection["Autofac"]
    end
    subgraph Data["Data Layer"]
        EntityFramework["Entity Framework 6"]
        Context["CatalogDBContext"]
        LocalDb[("SQL Server LocalDB")]
        MockList[("In-memory catalog list")]
    end
    subgraph CrossCutting["Cross-cutting"]
        Logging["log4net"]
        Telemetry["Application Insights modules"]
    end

    Browser -->|"HTTP page requests and form posts"| Routing
    Routing -->|"selects page"| Pages
    Pages -->|"catalog operations"| ServiceContract
    DependencyInjection -->|"registers real or mock service"| ServiceContract
    ServiceContract -->|"database mode"| RealService
    ServiceContract -->|"mock mode"| MockService
    RealService -->|"queries and saves"| Context
    Context -->|"ORM operations"| EntityFramework
    EntityFramework -->|"SQL"| LocalDb
    MockService -->|"reads and updates"| MockList
    Pages -.->|"request logging"| Logging
    Pages -.->|"instrumentation modules"| Telemetry
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---|---|
| Presentation | ASP.NET Web Forms | .NET Framework 4.7.2 | Server-rendered catalog pages and form workflows |
| Application | C# and Autofac | Autofac 4.9.1 | Catalog service abstraction and dependency registration |
| Data access | Entity Framework 6 | 6.2.0 | Catalog persistence and entity mapping |
| Storage | SQL Server LocalDB | Local instance | Catalog database when mock mode is disabled |
| Logging | log4net | 2.0.10 | Rolling-file and request logging |
| Telemetry | Application Insights | 2.9.1 family | ASP.NET and dependency telemetry modules |

### Data Storage & External Services

When mock mode is disabled, Entity Framework uses the configured SQL Server LocalDB connection. The project does not define remote APIs, message brokers, or a cache. Application Insights libraries and telemetry modules are present, but no instrumentation key or connection string is configured in the inspected application settings.

### Key Architectural Decisions

- Autofac binds `ICatalogService` to either the database service or the mock service based on `UseMockData`.
- The database path uses an Entity Framework context and a startup initializer to create and seed catalog data.
- The mock service is registered as a singleton and keeps a mutable in-memory catalog list shared across requests.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        CatalogPages["Catalog and home pages"]
        RouteConfig["RouteConfig"]
    end
    subgraph Business["Business Logic"]
        Contract["ICatalogService"]
        CatalogService["CatalogService"]
        MockService["CatalogServiceMock"]
        HiLo["CatalogItemHiLoGenerator"]
    end
    subgraph Persistence["Data Access"]
        Context["CatalogDBContext"]
        Entities["CatalogItem, CatalogBrand, CatalogType"]
        Database[("SQL Server LocalDB")]
        Seed["CatalogDBInitializer"]
    end
    subgraph Infrastructure["Infrastructure"]
        Autofac["ApplicationModule"]
        Log4Net["log4net"]
    end

    CatalogPages -->|"calls"| Contract
    RouteConfig -->|"maps page routes"| CatalogPages
    Autofac -.->|"registers implementation"| CatalogService
    Autofac -.->|"registers mock implementation"| MockService
    CatalogService -->|"queries and persists"| Context
    CatalogService -->|"generates catalog IDs"| HiLo
    Context -->|"maps"| Entities
    Context -->|"reads and writes"| Database
    Seed -->|"creates and seeds"| Context
    MockService -->|"uses in-memory list"| Entities
    CatalogPages -.->|"writes request events"| Log4Net
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| Catalog and home pages | Presentation | Web Forms pages | Browse and manage catalog items |
| RouteConfig | Presentation | Route registration | Maps friendly routes to pages |
| ICatalogService | Business Logic | Service interface | Defines catalog operations |
| CatalogService | Business Logic | EF-backed service | Reads and updates persisted catalog data |
| CatalogServiceMock | Business Logic | Mock service | Serves and mutates in-memory catalog data |
| CatalogItemHiLoGenerator | Business Logic | ID generator | Allocates catalog item identifiers |
| CatalogDBContext | Data Access | Entity Framework context | Exposes catalog entity sets and mappings |
| CatalogDBInitializer | Data Access | Database initializer | Creates database objects and seeds catalog data |
| ApplicationModule | Infrastructure | Autofac module | Selects and registers application services |
| log4net | Infrastructure | Logging library | Records request and application log events |
