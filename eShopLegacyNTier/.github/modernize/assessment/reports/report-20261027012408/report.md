# eShopWCFService

## Summary

| Metric | Value |
|--------|-------|
| Total Issues | 34 |
| Mandatory Blockers | 10 |
| Potential Issues | 16 |

## Component Information

| Property | Value |
|----------|-------|
| Language | C# |
| Frameworks | .NETFramework,Version=v4.6.1 |
| Build tools | MSBuild |

## Cloud Readiness Issues

| Issue Name | Criticality | Story Points | Occurrences |
|------------|-------------|--------------|-------------|
| Hardcoded URLs detected | Potential | 1 | [45](#Hardcoded_URLs_detected) |
| SQL database connection detected | Potential | 3 | [1](#SQL_database_connection_detected) |
| Local or network IO operations detected | Potential | 3 | [1](#Local_or_network_IO_operations_detected) |
| Local OS environment access detected | Potential | 1 | [1](#Local_OS_environment_access_detected) |
| Upgrade to newer target framework to get better cloud experience | Optional | 3 | [2](#Upgrade_to_newer_target_framework_to_get_better_cloud_experience) |
| Connection strings without configuration builders detected | Optional | 3 | [2](#Connection_strings_without_configuration_builders_detected) |
| System.Data.SqlClient dependency detected | Optional | 3 | [1](#System_Data_SqlClient_dependency_detected) |
| Static content detected | Optional | 3 | [1](#Static_content_detected) |

### Issue Details

<details id="Hardcoded_URLs_detected">
<summary><b>Hardcoded URLs detected</b> — affected files</summary>

- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 17)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 190)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 251)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 405)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 312)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 530)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 533)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 530)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 533)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 536)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 539)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 536)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 539)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 500)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 503)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 500)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 503)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 524)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 527)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 524)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 527)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 506)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 509)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 506)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 509)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 512)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 515)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 512)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 515)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 518)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 521)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 518)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 521)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 554)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 557)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 554)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 557)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 548)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 551)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 548)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 551)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 542)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 545)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 542)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 545)`

</details>

<details id="SQL_database_connection_detected">
<summary><b>SQL database connection detected</b> — affected files</summary>

- `src\eShopWCFService\Web.config`

</details>

<details id="Local_or_network_IO_operations_detected">
<summary><b>Local or network IO operations detected</b> — affected files</summary>

- `src\eShopWinForms\Views\CatalogView.cs (line 57)`

</details>

<details id="Local_OS_environment_access_detected">
<summary><b>Local OS environment access detected</b> — affected files</summary>

- `src\eShopWinForms\Views\CatalogView.cs (line 56)`

</details>

<details id="Upgrade_to_newer_target_framework_to_get_better_cloud_experience">
<summary><b>Upgrade to newer target framework to get better cloud experience</b> — affected files</summary>

- `src\eShopWCFService\eShopWCFService.csproj`
- `src\eShopWinForms\eShopWinForms.csproj`

</details>

<details id="Connection_strings_without_configuration_builders_detected">
<summary><b>Connection strings without configuration builders detected</b> — affected files</summary>

- `src\eShopWCFService\Web.config`
- `src\eShopWCFService\Web.config`

</details>

<details id="System_Data_SqlClient_dependency_detected">
<summary><b>System.Data.SqlClient dependency detected</b> — affected files</summary>

- `src\eShopWCFService\Web.config`

</details>

<details id="Static_content_detected">
<summary><b>Static content detected</b> — affected files</summary>

- `src\eShopWinForms\eShopWinForms.csproj`

</details>

## DotNET Upgrade Issues [View Details](scenarios/dotnet-version-upgrade/assessment.md)

| Issue Category | Criticality | Story Points | Occurrences |
|----------------|-------------|--------------|-------------|
| Binary incompatible for selected .NET version | Mandatory | 1 | [1644](#Binary_incompatible_for_selected_NET_version) |
| Project file needs to be converted to SDK-style | Mandatory | 1 | [2](#Project_file_needs_to_be_converted_to_SDK-style) |
| Project's target framework(s) needs to be changed | Mandatory | 1 | [2](#Project_s_target_framework_s_needs_to_be_changed) |
| NuGet package is incompatible | Mandatory | 1 | [1](#NuGet_package_is_incompatible) |
| WCF Client APIs | Mandatory | 1 | 0 |
| Legacy Configuration System | Mandatory | 2 | 0 |
| GDI+ / System.Drawing | Mandatory | 1 | 0 |
| Windows Forms Legacy Controls | Mandatory | 3 | 0 |
| Windows Forms | Mandatory | 1 | 0 |
| Source incompatible for selected .NET version | Potential | 1 | [118](#Source_incompatible_for_selected_NET_version) |
| NuGet package upgrade is recommended | Potential | 1 | [3](#NuGet_package_upgrade_is_recommended) |
| NuGet package is deprecated | Optional | 1 | [2](#NuGet_package_is_deprecated) |
| NuGet package contains security vulnerability | Optional | 1 | [1](#NuGet_package_contains_security_vulnerability) |

### Issue Details

<details id="Binary_incompatible_for_selected_NET_version">
<summary><b>Binary incompatible for selected .NET version</b> — affected files</summary>

- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 643, col 50)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 642, col 50)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 641, col 50)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 640, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 639, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 638, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 637, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 636, col 61)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 635, col 43)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 634, col 48)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 633, col 54)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 632, col 46)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 631, col 45)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 630, col 43)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 629, col 43)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 628, col 43)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 627, col 45)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 626, col 44)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 625, col 46)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 624, col 51)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 623, col 50)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 622, col 46)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 621, col 46)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 620, col 46)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 619, col 44)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 618, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 617, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 616, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 615, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 614, col 63)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 613, col 45)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 612, col 54)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 611, col 48)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 610, col 48)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 609, col 51)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 608, col 45)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 607, col 45)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 606, col 48)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 605, col 43)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 604, col 43)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 603, col 46)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 602, col 46)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 601, col 54)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 600, col 54)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 599, col 51)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 598, col 51)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 597, col 51)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 596, col 51)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 595, col 54)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 590, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 589, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 588, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 587, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 586, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 585, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 584, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 583, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 582, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 581, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 580, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 579, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 578, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 577, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 576, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 575, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 574, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 573, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 572, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 571, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 570, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 569, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 568, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 567, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 566, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 565, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 564, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 563, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 562, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 561, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 557, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 556, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 555, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 554, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 553, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 552, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 551, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 549, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 545, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 544, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 543, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 542, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 541, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 540, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 539, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 535, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 534, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 533, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 532, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 531, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 530, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 529, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 528, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 527, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 526, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 525, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 523, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 519, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 518, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 517, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 516, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 515, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 514, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 513, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 512, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 508, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 507, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 506, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 505, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 504, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 503, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 502, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 501, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 500, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 496, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 495, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 494, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 493, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 489, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 488, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 487, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 486, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 485, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 484, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 480, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 479, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 478, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 477, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 476, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 475, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 471, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 470, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 469, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 468, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 467, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 466, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 462, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 461, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 460, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 459, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 455, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 454, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 453, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 452, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 451, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 450, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 449, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 445, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 444, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 443, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 442, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 441, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 437, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 436, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 435, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 434, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 433, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 432, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 431, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 430, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 429, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 428, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 427, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 426, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 425, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 424, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 420, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 416, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 412, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 408, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 407, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 406, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 405, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 404, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 403, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 402, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 398, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 394, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 393, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 392, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 391, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 390, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 389, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 388, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 387, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 386, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 382, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 381, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 380, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 379, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 378, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 377, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 376, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 375, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 371, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 370, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 369, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 368, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 367, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 366, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 365, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 360, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 359, col 12)`
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
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 338, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 337, col 12)`
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
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 326, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 325, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 324, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 320, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 319, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 318, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 317, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 316, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 315, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 314, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 313, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 312, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 311, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 307, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 306, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 305, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 304, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 300, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 299, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 298, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 297, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 293, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 292, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 291, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 290, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 286, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 285, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 284, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 283, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 279, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 278, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 277, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 276, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 275, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 271, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 270, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 269, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 268, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 267, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 266, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 265, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 264, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 258, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 257, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 256, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 255, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 251, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 250, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 249, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 248, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 247, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 246, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 245, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 244, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 243, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 239, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 238, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 237, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 236, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 235, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 234, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 233, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 232, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 228, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 227, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 226, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 225, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 224, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 223, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 222, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 221, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 217, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 216, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 215, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 214, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 213, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 212, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 211, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 210, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 209, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 205, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 204, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 203, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 202, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 201, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 200, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 199, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 198, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 197, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 196, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 195, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 194, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 193, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 192, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 191, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 190, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 189, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 185, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 184, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 183, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 182, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 181, col 12)`
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
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 162, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 161, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 160, col 12)`
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
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 146, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 145, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 144, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 143, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 142, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 141, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 140, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 139, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 138, col 12)`
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
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 122, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 118, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 117, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 116, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 115, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 114, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 113, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 112, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 111, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 110, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 109, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 108, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 107, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 106, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 105, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 104, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 103, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 102, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 99, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 95, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 94, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 93, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 92, col 12)`
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

<details id="Project_file_needs_to_be_converted_to_SDK-style">
<summary><b>Project file needs to be converted to SDK-style</b> — affected files</summary>

- `src\eShopWCFService\eShopWCFService.csproj`
- `src\eShopWinForms\eShopWinForms.csproj`

</details>

<details id="Project_s_target_framework_s_needs_to_be_changed">
<summary><b>Project's target framework(s) needs to be changed</b> — affected files</summary>

- `src\eShopWCFService\eShopWCFService.csproj`
- `src\eShopWinForms\eShopWinForms.csproj`

</details>

<details id="NuGet_package_is_incompatible">
<summary><b>NuGet package is incompatible</b> — affected files</summary>

- `src\eShopWinForms\eShopWinForms.csproj`

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
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 584, col 8)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 580, col 8)`
- `src\eShopWinForms\Connected Services\eShopServiceReference\Reference.cs (line 562, col 119)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 557, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 551, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 539, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 512, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 501, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 343, col 12)`
- `src\eShopWinForms\Views\CatalogView.Designer.cs (line 127, col 12)`
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

