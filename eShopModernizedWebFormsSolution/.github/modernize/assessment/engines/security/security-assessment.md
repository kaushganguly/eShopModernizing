# Security Assessment Report

**Generated:** 2026-10-27T01:41:38.0502990Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 13 |
| CVE Vulnerabilities | 0 |
| CWE Vulnerabilities | 13 |
| Total Rules Assessed | 59 |
| Rules Passed | 46 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 0 |
| optional | 3 |
| potential | 10 |

## CVE Findings (Dependency Vulnerabilities)

No findings met the medium-or-higher CVE threshold. Note: all five unauthenticated GitHub Advisory API batches returned HTTP 403 rate-limit errors; the CVE result is therefore incomplete.
## CWE Findings (Code-Level Vulnerabilities)

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Models/CatalogItemHiLoGenerator.cs:19-20, src/eShopModernizedWebForms/Models/Infrastructure/CatalogDBInitializer.cs:323-327, src/eShopModernizedWebForms/Models/Infrastructure/dbo.catalog_hilo.Sequence.sql:4-9

The SQL sequence is declared as bigint, but GetNextSequenceValue and GetSequenceIdFromSelectedDBSequence cast its Int64 value to int without a range check. When a sequence value exceeds Int32.MaxValue, the conversion truncates and can yield invalid or duplicate catalog IDs.

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:107-115

UpLoadImageFromFile opens a FileStream with File.OpenRead and never disposes it, including when the Azure upload operation throws. InitializeCatalogImages invokes this helper repeatedly during startup, so the file handles remain open beyond their use.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:107-115

UpLoadImageFromFile opens a file handle using File.OpenRead but has no using/finally disposal after UploadFromStream. Repeated startup image uploads can retain file handles until finalization.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedWebForms/Modules/ApplicationModule.cs:23-27, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:11-16

When mock data is enabled, Autofac registers CatalogServiceMock as a SingleInstance. It owns mutable catalogItems state and performs unsynchronized operations on that list.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:47-65

CreateCatalogItem, UpdateCatalogItem, and RemoveCatalogItem mutate the same catalogItems list used by request handlers, without synchronization. Concurrent requests can race on the list and its generated IDs.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedWebForms/Modules/ApplicationModule.cs:23-27, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:47-65

The singleton mock catalog is shared across concurrent web requests, but its create, update, and remove operations have no exclusive synchronization around list access.

### CWE-667: Improper Locking
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:47-65

Shared catalog list mutations and the Max(Id)-then-increment allocation sequence are performed without a lock, allowing simultaneous requests to race and potentially assign duplicate IDs or corrupt list operations.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedWebForms/Modules/ApplicationModule.cs:23-27, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:11-16,47-65

The shared mutable catalogItems list is accessed by multithreaded requests through a singleton service, but the list reads and writes are not synchronized.

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopModernizedWebForms/Web.config:19-20

The CatalogDBContext connection string in Web.config contains a literal SQL password alongside the local SQL username. Although configuration builders can override settings, the committed literal remains a fallback credential.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Catalog/PicUploader.asmx.cs:64-77

Image upload validation catches all exceptions and silently marks the image invalid without recording the event. Rejected upload attempts therefore leave no security-relevant audit trail.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopModernizedWebForms/Web.config:19-20

The application configuration contains a hard-coded SQL authentication credential in the CatalogDBContext connection string; configuration builders may override it but do not remove the committed fallback credential.

### CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')
- **Category:** Injection Attacks
- **Severity:** optional
- **Story Points:** 8
- **Files:** src/eShopModernizedWebForms/Catalog/Details.aspx:16, src/eShopModernizedWebForms/Catalog/Details.aspx:23, src/eShopModernizedWebForms/Catalog/Create.aspx.cs:56-57, src/eShopModernizedWebForms/Catalog/Edit.aspx.cs:69-70

Catalog Name and Description values are accepted from form fields and persisted, then rendered into ASP.NET Label Text properties in Details.aspx without explicit HTML encoding (lines 16 and 23). This allows stored markup to be emitted as active page content to users viewing the catalog.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Catalog/Edit.aspx:18, src/eShopModernizedWebForms/Catalog/Edit.aspx.cs:78, src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:70-84

The TempImageName hidden field is client-controlled and is copied into the catalog item on edit. ImageAzureStorage.UpdateImage derives a blob key from that value, resolves it with GetBlockBlobReference, copies that blob to the item path, and deletes the source without verifying that the key belongs to a temporary upload for this item. A forged resource identifier can therefore select an unintended blob in the storage container.

