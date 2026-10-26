# Security Assessment Report

**Generated:** 2026-10-26T20:02:01.5552924Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 10 |
| CVE Vulnerabilities | 2 |
| CWE Vulnerabilities | 8 |
| Total Rules Assessed | 59 |
| Rules Passed | 51 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 0 |
| optional | 5 |
| potential | 5 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2024-21319: Microsoft ASP.NET Core project templates vulnerable to denial of service
- **Severity:** optional
- **Story Points:** 1
- **Files:** src/eShopModernizedWebForms/packages.config:53

[CVE-2024-21319](https://github.com/advisories/GHSA-59j7-ghrg-fj52): Microsoft ASP.NET Core project templates vulnerable to denial of service

Severity: MEDIUM

Affected dependencies:
  - Microsoft.IdentityModel.JsonWebTokens:5.6.0 (declared at packages.config:53, affected range < 5.7.0; patched )
  - Microsoft.IdentityModel.JsonWebTokens:5.6.0 (declared at packages.config:53, affected range >= 6.5.0, < 6.34.0; patched )
  - Microsoft.IdentityModel.JsonWebTokens:5.6.0 (declared at packages.config:53, affected range >= 7.0.0-preview, < 7.1.2; patched )

Recommended fix: upgrade to the first patched version listed above where available.

### CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters
- **Severity:** optional
- **Story Points:** 1
- **Files:** src/eShopModernizedWebForms/packages.config:11

[CVE-2026-40021](https://github.com/advisories/GHSA-4f7c-pmjv-c25w): Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters

Severity: MEDIUM

Affected dependencies:
  - log4net:2.0.10 (declared at packages.config:11, affected range < 3.3.0; patched )

Recommended fix: upgrade to the first patched version listed above where available.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Models/CatalogItemHiLoGenerator.cs:19-20, src/eShopModernizedWebForms/Models/Infrastructure/CatalogDBInitializer.cs:323-326, src/eShopModernizedWebForms/Models/Infrastructure/dbo.catalog_hilo.Sequence.sql:4-9

CatalogItemHiLoGenerator and CatalogDBInitializer convert SQL bigint sequence values to int without a range check. The catalog sequence is defined as bigint with a maximum of 9223372036854775807, so values beyond Int32 range wrap when used as catalog IDs.

### CWE-682: Incorrect Calculation
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedWebForms/Default.aspx.cs:31-35, src/eShopModernizedWebForms/Services/CatalogService.cs:24-30, src/eShopModernizedWebForms/ViewModel/PaginatedItemsViewModel.cs:18-24

The route-provided page size and index are converted and used without range checks. `pageSize * pageIndex` can overflow in the paging query, and a zero pageSize causes division by zero when TotalPages is calculated, disrupting requests.

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:107-115

UpLoadImageFromFile opens a FileStream with File.OpenRead and never disposes it after UploadFromStream completes. InitializeCatalogImages invokes this helper repeatedly, leaving file handles open.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:107-115

UpLoadImageFromFile opens a file handle using File.OpenRead but does not close or dispose the stream after uploading.

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** docker-compose.override.yml:21

The local SQL Server service supplies a fixed literal SA_PASSWORD in the committed Docker Compose override instead of resolving it from a secret source. The value is intentionally omitted here.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** docker-compose.override.yml:21

A SQL Server credential is embedded as a fixed literal in the committed Docker Compose override at the SA_PASSWORD setting. The credential value is intentionally omitted here.

### CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')
- **Category:** Injection Attacks
- **Severity:** optional
- **Story Points:** 8
- **Files:** src/eShopModernizedWebForms/Catalog/Details.aspx:16, src/eShopModernizedWebForms/Catalog/Details.aspx:23, src/eShopModernizedWebForms/Catalog/Edit.aspx.cs:69-70

Catalog item Name and Description are accepted from editable form fields in Edit.Save_Click (Edit.aspx.cs:69-70) and rendered through data-bound asp:Label Text properties without explicit HTML encoding in Details.aspx:16,23. This creates a stored cross-site scripting path.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedWebForms/Catalog/PicUploader.asmx.cs:40-41, src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:87-97

UploadImage accepts the request itemId and uploaded file name from the client (PicUploader.asmx.cs:40-41). ImageAzureStorage.UploadTempImage concatenates the parsed itemId and untrusted HttpPostedFile.FileName into the Azure blob identifier (ImageAzureStorage.cs:89,93), allowing the caller to select blob resource names outside the caller-specific scope.

