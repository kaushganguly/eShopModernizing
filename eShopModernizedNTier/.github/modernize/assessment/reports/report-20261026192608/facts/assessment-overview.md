# Assessment overview

Scope: `eShopModernizedNTier` only. This assessment does not modify the application.

## Supplementary documents

- [Architecture diagram](architecture-diagram.md) — Deployment boundaries, technology stack, and component relationships.
- [Dependency map](dependency-map.md) — Declared packages, framework references, container dependencies, and compatibility risks.
- [API and service contracts](api-service-contracts.md) — SOAP operations, data contracts, communication patterns, and endpoint configuration.
- [Data architecture](data-architecture.md) — Entity relationships, persistence ownership, initialization, and data access.
- [Configuration inventory](configuration-inventory.md) — Configuration sources, environment overrides, deployment settings, and operational gaps; credential values are redacted.
- [Business workflows](business-workflows.md) — Catalog browsing, discounts, stock lookup, and stock replacement sequences.

## Interpretation and limitations

- The core assessment includes all three project files, including the retained legacy desktop project. The .NET 10 upgrade assessment covers the two projects selected by the solution.
- Architecture and code-security findings are based on static analysis, not a deployed-system or penetration test.
- The CVE scan covers declared package/version pairs only. Missing `Microsoft.WebApplication.targets` prevented complete transitive dependency enumeration. A zero-CVE result does not establish that all dependencies are vulnerability-free.
- CWE findings are checklist mappings, not necessarily independent vulnerabilities. In particular, CWE-22, CWE-23, and CWE-99 describe the same image-path data flow.
- The upgrade assessment server wrote its artifacts to its default scenario directory despite the configured output path. Its five generated artifacts were copied unchanged into this versioned report.
