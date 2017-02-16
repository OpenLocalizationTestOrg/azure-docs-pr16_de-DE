<properties 
    pageTitle="Suchen den Datenverkehr Analytics Azure suchen | Microsoft Azure" 
    description="Aktivieren Sie suchen den Datenverkehr Analytics für Azure Suche eines Suchdiensts Cloud gehosteten auf Microsoft Azure zum Entsperren Rückschlüsse auf den Benutzern und die Daten ein." 
    services="search" 
    documentationCenter="" 
    authors="bernitorres" 
    manager="pablocas" 
    editor=""
/>

<tags 
    ms.service="search" 
    ms.devlang="multiple" 
    ms.workload="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="09/23/2016" 
    ms.author="betorres"
/>


# <a name="enabling-and-using-search-traffic-analytics"></a>Aktivieren und Verwenden von Suchen den Datenverkehr Analytics

Suche Datenverkehr Analytics ist eine Azure Suchfunktion, mit dem Sie Einblick in Ihre Suchdienst erlangen und Aufheben der Sperre Rückschlüsse auf den Benutzern und deren Verhalten. Wenn Sie dieses Feature aktivieren, werden die Suche-Daten mit einem Speicherkonto Ihrer Wahl kopiert. Zu diesen Daten gehören Ihre Suche Dienst Protokolle und zusammengefasster Betrieb Kennzahlen, dass Sie verarbeiten und für eine weitere Analyse bearbeiten können.

## <a name="how-to-enable-search-traffic-analytics"></a>So suchen den Datenverkehr Analytics aktivieren

Benötigen Sie ein Speicherkonto in derselben Region und Abonnement als Suchdienst.

> [AZURE.IMPORTANT] Standard anfallen für dieses Speicherkonto

Sie können die Suche Datenverkehr Analytics im Portal oder über PowerShell aktivieren. Sobald aktiviert, beginnt die Daten in Ihr Speicherkonto innerhalb von 5 bis 10 Minuten in diesen Containern zwei Blob entdeckt:

    insights-logs-operationlogs: search traffic logs
    insights-metrics-pt1m: aggregated metrics


