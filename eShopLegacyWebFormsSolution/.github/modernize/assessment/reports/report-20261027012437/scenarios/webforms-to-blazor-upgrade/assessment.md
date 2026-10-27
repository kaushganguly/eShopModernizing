# Projects and dependencies analysis

This document provides a comprehensive overview of the projects and their dependencies in the context of upgrading to .NETCoreApp,Version=v10.0.

## Table of Contents

- [Executive Summary](#executive-Summary)
  - [Highlevel Metrics](#highlevel-metrics)
  - [Projects Compatibility](#projects-compatibility)
  - [Package Compatibility](#package-compatibility)
  - [API Compatibility](#api-compatibility)
  - [Binding Redirect Configuration](#binding-redirect-configuration)
- [Aggregate NuGet packages details](#aggregate-nuget-packages-details)
- [Top API Migration Challenges](#top-api-migration-challenges)
  - [Technologies and Features](#technologies-and-features)
  - [Most Frequent API Issues](#most-frequent-api-issues)
- [Projects Relationship Graph](#projects-relationship-graph)
- [Project Details](#project-details)

  - [src\eShopLegacyWebForms\eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj)


## Executive Summary

### Highlevel Metrics

| Metric | Count | Status |
| :--- | :---: | :--- |
| Total Projects | 1 | All require upgrade |
| Total NuGet Packages | 44 | 26 need upgrade |
| Total Code Files | 36 |  |
| Total Code Files with Incidents | 3 |  |
| Total Lines of Code | 1834 |  |
| Total Number of Issues | 49 |  |
| Estimated LOC to modify | 0+ | at least 0.0% of codebase |

### Projects Compatibility

| Project | Target Framework | Difficulty | Package Issues | API Issues | Binding Issues | Est. LOC Impact | Description |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| [src\eShopLegacyWebForms\eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | net472 | 🔴 High | 41 | 0 | 5 |  | Wap, Sdk Style = False |

### Package Compatibility

| Status | Count | Percentage |
| :--- | :---: | :---: |
| ✅ Compatible | 18 | 40.9% |
| ⚠️ Incompatible | 18 | 40.9% |
| 🔄 Upgrade Recommended | 8 | 18.2% |
| ***Total NuGet Packages*** | ***44*** | ***100%*** |

### API Compatibility

| Category | Count | Impact |
| :--- | :---: | :--- |
| 🔴 Binary Incompatible | 0 | High - Require code changes |
| 🟡 Source Incompatible | 0 | Medium - Needs re-compilation and potential conflicting API error fixing |
| 🔵 Behavioral change | 0 | Low - Behavioral changes that may require testing at runtime |
| ✅ Compatible | 0 |  |
| ***Total APIs Analyzed*** | ***0*** |  |

### Binding Redirect Configuration

| Severity | Count | Description |
| :--- | :---: | :--- |
| 🔴Mandatory | 3 | Must be fixed to avoid runtime failures |
| 🟡Potential | 2 | May cause issues in certain scenarios |
| ***Total Binding Issues*** | ***5*** | ***Across 1 project(s)*** |

## Aggregate NuGet packages details

| Package | Current Version | Suggested Version | Projects | Description |
| :--- | :---: | :---: | :--- | :--- |
| Antlr | 3.5.0.2 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | Needs to be replaced with Replace with new package Antlr4=4.6.6 |
| AspNet.ScriptManager.bootstrap | 4.3.1 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is incompatible |
| AspNet.ScriptManager.jQuery | 3.3.1 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is incompatible |
| Autofac | 4.9.1 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ✅Compatible |
| Autofac.Web | 4.0.0 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is incompatible |
| bootstrap | 4.3.1 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ✅Compatible |
| EntityFramework | 6.2.0 | 6.5.2 | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package upgrade is recommended |
| jQuery | 3.5.0 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is deprecated |
| log4net | 2.0.10 | 3.5.0 | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package contains security vulnerability |
| Microsoft.ApplicationInsights | 2.9.1 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is deprecated |
| Microsoft.ApplicationInsights.Agent.Intercept | 2.4.0 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is incompatible |
| Microsoft.ApplicationInsights.DependencyCollector | 2.9.1 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is deprecated |
| Microsoft.ApplicationInsights.PerfCounterCollector | 2.9.1 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is deprecated |
| Microsoft.ApplicationInsights.Web | 2.9.1 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is incompatible |
| Microsoft.ApplicationInsights.WindowsServer | 2.9.1 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is deprecated |
| Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel | 2.9.1 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is deprecated |
| Microsoft.AspNet.FriendlyUrls | 1.0.2 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is incompatible |
| Microsoft.AspNet.FriendlyUrls.Core | 1.0.2 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is incompatible |
| Microsoft.AspNet.ScriptManager.MSAjax | 5.0.0 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is incompatible |
| Microsoft.AspNet.ScriptManager.WebForms | 5.0.0 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is incompatible |
| Microsoft.AspNet.SessionState.SessionStateModule | 1.1.0 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package functionality is included with framework reference |
| Microsoft.AspNet.TelemetryCorrelation | 1.0.5 | 1.0.3 | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is incompatible |
| Microsoft.AspNet.Web.Optimization | 1.1.3 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is incompatible |
| Microsoft.AspNet.Web.Optimization.WebForms | 1.1.3 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ⚠️NuGet package is incompatible |
| Microsoft.CodeDom.Providers.DotNetCompilerPlatform | 2.0.1 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package functionality is included with framework reference |
| Microsoft.Net.Compilers | 2.10.0 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package functionality is included with framework reference |
| Microsoft.Web.Infrastructure | 1.0.0.0 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package functionality is included with framework reference |
| Modernizr | 2.8.3 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ✅Compatible |
| Newtonsoft.Json | 12.0.1 | 13.0.4 | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package upgrade is recommended |
| Pipelines.Sockets.Unofficial | 1.0.7 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ✅Compatible |
| popper.js | 1.14.3 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ✅Compatible |
| Respond | 1.4.2 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ✅Compatible |
| System.Buffers | 4.4.0 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package functionality is included with framework reference |
| System.Diagnostics.DiagnosticSource | 4.5.1 | 10.0.14 | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package upgrade is recommended |
| System.Diagnostics.PerformanceCounter | 4.5.0 | 10.0.14 | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package upgrade is recommended |
| System.IO.Compression | 4.3.0 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package functionality is included with framework reference |
| System.IO.Compression.ZipFile | 4.3.0 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package functionality is included with framework reference |
| System.IO.Pipelines | 4.5.1 | 10.0.14 | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package upgrade is recommended |
| System.Memory | 4.5.1 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package functionality is included with framework reference |
| System.Numerics.Vectors | 4.4.0 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package functionality is included with framework reference |
| System.Runtime.CompilerServices.Unsafe | 4.5.0 | 6.1.2 | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package upgrade is recommended |
| System.Threading.Channels | 4.5.0 | 10.0.14 | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package upgrade is recommended |
| System.Threading.Tasks.Extensions | 4.5.1 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | NuGet package functionality is included with framework reference |
| WebGrease | 1.6.0 |  | [eShopLegacyWebForms.csproj](#srceshoplegacywebformseshoplegacywebformscsproj) | ✅Compatible |

## Top API Migration Challenges

### Technologies and Features

| Technology | Issues | Percentage | Migration Path |
| :--- | :---: | :---: | :--- |

### Most Frequent API Issues

| API | Count | Percentage | Category |
| :--- | :---: | :---: | :--- |

## Projects Relationship Graph

Legend:
📦 SDK-style project
⚙️ Classic project

```mermaid
flowchart LR
    P1["<b>⚙️&nbsp;eShopLegacyWebForms.csproj</b><br/><small>net472</small>"]
    click P1 "#srceshoplegacywebformseshoplegacywebformscsproj"

```

## Project Details

<a id="srceshoplegacywebformseshoplegacywebformscsproj"></a>
### src\eShopLegacyWebForms\eShopLegacyWebForms.csproj

#### Project Info

- **Current Target Framework:** net472
- **Proposed Target Framework:** net10.0
- **SDK-style**: False
- **Project Kind:** Wap
- **Dependencies**: 0
- **Dependants**: 0
- **Number of Files**: 166
- **Number of Files with Incidents**: 3
- **Lines of Code**: 1834
- **Estimated LOC to modify**: 0+ (at least 0.0% of the project)

#### Dependency Graph

Legend:
📦 SDK-style project
⚙️ Classic project

```mermaid
flowchart TB
    subgraph current["eShopLegacyWebForms.csproj"]
        MAIN["<b>⚙️&nbsp;eShopLegacyWebForms.csproj</b><br/><small>net472</small>"]
        click MAIN "#srceshoplegacywebformseshoplegacywebformscsproj"
    end

```

### API Compatibility

| Category | Count | Impact |
| :--- | :---: | :--- |
| 🔴 Binary Incompatible | 0 | High - Require code changes |
| 🟡 Source Incompatible | 0 | Medium - Needs re-compilation and potential conflicting API error fixing |
| 🔵 Behavioral change | 0 | Low - Behavioral changes that may require testing at runtime |
| ✅ Compatible | 0 |  |
| ***Total APIs Analyzed*** | ***0*** |  |

#### Binding Redirect Configuration

| Rule | Severity | Details | Recommendation |
| :--- | :---: | :--- | :--- |
| Manual redirect conflicts with auto-generated version | 🔴Mandatory | Manual redirect for Newtonsoft.Json targets 12.0.0.0 but auto-generation would target 12.0.1 (MSB3836 conflict) | Remove the conflicting manual binding redirect or disable auto-generation. |
| Manual redirect conflicts with auto-generated version | 🔴Mandatory | Manual redirect for WebGrease targets 1.6.5135.21930 but auto-generation would target 1.6.0 (MSB3836 conflict) | Remove the conflicting manual binding redirect or disable auto-generation. |
| Manual redirect conflicts with auto-generated version | 🔴Mandatory | Manual redirect for System.Diagnostics.DiagnosticSource targets 4.0.3.1 but auto-generation would target 4.5.1 (MSB3836 conflict) | Remove the conflicting manual binding redirect or disable auto-generation. |
| Binding redirect forces version downgrade | 🟡Potential | Binding redirect for Newtonsoft.Json targets 12.0.0.0 but package provides 12.0.1 | Update the binding redirect newVersion to match the version provided by the NuGet package. |
| Binding redirect forces version downgrade | 🟡Potential | Binding redirect for System.Diagnostics.DiagnosticSource targets 4.0.3.1 but package provides 4.5.1 | Update the binding redirect newVersion to match the version provided by the NuGet package. |
