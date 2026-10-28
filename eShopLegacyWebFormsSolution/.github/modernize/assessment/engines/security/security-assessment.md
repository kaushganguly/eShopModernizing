# Security Assessment Report

**Generated:** 2026-10-28T23:21:13.085604Z

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

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr): Newtonsoft.Json versions before 13.0.1 can be exposed to denial of service through deeply nested JSON input, resulting in excessive CPU or memory use or a stack overflow.

Severity: HIGH

Affected dependency: Newtonsoft.Json 12.0.1 (declared at src/eShopLegacyWebForms/packages.config:31).

Recommended fix: Upgrade Newtonsoft.Json to 13.0.1 or later, or configure a bounded MaxDepth for all relevant serializer operations.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopLegacyWebForms/Models/CatalogItemHiLoGenerator.cs:22-24, src/eShopLegacyWebForms/Models/Infrastructure/CatalogDBInitializer.cs:327-328

Both sequence retrieval paths read an Int64 SQL sequence value and cast it directly to int. Values outside the Int32 range can truncate or wrap, and the converted value is then used as a catalog entity identifier, potentially producing invalid or duplicate IDs.

### CWE-682: Incorrect Calculation
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyWebForms/Default.aspx.cs:26-28, src/eShopLegacyWebForms/Services/CatalogService.cs:29-32, src/eShopLegacyWebForms/ViewModel/PaginatedItemsViewModel.cs:18-24

The page accepts unbounded route values for page size and index, passes them to the service, and multiplies them as 32-bit integers for Skip. Large values can overflow and yield an incorrect offset or query failure; a zero page size also reaches a division in PaginatedItemsViewModel and throws.

### CWE-789: Memory Allocation with Excessive Size Value
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyWebForms/Default.aspx.cs:26-28, src/eShopLegacyWebForms/Services/CatalogService.cs:26-32

The page accepts a route-controlled page size without a maximum and passes it to the database-backed service, which uses Take(pageSize) and materializes all returned entities with ToList. A very large page size can therefore trigger retrieval and memory use proportional to the catalog size for a single request.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyWebForms/Modules/ApplicationModule.cs:18-22, src/eShopLegacyWebForms/Services/CatalogServiceMock.cs:12-16

When mock mode is enabled, ApplicationModule registers CatalogServiceMock as a SingleInstance. The singleton is accessed by concurrent web requests and owns mutable catalog state without synchronizing service operations.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyWebForms/Modules/ApplicationModule.cs:18-22, src/eShopLegacyWebForms/Services/CatalogServiceMock.cs:12,48-67, src/eShopLegacyWebForms/Web.config:15

UseMockData is enabled by default and Autofac shares one CatalogServiceMock instance. Its catalogItems List is read and modified by service methods without a lock, allowing simultaneous requests to race during enumeration, additions, replacement, and removal.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopLegacyWebForms/Services/CatalogServiceMock.cs:21,48-67

Concurrent web requests operate on the same mutable catalogItems List. Reads/enumeration and write operations are not mutually exclusive, so concurrent operations can observe inconsistent state or throw collection-modified exceptions.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopLegacyWebForms/Services/CatalogServiceMock.cs:12,21,48-67

The singleton mock catalog's List<CatalogItem> is shared across request threads, and its reads and mutations have no synchronization. The separate catalog ID generator lock does not protect this list.

### CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')
- **Category:** Injection Attacks
- **Severity:** optional
- **Story Points:** 8
- **Files:** src/eShopLegacyWebForms/Catalog/Create.aspx.cs:36, src/eShopLegacyWebForms/Catalog/Create.aspx.cs:37, src/eShopLegacyWebForms/Catalog/Details.aspx:14, src/eShopLegacyWebForms/Catalog/Details.aspx:21

Catalog/Create.aspx.cs accepts the submitted Name and Description values and persists them through CreateCatalogItem (lines 36-46). Catalog/Details.aspx binds the persisted values directly to ASP.NET Label Text expressions without explicit HTML encoding (lines 14 and 21); Label text rendering can therefore place attacker-controlled markup in the response to other users, creating stored cross-site scripting. Edit.aspx.cs similarly updates these values.
