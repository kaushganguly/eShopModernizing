# Configuration & Externalized Settings Inventory

The deployment scope contains Service Fabric manifests, deployment parameter sets, publish profiles, and ARM deployment scripts. Application runtime configuration sources are outside this workspace scope; sensitive values are intentionally redacted.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Service manifests | XML | Each application package under `ServiceFabric/*/ApplicationPackageRoot/*_ContainerPkg/ServiceManifest.xml` | Container image, environment variables, endpoint definitions |
| Application manifests | XML | Each application package under `ApplicationPackageRoot/ApplicationManifest.xml` | Deployment parameters, service DNS names, environment overrides, instance counts |
| Application parameters | XML | `ApplicationParameters/Cloud.xml`, `Local.1Node.xml`, `Local.5Node.xml` per app | Local and cloud deployment substitutions |
| Publish profiles | XML | `PublishProfiles/Cloud.xml`, `Local.1Node.xml`, `Local.5Node.xml` per app | Publish behavior and target parameters |
| Package manifests | XML | `packages.config` in each `.sfproj` directory | Fabric MSBuild dependency declaration |
| ARM deployment helpers | C# | `ServiceFabric-ARM-Templates-Scripts/*/DeploymentHelper.cs` | Reads local template and parameter JSON paths |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | Solution build configuration | Debug deployment project build | Service Fabric tooling; project-specific details unavailable |
| Release | Solution build configuration | Release deployment project build | Service Fabric tooling; project-specific details unavailable |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Local.1Node | Service Fabric application parameter set | `ApplicationParameters/Local.1Node.xml` | Local cluster settings |
| Local.5Node | Service Fabric application parameter set | `ApplicationParameters/Local.5Node.xml` | Local multi-node settings |
| Cloud | Service Fabric application parameter set and publish profile | `ApplicationParameters/Cloud.xml`, `PublishProfiles/Cloud.xml` | Storage, telemetry, identity, and instance-count parameter substitutions |

## Properties Inventory

| Service | Property / Variable | Default or Source | Profiles | Type |
|---|---|---|---|---|
| MVC / Web Forms | `UseMockData`, `UseCustomizationData` | `False` | Manifest defaults | Boolean-like string |
| MVC / Web Forms | `UseAzureStorage`, `StorageConnectionString` | Disabled / blank in service manifest; cloud substitution placeholder | Cloud and local | Feature flag / sensitive connection setting (value redacted) |
| MVC / Web Forms | `UseAzureActiveDirectory`, client ID, tenant, post-logout URI | Disabled / blank or local URI; cloud parameter placeholders | Cloud and local | Feature flag / identity configuration |
| MVC / Web Forms | `AppInsightsInstrumentationKey` | Blank by default; cloud parameter placeholder | Cloud and local | Sensitive telemetry setting (value not included) |
| SQL | `ACCEPT_EULA` | `Y` | Service manifest | Environment variable |
| SQL | SQL administrator password | Hard-coded manifest value (redacted) | Service manifest | Sensitive credential |
| SQL | SQL endpoint | 1433/TCP | Service manifest | Port |

## Startup Parameters & Resource Requirements

| Service | Runtime Options | Memory / CPU | Instance Count |
|---|---|---|---|
| MVC container | Environment variables from manifest parameters | Not specified | Default `-1` |
| Web Forms container | Environment variables from manifest parameters | Not specified | Default `-1` |
| SQL container | EULA acceptance and administrator password (redacted) | Not specified | 1 |

## Startup Dependency Chain

1. The SQL service is configured to provide `eshopmodernized.sqldata:1433`.
2. MVC and Web Forms manifests configure database connections to that DNS target.
3. No explicit readiness probe, startup wait, or dependency ordering is declared in the inspected application manifests.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| SQL administrator credential | Password | Literal in SQL service manifest; value redacted |
| SQL connection credential | Database credential | Connection settings in the MVC and Web Forms service manifests; password omitted |
| Storage connection string | Cloud storage credential | Cloud parameter substitution placeholder; value omitted |
| Application Insights instrumentation key | Telemetry credential | Cloud parameter substitution placeholder; value omitted |

### Secrets Provisioning Workflow

The manifests do not declare a secret store, managed identity, or encrypted-secret mechanism. Cloud parameters contain placeholders for selected credentials; the SQL service manifest contains a literal administrator credential (not reproduced here). No provisioning or rotation workflow is described in the scoped files.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | False | Container environment variable |
| `UseCustomizationData` | False | Container environment variable |
| `UseAzureStorage` | False | Application parameter and environment override |
| `UseAzureActiveDirectory` | False | Application parameter and environment override |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| Service Fabric MSBuild package | 1.6.1 | `packages.config` in each deployment project |
| Package target framework metadata | .NET Framework 4.6.1 (MVC, SQL); 4.6.2 (Web Forms) | `packages.config` |
| SQL Server Developer image | 2017 | SQL `ServiceManifest.xml` |
| MVC / Web Forms image tags | Not specified | Service manifests |