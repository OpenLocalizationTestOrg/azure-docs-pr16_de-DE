<properties 
    pageTitle="Lernprogramm Service Bus Relay | Microsoft Azure"
    description="Erstellen Sie einen Dienstbus Client Anwendung und dem Dienst Bus Relay verwenden."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-tutorial"></a>Lernprogramm Service Bus Relay

In diesem Lernprogramm beschrieben, wie eine einfache Dienstbus-Clientanwendung und Dienst mithilfe der Dienstbus "Relay" Funktionen zu erstellen. Ein entsprechendes Lernprogramm verwendet, die Dienstbus [vermittelten messaging](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging), finden Sie im [Service Bus vermittelte Messaging .NET Lernprogramm](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Dieses Lernprogramm bietet Ihnen einen Überblick über die Schritte, die zum Erstellen einer Dienstbus Client und Dienst Anwendungs erforderlich sind. Wie ihren Gegenstücken WCF ist ein Dienst ein erstellen, die eine oder mehrere Endpunkte verfügbar macht jeweils eine oder mehrere Service-Operationen verfügbar macht. Der Endpunkt eines Diensts gibt es sich um eine Adresse Stelle, an der der Dienst gefunden werden kann, eine Bindung, die die Informationen enthält, die ein Client kommunizieren muss, mit dem Dienst und dann einen Vertrag, der die Funktionen, die vom Dienst bereitgestellt werden, um seine Clients definiert. Der wichtigste Unterschied zwischen einem WCF und einem Dienstbus Dienst ist, dass der Endpunkt in der Cloud statt lokal auf Ihrem Computer verfügbar gemacht wird.

Nachdem Sie die Reihenfolge der Themen in diesem Lernprogramm durcharbeiten, müssen Sie einen laufenden Dienst und ein Client, der die Vorgänge des Diensts aufgerufen werden kann. Das erste Thema beschrieben, wie ein Konto einrichten. Die nächsten Schritte beschreiben so einen Dienst definieren, der einen Vertrag verwendet, wie den Dienst implementiert wird und so konfigurieren Sie den Dienst Code. Außerdem werden so hosten, und führen Sie den Dienst beschrieben. Der Dienst, der erstellt wurde ist lokal gehosteten, und der Client und Dienst auf demselben Computer ausführen. Sie können den Dienst mithilfe von Code oder eine Konfigurationsdatei konfigurieren.

Die letzten drei Schritte beschreiben, wie eine Clientanwendung erstellen, die Clientanwendung konfigurieren und erstellen und verwenden Sie einen Client, der die Funktionalität des Hosts zugreifen können.

Alle Themen in diesem Abschnitt wird davon ausgegangen, dass Sie Visual Studio als die Entwicklungsumgebung verwenden.

## <a name="sign-up-for-an-account"></a>Melden Sie für ein Konto an

Dieser erste Schritt besteht, um einen Namespace erstellen und einen freigegebenen Access Signatur (SAS) Key zu erhalten. Ein Namespace stellt eine Begrenzungslinie Anwendung für jede Anwendung über Dienstbus zur Verfügung gestellt. Eine SAS-Taste wird automatisch vom System generiert, wenn ein Dienstnamespace erstellt wird. Die Kombination von Dienstnamespace und SAS-Taste stellt die Anmeldeinformationen für Dienstbus zum Authentifizieren des Zugriffs auf eine Anwendung.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="define-a-wcf-service-contract-to-use-with-service-bus"></a>Definieren von einem WCF-Servicevertrag zur Verwendung mit Dienstbus

Servicevertrag gibt an, welche Vorgänge (Web Service Terminologie für Methoden oder Funktionen) der Dienst unterstützt. Verträge werden erstellt, indem eine Schnittstelle C++, c# oder Visual Basic definieren. Jede Methode die Benutzeroberfläche entspricht einer bestimmten Vorgang. Jede Schnittstelle müssen das [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) -Attribut angewendet, und jeden Vorgang müssen das [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) Attribut angewendet. Wenn eine Methode in einer Schnittstelle, die das Attribut [dem ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) weist nicht das Attribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) ausgestattet ist, wird diese Methode ist nicht verfügbar. Der Code für diese Vorgänge wird im Beispiel dem Verfahren bereitgestellt. Eine größere Diskussion von Verträgen und-Diensten finden Sie unter [Entwerfen und Implementieren von Diensten](https://msdn.microsoft.com/library/ms729746.aspx) in der WCF-Dokumentation.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>So erstellen Sie einen Vertrag Dienstbus mit eine Benutzeroberfläche

1. Öffnen Sie Visual Studio als Administrator an, indem Sie mit der rechten Maustaste in des Programms im Menü **Start** klicken und **als Administrator ausführen**.

2. Erstellen eines neuen Projekts der Console-Anwendung. Klicken Sie im Menü **Datei** auf **neu**, und klicken Sie auf **Projekt**. Klicken Sie im Dialogfeld **Neues Projekt** klicken Sie auf **Visual c#** (falls **Visual c#** nicht suchen unter **Andere Sprachen**angezeigt wird). Klicken Sie auf die Vorlage **Console-Anwendung** , und nennen Sie es **EchoService**. Klicken Sie auf **OK** , um das Projekt zu erstellen.

    ![][2]

3. Installieren Sie das Dienst Bus NuGet-Paket ein. Dieses Paket fügt automatisch Verweise auf die Dienstbus Bibliotheken als auch die WCF- **System.ServiceModel**hinzu. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) ist der Namespace, mit dem Sie die grundlegenden Features von WCF programmgesteuert zugreifen kann. Dienstbus wird mit vielen der Objekte und Attribute von WCF Servicevertrag definiert.

    Im Explorer-Lösung mit der rechten Maustaste in der Lösung, und klicken Sie dann auf **NuGet-Pakete verwalten, für die Lösung**. Klicken Sie auf die Registerkarte **Durchsuchen** , und suchen Sie nach `Microsoft Azure Service Bus`. Stellen Sie sicher, dass im Feld **Versionen** des Projektnamens ausgewählt ist. Klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen.

    ![][3]

