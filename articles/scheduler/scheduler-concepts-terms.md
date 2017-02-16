<properties
 pageTitle="Scheduler Konzepte und Begriffe Einheiten | Microsoft Azure"
 description="Azure Scheduler Konzepte, Terminologie und Entitätshierarchie, einschließlich Einzelvorgänge und Position Websitesammlungen.  Zeigt ein Beispiel für einen geplanten Auftrag umfassendes an."
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="get-started-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-concepts-terminology--entity-hierarchy"></a>Scheduler Konzepte, Terminologie + Entitätshierarchie

## <a name="scheduler-entity-hierarchy"></a>Scheduler Entitätshierarchie

Die folgende Tabelle beschreibt die wichtigsten Ressourcen verfügbar gemacht oder von der Scheduler-API verwendet:

|Ressource | Beschreibung |
|---|---|
|**Position Websitesammlung**|Eine Auflistung Position enthält eine Gruppe von Aufträgen und unterhält Einstellungen, Kontingente und Drosselungen, die von Aufträgen in der Sammlung freigegeben wurden. Eine Auflistung Auftrag wird durch einen Abonnementbesitzer erstellt und Aufträge zusammen basierend auf Verwendung oder einer Anwendung Begrenzung gruppiert. Es ist eine Region eingeschränkt. Darüber hinaus können die Durchsetzung von Kontingenten, um die Verwendung aller Projekte in dieser Websitesammlung zu beschränken. Die Kontingente gehören MaxJobs und MaxRecurrence.|
|**Position**|Ein Auftrag definiert eine einzelne wiederkehrende Aktion, mit der einfache oder komplexe Strategien für die Ausführung. Aktionen können HTTP, Speicherwarteschlange, Service Bus Warteschlange oder Serviceanfragen Bus-Thema enthalten.|
|**Historie**|Eine Historie stellt die Details für eine Ausführung eines Auftrags dar. Erfolg im Vergleich zu Fehler sowie Details der Antwort enthält.|

## <a name="scheduler-entity-management"></a>Scheduler Entität management

Auf hoher Ebene der Scheduler und die Servicemanagement API verfügbar machen auf die Ressourcen die folgenden Aktionen:

|Funktion|Beschreibung und URI-Adresse|
|---|---|
|**Projektmanagement-Websitesammlung**|SETZEN zu gelangen, und löschen Sie Unterstützung beim Erstellen und Ändern der Position von Websitesammlungen und die darin enthaltenen Einzelvorgänge. Eine Auflistung Auftrag ist Container für Aufträge und Karten auf Kontingente und freigegebenen Einstellungen. Beispiele für Kontingente, die später beschrieben sind die maximale Anzahl von Projekten und kleinsten Serienintervall. <p>Setzen und löschen:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p><p>Erhalten:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p>
|**Projektmanagement**|ERHALTEN möchten, setzen, Posten, PATCH und Unterstützung beim Erstellen und Ändern von Aufträge löschen. Alle Aufträge müssen, damit es keine implizites erstellen ist eine Position-Auflistung, die bereits vorhanden ist, gehören. <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p>|
|**Verlauf Projektmanagement**|Anfordern von Unterstützung für das Abrufen von 60 Tage der Ausführung Historie, z. B. Auftrag verstrichene Zeit und die Ergebnisse der Ausführung Position. Fügt Abfrage Zeichenfolge Parameter Unterstützung für die Filterung basierend auf Zustand und seiner ursprünglichen Status hinzu. <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p>|

## <a name="job-types"></a>Position-Typen

Es gibt mehrere Typen von Aufträgen: HTTP (einschließlich HTTPS-Aufträge, die SSL unterstützen), Speicher Warteschlangenaufträge, Service Bus Warteschlangenaufträge und Service Bus Thema Einzelvorgänge. HTTP-Einzelvorgänge sind ideal, wenn Sie einen Endpunkt einer vorhandenen Arbeitsbelastung oder Dienst haben. Speicher Warteschlangenaufträge können Sie Nachrichten an Speicher Warteschlangen, senden, damit diese Einzelvorgänge ideal für Auslastung werden, in denen Storage Warteschlangen verwendet. Auf ähnliche Weise sind Service Bus Aufträge ideal für Auslastung, die Bus Servicewarteschlangen und Themen verwenden.

## <a name="the-job-entity-in-detail"></a>Die Entität "Projekt" im detail

Grundlegende Ebene weist ein geplanter Auftrag mehrere Webparts:

