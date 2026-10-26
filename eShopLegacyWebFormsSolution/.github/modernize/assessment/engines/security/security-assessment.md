# Security Assessment Report

**Generated:** 2026-10-26T20:04:23.7420956Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 9 |
| CVE Vulnerabilities | 1 |
| CWE Vulnerabilities | 8 |
| Total Rules Assessed | 59 |
| Rules Passed | 51 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 1 |
| optional | 1 |
| potential | 7 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** src/eShopLegacyWebForms/packages.config:31

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr): Newtonsoft.Json versions before 13.0.1 mishandle deeply nested data, which can cause excessive CPU and memory use or a stack overflow denial of service. Severity: HIGH. Affected dependency: Newtonsoft.Json 12.0.1 (declared at src/eShopLegacyWebForms/packages.config:31). Recommended patched version: 13.0.1 or later.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopLegacyWebForms/Models/Infrastructure/CatalogDBInitializer.cs:327-328, src/eShopLegacyWebForms/Models/CatalogItemHiLoGenerator.cs:22-23

CatalogDBInitializer.GetSequenceIdFromSelectedDBSequence and CatalogItemHiLoGenerator.GetNextSequenceValue convert the database sequence result from Int64 to Int32 using explicit casts. Values beyond the Int32 range wrap/truncate in the unchecked context, producing incorrect catalog identifiers.

### CWE-682: Incorrect Calculation
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyWebForms/Services/CatalogService.cs:30, src/eShopLegacyWebForms/Services/CatalogServiceMock.cs:25, src/eShopLegacyWebForms/Default.aspx.cs:26-28

The catalog page accepts page size and index as route integers and computes pageSize * pageIndex for Skip in both CatalogService and CatalogServiceMock. The unchecked multiplication can overflow and produce an incorrect or negative pagination offset.

### CWE-789: Memory Allocation with Excessive Size Value
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyWebForms/Default.aspx.cs:26-28, src/eShopLegacyWebForms/Services/CatalogService.cs:26-32

Default.Page_Load accepts an arbitrary positive or very large route size and passes it to GetCatalogItemsPaginated; CatalogService applies Take(pageSize) then materializes all selected rows with ToList without a maximum page size, allowing an oversized response to consume excessive resources.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyWebForms/Modules/ApplicationModule.cs:18-22, src/eShopLegacyWebForms/Services/CatalogServiceMock.cs:12-16, src/eShopLegacyWebForms/Global.asax.cs:53-54

When UseMockData is true, ApplicationModule registers CatalogServiceMock as SingleInstance. The singleton owns a mutable List<CatalogItem> and its create, update, and remove operations mutate that list without synchronization, so concurrent Web Forms requests can race.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyWebForms/Services/CatalogServiceMock.cs:12, src/eShopLegacyWebForms/Services/CatalogServiceMock.cs:48-67

CatalogServiceMock exposes its shared catalogItems list to concurrent request handlers. CreateCatalogItem, UpdateCatalogItem, and RemoveCatalogItem mutate or replace list elements without a lock while readers enumerate the same list.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopLegacyWebForms/Services/CatalogServiceMock.cs:48-67

CatalogServiceMock performs concurrent mutations and enumeration against its singleton-owned catalogItems List without exclusive synchronization, allowing collection corruption or inconsistent catalog data.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopLegacyWebForms/Modules/ApplicationModule.cs:18-22, src/eShopLegacyWebForms/Services/CatalogServiceMock.cs:48-67

The application selects the singleton mock catalog service when UseMockData is enabled, but CatalogServiceMock does not synchronize its shared mutable catalogItems list across concurrent requests.

### CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')
- **Category:** Injection Attacks
- **Severity:** optional
- **Story Points:** 8
- **Files:** src/eShopLegacyWebForms/Catalog/Details.aspx:14

Catalog/Details.aspx binds product.Name directly to an asp:Label Text property. The value is entered through the catalog maintenance form and persisted; the Label writes its Text as HTML rather than encoding it, allowing stored markup to be emitted to other users.

