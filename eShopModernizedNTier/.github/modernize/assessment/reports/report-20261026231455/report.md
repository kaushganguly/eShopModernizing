# eShopModernizedNTier

[Assessment overview and navigation](facts/assessment-overview.md) · [Security report and coverage](security/security-assessment.md)

> **Scope:** Only `eShopModernizedNTier`. Component information below reflects the first project selected by the renderer; the core scan includes three project files and the solution-based .NET 10 scan includes two. Security analysis is static: dependency resolution failed on missing legacy web targets, so advisory matches use manifests and potentially stale resolved assets. The 17 security findings are rule matches, including overlapping CWE classifications, not verified deployed exploits.

## Summary

| Metric | Value |
|--------|-------|
| Total Issues | 36 |
| Mandatory Blockers | 13 |
| Potential Issues | 12 |

## Component Information

| Property | Value |
|----------|-------|
| Language | C# |
| Frameworks | .NETFramework,Version=v4.6.1 |
| Build tools | MSBuild |

## Cloud Readiness Issues

| Issue Name | Criticality | Story Points | Occurrences |
|------------|-------------|--------------|-------------|
| Hardcoded URLs detected | Potential | 1 | [92](#Hardcoded_URLs_detected) |
| Local or network IO operations detected | Potential | 3 | [2](#Local_or_network_IO_operations_detected) |
| Local OS environment access detected | Potential | 1 | [2](#Local_OS_environment_access_detected) |
| SQL database connection detected | Potential | 3 | [1](#SQL_database_connection_detected) |
| Connection strings without configuration builders detected | Optional | 3 | [2](#Connection_strings_without_configuration_builders_detected) |
| Static content detected | Optional | 3 | [2](#Static_content_detected) |
| Upgrade to newer target framework to get better cloud experience | Optional | 3 | [1](#Upgrade_to_newer_target_framework_to_get_better_cloud_experience) |
| System.Data.SqlClient dependency detected | Optional | 3 | [1](#System_Data_SqlClient_dependency_detected) |

### Issue Details

<details id="Hardcoded_URLs_detected">
<summary><b>Hardcoded URLs detected</b> — affected files</summary>

- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 158)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 16)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 195)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 299)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 232)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 399)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 402)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 399)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 402)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 405)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 408)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 405)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 408)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 369)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 372)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 369)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 372)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 393)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 396)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 393)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 396)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 375)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 378)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 375)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 378)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 381)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 384)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 381)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 384)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 387)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 390)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 387)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 390)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 423)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 426)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 423)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 426)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 417)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 420)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 417)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 420)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 411)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 414)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 411)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 414)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 608)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 158)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 16)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 195)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 299)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 232)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 399)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 402)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 399)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 402)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 405)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 408)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 405)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 408)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 369)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 372)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 369)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 372)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 393)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 396)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 393)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 396)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 375)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 378)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 375)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 378)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 381)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 384)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 381)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 384)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 387)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 390)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 387)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 390)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 423)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 426)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 423)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 426)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 417)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 420)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 417)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 420)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 411)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 414)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 411)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 414)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 608)`

</details>

<details id="Local_or_network_IO_operations_detected">
<summary><b>Local or network IO operations detected</b> — affected files</summary>

- `src\eShopWinForms\Views\CatalogView.cs (line 57)`
- `src\eShopWinForms\Views\CatalogView.cs (line 57)`

</details>

<details id="Local_OS_environment_access_detected">
<summary><b>Local OS environment access detected</b> — affected files</summary>

- `src\eShopWinForms\Views\CatalogView.cs (line 56)`
- `src\eShopWinForms\Views\CatalogView.cs (line 56)`

</details>

<details id="SQL_database_connection_detected">
<summary><b>SQL database connection detected</b> — affected files</summary>

- `src\eShopWCFService\Web.config`

</details>

<details id="Connection_strings_without_configuration_builders_detected">
<summary><b>Connection strings without configuration builders detected</b> — affected files</summary>

- `src\eShopWCFService\Web.config`
- `src\eShopWCFService\Web.config`

</details>

<details id="Static_content_detected">
<summary><b>Static content detected</b> — affected files</summary>

- `src\eShopWinForms\eShopWinForms.csproj`
- `src\eShopWinForms\eShopWinForms.fx.csproj`

</details>

<details id="Upgrade_to_newer_target_framework_to_get_better_cloud_experience">
<summary><b>Upgrade to newer target framework to get better cloud experience</b> — affected files</summary>

- `src\eShopWCFService\eShopWCFService.csproj`

</details>

<details id="System_Data_SqlClient_dependency_detected">
<summary><b>System.Data.SqlClient dependency detected</b> — affected files</summary>

- `src\eShopWCFService\Web.config`

</details>

## DotNET Upgrade Issues [View Details](scenarios/dotnet-version-upgrade/assessment.md)

| Issue Category | Criticality | Story Points | Occurrences |
|----------------|-------------|--------------|-------------|
| Binary incompatible for selected .NET version | Mandatory | 1 | [1562](#Binary_incompatible_for_selected_NET_version) |
| Project's target framework(s) needs to be changed | Mandatory | 1 | [2](#Project_s_target_framework_s_needs_to_be_changed) |
| Project file needs to be converted to SDK-style | Mandatory | 1 | [1](#Project_file_needs_to_be_converted_to_SDK-style) |
| WCF Client APIs | Mandatory | 1 | 0 |
| Legacy Configuration System | Mandatory | 2 | 0 |
| GDI+ / System.Drawing | Mandatory | 1 | 0 |
| Windows Forms Legacy Controls | Mandatory | 3 | 0 |
| Windows Forms | Mandatory | 1 | 0 |
| Source incompatible for selected .NET version | Potential | 1 | [111](#Source_incompatible_for_selected_NET_version) |
| NuGet package upgrade is recommended | Potential | 1 | [6](#NuGet_package_upgrade_is_recommended) |
| NuGet package is deprecated | Optional | 1 | [6](#NuGet_package_is_deprecated) |

### Issue Details

<details id="Binary_incompatible_for_selected_NET_version">
<summary><b>Binary incompatible for selected .NET version</b> — affected files</summary>

- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 616, col 50)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 615, col 50)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 614, col 50)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 613, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 612, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 611, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 610, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 609, col 61)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 608, col 43)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 607, col 48)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 606, col 46)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 605, col 45)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 604, col 43)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 603, col 43)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 602, col 43)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 601, col 45)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 600, col 44)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 599, col 46)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 598, col 51)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 597, col 50)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 596, col 46)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 595, col 46)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 594, col 46)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 593, col 44)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 592, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 591, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 590, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 589, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 588, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 587, col 45)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 586, col 54)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 585, col 48)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 584, col 48)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 583, col 51)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 582, col 45)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 581, col 45)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 580, col 48)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 579, col 43)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 578, col 43)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 577, col 46)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 576, col 46)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 575, col 54)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 574, col 54)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 573, col 51)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 572, col 51)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 571, col 51)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 570, col 51)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 565, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 564, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 563, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 562, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 561, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 560, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 559, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 558, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 557, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 556, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 555, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 554, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 553, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 552, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 551, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 550, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 549, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 548, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 547, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 546, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 545, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 544, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 543, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 542, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 541, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 540, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 539, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 538, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 537, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 536, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 535, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 534, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 533, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 532, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 528, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 527, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 526, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 525, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 524, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 523, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 522, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 521, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 519, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 515, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 514, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 513, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 512, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 511, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 510, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 509, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 508, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 504, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 503, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 502, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 501, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 500, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 499, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 498, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 497, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 496, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 492, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 491, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 490, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 489, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 488, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 487, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 486, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 485, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 481, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 480, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 479, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 478, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 477, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 473, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 472, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 471, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 470, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 469, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 468, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 467, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 463, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 462, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 461, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 460, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 459, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 458, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 457, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 453, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 452, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 451, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 450, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 449, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 448, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 447, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 443, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 442, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 441, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 440, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 439, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 435, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 434, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 433, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 432, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 431, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 430, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 429, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 428, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 424, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 423, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 422, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 421, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 420, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 419, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 415, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 414, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 413, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 412, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 411, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 410, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 409, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 408, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 407, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 406, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 405, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 404, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 403, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 402, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 401, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 400, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 396, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 392, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 388, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 384, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 383, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 382, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 381, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 380, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 379, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 378, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 374, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 370, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 369, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 368, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 367, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 366, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 365, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 364, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 363, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 362, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 358, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 357, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 356, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 355, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 354, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 353, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 352, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 351, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 347, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 346, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 345, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 344, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 343, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 342, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 341, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 336, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 335, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 334, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 333, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 332, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 331, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 330, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 329, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 328, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 327, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 323, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 322, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 321, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 320, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 319, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 318, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 314, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 313, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 312, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 311, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 310, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 309, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 308, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 307, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 306, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 305, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 304, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 303, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 302, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 301, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 300, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 299, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 295, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 294, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 293, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 292, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 291, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 290, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 289, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 288, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 287, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 286, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 282, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 281, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 280, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 279, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 275, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 274, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 273, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 272, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 268, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 267, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 266, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 265, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 261, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 260, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 259, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 258, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 254, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 253, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 252, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 251, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 250, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 246, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 245, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 244, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 243, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 242, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 241, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 240, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 239, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 233, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 232, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 231, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 230, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 226, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 225, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 224, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 223, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 222, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 221, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 220, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 219, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 218, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 214, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 213, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 212, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 211, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 210, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 209, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 208, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 207, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 203, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 202, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 201, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 200, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 199, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 198, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 197, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 196, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 192, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 191, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 190, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 189, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 188, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 187, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 186, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 185, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 184, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 180, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 179, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 178, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 177, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 176, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 175, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 174, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 173, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 172, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 171, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 170, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 169, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 168, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 167, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 166, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 165, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 164, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 163, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 159, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 158, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 157, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 156, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 155, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 154, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 153, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 152, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 151, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 150, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 149, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 148, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 147, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 146, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 145, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 144, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 143, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 142, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 141, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 140, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 139, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 138, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 137, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 136, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 135, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 134, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 133, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 132, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 131, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 130, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 129, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 128, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 127, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 126, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 125, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 124, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 123, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 119, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 118, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 117, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 116, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 115, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 114, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 113, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 112, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 111, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 107, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 106, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 105, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 104, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 103, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 102, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 101, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 100, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 99, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 98, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 95, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 91, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 90, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 89, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 88, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 87, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 86, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 85, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 84, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 83, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 82, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 81, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 80, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 79, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 78, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 77, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 76, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 75, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 74, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 73, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 72, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 71, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 70, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 69, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 68, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 67, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 66, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 65, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 64, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 63, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 62, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 61, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 60, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 59, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 58, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 57, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 56, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 55, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 54, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 53, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 52, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 51, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 50, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 49, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 48, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 47, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 46, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 45, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 44, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 43, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 42, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 41, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 40, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 39, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 38, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 37, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 36, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 35, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 34, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 33, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 32, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 31, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 19, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 194, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 193, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 192, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 190, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 181, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 180, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 179, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 177, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 176, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 175, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 174, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 163, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 162, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 161, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 158, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 146, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 144, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 132, col 16)`
- `src\eShopWinForms\Views\CatalogView.cs (line 131, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 129, col 16)`
- `src\eShopWinForms\Views\CatalogView.cs (line 128, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 114, col 16)`
- `src\eShopWinForms\Views\CatalogView.cs (line 113, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 111, col 16)`
- `src\eShopWinForms\Views\CatalogView.cs (line 110, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 98, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 97, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 96, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 88, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 87, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 86, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 78, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 69, col 16)`
- `src\eShopWinForms\Views\CatalogView.cs (line 68, col 16)`
- `src\eShopWinForms\Views\CatalogView.cs (line 60, col 16)`
- `src\eShopWinForms\Views\CatalogView.cs (line 41, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 40, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 22, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 19, col 8)`
- `src\eShopWinForms\Views\CatalogView.cs (line 17, col 39)`
- `src\eShopWinForms\Program.cs (line 26, col 12)`
- `src\eShopWinForms\Program.cs (line 19, col 12)`
- `src\eShopWinForms\Program.cs (line 18, col 12)`

