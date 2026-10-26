# Security Assessment Report

**Generated:** 2026-10-26T20:00:59.068903Z
**Scope:** `D:\a\eShopModernizing\eShopModernizing\eShopLegacyNTier`
**Minimum CVE severity:** medium

## Summary

| Metric | Count |
|--------|-------|
| TotalFindings | 9 |
| CveCount | 1 |
| CweCount | 8 |
| TotalRulesAssessed | 59 |
| RulesPassed | 51 |

### By Severity

| Classification | Count |
|----------------|-------|
| mandatory | 1 |
| optional | 2 |
| potential | 6 |

### By Category

| Classification | Count |
|----------------|-------|
| CVE | 1 |
| File & Path Security | 2 |
| Injection Attacks | 1 |
| Code Quality | 2 |
| Credentials & Secrets | 1 |
| Concurrency & Synchronization | 2 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json
- **Category:** CVE
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** `src\eShopWinForms\packages.config:5`

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr): Improper Handling of Exceptional Conditions in Newtonsoft.Json

Severity: HIGH

Affected dependencies:
- Newtonsoft.Json@6.0.4; affected range < 13.0.1; patched 13.0.1.

Recommended fix: upgrade affected packages to the verified patched version or later. Manifest presence establishes dependency exposure, not demonstrated exploitability. CLI dependency resolution failed (missing Microsoft.WebApplication.targets); packages.config inventory fallback, transitive provenance unavailable.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 8
- **Files:** `src\eShopWinForms\Views\CatalogView.cs:57`, `src\eShopWCFService\CatalogService.svc.cs:65`

CatalogView.SetCatalogItems concatenates service-returned Picturefilename into the local Catalog image directory at lines 57-58 without filename restriction or canonical containment. CatalogService.CreateCatalogItem (65-70) and UpdateCatalogItem (73-76) persist the supplied model without filename validation. Relative parent-directory segments can select images outside the intended directory under the desktop process identity; attacker needs catalog write access or control of the service response. No arbitrary file-content exfiltration is demonstrated.

### CWE-23: Relative Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** `src\eShopWinForms\Views\CatalogView.cs:57`, `src\eShopWCFService\CatalogService.svc.cs:65`

CatalogView.SetCatalogItems concatenates service-returned Picturefilename into the local Catalog image directory at lines 57-58 without filename restriction or canonical containment. CatalogService.CreateCatalogItem (65-70) and UpdateCatalogItem (73-76) persist the supplied model without filename validation. Relative parent-directory segments can select images outside the intended directory under the desktop process identity; attacker needs catalog write access or control of the service response. No arbitrary file-content exfiltration is demonstrated.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** `src\eShopWinForms\Views\CatalogView.cs:57`, `src\eShopWinForms\Controllers\CatalogController.cs:78`

CatalogController.LoadCatalogItems forwards WCF catalog models to CatalogView.SetCatalogItems. Picturefilename controls an image resource opened by Image.FromFile at lines 57-58 without containment checks. Catalog writers or a controlled response can select resources outside the catalog directory via parent segments. This is the same underlying data flow as CWE-22/CWE-23, not an independent exploit.

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** `src\eShopWinForms\Views\CatalogView.cs:48`, `src\eShopWinForms\Views\CatalogView.cs:58`, `src\eShopWinForms\Views\CatalogView.cs:39`

CatalogView.SetCatalogItems creates an Image with Image.FromFile at line 58, makes a separate thumbnail at 59, and discards the original Image reference without Dispose. Image.FromFile retains the backing file lock until Image disposal; each filter-triggered reload repeats this allocation. ClearGrid at 39-42 removes rows without explicitly releasing thumbnail Images. This confirms missing deterministic release of image/native resources and backing image file handles, with repeated refresh resource pressure and locked image files. CWE-772 and CWE-775 describe overlapping aspects of this same lifetime defect; exhaustion timing was not measured.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** `src\eShopWinForms\Views\CatalogView.cs:48`, `src\eShopWinForms\Views\CatalogView.cs:58`, `src\eShopWinForms\Views\CatalogView.cs:39`

CatalogView.SetCatalogItems creates an Image with Image.FromFile at line 58, makes a separate thumbnail at 59, and discards the original Image reference without Dispose. Image.FromFile retains the backing file lock until Image disposal; each filter-triggered reload repeats this allocation. ClearGrid at 39-42 removes rows without explicitly releasing thumbnail Images. This confirms missing deterministic release of image/native resources and backing image file handles, with repeated refresh resource pressure and locked image files. CWE-772 and CWE-775 describe overlapping aspects of this same lifetime defect; exhaustion timing was not measured.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** `src\eShopWCFService\CatalogService.svc.cs:73`, `src\eShopWCFService\CatalogService.svc.cs:99`, `src\eShopWCFService\Web.config:23`

