<properties 
    pageTitle="Was ist der Azure WebJobs SDK" 
    description="Eine Einführung in die Azure WebJobs SDK. Erläutert, was das SDK ist, folgende gängige Szenarien erkannt eignet sich für und Fehlercode Beispiele." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="what-is-the-azure-webjobs-sdk"></a>Was ist der Azure WebJobs SDK

## <a name="a-idoverviewaoverview"></a><a id="overview"></a>(Übersicht)

In diesem Artikel wird erläutert, was das WebJobs SDK ist, prüft anhand einiger allgemeinen Szenarien, die erkannt eignet sich für und bietet einen Überblick darüber, wie Sie es im Code verwenden.

[WebJobs](websites-webjobs-resources.md) ist ein Feature von Azure-App-Dienst, der Sie ein Programm oder Skript in demselben Kontext wie ein Web-app, API app oder mobile-app ausführen können. Der Zweck des [WebJobs SDK](websites-webjobs-resources.md) ist der Code, den Sie für häufige Aufgaben schreiben, dass eine WebJob ausführen, wie z. B. Image-Verarbeitung, Warteschlange Verarbeitung können, RSS-Aggregation, Wartung und Senden von e-Mails zu vereinfachen. Das WebJobs SDK weist integrierte Features für die Arbeit mit Azure-Speicher und Dienstbus, für die Planung von Vorgängen und Behandeln von Fehlern und viele andere allgemeine Szenarien. Darüber hinaus hat es erweiterbar sein soll. [WebJobs SDK ist Quelle öffnen](https://github.com/Azure/azure-webjobs-sdk/), und es geht [Quellrepository für Erweiterungen zu öffnen](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

Das WebJobs SDK umfasst die folgenden Komponenten:

* **NuGet-Pakete**. NuGet-Paketen, die Sie eines Projekts Visual Studio Console-Anwendung hinzufügen bieten einen Rahmen, den von Code verwendet, indem Sie Ihre Methoden mit WebJobs SDK Attributen ergänzen.
  
* **Dashboard**. Teil des WebJobs SDK befindet sich auf App-Verwaltungsdienst Azure und stellt Rich-Cmdlets für die Überwachung und Diagnose für Programme, die die NuGet-Pakete verwenden. Sie müssen nicht zum Schreiben von Code zum Verwenden dieser Funktionen für die Überwachung und Diagnose.

## <a name="a-idscenariosascenarios"></a><a id="scenarios"></a>Szenarien

Hier sind einige typischen Szenarien, die mit dem Azure WebJobs SDK leichter verarbeitet werden können:

* Abbildung des Verarbeitung oder eine andere CPU ankommt Arbeit aus. Ein häufig verwendetes Feature von Websites ist die Möglichkeit zum Hochladen von Bildern oder Videos. Häufig möchten Sie den Inhalt bearbeiten, nachdem er hochgeladen wird, aber nicht soll den Benutzer, warten Sie, während Sie erledigen.

* Warteschlange Verarbeitung. Ein gängiges Verfahren für eine Web-Front-End zur Kommunikation mit einem Back-End-Dienst besteht darin, Warteschlangen verwenden. Wenn die Website erledigen Arbeit muss, legt es zu eine Nachricht für eine Warteschlange. Ein Back-End-Dienst Meldung in der Warteschlange und führt die Arbeit. Sie könnten Warteschlangen für die Verarbeitung von Bild verwenden: z. B. nachdem der Benutzer eine Reihe von Dateien hochgeladen, setzen Sie die Dateinamen in einer Nachricht Warteschlange, indem Sie die Back-End für die Verarbeitung von entnommen werden. Oder könnten Sie Warteschlangen zur Verbesserung der Reaktionszeiten der Website verwenden. Beispielsweise von direkt in einer SQL­Datenbank, Schreiben für eine Warteschlange, weisen Sie den Benutzer, die, den Sie fertig sind, den und lassen Sie die Back-End-Dienst Ziehpunkt Wartezeiten relationalen Datenbank arbeiten. Ein Beispiel für die Verarbeitung mit Bild Prozess Warteschlange finden Sie im [Lernprogramm WebJobs SDK erste Schritte](websites-dotnet-webjobs-sdk-get-started.md).

* RSS-Aggregation. Wenn Sie eine Website, die eine Liste von RSS-Feeds verwaltet haben, könnten Sie aller Artikel über die Feeds in einem Hintergrundprozess ziehen.

* Datei Wartungsaufgaben wie das Aggregieren oder Protokolldateien bereinigen.  Möglicherweise müssen Sie die Protokolldateien erstellt wird, indem Sie mehrere Websites oder separaten Zeitspannen welche kombiniert, um die Analyse Aufträge diesen ausgeführt werden sollen. Oder möglicherweise möchten Sie eine Aufgabe, um die alte Protokolldateien zu bereinigen wöchentliche zu planen.

* Eingehende in Azure Tabellen. Sie möglicherweise Dateien gespeichert und Blobs haben und analysieren sie und die Daten in Tabellen speichern möchten. Die Funktion eingehende konnte schreiben, wenn Sie viele Zeilen (in einigen Fällen Millionen), und das WebJobs SDK ermöglicht es, diese Funktionalität einfach zu implementieren. Das SDK enthält auch von Statusindikatoren wie die Anzahl der Zeilen in der Tabelle geschrieben in Echtzeit überwacht.

* Andere langer-Aufgaben, die Sie in einem Hintergrund-Thread wie das [Senden von e-Mails](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure)ausführen möchten. 

* Alle Aufgaben, die in einem Zeitplan, z. B.: Ausführen eines Vorgangs sichern jede Nacht ausgeführt werden sollen.

In vielen Szenarien möchten Sie möglicherweise skalieren eine Web app auszuführenden auf mehreren virtuellen Computern, die mehrere WebJobs gleichzeitig ausführen möchten. In einigen Szenarien könnte dies dieselben Daten mehrmals verarbeitet, aber dies ist kein Problem, wenn Sie die integrierten Warteschlange, Blob und Dienstbus Trigger des WebJobs SDK verwenden. Das SDK wird sichergestellt, dass Ihre Funktionen nur einmal für jede Nachricht oder Blob verarbeitet werden.

Das WebJobs SDK erleichtert auch die allgemeine Fehlerbehandlung Szenarien zu behandeln. Sie können Benachrichtigungen einrichten, um Benachrichtigungen senden, wenn eine Funktion schlägt fehl, und Sie Zeitlimit festlegen können, dass eine Funktion automatisch abgebrochen wird, wenn er nicht innerhalb eines bestimmten Zeitraums ausgeführt werden.

## <a name="a-idcodea-code-samples"></a><a id="code"></a>Codebeispielen

Der Code für den Umgang mit häufiger Aufgaben, die Arbeit mit Azure-Speicher ist einfach. In der Console-Anwendung `Main` Methode, die Sie beim Erstellen einer `JobHost` Objekt, das die Anrufe an Methoden, die Sie schreiben koordiniert. WebJobs SDK Rahmen weiß, wann Sie Ihre Methoden aufrufen und was Parameterwerte mit den Attributen WebJobs SDK basierend auf, die Sie in diese verwenden. Das SDK enthält *Trigger* , die angeben, wie die Funktion aufgerufen werden Umständen auftreten und *Bindemittel* , die so erhalten Sie Informationen in die und aus Methodenparameter angeben.

Das Attribut [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) wird beispielsweise eine Funktion, wenn eine Nachricht in einer Warteschlange empfangen, und wenn das Nachrichtenformat JSON für ein Byte-Array oder einen benutzerdefinierten Typ ist, die Nachricht automatisch deserialisierte ist aufgerufen werden. Das Attribut [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) löst ein Prozesses, sobald ein neuer Blob in einem Speicher Azure-Konto erstellt wird.

Hier ist ein einfaches Programm, das eine Warteschlange abfragt und einen Blob für jede Nachricht Warteschlange erstellt:

        public static void Main()
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)
        {
            writer.WriteLine(inputText);
        }

