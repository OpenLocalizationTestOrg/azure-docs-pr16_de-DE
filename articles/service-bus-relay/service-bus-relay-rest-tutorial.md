<properties
    pageTitle="Service Bus REST zusammengehörenden mit weitergeleitet messaging | Microsoft Azure"
    description="Erstellen einer einfachen Dienstbus Relay Host-Anwendungs, die eine REST-basierte Schnittstelle bereitstellt."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-rest-tutorial"></a>Lernprogramm Service Bus Relay REST

In diesem Lernprogramm beschrieben, wie eine einfache Dienstbus Host-Anwendung zu erstellen, die eine REST-basierte Schnittstelle macht. REST ermöglicht einen Webclient, beispielsweise einem Webbrowser auf den Dienst Bus APIs über HTTP-Anfragen zu.

In diesem Lernprogramm verwendet den REST Windows Communication Foundation (WCF) zum Erstellen eines REST-Diensts auf Dienstbus programming Modell. Weitere Informationen finden Sie in der WCF-Dokumentation [WCF REST Programming Model](https://msdn.microsoft.com/library/bb412169.aspx) and [Entwerfen und implementieren Services](https://msdn.microsoft.com/library/ms729746.aspx) .

## <a name="step-1-create-a-service-namespace"></a>Schritt 1: Erstellen eines Dienst Namespaces

Dieser erste Schritt besteht, um einen Namespace erstellen und einen freigegebenen Access Signatur (SAS) Key zu erhalten. Ein Namespace stellt eine Begrenzungslinie Anwendung für jede Anwendung über Dienstbus zur Verfügung gestellt. Eine SAS-Taste wird automatisch vom System generiert, wenn ein Dienstnamespace erstellt wird. Die Kombination von Dienstnamespace und SAS-Taste stellt die Anmeldeinformationen für Dienstbus zum Authentifizieren des Zugriffs auf eine Anwendung.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="step-2-define-a-rest-based-wcf-service-contract-to-use-with-service-bus"></a>Schritt 2: Definieren von einem WCF REST-basierten Servicevertrag zur Verwendung mit Dienstbus

Als mit anderen Diensten Dienstbus müssen beim Erstellen eines Diensts REST-Schreibweise Sie den Vertrag definieren. Der Vertrag gibt an, welche Vorgänge der Host unterstützt wird. Ein Vorgang kann als Web Service Methode vorstellen. Verträge werden erstellt, indem eine Schnittstelle C++, c# oder Visual Basic definieren. Jede Methode in der Benutzeroberfläche entspricht einen bestimmten Dienstvorgang. Das Attribut [dem ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) muss auf jede Schnittstelle angewendet werden, und das Attribut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) muss auf jeden Vorgang angewendet werden. Wenn eine Methode in eine Schnittstelle, die [dem ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) verfügt, nicht über die [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx)verfügt, wird diese Methode ist nicht verfügbar. Im Beispiel dem Verfahren wird der Code für diese Aufgaben verwendet angezeigt.

Der primäre Unterschied zwischen einer einfachen Dienstbus Vertrag und einen Vertrag REST-Format ist das Hinzufügen einer Eigenschaft zu der [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Mit dieser Eigenschaft können Sie eine Methode in der Benutzeroberfläche auf eine Methode für den Rand der Benutzeroberfläche zuordnen. In diesem Fall werden wir [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) verwenden, um eine Methode zum Abrufen von HTTP zu verknüpfen. Dadurch Dienstbus präzise abrufen und bei der Interpretation Befehle, die auf die Schnittstelle gesendet wird.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>So erstellen Sie einen Vertrag Dienstbus mit eine Benutzeroberfläche

1. Öffnen Sie Visual Studio als Administrator: mit der rechten Maustaste in des Programms im Menü **Start** , und klicken Sie dann auf **als Administrator ausführen**.

2. Erstellen eines neuen Projekts der Console-Anwendung. Klicken Sie im Menü **Datei** auf, und wählen Sie **neu**, und wählen Sie dann **Projekt**. Klicken Sie im Dialogfeld **Neues Projekt** klicken Sie auf **Visual C#-**, wählen Sie die Vorlage **Console-Anwendung** , und nennen Sie es **ImageListener**. Verwenden Sie den standardmäßigen **Speicherort**. Klicken Sie auf **OK** , um das Projekt zu erstellen.

3. Für ein C#-Projekt, Visual Studio erstellt eine `Program.cs` Datei. Diese Klasse enthält eine leere `Main()` Methode für ein Projekt ordnungsgemäß erstellt Console-Anwendung erforderlich ist.

4. Fügen Sie Verweise auf Dienstbus und **System.ServiceModel.dll** des Projekts, indem Sie das Dienst Bus NuGet-Paket installieren. Dieses Paket fügt automatisch Verweise auf die Dienstbus Bibliotheken als auch die WCF- **System.ServiceModel**hinzu. Im Explorer-Lösung mit der rechten Maustaste in des Projekts **ImageListener** , und klicken Sie dann auf **NuGet-Pakete verwalten**. Klicken Sie auf die Registerkarte **Durchsuchen** , und suchen Sie nach `Microsoft Azure Service Bus`. Klicken Sie auf **Installieren**, und akzeptieren Sie der Vereinbarung.

5. Sie müssen einen Verweis auf **System.ServiceModel.Web.dll** explizit zum Projekt hinzufügen:

    ein. Im Lösung-Explorer mit der rechten Maustaste in des Ordners " **Verweise** " unter dem Projektordner, und klicken Sie dann auf **Verweis hinzufügen**.

    b. Klicken Sie im Dialogfeld **Verweis hinzufügen** klicken Sie auf der Registerkarte **Framework** auf der linken Bildschirmseite und in **das Suchfeld** , geben Sie **System.ServiceModel.Web**ein. Aktivieren Sie das Kontrollkästchen **System.ServiceModel.Web** und dann auf **OK**.

6. Fügen Sie den folgenden `using` Anweisungen am oberen Rand der Datei Program.cs.

    ```
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) ist der Namespace, der programmgesteuerten Zugriff auf die grundlegenden Features von WCF ermöglicht. Dienstbus wird mit vielen der Objekte und Attribute von WCF Servicevertrag definiert. Verwenden Sie dieses Namespace in den meisten Dienstbus Relay Anwendungen. Auf ähnliche Weise hilft [vom Typ System.ServiceModel.Channels dargestellt](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) , den Channel zu definieren, der das Objekt wird über dem Sie mit Dienstbus und Webbrowser des Clients kommunizieren. Schließlich enthält [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) die Typen, mit die Sie webbasierte Applications erstellen können.

7. Umbenennen der `ImageListener` Namespace zu **Microsoft.ServiceBus.Samples**.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

8. Direkt nach dem definieren die öffnende geschweifte Klammer der Namespacedeklaration, eine neue Schnittstelle mit dem Namen **IImageContract** , und wenden das Attribut **dem ServiceContractAttribute** auf die Schnittstelle mit dem Wert `http://samples.microsoft.com/ServiceModel/Relay/`. Der Namespacewert unterscheidet sich von den Namespace, den Sie in den Bereich des Codes verwenden. Für den Wert als einen eindeutigen Bezeichner für dieses Vertrags verwendet wird, und sollten Versionsinformationen verfügen. Weitere Informationen finden Sie unter [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498). Den Namespace explizit angeben wird verhindert, dass den Namespace Standardwert den Vertragsnamen hinzugefügt werden.

    ```
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

9. Innerhalb der `IImageContract` Schnittstelle, eine Methode für die einzelnen Operation Deklarieren der `IImageContract` Vertrag macht die Benutzeroberfläche und Anwenden der `OperationContractAttribute` Attribut auf die Methode, die Sie als Teil der öffentlichen Dienstbus Vertrag verfügbar machen möchten.

    ```
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

10. Fügen Sie in dem Attribut **OperationContract** **WebGet** Wert ein.

    ```
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

    Auf diese Weise können Sie Dienstbus zum Routing HTTP GET-Anfragen zu `GetImage`, und die Rückgabewerte der übersetzen `GetImage` in einer Antwort HTTP GETRESPONSE. Später im Lernprogramm werden Sie einen Webbrowser verwenden, auf diese Methode zugreifen und das Bild im Browser angezeigt werden.

11. Direkt nach der `IImageContract` Definition, deklarieren Sie einen Kanal, die von beiden erbt die `IImageContract` und `IClientChannel` Schnittstellen.

    ```
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

    Ein Kanal ist, der durch den der Dienst und Client Informationen miteinander verlaufen WCF-Objekt. Erstellen Sie später den Kanal in Ihrer Anwendung. Dienstbus nutzt diesen Kanal klicken Sie dann die HTTP GET-Anfragen über den Browser auf Ihre Implementierung **GetImage** übergeben. Dienstbus mithilfe auch den Kanal den Rückgabewert **GetImage** und in einer HTTP-GETRESPONSE für den Clientbrowser übersetzen.

12. Klicken Sie im Menü **Erstellen** auf **Lösung erstellen** , um die Genauigkeit der Ihre Arbeit bisher zu bestätigen.

### <a name="example"></a>Beispiel

Der folgende Code zeigt eine einfache Benutzeroberfläche, die einen Vertrag Dienstbus definiert.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-to-use-service-bus"></a>Schritt 3: Implementieren eines REST-basierte WCF-Servicevertrags Dienstbus verwenden

Erstellen eines Diensts REST-Schreibweise Dienstbus erfordert, dass Sie zuerst den Vertrag, die definiert ist erstellen, indem eine Schnittstelle. Im nächsten Schritt wird die Schnittstelle implementieren. Dies umfasst das Erstellen einer Klasse mit dem Namen **ImageService** , das benutzerdefinierte **IImageContract** -Schnittstelle implementiert. Nachdem Sie den Vertrag implementiert, konfigurieren Sie dann die Benutzeroberfläche eine App verwenden. Die Konfigurationsdatei enthält die erforderlichen Informationen für die Anwendung, beispielsweise den Namen des Diensts, den Namen des Vertrags und den Typ des Protokoll, zur Kommunikation mit Dienstbus verwendet wird. Der Code für diese Aufgaben verwendet werden im Beispiel dem Verfahren bereitgestellt.

Wie bei den vorherigen Schritten, ist es sehr kleinen Unterschied zwischen einen Vertrag REST-Stil und einen einfachen Dienstbus Vertrag implementieren.

### <a name="to-implement-a-rest-style-service-bus-contract"></a>Einen REST-Schreibweise Dienstbus Vertrag implementiert wird.

1. Erstellen Sie eine neue Klasse namens **ImageService** direkt nach der Definition der Schnittstelle **IImageContract** . Die **ImageService** Klasse implementiert die **IImageContract** -Schnittstelle.

    ```
    class ImageService : IImageContract
    {
    }
    ```
    Ähnlich wie andere Implementierungen Benutzeroberflächen, können Sie die Definition in einer anderen Datei implementieren. In diesem Lernprogramm die Implementierung bleibt allerdings in derselben Datei wie die Definition der Benutzeroberfläche und `Main()` Methode.

2. Wenden Sie das Attribut [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) zur Klasse **IImageService** , um darauf hinzuweisen, dass die Klasse eine Implementierung von einem WCF-Vertrag ist.

    ```
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    Wie bereits zuvor erwähnt, ist dieser Namespace kein traditionelle Namespace. In diesem Fall ist es Teil der WCF-Architektur, die den Vertrag identifiziert. Weitere Informationen finden Sie unter dem Thema [Daten Vertragsnamen](https://msdn.microsoft.com/library/ms731045.aspx) in der WCF-Dokumentation.

3. Hinzufügen eines Bilds jpg zu einem Projekt.  

    Dies ist ein Bild, das der Dienst im empfangen Browser angezeigt wird. Mit der rechten Maustaste in Ihrem Projekts, und klicken Sie auf **Hinzufügen**. Klicken Sie dann auf **Vorhandenes Element**. Verwenden Sie das Dialogfeld **Vorhandenes Element hinzufügen** , um eine entsprechende JPG zu suchen, und klicken Sie dann auf **Hinzufügen**.

    Wenn Sie die Datei hinzufügen möchten, stellen Sie sicher, dass **Alle Dateien** in der Dropdown-Liste, neben ausgewählt ist der **Dateiname:** Feld. Im weiteren Verlauf dieses Lernprogramms wird davon ausgegangen, dass der Name des Bilds "image.jpg" ist. Wenn Sie eine andere Datei haben, müssen Sie das Bild umbenennen oder ändern Sie den Code angepasst.

4. Um sicherzustellen, dass der laufende Dienst die Bilddatei finden kann, in **Lösung Explorer** mit der rechten Maustaste in der Bilddatei, und klicken Sie auf **Eigenschaften**. Legen Sie im Bereich **Eigenschaften** **Copy to Output Directory** auf **Kopieren, wenn neuer**ein.

5. Einen Verweis auf die Assembly **System.Drawing.dll** des Projekts und auch hinzufügen die folgenden verknüpft ist `using` Anweisungen.  

    ```
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

6. Fügen Sie den folgenden Konstruktor, der lädt die Bitmap, und senden Sie es in den Clientbrowser vorbereitet, in der Klasse **ImageService** .

    ```
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

7. Fügen Sie direkt nach dem vorherigen Code, die folgende **GetImage** -Methode in der Klasse **ImageService** zurückzugebenden eine HTTP-Nachricht mit dem Bild.

    ```
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);

        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

        return stream;
    }
    ```

    Diese Implementierung verwendet **MemoryStream** , um das Bild abrufen und Vorbereiten der Arbeitsmappe für das streaming an den Browser. Die Position im Stream bei Null beginnt, deklariert den Stream Inhalt als Jpeg und streamt die Informationen.

8. Klicken Sie im Menü **Erstellen** auf **Lösung erstellen**.

### <a name="to-define-the-configuration-for-running-the-web-service-on-service-bus"></a>Definiert die Konfiguration für den Webdienst Dienstbus ausgeführt

1. Doppelklicken Sie auf **Lösung Explorer** **App.config** , um sie in der Visual Studio-Editor zu öffnen.

    **Die App** ähnelt eine WCF-Konfigurationsdatei und enthält den Namen, Endpunkt (d. h., den Speicherort Dienstbus macht für Kunden und Hosts kommunizieren) und Bindung (den Typ des Protokoll, mit dem Sie kommunizieren). Hier der wichtigste Unterschied ist, dass der konfigurierten Endpunkt auf eine [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) Bindung verweist, die nicht Bestandteil von .NET Framework ist.

2. Die `<system.serviceModel>` XML-Element ist ein WCF-Element, das einen oder mehrere Dienste definiert. Hier wird es verwendet, um den Dienst und den Endpunkt definieren. Am Ende der `<system.serviceModel>` Element (aber weiterhin innerhalb `<system.serviceModel>`), Hinzufügen einer `<bindings>` Element, das den folgenden Inhalt aufweist. Hiermit wird die in der Anwendung verwendeten Bindungen definiert. Sie können mehrere Bindungen definieren, aber in diesem Lernprogramm sind nur eine definieren.

    ```
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    Dieser Schritt definiert eine Bindung Dienstbus [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) mit **RelayClientAuthenticationType** **keine**festgelegt. Diese Einstellung gibt an, dass ein Endpunkt mithilfe dieser Bindung keine Client-Anmeldeinformationen erforderlich sind.

