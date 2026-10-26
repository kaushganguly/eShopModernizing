# API & Service Communication Contracts

The manifests declare two HTTP container listeners and one SQL TCP listener. Application routes, DTOs, authentication middleware, and downstream client behavior are inside container images whose source is not present in this workspace.

## Service Catalog

| Service | Port | Category | Purpose |
|---|---|---|---|
| MVC_Container | Container 80, Service Fabric endpoint 5115 | API Layer | Hosts the prebuilt MVC container image |
| WebForms_Container | Container 80, Service Fabric endpoint 5114 | API Layer | Hosts the prebuilt Web Forms container image |
| SQL_Container | 1433/TCP | Infrastructure | Provides the SQL Server container endpoint |

## API Endpoints Inventory

| Service | Method | Path | Request Type | Response Type |
|---|---|---|---|---|
| MVC_Container | Not available | Not available | Application source is external to this subtree | Not available |
| WebForms_Container | Not available | Not available | Application source is external to this subtree | Not available |

The Service Fabric manifests expose HTTP listener ports but do not declare application URL paths or HTTP methods.

## Management & Observability Endpoints

No health, metrics, or management endpoints are declared in the scoped manifests. Application-level endpoints cannot be inspected because the container source is not included.

## DTOs & Contracts

No DTO or API contract source is included. The web application container images are referenced by name without their source or API schemas; serialization details are therefore unknown.

## Communication Patterns

The service manifests configure the MVC and Web Forms containers to use the SQL container's Service Fabric DNS name on TCP 1433. The web containers declare HTTP listener schemes; the manifests do not establish TLS. Azure Active Directory is configurable but defaults to disabled in the application manifests. Authentication and authorization inside the container applications cannot be determined from deployment metadata. No asynchronous message broker, retry policy, circuit breaker, or HTTP gateway aggregation is declared.

## Service Technology Matrix

| Service | Web | Data Access | Discovery | Gateway | Health | Cache | Metrics |
|---|---|---|---|---|---|---|---|
| MVC_Container | MVC image, framework unknown | SQL Server connection configured | Service Fabric DNS | No evidence | Not declared | Not declared | Instrumentation key parameter empty by default |
| WebForms_Container | Web Forms image, framework unknown | SQL Server connection configured | Service Fabric DNS | No evidence | Not declared | Not declared | Instrumentation key parameter empty by default |
| SQL_Container | SQL Server Developer 2017 | SQL Server | Service Fabric DNS | No | Not declared | N/A | Not declared |

## Service Communication Sequence

```mermaid
sequenceDiagram
    participant Client as "Web client"
    participant MVC as "MVC container"
    participant SQL as "SQL container"
    Client->>MVC: HTTP request to declared listener
    MVC->>SQL: SQL connection to catalog database on TCP 1433
    SQL-->>MVC: Database response
    MVC-->>Client: HTTP response; application contract is not visible here
```
