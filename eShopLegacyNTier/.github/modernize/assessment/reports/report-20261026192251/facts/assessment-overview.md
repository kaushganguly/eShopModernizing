# Assessment Overview

This assessment covers only `eShopLegacyNTier`, including its WCF service and WinForms client. Application source and project files were not changed.

## Supplementary documents

| Document | Description |
|---|---|
| [Architecture diagram](architecture-diagram.md) | Application layers, deployment boundaries, technology stack, and component relationships. |
| [Dependency map](dependency-map.md) | Declared framework and package dependencies, compatibility risks, and test dependency coverage. |
| [API and service contracts](api-service-contracts.md) | SOAP operations, request and response contracts, communication patterns, and transport configuration. |
| [Data architecture](data-architecture.md) | Catalog entities, persistence, ownership, caching, and data classification. |
| [Configuration inventory](configuration-inventory.md) | Configuration sources, build settings, runtime dependencies, and sensitive configuration handling. |
| [Business workflows](business-workflows.md) | Catalog browsing and maintenance flows, validation, and business decisions. |

## Assessment provenance

- Core assessment: .NET AppCAT CLI 1.0.1127, restricted privacy mode, started at `2026-10-26T19:22:51.7392477Z`.
- Upgrade assessment: the scoped solution was assessed for `net10.0`; see [upgrade assessment](../scenarios/dotnet-version-upgrade/assessment.md).
- Security assessment: see the [merged findings and coverage limitations](../security/security-assessment.md). The [raw scan results](../security/) preserve one CVE scan and all six sequential CWE checklists. CVE filtering includes medium severity and above; findings remain unremediated because this task is assessment only.
- The upgrade MCP server wrote its artifacts outside the requested output directory despite the output settings. Its five generated assessment artifacts were copied into this versioned report without changing their analysis.
- These are static assessments, not evidence that the application was deployed or exercised against a live database.
- The bundled HTML renderer loads Mermaid from a CDN; diagram rendering requires access to that resource.