<details id="NuGet_package_contains_security_vulnerability">
<summary><b>NuGet package contains security vulnerability</b> — affected files</summary>

- `src\eShopWinForms\eShopWinForms.csproj`

</details>

## Security Issues

> **Note:** These issues were generated by AI and may contain inaccuracies or incomplete information. Please review carefully.

| Issue Name | Criticality | Story Points | Files |
|------------|-------------|--------------|-------|
| CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json | Mandatory | 1 | [1](#CVE-2024-21907_Improper_Handling_of_Exceptional_Conditions_in_Newtonsoft_Json) |
| CWE-662: Improper Synchronization | Potential | 8 | [2](#CWE-662_Improper_Synchronization) |
| CWE-667: Improper Locking | Potential | 8 | [2](#CWE-667_Improper_Locking) |
| CWE-820: Missing Synchronization | Potential | 8 | [2](#CWE-820_Missing_Synchronization) |
| CWE-682: Incorrect Calculation | Potential | 5 | [1](#CWE-682_Incorrect_Calculation) |
| CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context | Potential | 5 | [2](#CWE-567_Unsynchronized_Access_to_Shared_Data_in_a_Multithreaded_Context) |
| CWE-99: Improper Control of Resource Identifiers ('Resource Injection') | Potential | 3 | [2](#CWE-99_Improper_Control_of_Resource_Identifiers_Resource_Injection) |
| CWE-606: Unchecked Input for Loop Condition | Potential | 3 | [2](#CWE-606_Unchecked_Input_for_Loop_Condition) |
| CWE-772: Missing Release of Resource after Effective Lifetime | Potential | 3 | [1](#CWE-772_Missing_Release_of_Resource_after_Effective_Lifetime) |
| CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime | Potential | 3 | [1](#CWE-775_Missing_Release_of_File_Descriptor_or_Handle_after_Effective_Lifetime) |
| CWE-778: Insufficient Logging | Potential | 3 | [3](#CWE-778_Insufficient_Logging) |
| CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal') | Optional | 8 | [3](#CWE-22_Improper_Limitation_of_a_Pathname_to_a_Restricted_Directory_Path_Traversal) |
| CWE-23: Relative Path Traversal | Optional | 5 | [3](#CWE-23_Relative_Path_Traversal) |

### Security Issue Details

<details id="CVE-2024-21907_Improper_Handling_of_Exceptional_Conditions_in_Newtonsoft_Json">
<summary><b>CVE-2024-21907: Improper Handling of Exceptional Conditions in Newtonsoft.Json</b> — affected files</summary>

- `src/eShopWinForms/packages.config:5`

</details>

<details id="CWE-662_Improper_Synchronization">
<summary><b>CWE-662: Improper Synchronization</b> — affected files</summary>

- `src/eShopWCFService/CatalogService.svc.cs:65-70`
- `src/eShopWCFService/Models/CatalogItem.cs:13-15`

</details>

<details id="CWE-667_Improper_Locking">
<summary><b>CWE-667: Improper Locking</b> — affected files</summary>

- `src/eShopWCFService/CatalogService.svc.cs:65-70`
- `src/eShopWCFService/Models/CatalogItem.cs:13-15`

</details>

<details id="CWE-820_Missing_Synchronization">
<summary><b>CWE-820: Missing Synchronization</b> — affected files</summary>

- `src/eShopWCFService/CatalogService.svc.cs:65-70`
- `src/eShopWCFService/Models/CatalogItem.cs:13-15`

</details>

<details id="CWE-682_Incorrect_Calculation">
<summary><b>CWE-682: Incorrect Calculation</b> — affected files</summary>

- `src/eShopWCFService/CatalogService.svc.cs:65-70`

</details>

<details id="CWE-567_Unsynchronized_Access_to_Shared_Data_in_a_Multithreaded_Context">
<summary><b>CWE-567: Unsynchronized Access to Shared Data in a Multithreaded Context</b> — affected files</summary>

- `src/eShopWCFService/CatalogService.svc.cs:65-70`
- `src/eShopWCFService/Models/CatalogItem.cs:13-15`

</details>

<details id="CWE-99_Improper_Control_of_Resource_Identifiers_Resource_Injection">
<summary><b>CWE-99: Improper Control of Resource Identifiers ('Resource Injection')</b> — affected files</summary>

- `src/eShopWinForms/Views/CatalogView.cs:55-58`
- `src/eShopWCFService/Models/CatalogItem.cs:24-25`

</details>

<details id="CWE-606_Unchecked_Input_for_Loop_Condition">
<summary><b>CWE-606: Unchecked Input for Loop Condition</b> — affected files</summary>

- `src/eShopWinForms/Views/CatalogView.cs:48-62`
- `src/eShopWCFService/CatalogService.svc.cs:56-70`

</details>

<details id="CWE-772_Missing_Release_of_Resource_after_Effective_Lifetime">
<summary><b>CWE-772: Missing Release of Resource after Effective Lifetime</b> — affected files</summary>

- `src/eShopWinForms/Views/CatalogView.cs:48-62`

</details>

<details id="CWE-775_Missing_Release_of_File_Descriptor_or_Handle_after_Effective_Lifetime">
<summary><b>CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime</b> — affected files</summary>

- `src/eShopWinForms/Views/CatalogView.cs:55-61`

</details>

<details id="CWE-778_Insufficient_Logging">
<summary><b>CWE-778: Insufficient Logging</b> — affected files</summary>

- `src/eShopWCFService/CatalogService.svc.cs:65-82`
- `src/eShopWCFService/CatalogService.svc.cs:99-116`
- `src/eShopWCFService/Web.config:22-43`

</details>

<details id="CWE-22_Improper_Limitation_of_a_Pathname_to_a_Restricted_Directory_Path_Traversal">
<summary><b>CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')</b> — affected files</summary>

- `src/eShopWinForms/Views/CatalogView.cs:55-58`
- `src/eShopWCFService/CatalogService.svc.cs:65-76`
- `src/eShopWCFService/ICatalogService.cs:27-30`

</details>

<details id="CWE-23_Relative_Path_Traversal">
<summary><b>CWE-23: Relative Path Traversal</b> — affected files</summary>

- `src/eShopWinForms/Views/CatalogView.cs:55-58`
- `src/eShopWCFService/CatalogService.svc.cs:65-76`
- `src/eShopWCFService/ICatalogService.cs:27-30`

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