3. Explorer-Lösung Doppelklicken Sie auf die Datei Program.cs, um es im Editor zu öffnen, wenn er noch nicht geöffnet ist.

4. Fügen Sie den folgenden Anweisungen am oberen Rand der Datei verwenden:

    ```
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```

1. Ändern Sie den Namen des Namespaces aus den Standardnamen der **EchoService** in **Microsoft.ServiceBus.Samples**.

    >[AZURE.IMPORTANT] In diesem Lernprogramm verwendet den c#-Namespace **Microsoft.ServiceBus.Samples**, also den Namespace des Typs Vertrag verwaltet, die in der Konfigurationsdatei Schritt [Konfigurieren der WCF-Client](#configure-the-wcf-client) verwendet wird. Sie können einem beliebigen Namespace angeben, werden sollen, wenn Sie dieses Beispiel erstellen möchten; Das Lernprogramm funktioniert jedoch nicht, es sei denn, Sie dann den Namespace des Vertrags ändern und Kundendienst für entsprechend, in der Konfigurationsdatei der. In der App angegebene Namespace muss in C#-Dateien angegebene Namespace identisch sein.

1. Direkt nach der `Microsoft.ServiceBus.Samples` Namespace-Deklarationen jedoch innerhalb des Namespace definieren Sie eine neue Schnittstelle mit dem Namen `IEchoContract` und Anwenden der `ServiceContractAttribute` Attribut auf die Schnittstelle mit einem Namespacewert des **http://samples.microsoft.com/ServiceModel/Relay/**. Der Namespacewert unterscheidet sich von den Namespace, den Sie in den Bereich des Codes verwenden. In diesem Fall wird für den Wert als einen eindeutigen Bezeichner für diesen Vertrag verwendet. Den Namespace explizit angeben wird verhindert, dass den Namespace Standardwert den Vertragsnamen hinzugefügt werden.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

    >[AZURE.NOTE] Der Namespace des Service-Vertrag enthält in der Regel ein naming Schema, die Versionsinformationen enthält. Einschließlich Versionsinformationen im Dienst Vertragsnamespace ermöglicht Diensten, damit die wichtige Änderungen isolieren, indem Sie einen neuen Servicevertrag mit einem neuen Namespace definieren und es auf einem neuen Endpunkt verfügbar zu machen. Auf diese Weise können Clients weiterhin den alte Servicevertrag zu verwenden, ohne dass aktualisiert werden. Versionsinformationen kann ein Datum oder eine Build-Nummer bestehen. Weitere Informationen finden Sie unter [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498). Für die Zwecke dieses Lernprogramms enthält das Schema naming des Namespace Vertrag Dienst keine Versionsinformationen.

