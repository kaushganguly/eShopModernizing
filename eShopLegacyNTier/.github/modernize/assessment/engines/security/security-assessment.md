# Security Assessment Report

**Generated:** 2026-10-26T20:44:18.0000000Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 4 |
| CVE Vulnerabilities | 1 |
| CWE Vulnerabilities | 3 |
| Total Rules Assessed | 59 |
| Rules Passed | 56 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 1 |
| optional | 0 |
| potential | 3 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** eShopLegacyNTier/src/eShopWinForms/packages.config:5, eShopLegacyNTier/src/eShopWinForms/eShopWinForms.csproj:63

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr): Improper Handling of Exceptional Conditions in Newtonsoft.Json

Severity: HIGH

Affected dependencies:
  - Newtonsoft.Json:6.0.4 (declared at eShopLegacyNTier/src/eShopWinForms/packages.config:5)

Recommended fix:
  - Upgrade Newtonsoft.Json to 13.0.1 or later

## CWE Findings (Code-Level Vulnerabilities)

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopLegacyNTier/src/eShopWinForms/Program.cs

Program.Main creates eShopServiceReference.CatalogServiceClient (a WCF ClientBase, which must be closed) at line 584 and never closes or disposes it.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** eShopLegacyNTier/src/eShopWCFService/CatalogService.svc.cs

CatalogService.CreateCatalogItem (lines 141-144) and CreateAvailableStock (lines 187-190) compute the next primary key as Max(Id)+1 and save without any locking or transaction, so concurrent WCF calls can generate duplicate keys or lose updates.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopLegacyNTier/src/eShopWCFService/CatalogService.svc.cs

The WCF service performs create, update and remove operations on catalog data (lines 139-157) without any audit or security event logging; no logging framework is used anywhere in the solution.
