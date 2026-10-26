# Security Assessment Report

**Generated:** 2026-10-26T19:28:04.4656470Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 6 |
| CVE Vulnerabilities | 2 |
| CWE Vulnerabilities | 4 |
| Total Rules Assessed | 59 |
| Rules Passed | 55 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 0 |
| optional | 2 |
| potential | 4 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2024-21319: Microsoft ASP.NET Core project templates vulnerable to denial of service
- **Severity:** optional
- **Story Points:** 1
- **Files:** src/eShopModernizedMVC/packages.config:51, src/eShopModernizedMVC/packages.config:97

[CVE-2024-21319](https://github.com/advisories/GHSA-59j7-ghrg-fj52): Microsoft ASP.NET Core project templates vulnerable to denial of service

Severity: MEDIUM

Affected dependencies:
  - Microsoft.IdentityModel.JsonWebTokens 5.6.0 at packages.config:51 (affected range < 5.7.0; first patched )
  - Microsoft.IdentityModel.JsonWebTokens 5.6.0 at packages.config:51 (affected range >= 6.5.0, < 6.34.0; first patched )
  - Microsoft.IdentityModel.JsonWebTokens 5.6.0 at packages.config:51 (affected range >= 7.0.0-preview, < 7.1.2; first patched )

Recommended fix: upgrade affected packages to the first patched version listed when available.

Additional affected dependencies:
  - System.IdentityModel.Tokens.Jwt 5.6.0 at packages.config:97 (affected range < 5.7.0; first patched )
  - System.IdentityModel.Tokens.Jwt 5.6.0 at packages.config:97 (affected range >= 6.5.0, < 6.34.0; first patched )
  - System.IdentityModel.Tokens.Jwt 5.6.0 at packages.config:97 (affected range >= 7.0.0-preview, < 7.1.2; first patched )

### CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters
- **Severity:** optional
- **Story Points:** 1
- **Files:** src/eShopModernizedMVC/packages.config:10

[CVE-2026-40021](https://github.com/advisories/GHSA-4f7c-pmjv-c25w): Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters

Severity: MEDIUM

Affected dependencies:
  - log4net 2.0.12 at packages.config:10 (affected range < 3.3.0; first patched )

Recommended fix: upgrade affected packages to the first patched version listed when available.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Models/CatalogItemHiLoGenerator.cs:19-20, src/eShopModernizedMVC/Models/Infrastructure/CatalogDBInitializer.cs:324-328, src/eShopModernizedMVC/Models/Infrastructure/dbo.catalog_hilo.Sequence.sql:4

CatalogItemHiLoGenerator.GetNextSequenceValue and CatalogDBInitializer.GetSequenceIdFromSelectedDBSequence read SQL Server bigint sequence values as Int64 and cast them directly to int. The sequence scripts declare these as bigint, so values beyond Int32 range can be truncated and produce invalid or duplicate integer IDs.

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Services/ImageAzureStorage.cs:109-117

ImageAzureStorage.UpLoadImageFromFile opens a FileStream at line 111 and passes it to UploadFromStream without disposing it, leaving the file handle open after the operation completes.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Services/ImageAzureStorage.cs:109-117

ImageAzureStorage.UpLoadImageFromFile opens a file handle with File.OpenRead at line 111 but does not release or dispose the stream after UploadFromStream returns.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/App_Start/Startup.Auth.cs:57-61, src/eShopModernizedMVC/Controllers/AccountController.cs:14-35

The OpenID Connect AuthenticationFailed handler handles the exception and redirects to an error page without recording the failed authentication event. AccountController.SignIn and SignOut log only that processing started, not the authentication outcome, leaving failed sign-in events absent from the application log.

