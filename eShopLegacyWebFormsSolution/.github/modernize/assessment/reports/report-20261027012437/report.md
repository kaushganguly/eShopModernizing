# eShopLegacyWebForms

## Summary

| Metric | Value |
|--------|-------|
| Total Issues | 25 |
| Mandatory Blockers | 9 |
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
| Logging to local or network paths detected | Mandatory | 3 | [1](#Logging_to_local_or_network_paths_detected) |
| Windows authentication detected | Mandatory | 3 | [1](#Windows_authentication_detected) |
| Connection string is detected | Potential | 3 | [1](#Connection_string_is_detected) |
| Session state stored in-proc or in local process is detected | Potential | 3 | [1](#Session_state_stored_in-proc_or_in_local_process_is_detected) |
| Connection strings without configuration builders detected | Optional | 3 | [2](#Connection_strings_without_configuration_builders_detected) |
| Static content detected | Optional | 3 | [1](#Static_content_detected) |
| System.Data.SqlClient dependency detected | Optional | 3 | [1](#System_Data_SqlClient_dependency_detected) |

### Issue Details

<details id="Logging_to_local_or_network_paths_detected">
<summary><b>Logging to local or network paths detected</b> — affected files</summary>

- `src\eShopLegacyWebForms\log4Net.xml`

</details>

<details id="Windows_authentication_detected">
<summary><b>Windows authentication detected</b> — affected files</summary>

- `src\eShopLegacyWebForms\Web.config`

</details>

<details id="Connection_string_is_detected">
<summary><b>Connection string is detected</b> — affected files</summary>

- `src\eShopLegacyWebForms\Web.config`

</details>

<details id="Session_state_stored_in-proc_or_in_local_process_is_detected">
<summary><b>Session state stored in-proc or in local process is detected</b> — affected files</summary>

- `src\eShopLegacyWebForms\Web.config`

</details>

<details id="Connection_strings_without_configuration_builders_detected">
<summary><b>Connection strings without configuration builders detected</b> — affected files</summary>

- `src\eShopLegacyWebForms\Web.config`
- `src\eShopLegacyWebForms\Web.config`

</details>

<details id="Static_content_detected">
<summary><b>Static content detected</b> — affected files</summary>

- `src\eShopLegacyWebForms\eShopLegacyWebForms.csproj`

</details>

<details id="System_Data_SqlClient_dependency_detected">
<summary><b>System.Data.SqlClient dependency detected</b> — affected files</summary>

- `src\eShopLegacyWebForms\Web.config`

</details>

## DotNET Upgrade Issues

| Issue Category | Criticality | Story Points | Occurrences |
|----------------|-------------|--------------|-------------|
| NuGet package is incompatible | Mandatory | 1 | [12](#NuGet_package_is_incompatible) |
| NuGet package functionality is included with framework reference | Mandatory | 1 | [10](#NuGet_package_functionality_is_included_with_framework_reference) |
| Manual redirect conflicts with auto-generated version | Mandatory | 1 | [3](#Manual_redirect_conflicts_with_auto-generated_version) |
| Project file needs to be converted to SDK-style | Mandatory | 1 | [1](#Project_file_needs_to_be_converted_to_SDK-style) |
| Project's target framework(s) needs to be changed | Mandatory | 1 | [1](#Project_s_target_framework_s_needs_to_be_changed) |
| Convert application initialization code from Global.asax.cs to .NET Core and clean up Global.asax.cs | Mandatory | 1 | [1](#Convert_application_initialization_code_from_Global_asax_cs_to_NET_Core_and_clean_up_Global_asax_cs) |
| NuGet package upgrade is recommended | Potential | 1 | [8](#NuGet_package_upgrade_is_recommended) |
| Binding redirect forces version downgrade | Potential | 1 | [2](#Binding_redirect_forces_version_downgrade) |
| NuGet package is deprecated | Optional | 1 | [9](#NuGet_package_is_deprecated) |
| NuGet package contains security vulnerability | Optional | 1 | [2](#NuGet_package_contains_security_vulnerability) |

### Issue Details

<details id="NuGet_package_is_incompatible">
<summary><b>NuGet package is incompatible</b> — affected files</summary>

- `src\eShopLegacyWebForms\eShopLegacyWebForms.csproj`

</details>

<details id="NuGet_package_functionality_is_included_with_framework_reference">
<summary><b>NuGet package functionality is included with framework reference</b> — affected files</summary>

- `src\eShopLegacyWebForms\eShopLegacyWebForms.csproj`

</details>

<details id="Manual_redirect_conflicts_with_auto-generated_version">
<summary><b>Manual redirect conflicts with auto-generated version</b> — affected files</summary>

- `src\eShopLegacyWebForms\Web.config`

</details>

<details id="Project_file_needs_to_be_converted_to_SDK-style">
<summary><b>Project file needs to be converted to SDK-style</b> — affected files</summary>

- `src\eShopLegacyWebForms\eShopLegacyWebForms.csproj`

</details>

<details id="Project_s_target_framework_s_needs_to_be_changed">
<summary><b>Project's target framework(s) needs to be changed</b> — affected files</summary>

- `src\eShopLegacyWebForms\eShopLegacyWebForms.csproj`

</details>

<details id="Convert_application_initialization_code_from_Global_asax_cs_to_NET_Core_and_clean_up_Global_asax_cs">
<summary><b>Convert application initialization code from Global.asax.cs to .NET Core and clean up Global.asax.cs</b> — affected files</summary>

- `src\eShopLegacyWebForms\Global.asax.cs`

</details>

<details id="NuGet_package_upgrade_is_recommended">
<summary><b>NuGet package upgrade is recommended</b> — affected files</summary>

- `src\eShopLegacyWebForms\eShopLegacyWebForms.csproj`

</details>

<details id="Binding_redirect_forces_version_downgrade">
<summary><b>Binding redirect forces version downgrade</b> — affected files</summary>

- `src\eShopLegacyWebForms\Web.config`

</details>

<details id="NuGet_package_is_deprecated">
<summary><b>NuGet package is deprecated</b> — affected files</summary>

- `src\eShopLegacyWebForms\eShopLegacyWebForms.csproj`

</details>

<details id="NuGet_package_contains_security_vulnerability">
<summary><b>NuGet package contains security vulnerability</b> — affected files</summary>

- `src\eShopLegacyWebForms\eShopLegacyWebForms.csproj`

</details>

## Security Issues

> **Note:** These issues were generated by AI and may contain inaccuracies or incomplete information. Please review carefully.

| Issue Name | Criticality | Story Points | Files |
|------------|-------------|--------------|-------|
| CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json | Mandatory | 1 | [1](#CVE-2024-21907_Improper_Handling_of_Exceptional_Conditions_in_Newtonsoft_Json) |
| CWE-662: Improper Synchronization | Potential | 8 | [2](#CWE-662_Improper_Synchronization) |
| CWE-820: Missing Synchronization | Potential | 8 | [2](#CWE-820_Missing_Synchronization) |
| CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context | Potential | 5 | [2](#CWE-543_Use_of_Singleton_Pattern_Without_Synchronization_in_a_Multithreaded_Context) |
| CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context | Potential | 5 | [2](#CWE-567_Unsynchronized_Access_to_Shared_Data_in_a_Multithreaded_Context) |
| CWE-778: Insufficient Logging | Potential | 3 | [3](#CWE-778_Insufficient_Logging) |
| CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting') | Optional | 8 | [3](#CWE-79_Improper_Neutralization_of_Input_During_Web_Page_Generation_Cross-site_Scripting) |
| CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters | Optional | 1 | [1](#CVE-2026-40021_Apache_Log4net_Silent_log_event_loss_in_XmlLayout_and_XmlLayoutSchemaLog4J_due_to_unescaped_XML_1_0_forbidden_characters) |

### Security Issue Details

<details id="CVE-2024-21907_Improper_Handling_of_Exceptional_Conditions_in_Newtonsoft_Json">
<summary><b>CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json</b> — affected files</summary>

- `src/eShopLegacyWebForms/packages.config:31`

</details>

<details id="CWE-662_Improper_Synchronization">
<summary><b>CWE-662: Improper Synchronization</b> — affected files</summary>

- `src/eShopLegacyWebForms/Modules/ApplicationModule.cs:18-22`
- `src/eShopLegacyWebForms/Services/CatalogServiceMock.cs:12-16,48-64`

</details>

<details id="CWE-820_Missing_Synchronization">
<summary><b>CWE-820: Missing Synchronization</b> — affected files</summary>

- `src/eShopLegacyWebForms/Modules/ApplicationModule.cs:18-22`
- `src/eShopLegacyWebForms/Services/CatalogServiceMock.cs:12-16,48-64`

</details>

<details id="CWE-543_Use_of_Singleton_Pattern_Without_Synchronization_in_a_Multithreaded_Context">
<summary><b>CWE-543: Use of Singleton Pattern Without Synchronization in a Multithreaded Context</b> — affected files</summary>

- `src/eShopLegacyWebForms/Modules/ApplicationModule.cs:18-22`
- `src/eShopLegacyWebForms/Services/CatalogServiceMock.cs:12-16,48-64`

</details>

<details id="CWE-567_Unsynchronized_Access_to_Shared_Data_in_a_Multithreaded_Context">
<summary><b>CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context</b> — affected files</summary>

- `src/eShopLegacyWebForms/Modules/ApplicationModule.cs:18-22`
- `src/eShopLegacyWebForms/Services/CatalogServiceMock.cs:12-16,48-64`

</details>

<details id="CWE-778_Insufficient_Logging">
<summary><b>CWE-778: Insufficient Logging</b> — affected files</summary>

- `src/eShopLegacyWebForms/Catalog/Create.aspx.cs:17,30-46`
- `src/eShopLegacyWebForms/Catalog/Edit.aspx.cs:24,47-67`
- `src/eShopLegacyWebForms/Catalog/Delete.aspx.cs:21,27-33`

</details>

<details id="CWE-79_Improper_Neutralization_of_Input_During_Web_Page_Generation_Cross-site_Scripting">
<summary><b>CWE-79: Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')</b> — affected files</summary>

- `src/eShopLegacyWebForms/Catalog/Create.aspx:1`
- `src/eShopLegacyWebForms/Catalog/Create.aspx.cs:36-46`
- `src/eShopLegacyWebForms/Catalog/Details.aspx:14,21`

</details>

<details id="CVE-2026-40021_Apache_Log4net_Silent_log_event_loss_in_XmlLayout_and_XmlLayoutSchemaLog4J_due_to_unescaped_XML_1_0_forbidden_characters">
<summary><b>CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters</b> — affected files</summary>

- `src/eShopLegacyWebForms/packages.config:11`

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
