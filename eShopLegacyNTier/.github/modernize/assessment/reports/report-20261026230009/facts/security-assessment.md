# Security Assessment Report

**Generated:** 2026-10-26T23:05:28.546283+00:00

## Summary

| Metric | Count |
|---|---|
| TotalFindings | 10 |
| CveCount | 1 |
| CweCount | 9 |
| TotalRulesAssessed | 59 |
| RulesPassed | 50 |

Minimum CVE severity: **medium**. CWE counts are checklist matches, not necessarily distinct defects. NOT_FOUND means no confirmed match in the inspected source, not a proof of absence.

### Coverage and limitations

- Scope: only eShopLegacyNTier; generated code excluded from CWE analysis.
- All six CWE categories were assessed sequentially (59 rules).
- Dependency CLI enumeration failed because legacy WebApplication targets were unavailable. Declared packages.config/project dependencies were checked; transitive graph completeness is unverified.
- Results are static assessment findings, not runtime exploit verification. No application remediation was performed.

### By Severity

| Severity | Count |
|---|---|
| mandatory | 1 |
| optional | 2 |
| potential | 7 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json
- **Category:** CVE
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** src\eShopWinForms\packages.config:5, src\eShopWinForms\eShopWinForms.csproj:63, src\eShopWinForms\eShopWinForms.csproj:64

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr): Improper Handling of Exceptional Conditions in Newtonsoft.Json

Severity: HIGH

Affected dependencies:
  - Newtonsoft.Json:6.0.4 (declared at src\eShopWinForms\packages.config:5; project assembly reference at src\eShopWinForms\eShopWinForms.csproj:63 and package HintPath at src\eShopWinForms\eShopWinForms.csproj:64).

Affected range: < 13.0.1. Highly nested JSON can cause stack overflow or excessive CPU/memory consumption, resulting in denial of service. This is a dependency-version finding; application exploitability was not tested.

Recommended fix:
  - Upgrade Newtonsoft.Json to 13.0.1 or later after compatibility review. No dependency versions or application files were changed.

Assessment coverage and limitations:
  - Required dotnet list package --include-transitive --format json exited 1 because Microsoft.WebApplication.targets was unavailable.
  - Fallback inspected both packages.config files and both project files, including PackageReference, assembly Reference/HintPath and ProjectReference declarations. The three distinct detected NuGet coordinates were EntityFramework:6.1.3, Microsoft.AspNet.WebApi.Client:5.2.3 and Newtonsoft.Json:6.0.4. Manifest versions were corroborated with package HintPaths; assembly identity versions were not mistaken for NuGet versions. No PackageReference or ProjectReference declarations were found.
  - All three installed-version declarations were checked with the runtime advisory tool and the paginated public GitHub Advisories API (ecosystem nuget). No active advisories were returned for EntityFramework:6.1.3 or Microsoft.AspNet.WebApi.Client:5.2.3. Withdrawn duplicate GHSA-8rfx-6mr3-5jh3 was excluded.
  - packages.config is a flat installed-package list and does not reliably distinguish direct from transitive packages. Resolved transitive dependency graph completeness could not be verified; undeclared dependencies and .NET Framework/platform vulnerabilities are outside this NuGet assessment. No restore, build, tests or linters were run.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopLegacyNTier\src\eShopWinForms\Views\CatalogView.cs, eShopLegacyNTier\src\eShopWinForms\Controllers\CatalogController.cs

CatalogView.SetCatalogItems lines 50-61 allocates an Image with Image.FromFile and a thumbnail with GetThumbnailImage for each catalog item without disposing either. The source image is no longer needed after thumbnail creation. ClearGrid lines 39-42 clears rows without releasing the thumbnail Image objects. CatalogController.LoadCatalogItems lines 75-84 repeats both methods on filter changes, retaining unmanaged GDI/image resources until nondeterministic finalization and permitting repeated resource pressure.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopLegacyNTier\src\eShopWinForms\Views\CatalogView.cs, eShopLegacyNTier\src\eShopWinForms\Controllers\CatalogController.cs

CatalogView.SetCatalogItems lines 57-59 opens each catalog image via Image.FromFile but never disposes the resulting source Image after making its thumbnail. Image.FromFile keeps the underlying file locked until Image.Dispose/finalization. CatalogController.LoadCatalogItems lines 75-84 and filterChanged lines 27-30 repeatedly execute this path, so file resources remain open beyond their effective lifetime rather than being deterministically released.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** eShopLegacyNTier\src\eShopWCFService\CatalogService.svc.cs, eShopLegacyNTier\src\eShopWCFService\Models\CatalogItem.cs, eShopLegacyNTier\src\eShopWCFService\Models\CatalogItemsStock.cs, eShopLegacyNTier\src\eShopWCFService\Web.config

CatalogService.CreateCatalogItem lines 65-70 reads the shared database maximum Id, increments it locally, then inserts; CatalogItem lines 13-15 disables database-generated identifiers. Independent requests to the configured basicHttp WCF endpoint can interleave and choose the same primary key, causing one SaveChanges to fail. The same allocation race exists for stock at CatalogService lines 113-116, with DatabaseGeneratedOption.None at CatalogItemsStock lines 22-25. There is no cross-request transaction/serialization spanning MAX and insert; the implicit SaveChanges transaction starts too late to protect the preceding read. This is shared-database concurrency, not simultaneous access to a single DbContext. Primary keys prevent duplicate-key persistence, but do not prevent failed requests. Additionally CreateAvailableStock lines 101-116 checks for an existing item/date before insertion without a unique item/date constraint, allowing duplicate logical stock rows when interleaved requests observe absence and later choose different IDs.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** eShopLegacyNTier\src\eShopWCFService\CatalogService.svc.cs, eShopLegacyNTier\src\eShopWCFService\Models\CatalogItem.cs, eShopLegacyNTier\src\eShopWCFService\Models\CatalogItemsStock.cs, eShopLegacyNTier\src\eShopWCFService\Web.config