</details>

<details id="Project_s_target_framework_s_needs_to_be_changed">
<summary><b>Project's target framework(s) needs to be changed</b> — affected files</summary>

- `src\eShopWCFService\eShopWCFService.csproj`
- `src\eShopWinForms\eShopWinForms.csproj`

</details>

<details id="Project_file_needs_to_be_converted_to_SDK-style">
<summary><b>Project file needs to be converted to SDK-style</b> — affected files</summary>

- `src\eShopWCFService\eShopWCFService.csproj`

</details>

<details id="Source_incompatible_for_selected_NET_version">
<summary><b>Source incompatible for selected .NET version</b> — affected files</summary>

- `src\eShopWCFService\ICatalogService.cs (line 11, col 5)`
- `src\eShopWCFService\ICatalogService.cs (line 32, col 9)`
- `src\eShopWCFService\ICatalogService.cs (line 30, col 9)`
- `src\eShopWCFService\ICatalogService.cs (line 28, col 9)`
- `src\eShopWCFService\ICatalogService.cs (line 26, col 9)`
- `src\eShopWCFService\ICatalogService.cs (line 24, col 9)`
- `src\eShopWCFService\ICatalogService.cs (line 22, col 9)`
- `src\eShopWCFService\ICatalogService.cs (line 20, col 9)`
- `src\eShopWCFService\ICatalogService.cs (line 18, col 9)`
- `src\eShopWCFService\ICatalogService.cs (line 16, col 9)`
- `src\eShopWCFService\ICatalogService.cs (line 14, col 9)`
- `src\eShopWinForms\Properties\Settings.Designer.cs (line 15, col 74)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 266, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 265, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 256, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 255, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 246, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 245, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 236, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 235, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 226, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 225, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 216, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 215, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 206, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 205, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 196, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 195, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 186, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 185, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 176, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 175, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 166, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 165, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 156, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 155, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 146, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 145, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 136, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 135, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 126, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 125, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 116, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 115, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 106, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 105, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 96, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 95, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 86, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 85, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 76, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 75, col 8)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 66, col 12)`
- `src\eShopWinForms\Properties\Resources.Designer.cs (line 65, col 8)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 528, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 521, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 508, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 497, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 485, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 319, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 100, col 12)`
- `src\eShopWinForms\Views\CatalogView.cs (line 58, col 16)`
- `src\eShopWinForms\Views\CatalogView.cs (line 57, col 16)`

