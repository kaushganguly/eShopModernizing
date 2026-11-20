# Security Assessment Report

**Generated:** 
11/20/2026 11:12:40 AM

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

No CVE findings met the high severity threshold.

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
eShopModernizedSQL_ServiceFabricApp/ApplicationPackageRoot/SQL_ContainerPkg/ServiceManifest.xml:24, ServiceFabric-ARM-Templates-Scripts/pro-eshop-sfwin-cluster-ARM-SECURED-tests/parameters.json:17, ServiceFabric-ARM-Templates-Scripts/pro-eshop-sfwin-cluster-ARM-template-SECURED-NOFUNCI/parameters.json:17, ServiceFabric-ARM-Templates-Scripts/Sample-ServiceFabric-ARM/azuredeploy.parameters.json:17

The SQL container manifest provides SA_PASSWORD as a non-empty literal, and three checked-in ARM parameter files provide adminPassword as non-empty literals. Credential values are intentionally omitted.

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
eShopModernizedSQL_ServiceFabricApp/ApplicationPackageRoot/SQL_ContainerPkg/ServiceManifest.xml:24, ServiceFabric-ARM-Templates-Scripts/pro-eshop-sfwin-cluster-ARM-SECURED-tests/parameters.json:17, ServiceFabric-ARM-Templates-Scripts/pro-eshop-sfwin-cluster-ARM-template-SECURED-NOFUNCI/parameters.json:17, ServiceFabric-ARM-Templates-Scripts/Sample-ServiceFabric-ARM/azuredeploy.parameters.json:17

The SQL container manifest and checked-in ARM parameter files contain non-empty literal values for authentication parameters. The ARM DeploymentHelper credential fields are explicit placeholders, and values are omitted here.
