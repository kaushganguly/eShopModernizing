# Architecture Diagram

This assessment describes the Service Fabric deployment assets in this workspace. The application containers' implementation source is not included, so internal application components are shown only at the service boundary.

## Application Architecture

```mermaid
flowchart TD
    subgraph Clients["Client Layer"]
        Browser["Web clients"]
    end
    subgraph Fabric["Azure Service Fabric Cluster"]
        subgraph Web["Web Container Services"]
            MVC["MVC container HTTP endpoint 5115"]
            Forms["WebForms container HTTP endpoint 5114"]
        end
        SQL[("SQL Server 2017 container TCP 1433")]
    end
    subgraph Azure["Optional Azure Integrations"]
        AAD["Azure Active Directory"]
        Storage["Azure Storage"]
        Insights["Application Insights"]
        Vault["Key Vault certificate reference"]
    end
    Browser -->|"HTTP"| MVC
    Browser -->|"HTTP"| Forms
    MVC -->|"SQL connection via Service Fabric DNS"| SQL
    Forms -->|"SQL connection via Service Fabric DNS"| SQL
    MVC -.->|"optional identity, storage, telemetry"| AAD
    MVC -.->|"optional storage and telemetry"| Storage
    MVC -.->|"optional telemetry"| Insights
    Fabric -.->|"cluster certificate configuration"| Vault
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---|---|
| Orchestration | Azure Service Fabric application projects | Not specified | Deploys and configures the container services |
| Web services | MVC and WebForms containers | Image tags not specified | Exposes HTTP endpoints to clients |
| Data | SQL Server Windows container | 2017 | Hosts the catalog database |
| Infrastructure | Azure Resource Manager templates | Not specified | Defines cluster infrastructure and certificate references |

### Data Storage & External Services

The MVC and WebForms container manifests configure SQL connections to a shared catalog database hosted by the SQL container and addressed through Service Fabric DNS. Application Insights, Azure Storage, Azure Active Directory, and Key Vault certificate references are represented in deployment configuration; their use is optional or parameterized.

### Key Architectural Decisions

- Web and database workloads are packaged as separate Service Fabric container services.
- The SQL service has a fixed instance count, while the web service instance counts are configured for dynamic placement.
- Container services use Service Fabric DNS names rather than hard-coded cluster node addresses for internal database connectivity.

## Component Relationships

```mermaid
flowchart LR
    subgraph Presentation["Presentation Services"]
        MVC["MVC container"]
        Forms["WebForms container"]
    end
    subgraph DataAccess["Data Service"]
        SQL["SQL Server container"]
    end
    subgraph Platform["Platform"]
        SF["Service Fabric cluster and DNS"]
    end
    MVC -->|"catalog database connection"| SQL
    Forms -->|"catalog database connection"| SQL
    SF -.->|"hosts and resolves services"| MVC
    SF -.->|"hosts and resolves services"| Forms
    SF -.->|"hosts and resolves services"| SQL
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| MVC container service | Presentation | Stateless container service | Hosts the MVC web application; its implementation is not present in this workspace |
| WebForms container service | Presentation | Stateless container service | Hosts the WebForms application; its implementation is not present in this workspace |
| SQL container service | Data | Stateless container service | Hosts SQL Server and the configured catalog database |
| Service Fabric cluster | Infrastructure | Container orchestrator | Hosts services and provides service naming and deployment management |
