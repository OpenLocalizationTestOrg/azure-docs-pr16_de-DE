<properties 
    pageTitle="Verwenden von Azure Service Bus mit der WebJobs SDK" 
    description="Erfahren Sie, wie Sie mit dem WebJobs SDK Azure Servicebuswarteschlangen und Themen verwenden." 
    services="app-service\web, service-bus" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a>Verwenden von Azure Service Bus mit der WebJobs SDK

## <a name="overview"></a>(Übersicht)

Dieses Handbuch bietet C#-Codebeispielen, die zeigen, wie ein Prozesses ausgelöst, wenn eine Nachricht Azure-Dienstbus eingeht. Verwenden Sie die Codebeispielen [WebJobs SDK](websites-dotnet-webjobs-sdk.md) -Version 1.x.

Das Handbuch wird davon ausgegangen, dass Sie wissen, [wie Sie ein Projekt WebJob in Visual Studio mit Verbindungszeichenfolgen erstellen, die mit Ihrem Speicherkonto zeigen](websites-dotnet-webjobs-sdk-get-started.md).

Die Codeausschnitte anzeigen nur Funktionen, die nicht den Code, erstellt der `JobHost` Objekt wie im folgenden Beispiel:

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

Ein [Vollständiges Dienstbus Codebeispiel](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) ist im Repository Azure-Webjobs-Sdk-Beispiele auf GitHub.com.

## <a name="a-idprerequisitesa-prerequisites"></a><a id="prerequisites"></a>Erforderliche Komponenten

