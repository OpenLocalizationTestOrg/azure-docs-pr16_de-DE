<properties
   pageTitle="Übersicht über den zuverlässigen Services Communication | Microsoft Azure"
   description="Übersicht des zuverlässigen Services Kommunikation dar, einschließlich der öffnenden Listener auf Dienste, Auflösen von Endpunkten und Kommunikation zwischen Diensten."
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

# <a name="how-to-use-the-reliable-services-communication-apis"></a>So verwenden Sie die Kommunikation zuverlässigen Services APIs

Azure Service-Struktur als Plattform ist vollständig unabhängig Kommunikation zwischen Services. Alle Protokolle und Stapel sind annehmbar, von UDP auf HTTP. Es ist von dem Dienst Entwickler auswählen, wie Dienste kommunizieren soll. Zuverlässigen Services Application Framework bietet integrierte Kommunikation Stapel als auch APIs, die Sie verwenden können, um Ihre benutzerdefinierten Kommunikationskomponenten zu erstellen. 

## <a name="set-up-service-communication"></a>Einrichten der Kommunikation mit service

Zuverlässigen Webdienste-API verwendet eine einfache Schnittstelle zum Dienst kommunizieren. Implementieren Sie diese Schnittstelle einfach, um einen Endpunkt des Diensts zu öffnen:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

Anschließend können Sie Ihre Kommunikation Zuhörer Implementierung hinzufügen, indem Sie es in eine Methode zum Überschreiben Service-basierte Klasse zurückgeben.

Für zustandsloser Dienste:

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```

Für dynamische Dienste:

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

In beiden Fällen kehren Sie eine Auflistung von Listenern zurück. Dadurch wird den Dienst für mehrere Endpunkte, potenziell mithilfe verschiedener Protokolle, mehrere Listener abhören. Angenommen, Sie eine HTTP-Zuhörer und einer separaten WebSocket Zuhörer möglicherweise. Jede Zuhörer erhält einen Namen, und der resultierende-Auflistung des *Namen: Adresse* -Paare als JSON-Objekt dargestellt wird, wenn ein Client die Adressen für eine Instanz oder eine Partition anfordert.

In einem statusfreie-Dienst gibt die Außerkraftsetzung eine Auflistung von ServiceInstanceListeners. Eine ServiceInstanceListener enthält eine Funktion zum Erstellen einer ICommunicationListener und weist ihr einen Namen. Für dynamische Dienste gibt die Außerkraftsetzung eine Auflistung von ServiceReplicaListeners. Dies unterscheidet sich leicht von statusfreie Gegenstück, da eine ServiceReplicaListener eine Option zum Öffnen einer ICommunicationListener auf sekundäre Replikate verfügt. Nicht nur können Sie mehrere Kommunikation Listener in einem Dienst verwenden, aber Sie können auch angeben, welche Listener Anfragen auf sekundäre Replikate akzeptieren und, welche nur primäre Replikate zu überwachen.

Beispielsweise können Sie eine ServiceRemotingListener, die RPC Anrufe nur auf dem primären Replikate akzeptiert haben, und eine zweite, benutzerdefinierte Zuhörer, die gelesen akzeptiert anfordert auf sekundäre Replikate über HTTP:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [AZURE.NOTE] Wenn Sie mehrere Listener für einen Dienst zu erstellen, werden jede Zuhörer **muss** angegeben, einen eindeutigen Namen.

Beschreiben Sie abschließend die Endpunkte, die für den Dienst in der [Dienst manifest](service-fabric-application-model.md) unter dem Abschnitt auf Endpunkten erforderlich sind.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

Die Kommunikation Zuhörer die Endpunkt Ressourcen reserviert aus zugreifen kann die `CodePackageActivationContext` in der `ServiceContext`. Die Zuhörer kann dann beginnen Anfragen abhören, wenn es geöffnet wird.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```

> [AZURE.NOTE] Endpunkt Ressourcen gelten für das gesamte Service-Paket, und sie werden vom Dienst Fabric reserviert, wenn das Service-Paket aktiviert ist. Mehrere Dienst Replikate in der gleichen ServiceHost gehostet wird möglicherweise denselben Port verwenden. Dies bedeutet, dass die Kommunikation Zuhörer Portfreigabe unterstützen soll. Es wird empfohlen, der auf diese Weise für die Kommunikation Zuhörer die Partition-ID sowie Instanzreplikats/verwendet, beim Generieren von Listen Adresse.

### <a name="service-address-registration"></a>Adresse-Dienste registrieren

