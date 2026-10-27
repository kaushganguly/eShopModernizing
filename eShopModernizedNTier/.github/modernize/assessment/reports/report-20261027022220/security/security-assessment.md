# Security Assessment Report

**Generated:** 2026-10-27T02:32:21.745477+00:00

## Summary

| Metric | Count |
|---|---:|
| TotalFindings | 20 |
| CveCount | 7 |
| CweCount | 13 |
| TotalRulesAssessed | 59 |
| RulesPassed | 46 |

### By Severity

| Severity | Count |
|---|---:|
| mandatory | 5 |
| optional | 6 |
| potential | 9 |

## Coverage and limitations

- Static whole-application review; findings include overlapping CWE mappings and explicitly identified inactive paths. NOT_FOUND is not proof of absence.
- Dependency CLI failed because legacy WebApplication.targets is unavailable. Fallback inspected project manifests and packages.config, plus the existing WinForms assets graph. Assets may be stale; complete service and alternate-project transitive dependency graphs were not regenerated.
- GitHub advisory queries succeeded. CVE minimum severity: medium. Package presence does not establish vulnerable API execution or platform applicability.
- No application source was changed; remediation remains future work.

## CVE Findings (Dependency Vulnerabilities)

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json
- **Category:** CVE
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** eShopModernizedNTier\src\eShopWinForms\eShopWinForms.csproj:23

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr)
Severity: high. Affected resolved transitive dependency: Newtonsoft.Json 10.0.1, attributable to direct package Microsoft.AspNet.WebApi.Client at the listed declaration; the vulnerable dependency is not explicitly declared there. Resolution evidence: eShopModernizedNTier\src\eShopWinForms\obj\project.assets.json. Upgrade to 13.0.1 or a supported patched release, updating the direct parent as necessary.
Reachability: package present in the existing restored WinForms dependency graph; actual vulnerable API execution is not proven. Runtime/RID applicability must be verified, particularly GDI+ on Windows.
Coverage limitation: dotnet list package --include-transitive --format json exited 1 (missing legacy WebApplication.targets). All project PackageReferences and packages.config were inspected. The existing WinForms assets graph supplied transitive versions but may be stale; complete WCF-service and inactive .fx-project transitive graphs could not be regenerated. Direct dependencies returned no matching advisories. GitHub CLI lacked authentication; unauthenticated API queries of all coordinates in batches of 30 succeeded with pagination handling. Withdrawn advisories were excluded.



### CVE-2018-8292: .NET Core Information Disclosure
- **Category:** CVE
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** eShopModernizedNTier\src\eShopWinForms\eShopWinForms.csproj:23

