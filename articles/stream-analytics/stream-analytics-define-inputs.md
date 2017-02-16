<properties
    pageTitle="Datenverbindung: Data stream Eingaben aus einem Ereignisstream | Microsoft Azure"
    description="Informationen Sie zum Einrichten von einer Datenverbindung zu Stream Analytics "Eingaben" bezeichnet. Eingaben Datenstream von Ereignissen enthalten, und auch verweisen auf Daten."
    keywords="Datenstream, Datenverbindung, Stream von Ereignissen"
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

# <a name="data-connection-learn-about-data-stream-inputs-from-events-to-stream-analytics"></a>Datenverbindung: erfahren Sie mehr über Daten Stream Eingaben von Ereignissen zu Stream Analytics

Die Verbindung mit Analytics Stream ist ein Datenstream von Ereignissen aus einer Datenquelle. Dies ist eine "Eingabe" bezeichnet. Stream Analytics weist herausragende Integration in Azure-Daten Stream Quellen Ereignis Hub, IoT Hub und BLOB-Speicher, die aus dem gleichen oder einem anderen Azure-Abonnement als Ihre Arbeit Analytics werden können.

## <a name="data-input-types-data-stream-and-reference-data"></a>Eingabe Typen von Daten: Daten Stream und Bezug von Daten
Wie Daten mit einer Datenquelle verschoben werden, ist es durch den Auftrag Stream Analytics verbraucht und in Echtzeit verarbeitet. Eingaben sind in zwei Typen unterteilt: Daten streamen Eingaben und Dateneingaben verweisen.

### <a name="data-stream-inputs"></a>Stream Dateneingaben
Ein Datenstream ist ungebundener Abfolge von Ereignissen, die über einen Zeitraum in Kürze. Stream Analytics Einzelvorgänge müssen mindestens eine Stream Dateneingabe um genutzt werden und durch den Auftrag transformiert beinhalten. BLOB-Speicher, Ereignis Hubs und IoT Hubs werden als Eingabe Stream Datenquellen unterstützt. Ereignis Hubs werden verwendet, um die Ereignisstreams aus mehreren Geräten und Diensten, wie soziale Medien Aktivitätsfeeds, vorhandenen Trade Informationen oder Daten aus Sensoren sammeln. Sammeln von Daten aus verbundenen Geräte im Internet der Dinge (IoT) Szenarien sind IoT Hubs optimiert.  BLOB-Speicher kann als Standardeingabesprache Quelle für Aufnahme Datenmengen als Stream verwendet werden.  

### <a name="reference-data"></a>Bezug Daten
Stream Analytics unterstützt einen zweiten Typ der Eingabe als Referenzdaten bezeichnet. Dies sind zusätzliche Daten der statische oder langsam veränderliche über einen Zeitraum ist und in der Regel zur Durchführung Korrelationskoeffizienten und Suchen verwendet werden. Azure Blob-Speicher gibt es zurzeit aus der einzige unterstützte Eingabewerte Datenquelle verweisen. Verweis Quelle Datenblobs sind auf 100MB Größe beschränkt.
So erstellen Sie Bezug Dateneingaben finden Sie unter [Verwenden Bezug Daten](stream-analytics-use-reference-data.md)  

## <a name="create-a-data-stream-input-with-an-event-hub"></a>Erstellen Sie eine Stream Dateneingabe mit einem Ereignis-Hub

