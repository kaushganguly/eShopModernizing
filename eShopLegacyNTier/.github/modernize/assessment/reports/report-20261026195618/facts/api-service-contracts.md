# API & Service Communication Contracts

One source-defined SOAP application endpoint exposes **10 WCF operations**, with separate metadata exposure; there are no source-defined REST routes. This is a static contract assessment of `eShopLegacyNTier` only; paths below are relative to that root and deployed compatibility is unverified.

## Service Catalog

| Service | Port | Category | Purpose |
|---|---|---|---|
| eShopWCFService | 62314, development setting only | Business | IIS-hosted CatalogService; WCF and EF declared in `src\eShopWCFService\eShopWCFService.csproj:45-65,119-123` |
| eShopWinForms | No listening port declared | Client, not API Layer | Independently packaged desktop consumer; Windows Forms and WCF references (`src\eShopWinForms\eShopWinForms.csproj:8-11,77-85`) |

The scoped solution contains only these projects (`eShopLegacyNTier.sln:6-9`). No source-built gateway, discovery, messaging, observability service or third-party container definition was found in scope.

## API Endpoints Inventory

All rows below are **SOAP POST operations on `/CatalogService.svc`**, not REST resource endpoints. Server source is authoritative: `src\eShopWCFService\ICatalogService.cs:15-34`; activation is `src\eShopWCFService\CatalogService.svc:1`, binding is `src\eShopWCFService\Web.config:38-40`.

| Service | Method | Path | Request Type | Response Type |
|---|---|---|---|---|
| CatalogService | POST / SOAP FindCatalogItem | `/CatalogService.svc` | int id | CatalogItem or null |
| CatalogService | POST / SOAP GetCatalogBrands | Same SOAP endpoint | No parameters | List of CatalogBrand |
| CatalogService | POST / SOAP GetCatalogItems | Same SOAP endpoint | int brandIdFilter, int typeIdFilter | List of CatalogItem |
| CatalogService | POST / SOAP GetCatalogTypes | Same SOAP endpoint | No parameters | List of CatalogType |
| CatalogService | POST / SOAP GetAvailableStock | Same SOAP endpoint | DateTime date, int catalogItemId | int; zero if no matching record |
| CatalogService | POST / SOAP CreateAvailableStock | Same SOAP endpoint | CatalogItemsStock | void SOAP reply |
| CatalogService | POST / SOAP CreateCatalogItem | Same SOAP endpoint | CatalogItem | void SOAP reply; generated ID not returned |
| CatalogService | POST / SOAP UpdateCatalogItem | Same SOAP endpoint | CatalogItem | void SOAP reply |
| CatalogService | POST / SOAP RemoveCatalogItem | Same SOAP endpoint | CatalogItem | void SOAP reply |
| CatalogService | POST / SOAP GetDiscount | Same SOAP endpoint | DateTime day | DiscountItem or null |

SOAP actions in the generated snapshot use `http://tempuri.org/ICatalogService/<Operation>` and corresponding `<Operation>Response` reply actions (`src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs:501-559`). No explicit API versioning, FaultContract, REST status contract, or WebGet/WebInvoke mapping is defined. Normal request/reply and SOAP faults follow WCF; specific HTTP status behavior was not exercised. IDisposable is inherited by the interface but is **not** an OperationContract.

## Management & Observability Endpoints

| Service | Endpoint | Custom Metrics (if any) |
|---|---|---|
| CatalogService | `/CatalogService.svc?wsdl`, HTTP metadata enabled | None declared |
| CatalogService | `/CatalogService.svc/mex`, mexHttpBinding | None declared |
| CatalogService | HTTPS metadata enabled as a setting, no working TLS listener verified | None declared |
| Desktop / service | No dedicated health, readiness, metrics or Swagger endpoint found | None declared |

Evidence: `src\eShopWCFService\Web.config:25-40`. Metadata is discovery material, not an application operation or health check.

## DTOs & Contracts

| Contract class | API role | Mutability / ownership |
|---|---|---|
| CatalogItem | Catalog query response and create/update/remove request | Mutable service-owned data contract |
| CatalogBrand, CatalogType | Lookup response | Mutable service-owned data contracts |
| CatalogItemsStock | Availability write request | Mutable service-owned data contract |
| DiscountItem | Date-based promotion response | Mutable service-owned data contract |

The server's entity classes double as `[DataContract]` / `[DataMember]` DTOs; there is no gateway aggregation DTO or immutable contract type. See `data-architecture.md` for fields and relationships. SOAP XML serialization uses WCF data contracts, not the merely declared Json.NET dependency.

