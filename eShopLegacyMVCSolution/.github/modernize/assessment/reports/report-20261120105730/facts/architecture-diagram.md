# Architecture Diagram

The solution contains two catalog web applications and a shared serialization utility. The legacy MVC application and its ASP.NET Core port are separate deployables with their own persistence configuration.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Browser"]
        ApiClient["HTTP API Client"]
    end
    subgraph Legacy["Legacy Application - ASP.NET MVC 5"]
        LegacyMvc["MVC Views and Web API"]
        LegacyServices["Catalog Services"]
        LegacyOrm["Entity Framework 6"]
    end
    subgraph Ported["Ported Application - ASP.NET Core MVC"]
        PortedMvc["MVC Views and API Controllers"]
        PortedServices["Catalog Services"]
        PortedOrm["Entity Framework Core"]
    end
    subgraph Data["Data Stores"]
        LegacyDb[("SQL Server LocalDB - legacy catalog")]
        PortedDb[("SQL Server LocalDB - ported catalog")]
        Images[("Local product image files")]
    end
    subgraph CrossCutting["Cross-Cutting"]
        Telemetry["Application Insights and log4net"]
        Serialization["Shared Binary Serialization Utility"]
    end

    Browser -->|"HTTP requests"| LegacyMvc
    Browser -->|"HTTP requests"| PortedMvc
    ApiClient -->|"HTTP API requests"| LegacyMvc
    ApiClient -->|"HTTP API requests"| PortedMvc
    LegacyMvc -->|"catalog operations"| LegacyServices
    PortedMvc -->|"catalog operations"| PortedServices
    LegacyServices -->|"queries and updates"| LegacyOrm
    PortedServices -->|"queries and updates"| PortedOrm
    LegacyOrm -->|"EF6 persistence"| LegacyDb
    PortedOrm -->|"EF Core persistence"| PortedDb
    LegacyMvc -->|"serves catalog pictures"| Images
    PortedMvc -->|"serves catalog pictures"| Images
    LegacyMvc -->|"binary brand response"| Serialization
    PortedMvc -->|"binary brand response"| Serialization
    LegacyMvc -.->|"telemetry and logging"| Telemetry
    PortedMvc -.->|"logging"| Telemetry
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---:|---|
| Legacy presentation | ASP.NET MVC and Web API | MVC 5.2.7; Web API 5.2.7 | Server-rendered catalog pages and HTTP API |
| Legacy runtime | .NET Framework | 4.7.2 | Hosts the legacy MVC application |
| Ported presentation | ASP.NET Core MVC | 2.2.0 | Ported catalog pages and API controllers |
| Ported runtime | .NET Framework | 4.6.1 | Target framework declared by the ported project |
| Dependency injection | Autofac | 6.1.0 legacy; 4.9.1 ported | Registers catalog services and application dependencies |
| Legacy persistence | Entity Framework 6 | 6.2.0 | Maps catalog entities to SQL Server |
| Ported persistence | Entity Framework Core | 2.2.6 | Maps catalog entities and migrations to SQL Server |
| Observability | Application Insights, log4net | 2.9.1; 2.0.10 | Legacy telemetry and application logging |
| Shared utility | BinaryFormatter serialization | Framework API | Serializes brand DTO collections for the files API |

### Data Storage & External Services

Both applications are configured for SQL Server LocalDB with integrated Windows authentication, but use separate database names and persistence frameworks. Catalog pictures are read from local image files. No message broker, cache, external business API, or service-discovery component was identified; Application Insights and log4net provide telemetry/logging integration.

### Key Architectural Decisions

- The ported application is a parallel implementation rather than a downstream service; there is no cross-application API or shared database access path.
- `ICatalogService` isolates MVC and API controllers from either EF-backed catalog operations or the optional mock implementation.
- Autofac provides module-based dependency registration; `UseMockData` selects the in-memory service mode.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        LegacyCatalog["Legacy CatalogController"]
        LegacyPicture["Legacy PicController"]
        LegacyApi["Legacy Brands and Files API"]
        PortedCatalog["Ported CatalogController"]
        PortedPicture["Ported PicController"]
        PortedApi["Ported Brands and Files API"]
    end
    subgraph Business["Business Logic"]
        LegacyService["Legacy ICatalogService"]
        PortedService["Ported ICatalogService"]
        LegacyMock["Legacy CatalogServiceMock"]
        PortedMock["Ported CatalogServiceMock"]
    end
    subgraph DataAccess["Data Access"]
        LegacyContext["EF6 CatalogDBContext"]
        PortedContext["EF Core CatalogDBContext"]
        LegacySql[("Legacy LocalDB")]
        PortedSql[("Ported LocalDB")]
    end
    subgraph Infrastructure["Infrastructure"]
        Autofac["Autofac Modules"]
        BinaryUtil["Shared Serialization Utility"]
        Logging["log4net and Application Insights"]
    end

    LegacyCatalog -->|"uses"| LegacyService
    LegacyPicture -->|"looks up catalog item"| LegacyService
    LegacyApi -->|"uses"| LegacyService
    PortedCatalog -->|"uses"| PortedService
    PortedPicture -->|"looks up catalog item"| PortedService
    PortedApi -->|"uses"| PortedService
    LegacyService -->|"queries and updates"| LegacyContext
    PortedService -->|"queries and updates"| PortedContext
    LegacyContext -->|"persists"| LegacySql
    PortedContext -->|"persists"| PortedSql
    LegacyService -.->|"mock mode"| LegacyMock
    PortedService -.->|"mock mode"| PortedMock
    Autofac -.->|"registers"| LegacyService
    Autofac -.->|"registers"| PortedService
    LegacyApi -->|"serializes brand list"| BinaryUtil
    PortedApi -->|"serializes brand list"| BinaryUtil
    LegacyCatalog -.->|"logs and traces"| Logging
    PortedCatalog -.->|"logs"| Logging
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| Legacy CatalogController | Presentation | MVC Controller | Lists and manages catalog items |
| Legacy PicController | Presentation | MVC Controller | Returns an image for a catalog item |
| Legacy BrandsController | Presentation | Web API Controller | Reads catalog brands and handles a demo delete route |
| Legacy FilesController | Presentation | Web API Controller | Returns serialized brand DTOs |
| Ported CatalogController | Presentation | MVC Controller | Ported catalog page and CRUD flow |
| Ported BrandsController | Presentation | API Controller | Returns brands and handles a demo delete route |
| Ported FilesController | Presentation | API Controller | Returns serialized brand DTOs |
| ICatalogService | Business Logic | Service Interface | Defines catalog item, brand, and type operations |
| CatalogService | Business Logic | EF-backed Service | Executes catalog queries and updates |
| CatalogServiceMock | Business Logic | In-memory Service | Provides catalog operations over a mutable in-memory list |
| CatalogDBContext | Data Access | EF DbContext | Maps catalog item, brand, and type data |
| ApplicationModule | Infrastructure | Autofac Module | Selects and registers services based on mock-data configuration |
| Serializing | Infrastructure | Utility | Serializes brand DTOs with BinaryFormatter |
| CatalogDBInitializer | Infrastructure | EF6 Initializer | Initializes legacy catalog sequences and optional seed data |
