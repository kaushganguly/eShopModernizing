# Security Assessment Report

**Generated:** 2026-10-26T21:00:54.6931400Z

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
- **Files:** eShopModernizedMVCSolution/src/eShopModernizedMVC/packages.config:10

[CVE-2026-40021](https://github.com/advisories/GHSA-4f7c-pmjv-c25w): Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters

Severity: MEDIUM

Affected dependencies:
  - log4net:2.0.12 (declared at packages.config:10; affected range < 3.3.0)

Recommended fix:
  - Upgrade log4net to 3.3.0 or later

### CVE-2024-21319: Microsoft ASP.NET Core project templates vulnerable to denial of service
- **Severity:** optional
- **Story Points:** 1
- **Files:** eShopModernizedMVCSolution/src/eShopModernizedMVC/packages.config:51, eShopModernizedMVCSolution/src/eShopModernizedMVC/packages.config:97

[CVE-2024-21319](https://github.com/advisories/GHSA-59j7-ghrg-fj52): Microsoft ASP.NET Core project templates vulnerable to denial of service

Severity: MEDIUM

Affected dependencies:
  - Microsoft.IdentityModel.JsonWebTokens:5.6.0 (declared at packages.config:51; affected range < 5.7.0)
  - System.IdentityModel.Tokens.Jwt:5.6.0 (declared at packages.config:97; affected range < 5.7.0)

Recommended fix:
  - Upgrade Microsoft.IdentityModel.JsonWebTokens to 5.7.0 or later
  - Upgrade System.IdentityModel.Tokens.Jwt to 5.7.0 or later

## CWE Findings (Code-Level Vulnerabilities)

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Models/Infrastructure/CatalogDBInitializer.cs:324-328, src/eShopModernizedMVC/Models/CatalogItemHiLoGenerator.cs:19-20

CatalogDBInitializer.GetSequenceIdFromSelectedDBSequence and CatalogItemHiLoGenerator.GetNextSequenceValue cast the Int64 value returned from the database sequence to int without checking its range (CatalogDBInitializer.cs:327; CatalogItemHiLoGenerator.cs:20). Once the sequence exceeds the Int32 range, the value can wrap and be used as a catalog item or catalog entity identifier.

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Services/ImageAzureStorage.cs:109-117

ImageAzureStorage.UpLoadImageFromFile opens a FileStream with File.OpenRead, uploads it, and exits without disposing or closing the stream (lines 111-116), retaining a file resource after the upload has completed.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Services/ImageAzureStorage.cs:109-117

ImageAzureStorage.UpLoadImageFromFile opens a file handle through File.OpenRead and never closes or disposes it after UploadFromStream returns (lines 111-116), leaking the handle beyond its required lifetime.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs:22-26, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:11-16,47-65

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a SingleInstance service. The singleton owns a mutable List<CatalogItem>; CreateCatalogItem, UpdateCatalogItem, RemoveCatalogItem and reads access that list without a lock, so concurrent web requests can race while reading or changing it.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs:22-26, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:11-16,47-65

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a SingleInstance service. The singleton owns a mutable List<CatalogItem>; CreateCatalogItem, UpdateCatalogItem, RemoveCatalogItem and reads access that list without a lock, so concurrent web requests can race while reading or changing it.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs:22-26, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:11-16,47-65

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a SingleInstance service. The singleton owns a mutable List<CatalogItem>; CreateCatalogItem, UpdateCatalogItem, RemoveCatalogItem and reads access that list without a lock, so concurrent web requests can race while reading or changing it.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs:22-26, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:11-16,47-65

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a SingleInstance service. The singleton owns a mutable List<CatalogItem>; CreateCatalogItem, UpdateCatalogItem, RemoveCatalogItem and reads access that list without a lock, so concurrent web requests can race while reading or changing it.

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** docker-compose.override.yml:6,21, docker-compose.override.nosql.yml:6

The checked-in Docker Compose overrides contain literal database authentication passwords in the SQL connection string and SA_PASSWORD environment assignment. These fixed credentials are used to authenticate to the SQL container; the values are intentionally omitted here.

### CWE-732: Incorrect Permission Assignment for Critical Resource
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Services/ImageAzureStorage.cs:39-48,89-101, src/eShopModernizedMVC/Controllers/PicController.cs:25-39

InitializeCatalogImages assigns BlobContainerPublicAccessType.Blob to the entire pics container, and UploadTempImage writes user-submitted images into that same container. Because the upload endpoint accepts requests without an authorization attribute, temporary uploads in the shared container are publicly readable rather than restricted to an intended user.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/App_Start/Startup.Auth.cs:55-62

The OpenID Connect AuthenticationFailed callback handles authentication failures and redirects with the exception message, but it does not record the failure in the application log. This leaves a security-critical sign-in failure event without an audit log entry.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** docker-compose.override.yml:6,21, docker-compose.override.nosql.yml:6

The checked-in Docker Compose overrides embed fixed database credentials in configuration instead of obtaining them from a runtime secret source. The secret values are intentionally omitted here.

### CWE-434: Unrestricted Upload of File with Dangerous Type
- **Category:** File & Path Security
- **Severity:** mandatory
- **Story Points:** 8
- **Files:** src/eShopModernizedMVC/Controllers/PicController.cs:27-46, src/eShopModernizedMVC/Services/ImageAzureStorage.cs:46-48,89-101

PicController.UploadImage validates only that Image.FromStream can decode the content as JPEG, PNG, or GIF; it does not bind the validated format to the submitted filename or MIME type. ImageAzureStorage.UploadTempImage then uses the client-controlled file.FileName as the blob name and file.ContentType as the stored content type. The same service makes the pics container publicly readable (lines 46-48), so uploaded content may be served publicly under attacker-selected metadata.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Controllers/CatalogController.cs:124,129-132, src/eShopModernizedMVC/Services/ImageAzureStorage.cs:67-86

CatalogController.Edit binds client-supplied TempImageName (line 124) and passes it to ImageAzureStorage.UpdateImage (line 131). UpdateImage only removes the literal '/pics/' prefix before using the remaining input as a blob name with GetBlockBlobReference, then copies from and deletes that blob (lines 72-86). There is no validation that the identifier refers to the expected temporary image, allowing the request to select another blob resource within the container.
