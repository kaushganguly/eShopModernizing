# Security Assessment Report

**Generated:** 2026-10-26T20:51:02.6785663Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 13 |
| CVE Vulnerabilities | 2 |
| CWE Vulnerabilities | 11 |
| Total Rules Assessed | 59 |
| Rules Passed | 48 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 1 |
| optional | 5 |
| potential | 7 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters
- **Severity:** optional
- **Story Points:** 1
- **Files:** eShopPorted/eShopPorted.csproj, src/eShopLegacyMVC/eShopLegacyMVC.csproj

[CVE-2026-40021](https://github.com/advisories/GHSA-4f7c-pmjv-c25w): Apache Log4net XML layouts can silently drop log events when processing unescaped XML 1.0 forbidden characters.

Severity: MEDIUM

Affected dependencies:
- log4net 2.0.10 (affected range < 3.3.0), declared in eShopPorted/eShopPorted.csproj and src/eShopLegacyMVC/eShopLegacyMVC.csproj

Recommended fix:
- Upgrade log4net to 3.3.0 or later.

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** src/eShopLegacyMVC/eShopLegacyMVC.csproj

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr): Newtonsoft.Json is vulnerable to improper handling of exceptional conditions.

Severity: HIGH

Affected dependencies:
- Newtonsoft.Json 12.0.1 (affected range < 13.0.1), declared in src/eShopLegacyMVC/eShopLegacyMVC.csproj

Recommended fix:
- Upgrade Newtonsoft.Json to 13.0.1 or later. The ported project declares 13.0.2, which is outside the affected range.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-477: Use of Obsolete Function
- **Category:** Code Quality
- **Severity:** optional
- **Story Points:** 1
- **Files:** eShopLegacy.Utilities/Serializing.cs

Serializing.SerializeBinary and DeserializeBinary instantiate System.Runtime.Serialization.Formatters.Binary.BinaryFormatter (Serializing.cs:11,19). BinaryFormatter is obsolete in modern .NET and remains a legacy serialization API.

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopLegacyMVC/Models/CatalogItemHiLoGenerator.cs, src/eShopLegacyMVC/Models/Infrastructure/CatalogDBInitializer.cs, src/eShopLegacyMVC/Models/Infrastructure/dbo.catalog_hilo.Sequence.sql

The legacy sequence generator converts the SQL sequence result from Int64 to int (CatalogItemHiLoGenerator.cs:23; CatalogDBInitializer.cs:328-329), while catalog_hilo is explicitly defined as a bigint sequence (dbo.catalog_hilo.Sequence.sql:4-9). Values outside the int range overflow when used as catalog identifiers.

### CWE-789: Memory Allocation with Excessive Size Value
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Controllers/CatalogController.cs, src/eShopLegacyMVC/Services/CatalogService.cs

CatalogController.Index accepts pageSize as an unbounded query parameter (CatalogController.cs:22) and CatalogService.GetCatalogItemsPaginated applies Take(pageSize) then materializes results with ToList (CatalogService.cs:25-31). A very large requested page can cause excessive result materialization and memory use.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs, src/eShopLegacyMVC/Services/CatalogServiceMock.cs

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a SingleInstance (ApplicationModule.cs:18-22). The singleton stores catalogItems in a mutable List and reads, modifies, and removes entries without synchronization (CatalogServiceMock.cs:12,33-67); concurrent requests can race while accessing shared catalog state.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs, src/eShopLegacyMVC/Services/CatalogServiceMock.cs

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a SingleInstance (ApplicationModule.cs:18-22). The singleton stores catalogItems in a mutable List and reads, modifies, and removes entries without synchronization (CatalogServiceMock.cs:12,33-67); concurrent requests can race while accessing shared catalog state.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs, src/eShopLegacyMVC/Services/CatalogServiceMock.cs

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a SingleInstance (ApplicationModule.cs:18-22). The singleton stores catalogItems in a mutable List and reads, modifies, and removes entries without synchronization (CatalogServiceMock.cs:12,33-67); concurrent requests can race while accessing shared catalog state.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs, src/eShopLegacyMVC/Services/CatalogServiceMock.cs

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a SingleInstance (ApplicationModule.cs:18-22). The singleton stores catalogItems in a mutable List and reads, modifies, and removes entries without synchronization (CatalogServiceMock.cs:12,33-67); concurrent requests can race while accessing shared catalog state.

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 8
- **Files:** src/eShopLegacyMVC/Controllers/PicController.cs, src/eShopLegacyMVC/Controllers/CatalogController.cs

PicController.Index combines the Pics directory with CatalogItem.PictureFileName (PicController.cs:38-40) without verifying the resolved path stays under Pics. CatalogController.Create/Edit bind PictureFileName from form input (CatalogController.cs:62,100), allowing a stored value containing traversal segments to be used in the file path.

### CWE-23: Relative Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Controllers/PicController.cs, src/eShopLegacyMVC/Controllers/CatalogController.cs

PicController.Index combines the Pics directory with CatalogItem.PictureFileName without canonicalizing or validating the result (PicController.cs:38-40). The picture filename can be supplied through catalog item create/edit form binding (CatalogController.cs:62,100), so relative traversal segments can escape the intended directory.

### CWE-36: Absolute Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Controllers/PicController.cs, src/eShopLegacyMVC/Controllers/CatalogController.cs

PicController.Index uses Path.Combine(webRoot, item.PictureFileName) (PicController.cs:38-40) without checking whether the supplied filename is rooted; catalog forms bind PictureFileName from request data (CatalogController.cs:62,100). An absolute path can therefore bypass the Pics directory.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopLegacyMVC/Controllers/PicController.cs, src/eShopLegacyMVC/Controllers/CatalogController.cs

PicController.Index uses CatalogItem.PictureFileName to select a filesystem resource through Path.Combine and File.ReadAllBytes (PicController.cs:38-45). The value is bound from catalog create/edit request data (CatalogController.cs:62,100), so external data controls which file resource is read.

