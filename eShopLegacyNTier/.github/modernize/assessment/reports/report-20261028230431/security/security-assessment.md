# Security Assessment Report

**Generated:** 2026-10-28T23:09:35.244588+00:00

**Scope:** `D:\a\eShopModernizing\eShopModernizing\eShopLegacyNTier` only. CVE minimum severity: MEDIUM.

## Summary

Dependency coverage is manifest-only; full dependency enumeration/build was blocked by missing Microsoft.WebApplication.targets. No runtime exploitation tests were performed. The core assessment tool was pinned to version 1.0.1127 and run in Restricted privacy mode.

| Metric | Count |
|--------|-------|
| Total Findings (rule classifications, not unique root causes) | 13 |
| CVE Vulnerabilities | 1 |
| CWE Findings | 12 |
| Total Rules Assessed | 59 |
| Rules Passed (NOT_FOUND, not assurance) | 47 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 1 |
| optional | 2 |
| potential | 10 |

## Scope, interpretation and limitations

- Static source/configuration review only; application code was not changed. No deployment, exploit, load, native-library or concurrent integration tests performed.
- Dependency enumeration failed because Microsoft.WebApplication.targets is absent. packages.config and project references were inspected; complete transitive/runtime coverage is not guaranteed. Live GitHub advisories returned one active affected advisory and one withdrawn duplicate (excluded).
- CWE FOUND includes requested availability/code-quality patterns; it does not mean every item is an exploitable vulnerability. NOT_FOUND means no confirmed match in reviewed source, not a proof of safety.
- CWE-22/23/99 are overlapping classifications of the same image-path issue. CWE-772/775 overlap on image lifetime. CWE-567/662/820 overlap on manual database key allocation. Counts are checklist classifications, not unique vulnerabilities.
- UWP Helpers are present in the tree but excluded by the WinForms project Compile list; no runtime findings were attributed solely to those helpers. Generated code was excluded from CWE review.
- Deployment ACLs, environment secret values, external audit logging, database isolation/quotas and network exposure were not verified. No secret values reproduced.

### Category review evidence

#### CWE - Code Quality

Assessed 16 CWE rules in Code Quality: 5 FOUND, 11 NOT_FOUND
- Reviewed compiled business/UI/data source, loops, initialization, numeric calculations and IDisposable lifetimes. Confirmed decimal-to-double precision loss with a .NET conversion probe. Excluded generated code and did not elevate unused UWP helpers (not compiled) to deployed findings. Finite fixed seed loops are not infinite loops; framework age alone is not obsolete-function evidence. No design requirement supporting CWE-1057 was found. Static assessment; no application build, load test, deployed quotas or memory profiling. FOUND entries include requested code-quality/availability patterns rather than only exploitable security vulnerabilities. CWE-772/775 overlap.

#### CWE - Concurrency & Synchronization

Assessed 6 CWE rules in Concurrency & Synchronization: 3 FOUND, 3 NOT_FOUND
- Reviewed service construction, shared database state, manual key allocation, model attributes, service behavior and lock/transaction/concurrency-token usage. Found non-atomic shared database key allocation across requests. No explicit incorrect lock or synchronization implementation found; absence is reported under missing/improper synchronization rather than inventing a lock misuse. Singleton helper uses ConcurrentDictionary and is not included in WinForms Compile items. No runtime race reproduction or deployed isolation/schema inspection performed.

#### CWE - Credentials & Secrets

Assessed 5 CWE rules in Credentials & Secrets: 1 FOUND, 4 NOT_FOUND
- Scanned hand-written source and checked-in application configuration for passwords, credentials, embedded crypto keys, explicit ACL assignments and logging. Default LocalDB connection string contains no password; environment-provided ConnectionString contents were not read. No secret values reproduced. Missing endpoint authentication and directory browsing are configuration concerns, but do not alone prove an explicit incorrect ACL assignment (CWE-732). Deployment ACLs, external secret stores and infrastructure audit logging were not examined. Static review only.

#### CWE - File & Path Security

Assessed 5 CWE rules in File & Path Security: 2 FOUND, 3 NOT_FOUND
- Reviewed compiled WCF/WinForms source, file/image sinks, service model input and configuration. UWP helper files are not Compile items in eShopWinForms.csproj and have no confirmed runtime reachability. No custom external-entity-enabled XML parser or server upload endpoint found. Static review only; NOT_FOUND is not proof of absence.

#### CWE - Injection Attacks

