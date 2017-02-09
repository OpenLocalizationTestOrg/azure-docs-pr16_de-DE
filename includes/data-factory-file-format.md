## <a name="specifying-formats"></a>Angeben von Formaten

Azure Data Factory unterstützt die folgenden Formattypen: 

- [Formatieren von Text](#specifying-textformat)
- [JSON-Format](#specifying-jsonformat)
- [Avro formatieren](#specifying-avroformat)
- [ORC formatieren](#specifying-orcformat)
- [Parquet formatieren](#specifying-parquetformat)

### <a name="specifying-textformat"></a>Angeben von TextFormat

Wenn das Format **TextFormat**festgelegt ist, können Sie die folgenden **optionalen** Eigenschaften im Abschnitt **Format** angeben.

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------- | -------- | 
| columnDelimiter | Das Zeichen zum Trennen von Spalten in einer Datei verwendet. | Nur ein Zeichen zulässig ist. Der Standardwert ist Komma (','). <br/><br/>Wenn ein Unicode-Zeichen verwenden möchten, finden Sie unter [Unicode-Zeichen](https://en.wikipedia.org/wiki/List_of_Unicode_characters) in den entsprechenden Code dafür zu erhalten. Beispielsweise können Sie eine seltenen nicht druckbaren Zeichen verwenden, die in den Daten wahrscheinlich nicht vorhanden berücksichtigen: Geben Sie "\u0001" das Starten von Überschrift (SOH) darstellt. | Nein |
| rowDelimiter | Das Zeichen verwendet, um Zeilen in einer Datei zu trennen. | Nur ein Zeichen zulässig ist. Der Standardwert ist eine der folgenden Werte auf gelesen: ["\r\n", "\r", "\n"] und "\r\n" auf schreiben. | Nein |
| escapeChar | Das Sonderzeichen zum Umwandeln einer Spaltentrennzeichen in den Inhalt der eingegebenen Datei Escapezeichen verwendet. <br/><br/>Sie können keine sowohl EscapeChar und QuoteChar für eine Tabelle angeben. | Nur ein Zeichen zulässig ist. Kein Standardwert. <br/><br/>Beispiel: Wenn Sie Komma haben (', ') als das Spaltentrennzeichen, aber Sie haben die Komma Text möchten (Beispiel: "Hallo, Welt"), Sie können '$' als Escapezeichen definieren und verwenden Sie die Zeichenfolge "$Hallo, Welt" in der Quelle. | Nein | 
| quoteChar | Das Zeichen verwendet, um einen Zeichenfolgenwert Angebot. Die Spalten- und Trennzeichen innerhalb der Angebot Zeichen würde als Teil der Zeichenfolgenwert behandelt werden. Diese Eigenschaft ist sowohl Eingabe- und Datasets anwendbar.<br/><br/>Sie können keine sowohl EscapeChar und QuoteChar für eine Tabelle angeben. | Nur ein Zeichen zulässig ist. Kein Standardwert. <br/><br/>Beispielsweise, stehen Ihnen Komma (', ') als das Spaltentrennzeichen aber Komma im Text haben möchten (Beispiel: < Hallo, Welt >), können Sie definieren "(Anführungszeichen) als das Angebot Zeichen und die Verwendung der Zeichenfolge"Hallo, Welt"in der Quelle. | Nein |
| nullValue | Ein oder mehrere Zeichen verwendet, um einen Nullwert darzustellen. | Ein oder mehrere Zeichen. Die angezeigten Werte sind "\N" und "NULL" gelesen "und"\N"auf schreiben. | Nein |
| encodingName | Geben Sie den codieren an. | Einen gültigen Codierung ein. Siehe [Encoding.EncodingName-Eigenschaft](https://msdn.microsoft.com/library/system.text.encoding.aspx). Beispiel: Windows-1250 oder Shift_jis. Der Standardwert ist UTF-8. | Nein | 
| firstRowAsHeader | Gibt an, ob die erste Zeile als einer Kopfzeile zu berücksichtigen. Für eine Eingabe-Dataset liest Daten Factory erste Zeile als eine Kopfzeile ein. Für ein Dataset Ausgabe schreibt Daten Factory erste Zeile als eine Kopfzeile ein. <br/><br/>Beispielszenarien finden Sie unter [Szenarien für die Verwendung von **FirstRowAsHeader** und **SkipLineCount** ](#scenarios-for-using-firstrowasheader-and-skiplinecount) . | WAHR<br/>Falsch (Standard) | Nein |
| skipLineCount | Gibt die Anzahl von Zeilen zu überspringen, wenn das Lesen von Daten aus von Dateien an. Wenn sowohl SkipLineCount und FirstRowAsHeader angegeben sind, werden zuerst die Zeilen übersprungen, und klicken Sie dann die Kopfzeileninformationen aus eingegebenen Datei gelesen. <br/><br/>Beispielszenarien finden Sie unter [Szenarien für die Verwendung von FirstRowAsHeader und SkipLineCount](#scenarios-for-using-firstrowasheader-and-skiplinecount) . | Ganze Zahl | Nein | 
| treatEmptyAsNull | Gibt an, ob null oder eine leere Zeichenfolge als null-Wert beim Lesen von Daten aus einer Datei Eingabewerte zu behandeln. | True (Standard)<br/>Falsch | Nein |  

#### <a name="textformat-example"></a>Beispiel für TextFormat
Im folgende Beispiel werden einige der Formateigenschaften für TextFormat.

    "typeProperties":
    {
        "folderPath": "mycontainer/myfolder",
        "fileName": "myblobname"
        "format":
        {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";",
            "quoteChar": "\"",
            "NullValue": "NaN"
            "firstRowAsHeader": true,
            "skipLineCount": 0,
            "treatEmptyAsNull": true
        }
    },

Um eine EscapeChar statt QuoteChar verwenden, ersetzen Sie die Zeile mit QuoteChar mit den folgenden EscapeChar aus:

    "escapeChar": "$",



### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>Szenarien für die Verwendung von FirstRowAsHeader und skipLineCount

- Sie sind in eine Textdatei aus einer Quelle nicht Datei kopieren und eine Kopfzeile mit den Schemametadaten hinzufügen möchten (zum Beispiel: SQL-Schema). Geben Sie als WAHR im Dataset Ausgabe für dieses Szenario **FirstRowAsHeader** an. 
- Eine Textdatei mit einer Kopfzeile zu einem Empfänger nicht Datei kopieren, und legen Sie die Linie möchten. Geben Sie als WAHR in der Eingabe-Dataset **FirstRowAsHeader** an.
- Sie sind aus einer Textdatei kopieren und ein paar Zeilen am Anfang überspringen, die keine Daten oder Kopfzeile Informationen enthalten soll. Geben Sie **SkipLineCount** , um die Anzahl der Zeilen übersprungen anzugeben. Wenn der Rest der Datei eine Kopfzeile enthält, können Sie auch **FirstRowAsHeader**angeben. Sowohl **SkipLineCount** und **FirstRowAsHeader** angegeben, werden zuerst die Zeilen übersprungen, und klicken Sie dann die Kopfzeileninformationen aus der Eingabewerte Datei gelesen wird

### <a name="specifying-avroformat"></a>Angeben von AvroFormat
Wenn Sie das Format auf AvroFormat festgelegt ist, müssen Sie keine Eigenschaften im Abschnitt "Format" in dem Abschnitt TypeProperties angeben. Beispiel:

    "format":
    {
        "type": "AvroFormat",
    }

Wenn in eine strukturtabelle Avro Format verwenden möchten, können Sie auf [Lernprogramm Apache Struktur](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe)verweisen.

Beachten Sie die folgenden Punkte:  

- [Komplexe Datentypen](http://avro.apache.org/docs/current/spec.html#schema_complex) werden nicht unterstützt (Einträge, Enumerationen, Arrays, Karten, Unions und feste)

### <a name="specifying-jsonformat"></a>Angeben von JsonFormat

Wenn Sie das Format auf **JsonFormat**festgelegt ist, können Sie die folgenden **optionalen** Eigenschaften im Abschnitt **Format** angeben.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| filePattern | Geben Sie an die Muster der Daten in den einzelnen JSON-Dateien gespeichert. Sind die Werte zulässig: **SetOfObjects** und **ArrayOfObjects**. Ist **der Standardwert** : **SetOfObjects**. Finden Sie unter folgenden Abschnitte ausführliche Informationen zu diesen Mustern.| Nein |
| encodingName | Geben Sie den codieren an. Finden Sie in der Liste der gültige Codierung Namen: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) Eigenschaft. Beispiel: Windows-1250 oder Shift_jis. Ist **der Standardwert** : **UTF-8**. | Nein | 
| nestingSeparator | Zeichen, das zum Trennen von Verschachtelungsebenen verwendet wird. Der Standardwert ist "." (Punkt). | Nein | 


#### <a name="setofobjects-file-pattern"></a>SetOfObjects Dateimuster

Jede Datei enthält einzelnes Objekt oder eine Linie-getrennt/verkettet mehrere Objekte. Wenn in einem Dataset Ausgabe diese Option ausgewählt ist, führt kopieren Aktivität eine einzelne JSON-Datei mit jedem Objekt pro Zeile (Zeile-getrennt).

**einzelnes Objekt** 

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }

**Zeile getrennt JSON** 

    {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
    {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
    {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"789037573","callingnum2":"789044691","switch1":"UK","switch2":"Australia"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789044691","switch1":"US","switch2":"Australia"}

**verkettete JSON**

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }
    {
        "time": "2015-04-29T07:13:21.0220000Z",
        "callingimsi": "466922202613463",
        "callingnum1": "123436380",
        "callingnum2": "789037573",
        "switch1": "US",
        "switch2": "UK"
    }
    {
        "time": "2015-04-29T07:13:21.4370000Z",
        "callingimsi": "466923101048691",
        "callingnum1": "678901578",
        "callingnum2": "345626404",
        "switch1": "Germany",
        "switch2": "UK"
    }


#### <a name="arrayofobjects-file-pattern"></a>ArrayOfObjects Dateimuster. 

Jede Datei enthält ein Array von Objekten. 

    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "789037573",
            "callingnum2": "789044691",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789044691",
            "switch1": "US",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:24.2120000Z",
            "callingimsi": "466922201102759",
            "callingnum1": "345698602",
            "callingnum2": "789097900",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:25.6320000Z",
            "callingimsi": "466923300236137",
            "callingnum1": "567850552",
            "callingnum2": "789086133",
            "switch1": "China",
            "switch2": "Germany"
        }
    ]

### <a name="jsonformat-example"></a>Beispiel für JsonFormat

Wenn Sie eine JSON-Datei mit dem folgenden Inhalt haben:  

    {
        "Id": 1,
        "Name": {
            "First": "John",
            "Last": "Doe"
        },
        "Tags": ["Data Factory”, "Azure"]
    }

und Sie es in einer SQL Azure-Tabelle in folgendem Format kopieren möchten: 

ID  | Name.First | Name.Middle | Name.Last | Kategorien
--- | ---------- | ----------- | --------- | ----
1 | Johann | NULL | Doe | ["Data Factory", "Azure"]

Das Eingabe-Dataset mit JsonFormat Typ ist wie folgt definiert: (teilweise Definition mit nur die relevanten Teile)

    "properties": {
        "structure": [
            {"name": "Id", "type": "Int"},
            {"name": "Name.First", "type": "String"},
            {"name": "Name.Middle", "type": "String"},
            {"name": "Name.Last", "type": "String"},
            {"name": "Tags", "type": "string"}
        ],
        "typeProperties":
        {
            "folderPath": "mycontainer/myfolder",
            "format":
            {
                "type": "JsonFormat",
                "filePattern": "setOfObjects",
                "encodingName": "UTF-8",
                "nestingSeparator": "."
            }
        }
    }

Wenn die Struktur nicht definiert ist, wird die Aktivität kopieren die Struktur standardmäßig reduziert und jedes Objekt kopieren. 

#### <a name="supported-json-structure"></a>Unterstützte JSON-Struktur
Beachten Sie die folgenden Punkte: 

- Eine Zeile mit Daten in einem tabellarischen Format wird jedes Objekt mit einer Sammlung von Name/Wert-Paare zugeordnet. Objekte geschachtelt werden können, und können Sie definieren, wie Sie die Struktur in einem Dataset standardmäßig mit dem schachteln Trennzeichen (.) zu reduzieren. Finden Sie im vorhergehenden Abschnitt ein Beispiel für die [JsonFormat Beispiel](#jsonformat-example) .  
- Wenn die Struktur nicht im Dataset Daten Factory definiert ist, wird die Aktivität kopieren erkennt das Schema aus dem ersten Objekt und reduzieren das gesamte Objekt. 
- Wenn die Eingabe JSON ein Array enthält, konvertiert die Aktivität kopieren den gesamten Array-Wert in einer Zeichenfolge an. Sie können auswählen, ihn mithilfe der [Spalte Zuordnung oder Filtern](#column-mapping-with-translator-rules)zu überspringen.
- Wenn es doppelte Namen auf der gleichen Ebene sind, wählt die Aktivität Kopieren der letzten.
- Eigenschaft Groß-und Kleinschreibung. Zwei Eigenschaften mit demselben Namen, aber andere Därme werden als zwei separaten Eigenschaften behandelt. 

### <a name="specifying-orcformat"></a>Angeben von OrcFormat
Wenn Sie das Format auf OrcFormat festgelegt ist, müssen Sie keine Eigenschaften im Abschnitt "Format" in dem Abschnitt TypeProperties angeben. Beispiel:

    "format":
    {
        "type": "OrcFormat"
    }

> [AZURE.IMPORTANT] Wenn Sie nicht ORC Dateien kopieren **als-ist** zwischen lokalen und Cloud Datenspeicher, müssen Sie die 8 JRE (Java Runtime-Umgebung) auf Ihrem Gatewaycomputer zu installieren. Ein 64-Bit-Gateway erfordert JRE 64-Bit- und 32-Bit-Gateway 32-Bit-JRE. Sie können beide Versionen von [hier](http://go.microsoft.com/fwlink/?LinkId=808605)suchen. Wählen Sie den richtigen Typ aus.

Beachten Sie die folgenden Punkte:

-   Komplexe Daten, Typen nicht unterstützt wird (Struktur, Karte, Liste, UNION)
-   ORC Datei umfasst drei [Optionen im Zusammenhang mit Komprimierung](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): keine ZLIB, SNAPPY. Daten Factory unterstützt das Lesen von Daten aus ORC-Datei in einem der folgenden komprimierten Formate. Es verwendet die Komprimierung Codec ist in den Metadaten, die Daten zu lesen. Jedoch wählt beim Schreiben in eine Datei ORC Daten Factory ZLIB, welche ist die Standardeinstellung für ORC. Es gibt derzeit keine Option, um dieses Verhalten zu überschreiben. 

### <a name="specifying-parquetformat"></a>Angeben von ParquetFormat
Wenn Sie das Format auf ParquetFormat festgelegt ist, müssen Sie keine Eigenschaften im Abschnitt "Format" in dem Abschnitt TypeProperties angeben. Beispiel:

    "format":
    {
        "type": "ParquetFormat"
    }

> [AZURE.IMPORTANT] Wenn Sie nicht Parquet Dateien kopieren **als-ist** zwischen lokalen und Cloud Datenspeicher, müssen Sie die 8 JRE (Java Runtime-Umgebung) auf Ihrem Gatewaycomputer zu installieren. Ein 64-Bit-Gateway erfordert JRE 64-Bit- und 32-Bit-Gateway 32-Bit-JRE. Sie können beide Versionen von [hier](http://go.microsoft.com/fwlink/?LinkId=808605)suchen. Wählen Sie den richtigen Typ aus.

Beachten Sie die folgenden Punkte:

-   Komplexe Daten werden nicht unterstützt (Karte, Liste)
-   Parquet Datei weist die folgenden Optionen im Zusammenhang mit Komprimierung: keine, SNAPPY, GZIP und LZO. Daten Factory unterstützt das Lesen von Daten aus ORC-Datei in einem der folgenden komprimierten Formate. Es wird den Komprimierungscodec zum Lesen der Daten in den Metadaten verwendet. Jedoch wählt beim Schreiben in eine Datei Parquet Daten Factory SNAPPY, welche ist die Standardeinstellung für Parquet-Format. Es gibt derzeit keine Option, um dieses Verhalten zu überschreiben. 
