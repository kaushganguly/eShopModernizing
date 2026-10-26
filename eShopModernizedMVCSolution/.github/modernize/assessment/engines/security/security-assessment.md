# Security Assessment Report

**Generated:** 2026-10-26T20:01:49.460Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 12 |
| CVE Vulnerabilities | 2 |
| CWE Vulnerabilities | 10 |
| Total Rules Assessed | 59 |
| Rules Passed | 49 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 0 |
| optional | 4 |
| potential | 8 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters
- **Severity:** optional
- **Story Points:** 1
- **Files:** src/eShopModernizedMVC/packages.config:10

[CVE-2026-40021](https://github.com/advisories/GHSA-4f7c-pmjv-c25w): Apache Log4net versions before 3.3.0 fail to sanitize forbidden XML 1.0 characters in MDC keys and values or identity data when the XML layouts are used, which can silently drop log events. The declared log4net version 2.0.12 falls in the affected range (< 3.3.0). The project currently configures PatternLayout rather than either affected XML layout, so the vulnerable feature is not observed as enabled. Recommended fix: upgrade to log4net 3.3.0 or later. The legacy NuGet CLI dependency-tree command could not load Microsoft.WebApplication.targets; this result is based on packages.config direct dependencies only.

### CVE-2024-21319: Microsoft ASP.NET Core project templates vulnerable to denial of service
- **Severity:** optional
- **Story Points:** 1
- **Files:** src/eShopModernizedMVC/packages.config:51, src/eShopModernizedMVC/packages.config:97

[CVE-2024-21319](https://github.com/advisories/GHSA-59j7-ghrg-fj52): the advisory maps the affected ranges for Microsoft.IdentityModel.JsonWebTokens and System.IdentityModel.Tokens.Jwt. The declared versions 5.6.0 are below the first patched version 5.7.0. Applicability caveat: the advisory describes ASP.NET Core projects generated with affected JWT-auth templates; this target is ASP.NET MVC 5 on .NET Framework and uses OWIN OpenID Connect, so the template-specific exploit preconditions were not confirmed. Recommended fix for affected package usage: upgrade the declared packages to patched versions. The legacy NuGet CLI dependency-tree command could not load Microsoft.WebApplication.targets; this result is based on packages.config direct dependencies only.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Models/CatalogItemHiLoGenerator.cs:20, src/eShopModernizedMVC/Models/Infrastructure/CatalogDBInitializer.cs:327

Both methods cast the Int64 returned by a database sequence directly to Int32 without range checking. Values above the Int32 range can wrap or truncate, producing incorrect catalog identifiers.

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Services/ImageAzureStorage.cs:111

UpLoadImageFromFile opens a FileStream with File.OpenRead and uploads from it, but never disposes the stream. Repeated initialization can retain file handles beyond their effective lifetime.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Services/ImageAzureStorage.cs:111

UpLoadImageFromFile opens a file handle via File.OpenRead and does not close or dispose it after UploadFromStream completes, potentially leaking handles during repeated image initialization.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs:22-26, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:11-15, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:47-65

CatalogServiceMock is registered as a single instance while storing mutable catalogItems in a List<CatalogItem>. Concurrent request methods enumerate, add, replace, and remove entries without synchronization, permitting races and inconsistent list state.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs:22-26, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:11-15, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:47-65

CatalogServiceMock is registered as a single instance while storing mutable catalogItems in a List<CatalogItem>. Concurrent request methods enumerate, add, replace, and remove entries without synchronization, permitting races and inconsistent list state.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs:22-26, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:11-15, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:47-65

CatalogServiceMock is registered as a single instance while storing mutable catalogItems in a List<CatalogItem>. Concurrent request methods enumerate, add, replace, and remove entries without synchronization, permitting races and inconsistent list state.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs:22-26, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:11-15, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:47-65

CatalogServiceMock is registered as a single instance while storing mutable catalogItems in a List<CatalogItem>. Concurrent request methods enumerate, add, replace, and remove entries without synchronization, permitting races and inconsistent list state.

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** docker-compose.override.nosql.yml:21

The SQL Server service configuration contains a literal administrator password in the compose environment. The fixed credential is used by the container for inbound database authentication; its value is intentionally omitted here.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Controllers/AccountController.cs:16, src/eShopModernizedMVC/Controllers/AccountController.cs:25

Sign-in and sign-out actions log only that processing began. The application has no corresponding logging of authentication success, failure, or outcome, leaving security-critical authentication events unaudited.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** docker-compose.override.nosql.yml:21

The SQL Server container is configured with a hard-coded administrator credential rather than a per-deployment secret. Credential contents are omitted here.
