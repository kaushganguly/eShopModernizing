# Security Assessment Report

**Generated:** 2026-11-20T06:54:10.171885Z

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
| optional | 5 |
| potential | 8 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters
- **Severity:** optional
- **Story Points:** 1
- **Files:** src/eShopModernizedWebForms/packages.config:11

[CVE-2026-40021](https://github.com/advisories/GHSA-4f7c-pmjv-c25w): Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters

Severity: MEDIUM

Affected dependencies:
  - log4net:2.0.10 (declared at src/eShopModernizedWebForms/packages.config:11); affected range: < 3.3.0; first patched: not specified

Recommended fix: Upgrade affected dependencies to their first patched versions where available.

### CVE-2024-21319: Microsoft ASP.NET Core project templates vulnerable to denial of service
- **Severity:** optional
- **Story Points:** 1
- **Files:** src/eShopModernizedWebForms/packages.config:53, src/eShopModernizedWebForms/packages.config:98

[CVE-2024-21319](https://github.com/advisories/GHSA-59j7-ghrg-fj52): Microsoft ASP.NET Core project templates vulnerable to denial of service

Severity: MEDIUM

Affected dependencies:
  - System.IdentityModel.Tokens.Jwt:5.6.0 (declared at src/eShopModernizedWebForms/packages.config:98); affected range: < 5.7.0; first patched: not specified
  - System.IdentityModel.Tokens.Jwt:5.6.0 (declared at src/eShopModernizedWebForms/packages.config:98); affected range: >= 6.5.0, < 6.34.0; first patched: not specified
  - System.IdentityModel.Tokens.Jwt:5.6.0 (declared at src/eShopModernizedWebForms/packages.config:98); affected range: >= 7.0.0-preview, < 7.1.2; first patched: not specified
  - Microsoft.IdentityModel.JsonWebTokens:5.6.0 (declared at src/eShopModernizedWebForms/packages.config:53); affected range: < 5.7.0; first patched: not specified
  - Microsoft.IdentityModel.JsonWebTokens:5.6.0 (declared at src/eShopModernizedWebForms/packages.config:53); affected range: >= 6.5.0, < 6.34.0; first patched: not specified
  - Microsoft.IdentityModel.JsonWebTokens:5.6.0 (declared at src/eShopModernizedWebForms/packages.config:53); affected range: >= 7.0.0-preview, < 7.1.2; first patched: not specified

Recommended fix: Upgrade affected dependencies to their first patched versions where available.


## CWE Findings (Code-Level Vulnerabilities)

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Models/Infrastructure/CatalogDBInitializer.cs:326, src/eShopModernizedWebForms/Models/CatalogItemHiLoGenerator.cs:20

Both sequence generators cast a database Int64 sequence result directly to int without a range check. If a sequence value exceeds the Int32 range, the conversion can overflow or fail and produce an invalid catalog identifier.

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:107-114

UpLoadImageFromFile opens a file stream with File.OpenRead and uploads it, but never disposes or closes the stream. InitializeCatalogImages calls this method for each seeded image, leaving file handles open for garbage collection rather than releasing them at the end of each upload.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:107-114

UpLoadImageFromFile opens a file handle using File.OpenRead and does not dispose it after uploading. Repeated image uploads during catalog initialization can retain file handles beyond their effective lifetime.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedWebForms/Modules/ApplicationModule.cs:23-27, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:11-15,47-65

When mock data is enabled, Autofac registers CatalogServiceMock as a single instance. Its catalogItems List is shared across requests, while create, update, and remove operations mutate the list without synchronization.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:11-15,18-35,47-65

The singleton's mutable catalogItems list is accessed by catalog reads and concurrent create, update, and remove requests without a lock or other synchronization, allowing collection races and inconsistent results.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedWebForms/Modules/ApplicationModule.cs:23-27, src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:11-15,47-65

Multiple web requests can concurrently reach the singleton's mutable catalog list. The service performs collection reads and mutations without serializing access, so concurrent operations can interfere with one another.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedWebForms/Services/CatalogServiceMock.cs:11-15,18-35,47-65

The shared catalog list is read and modified by request-serving methods without any synchronization mechanism.

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** docker-compose.override.yml:21

The SQL Server container is configured with a fixed SA password directly in the Compose environment. The health check also relies on that credential, making it a hard-coded password used for component communication.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/App_Start/Startup.Auth.cs:40-45

The OpenID Connect AuthenticationFailed callback handles and redirects after an authentication failure but does not record the failure in the application log, leaving this security-relevant event unaudited.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** docker-compose.override.yml:21

The SQL Server container uses a fixed SA credential embedded in the Docker Compose environment configuration rather than a runtime-provided secret.

### CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')
- **Category:** Injection Attacks
- **Severity:** optional
- **Story Points:** 8
- **Files:** src/eShopModernizedWebForms/Catalog/Create.aspx:1, src/eShopModernizedWebForms/Catalog/Create.aspx.cs:56-57, src/eShopModernizedWebForms/Catalog/Details.aspx:16,23

Create.aspx disables request validation. Create_Click copies the submitted Name and Description fields to a catalog item, which is persisted by the catalog service. Details.aspx later binds those values directly to ASP.NET Label.Text using unencoded <%# ... %> expressions at lines 16 and 23, allowing stored markup or script from an item value to be rendered in other users' pages.
