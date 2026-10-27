# Data Architecture & Persistence Layer

The application models catalog items, brands, and types with Entity Framework 6.3.0 and can use SQL Server or a process-local mock catalog. Product images are stored separately through an optional Azure Blob Storage implementation.

## Database Configuration

| Service/Module | DB Type | Profile | Driver | Connection | Migration Tool |
|---|---|---|---|---|---|
| eShopModernizedWebForms | SQL Server | `UseMockData=false` | `System.Data.SqlClient` | `CatalogDBContext` connection string; managed identity is optional | EF initializer and SQL sequence scripts; no EF migration set found |
| eShopModernizedWebForms | In-memory list | `UseMockData=true` | None | Process-local `CatalogServiceMock` | None |

Initial catalog content can be loaded from CSV files by `CatalogDBInitializer`; sequence definitions are provided as SQL scripts. The database connection behavior and configuration sources are described in `configuration-inventory.md`.

## Data Ownership per Service

| Service | Tables Owned | ORM Framework | Caching | Notes |
|---|---|---|---|---|
| eShopModernizedWebForms | `Catalog`, `CatalogBrand`, `CatalogType` | Entity Framework 6.3.0 | None identified | Single application owns all catalog records; images are external blobs |

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

The corresponding entity files are `Models/CatalogBrand.cs`, `Models/CatalogType.cs`, and `Models/CatalogItem.cs`; mappings and foreign keys are configured in `Models/CatalogDBContext.cs`. Catalog items have required brand and type references. `PictureUri` and `TempImageName` are not persisted.

## Key Repository Methods

| Service | Repository | Notable Methods | Purpose |
|---|---|---|---|
| eShopModernizedWebForms | `CatalogService` (EF6; no repository interface) | `GetCatalogItemsPaginated(int pageSize, int pageIndex)` | Counts and pages catalog rows, loading brand and type |
| eShopModernizedWebForms | `CatalogService` | `FindCatalogItem(int id)` | Loads one item and its brand/type |
| eShopModernizedWebForms | `CatalogService` | `CreateCatalogItem`, `UpdateCatalogItem`, `RemoveCatalogItem` | Persist catalog mutations |
| eShopModernizedWebForms | `CatalogServiceMock` | Matching catalog service methods | In-memory implementation of the same operations |

## Caching Strategy

No application-level or EF query cache was identified. The mock catalog is held in memory for the process lifetime. Redis session-state provider configuration exists only as a commented example in `Web.config`; no active Redis cache configuration was found.

## Data Ownership Boundaries

The web application is the only application service in the solution and owns the complete catalog database; no service-to-service database sharing or CQRS boundary is present. Catalog reads and writes go through `ICatalogService`; the SQL implementation uses one EF context per lifetime scope, while the mock implementation mutates its local collection. Images are stored in the separate `pics` Azure Blob container when Azure storage is enabled.

### Data Classification & Sensitivity

| Entity | Sensitive Fields | Classification (PII/PHI/PCI/None) | Controls in Place |
|---|---|---|---|
| CatalogItem | Product names, descriptions, stock and pricing | None | No field-level encryption or masking identified |
| CatalogBrand | Brand name | None | No sensitive data controls required by the model |
| CatalogType | Type name | None | No sensitive data controls required by the model |

No PII, PHI, or PCI data was identified in the catalog entity model.
