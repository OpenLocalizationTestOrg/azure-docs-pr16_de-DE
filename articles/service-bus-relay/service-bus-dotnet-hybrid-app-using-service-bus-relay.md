<properties
    pageTitle="Hybrid lokale/Cloud-Anwendung (.NET) | Microsoft Azure"
    description="Informationen Sie zum Erstellen einer .NET lokale/Cloud Hybrid-Anwendung, die mit der Azure-Dienstbus Relay."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="net-on-premisescloud-hybrid-application-using-azure-service-bus-relay"></a>.NET lokale/Cloud Hybrid Anwendung Azure Service Bus Relay verwenden

## <a name="introduction"></a>Einführung

In diesem Artikel beschrieben, wie Sie eine Hybriden Cloud-Anwendung mit Microsoft Azure und Visual Studio erstellen. Das Lernprogramm wird davon ausgegangen, dass Sie keine vorherige Erfahrung mit Azure haben. In weniger als 30 Minuten haben Sie eine Anwendung, die mehrere Azure Ressourcen verwendet wird, nach oben und in der Cloud ausgeführt wird.

Lernen Sie Folgendes:

-   Informationen zum Erstellen oder Anpassen eines vorhandenen Webdiensts für Ernährung über eine Web-Lösung.
-   So verwenden Sie den Azure Service Bus Relaydienst zum Freigeben von Daten zwischen Azure-Anwendung und einem Webdienst gehostet an anderer Stelle.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-the-service-bus-relay-helps-with-hybrid-solutions"></a>Schutzmechanismen in das Relay Dienstbus mit Hybrid Lösungen

Business Solutions bestehen in der Regel aus einer Kombination von benutzerdefiniertem Code geschrieben zur Lösung neue vorübergehend und einmalig Business und der vorhandenen Funktionalität von Lösungen und Systeme, die bereits vorhanden sind.

Lösungsarchitekten sind beginnen Sie mit die Cloud für einfacher Handhabung von unteren Kosten- und Maßstab Anforderungen verwenden. Auf diese Weise finden sie, dass vorhandene-Service-Ressourcen aus, die sie nutzen als Bausteine für den dazugehörigen Lösungen innerhalb des Unternehmens Firewalls und Abmelden bei einfach sind möchten für den Zugriff durch die Cloudlösung erreicht haben. Viele interne Dienste werden nicht erstellt oder so, dass sie einfach am Rand Unternehmensnetzwerk zugänglich gemacht werden können gehostet.

Dienstbus Relay eignet sich für vorhandene Windows Communication Foundation (WCF)-Webdiensten Aufzeichnen von Anwendungsfall- und zugänglich Dienste sicher zu Lösungen, die außerhalb des Unternehmens ohne Einfluss Änderungen an der Infrastruktur Unternehmensnetzwerk befinden. Solche Relay Service Kraftomnibussen noch innerhalb ihrer vorhandenen Umgebung gehostet werden, aber sie delegieren warten auf eingehende Sitzungen und Besprechungsanfragen in der Cloud gehostete Dienstbus. Dienstbus Loss auch Dienste vor unbefugtem Zugriff mithilfe von [Access-Signatur freigegeben](../service-bus-messaging/service-bus-sas-overview.md) (SAS) Authentifizierung ein.

## <a name="solution-scenario"></a>Lösungsszenario

In diesem Lernprogramm erstellen Sie eine ASP.NET-Website, die Ihnen eine Liste der Produkte finden Sie auf der Seite Product Inventory ermöglicht.

![][0]

Das Lernprogramm wird davon ausgegangen, dass Sie Produktinformationen in einem vorhandenen lokalen System haben und mithilfe das Dienstbus Relay in diesem System erreicht haben. Dies ist von einem Webdienst eines simulierten, die in einer einfachen Console-Anwendung ausgeführt wird und von einer Gruppe in-Memory-Produkte gesichert wird. Führen Sie diese Console-Anwendung auf Ihrem eigenen Computer und die Web-Rolle in Azure bereitstellen werden. Auf diese Weise wird angezeigt wie die Webrolle im Azure Datencenter ausgeführt tatsächlich um in Ihrem Computer anrufen wird, obwohl Sie Ihren Computer fast immer mindestens eine Firewall und eine Ebene des Netzwerks Adresse (Netzwerkadressübersetzung) gespeichert werden soll.

