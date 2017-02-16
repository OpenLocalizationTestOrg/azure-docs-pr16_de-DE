<properties
   pageTitle="Optimieren Ihrer Azure-Code in Visual Studio | Microsoft Azure"
   description="Erfahren Sie, wie Azure Code optimieren von Tools in Visual Studio Code mehr robuste und diejenigen stellen."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="optimizing-your-azure-code"></a>Optimieren von Azure Code

Wenn Sie apps, die Microsoft Azure verwenden Programmierung sind, finden Sie einige Codierung Vorgehensweisen, denen, die Sie folgen sollte zur Vermeidung von Problemen mit app Skalierbarkeit, Verhalten und Leistung in einen Cloud-Umgebung, vorhanden. Microsoft stellt eine Azure Codeanalysetool, das erkennt und identifiziert mehrere dieser Probleme häufig auftreten können Sie sie auflösen. Sie können die Tools in Visual Studio über NuGet herunterladen.

## <a name="azure-code-analysis-rules"></a>Azure Code Analysis-Regeln

Das Tool Code Azure verwendet die folgenden Regeln Azure Code beim Auffinden der bekannten Probleme bei der Leistung beeinträchtigen automatisch zu kennzeichnen. Erkannt Probleme als eine Warnung angezeigt oder Compilerfehler. Durch ein Symbol Glühbirne werden häufig Code behoben oder Vorschläge zum Beheben der Warnung oder ein Fehler bereitgestellt.

## <a name="avoid-using-default-in-process-session-state-mode"></a>Vermeiden Sie Standardmodus (in Bearbeitung) Sitzung-Zustand

### <a name="id"></a>ID

AP0000

### <a name="description"></a>Beschreibung

Wenn Sie für Applikationen Cloud (in Bearbeitung) Sitzung Zustand Standardmodus verwenden, können Sie Sitzungszustand gehen verloren.

