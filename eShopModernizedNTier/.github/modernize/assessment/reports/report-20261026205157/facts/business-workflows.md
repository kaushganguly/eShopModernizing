# Business Workflows

The application manages a product catalog for the eShop sample.

## Browse catalog
1. Client loads brands and types (`GetCatalogBrands`, `GetCatalogTypes`).
2. User picks optional brand/type filters; client calls `GetCatalogItems(brandId, typeId)` (0 = all).
3. Selecting an item calls `FindCatalogItem(id)` to load brand/type details.

## Maintain catalog items
Create (`CreateCatalogItem`: id = max id + 1), edit (`UpdateCatalogItem`), delete (`RemoveCatalogItem`).

## Stock and discounts
`GetAvailableStock(date, itemId)` returns the stock for that date (0 if none). `CreateAvailableStock` upserts the stock record for an item/date. `GetDiscount(day)` returns the discount whose Start/End range contains the day.

```mermaid
sequenceDiagram
    actor U as User
    participant V as CatalogView
    participant S as CatalogService
    participant D as SQL Server
    U->>V: open app
    V->>S: GetCatalogBrands / GetCatalogTypes / GetCatalogItems(0,0)
    S->>D: query
    D-->>V: catalog data
    U->>V: edit item and save
    V->>S: UpdateCatalogItem(item)
    S->>D: SaveChanges
    U->>V: set stock for a date
    V->>S: CreateAvailableStock(stock)
    S->>D: update or insert
```
