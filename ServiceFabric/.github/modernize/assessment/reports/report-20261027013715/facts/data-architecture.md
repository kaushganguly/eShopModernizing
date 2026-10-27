# Data Architecture & Persistence Layer

The deployment manifests declare one SQL Server container and configure both web workloads to target a catalog database. Entity models, ORM configuration, repositories, and schema definitions are not included in the Service Fabric workspace scope.

## Database Configuration

| Service/Module | DB Type | Profile | Driver | Connection | Migration Tool |
|---|---|---|---|---|---|
| SQL container | SQL Server Developer 2017 | Service Fabric application | Not specified | TCP 1433; service DNS `eshopmodernized.sqldata` | Not specified |
| MVC and Web Forms workloads | SQL Server | Manifest-configured | Not specified | Catalog database name declared in connection setting; credentials omitted | Not specified |

## Data Ownership per Service

| Service | Tables Owned | ORM Framework | Caching | Notes |
|---|---|---|---|---|
| SQL container | Not identified | Not specified | Not specified | Hosts the declared database container; schema is not included |
| MVC container | Not identifiable | Not specified | Not specified | Connection to the catalog database is configured |
| Web Forms container | Not identifiable | Not specified | Not specified | Connection to the catalog database is configured |

## Entity Model

No entities, schema objects, or relationship definitions were found in the deployment descriptors; an ER model cannot be derived without inventing schema details.

## Key Repository Methods

No repository interfaces or data-access source files are included in this workspace scope.

## Caching Strategy

No cache provider, cache policy, or cache configuration is declared in the Service Fabric manifests.

## Data Ownership Boundaries

The manifests show a single SQL container DNS target shared by the MVC and Web Forms workloads. They do not establish whether the two applications use shared tables, separate schemas, or independent database ownership. No migrations, schema-per-service boundaries, CQRS patterns, or cross-service data access code are available.

### Data Classification & Sensitivity

| Entity | Sensitive Fields | Classification (PII/PHI/PCI/None) | Controls in Place |
|---|---|---|---|
| Not available | Not available | Not determined | Application data model and encryption/masking controls are outside the deployment scope |