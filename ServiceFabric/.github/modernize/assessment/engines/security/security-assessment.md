# Security Assessment Report

**Generated:** 
2026-10-26T20:59:50.2360923Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 
2
 |
| CVE Vulnerabilities | 
0
 |
| CWE Vulnerabilities | 
2
 |
| Total Rules Assessed | 
59
 |
| Rules Passed | 
57
 |

### By Severity

| Severity | Count |
|----------|-------|
| optional | 2 |

## CWE Findings (Code-Level Vulnerabilities)

### 
CWE-259
: 
Use of Hard-coded Password
- **Category:** 
Credentials & Secrets
- **Severity:** 
optional
- **Story Points:** 
5
- **Files:** 
eShopModernizedSQL_ServiceFabricApp/ApplicationPackageRoot/SQL_ContainerPkg/ServiceManifest.xml:24

The SQL container manifest provides the SA_PASSWORD environment variable as a literal value at line 24, so the SQL administrative credential is stored in deployment configuration rather than retrieved from a secret store.

### 
CWE-798
: 
Use of Hard-coded Credentials
- **Category:** 
Credentials & Secrets
- **Severity:** 
optional
- **Story Points:** 
5
- **Files:** 
eShopModernizedSQL_ServiceFabricApp/ApplicationPackageRoot/SQL_ContainerPkg/ServiceManifest.xml:24

The SQL container manifest provides the SA_PASSWORD environment variable as a literal value at line 24, so the SQL administrative credential is stored in deployment configuration rather than retrieved from a secret store.


