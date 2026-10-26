# Security Assessment Report

**Generated:** 2026-10-26T19:19:47.5367195Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 15 |
| CVE Vulnerabilities | 8 |
| CWE Vulnerabilities | 7 |
| Total Rules Assessed | 59 |
| Rules Passed | 52 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 4 |
| optional | 8 |
| potential | 3 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters
- **Severity:** optional
- **Story Points:** 1
- **Files:** src/eShopLegacyMVC/packages.config:10, eShopPorted/eShopPorted.csproj:13

[CVE-2026-40021](https://github.com/advisories/GHSA-4f7c-pmjv-c25w): Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters

Severity: MEDIUM

Affected dependencies:
  - log4net:2.0.10 (range < 3.3.0; patched: not specified)

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** src/eShopLegacyMVC/packages.config:29

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr): Improper Handling of Exceptional Conditions in Newtonsoft.Json

Severity: HIGH

Affected dependencies:
  - Newtonsoft.Json:12.0.1 (range < 13.0.1; patched: not specified)

### CVE-2025-55315: Microsoft Security Advisory CVE-2025-55315: .NET Security Feature Bypass Vulnerability
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** transitive dependency; no direct declaration

[CVE-2025-55315](https://github.com/advisories/GHSA-5rrx-jjjq-q2r5): Microsoft Security Advisory CVE-2025-55315: .NET Security Feature Bypass Vulnerability

Severity: CRITICAL

Affected dependencies:
  - Microsoft.AspNetCore.Server.Kestrel.Core:2.2.0 (range <= 2.3.0; patched: not specified)

### CVE-2022-41064: .NET Information Disclosure Vulnerability
- **Severity:** optional
- **Story Points:** 1
- **Files:** transitive dependency; no direct declaration

[CVE-2022-41064](https://github.com/advisories/GHSA-8g2p-5pqh-5jmc): .NET Information Disclosure Vulnerability

Severity: MEDIUM

Affected dependencies:
  - System.Data.SqlClient:4.6.1 (range <= 4.8.4; patched: not specified)

### CVE-2024-0056: Microsoft.Data.SqlClient and System.Data.SqlClient vulnerable to SQL Data Provider Security Feature Bypass 
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** transitive dependency; no direct declaration

[CVE-2024-0056](https://github.com/advisories/GHSA-98g6-xh36-x2p7): Microsoft.Data.SqlClient and System.Data.SqlClient vulnerable to SQL Data Provider Security Feature Bypass 

Severity: HIGH

Affected dependencies:
  - System.Data.SqlClient:4.6.1 (range < 4.8.6; patched: not specified)

### CVE-2021-26701: .NET Core Remote Code Execution Vulnerability
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** transitive dependency; no direct declaration

[CVE-2021-26701](https://github.com/advisories/GHSA-ghhp-997w-qr28): .NET Core Remote Code Execution Vulnerability

Severity: CRITICAL

Affected dependencies:
  - System.Text.Encodings.Web:4.5.0 (range >= 4.0.0, < 4.5.1; patched: not specified)

### CVE-2019-1075: Open redirect in ASP.NET Core
- **Severity:** optional
- **Story Points:** 1
- **Files:** transitive dependency; no direct declaration

[CVE-2019-1075](https://github.com/advisories/GHSA-prrf-397v-83xh): Open redirect in ASP.NET Core

Severity: MEDIUM

Affected dependencies:
  - Microsoft.AspNetCore.Server.IIS:2.2.0 (range >= 2.2.0, < 2.2.6; patched: not specified)

### CVE-2022-34716: .NET Information Disclosure Vulnerability
- **Severity:** optional
- **Story Points:** 1
- **Files:** transitive dependency; no direct declaration

[CVE-2022-34716](https://github.com/advisories/GHSA-vh55-786g-wjwj): .NET Information Disclosure Vulnerability

Severity: MEDIUM

Affected dependencies:
  - System.Security.Cryptography.Xml:4.5.0 (range <= 4.7.0; patched: not specified)

## CWE Findings (Code-Level Vulnerabilities)

### CWE-477: Use of Obsolete Function
- **Category:** Code Quality
- **Severity:** optional
- **Story Points:** 1
- **Files:** eShopLegacy.Utilities/Serializing.cs:11, eShopLegacy.Utilities/Serializing.cs:19

Serializing.SerializeBinary and DeserializeBinary instantiate BinaryFormatter. BinaryFormatter is obsolete in current .NET releases, and the helper remains used by the application for binary serialization.

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopLegacyMVC/Models/Infrastructure/CatalogDBInitializer.cs:328-329, src/eShopLegacyMVC/Models/CatalogItemHiLoGenerator.cs:22-23

Both catalog ID generators obtain an Int64 sequence value and cast it directly to Int32 without a range check. Values outside the Int32 range can be truncated/wrapped before use as catalog IDs.

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopLegacy.Utilities/Serializing.cs:8-14, eShopPorted/Controllers/Api/FilesController.cs:28-32

SerializeBinary allocates and returns a MemoryStream without disposing it; FilesController serializes catalog data and passes the stream to Ok without disposing it, leaving resource lifetime to an external formatter.

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 8
- **Files:** src/eShopLegacyMVC/Controllers/PicController.cs:39, src/eShopLegacyMVC/Controllers/CatalogController.cs:63, eShopPorted/Controllers/PicController.cs:40, eShopPorted/Controllers/CatalogController.cs:62

Both picture controllers combine a fixed image root with `CatalogItem.PictureFileName` and read the resulting path without verifying containment. The MVC create/edit actions bind `PictureFileName` from request data, allowing path traversal sequences to persist in the catalog item and be used by the picture endpoint.

### CWE-23: Relative Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Controllers/PicController.cs:39, src/eShopLegacyMVC/Controllers/CatalogController.cs:63, eShopPorted/Controllers/PicController.cs:40, eShopPorted/Controllers/CatalogController.cs:62

The request-bound `PictureFileName` is passed to `Path.Combine` with the image root and then used by `File.ReadAllBytes` without rejecting `..` segments or checking the canonical resolved path. Relative traversal can therefore read files outside the intended image directory.

### CWE-36: Absolute Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopLegacyMVC/Controllers/PicController.cs:39, src/eShopLegacyMVC/Controllers/CatalogController.cs:63, eShopPorted/Controllers/PicController.cs:40, eShopPorted/Controllers/CatalogController.cs:62

The picture controllers use the request-bound `PictureFileName` as the second `Path.Combine` argument. A rooted path can replace the intended root, and the resulting path is read with `File.ReadAllBytes` without an absolute-path containment check.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopLegacyMVC/Controllers/PicController.cs:39, src/eShopLegacyMVC/Controllers/CatalogController.cs:63, eShopPorted/Controllers/PicController.cs:40, eShopPorted/Controllers/CatalogController.cs:62

The picture endpoints use the catalog item's request-bindable PictureFileName as a file resource identifier and read that resource without limiting it to the image directory. The user-controlled identifier may resolve to an unintended file resource.