1. Innerhalb der `IEchoContract` Schnittstelle, eine Methode für die einzelnen Operation Deklarieren der `IEchoContract` Vertrag macht die Benutzeroberfläche und Anwenden der `OperationContractAttribute` Attribut auf die Methode, die Sie als Teil der öffentlichen Dienstbus Vertrag verfügbar machen möchten.

    ```
    [OperationContract]
    string Echo(string text);
    ```

1. Direkt nach der `IEchoContract` Schnittstelle Definition, deklarieren Sie einen Kanal, die von beiden erbt `IEchoContract` und auch auf die `IClientChannel` Schnittstelle, wie hier dargestellt:

    ```
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Ein Kanal ist der WCF-Objekt, das durch das Host und Client Informationen miteinander verlaufen. Schreiben Sie später Code anhand der Kanal Informationen zwischen den beiden Clientanwendungen echo an.

1. Klicken Sie über das Menü **Erstellen** auf die **Lösung erstellen** , oder drücken Sie **STRG + UMSCHALT + B** , um die Genauigkeit der Ihre Arbeit bisher zu bestätigen.

### <a name="example"></a>Beispiel

Der folgende Code zeigt eine einfache Benutzeroberfläche, die einen Vertrag Dienstbus definiert.

```
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Nachdem Sie nun die Benutzeroberfläche erstellt wurde, können Sie die Schnittstelle implementieren.

## <a name="implement-the-wcf-contract-to-use-service-bus"></a>Implementieren der WCF-Vertrag Dienstbus verwenden

