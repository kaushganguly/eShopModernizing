# Security Assessment Report

**Generated:** 2026-10-26T23:25:09.173290+00:00

**Scope:** eShopModernizedNTier; minimum CVE severity: medium.

## Coverage and limitations

- Static assessment of the full existing eShopModernizedNTier application; no application source was modified and no runtime exploitation, load testing or deployment verification was performed.
- The dependency CLI failed because Microsoft.WebApplication.targets was unavailable. Fallback inspected scoped package manifests, project references and existing project.assets.json; 114 distinct dependency/version coordinates were queried in four successful GitHub advisory API batches. Assets may be stale and a freshly restored complete service transitive dependency graph was unavailable.
- CVE findings identify affected versions recorded in existing resolved assets, not verified deployed packages or demonstrated exploitability. Minimum CVE severity: medium. Transitive packages have no direct declaration location attributed.
- CWE source review excluded generated code, obj/bin, .vs, temp and vendor artifacts. NOT_FOUND means no confirmed pattern in the reviewed first-party sources, not proof of absence.
- CWE classifications can overlap: path traversal/resource injection, hard-coded password/credential, image resource/handle leaks and stock synchronization findings may describe the same underlying issue. Counts are rule matches, not independent exploits.

## Summary

| Metric | Count |
|---|---:|
| TotalFindings | 17 |
| CveCount | 7 |
| CweCount | 10 |
| TotalRulesAssessed | 59 |
| RulesPassed | 49 |

### By severity

| Severity | Count |
|---|---:|
| mandatory | 5 |
| optional | 6 |
| potential | 6 |

### By category

| Category | Count |
|---|---:|
| CVE | 7 |
| Code Quality | 2 |
| Concurrency & Synchronization | 2 |
| Credentials & Secrets | 3 |
| File & Path Security | 2 |
| Injection Attacks | 1 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2024-0056: Microsoft.Data.SqlClient and System.Data.SqlClient vulnerable to SQL Data Provider Security Feature Bypass

- **Category:** CVE
- **Severity:** mandatory
- **Story points:** 1
- **Files:** Transitive dependency; resolved-asset provenance is listed below.

