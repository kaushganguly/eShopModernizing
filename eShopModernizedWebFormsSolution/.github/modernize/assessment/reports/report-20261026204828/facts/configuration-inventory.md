# Configuration & Externalized Settings Inventory

Configuration is centered on Web.config, with environment, user-secrets, and optional Key Vault configuration builders. The project has Debug and Release build configurations and uses application settings as feature toggles.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web.config | ASP.NET runtime and application settings | `src/eShopModernizedWebForms/Web.config` | Contains connection string, feature flags, authentication values, and configuration-builder registrations; credential values are masked or placeholders |
| Web.Debug.config / Web.Release.config | Build transforms | `src/eShopModernizedWebForms/` | Environment-specific Web.config transforms |
| packages.config | NuGet manifest | `src/eShopModernizedWebForms/packages.config` | Declares 147 NuGet packages |
| ApplicationInsights.config | Telemetry configuration | `src/eShopModernizedWebForms/ApplicationInsights.config` | Application Insights telemetry module configuration |
| Bundle.config | Client asset configuration | `src/eShopModernizedWebForms/Bundle.config` | Bundling/minification configuration |
| Docker Compose files | Container configuration | Solution root | Compose and override files configure container execution |
| Environment configuration builder | Externalized settings | ASP.NET configuration builder chain | Reads application settings and connection strings from environment variables |
| User Secrets configuration builder | Secret store | Web.config `Secrets` builder | References a user-secrets identifier; secret values are not included here |
| Azure Key Vault configuration builder | Secret store | Web.config `AzureKeyVault` builder | Optional builder parameterized by `KeyVaultName` |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | Visual Studio/MSBuild configuration | Development compilation and debugging | Web.config Debug transform |
| Release | Visual Studio/MSBuild configuration | Release compilation and packaging | Web.config Release transform |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Mock/default | `UseMockData=true` in Web.config | Web.config | In-memory catalog and local image service |
| SQL catalog | `UseMockData=false` | Web.config plus environment/secret builders | EF6 SQL Server context and database initializer |
| Azure images | `UseAzureStorage=true` | Web.config plus secret builders | Azure Blob Storage connection |
| Azure identity | `UseAzureManagedIdentity=true` | Web.config | Managed identity SQL connection factory |
| Azure AD | `UseAzureActiveDirectory=true` | Web.config | OWIN OpenID Connect sign-in |
| Custom data | `UseCustomizationData=true` | Web.config and Setup files | Seed catalog from CSV and image archive |

## Properties Inventory

### eShopModernizedWebForms

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `CatalogDBContext` | Local SQL Server connection placeholder; credentials masked | SQL catalog | Web.config connectionStrings |
| `KeyVaultName` | Empty | Optional Key Vault | Web.config appSettings |
| `UseMockData` | `true` | Mock / SQL catalog | Web.config appSettings |
| `UseCustomizationData` | `false` | Custom data | Web.config appSettings |
| `StorageConnectionString` | Placeholder | Azure images | Web.config appSettings; environment/secret builders |
| `UseAzureManagedIdentity` | `false` | Azure identity | Web.config appSettings |
| `UseAzureStorage` | `false` | Azure images | Web.config appSettings |
| `UseAzureActiveDirectory` | `false` | Azure AD | Web.config appSettings |
| `AzureActiveDirectoryClientId` | Setup placeholder | Azure AD | Web.config appSettings |
| `AzureActiveDirectoryTenant` | Setup placeholder | Azure AD | Web.config appSettings |
| `AzureActiveDirectoryInstance` | `https://login.microsoftonline.com/{0}` | Azure AD | Web.config appSettings |
| `PostLogoutRedirectUri` | `https://localhost:44322/` | Authentication | Web.config appSettings |
| `AppInsightsInstrumentationKey` | Not set in the listed defaults | Telemetry | Web.config appSettings |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| eShopModernizedWebForms | ASP.NET on .NET Framework 4.7.2; no fixed application runtime options found | Not specified in application files | Determined by IIS/container host |

## Startup Dependency Chain

1. ASP.NET application startup registers routes and bundles.
2. Autofac constructs the selected catalog and image implementations.
3. When mock data is disabled, EF database initialization runs and seeds catalog data.
4. Image service initialization runs; Azure storage must be reachable when Azure images are enabled.
5. Application Insights telemetry initialization is configured before requests are processed.

No explicit readiness probes or configurable dependency wait mechanisms were found in the target project configuration.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `CatalogDBContext` connection string | Database credentials | Web.config builders; literal credential portion masked in the project file |
| `StorageConnectionString` | Azure Storage credential | Placeholder in Web.config; resolved through builders |
| `AzureKeyVault` / `KeyVaultName` | Secret-store reference | Optional Key Vault configuration builder |
| User Secrets identifier | Local development secret source | Identifier configured; secret values external |

### Secrets Provisioning Workflow

The configuration builder chain can resolve settings from user secrets, environment variables, and optional Azure Key Vault. The project contains no actual credential values in the inspected configuration. Identity and Key Vault access provisioning is supplied by the host environment and is not defined in the Web Forms project.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `true` | Web.config/environment |
| `UseCustomizationData` | `false` | Web.config/environment |
| `UseAzureManagedIdentity` | `false` | Web.config/environment |
| `UseAzureStorage` | `false` | Web.config/environment |
| `UseAzureActiveDirectory` | `false` | Web.config/environment |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET Framework | 4.7.2 | Project and Web.config |
| ASP.NET Web Forms | System.Web framework | Project references |
| Entity Framework | 6.3.0 | packages.config |
| Autofac | 4.9.4 | packages.config |
| log4net | 2.0.10 | packages.config |
| Application Insights | 2.11.x | packages.config |
| Azure Storage | 9.3.3 | packages.config |
| NuGet package count | 147 | packages.config |
