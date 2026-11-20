# Architecture Diagram

The scoped workspace contains Service Fabric deployment packages for the MVC and Web Forms front ends and a SQL Server container. Application implementation code is outside this workspace, so the diagrams describe the deployed topology and not internal application components.

## Application Architecture

```mermaid
flowchart TD
    subgraph Client["Client Layer"]
        Browser["Web Client"]
    end
    subgraph Fabric["Service Fabric Application"]
        MVC["MVC container service"]
        WebForms["Web Forms container service"]
        SQL["SQL Server 2017 container"]
    end
    subgraph Data["Data Layer"]
        CatalogDB[("Catalog database")]
    end
    subgraph Optional["Optional Integrations"]
        Storage["Azure Storage disabled by default"]
        Identity["Azure Active Directory disabled by default"]
        Insights["Application Insights key unset by default"]
    end

    Browser -->|"HTTP port 5115"| MVC
    Browser -->|"HTTP port 5114"| WebForms
    MVC -->|"SQL endpoint 1433"| SQL
    WebForms -->|"SQL endpoint 1433"| SQL
    SQL -->|"hosts"| CatalogDB
    MVC -.->|"optional configuration"| Storage
    WebForms -.->|"optional configuration"| Storage
    MVC -.->|"optional configuration"| Identity
    WebForms -.->|"optional configuration"| Identity
    MVC -.->|"optional configuration"| Insights
    WebForms -.->|"optional configuration"| Insights
```

### Technology Stack Summary

| Layer | Technology | Version | Purpose |
|---|---|---|---|
| Deployment | Azure Service Fabric application projects | Project version 1.7 | Packages and deploys container services |
| Presentation | MVC container image `eshop/modernizedmvc` | Not pinned in manifest | Customer-facing web application |
| Presentation | Web Forms container image `eshop/modernizedwebforms` | Not pinned in manifest | Customer-facing web application |
| Data | SQL Server Windows Developer container | 2017 | Catalog database service |
| Project tooling | .NET Framework target | 4.6.1 / 4.6.2 | Service Fabric project build targets |

### Data Storage & External Services

Both front-end service manifests declare a SQL connection to the Service Fabric SQL service endpoint (`eshopmodernized.sqldata:1433`). Azure Storage and Azure Active Directory are configurable but disabled by default; the Application Insights instrumentation key is empty by default. The application source and database schema are not included in this scoped workspace.

### Key Architectural Decisions

- MVC, Web Forms, and SQL are deployed as separate Service Fabric container service packages.
- Service Fabric DNS names and manifest port bindings connect the front ends to the SQL container.
- Optional cloud integrations are externalized as application parameters and disabled or unset by default.

## Component Relationships

```mermaid
flowchart LR
    subgraph Frontends["Presentation Services"]
        MVCManifest["MVC service manifest"]
        WebManifest["Web Forms service manifest"]
    end
    subgraph Deployment["Deployment Configuration"]
        AppManifest["Service Fabric application manifest"]
        Params["Environment parameter files"]
    end
    subgraph Persistence["Persistence Service"]
        SQLManifest["SQL service manifest"]
        SQLContainer["SQL Server container"]
    end

    AppManifest -->|"imports"| MVCManifest
    AppManifest -->|"imports"| WebManifest
    AppManifest -->|"imports"| SQLManifest
    Params -->|"sets deployment parameters"| AppManifest
    MVCManifest -->|"declares SQL connection"| SQLContainer
    WebManifest -->|"declares SQL connection"| SQLContainer
    SQLManifest -->|"starts image"| SQLContainer
```

### Component Inventory

| Component | Layer | Type | Responsibility |
|---|---|---|---|
| Application manifests | Deployment | Service Fabric manifest | Defines service packages, parameters, DNS names, and instance counts |
| MVC service package | Presentation | Container service | Runs the MVC image on the declared HTTP endpoint |
| Web Forms service package | Presentation | Container service | Runs the Web Forms image on the declared HTTP endpoint |
| SQL service package | Persistence | Container service | Runs SQL Server and exposes TCP port 1433 |
| Application parameter files | Deployment | Environment configuration | Supplies Local and Cloud deployment values |