Die `JobHost` Objekt ist ein Container für eine Reihe von Hintergrundfunktionen. Die `JobHost` Objekt überwacht die Funktionen, Überwachungen für Ereignisse, die diese auslösen und führt die Funktionen, auftreten von Ereignissen auslösen. Rufen Sie eine `JobHost` Methode, um anzugeben, ob den Prozess Container für den aktuellen Thread oder einen Hintergrund-Thread ausgeführt werden soll. Im Beispiel der `RunAndBlock` Methode führt den Prozess kontinuierlich für den aktuellen Thread.

Da der `ProcessQueueMessage` Methode in diesem Beispiel verfügt über eine `QueueTrigger` Attribut, den Trigger für die Funktion der Empfang einer neuen Nachricht der Warteschlange ist. Die `JobHost` Objekt Überwachungen Nachrichteneingang Warteschlange auf die angegebene Warteschlange (in diesem Beispiel "Webjobsqueue"), und wenn eine gefunden wird, ruft `ProcessQueueMessage`. 

Die `QueueTrigger` Attribut bindet das `inputText` Parameter an den Wert der Warteschlange Nachricht. Und die `Blob` Attribut bindet eine `TextWriter` Objekt in ein Blob mit dem Namen "Blobname" in einem Container mit dem Namen "Containername".  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

