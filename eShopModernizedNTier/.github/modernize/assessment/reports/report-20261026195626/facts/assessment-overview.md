# Assessment overview

Scope: `eShopModernizedNTier` only. Application source was not changed.

## Supplementary documents

- [Architecture diagram](architecture-diagram.md): desktop client, WCF service, database, deployment boundaries, and modernization considerations.
- [Dependency map](dependency-map.md): project, package, framework, and external dependencies.
- [API and service contracts](api-service-contracts.md): SOAP operations, generated client contracts, and communication sequences.
- [Data architecture](data-architecture.md): Entity Framework model, relationships, persistence, and initialization.
- [Configuration inventory](configuration-inventory.md): application, service, database, and container settings; sensitive values are omitted.
- [Business workflows](business-workflows.md): catalog browsing, filtering, editing, and persistence sequences.

## Assessment artifacts and interpretation

- [Core assessment data](../report.json): AppCAT analysis of all three project files, including the alternative framework client project.
- [.NET 10 upgrade assessment](../scenarios/dotnet-version-upgrade/assessment.md): analysis of the two projects included in the solution.

The upgrade scan used the requested `net10.0` target. Windows Forms requires a Windows-specific target such as `net10.0-windows`; desktop API findings and generated proxy occurrences must not be interpreted as an automatic requirement to rewrite those APIs. The tool labels the desktop project as Wpf, while the source implements Windows Forms. Counts represent tool findings, not independently confirmed defects or migration effort.

The upgrade MCP tool emitted its five assessment artifacts in its default scenario location despite the configured output path. Those artifacts were copied unchanged into this versioned report's `scenarios/dotnet-version-upgrade/` directory.
