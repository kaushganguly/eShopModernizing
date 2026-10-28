# Assessment Overview

This assessment covers only `eShopLegacyNTier`, containing the WinForms client and WCF catalog service. Application source was not modified.

## Supplementary documents

| Document | Description |
| --- | --- |
| [Architecture diagram](architecture-diagram.md) | Application layers, technology stack, and component relationships. |
| [Dependency map](dependency-map.md) | Project, framework, and package dependencies and service boundaries. |
| [API and service contracts](api-service-contracts.md) | SOAP operations, data contracts, client interactions, and contract discrepancies. |
| [Data architecture](data-architecture.md) | Entity Framework persistence, catalog entities, relationships, and initialization. |
| [Configuration inventory](configuration-inventory.md) | Application settings, endpoints, database configuration, and deployment settings, with sensitive values masked. |
| [Business workflows](business-workflows.md) | Catalog browsing, filtering, maintenance, stock, and discount workflows with sequence diagrams. |

## Assessment context and limitations

- Core analysis: .NET AppCAT CLI `1.0.1127`, Restricted privacy mode, started at `2026-10-28T23:04:31.3593762Z`. The automated report contains 8 issue types and 54 incidents across the two projects; these are migration findings, not counts of confirmed security vulnerabilities.
- The WCF service targets .NET Framework 4.6.1; the WinForms client targets .NET Framework 4.7.
- The upgrade assessment used the requested `net10.0` target. A future WinForms migration requires a Windows-specific target such as `net10.0-windows`; generic-target incompatibility counts should not be read as proof that WinForms is unavailable on modern .NET.
- These are static assessments, not a completed upgrade, deployment validation, runtime security test, or certification. No test project was found within the assessed directory.