Im folgenden finden einen Screenshot der Startseite der Anwendung fertig gestellte Web.

![][1]

## <a name="set-up-the-development-environment"></a>Einrichten der Entwicklungsumgebung

Bevor Sie die Entwicklung von Azure Applications beginnen können, erhalten Sie die Tools, und richten Sie Ihre Entwicklungsumgebung.

1.  Installieren der Azure SDK für .NET von der Seite [erste Tools und SDK][] an.

2.  Klicken Sie auf **das SDK installieren** , nach der Version von Visual Studio, die Sie verwenden. Die Schritte in diesem Lernprogramm verwenden Sie Visual Studio 2015.

4.  Wenn Sie dazu aufgefordert werden, ausführen oder speichern das Installationsprogramm, klicken Sie auf **Ausführen**.

5.  Klicken Sie im **Web Platform Installer**auf **Installieren** , und fahren Sie mit der Installation.

6.  Sobald die Installation abgeschlossen ist, haben Sie alles, was Sie zu beginnen, um die app zu entwickeln. Das SDK enthält Tools, mit die Sie einfach Azure Applications in Visual Studio entwickeln können. Wenn Sie nicht Visual Studio installiert haben, installiert das SDK auch kostenlosen Visual Studio Express.

## <a name="create-a-namespace"></a>Erstellen Sie einen namespace

Um mit Dienstbus Features in Azure zu beginnen, müssen Sie zuerst einen Dienstnamespace erstellen. Ein Namespace stellt einen Bereiche Container zum Adressieren Dienstbus Ressourcen innerhalb Ihrer Anwendung bereit.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-an-on-premises-server"></a>Erstellen eines lokalen Servers

Erstellen Sie zuerst eine lokale (simulierten) Produkt Katalog-System. Recht einfach werden; sehen Sie dies als Darstellung einer tatsächlichen lokalen Produkt Katalogsystem mit einer vollständigen Dienst-Oberfläche, die wir versuchen, integriert werden soll.

Dieses Projekt ist eine Visual Studio Console-Anwendung, und das [Azure Service Bus NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) verwendet, um eine Dienstbus Bibliotheken und Konfiguration Einstellungen übernehmen.

### <a name="create-the-project"></a>Erstellen Sie das Projekt

1.  Starten Sie Microsoft Visual Studio mithilfe von Administratorrechte. Um Visual Studio mit Administratorrechten starten, mit der rechten Maustaste in das Symbol für das **Visual Studio** -Programm, und klicken Sie dann auf **als Administrator ausführen**.

2.  Klicken Sie in Visual Studio im Menü **Datei** auf **neu**, und klicken Sie dann auf **Projekt**.

3.  Klicken Sie auf Grundlage von **Vorlagen installiert**unter **Visual c#**, **Console-Anwendung**auf. Geben Sie im Feld **Name** den Namen **ProductsServer**ein:

    ![][11]

4.  Klicken Sie auf **OK** , um das Projekt **ProductsServer** zu erstellen.

7.  Wenn Sie das NuGet-Paket-Manager für Visual Studio bereits installiert haben, fahren Sie mit dem nächsten Schritt fort. Andernfalls besuchen Sie [NuGet][] , und klicken Sie auf [NuGet installieren](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c). Führen Sie die Anweisungen, um die NuGet Package Manager zu installieren, und klicken Sie dann erneut starten Sie Visual Studio.

7.  Klicken Sie im Explorer-Lösung mit der rechten Maustaste in des **ProductsServer** Projekts und dann auf **NuGet-Pakete verwalten**.

8.  Klicken Sie auf die Registerkarte **Durchsuchen** , und suchen Sie nach `Microsoft Azure Service Bus`. Klicken Sie auf **Installieren**, und akzeptieren Sie der Vereinbarung.

    ![][13]

    Beachten Sie, dass nun die erforderlichen Clientassemblys verwiesen wird.

9.  Fügen Sie eine neue Klasse für Ihre Product Vertrag. Im Lösung-Explorer mit der rechten Maustaste in des Projekts **ProductsServer** und klicken Sie auf **Hinzufügen**und dann auf **Klasse**.

