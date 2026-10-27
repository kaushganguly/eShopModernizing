# Assessment overview

This assessment covers only `eShopLegacyNTier`: the Windows Forms catalog client and its WCF service.

## Supplementary documents

- [Architecture diagram](architecture-diagram.md): application components, deployment boundaries, and communication paths.
- [Dependency map](dependency-map.md): project, framework, and package dependencies.
- [API and service contracts](api-service-contracts.md): WCF operations, message contracts, and interaction sequences.
- [Data architecture](data-architecture.md): entities, persistence, relationships, and database behavior.
- [Configuration inventory](configuration-inventory.md): settings, environment overrides, build profiles, and deployment assumptions.
- [Business workflows](business-workflows.md): catalog and stock workflows with sequence diagrams.

## Assessment outputs

- [Application assessment](../report.md): cloud-readiness, security, and upgrade findings.
- [Interactive report](../report.html): rendered assessment with detailed findings.
- [.NET 10 upgrade assessment](../scenarios/dotnet-version-upgrade/assessment.md): project, package, and API compatibility analysis.

## Scope and interpretation

The assessment does not change application source or execute an upgrade. Tool findings are migration indicators, not proof that the application builds or runs on .NET 10. The Windows Forms client requires a Windows-specific target; WCF server hosting requires a migration decision.

The upgrade assessment tool generated its artifacts in the repository-level scenario directory despite the configured output path. Its five generated artifacts were copied into this versioned report's `scenarios/dotnet-version-upgrade/` directory.
