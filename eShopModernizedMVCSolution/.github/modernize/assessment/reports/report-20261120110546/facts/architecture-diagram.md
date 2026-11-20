# Architecture Diagram

The application is a single ASP.NET MVC catalog site with configurable mock or SQL-backed data and mock or Azure Blob image storage. Optional identity, telemetry, and container settings are selected through configuration.

## Application Architecture

```mermaid
flowchart TD
    subgraph AClient["Client Layer"]
        A_Browser["Browser"]
    end
    subgraph AApp["Application Layer - ASP.NET MVC 5 on .NET Framework 4.7.2"]
        A_Routes["MVC Routing and Views"]
        A_Controllers["Catalog and Image Controllers"]
        A_Services["Catalog and Image Services"]
        A_DI["Autofac Dependency Injection"]
    end
    subgraph AData["Data Layer"]
        A_EF["Entity Framework 6"]
        A_SQL[("SQL Server LocalDB or Compose SQL Server")]
        A_Mock["In-memory Mock Catalog"]
    end
    subgraph AExternal["External Services"]
        A_Blob["Azure Blob Storage"]
        A_AAD["Azure Active Directory OpenID Connect"]
        A_AI["Application Insights"]
    end

    A_Browser -->|"HTTP page and form requests"| A_Routes
    A_Routes --> A_Controllers
    A_Controllers --> A_Services
    A_DI -.->|"selects implementations"| A_Services
    A_Services -->|"catalog queries and updates"| A_EF
    A_EF -->|"SQL operations"| A_SQL
    A_Services -->|"mock-data mode"| A_Mock
    A_Services -->|"optional image operations"| A_Blob
    A_Browser -->|"sign-in redirect when enabled"| A_AAD
    A_Controllers -->|"telemetry"| A_AI
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---:|---|
| Presentation | ASP.NET MVC | 5.2.7 | Server-rendered catalog pages and form actions |
| Runtime | .NET Framework | 4.7.2 | Application runtime |
| Dependency injection | Autofac | 4.9.4 | Selects catalog, storage, and SQL connection implementations |
| Data access | Entity Framework | 6.3.0 | SQL Server persistence |
| Authentication | OWIN Cookie and OpenID Connect | 4.0.1 | Optional Azure AD sign-in |
| Observability | Application Insights | 2.11.0 | Application telemetry |
| Hosting | IIS in Windows container | 4.7.2 image | Hosts the MVC application |

### Data Storage & External Services

Catalog data is held in an in-memory mock service or persisted through Entity Framework to SQL Server LocalDB / the Compose SQL Server service. Images use a mock service or Azure Blob Storage. Azure AD OpenID Connect is optional; Application Insights is configured for telemetry. No active message-broker workflow was found.

### Key Architectural Decisions

- Autofac selects mock or production-style services from configuration.
- Catalog persistence uses an EF6 context directly from the catalog service; there is no separate repository implementation.
- Image storage is abstracted behind `IImageService`, with mock and Azure Blob implementations.

## Component Relationships

```mermaid
flowchart LR
    subgraph RPresentation["Presentation"]
        R_CatalogController["CatalogController"]
        R_PicController["PicController"]
        R_AccountController["AccountController"]
    end
    subgraph RBusiness["Business Logic"]
        R_CatalogService["CatalogService"]
        R_CatalogMock["CatalogServiceMock"]
        R_ImageService["IImageService"]
        R_ImageMock["ImageMockStorage"]
        R_ImageAzure["ImageAzureStorage"]
    end
    subgraph RData["Data Access"]
        R_Context["CatalogDBContext"]
        R_Entities["CatalogItem CatalogBrand CatalogType"]
        R_Initializer["CatalogDBInitializer"]
    end
    subgraph RInfrastructure["Infrastructure"]
        R_Module["ApplicationModule"]
        R_Auth["Startup and Authentication Middleware"]
        R_Sequence["CatalogItemHiLoGenerator"]
    end

    R_CatalogController -->|"catalog operations"| R_CatalogService
    R_PicController -->|"upload validation and image operations"| R_ImageService
    R_AccountController -->|"sign-in and sign-out"| R_Auth
    R_Module -.->|"registers configured implementations"| R_CatalogService
    R_Module -.->|"registers configured implementations"| R_ImageService
    R_CatalogService -->|"queries and saves"| R_Context
    R_CatalogService -->|"generates identifiers"| R_Sequence
    R_Context -->|"maps"| R_Entities
    R_Initializer -->|"creates sequences and seed data"| R_Context
    R_ImageService -.->|"mock implementation"| R_ImageMock
    R_ImageService -.->|"Azure implementation"| R_ImageAzure
    R_Auth -.->|"applies to protected actions"| R_CatalogController
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| CatalogController | Presentation | MVC Controller | Catalog list, details, and authorized catalog administration |
| PicController | Presentation | MVC Controller | Validates image uploads and returns image URL data |
| AccountController | Presentation | MVC Controller | Sign-in, sign-out, and session-end actions |
| CatalogService | Business Logic | Service | Queries and mutates SQL-backed catalog items |
| CatalogServiceMock | Business Logic | Service | Serves and mutates an in-memory catalog |
| ImageAzureStorage | Business Logic | Storage service | Reads and writes catalog image blobs |
| ImageMockStorage | Business Logic | Storage service | Supplies local image URLs in mock mode |
| CatalogDBContext | Data Access | EF6 DbContext | Maps catalog entities to SQL Server |
| CatalogDBInitializer | Data Access | Initializer | Creates sequence objects and seeds catalog data |
| ApplicationModule | Infrastructure | Autofac module | Registers service implementations and lifetimes |
| Startup | Infrastructure | OWIN startup | Configures authentication middleware |
