<properties
    pageTitle="Anwendung von .NET mit mehreren Ebenen | Microsoft Azure"
    description="Ein, die Ihnen dabei hilft .NET-Lernprogramm entwickeln eine app mit mehreren Ebene in Azure, die Servicebuswarteschlangen zur Kommunikation zwischen Ebenen verwendet."
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
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>

# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>Anwendung Azure Servicebuswarteschlangen mithilfe von .NET mit mehreren Ebenen

## <a name="introduction"></a>Einführung

Entwickeln für Microsoft Azure ist einfach mithilfe von Visual Studio und dem kostenlosen Azure SDK für .NET. Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen einer Anwendung, die mehrere Azure Ressourcen, die in Ihrer lokalen Umgebung ausgeführt verwendet. Den Schritten wird vorausgesetzt, dass Sie keine vorherige Erfahrung mit Azure haben.

Sie lernen Folgendes:

-   So aktivieren Sie den Computer für Azure-Entwicklung mit einem einzelnen herunterladen und installieren.
-   So verwenden Sie Visual Studio für Azure entwickeln.
-   Informationen zum Erstellen einer Anwendungs mit mehreren Ebenen in Azure Web- und Worker Rollen verwenden.
-   Informationen zum Kommunizieren zwischen Ebenen Servicebuswarteschlangen verwenden.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

