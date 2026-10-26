# Security Assessment Report

**Generated:** 2026-10-26T23:19:12.7918003Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 2 |
| CVE Vulnerabilities | 0 |
| CWE Vulnerabilities | 2 |
| Total Rules Assessed | 59 |
| Rules Passed | 57 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 0 |
| optional | 2 |
| potential | 0 |

## CVE Findings (Dependency Vulnerabilities)

No CVE findings met the medium-or-higher severity threshold.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** ServiceFabric/eShopModernizedSQL_ServiceFabricApp/ApplicationPackageRoot/SQL_ContainerPkg/ServiceManifest.xml:24

The SQL container manifest declares the SQL Server SA password as a literal value in an EnvironmentVariable. The literal is intentionally omitted from this report.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** ServiceFabric/eShopModernizedSQL_ServiceFabricApp/ApplicationPackageRoot/SQL_ContainerPkg/ServiceManifest.xml:24

The SQL container manifest declares the SQL Server SA password as a literal value in an EnvironmentVariable. The literal is intentionally omitted from this report.
