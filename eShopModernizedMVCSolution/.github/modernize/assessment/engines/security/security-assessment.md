# Security Assessment Report

**Generated:** 2026-10-27T01:43:13.665Z

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

No CVE findings met the medium severity threshold. The CLI dependency listing was unavailable for this legacy project; packages.config was used for package enumeration.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Models/CatalogItemHiLoGenerator.cs:19-20, src/eShopModernizedMVC/Models/Infrastructure/CatalogDBInitializer.cs:326-327

Both sequence-reading paths fetch an Int64 SQL value and cast it directly to int without a range check. If a sequence value exceeds the Int32 range, the identifier value can overflow and no longer represent the sequence value.

### CWE-682: Incorrect Calculation
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Services/CatalogService.cs:24-29, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:22-26

The client-provided pageSize and pageIndex are multiplied as 32-bit integers before being passed to Skip. Large values can overflow the offset calculation and yield an unintended page offset or query behavior.

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Services/ImageAzureStorage.cs:109-117

UpLoadImageFromFile opens a FileStream using File.OpenRead and passes it to UploadFromStream, but does not dispose the stream after the upload, leaving the resource unreleased.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Services/ImageAzureStorage.cs:109-117

UpLoadImageFromFile opens a file handle using File.OpenRead and never closes or disposes it after UploadFromStream completes.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs:22-26, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:11-16,47-66, docker-compose.override.yml:7

When UseMockData is enabled (the override defaults it to true), Autofac registers CatalogServiceMock as a singleton. The singleton owns a mutable List<CatalogItem> that concurrent MVC requests read and modify in GetCatalogItemsPaginated, CreateCatalogItem, UpdateCatalogItem, and RemoveCatalogItem without synchronization. This creates a race on shared catalog state.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs:22-26, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:11-16,47-66, docker-compose.override.yml:7

When UseMockData is enabled (the override defaults it to true), Autofac registers CatalogServiceMock as a singleton. The singleton owns a mutable List<CatalogItem> that concurrent MVC requests read and modify in GetCatalogItemsPaginated, CreateCatalogItem, UpdateCatalogItem, and RemoveCatalogItem without synchronization. This creates a race on shared catalog state.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs:22-26, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:11-16,47-66, docker-compose.override.yml:7

When UseMockData is enabled (the override defaults it to true), Autofac registers CatalogServiceMock as a singleton. The singleton owns a mutable List<CatalogItem> that concurrent MVC requests read and modify in GetCatalogItemsPaginated, CreateCatalogItem, UpdateCatalogItem, and RemoveCatalogItem without synchronization. This creates a race on shared catalog state.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs:22-26, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:11-16,47-66, docker-compose.override.yml:7

When UseMockData is enabled (the override defaults it to true), Autofac registers CatalogServiceMock as a singleton. The singleton owns a mutable List<CatalogItem> that concurrent MVC requests read and modify in GetCatalogItemsPaginated, CreateCatalogItem, UpdateCatalogItem, and RemoveCatalogItem without synchronization. This creates a race on shared catalog state.

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** docker-compose.override.yml:21

The Compose override embeds a static SQL Server administrator password in the sql.data service environment. The committed credential is not environment-sourced or generated; its literal value is intentionally omitted from this report.

### CWE-732: Incorrect Permission Assignment for Critical Resource
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Services/ImageAzureStorage.cs:46-48

InitializeCatalogImages sets the entire pics blob container to Blob public access. This allows unauthenticated readers to fetch any known blob URI in the container, including temporary uploaded images, rather than restricting reads to an authenticated application path.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/App_Start/Startup.Auth.cs:55-62, src/eShopModernizedMVC/Controllers/AccountController.cs:14-21

The OpenID Connect AuthenticationFailed callback handles and redirects the authentication exception without logging it. AccountController records that sign-in was initiated, but does not record a failed sign-in outcome or associated failure details.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** docker-compose.override.yml:21

A committed Compose override contains a static SQL Server administrator credential used by the application stack. The literal is not reproduced here.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Controllers/CatalogController.cs:124,129-136, src/eShopModernizedMVC/Services/ImageAzureStorage.cs:67-86

CatalogController.Edit binds TempImageName from the POST request. ImageAzureStorage.UpdateImage derives a blob identifier from this value, resolves it with GetBlockBlobReference, and deletes that referenced blob. The request-controlled identifier is not checked to ensure it is the temporary blob issued by the upload flow, so a crafted value can select and delete a different blob within the configured pics container.
