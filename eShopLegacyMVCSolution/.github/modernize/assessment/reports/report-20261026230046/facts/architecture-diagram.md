# Architecture Diagram

The solution contains a legacy ASP.NET MVC 5 catalog application and a separately packaged ported MVC application. Both expose catalog browsing and maintenance and use either SQL Server persistence or a mock-data implementation.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        BrowserA["Browser"]
    end
    subgraph LegacyApp["Legacy Application - ASP.NET MVC 5"]
        WebA["MVC Controllers and Razor Views"]
        ApiA["ASP.NET Web API 2"]
        ServiceA["Catalog Services"]
        DataA["Entity Framework 6"]
    end
    subgraph PortedApp["Ported Application - ASP.NET Core 2.2"]
        WebB["MVC Controllers and Razor Views"]
        ServiceB["Catalog Services"]
        DataB["Entity Framework Core 2.2"]
    end
    subgraph Storage["Data Storage"]
        SqlA[("Legacy SQL Server configuration")]
        SqlB[("Ported LocalDB default")]
        MockA["In-memory Mock Catalog"]
    end
    subgraph Telemetry["Telemetry"]
        InsightsA["Application Insights"]
        LogA["log4net"]
    end

    BrowserA -->|"HTTP requests"| WebA
    BrowserA -->|"HTTP API requests"| ApiA
    WebA -->|"in-process calls"| ServiceA
    ApiA -->|"in-process calls"| ServiceA
    ServiceA -->|"ORM queries"| DataA
    DataA -->|"SQL"| SqlA
    ServiceA -->|"mock mode"| MockA
    BrowserA -->|"HTTP requests"| WebB
    WebB -->|"in-process calls"| ServiceB
    ServiceB -->|"ORM queries"| DataB
    DataB -->|"SQL"| SqlB
    ServiceB -->|"mock mode"| MockA
    WebA -.->|"telemetry and logs"| InsightsA
    WebA -.->|"application logs"| LogA
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---:|---|
| Presentation | ASP.NET MVC 5 / Razor | 5.2.7 | Server-rendered catalog pages |
| API | ASP.NET Web API 2 | 5.2.7 | Brand and file endpoints |
| Ported presentation | ASP.NET Core MVC | 2.2.0 | Ported catalog web application |
| Business | C# catalog services | .NET Framework 4.7.2 / net461 | Catalog operations and mock-data mode |
| Data access | Entity Framework 6 | 6.2.0 | Legacy SQL Server persistence |
| Ported data access | Entity Framework Core | 2.2.6 | Ported SQL Server persistence |
| Dependency injection | Autofac | 6.1.0 / 4.9.1 | Component registration |
| Observability | Application Insights and log4net | 2.9.1 / 2.0.10 | Telemetry and application logging |

### Data Storage & External Services

When mock data is disabled, both applications use SQL Server through their respective Entity Framework contexts. Mock mode serves catalog items from an in-memory list. Application Insights is configured for the legacy application; no external business APIs, message brokers, or cache service were identified.

### Key Architectural Decisions

- Controllers delegate catalog operations to the `ICatalogService` abstraction.
- Autofac selects a database-backed or mock service at startup; the mock service is registered as a singleton.
- The legacy MVC and ported applications are separate projects in the same solution and maintain distinct ORM implementations.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        CatalogUi["CatalogController"]
        PictureUi["PicController"]
        BrandApi["BrandsController"]
        FileApi["FilesController"]
    end
    subgraph Business["Business Logic"]
        CatalogSvc["CatalogService"]
        MockSvc["CatalogServiceMock"]
        Contract["ICatalogService"]
    end
    subgraph Persistence["Data Access"]
        LegacyDb["CatalogDBContext - EF6"]
        PortedDb["CatalogDBContext - EF Core"]
        HiLo["CatalogItemHiLoGenerator"]
    end
    subgraph Infrastructure["Infrastructure"]
        Di["Autofac ApplicationModule"]
        Seed["CatalogDBInitializer"]
    end

    CatalogUi -->|"uses"| Contract
    PictureUi -->|"uses"| Contract
    BrandApi -->|"uses"| Contract
    FileApi -->|"uses"| Contract
    CatalogSvc -.->|"implements"| Contract
    MockSvc -.->|"implements"| Contract
    CatalogSvc -->|"queries and updates"| LegacyDb
    CatalogSvc -->|"requests identifiers"| HiLo
    HiLo -->|"sequence query"| LegacyDb
    Seed -->|"initializes"| LegacyDb
    Di -->|"registers"| CatalogSvc
    Di -->|"registers mock service"| MockSvc
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| CatalogController | Presentation | MVC controller | Catalog browsing and item maintenance |
| PicController | Presentation | MVC controller | Reads and returns catalog pictures |
| BrandsController | Presentation | Web API controller | Reads and deletes catalog brands |
| FilesController | Presentation | Web API controller | Returns serialized brand data |
| ICatalogService | Business | Service contract | Defines catalog operations |
| CatalogService | Business | Service | Performs database-backed catalog operations |
| CatalogServiceMock | Business | Service | Serves and mutates in-memory catalog data |
| CatalogDBContext | Data access | EF6 DbContext | Persists catalog entities |
| CatalogItemHiLoGenerator | Data access | Identifier generator | Allocates catalog item identifiers |
| CatalogDBInitializer | Infrastructure | EF initializer | Creates sequences and seeds catalog data |
| ApplicationModule | Infrastructure | Autofac module | Configures service lifetimes and implementations |
