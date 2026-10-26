# eShopModernizedMVC

## Summary

| Metric | Value |
|--------|-------|
| Total Issues | 24 |
| Mandatory Blockers | 8 |
| Potential Issues | 9 |

## Component Information

| Property | Value |
|----------|-------|
| Language | C# |
| Frameworks | .NETFramework,Version=v4.7.2 |
| Build tools | MSBuild |

## Cloud Readiness Issues

| Issue Name | Criticality | Story Points | Occurrences |
|------------|-------------|--------------|-------------|
| Windows authentication detected | Mandatory | 3 | [1](#Windows_authentication_detected) |
| Certificate management dependency detected | Potential | 5 | [2](#Certificate_management_dependency_detected) |
| SQL database connection detected | Potential | 3 | [1](#SQL_database_connection_detected) |
| Hardcoded URLs detected | Potential | 1 | [1](#Hardcoded_URLs_detected) |
| Static content detected | Optional | 3 | [1](#Static_content_detected) |
| System.Data.SqlClient dependency detected | Optional | 3 | [1](#System_Data_SqlClient_dependency_detected) |
| Hardcoded sensitive data detected | Optional | 3 | [1](#Hardcoded_sensitive_data_detected) |

### Issue Details

<details id="Windows_authentication_detected">
<summary><b>Windows authentication detected</b> — affected files</summary>

- `src\eShopModernizedMVC\Web.config`

</details>

<details id="Certificate_management_dependency_detected">
<summary><b>Certificate management dependency detected</b> — affected files</summary>

- `src\eShopModernizedMVC\eShopModernizedMVC.csproj`
- `src\eShopModernizedMVC\eShopModernizedMVC.csproj`

</details>

<details id="SQL_database_connection_detected">
<summary><b>SQL database connection detected</b> — affected files</summary>

- `src\eShopModernizedMVC\Web.config`

</details>

<details id="Hardcoded_URLs_detected">
<summary><b>Hardcoded URLs detected</b> — affected files</summary>

- `src\eShopModernizedMVC\App_Start\SqlAccessTokenProvider.cs (line 30)`

</details>

<details id="Static_content_detected">
<summary><b>Static content detected</b> — affected files</summary>

- `src\eShopModernizedMVC\eShopModernizedMVC.csproj`

</details>

<details id="System_Data_SqlClient_dependency_detected">
<summary><b>System.Data.SqlClient dependency detected</b> — affected files</summary>

- `src\eShopModernizedMVC\Web.config`

</details>

<details id="Hardcoded_sensitive_data_detected">
<summary><b>Hardcoded sensitive data detected</b> — affected files</summary>

- `src\eShopModernizedMVC\Web.config`

</details>

## DotNET Upgrade Issues [View Details](scenarios/dotnet-version-upgrade/assessment.md)

| Issue Category | Criticality | Story Points | Occurrences |
|----------------|-------------|--------------|-------------|
| NuGet package functionality is included with framework reference | Mandatory | 1 | [64](#NuGet_package_functionality_is_included_with_framework_reference) |
| NuGet package is incompatible | Mandatory | 1 | [20](#NuGet_package_is_incompatible) |
| Manual redirect conflicts with auto-generated version | Mandatory | 1 | [13](#Manual_redirect_conflicts_with_auto-generated_version) |
| System.Web.Optimization bundling and minification is not supported in .NET Core and should be replaced with actual html tags pointing to content files | Mandatory | 1 | [3](#System_Web_Optimization_bundling_and_minification_is_not_supported_in_NET_Core_and_should_be_replaced_with_actual_html_tags_pointing_to_content_files) |
| Project file needs to be converted to SDK-style | Mandatory | 1 | [1](#Project_file_needs_to_be_converted_to_SDK-style) |
| Project's target framework(s) needs to be changed | Mandatory | 1 | [1](#Project_s_target_framework_s_needs_to_be_changed) |
| Convert application initialization code from Global.asax.cs to .NET Core and clean up Global.asax.cs | Mandatory | 1 | [1](#Convert_application_initialization_code_from_Global_asax_cs_to_NET_Core_and_clean_up_Global_asax_cs) |
| NuGet package upgrade is recommended | Potential | 1 | [22](#NuGet_package_upgrade_is_recommended) |
| Binding redirect forces version downgrade | Potential | 1 | [12](#Binding_redirect_forces_version_downgrade) |
| NuGet package is deprecated | Optional | 1 | [39](#NuGet_package_is_deprecated) |
| NuGet package contains security vulnerability | Optional | 1 | [3](#NuGet_package_contains_security_vulnerability) |

### Issue Details

<details id="NuGet_package_functionality_is_included_with_framework_reference">
<summary><b>NuGet package functionality is included with framework reference</b> — affected files</summary>

- `src\eShopModernizedMVC\eShopModernizedMVC.csproj`

</details>

<details id="NuGet_package_is_incompatible">
<summary><b>NuGet package is incompatible</b> — affected files</summary>

- `src\eShopModernizedMVC\eShopModernizedMVC.csproj`

</details>

<details id="Manual_redirect_conflicts_with_auto-generated_version">
<summary><b>Manual redirect conflicts with auto-generated version</b> — affected files</summary>

- `src\eShopModernizedMVC\Web.config`

</details>

<details id="System_Web_Optimization_bundling_and_minification_is_not_supported_in_NET_Core_and_should_be_replaced_with_actual_html_tags_pointing_to_content_files">
<summary><b>System.Web.Optimization bundling and minification is not supported in .NET Core and should be replaced with actual html tags pointing to content files</b> — affected files</summary>

- `src\eShopModernizedMVC\Views\Catalog\Create.cshtml`
- `src\eShopModernizedMVC\Views\Catalog\Edit.cshtml`
- `src\eShopModernizedMVC\Views\Shared\_Layout.cshtml`

</details>

<details id="Project_file_needs_to_be_converted_to_SDK-style">
<summary><b>Project file needs to be converted to SDK-style</b> — affected files</summary>

- `src\eShopModernizedMVC\eShopModernizedMVC.csproj`

</details>

<details id="Project_s_target_framework_s_needs_to_be_changed">
<summary><b>Project's target framework(s) needs to be changed</b> — affected files</summary>

- `src\eShopModernizedMVC\eShopModernizedMVC.csproj`

</details>

<details id="Convert_application_initialization_code_from_Global_asax_cs_to_NET_Core_and_clean_up_Global_asax_cs">
<summary><b>Convert application initialization code from Global.asax.cs to .NET Core and clean up Global.asax.cs</b> — affected files</summary>

- `src\eShopModernizedMVC\Global.asax.cs`

</details>

<details id="NuGet_package_upgrade_is_recommended">
<summary><b>NuGet package upgrade is recommended</b> — affected files</summary>

- `src\eShopModernizedMVC\eShopModernizedMVC.csproj`

</details>

<details id="Binding_redirect_forces_version_downgrade">
<summary><b>Binding redirect forces version downgrade</b> — affected files</summary>

- `src\eShopModernizedMVC\Web.config`

</details>

<details id="NuGet_package_is_deprecated">
<summary><b>NuGet package is deprecated</b> — affected files</summary>

- `src\eShopModernizedMVC\eShopModernizedMVC.csproj`

</details>

<details id="NuGet_package_contains_security_vulnerability">
<summary><b>NuGet package contains security vulnerability</b> — affected files</summary>

- `src\eShopModernizedMVC\eShopModernizedMVC.csproj`

</details>

## Security Issues

> **Note:** These issues were generated by AI and may contain inaccuracies or incomplete information. Please review carefully.

| Issue Name | Criticality | Story Points | Files |
|------------|-------------|--------------|-------|
| CWE-681: Incorrect Conversion between Numeric Types | Potential | 3 | [3](#CWE-681_Incorrect_Conversion_between_Numeric_Types) |
| CWE-772: Missing Release of Resource after Effective Lifetime | Potential | 3 | [1](#CWE-772_Missing_Release_of_Resource_after_Effective_Lifetime) |
| CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime | Potential | 3 | [1](#CWE-775_Missing_Release_of_File_Descriptor_or_Handle_after_Effective_Lifetime) |
| CWE-778: Insufficient Logging | Potential | 3 | [2](#CWE-778_Insufficient_Logging) |
| CVE-2024-21319: Microsoft ASP.NET Core project templates vulnerable to denial of service | Optional | 1 | [2](#CVE-2024-21319_Microsoft_ASP_NET_Core_project_templates_vulnerable_to_denial_of_service) |
| CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters | Optional | 1 | [1](#CVE-2026-40021_Apache_Log4net_Silent_log_event_loss_in_XmlLayout_and_XmlLayoutSchemaLog4J_due_to_unescaped_XML_1_0_forbidden_characters) |

### Security Issue Details

<details id="CWE-681_Incorrect_Conversion_between_Numeric_Types">
<summary><b>CWE-681: Incorrect Conversion between Numeric Types</b> — affected files</summary>

- `src/eShopModernizedMVC/Models/CatalogItemHiLoGenerator.cs:19-20`
- `src/eShopModernizedMVC/Models/Infrastructure/CatalogDBInitializer.cs:324-328`
- `src/eShopModernizedMVC/Models/Infrastructure/dbo.catalog_hilo.Sequence.sql:4`

</details>

<details id="CWE-772_Missing_Release_of_Resource_after_Effective_Lifetime">
<summary><b>CWE-772: Missing Release of Resource after Effective Lifetime</b> — affected files</summary>

- `src/eShopModernizedMVC/Services/ImageAzureStorage.cs:109-117`

</details>

<details id="CWE-775_Missing_Release_of_File_Descriptor_or_Handle_after_Effective_Lifetime">
<summary><b>CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime</b> — affected files</summary>

- `src/eShopModernizedMVC/Services/ImageAzureStorage.cs:109-117`

</details>

<details id="CWE-778_Insufficient_Logging">
<summary><b>CWE-778: Insufficient Logging</b> — affected files</summary>

- `src/eShopModernizedMVC/App_Start/Startup.Auth.cs:57-61`
- `src/eShopModernizedMVC/Controllers/AccountController.cs:14-35`

</details>

<details id="CVE-2024-21319_Microsoft_ASP_NET_Core_project_templates_vulnerable_to_denial_of_service">
<summary><b>CVE-2024-21319: Microsoft ASP.NET Core project templates vulnerable to denial of service</b> — affected files</summary>

- `src/eShopModernizedMVC/packages.config:51`
- `src/eShopModernizedMVC/packages.config:97`

</details>

<details id="CVE-2026-40021_Apache_Log4net_Silent_log_event_loss_in_XmlLayout_and_XmlLayoutSchemaLog4J_due_to_unescaped_XML_1_0_forbidden_characters">
<summary><b>CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters</b> — affected files</summary>

- `src/eShopModernizedMVC/packages.config:10`

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
