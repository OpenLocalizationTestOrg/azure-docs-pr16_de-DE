<properties
   pageTitle="Sicheren Kommunikation für Services Dienst Struktur Hilfe | Microsoft Azure"
   description="Übersicht über sichere Kommunikation für zuverlässigen Services helfen, die in einem Cluster Azure Service Fabric ausgeführt werden."
   services="service-fabric"
   documentationCenter=".net"
   authors="suchiagicha"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/06/2016"
   ms.author="suchiagicha"/>

# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a>Hilfe für Dienste in Azure Service Architektur sicheren Kommunikation

Sicherheit ist einer der wichtigsten Aspekte der Kommunikation. Application Framework zuverlässigen Services bietet ein paar vordefinierten Kommunikation Stapel und Tools, die Sie verwenden können, um die Sicherheit zu verbessern. In diesem Artikel wird erläutert, wie die Sicherheit zu verbessern, wenn Sie Remote Service und den Windows Communication Foundation (WCF) Kommunikationsstapel verwenden.

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a>Schützen Sie einen Dienst können, wenn Sie den Dienst Remote verwenden

Wir werden eines vorhandenen [Beispiel](service-fabric-reliable-services-communication-remoting.md) verwenden, die zum Einrichten der Remote für zuverlässigen Dienste erläutert. Um einen Dienst sichern, wenn Sie Remote Dienst verwenden, gehen Sie folgendermaßen vor:

