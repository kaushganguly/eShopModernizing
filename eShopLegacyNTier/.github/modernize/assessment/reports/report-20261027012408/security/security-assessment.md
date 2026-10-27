# Security Assessment Report

**Generated:** 2026-10-27T01:34:15.918652Z

**Scope:** eShopLegacyNTier only. Minimum CVE severity: **medium**.

## Summary

| Metric | Count |
|---|---:|
| Total Findings | 13 |
| CVE Vulnerabilities | 1 |
| CWE Findings | 12 |
| Total Rules Assessed | 59 |
| Rules Passed | 47 |

CWE findings count matching checklist rules, not independent vulnerabilities. In particular, CWE-567, CWE-662, CWE-667 and CWE-820 describe the same database key-allocation race; CWE-22 and CWE-23 overlap, and CWE-99 describes the related resource-selection issue. CWE-772 and CWE-775 describe related image-resource lifetimes.

## Coverage and Limitations

- Static inspection only; no deployed endpoints, database, filesystem permissions or framework patch levels were exercised.
- The mandatory dependency inventory command failed because Microsoft.WebApplication.targets was unavailable. Checked-in legacy package manifests were used; transitive resolution and dependency ancestry remain unverified.
- GitHub advisory REST fallback succeeded. Withdrawn advisories were excluded; the original advisory severity is preserved in cve-assessment-result.json.
- Uncompiled helper patterns without an established production call path are not counted as production findings. NOT_FOUND is not a guarantee of absence.
- Rules Passed is the count of NOT_FOUND checklist entries, not a runtime test pass count.
- An affected dependency version is not proof of an exploitable application code path.

### By Severity

| Severity | Count |
|---|---:|
| mandatory | 1 |
| optional | 2 |
| potential | 10 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json

- **Category:** CVE
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** `src/eShopWinForms/packages.config:5`

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr), HIGH: Newtonsoft.Json 6.0.4 is within affected range < 13.0.1. Deeply nested JSON can cause high CPU/RAM consumption or stack overflow (DoS). Upgrade to 13.0.1 or later; bound serializer depth. Dependency inventory command dotnet list package --include-transitive --format json failed (exit 1): missing Microsoft.WebApplication.targets. Legacy packages.config fallback identified EntityFramework 6.1.3, Microsoft.AspNet.WebApi.Client 5.2.3 and Newtonsoft.Json 6.0.4. Unauthenticated GitHub REST returned HTTP 200; no next page. Withdrawn GHSA-8rfx-6mr3-5jh3 excluded. No active matches returned for other two coordinates. Transitive resolution could not be verified; packages.config records installed dependencies but does not establish parent chains. Checked at 2026-10-27T01:28:41.9492016Z


## CWE Findings (Code-Level Patterns)

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')

- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 8
- **Files:** `src/eShopWinForms/Views/CatalogView.cs:55-58`, `src/eShopWCFService/CatalogService.svc.cs:65-76`, `src/eShopWCFService/ICatalogService.cs:27-30`

CatalogView.SetCatalogItems concatenates service-supplied Picturefilename to the Catalog image directory and passes it to Image.FromFile without validating or canonicalizing the result (lines 55-58). CatalogService.CreateCatalogItem and UpdateCatalogItem persist caller-supplied catalog fields through WCF operations. A relative filename containing parent-directory segments can escape the intended image directory; successful decoding requires a readable image. This is a relative path pattern; rooted paths are not passed independently to this sink.

### CWE-23: Relative Path Traversal

- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** `src/eShopWinForms/Views/CatalogView.cs:55-58`, `src/eShopWCFService/CatalogService.svc.cs:65-76`, `src/eShopWCFService/ICatalogService.cs:27-30`

CatalogView.SetCatalogItems concatenates service-supplied Picturefilename to the Catalog image directory and passes it to Image.FromFile without validating or canonicalizing the result (lines 55-58). CatalogService.CreateCatalogItem and UpdateCatalogItem persist caller-supplied catalog fields through WCF operations. A relative filename containing parent-directory segments can escape the intended image directory; successful decoding requires a readable image. This is a relative path pattern; rooted paths are not passed independently to this sink.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')

- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** `src/eShopWinForms/Views/CatalogView.cs:55-58`, `src/eShopWCFService/Models/CatalogItem.cs:24-25`

CatalogView.SetCatalogItems uses a service-supplied Picturefilename as part of the Image.FromFile resource identifier without restricting it to the Catalog directory. Relative traversal segments can select readable image resources outside that directory.

### CWE-606: Unchecked Input for Loop Condition

- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** `src/eShopWinForms/Views/CatalogView.cs:48-62`, `src/eShopWCFService/CatalogService.svc.cs:56-70`

CatalogView.SetCatalogItems iterates every service-provided item and loads/decodes/resizes its image into a grid without a work/item limit. CatalogService.GetCatalogItems materializes the entire mutable catalog with ToList before filtering and returns an unpaginated list. Catalog size therefore controls unbounded per-refresh looping and image work on the UI thread.

### CWE-682: Incorrect Calculation

- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** `src/eShopWCFService/CatalogService.svc.cs:65-70`

CatalogService.CreateCatalogItem calculates a new manually managed Int32 primary key with ++maxId (lines 67-68) without overflow handling. If the existing maximum is Int32.MaxValue, unchecked addition wraps to Int32.MinValue, generating an unintended resource identifier. This is a conditional calculation defect, not evidence that the present seed database has reached that value.

### CWE-772: Missing Release of Resource after Effective Lifetime

- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** `src/eShopWinForms/Views/CatalogView.cs:48-62`

CatalogView.SetCatalogItems allocates Image img with Image.FromFile at line 58 and only retains a thumbnail. The original image is never disposed after thumbnail generation; repeated refreshes accumulate GDI/native resources pending finalization.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime

- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** `src/eShopWinForms/Views/CatalogView.cs:55-61`

CatalogView.SetCatalogItems opens a file-backed Image with Image.FromFile at line 58 and does not dispose it when thumbnail creation finishes. Image.FromFile retains the source-file lock/handle until disposal, extending file-handle lifetime beyond its use.

### CWE-778: Insufficient Logging

- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** `src/eShopWCFService/CatalogService.svc.cs:65-82`, `src/eShopWCFService/CatalogService.svc.cs:99-116`, `src/eShopWCFService/Web.config:22-43`

CatalogService.CreateCatalogItem, UpdateCatalogItem, RemoveCatalogItem and CreateAvailableStock modify catalog or stock data via WCF and call SaveChanges without an audit event recording actor, target, action or outcome. No application logger, EventLog/Trace audit calls or WCF diagnostic audit configuration is present in reviewed source/config. IIS request logs, if enabled externally, would not establish per-record mutation details; external deployment logging was not inspected.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context

- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** `src/eShopWCFService/CatalogService.svc.cs:65-70`, `src/eShopWCFService/Models/CatalogItem.cs:13-15`

Independent WCF requests share the CatalogItems database table. CreateCatalogItem reads the current maximum key and later inserts max+1 without synchronizing that shared allocation state; overlapping requests can choose the same primary key and one SaveChanges fails. Per-service-instance serialization does not serialize different instances against the same table. Overlap: this is the same key-allocation race recorded under CWE-567/CWE-662/CWE-667/CWE-820, not an additional independent vulnerability.

### CWE-662: Improper Synchronization

- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** `src/eShopWCFService/CatalogService.svc.cs:65-70`, `src/eShopWCFService/Models/CatalogItem.cs:13-15`

CreateCatalogItem performs a read-maximum/increment/insert operation on shared, manually assigned database keys without an atomic allocation operation or enclosing serializing transaction. Concurrent requests can both obtain the same maximum before either insert commits, producing a primary-key collision. SaveChanges atomicity does not cover the preceding Max query. Overlap: this is the same key-allocation race recorded under CWE-567/CWE-662/CWE-667/CWE-820, not an additional independent vulnerability.

### CWE-667: Improper Locking

- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** `src/eShopWCFService/CatalogService.svc.cs:65-70`, `src/eShopWCFService/Models/CatalogItem.cs:13-15`

The exclusive key-allocation critical section in CreateCatalogItem has no lock/serializable transaction protecting the Max query through insertion. Locks taken only for the later database write do not prevent competing callers from selecting the same next key. This is absent protective locking for an inherently exclusive operation, not a lock-release/deadlock claim. Overlap: this is the same key-allocation race recorded under CWE-567/CWE-662/CWE-667/CWE-820, not an additional independent vulnerability.

### CWE-820: Missing Synchronization

- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** `src/eShopWCFService/CatalogService.svc.cs:65-70`, `src/eShopWCFService/Models/CatalogItem.cs:13-15`

CreateCatalogItem accesses shared database key state concurrently but makes no synchronization attempt around Max(i => i.Id), ++maxId and Add/SaveChanges. Two callers can calculate the same next Id; the database uniqueness constraint detects the collision only after the race and the service has no retry path. Overlap: this is the same key-allocation race recorded under CWE-567/CWE-662/CWE-667/CWE-820, not an additional independent vulnerability.