Erstellen einer Dienstbus Relay erfordert, dass Sie zuerst den Vertrag, die definiert ist erstellen, indem eine Schnittstelle. Weitere Informationen zum Erstellen der Benutzeroberfläche finden Sie im vorherigen Schritt. Im nächsten Schritt wird die Schnittstelle implementieren. Dies umfasst das Erstellen einer Klasse mit dem Namen `EchoService` , implementiert der benutzerdefinierte `IEchoContract` Schnittstelle. Nachdem Sie die Schnittstelle implementieren, konfigurieren Sie dann die Benutzeroberfläche eine App-Konfiguration verwenden. Die Konfigurationsdatei enthält die erforderlichen Informationen für die Anwendung, beispielsweise den Namen des Diensts, den Namen des Vertrags und den Typ des Protokoll, zur Kommunikation mit Dienstbus verwendet wird. Der Code für diese Aufgaben verwendet werden im Beispiel dem Verfahren bereitgestellt. Weitere allgemeine Informationen dazu, wie Sie einen Servicevertrag implementieren finden Sie in der Dokumentation WCF [Servicevertrag implementieren](https://msdn.microsoft.com/library/ms733764.aspx) .

1. Erstellen eine neue Klasse namens `EchoService` direkt nach der Definition der `IEchoContract` Schnittstelle. Die `EchoService` Klasse implementiert die `IEchoContract` Schnittstelle. 

    ```
    class EchoService : IEchoContract
    {
    }
    ```
    
    Ähnlich wie andere Implementierungen Benutzeroberflächen, können Sie die Definition in einer anderen Datei implementieren. Jedoch in diesem Lernprogramm die Implementierung befindet sich in derselben Datei wie die Definition der Benutzeroberfläche und den `Main` Methode.

1. Wenden Sie das Attribut [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) , um die `IEchoContract` Schnittstelle. Das Attribut gibt dem Dienstnamen und Namespace. Nachdem Sie auf diese Weise, die `EchoService` Klasse sieht wie folgt aus:

    ```
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. Implementieren der `Echo` Methode definiert, der `IEchoContract` Benutzeroberfläche der `EchoService` Class. 

    ```
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. Klicken Sie auf **Erstellen**, und klicken Sie auf **Lösung erstellen** , um die Genauigkeit der Ihrer Arbeit zu bestätigen.

### <a name="to-define-the-configuration-for-the-service-host"></a>Die Konfiguration für den Diensthost definiert.

1. Die Konfigurationsdatei ist sehr ähnlich wie ein WCF-Konfigurationsdatei. Er enthält den Namen, Endpunkt (d. h., den Speicherort Dienstbus macht für Kunden und Hosts kommunizieren) und die Bindung (den Typ des Protokoll, mit dem Sie kommunizieren). Der wichtigste Unterschied ist, dass diese konfigurierten Service-Endpunkts an auf eine [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) Bindung verweist, die nicht Bestandteil von .NET Framework ist. [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) ist eine Bindung durch Dienstbus definiert.

1. **Lösung-Explorer**Doppelklicken Sie auf die App, um es im Visual Studio-Editor zu öffnen.

2. In der `<appSettings>` Element, ersetzen Sie den Platzhalter mit dem Namen der Dienstnamespace und die SAS wichtiger, dass Sie in einem früheren Schritt kopiert haben. 

1. Innerhalb der `<system.serviceModel>` Tags, Hinzufügen einer `<services>` Element. Sie können mehrere Dienstbus Applications in einer einzelnen Konfigurationsdatei definieren. In diesem Lernprogramm definiert jedoch nur eine.
 
    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. Innerhalb der `<services>` Element, Hinzufügen einer `<service>` Element den Namen des Diensts definieren.

    ```
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. Innerhalb der `<service>` Element, die Position der Endpunktevertrag und auch den Typ der Bindung für den Endpunkt zu definieren.

    ```
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Der Endpunkt definiert, in dem der Client für die Host-Anwendung aussehen wird. Später verwendet des Lernprogramms dieses Schritts, um einen URI zu erstellen, die den Host über Dienstbus stellt zur Verfügung. Die Bindung deklariert, dass wir zur Kommunikation mit Dienstbus TCP als Protokoll verwendet wird.

1. Klicken Sie im Menü **Erstellen** auf **Lösung erstellen** , um die Genauigkeit der Ihrer Arbeit zu bestätigen.

### <a name="example"></a>Beispiel

Der folgende Code zeigt die Durchführung der im Servicevertrag.

```
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

Der folgende Code zeigt das grundlegende Format von der App, die mit dem Diensthost verknüpft ist.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-to-register-with-service-bus"></a>Hosten Sie, und führen Sie einen einfachen Webdienst mit Dienstbus registrieren

Dieser Schritt beschrieben, wie einen einfachen Dienstbus Dienst ausgeführt werden.

### <a name="to-create-the-service-bus-credentials"></a>So erstellen die Anmeldeinformationen Dienstbus

1. In `Main()`, erstellen Sie zwei Variablen in dem Namespace gespeichert und die SAS wichtiger, die aus dem Fenster Console gelesen werden.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    Die SAS-Taste wird später Zugriff auf Ihre Dienstbus Projekt verwendet werden. Der Namespace wird als Parameter zu übergeben `CreateServiceUri` Dienst-URI erstellen.

4. Verwenden ein Objekt [TransportClientEndpointBehavior](https://msdn.microsoft.com/library/microsoft.servicebus.transportclientendpointbehavior.aspx) , deklarieren Sie, dass Sie einen Schlüssel SAS Dateityp Anmeldeinformationen verwenden. Fügen Sie den folgenden Code direkt nach dem Code, der im letzten Schritt hinzugefügt.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="to-create-a-base-address-for-the-service"></a>So erstellen eine Basisadresse für den Dienst

1. Folgen den Code, die Sie hinzugefügt, in der letzte Schritt darin haben, Erstellen einer `Uri` Instanz für die Basisadresse des Diensts. Dieser URI Gibt an, das Schema Dienstbus Namespace und den Pfad der Dienstschnittstelle.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

    "Sb" ist eine Abkürzung für das Schema Dienstbus, und gibt an, dass wir TCP als Protokoll verwenden. Dies wurde in der Konfigurationsdatei auch zuvor angezeigt, wenn die Bindung [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) angegeben wurde.
    
    In diesem Lernprogramm URI ist `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="to-create-and-configure-the-service-host"></a>Erstellen und konfigurieren Sie den Diensthost

1. Legen Sie den Connectivity-Modus auf `AutoDetect`.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Der Connectivity-Modus beschreibt das Protokoll der Dienst zur Kommunikation mit Dienstbus verwendet; HTTP oder TCP. Verwenden die Standardeinstellung für das `AutoDetect`, der Dienst versucht, Herstellen einer Verbindung mit Dienstbus über TCP, sofern verfügbar und HTTP Wenn TCP nicht verfügbar ist. Beachten Sie, dass dies den Dienst von das Protokoll unterscheidet gibt an, für die Kommunikation mit Kunden. Dieses Protokoll wird durch die verwendete Bindung bestimmt. Beispielsweise kann ein Dienst die Bindung [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) verwenden, gibt an, dass deren Endpunkt (verfügbar gemacht werden Dienstbus) über HTTP mit Clients kommuniziert. Diesen Dienst könnten **ConnectivityMode.AutoDetect** angeben, damit der Dienst über TCP mit Dienstbus kommuniziert.

1. Den Diensthost erstellen, mit dem URI zuvor in diesem Abschnitt erstellt.

    ```
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    Der Diensthost ist das WCF-Objekt, das den Dienst instanziiert. Hier, Sie übergeben sie den Typ des zu erstellenden Service (eine `EchoService` Typ), und auch auf die Adresse, an dem Sie den Dienst verfügbar machen möchten.

1. Fügen Sie am oberen Rand der Datei Program.cs Verweise auf [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) und [Microsoft.ServiceBus.Description](https://msdn.microsoft.com/library/microsoft.servicebus.description.aspx)ein.

    ```
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. Wieder `Main()`, den Endpunkt zum Aktivieren des Zugriffs für Öffentliche konfigurieren.

    ```
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Dieser Schritt informiert, dass für Ihr Projekt Dienstbus, die Ihrer Anwendung öffentlich gefunden werden kann, indem Sie den Dienst Bus ATOM-feed. Wenn Sie auf **private** **DiscoveryType** festlegen, wäre ein Client weiterhin auf den Dienst zugreifen. Der Dienst sollte jedoch nicht angezeigt, bei der Suche Dienstbus Namespace. Stattdessen Nutzen der Client, müssen Sie den Endpunkt Pfad im Voraus kennen.

1. Wenden Sie die Dienstanmeldeinformationen auf die Endpunkte definiert, in der App an:

    ```
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Wie im vorherigen Schritt erwähnt, können Sie mehrere Dienste und Endpunkte in der Konfigurationsdatei deklariert haben. Wenn Sie hatten, würde dieser Code durchlaufen, die Konfigurationsdatei und suchen Sie nach jeder Endpunkt, dem sie Ihre Anmeldeinformationen gelten soll. In diesem Lernprogramm hat die Konfigurationsdatei jedoch nur einen Endpunkt aus.

### <a name="to-open-the-service-host"></a>So öffnen Sie den Diensthost

1. Öffnen Sie den Dienst an.

    ```
    host.Open();
    ```

1. Informieren Sie den Benutzer, dass der Dienst ausgeführt wird, und erläutert, wie Sie den Dienst beenden.

    ```
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Klicken Sie abschließend schließen Sie den Diensthost aus.

    ```
    host.Close();
    ```

1. Drücken Sie **STRG + UMSCHALT + B** , um das Projekt zu erstellen.

### <a name="example"></a>Beispiel

Im folgenden Beispiel wird im Servicevertrag und Implementierung aus den vorherigen Schritten in diesem Lernprogramm enthält, und den Dienst in einer Console-Anwendung hostet. Kompilieren Sie in eine ausführbare Datei mit dem Namen EchoService.exe vor.

```
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         
          
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Service Bus credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }
            
            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>Erstellen Sie einen WCF-Client für den Servicevertrag

