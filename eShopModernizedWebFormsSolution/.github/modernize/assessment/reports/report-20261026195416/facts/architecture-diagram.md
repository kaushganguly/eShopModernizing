# Architecture Diagram

The solution contains one ASP.NET Web Forms application, with SQL Server persistence and optional Azure services for identity, image storage, and telemetry. The browser-facing application and its supporting infrastructure are separated below.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Browser"]
    end
    subgraph App["Application Layer"]
        Web["ASP.NET Web Forms on .NET Framework 4.7.2"]
        Pages["Catalog pages and ASMX image uploader"]
        Auth["OWIN OpenID Connect"]
        Services["Catalog and image services"]
        EF["Entity Framework 6.3"]
    end
    subgraph Data["Data Layer"]
        SQL[("SQL Server Catalog database")]
    end
    subgraph External["External Services"]
        AAD["Azure Active Directory optional"]
        Blob[("Azure Blob Storage optional")]
        AI["Application Insights optional"]
    end

    Browser -->|"HTTP requests"| Web
    Web -->|"routes and renders"| Pages
    Pages -->|"delegates catalog work"| Services
    Web -->|"authentication challenge"| Auth
    Auth -->|"OpenID Connect"| AAD
    Services -->|"ORM queries"| EF
    EF -->|"SQL operations"| SQL
    Services -->|"image storage"| Blob
    Web -->|"telemetry"| AI
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---:|---|
| Presentation | ASP.NET Web Forms | .NET Framework 4.7.2 | Server-rendered catalog and administration pages |
| Web services | ASP.NET ASMX with ScriptService | .NET Framework 4.7.2 | AJAX image upload |
| Dependency injection | Autofac | 4.9.4 | Application service registrations |
| Authentication | OWIN OpenID Connect | Microsoft.Owin 4.2.2; middleware package versions vary | Optional Azure Active Directory sign-in |
| Business services | CatalogService and image services | Application code | Catalog operations and image handling |
| Persistence | Entity Framework 6 | 6.3.0 | SQL Server data access |
| Database | SQL Server | Container image version not pinned in compose | Catalog data |
| Object storage | Azure Blob Storage SDK | 9.3.3 | Optional catalog image storage |
| Observability | Application Insights and log4net | 2.11.x; 2.0.10 | Telemetry and application logging |

### Data Storage & External Services

Catalog items, brands, and types are persisted in SQL Server through Entity Framework. Image data can be stored in Azure Blob Storage; the local compose configuration instead selects the mock storage implementation. OpenID Connect with Azure Active Directory and Application Insights are optional integrations, controlled by configuration.

### Key Architectural Decisions

- A single Web Forms application hosts both user pages and the ASMX upload endpoint.
- Autofac composes catalog, image, database, and connection-factory implementations according to configuration flags.
- Entity Framework 6 uses a single catalog database; image blobs are stored separately when Azure storage is enabled.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        DefaultPage["Default page"]
        CatalogPages["Create, Edit, Details, Delete pages"]
        Uploader["PicUploader ASMX service"]
    end
    subgraph Business["Business Logic"]
        CatalogSvc["CatalogService"]
        MockCatalog["CatalogServiceMock"]
        ImageSvc["ImageAzureStorage"]
        MockImage["ImageMockStorage"]
    end
    subgraph Persistence["Data Access"]
        Context["CatalogDBContext"]
        Initializer["CatalogDBInitializer"]
        Generator["CatalogItemHiLoGenerator"]
    end
    subgraph Infrastructure["Infrastructure"]
        AuthModule["OWIN authentication"]
        DI["Autofac ApplicationModule"]
        SQL["SQL Server"]
        AzureBlob["Azure Blob Storage"]
    end

    DefaultPage -->|"queries catalog"| CatalogSvc
    CatalogPages -->|"creates, updates, deletes"| CatalogSvc
    Uploader -->|"validates and uploads images"| ImageSvc
    CatalogSvc -->|"uses"| Context
    CatalogSvc -->|"allocates identifiers"| Generator
    Initializer -->|"seeds and configures"| Context
    Context -->|"Entity Framework queries"| SQL
    ImageSvc -->|"stores images"| AzureBlob
    DI -.->|"registers implementations"| CatalogSvc
    DI -.->|"registers mock alternatives"| MockCatalog
    DI -.->|"registers image implementations"| MockImage
    AuthModule -.->|"protects administration pages"| CatalogPages
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| Default page | Presentation | Web Forms page | Lists and paginates catalog items |
| Catalog pages | Presentation | Web Forms pages | Create, edit, view, and delete catalog items |
| PicUploader | Presentation | ASMX web service | Validates image uploads and returns a JSON result |
| CatalogService | Business Logic | Service | Implements catalog queries and changes |
| CatalogServiceMock | Business Logic | Mock service | Supplies in-memory catalog data |
| ImageAzureStorage | Business Logic | Image service | Stores catalog images in Azure Blob Storage |
| ImageMockStorage | Business Logic | Image service | Uses local image assets for the mock configuration |
| CatalogDBContext | Data Access | EF6 DbContext | Maps catalog entities and tables |
| CatalogDBInitializer | Data Access | EF initializer | Creates sequences and seeds catalog data |
| CatalogItemHiLoGenerator | Data Access | Identifier generator | Allocates catalog item identifiers |
| ApplicationModule | Infrastructure | Autofac module | Registers application and infrastructure services |
| OWIN authentication | Infrastructure | Middleware | Initiates OpenID Connect sign-in when enabled |