3. Nach der `<bindings>` Element, Hinzufügen einer `<services>` Element. Ähnlich wie der Bindungen, können Sie mehrere Dienste in einer einzelnen Konfigurationsdatei definieren. Definieren jedoch in diesem Lernprogramm Sie nur eine.

    ```
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    Dieser Schritt konfiguriert einen Dienst, der zuvor definierten Standardwert **WebHttpRelayBinding**verwendet. Darüber hinaus verwendet die standardmäßige **SbTokenProvider**, die im nächsten Schritt definiert ist.

4. Nach der `<services>` Element, Erstellen einer `<behaviors>` Element mit dem folgenden Inhalt, ersetzen "SAS_KEY" mit dem *Freigegebenen Access Signatur* (SAS) Schlüssel zuvor aus dem [Azure-Portal erworbenen][].

    ```
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

5. Weiterhin in App.config, in der `<appSettings>` Element, das die gesamte Verbindung mit der Verbindungszeichenfolge aus dem Portal zuvor erworbenen Zeichenfolgenwert ersetzen. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

6. Klicken Sie im Menü **Erstellen** auf **Lösung erstellen** , um die gesamte Lösung zu erstellen.

### <a name="example"></a>Beispiel

Der folgende Code zeigt die Implementierung Vertrag und Dienst für ein REST-basierten Dienst, der mit der die **WebHttpRelayBinding** Bindung auf Dienstbus ausgeführt wird.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Im folgenden Beispiel wird die App, die mit dem Dienst verknüpft ist.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="[SAS_KEY]" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-the-rest-based-wcf-service-to-use-service-bus"></a>Schritt 4: Hosten Sie die REST-basierte WCF-Webdienst, um Dienstbus verwenden