Im nächsten Schritt wird eine grundlegende Dienstbus Clientanwendung erstellen und definieren den Servicevertrag, die in nachfolgenden Schritten implementiert werden sollen. Beachten Sie, dass viele dieser Schritte so, dass die Schritte zum Erstellen eines Diensts aus: definieren dann einen Vertrag, bearbeiten eine App.config-Datei, mit der Anmeldeinformationen für die Verbindung zu Dienstbus, usw.. Der Code für diese Aufgaben verwendet werden im Beispiel dem Verfahren bereitgestellt.

1. Erstellen eines neuen Projekts in der aktuellen Visual Studio-Lösung für den Client, indem Sie folgende Schritte ausführen:
    1. Klicken Sie im Explorer-Lösung in der gleichen Lösung, die den Dienst, enthält mit der rechten Maustaste in der aktuellen Lösung (nicht das Projekt), und klicken Sie auf **Hinzufügen**. Klicken Sie dann auf **Neues Projekt**.
    2. Klicken Sie im Dialogfeld **Neues Projekt hinzufügen** auf **Visual c#** (falls **Visual c#** nicht suchen unter **Andere Sprachen**angezeigt wird), wählen Sie die Vorlage **Console-Anwendung** , und nennen Sie es **EchoClient**.
    3. Klicken Sie auf **OK**.