10. Geben Sie im Feld **Name** den Namen **ProductsContract.cs**ein. Klicken Sie dann auf **Hinzufügen**.

11. Ersetzen Sie die Namespacedefinition in **ProductsContract.cs**mit den folgenden Code ein, der den Vertrag für den Dienst definiert.

    ```
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;
    
        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }
    
        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();
    
        }
    
        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```

12. Ersetzen Sie die Namespacedefinition in Program.cs mit den folgenden Code ein, der den Profildienst und der Host dafür hinzufügt.

    ```
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;
    
        // Implement the IProducts interface.
        class ProductsService : IProducts
        {
    
            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };
    
            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }
    
        }
    
        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();
    
                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();
    
                sh.Close();
            }
        }
    }
    ```

13. Explorer-Lösung Doppelklicken Sie auf **die App, um es im Visual Studio-Editor zu öffnen** . Am Ende der ** &lt;System. ServiceModel&gt; ** Element (aber weiterhin innerhalb &lt;System. ServiceModel&gt;), die folgenden XML-Code hinzu. Achten Sie darauf, dass *YourServiceNamespace* mit dem Namen von Namespace und *YourKey* mit dem SAS Schlüssel ersetzen, den Sie zuvor aus dem Portal abgerufen werden:

    ```
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
14. Weiterhin in App.config, in der ** &lt;AppSettings&gt; ** Element, das die Verbindung mit der Verbindungszeichenfolge aus dem Portal zuvor erworbenen Zeichenfolgenwert ersetzen. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

14. Drücken Sie **STRG + UMSCHALT + B** , oder klicken Sie im Menü **Erstellen** auf **Lösung erstellen** , erstellen Sie die Anwendung, und überprüfen die Genauigkeit der Ihre Arbeit bisher.

## <a name="create-an-aspnet-application"></a>Erstellen einer ASP.NET-Anwendung

In diesem Abschnitt erstellen Sie eine einfache ASP.NET-Anwendung, die aus dem Produkt Dienst abgerufenen Daten angezeigt.

### <a name="create-the-project"></a>Erstellen Sie das Projekt

1.  Stellen Sie sicher, dass Visual Studio mit Administratorrechten ausgeführt wird.

2.  Klicken Sie in Visual Studio im Menü **Datei** auf **neu**, und klicken Sie dann auf **Projekt**.

3.  Klicken Sie auf Grundlage von **Vorlagen installiert**unter **Visual c#**, **ASP.NET Web-Anwendung**auf. Nennen Sie das Projekt **ProductsPortal**. Klicken Sie dann auf **OK**.

    ![][15]

4.  Klicken Sie in der Liste **Wählen Sie eine Vorlage** auf **MVC**. 

6.  Aktivieren Sie das Kontrollkästchen für **Host in der Cloud**.

    ![][16]

5. Klicken Sie auf die Schaltfläche **Authentifizierung ändern** . Führen Sie im Dialogfeld **Authentifizierung ändern** klicken Sie auf **Keine Authentifizierung**, und klicken Sie dann auf **OK**. In diesem Lernprogramm haben Sie eine app bereitstellen, die eine Anmeldung des Benutzers nicht.

    ![][18]

6.  Im Dialogfeld **Neues Projekt von ASP.NET** im Abschnitt **Microsoft Azure** Vergewissern Sie sich, dass **in der Cloud zu hosten** ausgewählt ist, dass die **App-Dienst** in der Dropdown-Liste ausgewählt ist.

    ![][19]

7. Klicken Sie auf **OK**. 

8. Sie müssen jetzt Azure Ressourcen für eine neue Web app konfigurieren. Führen Sie alle Schritte im Abschnitt [Azure Konfigurieren von Ressourcen für eine neue Web app](../app-service-web/web-sites-dotnet-get-started.md#configure-azure-resources-for-a-new-web-app). Klicken Sie dann in diesem Lernprogramm wieder, und fahren Sie mit dem nächsten Schritt fort.

5.  Im Lösung-Explorer mit der rechten Maustaste **Modelle** dann klicken Sie auf **Hinzufügen**, und klicken Sie auf **Klasse**. Geben Sie im Feld **Name** den Namen **Product.cs**aus. Klicken Sie dann auf **Hinzufügen**.

    ![][17]

### <a name="modify-the-web-application"></a>Ändern der Webanwendung

1.  Ersetzen Sie in der Datei Product.cs in Visual Studio die vorhandene Namespacedefinition mit den folgenden Code ein.

    ```
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
        public class Product
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public string Quantity { get; set; }
        }
    }
    ```

2.  Explorer-Lösung erweitern Sie den Ordner **Controller** , und doppelklicken Sie auf die Datei **HomeController.cs** , um es in Visual Studio öffnen.

3. Ersetzen Sie in **HomeController.cs**die vorhandenen Namespacedefinition mit den folgenden Code ein.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;
    
        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```

