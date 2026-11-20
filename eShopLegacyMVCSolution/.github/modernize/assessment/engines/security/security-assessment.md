# Security Assessment Report

**Generated:** 2026-11-20T06:46:51.9807804Z

## Summary

| Metric | Count |
|---|---:|
| Total Findings | 14 |
| CVE Vulnerabilities | 2 |
| CWE Vulnerabilities | 12 |
| Total Rules Assessed | 59 |
| Rules Passed | 47 |

### By Severity

| Severity | Count |
|---|---:|
| mandatory | 2 |
| optional | 5 |
| potential | 7 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters
- **Severity:** optional
- **Story Points:** 1
- **Files:** eShopPorted\eShopPorted.csproj:13, src\eShopLegacyMVC\eShopLegacyMVC.csproj:242, src\eShopLegacyMVC\packages.config:10

[CVE-2026-40021](https://github.com/advisories/GHSA-4f7c-pmjv-c25w): Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters

Severity: MEDIUM
Vulnerable range: log4net < 3.3.0
Affected dependencies:
  - log4net 2.0.10 (declared at eShopPorted\eShopPorted.csproj:13)
  - log4net 2.0.10 (declared at src\eShopLegacyMVC\eShopLegacyMVC.csproj:242)
  - log4net 2.0.10 (declared at src\eShopLegacyMVC\packages.config:10)

Recommended fix: upgrade to log4net 3.3.0 or later.

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** src/eShopLegacyMVC/eShopLegacyMVC.csproj:102, src/eShopLegacyMVC/packages.config:29

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr): Improper Handling of Exceptional Conditions in Newtonsoft.Json

Severity: HIGH
Vulnerable range: Newtonsoft.Json < 13.0.1
Affected dependencies:
  - Newtonsoft.Json 12.0.1 (declared at src\eShopLegacyMVC\eShopLegacyMVC.csproj:102)
  - Newtonsoft.Json 12.0.1 (declared at src\eShopLegacyMVC\packages.config:29)

Recommended fix: upgrade to Newtonsoft.Json 13.0.1 or later. eShopPorted declares 13.0.2, which is outside the vulnerable range.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-477: Use of Obsolete Function
- **Category:** Code Quality
- **Severity:** optional
- **Story Points:** 1
- **Files:** eShopLegacy.Utilities/Serializing.cs:19

Serializing.DeserializeBinary invokes BinaryFormatter.Deserialize, an obsolete and unsafe serialization API in modern .NET; the call must be revisited for the requested net10.0 target.

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopLegacyMVC/Models/CatalogItemHiLoGenerator.cs:23, src/eShopLegacyMVC/Models/Infrastructure/CatalogDBInitializer.cs:329

Both sequence readers receive Int64 values from SQL and cast them directly to Int32 without a range check. Sequence values beyond Int32.MaxValue can overflow and produce invalid catalog identifiers.

### CWE-682: Incorrect Calculation
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Services/CatalogService.cs:29, src/eShopLegacyMVC/ViewModel/PaginatedItemsViewModel.cs:23, src/eShopLegacyMVC/Controllers/CatalogController.cs:22, eShopPorted/Controllers/CatalogController.cs:23, eShopPorted/Services/CatalogService.cs:26, eShopPorted/Services/CatalogServiceMock.cs:25, eShopPorted/ViewModel/PaginatedItemsViewModel.cs:23

Index accepts pageSize and pageIndex from the request without bounds checks. Their product is used as the Skip offset and can overflow, while a zero pageSize later causes division by zero when TotalPages is calculated, yielding incorrect pagination or an exception. The ported application repeats the unbounded page parameters and offset calculation.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** eShopPorted/Modules/ApplicationModule.cs:22, eShopPorted/Services/CatalogServiceMock.cs:12, eShopPorted/Services/CatalogServiceMock.cs:16, eShopPorted/Services/CatalogServiceMock.cs:21, eShopPorted/Services/CatalogServiceMock.cs:48, eShopPorted/Services/CatalogServiceMock.cs:52, eShopPorted/Services/CatalogServiceMock.cs:55, eShopPorted/Services/CatalogServiceMock.cs:60, eShopPorted/Services/CatalogServiceMock.cs:66, src/eShopLegacyMVC/Modules/ApplicationModule.cs:22, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:12, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:16, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:21, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:48, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:52, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:55, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:60, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:66

