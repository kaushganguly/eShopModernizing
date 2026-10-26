# Configuration Inventory

| Setting | Location | Value / Notes |
|---------|----------|---------------|
| `EntityModel` connection string | eShopWCFService/Web.config | `Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=eShopDatabase;Persist Security Info=True;` (System.Data.SqlClient) |
| `ConnectionString` env var | CatalogConfiguration.cs, docker-compose.override.yml | Overrides the Web.config connection; compose value uses `sa` credentials in plain text |
| `aspnet:UseTaskFriendlySynchronizationContext` | Web.config appSettings | true |
| `compilation debug` | Web.config | `true`, targetFramework 4.6.1 |
| serviceMetadata | Web.config | `httpGetEnabled`/`httpsGetEnabled` true |
| serviceDebug includeExceptionDetailInFaults | Web.config | false |
| `directoryBrowse` | Web.config | enabled="true" (should be disabled for production) |
| WCF service endpoints | Web.config | basicHttpBinding, mexHttpBinding; https protocol mapping basicHttpsBinding |
| EF default connection factory | Web.config, App.config | LocalDbConnectionFactory (`mssqllocaldb`) |
| WCF client endpoint | eShopWinForms/App.config | `http://localhost:5113/CatalogService.svc` |
| DpiAwareness | eShopWinForms/App.config | PerMonitorV2 |
| Ports | docker-compose.override.yml | 5113:80 (service), 5433:1433 (SQL) |
| `SA_PASSWORD`, `ACCEPT_EULA` | docker-compose.override.yml | SQL container settings; password hard-coded |
| Docker network | docker-compose.override.yml | external `nat` |
| Base image | eShopWCFService/Dockerfile | `mcr.microsoft.com/dotnet/framework/wcf:4.7.2` |

Secrets are stored in plain text in configuration; externalize through a secret store (Key Vault / configuration builders) when moving to Azure.
