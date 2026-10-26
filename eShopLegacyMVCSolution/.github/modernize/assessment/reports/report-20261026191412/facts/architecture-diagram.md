# Architecture Diagram

The solution contains two catalog web applications, a legacy ASP.NET MVC application and a ported ASP.NET Core application. Both have a catalog service layer and SQL Server persistence mode; the ported application selects mock data by default. A small utilities library is shared between them.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Browser"]
    end
    subgraph Legacy["Legacy Web Application"]
        LegacyMVC["ASP.NET MVC 5 and Web API 2"]
        LegacyService["CatalogService"]
        LegacyEF["Entity Framework 6"]
    end
    subgraph Ported["Ported Web Application"]
        PortedMVC["ASP.NET Core MVC 2.2"]
        PortedService["CatalogService"]
        PortedEF["Entity Framework Core 2.2"]
    end
    subgraph Data["Data Stores"]
        LegacyDB[("SQL Server LocalDB catalog database")]
        PortedDB[("SQL Server LocalDB eShopPorted database")]
    end
    Utilities["eShopLegacy.Utilities"]
    Files["Local catalog picture files"]

    Browser -->|"web and API requests"| LegacyMVC
    Browser -->|"web and API requests"| PortedMVC
    LegacyMVC -->|"catalog operations"| LegacyService
    PortedMVC -->|"catalog operations"| PortedService
    LegacyService -->|"queries and updates"| LegacyEF
    PortedService -->|"queries and updates"| PortedEF
    LegacyEF -->|"SQL"| LegacyDB
    PortedEF -->|"SQL"| PortedDB
    LegacyMVC -->|"binary serialization helper"| Utilities
    PortedMVC -->|"binary serialization helper"| Utilities
    LegacyMVC -->|"serves pictures"| Files
    PortedMVC -->|"serves pictures"| Files
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---|---|
| Presentation | ASP.NET MVC and Web API | MVC 5.2.7; Web API 5.2.7 | Legacy catalog pages and API |
| Presentation | ASP.NET Core MVC | 2.2.0 | Ported catalog pages and API |
| Business | CatalogService and ICatalogService | In solution | Catalog operations and mock-data selection |
| Data access | Entity Framework 6 | 6.2.0 | Legacy SQL Server persistence |
| Data access | Entity Framework Core and SQL Server provider | 2.2.6 | Ported SQL Server persistence |
| Dependency injection | Autofac | 4.9.1 | Service registration |
| Shared utility | eShopLegacy.Utilities | .NET Framework 4.6.1 | Binary serialization helper |
| Runtime | .NET Framework | 4.6.1 projects | Application and library target framework |

### Data Storage & External Services

The legacy application uses SQL Server LocalDB by default. The ported application's `UseMockData` setting defaults to true; when database mode is enabled, it uses SQL Server LocalDB. Their configured database names are different. The legacy app uses the `CatalogDBContext` connection in `Web.config`; the ported app uses `DefaultConnection` in `appsettings.json`. Catalog pictures are read from local application files. No external service, message broker, or cache integration was identified.

### Key Architectural Decisions

- The solution contains two parallel catalog implementations with separate EF contexts and data stores.
- `ICatalogService` abstracts catalog operations and is registered with either a database-backed or mock implementation.
- Autofac provides dependency injection; the applications share a binary serialization utility.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        LegacyControllers["Legacy MVC and Web API controllers"]
        PortedControllers["Ported MVC and API controllers"]
    end
    subgraph Business["Business Logic"]
        LegacyCatalog["Legacy ICatalogService and CatalogService"]
        PortedCatalog["Ported ICatalogService and CatalogService"]
        LegacyMock["Legacy CatalogServiceMock"]
        PortedMock["Ported CatalogServiceMock"]
    end
    subgraph DataAccess["Data Access"]
        LegacyContext["EF6 CatalogDBContext"]
        PortedContext["EF Core CatalogDBContext"]
        LegacyStore[("Legacy SQL Server LocalDB")]
        PortedStore[("Ported SQL Server LocalDB")]
    end
    subgraph Infrastructure["Infrastructure"]
        Autofac["Autofac ApplicationModule"]
        Shared["eShopLegacy.Utilities"]
        StaticFiles["Picture files"]
    end

    LegacyControllers -->|"delegates catalog operations"| LegacyCatalog
    PortedControllers -->|"delegates catalog operations"| PortedCatalog
    Autofac -.->|"registers"| LegacyCatalog
    Autofac -.->|"registers"| PortedCatalog
    LegacyCatalog -->|"mock mode"| LegacyMock
    PortedCatalog -->|"mock mode"| PortedMock
    LegacyCatalog -->|"queries and updates"| LegacyContext
    PortedCatalog -->|"queries and updates"| PortedContext
    LegacyContext -->|"SQL"| LegacyStore
    PortedContext -->|"SQL"| PortedStore
    LegacyControllers -->|"binary export helper"| Shared
    PortedControllers -->|"binary export helper"| Shared
    LegacyControllers -->|"reads images"| StaticFiles
    PortedControllers -->|"reads images"| StaticFiles
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| CatalogController | Presentation | MVC controller | Catalog listing, details, create, edit, and delete flows |
| PicController | Presentation | MVC controller | Loads a catalog item's picture |
| BrandsController | Presentation | API controller | Lists and looks up brands; demo delete action |
| FilesController | Presentation | API controller | Returns serialized brand data |
| CatalogService | Business Logic | Service | Catalog queries and persistence operations |
| CatalogServiceMock | Business Logic | Mock service | Supplies predefined catalog data |
| CatalogDBContext | Data Access | EF context | Maps catalog entities to SQL Server |
| CatalogItemHiLoGenerator | Data Access | ID generator | Allocates catalog item IDs from a SQL sequence |
| ApplicationModule | Infrastructure | Autofac module | Registers services and context dependencies |
| Serializing | Infrastructure | Utility | Serializes objects to a binary stream |
