# Security Assessment Report

**Generated:** 2026-10-27T01:43:11.696115Z

**Scope:** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier`

**Evidence path base:** `D:\a\eShopModernizing\eShopModernizing\eShopModernizedNTier`. Finding file lists are relative to this workspace; explanatory prose may retain the `eShopModernizedNTier` prefix.

**Minimum CVE severity:** medium (unknown severity excluded).

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 11 |
| CVE Vulnerabilities | 0 |
| CWE Vulnerabilities | 11 |
| Total Rules Assessed | 59 |
| Rules Passed | 48 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 0 |
| optional | 4 |
| potential | 7 |

### By Category

| Category | Count |
|----------|-------|
| Code Quality | 2 |
| Concurrency & Synchronization | 3 |
| Credentials & Secrets | 3 |
| File & Path Security | 2 |
| Injection Attacks | 1 |

## Coverage and Limitations

Dependency scan status: **PARTIAL_DEPENDENCY_COVERAGE**. GitHub advisories were checked for 8 enumerated package/version pairs; no matching advisories were reported. This is not a clean, complete dependency scan.

**Transitive dependency coverage is unknown and incomplete.** The dependency-discovery command failed because `Microsoft.WebApplication.targets` was unavailable for the legacy WCF project. No restored dependency graph, `project.assets.json`, or `packages.lock.json` was available; zero resolved transitive packages does not mean zero transitive dependencies. Only declared package versions and legacy `packages.config` entries were checked.

- The solution-wide CLI failed; no restored dependency graph was obtained. Direct package declarations and legacy packages.config entries only were assessed.
- No project.assets.json or packages.lock.json was found in the scoped workspace. Transitive dependency coverage is unknown and incomplete, not zero dependencies.
- Declared versions were checked, not verified installed or deployed versions. Legacy packages.config entries do not reliably distinguish direct from transitive dependencies.
- Framework/runtime assemblies and operating-system components are not NuGet package findings and were not assessed.
- An empty findings array means no GitHub advisories matched the eight enumerated package/version pairs; it does not establish a clean complete dependency scan.

Raw CVE/CWE results and `cve-scan-coverage.json` are retained alongside this report. CWE results are static assessment findings, not runtime exploit verification; related CWE rules can describe the same underlying issue, so counts represent rule findings rather than deduplicated root causes.

## CVE Findings (Dependency Vulnerabilities)

No findings meeting the minimum severity threshold were reported for the assessed dependencies; coverage limitations above apply.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** `e`, `S`, `h`, `o`, `p`, `M`, `o`, `d`, `e`, `r`, `n`, `i`, `z`, `e`, `d`, `N`, `T`, `i`, `e`, `r`, `/`, `s`, `r`, `c`, `/`, `e`, `S`, `h`, `o`, `p`, `W`, `i`, `n`, `F`, `o`, `r`, `m`, `s`, `/`, `V`, `i`, `e`, `w`, `s`, `/`, `C`, `a`, `t`, `a`, `l`, `o`, `g`, `V`, `i`, `e`, `w`, `.`, `c`, `s`

The product does not release a resource after its effective lifetime has ended, i.e., after the resource is no longer needed.

CatalogView.SetCatalogItems (eShopModernizedNTier\src\eShopWinForms\Views\CatalogView.cs:50-61) creates an Image with Image.FromFile and a separate thumbnail on every catalog load. The original Image is never disposed after the thumbnail is created and its reference is lost at the end of each iteration. Filter changes repeatedly reload the catalog (eShopModernizedNTier\src\eShopWinForms\Controllers\CatalogController.cs:27-30,75-84), accumulating unmanaged image resources until nondeterministic finalization.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** `e`, `S`, `h`, `o`, `p`, `M`, `o`, `d`, `e`, `r`, `n`, `i`, `z`, `e`, `d`, `N`, `T`, `i`, `e`, `r`, `/`, `s`, `r`, `c`, `/`, `e`, `S`, `h`, `o`, `p`, `W`, `i`, `n`, `F`, `o`, `r`, `m`, `s`, `/`, `V`, `i`, `e`, `w`, `s`, `/`, `C`, `a`, `t`, `a`, `l`, `o`, `g`, `V`, `i`, `e`, `w`, `.`, `c`, `s`

The product does not release a file descriptor or handle after its effective lifetime has ended, i.e., after the file descriptor/handle is no longer needed.

CatalogView.SetCatalogItems (eShopModernizedNTier\src\eShopWinForms\Views\CatalogView.cs:57-61) opens each image file using Image.FromFile. That API retains the file lock/handle until the Image is disposed; the original image is never disposed after its thumbnail is generated and its local reference is discarded. Repeated catalog refreshes therefore leave file handles/locks pending garbage collection.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** `src/eShopWCFService/CatalogService.svc.cs`, `src/eShopWCFService/Models/CatalogItem.cs`, `src/eShopWCFService/Web.config`

The product does not properly synchronize shared data, such as static variables across threads, which can lead to undefined behavior and unpredictable data changes.

CatalogService.CreateCatalogItem (eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:65-70) reads the shared database MAX(Id), increments it locally, then saves an inserted row without synchronization covering that read/write sequence. CatalogItem explicitly disables database-generated identifiers (eShopModernizedNTier\src\eShopWCFService\Models\CatalogItem.cs:13-15). The basicHttpBinding service endpoint (eShopModernizedNTier\src\eShopWCFService\Web.config:37-40) can serve concurrent independent request instances; per-instance WCF serialization does not serialize their shared database. Two requests can read the same maximum and insert the same new primary key, causing a failed request. SaveChanges atomicity does not include the earlier MAX query. This single race is mapped to the applicable shared-data/synchronization rules, not separate independent incidents.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** `src/eShopWCFService/CatalogService.svc.cs`, `src/eShopWCFService/Models/CatalogItem.cs`, `src/eShopWCFService/Web.config`

The product utilizes multiple threads or processes to allow temporary access to a shared resource that can only be exclusive to one process at a time, but it does not properly synchronize these actions, which might cause simultaneous accesses of this resource by multiple threads or processes.

CatalogService.CreateCatalogItem (eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:65-70) reads the shared database MAX(Id), increments it locally, then saves an inserted row without synchronization covering that read/write sequence. CatalogItem explicitly disables database-generated identifiers (eShopModernizedNTier\src\eShopWCFService\Models\CatalogItem.cs:13-15). The basicHttpBinding service endpoint (eShopModernizedNTier\src\eShopWCFService\Web.config:37-40) can serve concurrent independent request instances; per-instance WCF serialization does not serialize their shared database. Two requests can read the same maximum and insert the same new primary key, causing a failed request. SaveChanges atomicity does not include the earlier MAX query. This single race is mapped to the applicable shared-data/synchronization rules, not separate independent incidents.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** `src/eShopWCFService/CatalogService.svc.cs`, `src/eShopWCFService/Models/CatalogItem.cs`, `src/eShopWCFService/Web.config`

The product utilizes a shared resource in a concurrent manner but does not attempt to synchronize access to the resource.

CatalogService.CreateCatalogItem (eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:65-70) reads the shared database MAX(Id), increments it locally, then saves an inserted row without synchronization covering that read/write sequence. CatalogItem explicitly disables database-generated identifiers (eShopModernizedNTier\src\eShopWCFService\Models\CatalogItem.cs:13-15). The basicHttpBinding service endpoint (eShopModernizedNTier\src\eShopWCFService\Web.config:37-40) can serve concurrent independent request instances; per-instance WCF serialization does not serialize their shared database. Two requests can read the same maximum and insert the same new primary key, causing a failed request. SaveChanges atomicity does not include the earlier MAX query. This single race is mapped to the applicable shared-data/synchronization rules, not separate independent incidents.

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** `docker-compose.override.yml`

The product contains a hard-coded password, which it uses for its own inbound authentication or for outbound communication to external components.

The SQL Server deployment contains a literal administrator password [MASKED] in sql.data.environment.SA_PASSWORD (eShopModernizedNTier\docker-compose.override.yml:13). The healthcheck also embeds that password [MASKED] for sqlcmd authentication (line 18). These are active deployment values rather than placeholders or comments; no secret values are reproduced.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** `src/eShopWCFService/CatalogService.svc.cs`

When a security-critical event occurs, the product either does not record the event or omits important details about the event when logging it.

CatalogService.CreateCatalogItem (eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:65-71) accepts a remote catalog creation request and persists it with SaveChanges without recording an audit event, caller identity or affected record. The exposed operation is declared at eShopModernizedNTier\src\eShopWCFService\ICatalogService.cs:27-28. Full handwritten source and service configuration contain no application audit logger or interceptor for this security-relevant catalog mutation. Host access logs, if any, are not evidence of record-level auditing.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** `docker-compose.override.yml`

The product contains hard-coded credentials, such as a password or cryptographic key.

The SQL Server deployment contains a literal administrator password [MASKED] in sql.data.environment.SA_PASSWORD (eShopModernizedNTier\docker-compose.override.yml:13). The healthcheck also embeds that password [MASKED] for sqlcmd authentication (line 18). These are active deployment values rather than placeholders or comments; no secret values are reproduced.

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 8
- **Files:** `src/eShopWinForms/Views/CatalogView.cs`, `src/eShopWCFService/CatalogService.svc.cs`, `src/eShopWCFService/ICatalogService.cs`

The product uses external input to construct a pathname that is intended to identify a file or directory that is located underneath a restricted parent directory, but the product does not properly neutralize special elements within the pathname that can cause the pathname to resolve to a location that is outside of the restricted directory.

CatalogView.SetCatalogItems (eShopModernizedNTier\src\eShopWinForms\Views\CatalogView.cs:55-58) concatenates catalogItem.Picturefilename to the restricted Catalog image directory and passes it to Image.FromFile without validating traversal segments. Remote CreateCatalogItem/UpdateCatalogItem operations accept and persist that filename (eShopModernizedNTier\src\eShopWCFService\ICatalogService.cs:27-30; eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:65-76). A filename containing ..\ segments can read an image outside that directory when a client displays the catalog.

### CWE-23: Relative Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** `src/eShopWinForms/Views/CatalogView.cs`, `src/eShopWCFService/CatalogService.svc.cs`, `src/eShopWCFService/ICatalogService.cs`

The product uses external input to construct a pathname that should be within a restricted directory, but it does not properly neutralize sequences such as .. that can resolve to a location that is outside of that directory.

CatalogView.SetCatalogItems (eShopModernizedNTier\src\eShopWinForms\Views\CatalogView.cs:55-58) concatenates catalogItem.Picturefilename to the restricted Catalog image directory and passes it to Image.FromFile without validating traversal segments. Remote CreateCatalogItem/UpdateCatalogItem operations accept and persist that filename (eShopModernizedNTier\src\eShopWCFService\ICatalogService.cs:27-30; eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:65-76). A filename containing ..\ segments can read an image outside that directory when a client displays the catalog.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** `src/eShopWinForms/Views/CatalogView.cs`, `src/eShopWCFService/CatalogService.svc.cs`

The product receives input from an upstream component, but it does not restrict or incorrectly restricts the input before it is used as an identifier for a resource that may be outside the intended sphere of control.

CatalogView.SetCatalogItems (eShopModernizedNTier\src\eShopWinForms\Views\CatalogView.cs:55-58) uses the remote catalog Picturefilename as part of the image resource identifier without confinement. CatalogService.CreateCatalogItem/UpdateCatalogItem (eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:65-76) persist the caller-controlled value. Traversal segments select an image outside the intended assets directory for loading in the desktop client.
