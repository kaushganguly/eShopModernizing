# Security Assessment Report

**Generated:** 2026-10-27T02:20:07.2511273Z

## Summary

| Metric | Count |
|---|---:|
| Total Findings | 14 |
| CVE Vulnerabilities | 1 |
| CWE Vulnerabilities | 13 |
| Total Rules Assessed | 59 |
| Rules Passed | 46 |

### By Severity

| Severity | Count |
|---|---:|
| mandatory | 1 |
| optional | 5 |
| potential | 8 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters
- **Severity:** optional
- **Story Points:** 1
- **Files:** src/eShopLegacyMVC/packages.config:10, eShopPorted/eShopPorted.csproj:13

[CVE-2026-40021](https://github.com/advisories/GHSA-4f7c-pmjv-c25w): Apache log4net versions before 3.3.0 do not sanitize XML 1.0 forbidden characters in MDC properties and identity, which can cause affected log events to be silently dropped. Severity: MEDIUM. Affected declarations: log4net 2.0.10 in src/eShopLegacyMVC/packages.config:10 and eShopPorted/eShopPorted.csproj:13. Recommended fix: upgrade to log4net 3.3.0 or later.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-477: Use of Obsolete Function
- **Category:** Code Quality
- **Severity:** optional
- **Story Points:** 1
- **Files:** eShopLegacy.Utilities/Serializing.cs:11,19

BinaryFormatter is used for serialization and deserialization. BinaryFormatter is obsolete in modern .NET and is no longer a supported serialization mechanism.

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopLegacyMVC/Models/Infrastructure/CatalogDBInitializer.cs:328-329

GetSequenceIdFromSelectedDBSequence receives an Int64 sequence value and casts it directly to int. Values outside the Int32 range are truncated/wrapped, producing an incorrect sequence identifier.

### CWE-682: Incorrect Calculation
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Services/CatalogService.cs:29, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:25, eShopPorted/Services/CatalogService.cs:26, eShopPorted/Services/CatalogServiceMock.cs:25

GetCatalogItemsPaginated computes pageSize * pageIndex using unchecked Int32 arithmetic for Skip. Large request values can overflow and produce an incorrect pagination offset.

### CWE-789: Memory Allocation with Excessive Size Value
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Controllers/CatalogController.cs:22-25, src/eShopLegacyMVC/Services/CatalogService.cs:22-30, eShopPorted/Controllers/CatalogController.cs:23-26, eShopPorted/Services/CatalogService.cs:22-28

The pageSize query parameter is accepted without an upper bound and passed to Take(pageSize).ToList(), allowing a client to request materialization of an arbitrarily large page from the catalog.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs:18-22, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:12-16,21,35,48-66

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a SingleInstance. Its shared mutable catalogItems list is accessed by request methods and modified by CreateCatalogItem, UpdateCatalogItem, and RemoveCatalogItem without synchronization, allowing concurrent collection reads and writes.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs:18-22, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:12-16,21,35,48-66

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a SingleInstance. Its shared mutable catalogItems list is accessed by request methods and modified by CreateCatalogItem, UpdateCatalogItem, and RemoveCatalogItem without synchronization, allowing concurrent collection reads and writes.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs:18-22, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:12-16,21,35,48-66

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a SingleInstance. Its shared mutable catalogItems list is accessed by request methods and modified by CreateCatalogItem, UpdateCatalogItem, and RemoveCatalogItem without synchronization, allowing concurrent collection reads and writes.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopLegacyMVC/Modules/ApplicationModule.cs:18-22, src/eShopLegacyMVC/Services/CatalogServiceMock.cs:12-16,21,35,48-66

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a SingleInstance. Its shared mutable catalogItems list is accessed by request methods and modified by CreateCatalogItem, UpdateCatalogItem, and RemoveCatalogItem without synchronization, allowing concurrent collection reads and writes.

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 8
- **Files:** src/eShopLegacyMVC/Controllers/CatalogController.cs:62, src/eShopLegacyMVC/Controllers/CatalogController.cs:100, src/eShopLegacyMVC/Controllers/PicController.cs:39-45, eShopPorted/Controllers/CatalogController.cs:63, eShopPorted/Controllers/CatalogController.cs:101, eShopPorted/Controllers/PicController.cs:40-47

The MVC create and edit actions bind PictureFileName from request model data. PicController.Index uses that stored value in Path.Combine and reads the resulting path with File.ReadAllBytes without verifying that the resolved path remains under the pictures directory. A crafted filename containing traversal segments can escape the intended directory. The ported controller combines the filename with an empty webRoot, leaving the read unconstrained.

### CWE-23: Relative Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Controllers/CatalogController.cs:62, src/eShopLegacyMVC/Controllers/CatalogController.cs:100, src/eShopLegacyMVC/Controllers/PicController.cs:39-45, eShopPorted/Controllers/CatalogController.cs:63, eShopPorted/Controllers/CatalogController.cs:101, eShopPorted/Controllers/PicController.cs:40-47

PictureFileName is request-bindable through catalog create and edit. PicController.Index passes it to Path.Combine and reads the result without canonicalizing or checking the parent directory, so relative segments such as .. can escape the intended picture directory.

### CWE-36: Absolute Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Controllers/CatalogController.cs:62, src/eShopLegacyMVC/Controllers/CatalogController.cs:100, src/eShopLegacyMVC/Controllers/PicController.cs:39-45, eShopPorted/Controllers/CatalogController.cs:63, eShopPorted/Controllers/CatalogController.cs:101, eShopPorted/Controllers/PicController.cs:40-47

PictureFileName is request-bindable through catalog create and edit. An absolute filename supplied to Path.Combine can replace the intended base directory; the resulting path is passed to File.ReadAllBytes without a containment check.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopLegacyMVC/Controllers/CatalogController.cs:62, src/eShopLegacyMVC/Controllers/CatalogController.cs:100, src/eShopLegacyMVC/Controllers/PicController.cs:39-45, eShopPorted/Controllers/CatalogController.cs:63, eShopPorted/Controllers/CatalogController.cs:101, eShopPorted/Controllers/PicController.cs:40-47

Catalog create and edit request models bind PictureFileName. PicController.Index uses this value as the file resource identifier passed to Path.Combine and File.ReadAllBytes without restricting it to an approved filename. This overlaps the path traversal already documented under CWE-22/23/36.

### CWE-502: Deserialization of Untrusted Data
- **Category:** Injection Attacks
- **Severity:** mandatory
- **Story Points:** 13
- **Files:** eShopLegacy.Utilities/Serializing.cs:17-21

Serializing.DeserializeBinary is a public method that passes its Stream directly to BinaryFormatter.Deserialize without validating the stream contents. No call site for DeserializeBinary was found in the target solution, so externally reachable exploitability is not established by this scan.
