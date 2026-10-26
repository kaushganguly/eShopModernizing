# Assessment Overview

This assessment covers only `eShopModernizedNTier`. Application source and configuration were not modified.

## Supplementary documents

| Document | Description |
|---|---|
| [Architecture diagram](architecture-diagram.md) | Desktop, WCF service, persistence and declared deployment boundaries. |
| [Dependency map](dependency-map.md) | Project, package and external dependency relationships. |
| [API and service contracts](api-service-contracts.md) | SOAP operations, data contracts, client calls and communication sequences. |
| [Data architecture](data-architecture.md) | Catalog entities, relationships, persistence and database lifecycle. |
| [Configuration inventory](configuration-inventory.md) | Configuration sources, environment overrides and deployment settings, with credentials masked. |
| [Business workflows](business-workflows.md) | Catalog browsing, item maintenance and stock workflows with sequence diagrams. |

## Assessment scope and limitations

- The core assessment scanned all three scoped C# project files, including the legacy `.fx.csproj` alternative. Its 8 issue groups represent 103 incidents, not 103 distinct rules.
- The .NET 10 upgrade assessment followed `eShopModernizedNTier.sln`, which includes two C# projects. Its five generated artifacts are preserved under `../scenarios/dotnet-version-upgrade/`.
- The upgrade MCP tool wrote to its default scenario directory despite the configured output path; the generated artifacts were copied into this versioned report without changing application code.
- These are static assessments, not proof of deployed behavior. Database availability, runtime connectivity, TLS, deployment secrets and production settings were not exercised.
- Source citations describe this repository snapshot. Security results and their coverage limitations are included in the consolidated report.

## Security results

The [security assessment](../security/security-assessment.md) records 7 CVE advisories at medium severity or above and 10 CWE rule matches across 59 checks. The [machine-readable summary](../security/security-assessment.json) and seven individual scan result files are preserved alongside it.

The dependency CLI could not load legacy web application targets. Advisory matching used manifests and existing resolved assets, which may be stale; a complete fresh service dependency graph was unavailable. These findings do not establish which packages are deployed. Several CWE classifications overlap and must not be interpreted as independent exploits.
