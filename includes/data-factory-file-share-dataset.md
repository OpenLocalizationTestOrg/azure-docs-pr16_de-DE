## <a name="fileshare-dataset-type-properties"></a>Dateifreigabe Typ Datensatzeigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung finden Sie im Artikel [Datasets erstellen](../articles/data-factory/data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinie eines Datasets JSON ähneln für alle Dataset-Typen. 

Im Abschnitt **TypeProperties** unterscheidet sich für jede Art von Dataset. Es stellt Informationen, die in den Typ Dataset spezifisch sind. Im Abschnitt TypeProperties für ein Dataset vom Typ **Dateifreigabe** Dataset weist die folgenden Eigenschaften:

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Ordnerpfad | Sub-Pfad zu dem Ordner. Verwenden Sie Escapezeichen ' \ ' für Sonderzeichen in der Zeichenfolge. Beispiele finden Sie in der [Stichprobe verknüpft Dienst und Dataset Definitionen](#sample-linked-service-and-dataset-definitions) .<br/><br/>Sie können diese Eigenschaft mit **PartitionBy** Ordnerpfade auf Grundlage Segment haben kombinieren Anfangs-/Ende-Datum / Uhrzeit. | Ja
Dateiname | Geben Sie den Namen der Datei in den **Ordnerpfad** , wenn Sie die Tabelle verweisen auf eine bestimmte Datei in den Ordner soll. Wenn Sie einen beliebigen Wert für diese Eigenschaft nicht angeben, verweist die Tabelle auf alle Dateien in den Ordner.<br/><br/>Wenn FileName nicht für ein Dataset Ausgabe angegeben ist, der Namen der generierten Datei in den folgenden wäre dieses Format: <br/><br/>Daten. <Guid>txt (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | Nein
partitionedBy | PartitionedBy kann verwendet werden, um eine dynamische Ordnerpfad, Dateinamen für die Reihe Zeitdaten anzugeben. Beispielsweise Ordnerpfad parametrisierte für jede Stunde Daten. | Nein
Format | Die folgenden Arten von Format werden unterstützt: **TextFormat**, **AvroFormat**, **JsonFormat**und **OrcFormat**. Legen Sie die Eigenschaft **Typ** , klicken Sie unter Format auf einen der folgenden Werte ein. Finden Sie Details Abschnitten [TextFormat zurück, der angibt](#specifying-textformat), [AvroFormat zurück, der angibt](#specifying-avroformat), [JsonFormat angeben](#specifying-jsonformat)und [OrcFormat angeben](#specifying-orcformat) . Wenn Sie die Dateien als kopieren möchten – ist zwischen Datei-basierten Stores (binäre kopieren), können Sie den Formatabschnitt beide Definitionen Eingabe- und Dataset überspringen. | Nein
fileFilter | Geben Sie einen Filter verwendet werden soll, wählen Sie eine Teilmenge der Dateien in den Ordnerpfad statt alle Dateien ein.<br/><br/>Sind die Werte zulässig: `*` (mehrere Zeichen) und `?` (einzelnes Zeichen).<br/><br/>Beispiele für die 1:`"fileFilter": "*.log"`<br/>Beispiel 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> FileFilter gilt für eine Eingabe Dateifreigabe-Dataset. Diese Eigenschaft wird mit HDFS nicht unterstützt.  | Nein
| Komprimierung | Geben Sie den Typ und die Ebene der Komprimierung für die Daten ein. Unterstützte Datentypen sind: **GZip**, **Deflate**, und **BZip2** und unterstützten Ebenen sind: **Optimal** und **am schnellsten**. Komprimierungseinstellungen werden für Daten in **AvroFormat** oder **OrcFormat**derzeit nicht unterstützt. Weitere Informationen finden Sie unter [Unterstützung der Komprimierung](#compression-support) Abschnitt.  | Nein |
| useBinaryTransfer | Angeben, ob binäre Übertragung-Modus verwenden. True, wenn die binären Modus und falsch ASCII. Standardwert: True. Diese Eigenschaft kann nur verwendet werden, wenn vom Typ zugeordneten verknüpfte Diensttyp ist: FTP-Server. | Nein | 
 

> [AZURE.NOTE] FileName und FileFilter können nicht gleichzeitig verwendet werden.

### <a name="using-partionedby-property"></a>Verwenden die Eigenschaft partionedBy

Wie im vorherigen Abschnitt erwähnt, können Sie eine dynamische Ordnerpfad, Dateinamen für die Reihe Zeitdaten mit PartitionedBy angeben. Sie können dazu mit den Daten Factory-Makros und Systemvariablen SliceStart, SliceEnd, die für einen angegebenen Daten Segment den logischen Zeitraum angeben. 

Weitere Informationen zu Zeit Reihe Datasets, Planung und Segmente finden Sie unter [Datasets erstellen](../articles/data-factory/data-factory-create-datasets.md), [Planung & Ausführung](../articles/data-factory/data-factory-scheduling-and-execution.md)und Artikeln [Pipelines erstellen](../articles/data-factory/data-factory-create-pipelines.md) . 

#### <a name="sample-1"></a>Beispiel 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

In diesem Beispiel {Segment} wird mit dem angegebenen Wert Daten Factory System Variablen SliceStart im Format (YYYYMMDDHH) ersetzt. Die SliceStart bezieht sich zur Startzeit des Segments. Der Ordnerpfad unterscheidet sich für jedes Segment. Beispiel: Wikidatagateway/Wikisampledataout/2014100103 oder Wikidatagateway/Wikisampledataout/2014100104.

#### <a name="sample-2"></a>Beispiel 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

In diesem Beispiel werden Jahr, Monat, Tag und die Uhrzeit der SliceStart in separaten Variablen extrahiert, die von den Ordnerpfad und den Dateinamen Eigenschaften verwendet werden.
