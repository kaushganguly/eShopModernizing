# Architecture Diagram

The solution contains a classic ASP.NET MVC application and a separate ASP.NET Core 2.2 port, both implementing catalog browsing and management with SQL Server-backed persistence.

## Application Architecture

```mermaid
flowchart TD
    subgraph Clients["Client Layer"]
        Browser["Browser"]
    end
    subgraph Legacy["Legacy Web Application - .NET Framework 4.7.2"]
        Mvc["ASP.NET MVC 5"]
        WebApi["ASP.NET Web API 2"]
        Service["Catalog service"]
        Ef6["Entity Framework 6"]
        Autofac["Autofac dependency injection"]
    end
    subgraph Ported["Ported Web Application - ASP.NET Core 2.2"]
        CoreMvc["ASP.NET Core MVC"]
        CoreService["Catalog service"]
        EfCore["Entity Framework Core"]
    end
    subgraph Data["Data Layer"]
        SqlServer[("SQL Server LocalDB")]
    end

    Browser -->|"HTTP pages and API requests"| Mvc
    Browser -->|"HTTP API requests"| WebApi
    Browser -->|"HTTP requests"| CoreMvc
    Mvc -->|"catalog operations"| Service
    WebApi -->|"catalog operations"| Service
    Service -->|"Entity Framework queries"| Ef6
    CoreMvc -->|"catalog operations"| CoreService
    CoreService -->|"Entity Framework queries"| EfCore
    Ef6 -->|"SQL"| SqlServer
    EfCore -->|"SQL"| SqlServer
    Autofac -.->|"registers MVC and Web API services"| Service
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---:|---|
| Legacy presentation | ASP.NET MVC | 5.2.7 | Server-rendered catalog pages |
| Legacy API | ASP.NET Web API | 5.2.7 | Brand and file API endpoints |
| Legacy runtime | .NET Framework | 4.7.2 | Hosts the classic MVC application |
| Ported presentation | ASP.NET Core MVC | 2.2.0 | Server-rendered catalog pages in the port |
| Dependency injection | Autofac | 4.9.1 | Registers controllers and catalog services |
| Legacy data access | Entity Framework | 6.2.0 | Maps catalog entities to SQL Server |
| Ported data access | Entity Framework Core | 2.2.6 | Maps catalog entities to SQL Server |
| Storage | SQL Server LocalDB | — | Stores catalog items, brands, and types |

### Data Storage & External Services

The legacy project configures `CatalogDBContext` to use SQL Server LocalDB. The ASP.NET Core port configures a separate `DefaultConnection` to SQL Server LocalDB and uses Entity Framework Core. No external APIs, caches, or message brokers are configured for these applications; catalog images are served from the legacy application's local `Pics` directory.

### Key Architectural Decisions

- Both applications separate MVC controllers from catalog operations behind `ICatalogService`.
- Autofac configures dependency injection in both applications, with mock-data implementations selectable from configuration.
- The port is a distinct application using ASP.NET Core MVC and EF Core rather than a shared runtime with the legacy MVC app.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        CatalogMvc["CatalogController"]
        PicMvc["PicController"]
        BrandApi["BrandsController"]
        FilesApi["FilesController"]
        CoreControllers["Ported MVC controllers"]
    end
    subgraph Business["Business Logic"]
        CatalogContract["ICatalogService"]
        CatalogService["CatalogService"]
        MockService["CatalogServiceMock"]
        PortedCatalogService["Ported CatalogService"]
    end
    subgraph Persistence["Data Access"]
        Ef6Context["CatalogDBContext - EF6"]
        EfCoreContext["CatalogDBContext - EF Core"]
        Initializer["CatalogDBInitializer"]
        LegacyDb[("Legacy SQL Server LocalDB")]
        PortedDb[("Ported SQL Server LocalDB")]
    end
    subgraph Infrastructure["Infrastructure"]
        AutofacConfig["Autofac ApplicationModule"]
        ImageFiles["Local Pics directory"]
    end

    CatalogMvc -->|"catalog operations"| CatalogContract
    PicMvc -->|"finds item"| CatalogContract
    BrandApi -->|"brand queries"| CatalogContract
    FilesApi -->|"brand queries"| CatalogContract
    CoreControllers -->|"catalog operations"| PortedCatalogService
    CatalogContract --> CatalogService
    CatalogContract --> MockService
    CatalogService -->|"queries and updates"| Ef6Context
    Ef6Context -->|"SQL"| LegacyDb
    PortedCatalogService -->|"queries and updates"| EfCoreContext
    EfCoreContext -->|"SQL"| PortedDb
    PicMvc -->|"reads image files"| ImageFiles
    AutofacConfig -.->|"registers services and controllers"| Presentation
    AutofacConfig -.->|"selects implementation"| CatalogContract
    AutofacConfig -.->|"registers"| Initializer
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| CatalogController | Presentation | MVC controller | Displays and manages catalog items |
| PicController | Presentation | MVC controller | Serves catalog images from the local image directory |
| BrandsController | Presentation | Web API controller | Returns brand data and handles the demo delete endpoint |
| FilesController | Presentation | Web API controller | Serializes brand data into a binary response |
| Ported MVC controllers | Presentation | ASP.NET Core MVC controllers | Expose catalog and image operations in the port |
| ICatalogService | Business Logic | Service contract | Defines catalog reads and writes |
| CatalogService | Business Logic | Service implementation | Queries and updates catalog data |
| CatalogServiceMock | Business Logic | Mock service | Supplies mock catalog data when configured |
| CatalogDBContext | Data Access | EF6 / EF Core context | Maps catalog entities to relational storage |
| CatalogDBInitializer | Data Access | Database initializer | Initializes the legacy catalog database |
| ApplicationModule | Infrastructure | Autofac module | Registers service implementations and data components |
