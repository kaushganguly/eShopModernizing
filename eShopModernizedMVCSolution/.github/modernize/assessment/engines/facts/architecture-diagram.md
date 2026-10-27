# Architecture Diagram

The workspace contains a single ASP.NET MVC catalog application. It runs with either a mock catalog or Entity Framework-backed SQL Server storage, and can optionally use Azure identity, blob storage, and Application Insights.

## Application Architecture

```mermaid
flowchart TD
    Browser["Web Browser"]
    subgraph App["Application Layer - ASP.NET MVC 5 on .NET Framework 4.7.2"]
        Routes["MVC Routes and Razor Views"]
        Controllers["Catalog and Picture Controllers"]
        Services["Catalog and Image Services"]
        DI["Autofac Dependency Injection"]
        Auth["OWIN Cookie and OpenID Connect Authentication"]
    end
    subgraph Data["Data Layer"]
        EF["Entity Framework 6"]
        SQL[("SQL Server Catalog Database")]
    end
    subgraph External["Optional External Services"]
        Blob[("Azure Blob Storage")]
        AAD["Azure Active Directory"]
        AI["Application Insights"]
    end

    Browser -->|"HTTP requests"| Routes
    Routes --> Controllers
    Controllers --> Services
    DI -.->|"resolves"| Services
    Auth -.->|"authenticates requests when enabled"| Routes
    Services -->|"catalog queries"| EF
    EF -->|"SQL"| SQL
    Services -->|"image upload and retrieval"| Blob
    Auth -->|"OpenID Connect"| AAD
    App -->|"telemetry"| AI
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---:|---|
| Presentation | ASP.NET MVC and Razor | MVC 5.2.7 | Server-rendered catalog pages and MVC actions |
| Runtime | .NET Framework | 4.7.2 | Web application runtime |
| Dependency injection | Autofac and Autofac.Mvc5 | 4.9.4 / 4.0.2 | Controller and service registration |
| Data access | Entity Framework 6 | 6.3.0 | Catalog entity persistence |
| Database | SQL Server | Compose image version unspecified | Catalog persistence when mock data is disabled |
| Authentication | OWIN cookies and OpenID Connect | OWIN packages 4.0.1–4.2.2 | Optional Azure AD sign-in |
| Image storage | Azure Storage Blobs | WindowsAzure.Storage 9.3.3 | Optional product image storage |
| Observability | Application Insights and log4net | 2.11.0 / 2.0.12 | Request telemetry and application logging |

### Data Storage & External Services

The catalog is stored in SQL Server through Entity Framework when `UseMockData` is false; otherwise the application uses an in-memory mock service. Product images can be stored in Azure Blob Storage, and Azure Active Directory and Application Insights are configured as optional integrations. Redis session support is referenced by a package but its session configuration is commented out.

### Key Architectural Decisions

- The application is a single MVC deployment with controllers, services, and an EF6 context registered through Autofac.
- Configuration flags select mock or database catalog storage, local or Azure image storage, and local or managed-identity SQL authentication.
- MVC actions render Razor views; no separate API gateway or independently deployable business service is present.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        CatalogController["CatalogController"]
        PicController["PicController"]
        Razor["Razor Views"]
    end
    subgraph Business["Business Logic"]
        CatalogService["CatalogService"]
        MockService["CatalogServiceMock"]
        ImageService["IImageService"]
        AuthMiddleware["OWIN Authentication"]
    end
    subgraph DataAccess["Data Access"]
        DbContext["CatalogDBContext"]
        SQLDB[("SQL Server")]
        MockList["In-memory CatalogItem List"]
    end
    subgraph Infrastructure["Infrastructure"]
        Autofac["Autofac"]
        AzureBlob[("Azure Blob Storage")]
        AppInsights["Application Insights"]
    end

    CatalogController -->|"delegates catalog operations"| CatalogService
    CatalogController -->|"delegates image URLs"| ImageService
    PicController -->|"uploads validated image"| ImageService
    CatalogService -->|"queries and saves"| DbContext
    DbContext -->|"EF6"| SQLDB
    MockService -->|"reads and mutates"| MockList
    ImageService -->|"optional blob operations"| AzureBlob
    Autofac -.->|"registers controllers and services"| CatalogController
    Autofac -.->|"selects implementation by flags"| CatalogService
    AuthMiddleware -.->|"request authentication"| CatalogController
    AppInsights -.->|"collects telemetry"| Presentation
    CatalogController -->|"returns views"| Razor
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| CatalogController | Presentation | MVC controller | Displays and edits catalog items |
| PicController | Presentation | MVC controller | Accepts image uploads and validates decoded image format |
| CatalogService | Business Logic | Service | Queries and updates catalog data through EF6 |
| CatalogServiceMock | Business Logic | Mock service | Provides in-memory catalog behavior |
| ImageAzureStorage | Business / Infrastructure | Image service | Initializes, uploads, and moves blobs |
| ImageMockStorage | Business / Infrastructure | Image service | Resolves local mock image URLs |
| CatalogDBContext | Data Access | EF6 DbContext | Maps catalog entities to SQL Server |
| ApplicationModule | Infrastructure | Autofac module | Selects lifetime and implementation for application services |
| Startup | Infrastructure | OWIN startup | Enables OpenID Connect or local authentication middleware |
