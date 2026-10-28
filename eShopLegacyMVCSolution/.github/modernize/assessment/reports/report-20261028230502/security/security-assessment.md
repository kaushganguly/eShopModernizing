# Security Assessment Report

**Generated:** 2026-10-28T23:15:20.014049Z

**Scope:** eShopLegacyMVCSolution; aggregation of the completed dependency and six CWE assessments. No rescanning or runtime validation was performed during aggregation.

**Minimum CVE severity:** medium. CVE critical/high map to mandatory, medium to optional. CWE assessment severities are preserved; these labels are assessment priorities, not CVSS scores.

## Summary

| Metric | Count |
|--------|-------|
| Total findings (CVE exposures plus CWE classifications) | 11 |
| CVE dependency exposures | 7 |
| CWE FOUND classifications | 4 |
| Independent code-level root issues | 1 |
| Total CWE rules assessed | 59 |
| Rules passed (NOT_FOUND) | 55 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 3 |
| optional | 7 |
| potential | 1 |

### By Category

| Category | Count |
|----------|-------|
| CVE | 7 |
| File & Path Security | 3 |
| Injection Attacks | 1 |

## Coverage and limitations

- Dependency coverage comprises 216 declared/resolved package-version coordinates queried across eight successful paginated GitHub Advisory API batches. Eight distinct advisories were assessed; seven are retained. CVE-2024-21907 for Newtonsoft.Json 12.0.1 was excluded by the originating assessment because its impact is exclusively denial of service, not because of the medium threshold.
- Fresh dependency resolution failed because Microsoft.WebApplication.targets was unavailable. Existing eShopPorted assets and declarations supplied the package evidence; they do not prove deployed versions or constitute a fresh, complete dependency inventory. Conflicting declared/resolved Autofac versions were both queried.
- Complete fresh resolution, deployment versions, runtime exploitability, and vendored/CDN JavaScript advisory coverage remain unverified. Dependency-version exposure does not establish application exploitability. Individual findings preserve vulnerable-feature and deployment prerequisites; the configured log4net PatternLayout is not an affected XML layout.
- CWE results are static source review, not a penetration test. No exploit execution, build, stress test, race instrumentation, runtime profiling, secret-validity checks, or deployed identity/ACL inspection was performed. Framework/native dependency implementations and vendored JavaScript were not exhaustively audited. Execution durations were not measured.
- NOT_FOUND means no qualifying high-confidence exploitable security issue was established under the originating assessment threshold, not proof of safety or absence of the pattern. Availability-only and generic quality defects were excluded. The concurrency payload records an unsynchronized singleton defect without an established qualifying security exploit; unused BinaryFormatter deserialization remains a review caveat.

## Overlapping CWE classifications: one root issue

**CWE-22, CWE-23, CWE-36, and CWE-99 classify the same arbitrary-file-read vulnerability**, not four independent vulnerabilities. Client-controlled PictureFileName is persisted through catalog Create/Edit and passed to the picture filesystem read without path confinement. Relative and absolute path traversal and resource injection are overlapping classifications of this one flow. Both the merged JSON and report.json deliberately retain all four classifications and their evidence; the independent code-level root-issue count is one. Story points must not be summed across these four entries as separate remediation work.

## CVE Findings (Dependency Vulnerabilities)

### CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters
- **Severity:** optional (advisory: medium)
- **Story Points:** 1
- **Files:** eShopLegacyMVCSolution/src/eShopLegacyMVC/eShopLegacyMVC.csproj:242, eShopLegacyMVCSolution/eShopPorted/eShopPorted.csproj:13

