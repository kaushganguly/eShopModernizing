# Architecture Diagram

The application is a server-rendered catalog management website with an optional image-upload endpoint. It runs as a single .NET Framework application and uses configuration-selected implementations for data, identity, and image storage.

## Application Architecture

```mermaid
flowchart TD
    Browser["Browser"]
    subgraph App["ASP.NET Web Forms application"]
        Pages["Web Forms pages and routing"]
        Upload["ASMX image upload"]
        Auth["OWIN authentication"]
        Services["Catalog and image services"]
        EF["Entity Framework 6"]
        Config["Configuration builders"]
        Telemetry["Logging and telemetry"]
    end
    SQL[("SQL Server")]
    Blob[("Azure Blob Storage")]
    Redis[("Redis session store optional")]
    AAD["Azure Active Directory optional"]
    Vault["Azure Key Vault optional"]
    Insights["Application Insights"]

    Browser -->|"HTTP and form postbacks"| Pages
    Browser -->|"image upload"| Upload
    Pages --> Auth
    Upload --> Services
    Pages --> Services
    Services -->|"catalog queries"| EF
    EF -->|"SQL"| SQL
    Services -->|"image operations when enabled"| Blob
    Config -->|"configuration values"| Vault
    Config -->|"identity settings"| AAD
    Auth -->|"OpenID Connect when enabled"| AAD
    Auth -.->|"session state if configured"| Redis
    Telemetry --> Insights
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---:|---|
| Presentation | ASP.NET Web Forms | .NET Framework 4.7.2 | Server-rendered catalog and administration pages |
| Business logic | C# and Autofac | Autofac 4.9.4 | Catalog and image services with dependency injection |
| Data access | Entity Framework | 6.3.0 | SQL Server persistence |
| Identity | OWIN and OpenID Connect | OWIN.Security.OpenIdConnect 4.0.1 | Optional Azure Active Directory sign-in |
| Image storage | Azure Storage client | 9.3.3 | Optional Azure Blob image storage |
| Observability | log4net and Application Insights | 2.0.10 / 2.11.0 | Application logging and telemetry |

### Data Storage & External Services

Catalog records are stored in SQL Server through Entity Framework 6. Azure Blob Storage is an optional image backend; local images are used by the mock implementation. Azure Active Directory, Key Vault, Application Insights, and Redis-backed session state are configuration-dependent integrations. No message broker communication was found in the application source.

### Key Architectural Decisions

- Autofac selects mock or database-backed catalog services and local or Azure image storage from configuration.
- Entity Framework owns relational persistence; catalog images are stored separately from catalog metadata.
- The application uses server-rendered Web Forms and an ASMX method rather than a separate REST API or gateway.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation"]
        CatalogPages["Catalog pages"]
        HomePage["Default page"]
        Uploader["PicUploader ASMX"]
    end
    subgraph Business["Business logic"]
        CatalogService["ICatalogService implementation"]
        ImageService["IImageService implementation"]
    end
    subgraph DataAccess["Data access"]
        Context["CatalogDBContext"]
        Entities["Catalog entities"]
    end
    subgraph Infrastructure["Infrastructure"]
        Autofac["Autofac module"]
        OWIN["OWIN pipeline"]
        Telemetry["Logging and telemetry"]
        SQL[("SQL Server")]
        Blob[("Azure Blob Storage")]
    end

    HomePage -->|"browse pages"| CatalogService
    CatalogPages -->|"create update delete"| CatalogService
    Uploader -->|"validate and upload image"| ImageService
    CatalogService -->|"query and persist"| Context
    Context --> Entities
    Context -->|"SQL"| SQL
    ImageService -->|"optional remote storage"| Blob
    Autofac -.->|"registers implementations"| CatalogService
    Autofac -.->|"registers implementations"| ImageService
    OWIN -.->|"authentication pipeline"| CatalogPages
    Telemetry -.->|"cross-cutting diagnostics"| Presentation
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| Default page | Presentation | Web Forms page | Lists catalog items and paginates results |
| Create, Edit, Details, Delete | Presentation | Web Forms pages | Catalog item administration and display |
| PicUploader | Presentation | ASMX WebMethod | Validates and uploads image content |
| CatalogService / CatalogServiceMock | Business logic | Service implementations | Reads and mutates catalog records |
| ImageAzureStorage / ImageMockStorage | Business logic | Image service implementations | Stores or resolves catalog images |
| CatalogDBContext | Data access | Entity Framework DbContext | Maps catalog entities to SQL Server |
| ApplicationModule | Infrastructure | Autofac module | Registers lifetime and configuration-selected services |
| Startup and OWIN middleware | Infrastructure | Authentication pipeline | Enables OpenID Connect or the configured fallback |
| Global | Infrastructure | ASP.NET application lifecycle | Initializes routes, dependency injection, database, images, and telemetry |