3.  Erweitern Sie den Ordner Views\Shared ebenfalls einen Lösung-Explorer, und doppelklicken Sie auf **_Layout.cshtml** , um es im Visual Studio-Editor zu öffnen.

5.  Ändern Sie alle Vorkommen des **My ASP.NET Application** in **LITWAREs-Produkte**ein.

6. Entfernen Sie die Links **Home**, **zu**und **Kontakt** . Löschen Sie im folgenden Beispiel den hervorgehobenen Code ein.

    ![][41]

7.  Explorer-Lösung erweitern Sie den Views\Home den Ordner, und doppelklicken Sie auf **Index.cshtml** , um es im Visual Studio-Editor zu öffnen.
    Ersetzen Sie den gesamten Inhalt der Datei durch den folgenden Code ein.

    ```
    @model IEnumerable<ProductsWeb.Models.Product>
    
    @{
            ViewBag.Title = "Index";
    }
    
    <h2>Prod Inventory</h2>
    
    <table>
            <tr>
                <th>
                    @Html.DisplayNameFor(model => model.Name)
                </th>
                  <th></th>
                <th>
                    @Html.DisplayNameFor(model => model.Quantity)
                </th>
            </tr>
    
    @foreach (var item in Model) {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.Name)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Quantity)
                </td>
            </tr>
    }
    
    </table>
    ```

9.  Um zu überprüfen, ob die Genauigkeit der Ihre Arbeit zurückbekommen, können Sie **STRG + UMSCHALT + B** zum Erstellen des Projekts drücken.


### <a name="run-the-app-locally"></a>Führen Sie die app lokal

Führen Sie die Anwendung, um sicherzustellen, dass er immer funktioniert.

1.  Stellen Sie sicher, dass **ProductsPortal** des aktiven Projekts ist. Mit der rechten Maustaste im Explorer-Lösung des Projektnamens ein, und wählen Sie **Als beim Start Project**.
2.  Drücken Sie F5 in Visual Studio.
3.  Die Anwendung sollte angezeigt werden, in einem Browser ausgeführt.

    ![][21]

## <a name="put-the-pieces-together"></a>Teile eines ganzen

Im nächsten Schritt wird der Server lokal Produkte mit der Anwendung ASP.NET einbinden.

