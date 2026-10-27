# Assessment Overview

This assessment covers only `eShopLegacyNTier`: the Windows Forms desktop client and its WCF catalog service. Paths cited in the supplementary documents are relative to that application directory. Diagrams describe checked-in source and configuration, not verified deployed infrastructure.

## Supplementary Documents

| Document | Description |
|---|---|
| [Architecture diagram](architecture-diagram.md) | Application layers, technology stack, component responsibilities, and interactions. |
| [Dependency map](dependency-map.md) | Declared packages, framework assemblies, project boundaries, and dependency inventory limitations. |
| [API and service contracts](api-service-contracts.md) | Ten SOAP operations, data contracts, endpoint configuration, and a service communication sequence. |
| [Data architecture](data-architecture.md) | Entity relationships, persistence mappings, initialization, and data access behavior. |
| [Configuration inventory](configuration-inventory.md) | Application settings, database connection selection, service bindings, and environment-specific configuration. |
| [Business workflows](business-workflows.md) | Catalog browsing, filtering, discounts, stock lookup, and shipment workflows with sequence diagrams. |

## Main Reports

- [Assessment report](../report.md) and [interactive HTML report](../report.html).
- [.NET 10 upgrade assessment](../scenarios/dotnet-version-upgrade/assessment.md), generated for both projects without modifying application code. The upgrade service initially wrote its artifacts to a repository-level scenario directory; those generated artifacts were copied into this versioned report.

## Interpretation

The core cloud-readiness assessment and the .NET version-upgrade assessment address different concerns; their issue totals are not interchangeable. Static findings and migration recommendations are not evidence of runtime testing or of deployed security controls.