Assessed 12 CWE rules in Injection Attacks: 1 FOUND, 11 NOT_FOUND
- Reviewed WCF contracts, EntityModel, CatalogService, WinForms rendering and helpers; searched command/SQL/LDAP/XML/XPath/XQuery/deserialization sinks. LINQ-to-Entities does not concatenate SQL. No unsafe polymorphic serializer configuration found; WCF DTO deserialization alone does not establish CWE-502. Json helper is excluded from Compile items. No HTML web-page rendering found. Static review only; deployed configuration and runtime exploitability unverified.

#### CWE - Memory Safety

Assessed 15 CWE rules in Memory Safety: 0 FOUND, 15 NOT_FOUND
- Reviewed hand-written managed C# application sources and project Compile items; searched unsafe code, unmanaged allocation/free, native interop, pointer operations, buffer operations and resource use after disposal. The thumbnail call passes IntPtr.Zero, not an untrusted pointer. No confirmed lifetime or buffer violation found. Native GDI+/framework internals and third-party binaries were not audited or fuzzed; managed code alone is not proof of total memory safety. Generated code excluded.

## CVE Findings (Dependency Vulnerabilities)

### CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json
- **Category:** CVE
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** src/eShopWinForms/packages.config:5

[CVE-2024-21907](https://github.com/advisories/GHSA-5crp-9r3c-p9vr): HIGH severity denial of service from deeply nested JSON. Newtonsoft.Json 6.0.4 is within affected range < 13.0.1; upgrade to 13.0.1 or later supported release. Dependency vulnerability is confirmed, not remote exploitability. GitHub advisories queried for all three distinct manifest packages (also EntityFramework 6.1.3 and Microsoft.AspNet.WebApi.Client 5.2.3); withdrawn duplicate GHSA-8rfx-6mr3-5jh3 excluded. Coverage limitation: dotnet list package --include-transitive --format json failed because Microsoft.WebApplication.targets is missing. Fallback inspected packages.config and project references; transitive/runtime dependency completeness is not guaranteed.

## CWE Findings (Code-Level / Availability Findings)

### CWE-606: Unchecked Input for Loop Condition
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopWinForms/Views/CatalogView.cs:48-61, src/eShopWCFService/CatalogService.svc.cs:56-70

CatalogView.SetCatalogItems loops over every service-provided item and loads/decodes/resizes one image per iteration without a count bound. CatalogService permits adding items and returns all matching items without paging. A large catalog drives excessive UI-thread work and unresponsiveness. Availability/code-quality finding; no infinite loop or demonstrated unauthenticated remote exploit is claimed.

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopWinForms/Views/CatalogView.cs:52-61, src/eShopWCFService/EntityModel.cs:30-32, src/eShopWCFService/Models/CatalogItem.cs:21-23

CatalogView.SetCatalogItems converts decimal monetary Price to double through a string, losing precision for valid SQL money/decimal values. Verified using .NET numeric conversion: 900000000000000.01 renders as 900000000000000.00 after double parsing and F2 formatting, even with zero discount. This amount is within the SQL money range used by CatalogItem.Price and decimal(19,4) precision configuration. This affects displayed monetary calculations; no payment authorization effect established.

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopWinForms/Views/CatalogView.cs:48-62

CatalogView.SetCatalogItems allocates Image img using Image.FromFile and creates a thumbnail, but never disposes the original image after thumbnail generation. Each catalog refresh repeats these allocations and retains native image resources until eventual finalization. Thumbnail disposal when rows are cleared is also not explicit. This is a resource-lifetime/availability issue, not unmanaged memory corruption.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopWinForms/Views/CatalogView.cs:57-61

CatalogView.SetCatalogItems does not dispose the Image returned by Image.FromFile after producing its thumbnail. Image.FromFile keeps the source file locked until Image disposal; subsequent refreshes therefore leave file/native image handles awaiting finalization rather than releasing them at effective lifetime end. Same underlying image-lifetime issue as CWE-772.

### CWE-789: Memory Allocation with Excessive Size Value
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopWCFService/CatalogService.svc.cs:56-70

CatalogService.GetCatalogItems calls ents.CatalogItems.ToList() before filtering and then allocates another list. CreateCatalogItem allows caller-provided records to grow the table without an application count limit. Result allocation scales with caller-influenced database cardinality rather than a bounded page, permitting excessive memory use for large catalogs. This is an availability pattern, not a measured memory exhaustion exploit; infrastructure quotas and WCF transport limits were not tested.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopWCFService/CatalogService.svc.cs:65-70, src/eShopWCFService/Models/CatalogItem.cs:13-15, src/eShopWCFService/CatalogService.svc.cs:99-116, src/eShopWCFService/Models/CatalogItemsStock.cs:22-25

CatalogService.CreateCatalogItem reads the shared database MAX(Id), increments it, and inserts in a separate SaveChanges operation without synchronization spanning the read/allocate/write sequence. Id explicitly uses DatabaseGeneratedOption.None. Concurrent requests handled by different service instances can read the same maximum, choose the same primary key and cause one insert to fail. CreateAvailableStock repeats this pattern for StockId and has a non-atomic existence check. A per-instance WCF single-concurrency default does not serialize all service instances. Database constraints may reject a conflicting write but do not make allocation atomic. CWE-567/662/820 describe the same missing-synchronization root cause, not three independent vulnerabilities. No concurrent execution test performed.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopWCFService/CatalogService.svc.cs:65-70, src/eShopWCFService/Models/CatalogItem.cs:13-15, src/eShopWCFService/CatalogService.svc.cs:99-116, src/eShopWCFService/Models/CatalogItemsStock.cs:22-25

CatalogService.CreateCatalogItem reads the shared database MAX(Id), increments it, and inserts in a separate SaveChanges operation without synchronization spanning the read/allocate/write sequence. Id explicitly uses DatabaseGeneratedOption.None. Concurrent requests handled by different service instances can read the same maximum, choose the same primary key and cause one insert to fail. CreateAvailableStock repeats this pattern for StockId and has a non-atomic existence check. A per-instance WCF single-concurrency default does not serialize all service instances. Database constraints may reject a conflicting write but do not make allocation atomic. CWE-567/662/820 describe the same missing-synchronization root cause, not three independent vulnerabilities. No concurrent execution test performed.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopWCFService/CatalogService.svc.cs:65-70, src/eShopWCFService/Models/CatalogItem.cs:13-15, src/eShopWCFService/CatalogService.svc.cs:99-116, src/eShopWCFService/Models/CatalogItemsStock.cs:22-25

CatalogService.CreateCatalogItem reads the shared database MAX(Id), increments it, and inserts in a separate SaveChanges operation without synchronization spanning the read/allocate/write sequence. Id explicitly uses DatabaseGeneratedOption.None. Concurrent requests handled by different service instances can read the same maximum, choose the same primary key and cause one insert to fail. CreateAvailableStock repeats this pattern for StockId and has a non-atomic existence check. A per-instance WCF single-concurrency default does not serialize all service instances. Database constraints may reject a conflicting write but do not make allocation atomic. CWE-567/662/820 describe the same missing-synchronization root cause, not three independent vulnerabilities. No concurrent execution test performed.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopWCFService/CatalogService.svc.cs:65-82, src/eShopWCFService/Web.config:22-43

CatalogService.CreateCatalogItem, UpdateCatalogItem and RemoveCatalogItem modify persistent catalog data and call SaveChanges without recording actor, operation, target or outcome. No application audit logger or configured WCF audit behavior was found. Consequently security-relevant changes cannot be attributed using application audit records. External IIS/database monitoring might exist but was not provided and cannot be inferred.

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 8
- **Files:** src/eShopWinForms/Views/CatalogView.cs:55-59, src/eShopWCFService/CatalogService.svc.cs:65-76

CatalogView.SetCatalogItems concatenates service-provided Picturefilename into the catalog image directory and passes it to Image.FromFile without canonicalization or containment checking. Relative .. segments can escape that intended directory. CatalogService.CreateCatalogItem/UpdateCatalogItem persist supplied model filenames. Impact is loading an accessible image outside the catalog directory; arbitrary file disclosure or code execution is not established.

### CWE-23: Relative Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopWinForms/Views/CatalogView.cs:55-59, src/eShopWCFService/CatalogService.svc.cs:65-76

CatalogView.SetCatalogItems concatenates service-provided Picturefilename into the catalog image directory and passes it to Image.FromFile without canonicalization or containment checking. Relative .. segments can escape that intended directory. CatalogService.CreateCatalogItem/UpdateCatalogItem persist supplied model filenames. Impact is loading an accessible image outside the catalog directory; arbitrary file disclosure or code execution is not established.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopWinForms/Views/CatalogView.cs:55-59, src/eShopWCFService/CatalogService.svc.cs:65-76

CatalogView.SetCatalogItems uses the upstream service Picturefilename as part of a local image resource identifier without directory containment checks. CatalogService accepts and persists this field. Relative traversal can select an accessible image outside the intended catalog directory. This is the same underlying path issue as CWE-22/23, not an independent exploit.