Ein Systemdienst namens *Naming Service* bei Dienst Fabric Cluster ausgeführt wird. Der DNS-Dienst ist Registrar für Dienste und deren Adressen, denen jeder Instanz oder eine Kopie der Dienst überwacht. Wenn die `OpenAsync` Methode zum einer `ICommunicationListener` abgeschlossen ist, handelt es sich bei seiner Rückkehr Wert in der DNS-Dienst registriert wird. Dieser Rückgabewert, die in der DNS-Dienst veröffentlicht ruft ist eine Zeichenfolge zurück, wobei die Werte nichts gar werden kann. Dieser Zeichenfolgenwert ist, was Clients angezeigt werden, wenn sie eine Adresse für den Dienst von DNS-Dienst anfordern.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);
                        
    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
            
    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));
    
    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```

Dienst Fabric enthält APIs, die Clients und andere Dienste für diese Adresse Dienst namentlich bitten Sie können. Dies ist wichtig, weil die Adresse Dienst nicht statisch ist. Dienste werden im Cluster für die Ressource Lastenausgleich und Verfügbarkeit Zwecke um übernommen. Dies ist das Verfahren, das zum Auflösen der überwachenden Adresse für einen Dienst Clients ermöglicht.

> [AZURE.NOTE] Für eine vollständige Exemplarische Vorgehensweise zum Schreiben einer `ICommunicationListener`, finden Sie unter [Service Fabric Web-API Services mit OWIN Self-hosting](service-fabric-reliable-services-communication-webapi.md)

## <a name="communicating-with-a-service"></a>Kommunikation mit einem Dienst
Die zuverlässigen Webdienste-API bietet die folgenden Bibliotheken zum Schreiben von Clients, die mit den Diensten kommunizieren.

### <a name="service-endpoint-resolution"></a>Service-Endpunkt Auflösung
Der erste Schritt zur Kommunikation mit einem Dienst ist Problembehebung Endpunktadresse die Partition oder eine Instanz des Diensts, die Sie sprechen möchten. Die `ServicePartitionResolver` Programm-Klasse ist ein einfacher Basiselement, mit dem Clients den Endpunkt eines Diensts zur Laufzeit bestimmen zu können. Die Vorgehensweise zum Bestimmen des Endpunkts eines Diensts wird in der Dienst Fabric Terminologie als die *Service-Endpunkts Auflösung*bezeichnet.

Verbindung zu Diensten in einem Cluster `ServicePartitionResolver` mit Standardeinstellungen erstellt werden können. Dies ist die empfohlene Verwendung in den meisten Fällen aus:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```

Verbindung zu Diensten in einem anderen Cluster, eine `ServicePartitionResolver` mit einer Reihe von Cluster Gateway Endpunkte erstellt werden können. Beachten Sie, dass Gateway Endpunkte einfach auf einen anderen Endpunkte zum Herstellen einer Verbindung mit dem gleichen Cluster sind. Beispiel:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Alternativ `ServicePartitionResolver` kann eine Funktion angegeben werden, zum Erstellen einer `FabricClient` intern verwenden: 
 
```csharp
public delegate FabricClient CreateFabricClientDelegate();
```

`FabricClient`ist das Objekt, das Kommunikation mit dem Dienst Fabric Cluster für verschiedene Management Vorgänge auf dem Cluster verwendet wird. Dies ist nützlich, wenn der gewünschte besser steuern, wie `ServicePartitionResolver` mit Ihren Cluster interagiert. `FabricClient`führt intern Zwischenspeichern und kostet im Allgemeinen zu erstellen, damit es wichtig ist, wiederverwenden `FabricClient` Instanzen so weit wie möglich. 

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```

Eine Auflösungsmethode wird dann zum Abrufen der Adresse einer Dienst oder Dienst für partitionierte Services.

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```

Eine Adresse Dienst behoben werden kann, einfach mithilfe eines `ServicePartitionResolver`, aber mehr Arbeit ist erforderlich, um sicherzustellen, dass die Adresse gelöst ordnungsgemäß verwendet werden kann. Ihren Kunden müssen ermitteln, ob der Verbindungsversuch aufgrund einer vorübergehenden Fehler fehlgeschlagen ist und wiederholt werden kann (z. B. Dienst verschoben oder ist vorübergehend nicht verfügbar), oder ein permanenter Fehler (z. B. Dienst wurde gelöscht oder die angeforderte Ressource nicht mehr vorhanden ist). Dienstinstanzen oder Replikate können von Knoten zu Knoten zu einem beliebigen Zeitpunkt mehrere Gründen navigieren. Der Dienst Adresse gelöst durch `ServicePartitionResolver` ist möglicherweise veraltete bis zu dem Zeitpunkt Ihrer Client-Code versucht, eine Verbindung herzustellen. In diesem Fall wird der Client müssen erneut die Adresse erneut zu beheben. Bereitstellen der vorherigen `ResolvedServicePartition` gibt an, dass die Auflösung und nicht einfach abrufen eine zwischengespeicherte Adresse versuchen Sie es erneut muss.

