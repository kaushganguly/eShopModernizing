# Data Architecture & Persistence Layer

The deployment manifests describe a SQL Server container shared by the MVC and Web Forms services. Entity models, repositories, schema management, and application data-access code are not included in the scoped workspace.

## Database Configuration

| Service/Module | DB Type | Profile | Driver | Connection | Migration Tool |
|---|---|---|---|---|---|
| SQL container | SQL Server Developer 2017 | Container deployment | Not applicable | TCP 1433 | Not identified |
| MVC container | SQL Server | Default manifest configuration | Not identified | Service DNS `eshopmodernized.sqldata`, port 1433; credentials masked | Not identified |
| Web Forms container | SQL Server | Default manifest configuration | Not identified | Service DNS `eshopmodernized.sqldata`, port 1433; credentials masked | Not identified |

## Data Ownership per Service

| Service | Tables Owned | ORM Framework | Caching | Notes |
|---|---|---|---|---|
| SQL container | Not discoverable | Not discoverable | Not identified | Database service is deployed independently |
| MVC container | Not discoverable | Not discoverable | Not identified | Connection configured to shared SQL service |
| Web Forms container | Not discoverable | Not discoverable | Not identified | Connection configured to shared SQL service |

## Entity Model

No entity or schema definitions are included in the ServiceFabric workspace; therefore, no entity relationships can be inferred.

```mermaid
erDiagram
```

## Key Repository Methods

No repository interfaces or data-access source files are present in scope.

## Caching Strategy

No caching provider or cache configuration is declared by the deployment manifests.

## Data Ownership Boundaries

The deployment exposes one SQL container service and configures both web containers to connect to it. Whether the applications use separate schemas, tables, or data ownership boundaries cannot be determined without their source or database schema.

### Data Classification & Sensitivity

| Entity | Sensitive Fields | Classification (PII/PHI/PCI/None) | Controls in Place |
|---|---|---|---|
| Application entities | Not available in scope | Undetermined | Cannot be assessed from deployment manifests |
