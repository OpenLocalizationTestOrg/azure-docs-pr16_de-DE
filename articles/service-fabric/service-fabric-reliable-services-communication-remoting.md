<properties
   pageTitle="Dienst Remote-Dienst Struktur | Microsoft Azure"
   description="Remote-Dienst Fabric ermöglicht Clients und Dienste mit den Diensten Kommunikation mithilfe eines Remoteprozeduraufrufs."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="BharatNarasimman"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="service-remoting-with-reliable-services"></a>Dienst Remote mit zuverlässigen Diensten
Für Dienste, die nicht mit einer bestimmten Communication Protocol oder Stapel, beispielsweise WebAPI, Windows Communication Foundation (WCF) oder andere, verknüpft sind bietet zuverlässigen Services Framework einer Remotemechanismus Remoteprozeduraufruf für Dienste schnell und einfach einrichten.

## <a name="set-up-remoting-on-a-service"></a>Einrichten von Remote auf einem Dienst
Einrichten von Remote für einen Dienst geschieht in zwei Schritten:

1. Erstellen Sie eine Benutzeroberfläche für Ihren Dienst implementiert wird. Diese Schnittstelle definiert die Methoden, die für einen Remoteprozeduraufruf auf dem Dienst zur Verfügung stehen. Die Methoden muss Vorgang zurückgeben asynchrone Methoden. Die Benutzeroberfläche muss implementieren `Microsoft.ServiceFabric.Services.Remoting.IService` um darauf hinzuweisen, dass der Dienst eine Remotingschnittstelle hat.
2. Verwenden einer Remote Zuhörer in Ihrem Dienst an. Dies ist ein `ICommunicationListener` Implementierung, Remote-Funktionen bereitstellt. Die `Microsoft.ServiceFabric.Services.Remoting.Runtime` Namespace verfügt über eine Erweiterungsmethode `CreateServiceRemotingListener` für statusfreie und dynamische Dienste, die zum Erstellen einer Remote-Zuhörer mit dem standardmäßigen Remote Transportregel-Protokoll verwendet werden können.

Der folgende statusfreie Dienst stellt beispielsweise eine einzelne Methode, wenn Sie über ein remote Procedure Call "Hallo Welt" zu erhalten.

```csharp
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Remoting;
using Microsoft.ServiceFabric.Services.Remoting.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

public interface IMyService : IService
{
    Task<string> HelloWorldAsync();
}

class MyService : StatelessService, IMyService
{
    public MyService(StatelessServiceContext context)
        : base (context)
    {
    }

    public Task HelloWorldAsync()
    {
        return Task.FromResult("Hello!");
    }

    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] { new ServiceInstanceListener(context => 
            this.CreateServiceRemotingListener(context)) };
    }
}
```
> [AZURE.NOTE] Die Argumente und der Rückgabetyp Typen die Benutzeroberfläche Dienst können eine einfachen, komplexen oder benutzerdefinierten Typen sein, jedoch nur durch die .NET [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx)serialisierbar.


## <a name="call-remote-service-methods"></a>Rufen Sie Remotedienst Methoden
Aufrufen von Methoden für einen Dienst mithilfe des Remote-Stapels abgeschlossen ist, mit einem lokalen Proxy dem Dienst durch die `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` Class. Die `ServiceProxy` Methode erstellt einen lokalen Proxy mithilfe der gleichen Benutzeroberfläche, die den Dienst implementiert. Mit diesen Proxy können Sie einfach Methoden für die Benutzeroberfläche Remote aufrufen.


```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

Remotingframework wird bei dem Dienst an dem Client ausgelösten Ausnahmen weitergegeben. Daher Handling von Ausnahmen Logik auf dem Client mithilfe von `ServiceProxy` Ausnahmen, die der Dienst auslöst können direkt behandeln.

## <a name="next-steps"></a>Nächste Schritte

* [Web-API mit OWIN zuverlässigen-Dienste](service-fabric-reliable-services-communication-webapi.md)

* [WCF-Kommunikation mit zuverlässigen Diensten](service-fabric-reliable-services-communication-wcf.md)

* [Sichern der Kommunikation für zuverlässigen Dienste](service-fabric-reliable-services-secure-communication.md)