When mock data is enabled, ApplicationModule registers CatalogServiceMock as a singleton. The singleton holds one mutable List<CatalogItem> shared across requests; catalog reads enumerate it while create, update, and remove mutate it without synchronization, allowing races and inconsistent results. eShopPorted also registers CatalogServiceMock as a singleton when mock mode is configured.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** eShopPorted/Modules/ApplicationModule.cs:22, eShopPorted/Services/CatalogServiceMock.cs:12, eShopPorted/Services/CatalogServiceMock.cs:16, eShopPorted/Services/CatalogServiceMock.cs:21, eShopPorted/Services/CatalogServiceMock.cs:48, eShopPorted/Services/CatalogServiceMock.cs:52, eShopPorted/Services/CatalogServiceMock.cs:55, eShopPorted/Services/CatalogServiceMock.cs:60, eShopPorted/Services/CatalogServiceMock.cs:66, src/eShopLegacyMVC/Modules/ApplicationModule.cs:22, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:12, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:16, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:21, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:48, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:52, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:55, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:60, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:66

When mock data is enabled, ApplicationModule registers CatalogServiceMock as a singleton. The singleton holds one mutable List<CatalogItem> shared across requests; catalog reads enumerate it while create, update, and remove mutate it without synchronization, allowing races and inconsistent results.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** eShopPorted/Modules/ApplicationModule.cs:22, eShopPorted/Services/CatalogServiceMock.cs:12, eShopPorted/Services/CatalogServiceMock.cs:16, eShopPorted/Services/CatalogServiceMock.cs:21, eShopPorted/Services/CatalogServiceMock.cs:48, eShopPorted/Services/CatalogServiceMock.cs:52, eShopPorted/Services/CatalogServiceMock.cs:55, eShopPorted/Services/CatalogServiceMock.cs:60, eShopPorted/Services/CatalogServiceMock.cs:66, src/eShopLegacyMVC/Modules/ApplicationModule.cs:22, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:12, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:16, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:21, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:48, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:52, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:55, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:60, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:66

When mock data is enabled, ApplicationModule registers CatalogServiceMock as a singleton. The singleton holds one mutable List<CatalogItem> shared across requests; catalog reads enumerate it while create, update, and remove mutate it without synchronization, allowing races and inconsistent results.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** eShopPorted/Modules/ApplicationModule.cs:22, eShopPorted/Services/CatalogServiceMock.cs:12, eShopPorted/Services/CatalogServiceMock.cs:16, eShopPorted/Services/CatalogServiceMock.cs:21, eShopPorted/Services/CatalogServiceMock.cs:48, eShopPorted/Services/CatalogServiceMock.cs:52, eShopPorted/Services/CatalogServiceMock.cs:55, eShopPorted/Services/CatalogServiceMock.cs:60, eShopPorted/Services/CatalogServiceMock.cs:66, src/eShopLegacyMVC/Modules/ApplicationModule.cs:22, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:12, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:16, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:21, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:48, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:52, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:55, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:60, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:66

When mock data is enabled, ApplicationModule registers CatalogServiceMock as a singleton. The singleton holds one mutable List<CatalogItem> shared across requests; catalog reads enumerate it while create, update, and remove mutate it without synchronization, allowing races and inconsistent results.

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 8
- **Files:** eShopPorted/Controllers/CatalogController.cs:101, eShopPorted/Controllers/CatalogController.cs:63, eShopPorted/Controllers/PicController.cs:40, eShopPorted/Controllers/PicController.cs:45, src/eShopLegacyMVC/Controllers/CatalogController.cs:100, src/eShopLegacyMVC/Controllers/CatalogController.cs:62, src/eShopLegacyMVC/Controllers/PicController.cs:39, src/eShopLegacyMVC/Controllers/PicController.cs:44