### <a name="a-using-the-portal"></a>A Verwenden des Portals
Öffnen Sie Ihrer Azure Suchdienst im [Azure-Portal](http://portal.azure.com)an. Finden Sie unter Einstellungen die Option zur Suche Datenverkehr Analytics aus. 

![][1]

Ändern Sie den Status in **auf**, wählen Sie das Konto Azure-Speicher zu verwenden, und wählen Sie die Daten, die Sie kopieren möchten: Protokolle, Kennzahlen oder beides. Es empfiehlt sich, Protokolle und Kennzahlen kopieren. Sie können die Aufbewahrungsrichtlinie für Ihre Daten von 1 bis 365 Tage festlegen. Wenn Sie die Daten endlos beibehalten möchten, legen Sie die Aufbewahrung (Tage) 0.

![][2]

### <a name="b-using-powershell"></a>B. Mithilfe der PowerShell

Vergewissern Sie sich, dass Sie die neuesten [Azure PowerShell-Cmdlets](https://github.com/Azure/azure-powershell/releases) installiert haben.

Klicken Sie dann erhalten Sie der Ressource-Ids für Ihre Suchdienst und Ihr Konto Speicher. Sie können ResourceId-finden sie in der Portalseite Navigieren zu den Einstellungen-Eigenschaften > >.

![][3]

```PowerShell
Login-AzureRmAccount
$SearchServiceResourceId = "Your Search service resource id"
$StorageAccountResourceId = "Your Storage account resource id"
Set-AzureRmDiagnosticSetting -ResourceId $SearchServiceResourceId StorageAccountId $StorageAccountResourceId -Enabled $true
```

## <a name="understanding-the-data"></a>Grundlegendes zu den Daten

Die Daten werden in Azure-Speicher Blobs als JSON formatiert gespeichert.

Es gibt eine Blob, pro Stunde, und Container aus.
  
Beispielpfad:`resourceId=/subscriptions/<subscriptionID>/resourcegroups/<resourceGroupName>/providers/microsoft.search/searchservices/<searchServiceName>/y=2015/m=12/d=25/h=01/m=00/name=PT1H.json`

### <a name="logs"></a>Protokolle

Die Protokolle Blobs enthalten Ihrer Suche Dienst Datenverkehr Protokolle.
Jede Blob weist eine Stammobjekt genannte **Datensätze** , die ein Array von Objekten Log enthält.
Jede Blob hat Datensätze auf den Vorgang aus, der während der gleichen Stunde durchgeführt wurde.

####<a name="log-schema"></a>Log-schema

Namen |Typ |Beispiel |Notizen 
------|-----|----|-----
Zeit |"DateTime" |"2015-12-07T00:00:43.6872559Z" |Zeitstempel des Vorgangs
resourceId |Zeichenfolge |"/ ABONNEMENTS/11111111-1111-1111-1111-111111111111 /<br/>RESOURCEGROUPS/STANDARD-ANBIETER /<br/> MICROSOFT. SUCHEN/SEARCHSERVICES/SEARCHSERVICE" |Ihre ResourceId
operationName |Zeichenfolge |"Query.Search" |Der Name des Vorgangs
operationVersion |Zeichenfolge |"2015-02-28"|Die verwendete api-version
Kategorie |Zeichenfolge |"OperationLogs" |Konstante 
ResultType-Wert |Zeichenfolge |"Erfolg" |Mögliche Werte: zur erfolgreichen oder nicht 
resultSignature |Ganzzahl |200 |HTTP-Ergebniscode 
durationMS |Ganzzahl |50 |Dauer des Vorgangs in Millisekunden 
Eigenschaften |Objekt |in der folgenden Tabelle finden Sie unter |Objekt, spezifische Daten enthalten

####<a name="properties-schema"></a>Eigenschaftsschema

|Namen |Typ |Beispiel |Notizen|
|------|-----|----|-----|
|Beschreibung|Zeichenfolge |"GET-/indexes('content')/docs" |Endpunkt des Vorgangs |
|Abfrage |Zeichenfolge |"? Suchen = AzureSearch & $count = WAHR & api-Version 2015-02-28 =" |Die Abfrageparameter |
|Dokumenten |Ganzzahl |42 |Anzahl der Dokumente verarbeitet|
|IndexName |Zeichenfolge |"Testindex"|Name des Indexes mit dem Vorgang zugeordnet ist |

### <a name="metrics"></a>Kennzahlen

Die Kennzahlen Blobs enthalten aggregierte Werte für Ihre Suchdienst. Jede Datei weist einen Stammobjekt genannte **Datensätze** , die ein Array von metrischen Objekte enthält. Dieses Stammobjekt enthält Kennzahlen für jede Minute für die Daten verfügbar waren. 

Verfügbare statistische Werte:

- SearchLatency: Anzeigedauer den Suchdienst Suchabfragen pro Minute zusammengefasster Verarbeitungszeit erforderlich.
- SearchQueriesPerSecond: Zahl der Suche zusammengefasster pro Sekunde empfangene Abfragen pro Minute.
- ThrottledSearchQueriesPercentage: Den Prozentsatz der Suchabfragen, die pro Minute zusammengefasster gedrosselt wurden.

> [AZURE.IMPORTANT] Begrenzungsebene tritt auf, wenn zu viele Abfragen verbraucht des Diensts bereitgestellte Ressourcenkapazität gesendet werden, aus. Sollten Sie weiteren Kopien dem Dienst hinzufügen.

####<a name="metrics-schema"></a>Kennzahlen schema

|Namen |Typ |Beispiel |Notizen|
|------|-----|----|-----|
|resourceId |Zeichenfolge |"/ ABONNEMENTS/11111111-1111-1111-1111-111111111111 /<br/>RESOURCEGROUPS/STANDARD-ANBIETER /<br/>MICROSOFT. SUCHEN/SEARCHSERVICES/SEARCHSERVICE"  |Ihre Ressource-id |
|metricName |Zeichenfolge |"Wartezeit" |der Name der Metrik |
|Zeit|"DateTime" |"2015-12-07T00:00:43.6872559Z" |der Vorgang des timestamp |
|Mittelwert |Ganzzahl |64|Mittelwert der mittlere Wert der unformatierten Beispiele in den metrischen Zeitraum |
|Minimalwert |Ganzzahl |37 |Den kleinsten Wert der unformatierten Beispiele in den metrischen Zeitraum |
|maximale |Ganzzahl |78 |Der maximale Wert der unformatierten Beispiele in den metrischen Zeitraum |
|Summe |Ganzzahl |258 |Der Gesamtwert der unformatierten Beispiele in den metrischen Zeitraum |
|zählen |Ganzzahl |4 |Die Anzahl der unformatierten Beispiele verwendet, um die Metrik generieren |
|timegrain |Zeichenfolge |"PT1M" |Die Uhrzeit Körner, die Metrik in ISO 8601|

Alle Kriterien werden in Intervallen von einer Minute gemeldet. Jeder Metrisch macht minimum, maximum und Durchschnitt Werte pro Minute verfügbar.

Für die Metrik SearchQueriesPerSecond ist Minimalwert den niedrigsten Wert für Suchabfragen pro Sekunde, die während dieser Minute registriert wurde. Dasselbe gilt für den größten Wert. Mittelwert, ist das Aggregat über die gesamte Minute. Überlegen Sie dieses Szenario während einer Minute: eine Sekunde hoch laden den Höchstwert d. h. für SearchQueriesPerSecond, gefolgt von 58 Sekunden durchschnittliche Auslastung und schließlich eine Sekunde mit nur eine Abfrage, welche ist die minimale.

Für ThrottledSearchQueriesPercentage, minimum, maximum, Mittelwert und Summe, alle denselben Wert aufweisen: der Prozentsatz der Suchabfragen, die aus der Gesamtzahl der Suchabfragen während einer Minute gedrosselt wurden.

## <a name="analyzing-your-data"></a>Analysieren Ihrer Daten

Die Daten in Ihrem eigenen Speicherkonto ist, und wir empfehlen Ihnen, diese Daten in die Art und Weise, die am besten für den Fall, untersuchen.

Es wird empfohlen, als Ausgangspunkt [Power BI](https://powerbi.microsoft.com) zum Durchsuchen und Visualisieren von Daten verwenden. Können Sie problemlos mit Ihrem Konto der Azure-Speicher verbinden und Analysieren Ihrer Daten schnell zu starten. 

#### <a name="power-bi-online"></a>Power BI Online

[Power BI-Inhalte Pack](https://app.powerbi.com/getdata/services/azure-search): Erstellen einer Power BI-Dashboard und eine Reihe von Power BI-Berichte, die automatisch anzeigen von Daten und ermöglichen visual Einblicke zu Suchdienst. Finden Sie die [Inhalte Pack Seite helfen](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-search/).

![][4]

#### <a name="power-bi-desktop"></a>Power BI-Desktop

[Power BI-Desktop](https://powerbi.microsoft.com/en-us/desktop): Untersuchen von Daten und Ihrer eigenen Visualisierungen für Ihre Daten erstellen. Die Abfrage Starter im folgenden Abschnitt finden Sie unter:

1. Öffnen eines neuen PowerBI Desktop-Berichts
2. Select Abrufen von Daten -> Weitere...

    ![][5]

3. Wählen Sie aus Microsoft Azure BLOB-Speicher und verbinden

    ![][6]

4. Geben Sie den Namen und Kontoschlüssel Ihres Kontos Speicher
5. Wählen Sie "Einblicke Protokolle Operationlogs" und "Einsichten-Kennzahlen-pt1m", und klicken Sie dann auf Bearbeiten.
6. Wenn der Abfrage-Editor geöffnet wird, stellen Sie sicher, dass "Einblicke Protokolle Operationlogs" auf der linken Seite ausgewählt ist. Jetzt öffnen-die Erweiterter Editor, indem Sie Ansicht auswählen > Erweiterter Editor

    ![][7]

7. Behalten Sie die ersten beiden Zeilen bei aus, und Ersetzen Sie den Rest durch die folgende Abfrage:

    >     #"insights-logs-operationlogs" = Source{[Name="insights-logs-operationlogs"]}[Data],
    >     #"Sorted Rows" = Table.Sort(#"insights-logs-operationlogs",{{"Date modified", Order.Descending}}),
    >     #"Kept First Rows" = Table.FirstN(#"Sorted Rows",744),
    >     #"Removed Columns" = Table.RemoveColumns(#"Kept First Rows",{"Name", "Extension", "Date accessed", "Date modified", "Date created", "Attributes", "Folder Path"}),
    >     #"Parsed JSON" = Table.TransformColumns(#"Removed Columns",{},Json.Document),
    >     #"Expanded Content" = Table.ExpandRecordColumn(#"Parsed JSON", "Content", {"records"}, {"records"}),
    >     #"Expanded records" = Table.ExpandListColumn(#"Expanded Content", "records"),
    >     #"Expanded records1" = Table.ExpandRecordColumn(#"Expanded records", "records", {"time", "resourceId", "operationName", "operationVersion", "category", "resultType", "resultSignature", "durationMS", "properties"}, {"time", "resourceId", "operationName", "operationVersion", "category", "resultType", "resultSignature", "durationMS", "properties"}),
    >     #"Expanded properties" = Table.ExpandRecordColumn(#"Expanded records1", "properties", {"Description", "Query", "IndexName", "Documents"}, {"Description", "Query", "IndexName", "Documents"}),
    >     #"Renamed Columns" = Table.RenameColumns(#"Expanded properties",{{"time", "Datetime"}, {"resourceId", "ResourceId"}, {"operationName", "OperationName"}, {"operationVersion", "OperationVersion"}, {"category", "Category"}, {"resultType", "ResultType"}, {"resultSignature", "ResultSignature"}, {"durationMS", "Duration"}}),
    >     #"Added Custom2" = Table.AddColumn(#"Renamed Columns", "QueryParameters", each Uri.Parts("http://tmp" & [Query])),
    >     #"Expanded QueryParameters" = Table.ExpandRecordColumn(#"Added Custom2", "QueryParameters", {"Query"}, {"Query.1"}),
    >     #"Expanded Query.1" = Table.ExpandRecordColumn(#"Expanded QueryParameters", "Query.1", {"search", "$skip", "$top", "$count", "api-version", "searchMode", "$filter"}, {"search", "$skip", "$top", "$count", "api-version", "searchMode", "$filter"}),
    >     #"Removed Columns1" = Table.RemoveColumns(#"Expanded Query.1",{"OperationVersion"}),
    >     #"Changed Type" = Table.TransformColumnTypes(#"Removed Columns1",{{"Datetime", type datetimezone}, {"ResourceId", type text}, {"OperationName", type text}, {"Category", type text}, {"ResultType", type text}, {"ResultSignature", type text}, {"Duration", Int64.Type}, {"Description", type text}, {"Query", type text}, {"IndexName", type text}, {"Documents", Int64.Type}, {"search", type text}, {"$skip", Int64.Type}, {"$top", Int64.Type}, {"$count", type logical}, {"api-version", type text}, {"searchMode", type text}, {"$filter", type text}}),
    >     #"Inserted Date" = Table.AddColumn(#"Changed Type", "Date", each DateTime.Date([Datetime]), type date),
    >     #"Duplicated Column" = Table.DuplicateColumn(#"Inserted Date", "ResourceId", "Copy of ResourceId"),
    >     #"Split Column by Delimiter" = Table.SplitColumn(#"Duplicated Column","Copy of ResourceId",Splitter.SplitTextByEachDelimiter({"/"}, null, true),{"Copy of ResourceId.1", "Copy of ResourceId.2"}),
    >     #"Changed Type1" = Table.TransformColumnTypes(#"Split Column by Delimiter",{{"Copy of ResourceId.1", type text}, {"Copy of ResourceId.2", type text}}),
    >     #"Removed Columns2" = Table.RemoveColumns(#"Changed Type1",{"Copy of ResourceId.1"}),
    >     #"Renamed Columns1" = Table.RenameColumns(#"Removed Columns2",{{"Copy of ResourceId.2", "ServiceName"}}),
    >     #"Lowercased Text" = Table.TransformColumns(#"Renamed Columns1",{{"ServiceName", Text.Lower}}),
    >     #"Added Custom" = Table.AddColumn(#"Lowercased Text", "DaysFromToday", each Duration.Days(DateTimeZone.UtcNow() - [Datetime])),
    >     #"Changed Type2" = Table.TransformColumnTypes(#"Added Custom",{{"DaysFromToday", Int64.Type}})
    >     in
    >     #"Changed Type2"

8. Klicken Sie auf Fertig

9. Wählen Sie jetzt "Einsichten-Kennzahlen-pt1m" aus der müssen Sie die Abfragen auf die Links, und Öffnen der erweiterte Editor erneut. Behalten Sie die ersten beiden Zeilen bei aus, und Ersetzen Sie den Rest durch die folgende Abfrage: 

    >     #"insights-metrics-pt1m1" = Source{[Name="insights-metrics-pt1m"]}[Data],
    >     #"Sorted Rows" = Table.Sort(#"insights-metrics-pt1m1",{{"Date modified", Order.Descending}}),
    >     #"Kept First Rows" = Table.FirstN(#"Sorted Rows",744),
        #"Removed Columns" = Table.RemoveColumns(#"Kept First Rows",{"Name", "Extension", "Date accessed", "Date modified", "Date created", "Attributes", "Folder Path"}),
    >     #"Parsed JSON" = Table.TransformColumns(#"Removed Columns",{},Json.Document),
    >     #"Expanded Content" = Table.ExpandRecordColumn(#"Parsed JSON", "Content", {"records"}, {"records"}),
    >     #"Expanded records" = Table.ExpandListColumn(#"Expanded Content", "records"),
    >     #"Expanded records1" = Table.ExpandRecordColumn(#"Expanded records", "records", {"resourceId", "metricName", "time", "average", "minimum", "maximum", "total", "count", "timeGrain"}, {"resourceId", "metricName", "time", "average", "minimum", "maximum", "total", "count", "timeGrain"}),
    >     #"Filtered Rows" = Table.SelectRows(#"Expanded records1", each ([metricName] = "Latency")),
    >     #"Removed Columns1" = Table.RemoveColumns(#"Filtered Rows",{"timeGrain"}),
    >     #"Renamed Columns" = Table.RenameColumns(#"Removed Columns1",{{"time", "Datetime"}, {"resourceId", "ResourceId"}, {"metricName", "MetricName"}, {"average", "Average"}, {"minimum", "Minimum"}, {"maximum", "Maximum"}, {"total", "Total"}, {"count", "Count"}}),
    >     #"Changed Type" = Table.TransformColumnTypes(#"Renamed Columns",{{"ResourceId", type text}, {"MetricName", type text}, {"Datetime", type datetimezone}, {"Average", type number}, {"Minimum", Int64.Type}, {"Maximum", Int64.Type}, {"Total", Int64.Type}, {"Count", Int64.Type}}),
    >         Rounding = Table.TransformColumns(#"Changed Type",{{"Average", each Number.Round(_, 2)}}),
    >     #"Changed Type1" = Table.TransformColumnTypes(Rounding,{{"Average", type number}}),
    >     #"Inserted Date" = Table.AddColumn(#"Changed Type1", "Date", each DateTime.Date([Datetime]), type date)
    >     in
        #"Inserted Date"

10. Klicken Sie auf Fertig, und wählen Sie dann schließen und Übernehmen der Registerkarte Start.

11. Die Daten für den letzten 30 Tagen ist jetzt verbraucht werden. Fortfahren und einige [Visualisierungen](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-report-view/)erstellen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Suchen die Formelsyntax und die Abfrage Parameter. Details finden Sie unter [Dokumente durchsuchen (Azure Suche REST-API)](https://msdn.microsoft.com/library/azure/dn798927.aspx) .

Erfahren Sie mehr über das Erstellen von außergewöhnlichen Berichte aus. Details finden Sie unter [Erste Schritte mit Power BI-Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/)

<!--Image references-->

[1]: ./media/search-traffic-analytics/SettingsBlade.png
[2]: ./media/search-traffic-analytics/DiagnosticsBlade.png
[3]: ./media/search-traffic-analytics/ResourceId.png
[4]: ./media/search-traffic-analytics/Dashboard.png
[5]: ./media/search-traffic-analytics/GetData.png
[6]: ./media/search-traffic-analytics/BlobStorage.png
[7]: ./media/search-traffic-analytics/QueryEditor.png

