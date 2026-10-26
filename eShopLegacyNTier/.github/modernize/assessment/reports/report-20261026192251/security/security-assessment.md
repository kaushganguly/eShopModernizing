# Security Assessment Report

**Generated:** 2026-10-26T19:29:25.807064+00:00

## Scope and limitations

Static baseline assessment of `eShopLegacyNTier` only. Minimum CVE severity: **medium**. No application source was modified. Findings are assessment observations, not remediations. Some CWE findings classify the same root cause and must not be interpreted as independent exploits.

Declared package inventory was checked against GitHub advisories. Transitive enumeration failed because legacy MSBuild web targets were unavailable; transitive dependencies, deployed infrastructure, and runtime exploitability are not exhaustively assessed. `NOT_FOUND` means not detected by this static review, not proof of absence.

## Summary

| Metric | Count |
|---|---|
| TotalFindings | 9 |
| CveCount | 1 |
| CweCount | 8 |
| TotalRulesAssessed | 59 |
| RulesPassed | 51 |

### By Severity

| Severity | Count |
|---|---|
| mandatory | 1 |
| optional | 2 |
| potential | 6 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json

- **Category:** CVE
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** eShopLegacyNTier/src/eShopWinForms/packages.config:5, eShopLegacyNTier/src/eShopWinForms/eShopWinForms.csproj:64

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr), HIGH. Newtonsoft.Json 6.0.4 is declared in packages.config:5 and referenced by the WinForms project at line 64; affected range <13.0.1, first patched 13.0.1. Upgrade to a supported patched release after compatibility testing. Malicious deeply nested JSON can exhaust the parser stack if attacker-controlled JSON reaches deserialization. Dependency presence is confirmed, but remote JSON reachability is not established; the WCF interface uses SOAP. Withdrawn duplicate GHSA-8rfx-6mr3-5jh3 excluded. Coverage: dotnet list package --include-transitive --format json failed (missing Microsoft.WebApplication.targets, exit 1); packages.config and both csproj HintPaths examined instead. Three distinct declared NuGet packages checked against advisory DB: EntityFramework 6.1.3, Microsoft.AspNet.WebApi.Client 5.2.3, Newtonsoft.Json 6.0.4. No advisory returned for the other two. Transitive inventory incomplete; framework/OS and unmanaged binary vulnerabilities are not covered.

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr), HIGH. Newtonsoft.Json 6.0.4 is declared in packages.config:5 and referenced by the WinForms project at line 64; affected range <13.0.1, first patched 13.0.1. Upgrade to a supported patched release after compatibility testing. Malicious deeply nested JSON can exhaust the parser stack if attacker-controlled JSON reaches deserialization. Dependency presence is confirmed, but remote JSON reachability is not established; the WCF interface uses SOAP. Withdrawn duplicate GHSA-8rfx-6mr3-5jh3 excluded. Coverage: dotnet list package --include-transitive --format json failed (missing Microsoft.WebApplication.targets, exit 1); packages.config and both csproj HintPaths examined instead. Three distinct declared NuGet packages checked against advisory DB: EntityFramework 6.1.3, Microsoft.AspNet.WebApi.Client 5.2.3, Newtonsoft.Json 6.0.4. No advisory returned for the other two. Transitive inventory incomplete; framework/OS and unmanaged binary vulnerabilities are not covered.


## CWE Findings (Code-Level Vulnerabilities)

### CWE-772: Missing Release of Resource after Effective Lifetime

- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopLegacyNTier/src/eShopWinForms/Views/CatalogView.cs:39-42, eShopLegacyNTier/src/eShopWinForms/Views/CatalogView.cs:48-62, eShopLegacyNTier/src/eShopWinForms/Controllers/CatalogController.cs:75-84

The product does not release a resource after its effective lifetime has ended, i.e., after the resource is no longer needed.

