# eShopModernizedWebForms

## Summary

| Metric | Value |
|--------|-------|
| Total Issues | 26 |
| Mandatory Blockers | 6 |
| Potential Issues | 10 |

## Component Information

| Property | Value |
|----------|-------|
| Language | C# |
| Frameworks | .NETFramework,Version=v4.7.2 |
| Build tools | MSBuild |

## Cloud Readiness Issues

| Issue Name | Criticality | Story Points | Occurrences |
|------------|-------------|--------------|-------------|
| Certificate management dependency detected | Potential | 5 | [2](#Certificate_management_dependency_detected) |
| SQL database connection detected | Potential | 3 | [1](#SQL_database_connection_detected) |
| Hardcoded URLs detected | Potential | 1 | [1](#Hardcoded_URLs_detected) |
| Static content detected | Optional | 3 | [1](#Static_content_detected) |
| System.Data.SqlClient dependency detected | Optional | 3 | [1](#System_Data_SqlClient_dependency_detected) |
| Hardcoded sensitive data detected | Optional | 3 | [1](#Hardcoded_sensitive_data_detected) |

### Issue Details

<details id="Certificate_management_dependency_detected">
<summary><b>Certificate management dependency detected</b> — affected files</summary>

- `src\eShopModernizedWebForms\eShopModernizedWebForms.csproj`
- `src\eShopModernizedWebForms\eShopModernizedWebForms.csproj`

</details>

<details id="SQL_database_connection_detected">
<summary><b>SQL database connection detected</b> — affected files</summary>

- `src\eShopModernizedWebForms\Web.config`

</details>

<details id="Hardcoded_URLs_detected">
<summary><b>Hardcoded URLs detected</b> — affected files</summary>

- `src\eShopModernizedWebForms\App_Start\SqlAccessTokenProvider.cs (line 30)`

</details>

<details id="Static_content_detected">
<summary><b>Static content detected</b> — affected files</summary>

- `src\eShopModernizedWebForms\eShopModernizedWebForms.csproj`

</details>

<details id="System_Data_SqlClient_dependency_detected">
<summary><b>System.Data.SqlClient dependency detected</b> — affected files</summary>

- `src\eShopModernizedWebForms\Web.config`

</details>

<details id="Hardcoded_sensitive_data_detected">
<summary><b>Hardcoded sensitive data detected</b> — affected files</summary>

- `src\eShopModernizedWebForms\Web.config`

</details>

## DotNET Upgrade Issues [View Details](scenarios/dotnet-version-upgrade/assessment.md)

| Issue Category | Criticality | Story Points | Occurrences |
|----------------|-------------|--------------|-------------|
| NuGet package functionality is included with framework reference | Mandatory | 1 | [61](#NuGet_package_functionality_is_included_with_framework_reference) |
| NuGet package is incompatible | Mandatory | 1 | [27](#NuGet_package_is_incompatible) |
| Manual redirect conflicts with auto-generated version | Mandatory | 1 | [15](#Manual_redirect_conflicts_with_auto-generated_version) |
| Project file needs to be converted to SDK-style | Mandatory | 1 | [1](#Project_file_needs_to_be_converted_to_SDK-style) |
| Project's target framework(s) needs to be changed | Mandatory | 1 | [1](#Project_s_target_framework_s_needs_to_be_changed) |
| Convert application initialization code from Global.asax.cs to .NET Core and clean up Global.asax.cs | Mandatory | 1 | [1](#Convert_application_initialization_code_from_Global_asax_cs_to_NET_Core_and_clean_up_Global_asax_cs) |
| NuGet package upgrade is recommended | Potential | 1 | [22](#NuGet_package_upgrade_is_recommended) |
| Binding redirect forces version downgrade | Potential | 1 | [14](#Binding_redirect_forces_version_downgrade) |
| NuGet package is deprecated | Optional | 1 | [37](#NuGet_package_is_deprecated) |
| NuGet package contains security vulnerability | Optional | 1 | [3](#NuGet_package_contains_security_vulnerability) |

### Issue Details

<details id="NuGet_package_functionality_is_included_with_framework_reference">
<summary><b>NuGet package functionality is included with framework reference</b> — affected files</summary>

- `src\eShopModernizedWebForms\eShopModernizedWebForms.csproj`

</details>

<details id="NuGet_package_is_incompatible">
<summary><b>NuGet package is incompatible</b> — affected files</summary>

- `src\eShopModernizedWebForms\eShopModernizedWebForms.csproj`

</details>

<details id="Manual_redirect_conflicts_with_auto-generated_version">
<summary><b>Manual redirect conflicts with auto-generated version</b> — affected files</summary>

- `src\eShopModernizedWebForms\Web.config`

</details>

<details id="Project_file_needs_to_be_converted_to_SDK-style">
<summary><b>Project file needs to be converted to SDK-style</b> — affected files</summary>

- `src\eShopModernizedWebForms\eShopModernizedWebForms.csproj`

</details>

<details id="Project_s_target_framework_s_needs_to_be_changed">
<summary><b>Project's target framework(s) needs to be changed</b> — affected files</summary>

- `src\eShopModernizedWebForms\eShopModernizedWebForms.csproj`

</details>

<details id="Convert_application_initialization_code_from_Global_asax_cs_to_NET_Core_and_clean_up_Global_asax_cs">
<summary><b>Convert application initialization code from Global.asax.cs to .NET Core and clean up Global.asax.cs</b> — affected files</summary>

- `src\eShopModernizedWebForms\Global.asax.cs`

</details>

<details id="NuGet_package_upgrade_is_recommended">
<summary><b>NuGet package upgrade is recommended</b> — affected files</summary>

- `src\eShopModernizedWebForms\eShopModernizedWebForms.csproj`

</details>

<details id="Binding_redirect_forces_version_downgrade">
<summary><b>Binding redirect forces version downgrade</b> — affected files</summary>

- `src\eShopModernizedWebForms\Web.config`

</details>

<details id="NuGet_package_is_deprecated">
<summary><b>NuGet package is deprecated</b> — affected files</summary>

- `src\eShopModernizedWebForms\eShopModernizedWebForms.csproj`

</details>

<details id="NuGet_package_contains_security_vulnerability">
<summary><b>NuGet package contains security vulnerability</b> — affected files</summary>

- `src\eShopModernizedWebForms\eShopModernizedWebForms.csproj`

</details>

## Security Issues

> **Note:** These issues were generated by AI and may contain inaccuracies or incomplete information. Please review carefully.

| Issue Name | Criticality | Story Points | Files |
|------------|-------------|--------------|-------|
| CWE-682: Incorrect Calculation | Potential | 5 | [3](#CWE-682_Incorrect_Calculation) |
| CWE-681: Incorrect Conversion between Numeric Types | Potential | 3 | [3](#CWE-681_Incorrect_Conversion_between_Numeric_Types) |
| CWE-772: Missing Release of Resource after Effective Lifetime | Potential | 3 | [1](#CWE-772_Missing_Release_of_Resource_after_Effective_Lifetime) |
| CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime | Potential | 3 | [1](#CWE-775_Missing_Release_of_File_Descriptor_or_Handle_after_Effective_Lifetime) |
| CWE-99: Improper Control of Resource Identifiers ('Resource Injection') | Potential | 3 | [2](#CWE-99_Improper_Control_of_Resource_Identifiers_Resource_Injection) |
| CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting') | Optional | 8 | [3](#CWE-79_Improper_Neutralization_of_Input_During_Web_Page_Generation_Cross-site_Scripting) |
| CWE-259: Use of Hard-coded Password | Optional | 5 | [1](#CWE-259_Use_of_Hard-coded_Password) |
| CWE-798: Use of Hard-coded Credentials | Optional | 5 | [1](#CWE-798_Use_of_Hard-coded_Credentials) |
| CVE-2024-21319: Microsoft ASP.NET Core project templates vulnerable to denial of service | Optional | 1 | [1](#CVE-2024-21319_Microsoft_ASP_NET_Core_project_templates_vulnerable_to_denial_of_service) |
| CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters | Optional | 1 | [1](#CVE-2026-40021_Apache_Log4net_Silent_log_event_loss_in_XmlLayout_and_XmlLayoutSchemaLog4J_due_to_unescaped_XML_1_0_forbidden_characters) |

### Security Issue Details

<details id="CWE-682_Incorrect_Calculation">
<summary><b>CWE-682: Incorrect Calculation</b> — affected files</summary>

- `src/eShopModernizedWebForms/Default.aspx.cs:31-35`
- `src/eShopModernizedWebForms/Services/CatalogService.cs:24-30`
- `src/eShopModernizedWebForms/ViewModel/PaginatedItemsViewModel.cs:18-24`

</details>

<details id="CWE-681_Incorrect_Conversion_between_Numeric_Types">
<summary><b>CWE-681: Incorrect Conversion between Numeric Types</b> — affected files</summary>

- `src/eShopModernizedWebForms/Models/CatalogItemHiLoGenerator.cs:19-20`
- `src/eShopModernizedWebForms/Models/Infrastructure/CatalogDBInitializer.cs:323-326`
- `src/eShopModernizedWebForms/Models/Infrastructure/dbo.catalog_hilo.Sequence.sql:4-9`

</details>

<details id="CWE-772_Missing_Release_of_Resource_after_Effective_Lifetime">
<summary><b>CWE-772: Missing Release of Resource after Effective Lifetime</b> — affected files</summary>

- `src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:107-115`

</details>

<details id="CWE-775_Missing_Release_of_File_Descriptor_or_Handle_after_Effective_Lifetime">
<summary><b>CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime</b> — affected files</summary>

- `src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:107-115`

</details>

<details id="CWE-99_Improper_Control_of_Resource_Identifiers_Resource_Injection">
<summary><b>CWE-99: Improper Control of Resource Identifiers ('Resource Injection')</b> — affected files</summary>

- `src/eShopModernizedWebForms/Catalog/PicUploader.asmx.cs:40-41`
- `src/eShopModernizedWebForms/Services/ImageAzureStorage.cs:87-97`

</details>

<details id="CWE-79_Improper_Neutralization_of_Input_During_Web_Page_Generation_Cross-site_Scripting">
<summary><b>CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')</b> — affected files</summary>

- `src/eShopModernizedWebForms/Catalog/Details.aspx:16`
- `src/eShopModernizedWebForms/Catalog/Details.aspx:23`
- `src/eShopModernizedWebForms/Catalog/Edit.aspx.cs:69-70`

</details>

<details id="CWE-259_Use_of_Hard-coded_Password">
<summary><b>CWE-259: Use of Hard-coded Password</b> — affected files</summary>

- `docker-compose.override.yml:21`

</details>

<details id="CWE-798_Use_of_Hard-coded_Credentials">
<summary><b>CWE-798: Use of Hard-coded Credentials</b> — affected files</summary>

- `docker-compose.override.yml:21`

</details>

<details id="CVE-2024-21319_Microsoft_ASP_NET_Core_project_templates_vulnerable_to_denial_of_service">
<summary><b>CVE-2024-21319: Microsoft ASP.NET Core project templates vulnerable to denial of service</b> — affected files</summary>

- `src/eShopModernizedWebForms/packages.config:53`

</details>

<details id="CVE-2026-40021_Apache_Log4net_Silent_log_event_loss_in_XmlLayout_and_XmlLayoutSchemaLog4J_due_to_unescaped_XML_1_0_forbidden_characters">
<summary><b>CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters</b> — affected files</summary>

- `src/eShopModernizedWebForms/packages.config:11`

</details>

---

## Codebase Insights

> **Note:** These documents are generated by AI and may contain inaccuracies or incomplete information. Please review carefully.

1. **[Architecture Diagram](facts/architecture-diagram.md)** — Understand the big picture: system layers and component relationships
2. **[Dependency Map](facts/dependency-map.md)** — Know what the project depends on and where the risks are
3. **[API & Service Contracts](facts/api-service-contracts.md)** — See how services communicate and what contracts they expose
4. **[Data Architecture](facts/data-architecture.md)** — Explore data models, storage, and data flow patterns
5. **[Configuration Inventory](facts/configuration-inventory.md)** — Review how the application is configured across environments
6. **[Business Workflows](facts/business-workflows.md)** — Trace end-to-end business processes and domain logic

[Share feedback](https://aka.ms/ghcp-appmod/feedback)
