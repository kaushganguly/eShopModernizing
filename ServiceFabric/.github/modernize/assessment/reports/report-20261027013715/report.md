# ServiceFabric Assessment Report

**Report ID:** `20261027013715`  
**Scope:** `ServiceFabric`  
**Generated:** 2026-10-27  

## Assessment Summary

| Assessment | Result |
|---|---|
| Core AppCAT assessment | Not generated. The workspace contains Service Fabric `.sfproj` deployment projects and no `.csproj` application projects; AppCAT rejected the `.sln` input as an invalid project path. |
| .NET upgrade assessment | Scenario initialized for `net10.0`; only `scenario.json` was produced. The expected `assessment.json`, `assessment.csv`, `assessment.md`, and `dependencies-health.json` were not generated for this Service Fabric solution. |
| CVE dependency scan | No known advisories found for the package versions available in scope; no CVE findings met the medium severity threshold. Dependency inventory is incomplete because the Service Fabric projects do not provide restoreable .NET application project files. |
| CWE code scan | 2 findings across 59 assessed rules; 57 rules passed. |

## Security Findings

### CWE-259: Use of Hard-coded Password

- **Category:** Credentials & Secrets
- **Severity:** optional
- **File:** `ServiceFabric/eShopModernizedSQL_ServiceFabricApp/ApplicationPackageRoot/SQL_ContainerPkg/ServiceManifest.xml:24`

The SQL container manifest declares an administrator password directly in an environment variable. The configured value is intentionally redacted.

### CWE-798: Use of Hard-coded Credentials

- **Category:** Credentials & Secrets
- **Severity:** optional
- **File:** `ServiceFabric/eShopModernizedSQL_ServiceFabricApp/ApplicationPackageRoot/SQL_ContainerPkg/ServiceManifest.xml:24`

The SQL container manifest contains a hard-coded database administrator credential. Its value is intentionally redacted.

## Codebase Insights

The supplementary documents below describe declared deployment structure and explicitly note when the application source required for deeper analysis is outside the `ServiceFabric` scope.

- [Assessment Facts Overview](facts/assessment-overview.md)
- [Architecture Diagram](facts/architecture-diagram.md)
- [Dependency Map](facts/dependency-map.md)
- [API & Service Communication Contracts](facts/api-service-contracts.md)
- [Data Architecture & Persistence Layer](facts/data-architecture.md)
- [Configuration & Externalized Settings Inventory](facts/configuration-inventory.md)
- [Core Business Workflows](facts/business-workflows.md)