CatalogView.SetCatalogItems opens each source image via Image.FromFile at line 58 and creates a thumbnail at line 59 without disposing the source image after thumbnail creation. ClearGrid removes rows at line 41 without explicitly disposing thumbnail images. Source Image.FromFile retains a file lock until disposal/finalization. Repeated filter-triggered catalog loads create additional GDI+/file resources, potentially degrading desktop availability or blocking image replacement until finalization. Requires desktop refresh activity; no deterministic remote process exhaustion demonstrated, and eventual GC/finalizers limit persistence. CWE-772 and CWE-775 describe overlapping resource/file-handle aspects of the same source-image lifetime defect.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime

- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopLegacyNTier/src/eShopWinForms/Views/CatalogView.cs:39-42, eShopLegacyNTier/src/eShopWinForms/Views/CatalogView.cs:48-62, eShopLegacyNTier/src/eShopWinForms/Controllers/CatalogController.cs:75-84

The product does not release a file descriptor or handle after its effective lifetime has ended, i.e., after the file descriptor/handle is no longer needed.

CatalogView.SetCatalogItems opens each source image via Image.FromFile at line 58 and creates a thumbnail at line 59 without disposing the source image after thumbnail creation. ClearGrid removes rows at line 41 without explicitly disposing thumbnail images. Source Image.FromFile retains a file lock until disposal/finalization. Repeated filter-triggered catalog loads create additional GDI+/file resources, potentially degrading desktop availability or blocking image replacement until finalization. Requires desktop refresh activity; no deterministic remote process exhaustion demonstrated, and eventual GC/finalizers limit persistence. CWE-772 and CWE-775 describe overlapping resource/file-handle aspects of the same source-image lifetime defect.

### CWE-662: Improper Synchronization

- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** eShopLegacyNTier/src/eShopWCFService/CatalogService.svc.cs:65-70, eShopLegacyNTier/src/eShopWCFService/Models/CatalogItem.cs:13-15, eShopLegacyNTier/src/eShopWCFService/Web.config:37-40

The product utilizes multiple threads or processes to allow temporary access to a shared resource that can only be exclusive to one process at a time, but it does not properly synchronize these actions, which might cause simultaneous accesses of this resource by multiple threads or processes.

CatalogService.CreateCatalogItem reads the shared database maximum Id at line 67, increments it at line 68 and inserts at lines 69-70 without a transaction covering both read and insert or cross-instance synchronization. CatalogItem disables database-generated IDs at lines 13-15. Concurrent SOAP calls handled by separate WCF service instances can read the same maximum and attempt the same primary key; one insert can fail with a duplicate-key exception. WCF default per-instance single concurrency does not serialize access across all instances. A service-reachable actor issuing concurrent creations can induce intermittent legitimate write failures. Database primary-key constraints prevent duplicate committed identifiers; no data overwrite or lasting global outage established. Requires overlapping requests against the same database, not runtime reproduced. CWE-662 and CWE-820 are overlapping classifications of this missing atomicity, not two independent defects.

### CWE-820: Missing Synchronization

- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** eShopLegacyNTier/src/eShopWCFService/CatalogService.svc.cs:65-70, eShopLegacyNTier/src/eShopWCFService/Models/CatalogItem.cs:13-15, eShopLegacyNTier/src/eShopWCFService/Web.config:37-40

The product utilizes a shared resource in a concurrent manner but does not attempt to synchronize access to the resource.

CatalogService.CreateCatalogItem reads the shared database maximum Id at line 67, increments it at line 68 and inserts at lines 69-70 without a transaction covering both read and insert or cross-instance synchronization. CatalogItem disables database-generated IDs at lines 13-15. Concurrent SOAP calls handled by separate WCF service instances can read the same maximum and attempt the same primary key; one insert can fail with a duplicate-key exception. WCF default per-instance single concurrency does not serialize access across all instances. A service-reachable actor issuing concurrent creations can induce intermittent legitimate write failures. Database primary-key constraints prevent duplicate committed identifiers; no data overwrite or lasting global outage established. Requires overlapping requests against the same database, not runtime reproduced. CWE-662 and CWE-820 are overlapping classifications of this missing atomicity, not two independent defects.

### CWE-778: Insufficient Logging

- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopLegacyNTier/src/eShopWCFService/CatalogService.svc.cs:73-77, eShopLegacyNTier/src/eShopWCFService/ICatalogService.cs:29-30, eShopLegacyNTier/src/eShopWCFService/Web.config:22-42

When a security-critical event occurs, the product either does not record the event or omits important details about the event when logging it.

CatalogService.UpdateCatalogItem marks the submitted entity Modified and calls SaveChanges at lines 75-76 without an application audit record of actor, item identity, old/new values or outcome. The remotely exposed operation can alter catalog prices and image filenames. A service-reachable actor can change an item and later restore its values without an application-level trace of the intermediate change. No audit interceptor or logging sink is wired in the assessed source/configuration. IIS access logs or externally configured SQL/database auditing may exist outside this repository; their absence is not asserted. The finding concerns missing semantic application audit events, not a demonstrated bypass of external monitoring.

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')

- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 8
- **Files:** eShopLegacyNTier/src/eShopWinForms/Views/CatalogView.cs:55-59, eShopLegacyNTier/src/eShopWinForms/Controllers/CatalogController.cs:75-84, eShopLegacyNTier/src/eShopWCFService/CatalogService.svc.cs:65-76, eShopLegacyNTier/src/eShopWCFService/ICatalogService.cs:27-30

The product uses external input to construct a pathname that is intended to identify a file or directory that is located underneath a restricted parent directory, but the product does not properly neutralize special elements within the pathname that can cause the pathname to resolve to a location that is outside of the restricted directory.

CatalogView.SetCatalogItems concatenates service-supplied Picturefilename to the Catalog image directory at line 57 and opens it with Image.FromFile at line 58, without canonicalization or directory containment. An actor able to submit CreateCatalogItem/UpdateCatalogItem SOAP requests can persist a relative traversal filename; when a desktop user loads the catalog, a readable image outside Assets is opened in the client security context. The configured basicHttpBinding endpoint has no application authorization checks. Requires endpoint reachability and a valid catalog item/known image path; this establishes out-of-directory image reads, not arbitrary file exfiltration or code execution. Absolute paths do not replace the base because string concatenation is used.

### CWE-23: Relative Path Traversal

- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** eShopLegacyNTier/src/eShopWinForms/Views/CatalogView.cs:55-59, eShopLegacyNTier/src/eShopWinForms/Controllers/CatalogController.cs:75-84, eShopLegacyNTier/src/eShopWCFService/CatalogService.svc.cs:65-76, eShopLegacyNTier/src/eShopWCFService/ICatalogService.cs:27-30

The product uses external input to construct a pathname that should be within a restricted directory, but it does not properly neutralize sequences such as .. that can resolve to a location that is outside of that directory.

CatalogView.SetCatalogItems concatenates service-supplied Picturefilename to the Catalog image directory at line 57 and opens it with Image.FromFile at line 58, without canonicalization or directory containment. An actor able to submit CreateCatalogItem/UpdateCatalogItem SOAP requests can persist a relative traversal filename; when a desktop user loads the catalog, a readable image outside Assets is opened in the client security context. The configured basicHttpBinding endpoint has no application authorization checks. Requires endpoint reachability and a valid catalog item/known image path; this establishes out-of-directory image reads, not arbitrary file exfiltration or code execution. Absolute paths do not replace the base because string concatenation is used.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')

- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopLegacyNTier/src/eShopWinForms/Views/CatalogView.cs:55-59, eShopLegacyNTier/src/eShopWinForms/Controllers/CatalogController.cs:75-84, eShopLegacyNTier/src/eShopWCFService/CatalogService.svc.cs:65-76

The product receives input from an upstream component, but it does not restrict or incorrectly restricts the input before it is used as an identifier for a resource that may be outside the intended sphere of control.

CatalogView.SetCatalogItems uses externally supplied Picturefilename as part of the image resource path, then Image.FromFile opens it. A caller with access to the WCF catalog mutation operations can persist a relative traversal path and influence a subsequent desktop image load outside Assets. This is the same root cause as CWE-22/CWE-23, not an independent vulnerability. Requires a reachable service, accepted catalog object and an existing readable image; no arbitrary code execution or file contents exfiltration demonstrated.

