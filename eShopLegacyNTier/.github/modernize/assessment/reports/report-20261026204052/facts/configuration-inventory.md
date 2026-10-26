# Configuration & Externalized Settings Inventory

Configuration is held in two XML files (`Web.config` for the service, `App.config` for the client) plus one environment variable; there are no secrets stores or feature flags.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|--------|------|---------------|-------|
| Web.config | XML | eShopLegacyNTier/src/eShopWCFService/Web.config | WCF, EF, connection string |
| Web.Debug.config / Web.Release.config | XML transform | eShopLegacyNTier/src/eShopWCFService/ | Build-time transforms |
| App.config | XML | eShopLegacyNTier/src/eShopWinForms/App.config | WCF client endpoint, runtime |
| ConnectionString | Environment variable | process environment | Overrides the DB connection |
| Settings.settings | Application settings | eShopLegacyNTier/src/eShopWinForms/Properties/ | Client settings designer |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---------|-----------|---------|--------------------------|
| Debug | default MSBuild configuration | Local development | Web.Debug.config transform |
| Release | `/p:Configuration=Release` | Deployment | Web.Release.config transform |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---------|------------------|--------------|---------------|
| Default | n/a | Web.config, App.config | Connection string can be overridden by `ConnectionString` env var |

## Properties Inventory

### eShopWCFService

| Property Key | Default | Profiles | Source |
|--------------|---------|----------|--------|
| aspnet:UseTaskFriendlySynchronizationContext | true | all | Web.config appSettings |
| EntityModel connection string | (localdb)\MSSQLLocalDB, eShopDatabase | all | Web.config connectionStrings |
| ConnectionString | unset | all | Environment variable |
| serviceMetadata httpGetEnabled / httpsGetEnabled | true / true | all | Web.config serviceBehaviors |
| serviceDebug includeExceptionDetailInFaults | false | all | Web.config |
| directoryBrowse | enabled | all | Web.config system.webServer |
| compilation debug | true | all | Web.config |
| aspNetCompatibilityEnabled | true | all | Web.config |
| defaultConnectionFactory | LocalDbConnectionFactory (mssqllocaldb) | all | Web.config entityFramework |

### eShopWinForms

| Property Key | Default | Profiles | Source |
|--------------|---------|----------|--------|
| client endpoint address | http://localhost:62314/CatalogService.svc | all | App.config |
| binding | basicHttpBinding | all | App.config |
| supportedRuntime | .NETFramework v4.7 | all | App.config |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---------|---------------------|--------|----------------|
| eShopWCFService | IIS / IIS Express, .NET Framework 4.6.1 | not specified | 1 |
| eShopWinForms | .NET Framework 4.7 desktop | not specified | 1 per user |

## Startup Dependency Chain

1. SQL Server LocalDB available (database auto-created and seeded on first request)
2. eShopWCFService running on port 62314
3. eShopWinForms starts, then calls the service on load (no wait or retry mechanism)

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|------------------|------|------------------|
| EntityModel connection string | Database connection (integrated, no credentials) | Web.config plain text |
| ConnectionString | Database connection | Environment variable |

### Secrets Provisioning Workflow

No secret management exists. The default connection uses Windows integrated access to LocalDB; an alternative connection string can be supplied through the environment variable.

## Feature Flags

No feature flags detected.

## Framework & Runtime Versions

| Component | Version | Source |
|-----------|---------|--------|
| .NET Framework (service) | 4.6.1 | eShopWCFService.csproj |
| .NET Framework (client) | 4.7 | eShopWinForms.csproj |
| Entity Framework | 6.1.3 | packages.config |
| Newtonsoft.Json | 6.0.4 | eShopWinForms packages.config |
| Microsoft.AspNet.WebApi.Client | 5.2.3 | eShopWinForms packages.config |
| Solution format | Visual Studio 2017 (v15) | eShopLegacyNTier.sln |
