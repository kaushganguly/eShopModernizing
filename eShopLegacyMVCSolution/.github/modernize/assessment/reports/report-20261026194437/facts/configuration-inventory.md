# Configuration & Externalized Settings Inventory

The solution has classic XML configuration for the legacy application and JSON plus launch profiles for the ported application. No external configuration server or secret-store integration was identified; connection settings use local SQL Server LocalDB and integrated authentication.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web.config | Runtime configuration | `src/eShopLegacyMVC/Web.config` | Connection, app settings, framework settings, session, telemetry modules, and EF provider |
| Web.Debug.config | XML transform | `src/eShopLegacyMVC/Web.Debug.config` | Contains no active transform; sample connection transform is commented out |
| Web.Release.config | XML transform | `src/eShopLegacyMVC/Web.Release.config` | Removes the compilation `debug` attribute; sample connection transform is commented out |
| Views Web.config | View-engine configuration | `src/eShopLegacyMVC/Views/Web.config` | Razor namespaces, view compilation, and view handler configuration |
| ApplicationInsights.config | Telemetry configuration | `src/eShopLegacyMVC/ApplicationInsights.config` | Application Insights telemetry modules and processors; no instrumentation key is configured here |
| log4Net.xml | Logging configuration | `src/eShopLegacyMVC/log4Net.xml` | Rolling file appender and logging layout |
| appsettings.json | Runtime configuration | `eShopPorted/appsettings.json` | Connection string, logging level, allowed hosts, and mock/customization flags |
| appsettings.Development.json | Development override | `eShopPorted/appsettings.Development.json` | Development logging overrides |
| launchSettings.json | Local launch profiles | `eShopPorted/Properties/launchSettings.json` | Local IIS Express/project URLs and `ASPNETCORE_ENVIRONMENT` |
| app.config | Runtime configuration | `eShopPorted/app.config` | Enables server GC |
| Project files and packages.config | Build/dependency configuration | `*.csproj`, `src/eShopLegacyMVC/packages.config` | Target frameworks, package versions, and standard Debug/Release configurations |

No external configuration repository, Key Vault, Vault, AWS Secrets Manager, Docker Compose, Kubernetes ConfigMap, or Kubernetes Secret is configured within this solution folder.

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | Standard MSBuild/Visual Studio configuration | Development compilation and debugging | Legacy project enables debug compilation; no profile-specific package set identified |
| Release | Standard MSBuild/Visual Studio configuration | Production-style compilation and packaging | Release XML transform removes the `debug` compilation attribute |

The port uses the SDK-style web project format; no custom conditional package profiles or profile-specific compilation symbols were detected.

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Legacy default | ASP.NET configuration loading | `src/eShopLegacyMVC/Web.config` | Uses real database mode and disables customization data |
| Ported default | ASP.NET Core configuration loading | `eShopPorted/appsettings.json` | Enables mock and customization data; default logging level is Warning |
| Development | Launch profile sets `ASPNETCORE_ENVIRONMENT=Development` | `eShopPorted/appsettings.Development.json`, `eShopPorted/Properties/launchSettings.json` | Development logging is more verbose; local URLs are configured in launch settings |
| Debug / Release | Build and publish configuration | `Web.Debug.config`, `Web.Release.config` | Debug/Release transforms are build-time configuration, not runtime profile files |

No Production-specific `appsettings` file or explicit profile-composition configuration was found.

## Properties Inventory

### eShopLegacyMVC

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `CatalogDBContext` connection string | LocalDB; integrated authentication; MARS enabled | Default | `src/eShopLegacyMVC/Web.config` |
| `webpages:Version` | `3.0.0.0` | Default | `src/eShopLegacyMVC/Web.config` |
| `webpages:Enabled` | `false` | Default | `src/eShopLegacyMVC/Web.config` |
| `ClientValidationEnabled` | `true` | Default | `src/eShopLegacyMVC/Web.config` |
| `UnobtrusiveJavaScriptEnabled` | `true` | Default | `src/eShopLegacyMVC/Web.config` |
| `UseMockData` | `false` | Default | `src/eShopLegacyMVC/Web.config` |
| `UseCustomizationData` | `false` | Default | `src/eShopLegacyMVC/Web.config` |
| `compilation.debug` | `true` | Removed for Release | `src/eShopLegacyMVC/Web.config`, `Web.Release.config` |
| `compilation.targetFramework` | `4.7.2` | Default | `src/eShopLegacyMVC/Web.config` |
| `httpRuntime.targetFramework` | `4.6.1` | Default | `src/eShopLegacyMVC/Web.config` |
| `sessionState.mode` | `InProc` | Default | `src/eShopLegacyMVC/Web.config` |
| UI culture / culture | `en-US` / `en-US` | Default | `src/eShopLegacyMVC/Web.config` |
| EF default connection factory | SQL Server LocalDB | Default | `src/eShopLegacyMVC/Web.config` |
| log4net root level / rolling-file options | `ALL`; 10 MB files with five backups | Default | `src/eShopLegacyMVC/log4Net.xml` |
| Application Insights telemetry modules and sampling | Configured in XML | Default | `src/eShopLegacyMVC/ApplicationInsights.config` |

