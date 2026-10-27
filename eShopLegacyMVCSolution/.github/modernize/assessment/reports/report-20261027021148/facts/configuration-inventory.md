# Configuration & Externalized Settings Inventory

Configuration is split between the legacy application's XML configuration and the ported application's JSON settings. Each application exposes mock-data toggles; the checked-in database connections target local SQL Server LocalDB.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| ASP.NET application settings and connection strings | Runtime configuration | `src/eShopLegacyMVC/Web.config` | LocalDB connection and mock/customization toggles |
| Debug transform | Build/deployment transform | `src/eShopLegacyMVC/Web.Debug.config` | Overrides connection-string settings for Debug |
| Release transform | Build/deployment transform | `src/eShopLegacyMVC/Web.Release.config` | Overrides connection-string settings for Release |
| Ported app settings | Runtime configuration | `eShopPorted/appsettings.json` | LocalDB connection, logging, allowed hosts, and feature toggles |
| ASP.NET view configuration | Framework configuration | `src/eShopLegacyMVC/Views/Web.config`, `eShopPorted/Views/Web.config` | Razor view-engine settings |
| Telemetry and logging | Runtime configuration | `src/eShopLegacyMVC/ApplicationInsights.config`, `src/eShopLegacyMVC/log4Net.xml` | Application Insights and log4net configuration |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Development compilation and Debug web transform | Standard project references |
| Release | MSBuild `Release` configuration | Release compilation and Release web transform | Standard project references |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Legacy default | IIS/application configuration | `Web.config` | `UseMockData=false`; `UseCustomizationData=false` |
| Legacy Debug | Build transform | `Web.Debug.config` | Connection string transform |
| Legacy Release | Build transform | `Web.Release.config` | Connection string transform |
| Ported default | ASP.NET Core configuration | `appsettings.json` | `UseMockData=true`; `UseCustomizationData=true` |
| Development/Production | ASP.NET Core environment convention | No environment-specific settings files found | Runtime environment may override JSON settings |

## Properties Inventory

### eShopLegacyMVC

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `CatalogDBContext` | LocalDB database with integrated security | Default, transformed by build profile | `Web.config` connection string |
| `UseMockData` | `false` | Default | `Web.config` app setting |
| `UseCustomizationData` | `false` | Default | `Web.config` app setting |
| `ClientValidationEnabled` | `true` | Default | `Web.config` app setting |
| `UnobtrusiveJavaScriptEnabled` | `true` | Default | `Web.config` app setting |

### eShopPorted

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `ConnectionStrings:DefaultConnection` | LocalDB `eShopPorted`, trusted connection | Default | `appsettings.json` |
| `UseMockData` | `true` | Default | `appsettings.json` |
| `UseCustomizationData` | `true` | Default | `appsettings.json` |
| `Logging:LogLevel:Default` | `Warning` | Default | `appsettings.json` |
| `AllowedHosts` | `*` | Default | `appsettings.json` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopLegacyMVC | ASP.NET on .NET Framework 4.7.2 | Not specified | Not specified |
| eShopPorted | ASP.NET Core 2.2 on .NET Framework 4.6.1 target | Not specified | Not specified |

No JVM parameters, container resource limits, or instance scaling settings were found in this target.

## Startup Dependency Chain

1. The legacy application registers controllers and services at application start. When mock mode is disabled, database initialization uses the configured LocalDB context.
2. The ported application builds its ASP.NET Core host and registers MVC and Autofac services. When mock mode is disabled, it registers the EF Core context and SQL Server provider.

No separate configuration server, service-discovery dependency, or explicit database readiness wait mechanism was identified.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` | Local database connection | Integrated/Windows authentication; no password stored |
| `ConnectionStrings:DefaultConnection` | Local database connection | Trusted connection; no password stored |

No hard-coded password, API key, or external secret-store reference was identified.

### Secrets Provisioning Workflow

No secret provisioning workflow or external vault integration is configured in the inspected project. Database connections rely on local trusted/integrated authentication; deployment-specific credentials would need to be supplied outside these checked-in defaults.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `false` legacy; `true` ported | Application setting |
| `UseCustomizationData` | `false` legacy; `true` ported | Application setting |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Legacy MVC | ASP.NET MVC 5.2.7 | `packages.config` |
| Legacy target | .NET Framework 4.7.2 | `eShopLegacyMVC.csproj` |
| Ported web framework | ASP.NET Core 2.2.0 | `eShopPorted.csproj` |
| Ported target | .NET Framework 4.6.1 | `eShopPorted.csproj` |
| Legacy ORM | Entity Framework 6.2.0 | `packages.config` |
| Ported ORM | EF Core 2.2.6 | `eShopPorted.csproj` |
| Dependency injection | Autofac 4.9.1 | Project package declarations |
| Logging | log4net 2.0.10 | Project package declarations |
