# Security Assessment Report

**Generated:** 2026-10-26T19:57:16.5483492Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 15 |
| CVE Vulnerabilities | 2 |
| CWE Vulnerabilities | 13 |
| Total Rules Assessed | 59 |
| Rules Passed | 46 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 1 |
| optional | 5 |
| potential | 9 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** src/eShopLegacyMVC/eShopLegacyMVC.csproj:102

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr): Improper Handling of Exceptional Conditions in Newtonsoft.Json

Severity: high

Affected dependencies:
  - Newtonsoft.Json 12.0.1 (declared at src/eShopLegacyMVC/eShopLegacyMVC.csproj:102)

Recommended fix:
  - Upgrade Newtonsoft.Json to 13.0.1 or later.

### CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters
- **Severity:** optional
- **Story Points:** 1
- **Files:** src/eShopLegacyMVC/eShopLegacyMVC.csproj:242

[CVE-2026-40021](https://github.com/advisories/GHSA-4f7c-pmjv-c25w): Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters

Severity: medium

Affected dependencies:
  - log4net 2.0.10 (declared at src/eShopLegacyMVC/eShopLegacyMVC.csproj:242)

Recommended fix:
  - Upgrade log4net to 3.3.0 or later.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-477: Use of Obsolete Function
- **Category:** Code Quality
- **Severity:** optional
- **Story Points:** 1
- **Files:** eShopLegacy.Utilities/Serializing.cs, src/eShopLegacyMVC/Controllers/WebApi/FilesController.cs

Serializing.SerializeBinary and DeserializeBinary instantiate System.Runtime.Serialization.Formatters.Binary.BinaryFormatter (lines 11-12 and 19-21). BinaryFormatter is obsolete in modern .NET; the legacy FilesController uses SerializeBinary to produce an API response.

### CWE-665: Improper Initialization
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopPorted/Controllers/PicController.cs

The ported PicController.Index initializes webRoot to an empty string (line 38) before combining it with the image name and reading the file (lines 40-45), so the resource path is not initialized to the intended application image directory.

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopLegacyMVC/Models/CatalogItemHiLoGenerator.cs, src/eShopLegacyMVC/Models/Infrastructure/CatalogDBInitializer.cs

The legacy CatalogItemHiLoGenerator converts a database Int64 sequence result to int without a range check (lines 22-23); CatalogDBInitializer makes a similar conversion at lines 328-329. Sequence values outside the Int32 range can yield incorrect item identifiers.

### CWE-682: Incorrect Calculation
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Controllers/CatalogController.cs, src/eShopLegacyMVC/Services/CatalogService.cs

CatalogController.Index accepts pageSize and pageIndex as request parameters, and CatalogService computes the skip offset using pageSize * pageIndex (lines 29-30) without overflow handling. Large values can overflow and produce an incorrect pagination offset.

### CWE-789: Memory Allocation with Excessive Size Value
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Controllers/CatalogController.cs, src/eShopLegacyMVC/Services/CatalogService.cs

CatalogController.Index accepts an unrestricted pageSize query parameter (line 22); CatalogService passes it directly to Take(pageSize) and materializes the result with ToList (lines 25-31), allowing a request to ask for an excessive number of rows and memory.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs, src/eShopLegacyMVC/Services/CatalogServiceMock.cs

ApplicationModule registers CatalogServiceMock as SingleInstance (lines 18-22). The mock service stores CatalogItem objects in a mutable List (line 12), and CreateCatalogItem, UpdateCatalogItem, RemoveCatalogItem, FindCatalogItem, and ComposeCatalogItems access or mutate it without a lock (lines 33-80). Concurrent requests can race on list operations and ID assignment.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs, src/eShopLegacyMVC/Services/CatalogServiceMock.cs

ApplicationModule registers CatalogServiceMock as SingleInstance (lines 18-22). The mock service stores CatalogItem objects in a mutable List (line 12), and CreateCatalogItem, UpdateCatalogItem, RemoveCatalogItem, FindCatalogItem, and ComposeCatalogItems access or mutate it without a lock (lines 33-80). Concurrent requests can race on list operations and ID assignment.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs, src/eShopLegacyMVC/Services/CatalogServiceMock.cs

ApplicationModule registers CatalogServiceMock as SingleInstance (lines 18-22). The mock service stores CatalogItem objects in a mutable List (line 12), and CreateCatalogItem, UpdateCatalogItem, RemoveCatalogItem, FindCatalogItem, and ComposeCatalogItems access or mutate it without a lock (lines 33-80). Concurrent requests can race on list operations and ID assignment.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs, src/eShopLegacyMVC/Services/CatalogServiceMock.cs

ApplicationModule registers CatalogServiceMock as SingleInstance (lines 18-22). The mock service stores CatalogItem objects in a mutable List (line 12), and CreateCatalogItem, UpdateCatalogItem, RemoveCatalogItem, FindCatalogItem, and ComposeCatalogItems access or mutate it without a lock (lines 33-80). Concurrent requests can race on list operations and ID assignment.

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 8
- **Files:** src/eShopLegacyMVC/Controllers/CatalogController.cs, src/eShopLegacyMVC/Controllers/PicController.cs

CatalogController.Create and Edit bind PictureFileName from the submitted CatalogItem (Create line 62; Edit line 100). PicController.Index combines that persisted value with the Pics directory and reads the resulting path (lines 38-44), without restricting the resolved path to the image directory.

### CWE-23: Relative Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Controllers/CatalogController.cs, src/eShopLegacyMVC/Controllers/PicController.cs

The submitted PictureFileName is persisted through CatalogController.Create/Edit and later passed to Path.Combine in PicController.Index (lines 38-44) before File.ReadAllBytes. Relative traversal segments are not rejected or normalized against the Pics directory.

### CWE-36: Absolute Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Controllers/CatalogController.cs, src/eShopLegacyMVC/Controllers/PicController.cs

CatalogController accepts PictureFileName from the submitted model, and PicController.Index combines it with the intended Pics directory before reading the file (lines 38-44). A fully qualified second path can cause Path.Combine to discard the base directory, and no containment check is present.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopLegacyMVC/Controllers/CatalogController.cs, src/eShopLegacyMVC/Controllers/PicController.cs

CatalogController.Create/Edit accept PictureFileName from submitted model data, and PicController.Index uses that stored value to identify the file to read (Path.Combine and File.ReadAllBytes, lines 38-44). The value is not constrained to the intended image resource.