1.  Ist dies nicht bereits geöffnet, in Visual Studio erneut geöffnet **ProductsPortal** Projekts, die Sie im Abschnitt [Erstellen einer ASP.NET-Anwendung](#create-an-aspnet-application) erstellt haben.

2.  Ähnlich wie der Schritt im Abschnitt "Erstellen eines auf lokale Servers" hinzufügen NuGet-Paket den Projekt verweisen. Klicken Sie im Explorer-Lösung mit der rechten Maustaste in des **ProductsPortal** Projekts und dann auf **NuGet-Pakete verwalten**.

3.  Suchen Sie nach "Dienstbus", und wählen Sie das Element **Microsoft Azure-Dienstbus** . Klicken Sie dann abgeschlossen Sie die Installation, und schließen Sie das Dialogfeld.

4.  Lösung-Explorer mit der rechten Maustaste in des Projekts **ProductsPortal** , und klicken Sie dann klicken Sie auf **Hinzufügen**, klicken Sie dann die **Vorhandene Element**.

5.  Navigieren Sie zu der Datei **ProductsContract.cs** aus dem **ProductsServer** Console-Projekt aus. Klicken Sie auf diese Option, um ProductsContract.cs zu markieren. Klicken Sie auf den Pfeil nach unten neben **Hinzufügen**, und klicken Sie auf **als Link hinzufügen**.

    ![][24]

6.  Jetzt öffnen Sie die Datei **HomeController.cs** im Visual Studio-Editor, und Ersetzen Sie die Namespacedefinition mit den folgenden Code. Achten Sie darauf, dass *YourServiceNamespace* durch den Namen Ihrer Dienstnamespace und *YourKey* mit Ihrem SAS Schlüssel zu ersetzen. Dadurch wird den Client zu der lokalen, gibt das Ergebnis des Anrufs Serviceanfrage aktiviert.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Linq;
        using System.ServiceModel;
        using System.Web.Mvc;
        using Microsoft.ServiceBus;
        using Models;
        using ProductsServer;
    
        public class HomeController : Controller
        {
            // Declare the channel factory.
            static ChannelFactory<IProductsChannel> channelFactory;
    
            static HomeController()
            {
                // Create shared access signature token credentials for authentication.
                channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                    "sb://yourServiceNamespace.servicebus.windows.net/products");
                channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                    TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                        "RootManageSharedAccessKey", "yourKey") });
            }
    
            public ActionResult Index()
            {
                using (IProductsChannel channel = channelFactory.CreateChannel())
                {
                    // Return a view of the products inventory.
                    return this.View(from prod in channel.GetProducts()
                                     select
                                         new Product { Id = prod.Id, Name = prod.Name,
                                             Quantity = prod.Quantity });
                }
            }
        }
    }
    ```

7.  Explorer-Lösung mit der Maustaste der Lösung **ProductsPortal** (Stellen Sie sicher, dass die Lösung, die nicht das Projekt Rechtsklick). Klicken Sie auf **Hinzufügen**, und klicken Sie auf **Vorhandenes Projekt**.

8.  Navigieren Sie zu dem Projekt **ProductsServer** , und doppelklicken Sie auf die Datei **ProductsServer.csproj** Lösung, um es hinzuzufügen.

9.  **ProductsServer** muss ausgeführt werden, damit die Daten auf **ProductsPortal**angezeigt. Lösung-Explorer mit der rechten Maustaste in der Lösung **ProductsPortal** und dann auf **Eigenschaften**. Im Dialogfeld **Eigenschaften** wird angezeigt.

10. Klicken Sie auf der linken Seite auf **Start-Projekt**. Klicken Sie auf der rechten Seite auf **mehrere Startprojekte**. Sicherstellen Sie, dass **ProductsServer** und **ProductsPortal** , in dieser Reihenfolge an, mit **Starten** festlegen als Aktion für beide angezeigt werden.

      ![][25]

11. Klicken Sie weiterhin im Dialogfeld **Eigenschaften** auf der linken Seite auf **Projekt Abhängigkeiten** .

12. Klicken Sie in der Liste **Projekte** auf **ProductsServer**. Stellen Sie sicher, dass **ProductsPortal** ist **nicht** aktiviert.

14. Klicken Sie in der Liste **Projekte** auf **ProductsPortal**. Stellen Sie sicher, dass **ProductsServer** ausgewählt ist. 

    ![][26]

15. Klicken Sie auf **OK** im Dialogfeld **Eigenschaften** .

## <a name="run-the-project-locally"></a>Führen Sie das Projekt lokal

Wenn Sie um die Anwendung lokal zu testen, drücken Sie **F5**in Visual Studio. Der Server lokal (**ProductsServer**) sollte ersten, und dann die **ProductsPortal** -Anwendung sollte in einem Browserfenster zu starten. Diesmal, sehen Sie, dass Produktbestands aus dem Produkt Dienst lokalen System abgerufene Daten enthält.

![][10]

Drücken Sie auf der Seite **ProductsPortal** **Aktualisieren** . Jedes Mal, aktualisieren Sie die Seite, sehen Sie die Server-app Anzeigen einer Meldung beim `GetProducts()` von **ProductsServer** aufgerufen wird.

Schließen Sie beide Programme, bevor Sie mit dem nächsten Schritt fort.

## <a name="deploy-the-productsportal-project-to-an-azure-web-app"></a>Bereitstellen des Projekts ProductsPortal zu einer Azure Web app

Im nächsten Schritt wird die **ProductsPortal** Front-End in einer Azure Web app konvertiert. Zunächst bereitstellen Sie **ProductsPortal** Projekt, alle die Schritte im Abschnitt [Bereitstellen der Project Web zu Azure Web app](../app-service-web/web-sites-dotnet-get-started.md#deploy-the-web-project-to-the-azure-web-app). Nach Abschluss der Bereitstellung dieses Lernprogramms wieder, und fahren Sie mit dem nächsten Schritt fort.

> [AZURE.NOTE] Sie können eine Fehlermeldung im Browserfenster möglicherweise angezeigt, wenn das **ProductsPortal** Web-Projekt nach der Bereitstellung automatisch gestartet wird. Dies ist zu erwarten und tritt auf, da die Anwendung **ProductsServer** noch ausgeführt wird.

Kopieren Sie die URL des bereitgestellten Web app, da Sie die URL im nächsten Schritt benötigen. Sie können auch diese URL aus dem Fenster Azure App Dienst Aktivität in Visual Studio erhalten:

![][9] 

### <a name="set-productsportal-as-web-app"></a>Festlegen von ProductsPortal als Web app

Bevor Sie die Anwendung in der Cloud ausführen, müssen Sie sicherstellen, dass **ProductsPortal** in Visual Studio als Web app gestartet wird.

1. In Visual Studio mit der rechten Maustaste in des Projekts **ProjectsPortal** , und klicken Sie dann auf **Eigenschaften**.

3. Klicken Sie in der linken Spalte auf **Web**.

5. Im Abschnitt **Aktion beginnen** klicken Sie auf die Schaltfläche **Start-URL** , und geben Sie im Textfeld die URL für Ihre zuvor bereitgestellte Web app; beispielsweise `http://productsportal1234567890.azurewebsites.net/`.

    ![][27]