CatalogController.Create and Edit bind PictureFileName directly from POST input and persist the catalog item. PicController.Index combines that stored value with the Pics directory and reads the resulting path without verifying it remains under the restricted directory. A value containing traversal segments can therefore cause a read outside Pics. The ported application has the same flow: CatalogController binds PictureFileName from create/edit requests, then PicController combines it with an empty root and reads the resulting path without containment validation.

### CWE-23: Relative Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** eShopPorted/Controllers/CatalogController.cs:101, eShopPorted/Controllers/CatalogController.cs:63, eShopPorted/Controllers/PicController.cs:40, eShopPorted/Controllers/PicController.cs:45, src/eShopLegacyMVC/Controllers/CatalogController.cs:100, src/eShopLegacyMVC/Controllers/CatalogController.cs:62, src/eShopLegacyMVC/Controllers/PicController.cs:39, src/eShopLegacyMVC/Controllers/PicController.cs:44

CatalogController accepts PictureFileName from create/edit POSTs, and PicController.Index passes the stored value to Path.Combine with the Pics root before File.ReadAllBytes. No canonical-path containment check or rejection of relative traversal segments is performed, so a value such as ..\\..\\outside can escape Pics. The ported application has the same flow: CatalogController binds PictureFileName from create/edit requests, then PicController combines it with an empty root and reads the resulting path without containment validation.

### CWE-36: Absolute Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** eShopPorted/Controllers/CatalogController.cs:101, eShopPorted/Controllers/CatalogController.cs:63, eShopPorted/Controllers/PicController.cs:40, eShopPorted/Controllers/PicController.cs:45, src/eShopLegacyMVC/Controllers/CatalogController.cs:100, src/eShopLegacyMVC/Controllers/CatalogController.cs:62, src/eShopLegacyMVC/Controllers/PicController.cs:39, src/eShopLegacyMVC/Controllers/PicController.cs:44

CatalogController accepts PictureFileName from create/edit POSTs, and PicController.Index passes the stored value to Path.Combine with the Pics root before File.ReadAllBytes. No check rejects rooted paths or confirms the resolved path is beneath Pics, so an absolute filename may escape the intended directory. The ported application has the same flow: CatalogController binds PictureFileName from create/edit requests, then PicController combines it with an empty root and reads the resulting path without containment validation.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopPorted/Controllers/CatalogController.cs:101, eShopPorted/Controllers/CatalogController.cs:63, eShopPorted/Controllers/PicController.cs:40, eShopPorted/Controllers/PicController.cs:45, src/eShopLegacyMVC/Controllers/CatalogController.cs:100, src/eShopLegacyMVC/Controllers/CatalogController.cs:62, src/eShopLegacyMVC/Controllers/PicController.cs:39, src/eShopLegacyMVC/Controllers/PicController.cs:44

CatalogController.Create and Edit bind PictureFileName from request data. PicController.Index combines the stored value with the Pics directory and reads the resulting file without validating the resolved resource, allowing a caller-controlled catalog value to select a file outside the intended directory. The same caller-controlled resource selection is present in eShopPorted: CatalogController binds PictureFileName in Create/Edit and PicController uses it in Path.Combine and File.ReadAllBytes.

### CWE-502: Deserialization of Untrusted Data
- **Category:** Injection Attacks
- **Severity:** mandatory
- **Story Points:** 13
- **Files:** eShopLegacy.Utilities/Serializing.cs:17, eShopLegacy.Utilities/Serializing.cs:19, eShopLegacy.Utilities/Serializing.cs:21

Serializing.DeserializeBinary accepts an arbitrary caller-provided Stream and passes it directly to BinaryFormatter.Deserialize without validating or restricting the object graph. This exposes an unsafe deserialization sink to any caller able to supply untrusted stream data.

