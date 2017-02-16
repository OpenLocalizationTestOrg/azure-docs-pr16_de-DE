<properties
    pageTitle="Azure Ereignis Hubs Archiv | Microsoft Azure"
    description="Übersicht über das Feature Azure Ereignis Hubs archivieren."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="azure-event-hubs-archive"></a>Azure Ereignis Hubs Archiv

Azure Ereignis Hubs Archiv ermöglicht es Ihnen die streaming Daten automatisch in Ihrem Ereignis Hubs mit einem Blob-Speicher-Konto Ihrer Wahl vorführen, mit zusätzlichen Flexibilität, geben Sie einen Zeitraum oder dessen Größe ändern Intervall Ihrer Wahl. Einrichten von Archiv geht schnell, es gibt keine Verwaltungskosten für die Ausführung und skaliert automatisch mit Ihrem Ereignis Hubs [Durchsatz Einheiten](event-hubs-overview.md#capacity-and-security). Ereignis Hubs Archiv ist die einfachste Möglichkeit zum streaming Daten in Azure laden und ermöglicht Ihnen, auf Daten erfassen, anstatt Datenverarbeitung konzentrieren.

Azure Ereignis Hubs Archiv ermöglicht es Ihnen in Echtzeit und Stapel-basierten Pipelines für den gleichen Stream Verarbeitungszeit. So können Sie Lösungen zu erstellen, die mit Ihren Anforderungen Lauf der Zeit wachsen kann. Ob Sie Stapel-basierte Betriebssysteme heute mit Auge in Richtung Verarbeitung von zukünftigen in Echtzeit erstellen oder einen effizienten kalten Pfad in eine vorhandene in Echtzeit-Lösung hinzufügen möchten, ist das Ereignis Hubs Archiv arbeiten mit Daten besser streaming.

## <a name="how-event-hubs-archive-works"></a>Funktionsweise des Ereignisses Hubs Archiv

Ereignis Hubs ist ein Time-Aufbewahrungsrichtlinien dauerhaften Puffer für werden eingehende, ähnlich wie ein Protokoll verteilt. Die EINGABETASTE, um in Ereignis Hubs skalieren ist das [Modell partitionierten Consumer](event-hubs-overview.md#partition-key). Jede Partition ist eine unabhängige Segment Daten und verbraucht unabhängig ist. Im Alter dieser Daten über einen Zeitraum deaktivieren, basierend auf der konfigurierbare Aufbewahrungszeitraum. Daher ruft einen angegebenen Ereignis Hub nie "zu voll."

Ereignis Hubs Archiv können Sie angeben, eigene Azure BLOB-Speicher-Konto und Container, der zum Speichern der archivierten Daten verwendet werden. Dieses Konto kann in derselben Region als Ihre Hub Ereignis oder in einem anderen Region, werden die Flexibilität des Ereignisses Hubs Archiv Features hinzufügen.

Archivierte Daten werden im Format [Apache Avro][] geschrieben; ein komprimieren, schnelle, binäre Format, das Rich-Datenstrukturen mit eingebetteten Schemas enthält. Dieses Format wird stark in der Hadoop-Netz sowie durch Stream Analytics und Azure Data Factory verwendet. Weitere Informationen zum Arbeiten mit Avro steht in diesem Artikel.

### <a name="archive-windowing"></a>Archivieren Windowing

Ereignis Hubs Archiv ermöglicht Ihnen das Einrichten eines Fensters Archivierung steuern. In diesem Fenster ist eine Mindestgröße und die Uhrzeit Konfiguration mit einer "erste Wins Richtlinie," Was bedeutet, dass der erste Trigger festgestellt einen Archivierungsvorgang verursacht. Wenn Sie ein 15-minütiges/100 MB Archiv-Fenster und 1 MB/s senden, wird das Fenster Größe vor dem Zeitfenster auslösen. Jede Partition unabhängig voneinander archiviert und schreibt einem Blob abgeschlossenen Block zum Zeitpunkt der Archivierung, für die Zeit mit der Bezeichnung Wenn das Archiv Intervall aufgetreten ist. Die Benennungskonvention sieht wie folgt aus:

```
<Namespace>/<EventHub>/<Partition>/<YYYY>/<MM>/<DD>/<HH>/<mm>/<ss>
```

### <a name="scaling-to-throughput-units"></a>Skalierung auf Durchsatz Einheiten

Hubs Netzwerkverkehr wird durch [Durchsatz Einheiten](event-hubs-overview.md#capacity-and-security)gesteuert. Eine einzelne Durchsatz Einheit ermöglicht 1 MB/s oder 1000 Ereignisse pro Sekunde für eingehende und zweimal die Größe des Ausgang. Standard-Ereignis Hubs mit 1-20 Durchsatz Einheiten konfiguriert werden können, und weitere kann über eine Kontingent erhöhen [Support anfordern][]erworben werden. Verwendung jenseits erworbenen Durchsatz Einheiten beschränkt. Ereignis Hubs Archiv kopiert Daten direkt aus dem internen Ereignis Hubs-Speicher, Durchsatz Einheit Ausgang Kontingente umgehen, und speichern Ihre Ausgang für andere Leser Verarbeitung wie Stream Analytics oder Spark.

Nachdem so konfiguriert ist, wird Ereignis Hubs Archiv automatisch ausgeführt, sobald Sie das erste Ereignis senden. So lange Ausführung aller Vorkommen. Vereinfachen, für die der untergeordneten Verarbeitung, wenn Sie wissen, dass der Prozess arbeitet, schreibt Ereignis Hubs leere Dateien aus, wenn keine Daten vorhanden sind. Dies bietet eine vorhersehbar Cadence und die Markierung aus, die Ihrer Stapel Prozessoren feed kann.

## <a name="setting-up-event-hubs-archive"></a>Einrichten von Ereignis Hubs Archiv

Ereignis Hubs Archiv kann zum Zeitpunkt der Erstellung Ereignis Hub über das Portal oder Azure Ressourcenmanager konfiguriert werden. Sie aktivieren einfach Archiv, indem Sie auf die Schaltfläche **auf** . Konfigurieren Sie eine Speicher-Konto und Container durch Klicken auf im **Container** Abschnitt des Blades an. Da Ereignis Hubs Archiv mit Speicher-Dienst Authentifizierung verwendet, müssen Sie keine Verbindungszeichenfolge Speicher angeben. Die Ressource Datumsauswahl wählt den Ressourcen-URI für Ihr Speicherkonto auf automatisch. Wenn Sie Ressourcenmanager Azure verwenden, müssen Sie diesen URI als Zeichenfolge explizit angeben.

Das standardmäßige Zeitfenster beträgt fünf Minuten. Der kleinste Wert ist 1, die maximal 15. Das Fenster **Größe** hat einen Bereich von 10-500 MB.

![][1]

## <a name="adding-archive-to-an-existing-event-hub"></a>Hinzufügen eines vorhandenen Ereignis Hub Archiv

Archiven können auf vorhandenen Ereignis Hubs konfiguriert werden, die in einem Namespace Ereignis Hubs sind. Das Feature ist nicht verfügbar für ältere Messaging oder gemischt Typ Namespaces. Archivieren auf einem vorhandenen Ereignis-Hub zu aktivieren oder Ihre Archiv Einstellungen zu ändern, klicken Sie auf Ihren Namespace zum Laden des **Essentials** Blades, klicken Sie dann auf das Ereignis Hub für die Sie aktivieren oder ändern Sie die Einstellung archivieren möchten. Klicken Sie abschließend auf des geöffneten Blades im Bereich **Eigenschaften** , wie in der folgenden Abbildung gezeigt.

![][2]

Sie können auch Ereignis Hubs Archiv über Azure Ressourcenmanager Vorlagen konfigurieren. Weitere Informationen finden Sie [in diesem Artikel](event-hubs-resource-manager-namespace-event-hub-enable-archive.md)aus.

## <a name="exploring-the-archive-and-working-with-avro"></a>Untersuchen das Archiv und Arbeiten mit Avro

Nach der Konfiguration erstellt Ereignis Hubs Archiv Dateien im Speicher Azure-Konto und Container auf die konfigurierten Zeitfensters bereitgestellt. Sie können diese Dateien in einem beliebigen Programm z. B. [Azure-Speicher-Explorer][]anzeigen. Sie können die Dateien lokal zur Arbeit an diesen herunterladen.

Ereignis Hubs Archiv erstellten Dateien stehen das folgende Avro Schema:

![][3]

Untersuchen der Avro Dateien auf einfache Weise wird mithilfe der [Tools Avro][] Jar von Apache. Nach dem Download dieser Jar können Sie das Schema einer bestimmten Avro Datei anzeigen, indem Sie den folgenden Befehl ausführen:

```
java -jar avro-tools-1.8.1.jar getschema \<name of archive file\>
```

Dieser Befehl gibt

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

Sie können auch Avro Tools verwenden, können die Datei in JSON-Format konvertieren und andere Verarbeitung ausführen.

Um zu erweiterte verarbeiten für herunterladen Sie und installieren Sie Avro Ihrer Wahl der Plattform. Zum Zeitpunkt der Erstellung dieses Artikels, es stehen Implementierungen für C, C++, C\#, Java, NodeJS, Perl, PHP, Python und Ruby.

Apache Avro weist abgeschlossen überfordert Führungslinien für [Java][] und [Python][]. Sie können auch den [Erste Schritte mit Ereignis Hubs Archiv](event-hubs-archive-python.md) Artikel lesen.

## <a name="how-event-hubs-archive-is-charged"></a>Wie Ereignis Hubs Archiv belastet wird

Ereignis Hubs Archiv wird als eine stündlich Gebühr auf ähnliche Weise zu Durchsatz Einheiten, getaktete. Die Gebühr ist direkt proportional auf die Anzahl der Durchsatz Einheiten für den Namespace erworben haben. Wie Durchsatz Einheiten erhöht und verringert werden, wird Ereignis Hubs Archiv erhöht und verringert um übereinstimmende Leistung zu ermöglichen. Der Meter erfolgen gemeinsam. Die Gebühr für Ereignis Hubs Archiv ist $0.10 pro Stunde und Durchsatz Einheit, mit einem Rabatt 50 %, während der Preview-Zeitraum angeboten.

Ereignis Hubs Archiv ist wirklich die einfachste Möglichkeit zum Azure Daten zu verschaffen. Azure Daten Lake, Azure Data Factory und Azure HDInsight verwenden, können Sie Stapelverarbeitung und andere Analytics Ihrer Wahl ausführen mithilfe von vertrauten Tools und Plattformen bei einer beliebigen Skalierung, die Sie benötigen.

## <a name="next-steps"></a>Nächste Schritte

Sie erhalten weitere Informationen zu Ereignis Hubs, indem Sie die folgenden Links:

- Eine vollständige [Beispiel-Anwendung, die Ereignis Hubs verwendet][].
- Das Beispiel [Skalierung Ereignis mit Ereignis Hubs verarbeiten][] .
- [Ereignis Hubs (Übersicht)][]

[Apache Avro]: http://avro.apache.org/
[Supportanfrage]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[1]: ./media/event-hubs-archive-overview/event-hubs-archive1.png
[2]: media/event-hubs-archive-overview/event-hubs-archive2.png
[Azure-Speicher-Explorer]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-archive-overview/event-hubs-archive3.png
[Avro-Tools]: http://www-us.apache.org/dist/avro/avro-1.8.1/java/avro-tools-1.8.1.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Ereignis Hubs (Übersicht)]: event-hubs-overview.md
[Beispiel-Anwendung, Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Ereignis mit Ereignis Hubs Verarbeitung skalieren]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3