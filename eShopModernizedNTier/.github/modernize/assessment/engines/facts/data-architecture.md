# Data Architecture

- Store: SQL Server database `eShopDatabase` (LocalDB `(localdb)\MSSQLLocalDB` in development, `sql.data` container via compose).
- Access: Entity Framework 6.1.3 Code First, `EntityModel : DbContext` (`System.Data.SqlClient` provider).
- Connection string: environment variable `ConnectionString` if set, else named connection `EntityModel` in `Web.config` (`CatalogConfiguration.ConnectionString`).
- Initialization: `CatalogDBInitializer` seeds data from `PreconfiguredData`; `CatalogItemHiLoGenerator` exists for id generation. Catalog item and stock ids are generated in service code as `Max(Id)+1`.

```mermaid
erDiagram
    CatalogBrand ||--o{ CatalogItem : has
    CatalogType ||--o{ CatalogItem : has
    CatalogItem ||--o{ CatalogItemsStock : "stock per date"
    DiscountItem {
        int Id
        datetime Start
        datetime End
    }
    CatalogItem {
        int Id
        string Name
        string Description
        money Price
        string Picturefilename
        int CatalogBrandId
        int CatalogTypeId
    }
    CatalogItemsStock {
        int StockId
        int CatalogItemId
        datetime Date
        int AvailableStock
    }
    CatalogBrand { int Id string Brand }
    CatalogType { int Id string Type }
```

Notes: several queries call `ToList()` before filtering (client-side evaluation); `CatalogItem.Price` is `money` / precision (19,4). The service holds an `EntityModel` per service instance.
