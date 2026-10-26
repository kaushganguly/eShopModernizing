# Security Assessment Report

**Generated:** 2026-10-26T20:02:34.7371293Z

## Summary

| Metric | Count |
|---|---:|
| Total Findings | 2 |
| CVE Vulnerabilities | 0 |
| CWE Vulnerabilities | 2 |
| Total Rules Assessed | 59 |
| Rules Passed | 57 |

### By Severity

| Severity | Count |
|---|---:|
| mandatory | 0 |
| optional | 2 |
| potential | 0 |

## CVE Findings (Dependency Vulnerabilities)

No CVE findings met the configured minimum severity threshold (medium).

## CWE Findings (Code-Level Vulnerabilities)

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** eShopModernizedSQL_ServiceFabricApp/ApplicationPackageRoot/SQL_ContainerPkg/ServiceManifest.xml:24

The SQL container service manifest assigns a static password value to the SA_PASSWORD environment variable. The secret value is intentionally omitted.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** eShopModernizedSQL_ServiceFabricApp/ApplicationPackageRoot/SQL_ContainerPkg/ServiceManifest.xml:24

The SQL container service manifest assigns a static password value to the SA_PASSWORD environment variable. The secret value is intentionally omitted.
