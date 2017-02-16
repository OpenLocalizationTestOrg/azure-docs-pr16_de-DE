<properties
    pageTitle="Skalieren Stream Analytics Aufträge Datendurchsatz | Microsoft Azure"
    description="Informationen Sie zum Stream Analytics Aufträge skalieren Eingabewerte Partitionen konfigurieren und optimieren die Abfragedefinition Auftrag streaming Einheiten festlegen."
    keywords="streaming, Daten Abstimmung Analytics streaming Datenverarbeitung"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="scale-azure-stream-analytics-jobs-to-increase-stream-data-processing-throughput"></a>Skalieren Sie Azure Stream Analytics Aufträge zum Stream Datenverarbeitung Durchsatz erhöhen

Informationen zum Optimieren von Analytics Aufträge und berechnen *streaming Einheiten* für Stream Analytics, wie Stream Analytics Aufträge zu skalieren, indem Sie Konfigurieren von Partitionen, optimieren die Abfragedefinition Analytics und Position streaming Einheiten festlegen. 

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Was sind die Teile eines Auftrags Stream Analytics?
Stream Analytics Auftragsdefinition enthält Eingaben, eine Abfrage, und die Ausgabe an. Eingaben sind aus, in dem die Position den Datenstream liest, die Abfrage wird verwendet, um die Eingabewerte Dateneingabestream transformieren und die Ausgabe ist, in dem der Auftrag Auftragsergebnisse zu sendet.  

Ein Auftrag erfordert mindestens eine Datenquelle für das Datenstreaming von. Die Eingabewerte Stream Datenquelle kann in einem Azure Service Bus Ereignis-Hub oder Azure Blob Storage gespeichert werden. Weitere Informationen finden Sie unter [Einführung in Azure Stream Analytics](stream-analytics-introduction.md) und [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md).

## <a name="configuring-streaming-units"></a>Konfigurieren von streaming Einheiten
Streaming Einheiten (SUs) stehen für Ressourcen und für die Ausführung eines Auftrags Azure Stream Analytics leistungsstarken. SUs bieten eine Möglichkeit, die relative Verarbeitung basierend auf einer angeglichene Maß für die CPU, Speicher, Kapazität Veranstaltung und lesen und Schreiben Sätzen. Jede streaming Einheit entspricht ungefähr 1MB pro Sekunde Durchsatz. 