6. Klicken Sie in Visual Studio im Menü **Datei** auf **Alles speichern**.

7. Klicken Sie im Menü Build in Visual Studio auf **Lösung neu erstellen**.

## <a name="run-the-application"></a>Führen Sie die Anwendung

2.  Drücken Sie F5, um zu erstellen, und führen Sie die Anwendung aus. Sollte der lokalen Server (die Anwendung **ProductsServer** Console) ersten Starten und dann die Anwendung **ProductsPortal** sollte in einem Browserfenster beginnen, wie im folgenden Screenshot dargestellt. Beachten Sie erneut, dass Produktbestands führt die Daten aus dem Produkt Dienst lokalen Systems abgerufen und diese Daten in der Web-app zeigt. Überprüfen Sie die URL, um sicherzustellen, dass **ProductsPortal** in der Cloud, als einer Azure Web app ausgeführt wird. 

    ![][1]

    > [AZURE.IMPORTANT] Die Anwendung **ProductsServer** Console muss ausgeführt werden und die Daten mit der Anwendung **ProductsPortal** bedienen. Wenn einen Fehler im Browser angezeigt wird, warten Sie ein paar weitere Sekunden **ProductsServer** laden und die folgende Meldung angezeigt. Drücken Sie dann im Browser **Aktualisieren** .

    ![][37]

3. Wieder im Browser drücken Sie auf der Seite **ProductsPortal** **Aktualisieren** . Jedes Mal, aktualisieren Sie die Seite, sehen Sie die Server-app Anzeigen einer Meldung beim `GetProducts()` von **ProductsServer** aufgerufen wird.

    ![][38]

## <a name="next-steps"></a>Nächste Schritte  

Weitere Informationen zu Dienstbus finden Sie unter den folgenden Ressourcen:  

* [Azure Dienstbus][sbwacom]  
* [So verwenden Sie Bus Servicewarteschlangen][sbwacomqhowto]  


  [0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
  [1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [Get-Tools und SDK]: http://go.microsoft.com/fwlink/?LinkId=271920
  [NuGet]: http://nuget.org
  
  [11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
  [13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
  [15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
  [16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
  [17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
  [18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
  [19]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-6.png
  [9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
  [10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

  [21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
  [24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
  [25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
  [26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
  [27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png
  
  [36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
  [38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
  [41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
  [43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png


  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

