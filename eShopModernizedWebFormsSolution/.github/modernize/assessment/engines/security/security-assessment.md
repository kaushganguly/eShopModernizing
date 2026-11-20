# Security Assessment Report

**Generated:** 2026-11-20T11:09:34.5182093Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 14 |
| CVE Vulnerabilities meeting high threshold | 0 |
| CWE Vulnerabilities | 14 |
| Total Rules Assessed | 59 |
| Rules Passed | 45 |

Two medium-severity dependency advisories were below the required high CVE threshold and are not included in the merged findings.

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 0 |
| optional | 4 |
| potential | 10 |

## CVE Findings (Dependency Vulnerabilities)

No CVE findings meet the minimum high-severity threshold.
## CWE Findings (Code-Level Vulnerabilities)

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Models/CatalogItemHiLoGenerator.cs:20, src/eShopModernizedWebForms/Models/Infrastructure/CatalogDBInitializer.cs:326

CatalogItemHiLoGenerator.GetNextSequenceValue and CatalogDBInitializer.GetSequenceIdFromSelectedDBSequence cast the Int64 value returned by a SQL sequence to int without a range check. Values beyond Int32 range can wrap/truncate and produce invalid or duplicate identifiers.

### CWE-682: Incorrect Calculation
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedWebForms/Default.aspx.cs:26, src/eShopModernizedWebForms/ViewModel/PaginatedItemsViewModel.cs:20

Default.aspx.cs accepts the route size as pageSize and passes it through. PaginatedItemsViewModel divides count by pageSize without validating that it is nonzero; a size of zero causes an unhandled divide-by-zero exception while building the response.

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:109-114

ImageAzureStorage.UpLoadImageFromFile opens a FileStream with File.OpenRead and passes it to UploadFromStream, but does not dispose or close the stream after the upload, leaving the file handle open.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:109-114

ImageAzureStorage.UpLoadImageFromFile opens a FileStream with File.OpenRead and passes it to UploadFromStream, but does not dispose or close the stream after the upload, leaving the file handle open.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedWebForms/Modules/ApplicationModule.cs:25-27, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:11-15

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a singleton. The singleton owns a mutable List<CatalogItem> that request handlers enumerate and mutate without a lock, allowing concurrent web requests to race.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:11, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:34-65

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a singleton. The singleton owns a mutable List<CatalogItem> that request handlers enumerate and mutate without a lock, allowing concurrent web requests to race.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedWebForms/Modules/ApplicationModule.cs:25-27, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:34-65

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a singleton. The singleton owns a mutable List<CatalogItem> that request handlers enumerate and mutate without a lock, allowing concurrent web requests to race.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedWebForms/Modules/ApplicationModule.cs:25-27, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:34-65

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a singleton. The singleton owns a mutable List<CatalogItem> that request handlers enumerate and mutate without a lock, allowing concurrent web requests to race.

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** docker-compose.override.yml:21, docker-compose.override.yml:26

The Compose configuration embeds the SQL Server administrator password in the database container environment and health-check command. The credential value is intentionally omitted from this assessment output.

### CWE-732: Incorrect Permission Assignment for Critical Resource
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:47, src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:93

ImageAzureStorage.InitializeCatalogImages assigns blob-level public read access to the pics container. UploadTempImage stores temporary user uploads in that same container, so temporary objects are also readable without application authentication.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Catalog/Create.aspx.cs:50, src/eShopModernizedWebForms/Catalog/Edit.aspx.cs:62, src/eShopModernizedWebForms/Catalog/Delete.aspx.cs:34, src/eShopModernizedWebForms/App_Start/Startup.Auth.cs:40

The Create_Click, Save_Click, and Delete_Click mutation handlers do not emit audit events, and the OIDC AuthenticationFailed callback redirects without logging the failure. Page-load messages do not record the security-relevant outcome or changed item.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** docker-compose.override.yml:21, docker-compose.override.yml:26

The Compose configuration embeds a SQL Server administrator credential for the database container and its health check. The credential value is intentionally omitted from this assessment output.

### CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')
- **Category:** Injection Attacks
- **Severity:** optional
- **Story Points:** 8
- **Files:** src/eShopModernizedWebForms/Catalog/Details.aspx:16, src/eShopModernizedWebForms/Catalog/Delete.aspx:18, src/eShopModernizedWebForms/Catalog/Create.aspx.cs:56-57, src/eShopModernizedWebForms/Services/CatalogService.cs:50

Catalog item Name and Description values are accepted from form input and persisted by CatalogService. Details.aspx and Delete.aspx render stored values with asp:Label Text data bindings rather than an HTML-encoding binding, allowing stored markup to be emitted as executable page content.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Catalog/PicUploader.asmx.cs:36-62, src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:85-94

PicUploader passes an uploaded file to ImageAzureStorage.UploadTempImage, which builds the blob object identifier from the caller-controlled file.FileName without normalizing path separators. The container is fixed, but the client can influence the object key and path segments within that storage container.