<br />

1. Explorer-Lösung Doppelklicken Sie auf die Datei Program.cs im Projekt **EchoClient** , damit es in der-Editor geöffnet, wenn er noch nicht geöffnet ist.

1. Ändern des Namespacenamens aus den Standardnamen der `EchoClient` auf `Microsoft.ServiceBus.Samples`.

1. Installieren Sie das [Service Bus NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus)ein. Im Explorer-Lösung mit der rechten Maustaste in des Projekts **EchoClient** , und klicken Sie dann auf **NuGet-Pakete verwalten**. Klicken Sie auf die Registerkarte **Durchsuchen** , und suchen Sie nach `Microsoft Azure Service Bus`. Klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen.

    ![][3]

1. Hinzufügen eines `using` -Anweisung für den Namespace [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) in der Datei Program.cs. 

    ```
    using System.ServiceModel;
    ```

1. Hinzufügen der Dienst Vertragsdefinition auf den Namespace an, wie im folgenden Beispiel dargestellt. Beachten Sie, dass diese Definition im **Dienst** Projekt verwendete Definition entspricht. Fügen Sie diesen Code am oberen Rand der `Microsoft.ServiceBus.Samples` Namespace.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. Drücken Sie **STRG + UMSCHALT + B** , um die Clientbuild an.

### <a name="example"></a>Beispiel

Der folgende Code zeigt den aktuellen Status der Datei Program.cs im Projekt EchoClient an.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>Konfigurieren des WCF-Clients

In diesem Schritt erstellen Sie eine App für eine einfache Clientanwendung, die zuvor in diesem Lernprogramm erstellt Dienst greift auf ein. Diese App definiert den Vertrag, Bindung und Name des Endpunkts an. Der Code für diese Aufgaben verwendet werden im Beispiel dem Verfahren bereitgestellt.

1. Doppelklicken Sie im Explorer-Lösung im Projekt **EchoClient** auf **App.config** zum Öffnen der Datei in der Visual Studio-Editor.

2. In der `<appSettings>` Element, ersetzen Sie den Platzhalter mit dem Namen der Dienstnamespace und die SAS wichtiger, dass Sie in einem früheren Schritt kopiert haben.

1. Fügen Sie innerhalb des Elements system.serviceModel einer `<client>` Element.

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Dieser Schritt deklariert, dass Sie eine Clientanwendung WCF-Schreibweise definieren.

