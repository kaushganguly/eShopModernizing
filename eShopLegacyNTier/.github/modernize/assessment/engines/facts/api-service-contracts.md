# API & Service Communication Contracts

The scoped application exposes **ten SOAP operations at one WCF service address**, consumed synchronously by the desktop. These are operation-based SOAP contracts, not ten REST routes.

## Service Catalog

| Service | Port | Category | Purpose |
|---|---|---|---|
| eShopWCFService / CatalogService | 62314, development configuration | Business | Catalog, classification, discounts and dated stock operations; WCF plus EF6 |
| eShopWinForms | No listening port | Presentation client, not an API service | Operator UI; generated WCF client; Windows Forms |

Projects: `eShopLegacyNTier.sln:6-9`. Host settings: `src\eShopWCFService\eShopWCFService.csproj:119-124`. Client address: `src\eShopWinForms\App.config:25-28`. No additional deployment manifests or source-built services were found within scope.

## API Endpoints Inventory

All rows use the configured address `http://localhost:62314/CatalogService.svc`. The path suffix below identifies the **SOAPAction operation**, not an appended URL path. Requests are SOAP XML bodies; responses use WCF SOAP wrappers.

| Service | Method | Path | Request Type | Response Type |
|---|---|---|---|---|
| CatalogService | POST / SOAP FindCatalogItem | /CatalogService.svc | int id | CatalogItem, nullable when absent |
| CatalogService | POST / SOAP GetCatalogBrands | /CatalogService.svc | No application parameters | List of CatalogBrand |
| CatalogService | POST / SOAP GetCatalogItems | /CatalogService.svc | int brandIdFilter, int typeIdFilter | List of CatalogItem |
| CatalogService | POST / SOAP GetCatalogTypes | /CatalogService.svc | No application parameters | List of CatalogType |
| CatalogService | POST / SOAP GetAvailableStock | /CatalogService.svc | DateTime date, int catalogItemId | int |
| CatalogService | POST / SOAP CreateAvailableStock | /CatalogService.svc | CatalogItemsStock | void operation response |
| CatalogService | POST / SOAP CreateCatalogItem | /CatalogService.svc | CatalogItem | void operation response |
| CatalogService | POST / SOAP UpdateCatalogItem | /CatalogService.svc | CatalogItem | void operation response |
| CatalogService | POST / SOAP RemoveCatalogItem | /CatalogService.svc | CatalogItem | void operation response |
| CatalogService | POST / SOAP GetDiscount | /CatalogService.svc | DateTime day | DiscountItem, nullable when absent |

Authoritative server signatures: `src\eShopWCFService\ICatalogService.cs:12-34`. Basic HTTP binding: `src\eShopWCFService\Web.config:37-40`. Generated actions follow `http://tempuri.org/ICatalogService/<Operation>` with corresponding `<Operation>Response` reply actions (`src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs:498-559`). There is no explicit API versioning scheme, REST status-code mapping, custom FaultContract or validation-to-400 contract. Normal SOAP success/fault transport conventions apply; actual fault statuses were not exercised.

## Management & Observability Endpoints

| Service | Endpoint | Custom Metrics (if any) |
|---|---|---|
| CatalogService | /CatalogService.svc?wsdl, HTTP metadata enabled; HTTPS metadata also enabled subject to hosting | None declared |
| CatalogService | /CatalogService.svc/mex, mexHttpBinding | None declared |
| eShopWinForms | None | None declared |

Evidence: `src\eShopWCFService\Web.config:25-29,38-40`. No health/readiness, Swagger or metrics endpoint is declared in the scoped service configuration or service contract.

## DTOs & Contracts

| Contract class | API role | Mutability |
|---|---|---|
| CatalogItem | Catalog lookup/list response and create/update/delete input | Mutable entity/data contract |
| CatalogBrand, CatalogType | Classification list responses and item nested objects | Mutable entity/data contracts |
| CatalogItemsStock | Stock write request | Mutable entity/data contract |
| DiscountItem | Discount lookup response | Mutable entity/data contract |

Entities are annotated with DataContract/DataMember and use WCF data-contract serialization, not a JSON REST serializer (`src\eShopWCFService\Models\CatalogItem.cs:10-35`; `src\eShopWCFService\Models\CatalogBrand.cs:10-19`; `src\eShopWCFService\Models\CatalogType.cs:10-19`; `src\eShopWCFService\Models\CatalogItemsStock.cs:10-25`; `src\eShopWCFService\Models\DiscountItem.cs:11-31`). See `data-architecture.md` for entity fields and mappings.

