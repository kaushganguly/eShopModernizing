# Assessment Overview

Scope: `eShopLegacyMVCSolution`, including all three projects in `eShopLegacyMVC.sln`. These documents describe the checked-in application; routes and workflows were assessed statically, not exercised against a running deployment.

| Document | Description |
|---|---|
| [Architecture diagram](architecture-diagram.md) | Application layers, technology stack, and component relationships. |
| [Dependency map](dependency-map.md) | Project references, package dependencies, and manifest discrepancies. |
| [API and service contracts](api-service-contracts.md) | MVC endpoints, internal service contracts, and request sequences. |
| [Data architecture](data-architecture.md) | Catalog entities, persistence, initialization, and mock-data behavior. |
| [Configuration inventory](configuration-inventory.md) | Configuration sources, environment differences, and external settings. |
| [Business workflows](business-workflows.md) | Catalog browsing, creation, editing, deletion, and startup sequences. |

The applications configure different databases, and the incomplete port defaults to mock data. Dependency manifest discrepancies limit certainty about deployed package versions. See the [core report](../report.md) and [.NET 10 upgrade assessment](../scenarios/dotnet-version-upgrade/assessment.md) for migration findings.

The [security assessment](../security/security-assessment.md) includes dependency exposures, code findings, and coverage limitations. Four CWE classifications describe one arbitrary-file-read root issue, not four independent vulnerabilities.