Dieser Schritt beschrieben, wie einen Webdienst, der mit einer Console-Anwendung auf Dienstbus ausführen. Eine vollständige Auflistung des Codes in diesem Schritt geschrieben werden im Beispiel dem Verfahren bereitgestellt.

### <a name="to-create-a-base-address-for-the-service"></a>So erstellen eine Basisadresse für den Dienst

1. In der `Main()` Funktion Deklaration, erstellen Sie eine Variable zum Speichern des Namespaces des Projekts Dienstbus. Denken Sie daran `yourNamespace` mit dem Namen des Dienstnamespace Sie zuvor erstellt haben.

    ```
    string serviceNamespace = "yourNamespace";
    ```
    Dienstbus verwendet den Namen des Namespace einen eindeutigen URI zu erstellen.

2. Erstellen einer `Uri` Instanz für die Basisadresse des Diensts, die auf den Namespace basiert.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="to-create-and-configure-the-web-service-host"></a>Erstellen und Konfigurieren des Web-Service-Hosts

- Erstellen Sie Web-Service-Host mithilfe der in diesem Abschnitt zuvor erstellten URI-Adresse ein.

    ```
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    Der Diensthost ist das WCF-Objekt, das die Host-Anwendung instanziiert. In diesem Beispiel übergibt es den Typ des Host (eine **ImageService**) erstellen möchten, und die Adresse, an der Sie die Host-Anwendung verfügbar machen möchten.

### <a name="to-run-the-web-service-host"></a>Zum Ausführen des Web Service-Hosts

1. Öffnen Sie den Dienst an.

    ```
    host.Open();
    ```
    Der Dienst wird jetzt ausgeführt.

2. Anzeigen einer Nachricht, die angibt, dass der Dienst ausgeführt wird, und wie Sie den Dienst zu beenden.

    ```
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

