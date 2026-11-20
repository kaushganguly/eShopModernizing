# Configuration & Externalized Settings Inventory

The workspace has legacy XML configuration and a separate JSON-based development configuration for the ported application. No external secret manager or production configuration repository was identified in the assessed files.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Legacy application configuration | XML | `src/eShopLegacyMVC/Web.config` | Connection string, mock/customization flags, framework and provider settings |
| Debug transform | XML transform | `src/eShopLegacyMVC/Web.Debug.config` | Replaces selected connection settings for Debug packaging |
| Release transform | XML transform | `src/eShopLegacyMVC/Web.Release.config` | Replaces selected connection settings for Release packaging |
| Legacy view configuration | XML | `src/eShopLegacyMVC/Views/Web.config` | Razor view configuration |
| Logging configuration | XML | `src/eShopLegacyMVC/log4Net.xml` | log4net logging configuration |
| Telemetry configuration | XML | `src/eShopLegacyMVC/ApplicationInsights.config` | Application Insights instrumentation configuration |
| Ported application configuration | JSON | `eShopPorted/appsettings.json` | Default connection name and application flags |
| Development overrides | JSON | `eShopPorted/appsettings.Development.json` | Development logging/configuration overrides |
| Launch profiles | JSON | `eShopPorted/Properties/launchSettings.json` | Local URLs and `ASPNETCORE_ENVIRONMENT` |
| Ported application config | XML | `eShopPorted/app.config` | Legacy framework configuration |
| Project package metadata | XML | `src/eShopLegacyMVC/packages.config` and project files | Build-time packages and framework targets |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild configuration | Local development build | Debug symbols and Web.config transform |
| Release | MSBuild configuration | Release packaging | Release Web.config transform |
| Ported Development | `ASPNETCORE_ENVIRONMENT=Development` in launch profiles | Local development runtime | Development JSON configuration |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Legacy default | IIS/application configuration | `Web.config` | Database-backed mode; mock and customization flags are false |
| Legacy Debug | Build transform | `Web.Debug.config` | Connection settings are transformed |
| Legacy Release | Build transform | `Web.Release.config` | Connection settings are transformed |
| Ported Development | `ASPNETCORE_ENVIRONMENT` launch setting | `appsettings.json`, `appsettings.Development.json` | Development runtime settings; mock mode enabled in base ported settings |

## Properties Inventory

### eShopLegacyMVC

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `connectionStrings:CatalogDBContext` | SQL Server LocalDB with integrated security | Debug/Release transform variants | `Web.config` and transforms |
| `UseMockData` | `false` | Default | `Web.config` |
| `UseCustomizationData` | `false` | Default | `Web.config` |
| log4net appenders and levels | Defined in XML | Default | `log4Net.xml` |
| Application Insights instrumentation settings | Defined in XML; sensitive identifiers not reproduced | Default | `ApplicationInsights.config` |

### eShopPorted

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `ConnectionStrings:DefaultConnection` | SQL Server LocalDB with integrated security | Development | `appsettings.json` |
| `UseMockData` | `true` | Default/development | `appsettings.json` |
| `UseCustomizationData` | `true` | Default/development | `appsettings.json` |
| `Logging:LogLevel` | Defined in JSON | Development | `appsettings.Development.json` |
| `ASPNETCORE_ENVIRONMENT` | `Development` in launch profiles | Local launch | `Properties/launchSettings.json` |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopLegacyMVC | IIS-hosted .NET Framework 4.7.2; no explicit heap/runtime limits found | Not specified | Not specified |
| eShopPorted | ASP.NET Core packages on .NET Framework 4.6.1; local launch URLs include HTTP and HTTPS | Not specified | Not specified |

The ported launch profiles specify local HTTP/HTTPS bindings; no CPU, memory, or replica limits are declared in the assessed workspace.

## Startup Dependency Chain

1. The web host loads its application configuration and DI module.
2. The selected mock service is registered without a database connection. In database-backed mode, the application requires its configured SQL Server endpoint.
3. The legacy application initializes/seeds catalog data and SQL sequences through its initializer.

No container health-check or explicit wait-for-database mechanism was found in the assessed workspace.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| SQL Server connection settings | Database connection | LocalDB and integrated security; no password value identified |
| Application Insights instrumentation | Telemetry identifier/settings | Configuration file; values omitted |

No Key Vault, Vault, AWS Secrets Manager, or encrypted secret-provisioning workflow was identified. The inspected SQL configuration uses integrated security rather than an embedded SQL password.

### Secrets Provisioning Workflow

No secret manager or deployment provisioning workflow is present in the assessed workspace. Database access relies on the host's integrated identity in the checked-in development configuration. Deployment-specific SQL credentials and telemetry secrets are not established by the repository files.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `false` in legacy; `true` in ported | Legacy `Web.config`; ported `appsettings.json` |
| `UseCustomizationData` | `false` in legacy; `true` in ported | Legacy `Web.config`; ported `appsettings.json` |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Legacy target framework | .NET Framework 4.7.2 | `eShopLegacyMVC.csproj` |
| Ported target framework | .NET Framework 4.6.1 | `eShopPorted.csproj` |
| Legacy ASP.NET MVC | 5.2.7 | Package declarations |
| Legacy Entity Framework | 6.2.0 | Package declarations |
| Ported ASP.NET Core packages | 2.2.0 | `eShopPorted.csproj` |
| Ported Entity Framework Core | 2.2.6 | `eShopPorted.csproj` |
| Application Insights | 2.9.1 | Legacy package declarations |
| log4net | 2.0.10 | Package declarations |
