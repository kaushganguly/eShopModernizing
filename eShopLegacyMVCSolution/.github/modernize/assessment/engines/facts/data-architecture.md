# Data Architecture & Persistence Layer

The solution models a product catalog with three primary entity types. The legacy application uses Entity Framework 6 and the ported application uses Entity Framework Core; both configure SQL Server LocalDB as the default database.

## Database Configuration

| Service/Module | DB Type | Profile | Driver | Connection | Migration Tool |
|---|---|---|---|---|---|
| eShopLegacyMVC | SQL Server LocalDB | Default | System.Data.SqlClient | `CatalogDBContext` in `Web.config`; integrated authentication | `CatalogDBInitializer` and SQL scripts |
| eShopPorted | SQL Server LocalDB | Database-backed mode (`UseMockData=false`) | EF Core SQL Server provider | `DefaultConnection` in `appsettings.json`; LocalDB | EF Core migrations |

The ported application registers its context only when `UseMockData` is false. The legacy initializer can load catalog seed data and SQL scripts; optional picture customization extracts a fixed project archive into the pictures directory.

## Data Ownership per Service

| Service | Tables Owned | ORM Framework | Caching | Notes |
|---|---|---|---|---|
| eShopLegacyMVC | Catalog, CatalogBrand, CatalogType | Entity Framework 6 | None identified | Database-backed catalog with HiLo item IDs |
| eShopPorted | CatalogItem, CatalogBrand, CatalogType | Entity Framework Core 2.2 | None identified | EF migrations and optional mock data |

## Entity Model

```mermaid
erDiagram
    CatalogBrand ||--o{ CatalogItem : "brands"
    CatalogType ||--o{ CatalogItem : "categorizes"
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
        int CatalogBrandId FK
        int CatalogTypeId FK
        int AvailableStock
        int RestockThreshold
        int MaxStockThreshold
        bool OnReorder
    }
```

The entity types are defined in `src/eShopLegacyMVC/Models/` and duplicated in `eShopPorted/Models/`. The legacy context maps `CatalogItem` to `Catalog`, `CatalogBrand` to `CatalogBrand`, and `CatalogType` to `CatalogType`; the ported context applies entity configuration classes from its assembly. The legacy context requires an item brand and type.

## Key Repository Methods

| Service | Repository | Notable Methods | Purpose |
|---|---|---|---|
| eShopLegacyMVC | `CatalogService` (`src/eShopLegacyMVC/Services/CatalogService.cs`) | `GetCatalogItemsPaginated(pageSize, pageIndex)` | Counts, orders, pages, and includes brand/type entities |
| eShopLegacyMVC | `CatalogService` | `FindCatalogItem(id)` | Loads an item with its brand and type |
| eShopLegacyMVC | `CatalogService` | `CreateCatalogItem`, `UpdateCatalogItem`, `RemoveCatalogItem` | Persists catalog changes |
| eShopPorted | `CatalogService` (`eShopPorted/Services/CatalogService.cs`) | Catalog query and mutation methods | Ported equivalents using EF Core |

No repository interface separate from `ICatalogService` was identified. The service classes access their DbContext directly; no custom cache or bulk cross-service lookup method was found.

## Caching Strategy

No application cache, distributed cache, second-level ORM cache, or cache-aside strategy was identified. Database-backed catalog reads query Entity Framework directly; mock mode uses in-memory predefined data.

## Data Ownership Boundaries

Each web application owns a separate catalog persistence context and uses a different LocalDB database name by default (`Microsoft.eShopOnContainers.Services.CatalogDb` for the legacy app and `eShopPorted` for the ported app). No cross-service database access or API-based data composition was found. The mock-data implementations are an alternate local data source rather than a shared data store.

### Data Classification & Sensitivity

| Entity | Sensitive Fields | Classification (PII/PHI/PCI/None) | Controls in Place |
|---|---|---|---|
| CatalogItem | Product description, pricing, inventory, image filename | None | Not applicable |
| CatalogBrand | Brand name | None | Not applicable |
| CatalogType | Product category | None | Not applicable |

No PII, PHI, or PCI data was detected in the catalog entity model.
