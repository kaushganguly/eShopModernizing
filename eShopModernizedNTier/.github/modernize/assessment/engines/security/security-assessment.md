# Security Assessment Report

**Generated:** 2026-10-26T20:53:23.0000000Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 7 |
| CVE Vulnerabilities | 1 |
| CWE Vulnerabilities | 6 |
| Total Rules Assessed | 59 |
| Rules Passed | 53 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 3 |
| optional | 2 |
| potential | 2 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2024-21907: Newtonsoft.Json: Improper handling of exceptional conditions (DoS via deeply nested JSON)
- **Severity:** mandatory
- **Story Points:** 2
- **Files:** src/eShopWinForms/eShopWinForms.csproj:23

Microsoft.AspNet.WebApi.Client 5.2.7 transitively resolves Newtonsoft.Json 6.0.4 (affected versions < 13.0.1, patched 13.0.1; GHSA-5crp-9r3c-p9vr). Add a direct PackageReference to Newtonsoft.Json 13.0.1 or later.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-477: Use of Obsolete Function
- **Category:** Code Quality
- **Severity:** optional
- **Story Points:** 3
- **Files:** src/eShopWCFService/eShopWCFService.csproj, src/eShopWCFService/packages.config:3

Service targets .NET Framework 4.6.1 with legacy EntityFramework 6.1.3 and System.Data.SqlClient; WCF server APIs are not available on modern .NET.

### CWE-1057: Data Access Operations Outside of Expected Data Manager Component
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopWCFService/CatalogService.svc.cs:11-30

CatalogService (WCF service layer) uses the EF DbContext directly and performs filtering client-side after ToList() (GetCatalogItems, GetDiscount, GetAvailableStock), loading entire tables into memory.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** optional
- **Story Points:** 3
- **Files:** src/eShopWCFService/CatalogService.svc.cs:73-78, src/eShopWCFService/CatalogService.svc.cs:104-110

CreateCatalogItem and CreateAvailableStock compute Max(Id)+1 and then insert without a transaction or lock, so concurrent requests can generate duplicate keys.

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** mandatory
- **Story Points:** 3
- **Files:** docker-compose.override.yml:9, docker-compose.override.yml:17

docker-compose.override.yml sets SA_PASSWORD and the sqlcmd healthcheck with the literal password 'Pass@word' and the service uses the 'sa' account.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 2
- **Files:** src/eShopWCFService/CatalogService.svc.cs

The service has no logging or auditing for create/update/remove catalog operations or failures.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** mandatory
- **Story Points:** 3
- **Files:** docker-compose.override.yml:5, docker-compose.override.yml:9

Connection string and SQL sa credentials are stored in source control rather than a secret store.

