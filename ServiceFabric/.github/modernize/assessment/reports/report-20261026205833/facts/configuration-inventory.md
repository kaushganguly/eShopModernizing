# Configuration & Externalized Settings Inventory

The subtree contains three Service Fabric application packages, per-environment parameter and publish profiles, and cluster ARM templates. Most app-level settings are manifest environment values or deployment parameters; the SQL container manifest includes a literal administrative password, redacted below.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Service manifests | XML | Each `ApplicationPackageRoot/*_ContainerPkg/ServiceManifest.xml` | Container image, endpoint, and environment declarations |
| Application manifests | XML | Each `ApplicationPackageRoot/ApplicationManifest.xml` | Service DNS name, instance count, port binding, and environment overrides |
| Settings | XML | Each `ApplicationPackageRoot/*_ContainerPkg/Config/Settings.xml` | Empty Service Fabric configuration templates |
| Application parameters | XML | Each service's `ApplicationParameters/Cloud.xml`, `Local.1Node.xml`, and `Local.5Node.xml` | Cloud placeholders and local deployment profiles |
| Publish profiles | XML | Each service's `PublishProfiles/Cloud.xml`, `Local.1Node.xml`, and `Local.5Node.xml` | Cluster deployment connection settings |
| NuGet declarations | XML | Three `packages.config` files | Service Fabric MSBuild package version 1.6.1 |
| Cluster templates | JSON | `ServiceFabric-ARM-Templates-Scripts/**/template.json`, `azuredeploy.json`, and parameter files | Azure cluster/resource provisioning inputs |
| Deployment scripts | PowerShell | `Scripts/Deploy-FabricApplication.ps1` in each service project and ARM helper folders | Publish/deploy automation |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug x64 | MSBuild configuration | Build/deploy Service Fabric application package | Fabric MSBuild 1.6.1 |
| Release x64 | MSBuild configuration | Build/deploy Service Fabric application package | Fabric MSBuild 1.6.1 |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Cloud | Selected publish profile | Each `ApplicationParameters/Cloud.xml` and `PublishProfiles/Cloud.xml` | Cloud deployment values are parameterized; MVC/Web Forms expose storage, instrumentation, identity, and redirect parameters |
| Local 1 node | Selected local publish profile | Each `ApplicationParameters/Local.1Node.xml` and `PublishProfiles/Local.1Node.xml` | Single-node Service Fabric deployment; no app parameter overrides found |
| Local 5 nodes | Selected local publish profile | Each `ApplicationParameters/Local.5Node.xml` and `PublishProfiles/Local.5Node.xml` | Five-node Service Fabric deployment; no app parameter overrides found |

## Properties Inventory

### MVC_Container

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| ConnectionString | SQL DNS `eshopmodernized.sqldata:1433`, catalog database; credential masked | Service manifest | `ServiceManifest.xml` environment variable |
| UseAzureStorage | False | Cloud parameter override available | Application and service manifests |
| StorageConnectionString | Empty | Cloud parameter override available | Application manifest |
| AppInsightsInstrumentationKey | Empty | Cloud parameter override available | Application manifest |
| UseAzureActiveDirectory | False | Cloud parameter override available | Application and service manifests |
| AzureActiveDirectoryClientId / Tenant | Empty | Cloud parameter override available | Application manifest |
| PostLogoutRedirectUri | Empty in app parameter; local value in service manifest | Cloud/local | Application and service manifests |

### WebForms_Container

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| ConnectionString | SQL DNS `eshopmodernized.sqldata:1433`, catalog database; credential masked | Service manifest | `ServiceManifest.xml` environment variable |
| UseMockData | False | Service manifest | Environment variable |
| UseCustomizationData | False | Service manifest | Environment variable |
| UseAzureStorage | False | Cloud parameter override available | Application and service manifests |
| StorageConnectionString / AppInsightsInstrumentationKey | Empty | Cloud parameter override available | Application manifest |
| UseAzureActiveDirectory | False | Cloud parameter override available | Application and service manifests |
| AzureActiveDirectoryClientId / Tenant | Empty | Cloud parameter override available | Application manifest |

### SQL_Container

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| SA_PASSWORD | [MASKED] | Service manifest | SQL container environment variable; literal value is present in the deployment manifest |
| ACCEPT_EULA | Y | Service manifest | SQL container environment variable |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| MVC_Container | None declared | Not specified | Default -1 (all nodes) |
| WebForms_Container | None declared | Not specified | Default -1 (all nodes) |
| SQL_Container | None declared | Not specified | Default 1 |

## Startup Dependency Chain

1. Deploy SQL_Container to provide the `eshopmodernized.sqldata` DNS name and TCP 1433 endpoint.
2. Deploy MVC_Container and WebForms_Container, whose connection settings reference that SQL service.

The manifests do not define a health check, readiness wait, or explicit dependency ordering; this sequence is inferred from the configured SQL connection.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| SQL container `SA_PASSWORD` | Database administrator password | Literal in `SQL_ContainerPkg/ServiceManifest.xml`; [MASKED] |
| Web application SQL connection | Database credential | Connection string in MVC and Web Forms manifests; password is masked in this report |
| Azure AD and instrumentation settings | Client/telemetry values | Empty or parameterized; no populated secret store reference found |

### Secrets Provisioning Workflow

No Key Vault, Vault, or cloud secret-store integration is declared in this subtree. The SQL administrative password is supplied directly by the SQL service manifest, while app connection strings are embedded in the web service manifests. Azure Active Directory and Application Insights values are exposed as deployment parameters but disabled or empty by default. The SQL password is redacted from this report; see the security assessment for the finding.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| UseAzureStorage | False | Application parameters / environment variable |
| UseAzureActiveDirectory | False | Application parameters / environment variable |
| UseMockData | False where declared | Web Forms service environment |
| UseCustomizationData | False | Web Forms service environment |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| MVC Service Fabric deployment project | .NET Framework 4.6.1 | `.sfproj` |
| SQL Service Fabric deployment project | .NET Framework 4.6.1 | `.sfproj` |
| Web Forms Service Fabric deployment project | .NET Framework 4.6.2 | `.sfproj` |
| Service Fabric MSBuild package | 1.6.1 | `packages.config` |
| SQL container | SQL Server Windows Developer 2017 | Service manifest image tag |
| MVC and Web Forms containers | Image versions not specified | Service manifest image names |
