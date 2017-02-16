<properties
   pageTitle="Zuverlässigen WCF Services Kommunikationsstapel | Microsoft Azure"
   description="Der integrierte WCF Kommunikationsstapel Dienst Struktur bietet Client-Service-WCF-Kommunikation für zuverlässigen Dienste."
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/26/2016"
   ms.author="bharatn"/>

# <a name="wcf-based-communication-stack-for-reliable-services"></a>WCF-basierte Kommunikationsstapel für zuverlässigen Dienste
Das zuverlässigen Services Framework ermöglicht Autoren Dienst, den Kommunikationsstapel auswählen, den sie für den Dienst verwenden möchten. Sie können den Kommunikationsstapel ihrer Wahl über die **ICommunicationListener** zurückgegeben, die von den Methoden [CreateServiceReplicaListeners oder CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) anschließen. Das Framework bietet eine Implementierung des Stapels Kommunikation basierend auf der Windows Communication Foundation (WCF) Dienst Autoren, die WCF-basierte Kommunikation verwenden möchten.

## <a name="wcf-communication-listener"></a>WCF Kommunikation Zuhörer
Die WCF-spezifische Implementierung von **ICommunicationListener** wird von der Klasse **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** bereitgestellt.

Müssen Sie die sagen wir einen Servicevertrag vom Typ haben`ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

Wir können ein WCF-Kommunikation Zuhörer im Dienst auf folgende Weise erstellen.

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // that identifies the endpoint that the WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## <a name="writing-clients-for-the-wcf-communication-stack"></a>Schreiben von Clients für den Stapel der WCF-Kommunikation
Zum Schreiben von Clients mit den Diensten kommunizieren mithilfe von WCF, liefert den Rahmen **WcfClientCommunicationFactory**, also die WCF-spezifische Implementierung von [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

Der WCF-Kommunikationskanal kann aus der **WcfCommunicationClient** der **WcfCommunicationClientFactory**erstellte zugegriffen werden.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

Client-Code können die **WcfCommunicationClientFactory** zusammen mit den **WcfCommunicationClient** die **ServicePartitionClient** zum Bestimmen des Endpunkts und der Kommunikation mit dem Dienst implementiert.

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with the ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call the service to perform the operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
>[AZURE.NOTE] Die Standardeinstellung ServicePartitionResolver wird davon ausgegangen, dass der Client in demselben Cluster als Dienst ausgeführt wird. Wenn das heißt nicht der Fall, erstellen Sie ein ServicePartitionResolver Objekt, und die Endpunkte des Cluster Verbindung übergeben.

## <a name="next-steps"></a>Nächste Schritte
* [Remoteprozeduraufruf mit zuverlässigen Services Remote](service-fabric-reliable-services-communication-remoting.md)

* [Web-API mit OWIN zuverlässigen-Dienste](service-fabric-reliable-services-communication-webapi.md)

* [Sichern der Kommunikation für zuverlässigen Dienste](service-fabric-reliable-services-secure-communication.md)