- Die Aktion ausführen, wenn der Zeitgeber Auftrag ausgelöst wird  

- (Optional) Die Zeit zum Ausführen des Auftrags  

- (Optional) Wann und wie oft wiederholen Sie den Auftrag  

- (Optional) Eine Aktion ausgelöst, wenn die primäre Aktion fehlschlägt  

Intern enthält ein geplanter Auftrag auch System bereitgestellten Daten wie das nächste Mal der geplanten Ausführung.

Mit dem folgende Code enthält ein Beispiel für einen geplanten Auftrag umfassendes. Details werden in den folgenden Abschnitten bereitgestellt.

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often to fire (default to 1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default to recur infinitely)
            "endTime": "2012-11-04",                     // optional (default to recur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

Wie in der Stichprobe geplante Auftrag oben gesehen, verfügt über eine Position Definition mehrere Webparts:

- Startzeit ("Startzeit")  

- Aktion ("Aktion"), wozu auch Fehleraktion ("ErrorAction")

- Serie ("Serie")  

- Bundesstaat ("State")  

- Status ("Status")  

- Wiederholen Sie die Richtlinie ("RetryPolicy")  

Betrachten Sie jede dieser im Detail aus:

## <a name="starttime"></a>Startzeit

Der "Startzeit" wird die Startzeit und ermöglicht den Anrufer einen Zeitzonenoffset bei der Übertragung im [ISO-8601-Format](http://en.wikipedia.org/wiki/ISO_8601)angeben.

## <a name="action-and-erroraction"></a>Aktion und errorAction

"Aktion" ist mit der Aktion für jede Wiederholung aufgerufen und einen Typ der Dienst zwischenzuspeichern beschreibt. Die Aktion ist, was auf den Terminplan bereitgestellten ausgeführt wird. Scheduler unterstützt HTTP, Speicherwarteschlange, Service Bus Thema und Service Bus Warteschlange-Aktionen.

Die Aktion im obigen Beispiel handelt es sich um eine Aktion HTTP. Nachstehend ist ein Beispiel für eine Speicher Warteschlange Aktion aus:

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

Es folgt ein Beispiel für eine Service Bus Thema Aktion.

  "Aktion": {"Typ": "ServiceBusTopic", "ServiceBusTopicMessage": {"TopicPath": "t1",  
      "Namespace": "MySBNamespace", "TransportType": "NetMessaging," / / NetMessaging oder AMQP "Authentifizierung" sein: {"SasKeyName": "QPolicy", "Typ": "SharedAccessKey"}, "Nachricht": "Einige Nachricht", "BrokeredMessageProperties": {}, "CustomMessageProperties": {"Appname": "FromScheduler"}},}

Es folgt ein Beispiel für eine Service Bus Warteschlange Aktion:


  "Aktion": {"ServiceBusQueueMessage": {"QueueName": "q1",  
      "Namespace": "MySBNamespace", "TransportType": "NetMessaging," / / NetMessaging oder AMQP "Authentifizierung" sein: {}  
        "SasKeyName": "QPolicy", "Typ": "SharedAccessKey"}, "Nachricht": "Einige Nachricht"  
      "BrokeredMessageProperties": {}, "CustomMessageProperties": {"Appname": "FromScheduler"}}, "Typ": "ServiceBusQueue"}