Die Funktion verwendet diese Parameter, um den Wert der Warteschlange Nachricht in den Blob schreiben:

        writer.WriteLine(inputText);

Die Trigger und Binder Features des WebJobs SDK vereinfachen erheblich des Codes, die Sie schreiben müssen. Der Low-Level Code erforderlich Verarbeitungszeit Warteschlangen, Blobs oder Dateien oder geplante Vorgängen einleiten wird vom Framework WebJobs SDK für Sie erledigt. Beispielsweise das Framework erstellt Warteschlangen noch, wird nicht der Warteschlange, liest die Warteschlange Nachrichten, löscht Warteschlange Nachrichten aus, wenn Verarbeitung abgeschlossen ist, erstellt Blob-Container, die nicht vorhanden ist noch, in Blobs usw. schreibt.

Im folgenden Codebeispiel wird eine Vielzahl von Triggern in einem WebJob: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, und `ErrorTrigger`. 

```
    public class Functions
    {
        public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        TextWriter log)
        {
            log.WriteLine(message);
        }

        public static void ProcessFileAndUploadToBlob(
            [FileTrigger(@"import\{name}", "*.*", autoDelete: true)] Stream file,
            [Blob(@"processed/{name}", FileAccess.Write)] Stream output,
            string name,
            TextWriter log)
        {
            output = file;
            file.Close();
            log.WriteLine(string.Format("Processed input file '{0}'!", name));
        }

        [Singleton]
        public static void ProcessWebHookA([WebHookTrigger] string body, TextWriter log)
        {
            log.WriteLine(string.Format("WebHookA invoked! Body: {0}", body));
        }

        public static void ProcessGitHubWebHook([WebHookTrigger] string body, TextWriter log)
        {
            dynamic issueEvent = JObject.Parse(body);
            log.WriteLine(string.Format("GitHub WebHook invoked! ('{0}', '{1}')",
                issueEvent.issue.title, issueEvent.action));
        }

        public static void ErrorMonitor(
        [ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
        [SendGrid(
            To = "admin@emailaddress.com",
            Subject = "Error!")]
         SendGridMessage message)
        {
            // log last 5 detailed errors to the Dashboard
            log.WriteLine(filter.GetDetailedMessage(5));
            message.Text = filter.GetDetailedMessage(1);
        }
    }
```

## <a name="a-idschedulea-scheduling"></a><a id="schedule"></a>Planung

Die `TimerTrigger` Attribut gibt Ihnen die Möglichkeit zum Auslösen Funktionen auf einen Zeitplan ausgeführt werden. Sie können eine WebJob als Ganzes durch Azure oder Terminplan einzelne Funktionen für eine WebJob mit dem WebJobs SDK planen `TimerTrigger`. Hier ist ein Codebeispiel ein.