### eShopPorted

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `ConnectionStrings:DefaultConnection` | LocalDB; trusted connection; MARS enabled | Default | `eShopPorted/appsettings.json` |
| `Logging:LogLevel:Default` | `Warning` | Default | `eShopPorted/appsettings.json` |
| `Logging:LogLevel:Default` | `Debug` | Development | `eShopPorted/appsettings.Development.json` |
| `Logging:LogLevel:System` | `Information` | Development | `eShopPorted/appsettings.Development.json` |
| `Logging:LogLevel:Microsoft` | `Information` | Development | `eShopPorted/appsettings.Development.json` |
| `AllowedHosts` | `*` | Default | `eShopPorted/appsettings.json` |
| `UseMockData` | `true` | Default | `eShopPorted/appsettings.json` |
| `UseCustomizationData` | `true` | Default | `eShopPorted/appsettings.json` |
| `ASPNETCORE_ENVIRONMENT` | `Development` | Local launch profiles | `eShopPorted/Properties/launchSettings.json` |
| `applicationUrl` | IIS Express `http://localhost:52637`; project `https://localhost:5001;http://localhost:5000` | Local launch profiles | `eShopPorted/Properties/launchSettings.json` |
| Server GC | `true` | Default | `eShopPorted/app.config` |

No environment-variable placeholders or external property-resolution chains were detected.

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopLegacyMVC | ASP.NET Framework runtime; no custom startup flags identified | Not specified | Not specified |
| eShopPorted | .NET runtime; server GC enabled in app.config | Not specified | Not specified |

No JVM options, container resource limits, CPU allocations, scale counts, or explicit runtime memory limits were found.

## Startup Dependency Chain

1. Each web application initializes its dependency injection registrations and routes during application startup.
2. The legacy application configures its database initializer when real data is selected; database availability is required for normal catalog access.
3. The port registers its EF Core context only when mock data is disabled; the default mock-data setting avoids that database dependency.

No service-to-service startup ordering, readiness probes, wait scripts, or startup retry configuration was found.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` connection | Database connection string | LocalDB integrated authentication; no password present |
| `ConnectionStrings:DefaultConnection` | Database connection string | LocalDB trusted connection; no password present |
| Application Insights instrumentation key | Telemetry credential | Not configured in the inspected files |

No API keys, database passwords, encrypted configuration, or secret-store references were identified.

### Secrets Provisioning Workflow

No secrets provisioning workflow is configured. The database connection strings use local integrated/trusted authentication, and the inspected Application Insights configuration contains no instrumentation key. No managed identity, service principal, Key Vault, or secret binding is specified.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `false` for legacy; `true` for ported | `Web.config` or `appsettings.json` |
| `UseCustomizationData` | `false` for legacy; `true` for ported | `Web.config` or `appsettings.json` |

These are application-specific switches for selecting mock data and customization data, not a dedicated feature-management framework.

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Legacy MVC application target | .NET Framework 4.7.2 | `src/eShopLegacyMVC/eShopLegacyMVC.csproj` |
| Legacy `httpRuntime` target | .NET Framework 4.6.1 | `src/eShopLegacyMVC/Web.config` |
| Utility library target | .NET Framework 4.6.1 | `eShopLegacy.Utilities/eShopLegacy.Utilities.csproj` |
| Ported application target | .NET Framework 4.6.1 | `eShopPorted/eShopPorted.csproj` |
| ASP.NET MVC | 5.2.7 | `src/eShopLegacyMVC/packages.config` |
| ASP.NET Web API | 5.2.7 | Legacy application project references |
| ASP.NET Core / ASP.NET Core MVC | 2.2.0 | `eShopPorted/eShopPorted.csproj` |
| Entity Framework | 6.2.0 | `src/eShopLegacyMVC/packages.config` |
| Entity Framework Core | 2.2.6 | `eShopPorted/eShopPorted.csproj` |
| Autofac | 4.9.1 | Both application dependency manifests |
| log4net | 2.0.10 | Both application dependency manifests |
| Build configuration | MSBuild Debug / Release | Solution and project files |