CatalogService.UpdateCatalogItem (73-76), RemoveCatalogItem (79-82), and CreateAvailableStock (99-116) modify catalog pricing/inventory and commit changes without recording actor, target, before/after values, outcome, or an audit event. No application logging/audit calls, EF audit override or WCF message audit configuration was found in scoped source/configuration. These business-sensitive mutations therefore have no source-level application audit trail; external IIS/database auditing is deployment-dependent and was not assessed.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** `src\eShopWCFService\CatalogService.svc.cs:65`, `src\eShopWCFService\CatalogService.svc.cs:99`, `src\eShopWCFService\Models\CatalogItemsStock.cs:22`

CatalogService.CreateCatalogItem (65-70) reads global MAX(Id), increments it locally, then inserts and saves without a transaction spanning the read/write. Concurrent WCF request instances sharing the database can select the same next ID; primary-key enforcement rejects one write rather than preventing the race. CreateAvailableStock (99-116) also separates existence check and MAX(StockId) from insertion; StockId is explicitly DatabaseGeneratedOption.None (CatalogItemsStock 22-25). WCF per-session/per-call instance isolation does not serialize database access across clients. Confirmed non-atomic identifier allocation causes competing insert failures; database corruption or bypass of constraints is not claimed. CWE-662 and CWE-820 label the same missing synchronization defect.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** `src\eShopWCFService\CatalogService.svc.cs:65`, `src\eShopWCFService\CatalogService.svc.cs:99`, `src\eShopWCFService\Models\CatalogItemsStock.cs:22`

CatalogService.CreateCatalogItem (65-70) reads global MAX(Id), increments it locally, then inserts and saves without a transaction spanning the read/write. Concurrent WCF request instances sharing the database can select the same next ID; primary-key enforcement rejects one write rather than preventing the race. CreateAvailableStock (99-116) also separates existence check and MAX(StockId) from insertion; StockId is explicitly DatabaseGeneratedOption.None (CatalogItemsStock 22-25). WCF per-session/per-call instance isolation does not serialize database access across clients. Confirmed non-atomic identifier allocation causes competing insert failures; database corruption or bypass of constraints is not claimed. CWE-662 and CWE-820 label the same missing synchronization defect.

## Coverage and limitations

- Full scoped source assessment, not a diff review. No application code changes, restore, build, exploit execution, staging or commits. Generated proxies/designers are not independently audited; build inclusion and service contracts were checked. No .github/agents access.
- Inputs are explicitly requested flat arrays, normalized for merge instead of wrapper objects. Minimum CVE severity: medium. All 59 requested CWE rules were assessed.
- NuGet CLI exited 1: missing Microsoft.WebApplication.targets. Fallback parsed both legacy packages.config files and checked project HintPaths: four declarations, three distinct package/version coordinates. This flattened inventory does not expose direct/transitive ancestry; unresolved or additional transitive dependencies cannot be ruled out. Runtime/framework, OS and native third-party implementations were not comprehensively assessed.
- GitHub reviewed advisories queried all three declared NuGet coordinates in one batch with pagination handling. CVE-2024-21907 affects Newtonsoft.Json 6.0.4 (<13.0.1; patched 13.0.1). Withdrawn duplicate GHSA-8rfx-6mr3-5jh3 excluded. No advisory matched the other two coordinates. Dependency presence is not demonstrated exploitability: Json helper is not compiled into WinForms.
- NOT_FOUND means no confirmed pattern in scoped static source, not proof of absence. Runtime deployment, IIS authentication/ACLs, database isolation/constraints, external auditing and attack reachability were not tested. No hard-coded credentials confirmed; reports contain no secret values.
- Counts represent rule matches, not independent vulnerabilities. CWE-22/CWE-23/CWE-99 share one traversal flow; CWE-772/CWE-775 overlap in image lifetime; CWE-662/CWE-820 share non-atomic identifier allocation. CWE-778 is an audit gap. Eight CWE matches describe four distinct source concerns.
- Assessment is limited to requested CVE/CWE coverage. Missing method-level authorization and transport issues outside these requested rule IDs were not misrepresented as unrelated CWE findings.
