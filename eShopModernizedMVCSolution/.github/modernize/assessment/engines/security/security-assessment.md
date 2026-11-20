# Security Assessment Report

**Generated:** 2026-11-20T06:41:54.2034680Z

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
| optional | 4 |
| potential | 9 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters
- **Severity:** optional
- **Story Points:** 1
- **Files:** src/eShopModernizedMVC/packages.config

[CVE-2026-40021](https://github.com/advisories/GHSA-4f7c-pmjv-c25w): Apache Log4net XML layout issue. Severity: MEDIUM. Affected dependency: log4net 2.0.12 (src/eShopModernizedMVC/packages.config), vulnerable range < 3.3.0. Recommended fix: upgrade to 3.3.0 or later.

### CVE-2024-21319: Microsoft ASP.NET Core project templates vulnerable to denial of service
- **Severity:** optional
- **Story Points:** 1
- **Files:** src/eShopModernizedMVC/packages.config

[CVE-2024-21319](https://github.com/advisories/GHSA-59j7-ghrg-fj52): Microsoft IdentityModel JWT denial of service. Severity: MEDIUM. Affected dependencies: System.IdentityModel.Tokens.Jwt 5.6.0 and Microsoft.IdentityModel.JsonWebTokens 5.6.0 (src/eShopModernizedMVC/packages.config); for both, the applicable range is < 5.7.0. Recommended fix: upgrade both to 5.7.0 or later.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Models/CatalogItemHiLoGenerator.cs, src/eShopModernizedMVC/Models/Infrastructure/CatalogDBInitializer.cs

GetNextSequenceValue casts a SQL Int64 sequence result to int at CatalogItemHiLoGenerator.cs:20; GetSequenceIdFromSelectedDBSequence repeats the cast at CatalogDBInitializer.cs:327. Values beyond Int32 range are truncated/wrapped and used as catalog IDs.

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Services/ImageAzureStorage.cs

UpLoadImageFromFile opens a FileStream with File.OpenRead at ImageAzureStorage.cs:111 and passes it to UploadFromStream at line 116, but never disposes the stream after upload, retaining the file resource.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Services/ImageAzureStorage.cs

UpLoadImageFromFile opens a file handle using File.OpenRead at ImageAzureStorage.cs:111 and never closes or disposes it after UploadFromStream completes at line 116.

### CWE-789: Memory Allocation with Excessive Size Value
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Controllers/CatalogController.cs, src/eShopModernizedMVC/Services/CatalogService.cs

CatalogController.Index accepts the query-string pageSize without a maximum (CatalogController.cs:25), then CatalogService.GetCatalogItemsPaginated passes it to Take(pageSize) and materializes the result with ToList (CatalogService.cs:23-27). A very large pageSize can drive an unbounded database result into application memory.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs, src/eShopModernizedMVC/Services/CatalogServiceMock.cs, src/eShopModernizedMVC/Web.config

Web.config enables UseMockData by default at line 36. ApplicationModule registers CatalogServiceMock as a SingleInstance at lines 22-26, while the service shares a mutable List<CatalogItem> and performs unsynchronized reads, adds, replacements, and removals (CatalogServiceMock.cs:11-15, 19-66). Concurrent web requests can race on the same list.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs, src/eShopModernizedMVC/Services/CatalogServiceMock.cs, src/eShopModernizedMVC/Web.config

Web.config enables UseMockData by default at line 36. ApplicationModule registers CatalogServiceMock as a SingleInstance at lines 22-26, while the service shares a mutable List<CatalogItem> and performs unsynchronized reads, adds, replacements, and removals (CatalogServiceMock.cs:11-15, 19-66). Concurrent web requests can race on the same list.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs, src/eShopModernizedMVC/Services/CatalogServiceMock.cs, src/eShopModernizedMVC/Web.config

Web.config enables UseMockData by default at line 36. ApplicationModule registers CatalogServiceMock as a SingleInstance at lines 22-26, while the service shares a mutable List<CatalogItem> and performs unsynchronized reads, adds, replacements, and removals (CatalogServiceMock.cs:11-15, 19-66). Concurrent web requests can race on the same list.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs, src/eShopModernizedMVC/Services/CatalogServiceMock.cs, src/eShopModernizedMVC/Web.config

Web.config enables UseMockData by default at line 36. ApplicationModule registers CatalogServiceMock as a SingleInstance at lines 22-26, while the service shares a mutable List<CatalogItem> and performs unsynchronized reads, adds, replacements, and removals (CatalogServiceMock.cs:11-15, 19-66). Concurrent web requests can race on the same list.

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** docker-compose.override.yml, docker-compose.override.nosql.yml

The SQL Server container configuration supplies a literal SA_PASSWORD, and the application connection-string environment override also includes a literal SQL password (docker-compose.override.yml:6,21; docker-compose.override.nosql.yml:6). These development credentials are stored in version-controlled deployment configuration.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/App_Start/Startup.Auth.cs, src/eShopModernizedMVC/Controllers/AccountController.cs

The OpenID Connect AuthenticationFailed callback handles the failure and redirects with the exception message at Startup.Auth.cs:57-61, but does not log the failed authentication. AccountController.SignIn logs initiation only (AccountController.cs:14-18), so failed sign-in outcomes are not recorded.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** docker-compose.override.yml, docker-compose.override.nosql.yml

Both Docker override configurations contain literal SQL Server SA credentials for the database container and application connection string (docker-compose.override.yml:6,21; docker-compose.override.nosql.yml:6). The credential values are not repeated here.

