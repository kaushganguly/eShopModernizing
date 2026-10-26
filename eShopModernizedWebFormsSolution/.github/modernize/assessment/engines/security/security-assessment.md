# Security Assessment Report

**Generated:** 2026-10-26T23:16:04.8205838Z

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
| mandatory | 0 |
| optional | 2 |
| potential | 11 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters
- **Severity:** optional (raw: medium)
- **Story Points:** 1
- **Files:** src/eShopModernizedWebForms/packages.config:11

[CVE-2026-40021](https://github.com/advisories/GHSA-4f7c-pmjv-c25w): Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters

Severity: MEDIUM

Affected dependencies:
  - log4net 2.0.10 (packages.config:11); vulnerable range < 3.3.0

Recommended fix: upgrade affected packages to a patched version listed by the advisory.

### CVE-2024-21319: Microsoft ASP.NET Core project templates vulnerable to denial of service
- **Severity:** optional (raw: medium)
- **Story Points:** 1
- **Files:** src/eShopModernizedWebForms/packages.config:53, src/eShopModernizedWebForms/packages.config:98

[CVE-2024-21319](https://github.com/advisories/GHSA-59j7-ghrg-fj52): Microsoft ASP.NET Core project templates vulnerable to denial of service

Severity: MEDIUM

Affected dependencies:
  - System.IdentityModel.Tokens.Jwt 5.6.0 (packages.config:98); vulnerable range < 5.7.0
  - System.IdentityModel.Tokens.Jwt 5.6.0 (packages.config:98); vulnerable range >= 6.5.0, < 6.34.0
  - System.IdentityModel.Tokens.Jwt 5.6.0 (packages.config:98); vulnerable range >= 7.0.0-preview, < 7.1.2
  - Microsoft.IdentityModel.JsonWebTokens 5.6.0 (packages.config:53); vulnerable range < 5.7.0
  - Microsoft.IdentityModel.JsonWebTokens 5.6.0 (packages.config:53); vulnerable range >= 6.5.0, < 6.34.0
  - Microsoft.IdentityModel.JsonWebTokens 5.6.0 (packages.config:53); vulnerable range >= 7.0.0-preview, < 7.1.2

Recommended fix: upgrade affected packages to a patched version listed by the advisory.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Models/CatalogItemHiLoGenerator.cs:19-20, src/eShopModernizedWebForms/Models/Infrastructure/CatalogDBInitializer.cs:323-327

CatalogItemHiLoGenerator.GetNextSequenceValue and CatalogDBInitializer.GetSequenceIdFromSelectedDBSequence read Int64 database sequence values and cast them directly to int. Values outside the Int32 range are truncated, potentially reusing or corrupting catalog identifiers.

### CWE-682: Incorrect Calculation
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedWebForms/Default.aspx.cs:31-35, src/eShopModernizedWebForms/Services/CatalogService.cs:20-33, src/eShopModernizedWebForms/ViewModel/PaginatedItemsViewModel.cs:18-24

Default.Page_Load accepts route-provided size and index and passes them through without range checks. CatalogService.GetCatalogItemsPaginated multiplies them as 32-bit integers for Skip and uses size in Take; the view model divides by pageSize. Extreme values can overflow the offset, and a zero size causes division by zero.

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:107-115

ImageAzureStorage.UpLoadImageFromFile opens a FileStream with File.OpenRead and uploads it, but never disposes or closes the stream after the upload, leaving the resource unreleased.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:107-115

ImageAzureStorage.UpLoadImageFromFile opens a file handle using File.OpenRead and does not close or dispose it after UploadFromStream completes.

### CWE-789: Memory Allocation with Excessive Size Value
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedWebForms/Default.aspx.cs:31-35, src/eShopModernizedWebForms/Services/CatalogService.cs:20-30

The route-controlled size is passed directly to CatalogService.GetCatalogItemsPaginated, where it is used as the Take count and the results are materialized with ToList without a maximum bound. A very large page size can cause an unnecessarily large result set and memory consumption.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedWebForms/Modules/ApplicationModule.cs:23-27, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:11-15, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:47-66

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a singleton. CatalogServiceMock stores catalogItems in a mutable List and CreateCatalogItem, UpdateCatalogItem, and RemoveCatalogItem modify it without a lock, allowing concurrent requests to race.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedWebForms/Modules/ApplicationModule.cs:23-27, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:11-15, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:47-66

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a singleton. CatalogServiceMock stores catalogItems in a mutable List and CreateCatalogItem, UpdateCatalogItem, and RemoveCatalogItem modify it without a lock, allowing concurrent requests to race.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedWebForms/Modules/ApplicationModule.cs:23-27, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:11-15, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:47-66

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a singleton. CatalogServiceMock stores catalogItems in a mutable List and CreateCatalogItem, UpdateCatalogItem, and RemoveCatalogItem modify it without a lock, allowing concurrent requests to race.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedWebForms/Modules/ApplicationModule.cs:23-27, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:11-15, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:47-66

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a singleton. CatalogServiceMock stores catalogItems in a mutable List and CreateCatalogItem, UpdateCatalogItem, and RemoveCatalogItem modify it without a lock, allowing concurrent requests to race.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Catalog/PicUploader.asmx.cs:30-49, src/eShopModernizedWebForms/Catalog/PicUploader.asmx.cs:58-61

PicUploader.UploadImage logs only a generic processing-start message. The invalid-image rejection path returns HTTP 400 without recording the event, and successful uploads do not log the outcome or affected catalog item, leaving these security-relevant upload events without an auditable record.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Catalog/PicUploader.asmx.cs:41-52, src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:87-97

PicUploader.UploadImage reads itemId from the request form and passes the parsed value with the uploaded file to IImageService.UploadTempImage. ImageAzureStorage.UploadTempImage uses that externally supplied item ID and file.FileName to construct the Azure blob identifier (path + file.FileName.ToLower()) and uploads to that blob without restricting the selected catalog-item namespace. A caller can therefore select another item's blob namespace through the resource identifier.