[CVE-2026-40021](https://github.com/advisories/GHSA-4f7c-pmjv-c25w). Severity: MEDIUM. Both projects declare log4net 2.0.10, matching affected range <3.3.0. Attacker-influenced forbidden XML characters can suppress log events when affected XML layouts are used. The checked-in legacy configuration uses PatternLayout, not an affected XML layout; application exploitability is not established. Upgrade to 3.3.0 or later. This is a dependency-version exposure, not a confirmed application vulnerability.

### CVE-2025-55315: Microsoft Security Advisory CVE-2025-55315: .NET Security Feature Bypass Vulnerability
- **Severity:** mandatory (advisory: critical)
- **Story Points:** 1
- **Files:** eShopLegacyMVCSolution/eShopPorted/eShopPorted.csproj:7

[CVE-2025-55315](https://github.com/advisories/GHSA-5rrx-jjjq-q2r5). Severity: CRITICAL. Existing eShopPorted assets resolve Microsoft.AspNetCore.Server.Kestrel.Core 2.2.0 through Microsoft.AspNetCore 2.2.0 -> Microsoft.AspNetCore.Server.Kestrel -> Microsoft.AspNetCore.Server.Kestrel.Core. GitHub lists <=2.3.0 as affected by HTTP request smuggling/security-feature bypass and 2.3.6 as patched. Deployment and request-processing prerequisites were not verified. Upgrade to a supported patched framework dependency set; compatibility of an isolated transitive override is unverified. Fresh dependency resolution failed because Microsoft.WebApplication.targets was unavailable; existing assets are not proof of deployed versions.

### CVE-2022-41064: .NET Information Disclosure Vulnerability
- **Severity:** optional (advisory: medium)
- **Story Points:** 1
- **Files:** eShopLegacyMVCSolution/eShopPorted/eShopPorted.csproj:22

[CVE-2022-41064](https://github.com/advisories/GHSA-8g2p-5pqh-5jmc). Severity: MEDIUM. Existing eShopPorted assets resolve System.Data.SqlClient 4.6.1 through Microsoft.EntityFrameworkCore.SqlServer 2.2.6. GitHub lists <=4.8.4 as affected and 4.8.5 as patched. Timeouts under high load during asynchronous queries can cause incorrect results and information disclosure. Application triggering conditions were not demonstrated. Upgrade to at least 4.8.6 to also address CVE-2024-0056, subject to compatibility. Fresh resolution failed; existing assets are not proof of deployed versions.

### CVE-2024-0056: Microsoft.Data.SqlClient and System.Data.SqlClient vulnerable to SQL Data Provider Security Feature Bypass
- **Severity:** mandatory (advisory: high)
- **Story Points:** 1
- **Files:** eShopLegacyMVCSolution/eShopPorted/eShopPorted.csproj:22

[CVE-2024-0056](https://github.com/advisories/GHSA-98g6-xh36-x2p7). Severity: HIGH. Existing eShopPorted assets resolve System.Data.SqlClient 4.6.1 through Microsoft.EntityFrameworkCore.SqlServer 2.2.6, matching affected range <4.8.6. The advisory identifies SQL data-provider security-feature bypass. Connection-security and network-attack prerequisites were not demonstrated. Upgrade to 4.8.6 or a compatible supported successor. Fresh resolution failed; existing assets are not proof of deployed versions.

### CVE-2021-26701: .NET Core Remote Code Execution Vulnerability
- **Severity:** mandatory (advisory: critical)
- **Story Points:** 1
- **Files:** eShopLegacyMVCSolution/eShopPorted/eShopPorted.csproj:17

[CVE-2021-26701](https://github.com/advisories/GHSA-ghhp-997w-qr28). Severity: CRITICAL. Existing eShopPorted assets resolve System.Text.Encodings.Web 4.5.0 through Microsoft.AspNetCore.StaticFiles 2.2.0 -> Microsoft.Extensions.WebEncoders -> System.Text.Encodings.Web, matching affected range >=4.0.0,<4.5.1. The advisory explicitly includes .NET Framework applications using affected package versions. Vulnerable text encoding can cause remote code execution; no application exploit was executed. Upgrade to at least 4.5.1 on this release line or a compatible supported successor. Fresh resolution failed; existing assets are not proof of deployed versions.

### CVE-2019-1075: Open redirect in ASP.NET Core
- **Severity:** optional (advisory: medium)
- **Story Points:** 1
- **Files:** eShopLegacyMVCSolution/eShopPorted/eShopPorted.csproj:7

[CVE-2019-1075](https://github.com/advisories/GHSA-prrf-397v-83xh). Severity: MEDIUM. Existing eShopPorted assets resolve Microsoft.AspNetCore.Server.IIS 2.2.0 through Microsoft.AspNetCore 2.2.0, matching affected range >=2.2.0,<2.2.6. The advisory identifies open redirect/spoofing; IIS deployment and application exploitability were not verified. Version 2.2.6 is patched for this advisory; migrate to a supported patched framework dependency set. Fresh resolution failed; existing assets are not proof of deployed versions.

### CVE-2022-34716: .NET Information Disclosure Vulnerability
- **Severity:** optional (advisory: medium)
- **Story Points:** 1
- **Files:** eShopLegacyMVCSolution/eShopPorted/eShopPorted.csproj:16

[CVE-2022-34716](https://github.com/advisories/GHSA-vh55-786g-wjwj). Severity: MEDIUM. Existing eShopPorted assets resolve System.Security.Cryptography.Xml 4.5.0 through Microsoft.AspNetCore.Mvc -> Microsoft.AspNetCore.Mvc.ViewFeatures -> Microsoft.AspNetCore.Antiforgery -> Microsoft.AspNetCore.DataProtection -> System.Security.Cryptography.Xml. GitHub lists <=4.7.0 as affected and 4.7.1 as patched. Vulnerable-feature usage and an application information-disclosure path were not established. Upgrade to at least 4.7.1 on that release line or a compatible supported successor. Fresh resolution failed; existing assets are not proof of deployed versions. Overall coverage: 216 declared/resolved package-version coordinates queried across eight successful paginated GitHub Advisory API batches. Eight distinct advisories were assessed; CVE-2024-21907 affects Newtonsoft.Json 12.0.1 but is excluded because its impact is exclusively denial of service. Conflicting declared/resolved Autofac versions were both queried. Complete fresh resolution, deployment versions, runtime exploitability, and vendored/CDN JavaScript advisory coverage remain unverified.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 8
- **Files:** eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/CatalogController.cs:62, eShopLegacyMVCSolution/src/eShopLegacyMVC/Models/CatalogItem.cs:28, eShopLegacyMVCSolution/src/eShopLegacyMVC/Services/CatalogService.cs:51, eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/PicController.cs:38

The product uses external input to construct a pathname that is intended to identify a file or directory that is located underneath a restricted parent directory, but the product does not properly neutralize special elements within the pathname that can cause the pathname to resolve to a location that is outside of the restricted directory.

CatalogController.Create/Edit bind attacker-supplied PictureFileName and persist it without confinement validation. PicController.Index combines the stored name with ~/Pics at lines 38-39, reads all bytes at line 44, and returns them at line 46. A client can submit a valid catalog entry with ../Web.config, then request its picture to retrieve application configuration readable by the worker identity. CatalogServiceMock likewise stores submitted objects. No application authorization filter protects catalog actions; antiforgery does not restrict a direct client with its own token. No extension allowlist guards the read; unknown extensions are returned as application/octet-stream. Confidence 9/10; impact HIGH arbitrary file disclosure.

### CWE-23: Relative Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/CatalogController.cs:62, eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/PicController.cs:39

The product uses external input to construct a pathname that should be within a restricted directory, but it does not properly neutralize sequences such as .. that can resolve to a location that is outside of that directory.

Same arbitrary-file-read issue as CWE-22: PictureFileName is writable through catalog Create/Edit, and PicController.Index passes a relative traversal such as ../Web.config through Path.Combine directly into File.ReadAllBytes at line 44. There is no canonical-path containment check or filename restriction. Access is limited only by the application process file permissions. Confidence 9/10; impact HIGH.

### CWE-36: Absolute Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/CatalogController.cs:100, eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/PicController.cs:39

The product uses external input to construct a pathname that should be within a restricted directory, but it does not properly neutralize absolute path sequences such as /abs/path that can resolve to a location that is outside of that directory.

Same arbitrary-file-read issue as CWE-22: catalog Create/Edit accept absolute PictureFileName values, including Windows drive-rooted paths. Path.Combine(webRoot, item.PictureFileName) discards webRoot when its second argument is rooted; File.ReadAllBytes and File return the targeted file. No rooted-path rejection or containment check exists. An attacker can retrieve any known file readable by the worker identity. Confidence 9/10; impact HIGH.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/CatalogController.cs:62, eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/CatalogController.cs:100, eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/PicController.cs:39

The product receives input from an upstream component, but it does not restrict or incorrectly restricts the input before it is used as an identifier for a resource that may be outside the intended sphere of control.

CatalogController.Create/Edit accept and persist client-controlled PictureFileName. PicController.Index uses it as a filesystem resource identifier without confinement checks, reads the selected file at line 44, and returns it at line 46. A submitted value such as ../Web.config selects a resource outside Pics. Model validation does not restrict filenames, and client-side readonly UI is not a server-side guard. This is the same arbitrary-file-read issue as CWE-22/23/36, not an additional independent vulnerability. Confidence 9/10; impact HIGH.

## CWE assessment coverage

| Assessment | Rules | FOUND | NOT_FOUND |
|------------|-------|-------|-----------|
| CWE - File & Path Security | 5 | 3 | 2 |
| CWE - Injection Attacks | 12 | 1 | 11 |
| CWE - Memory Safety | 15 | 0 | 15 |
| CWE - Code Quality | 16 | 0 | 16 |
| CWE - Credentials & Secrets | 5 | 0 | 5 |
| CWE - Concurrency & Synchronization | 6 | 0 | 6 |

## Original assessment caveats

The seven raw JSON files are retained alongside this report with all rule results, advisory evidence, category-level review notes, and timestamps unchanged in content.

### CWE - File & Path Security

- Static assessment restricted to eShopLegacyMVCSolution; reviewed both controller implementations, catalog binding/model validation, service persistence, routes, filters, initialization file access, and XML/upload sinks.
- No running deployment or exploit execution. Findings describe one arbitrary-file-read vulnerability under three related CWE classifications, not three independent vulnerabilities.
- PictureFileName has no filename/confinement validation. Antiforgery protects catalog POSTs from CSRF, not malicious direct clients; a client can obtain its own token. Initializer paths are application-controlled local inputs. No application XML parser or upload entry point was identified.
- Execution duration was not measured. Timestamp records overall assessment completion.

### CWE - Injection Attacks

- Static assessment of scoped controllers, services, initialization SQL, serialization utility, Razor templates, and client-script sink searches. No runtime exploit testing.
- Entity Framework LINQ queries parameterize values. Initializer interpolated sequence identifiers and SQL script paths originate from application constants, not web input. Razor DisplayFor/EditorFor and ordinary Razor expressions encode values; the explicit HtmlString contains server-generated machine/time session values.
- BinaryFormatter.Deserialize exists in Serializing.DeserializeBinary, but no caller was found in the scoped application; exposed Files controllers serialize output only. No untrusted-input deserialization path was established, so CWE-502 is NOT_FOUND under the high-confidence exploitability threshold, not a claim that the utility is safe for untrusted input.
- Vendored third-party JavaScript internals were searched for sinks but not exhaustively audited. No attacker-controlled application call path into such a sink was confirmed.
- CWE-99 is the same arbitrary-file-read vulnerability already classified in the file/path report.
- Execution duration was not measured. Timestamp records overall assessment completion.

### CWE - Memory Safety

- Reviewed managed C# buffers, indexing, stream serialization, controller/service disposal, and searches for unsafe blocks, stackalloc, fixed pointers, IntPtr, Marshal, native imports, and manual allocation/free. No application unmanaged-memory operations were identified.
- Managed array/list bounds errors are not evidence of arbitrary-memory access. No exploitable use-after-disposal path was established; database services are registered per lifetime scope.
- Static source assessment only. Runtime, CLR/framework internals, and native implementations of dependencies were not audited or fuzzed; dependency advisories are reported separately.
- Execution duration was not measured. Timestamp records overall assessment completion.

### CWE - Code Quality

- Reviewed initialization, pagination arithmetic, numeric conversions, sequence generation, loops, database access ownership, stream handling, and disposal in both application implementations and the utility project.
- Only high-confidence exploitable security vulnerabilities count as FOUND. Pagination division/overflow, excessive allocation, resource exhaustion, and generic maintainability concerns are outside the reporting threshold. No security-sensitive numeric decision or access-control bypass was identified.
- BinaryFormatter is present, but its deserialization method has no scoped caller. Obsolete API presence alone does not establish an exploitable vulnerability. Framework/compiler migration compatibility was not assessed.
- Static source review only; no build, stress test, runtime resource profiling, or third-party implementation audit.
- Execution duration was not measured. Timestamp records overall assessment completion.

### CWE - Credentials & Secrets

- Reviewed scoped source/configuration indicators for passwords, credentials, API keys, machine keys, cryptography, ACL APIs, authentication/authorization configuration, and logging. Indicator output suppressed credential values.
- Checked-in connection configurations did not contain password assignments; the legacy application uses integrated security. No hard-coded cryptographic key or explicit unsafe critical-resource ACL assignment was established.
- Catalog MVC actions log operations. Missing audit detail alone was not classified as an exploitable security vulnerability. The configured legacy log4net layout is PatternLayout, not the XML layouts implicated by CVE-2026-40021.
- No secret validity checks, deployed identity/ACL inspection, external secret-store inspection, or exhaustive high-entropy secret detection. Source review cannot establish deployed permissions. Generic missing authentication is not relabeled as an explicit permission-assignment defect under CWE-732.
- Execution duration was not measured. Timestamp records overall assessment completion.

### CWE - Concurrency & Synchronization

- Reviewed singleton/lifetime registrations, shared mutable state, list operations, sequence allocation, and synchronization primitives in both application implementations.
- Memory verified: eShopLegacyMVCSolution/src/eShopLegacyMVC/Modules/ApplicationModule.cs:18-22 registers CatalogServiceMock as SingleInstance; Services/CatalogServiceMock.cs:12-16 initializes a shared List and lines 19-66 read/update/add/remove without synchronization. The eShopPorted counterparts contain the same pattern. Legacy checked-in UseMockData is false; eShopPorted UseMockData is true.
- The unsynchronized singleton is a confirmed concurrency defect, but no additional unauthorized access, privilege-boundary crossing, sensitive-data disclosure, or other qualifying security exploit was established. Duplicate IDs, lost updates, and collection exceptions alone do not meet this security-only assessment threshold; availability-only impact is excluded. Thus NOT_FOUND means no qualifying exploitable security vulnerability, not absence of the concurrency pattern.
- CatalogItemHiLoGenerator.GetNextSequenceValue protects its mutable sequence state with lock(sequenceLock). Database catalog services are registered per lifetime scope.
- Static review only; no concurrent-request execution, race instrumentation, deployment topology inspection, or database transaction isolation testing.
- Execution duration was not measured. Timestamp records overall assessment completion.
