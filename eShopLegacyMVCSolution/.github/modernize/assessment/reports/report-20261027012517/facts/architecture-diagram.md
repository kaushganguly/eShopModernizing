# Architecture Diagram

The solution contains a legacy ASP.NET MVC application and a separately hosted ported MVC application, backed by a shared catalog domain. Each application can use its own SQL Server persistence implementation or an in-memory mock service.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Browser"]
        ApiClient["HTTP API Client"]
    end
    subgraph Legacy["Legacy Application - ASP.NET MVC 5"]
        LegacyMvc["MVC 5 Views and Controllers"]
        LegacyApi["ASP.NET Web API 2"]
        LegacyService["Catalog Service"]
        LegacyEf["Entity Framework 6"]
    end
    subgraph Ported["Ported Application - ASP.NET Core 2.2"]
        PortedMvc["ASP.NET Core MVC"]
        PortedApi["Attribute-routed API Controllers"]
        PortedService["Catalog Service"]
        PortedEf["Entity Framework Core 2.2"]
    end
    subgraph Shared["Shared Library"]
        Utilities["eShopLegacy.Utilities"]
    end
    subgraph Storage["Data Storage"]
        LegacyDb[("SQL Server LocalDB - CatalogDb")]
        PortedDb[("SQL Server LocalDB - eShopPorted")]
        LegacyMock["Legacy in-memory catalog mock"]
        PortedMock["Ported in-memory catalog mock"]
    end

    Browser -->|"MVC requests"| LegacyMvc
    ApiClient -->|"HTTP requests"| LegacyApi
    LegacyMvc -->|"catalog operations"| LegacyService
    LegacyApi -->|"catalog operations"| LegacyService
    LegacyService -->|"queries and updates"| LegacyEf
    LegacyEf -->|"SQL"| LegacyDb
    Browser -->|"MVC requests"| PortedMvc
    ApiClient -->|"HTTP requests"| PortedApi
    PortedMvc -->|"catalog operations"| PortedService
    PortedApi -->|"catalog operations"| PortedService
    PortedService -->|"queries and updates"| PortedEf
    PortedEf -->|"SQL when enabled"| PortedDb
    LegacyService -.->|"mock mode"| LegacyMock
    PortedService -.->|"mock mode"| PortedMock
    LegacyApi -.->|"binary response serialization"| Utilities
    PortedApi -.->|"binary response serialization"| Utilities
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---|---|
| Legacy presentation/API | ASP.NET MVC and ASP.NET Web API | MVC 5.2.7; Web API 5.2.7 | Server-rendered catalog pages and HTTP endpoints |
| Legacy data | Entity Framework 6 | 6.2.0 | SQL Server access and catalog persistence |
| Ported presentation/API | ASP.NET Core MVC | 2.2.0 | Ported server-rendered pages and attribute-routed endpoints |
| Ported data | Entity Framework Core with SQL Server provider | 2.2.6 | SQL Server access and migrations |
| Dependency injection | Autofac | 6.1.0 (legacy); 4.9.1 (ported) | Service registration and lifetime management |
| Shared utilities | eShopLegacy.Utilities | .NET Framework 4.6.1 | Binary serialization helper |
| Logging/telemetry | log4net; Application Insights | 2.0.10; 2.9.1 (legacy) | Application logging and telemetry |

### Data Storage & External Services

The legacy application is configured for SQL Server LocalDB and Entity Framework 6. The ported application has a separate LocalDB connection and EF Core model, but its checked-in settings enable mock data by default. No external APIs, message brokers, caches, or service-discovery systems were identified.

### Key Architectural Decisions

- Both applications expose the same catalog domain through separate MVC/API and persistence stacks; they are not a distributed service system.
- Autofac selects either a per-application mock catalog service or a database-backed service through the `UseMockData` setting.
- The mock implementation is a singleton with mutable in-memory catalog state when enabled.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        LegacyUi["Legacy CatalogController and PicController"]
        LegacyRest["Legacy Web API Controllers"]
        PortedUi["Ported CatalogController and PicController"]
        PortedRest["Ported API Controllers"]
    end
    subgraph Business["Business Logic"]
        LegacyCatalog["Legacy ICatalogService"]
        PortedCatalog["Ported ICatalogService"]
        LegacyMock["Legacy CatalogServiceMock"]
        PortedMock["Ported CatalogServiceMock"]
    end
    subgraph DataAccess["Data Access"]
        LegacyContext["Legacy CatalogDBContext"]
        PortedContext["Ported CatalogDBContext"]
    end
    subgraph Infrastructure["Infrastructure"]
        Autofac["Autofac ApplicationModule"]
        Serializer["eShopLegacy.Utilities Serializing"]
        Telemetry["log4net and Application Insights"]
    end

    LegacyUi -->|"catalog actions"| LegacyCatalog
    LegacyRest -->|"catalog lookups"| LegacyCatalog
    PortedUi -->|"catalog actions"| PortedCatalog
    PortedRest -->|"catalog lookups"| PortedCatalog
    LegacyCatalog -->|"EF6 queries"| LegacyContext
    PortedCatalog -->|"EF Core queries"| PortedContext
    Autofac -.->|"selects implementation"| LegacyCatalog
    Autofac -.->|"selects implementation"| LegacyMock
    Autofac -.->|"selects implementation"| PortedCatalog
    Autofac -.->|"selects implementation"| PortedMock
    LegacyRest -->|"serializes file response"| Serializer
    PortedRest -->|"serializes file response"| Serializer
    LegacyUi -.->|"request logging"| Telemetry
    PortedUi -.->|"request logging"| Telemetry
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| Legacy `CatalogController` | Presentation | MVC controller | Catalog browse, create, update, and delete pages |
| Legacy `PicController` | Presentation | MVC controller | Reads catalog image files |
| Legacy Web API controllers | Presentation | API controllers | Expose brands, files, and a sample catalog response |
| Ported `CatalogController` | Presentation | MVC controller | Ported catalog page and CRUD flow |
| Ported `BrandsController` and `FilesController` | Presentation | API controllers | Expose brand data and serialized brand data |
| `ICatalogService` implementations | Business Logic | Service | Catalog queries and changes, backed by EF or mock data |
| `CatalogDBContext` | Data Access | EF context | Maps catalog items, brands, and types |
| `ApplicationModule` | Infrastructure | DI module | Selects mock or database-backed catalog services |
| `eShopLegacy.Utilities.Serializing` | Infrastructure | Utility | Serializes brand DTO collections using `BinaryFormatter` |
