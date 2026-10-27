# Data Architecture & Persistence Layer

The data model consists of three catalog entities stored in SQL Server through Entity Framework 6. A separate image service can store picture content in Azure Blob Storage; the application also supports mock data and image implementations.

## Database Configuration

| Service/Module | DB Type | Profile | Driver | Connection | Migration Tool |
|---|---|---|---|---|---|
| eShopModernizedMVC | SQL Server | Plain .NET default | System.Data.SqlClient | LocalDB connection in Web.config | None; EF `CreateDatabaseIfNotExists` initializer |
| eShopModernizedMVC | SQL Server | Docker Compose | System.Data.SqlClient | Compose SQL Server service; credential value omitted | None; EF initializer creates schema and runs sequence scripts |

Catalog data is seeded at initialization from built-in data or optional CSV files. SQL sequence scripts support identifier generation. No EF migration history or external schema migration tool was found.

## Data Ownership per Service

| Service | Tables Owned | ORM Framework | Caching | Notes |
|---|---|---|---|---|
| eShopModernizedMVC | Catalog, CatalogBrand, CatalogType | Entity Framework 6 | None | One `CatalogDBContext` manages all catalog entities; optional mock service keeps in-memory data |
| Azure Blob Storage | `pics` container objects | Azure Storage SDK | None | Image data is separate from the relational catalog records |

## Entity Model

```mermaid
erDiagram
    CatalogBrand ||--o{ CatalogItem : "brands"
    CatalogType ||--o{ CatalogItem : "classifies"
    CatalogBrand {
        int Id PK
        string Brand
    }
    CatalogType {
        int Id PK
        string Type
    }
    CatalogItem {
        int Id PK
        string Name
        string Description
        decimal Price
        string PictureFileName
        int CatalogTypeId FK
        int CatalogBrandId FK
        int AvailableStock
        int RestockThreshold
        int MaxStockThreshold
        bool OnReorder
    }
```

`CatalogItem`, `CatalogBrand`, and `CatalogType` are defined under `src/eShopModernizedMVC/Models/`. `CatalogDBContext` maps them to the `Catalog`, `CatalogBrand`, and `CatalogType` tables. The model requires item name, constrains price and stock-related values, and relates each item to one brand and one type. Picture URI and temporary-image values are not persisted.

## Key Repository Methods

| Service | Repository | Notable Methods | Purpose |
|---|---|---|---|
| eShopModernizedMVC | `CatalogService` (`Services/CatalogService.cs`) | `GetCatalogItemsPaginated(pageSize, pageIndex)` | Counts and returns an ordered page of catalog items with brand and type |
| eShopModernizedMVC | `CatalogService` (`Services/CatalogService.cs`) | `FindCatalogItem(id)` | Loads one catalog item and its brand and type |
| eShopModernizedMVC | `CatalogService` (`Services/CatalogService.cs`) | `GetCatalogTypes()`, `GetCatalogBrands()` | Supplies form selection data |
| eShopModernizedMVC | `CatalogService` (`Services/CatalogService.cs`) | `CreateCatalogItem`, `UpdateCatalogItem`, `RemoveCatalogItem` | Writes catalog changes through the Entity Framework context |

The project has no separate repository interfaces; `ICatalogService` is the application-facing data service abstraction.

## Caching Strategy

No active catalog cache was identified. MVC output caching is configured with a zero duration and `NoStore=true`; Redis session configuration is commented out in Web.config. Mock-data mode is an in-memory development implementation, not a production cache.

## Data Ownership Boundaries

The application has one relational database and one catalog bounded context; no cross-service database sharing or CQRS flow was identified. Catalog operations go through `ICatalogService` and its Entity Framework-backed implementation. Image content is stored separately through the image service when Azure storage is enabled. No other service consumes catalog data through a remote API.

### Data Classification & Sensitivity

| Entity | Sensitive Fields | Classification (PII/PHI/PCI/None) | Controls in Place |
|---|---|---|---|
| CatalogItem | Product description, stock and price | None | No field-level data protection is identified |
| CatalogBrand | Brand name | None | No field-level data protection is identified |
| CatalogType | Type label | None | No field-level data protection is identified |

No PII, PHI, or PCI data was detected in the catalog entity model.
