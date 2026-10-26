# Architecture Diagram

The scoped solution deploys three containerized services to Service Fabric. Application internals are not included; the diagrams describe only the deployment topology and manifest-defined connections.

## Application Architecture

```mermaid
flowchart TD
    subgraph Clients["Client Layer"]
        Browser["Web client"]
    end
    subgraph Fabric["Service Fabric deployment"]
        MVC["MVC container HTTP 5115"]
        WebForms["Web Forms container HTTP 5114"]
        SQL[("SQL Server Developer 2017 TCP 1433")]
    end

    Browser -->|"HTTP"| MVC
    Browser -->|"HTTP"| WebForms
    MVC -->|"SQL connection"| SQL
    WebForms -->|"SQL connection"| SQL
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---|---|
| Deployment | Azure Service Fabric application projects | Project version 1.7 | Package and deploy container services |
| Web runtime | MVC container image | Image contents not in scope | Web application deployment |
| Web runtime | Web Forms container image | Image contents not in scope | Web application deployment |
| Data | SQL Server Developer container | 2017 | Shared catalog database deployment |

### Data Storage & External Services

The SQL container exposes TCP port 1433 and is referenced by both web service manifests using the Service Fabric DNS name `eshopmodernized.sqldata`. No cache, message broker, or external API configuration is visible in the scoped project.

### Key Architectural Decisions

- MVC and Web Forms applications are deployed as separate container services.
- Both web services are configured to connect to the SQL Server service.
- The application source and runtime dependencies inside the web images are outside this workspace.

## Component Relationships

```mermaid
flowchart LR
    subgraph Deployment["Deployment components"]
        Solution["Service Fabric solution"]
        MVCApp["MVC application manifest"]
        WebApp["Web Forms application manifest"]
        SQLApp["SQL application manifest"]
        MVCContainer["MVC container"]
        WebContainer["Web Forms container"]
        SQLContainer["SQL container"]
    end

    Solution -->|"packages"| MVCApp
    Solution -->|"packages"| WebApp
    Solution -->|"packages"| SQLApp
    MVCApp -->|"starts"| MVCContainer
    WebApp -->|"starts"| WebContainer
    SQLApp -->|"starts"| SQLContainer
    MVCContainer -->|"database connection"| SQLContainer
    WebContainer -->|"database connection"| SQLContainer
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| Service Fabric solution | Deployment | Solution | Groups the three deployment projects |
| MVC application manifest | Deployment | Manifest | Configures the MVC container package and service |
| Web Forms application manifest | Deployment | Manifest | Configures the Web Forms container package and service |
| SQL application manifest | Deployment | Manifest | Configures the SQL container package and service |
| MVC, Web Forms, and SQL containers | Runtime | Containers | Execute the images referenced by the manifests |

The scoped files do not include application controllers, services, repositories, or other internal components.