1. Erstellen Sie eine Benutzeroberfläche, `IHelloWorldStateful`, definiert die Methoden, die für einen Remoteprozeduraufruf auf dem Dienst zur Verfügung stehen. Der Dienst wird verwendet `FabricTransportServiceRemotingListener`, der deklariert wird, der `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` Namespace. Dies ist ein `ICommunicationListener` Implementierung, Remote-Funktionen bereitstellt.

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```

2. Hinzufügen von Zuhörer Einstellungen und Sicherheitsanmeldeinformationen.

    Stellen Sie sicher, dass das Zertifikat, das Sie zum Schützen Ihrer Service-Kommunikation verwenden möchten, die auf allen Knoten im Cluster installiert ist. Es gibt zwei Methoden, um Sie Zuhörer Einstellungen und Sicherheitsanmeldeinformationen bereitstellen können:

    1. Bieten Sie sie direkt in den Code ein:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            FabricTransportListenerSettings listenerSettings = new FabricTransportListenerSettings
            {
                MaxMessageSize = 10000000,
                SecurityCredentials = GetSecurityCredentials()
            };
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
            };
        }

        private static SecurityCredentials GetSecurityCredentials()
        {
            // Provide certificate details.
            var x509Credentials = new X509Credentials
            {
                FindType = X509FindType.FindByThumbprint,
                FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
                StoreLocation = StoreLocation.LocalMachine,
                StoreName = "My",
                ProtectionLevel = ProtectionLevel.EncryptAndSign
            };
            x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
            return x509Credentials;
        }
        ```
    2. Bieten sie mithilfe eines [Config Paket](service-fabric-application-model.md):

        Hinzufügen eines `TransportSettings` Abschnitt in der Datei settings.xml.

        ```xml
        <!--Section name should always end with "TransportSettings".-->
        <!--Here we are using a prefix "HelloWorldStateful".-->
        <Section Name="HelloWorldStatefulTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509" />
            <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
            <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
            <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
            <Parameter Name="CertificateStoreName" Value="My" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
            <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
        </Section>
        ```

        In diesem Fall die `CreateServiceReplicaListeners` Methode wird wie folgt aussehen:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(
                        context,this,FabricTransportListenerSettings.LoadFrom("HelloWorldStateful")))
            };
        }
        ```

         Wenn Sie beim Hinzufügen eines `TransportSettings` Abschnitt in der Datei settings.xml ohne Präfix, `FabricTransportListenerSettings` werden alle Einstellungen standardmäßig aus diesem Abschnitt laden.

         ```xml
         <!--"TransportSettings" section without any prefix.-->
         <Section Name="TransportSettings">
             ...
         </Section>
         ```
         In diesem Fall die `CreateServiceReplicaListeners` Methode wird wie folgt aussehen:

         ```csharp
         protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
         {
             return new[]
             {
                 return new[]{
                         new ServiceReplicaListener(
                             (context) => new FabricTransportServiceRemotingListener(context,this))};
             };
         }
         ```

3. Wenn rufen Sie Methoden für eine gesicherte Dienst mithilfe des Remote-Stapels anstelle der `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` Klasse verwenden, um einen Dienstproxy erstellen `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. Übergeben Sie `FabricTransportSettings`, enthält die `SecurityCredentials`.

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");

    FabricTransportSettings transportSettings = new FabricTransportSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Wenn der Client-Code als Teil eines Diensts ausgeführt wird, können Sie laden `FabricTransportSettings` aus der Datei settings.xml. Erstellen Sie einen TransportSettings-Abschnitt, der ähnlich wie der Dienstcode wie zuvor gezeigt. Nehmen Sie die folgenden Änderungen an der Client-Code ein:

    ```csharp

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportSettings.LoadFrom("TransportSettingsPrefix")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Wenn der Client nicht als Teil eines Diensts ausgeführt wird, können Sie eine Datei client_name.settings.xml an derselben Stelle erstellen, wo finde ich die client_name.exe. Erstellen Sie dann einen Abschnitt TransportSettings in dieser Datei ein.

    Ähnlich wie der Dienst, wenn Sie Hinzufügen eines `TransportSettings` Abschnitt ohne Präfix im Client-settings.xml/client_name.settings.xml `FabricTransportSettings` werden alle Einstellungen standardmäßig aus diesem Abschnitt laden.

    In diesem Fall ist der frühere Code noch feiner vereinfacht:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## <a name="help-secure-a-service-when-youre-using-a-wcf-based-communication-stack"></a>Schützen Sie einen Dienst können, wenn Sie einen Stapel WCF-basierte Kommunikation verwenden

Wir werden eines vorhandenen [Beispiel](service-fabric-reliable-services-communication-wcf.md) verwenden, die erläutert, wie ein WCF-basierte Kommunikationsstapel für zuverlässigen Diensten einrichten. Um einen Dienst zu sichern, wenn Sie einen Stapel WCF-basierte Kommunikation verwenden, gehen Sie folgendermaßen vor:

1. Für den Dienst, müssen Sie die WCF-Kommunikation Zuhörer schützen (`WcfCommunicationListener`), die Sie erstellen. Ändern Sie hierzu die `CreateServiceReplicaListeners` Methode.

    ```csharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        return new[]
        {
            new ServiceReplicaListener(
                this.CreateWcfCommunicationListener)
        };
    }

    private WcfCommunicationListener<ICalculator> CreateWcfCommunicationListener(StatefulServiceContext context)
    {
       var wcfCommunicationListener = new WcfCommunicationListener<ICalculator>(
            serviceContext:context,
            wcfServiceObject:this,
            // For this example, we will be using NetTcpBinding.
            listenerBinding: GetNetTcpBinding(),
            endpointResourceName:"WcfServiceEndpoint");

        // Add certificate details in the ServiceHost credentials.
        wcfCommunicationListener.ServiceHost.Credentials.ServiceCertificate.SetCertificate(
            StoreLocation.LocalMachine,
            StoreName.My,
            X509FindType.FindByThumbprint,
            "9DC906B169DC4FAFFD1697AC781E806790749D2F");
        return wcfCommunicationListener;
    }

    private static NetTcpBinding GetNetTcpBinding()
    {
        NetTcpBinding b = new NetTcpBinding(SecurityMode.TransportWithMessageCredential);
        b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
        return b;
    }
    ```

2. Im Client der `WcfCommunicationClient` Klasse, die im vorherigen [Beispiel](service-fabric-reliable-services-communication-wcf.md) erstellt wurde, bleibt unverändert. Außer Kraft setzen müssen Sie jedoch die `CreateClientAsync` Methode zum `WcfCommunicationClientFactory`:

    ```csharp
    public class SecureWcfCommunicationClientFactory<TServiceContract> : WcfCommunicationClientFactory<TServiceContract> where TServiceContract : class
    {
        private readonly Binding clientBinding;
        private readonly object callbackObject;
        public SecureWcfCommunicationClientFactory(
            Binding clientBinding,
            IEnumerable<IExceptionHandler> exceptionHandlers = null,
            IServicePartitionResolver servicePartitionResolver = null,
            string traceId = null,
            object callback = null)
            : base(clientBinding, exceptionHandlers, servicePartitionResolver,traceId,callback)
        {
            this.clientBinding = clientBinding;
            this.callbackObject = callback;
        }

        protected override Task<WcfCommunicationClient<TServiceContract>> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
        {
            var endpointAddress = new EndpointAddress(new Uri(endpoint));
            ChannelFactory<TServiceContract> channelFactory;
            if (this.callbackObject != null)
            {
                channelFactory = new DuplexChannelFactory<TServiceContract>(
                this.callbackObject,
                this.clientBinding,
                endpointAddress);
            }
            else
            {
                channelFactory = new ChannelFactory<TServiceContract>(this.clientBinding, endpointAddress);
            }
            // Add certificate details to the ChannelFactory credentials.
            // These credentials will be used by the clients created by
            // SecureWcfCommunicationClientFactory.  
            channelFactory.Credentials.ClientCertificate.SetCertificate(
                StoreLocation.LocalMachine,
                StoreName.My,
                X509FindType.FindByThumbprint,
                "9DC906B169DC4FAFFD1697AC781E806790749D2F");
            var channel = channelFactory.CreateChannel();
            var clientChannel = ((IClientChannel)channel);
            clientChannel.OperationTimeout = this.clientBinding.ReceiveTimeout;
            return Task.FromResult(this.CreateWcfCommunicationClient(channel));
        }
    }
    ```

    Verwenden Sie `SecureWcfCommunicationClientFactory` Erstellen eines WCF-Clients Kommunikation (`WcfCommunicationClient`). Verwenden Sie den Client, Dienstmethoden aufzurufen.

    ```csharp
    IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();

    var wcfClientFactory = new SecureWcfCommunicationClientFactory<ICalculator>(clientBinding: GetNetTcpBinding(), servicePartitionResolver: partitionResolver);

    var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
        wcfClientFactory,
        ServiceUri,
        ServicePartitionKey.Singleton);

    var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
        client => client.Channel.Add(2, 3)).Result;
    ```

## <a name="next-steps"></a>Nächste Schritte

* [Web-API mit OWIN zuverlässigen-Dienste](service-fabric-reliable-services-communication-webapi.md)