Zum Arbeiten mit Dienstbus müssen Sie das [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet-Paket zusätzlich zu den anderen WebJobs SDK-Paketen zu installieren. 

Sie können auch die Verbindungszeichenfolge AzureWebJobsServiceBus sowie die Speicher Verbindungszeichenfolgen festgelegt.  Sie können dies tun, der `connectionStrings` Abschnitt der App, wie im folgenden Beispiel gezeigt:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

Ein Beispiel für Projekt, die Einstellung für die Dienstbus Verbindungszeichenfolge in der App enthält, finden Sie unter [Dienstbus Beispiel](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

Die Verbindungszeichenfolgen können auch in der Umgebung Azure Laufzeit festgelegt werden, die dann die App.config Einstellungen überschrieben wird, wenn die WebJob in Azure ausgeführt wird; Weitere Informationen finden Sie unter [Erste Schritte mit dem SDK WebJobs](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).

## <a name="a-idtriggera-how-to-trigger-a-function-when-a-service-bus-queue-message-is-received"></a><a id="trigger"></a>Wie eine Funktion ausgelöst, wenn eine Dienstbus Warteschlange-Nachricht empfangen wird

Um eine Funktion zu schreiben, die das WebJobs SDK Anrufe, wenn eine Nachricht Warteschlange eingeht, verwenden Sie die `ServiceBusTrigger` Attribut. Der Attributkonstruktor hat einen Parameter, der den Namen der Warteschlange abgefragt werden soll.

### <a name="how-servicebustrigger-works"></a>Funktionsweise von ServiceBusTrigger

Das SDK erhält eine Nachricht in `PeekLock` Modus und Anrufe `Complete` auf die Nachricht, wenn die Funktion erfolgreich abgeschlossen wird, oder Anrufe `Abandon` Wenn die Funktion fehlschlägt. Wenn die Funktion mehr als ausgeführt wird die `PeekLock` Timeout, die Sperre wird automatisch erneuert.

Dienstbus unterstützt die eigene Behandlung durch beschädigte Warteschlange die nicht gesteuert oder vom SDK WebJobs konfiguriert. 

### <a name="string-queue-message"></a>Zeichenfolge Warteschlange Nachricht

Im folgenden Beispiel liest eine Warteschlange-Nachricht, die eine Zeichenfolge enthält, und die Zeichenfolge in dem Dashboard WebJobs SDK geschrieben.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Hinweis:** Wenn Sie Nachrichten in der Warteschlange in einer Anwendung erstellen, in denen das WebJobs SDK verwendet wird nicht, stellen Sie sicher, um [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) auf "Text/nur" festzulegen.

### <a name="poco-queue-message"></a>POCO Warteschlange Nachricht

Das SDK wird automatisch eine Warteschlange-Nachricht, die JSON für eine POCO enthält Deserialisieren [(schlicht alten CLR-Objekt](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) Typ. Im folgenden Beispiel liest eine Warteschlange Nachricht mit einer `BlobInformation` Objekt mit einer `BlobName` Eigenschaft:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Codebeispielen gezeigt wird, wie die POCO Eigenschaften für die Arbeit mit Blobs und Tabellen in die gleiche Funktion verwenden finden Sie unter der [Speicher Warteschlangen Version dieses Artikels](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).

Wenn der Code, der die Nachricht Warteschlange erstellt die WebJobs SDK nicht verwenden möchten, verwenden Sie ähnlichen Code wie im folgenden Beispiel:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Typen von ServiceBusTrigger funktioniert mit

Außer `string` und POCO-Typen, verwenden Sie die `ServiceBusTrigger` Attribut mit einem Array von Bytes oder eine `BrokeredMessage` Objekt.

## <a name="a-idcreatea-how-to-create-service-bus-queue-messages"></a><a id="create"></a>So erstellen Sie Dienstbus Warteschlangennachrichten

Eine Funktion zu schreiben, verwenden Sie eine neue Warteschlange Nachricht erstellt, die `ServiceBus` Attribut aus, und klicken Sie in den Namen der Warteschlange an den Attributkonstruktor übergeben. 


### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Erstellen Sie eine einzelne Warteschlange Nachricht in einer nicht asynchrone-Funktion

Im folgenden Beispiel wird einen Output-Parameter zum Erstellen einer neuen Nachricht in der Warteschlange mit dem Namen "Outputqueue" mit denselben Inhalt wie die Nachricht in der Warteschlange mit dem Namen "Inputqueue" verwendet.

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

Der Ausgabeparameter für das Erstellen einer einzelnen Warteschlange Nachricht kann die folgenden Arten darstellen:

* `string`
* `byte[]`
* `BrokeredMessage`
* Ein serialisierbarer POCO-Typ, den Sie definieren. Automatisch serialisiert als JSON.

Für POCO Typparameter wird eine Meldung Warteschlange immer erstellt, nach Beendigung der Funktion; Wenn der Parameter null ist, wird das SDK eine Warteschlange-Nachricht, die null zurückgibt, wenn die Nachricht empfangen und deserialisiert wird erstellt. Wenn der Parameter null ist, wird keine Warteschlange-Meldung für die anderen Arten erstellt.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Erstellen mehrerer Warteschlangennachrichten oder asynchronen Funktionen

Um mehrere Nachrichten zu erstellen, verwenden Sie die `ServiceBus` Attribut mit `ICollector<T>` oder `IAsyncCollector<T>`, wie im folgenden Beispiel gezeigt:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Jede Nachricht Warteschlange sofort erstellt wird bei der `Add` wird aufgerufen.

## <a name="a-idtopicsahow-to-work-with-service-bus-topics"></a><a id="topics"></a>Zum Arbeiten mit Dienstbus Themen

Um eine Funktion zu schreiben, die das SDK Anrufe, wenn eine Meldung zu einem Thema Dienstbus eingeht, verwenden Sie die `ServiceBusTrigger` Attribut mit Konstruktors, dem Namen des Themas und Abonnementname, wie im folgenden Beispiel gezeigt:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

Verwenden Sie zum Erstellen einer Nachricht zu einem bestimmten Thema der `ServiceBus` Attribut mit einem Thema Namen die gleiche Weise, wie Sie mit einem Warteschlangennamen verwenden.

## <a name="features-added-in-release-11"></a>In Version 1.1 hinzugefügten Features

Die folgenden Funktionen wurden in Version 1.1 hinzugefügt:

* Ermöglichen die Tiefen Anpassung der Nachricht Verarbeitung mittels `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`unterstützt die Anpassung der Service- `MessagingFactory` und `NamespaceManager`.
* A `MessageProcessor` Strategiemuster ermöglicht es Ihnen, einen Prozessor pro Warteschlange/Thema angeben.
* Nachricht Verarbeitung Parallelität wird standardmäßig unterstützt. 
* Einfache Anpassung der `OnMessageOptions` über `ServiceBusConfiguration.MessageOptions`.
* Zulassen von [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) auf angegeben werden `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (für Szenarios, in denen Sie möglicherweise nicht die Verwaltung von Informationsrechten verwalten). 

## <a name="a-idqueuesarelated-topics-covered-by-the-storage-queues-how-to-article"></a><a id="queues"></a>Verwandte Themen im Speicher Warteschlangen unterstützenden Artikel fallende

Informationen zu WebJobs SDK Szenarien Dienstbus nicht spezifisch finden Sie unter [Azure Warteschlangenspeicher mit dem WebJobs SDK verwenden](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Die folgenden: Themen in diesem Artikel behandelt

* Asynchrone Funktionen
* Mehrere Instanzen
* Sicheres war(en)
* Verwenden Sie WebJobs SDK Attribute in den Textkörper einer Funktion
* Legen Sie die SDK Verbindungszeichenfolgen im code
* Festlegen der Werte für WebJobs SDK Parameter in code
* Eine Funktion manuell auslösen
* Schreiben von Protokollen

## <a name="a-idnextstepsa-next-steps"></a><a id="nextsteps"></a>Nächste Schritte

In diesem Handbuch weist Codebeispielen bereitgestellt, die veranschaulichen, allgemeine Szenarien für das Arbeiten mit Azure Service Bus zu behandeln. Weitere Informationen zum Verwenden von Azure WebJobs und das WebJobs SDK finden Sie unter [Azure WebJobs empfohlen Ressourcen](http://go.microsoft.com/fwlink/?linkid=390226).
 
