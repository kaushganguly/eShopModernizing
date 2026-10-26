# Architecture Diagram

This solution contains a legacy ASP.NET MVC 5 application and an ASP.NET Core 2.2 ported application, both centered on a catalog service. The projects use Entity Framework against SQL Server and Autofac for dependency injection.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Browser"]
        ApiClient["HTTP API Client"]
    end
    subgraph Legacy["Legacy Application - ASP.NET MVC 5"]
        MvcUi["MVC Controllers and Razor Views"]
        WebApi["ASP.NET Web API 2"]
        LegacyService["Catalog Service"]
        Ef6["Entity Framework 6"]
    end
    subgraph Ported["Ported Application - ASP.NET Core 2.2"]
        CoreUi["MVC Controllers and Razor Views"]
        CoreApi["ASP.NET Core API Controllers"]
        CoreService["Catalog Service"]
        EfCore["Entity Framework Core 2.2"]
    end
    subgraph Storage["Data Storage"]
        SqlServer[("SQL Server Catalog Database")]
        LocalDb[("SQL Server LocalDB")]
    end
    subgraph CrossCutting["Cross-Cutting Services"]
        Autofac["Autofac Dependency Injection"]
        Telemetry["Application Insights and log4net"]
    end

    Browser -->|"HTTP pages and images"| MvcUi
    ApiClient -->|"HTTP API requests"| WebApi
    MvcUi -->|"catalog operations"| LegacyService
    WebApi -->|"catalog operations"| LegacyService
    LegacyService -->|"queries and updates"| Ef6
    Ef6 -->|"SQL"| LocalDb
    Browser -->|"HTTP pages and images"| CoreUi
    ApiClient -->|"HTTP API requests"| CoreApi
    CoreUi -->|"catalog operations"| CoreService
    CoreApi -->|"catalog operations"| CoreService
    CoreService -->|"queries and updates"| EfCore
    EfCore -->|"SQL"| SqlServer
    Autofac -.->|"registers application services"| Legacy
    Autofac -.->|"registers application services"| Ported
    Telemetry -.->|"application telemetry and logs"| Legacy
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---|---|
| Presentation | ASP.NET MVC | 5.2.7 | Server-rendered catalog pages |
| API | ASP.NET Web API | 2 | HTTP endpoints for catalog brands and files |
| Ported application | ASP.NET Core MVC | 2.2 | ASP.NET Core version of the catalog application |
| Business logic | Catalog service and Autofac | Autofac 4.x | Catalog operations and dependency registration |
| Data access | Entity Framework | 6.2.0 | Catalog persistence in the legacy application |
| Data access | Entity Framework Core | 2.2.6 | Catalog persistence in the ported application |
| Data storage | SQL Server / LocalDB | Configured locally | Catalog and lookup data |
| Diagnostics | Application Insights and log4net | Package versions in project | Application telemetry and logging |

### Data Storage & External Services

The legacy MVC application configures a SQL Server LocalDB connection in `Web.config`; the ported application configures SQL Server through Entity Framework Core and `DefaultConnection` settings. Both persist catalog items, brands, and types. Application Insights and log4net provide diagnostics; no additional external APIs, caches, or message brokers were identified.

### Key Architectural Decisions

- The MVC 5 and ASP.NET Core projects use separate service and persistence implementations while modeling the same catalog domain.
- Autofac modules select either mock catalog data or database-backed services from application configuration.
- The legacy application combines MVC pages and Web API endpoints in one web application.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        CatalogMvc["CatalogController"]
        PicMvc["PicController"]
        BrandApi["BrandsController"]
        FilesApi["FilesController"]
        CoreCatalog["Ported CatalogController"]
    end
    subgraph Business["Business Logic"]
        CatalogService["CatalogService"]
        MockService["CatalogServiceMock"]
        CatalogContract["ICatalogService"]
    end
    subgraph DataAccess["Data Access"]
        LegacyContext["CatalogDBContext EF6"]
        CoreContext["CatalogDBContext EF Core"]
        Hilo["CatalogItemHiLoGenerator"]
    end
    subgraph Infrastructure["Infrastructure"]
        LegacyModule["ApplicationModule"]
        CoreModule["ApplicationModule"]
        Startup["MvcApplication and Startup"]
        TelemetryInfra["Logging and telemetry"]
    end

    CatalogMvc -->|"uses"| CatalogContract
    PicMvc -->|"uses"| CatalogContract
    BrandApi -->|"uses"| CatalogContract
    FilesApi -->|"uses"| CatalogContract
    CoreCatalog -->|"uses"| CatalogContract
    CatalogContract -->|"implemented by"| CatalogService
    CatalogContract -->|"implemented by"| MockService
    CatalogService -->|"reads and writes"| LegacyContext
    CatalogService -->|"allocates item identifiers"| Hilo
    CoreContext -->|"maps catalog entities"| CatalogService
    LegacyModule -.->|"registers implementations"| CatalogService
    CoreModule -.->|"registers implementation"| CatalogService
    Startup -.->|"configures dependency injection"| LegacyModule
    Startup -.->|"configures diagnostics"| TelemetryInfra
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| CatalogController | Presentation | MVC controller | Displays and updates catalog items |
| PicController | Presentation | MVC controller | Serves catalog item pictures |
| BrandsController | Presentation | Web API controller | Exposes catalog brand data |
| FilesController | Presentation | Web API controller | Exposes catalog item files |
| CatalogService | Business Logic | Service | Retrieves and changes catalog records |
| CatalogServiceMock | Business Logic | Mock service | Provides in-memory catalog data when enabled |
| ICatalogService | Business Logic | Service interface | Defines catalog operations used by controllers |
| CatalogDBContext | Data Access | EF6 / EF Core context | Maps catalog items, brands, and types |
| CatalogItemHiLoGenerator | Data Access | Identifier generator | Allocates catalog item identifiers |
| ApplicationModule | Infrastructure | Autofac module | Registers catalog services and database components |
| MvcApplication / Startup | Infrastructure | Application startup | Configures routes, dependency injection, and request pipeline |
| Logging and telemetry | Infrastructure | Cross-cutting services | Captures application logs and telemetry |
