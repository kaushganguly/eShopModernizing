# Data Architecture & Persistence Layer

The solution models three catalog entities in two SQL Server LocalDB-backed application contexts. The legacy application uses Entity Framework 6, while the port uses Entity Framework Core.

## Database Configuration

| Service/Module | DB Type | Profile | Driver | Connection | Migration Tool |
|---|---|---|---|---|---|
| eShopLegacyMVC | SQL Server LocalDB | Default configuration; Debug/Release transforms are present | System.Data.SqlClient / EF6 SQL Server provider | `CatalogDBContext` connects to the local `Microsoft.eShopOnContainers.Services.CatalogDb` database | No migration tool detected; EF initializes a missing database and seeds it |
| eShopPorted | SQL Server LocalDB | Default and Development settings; mock data is enabled by default | EF Core SQL Server provider | `eShopPorted` local database | EF Core `Initial` migration and model snapshot are present |

The legacy initializer seeds catalog types, brands, and items, and creates SQL sequences. The port defines its initial schema through EF Core migrations and seeds catalog entities through model configuration. No separate production profile or connection pool configuration was identified.

## Data Ownership per Service

| Service | Tables Owned | ORM Framework | Caching | Notes |
|---|---|---|---|---|
| eShopLegacyMVC | Catalog, CatalogBrand, CatalogType | Entity Framework 6 | None identified | `CatalogDBContext` owns the catalog schema and `CatalogDBInitializer` seeds it |
| eShopPorted | Catalog, CatalogBrand, CatalogType | Entity Framework Core | None identified | Separate context and database; mock service is selected by default |
| eShopLegacy.Utilities | None | None | None | Shared helper library; no persistence ownership |

## Entity Model

Both contexts map the same logical catalog model. Each application maintains its own context and configured database; `PictureUri` is a presentation value and is ignored by the legacy ORM mapping and the ported entity configuration.

```mermaid
erDiagram
    CatalogBrand ||--o{ CatalogItem : "brands"
    CatalogType ||--o{ CatalogItem : "categorizes"
    CatalogItem {
        int Id PK
        string Name
        string Description
        decimal Price
        string PictureFileName
        int CatalogBrandId FK
        int CatalogTypeId FK
        int AvailableStock
        int RestockThreshold
        int MaxStockThreshold
        bool OnReorder
    }
    CatalogBrand {
        int Id PK
        string Brand
    }
    CatalogType {
        int Id PK
        string Type
    }
```

`CatalogItem` has required many-to-one relationships to `CatalogBrand` and `CatalogType`; neither parent model defines a collection navigation property. Explicit transaction scopes were not found; updates use the ORM's `SaveChanges` behavior.

## Key Repository Methods

| Service | Repository | Notable Methods | Purpose |
|---|---|---|---|
| eShopLegacyMVC | None; `CatalogService` directly uses `CatalogDBContext` (`src/eShopLegacyMVC/Services/CatalogService.cs`) | `GetCatalogItemsPaginated`, `FindCatalogItem`, `GetCatalogTypes`, `GetCatalogBrands`, `CreateCatalogItem`, `UpdateCatalogItem`, `RemoveCatalogItem` | Direct LINQ queries and CRUD operations over the EF6 context |
| eShopPorted | None; `CatalogService` directly uses `CatalogDBContext` (`eShopPorted/Services/CatalogService.cs`) | `GetCatalogItemsPaginated`, `FindCatalogItem`, `GetCatalogTypes`, `GetCatalogBrands`, `CreateCatalogItem`, `UpdateCatalogItem`, `RemoveCatalogItem` | Direct LINQ queries and CRUD operations over the EF Core context |

No repository interfaces, bulk aggregation queries, named queries, or stored procedure interfaces were identified.

## Caching Strategy

No data/query caching provider or cache-aside pattern was identified. The applications provide mock catalog service implementations, but these are selected as alternate data sources rather than a cache; the port enables mock data in its default settings.

## Data Ownership Boundaries

The legacy and ported applications each own a catalog schema in a separate configured database, with parallel `Catalog`, `CatalogBrand`, and `CatalogType` models. No cross-service API calls, shared database access, or synchronization mechanism between the applications was identified. Each application reads and writes through its own ORM context; no CQRS split was found.

### Data Classification & Sensitivity

| Entity | Sensitive Fields | Classification (PII/PHI/PCI/None) | Controls in Place |
|---|---|---|---|
| CatalogItem | None identified; product description, price, and stock values | None | No field-level controls or encryption configuration identified |
| CatalogBrand | None identified; brand label | None | No field-level controls or encryption configuration identified |
| CatalogType | None identified; category label | None | No field-level controls or encryption configuration identified |

No PII, PHI, or PCI data was detected in the catalog entity model.
