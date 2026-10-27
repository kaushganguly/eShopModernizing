# Assessment overview

Scope: `eShopModernizedNTier` only. No application source was changed.

## Supplementary documentation

- [Architecture diagram](architecture-diagram.md) — Desktop client, SOAP service, database, deployment boundaries, and runtime relationships.
- [Dependency map](dependency-map.md) — Active solution projects, alternate framework project, package dependencies, and deployment dependencies.
- [API and service contracts](api-service-contracts.md) — SOAP operations, message types, client configuration, and service interaction sequences.
- [Data architecture](data-architecture.md) — Entity Framework model, SQL persistence, initialization, and mock-data behavior.
- [Configuration inventory](configuration-inventory.md) — Application, service, container, environment, and build settings, with credential values omitted.
- [Business workflows](business-workflows.md) — Catalog browsing, filtering, editing, image retrieval, and database initialization sequences.

## Assessment scope and interpretation

The [core report](../report.json) assesses all three project files in this directory, including the alternate framework project. The [solution-based .NET 10 assessment](../scenarios/dotnet-version-upgrade/assessment.md) assesses the two projects included in the solution. Their issue counts and effort estimates describe different analyses and must not be interpreted as interchangeable totals.

The upgrade assessment tool wrote its five artifacts to its default scenario directory despite the requested output settings. Those generated artifacts were copied unchanged into this versioned report's `scenarios/dotnet-version-upgrade/` directory.

Architecture documentation describes both active execution paths and explicitly identified alternatives; the presence of a mock implementation does not establish that it is selected at runtime. Security findings are static assessment results, not proof of exploitability or a production security certification.

## Security assessment

The [security report](../security/security-assessment.md) records 7 CVE findings at or above the medium threshold and 13 CWE findings across 59 checks. Some CWE mappings overlap or concern inactive code; consult each finding's reachability evidence before prioritizing remediation. The [machine-readable summary](../security/security-assessment.json) and seven raw result files are preserved alongside it.

The dependency-list command failed because legacy `WebApplication.targets` was unavailable. The scan used project manifests, `packages.config`, and an existing WinForms restore graph that may be stale. Full service and alternate-project transitive dependency coverage was not established. Advisory queries succeeded; package presence alone does not establish vulnerable API execution or platform applicability.