Normalerweise muss der Client-Code nicht arbeiten mit den `ServicePartitionResolver` direkt. Er erstellt und an die Kommunikation Client Factory in der zuverlässigen Webdienste-API weitergegeben werden. Die Factory verwenden die Auflösung intern, um ein Clientobjekt zu erzeugen, die zum Kommunizieren mit den Diensten verwendet werden können.

### <a name="communication-clients-and-factories"></a>Kommunikation-Clients und Factory

Die Kommunikation Factory-Bibliothek implementiert ein typische-Fehlerbehandlung "Wiederholen" Muster, die Wiederholung Verbindungen mit gelöst Endpunkte erleichtert. Die Factory-Bibliothek bietet das Verfahren "Wiederholen" aus, während Sie die Ereignishandler zurück zur Verfügung stellen.

`ICommunicationClientFactory`definiert die base Schnittstelle implementiert von einer Kommunikation Client Factory, die Clients erzeugt, die in einen Dienst Fabric Service kommunizieren zu können. Die Durchführung der CommunicationClientFactory hängt von den Kommunikationsstapel vom Dienst Fabric-Dienst verwendet werden, in denen möchte des Clients kommunizieren. Die zuverlässigen Services-API bietet eine `CommunicationClientFactoryBase<TCommunicationClient>`. Dies stellt eine base Implementierung der `ICommunicationClientFactory` Schnittstelle und Aufgaben, die alle Kommunikation Stapel feldbezogene ausführt. (Diese Aufgaben aufnehmen mit einer `ServicePartitionResolver` anhand den Endpunkt). Clients implementieren normalerweise die abstrakte CommunicationClientFactoryBase-Klasse, um die Logik zu behandeln, für die Kommunikationsstapel spezifisch ist.

Der Kommunikationsclient nur erhält eine Adresse und für die Verbindung zu einem Dienst verwendet. Der Client kann jeden Protokoll verwenden, werden sollen.

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```

Die Client Factory ist hauptsächlich Softwareentwickler Kommunikation Clients. Für Clients, die eine dauerhafte Verbindung, wie etwa ein HTTP-Client, halten nicht muss die Factory nur erstellen und den Kunden zurückzukehren. Andere Protokolle, die eine dauerhafte Verbindung, z. B. einige binäre Protokolle, halten sollten auch geprüft werden, von der Factory zu bestimmen, ob die Verbindung neu erstellt werden muss.  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```

Schließlich ist ein Ausnahme Ereignishandler Reponsible zum Bestimmen der Vorgehensweise zu erhalten, wenn eine Ausnahme auftritt. Ausnahmen sind in kategorisiert **wiederholbar** und **nicht wiederholbar**. 

 - **Nicht wiederholbar** Ausnahmen erhalten zurück, die einfach erneut ausgelöst. 
 - **Retriable** Ausnahmen sind in **vorübergehende** und **Dauerhaftes**weiteren kategorisiert.
  - **Vorübergehende** Ausnahmen sind diejenigen, die einfach wiederholt werden kann, ohne erneut die Dienstendpunktadresse aufzulösen. Diese enthalten vorübergehende Netzwerkprobleme oder Dienst Fehlerantworten als diejenigen, die darauf hinweisen, dass die Dienstendpunktadresse nicht vorhanden ist. 
  - **Nicht vorübergehend** Ausnahmen sind diejenigen, die die Dienstendpunktadresse erneut gelöst werden müssen. Diese Ausnahmen gehören, die darauf hinweisen, dass der Endpunkt nicht erreicht wurde, wird vom Dienst an einen anderen Knoten verschoben. 

Die `TryHandleException` entscheiden, eine bestimmte Ausnahme macht. Wenn sie **weiß nicht,** was Entscheidungen über eine Ausnahme vornehmen, es gibt **false**zurück. Wenn sie **weiß** was Entscheidung vornehmen, sollten sie das Ergebnis entsprechend festlegen und gibt **true**zurück.
 
```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
### <a name="putting-it-all-together"></a>Zusammenstellen der
Mit einer `ICommunicationClient`, `ICommunicationClientFactory`, und `IExceptionHandler` um ein Kommunikationsprotokoll aufgebaut einer `ServicePartitionClient` ist umschließt er überhaupt und stellt die Service-Fehlerbehandlung und die Adresse mit einer Auflösung von Schleife um diese Komponenten.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```

## <a name="next-steps"></a>Nächste Schritte
 - Sehen Sie ein Beispiel für HTTP-Kommunikation zwischen Dienste in einem [Projekt auf GitHUb Beispiel](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [Remoteprozeduraufruf Anrufe mit zuverlässigen Services Remote](service-fabric-reliable-services-communication-remoting.md)

 - [Web-API, OWIN zuverlässigen Dienste verwendet.](service-fabric-reliable-services-communication-webapi.md)

 - [WCF-Kommunikation mithilfe von zuverlässigen Diensten](service-fabric-reliable-services-communication-wcf.md)
