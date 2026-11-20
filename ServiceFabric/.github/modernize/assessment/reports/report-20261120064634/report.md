# ServiceFabric Assessment

## Summary

| Metric | Value |
|---|---:|
| Total Issues | 2 |
| Mandatory Blockers | 0 |
| Potential Issues | 0 |

Core AppCAT assessment could not analyze the scoped Service Fabric solution because its projects are legacy `.sfproj` deployment projects. The report therefore contains security findings and deployment facts only.

## Component Information

| Property | Value |
|---|---|
| Language | Not determined from deployment projects |
| Frameworks | .NET Framework 4.6.1 and 4.6.2 project targets |
| Build tools | Service Fabric project format; NuGet `packages.config` |

## Security Findings

| Issue Name | Criticality | Story Points |
|---|---|---:|
| Use of Hard-coded Password | Optional | 5 |
| Use of Hard-coded Credentials | Optional | 5 |

### CWE-259: Use of Hard-coded Password

- **File:** `eShopModernizedSQL_ServiceFabricApp/ApplicationPackageRoot/SQL_ContainerPkg/ServiceManifest.xml:24`
- **Finding:** The SQL container manifest declares a literal `SA_PASSWORD` value. The value is omitted from this report.

### CWE-798: Use of Hard-coded Credentials

- **File:** `eShopModernizedSQL_ServiceFabricApp/ApplicationPackageRoot/SQL_ContainerPkg/ServiceManifest.xml:24`
- **Finding:** The SQL container manifest declares a literal authentication credential. The value is omitted from this report.

## Codebase Insights

- [Supplementary Assessment Documents](facts/assessment-overview.md)
- [Architecture Diagram](facts/architecture-diagram.md)
- [Dependency Map](facts/dependency-map.md)
- [API & Service Communication Contracts](facts/api-service-contracts.md)
- [Data Architecture](facts/data-architecture.md)
- [Configuration Inventory](facts/configuration-inventory.md)
- [Business Workflows](facts/business-workflows.md)