```
public class Functions
{
    public static void ProcessTimer([TimerTrigger("*/15 * * * * *", RunOnStartup = true)]
    TimerInfo info, [Queue("queue")] out string message)
    {
        message = info.FormatNextOccurrences(1);
    }
}
```

Weitere Beispiel-Code finden Sie unter [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) im Repository Azure-Webjobs-Sdk-Erweiterungen auf GitHub.com.

## <a name="extensibility"></a>Erweiterbarkeit

Das WebJobs SDK, die Sie nicht auf integrierte Funktionen – beschränkt sind können Sie benutzerdefinierte Trigger und Ringbücher schreiben.  Beispielsweise können Sie Trigger für Cacheereignisse und periodisch Terminpläne geschrieben werden. [Öffnen der Quellrepository](https://github.com/Azure/azure-webjobs-sdk-extensions) enthält einen [ausführlicher Leitfaden zur WebJobs SDK Erweiterbarkeit](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) und Beispiel-Code helfen Ihnen bei den ersten Schreiben eigener Trigger und Ringbücher.

## <a name="a-idworkerroleausing-the-webjobs-sdk-outside-of-webjobs"></a><a id="workerrole"></a>Mithilfe des WebJobs SDK außerhalb WebJobs

Ein Programm, verwendet die WebJobs SDK ist eine standardmäßige Console-Anwendung und überall ausführen – es nicht als eine WebJob ausführen muss. Sie können das Programm auf Ihrem Entwicklungscomputer lokal testen, und Herstellung können Sie ihn ausführen in einen Cloud-Dienst Worker-Rolle oder einen Windows-Dienst auf Wunsch eine der diese Umgebungen. 

Das Dashboard ist jedoch nur als Erweiterung für eine App-Verwaltungsdienst Azure Web app zur Verfügung. Wenn Sie außerhalb eines WebJob ausgeführt und das Dashboard weiterhin verwenden möchten, können Sie konfigurieren eine Web-app, um das gleiche Speicherkonto zu verwenden, die Ihre WebJobs SDK Dashboard Verbindungszeichenfolge bezieht sich auf und die Web-app WebJobs Dashboard wird dann Daten zu Funktion Execution aus dem Programm, das an anderer Stelle ausgeführt wird angezeigt. Sie können dem Dashboard mithilfe der URL https://*{Webappname}*.scm.azurewebsites.net/azurejobs/#/functions erhalten. Weitere Informationen finden Sie unter [erste eines Dashboards für lokale Entwicklung mit dem SDK WebJobs](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), aber beachten Sie, dass im Blogbeitrag einen alten Namen für die Verbindungszeichenfolge zeigt. 

## <a name="a-idnostorageadashboard-features"></a><a id="nostorage"></a>Dashboard-Funktionen

Das WebJobs SDK bietet mehrere Vorteile, auch wenn Sie WebJobs SDK Trigger oder Ringbücher verwenden nicht:

* Sie können Funktionen aus dem Dashboard aufrufen.
* Sie können Funktionen aus dem Dashboard wiedergeben.
* Sie können die Protokolle im Dashboard verknüpft die bestimmten WebJob (Anwendungsprotokolle, geschrieben mithilfe von Console.Out, Console.Error, Spur usw.) oder bestimmte Funktion aufrufen, die sie generiert verknüpfte anzeigen (Protokolle geschrieben mithilfe einer `TextWriter` Objekt, das das SDK für die Funktion als Parameter übergibt). 

Weitere Informationen finden Sie unter [manuell eine Funktion aufrufen](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) und [zum Schreiben von Protokollen](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs) 

## <a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte

Weitere Informationen zu den WebJobs SDK finden Sie unter [Azure WebJobs empfohlen Ressourcen](http://go.microsoft.com/fwlink/?linkid=390226).

Informationen zu den neuesten Erweiterungen zu WebJobs SDK finden Sie unter den [Versionsinformationen](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes).
 
