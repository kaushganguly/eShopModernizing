# Assessment Report

## Summary

| Metric | Count |
|---|---:|
| Total core assessment issues | Not available |
| Security findings | 2 |
| CVE findings at or above high severity | 0 |
| CWE findings | 2 |
| CWE rules assessed | 59 |

## Application Information

| Item | Value |
|---|---|
| Workspace | ServiceFabric |
| Assessment status | Core AppCAT assessment unavailable: solution contains three legacy `.sfproj` deployment projects and no `.csproj` application projects |
| Security status | 2 hard-coded credential findings; no CVE findings met the high severity threshold |
| .NET upgrade assessment | Not produced: the upgrade assessment tool returned no report artifacts for this unsupported solution shape |
| External assessment | SQL migration readiness not run because the required `datamigration` Azure CLI extension is not installed; no tooling was installed |

## Security Findings

### CWE-259: Use of Hard-coded Password

- **Severity:** optional
- **Category:** Credentials & Secrets
- **Files:** `D:\a\eShopModernizing\eShopModernizing\ServiceFabric\eShopModernizedSQL_ServiceFabricApp\ApplicationPackageRoot\SQL_ContainerPkg\ServiceManifest.xml:24`, ARM parameter files under `D:\a\eShopModernizing\eShopModernizing\ServiceFabric\ServiceFabric-ARM-Templates-Scripts\`

A non-empty literal is configured for the SQL container administrator password and in checked-in ARM parameter files for VM administrator passwords. Values are omitted from this report.

### CWE-798: Use of Hard-coded Credentials

- **Severity:** optional
- **Category:** Credentials & Secrets
- **Files:** `D:\a\eShopModernizing\eShopModernizing\ServiceFabric\eShopModernizedSQL_ServiceFabricApp\ApplicationPackageRoot\SQL_ContainerPkg\ServiceManifest.xml:24`, ARM parameter files under `D:\a\eShopModernizing\eShopModernizing\ServiceFabric\ServiceFabric-ARM-Templates-Scripts\`

Deployment configuration contains non-empty literal authentication values. Credential values are masked and are not reproduced.

## Codebase Insights

- [Architecture Diagram](facts/architecture-diagram.md) — Service Fabric deployment topology and container relationships.
- [Dependency Map](facts/dependency-map.md) — Declared deployment packages and restore limitations.
- [API and Service Contracts](facts/api-service-contracts.md) — API endpoint discovery status.
- [Data Architecture](facts/data-architecture.md) — Data-access discovery status.
- [Configuration Inventory](facts/configuration-inventory.md) — Deployment profiles, settings, and masked sensitive configuration.
- [Business Workflows](facts/business-workflows.md) — Business workflow discovery status.