[CVE-2024-0056](https://github.com/advisories/GHSA-98g6-xh36-x2p7): Microsoft.Data.SqlClient and System.Data.SqlClient vulnerable to SQL Data Provider Security Feature Bypass 

Severity: high

Affected dependencies:
- System.Data.SqlClient@4.8.1 [transitive/resolved assets] (src\eShopWinForms\obj\project.assets.json); affected < 4.8.6; patched: 4.8.6

Recommended fix: upgrade affected packages to listed patched versions or newer compatible releases. Confidence: high for declared/resolved version match; exploitation and deployment not verified. Resolved assets can be stale. CLI failed because Microsoft.WebApplication.targets is missing; packages.config, all project references and available project.assets.json were used. No complete newly restored service transitive graph is available. Transitive entries have no direct declaration evidence attributed.

### CVE-2018-8292: .NET Core Information Disclosure

- **Category:** CVE
- **Severity:** mandatory
- **Story points:** 1
- **Files:** Transitive dependency; resolved-asset provenance is listed below.

[CVE-2018-8292](https://github.com/advisories/GHSA-7jgj-8wvc-jh57): .NET Core Information Disclosure

Severity: high

Affected dependencies:
- System.Net.Http@4.3.0 [transitive/resolved assets] (src\eShopWinForms\obj\project.assets.json); affected < 4.3.4; patched: 4.3.4

Recommended fix: upgrade affected packages to listed patched versions or newer compatible releases. Confidence: high for declared/resolved version match; exploitation and deployment not verified. Resolved assets can be stale. CLI failed because Microsoft.WebApplication.targets is missing; packages.config, all project references and available project.assets.json were used. No complete newly restored service transitive graph is available. Transitive entries have no direct declaration evidence attributed.

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json

- **Category:** CVE
- **Severity:** mandatory
- **Story points:** 1
- **Files:** Transitive dependency; resolved-asset provenance is listed below.

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr): Improper Handling of Exceptional Conditions in Newtonsoft.Json

Severity: high

Affected dependencies:
- Newtonsoft.Json@10.0.1 [transitive/resolved assets] (src\eShopWinForms\obj\project.assets.json); affected < 13.0.1; patched: 13.0.1

Recommended fix: upgrade affected packages to listed patched versions or newer compatible releases. Confidence: high for declared/resolved version match; exploitation and deployment not verified. Resolved assets can be stale. CLI failed because Microsoft.WebApplication.targets is missing; packages.config, all project references and available project.assets.json were used. No complete newly restored service transitive graph is available. Transitive entries have no direct declaration evidence attributed.

### CVE-2022-41064: .NET Information Disclosure Vulnerability

- **Category:** CVE
- **Severity:** optional
- **Story points:** 1
- **Files:** Transitive dependency; resolved-asset provenance is listed below.

[CVE-2022-41064](https://github.com/advisories/GHSA-8g2p-5pqh-5jmc): .NET Information Disclosure Vulnerability

Severity: medium

Affected dependencies:
- System.Data.SqlClient@4.8.1 [transitive/resolved assets] (src\eShopWinForms\obj\project.assets.json); affected <= 4.8.4; patched: 4.8.5

Recommended fix: upgrade affected packages to listed patched versions or newer compatible releases. Confidence: high for declared/resolved version match; exploitation and deployment not verified. Resolved assets can be stale. CLI failed because Microsoft.WebApplication.targets is missing; packages.config, all project references and available project.assets.json were used. No complete newly restored service transitive graph is available. Transitive entries have no direct declaration evidence attributed.

### CVE-2022-34716: .NET Information Disclosure Vulnerability

- **Category:** CVE
- **Severity:** optional
- **Story points:** 1
- **Files:** Transitive dependency; resolved-asset provenance is listed below.

[CVE-2022-34716](https://github.com/advisories/GHSA-vh55-786g-wjwj): .NET Information Disclosure Vulnerability

Severity: medium

Affected dependencies:
- System.Security.Cryptography.Xml@4.7.0 [transitive/resolved assets] (src\eShopWinForms\obj\project.assets.json); affected <= 4.7.0; patched: 4.7.1

Recommended fix: upgrade affected packages to listed patched versions or newer compatible releases. Confidence: high for declared/resolved version match; exploitation and deployment not verified. Resolved assets can be stale. CLI failed because Microsoft.WebApplication.targets is missing; packages.config, all project references and available project.assets.json were used. No complete newly restored service transitive graph is available. Transitive entries have no direct declaration evidence attributed.

### CVE-2019-0820: Regular Expression Denial of Service in System.Text.RegularExpressions

- **Category:** CVE
- **Severity:** mandatory
- **Story points:** 1
- **Files:** Transitive dependency; resolved-asset provenance is listed below.

[CVE-2019-0820](https://github.com/advisories/GHSA-cmhx-cq75-c4mj): Regular Expression Denial of Service in System.Text.RegularExpressions

Severity: high

Affected dependencies:
- System.Text.RegularExpressions@4.3.0 [transitive/resolved assets] (src\eShopWinForms\obj\project.assets.json); affected >= 4.3.0, < 4.3.1; patched: 4.3.1

Recommended fix: upgrade affected packages to listed patched versions or newer compatible releases. Confidence: high for declared/resolved version match; exploitation and deployment not verified. Resolved assets can be stale. CLI failed because Microsoft.WebApplication.targets is missing; packages.config, all project references and available project.assets.json were used. No complete newly restored service transitive graph is available. Transitive entries have no direct declaration evidence attributed.

### CVE-2021-24112: .NET Core Remote Code Execution Vulnerability

- **Category:** CVE
- **Severity:** mandatory
- **Story points:** 1
- **Files:** Transitive dependency; resolved-asset provenance is listed below.

[CVE-2021-24112](https://github.com/advisories/GHSA-rxg9-xrhp-64gj): .NET Core Remote Code Execution Vulnerability

Severity: critical

Affected dependencies:
- System.Drawing.Common@4.7.0 [transitive/resolved assets] (src\eShopWinForms\obj\project.assets.json); affected >= 4.0.0, < 4.7.2; patched: 4.7.2

Recommended fix: upgrade affected packages to listed patched versions or newer compatible releases. Confidence: high for declared/resolved version match; exploitation and deployment not verified. Resolved assets can be stale. CLI failed because Microsoft.WebApplication.targets is missing; packages.config, all project references and available project.assets.json were used. No complete newly restored service transitive graph is available. Transitive entries have no direct declaration evidence attributed.


## CWE Findings (Code-Level Vulnerabilities)

### CWE-772: Missing Release of Resource after Effective Lifetime

- **Category:** Code Quality
- **Severity:** potential
- **Story points:** 3
- **Files:** src\eShopWinForms\Views\CatalogView.cs:39-42; src\eShopWinForms\Views\CatalogView.cs:48-62

Confidence: high for missing deterministic release; medium for denial-of-service impact. CatalogView.SetCatalogItems creates an Image with Image.FromFile at line 58 and derives a thumbnail at line 59 but never disposes the original image, whose effective lifetime ends after thumbnail creation. System.Drawing keeps the source file locked until Image.Dispose or eventual finalization. ClearGrid removes rows at line 41 without explicit thumbnail disposal. Repeated filter changes reload images and can accumulate native image/file resources and prolonged file locks; no load test measured exhaustion and GC/finalization may eventually release them. This is a desktop resource-lifetime issue, not demonstrated remote code execution.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime

- **Category:** Code Quality
- **Severity:** potential
- **Story points:** 3
- **Files:** src\eShopWinForms\Views\CatalogView.cs:39-42; src\eShopWinForms\Views\CatalogView.cs:48-62

Confidence: high for missing deterministic release; medium for denial-of-service impact. CatalogView.SetCatalogItems creates an Image with Image.FromFile at line 58 and derives a thumbnail at line 59 but never disposes the original image, whose effective lifetime ends after thumbnail creation. System.Drawing keeps the source file locked until Image.Dispose or eventual finalization. ClearGrid removes rows at line 41 without explicit thumbnail disposal. Repeated filter changes reload images and can accumulate native image/file resources and prolonged file locks; no load test measured exhaustion and GC/finalization may eventually release them. This is a desktop resource-lifetime issue, not demonstrated remote code execution.

### CWE-662: Improper Synchronization

- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story points:** 8
- **Files:** src\eShopWCFService\CatalogService.svc.cs:65-70; src\eShopWCFService\CatalogService.svc.cs:99-117; src\eShopWCFService\Models\CatalogItem.cs:13-15; src\eShopWCFService\Models\CatalogItemsStock.cs:22-25

Confidence: high for non-atomic ID allocation; medium for deployed concurrent exploitability. CatalogService.CreateCatalogItem reads the shared database MAX(Id), increments it in process and later inserts. CreateAvailableStock similarly performs an existence check and MAX(StockId)+1 before SaveChanges. IDs explicitly use DatabaseGeneratedOption.None. Concurrent requests handled by separate WCF instances can both read the same maximum before either commits; the second insert then fails on the duplicate key, causing request/inventory-update failure. SaveChanges transactions do not encompass the earlier MAX read; there is no explicit encompassing serializable transaction or cross-request synchronization. WCF default per-session/per-call handling does not globally serialize separate clients. No stress test or deployment concurrency observation was performed. Database uniqueness may prevent corruption but does not prevent the failing request. CWE-662 and CWE-820 describe this same root cause, not independent exploits.

### CWE-820: Missing Synchronization

- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story points:** 8
- **Files:** src\eShopWCFService\CatalogService.svc.cs:65-70; src\eShopWCFService\CatalogService.svc.cs:99-117; src\eShopWCFService\Models\CatalogItem.cs:13-15; src\eShopWCFService\Models\CatalogItemsStock.cs:22-25

Confidence: high for non-atomic ID allocation; medium for deployed concurrent exploitability. CatalogService.CreateCatalogItem reads the shared database MAX(Id), increments it in process and later inserts. CreateAvailableStock similarly performs an existence check and MAX(StockId)+1 before SaveChanges. IDs explicitly use DatabaseGeneratedOption.None. Concurrent requests handled by separate WCF instances can both read the same maximum before either commits; the second insert then fails on the duplicate key, causing request/inventory-update failure. SaveChanges transactions do not encompass the earlier MAX read; there is no explicit encompassing serializable transaction or cross-request synchronization. WCF default per-session/per-call handling does not globally serialize separate clients. No stress test or deployment concurrency observation was performed. Database uniqueness may prevent corruption but does not prevent the failing request. CWE-662 and CWE-820 describe this same root cause, not independent exploits.

### CWE-259: Use of Hard-coded Password

- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story points:** 5
- **Files:** docker-compose.override.yml:7; docker-compose.override.yml:13; docker-compose.override.yml:18

Confidence: high that reusable credentials are hard-coded; deployment use not verified. Docker Compose supplies a literal SQL administrator password through SA_PASSWORD, embeds the same authentication secret in the application ConnectionString, and provides a literal password argument to the sqlcmd healthcheck. All password values are [REDACTED]. CatalogConfiguration.ConnectionString (src\\eShopWCFService\\Models\\Infrastructure\\CatalogConfiguration.cs:16-17) reads the environment and EntityModel consumes it. Anyone with access to the tracked configuration obtains the configured database credential if deployed unchanged; host port mapping also exposes SQL subject to host firewall/network policy. This is repository configuration evidence, not proof the credential is currently active.

### CWE-778: Insufficient Logging

- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story points:** 3
- **Files:** src\eShopWCFService\CatalogService.svc.cs:65-83; src\eShopWCFService\CatalogService.svc.cs:99-117; src\eShopWCFService\ICatalogService.cs:25-32

Confidence: high for absence of application-level mutation audit events; medium for deployed audit coverage. CatalogService.CreateCatalogItem, UpdateCatalogItem, RemoveCatalogItem and CreateAvailableStock mutate catalog/inventory state and call SaveChanges without recording actor, operation, target or outcome. Full first-party source/config search found no audit logger or event source. Remote catalog/inventory changes therefore have no application-supplied security audit trail. IIS request logs, SQL audit or other deployment-level monitoring might exist outside this repository and were not inspected; this finding does not assert their absence.

### CWE-798: Use of Hard-coded Credentials

- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story points:** 5
- **Files:** docker-compose.override.yml:7; docker-compose.override.yml:13; docker-compose.override.yml:18

Confidence: high that reusable credentials are hard-coded; deployment use not verified. Docker Compose supplies a literal SQL administrator password through SA_PASSWORD, embeds the same authentication secret in the application ConnectionString, and provides a literal password argument to the sqlcmd healthcheck. All password values are [REDACTED]. CatalogConfiguration.ConnectionString (src\\eShopWCFService\\Models\\Infrastructure\\CatalogConfiguration.cs:16-17) reads the environment and EntityModel consumes it. Anyone with access to the tracked configuration obtains the configured database credential if deployed unchanged; host port mapping also exposes SQL subject to host firewall/network policy. This is repository configuration evidence, not proof the credential is currently active.

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')

- **Category:** File & Path Security
- **Severity:** optional
- **Story points:** 8
- **Files:** src\eShopWinForms\Views\CatalogView.cs:48-61; src\eShopWCFService\CatalogService.svc.cs:65-76; src\eShopWCFService\Models\CatalogItem.cs:24-25; src\eShopWCFService\ICatalogService.cs:27-30

Confidence: high for path escape, medium for practical impact. CatalogView.SetCatalogItems concatenates service-controlled Picturefilename into the catalog image directory at line 57 and calls Image.FromFile at line 58 without canonicalization or containment validation. CatalogService.CreateCatalogItem/UpdateCatalogItem persist the DataMember unchanged, and the WCF contract exposes these operations. A caller able to modify catalog data can supply ..\\ segments to open a local image outside the intended catalog directory under the desktop user identity, or crash rendering for invalid paths. Only image decoding/display is demonstrated, not arbitrary file exfiltration or remote code execution. Endpoint reachability and deployed authorization were not runtime-tested.

### CWE-23: Relative Path Traversal

- **Category:** File & Path Security
- **Severity:** optional
- **Story points:** 5
- **Files:** src\eShopWinForms\Views\CatalogView.cs:48-61; src\eShopWCFService\CatalogService.svc.cs:65-76; src\eShopWCFService\Models\CatalogItem.cs:24-25; src\eShopWCFService\ICatalogService.cs:27-30

Confidence: high for path escape, medium for practical impact. CatalogView.SetCatalogItems concatenates service-controlled Picturefilename into the catalog image directory at line 57 and calls Image.FromFile at line 58 without canonicalization or containment validation. CatalogService.CreateCatalogItem/UpdateCatalogItem persist the DataMember unchanged, and the WCF contract exposes these operations. A caller able to modify catalog data can supply ..\\ segments to open a local image outside the intended catalog directory under the desktop user identity, or crash rendering for invalid paths. Only image decoding/display is demonstrated, not arbitrary file exfiltration or remote code execution. Endpoint reachability and deployed authorization were not runtime-tested.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')

- **Category:** Injection Attacks
- **Severity:** potential
- **Story points:** 3
- **Files:** src\eShopWinForms\Views\CatalogView.cs:55-59; src\eShopWCFService\CatalogService.svc.cs:65-76

Confidence: high for uncontrolled image resource identifier; impact conditional on catalog write access and image readability. CatalogView.SetCatalogItems appends the WCF/DB-provided Picturefilename to a trusted asset directory and opens it with Image.FromFile. CreateCatalogItem and UpdateCatalogItem accept that member without restricting it. A modified catalog record with relative traversal selects a local image outside the intended directory or causes a rendering exception. This overlaps the path-traversal finding; it is not an independent arbitrary-code-execution finding. No runtime exploit performed.