1. Innerhalb der `client` Element, Name, Vertrag und Bindungstyp für den Endpunkt definiert.

    ```
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Dieser Schritt definiert den Namen des Endpunkts an, den Vertrag in den Dienst, und dass die Clientanwendung TCP verwendet zur Kommunikation mit Dienstbus definiert. Der Endpunktname wird im nächsten Schritt zum Verknüpfen von dieser Endpunktkonfiguration mit dem Dienst-URI verwendet.

1. Klicken Sie auf **Datei**und dann auf **Alles speichern**.

## <a name="example"></a>Beispiel

Der folgende Code zeigt die App für den Echoeffekt Client.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client-to-call-service-bus"></a>Implementieren des WCF-Clients, um Dienstbus anzurufen

In diesem Schritt implementieren Sie eine einfache Clientanwendung, die den Dienst greift auf, die, den Sie zuvor in diesem Lernprogramm erstellt haben. Ähnlich wie der Dienst, führt der Client viele der Vorgänge Dienstbus Zugriff auf:

1. Setzt den Connectivity-Modus.
1. Den URI, der den Hostdienst sucht nach wird erstellt.
1. Definiert die Sicherheitsanmeldeinformationen.
1. Gilt die Anmeldeinformationen für die Verbindung.
1. Die Verbindung wird geöffnet.
1. Führt anwendungsspezifische Aufgaben aus.
1. Schließt die Verbindung.

Einer der wichtigsten Unterschiede ist jedoch, dass die Clientanwendung einen Kanal verwendet, um Verbindung Dienstbus, während der Dienst ein Anrufs **ServiceHost**verwendet. Der Code für diese Aufgaben verwendet werden im Beispiel dem Verfahren bereitgestellt.

### <a name="to-implement-a-client-application"></a>Eine Clientanwendung implementiert wird.

1. Legen Sie den Connectivity-Modus **automatisch erkennen**fest. Fügen Sie den folgenden Code innerhalb der `Main()` Methode der Anwendung **EchoClient** .

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. Definieren von Variablen zum Speichern der Werte für den Dienstnamespace und SAS-Taste, die aus der Konsole gelesen werden.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. Erstellen des URIS, der die Position des Hosts im Projekt Dienstbus definiert.

    ```
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. Erstellen Sie das Anmeldeinformationsobjekt für Ihre Namespace-Endpunkt an.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. Erstellen Sie die ChannelFactory, die die Konfiguration beschrieben, in der App zu laden.

    ```
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Eine ChannelFactory ist ein WCF-Objekt, das einen Kanal erstellt über den die Dienst und Clientanwendungen kommunizieren.

1. Anwenden der Dienstbus Anmeldeinformationen an.

    ```
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. Erstellen Sie und öffnen Sie den Kanal zum Dienst.

    ```
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. Die grundlegende Benutzeroberfläche und Funktionen für den Echoeffekt zu schreiben.

    ```
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Beachten Sie, dass der Code die Instanz des Objekts Channel als Proxy für den Dienst verwendet.

1. Schließen Sie den Kanal, und schließen Sie die Factory.

    ```
    channel.Close();
    channelFactory.Close();
    ```

## <a name="to-run-the-applications"></a>Zur Ausführung der Anwendung

1. Drücken Sie **STRG + UMSCHALT + B** , um die Lösung zu erstellen. Damit wird das Clientprojekt und das Projekt-Dienst, das Sie in den vorherigen Schritten erstellt haben.

2. Vor dem Ausführen der Clientanwendung, müssen Sie sicherstellen, dass die Service-Anwendung ausgeführt wird. Klicken Sie im Explorer-Lösung in Visual Studio mit der rechten Maustaste in der Lösung **EchoService** und dann auf **Eigenschaften**.

3. Klicken Sie im Dialogfeld Eigenschaften der Lösung klicken Sie auf **Start-Projekt**, und klicken Sie auf die Schaltfläche **Start projektübergreifende** . Stellen Sie sicher, dass **EchoService** zuerst in der Liste angezeigt wird. 

