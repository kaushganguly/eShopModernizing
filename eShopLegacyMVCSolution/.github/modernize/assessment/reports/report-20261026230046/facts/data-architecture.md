# Data Architecture & Persistence Layer

The solution models a catalog using three entities and two relational persistence implementations. The legacy application uses EF6; the ported project uses EF Core and can instead run with mock catalog data.

## Database Configuration

| Service/Module | DB Type | Profile | Driver | Connection | Migration Tool |
|---|---|---|---|---|---|
| eShopLegacyMVC | SQL Server | Web.config / Debug and Release transforms | System.Data.SqlClient | `CatalogDBContext` connection string; deployment-specific value masked | EF6 database initializer and SQL sequence scripts |
| eShopPorted | SQL Server / LocalDB default | appsettings; Development overrides logging | EF Core SQL Server provider | `DefaultConnection` configured for LocalDB by default | EF Core migrations included |
| eShopLegacyMVC and eShopPorted | In-memory mock | `UseMockData` enabled | None | Process memory | None |

The legacy initializer runs sequence scripts and seeds catalog data. Optional customization data is loaded from CSV files, and product pictures are extracted from a setup archive. The ported project includes an initial EF Core migration.

## Data Ownership per Service

| Service | Tables / Entities Owned | ORM Framework | Caching | Notes |
|---|---|---|---|---|
| eShopLegacyMVC | CatalogItem, CatalogBrand, CatalogType | EF6 | None; mock mode uses an in-memory mutable list | Legacy app seeds catalog data at initialization |
| eShopPorted | CatalogItem, CatalogBrand, CatalogType | EF Core 2.2 | None; mock mode uses an in-memory mutable list | Separate project and DbContext |

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

The legacy entity source files are `src/eShopLegacyMVC/Models/CatalogItem.cs`, `CatalogBrand.cs`, and `CatalogType.cs`; the same catalog model is present under `eShopPorted/Models`. Each catalog item requires one brand and one type; either brand or type may relate to many items.

## Key Repository Methods

| Service | Repository / Data Component | Notable Methods | Purpose |
|---|---|---|---|
| eShopLegacyMVC | `ICatalogService` / `CatalogService` (`src/eShopLegacyMVC/Services`) | `GetCatalogItemsPaginated`, `FindCatalogItem`, `GetCatalogTypes`, `GetCatalogBrands`, `CreateCatalogItem`, `UpdateCatalogItem`, `RemoveCatalogItem` | Queries and persists catalog data; no repository interface is defined |
| eShopLegacyMVC | `CatalogItemHiLoGenerator` (`src/eShopLegacyMVC/Models`) | `GetNextSequenceValue` | Retrieves a SQL sequence value and allocates item identifiers |
| eShopPorted | `ICatalogService` / `CatalogService` (`eShopPorted/Services`) | Catalog service CRUD and paginated query methods | Uses EF Core DbSets directly; no repository interface is defined |

## Caching Strategy

No dedicated cache provider or configured cache layer was identified. `CatalogServiceMock` keeps catalog items in an in-memory `List<CatalogItem>` for mock mode; it is application state, not a configured cache.

## Data Ownership Boundaries

Both web projects implement the same catalog bounded context with their own ORM context. They are separate projects, and no service-to-service API or event-based data exchange was identified. Database-backed access is through the relevant application's DbContext; mock mode is local process memory. No CQRS or cross-service aggregation pattern was found.

### Data Classification & Sensitivity

| Entity | Sensitive Fields | Classification (PII/PHI/PCI/None) | Controls in Place |
|---|---|---|---|
| CatalogItem | Product description, name, price, inventory values | None | Not sensitive personal or payment data |
| CatalogBrand | Brand name | None | Not sensitive personal or payment data |
| CatalogType | Type name | None | Not sensitive personal or payment data |

No PII, PHI, or PCI data was detected in the catalog entity model.
