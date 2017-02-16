<properties
    pageTitle="Verwenden von verweisen Daten und Nachschlagen Tabellen in Stream Analytics | Microsoft Azure"
    description="Verwenden Sie Daten in einer Abfrage Stream Analytics Bezug"
    keywords="Nachschlagetabelle, Verweisdaten"
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

# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Verwenden von verweisen Daten oder Nachschlagetabelle Tabellen in einer Eingabe Stream Analytics-stream

Verweisdaten (auch bekannt als Nachschlagetabelle) einer begrenzten Datengruppe zurück, die statisch ist oder verlangsamen Natur, ändern verwendet werden eine Suche durchführen oder Ihre Datenstream zuordnen. Damit wird der Bezug Daten in Ihrem Auftrag Azure Stream Analytics verwenden Sie werden in der Regel ein [Bezug Daten verknüpfen](https://msdn.microsoft.com/library/azure/dn949258.aspx) in Ihrer Abfrage verwendet. Stream Analytics verwendet Azure Blob-Speicher als den Layer Speicher für Daten Bezug und mit Azure Data Factory Bezug Daten transformiert und/oder zu Azure Blob-Speicher, für den Einsatz als Referenzdaten aus [einer beliebigen Anzahl von Cloud Grundlage und lokalen Datenspeicher](../data-factory/data-factory-data-movement-activities.md)kopiert werden können. Daten Bezug können als eine Abfolge von Blobs (in der Eingabewerte Konfiguration definierten) in aufsteigender Reihenfolge von Datum und Uhrzeit der angegebenen im Blob-Namen verwendet werden. Es unterstützt **nur** bis zum Ende der Sequenz mithilfe eines Datum/Uhrzeit **größer** als der durch den letzten Blob in der Sequenz angegeben hinzufügen.

## <a name="configuring-reference-data"></a>Konfigurieren von Daten Bezug

Um die Daten Bezug zu konfigurieren, müssen Sie zunächst eine Eingabe zu erstellen, die vom Typ **Daten Bezug**ist. In der nachfolgenden Tabelle wird jede Eigenschaft, die Sie beim Erstellen der zugehörige Beschreibung der Verweis die Dateneingabe angeben müssen erläutert:

<table>
<tbody>
<tr>
<td>Eigenschaftsname</td>
<td>Beschreibung</td>
</tr>
<tr>
<td>Eingabe Alias</td>
<td>Ein Anzeigename, der in der Abfrage Position in Bezug auf diese Eingabe verwendet wird.</td>
</tr>
<tr>
<td>Speicher-Konto</td>
<td>Der Name des Speicherkontos, wo sich Ihre Blob-Dateien befinden. Ist im selben Abonnement als Ihre Stream Analytics vornehmen, können Sie es in der Dropdownliste unten auswählen.</td>
</tr>
<tr>
<td>Kontoschlüssel Speicher</td>
<td>Den geheimen Schlüssel mit Speicher-Konto verknüpft ist. Dies wird automatisch ausgefüllt, wenn das Speicherkonto im selben Abonnement als Ihre Arbeit Stream Analytics ist.</td>
</tr>
<tr>
<td>Speichercontainer</td>
<td>Container stellen eine logische Gruppierung für Blobs in Microsoft Azure Blob-Dienst gespeichert. Wenn Sie einen Blob zum Dienst Blob hochladen, müssen Sie einen Container für die Blob angeben.</td>
</tr>
<tr>
<td>Pfad Muster</td>
<td>Der Pfad der Datei verwendet, um Ihre Blobs im angegebenen Container suchen. In den Pfad können Sie eine oder mehrere Instanzen der folgenden 2 Variablen angeben:<BR>{Date}, {Time}<BR>Beispiel 1: products/{date}/{time}/product-list.csv<BR>Beispiel 2: products/{date}/product-list.csv
</tr>
<tr>
<td>Datumsformat [optional]</td>
<td>Wenn Sie {Date} innerhalb des Musters Pfad, die Sie angegeben haben verwendet haben, können Sie auswählen das Datumsformat in dem Organisation Ihrer Dateien aus der Dropdownliste der unterstützten Formate. Beispiel: JJJJ/MM/TT</td>
</tr>
<tr>
<td>Zeitformat [optional]</td>
<td>Wenn Sie {Time} innerhalb des Musters Pfad, die Sie angegeben haben verwendet haben, können dann Sie auswählen das Zeitformat in dem Organisation Ihrer Dateien aus der Dropdownliste der unterstützten Formate. Beispiel: HH</td>
</tr>
<tr>
<td>Das Ereignisformat</td>
<td>Um sicherzustellen, dass Ihre Abfragen funktionieren wie erwartet, Stream Analytics muss wissen, welche das Format verwenden Sie für eingehende Datenstreams. Für Daten verweisen sind die unterstützten Formate CSV und JSON.</td>
</tr>
<tr>
<td>Codierung</td>
<td>UTF-8 ist der einzige unterstützte Codierung Format zu diesem Zeitpunkt</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Generieren von Daten der Bezug auf einen Zeitplan

Ist Ihre Daten Bezug einer Datenmenge langsam ändern, klicken Sie dann für die Aktualisierung von verweisen, die Daten aktiviert ist, indem Sie ein Muster Pfad in die Eingabewerte-Konfiguration mithilfe der {Date} angeben unterstützen, und Ersetzen Token {Uhrzeit}. Stream Analytics wird eine Sicherungskopie der aktualisierten Bezug Daten auf der Grundlage dieser Pfad Musters wählen. Angenommen, ein Muster aus `sample/{date}/{time}/products.csv` mit einem Datumsformat **"JJJJ MM TT"** und eine Uhrzeit Formats **"Hh: mm"** Stream Analytics zu den aktualisierten Blob weist `sample/2015-04-16/17:30/products.csv` 5:30 Uhr am 16. April-2015 UTC Anzeigedauer Zone.

> [AZURE.NOTE] Suchen Sie aktuell Stream Analytics Einzelvorgänge für die Aktualisierung Blob nur, wenn das Computer Mal bis zu dem Zeitpunkt in den Namen der BLOB-codierte wechselt. Beispielsweise sieht der Auftrag für `sample/2015-04-16/17:30/products.csv` sobald möglich aber nicht früher als 17:30 Uhr am 16. April-2015 UTC Zone Anzeigedauer. Es werden *nie* Suchen nach einer Datei mit einer codierten Zeitblock vor der letzten Zeile, die ermittelt wird.
> 
> Z. B. Nachdem Sie der Auftrag das Blob findet `sample/2015-04-16/17:30/products.csv` ignoriert er keine Dateien mit diesem codierte Datum früher als 17:30 Uhr April 16. 2015 Dies ist eine spät kommen `sample/2015-04-16/17:25/products.csv` Blob erstellt wird im selben Container der Auftrag nicht verwendet.
> 
> Ebenso ist `sample/2015-04-16/17:30/products.csv` nur bei 10:03 Uhr April 16. 2015 erstellt wird, aber keine Blob mit einem älteren Datum im Container vorhanden ist, der Auftrag verwenden Sie diese Datei 10:03 Uhr April 16. 2015 ab und verwenden Sie die vorherigen Verweisdaten bis zu diesem Zeitpunkt wird.
> 
> Eine Ausnahme hierzu wird gestartet, wenn der Auftrag erneut verarbeitet Daten wieder in Zeit werden muss oder der Auftrag zum ersten Mal. Starten Sie zur Startzeit für das letzte Blob gefertigt vor den Auftrag der Auftrag gefunden wird angegebene Zeit ein. Dies geschieht, um sicherzustellen, dass es ein **nicht leerer** Verweis DataSet beim der Auftrag wird gestartet. Wenn eine gefunden werden kann, wird die Position der folgenden Diagnostic angezeigt: `Initializing input without a valid reference data blob for UTC time <start time>`.


[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) kann verwendet werden, zu koordinieren die Aufgabe des Erstellens die aktualisierten Blobs erforderlich Stream Analytics Bezug Datendefinitionen zu aktualisieren. Daten Factory ist eine cloudbasierte Integration Datendienst, der koordiniert und Automatisierung der Bewegung und Transformation von Daten. Daten Factory unterstützt [Herstellen einer Verbindung mit einer großen Anzahl von cloudbasierte und lokalen Datenspeicher](../data-factory/data-factory-data-movement-activities.md) und Verschieben von Daten problemlos in regelmäßigen Abständen, den Sie angeben. Weitere Informationen und schrittweise Anleitungen zum Einrichten einer Daten Factory Verkaufspipeline Bezug Daten für Stream Analytics generieren, die nach einem vordefinierten Zeitplan aktualisiert, schauen Sie sich dieses [Beispiel GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Tipps zum Aktualisieren von Daten Bezug ##

1. Überschreiben Datenblobs Verweis verursacht keine Stream Analytics Blob erneut laden und in einigen Fällen können sie den Auftrag zum Fehlschlagen verursachen. Es wird empfohlen zum Ändern von Daten Bezug zum Hinzufügen eines neuen BLOBs mit demselben Container und Pfad Muster definiert, in der Eingabe der Position und Verwenden eines Datum/Uhrzeit **größer** als der durch den letzten Blob in der Sequenz angegeben.
2.  Blobs nicht angeordnet werden von der Blob des "Letzte Änderung" Zeit, aber nur von Datum und Uhrzeit im Blob angegebenen Bezug Daten benennen Sie die {Date} und {time} ersetzen.
3.  Klicken Sie auf ein paar Anlässe, die ein Auftrag Zeitpunkt zurückkehren muss, müssen daher Datenblobs Bezug nicht geändert oder gelöscht werden.

## <a name="get-help"></a>Anfordern von Hilfe
Für weitere Unterstützung zu erhalten versuchen Sie es unsere [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
Sie haben eine Einführung in Stream Analytics, einen verwalteten Dienst für das streaming Analytics auf Daten aus dem Internet der Dinge wurde. Weitere Informationen zu diesem Dienst finden Sie unter:

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
