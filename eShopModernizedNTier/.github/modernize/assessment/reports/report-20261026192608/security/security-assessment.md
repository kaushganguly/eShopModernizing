# Security Assessment Report

**Generated:** 2026-10-26T19:34:38.773404Z

**Minimum CVE severity:** medium

## Summary

| Metric | Count |
|--------|-------|
| Total Findings (rule mappings) | 4 |
| CVE Vulnerabilities | 0 |
| CWE Findings (rule mappings) | 4 |
| Independent underlying issues | 2 |
| Total Rules Assessed | 59 |
| Rules Passed (NOT_FOUND within scope) | 55 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 0 |
| optional | 3 |
| potential | 1 |

### By Category

| Category | Count |
|----------|-------|
| Credentials & Secrets | 1 |
| File & Path Security | 2 |
| Injection Attacks | 1 |

## Scope and Limitations

- CVE assessment covers eight distinct declared NuGet package coordinates (direct PackageReferences and legacy packages.config) queried against GitHub advisories. Minimum included CVE severity is medium; no advisories were returned for those declared coordinates. This is not a clean bill of health for all dependencies.
- Transitive dependency enumeration was blocked by missing Microsoft.WebApplication.targets. Transitive dependencies, runtime/framework vulnerabilities, and image/native decoder internals were not fully assessed.
- CWE assessment is static-only: no runtime exploitation, concurrent tests, deployment isolation, IIS authentication, network ACL, or external audit verification was performed. Generated code was excluded; mocks were not used as vulnerability evidence.
- Four FOUND CWE rule mappings represent two underlying issues, not four independent vulnerabilities. CWE-22, CWE-23, and CWE-99 map to the same image-path traversal issue; CWE-732 maps to unauthenticated catalog mutations in the supplied default deployment.
- NOT_FOUND means no reportable vulnerability was confirmed within scope and review thresholds, not proof of absence. Demonstration/development credentials and availability-only or non-security correctness defects were excluded under the specialist review rules. Credential values are intentionally omitted.

## CVE Findings (Dependency Vulnerabilities)

No matching advisories were returned for the eight assessed declared NuGet coordinates. Transitive dependency coverage is incomplete; see limitations.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-732: Incorrect Permission Assignment for Critical Resource
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** eShopModernizedNTier/src/eShopWCFService/Web.config:39; eShopModernizedNTier/src/eShopWCFService/ICatalogService.cs:25; eShopModernizedNTier/src/eShopWCFService/CatalogService.svc.cs:73; eShopModernizedNTier/docker-compose.override.yml:9

The product specifies permissions for a security-critical resource in a way that allows that resource to be read or modified by unintended actors.

The supplied WCF endpoint uses basicHttpBinding with default security settings and no configured authentication/authorization checks. ICatalogService exposes CreateAvailableStock, CreateCatalogItem, UpdateCatalogItem and RemoveCatalogItem as OperationContracts. CatalogService.UpdateCatalogItem marks the caller-supplied entity Modified at lines 73-76 and saves it without checking caller identity or permissions; other mutation operations likewise persist caller data. Compose publishes the service on host port 5113. A caller able to reach the supplied default endpoint can issue SOAP mutation requests to alter catalog prices/descriptions and stock without application credentials. External IIS authentication or network isolation could constrain reachability but is not configured in the scoped deployment files.

### CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 8
- **Files:** eShopModernizedNTier/src/eShopWinForms/Views/CatalogView.cs:57; eShopModernizedNTier/src/eShopWinForms/Controllers/CatalogController.cs:78; eShopModernizedNTier/src/eShopWCFService/CatalogService.svc.cs:73; eShopModernizedNTier/src/eShopWCFService/Models/CatalogItem.cs:25

External input can resolve outside the intended restricted directory.

SetCatalogItems concatenates the catalog directory and remotely persisted Picturefilename at line 57 and opens it using Image.FromFile at line 58. CreateCatalogItem and UpdateCatalogItem accept the serialized field without containment validation. Parent-directory segments permit loading decodable images outside Assets/Images/Catalog under client filesystem permissions. Arbitrary non-image disclosure or remote exfiltration was not demonstrated.

### CWE-23: Relative Path Traversal
- **Category:** File & Path Security
- **Severity:** optional
- **Story Points:** 5
- **Files:** eShopModernizedNTier/src/eShopWinForms/Views/CatalogView.cs:57; eShopModernizedNTier/src/eShopWCFService/CatalogService.svc.cs:73; eShopModernizedNTier/src/eShopWCFService/Models/CatalogItem.cs:25

Parent-directory segments in external filenames can escape the intended directory.

The CWE-22 data flow permits relative traversal through Picturefilename, for example ..\outside-catalog.png. This is the relative-path manifestation of the same vulnerability, not an independent issue.

### CWE-99: Improper Control of Resource Identifiers ('Resource Injection')
- **Category:** Injection Attacks
- **Severity:** potential
- **Story Points:** 3
- **Files:** eShopModernizedNTier/src/eShopWinForms/Views/CatalogView.cs:57; eShopModernizedNTier/src/eShopWCFService/CatalogService.svc.cs:73; eShopModernizedNTier/src/eShopWCFService/Models/CatalogItem.cs:25

The product receives input from an upstream component, but it does not restrict or incorrectly restricts the input before it is used as an identifier for a resource that may be outside the intended sphere of control.

CatalogView.SetCatalogItems appends the remotely persisted Picturefilename resource identifier to the catalog image directory at line 57 and opens it with Image.FromFile at line 58. The WCF CatalogItem DataMember can be supplied through CreateCatalogItem/UpdateCatalogItem without directory-containment validation. Parent-directory segments select decodable local images outside the catalog directory under client permissions. This is the same underlying traversal issue as CWE-22/CWE-23. Arbitrary non-image disclosure or remote exfiltration was not demonstrated.

