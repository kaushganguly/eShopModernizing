# Assessment overview

Scope: `eShopLegacyNTier` only. These supplementary documents describe the checked-in application; proposed migration options are not implemented changes.

## Supplementary documents

- [Architecture diagram](architecture-diagram.md) — Application layers, component relationships, deployment boundaries, and architectural constraints.
- [Dependency map](dependency-map.md) — Project, package, framework, and external-system dependencies.
- [API and service contracts](api-service-contracts.md) — WCF operations, message shapes, client communication, and generated-contract differences.
- [Data architecture](data-architecture.md) — Entity relationships, persistence configuration, initialization, and data-access behavior.
- [Configuration inventory](configuration-inventory.md) — Runtime settings, endpoints, connection configuration, and environment-specific considerations.
- [Business workflows](business-workflows.md) — Catalog browsing and maintenance flows, sequence diagrams, and implementation gaps.

## Assessment context

- The [core report](../report.json) was generated with AppCAT `1.0.1127`, restricted privacy mode, and both scoped projects. Its issue counts depend on the selected deployment target.
- The [.NET upgrade assessment](../scenarios/dotnet-version-upgrade/assessment.md) targets `net10.0`, as requested. WinForms requires a Windows-specific target for an eventual migration; counts against plain `net10.0` are not a direct migration-effort estimate.
- Architecture documentation is a static source assessment, not a verification of a running deployment or live database schema.
- No application code, dependencies, or target frameworks were changed.