[Azure Ereignis Hubs](https://azure.microsoft.com/services/event-hubs/) sind hochgradig skalierbare Ereignis Ingestor veröffentlichen-abonnieren. Sie können Millionen von Ereignissen pro Sekunde, gesammelt werden, damit Sie verarbeiten und großer Datenmengen, von Ihrer verbundenen Geräte und Applikationen erstellte analysieren können. Es ist eine der am häufigsten verwendeten Eingaben für Stream Analytics. Ereignis Hubs und Stream Analytics bieten zusammen eine Lösung durchgehende für Echtzeit Analytics. Ereignis Hubs ermöglichen Kunden Ereignisse in Echtzeit in Azure-Feeds und Stream Analytics Aufträge können sie in Echtzeit verarbeiten. Beispielsweise können Kunden Web Klicks, Sensorwerte, online Protokollieren von Ereignissen zu Ereignis Hubs senden, und erstellen Stream Analytics Aufträge, wenn die Eingabedatenstreams Ereignis Hubs für Echtzeit filtern, aggregieren und Korrelationskoeffizienten verwenden.

Es ist wichtig, beachten Sie, dass der Standard-Zeitstempel der Ereignisse in Kürze aus Ereignis Hubs in Stream Analytics der Zeitstempel ist, den das Ereignis in Ereignis Hub eingegangen, also EventEnqueuedUtcTime. Um die Daten als Stream mit einem Zeitstempel in die Ereignisnutzlast zu verarbeiten, muss das Schlüsselwort [Zeitstempel durch](https://msdn.microsoft.com/library/azure/dn834998.aspx) verwendet werden.

### <a name="consumer-groups"></a>Consumer Gruppen

Jede Eingabe Stream Analytics Ereignis Hub sollte eine eigenen Gruppe Consumer haben konfiguriert sein. Eine Position eines Selbstjoins oder mehrere Eingaben enthält, möglicherweise einige Eingaben vom unterhalb, mehrere Reader gelesen werden, die die Anzahl der Leser in einer einzelnen Consumer Gruppe wirkt sich auf. Damit überschreiten Ereignis Hub maximal 5 Leser pro Consumer Gruppe pro Partition, ist es eine bewährte Methode, die eine Gruppe Consumer für jedes Projekt Stream Analytics angeben. Beachten Sie, dass es auch maximal 20 Consumer Gruppen pro Ereignis-Hub an. Weitere Informationen finden Sie unter des [Ereignisses Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md).

### <a name="configure-event-hub-as-an-input-data-stream"></a>Konfigurieren von Ereignis Hub als Eingabedaten stream

In der nachfolgenden Tabelle wird jede Eigenschaft für den Fall, Hub Registerkarte mit zugehörige Beschreibung Eingabemethoden erläutert:

| EIGENSCHAFTSNAME | BESCHREIBUNG |
|------|------|
| Eingabe Alias | Einen Anzeigenamen ein, der in der Abfrage Position in Bezug auf diese Eingabe verwendet wird |
| Service Bus Namespace | Ein Namespace Dienstbus ist ein Container für eine Gruppe von Personen messaging. Wenn Sie ein neues Ereignis Hub erstellt haben, haben Sie auch einen Namespace Dienstbus erstellt. |
| Ereignis-Hub | Der Name des Ihrer Ereignis Hub Eingabemethoden. |
| Name der Veranstaltung Hub Richtlinie | Die freigegebenen Zugriffsrichtlinie, die auf der Registerkarte Ereignis Hub konfigurieren erstellt werden können. Jede freigegebenen Zugriffsrichtlinie haben einen Namen, die Berechtigungen, die Sie festlegen möchten, und Tastenkombinationen. |
| Ereignis Hub Richtlinienschlüssel | Freigegebene Zugriffstaste zum Authentifizieren des Zugriffs auf den Namespace Dienstbus verwendet. |
| Ereignis Hub Consumer Gruppe (Optional) | Die Daten aus dem Ereignis Hub Aufnahme Consumer Gruppe. Wenn nicht angegeben, werden Stream Analytics Aufträge Standard Consumer mithilfe der Gruppe Daten aus dem Ereignis Hub Aufnahme. Es wird empfohlen, distinct Consumer Gruppe für jedes Projekt Stream Analytics verwendet werden soll. |
| Das Ereignisformat | Um sicherzustellen, dass Ihre Abfragen funktionieren wie erwartet, Stream Analytics muss wissen, welche Serialisierungsformat (JSON, CSV- oder Avro) verwenden Sie für eingehende Datenstreams. |
| Codierung | UTF-8 ist der einzige unterstützte Codierung Format zu diesem Zeitpunkt an. |

Wenn Ihre Daten aus einer Datenquelle Hub Ereignis stammt, können Sie in Ihrer Abfrage Stream Analytics einige Metadatenfeldern zugreifen. Die folgende Tabelle enthält die Felder und deren Beschreibung an.

| EIGENSCHAFT | BESCHREIBUNG |
|------|------|
| EventProcessedUtcTime | Das Datum und die Uhrzeit, zu der das Ereignis von Stream Analytics verarbeitet wurde. |
| EventEnqueuedUtcTime | Datum und Uhrzeit, die das Ereignis von Ereignis Hubs empfangen wurde. |
| PartitionId | Die nullbasierte Partitions-ID für die Eingabewerte Netzwerkadapter. |

Sie können beispielsweise eine Abfrage wie folgt geschrieben werden können:

````
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
````

## <a name="create-an-iot-hub-data-stream-input"></a>Erstellen einer IoT Hub Stream Dateneingabe

Azure Iot-Hub ist eine hochgradig skalierbare Veröffentlichen-Abonnieren Ereignis Ingestor für IoT Szenarien optimiert.
Es ist wichtig, beachten Sie, dass der Standard-Zeitstempel der Ereignisse in Kürze aus IoT Hubs in Stream Analytics der Zeitstempel ist, den das Ereignis in IoT Hub eingegangen, also EventEnqueuedUtcTime. Um die Daten als Stream mit einem Zeitstempel in die Ereignisnutzlast zu verarbeiten, muss das Schlüsselwort [Zeitstempel durch](https://msdn.microsoft.com/library/azure/dn834998.aspx) verwendet werden.

> [AZURE.NOTE] Nur mit einer Eigenschaft DeviceClient gesendete Nachrichten können verarbeitet werden.

### <a name="consumer-groups"></a>Consumer Gruppen

Jede Eingabe Stream Analytics IoT Hub sollte eine eigene Gruppe Consumer haben konfiguriert sein. Eine Position eines Selbstjoins oder mehrere Eingaben enthält, möglicherweise einige Eingaben vom unterhalb, mehrere Reader gelesen werden, die die Anzahl der Leser in einer einzelnen Consumer Gruppe wirkt sich auf. Damit überschreiten IoT Hub maximal 5 Leser pro Consumer Gruppe pro Partition, ist es eine bewährte Methode, die eine Gruppe Consumer für jedes Projekt Stream Analytics angeben.

### <a name="configure-iot-hub-as-an-data-stream-input"></a>Konfigurieren Sie als Datenstream Eingabemethoden IoT-Hub

In der nachfolgenden Tabelle wird jede Eigenschaft auf der Registerkarte IoT Hub Eingabewerte, mit deren Beschreibung erläutert:

| EIGENSCHAFTSNAME | BESCHREIBUNG |
|------|------|
| Eingabe Alias | Ein Anzeigename, der in der Abfrage Position in Bezug auf diese Eingabe verwendet wird. |
| IoT-Hub | Eine IoT-Hub ist ein Container für eine Gruppe von Personen per. |
| Endpunkt | Der Name Ihres Endpunkts IoT Hub. |
| Name des freigegebenen Access-Richtlinie | Die freigegebenen Zugriffsrichtlinie an den IoT Hub Zugriff gewähren. Jede freigegebenen Zugriffsrichtlinie haben einen Namen, die Berechtigungen, die Sie festlegen möchten, und Tastenkombinationen. |
| Freigegebene Richtlinie Zugriffstaste | Freigegebene Zugriffstaste zum Authentifizieren des Zugriffs auf die IoT Hub verwendet. |
| Consumer Gruppe (Optional) | Die Daten aus dem IoT Hub Aufnahme Consumer Gruppe. Wenn nicht angegeben, werden Stream Analytics Aufträge Standard Consumer mithilfe der Gruppe Daten aus dem IoT Hub Aufnahme. Es wird empfohlen, distinct Consumer Gruppe für jedes Projekt Stream Analytics verwendet werden soll. |
| Das Ereignisformat | Um sicherzustellen, dass Ihre Abfragen funktionieren wie erwartet, Stream Analytics muss wissen, welche Serialisierungsformat (JSON, CSV- oder Avro) verwenden Sie für eingehende Datenstreams. |
| Codierung | UTF-8 ist der einzige unterstützte Codierung Format zu diesem Zeitpunkt an. |

Wenn Ihre Daten aus einer Datenquelle IoT Hub stammt, können Sie in Ihrer Abfrage Stream Analytics einige Metadatenfeldern zugreifen. Die folgende Tabelle enthält die Felder und deren Beschreibung an.

| EIGENSCHAFT | BESCHREIBUNG |
|------|------|
| EventProcessedUtcTime | Datum und Uhrzeit, die das Ereignis verarbeitet wurde. |
| EventEnqueuedUtcTime | Datum und Uhrzeit, die das Ereignis vom IoT Hub empfangen wurde. |
| PartitionId | Die nullbasierte Partitions-ID für die Eingabewerte Netzwerkadapter. |
| IoTHub.MessageId | Hiermit wird eine bidirektionalen Kommunikation IoT Hub zu koordinieren. |
| IoTHub.CorrelationId | Antworten auf Nachrichten und Feedback IoT Hub verwendet. |
| IoTHub.ConnectionDeviceId | Die authentifizierten Id verwendet, um diese Nachricht, Stempel IoT Hub Servicebound Nachrichten zu senden. |
| IoTHub.ConnectionDeviceGenerationId | Die GenerationId des Geräts authentifizierten verwendet, um diese Nachricht zu senden sowie Servicebound Nachrichten von IoT Hub. |
| IoTHub.EnqueuedTime | Zeit, wenn die Nachricht vom IoT Hub empfangen wurde. |
| IoTHub.StreamId | Benutzerdefinierte Ereigniseigenschaft vom Gerät Absender hinzugefügt. |

## <a name="create-a-blob-storage-data-stream-input"></a>Erstellen Sie eine BLOB-Speicher Stream Dateneingabe

Für Szenarios mit großen Mengen von unstrukturierten Daten in der Cloud gespeichert bietet Blob-Speicher eine kostengünstigere und skalierbare Lösung. Daten im [BLOB-Speicher](https://azure.microsoft.com/services/storage/blobs/) werden im Allgemeinen als "at Rest" Daten aber als Datenstream durch Stream Analytics verarbeitet werden. Ein gängiges Szenario für BLOB-Speicher Eingaben mit Stream Analytics ist Log Verarbeitung, wobei werden werden von einem System erfasst und analysiert und aussagekräftige Daten extrahieren verarbeitet werden muss.

Es ist wichtig, beachten Sie, dass der Standard-Zeitstempel BLOB-Speicherereignisse im Stream Analytics der Zeitstempel ist, dass das Blob welche *IsBlobLastModifiedUtcTime*zuletzt geändert wurde. Um die Daten als Stream mit einem Zeitstempel in die Ereignisnutzlast zu verarbeiten, muss das Schlüsselwort [Zeitstempel durch](https://msdn.microsoft.com/library/azure/dn834998.aspx) verwendet werden.

Beachten Sie auch, dass CSV formatierte Eingaben **erfordern** eine Kopfzeile zum Definieren von Feldern für den Datensatz ein. Eine weitere Zeilenfelder müssen alle **eindeutig**sein.

> [AZURE.NOTE] Hinzufügen von Inhalten zu einer vorhandenen Blob unterstützt Stream Analytics nicht. Stream Analytics wird nur einen Blob zeigen Sie einmal und Änderungen abgeschlossen werden, nachdem diese gelesen werden nicht verarbeitet. Die bewährte Methode besteht darin alle Daten einmal hochladen und nicht keine weiteren Ereignisse im Blob-Speicher hinzufügen.

In der nachfolgenden Tabelle wird jede Eigenschaft in der BLOB-Speicher von Registerkarte mit zugehörige Beschreibung erläutert:

<table>
<tbody>
<tr>
<td>EIGENSCHAFTSNAME</td>
<td>BESCHREIBUNG</td>
</tr>
<tr>
<td>Eingabe Alias</td>
<td>Ein Anzeigename, der in der Abfrage Position in Bezug auf diese Eingabe verwendet wird.</td>
</tr>
<tr>
<td>Speicher-Konto</td>
<td>Der Name des Speicherkontos, wo sich Ihre Blob-Dateien befinden.</td>
</tr>
<tr>
<td>Kontoschlüssel Speicher</td>
<td>Den geheimen Schlüssel mit Speicher-Konto verknüpft ist.</td>
</tr>
<tr>
<td>Speichercontainer
</td>
<td>Container stellen eine logische Gruppierung für Blobs in Microsoft Azure Blob-Dienst gespeichert. Wenn Sie einen Blob zum Dienst Blob hochladen, müssen Sie einen Container für die Blob angeben.</td>
</tr>
<tr>
<td>Pfad Präfix Muster [optional]</td>
<td>Der Pfad der Datei verwendet, um Ihre Blobs im angegebenen Container suchen.
In den Pfad können Sie eine oder mehrere Instanzen der folgenden 3 Variablen angeben:<BR>{Date}, {Time}<BR>{Partition}<BR>Beispiel 1: cluster1/Protokolle / {date} / {time} / {partitionieren}<BR>Beispiel 2: cluster1/Protokolle / {date}<P>Beachten Sie, dass "*" ist kein zulässiger Wert für Pathprefix. Nur gültige <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Zeichen Azure Blob</a> zulässig sind.</td>
</tr>
<tr>
<td>Datumsformat [optional]</td>
<td>Wenn das Datum Token in den Pfad Präfix verwendet wird, können Sie das Datumsformat auswählen, in dem Ihre Dateien organisiert sind. Beispiel: JJJJ/MM/TT</td>
</tr>
<tr>
<td>Zeitformat [optional]</td>
<td>Wird das Uhrzeit Token in den Pfad Präfix verwendet, geben Sie das Zeitformat, in dem Ihre Dateien organisiert sind. Aktuell ist der einzige unterstützte Wert HH.</td>
</tr>
<tr>
<td>Das Ereignisformat</td>
<td>Um sicherzustellen, dass Ihre Abfragen funktionieren wie erwartet, Stream Analytics muss wissen, welche Serialisierungsformat (JSON, CSV- oder Avro) verwenden Sie für eingehende Datenstreams.</td>
</tr>
<tr>
<td>Codierung</td>
<td>Für CSV und JSON ist UTF-8 der einzige unterstützte Codierung Format zu diesem Zeitpunkt an.</td>
</tr>
<tr>
<td>Trennzeichen</td>
<td>Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen für Serialisieren der Daten im CSV-Format. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Registerkarte und senkrechten Strich.</td>
</tr>
</tbody>
</table>

Wenn Ihre Daten von einer BLOB-Speicherquelle stammt, können Sie ein paar Metadatenfeldern in Ihrer Abfrage Stream Analytics zugreifen. Die folgende Tabelle enthält die Felder und deren Beschreibung an.

| EIGENSCHAFT | BESCHREIBUNG |
|------|------|
| BlobName | Der Name des Eingabewerte Blob, dem das Ereignis stammt. |
| EventProcessedUtcTime | Das Datum und die Uhrzeit, zu der das Ereignis von Stream Analytics verarbeitet wurde. |
| BlobLastModifiedUtcTime | Datum und Uhrzeit der letzten Änderung das Blob. |
| PartitionId | Die nullbasierte Partitions-ID für die Eingabewerte Netzwerkadapter. |

Sie können beispielsweise eine Abfrage wie folgt geschrieben werden können:

````
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
````


## <a name="get-help"></a>Anfordern von Hilfe
Für weitere Unterstützung zu erhalten versuchen Sie es unsere [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
Sie haben für Ihre Aufträge Stream Analytics Datenverbindungsoptionen in Azure erfahren. Weitere Informationen zum Stream Analytics finden Sie unter:

- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren Sie Azure Stream Analytics Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Bezug](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
