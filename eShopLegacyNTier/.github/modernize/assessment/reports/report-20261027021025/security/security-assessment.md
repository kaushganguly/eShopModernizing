# Security Assessment Report

**Generated:** 2026-10-27T02:14:31.497781+00:00

**Scope:** Existing eShopLegacyNTier source and declared dependencies; static assessment, not diff-only review. Minimum CVE severity: medium.

## Assessment interpretation

- **Proven exploitable vulnerabilities: 0.** One version-matched dependency advisory; eight source-pattern classifications, some overlapping.
- Resource leaks and database races are code-quality/correctness concerns; missing audit instrumentation is a defense-in-depth gap. Static pattern confidence is not exploitability confidence.
- Traversal can cause a victim client to load/display a local image outside the catalog folder if an actor controls its filename. No exfiltration or execution was demonstrated.
- Per-CWE `outcome` records pass/fail of the scoped source-pattern check. Coverage gaps below are explicitly **skip**, never pass.

### Skipped coverage

- **SKIP Resolved transitive dependency graph**: Dependency CLI failed; only legacy manifest/HintPath package versions checked.
- **SKIP Deployment/IIS authentication and network exposure**: Deployment not available or verified; no anonymous access assertion.
- **SKIP Dynamic exploitability and production reachability**: No running application, attack simulation, exhaustion measurement or concurrent DB test performed.
- **SKIP Framework/OS and untracked bundled dependency advisories**: Outside the three queried NuGet coordinates; not counted as passed.

## Summary

| Metric | Count |
|---|---:|
| TotalFindings | 9 |
| CveCount | 1 |
| CweCount | 8 |
| TotalRulesAssessed | 59 |
| RulesPassed | 51 |

### By Severity

| Severity | Count |
|---|---:|
| mandatory | 1 |
| optional | 2 |
| potential | 6 |

## Scope and limitations

- Existing source assessed, not a change-set review. Static analysis only; no exploit or deployment verification.
- dotnet list package --include-transitive --format json failed because Microsoft.WebApplication.targets was unavailable. Legacy packages.config inventories and csproj HintPath versions were used. No restored complete transitive dependency graph was obtained.
- GitHub REST advisories query succeeded for EntityFramework 6.1.3, Microsoft.AspNet.WebApi.Client 5.2.3 and Newtonsoft.Json 6.0.4 (NuGet); one current advisory and one withdrawn duplicate returned; no next page. Coverage excludes unknown transitive dependencies, .NET Framework/Windows components, and bundled JavaScript not represented by these NuGet coordinates.
- Unused UWP helpers are present in source but absent from the WinForms Compile items; their presence alone is not evidence of exploitable runtime deserialization, upload, or singleton vulnerabilities. NOT_FOUND means no confirmed match in the scoped static review, not proof of absence.
- CWE severity values are prescribed assessment priorities, not CVSS exposure ratings. Related CWE entries may describe the same root cause; finding totals are not independent exploit counts.

- Read-only specialist independently corroborated dependency/source locations but did not certify existing-source CWE findings under its changes-only/exploitability remit. Path containment and resource-lifetime entries describe static weaknesses: attacker-observable file disclosure, deployed unauthorized access, and exploitable resource exhaustion were not demonstrated. They must not be interpreted as verified remote exploits.

## CVE Findings (Dependency Vulnerabilities)

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json
- **Category:** CVE
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** `src\eShopWinForms\packages.config:5`, `src\eShopWinForms\eShopWinForms.csproj:63-65`

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr), HIGH: Newtonsoft.Json 6.0.4 is declared in packages.config and referenced by the WinForms project; GitHub lists versions <13.0.1 as affected and 13.0.1 as first patched. Maliciously deeply nested JSON can cause stack overflow/denial of service when processed by vulnerable Json.NET. Dependency presence is confirmed; an active path processing attacker-controlled JSON was not established (the Json helper is not compiled). Upgrade to a supported patched version >=13.0.1 after compatibility validation. The withdrawn duplicate GHSA-8rfx-6mr3-5jh3 was excluded. Advisory query covered the three resolved manifest coordinates only; transitive/runtime coverage is incomplete.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** `src\eShopWinForms\Views\CatalogView.cs:48-62`

CatalogView.SetCatalogItems opens a GDI+ Image via Image.FromFile at line 58 and derives a thumbnail at line 59, but never disposes the original image, including on exceptions. Image.FromFile retains its backing file lock until disposal. Repeated catalog/filter reloads can retain file handles and native image resources until finalization and degrade client availability. Thumbnail retention is not used as the basis of this finding. No remote exploitation or measured exhaustion was demonstrated.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** `src\eShopWinForms\Views\CatalogView.cs:57-59`

CatalogView.SetCatalogItems opens a GDI+ Image via Image.FromFile at line 58 and derives a thumbnail at line 59, but never disposes the original image, including on exceptions. Image.FromFile retains its backing file lock until disposal. Repeated catalog/filter reloads can retain file handles and native image resources until finalization and degrade client availability. Thumbnail retention is not used as the basis of this finding. No remote exploitation or measured exhaustion was demonstrated. This is the retained-file-handle specialization of CWE-772, not a separate leak.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** `src\eShopWCFService\CatalogService.svc.cs:65-70`, `src\eShopWCFService\CatalogService.svc.cs:99-116`, `src\eShopWCFService\Models\CatalogItemsStock.cs:22-25`