4. Legen Sie im Feld **Action** für Projekte, die die **EchoService** und die **EchoClient** zu **Starten**.

    ![][5]

5. Klicken Sie auf **Projekt Abhängigkeiten**. Wählen Sie im Feld **Projekte** **EchoClient**ein. Klicken Sie im **Depends on** sicherzustellen Sie, dass **EchoService** aktiviert ist.

    ![][6]

6. Klicken Sie auf **OK** , um das Dialogfeld **Eigenschaften** zu schließen.

7. Drücken Sie **F5** , um beide Projekte auszuführen.

8. Beide Console-Fenster zu öffnen und zur Eingabe aufgefordert, den Namen des Namespaces. Der Dienst muss zuerst ausgeführt, geben Sie im Konsolenfenster **EchoService** , also den Namespace und drücken Sie dann die **EINGABETASTE**.

9. Als Nächstes werden Sie aufgefordert Ihre SAS Schlüssel einzugeben. Geben Sie die SAS-Taste, und drücken Sie die EINGABETASTE.

    Beispiel für die Ausgabe aus dem Fenster Console Sie hier. Beachten Sie, dass die Werte zur Verfügung gestellt, dass hier beispielsweise nur zu.

    `Your Service Namespace: myNamespace`
    `Your SAS Key: <SAS key value>`

    Die Service-Anwendung gedruckt wird im Fenster Konsole die Adresse, an der sie überwacht, wie im folgenden Beispiel gezeigt.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`
    `Press [Enter] to exit`
    
10. Geben Sie im Fenster **EchoClient** Console dieselbe Informationen, die Sie zuvor für die Service-Anwendung eingegeben haben. Führen Sie die vorherigen Schritte, um die gleichen Dienstnamespace und SAS Schlüsselwerte für die Clientanwendung eingeben.

11. Nachdem Sie diese Werte eingegeben haben, wird der Client öffnet einen Kanal zu dem Dienst und fordert Sie zur Eingabe von Text wie im folgenden Beispiel Console Ausgabe gesehen.

    `Enter text to echo (or [Enter] to exit):` 

    Geben Sie Text zum Senden an die Service-Anwendung, und drücken die EINGABETASTE. Dieser Text wird über den Vorgang im Echo an den Dienst gesendet und im Fenster Service Console wie die folgende Ausgabe angezeigt.

    `Echoing: My sample text`

    Die Clientanwendung erhält den Rückgabewert der `Echo` Vorgang aus, der den ursprünglichen Text ist, und gibt ihn an seiner Console-Fenster. Die folgende Abbildung Ausgabe eines Beispiels aus der Client Console-Fenster.

    `Server echoed: My sample text`

12. Sie können das Senden von Textnachrichten vom Client an den Dienst auf diese Weise weiter. Wenn Sie fertig sind, drücken Sie die EINGABETASTE in des Windows-Client und Dienst um beide Applications zu beenden.

## <a name="example"></a>Beispiel

Im folgenden Beispiel wird zum Erstellen einer Clientanwendung, wie die Vorgänge des Diensts aufgerufen und wie den Client zu schließen, nachdem Sie der Anruf Vorgang abgeschlossen ist.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;

            
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm gezeigt, wie eine Dienstbus Clientbuild Anwendung und die Verwendung der Funktionen für Dienstbus "Relay"-Dienst. Eine ähnliche Lernprogramm, in dem Dienstbus [Messaging](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)verwendet, finden Sie im [Service Bus vermittelte Messaging .NET Lernprogramm](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Weitere Informationen zu Dienstbus finden Sie unter den folgenden Themen.

- [Übersicht über Dienstbus messaging](../service-bus-messaging/service-bus-messaging-overview.md)
- [Dienstbus-Grundlagen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Dienst Busarchitektur](../service-bus-messaging/service-bus-architecture.md)

[Azure classic portal]: http://manage.windowsazure.com

[1]: ./media/service-bus-relay-tutorial/service-bus-policies.png
[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[4]: ./media/service-bus-relay-tutorial/create-ns.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
