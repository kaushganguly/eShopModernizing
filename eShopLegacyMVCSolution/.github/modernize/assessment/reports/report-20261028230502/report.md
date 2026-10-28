# eShopLegacy.Utilities

## Summary

| Metric | Value |
|--------|-------|
| Total Issues | 45 |
| Mandatory Blockers | 20 |
| Potential Issues | 11 |

## Component Information

| Property | Value |
|----------|-------|
| Language | C# |
| Frameworks | .NETFramework,Version=v4.6.1 |
| Build tools | MSBuild |

## Cloud Readiness Issues

| Issue Name | Criticality | Story Points | Occurrences |
|------------|-------------|--------------|-------------|
| Logging to local or network paths detected | Mandatory | 3 | [1](#Logging_to_local_or_network_paths_detected) |
| Windows authentication detected | Mandatory | 3 | [1](#Windows_authentication_detected) |
| Local or network IO operations detected | Potential | 3 | [13](#Local_or_network_IO_operations_detected) |
| Local application configuration detected | Potential | 1 | [3](#Local_application_configuration_detected) |
| Connection string is detected | Potential | 3 | [2](#Connection_string_is_detected) |
| Local OS environment access detected | Potential | 1 | [1](#Local_OS_environment_access_detected) |
| Session state stored in-proc or in local process is detected | Potential | 3 | [1](#Session_state_stored_in-proc_or_in_local_process_is_detected) |
| Log4Net logging framework detected | Optional | 3 | [8](#Log4Net_logging_framework_detected) |
| Upgrade to newer target framework to get better cloud experience | Optional | 3 | [2](#Upgrade_to_newer_target_framework_to_get_better_cloud_experience) |
| Static content detected | Optional | 3 | [2](#Static_content_detected) |
| Connection strings without configuration builders detected | Optional | 3 | [2](#Connection_strings_without_configuration_builders_detected) |
| System.Data.SqlClient dependency detected | Optional | 3 | [1](#System_Data_SqlClient_dependency_detected) |

### Issue Details

<details id="Logging_to_local_or_network_paths_detected">
<summary><b>Logging to local or network paths detected</b> — affected files</summary>

- `src\eShopLegacyMVC\log4Net.xml`

</details>

<details id="Windows_authentication_detected">
<summary><b>Windows authentication detected</b> — affected files</summary>

- `src\eShopLegacyMVC\Web.config`

</details>

<details id="Local_or_network_IO_operations_detected">
<summary><b>Local or network IO operations detected</b> — affected files</summary>

- `src\eShopLegacyMVC\Controllers\PicController.cs (line 43)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 335)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 345)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 345)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 346)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 135)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 171)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 99)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 145)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 184)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 109)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 299)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 352)`

</details>

<details id="Local_application_configuration_detected">
<summary><b>Local application configuration detected</b> — affected files</summary>

- `eShopPorted\appsettings.json`
- `eShopPorted\appsettings.json`
- `eShopPorted\appsettings.json`

</details>

<details id="Connection_string_is_detected">
<summary><b>Connection string is detected</b> — affected files</summary>

- `src\eShopLegacyMVC\Web.config`
- `eShopPorted\appsettings.json`

</details>

<details id="Local_OS_environment_access_detected">
<summary><b>Local OS environment access detected</b> — affected files</summary>

- `src\eShopLegacyMVC\Global.asax.cs (line 42)`

</details>

<details id="Session_state_stored_in-proc_or_in_local_process_is_detected">
<summary><b>Session state stored in-proc or in local process is detected</b> — affected files</summary>

- `src\eShopLegacyMVC\Web.config`

</details>

<details id="Log4Net_logging_framework_detected">
<summary><b>Log4Net logging framework detected</b> — affected files</summary>

- `src\eShopLegacyMVC\Global.asax.cs (line 22)`
- `src\eShopLegacyMVC\Global.asax.cs (line 49)`
- `src\eShopLegacyMVC\Global.asax.cs (line 51)`
- `src\eShopLegacyMVC\Global.asax.cs (line 22)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 11)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 11)`
- `src\eShopLegacyMVC\Controllers\PicController.cs (line 10)`
- `src\eShopLegacyMVC\Controllers\PicController.cs (line 10)`

</details>

<details id="Upgrade_to_newer_target_framework_to_get_better_cloud_experience">
<summary><b>Upgrade to newer target framework to get better cloud experience</b> — affected files</summary>

- `eShopLegacy.Utilities\eShopLegacy.Utilities.csproj`
- `eShopPorted\eShopPorted.csproj`

</details>

<details id="Static_content_detected">
<summary><b>Static content detected</b> — affected files</summary>

- `src\eShopLegacyMVC\eShopLegacyMVC.csproj`
- `eShopPorted\eShopPorted.csproj`

</details>

<details id="Connection_strings_without_configuration_builders_detected">
<summary><b>Connection strings without configuration builders detected</b> — affected files</summary>

- `src\eShopLegacyMVC\Web.config`
- `src\eShopLegacyMVC\Web.config`

</details>

<details id="System_Data_SqlClient_dependency_detected">
<summary><b>System.Data.SqlClient dependency detected</b> — affected files</summary>

- `src\eShopLegacyMVC\Web.config`

</details>

## DotNET Upgrade Issues [View Details](scenarios/dotnet-version-upgrade/assessment.md)

| Issue Category | Criticality | Story Points | Occurrences |
|----------------|-------------|--------------|-------------|
| Binary incompatible for selected .NET version | Mandatory | 1 | [225](#Binary_incompatible_for_selected_NET_version) |
| NuGet package functionality is included with framework reference | Mandatory | 1 | [16](#NuGet_package_functionality_is_included_with_framework_reference) |
| NuGet package is incompatible | Mandatory | 1 | [6](#NuGet_package_is_incompatible) |
| System.Web.Optimization bundling and minification is not supported in .NET Core and should be replaced with actual html tags pointing to content files | Mandatory | 1 | [4](#System_Web_Optimization_bundling_and_minification_is_not_supported_in_NET_Core_and_should_be_replaced_with_actual_html_tags_pointing_to_content_files) |
| Manual redirect conflicts with auto-generated version | Mandatory | 1 | [4](#Manual_redirect_conflicts_with_auto-generated_version) |
| Project's target framework(s) needs to be changed | Mandatory | 1 | [3](#Project_s_target_framework_s_needs_to_be_changed) |
| Project file needs to be converted to SDK-style | Mandatory | 1 | [2](#Project_file_needs_to_be_converted_to_SDK-style) |
| Convert from Autofac to ASP.NET Core dependency injection | Mandatory | 1 | [2](#Convert_from_Autofac_to_ASP_NET_Core_dependency_injection) |
| Classic EntityFramework initialization needs to be adjusted in .NET Core | Mandatory | 1 | [2](#Classic_EntityFramework_initialization_needs_to_be_adjusted_in_NET_Core) |
| GlobalFilterCollection is not supported in .NET Core and needs to be converted to the corresponding middleware registrations on the application object | Mandatory | 1 | [2](#GlobalFilterCollection_is_not_supported_in_NET_Core_and_needs_to_be_converted_to_the_corresponding_middleware_registrations_on_the_application_object) |
| Routes registration via RouteCollection is not supported in .NET Core and needs to be converted to the route mappings on the application object | Mandatory | 1 | [2](#Routes_registration_via_RouteCollection_is_not_supported_in_NET_Core_and_needs_to_be_converted_to_the_route_mappings_on_the_application_object) |
| Convert application initialization code from Global.asax.cs to .NET Core and clean up Global.asax.cs | Mandatory | 1 | [1](#Convert_application_initialization_code_from_Global_asax_cs_to_NET_Core_and_clean_up_Global_asax_cs) |
| Deprecated Remoting & Serialization | Mandatory | 4 | 0 |
| Legacy Configuration System | Mandatory | 2 | 0 |
| ASP.NET Framework (System.Web) | Mandatory | 4 | 0 |
| Source incompatible for selected .NET version | Potential | 1 | [51](#Source_incompatible_for_selected_NET_version) |
| NuGet package upgrade is recommended | Potential | 1 | [14](#NuGet_package_upgrade_is_recommended) |
| Behavioral change in selected .NET version | Potential | 1 | [3](#Behavioral_change_in_selected_NET_version) |
| Binding redirect forces version downgrade | Potential | 1 | [2](#Binding_redirect_forces_version_downgrade) |
| AutoGenerateBindingRedirects not set and no manual redirects | Potential | 1 | [1](#AutoGenerateBindingRedirects_not_set_and_no_manual_redirects) |
| NuGet package is deprecated | Optional | 1 | [17](#NuGet_package_is_deprecated) |
| NuGet package contains security vulnerability | Optional | 1 | [3](#NuGet_package_contains_security_vulnerability) |

### Issue Details

<details id="Binary_incompatible_for_selected_NET_version">
<summary><b>Binary incompatible for selected .NET version</b> — affected files</summary>

- `src\eShopLegacyMVC\Controllers\WebApi\BrandsController.cs (line 37, col 8)`
- `src\eShopLegacyMVC\Controllers\WebApi\BrandsController.cs (line 37, col 9)`
- `src\eShopLegacyMVC\Controllers\WebApi\BrandsController.cs (line 48, col 12)`
- `src\eShopLegacyMVC\Controllers\WebApi\BrandsController.cs (line 44, col 16)`
- `src\eShopLegacyMVC\Controllers\WebApi\BrandsController.cs (line 28, col 8)`
- `src\eShopLegacyMVC\Controllers\WebApi\BrandsController.cs (line 32, col 31)`
- `src\eShopLegacyMVC\Controllers\WebApi\BrandsController.cs (line 15, col 8)`
- `src\eShopLegacyMVC\Controllers\WebApi\BrandsController.cs (line 11, col 36)`
- `src\eShopLegacyMVC\Controllers\WebApi\FilesController.cs (line 14, col 8)`
- `src\eShopLegacyMVC\Controllers\WebApi\FilesController.cs (line 10, col 35)`
- `src\eShopLegacyMVC\Controllers\Api\CatalogController.cs (line 4, col 5)`
- `src\eShopLegacyMVC\Controllers\Api\CatalogController.cs (line 7, col 8)`
- `src\eShopLegacyMVC\Controllers\Api\CatalogController.cs (line 7, col 9)`
- `src\eShopLegacyMVC\Controllers\Api\CatalogController.cs (line 10, col 12)`
- `src\eShopLegacyMVC\Controllers\Api\CatalogController.cs (line 5, col 38)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 161, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 148, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 131, col 8)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 132, col 9)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 131, col 19)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 131, col 9)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 138, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 113, col 8)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 127, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 123, col 16)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 118, col 16)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 99, col 34)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 97, col 8)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 98, col 9)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 97, col 9)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 109, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 108, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 107, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 105, col 16)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 102, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 76, col 8)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 91, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 90, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 89, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 86, col 16)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 81, col 16)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 61, col 36)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 59, col 8)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 60, col 9)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 59, col 9)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 72, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 71, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 70, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 67, col 16)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 64, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 48, col 8)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 53, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 52, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 51, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 30, col 8)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 44, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 40, col 16)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 35, col 16)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 21, col 8)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 26, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 15, col 8)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 9, col 37)`
- `src\eShopLegacyMVC\Controllers\PicController.cs (line 22, col 8)`
- `src\eShopLegacyMVC\Controllers\PicController.cs (line 23, col 9)`
- `src\eShopLegacyMVC\Controllers\PicController.cs (line 22, col 9)`
- `src\eShopLegacyMVC\Controllers\PicController.cs (line 48, col 12)`
- `src\eShopLegacyMVC\Controllers\PicController.cs (line 45, col 16)`
- `src\eShopLegacyMVC\Controllers\PicController.cs (line 37, col 16)`
- `src\eShopLegacyMVC\Controllers\PicController.cs (line 30, col 16)`
- `src\eShopLegacyMVC\Controllers\PicController.cs (line 16, col 8)`
- `src\eShopLegacyMVC\Controllers\PicController.cs (line 8, col 33)`
- `src\eShopLegacyMVC\Global.asax.cs (line 77, col 12)`
- `src\eShopLegacyMVC\Global.asax.cs (line 73, col 12)`
- `src\eShopLegacyMVC\Global.asax.cs (line 33, col 12)`
- `src\eShopLegacyMVC\Global.asax.cs (line 32, col 12)`
- `src\eShopLegacyMVC\Global.asax.cs (line 31, col 12)`
- `src\eShopLegacyMVC\Global.asax.cs (line 30, col 12)`
- `src\eShopLegacyMVC\Global.asax.cs (line 29, col 12)`
- `src\eShopLegacyMVC\App_Start\WebApiConfig.cs (line 6, col 8)`
- `src\eShopLegacyMVC\App_Start\WebApiConfig.cs (line 11, col 12)`
- `src\eShopLegacyMVC\App_Start\WebApiConfig.cs (line 9, col 12)`
- `src\eShopLegacyMVC\App_Start\RouteConfig.cs (line 7, col 8)`
- `src\eShopLegacyMVC\App_Start\RouteConfig.cs (line 12, col 12)`
- `src\eShopLegacyMVC\App_Start\RouteConfig.cs (line 10, col 12)`
- `src\eShopLegacyMVC\App_Start\RouteConfig.cs (line 9, col 12)`
- `src\eShopLegacyMVC\App_Start\FilterConfig.cs (line 7, col 8)`
- `src\eShopLegacyMVC\App_Start\FilterConfig.cs (line 9, col 12)`
- `src\eShopLegacyMVC\App_Start\BundleConfig.cs (line 7, col 8)`
- `src\eShopLegacyMVC\App_Start\BundleConfig.cs (line 24, col 12)`
- `src\eShopLegacyMVC\App_Start\BundleConfig.cs (line 20, col 12)`
- `src\eShopLegacyMVC\App_Start\BundleConfig.cs (line 17, col 12)`
- `src\eShopLegacyMVC\App_Start\BundleConfig.cs (line 12, col 12)`
- `src\eShopLegacyMVC\App_Start\BundleConfig.cs (line 9, col 12)`
- `eShopPorted\Controllers\PicController.cs (line 22, col 8)`
- `eShopPorted\Controllers\PicController.cs (line 23, col 9)`
- `eShopPorted\Controllers\PicController.cs (line 22, col 9)`
- `eShopPorted\Controllers\PicController.cs (line 49, col 12)`
- `eShopPorted\Controllers\PicController.cs (line 46, col 16)`
- `eShopPorted\Controllers\PicController.cs (line 30, col 16)`
- `eShopPorted\Controllers\PicController.cs (line 16, col 8)`
- `eShopPorted\Controllers\PicController.cs (line 8, col 33)`
- `eShopPorted\Startup.cs (line 27, col 12)`

</details>

<details id="NuGet_package_functionality_is_included_with_framework_reference">
<summary><b>NuGet package functionality is included with framework reference</b> — affected files</summary>

- `src\eShopLegacyMVC\eShopLegacyMVC.csproj`
- `eShopPorted\eShopPorted.csproj`

</details>

<details id="NuGet_package_is_incompatible">
<summary><b>NuGet package is incompatible</b> — affected files</summary>

- `src\eShopLegacyMVC\eShopLegacyMVC.csproj`
- `eShopPorted\eShopPorted.csproj`

</details>

<details id="System_Web_Optimization_bundling_and_minification_is_not_supported_in_NET_Core_and_should_be_replaced_with_actual_html_tags_pointing_to_content_files">
<summary><b>System.Web.Optimization bundling and minification is not supported in .NET Core and should be replaced with actual html tags pointing to content files</b> — affected files</summary>

- `src\eShopLegacyMVC\Views\Catalog\Create.cshtml`
- `src\eShopLegacyMVC\Views\Catalog\Edit.cshtml`
- `src\eShopLegacyMVC\Views\Shared\_Layout.cshtml`
- `src\eShopLegacyMVC\App_Start\BundleConfig.cs`

</details>

<details id="Manual_redirect_conflicts_with_auto-generated_version">
<summary><b>Manual redirect conflicts with auto-generated version</b> — affected files</summary>

- `src\eShopLegacyMVC\Web.config`

</details>

<details id="Project_s_target_framework_s_needs_to_be_changed">
<summary><b>Project's target framework(s) needs to be changed</b> — affected files</summary>

- `eShopLegacy.Utilities\eShopLegacy.Utilities.csproj`
- `src\eShopLegacyMVC\eShopLegacyMVC.csproj`
- `eShopPorted\eShopPorted.csproj`

</details>

<details id="Project_file_needs_to_be_converted_to_SDK-style">
<summary><b>Project file needs to be converted to SDK-style</b> — affected files</summary>

- `eShopLegacy.Utilities\eShopLegacy.Utilities.csproj`
- `src\eShopLegacyMVC\eShopLegacyMVC.csproj`

</details>

<details id="Convert_from_Autofac_to_ASP_NET_Core_dependency_injection">
<summary><b>Convert from Autofac to ASP.NET Core dependency injection</b> — affected files</summary>

- `src\eShopLegacyMVC\Global.asax.cs`
- `src\eShopLegacyMVC\Modules\ApplicationModule.cs`

</details>

<details id="Classic_EntityFramework_initialization_needs_to_be_adjusted_in_NET_Core">
<summary><b>Classic EntityFramework initialization needs to be adjusted in .NET Core</b> — affected files</summary>

- `src\eShopLegacyMVC\Models\CatalogDBContext.cs`
- `src\eShopLegacyMVC\Global.asax.cs`

</details>

<details id="GlobalFilterCollection_is_not_supported_in_NET_Core_and_needs_to_be_converted_to_the_corresponding_middleware_registrations_on_the_application_object">
<summary><b>GlobalFilterCollection is not supported in .NET Core and needs to be converted to the corresponding middleware registrations on the application object</b> — affected files</summary>

- `src\eShopLegacyMVC\Global.asax.cs`
- `src\eShopLegacyMVC\App_Start\FilterConfig.cs`

</details>

<details id="Routes_registration_via_RouteCollection_is_not_supported_in_NET_Core_and_needs_to_be_converted_to_the_route_mappings_on_the_application_object">
<summary><b>Routes registration via RouteCollection is not supported in .NET Core and needs to be converted to the route mappings on the application object</b> — affected files</summary>

- `src\eShopLegacyMVC\Global.asax.cs`
- `src\eShopLegacyMVC\App_Start\RouteConfig.cs`

</details>

<details id="Convert_application_initialization_code_from_Global_asax_cs_to_NET_Core_and_clean_up_Global_asax_cs">
<summary><b>Convert application initialization code from Global.asax.cs to .NET Core and clean up Global.asax.cs</b> — affected files</summary>

- `src\eShopLegacyMVC\Global.asax.cs`

</details>

<details id="Source_incompatible_for_selected_NET_version">
<summary><b>Source incompatible for selected .NET version</b> — affected files</summary>

- `eShopLegacy.Utilities\Serializing.cs (line 18, col 12)`
- `eShopLegacy.Utilities\Serializing.cs (line 10, col 12)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 344, col 12)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 168, col 12)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 132, col 12)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 96, col 12)`
- `src\eShopLegacyMVC\Models\Infrastructure\CatalogDBInitializer.cs (line 28, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 161, col 12)`
- `src\eShopLegacyMVC\Controllers\PicController.cs (line 37, col 16)`
- `src\eShopLegacyMVC\Global.asax.cs (line 111, col 12)`
- `src\eShopLegacyMVC\Global.asax.cs (line 84, col 12)`
- `src\eShopLegacyMVC\Global.asax.cs (line 67, col 12)`
- `src\eShopLegacyMVC\Global.asax.cs (line 43, col 12)`
- `src\eShopLegacyMVC\Global.asax.cs (line 42, col 12)`
- `src\eShopLegacyMVC\Global.asax.cs (line 20, col 34)`
- `eShopPorted\Startup.cs (line 48, col 8)`
- `eShopPorted\Program.cs (line 13, col 12)`
- `eShopPorted\Program.cs (line 9, col 12)`

</details>

<details id="NuGet_package_upgrade_is_recommended">
<summary><b>NuGet package upgrade is recommended</b> — affected files</summary>

- `src\eShopLegacyMVC\eShopLegacyMVC.csproj`
- `eShopPorted\eShopPorted.csproj`

</details>

<details id="Behavioral_change_in_selected_NET_version">
<summary><b>Behavioral change in selected .NET version</b> — affected files</summary>

- `src\eShopLegacyMVC\Controllers\WebApi\FilesController.cs (line 29, col 12)`
- `src\eShopLegacyMVC\Controllers\CatalogController.cs (line 161, col 12)`
- `eShopPorted\Startup.cs (line 56, col 16)`

</details>

<details id="Binding_redirect_forces_version_downgrade">
<summary><b>Binding redirect forces version downgrade</b> — affected files</summary>

- `src\eShopLegacyMVC\Web.config`

</details>

<details id="AutoGenerateBindingRedirects_not_set_and_no_manual_redirects">
<summary><b>AutoGenerateBindingRedirects not set and no manual redirects</b> — affected files</summary>

- `eShopLegacy.Utilities\eShopLegacy.Utilities.csproj`

</details>

<details id="NuGet_package_is_deprecated">
<summary><b>NuGet package is deprecated</b> — affected files</summary>

- `src\eShopLegacyMVC\eShopLegacyMVC.csproj`
- `eShopPorted\eShopPorted.csproj`

</details>

<details id="NuGet_package_contains_security_vulnerability">
<summary><b>NuGet package contains security vulnerability</b> — affected files</summary>

- `src\eShopLegacyMVC\eShopLegacyMVC.csproj`
- `eShopPorted\eShopPorted.csproj`

</details>

## Security Issues

> **Note:** These issues were generated by AI and may contain inaccuracies or incomplete information. Please review carefully.

| Issue Name | Criticality | Story Points | Files |
|------------|-------------|--------------|-------|
| CVE-2025-55315: Microsoft Security Advisory CVE-2025-55315: .NET Security Feature Bypass Vulnerability | Mandatory | 1 | [1](#CVE-2025-55315_Microsoft_Security_Advisory_CVE-2025-55315_NET_Security_Feature_Bypass_Vulnerability) |
| CVE-2024-0056: Microsoft.Data.SqlClient and System.Data.SqlClient vulnerable to SQL Data Provider Security Feature Bypass | Mandatory | 1 | [1](#CVE-2024-0056_Microsoft_Data_SqlClient_and_System_Data_SqlClient_vulnerable_to_SQL_Data_Provider_Security_Feature_Bypass) |
| CVE-2021-26701: .NET Core Remote Code Execution Vulnerability | Mandatory | 1 | [1](#CVE-2021-26701_NET_Core_Remote_Code_Execution_Vulnerability) |
| CWE-99: Improper Control of Resource Identifiers ('Resource Injection') | Potential | 3 | [3](#CWE-99_Improper_Control_of_Resource_Identifiers_Resource_Injection) |
| CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal') | Optional | 8 | [4](#CWE-22_Improper_Limitation_of_a_Pathname_to_a_Restricted_Directory_Path_Traversal) |
| CWE-23: Relative Path Traversal | Optional | 5 | [2](#CWE-23_Relative_Path_Traversal) |
| CWE-36: Absolute Path Traversal | Optional | 5 | [2](#CWE-36_Absolute_Path_Traversal) |
| CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters | Optional | 1 | [2](#CVE-2026-40021_Apache_Log4net_Silent_log_event_loss_in_XmlLayout_and_XmlLayoutSchemaLog4J_due_to_unescaped_XML_1_0_forbidden_characters) |
| CVE-2022-41064: .NET Information Disclosure Vulnerability | Optional | 1 | [1](#CVE-2022-41064_NET_Information_Disclosure_Vulnerability) |
| CVE-2019-1075: Open redirect in ASP.NET Core | Optional | 1 | [1](#CVE-2019-1075_Open_redirect_in_ASP_NET_Core) |
| CVE-2022-34716: .NET Information Disclosure Vulnerability | Optional | 1 | [1](#CVE-2022-34716_NET_Information_Disclosure_Vulnerability) |

### Security Issue Details

<details id="CVE-2025-55315_Microsoft_Security_Advisory_CVE-2025-55315_NET_Security_Feature_Bypass_Vulnerability">
<summary><b>CVE-2025-55315: Microsoft Security Advisory CVE-2025-55315: .NET Security Feature Bypass Vulnerability</b> — affected files</summary>

- `eShopLegacyMVCSolution/eShopPorted/eShopPorted.csproj:7`

</details>

<details id="CVE-2024-0056_Microsoft_Data_SqlClient_and_System_Data_SqlClient_vulnerable_to_SQL_Data_Provider_Security_Feature_Bypass">
<summary><b>CVE-2024-0056: Microsoft.Data.SqlClient and System.Data.SqlClient vulnerable to SQL Data Provider Security Feature Bypass</b> — affected files</summary>

- `eShopLegacyMVCSolution/eShopPorted/eShopPorted.csproj:22`

</details>

<details id="CVE-2021-26701_NET_Core_Remote_Code_Execution_Vulnerability">
<summary><b>CVE-2021-26701: .NET Core Remote Code Execution Vulnerability</b> — affected files</summary>

- `eShopLegacyMVCSolution/eShopPorted/eShopPorted.csproj:17`

</details>

<details id="CWE-99_Improper_Control_of_Resource_Identifiers_Resource_Injection">
<summary><b>CWE-99: Improper Control of Resource Identifiers ('Resource Injection')</b> — affected files</summary>

- `eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/CatalogController.cs:62`
- `eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/CatalogController.cs:100`
- `eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/PicController.cs:39`

</details>

<details id="CWE-22_Improper_Limitation_of_a_Pathname_to_a_Restricted_Directory_Path_Traversal">
<summary><b>CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')</b> — affected files</summary>

- `eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/CatalogController.cs:62`
- `eShopLegacyMVCSolution/src/eShopLegacyMVC/Models/CatalogItem.cs:28`
- `eShopLegacyMVCSolution/src/eShopLegacyMVC/Services/CatalogService.cs:51`
- `eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/PicController.cs:38`

</details>

<details id="CWE-23_Relative_Path_Traversal">
<summary><b>CWE-23: Relative Path Traversal</b> — affected files</summary>

- `eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/CatalogController.cs:62`
- `eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/PicController.cs:39`

</details>

<details id="CWE-36_Absolute_Path_Traversal">
<summary><b>CWE-36: Absolute Path Traversal</b> — affected files</summary>

- `eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/CatalogController.cs:100`
- `eShopLegacyMVCSolution/src/eShopLegacyMVC/Controllers/PicController.cs:39`

</details>

<details id="CVE-2026-40021_Apache_Log4net_Silent_log_event_loss_in_XmlLayout_and_XmlLayoutSchemaLog4J_due_to_unescaped_XML_1_0_forbidden_characters">
<summary><b>CVE-2026-40021: Apache Log4net: Silent log event loss in XmlLayout and XmlLayoutSchemaLog4J due to unescaped XML 1.0 forbidden characters</b> — affected files</summary>

- `eShopLegacyMVCSolution/src/eShopLegacyMVC/eShopLegacyMVC.csproj:242`
- `eShopLegacyMVCSolution/eShopPorted/eShopPorted.csproj:13`

</details>

<details id="CVE-2022-41064_NET_Information_Disclosure_Vulnerability">
<summary><b>CVE-2022-41064: .NET Information Disclosure Vulnerability</b> — affected files</summary>

- `eShopLegacyMVCSolution/eShopPorted/eShopPorted.csproj:22`

</details>

<details id="CVE-2019-1075_Open_redirect_in_ASP_NET_Core">
<summary><b>CVE-2019-1075: Open redirect in ASP.NET Core</b> — affected files</summary>

- `eShopLegacyMVCSolution/eShopPorted/eShopPorted.csproj:7`

</details>

<details id="CVE-2022-34716_NET_Information_Disclosure_Vulnerability">
<summary><b>CVE-2022-34716: .NET Information Disclosure Vulnerability</b> — affected files</summary>

- `eShopLegacyMVCSolution/eShopPorted/eShopPorted.csproj:16`

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