3. Klicken Sie abschließend schließen Sie den Diensthost aus.

    ```
    host.Close();
    ```

## <a name="example"></a>Beispiel

Im folgende Beispiel umfasst die Servicevertrag und Implementierung aus den vorherigen Schritten des Lernprogramms und den Dienst in einer Console-Anwendung hostet. Kompilieren Sie den folgenden Code in eine ausführbare Datei namens ImageListener.exe.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-the-code"></a>Kompilieren des Codes

Nach dem Erstellen der Lösung, gehen Sie wie folgt vor, um die Anwendung ausführen:

1. Drücken Sie **F5**, oder wechseln Sie zum Speicherort ausführbare Datei (ImageListener\bin\Debug\ImageListener.exe), um den Dienst auszuführen. Lassen Sie die app ausgeführt, wie im nächsten Schritt erforderlich ist.

2. Kopieren und Einfügen der Adresse über die Befehlszeile in einem Browser, um das Bild anzuzeigen.

3. Wenn Sie fertig sind, drücken Sie die **EINGABETASTE** im Eingabeaufforderungsfenster die app zu schließen.

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie eine Anwendung erstellt haben, die den Relaydienst Dienstbus verwendet, finden Sie unter den folgenden Artikeln erfahren Sie mehr über die weitergeleitete Nachrichten:

- [Azure Service Bus-Architektur im Überblick](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)

- [So verwenden Sie den Dienst Bus Relay Service](service-bus-dotnet-how-to-use-relay.md)

[Azure-portal]: https://portal.azure.com