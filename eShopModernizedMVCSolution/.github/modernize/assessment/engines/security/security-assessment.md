# Security Assessment Report

**Generated:** 2026-10-27T02:22:26.5258694Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 15 |
| CVE Vulnerabilities | 2 |
| CWE Vulnerabilities | 13 |
| Total Rules Assessed | 59 |
| Rules Passed | 46 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 0 |
| optional | 5 |
| potential | 10 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters
- **Severity:** optional
- **Story Points:** 1
- **Files:** src/eShopModernizedMVC/packages.config:10

[CVE-2026-40021](https://github.com/advisories/GHSA-4f7c-pmjv-c25w): Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters

Severity: MEDIUM

Apache Log4net's  XmlLayout https://logging.apache.org/log4net/manual/configuration/layouts.html#layout-list  and  XmlLayoutSchemaLog4J https://logging.apache.org/log4net/manual/configuration/layouts.html#layout-list , in versions before 3.3.0, fail to sanitize characters forbidden by the  XML 1.0 specification https://www.w3.org/TR/xml/#charsets  in MDC property keys and values, as well as the identity field that may carry attacker-influenced data. This causes an exception during serialization and the silent loss of the affected log event.

An attacker who can influence any of these fields can exploit this to suppress individual log records, impairing audit trails and detection of malicious activity.

Users are advised to upgrade to Apache Log4net 3.3.0, which fixes this issue.

Affected dependencies:
  - log4net:2.0.12 (packages.config:10); range < 3.3.0; patched 

Recommended fix: upgrade log4net to 3.3.0 or later.

### CVE-2024-21319: Microsoft ASP.NET Core project templates vulnerable to denial of service
- **Severity:** optional
- **Story Points:** 1
- **Files:** src/eShopModernizedMVC/packages.config:51, src/eShopModernizedMVC/packages.config:97

[CVE-2024-21319](https://github.com/advisories/GHSA-59j7-ghrg-fj52): Microsoft ASP.NET Core project templates vulnerable to denial of service

Severity: MEDIUM

A Denial of Service vulnerability exists in ASP.NET Core project templates which utilize JWT-based authentication tokens. This vulnerability allows an unauthenticated client to consume arbitrarily large amounts of server memory, potentially triggering an out-of-memory condition on the server and making the server no longer able to respond to legitimate requests. 

## Announcement

Announcement for this issue can be found at  https://github.com/dotnet/announcements/issues/290

### Mitigation factors

This impacts only .NET Core-based projects that were created using any version of project templates listed in affected software. 
Other project templates e.g., console applications, MAUI applications, Windows Forms or WPF applications, are not affected.

## Affected software

This impacts only .NET Core-based projects that were created using any version of the below project templates.

-	ASP.NET Core Web App (Model-View-Controller)
-	ASP.NET Core Web API
-	ASP.NET Core Web App (Razor Pages)
-	Blazor Server App
-	Blazor WebAssembly App

## Advisory FAQ

### How do I know if I am affected?

If you are you using project templates listed in affected software, you may be exposed to the vulnerability.

### How do I fix the issue?

#### For existing projects:
If you ever created any of these projects via the dotnet new command or via Visual Studio's File -> New Project gesture, and if you enabled federated authentication at project creation time, your project may be vulnerable. To remediate the vulnerability, use your package manager to update any references you may have to the Microsoft.AspNetCore.Authentication.JwtBearer, Microsoft.AspNetCore.Authentication.OpenIdConnect, and Microsoft.IdentityModel.JsonWebTokens packages to their respective latest versions. 
- [Microsoft.AspNetCore.Authentication.JwtBearer](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.JwtBearer/) 
- [Microsoft.AspNetCore.Authentication.OpenIdConnect](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/) 

If your project does not reference any of those three packages, you are not exposed to this vulnerability.

#### For new projects:
To remediate this issue please update to the latest SDK listed below. Simply installing the SDK update is not sufficient to remediate projects already-created / already-deployed projects which existed prior to this update being released.

* To fix the issue please install the latest version of .NET 6.0 or .NET 7.0. If you have installed one or more .NET SDKs through Visual Studio, Visual Studio will prompt you to update Visual Studio, which will also update your .NET  SDKs.
* If you are using one of the affected packages, please update to the patched version listed above.
* If you have .NET 6.0 or greater installed, you can list the versions you have installed by running the `dotnet --info` command. You will see output like the following;

```
.NET Core SDK (reflecting any global.json):

 Version:   7.0.200
 Commit:    8473146e7d

Runtime Environment:

 OS Name:     Windows
 OS Version:  10.0.18363
 OS Platform: Windows
 RID:         win10-x64
 Base Path:   C:\Program Files\dotnet\sdk\7.0.300\

Host (useful for support):

  Version: 76.0.5
  Commit:  8473146e7d

.NET Core SDKs installed:

  7.0.200 [C:\Program Files\dotnet\sdk]

.NET Core runtimes installed:

  Microsoft.AspNetCore.App 7.0.5 [C:\Program Files\dotnet\shared\Microsoft.AspNetCore.App]
  Microsoft.NETCore.App 7.0.5 [C:\Program Files\dotnet\shared\Microsoft.NETCore.App]
  Microsoft.WindowsDesktop.App 7.0.5 [C:\Program Files\dotnet\shared\Microsoft.WindowsDesktop.App]

To install additional .NET Core runtimes or SDKs:
  https://aka.ms/dotnet-download
```

* If you're using .NET 8.0, you should download and install .NET 8.0.101 SDK (for Visual Studio 2022 v17.8) from https://dotnet.microsoft.com/download/dotnet-core/8.0.
* If you're using .NET 7.0, you should download and install .NET SDK 7.0.115 (for Visual Studio 2022 v17.4) from https://dotnet.microsoft.com/download/dotnet-core/7.0.
* If you're using .NET 6.0, you should download and install .NET SDK 6.0.321 (for Visual Studio 2022 v17.2) from https://dotnet.microsoft.com/download/dotnet-core/6.0.

.NET 6.0, .NET 7.0, .NET 8.0 updates are also available from Microsoft Update. To access this either type "Check for updates" in your Windows search, or open Settings, choose Update & Security and then click Check for Updates.

Once you have installed the updated runtime or SDK, restart your apps for the update to take effect.

Additionally, if you've deployed [self-contained applications](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) targeting any of the impacted versions, these applications are also vulnerable and must be recompiled and redeployed.

## Other Information

### Reporting Security Issues

If you have found a potential security issue in .NET 8.0 or .NET 7.0 or .NET 6.0, please email details to secure@microsoft.com. Reports may qualify for the Microsoft .NET Core & .NET 5 Bounty. Details of the Microsoft .NET Bounty Program including terms and conditions are at <https://aka.ms/corebounty>.

### Support

You can ask questions about this issue on GitHub in the .NET GitHub organization. The main repos are located at https://github.com/dotnet/runtime and https://github.com/dotnet/aspnet/. The Announcements repo (https://github.com/dotnet/Announcements) will contain this bulletin as an issue and will include a link to a discussion issue. You can ask questions in the linked discussion issue.

### Disclaimer

The information provided in this advisory is provided "as is" without warranty of any kind. Microsoft disclaims all warranties, either express or implied, including the warranties of merchantability and fitness for a particular purpose. In no event shall Microsoft Corporation or its suppliers be liable for any damages whatsoever including direct, indirect, incidental, consequential, loss of business profits or special damages, even if Microsoft Corporation or its suppliers have been advised of the possibility of such damages. Some states do not allow the exclusion or limitation of liability for consequential or incidental damages so the foregoing limitation may not apply.

### Acknowledgement

Morgan Brown, Microsoft Corp.

Affected dependencies:
  - System.IdentityModel.Tokens.Jwt:5.6.0 (packages.config:97); range < 5.7.0; patched 
  - System.IdentityModel.Tokens.Jwt:5.6.0 (packages.config:97); range >= 6.5.0, < 6.34.0; patched 
  - System.IdentityModel.Tokens.Jwt:5.6.0 (packages.config:97); range >= 7.0.0-preview, < 7.1.2; patched 
  - Microsoft.IdentityModel.JsonWebTokens:5.6.0 (packages.config:51); range < 5.7.0; patched 
  - Microsoft.IdentityModel.JsonWebTokens:5.6.0 (packages.config:51); range >= 6.5.0, < 6.34.0; patched 
  - Microsoft.IdentityModel.JsonWebTokens:5.6.0 (packages.config:51); range >= 7.0.0-preview, < 7.1.2; patched 

Recommended fix: upgrade affected Microsoft.IdentityModel.JsonWebTokens and System.IdentityModel.Tokens.Jwt packages from 5.6.0 to 5.7.0 or later, and verify the advisory applicability for this .NET Framework MVC deployment.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-681: Incorrect Conversion between Numeric Types
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Models/Infrastructure/CatalogDBInitializer.cs:327, src/eShopModernizedMVC/Models/Infrastructure/dbo.catalog_hilo.Sequence.sql:5

GetSequenceIdFromSelectedDBSequence casts the SQL sequence result from Int64 to int without a range check at CatalogDBInitializer.cs:327. The sequence is declared AS bigint at dbo.catalog_hilo.Sequence.sql:5, so values above Int32.MaxValue can be truncated when assigned as catalog IDs.

### CWE-682: Incorrect Calculation
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Controllers/CatalogController.cs:25-28, src/eShopModernizedMVC/Services/CatalogService.cs:28-29, src/eShopModernizedMVC/ViewModel/PaginatedItemsViewModel.cs:23

The Index action accepts pageSize and pageIndex from query parameters without bounds and passes them through. CatalogService multiplies them for Skip and uses pageSize for Take; the view model divides by pageSize and casts the ceiling to int. Zero, negative, or extreme values can produce a divide-by-zero/overflow or incorrect pagination results.

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Services/ImageAzureStorage.cs:109-116

UpLoadImageFromFile opens a FileStream with File.OpenRead at line 111, uploads it at line 116, and exits without disposing the stream. Repeated catalog initialization can retain file handles and resources.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Services/ImageAzureStorage.cs:109-116

UpLoadImageFromFile opens a file handle with File.OpenRead at line 111 and never closes or disposes it after UploadFromStream at line 116, so repeated calls can exhaust file handles.

### CWE-789: Memory Allocation with Excessive Size Value
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Controllers/CatalogController.cs:25-28, src/eShopModernizedMVC/Services/CatalogService.cs:24-30

The public Index action accepts an unbounded pageSize query parameter and passes it to CatalogService, which calls Take(pageSize).ToList() without a maximum. A very large pageSize can cause a large database result to be materialized in memory.

### CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs:22-26, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:11-15

When UseMockData is enabled, ApplicationModule registers CatalogServiceMock as a SingleInstance. The singleton owns a mutable List<CatalogItem> shared across concurrent web requests.

### CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Services/CatalogServiceMock.cs:11,47-65

CatalogServiceMock mutates and reads its shared catalogItems list in create, update, and remove operations without synchronization. Concurrent requests can race while reading or modifying the list.

### CWE-662: Improper Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedMVC/Modules/ApplicationModule.cs:22-26, src/eShopModernizedMVC/Services/CatalogServiceMock.cs:47-65

The singleton mock catalog is shared across request threads, but its list operations are not protected by a lock or concurrent collection, allowing simultaneous access during writes and reads.

### CWE-820: Missing Synchronization
- **Category:** Concurrency & Synchronization
- **Severity:** potential
- **Story Points:** 8
- **Files:** src/eShopModernizedMVC/Services/CatalogServiceMock.cs:11,18-29,47-65

The shared mutable catalogItems list is accessed by request methods without synchronization. A lock exists in CatalogItemHiLoGenerator for a different resource, but it does not guard this list.

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** docker-compose.override.yml:6, docker-compose.override.yml:21, docker-compose.override.yml:26

The checked-in local Docker Compose override sets a literal SQL Server SA password for the database container and repeats it in the health-check command; the application connection string also uses the built-in SA credential. These are development credentials stored in plaintext configuration, not secret references.

### CWE-732: Incorrect Permission Assignment for Critical Resource
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/eShopModernizedMVC/Services/ImageAzureStorage.cs:46-48, src/eShopModernizedMVC/Services/ImageAzureStorage.cs:89-99, src/eShopModernizedMVC/Controllers/PicController.cs:25-39

InitializeCatalogImages sets the entire pics blob container to BlobContainerPublicAccessType.Blob (public blob reads). The same container receives temporary user uploads via UploadTempImage, and the PicController upload action has no local authorization attribute, so uploaded temporary blobs are also publicly readable by URL.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/eShopModernizedMVC/Controllers/AccountController.cs:14-20

The sign-in action logs only a generic entry message before challenging OpenID Connect. It does not record the authentication result or principal, so successful and failed authentication outcomes are not auditable from the application log.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** docker-compose.override.yml:6, docker-compose.override.yml:21, docker-compose.override.yml:26

The checked-in local Docker Compose override sets a literal SQL Server SA password for the database container and repeats it in the health-check command; the application connection string also uses the built-in SA credential. These are development credentials stored in plaintext configuration, not secret references.