In diesem Lernprogramm Sie erstellen und Ausführen die Anwendung mit mehreren Ebene in einem Azure-Cloud-Dienst. Front-End kann ich eine ASP.NET-MVC Webrolle und Back-End wird eine Worker-Rolle, die eine Warteschlange Dienstbus verwendet werden. Sie können die gleiche Anwendung mit mehreren Ebene mit der front-End als ein Webprojekt erstellen, die auf eine Website zu Azure statt als Clouddienst bereitgestellt wird. Informationen dazu, was zu tun ist Klicken Sie auf einer Website Azure-front-End anders finden Sie unter Abschnitt [Weitere Schritte](#nextsteps) . Sie können auch die [.NET lokale/Cloud Hybrid Anwendung](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) Lernprogramm ausprobieren.

Die folgende Abbildung zeigt die fertige Anwendung.

![][0]

## <a name="scenario-overview-inter-role-communication"></a>Übersicht über das Szenario: Kommunikation zwischen Rolle

Um Ordnung für die Verarbeitung zu senden, muss die Front-End-Komponente der Benutzeroberfläche in die Web-Rolle ausführen mit der Ausführung im Worker-Rolle mittleren Ebene Logik interagieren. In diesem Beispiel wird die Dienstbus vermittelte messaging für die Kommunikation zwischen Ebenen verwendet.

Mithilfe von vermittelten messaging zwischen dem Internet und mittlere Ebenen trennt die beiden Komponenten. Im Gegensatz zu direkter messaging (d. h., TCP oder HTTP), wird die Webebene nicht der mittleren Ebene direkt Verbindung; Stattdessen legt es Einheiten für Arbeit, als Nachrichten in Dienstbus, die zuverlässig diese behält bis mittlere Ebene bereit sind ist, nutzen und verarbeitet werden.

Dienst Bedienfeld enthält zwei Einheiten vermittelten messaging unterstützt: Warteschlangen und Themen. Mit Warteschlangen wird jede Nachricht, die mit der Warteschlange von einem einzelnen Empfänger genutzt. Themen unterstützt das Veröffentlichen/Abonnieren Muster, in dem jede Nachricht veröffentlichten ein Abonnement mit dem Thema registriert zur Verfügung gestellt werden. Jedes Abonnement unterhält logisch seine eigene Warteschlange Nachrichten. Abonnements können auch Filterregeln konfiguriert werden, die den Satz von Nachrichten in der Warteschlange Abonnements auf, die mit dem Filter übereinstimmenden übergeben einschränken. Im folgende Beispiel wird die Servicebuswarteschlangen verwendet.

![][1]

Diesem Kommunikationsverfahren weist verschiedene Vorteile gegenüber direkte messaging:

-   **Zeitliche Entkopplung.** Mit dem Muster asynchronen messaging erfolgen Hersteller und Verbraucher online zur gleichen Zeit. Dienstbus speichert Nachrichten zuverlässig, bis die in Anspruch nehmen Partei erhalten sie bereit ist. Dadurch werden die Komponenten einer verteilten Anwendung, entweder freiwillig, beispielsweise für die Wartung oder aufgrund von einem Absturz Komponente getrennt werden beeinträchtigt das System als Ganzes. Darüber hinaus müssen die Anwendung in Anspruch nehmen nur bestimmten Zeiten des Tages online sind.

-   **Abgleich zu laden.** In vielen Programmen variierende System laden im Laufe der Zeit, während die für jede Arbeitseinheit benötigte Verarbeitungszeit normalerweise konstant ist. Intermediating Nachricht Hersteller und Nutzer mit einer Warteschlange bedeutet, dass nur muss die Anwendung in Anspruch nehmen (der Worker) bereitgestellt werden, um durchschnittliche Auslastung statt die Belastung aufnehmen zu können. Die Tiefe der Warteschlange wächst und reduziert, wenn die eingehende laden wird aktualisiert. Dadurch wird direkt Geld in Hinblick auf die erforderliche Infrastruktur, um die Anwendung laden service gespeichert.

-   **Für den Lastenausgleich.** Wie Auslastung gesteigert wird, können weitere Arbeitsprozesse Lesen aus der Warteschlange hinzugefügt werden. Jede Nachricht wird nur von einem der Arbeitsprozesse verarbeitet werden. Darüber hinaus ermöglicht diese Abruf-basierten Lastenausgleich optimale Nutzung der Computer Worker, selbst wenn die Worker Computer im Hinblick auf Verarbeitung Power, unterscheiden sich diese Nachrichten mit eigene maximale Rate ziehen werden. Dieses Muster spricht oft das Muster *Consumer als Mitbewerber auftritt* .

    ![][2]

In den folgenden Abschnitten erläutern Sie den Code, der diese Architektur implementiert.

## <a name="set-up-the-development-environment"></a>Einrichten der Entwicklungsumgebung

Bevor Sie die Entwicklung von Azure Applications beginnen können, erhalten Sie die Tools, und richten Sie Ihre Entwicklungsumgebung.

1.  Installieren Sie das Azure SDK für .NET auf [Tools und SDK erhalten][].

2.  Klicken Sie auf **das SDK installieren** , nach der Version von Visual Studio, die Sie verwenden. Die Schritte in diesem Lernprogramm verwenden Sie Visual Studio 2015.

4.  Wenn Sie dazu aufgefordert werden, ausführen oder speichern das Installationsprogramm, klicken Sie auf **Ausführen**.

5.  Klicken Sie im **Web Platform Installer**auf **Installieren** und die Installation fort.

6.  Sobald die Installation abgeschlossen ist, haben Sie alles, was Sie zu beginnen, um die app zu entwickeln. Das SDK enthält Tools, mit die Sie einfach Azure Applications in Visual Studio entwickeln können. Wenn Sie nicht Visual Studio installiert haben, installiert das SDK auch kostenlosen Visual Studio Express.

## <a name="create-a-namespace"></a>Erstellen Sie einen namespace

Im nächsten Schritt wird zu einen Dienstnamespace erstellen und Sie erhalten einen Schlüssel freigegeben Access Signatur (SAS). Ein Namespace stellt eine Begrenzungslinie Anwendung für jede Anwendung über Dienstbus zur Verfügung gestellt. Eine SAS-Taste vom System generiert, wenn Sie ein Namespace erstellt wird. Die Kombination von Namespace und SAS-Taste stellt die Anmeldeinformationen für Dienstbus zum Authentifizieren des Zugriffs auf eine Anwendung.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Erstellen einer Webrolle

In diesem Abschnitt erstellen Sie die front-End der Anwendung. Erstellen Sie Sie zuerst die Seiten, die in Ihrer Anwendung angezeigt.
Fügen Sie anschließend Code, der Elemente in einer Warteschlange Dienstbus sendet und zeigt Statusinformationen in der Warteschlange hinzu.

### <a name="create-the-project"></a>Erstellen Sie das Projekt

1.  Starten Sie Microsoft Visual Studio mithilfe von Administratorrechte. Um Visual Studio mit Administratorrechten starten, mit der rechten Maustaste in das Symbol für das **Visual Studio** -Programm, und klicken Sie dann auf **als Administrator ausführen**. Der Azure-Serveremulator weiter unten in diesem Artikel erläutert erfordert, dass Visual Studio mit Administratorrechten gestartet werden.

    Klicken Sie in Visual Studio im Menü **Datei** auf **neu**, und klicken Sie dann auf **Projekt**.

2.  **Installierte Vorlagen**unter **Visual c#**klicken Sie auf die **Cloud** und klicken Sie dann auf **Azure-Cloud-Dienst**. Nennen Sie das Projekt **MultiTierApp**. Klicken Sie dann auf **OK**.

    ![][9]

3.  **.NET Framework 4.5** Rollen Doppelklicken Sie auf **ASP.NET Webrolle**.

    ![][10]

4.  Zeigen Sie auf **WebRole1** unter **Azure-Cloud-Service-Lösung**, klicken Sie auf das Symbol Tool "Bleistift", und benennen Sie die Web-Rolle in **FrontendWebRole**. Klicken Sie dann auf **OK**. (Stellen Sie sicher, dass Sie mit einem Kleinbuchstaben "e," nicht "Front-End" "Front-End" eingeben.)

    ![][11]

5.  Klicken Sie im Dialogfeld **Neues Projekt von ASP.NET** in der Liste **Wählen Sie eine Vorlage** auf **MVC**.

    ![][12]

6. Weiterhin im Dialogfeld **Neue ASP.NET-Projekt** klicken Sie auf die Schaltfläche **Authentifizierung ändern** . Führen Sie im Dialogfeld **Authentifizierung ändern** klicken Sie auf **Keine Authentifizierung**, und klicken Sie dann auf **OK**. In diesem Lernprogramm haben Sie eine app bereitstellen, die eine Anmeldung des Benutzers nicht.

    ![][16]

7. Klicken Sie auf **OK** , um das Projekt zu erstellen, klicken Sie im Dialogfeld **Neues Projekt von ASP.NET** .

6.  Klicken Sie im **Explorer Lösung**im Projekt **FrontendWebRole** mit der rechten Maustaste **Verweise**und dann auf **NuGet-Pakete verwalten**.

7.  Klicken Sie auf die Registerkarte **Durchsuchen** , und suchen Sie nach `Microsoft Azure Service Bus`. Klicken Sie auf **Installieren**, und akzeptieren Sie der Vereinbarung.

    ![][13]

    Beachten Sie, dass die erforderlichen Clientassemblys werden nun auf die verwiesen wird, und einige neue Code hinzugefügt wurden.

9.  Mit der rechten Maustaste **Modelle** in **Lösung-Explorer**klicken Sie auf **Hinzufügen**, und klicken Sie auf **Klasse**. Geben Sie im Feld **Name** den Namen **OnlineOrder.cs**ein. Klicken Sie dann auf **Hinzufügen**.

### <a name="write-the-code-for-your-web-role"></a>Schreiben Sie den Code für Ihre Webrolle

In diesem Abschnitt erstellen Sie die verschiedenen Seiten, die in Ihrer Anwendung angezeigt.

1.  Ersetzen Sie in der Datei OnlineOrder.cs in Visual Studio die vorhandene Namespacedefinition mit den folgenden Code ein:

    ```
    namespace FrontendWebRole.Models
    {
        public class OnlineOrder
        {
            public string Customer { get; set; }
            public string Product { get; set; }
        }
    }
    ```

2.  Doppelklicken Sie auf **Lösung Explorer** **Controllers\HomeController**. Fügen Sie am oberen Rand der Datei Namespaces für das Datenmodell enthalten, die gerade erstellten, sowie Dienstbus folgenden Aussagen **verwenden** .

    ```
    using FrontendWebRole.Models;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    ```

3.  Ersetzen Sie in der Datei HomeController.cs in Visual Studio auch die vorhandene Namespacedefinition mit den folgenden Code. Dieser Code enthält Methoden für den Umgang mit Einreichen von Elementen in der Warteschlange an.

    ```
    namespace FrontendWebRole.Controllers
    {
        public class HomeController : Controller
        {
            public ActionResult Index()
            {
                // Simply redirect to Submit, since Submit will serve as the
                // front page of this application.
                return RedirectToAction("Submit");
            }
    
            public ActionResult About()
            {
                return View();
            }
    
            // GET: /Home/Submit.
            // Controller method for a view you will create for the submission
            // form.
            public ActionResult Submit()
            {
                // Will put code for displaying queue message count here.
    
                return View();
            }
    
            // POST: /Home/Submit.
            // Controller method for handling submissions from the submission
            // form.
            [HttpPost]
            // Attribute to help prevent cross-site scripting attacks and
            // cross-site request forgery.  
            [ValidateAntiForgeryToken]
            public ActionResult Submit(OnlineOrder order)
            {
                if (ModelState.IsValid)
                {
                    // Will put code for submitting to queue here.
    
                    return RedirectToAction("Submit");
                }
                else
                {
                    return View(order);
                }
            }
        }
    }
    ```

4.  Klicken Sie im Menü **Erstellen** auf **Lösung erstellen** , um die Genauigkeit der Ihre Arbeit bisher zu testen.

5.  Erstellen Sie nun die Ansicht für den `Submit()` Methode, die Sie zuvor erstellt haben. Mit der rechten Maustaste in die `Submit()` Methode (die Überladung der `Submit()` , die keine Parameter annimmt), und wählen Sie dann auf **Ansicht hinzufügen**.

    ![][14]

6.  Für die Ansicht erstellen, wird ein Dialogfeld angezeigt. Wählen Sie in der Liste **Vorlage** **Erstellen**. Klicken Sie in der Liste **Modell Verzicht** auf der Klasse **OnlineOrder** .

    ![][15]

7.  Klicken Sie auf **Hinzufügen**.

8.  Jetzt, Ändern der angezeigte Name der Anwendung. **Lösung-Explorer**, doppelklicken Sie auf die **Views\Shared ebenfalls einen\\_Layout.cshtml** Datei, damit es in der Visual Studio-Editor geöffnet.

9.  Ersetzen Sie alle Vorkommen des **My ASP.NET Application** mit **LITWAREs-Produkten**aus.

10. Entfernen Sie die Links **Home**, **zu**und **Kontakt** . Löschen Sie den hervorgehobenen Code ein:

    ![][28]

11. Ändern Sie abschließend die Seite Übermittlung um einige Informationen über die Warteschlange aufzunehmen. **Lösung-Explorer**Doppelklicken Sie auf die Datei **Views\Home\Submit.cshtml** , um es im Visual Studio-Editor zu öffnen. Fügen Sie die folgende Zeile nach `<h2>Submit</h2>`. Jetzt die `ViewBag.MessageCount` leer ist. Sie werden später auffüllen.

    ```
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```

12. Jetzt haben Sie die Benutzeroberfläche implementiert. Drücken Sie **F5** , um die Anwendung ausführen und bestätigen, dass es sieht so aus, wie erwartet.

    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a>Schreiben Sie den Code für das Senden an eine Warteschlange Dienstbus Elemente

Fügen Sie nun Code für Einreichen von Elementen zu einer Warteschlange hinzu. Zunächst erstellen Sie eine Klasse, die Ihre Dienstbus Warteschlange Verbindungsinformationen enthält. Klicken Sie dann Initialisierung die Verbindung aus Global.aspx.cs aus. Schließlich aktualisieren den Übermittlung Code zuvor in erstellten HomeController.cs Elemente an eine Warteschlange Dienstbus tatsächlich zu übermitteln.

1.  **Lösung-Explorer**mit der Maustaste **FrontendWebRole** (Rechtsklick am Projekt nicht die Rolle). Klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Class**.

2.  Nennen Sie die Klasse **QueueConnector.cs**. Klicken Sie auf **Hinzufügen** , um die Klasse zu erstellen.

3.  Fügen Sie nun Code, der die Verbindungsinformationen kapselt und die Verbindung mit einer Dienstbus Warteschlange initialisiert hinzu. Ersetzen Sie den gesamten Inhalt der QueueConnector.cs mit den folgenden Code ein, und geben Sie Werte für `your Service Bus namespace` (Ihr Namespacename) und `yourKey`, welche ist die **Primärschlüssel** zuvor vom Azure-Portal erworbenen.

    ```
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    
    namespace FrontendWebRole
    {
        public static class QueueConnector
        {
            // Thread-safe. Recommended that you cache rather than recreating it
            // on every request.
            public static QueueClient OrdersQueueClient;
    
            // Obtain these values from the portal.
            public const string Namespace = "your Service Bus namespace";
    
            // The name of your queue.
            public const string QueueName = "OrdersQueue";
    
            public static NamespaceManager CreateNamespaceManager()
            {
                // Create the namespace manager which gives you access to
                // management operations.
                var uri = ServiceBusEnvironment.CreateServiceUri(
                    "sb", Namespace, String.Empty);
                var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                    "RootManageSharedAccessKey", "yourKey");
                return new NamespaceManager(uri, tP);
            }
    
            public static void Initialize()
            {
                // Using Http to be friendly with outbound firewalls.
                ServiceBusEnvironment.SystemConnectivity.Mode =
                    ConnectivityMode.Http;
    
                // Create the namespace manager which gives you access to
                // management operations.
                var namespaceManager = CreateNamespaceManager();
    
                // Create the queue if it does not exist already.
                if (!namespaceManager.QueueExists(QueueName))
                {
                    namespaceManager.CreateQueue(QueueName);
                }
    
                // Get a client to the queue.
                var messagingFactory = MessagingFactory.Create(
                    namespaceManager.Address,
                    namespaceManager.Settings.TokenProvider);
                OrdersQueueClient = messagingFactory.CreateQueueClient(
                    "OrdersQueue");
            }
        }
    }
    ```

4.  Jetzt, stellen Sie sicher, dass Ihre **Initialisierung** Methode aufgerufen wird. **Lösung-Explorer**Doppelklicken Sie auf **Global.asax\Global.asax.cs**.

5.  Fügen Sie die folgende Zeile des Codes am Ende der Methode **Application_Start** hinzu.

    ```
    FrontendWebRole.QueueConnector.Initialize();
    ```

6.  Aktualisieren Sie schließlich the Web-Code, die Sie zuvor erstellt haben, um Elemente in der Warteschlange zu übermitteln. Doppelklicken Sie auf **Lösung Explorer** **Controllers\HomeController**.

7.  Aktualisieren der `Submit()` Methode (die Überladung, die keine Parameter annimmt) wie folgt, um die Nachricht für die Warteschlange ermitteln.

    ```
    public ActionResult Submit()
    {
        // Get a NamespaceManager which allows you to perform management and
        // diagnostic operations on your Service Bus queues.
        var namespaceManager = QueueConnector.CreateNamespaceManager();
    
        // Get the queue, and obtain the message count.
        var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
        ViewBag.MessageCount = queue.MessageCount;
    
        return View();
    }
    ```

8.  Aktualisieren der `Submit(OnlineOrder order)` Methode (die Überladung, die einen Parameter annimmt) wie folgt, um die Reihenfolge der Warteschlange Informationen zu übermitteln.

    ```
    public ActionResult Submit(OnlineOrder order)
    {
        if (ModelState.IsValid)
        {
            // Create a message from the order.
            var message = new BrokeredMessage(order);
    
            // Submit the order.
            QueueConnector.OrdersQueueClient.Send(message);
            return RedirectToAction("Submit");
        }
        else
        {
            return View(order);
        }
    }
    ```

9.  Sie können nun die Anwendung erneut ausführen. Jedes Mal, wenn Sie Ordnung, übermitteln erhöht die Anzahl der Nachricht.

    ![][18]

## <a name="create-the-worker-role"></a>Erstellen der Worker-Rolle

Erstellen Sie jetzt die Worker-Rolle, die die Reihenfolge Übermittlungen verarbeitet. In diesem Beispiel wird die **Worker-Rolle mit Service Bus Warteschlange** Visual Studio-Projektvorlage. Sie erhalten die erforderlichen Anmeldeinformationen bereits auf dem Portal haben.

1. Stellen Sie sicher, dass Sie Visual Studio mit Ihrem Azure-Konto verbunden haben.

2.  **In Visual Studio Solution Explorer** mit der Maustaste des Ordners **Rollen** unter dem Projekt **MultiTierApp** .

3.  Klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Neues Worker Rolle Projekt**. Das Dialogfeld **Neues Rolle Projekt hinzufügen** wird angezeigt.

    ![][26]

4.  Klicken Sie im Dialogfeld **Neues Projekt/Rolle hinzufügen** auf **Worker-Rolle mit Service Bus Warteschlange**.

    ![][23]

5.  Geben Sie im Feld **Name** dem Projekt **OrderProcessingRole**ein. Klicken Sie dann auf **Hinzufügen**.

6.  Kopieren Sie die Verbindungszeichenfolge, die Sie für Ihren Kunden in Schritt 9 des Abschnitts "Erstellen einen Dienstbus Namespace" in die Zwischenablage.

7.  **Lösung-Explorer**, mit der Maustaste die **OrderProcessingRole** , die Sie in Schritt 5 erstellten (Stellen Sie sicher, dass Sie mit der rechten **OrderProcessingRole** unter **Rollen**und nicht die Klasse Maustaste). Klicken Sie dann auf **Eigenschaften**.

8.  Klicken Sie in das Feld **Wert** für **Microsoft.ServiceBus.ConnectionString**auf der Registerkarte **Einstellungen** im Dialogfeld **Eigenschaften** , und fügen Sie dann den Endpunktwert, den Sie in Schritt 6 kopiert haben.

    ![][25]

9.  Erstellen Sie eine **OnlineOrder** -Klasse, um die Bestellungen darstellen, wie Sie sie aus der Warteschlange verarbeiten. Sie können eine Klasse wiederverwenden, die Sie bereits erstellt haben. **Lösung-Explorer**mit der Maustaste der Klasse **OrderProcessingRole** (der rechten Maustaste auf das Symbol Class, nicht die Rolle). Klicken Sie auf **Hinzufügen**, und klicken Sie auf **Vorhandenes Element**.

10. Suchen Sie den Unterordner für **FrontendWebRole\Models**, und doppelklicken Sie dann auf **OnlineOrder.cs** , um sie dieses Projekt hinzuzufügen.

11. In **WorkerRole.cs**, ändern Sie den Wert der **QueueName** Variable vom `"ProcessingQueue"` auf `"OrdersQueue"` wie im folgenden Code dargestellt.

    ```
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```

12. Fügen Sie die folgende Anweisung am oberen Rand der Datei WorkerRole.cs verwenden.

    ```
    using FrontendWebRole.Models;
    ```

13. In der `Run()` Funktion, innerhalb der `OnMessage()` anrufen, ersetzen Sie den Inhalt von der `try` -Klausel mit den folgenden Code.

    ```
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```

14. Die Anwendung abgeschlossen haben. Sie können die vollständige Anwendung testen, indem Sie mit der rechten Maustaste im Projekts MultiTierApp im Solution Explorer, **Festlegen als beim Start Project**auswählen, und drücken F5. Beachten Sie, dass die Anzahl der Nachricht nicht erhöht, da die Worker-Rolle Elemente aus der Warteschlange verarbeitet und diese als abgeschlossen markiert werden, ein. Sie können die Ausgabe Spur Ihrer Worker-Rolle anzeigen, indem Sie die Azure berechnen Emulator Benutzeroberfläche anzeigen. Dies ist möglich, indem Sie mit der rechten Maustaste in des Emulator-Symbol im Benachrichtigungsbereich der Taskleiste, und Auswählen von **Berechnen Emulator-Benutzeroberfläche anzeigen**.

    ![][19]

    ![][20]

## <a name="next-steps"></a>Nächste Schritte  

Weitere Informationen zu Dienstbus finden Sie unter den folgenden Ressourcen:  

* [Azure Dienstbus][sbmsdn]  
* [Dienstbus Service-Seite][sbwacom]  
* [So verwenden Sie Bus Servicewarteschlangen][sbwacomqhowto]  

Weitere Informationen zu Szenarien mit mehreren Ebenen finden Sie unter:  

* [Anwendung mit Speichertabellen, Warteschlangen und Blobs von .NET mit mehreren Ebenen][mutitierstorage]  

  [0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-01.png
  [1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
  [2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
  [Get-Tools und SDK]: http://go.microsoft.com/fwlink/?LinkId=271920


  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [Microsoft.WindowsAzure.Configuration.CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx
  [NamespaceMananger]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx

  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx

  [EventHubClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx

  [9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
  [10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
  [11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
  [12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
  [13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
  [14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
  [15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
  [16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
  [17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-36.png
  [18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-37.png

  [19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
  [20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
  [23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
  [25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
  [26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
  [28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

  [sbmsdn]: http://msdn.microsoft.com/library/azure/ee732537.aspx  
  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
  [mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
  