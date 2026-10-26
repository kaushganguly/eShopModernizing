# Assessment Overview

This assessment covers only `eShopLegacyNTier`. The following supplementary documents describe the inspected application, with source references and diagrams.

| Document | Description |
|---|---|
| [Architecture diagram](architecture-diagram.md) | Application layers, technology stack, and component relationships. |
| [Dependency map](dependency-map.md) | Declared external dependencies, versions, compatibility concerns, and test infrastructure. |
| [API and service contracts](api-service-contracts.md) | SOAP operations, request and response contracts, communication patterns, and service sequence. |
| [Data architecture](data-architecture.md) | EF6 entities, relationships, persistence, data ownership, and sensitivity. |
| [Configuration inventory](configuration-inventory.md) | Configuration sources, build profiles, settings, startup dependencies, and masked sensitive configuration. |
| [Business workflows](business-workflows.md) | Catalog workflows, business rules, cross-service flows, and decision logic. |
| [Security assessment](security-assessment.md) | Merged CVE and CWE findings, severity counts, evidence, and coverage limitations. |
| [Security assessment data](security-assessment.json) | Machine-readable merged security results. |
| [CVE scan data](cve-assessment-result.json) | Dependency advisory findings before severity normalization. |
| [File and path security scan](result-cwe-file-path-security.json) | Results for five file and path rules. |
| [Injection scan](result-cwe-injection-attacks.json) | Results for twelve injection rules. |
| [Memory safety scan](result-cwe-memory-safety.json) | Results for fifteen memory safety rules. |
| [Code quality scan](result-cwe-code-quality.json) | Results for sixteen code quality rules. |
| [Credentials and secrets scan](result-cwe-credentials-secrets.json) | Results for five credentials and secrets rules; sensitive values are not reproduced. |
| [Concurrency scan](result-cwe-concurrency-synchronization.json) | Results for six concurrency and synchronization rules. |

## Assessment outputs

- [Core assessment data](../report.json)
- [.NET 10 upgrade assessment](../scenarios/dotnet-version-upgrade/assessment.md)

The upgrade assessment tool emitted its artifacts outside the requested output directory despite the supplied settings; its five generated artifacts were copied unchanged into this versioned report.
