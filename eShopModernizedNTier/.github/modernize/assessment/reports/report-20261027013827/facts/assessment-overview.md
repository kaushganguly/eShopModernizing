# Assessment Overview

This assessment covers only `eShopModernizedNTier`. Evidence paths in the supplementary documents are relative to that workspace.

## Supplementary Documents

| Document | Description |
|---|---|
| [Architecture diagram](architecture-diagram.md) | Application layers, deployment boundaries, technology stack, and component relationships. |
| [Dependency map](dependency-map.md) | Declared external packages, versions, compatibility risks, and test dependency inventory. |
| [API and service communication contracts](api-service-contracts.md) | SOAP operations, contract types, communication patterns, and request sequence. |
| [Data architecture](data-architecture.md) | Database ownership, entity relationships, persistence methods, caching, and data sensitivity. |
| [Configuration inventory](configuration-inventory.md) | Build and runtime configuration, startup dependencies, masked sensitive settings, and runtime versions. |
| [Business workflows](business-workflows.md) | Catalog browsing, discount composition, stock changes, and business decision logic. |

## Scope and Limitations

- The core assessment includes all three scoped C# project files, including the alternate legacy client project. The .NET 10 upgrade assessment covers the two projects selected by `eShopModernizedNTier.sln`.
- Architecture and CWE assessments are static source/configuration reviews, not runtime penetration tests or deployment verification. CWE findings may overlap across rules.
- Dependency analysis checked eight declared package/version pairs against GitHub advisories. The dependency-list command failed because legacy WebApplication targets were unavailable; restored transitive dependency coverage is incomplete. An empty CVE result does not establish a clean full dependency scan.
- The upgrade assessment service wrote to its default output location despite the requested output setting. Its five generated artifacts were copied into this versioned report's `scenarios/dotnet-version-upgrade` directory without modifying application source.
- The generated HTML report opens and its navigation works. Mermaid diagrams require the renderer's external CDN, which was blocked in the assessment sandbox; graphical diagram rendering could not be verified there.