No gateway-level composite DTOs exist. WSDL/XSD artifacts and the generated mutable client models are checked into `src\eShopWinForms\Connected Services\eShopServiceReference`; the build explicitly includes Reference.cs (`src\eShopWinForms\eShopWinForms.csproj:89-93,258-264`). No OpenAPI, protobuf or GraphQL contract is present.

**Contract drift:** the generated client accepts nullable filter IDs and returns arrays, while the current server interface accepts non-nullable ints and returns Lists (`src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs:507-523`; `src\eShopWCFService\ICatalogService.cs:18-22`). The running controller supplies concrete int values; null compatibility is not verified.

## Communication Patterns

- **Synchronous:** UI events call the controller, then generated proxy operations; server calls EntityModel directly (`src\eShopWinForms\Controllers\CatalogController.cs:35-54,75-84`; `src\eShopWCFService\CatalogService.svc.cs:18-30`).
- **Async:** generated Task alternatives exist but the active controller uses synchronous methods. These are client async wrappers, not broker/event-driven communication (`src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs:501-559`).
- **Composition:** the controller fetches items and the current discount separately and combines them for display; no gateway, multi-backend join, discovery registry or load balancing is configured (`src\eShopWinForms\Controllers\CatalogController.cs:75-84`).
- **Resilience:** no retry, circuit breaker, bulkhead or service fallback is implemented. No timeout values are explicitly configured: the empty named basicHttpBinding uses framework defaults, not application-declared values (`src\eShopWinForms\App.config:20-28`). Exceptions from service calls are not caught by the controller.
- **Startup:** database access must be usable when the service handles initial calls; Program loads all view data before showing its dialog. No readiness gate or startup retry exists in that flow (`src\eShopWinForms\Program.cs:22-27`). See `configuration-inventory.md`.
- **Security:** the checked-in client and explicit service endpoint use HTTP basicHttpBinding with no configured transport credentials, authentication or operation authorization. HTTPS protocol mapping and HTTPS metadata are declared but do **not** make the explicit HTTP endpoint/client TLS-protected. IIS/deployment controls cannot be inferred (`src\eShopWCFService\Web.config:22-42`; `src\eShopWinForms\App.config:19-29`). No typed fault policy is declared; exception details in faults are disabled.

## Service Technology Matrix

| Service | Web | Data Access | Discovery | Gateway | Actuator | Cache | Metrics |
|---|---|---|---|---|---|---|---|
| eShopWCFService | WCF / ASP.NET hosting | EF6 | None declared | No | None | None declared | None declared |
| eShopWinForms | No HTTP server; WCF client | No active DB access evidenced | Static address | No; local display composition | None | None declared | None declared |

Evidence: `src\eShopWCFService\CatalogService.svc:1`; `src\eShopWCFService\EntityModel.cs:10-22`; `src\eShopWinForms\Program.cs:22-27`; `src\eShopWinForms\Controllers\CatalogController.cs:75-84`.

## Service Communication Sequence

```mermaid
sequenceDiagram
    participant Operator
    participant View as "CatalogView"
    participant Controller as "CatalogController"
    participant Proxy as "Generated WCF client"
    participant Service as "CatalogService"
    participant Data as "EntityModel"
    Operator->>View: Change catalog filters
    View->>Controller: Filter event
    Controller->>View: Clear displayed grid
    Controller->>Proxy: GetCatalogItems(int, int)
    Proxy->>Service: SOAP GetCatalogItems
    alt Service call succeeds
        Service->>Data: Materialize catalog and apply filters
        Data-->>Service: Catalog items
        Service-->>Proxy: List of CatalogItem
        Proxy-->>Controller: CatalogItem array
        Controller->>Proxy: GetDiscount(DateTime)
        Proxy->>Service: SOAP GetDiscount
        Service->>Data: Find discount covering supplied day
        Data-->>Service: Discount or null
        Service-->>Proxy: DiscountItem or null
        Proxy-->>Controller: Discount result
        Controller->>View: Items and discount fraction
    else SOAP or transport failure
        Proxy-->>Controller: Exception
        Note over Controller,View: No retry or fallback; grid was already cleared
    end
```

Sequence evidence: `src\eShopWinForms\Controllers\CatalogController.cs:27-29,75-84`; `src\eShopWCFService\CatalogService.svc.cs:28-30,56-62`. No deployed availability or wire-level behavior was tested.
