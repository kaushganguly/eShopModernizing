# Dependency Map

```mermaid
graph TD
    WF[eShopWinForms.csproj<br/>net6.0-windows] --> EF1[EntityFramework 6.4.4]
    WF --> WAC[Microsoft.AspNet.WebApi.Client 5.2.7]
    WAC --> NJ[Newtonsoft.Json 6.0.4 - transitive]
    WF --> SMD[System.ServiceModel.Duplex 4.8.0]
    WF --> SMH[System.ServiceModel.Http 4.8.0]
    WF --> SMN[System.ServiceModel.NetTcp 4.8.0]
    WF --> SMS[System.ServiceModel.Security 4.8.0]
    WF -. "WCF service reference (Connected Services)" .-> SVC[eShopWCFService.csproj<br/>net461]
    SVC --> EF2[EntityFramework 6.1.3]
    SVC --> FW[.NET Framework: System.ServiceModel, System.Data, System.Web]
    SVC --> DB[(SQL Server)]
```

| Project | Target | Packages |
|---------|--------|----------|
| eShopWCFService | .NET Framework 4.6.1 (non-SDK) | EntityFramework 6.1.3 |
| eShopWinForms | net6.0-windows (SDK) | EntityFramework 6.4.4, Microsoft.AspNet.WebApi.Client 5.2.7, System.ServiceModel.{Duplex,Http,NetTcp,Security} 4.8.0 |
| eShopWinForms.fx | .NET Framework 4.7.1 variant of the client | see project file |

There is no project reference between client and service; the client consumes the service through a generated WCF proxy (`Connected Services/eShopServiceReference/Reference.cs`).