</details>

<details id="NuGet_package_upgrade_is_recommended">
<summary><b>NuGet package upgrade is recommended</b> — affected files</summary>

- `src\eShopWCFService\eShopWCFService.csproj`
- `src\eShopWinForms\eShopWinForms.csproj`

</details>

<details id="NuGet_package_is_deprecated">
<summary><b>NuGet package is deprecated</b> — affected files</summary>

- `src\eShopWCFService\eShopWCFService.csproj`
- `src\eShopWinForms\eShopWinForms.csproj`

</details>

## Security Issues

> **Note:** These issues were generated by AI and may contain inaccuracies or incomplete information. Please review carefully.

| Issue Name | Criticality | Story Points | Files |
|------------|-------------|--------------|-------|
| CVE-2024-0056: Microsoft.Data.SqlClient and System.Data.SqlClient vulnerable to SQL Data Provider Security Feature Bypass | Mandatory | 1 | 0 |
| CVE-2018-8292: .NET Core Information Disclosure | Mandatory | 1 | 0 |
| CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json | Mandatory | 1 | 0 |
| CVE-2019-0820: Regular Expression Denial of Service in System.Text.RegularExpressions | Mandatory | 1 | 0 |
| CVE-2021-24112: .NET Core Remote Code Execution Vulnerability | Mandatory | 1 | 0 |
| CWE-662: Improper Synchronization | Potential | 8 | [4](#CWE-662_Improper_Synchronization) |
| CWE-820: Missing Synchronization | Potential | 8 | [4](#CWE-820_Missing_Synchronization) |
| CWE-772: Missing Release of Resource after Effective Lifetime | Potential | 3 | [2](#CWE-772_Missing_Release_of_Resource_after_Effective_Lifetime) |
| CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime | Potential | 3 | [2](#CWE-775_Missing_Release_of_File_Descriptor_or_Handle_after_Effective_Lifetime) |
| CWE-778: Insufficient Logging | Potential | 3 | [3](#CWE-778_Insufficient_Logging) |
| CWE-99: Improper Control of Resource Identifiers ('Resource Injection') | Potential | 3 | [2](#CWE-99_Improper_Control_of_Resource_Identifiers_Resource_Injection) |
| CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal') | Optional | 8 | [4](#CWE-22_Improper_Limitation_of_a_Pathname_to_a_Restricted_Directory_Path_Traversal) |
| CWE-259: Use of Hard-coded Password | Optional | 5 | [3](#CWE-259_Use_of_Hard-coded_Password) |
| CWE-798: Use of Hard-coded Credentials | Optional | 5 | [3](#CWE-798_Use_of_Hard-coded_Credentials) |
| CWE-23: Relative Path Traversal | Optional | 5 | [4](#CWE-23_Relative_Path_Traversal) |
| CVE-2022-41064: .NET Information Disclosure Vulnerability | Optional | 1 | 0 |
| CVE-2022-34716: .NET Information Disclosure Vulnerability | Optional | 1 | 0 |

### Security Issue Details

<details id="CWE-662_Improper_Synchronization">
<summary><b>CWE-662: Improper Synchronization</b> — affected files</summary>

- `src\eShopWCFService\CatalogService.svc.cs:65-70`
- `src\eShopWCFService\CatalogService.svc.cs:99-117`
- `src\eShopWCFService\Models\CatalogItem.cs:13-15`
- `src\eShopWCFService\Models\CatalogItemsStock.cs:22-25`

</details>

<details id="CWE-820_Missing_Synchronization">
<summary><b>CWE-820: Missing Synchronization</b> — affected files</summary>

- `src\eShopWCFService\CatalogService.svc.cs:65-70`
- `src\eShopWCFService\CatalogService.svc.cs:99-117`
- `src\eShopWCFService\Models\CatalogItem.cs:13-15`
- `src\eShopWCFService\Models\CatalogItemsStock.cs:22-25`

</details>

<details id="CWE-772_Missing_Release_of_Resource_after_Effective_Lifetime">
<summary><b>CWE-772: Missing Release of Resource after Effective Lifetime</b> — affected files</summary>

- `src\eShopWinForms\Views\CatalogView.cs:39-42`
- `src\eShopWinForms\Views\CatalogView.cs:48-62`

</details>

<details id="CWE-775_Missing_Release_of_File_Descriptor_or_Handle_after_Effective_Lifetime">
<summary><b>CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime</b> — affected files</summary>

- `src\eShopWinForms\Views\CatalogView.cs:39-42`
- `src\eShopWinForms\Views\CatalogView.cs:48-62`

</details>

<details id="CWE-778_Insufficient_Logging">
<summary><b>CWE-778: Insufficient Logging</b> — affected files</summary>

- `src\eShopWCFService\CatalogService.svc.cs:65-83`
- `src\eShopWCFService\CatalogService.svc.cs:99-117`
- `src\eShopWCFService\ICatalogService.cs:25-32`

</details>

<details id="CWE-99_Improper_Control_of_Resource_Identifiers_Resource_Injection">
<summary><b>CWE-99: Improper Control of Resource Identifiers ('Resource Injection')</b> — affected files</summary>

- `src\eShopWinForms\Views\CatalogView.cs:55-59`
- `src\eShopWCFService\CatalogService.svc.cs:65-76`

</details>

<details id="CWE-22_Improper_Limitation_of_a_Pathname_to_a_Restricted_Directory_Path_Traversal">
<summary><b>CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')</b> — affected files</summary>

- `src\eShopWinForms\Views\CatalogView.cs:48-61`
- `src\eShopWCFService\CatalogService.svc.cs:65-76`
- `src\eShopWCFService\Models\CatalogItem.cs:24-25`
- `src\eShopWCFService\ICatalogService.cs:27-30`

</details>

<details id="CWE-259_Use_of_Hard-coded_Password">
<summary><b>CWE-259: Use of Hard-coded Password</b> — affected files</summary>

- `docker-compose.override.yml:7`
- `docker-compose.override.yml:13`
- `docker-compose.override.yml:18`

</details>

<details id="CWE-798_Use_of_Hard-coded_Credentials">
<summary><b>CWE-798: Use of Hard-coded Credentials</b> — affected files</summary>

- `docker-compose.override.yml:7`
- `docker-compose.override.yml:13`
- `docker-compose.override.yml:18`

</details>

<details id="CWE-23_Relative_Path_Traversal">
<summary><b>CWE-23: Relative Path Traversal</b> — affected files</summary>

- `src\eShopWinForms\Views\CatalogView.cs:48-61`
- `src\eShopWCFService\CatalogService.svc.cs:65-76`
- `src\eShopWCFService\Models\CatalogItem.cs:24-25`
- `src\eShopWCFService\ICatalogService.cs:27-30`

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