Wie viele SUs auswählen für eine bestimmte Position der Partitionskonfiguration für die Eingaben und für das Projekt definierte Abfrage hängt erforderlich sind. Sie können bis zu Ihrer Quote in streaming Einheiten für ein Projekt mithilfe des klassischen Azure-Portal auswählen. Jede Azure-Abonnement standardmäßig weist ein Kontingent von bis zu 50 streaming Einheiten für alle Analytics Einzelvorgänge in einem bestimmten Bereich. Wenden Sie sich an [Microsoft Support](http://support.microsoft.com), um streaming Einheiten für Ihre Abonnements zu erhöhen.

Die Anzahl der streaming Einheiten, die ein Auftrag nutzen kann, hängt von der Partitionskonfiguration für die Eingaben und die Abfrage, die für das Projekt definiert. Beachten Sie auch, dass ein gültiger Wert für den Stream Einheiten verwendet werden muss. Die gültigen Werte beginnen mit 1, 3, 6 und dann nach oben in Schritten von 6, wie unten dargestellt.

![Azure Stream Analytics streamen Einheiten skalieren.][img.stream.analytics.streaming.units.scale]

In diesem Artikel wird gezeigt, wie zu berechnen und Optimieren Sie die Abfrage um Durchsatz für Analytics Aufträge zu erhöhen.

## <a name="embarrassingly-parallel-job"></a>Sehr parallel Position
Der sehr parallele Auftrag ist der am besten skalierbare Szenario, die, das wir in Azure Stream Analytics haben. Es wird ein Teil der Eingabe in eine Instanz der Abfrage mit einen Teil der Ausgabe verbunden. Diese Parallelism erreichen erfordert ein paar Punkte:

1.  Wenn Ihre Abfragelogik durch die gleiche Abfrageinstanz verarbeitet demselben Schlüssel abhängig ist, müssen Sie sicherstellen, dass die Ereignisse in der gleichen Teil Ihrer Eingabe wechseln. Bei Ereignis Hubs bedeutet dies, dass die Daten Ereignis **PartitionKey** festgelegt haben muss oder partitionierten Absender können. Für Blob bedeutet dies, dass die Ereignisse in der gleichen Partition Ordner gesendet werden. Wenn Ihre Abfragelogik durch die gleiche Abfrageinstanz denselben Schlüssel verarbeitet werden keine erforderlich sind, können Sie diese Anforderung ignorieren. Ein Beispiel dafür wäre eine einfache auswählen/Project/Filter Abfrage.  
2.  Sobald die Daten angeordnet ist, wie er auf der Seite der Eingabe werden muss, müssen wir stellen Sie sicher, dass Ihre Abfrage verteilt werden. Dazu müssen Sie alle Schritte **Partition By** verwenden. Mehrere Schritte zulässig sind, aber diese mit demselben Schlüssel aufgeteilt werden müssen. Eine andere ist zu beachten, dass derzeit Partitionierungsschlüssels **PartitionId** einen vollständig parallelen Auftrag haben festgelegt werden muss.  
3.  Zurzeit unterstützt nur Ereignis Hubs und Blob partitionierten Ausgabe. Für die Ausgabe Ereignis Hubs müssen Sie das Feld **PartitionKey** benutzerspezifisch **PartitionId**konfigurieren. Für Blob müssen Sie nicht können.  
4.  Ein anderes Objekt zu beachten Sie die Anzahl der Eingabewerte Partitionen muss die Anzahl der Ausgabe Partitionen gleich. BLOB Ausgabe nicht zurzeit Partitionen unterstützt, aber dies ist in Ordnung, weil es das Partitionierungsschema der übergeordneten Abfrage erben. Beispiele für Arbeitsspeicherpartitionswerte, die einen vollständig parallelen Auftrag zulassen möchten:  
    1.  8 Ereignis Hubs Eingabe-Partitionen und 8 Ereignis Hubs Partitionen
    2.  8 Ereignis Hubs Eingabemethoden Partitionen und Blob-Ausgabe  
    3.  8 Blob Eingabewerte Partitionen und Blob-Ausgabe  
    4.  8 Eingabewerte Blob-Partitionen und 8 Ereignis Hubs ausgeben Partitionen  

Hier sind einige Beispielszenarien, die sehr parallel sind.

### <a name="simple-query"></a>Auswahlabfrage
Eingabe – Ereignis Hubs mit 8 Partitionen Ausgabe – Ereignis Hub mit 8 Partitionen

**Abfrage:**

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Diese Abfrage ist ein einfacher Filter und als solche, wir müssen nicht über das Aufteilen von der Eingabe an Ereignis Hubs gesendete befassen. Beachten Sie, dass die Abfrage- **Partition By** **PartitionId**, verfügt, damit wir Anforderung #2 von oben durchführen können. Für die Ausgabe müssen wir konfigurieren die Ausgabe Ereignis Hubs im Auftrag Feld **PartitionKey** auf **PartitionId**festgelegt haben. Eine letzte überprüfen, Eingabewerte Partitionen == Ausgabe Partitionen. Diese Suchtopologie ist sehr parallel.

### <a name="query-with-grouping-key"></a>Abfrage mit Gruppierungsschlüssel
Eingabe – Ereignis Hubs mit 8 Partitionen Ausgabe – Blob

**Abfrage:**

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Diese Abfrage enthält einen Gruppierungsschlüssel und als solche demselben Schlüssel von der gleichen Abfrageinstanz verarbeitet werden muss. Dies bedeutet, dass Beginn müssen wir unsere Ereignisse an Ereignisse Hubs in einer partitionierten Weise senden. Welche Taste kümmern wir? **PartitionId** ist ein Auftrag Logik Konzept, entscheidend, die wir kümmern ist **TollBoothId**. Dies bedeutet, dass wir sollte die **PartitionKey** festlegen Ereignis Daten Wir senden an Ereignis Hubs werden die **TollBoothId** des Ereignisses. Die Abfrage enthält- **Partition By** **PartitionId**, damit wir es gute werden. Für die Ausgabe da es Blob, ist benötigen nicht wir Gedanken **PartitionKey**konfigurieren. Für die Anforderung #4 erneut, ist dies Blob, damit wir nicht mich damit befassen. Diese Suchtopologie ist sehr parallel.

### <a name="multi-step-query-with-grouping-key"></a>Schritt Multi-Abfrage mit Gruppierungsschlüssel ###
Eingabemethoden – teilt den Ereignis-Hub mit 8 Ausgabe – Ereignis Hub mit 8 Partitionen

**Abfrage:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Diese Abfrage enthält einen Gruppierungsschlüssel und als solche demselben Schlüssel von der gleichen Abfrageinstanz verarbeitet werden muss. Wir können dieselbe Strategie wie die vorherige Abfrage verwenden. Die Abfrage enthält mehrere Schritte. Verfügt jedes Schritt- **Partition By** **PartitionId**? Ja, damit wir gute werden. Für die Ausgabe müssen wir für die **PartitionKey** festzulegen, **PartitionId** wie weiter oben beschriebenen, und wir können auch anzeigen, dass sie die gleiche Anzahl von Partitionen als Eingabe verfügt. Diese Suchtopologie ist sehr parallel.


## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a>Beispielszenarien, die nicht sind sehr parallel

### <a name="mismatched-partition-count"></a>Nicht übereinstimmende Partitionsanzahl ###
Eingabe – Ereignis Hubs mit 8 Partitionen Ausgabe – Ereignis Hub mit 32 Partitionen

Ist es unerheblich, was die Abfrage in diesem Fall ist, da die Eingabe partitionieren zählen! = Partitionsanzahl Ausgabe.

### <a name="not-using-event-hubs-or-blobs-as-output"></a>Kein verwenden Ereignis Hubs oder Blobs in der Ausgabe
Eingabe – Ereignis Hubs mit 8 Partitionen Ausgabe – PowerBI

PowerBI Ausgabe unterstützen nicht aktuell Partitionierung.

### <a name="multi-step-query-with-different-partition-by-values"></a>Multi Schritt Abfrage mit unterschiedlichen Partition By-Werten
Eingabemethoden – teilt den Ereignis-Hub mit 8 Ausgabe – Ereignis Hub mit 8 Partitionen

**Abfrage:**

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    
Wie Sie sehen können, wird im zweite Schritt **TollBoothId** als Partitionierungsschlüssels verwendet. Dies ist nicht dieselbe als ersten Schritt und daher uns führen Sie eine zufällige Wiedergabe erforderlich. 

Dies sind einige Beispiele und Counterexamples Stream Analytics Einzelvorgänge, die eine sehr parallel Suchtopologie erzielen können und sie das Risiko, dass die maximalen Dezimalstellen. Für Projekte, die nicht dieser Profile passen, werden zukünftige Updates mit Informationen, wie Sie einige der anderen kanonische Stream Analytics Szenarios maximal zu skalieren vorgenommen werden.

Für jetzt erstellen die folgenden Faustregel wiederzuverwenden:

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Berechnen Sie die maximale Einheiten eines Auftrags streaming
Die Gesamtzahl der streaming Einheiten, die von einem Auftrag des Streams Analytics verwendet werden können, hängt von der Anzahl der Schritte in der Abfrage, die für das Projekt und die Anzahl der Partitionen für jeden Schritt definiert.

### <a name="steps-in-a-query"></a>Die Schritte in einer Abfrage
Eine Abfrage kann eine oder mehrere Schritte aus haben. Jeder Schritt besteht aus einer Unterabfrage durch das Schlüsselwort **mit** definiert. Die einzige Abfrage, die sich außerhalb der **WITH** -Schlüsselwort wird auch als einen Schritt, beispielsweise die **SELECT** -Anweisung in der folgenden Abfrage gezählt werden:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

Die vorherige Abfrage umfasst zwei Schritte.

> [AZURE.NOTE] Dieses Beispielabfrage werden später in diesem Artikel erläutert.

### <a name="partition-a-step"></a>Partitionieren eines Schritts

Partitionierung eines Schritts erfordert Folgendes:

- Die Datenquelle muss aufgeteilt werden. Weitere Informationen finden Sie im [Ereignis Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md).
- Die **SELECT** -Anweisung der Abfrage muss aus einer partitionierten Eingabewerte Quelle lesen.
- Die Abfrage in der Schritt muss das Schlüsselwort **Partition By** aufweisen.

Wenn eine Abfrage konfiguriert ist, die Eingabewerte Ereignisse werden verarbeitete und zusammengefasster in separaten Partitionsgruppen und Ausgaben Ereignisse sind für jede Gruppe generiert. Wenn eine kombinierte Aggregat erwünscht ist, müssen Sie einen zweiten nicht aufgeteilt Schritt erstellen zu aggregieren.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Berechnen der Max streaming Einheiten für ein Projekt

Alle nicht aufgeteilt Schritte können bis zu sechs streaming Einheiten für ein Projekt Stream Analytics zusammen skalieren. Zum Hinzufügen muss zusätzliche streaming Einheiten, ein Schritt aufgeteilt werden. Jede Partition kann sechs streaming Einheiten verfügen.

<table border="1">
<tr><th>Abfrage eines Auftrags</th><th>Max streaming Einheiten für das Projekt</th></td>

<tr><td>
<ul>
<li>Die Abfrage enthält einen Schritt.</li>
<li>Der Schritt ist nicht konfiguriert.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>Der Eingabedaten Stream ist durch 3 aufgeteilt.</li>
<li>Die Abfrage enthält einen Schritt.</li>
<li>Der Schritt ist aufgeteilt.</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>Die Abfrage enthält zwei Schritte.</li>
<li>Keiner der Schritte ist unterteilt.</li>
</ul>
</td>
<td>6</td></tr>



<tr><td>
<ul>
<li>Die Dateneingabe Stream ist durch 3 aufgeteilt.</li>
<li>Die Abfrage enthält zwei Schritte. Der Eingabewerte Schritt aufgeteilt und im zweite Schritt nicht.</li>
<li>Die SELECT-Anweisung liest aus der partitionierten Eingabe.</li>
</ul>
</td>
<td>24 (18 partitionierten Schritten) + 6 Schritten nicht aufgeteilt</td></tr>
</table>

### <a name="examples-of-scale"></a>Beispiele für skalieren
Die folgende Abfrage berechnet die Anzahl von Autos durchgehen einen Sender weder eine gebührenpflichtige mit drei Mautstationen innerhalb eines Fensters drei Minuten. Diese Abfrage kann bis zu sechs streaming Einheiten skaliert werden.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Um weitere streaming Einheiten für die Abfrage verwenden, stream sowohl die Daten darstellt und die Abfrage aufgeteilt werden muss. Vorausgesetzt, dass die Stream Datenpartition auf 3 festgelegt ist, kann die folgende geänderte Abfrage nach Zeitphasen bis zum 18 streaming Einheiten skaliert werden:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Wenn eine Abfrage konfiguriert ist, sind die Eingabewerte Ereignisse verarbeitet und in separaten Partitionsgruppen aggregiert. Ausgabe Ereignisse sind auch für jede der Gruppen generiert. Partitionierung kann einige unerwartete Ergebnisse verursachen, wenn das **Group by -** Feld nicht in die Dateneingabe Stream der Partitionsschlüssel ist. Zum Beispiel ist das **TollBoothId** -Feld in der vorherigen Beispielabfrage nicht Partition Schlüssel Input1. Die Daten aus der Gebührenschalter #1 können in mehreren Partitionen verteilt.

Jede der Partitionen Input1 verarbeitet werden getrennt von Stream Analytics und mehrere Datensätze der Auto-Pass-through Anzahl für die gleichen Gebührenschalter im selben Tumbling Fenster erstellt werden. Wenn die Eingabewerte Partitionsschlüssel geändert werden kann, können Sie dieses Problem behoben werden durch Hinzufügen von einem weiteren Schritt nicht Partition, beispielsweise:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Diese Abfrage kann auf 24 streaming Einheiten skaliert werden.

>[AZURE.NOTE] Wenn Sie zwei Streams teilnehmen möchten, stellen Sie sicher, dass die Streams durch die Partitionsschlüssel der Spalte formatiert sind, führen Sie die Verknüpfungen, und Sie haben die gleiche Anzahl von Partitionen in beiden Streams.


## <a name="configure-stream-analytics-job-partition"></a>Konfigurieren von Stream Analytics Auftrag partition

**Anpassen eine streaming Einheit für ein Projekt**

1. Melden Sie sich mit dem [Verwaltungsportal](https://manage.windowsazure.com).
2. Klicken Sie im linken Bereich auf **Stream Analytics** .
3. Klicken Sie auf den Stream Analytics Auftrag, den Sie skalieren möchten.
4. Klicken Sie auf **SKALA** am oberen Rand der Seite.

![Azure Stream Analytics streamen Einheiten skalieren.][img.stream.analytics.streaming.units.scale]

Im Portal Azure können skalierungseinstellungen unter Einstellungen zugegriffen werden:

![Konfiguration von Azure-Portal Stream Analytics Position][img.stream.analytics.preview.portal.settings.scale]

## <a name="monitor-job-performance"></a>Überwachen der Leistung von Position

Mit der Verwaltungsportal, können Sie den Durchsatz eines Auftrags in Ereignisse/Sekunde verfolgen:

![Azure Stream Analytics Monitor Aufträge][img.stream.analytics.monitor.job]

Berechnen des erwarteten Durchsatzes des die Arbeitsbelastung in Ereignisse/Sekunde. Ist der Durchsatz kleiner als erwartet, Optimieren Sie die Eingabewerte Partition, Optimieren Sie die Abfrage aus, und fügen Sie zusätzliche streaming Einheiten auf Ihre Arbeit.

## <a name="stream-analytics-throughput-at-scale---raspberry-pi-scenario"></a>Stream Analytics Durchsatz bei - Szenario Himbeeren Pi


Um zu verstehen, wie Stream Analytics Aufträge in einer Situation im Hinblick auf Verarbeitung Durchsatz über mehrere Streaming Einheiten skalieren, ist hier ein Versuch, die Sensordaten (Clients) in Ereignis Hub sendet, verarbeitet und Benachrichtigung oder Statistiken als Ausgabe an einen anderen Ereignis Hub sendet.

Der Client sendet synthetisierte Sensordaten an Ereignis Hubs im JSON-Format zu Stream Analytics und die Datenausgabe ist ebenfalls im JSON-Format.  So sieht wie die Beispieldaten wie – aussehen würde  

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

Abfrage: "senden eine Benachrichtigung, wenn die Light deaktiviert ist"  

    SELECT AVG(lght),
     “LightOff” as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

Messen Durchsatz: Durchsatz in diesem Zusammenhang ist die Menge der Eingabedaten verarbeiteten Stream Analytics in eine feste Zeitspanne (10 Minuten). Um optimale Verarbeitung Durchsatz für die Eingabedaten zu erreichen, stream sowohl die Daten darstellt, und die Abfrage aufgeteilt werden muss. Auch ist **COUNT()**enthalten, in der Abfrage messen, wie viele Eingabewerte Ereignisse verarbeitet wurden. Um sicherzustellen, dass die Position von Ereignissen kommen nicht einfach wartet, wurde jedes Teil der Eingabewerte Ereignis Hub mit ausreichend Eingabedaten (etwa 300MB) vorinstalliert werden.  

Nachstehend sind die Ergebnisse mit zunehmender Anzahl der Einheiten Streaming und entsprechende Partition in Ereignis Hubs ermittelt.  

<table border="1">
<tr><th>Eingabe Partitionen</th><th>Die Ausgabe Partitionen</th><th>Streaming Einheiten</th><th>Konstanter Datendurchsatz
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4.06 MB/s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8.06 MB/s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 MB/s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 MB/s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 MB/s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 MB/s</td>
</tr>
</table>

![IMG.Stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Anfordern von Hilfe
Versuchen Sie für weitere Unterstützung zu erhalten unseren [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Azure Stream Analytics Query Language Bezug](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings.png

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
 