CatalogService.CreateCatalogItem lines 65-70 reads the shared database maximum Id, increments it locally, then inserts; CatalogItem lines 13-15 disables database-generated identifiers. Independent requests to the configured basicHttp WCF endpoint can interleave and choose the same primary key, causing one SaveChanges to fail. The same allocation race exists for stock at CatalogService lines 113-116, with DatabaseGeneratedOption.None at CatalogItemsStock lines 22-25. There is no cross-request transaction/serialization spanning MAX and insert; the implicit SaveChanges transaction starts too late to protect the preceding read. This is shared-database concurrency, not simultaneous access to a single DbContext. Primary keys prevent duplicate-key persistence, but do not prevent failed requests. Additionally CreateAvailableStock lines 101-116 checks for an existing item/date before insertion without a unique item/date constraint, allowing duplicate logical stock rows when interleaved requests observe absence and later choose different IDs.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** eShopLegacyNTier\src\eShopWCFService\CatalogService.svc.cs, eShopLegacyNTier\src\eShopWCFService\Models\CatalogItem.cs, eShopLegacyNTier\src\eShopWCFService\Models\CatalogItemsStock.cs, eShopLegacyNTier\src\eShopWCFService\Web.config

CatalogService.CreateCatalogItem lines 65-70 reads the shared database maximum Id, increments it locally, then inserts; CatalogItem lines 13-15 disables database-generated identifiers. Independent requests to the configured basicHttp WCF endpoint can interleave and choose the same primary key, causing one SaveChanges to fail. The same allocation race exists for stock at CatalogService lines 113-116, with DatabaseGeneratedOption.None at CatalogItemsStock lines 22-25. There is no cross-request transaction/serialization spanning MAX and insert; the implicit SaveChanges transaction starts too late to protect the preceding read. This is shared-database concurrency, not simultaneous access to a single DbContext. Primary keys prevent duplicate-key persistence, but do not prevent failed requests. Additionally CreateAvailableStock lines 101-116 checks for an existing item/date before insertion without a unique item/date constraint, allowing duplicate logical stock rows when interleaved requests observe absence and later choose different IDs.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopLegacyNTier\src\eShopWCFService\CatalogService.svc.cs, eShopLegacyNTier\src\eShopWCFService\Web.config

CatalogService.CreateCatalogItem, UpdateCatalogItem and RemoveCatalogItem (lines 65-82), and CreateAvailableStock (lines 99-117), modify persistent catalog/pricing and inventory data exposed through WCF OperationContracts. These security-relevant write paths call SaveChanges without any application audit/logging of the operation, affected record, caller, outcome or failure. Web.config lines 22-43 defines service behavior without serviceSecurityAudit or an application audit facility. Ordinary host access logs, if enabled externally, would not establish record-level change auditing. Finding is absence of an application audit trail, not proof of disabled infrastructure logging.

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 8
- **Files:** eShopLegacyNTier\src\eShopWinForms\Views\CatalogView.cs, eShopLegacyNTier\src\eShopWCFService\CatalogService.svc.cs, eShopLegacyNTier\src\eShopWCFService\ICatalogService.cs

CatalogView.SetCatalogItems lines 55-59 appends catalogItem.Picturefilename to the catalog assets directory and passes it to Image.FromFile without validating traversal segments. Remote ICatalogService.CreateCatalogItem/UpdateCatalogItem accept CatalogItem records and persist Picturefilename (CatalogService lines 65-76); GetCatalogItems returns those records to CatalogController.LoadCatalogItems and this image sink. A relative filename containing .. segments can make the desktop client read/decode an image outside the intended assets directory under its own permissions. Absolute-path override is not established because the sink uses string concatenation, not Path.Combine. UWP storage/upload helpers are excluded from the WinForms project and have no reachable application callers.

### CWE-23: Relative Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** eShopLegacyNTier\src\eShopWinForms\Views\CatalogView.cs, eShopLegacyNTier\src\eShopWCFService\CatalogService.svc.cs, eShopLegacyNTier\src\eShopWCFService\ICatalogService.cs

CatalogView.SetCatalogItems lines 55-59 appends catalogItem.Picturefilename to the catalog assets directory and passes it to Image.FromFile without validating traversal segments. Remote ICatalogService.CreateCatalogItem/UpdateCatalogItem accept CatalogItem records and persist Picturefilename (CatalogService lines 65-76); GetCatalogItems returns those records to CatalogController.LoadCatalogItems and this image sink. A relative filename containing .. segments can make the desktop client read/decode an image outside the intended assets directory under its own permissions. Absolute-path override is not established because the sink uses string concatenation, not Path.Combine. UWP storage/upload helpers are excluded from the WinForms project and have no reachable application callers.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopLegacyNTier\src\eShopWinForms\Views\CatalogView.cs, eShopLegacyNTier\src\eShopWinForms\Controllers\CatalogController.cs, eShopLegacyNTier\src\eShopWCFService\CatalogService.svc.cs

Remote catalog records accepted by CatalogService.CreateCatalogItem/UpdateCatalogItem (lines 65-76) include Picturefilename. CatalogController.LoadCatalogItems (lines 75-84) forwards service results into CatalogView.SetCatalogItems (lines 48-61), which uses this unrestricted resource identifier in Image.FromFile (line 58). Relative parent-directory sequences can select and decode local image resources outside Assets/Images/Catalog with desktop-user permissions; there is no directory-containment or filename allowlist check.
