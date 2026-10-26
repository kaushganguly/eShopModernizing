# Security Assessment Report

**Generated:** 2026-10-26T23:06:42.451062Z

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

### CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters
- **Severity:** optional
- **Story Points:** 1
- **Files:** eShopPorted/eShopPorted.csproj:13, src/eShopLegacyMVC/eShopLegacyMVC.csproj:242, src/eShopLegacyMVC/packages.config:10

[CVE-2026-40021](https://github.com/advisories/GHSA-4f7c-pmjv-c25w): Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters

Severity: MEDIUM

Affected dependencies:
  - log4net:2.0.10 at eShopPorted/eShopPorted.csproj:13; vulnerable range < 3.3.0; first patched version not specified
  - log4net:2.0.10 at src/eShopLegacyMVC/eShopLegacyMVC.csproj:242; vulnerable range < 3.3.0; first patched version not specified
  - log4net:2.0.10 at src/eShopLegacyMVC/packages.config:10; vulnerable range < 3.3.0; first patched version not specified

Recommended fix: upgrade affected dependencies to their first patched versions where specified.

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** eShopPorted/eShopPorted.csproj:23, src/eShopLegacyMVC/eShopLegacyMVC.csproj:102, src/eShopLegacyMVC/packages.config:29

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr): Improper Handling of Exceptional Conditions in Newtonsoft.Json

Severity: HIGH

Affected dependencies:
  - Newtonsoft.Json:12.0.1 at src/eShopLegacyMVC/eShopLegacyMVC.csproj:102; vulnerable range < 13.0.1; first patched version not specified
  - Newtonsoft.Json:12.0.1 at src/eShopLegacyMVC/packages.config:29; vulnerable range < 13.0.1; first patched version not specified
  - Newtonsoft.Json:13.0.2 at eShopPorted/eShopPorted.csproj:23; vulnerable range < 13.0.1; first patched version not specified

Recommended fix: upgrade affected dependencies to their first patched versions where specified.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-477: Use of Obsolete Function
- **Category:** Code Quality
- **Severity:** optional
- **Story Points:** 1
- **Files:** eShopLegacy.Utilities/Serializing.cs

Serializing.SerializeBinary and DeserializeBinary instantiate and call BinaryFormatter (lines 11-12 and 19-21). BinaryFormatter is an obsolete serialization API in modern .NET and remains in this shared utility, creating a compatibility and maintenance risk.

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopLegacyMVC/Models/Infrastructure/CatalogDBInitializer.cs, src/eShopLegacyMVC/Models/CatalogItemHiLoGenerator.cs

CatalogDBInitializer.GetSequenceIdFromSelectedDBSequence (line 329) and CatalogItemHiLoGenerator.GetNextSequenceValue (line 23) explicitly cast the SQL Int64 sequence result to int. Values outside the Int32 range are truncated, producing incorrect identifiers.

### CWE-682: Incorrect Calculation
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Controllers/CatalogController.cs, src/eShopLegacyMVC/Services/CatalogService.cs

CatalogController.Index accepts pageSize and pageIndex from request parameters without range checks (lines 22-25). CatalogService.GetCatalogItemsPaginated multiplies them in pageSize * pageIndex at line 29; large values can overflow the Int32 calculation and yield incorrect pagination offsets.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs, src/eShopLegacyMVC/Services/CatalogServiceMock.cs

ApplicationModule.Load registers CatalogServiceMock as SingleInstance when mock data is enabled (lines 18-22), but its per-instance mutable catalogItems list is accessed by request methods without synchronization.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs, src/eShopLegacyMVC/Services/CatalogServiceMock.cs

CatalogServiceMock.catalogItems is mutable shared state (line 12); CreateCatalogItem, UpdateCatalogItem, and RemoveCatalogItem modify it (lines 48-67), while read methods access it concurrently, with no synchronization.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs, src/eShopLegacyMVC/Services/CatalogServiceMock.cs

The singleton CatalogServiceMock exposes concurrent collection reads and mutations without a lock or other synchronization, allowing overlapping requests to observe or modify inconsistent list state.

### CWE-667: Improper Locking
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs, src/eShopLegacyMVC/Services/CatalogServiceMock.cs

CatalogServiceMock uses a singleton mutable list without acquiring a lock in its CRUD/read operations, so callers can concurrently alter the resource.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs, src/eShopLegacyMVC/Services/CatalogServiceMock.cs

Autofac shares CatalogServiceMock across request scopes when mock mode is enabled (ApplicationModule.cs:18-22), but list access and mutation in CatalogServiceMock.cs:19-67 are unsynchronized.

### CWE-821: Incorrect Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs, src/eShopLegacyMVC/Services/CatalogServiceMock.cs

Although CatalogItemHiLoGenerator uses a lock, CatalogServiceMock does not synchronize access to its shared catalogItems list; concurrent create, update, delete, and read operations can race.

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 8
- **Files:** src/eShopLegacyMVC/Controllers/PicController.cs, src/eShopLegacyMVC/Controllers/CatalogController.cs

CatalogController.Create and CatalogController.Edit bind PictureFileName from submitted model data (lines 62 and 100). PicController.Index combines that persisted value with the Pics directory at line 39 and reads the resulting path at line 44 without verifying it remains under the directory. A crafted filename can therefore escape the intended Pics directory.

### CWE-23: Relative Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Controllers/PicController.cs, src/eShopLegacyMVC/Controllers/CatalogController.cs

CatalogController.Create/Edit accept a user-supplied PictureFileName (lines 62 and 100). PicController.Index passes the stored value directly to Path.Combine at line 39 and then reads the file at line 44, without normalizing or constraining traversal sequences such as ../.

### CWE-36: Absolute Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Controllers/PicController.cs, src/eShopLegacyMVC/Controllers/CatalogController.cs

CatalogController.Create/Edit accept PictureFileName from submitted data (lines 62 and 100). PicController.Index uses it as the second argument to Path.Combine at line 39; an absolute filename can replace the Pics root, and the resulting path is read at line 44.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopLegacyMVC/Controllers/CatalogController.cs, src/eShopLegacyMVC/Controllers/PicController.cs

CatalogController.Create/Edit bind PictureFileName from request data (lines 62 and 100). PicController.Index uses that value to identify the file resource via Path.Combine at line 39, then reads it at line 44 without restricting it to the Pics directory. This is the same confirmed file-resource path used in the file-path assessment.
