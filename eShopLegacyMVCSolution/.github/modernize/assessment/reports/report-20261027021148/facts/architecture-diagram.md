# Architecture Diagram

The solution contains a legacy ASP.NET MVC application and a separately implemented ASP.NET Core catalog application. Both expose catalog browsing and management, and can use SQL Server LocalDB or in-memory mock data.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Browser or API Client"]
    end
    subgraph Legacy["Legacy Application"]
        LegacyMvc["ASP.NET MVC 5 and Web API"]
        LegacyService["Catalog Service"]
        LegacyData["Entity Framework 6"]
    end
    subgraph Ported["Ported Application"]
        PortedMvc["ASP.NET Core MVC and API"]
        PortedService["Catalog Service"]
        PortedData["Entity Framework Core"]
    end
    subgraph Storage["Data and Files"]
        LocalDb[("SQL Server LocalDB")]
        MockData["In-memory Mock Catalog"]
        Pictures["Local Product Pictures"]
    end
    Browser -->|"HTTP requests"| LegacyMvc
    Browser -->|"HTTP requests"| PortedMvc
    LegacyMvc -->|"catalog operations"| LegacyService
    PortedMvc -->|"catalog operations"| PortedService
    LegacyService -->|"database queries"| LegacyData
    PortedService -->|"database queries"| PortedData
    LegacyData -->|"SQL"| LocalDb
    PortedData -->|"SQL"| LocalDb
    LegacyService -.->|"mock mode"| MockData
    PortedService -.->|"mock mode"| MockData
    LegacyMvc -->|"load catalog image"| Pictures
    PortedMvc -->|"load catalog image"| Pictures
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---:|---|
| Legacy presentation | ASP.NET MVC and Web API | MVC 5.2.7 | Server-rendered catalog pages and REST-style endpoints |
| Ported presentation | ASP.NET Core MVC | 2.2.0 | Second catalog web application |
| Legacy data | Entity Framework 6 | 6.2.0 | Catalog persistence |
| Ported data | Entity Framework Core and SQL Server provider | 2.2.6 | Catalog persistence and migrations |
| Dependency injection | Autofac | 4.9.1 | Service and database component registration |
| Runtime | .NET Framework | 4.7.2 / 4.6.1 | Legacy MVC and ported project target frameworks |
| Logging and telemetry | log4net and Application Insights | 2.0.10 / 2.9.1 | Application logging and telemetry |

### Data Storage & External Services

Both implementations configure SQL Server LocalDB for catalog data. Their mock-data modes instead use an in-memory catalog; no remote API, message broker, cache, or other independently deployed service was identified. Product pictures are read from local application files.

### Key Architectural Decisions

- Controllers depend on `ICatalogService`; the concrete service uses EF while mock mode registers an in-memory implementation.
- The legacy and ported projects maintain separate EF contexts and mappings for the same catalog domain.
- Autofac provides module-based dependency registration in both applications.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        CatalogController["CatalogController"]
        PicController["PicController"]
        BrandsController["BrandsController"]
        FilesController["FilesController"]
    end
    subgraph Business["Business Logic"]
        Contract["ICatalogService"]
        DbService["CatalogService"]
        MockService["CatalogServiceMock"]
    end
    subgraph Persistence["Data Access"]
        LegacyContext["CatalogDBContext with EF6"]
        PortedContext["CatalogDBContext with EF Core"]
        Database[("Catalog database")]
        MockCatalog["Mutable mock catalog list"]
    end
    subgraph CrossCutting["Cross Cutting"]
        Autofac["Autofac registration module"]
        Logger["log4net logging"]
    end
    CatalogController -->|"uses"| Contract
    PicController -->|"finds catalog item"| Contract
    BrandsController -->|"reads brands"| Contract
    FilesController -->|"reads brands"| Contract
    Contract -->|"database mode"| DbService
    Contract -->|"mock mode"| MockService
    DbService -->|"queries and saves"| LegacyContext
    DbService -->|"queries and saves"| PortedContext
    LegacyContext -->|"SQL"| Database
    PortedContext -->|"SQL"| Database
    MockService -->|"reads and updates"| MockCatalog
    Autofac -.->|"registers"| Contract
    Logger -.->|"records activity"| CatalogController
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| CatalogController | Presentation | MVC controller | Displays and edits catalog items |
| PicController | Presentation | MVC controller | Returns a catalog item's picture |
| BrandsController | Presentation | API controller | Lists, looks up, and demonstrates deletion of brands |
| FilesController | Presentation | API controller | Returns brand data serialized for the legacy-compatible files route |
| ICatalogService | Business Logic | Service contract | Separates controllers from catalog access |
| CatalogService | Business Logic | Data-backed service | Reads and changes catalog records |
| CatalogServiceMock | Business Logic | Mock service | Supplies and updates in-memory catalog data |
| CatalogDBContext | Data Access | EF context | Maps catalog entities to SQL Server |
| ApplicationModule | Infrastructure | Autofac module | Registers the selected service and data components |