CatalogService.CreateCatalogItem reads the maximum ID then assigns max+1 and saves at lines 65-70; CreateAvailableStock repeats max+1 at lines 113-116 after a separate existence check at lines 101-102. StockId explicitly disables database-generated IDs at Models/CatalogItemsStock.cs:22-25. Concurrent WCF sessions/processes can read the same maximum before either commits, producing duplicate-key failures; concurrent stock check/insert operations can also race. SaveChanges' individual transaction does not make the preceding reads and writes atomic together. Requires overlapping writes; sequential use does not trigger it. Actual deployment concurrency and resulting DB errors were not tested.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** `src\eShopWCFService\CatalogService.svc.cs:65-70`, `src\eShopWCFService\CatalogService.svc.cs:99-116`, `src\eShopWCFService\Models\CatalogItemsStock.cs:22-25`

CatalogService.CreateCatalogItem reads the maximum ID then assigns max+1 and saves at lines 65-70; CreateAvailableStock repeats max+1 at lines 113-116 after a separate existence check at lines 101-102. StockId explicitly disables database-generated IDs at Models/CatalogItemsStock.cs:22-25. Concurrent WCF sessions/processes can read the same maximum before either commits, producing duplicate-key failures; concurrent stock check/insert operations can also race. SaveChanges' individual transaction does not make the preceding reads and writes atomic together. Requires overlapping writes; sequential use does not trigger it. Actual deployment concurrency and resulting DB errors were not tested. This is the missing-synchronization classification of the same read-modify-write race as CWE-662.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** `src\eShopWCFService\CatalogService.svc.cs:65-82`, `src\eShopWCFService\CatalogService.svc.cs:99-116`, `src\eShopWCFService\ICatalogService.cs:25-32`

CatalogService.CreateCatalogItem, UpdateCatalogItem, RemoveCatalogItem and CreateAvailableStock directly persist catalog/inventory changes without recording an audit event, caller identity, target or outcome. These are service-exposed business-data mutations, so unauthorized or erroneous changes cannot be attributed using application-level audit records from the supplied source. This confirms missing application audit instrumentation, not proof that hosting/database/infrastructure logging is absent; external logs may partially compensate.

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 8
- **Files:** `src\eShopWinForms\Views\CatalogView.cs:55-58`, `src\eShopWCFService\CatalogService.svc.cs:65-76`, `src\eShopWCFService\ICatalogService.cs:27-30`

CatalogView.SetCatalogItems concatenates service-supplied CatalogItem.Picturefilename to the catalog assets directory at lines 55-58 and passes it to Image.FromFile without normalization or containment validation. CatalogService.CreateCatalogItem/UpdateCatalogItem persist the supplied entity at lines 65-76; ICatalogService exposes those operations at lines 27-30. A caller able to mutate catalog records (or an actor controlling service responses) can supply relative parent-directory segments and make a client open an image outside the intended assets directory under that client's identity. Requires write access to the service/data or control of a response; arbitrary binary disclosure, server file reads and code execution are not established. Invalid/non-image targets may instead raise an exception.

### CWE-23: Relative Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** `src\eShopWinForms\Views\CatalogView.cs:55-58`, `src\eShopWCFService\CatalogService.svc.cs:65-76`

CatalogView.SetCatalogItems concatenates service-supplied CatalogItem.Picturefilename to the catalog assets directory at lines 55-58 and passes it to Image.FromFile without normalization or containment validation. CatalogService.CreateCatalogItem/UpdateCatalogItem persist the supplied entity at lines 65-76; ICatalogService exposes those operations at lines 27-30. A caller able to mutate catalog records (or an actor controlling service responses) can supply relative parent-directory segments and make a client open an image outside the intended assets directory under that client's identity. Requires write access to the service/data or control of a response; arbitrary binary disclosure, server file reads and code execution are not established. Invalid/non-image targets may instead raise an exception. This is the relative-path specialization of the CWE-22 finding, not an additional independent exploit.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** `src\eShopWinForms\Views\CatalogView.cs:55-58`, `src\eShopWCFService\CatalogService.svc.cs:65-76`

CatalogView.SetCatalogItems concatenates service-supplied CatalogItem.Picturefilename to the catalog assets directory at lines 55-58 and passes it to Image.FromFile without normalization or containment validation. CatalogService.CreateCatalogItem/UpdateCatalogItem persist the supplied entity at lines 65-76; ICatalogService exposes those operations at lines 27-30. A caller able to mutate catalog records (or an actor controlling service responses) can supply relative parent-directory segments and make a client open an image outside the intended assets directory under that client's identity. Requires write access to the service/data or control of a response; arbitrary binary disclosure, server file reads and code execution are not established. Invalid/non-image targets may instead raise an exception. This is the resource-identifier aspect of the same image-path root cause.
