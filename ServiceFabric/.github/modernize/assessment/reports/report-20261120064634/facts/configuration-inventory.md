# Configuration & Externalized Settings Inventory

The scoped workspace contains Service Fabric application manifests, environment parameter files, publish profiles, package metadata, and deployment scripts. Local and Cloud profiles are present; credential-like values are masked below and should be supplied through controlled deployment configuration.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Service Fabric solution | Solution | `eShopModernizedMVC_ServiceFabricApp.sln` | Contains the MVC, SQL, and Web Forms deployment projects |
| Service Fabric project files | Build configuration | `eShopModernizedMVC-ServiceFabricApp/*.sfproj`, `eShopModernizedSQL_ServiceFabricApp/*.sfproj`, `eShopModernizedWebForms-ServiceApp/*.sfproj` | Legacy deployment project format; Debug and Release configurations |
| Application manifests | Deployment configuration | Each service project's `ApplicationPackageRoot/ApplicationManifest.xml` | Declares service package imports, parameters, DNS names, instance counts, and container port bindings |
| Service manifests | Container configuration | Each service project's `ApplicationPackageRoot/*/ServiceManifest.xml` | Declares container image, environment variables, and service endpoint |
| Settings files | Service Fabric settings | Each service package's `Config/Settings.xml` | Contains a placeholder configuration section and parameter |
| Environment parameter files | Runtime configuration | `ApplicationParameters/Local.1Node.xml`, `Local.5Node.xml`, and `Cloud.xml` | Local and Cloud deployment values |
| Publish profiles | Deployment profile | `PublishProfiles/Local.1Node.xml`, `Local.5Node.xml`, and `Cloud.xml` | Selects matching application parameter files and deployment settings |
| Package metadata | NuGet package configuration | `packages.config` in each project directory | Pins `Microsoft.VisualStudio.Azure.Fabric.MSBuild` 1.6.1 |
| Deployment scripts | Deployment automation | Each project `Scripts/Deploy-FabricApplication.ps1` and ARM deployment scripts | Orchestrates package publishing or Azure resource deployment |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | MSBuild `Debug` configuration | Local/debug deployment build | Service Fabric MSBuild package |
| Release | MSBuild `Release` configuration | Release deployment package | Service Fabric MSBuild package |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Local 1 Node | Local publish profile | `PublishProfiles/Local.1Node.xml`, `ApplicationParameters/Local.1Node.xml` | One-node cluster deployment parameters |
| Local 5 Nodes | Local publish profile | `PublishProfiles/Local.5Node.xml`, `ApplicationParameters/Local.5Node.xml` | Five-node cluster deployment parameters |
| Cloud | Cloud publish profile | `PublishProfiles/Cloud.xml`, `ApplicationParameters/Cloud.xml` | Cloud deployment parameters; storage connection value is a placeholder |

## Properties Inventory

### MVC and Web Forms services

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `ConnectionString` | `[MASKED]` | Manifest default | Container service manifest; connects to the SQL service DNS endpoint |
| `UseMockData` | `False` | Manifest default | Container service manifest |
| `UseCustomizationData` | `False` | Manifest default | Container service manifest |
| `UseAzureStorage` | `False` | Manifest/application parameter | Container and application manifests |
| `StorageConnectionString` | Empty / `[MASKED]` | Application parameter and Cloud parameter file | Container and application manifests; Cloud value is a placeholder |
| `AppInsightsInstrumentationKey` / `InstrumentationKey` | Empty | Application parameter | Container and application manifests |
| `UseAzureActiveDirectory` | `False` | Application parameter | Container and application manifests |
| `AzureActiveDirectoryClientId` | Empty | Application parameter | Container and application manifests |
| `AzureActiveDirectoryTenant` | Empty | Application parameter | Container and application manifests |
| `PostLogoutRedirectUri` | Empty application parameter; manifest has a local address | Local/Cloud parameter files | Container and application manifests |

### SQL service

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `SA_PASSWORD` | `[MASKED]` | Manifest default | SQL container service manifest; credential is hard-coded in the scoped manifest |
| `ACCEPT_EULA` | `Y` | Manifest default | SQL container service manifest |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| MVC container | Not specified | Not specified | Default `-1` |
| Web Forms container | Not specified | Not specified | Default `-1` |
| SQL Server container | Not specified | Not specified | Default `1` |

No CPU or memory limits, JVM options, or container resource allocations were found in the scoped manifests.

## Startup Dependency Chain

1. Service Fabric deploys the application and its service packages from the application manifest.
2. MVC and Web Forms manifests declare SQL connections through the `eshopmodernized.sqldata` service DNS name on TCP port 1433.
3. No explicit readiness probe, wait-for mechanism, or application-level startup timeout was identified in the scoped deployment files.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| SQL Server `SA_PASSWORD` | Database credential | Literal value in `eShopModernizedSQL_ServiceFabricApp/ApplicationPackageRoot/SQL_ContainerPkg/ServiceManifest.xml` (`[MASKED]`) |
| MVC / Web Forms `ConnectionString` | Database connection string | Literal manifest values (`[MASKED]`) |
| Cloud `StorageConnectionString` | Storage credential placeholder | Placeholder in Cloud parameter files (`[MASKED]`) |
| ARM deployment credential parameters | Deployment credentials | ARM template parameters; values are represented as parameters or placeholders |

### Secrets Provisioning Workflow

The SQL and application manifests provide fixed or placeholder values as container environment variables, while the Cloud profile carries an externalized storage connection placeholder. No Key Vault, Vault, managed identity, or other secret-store binding was identified in the scoped Service Fabric files. Actual secret values are intentionally omitted from this inventory.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseMockData` | `False` | MVC and Web Forms container manifest |
| `UseCustomizationData` | `False` | MVC and Web Forms container manifest |
| `UseAzureStorage` | `False` | Application parameter |
| `UseAzureActiveDirectory` | `False` | Application parameter |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| MVC Service Fabric project target | .NET Framework 4.6.1 | `eShopModernizedMVC_ServiceFabricApp.sfproj` |
| SQL Service Fabric project target | .NET Framework 4.6.1 | `eShopModernizedSQL_ServiceFabricApp.sfproj` |
| Web Forms Service Fabric project target | .NET Framework 4.6.2 | `eShopModernizedWebFormsSF.sfproj` |
| Service Fabric application project format | 1.7 | Each `.sfproj` file |
| Service Fabric MSBuild package | 1.6.1 | Each `packages.config` |
| SQL Server container image | Windows Developer 2017 | SQL `ServiceManifest.xml` |
| MVC / Web Forms container images | Image tag not specified | Corresponding `ServiceManifest.xml` files |
