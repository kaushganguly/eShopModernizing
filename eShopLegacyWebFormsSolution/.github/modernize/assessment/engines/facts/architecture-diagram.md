# Architecture Diagram

The application is a single ASP.NET Web Forms site for browsing and maintaining a product catalog. It runs with in-memory mock catalog data by default, with an Entity Framework and SQL Server LocalDB path available when mock mode is disabled.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web browser"]
    end
    subgraph Presentation["Presentation Layer"]
        Pages["ASP.NET Web Forms pages"]
    end
    subgraph Application["Application Layer"]
        Services["Catalog service"]
        Mock["In-memory catalog mock"]
        Telemetry["Application Insights modules"]
    end
    subgraph Data["Data Layer"]
        Context["Entity Framework 6 DbContext"]
        Database[("SQL Server LocalDB")]
    end

    Browser -->|"HTTP requests and postbacks"| Pages
    Pages -->|"catalog operations"| Services
    Services -->|"UseMockData enabled"| Mock
    Services -->|"UseMockData disabled"| Context
    Context -->|"queries and updates"| Database
    Pages -->|"request telemetry"| Telemetry
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---|---|
| Presentation | ASP.NET Web Forms on .NET Framework | 4.7.2 | Server-rendered catalog pages and postbacks |
| Dependency injection | Autofac and Autofac.Web | 4.9.1 / 4.0.0 | Register catalog services and Web Forms integration |
| Data access | Entity Framework 6 | 6.2.0 | Catalog persistence through `CatalogDBContext` |
| Database | SQL Server LocalDB | LocalDB | Catalog storage when mock mode is disabled |
| Observability | Application Insights | 2.9.1 | Web request and application telemetry |
| Logging | log4net | 2.0.10 | Application logging |

### Data Storage & External Services

The configured database is SQL Server LocalDB. `UseMockData` defaults to `true`, so the default runtime path uses a shared in-memory catalog; setting it to `false` activates Entity Framework persistence. Application Insights is the only identified external telemetry integration; no external business APIs or message brokers were found.

### Key Architectural Decisions

- Autofac registers the catalog service per request for database mode and as a singleton in mock mode.
- Entity Framework 6 maps catalog items, brands, and types to SQL Server tables.
- ASP.NET Web Forms handles both page rendering and catalog form submissions.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        DefaultPage["Default page"]
        CreatePage["Catalog create page"]
        EditPage["Catalog edit page"]
        DetailsPage["Catalog details page"]
        DeletePage["Catalog delete page"]
    end
    subgraph Business["Business Logic"]
        CatalogService["CatalogService"]
        MockService["CatalogServiceMock"]
        Contract["ICatalogService"]
    end
    subgraph DataAccess["Data Access"]
        DbContext["CatalogDBContext"]
        IdGenerator["CatalogItemHiLoGenerator"]
        Store["SQL Server LocalDB"]
    end
    subgraph CrossCutting["Infrastructure"]
        Autofac["Autofac module"]
        Insights["Application Insights modules"]
    end

    DefaultPage -->|"browse and paginate"| Contract
    CreatePage -->|"create item"| Contract
    EditPage -->|"find and update"| Contract
    DetailsPage -->|"find item"| Contract
    DeletePage -->|"find and remove"| Contract
    Contract -->|"database implementation"| CatalogService
    Contract -->|"mock implementation"| MockService
    CatalogService -->|"queries and updates"| DbContext
    CatalogService -->|"allocate catalog ID"| IdGenerator
    IdGenerator -->|"SQL sequence"| DbContext
    DbContext -->|"Entity Framework"| Store
    Autofac -.->|"registers implementations"| Contract
    Insights -.->|"observes web requests"| Presentation
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| Default page | Presentation | Web Forms page | Lists and paginates catalog items |
| Create, Edit, Details, Delete pages | Presentation | Web Forms pages | Perform catalog item CRUD interactions |
| `ICatalogService` | Business Logic | Service interface | Defines catalog operations used by pages |
| `CatalogService` | Business Logic | Service | Performs catalog reads and writes through EF6 |
| `CatalogServiceMock` | Business Logic | Mock service | Provides mutable in-memory catalog operations |
| `CatalogDBContext` | Data Access | EF6 DbContext | Maps catalog entities to SQL Server |
| `CatalogItemHiLoGenerator` | Data Access | ID generator | Allocates catalog IDs from a SQL sequence |
| `ApplicationModule` | Infrastructure | Autofac module | Selects and registers catalog implementations |