**Generated/stale artifacts:** the desktop explicitly compiles `Connected Services\eShopServiceReference\Reference.cs` (`src\eShopWinForms\eShopWinForms.csproj:89-92`), whose generated models support data binding. Its `GetCatalogItems(int?, int?)` differs from current server `GetCatalogItems(int,int)` (`Reference.cs:513-517`; server interface:20); collections become arrays on the generated client. Current controller passes concrete integers, but this does not establish compatibility for null callers or prove the WSDL is current. The `.wsdl`, four `.xsd` files and `.svcmap` are saved generation artifacts, not live observations (`src\eShopWinForms\Connected Services\eShopServiceReference\Reference.svcmap:21-30`). No OpenAPI, protobuf or GraphQL definition was found.

The handwritten `src\eShopWCFService\CatalogServiceClient.cs:11-38` has unimplemented methods and an obsolete parameterless call; it is **not included** in the service project's explicit compile items (`src\eShopWCFService\eShopWCFService.csproj:79-94`). Do not treat it as the active desktop proxy.

## Communication Patterns

- **Synchronous:** desktop controller invokes generated WCF client synchronously. Generated Task wrappers exist but are not used by these workflows (`src\eShopWinForms\Controllers\CatalogController.cs:35-84`; `Reference.cs:501-559`). No queue, event bus, callback or one-way messaging is wired.
- **Composition:** the desktop obtains catalog items and the current discount separately and calculates display pricing. This is client composition within one service, not a gateway calling multiple domains.
- **Resilience:** no application retry, circuit breaker, fallback, bulkhead or explicit binding timeout is configured. The empty named basicHttpBinding relies on WCF defaults; no timeout values are source-pinned (`src\eShopWinForms\App.config:20-23`). Errors propagate rather than returning a documented degraded response.
- **Addressing:** fixed development HTTP service address; no service discovery or load-balancing component. Startup requires the service and database to work before the dialog appears; details in `configuration-inventory.md`.
- **Security:** the active client and explicit service endpoint use HTTP/basicHttpBinding without configured authentication, transport security or application authorization checks. HTTPS protocol mapping and metadata flags are present but do not prove a TLS-protected active endpoint (`src\eShopWCFService\Web.config:27-40`). External IIS policy is unknown; this assessment does not claim actual Internet exposure. Exception details are disabled in SOAP faults.

## Service Technology Matrix

| Service | Web | Data Access | Discovery | Gateway | Actuator | Cache | Metrics |
|---|---|---|---|---|---|---|---|
| eShopWCFService | IIS / WCF SOAP | EF6 | None | None | Metadata only, not health | None configured | None configured |
| eShopWinForms | Desktop, WCF client | No active direct database workflow found | Fixed address | Client composition only | None | No application result cache found | None configured |

## Service Communication Sequence

```mermaid
sequenceDiagram
    participant User
    participant View as "CatalogView"
    participant Ctrl as "CatalogController"
    participant Proxy as "Generated WCF client"
    participant Svc as "CatalogService"
    participant EF as "EntityModel and SQL Server"
    User->>View: Change brand or type filter
    View->>Ctrl: Filter event with integer IDs
    Ctrl->>View: Clear displayed catalog
    Ctrl->>Proxy: GetCatalogItems filters
    Proxy->>Svc: SOAP GetCatalogItems
    Svc->>EF: Materialize catalog rows
    EF-->>Svc: CatalogItem records
    Svc->>Svc: Apply filters in memory
    Svc-->>Proxy: CatalogItem collection
    Proxy-->>Ctrl: Generated CatalogItem array
    Ctrl->>Proxy: GetDiscount current local day
    Proxy->>Svc: SOAP GetDiscount
    Svc->>EF: Materialize discount records
    EF-->>Svc: Discount records
    Svc-->>Proxy: First active discount or null
    Proxy-->>Ctrl: DiscountItem or null
    alt Both calls succeed
        Ctrl->>View: Set catalog and discount amount
        View-->>User: Prices and local product images
    else A service call faults
        Note over Ctrl,View: Exception propagates; no retry or fallback implemented
    end
```

Trace: `src\eShopWinForms\Controllers\CatalogController.cs:27-30,75-84`; `src\eShopWCFService\CatalogService.svc.cs:28-30,56-62`; `src\eShopWinForms\Views\CatalogView.cs:48-61`. Fault block describes missing handling, not an implemented circuit breaker.

**Limitations:** no build, tests, requests, metadata refresh or runtime verification; generated contract drift is explicitly retained, not silently resolved.
