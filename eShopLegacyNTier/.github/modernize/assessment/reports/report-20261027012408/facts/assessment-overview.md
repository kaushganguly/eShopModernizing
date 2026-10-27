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
- [Security assessment](../security/security-assessment.md), with the merged findings and all seven raw CVE/CWE result files preserved in the adjacent `security/` directory.

## Interpretation

The core cloud-readiness assessment and the .NET version-upgrade assessment address different concerns; their issue totals are not interchangeable. Static findings and migration recommendations are not evidence of runtime testing or of deployed security controls.

The supplied renderers use the first project's name and framework for the main report heading and component information. Although that heading names `eShopWCFService`, the report data and combined issue lists cover both the service and `eShopWinForms`.

The security assessment covers the six requested CWE checklists, not every possible weakness category. CWE finding counts include overlapping checks for the same underlying issue and must not be interpreted as counts of independent vulnerabilities. Uncompiled helper patterns without an established production call path are not counted as production findings.

The dependency inventory command could not load the legacy web project because `Microsoft.WebApplication.targets` was unavailable. The CVE scan therefore used the checked-in package manifests; transitive dependency resolution remains unverified. CVE filtering uses a minimum severity of **medium**. A dependency advisory identifies an affected package version, not proof that its vulnerable behavior is reachable in this application.