Bitte teilen Sie Ihre Ideen und ihr Feedback bei [Azure Code Analysis Feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Grund

Standardmäßig ist der Sitzung Zustand Modus in der Datei web.config angegebenen in Bearbeitung. Darüber hinaus wird standardmäßig ist kein Eintrag in der Konfigurationsdatei angegeben wird, der Modus Session State in Bearbeitung. In Bearbeitung-Modus speichert Sitzungszustand im Arbeitsspeicher auf dem Webserver. Wenn eine Instanz neu gestartet wurde, oder eine neue Instanz für den Lastenausgleich oder Failoversupport verwendet wird, wird der Sitzungszustand auf dem Webserver im Arbeitsspeicher gespeichert nicht gespeichert. Diese Situation verhindert, dass die Anwendung wird in der Cloud skalierbare.

ASP.NET-Sitzungszustand unterstützt verschiedene Speicheroptionen für Sitzung Zustandsdaten: InProc, StateServer, SQLServer, Benutzerdefiniert, und deaktivieren. Es wird empfohlen, dass Sie benutzerdefinierte Modus Hostdaten auf einem externen Session State-Speicher, wie etwa [Azure Session State-Anbieter für Redis](http://go.microsoft.com/fwlink/?LinkId=401521)verwenden.

### <a name="solution"></a>Lösung

Eine empfohlene Lösung ist Sitzungszustand auf einer verwalteten cachediensts gespeichert. Informationen Sie zum [Azure Session State-Anbieter für Redis](http://go.microsoft.com/fwlink/?LinkId=401521) zu verwenden, um den Sitzungsstatus Ihrer zu speichern. Sie können Sitzungszustand auch an anderen Stellen, um sicherzustellen, dass eine Anwendung skalierbar ist, klicken Sie auf der Cloud speichern. Weitere Informationen zu Alternative lesen Lösungen bitte [Sitzung Zustand Modi](https://msdn.microsoft.com/library/ms178586)aus.

## <a name="run-method-should-not-be-async"></a>Ausführen der Methode sollten nicht asynchrone

### <a name="id"></a>ID

AP1000

### <a name="description"></a>Beschreibung

Erstellen Sie asynchrone Methoden (wie [erwartet](https://msdn.microsoft.com/library/hh156528.aspx)) außerhalb der Methode [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) , und rufen Sie die asynchronen Methoden [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx). Deklarieren die [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) -Methode als asynchrone bewirkt, dass die Worker-Rolle eine Schleife Restart eingeben.

Bitte teilen Sie Ihre Ideen und ihr Feedback bei [Azure Code Analysis Feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Grund

Aufrufen von asynchronen Methoden innerhalb der Methode [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) bewirkt, dass die Cloud-Dienstlaufzeit freizugebenden Worker-Rolle. Wenn eine Worker-Rolle gestartet wird, findet alle Ausführung des Programms innerhalb der Methode [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) . Beenden die [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) -Methode bewirkt, dass die Worker-Rolle, neu zu starten. Wenn die Laufzeit Worker Rolle die asynchronen Methode Treffer, sendet alle Vorgänge nach der Methode der asynchrone und gibt dann zurück. Dadurch wird die Worker-Rolle aus der Methode [[[[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) beenden und neu starten. In der nächsten Iteration der Ausführung Worker-Rolle die asynchronen Methode erneut Treffer und neu gestartet wurde, bewirken, dass die Worker-Rolle freizugebenden neu zu.

### <a name="solution"></a>Lösung

Platzieren Sie alle asynchrone Operationen außerhalb der [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) Methode. Rufen Sie dann die umgestalteten asynchrone Methode aus innerhalb der [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) -Methode RunAsync () .wait ein. Das Tool Azure Code kann helfen, dieses Problem zu beheben.

Der folgende Codeausschnitt veranschaulicht Codefix für dieses Problem:

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## <a name="use-service-bus-shared-access-signature-authentication"></a>Verwenden Sie Service Bus freigegeben Access Signatur-Authentifizierung

### <a name="id"></a>ID

AP2000

### <a name="description"></a>Beschreibung

Verwenden Sie freigegebene Access Signatur (SAS) für die Authentifizierung ein. Access Control Service (ACS) wird nicht mehr für Service Bus Authentifizierung unterstützt wird.

Bitte teilen Sie Ihre Ideen und ihr Feedback bei [Azure Code Analysis Feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Grund

Zur Verbesserung der Sicherheit ist Azure Active Directory ACS-Authentifizierung mit SAS-Authentifizierung ersetzt werden. Informationen zu den Übergangsplan finden Sie unter [Azure Active Directory sieht die Zukunft der ACS](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) .

### <a name="solution"></a>Lösung

Verwenden Sie in Ihrer apps SAS-Authentifizierung ein. Im folgenden Beispiel wird gezeigt, wie einen vorhandenen SAS Token Zugriff auf eine Service Bus Namespace oder Entität verwenden.

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

Weitere Informationen in den folgenden Themen finden Sie unter.

- Eine Übersicht finden Sie unter [Freigegebene Access Signatur Authentifizierung mit Dienstbus](https://msdn.microsoft.com/library/dn170477.aspx)

- [Verwenden von freigegebenen Access Signatur Authentifizierung mit Dienstbus](https://msdn.microsoft.com/library/dn205161.aspx)

- Ein Beispiel für Projekt finden Sie unter [Verwenden von freigegebenen Access Signatur (SAS) Authentifizierung mit Service Bus Abonnements](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)

## <a name="consider-using-onmessage-method-to-avoid-receive-loop"></a>Erwägen Sie die OnMessage-Methode zur Vermeidung von "erhalten Schleife" verwenden

### <a name="id"></a>ID

AP2002

### <a name="description"></a>Beschreibung

Um zu vermeiden in einer "erhalten Schleife" abgelegt, **OnMessage** Methode ist eine bessere Lösung für den Empfang von Nachrichten als die **Receive** -Methode aufrufen. Wenn Sie die **Receive** -Methode verwenden müssen, und ein Standardserver Wartezeit angeben, stellen Sie jedoch sicher, dass die Server Wartezeit mehr als einer Minute ist.

Bitte teilen Sie Ihre Ideen und ihr Feedback bei [Azure Code Analysis Feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Grund

Wenn **OnMessage**aufrufen, beginnt der Client eine interne Meldungsverteilschleife, der die Warteschlange oder das Abonnement ständig abfragt. Diese Meldungsverteilschleife enthält eine unbegrenzte Schleife, die einen Anruf empfangen von Nachrichten Probleme. Wenn Sie der Anruf abgelaufen ist, gibt er einen neuen Anruf. Das Timeoutintervall wird durch den Wert der Eigenschaft [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) von der [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)bestimmt, die verwendet wird.

Der Vorteil der Verwendung von **OnMessage** im Vergleich zu **empfangen** ist, dass Benutzer nicht auf manuell Umfrage für Nachrichten, Behandeln von Ausnahmen, mehrere Nachrichten parallel verarbeiten, und führen Sie die Nachrichten.

Wenn Sie **empfangen** aufrufen, ohne den Standardwert, achten Sie darauf, dass der Wert *ServerWaitTime* mehr als einer Minute ist. Einstellung *ServerWaitTime* verhindert, dass mehr als einer Minute Server überschritten wurde, ohne dass die Nachricht vollständig empfangen wurde.

### <a name="solution"></a>Lösung

Finden Sie in den folgenden Codebeispielen für Verwendungen empfohlen. Weitere Informationen hierzu finden Sie unter [QueueClient.OnMessage-Methode (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)und [QueueClient.Receive Methode (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx).

Um die Leistung der Azure messaging-Infrastruktur zu verbessern, finden Sie unter dem Entwurfsmuster [Asynchronen Messaging Einführung in](https://msdn.microsoft.com/library/dn589781.aspx).

So sieht ein Beispiel der Verwendung von **OnMessage** Nachrichten empfangen werden sollen.

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

So sieht ein Beispiel für die standardmäßige Server Wartezeit **empfangen** mit.

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

So sieht ein Beispiel für die Verwendung von **empfangen** mit einer nicht standardmäßigen Server Wartezeit.

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## <a name="consider-using-asynchronous-service-bus-methods"></a>Erwägen Sie asynchrone Dienstbus Methoden

### <a name="id"></a>ID

AP2003

### <a name="description"></a>Beschreibung

Verwenden Sie asynchrone Dienstbus Methoden zur Verbesserung der Systemleistung mit vermittelten messaging aus.

Bitte teilen Sie Ihre Ideen und ihr Feedback bei [Azure Code Analysis Feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Grund

Wie asynchrone Methoden ist, aktiviert die Anwendung Programm gleichzeitige, da jeder Anruf aufgeführte nicht im primären Thread blockiert. Bei Verwendung von Dienstbus messaging Methoden, Ausführen eines Vorgangs (senden, empfangen, löschen, usw.) erfordert. Diesmal enthält der Vorgang vom Dienst Dienstbus sowie die Wartezeit der Anfrage und die Antwort an. Klicken Sie zum Erhöhen der Anzahl der Vorgänge pro Zeit müssen Vorgänge gleichzeitig ausgeführt werden. Weitere Informationen finden Sie unter [Bewährte Methoden für die Leistung Verbesserungen mithilfe Service Bus vermittelte Messaging](https://msdn.microsoft.com/library/azure/hh528527.aspx).

### <a name="solution"></a>Lösung

Informationen zum Verwenden der empfohlenen asynchronen Methode finden Sie unter [QueueClient Klasse (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) .

Um die Leistung der Azure messaging-Infrastruktur zu verbessern, finden Sie unter dem Entwurfsmuster [Asynchronen Messaging Einführung in](https://msdn.microsoft.com/library/dn589781.aspx).

## <a name="consider-partitioning-service-bus-queues-and-topics"></a>Erwägen Sie die vorherigen Servicebuswarteschlangen und Themen

### <a name="id"></a>ID

AP2004

### <a name="description"></a>Beschreibung

Partition Servicebuswarteschlangen und bessere Leistung bei Dienstbus messaging Themen.

Bitte teilen Sie Ihre Ideen und ihr Feedback bei [Azure Code Analysis Feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Grund

Partitionierung Servicebuswarteschlangen und Themen erhöht Leistung Durchsatz und Dienst Verfügbarkeit, da der Gesamtdurchsatz einer partitionierten Warteschlange oder das Thema nicht mehr durch die Leistung von einer einzelnen Nachricht Bank oder einem messaging Store begrenzt ist. Darüber hinaus wird nicht eine temporäre Ausfall eines messaging Store eine partitionierte Warteschlange oder das Thema nicht verfügbar machen. Weitere Informationen finden Sie unter [Messaging Einheiten Partitionierung](https://msdn.microsoft.com/library/azure/dn520246.aspx).

### <a name="solution"></a>Lösung

Der folgende Codeausschnitt zeigt, wie messaging Einheiten unterteilen.

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Weitere Informationen finden Sie unter Bus des [aufgeteilt Servicewarteschlangen und Themen | Microsoft Azure-Blog](https://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) und der Stichprobe [Microsoft Azure Service Bus aufgeteilt Warteschlange](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) Auschecken.

## <a name="do-not-set-sharedaccessstarttime"></a>Stellen Sie keine SharedAccessStartTime

### <a name="id"></a>ID

AP3001

### <a name="description"></a>Beschreibung

Vermeiden Sie SharedAccessStartTimeset auf die aktuelle Uhrzeit verwenden, um die freigegebene Zugriffsrichtlinie sofort zu starten. Sie müssen nur diese Eigenschaft festlegen, wenn die freigegebene Zugriffsrichtlinie zu einem späteren Zeitpunkt beginnen soll.

Bitte teilen Sie Ihre Ideen und ihr Feedback bei [Azure Code Analysis Feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Grund

Synchronisierung der Uhr bewirkt, dass eine geringe Differenz zwischen Rechenzentren. Beispielsweise können Sie glauben logisch Festlegen der Startzeit einer Richtlinie SAS-Speicher als die aktuelle Uhrzeit mit DateTime.Now oder eine ähnliche Methode bewirkt, dass die Richtlinie SAS sofort wirksam. Allerdings können Zeiten geringfügigen Unterschiede zwischen Rechenzentren dies Probleme verursachen, da in einigen Fällen Datacenter möglicherweise etwas später als die Startzeit, während andere voraus es. Daher kann die Richtlinie SAS abläuft, schnell (oder sogar unmittelbar), wenn die Richtlinie Gültigkeitsdauer zu kurz festgelegt ist.

Weitere Hilfe bei der freigegebenen Access Signatur auf Azure-Speicher verwenden finden Sie unter [Einführung in die Tabelle SAS (Access-Signatur freigegeben), Warteschlange-SAS- und Blob SAS - Microsoft Azure-Speicher-Teamblog - Website Home - MSDN-Blogs aktualisieren](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Lösung

Entfernen Sie die Anweisung, die Startzeit der freigegebenen Zugriffsrichtlinie festgelegt. Das Tool Code Azure bietet eine Lösung für dieses Problem. Weitere Informationen zu Sicherheitsmanagement finden Sie unter dem Entwurfsmuster [Valet Schlüssel Muster](https://msdn.microsoft.com/library/dn568102.aspx).

Der folgende Codeausschnitt veranschaulicht Codefix zur Behebung dieses Problems.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## <a name="shared-access-policy-expiry-time-must-be-more-than-five-minutes"></a>Freigegebene Zugriffsrichtlinie Ablaufzeit mehr als fünf Minuten werden müssen

### <a name="id"></a>ID

AP3002

### <a name="description"></a>Beschreibung

Es werden können, ähnlich wie der Differenz in einer fünf Minuten in Uhr zwischen Rechenzentren an unterschiedlichen Standorten aufgrund einer Bedingung bekannt als "Uhr Schiefe." Um zu verhindern, dass die SAS festlegen Richtlinie Token ablaufen früher als ursprünglich geplant, die Ablaufzeit mehr als fünf Minuten sein.

Bitte teilen Sie Ihre Ideen und ihr Feedback bei [Azure Code Analysis Feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Grund

Rechenzentren an unterschiedlichen Standorten auf der ganzen Welt synchronisieren, indem Sie ein Signal Zeit. Da Zeit für Uhrsignal an anderen Speicherorten zu folgen benötigt, darf eine Uhrzeit Varianz zwischen Rechenzentren an verschiedenen geografischen Standorten zwar alles eigentlich synchronisiert wird. Dieser Zeitunterschied kann das freigegebene Access Richtlinie Start Zeit und Ablauf Intervall auswirken. Daher, um sicherzustellen, dass freigegebene Zugriffsrichtlinie sofort wirksam wird, geben Sie die Startzeit. Darüber hinaus stellen Sie sicher, dass der Ablauf Mal mehr als 5 Minuten zu frühen Timeout zu verhindern.

Weitere Informationen zur Verwendung von Access-Signatur freigegeben auf Azure-Speicher finden Sie unter [Einführung in die Tabelle SAS (Access-Signatur freigegeben), Warteschlange-SAS- und Blob SAS - Microsoft Azure-Speicher-Teamblog - Website Home - MSDN-Blogs aktualisieren](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx).

### <a name="solution"></a>Lösung

Weitere Informationen zur Verwaltung von Sicherheit finden Sie unter dem Entwurfsmuster [Valet Schlüssel Muster](https://msdn.microsoft.com/library/dn568102.aspx).

So sieht ein Beispiel für eine freigegebene Access-Richtlinie-Anfangszeit nicht anzugeben.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

So sieht ein Beispiel für eine freigegebene Access Richtlinie-Anfangszeit mit einer Richtlinie Ablaufdauer mehr als fünf Minuten anzugeben.

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

Weitere Informationen finden Sie unter [Erstellen und Verwenden einer Access-Signatur freigegeben](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="use-cloudconfigurationmanager"></a>Verwenden von CloudConfigurationManager

### <a name="id"></a>ID

AP4000

### <a name="description"></a>Beschreibung

Verwenden die [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) -Klasse für Projekte, wie z. B. Azure-Website und Azure mobile-Diensten wird nicht Runtime-Probleme vorstellen können. Als bewährte Methode ist es jedoch eine gute Idee, Cloud[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) als eine einheitliche Methode zum Verwalten von Konfigurationen für alle Azure-Cloud-Anwendungen verwenden.

Bitte teilen Sie Ihre Ideen und ihr Feedback bei [Azure Code Analysis Feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Grund

CloudConfigurationManager liest die Konfigurationsdatei der Anwendung Umgebung entsprechend an.

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### <a name="solution"></a>Lösung

Gestalten Sie den Code, um die [CloudConfigurationManager Klasse](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)verwenden. Eine Code-Lösung für dieses Problem wird durch das Azure Codeanalysetool bereitgestellt.

Der folgende Codeausschnitt veranschaulicht Codefix zur Behebung dieses Problems. Ersetzen

`var settings = ConfigurationManager.AppSettings["mySettings"];`

mit

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

Hier ist ein Beispiel für die Einstellung Konfiguration in einer Datei App.config oder Web.config gespeichert. Fügen Sie die Einstellungen zum Abschnitt AppSettings der Konfigurationsdatei ein. Im folgenden finden die Datei Web.config für den im vorhergehenden Beispiel.

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## <a name="avoid-using-hard-coded-connection-strings"></a>Verwenden Sie keine hartcodierte Verbindungszeichenfolgen

### <a name="id"></a>ID

AP4001

### <a name="description"></a>Beschreibung

Wenn die hartcodierte Verbindungszeichenfolgen verwenden, und Sie müssen Sie dann später aktualisieren, müssen Sie nehmen Sie Änderungen an Ihren Quellcode und die Anwendung neu kompilieren. Jedoch, wenn Sie die Verbindungszeichenfolgen in einer Konfigurationsdatei speichern, können Sie diese später ändern, indem Sie einfach die Konfigurationsdatei aktualisieren.

Bitte teilen Sie Ihre Ideen und ihr Feedback bei [Azure Code Analysis Feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Grund

Programmieren Verbindungszeichenfolgen ist eine schlechte Verfahrensweise, da es Probleme werden, wenn Verbindungszeichenfolgen schnell geändert werden müssen. Darüber hinaus, wenn Sie das Projekt an Datenquellen-Steuerelement eingecheckt werden muss, stellen Sie vor hartcodierte Verbindungszeichenfolgen Sicherheitsupdates, da die Zeichenfolgen in der Quellcode angezeigt werden können.

### <a name="solution"></a>Lösung

Verbindungszeichenfolgen in der Konfigurationsdateien oder Azure-Umgebungen zu speichern.

- Verwenden Sie für Applikationen eigenständigen app.config Zeichenfolge Verbindungseinstellungen gespeichert.

- Verwenden Sie für IIS-gehosteten Webanwendungen web.config Verbindungszeichenfolgen gespeichert.

- Verwenden Sie für ASP vNext configuration.json Verbindungszeichenfolgen gespeichert.

Informationen zur Verwendung von Konfigurationsdateien wie web.config oder app.config finden Sie unter [Richtlinien für die Konfiguration von ASP.NET Web](https://msdn.microsoft.com/library/vstudio/ff400235(v=vs.100).aspx). Informationen darüber, wie Azure Umgebung Variablen arbeiten, finden Sie unter [Azure Websites: wie Anwendung Zeichenfolgen und Verbindung Zeichenfolgen arbeiten](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Informationen zum Speichern von Verbindungszeichenfolge im Datenquellen-Steuerelements finden Sie unter [zu vermeiden, vertrauliche Informationen, wie z. B. Verbindungszeichenfolgen in Dateien, die in der Quelle Code Repository gespeichert werden](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control).

## <a name="use-diagnostics-configuration-file"></a>Verwenden Sie die Diagnose Konfigurationsdatei

### <a name="id"></a>ID

AP5000

### <a name="description"></a>Beschreibung

Statt konfigurieren Diagnose in Ihrem Code wie mithilfe der API programming Microsoft.WindowsAzure.Diagnostics, sollten Sie in der Datei diagnostics.wadcfg Diagnose konfigurieren. (Oder diagnostics.wadcfgx, wenn Sie Azure SDK 2,5 verwenden). Auf diese Weise können Sie die Diagnose ändern, ohne Code neu kompilieren.

Bitte teilen Sie Ihre Ideen und ihr Feedback bei [Azure Code Analysis Feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Grund

Bevor Azure SDK 2,5 (der Azure-Diagnose 1.3 verwendet), Azure-Diagnose (WAD) konfiguriert werden konnte mithilfe verschiedene Methoden: in der Konfiguration Blob im Speicher, mithilfe von Code unbedingt erforderlich, deklarativen Konfiguration oder die standardmäßige Konfiguration hinzufügen. Ist jedoch die bevorzugte Methode zum Konfigurieren der Diagnose mit einer XML-Konfigurationsdatei (diagnostics.wadcfg oder diagnositcs.wadcfgx für SDK 2,5 und höher) im Anwendungsprojekt an. Bei dieser Vorgehensweise kann die Datei diagnostics.wadcfg vollständig definiert die Konfiguration werden aktualisiert und erneut bereitgestellt werden. Die Verwendung der Konfigurationsdatei diagnostics.wadcfg mit programmgesteuerten Methoden der Einstellung Konfigurationen mithilfe der [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)oder [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)Klassen mischen kann zu Verwirrung führen. Weitere Informationen finden Sie unter [Initialisierung oder Azure Diagnose-Konfiguration ändern](https://msdn.microsoft.com/library/azure/hh411537.aspx) .

Beginnend mit WAD 1.3 (im Lieferumfang Azure SDK 2,5), ist es nicht mehr möglich, Code verwenden, um die Diagnose konfigurieren. Daher können Sie nur die Konfiguration beim Anwenden oder aktualisieren die Diagnose Erweiterung bereitstellen.

### <a name="solution"></a>Lösung

Verwenden Sie den Diagnose Konfigurations-Designer, um diagnoseeinstellungen zur Konfigurationsdatei Diagnose (diagnositcs.wadcfg oder diagnositcs.wadcfgx für SDK 2,5 und höher) verschieben. Es wird auch empfohlen, dass Sie [Azure SDK 2,5](http://go.microsoft.com/fwlink/?LinkId=513188) installieren und verwenden die neuesten Diagnosefeatures.

1. Klicken Sie im Kontextmenü für die Rolle aus, die Sie konfigurieren möchten, wählen Sie Eigenschaften aus, und wählen Sie dann auf die Registerkarte Konfiguration.

1. Klicken Sie im Abschnitt **Diagnose** stellen Sie sicher, dass das Kontrollkästchen **Diagnose aktivieren** aktiviert ist.

1. Wählen Sie die Schaltfläche **Konfigurieren** .

  ![Zugreifen auf die Option Diagnose aktivieren](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

  Weitere Informationen finden Sie unter [Konfigurieren von Diagnose für Azure-Cloud-Diensten und virtuellen Computern](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) .


## <a name="avoid-declaring-dbcontext-objects-as-static"></a>Vermeiden Sie DbContext Objekte als statisch deklarieren

### <a name="id"></a>ID

AP6000

### <a name="description"></a>Beschreibung

Um Speicherplatz zu sparen, vermeiden Sie DBContext Objekte als statisch deklarieren.

Bitte teilen Sie Ihre Ideen und ihr Feedback bei [Azure Code Analysis Feedback](http://go.microsoft.com/fwlink/?LinkId=403771).

### <a name="reason"></a>Grund

Halten Sie die Abfrageergebnissen aus jeder Anruf DBContext Objekte. Statische DBContext Objekte werden nicht freigegeben, bis die Anwendungsdomäne entfernt wird. Ein statisches DBContext Objekt kann daher viel Speicher beanspruchen.

### <a name="solution"></a>Lösung

Deklarieren Sie DBContext als eine lokale Variable noch nicht statische Instanzenfeld, verwenden sie für einen Vorgang, und lassen Sie ihn nach Verwendung verworfen werden.

Im folgende Beispiel MVC Controller-Klasse veranschaulicht, wie das Objekt DBContext verwenden.

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Optimzing und zur Problembehandlung Azure apps finden Sie unter [Behandeln von einer Web app im App-Verwaltungsdienst Azure mithilfe von Visual Studio](./app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).
