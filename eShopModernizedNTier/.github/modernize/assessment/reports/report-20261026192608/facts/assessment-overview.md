# Assessment overview

Scope: `eShopModernizedNTier` only. This assessment does not modify the application.

## Supplementary documents

- [Architecture diagram](architecture-diagram.md) — Deployment boundaries, technology stack, and component relationships.
- [Dependency map](dependency-map.md) — Declared packages, framework references, container dependencies, and compatibility risks.
- [API and service contracts](api-service-contracts.md) — SOAP operations, data contracts, communication patterns, and endpoint configuration.
- [Data architecture](data-architecture.md) — Entity relationships, persistence ownership, initialization, and data access.
- [Configuration inventory](configuration-inventory.md) — Configuration sources, environment overrides, deployment settings, and operational gaps; credential values are redacted.
- [Business workflows](business-workflows.md) — Catalog browsing, discounts, stock lookup, and stock replacement sequences.

## Related reports

- [Assessment report](../report.md) — Combined cloud-readiness, upgrade, and security findings.
- [Interactive assessment report](../report.html) — Rendered findings, diagrams, and source locations.
- [Security assessment](../security/security-assessment.md) — Findings, checklist counts, methodology, and coverage limitations.
- [.NET 10 upgrade assessment](../scenarios/dotnet-version-upgrade/assessment.md) — Solution-selected project and API compatibility analysis.

## Interpretation and limitations

- The core assessment includes all three project files, including the retained legacy desktop project. The .NET 10 upgrade assessment covers the two projects selected by the solution.
- The generated summary cards use the first component's name and select individual framework values from the multi-project report; HTML and Markdown show different framework values. Use the architecture stack table and upgrade project details for project-specific targets, not the summary card as a solution-wide framework declaration.
- The supplied HTML renderer loads Mermaid from an external CDN, which was blocked during local preview. The report tabs remain usable and diagram source is preserved in the supplementary Markdown documents.
- Architecture and code-security findings are based on static analysis, not a deployed-system or penetration test.
- The CVE scan covers declared package/version pairs only. Missing `Microsoft.WebApplication.targets` prevented complete transitive dependency enumeration. A zero-CVE result does not establish that all dependencies are vulnerability-free.
- CWE findings are checklist mappings, not necessarily independent vulnerabilities. In particular, CWE-22, CWE-23, and CWE-99 describe the same image-path data flow.
- The upgrade assessment server wrote its artifacts to its default scenario directory despite the configured output path. Its five generated artifacts were copied unchanged into this versioned report.