[CVE-2018-8292](https://github.com/advisories/GHSA-7jgj-8wvc-jh57)
Severity: high. Affected resolved transitive dependency: System.Net.Http 4.3.0, attributable to direct package Microsoft.AspNet.WebApi.Client at the listed declaration; the vulnerable dependency is not explicitly declared there. Resolution evidence: eShopModernizedNTier\src\eShopWinForms\obj\project.assets.json. Upgrade to 4.3.4 or a supported patched release, updating the direct parent as necessary.
Reachability: package present in the existing restored WinForms dependency graph; actual vulnerable API execution is not proven. Runtime/RID applicability must be verified, particularly GDI+ on Windows.
Coverage limitation: dotnet list package --include-transitive --format json exited 1 (missing legacy WebApplication.targets). All project PackageReferences and packages.config were inspected. The existing WinForms assets graph supplied transitive versions but may be stale; complete WCF-service and inactive .fx-project transitive graphs could not be regenerated. Direct dependencies returned no matching advisories. GitHub CLI lacked authentication; unauthenticated API queries of all coordinates in batches of 30 succeeded with pagination handling. Withdrawn advisories were excluded.



### CVE-2022-41064: .NET Information Disclosure Vulnerability
- **Category:** CVE
- **Severity:** optional
- **Story Points:** 1
- **Files:** eShopModernizedNTier\src\eShopWinForms\eShopWinForms.csproj:22

[CVE-2022-41064](https://github.com/advisories/GHSA-8g2p-5pqh-5jmc)
Severity: medium. Affected resolved transitive dependency: System.Data.SqlClient 4.8.1, attributable to direct package EntityFramework at the listed declaration; the vulnerable dependency is not explicitly declared there. Resolution evidence: eShopModernizedNTier\src\eShopWinForms\obj\project.assets.json. Upgrade to 4.8.5 or a supported patched release, updating the direct parent as necessary.
Reachability: package present in the existing restored WinForms dependency graph; actual vulnerable API execution is not proven. Runtime/RID applicability must be verified, particularly GDI+ on Windows.
Coverage limitation: dotnet list package --include-transitive --format json exited 1 (missing legacy WebApplication.targets). All project PackageReferences and packages.config were inspected. The existing WinForms assets graph supplied transitive versions but may be stale; complete WCF-service and inactive .fx-project transitive graphs could not be regenerated. Direct dependencies returned no matching advisories. GitHub CLI lacked authentication; unauthenticated API queries of all coordinates in batches of 30 succeeded with pagination handling. Withdrawn advisories were excluded.



### CVE-2024-0056: Microsoft.Data.SqlClient and System.Data.SqlClient vulnerable to SQL Data Provider Security Feature Bypass 
- **Category:** CVE
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** eShopModernizedNTier\src\eShopWinForms\eShopWinForms.csproj:22

[CVE-2024-0056](https://github.com/advisories/GHSA-98g6-xh36-x2p7)
Severity: high. Affected resolved transitive dependency: System.Data.SqlClient 4.8.1, attributable to direct package EntityFramework at the listed declaration; the vulnerable dependency is not explicitly declared there. Resolution evidence: eShopModernizedNTier\src\eShopWinForms\obj\project.assets.json. Upgrade to 4.8.6 or a supported patched release, updating the direct parent as necessary.
Reachability: package present in the existing restored WinForms dependency graph; actual vulnerable API execution is not proven. Runtime/RID applicability must be verified, particularly GDI+ on Windows.
Coverage limitation: dotnet list package --include-transitive --format json exited 1 (missing legacy WebApplication.targets). All project PackageReferences and packages.config were inspected. The existing WinForms assets graph supplied transitive versions but may be stale; complete WCF-service and inactive .fx-project transitive graphs could not be regenerated. Direct dependencies returned no matching advisories. GitHub CLI lacked authentication; unauthenticated API queries of all coordinates in batches of 30 succeeded with pagination handling. Withdrawn advisories were excluded.



### CVE-2019-0820: Regular Expression Denial of Service in System.Text.RegularExpressions
- **Category:** CVE
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** eShopModernizedNTier\src\eShopWinForms\eShopWinForms.csproj:23

[CVE-2019-0820](https://github.com/advisories/GHSA-cmhx-cq75-c4mj)
Severity: high. Affected resolved transitive dependency: System.Text.RegularExpressions 4.3.0, attributable to direct package Microsoft.AspNet.WebApi.Client at the listed declaration; the vulnerable dependency is not explicitly declared there. Resolution evidence: eShopModernizedNTier\src\eShopWinForms\obj\project.assets.json. Upgrade to 4.3.1 or a supported patched release, updating the direct parent as necessary.
Reachability: package present in the existing restored WinForms dependency graph; actual vulnerable API execution is not proven. Runtime/RID applicability must be verified, particularly GDI+ on Windows.
Coverage limitation: dotnet list package --include-transitive --format json exited 1 (missing legacy WebApplication.targets). All project PackageReferences and packages.config were inspected. The existing WinForms assets graph supplied transitive versions but may be stale; complete WCF-service and inactive .fx-project transitive graphs could not be regenerated. Direct dependencies returned no matching advisories. GitHub CLI lacked authentication; unauthenticated API queries of all coordinates in batches of 30 succeeded with pagination handling. Withdrawn advisories were excluded.



### CVE-2021-24112: .NET Core Remote Code Execution Vulnerability
- **Category:** CVE
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** eShopModernizedNTier\src\eShopWinForms\eShopWinForms.csproj:27

[CVE-2021-24112](https://github.com/advisories/GHSA-rxg9-xrhp-64gj)
Severity: critical. Affected resolved transitive dependency: System.Drawing.Common 4.7.0, attributable to direct package System.ServiceModel.Security at the listed declaration; the vulnerable dependency is not explicitly declared there. Resolution evidence: eShopModernizedNTier\src\eShopWinForms\obj\project.assets.json. Upgrade to 4.7.2 or a supported patched release, updating the direct parent as necessary.
Reachability: package present in the existing restored WinForms dependency graph; actual vulnerable API execution is not proven. Runtime/RID applicability must be verified, particularly GDI+ on Windows.
Coverage limitation: dotnet list package --include-transitive --format json exited 1 (missing legacy WebApplication.targets). All project PackageReferences and packages.config were inspected. The existing WinForms assets graph supplied transitive versions but may be stale; complete WCF-service and inactive .fx-project transitive graphs could not be regenerated. Direct dependencies returned no matching advisories. GitHub CLI lacked authentication; unauthenticated API queries of all coordinates in batches of 30 succeeded with pagination handling. Withdrawn advisories were excluded.



### CVE-2022-34716: .NET Information Disclosure Vulnerability
- **Category:** CVE
- **Severity:** optional
- **Story Points:** 1
- **Files:** eShopModernizedNTier\src\eShopWinForms\eShopWinForms.csproj:27

[CVE-2022-34716](https://github.com/advisories/GHSA-vh55-786g-wjwj)
Severity: medium. Affected resolved transitive dependency: System.Security.Cryptography.Xml 4.7.0, attributable to direct package System.ServiceModel.Security at the listed declaration; the vulnerable dependency is not explicitly declared there. Resolution evidence: eShopModernizedNTier\src\eShopWinForms\obj\project.assets.json. Upgrade to 4.7.1 or a supported patched release, updating the direct parent as necessary.
Reachability: package present in the existing restored WinForms dependency graph; actual vulnerable API execution is not proven. Runtime/RID applicability must be verified, particularly GDI+ on Windows.
Coverage limitation: dotnet list package --include-transitive --format json exited 1 (missing legacy WebApplication.targets). All project PackageReferences and packages.config were inspected. The existing WinForms assets graph supplied transitive versions but may be stale; complete WCF-service and inactive .fx-project transitive graphs could not be regenerated. Direct dependencies returned no matching advisories. GitHub CLI lacked authentication; unauthenticated API queries of all coordinates in batches of 30 succeeded with pagination handling. Withdrawn advisories were excluded.



## CWE Findings (Code-Level Vulnerabilities)

### CWE-606: Unchecked Input for Loop Condition
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopModernizedNTier\src\eShopWinForms\Views\CatalogView.cs:48, eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:56

The product does not properly check inputs that are used for loop conditions, potentially leading to a denial of service or other consequences because of excessive looping.

Production-reachable input-sized synchronous rendering: CatalogView.SetCatalogItems (48-62) iterates every service-supplied catalog record, loads and thumbnails a file for each, and creates a grid row on the UI thread, without a count limit. CatalogService.GetCatalogItems (56-62) materializes all database items and returns all matching records without pagination; CreateCatalogItem exposes unbounded record creation. A large remotely supplied or populated catalog therefore drives excessive iteration, image allocation and UI denial of service. Startup and every filter change reach this path.

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopModernizedNTier\src\eShopWCFService\Models\CatalogItemHiLoGenerator.cs:21

When converting from one data type to another, such as long to integer, data can be omitted or translated in a way that produces unexpected values. If the resulting values are used in a sensitive context, then dangerous behaviors may occur.

Inactive helper path: CatalogItemHiLoGenerator.GetNextSequenceValue reads a SQL sequence as Int64 and casts it unchecked to Int32 at line 22, then uses it as a generated catalog identifier. Sequence values outside the Int32 range truncate/wrap and may produce negative or colliding keys. No range guard or checked conversion exists. No current production caller was found; the only dependency registration in ApplicationModule.Load (33-34) is commented out, so this is not asserted exploitable in the current startup configuration.

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopModernizedNTier\src\eShopWinForms\Views\CatalogView.cs:58

The product does not release a resource after its effective lifetime has ended, i.e., after the resource is no longer needed.

Production-reachable image resource leak: CatalogView.SetCatalogItems creates an Image with Image.FromFile (58) and a thumbnail with GetThumbnailImage (59) on each load. The source Image is never disposed after thumbnail creation. ClearGrid (39-42) drops rows without explicitly disposing their thumbnail Image values. Repeated startup/filter loads accumulate native GDI+/image resources until nondeterministic finalization, with no bound or deterministic release.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopModernizedNTier\src\eShopWinForms\Views\CatalogView.cs:58

The product does not release a file descriptor or handle after its effective lifetime has ended, i.e., after the file descriptor/handle is no longer needed.

Production-reachable handle lifetime leak: CatalogView.SetCatalogItems opens catalog image files using Image.FromFile (58), which retains the source-file handle/lock until the Image is disposed. Only the derived thumbnail is placed in the grid and the source Image is abandoned without Dispose after its effective lifetime. Repeated loads retain image-file handles until finalization and can exhaust handles or prevent file updates. Same root cause as CWE-772, not a separate independent defect.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:65, eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:99, eShopModernizedNTier\src\eShopWCFService\Models\CatalogItemsStock.cs:22

The product does not properly synchronize shared data, such as static variables across threads, which can lead to undefined behavior and unpredictable data changes.

Production-reachable cross-request database race: CatalogService.CreateCatalogItem (65-70) reads MAX(Id), increments it in process, then inserts. Separate concurrent WCF requests using independent service instances/DbContexts can read the same maximum before either commits, then both choose the same primary key; at least one request fails. CreateAvailableStock (99-116) similarly performs an unsynchronized read/check, MAX(StockId)+1 allocation and insert/update. Two calls can both observe no stock row, allocate the same key and fault, or allocate different keys and create duplicate item/date rows because CatalogItemsStock has no unique item/date constraint. No serializable read/write transaction, cross-instance lock, atomic allocation, row version or retry synchronizes the logical operation. EF SaveChanges wraps the later write, not the earlier read/check/MAX query. Default WCF single-instance dispatch synchronization would not serialize different service instances or hosts. These CWE mappings describe one shared database synchronization root cause; no claim that per-instance DbContext objects themselves are shared across threads.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:65, eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:99, eShopModernizedNTier\src\eShopWCFService\Models\CatalogItemsStock.cs:22

The product utilizes multiple threads or processes to allow temporary access to a shared resource that can only be exclusive to one process at a time, but it does not properly synchronize these actions, which might cause simultaneous accesses of this resource by multiple threads or processes.

Production-reachable cross-request database race: CatalogService.CreateCatalogItem (65-70) reads MAX(Id), increments it in process, then inserts. Separate concurrent WCF requests using independent service instances/DbContexts can read the same maximum before either commits, then both choose the same primary key; at least one request fails. CreateAvailableStock (99-116) similarly performs an unsynchronized read/check, MAX(StockId)+1 allocation and insert/update. Two calls can both observe no stock row, allocate the same key and fault, or allocate different keys and create duplicate item/date rows because CatalogItemsStock has no unique item/date constraint. No serializable read/write transaction, cross-instance lock, atomic allocation, row version or retry synchronizes the logical operation. EF SaveChanges wraps the later write, not the earlier read/check/MAX query. Default WCF single-instance dispatch synchronization would not serialize different service instances or hosts. These CWE mappings describe one shared database synchronization root cause; no claim that per-instance DbContext objects themselves are shared across threads.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:65, eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:99, eShopModernizedNTier\src\eShopWCFService\Models\CatalogItemsStock.cs:22

The product utilizes a shared resource in a concurrent manner but does not attempt to synchronize access to the resource.

Production-reachable cross-request database race: CatalogService.CreateCatalogItem (65-70) reads MAX(Id), increments it in process, then inserts. Separate concurrent WCF requests using independent service instances/DbContexts can read the same maximum before either commits, then both choose the same primary key; at least one request fails. CreateAvailableStock (99-116) similarly performs an unsynchronized read/check, MAX(StockId)+1 allocation and insert/update. Two calls can both observe no stock row, allocate the same key and fault, or allocate different keys and create duplicate item/date rows because CatalogItemsStock has no unique item/date constraint. No serializable read/write transaction, cross-instance lock, atomic allocation, row version or retry synchronizes the logical operation. EF SaveChanges wraps the later write, not the earlier read/check/MAX query. Default WCF single-instance dispatch synchronization would not serialize different service instances or hosts. These CWE mappings describe one shared database synchronization root cause; no claim that per-instance DbContext objects themselves are shared across threads.

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** eShopModernizedNTier\docker-compose.override.yml:7, eShopModernizedNTier\docker-compose.override.yml:13, eShopModernizedNTier\docker-compose.override.yml:18

The product contains a hard-coded password, which it uses for its own inbound authentication or for outbound communication to external components.

Active default Docker Compose override embeds a literal database login and password in the service ConnectionString (7), the database administrator password environment variable (13), and a sqlcmd healthcheck password argument (18). All credential values are [REDACTED], including samples. CatalogConfiguration.ConnectionString (16-17) reads the environment value used by EntityModel (13), proving the outbound database authentication path. Compose supplies the same fixed database credential to SQL Server and the service; values are neither external references nor placeholders. Reachable when using the checked-in default Compose deployment; validity in any live environment was not tested. Legacy/inactive deployment copies in the scoped temp directory also contain the pattern, but are not claimed active in normal startup. CWE-259 and CWE-798 describe the same credential root cause.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:65, eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:99, eShopModernizedNTier\src\eShopWCFService\Web.config:23

When a security-critical event occurs, the product either does not record the event or omits important details about the event when logging it.

Production-reachable security-significant catalog and stock mutations are not audited: CatalogService.CreateCatalogItem (65-70), UpdateCatalogItem (73-76), RemoveCatalogItem (79-82), and CreateAvailableStock (99-116) persist remote caller changes with no record of caller identity, item identifier, action, previous/new value or outcome. Whole application/configuration inspection found no application audit logger or WCF serviceSecurityAudit behavior. IIS access logging, if externally enabled, would not by itself record mutation details; external database/deployment audit facilities are unknown. This finding concerns application audit coverage, not an assertion that all infrastructure logs are disabled.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** eShopModernizedNTier\docker-compose.override.yml:7, eShopModernizedNTier\docker-compose.override.yml:13, eShopModernizedNTier\docker-compose.override.yml:18

The product contains hard-coded credentials, such as a password or cryptographic key.

Active default Docker Compose override embeds a literal database login and password in the service ConnectionString (7), the database administrator password environment variable (13), and a sqlcmd healthcheck password argument (18). All credential values are [REDACTED], including samples. CatalogConfiguration.ConnectionString (16-17) reads the environment value used by EntityModel (13), proving the outbound database authentication path. Compose supplies the same fixed database credential to SQL Server and the service; values are neither external references nor placeholders. Reachable when using the checked-in default Compose deployment; validity in any live environment was not tested. Legacy/inactive deployment copies in the scoped temp directory also contain the pattern, but are not claimed active in normal startup. CWE-259 and CWE-798 describe the same credential root cause.

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 8
- **Files:** eShopModernizedNTier\src\eShopWinForms\Views\CatalogView.cs:55, eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:65, eShopModernizedNTier\src\eShopWCFService\ICatalogService.cs:27

The product uses external input to construct a pathname that is intended to identify a file or directory that is located underneath a restricted parent directory, but the product does not properly neutralize special elements within the pathname that can cause the pathname to resolve to a location that is outside of the restricted directory.

Production-reachable stored relative traversal: CatalogService.CreateCatalogItem (65-70) and UpdateCatalogItem (73-76), exposed as WCF operations, persist caller-supplied Picturefilename without pathname checks. CatalogController.LoadCatalogItems passes returned records into CatalogView.SetCatalogItems, which concatenates Picturefilename under Assets\\Images\\Catalog and calls Image.FromFile (57-58). Parent-directory segments can escape that directory and open images elsewhere under the desktop user identity. No canonicalization or containment validation exists. Absolute-path override is not claimed because this sink uses string concatenation, not Path.Combine. No upload endpoint or explicit unsafe XML entity resolver was found.

### CWE-23: Relative Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** eShopModernizedNTier\src\eShopWinForms\Views\CatalogView.cs:55, eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:65, eShopModernizedNTier\src\eShopWCFService\ICatalogService.cs:27

The product uses external input to construct a pathname that should be within a restricted directory, but it does not properly neutralize sequences such as .. that can resolve to a location that is outside of that directory.

Production-reachable stored relative traversal: CatalogService.CreateCatalogItem (65-70) and UpdateCatalogItem (73-76), exposed as WCF operations, persist caller-supplied Picturefilename without pathname checks. CatalogController.LoadCatalogItems passes returned records into CatalogView.SetCatalogItems, which concatenates Picturefilename under Assets\\Images\\Catalog and calls Image.FromFile (57-58). Parent-directory segments can escape that directory and open images elsewhere under the desktop user identity. No canonicalization or containment validation exists. Absolute-path override is not claimed because this sink uses string concatenation, not Path.Combine. No upload endpoint or explicit unsafe XML entity resolver was found.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopModernizedNTier\src\eShopWinForms\Views\CatalogView.cs:57, eShopModernizedNTier\src\eShopWCFService\CatalogService.svc.cs:65

The product receives input from an upstream component, but it does not restrict or incorrectly restricts the input before it is used as an identifier for a resource that may be outside the intended sphere of control.

Production-reachable resource identifier injection: CatalogService.CreateCatalogItem and UpdateCatalogItem persist arbitrary Picturefilename received through the WCF contract. CatalogController.LoadCatalogItems passes those objects to CatalogView.SetCatalogItems. Lines 57-58 construct an image resource identifier from Picturefilename without restricting traversal segments and load it with Image.FromFile under the desktop user identity. The resource can escape the intended Catalog image directory. This is the same root cause as CWE-22/CWE-23, not an additional independent bug.