Die "ErrorAction" ist der Ereignishandler zurück, die Aktion aufgerufen, wenn die primäre Aktion fehlschlägt. Verwenden Sie diese Variable, rufen Sie einen Endpunkt Fehlerbehandlung oder Senden einer Benachrichtigung für Benutzer. Dies kann verwendet werden, zum Erreichen eines sekundären Endpunkts in der Groß-/Kleinschreibung der primären ist nicht verfügbar (z. B. bei einem Ausfall des Endpunkts Website) oder für eine Fehlerbehandlung Endpunkt benachrichtigen verwendet werden kann. Wie die primäre Aktion kann die Fehleraktion einfachen oder zusammengesetzten Logik basierend auf andere Aktionen sein. So erstellen Sie ein SAS Token finden Sie unter [Erstellen](https://msdn.microsoft.com/library/azure/jj721951.aspx)und Verwenden einer Access-Signatur freigegeben.

## <a name="recurrence"></a>Serie

Serie besteht aus mehreren Teilen:

- Häufigkeit: Eine der Minute, Stunde, Tag, Woche, Monat, Jahr  

- Intervall: Intervall mit der angegebenen Häufigkeit für die Häufigkeit  

- Vorgegebenen Zeitplan: Geben Sie Stunden, Minuten, Arbeitstagen, Monate und Monthdays der Serie  

- Zählen: Anzahl Vorkommen  

- Endzeit: keine Aufträge werden ausgeführt, nach der angegebenen Endzeit  

Ein Auftrag wiederholt sich, sobald sie ein wiederkehrendes Objekt in ihrer JSON-Definition angegeben hat. Wenn sowohl "Anzahl" und "Endzeit angegeben sind, wird die Fertigstellung-Regel, die zuerst eintritt berücksichtigt.

## <a name="state"></a>Bundesstaat

Der Zustand des Projekts ist eine der vier Werte: aktiviert, deaktiviert, abgeschlossen oder fehlerhaft. Sie können setzen oder PATCH Einzelvorgänge Weitere aktualisiert in den Zustand aktiviert oder deaktiviert. Wenn ein Projekt abgeschlossen oder fehlerhaft, d. h. einen endgültigen Status, die aktualisiert werden kann (auch wenn Sie der Auftrag noch gelöscht werden kann) wurde. Ein Beispiel für die State-Eigenschaft lautet wie folgt aus:


        "state": "disabled", // enabled, disabled, completed, or faulted
Abgeschlossene und fehlerhafte Aufträge werden nach 60 Tagen gelöscht.

## <a name="status"></a>Status

Nachdem Sie ein Projekt Scheduler gestartet hat, werden Informationen über den aktuellen Status des Projekts zurückgegeben. Dieses Objekt ist nicht vom Benutzer festgelegt werden kann – es durch das System festgelegt ist. Es ist jedoch in den Job-Objekts (statt in einer separaten verknüpfte Ressource) enthalten, damit eine den Status eines Auftrags einfach abrufen kann.

Projektstatus enthält die Uhrzeit der vorherigen Ausführung (falls vorhanden), die Anzeigedauer der nächsten geplanten Ausführung (für in Bearbeitung Aufträge) und die Ausführungsanzahl eines Auftrags.

## <a name="retrypolicy"></a>retryPolicy

Wenn ein Scheduler Auftrag fehlschlägt, ist es möglich, geben Sie eine Richtlinie "Wiederholen", um zu bestimmen, ob und wie die Aktion wiederholt wird. Dies wird durch das Objekt **RetryType** bestimmt – wird festgelegt, **keine** Wenn keine "Wiederholen" Richtlinie, wie oben gezeigt. Legen Sie es auf **feste** , es ist eine Richtlinie "Wiederholen".

Wenn Sie eine Richtlinie "Wiederholen" festlegen, können zwei zusätzliche Einstellungen angegeben werden: eine Wiederholungsintervalls (**RetryInterval**) und die Anzahl der Wiederholungsversuche (**RetryCount**).

Des Wiederholungsintervalls, mit dem Objekt **RetryInterval** angegeben ist der Zeitraum zwischen Wiederholungsversuche. Der Standardwert ist 30 Sekunden, deren konfigurierbare Minimalwert ist 15 Sekunden und deren Höchstwert ist 18 Monate. Aufträge in kostenlosen Auftrag Websitesammlungen haben eine konfigurierbare Minimalwert von 1 Stunde.  Es wird im Format ISO 8601 definiert. Auf ähnliche Weise wird der Wert, der die Anzahl der Wiederholungsversuche mit dem Objekt **RetryCount** angegeben. Es ist die Anzahl der Häufigkeit, mit die eine Wiederholung des Vorgangs. Der Standardwert ist 4 und deren Maximalwert 20\. beide **RetryInterval** und **RetryCount** sind optional. Werden können, deren Standard erhalten, wenn **RetryType** **feste** festgelegt ist und keine Werte explizit angegeben sind.

## <a name="see-also"></a>Siehe auch

 [Was ist der Scheduler?](scheduler-intro.md)

 [Erste Schritte mit Scheduler Azure-Portal](scheduler-get-started-portal.md)

 [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)

 [So erstellen Sie komplexe Zeitpläne und erweiterte Serie mit Azure Scheduler](scheduler-advanced-complexity.md)

 [Azure Scheduler REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Bezug auf Azure Scheduler PowerShell-cmdlets](scheduler-powershell-reference.md)

 [Azure Scheduler hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Grenzwerte für Azure Scheduler, Standardeinstellungen und Fehlercodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler ausgehende Authentifizierung](scheduler-outbound-authentication.md)
