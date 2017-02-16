<properties 
    pageTitle="Daten Factory-Funktionen und Systemvariablen | Microsoft Azure" 
    description="Enthält eine Liste der Azure-Daten Factory-Funktionen und Systemvariablen" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"
    services="data-factory"
/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---functions-and-system-variables"></a>Azure-Daten Factory - Funktionen und Systemvariablen
Dieser Artikel enthält Informationen zu den Funktionen und Variablen von Azure Daten Factory unterstützt.
  
## <a name="data-factory-system-variables"></a>Daten Factory Systemvariablen

Variablennamen | Beschreibung | Objektbereich | JSON-Umfang und Verwenden von Fällen
------------- | ----------- | ------------ | ------------------------
WindowStart | Starten des Zeitintervalls für den aktuellen Tätigkeit ein Fenster Ausführen | Aktivität | <ol><li>Geben Sie die Auswahl von Datenabfragen an. Finden Sie im Artikel [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) referenzierte Verbinder-Artikeln.</li><li>Übergeben von Parametern an Struktur Skript (Beispiel siehe oben).</li>
WindowEnd | Ende Zeitintervall für den aktuellen Tätigkeit ein Fenster Ausführen | Aktivität | identisch mit oben
SliceStart | Starten des Zeitintervalls für Daten Segment produziert werden | Aktivität<br/>DataSet | <ol><li>Geben Sie beim Arbeiten mit [Azure Blob](data-factory-azure-blob-connector.md) und [Dateisystem Datasets](data-factory-onprem-file-system-connector.md)dynamische Ordnerpfade und Dateinamen ein.</li><li>Angeben von Abhängigkeiten mit Daten Factory-Funktionen in Aktivität Eingaben Websitesammlung an.</li></ol>
SliceEnd | Ende Zeitintervall für den aktuellen Daten Segment produziert werden | Aktivität<br/>DataSet | dasselbe wie oben. 

> [AZURE.NOTE] Derzeit muss in Factory Daten mit der Terminplan genau in der Aktivität angegebenen den Zeitplan bei der Verfügbarkeit von Dataset Ausgabe angegeben übereinstimmen. Dies bedeutet, dass WindowStart, WindowEnd und SliceStart und SliceEnd immer denselben Zeitraum und ein einzelnes Ergebnis Segment zuordnen.
 
## <a name="data-factory-functions"></a>Daten Factory-Funktionen 

Sie können Funktionen in Daten Factory zusammen mit oben erwähnten Systemvariablen für folgende Zwecke verwenden:

1.  Angeben von Datenabfragen der Auswahl (Siehe Verbinder Artikeln auf [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) Artikel verweist.

    Die Syntax eine Data Factory-Funktion aufgerufen wird: ** $$ ** für die Auswahl von Datenabfragen und andere Eigenschaften in der Aktivität und Datasets.  
2. Angeben von Abhängigkeiten mit Daten Factory-Funktionen in Aktivität Eingaben Websitesammlung (siehe Beispiel oben).

    $$ ist nicht erforderlich, um anzugeben, Abhängigkeit von Ausdrücken.   

Im folgenden Beispiel wird die **SqlReaderQuery** -Eigenschaft in einer JSON-Datei auf einen Wert, der von der Funktion **Text.Format** zurückgegebene zugewiesen. In diesem Beispiel verwendet auch eine Systemvariable mit dem Namen **WindowStart**, die die Startzeit der Aktivität ausführen Fenster darstellt.
    
    {
        "Type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
    }

### <a name="functions"></a>Funktionen

Die folgende Tabelle enthält alle Funktionen in Azure Data Factory:

Kategorie | (Funktion) | Parameter | Beschreibung
-------- | -------- | ---------- | ----------- 
Zeit | AddHours(X,Y) | X DateTime <br/><br/>Tag: int | Die angegebene Zeit X hinzugefügt Y Stunden. <br/><br/>Beispiel: 9/5/2013 12:00:00 Uhr + 2 Stunden = 9/5/2013 2:00:00 PM
Zeit | AddMinutes(X,Y) | X DateTime <br/><br/>Tag: int | X hinzugefügt Y Minuten.<br/><br/>Beispiel: 9/15/2013 12:00:00 Uhr + 15 Minuten = 9/15/2013 12:15:00 Uhr
Zeit | StartOfHour(X) | X Datetime | Ruft die Startzeit für die Stunde durch die Stundenkomponente von X dargestellt werden. <br/><br/>Beispiel: StartOfHour 05:10:23 Uhr 9/15/2013 ist 9/15/2013 05:00:00 PM
Datum | AddDays(X,Y) | X DateTime<br/><br/>Tag: int | X hinzugefügt Y Tage.<br/><br/>Beispiel: 9/15/2013 12:00:00 Uhr + 2 Tage = 9/17/2013 12:00:00 Uhr
Datum | AddMonths(X,Y) | X DateTime<br/><br/>Tag: int | X hinzugefügt Y Monate.<br/><br/>Beispiel: 9/15/2013 12:00:00 Uhr + 1 Monat = 10/15/2013 12:00:00 Uhr 
Datum | AddQuarters(X,Y) | X DateTime <br/><br/>Tag: int | Fügt Y * 3 Monate bis X.<br/><br/>Beispiel: 9/15/2013 12:00:00 Uhr + 1 Quartal = 12/15/2013 12:00:00 Uhr
Datum | AddWeeks(X,Y) | X DateTime<br/><br/>Tag: int | Fügt Y * 7 Tage bis X<br/><br/>Beispiel: 9/15/2013 12:00:00 Uhr + 1 Woche = 9/22/2013 12:00:00 Uhr
Datum | AddYears(X,Y) | X DateTime<br/><br/>Tag: int | X hinzugefügt Y Jahre.<br/><br/>Beispiel: 9/15/2013 12:00:00 Uhr + 1 Jahr = 9/15/2014 12:00:00 Uhr
Datum | Day(X) | X DateTime | Ruft die Komponente für Tage der X ab.<br/><br/>Beispiel: Tag 9/15/2013 12:00:00 Uhr 9 ist. 
Datum | DayOfWeek(X) | X DateTime | Ruft den Tag der Wochentagkomponente von X ab.<br/><br/>Beispiel: Wochentag 9/15/2013 12:00:00 Uhr Sonntag ist.
Datum | DayOfYear(X) | X DateTime | Ruft den Tag des Jahres durch die Jahreskomponente von X dargestellt werden.<br/><br/>Beispiele:<br/>1/12/2015: 335 Wochentag 2015<br/>12/31/2015: 365 Wochentag 2015<br/>12/31/2016: 366 Wochentag 2016 (Sprung Jahr)
Datum | DaysInMonth(X) | X DateTime | Ruft die Tage Monats, dargestellt durch die Monatskomponente des Parameters X ab.<br/><br/>Beispiel: DaysInMonth von 9/15/2013 sind 30, da in den Monat September 30 Tage vorhanden sind.
Datum | EndOfDay(X) | X DateTime | Ruft Datum und Uhrzeit, die am Ende des Tages (Tagkomponente) von X darstellt.<br/><br/>Beispiel: EndOfDay 05:10:23 Uhr 9/15/2013 ist 9/15/2013 23:59:59 Uhr.
Datum | EndOfMonth(X) | X DateTime | Ruft Ende des Monats, dargestellt durch Monatskomponente der Parameter X ab. <br/><br/>Beispiel: EndOfMonth 05:10:23 Uhr 9/15/2013 ist 9/30/2013 23:59:59 Uhr (Uhrzeit, die am Ende des Monats September darstellt)
Datum | StartOfDay(X) | X DateTime | Ruft den Anfang des Tages durch die Komponente für Tage der Parameter X dargestellt werden.<br/><br/>Beispiel: StartOfDay 05:10:23 Uhr 9/15/2013 ist 9/15/2013 12:00:00 ein.
"DateTime" | FROM(X) | X-Zeichenfolge | Zeichenfolge X eine Uhrzeit zu analysieren.
"DateTime" | Ticks(X) | X DateTime | Ruft die Teilstriche-Eigenschaft des Parameters X ab. Eine Zeiteinheit gleich 100 Nanosekunden. Der Wert dieser Eigenschaft stellt die Anzahl der Teilstriche, die seit 12:00:00 Uhr, 1. Januar 0001 verstrichen sind. 
Text | Format(X) | X Zeichenfolge-variable | Der Text formatiert.

#### <a name="textformat-example"></a>Beispiel für Text.Format

    "defines": { 
        "Year" : "$$Text.Format('{0:yyyy}',WindowStart)",
        "Month" : "$$Text.Format('{0:MM}',WindowStart)",
        "Day" : "$$Text.Format('{0:dd}',WindowStart)",
        "Hour" : "$$Text.Format('{0:hh}',WindowStart)"
    }

Finden Sie unter [Benutzerdefiniert Datum und Uhrzeit-Formatzeichenfolgen](https://msdn.microsoft.com/library/8kb3ddd4.aspx) -Thema, unterschiedliche Formatierungsoptionen beschrieben, die Sie verwenden können (zum Beispiel: JJ im Vergleich zu JJJJ). 

> [AZURE.NOTE] Wenn Sie eine Funktion innerhalb einer anderen Funktion verwenden zu können, müssen Sie nicht verwenden **$$** Präfix für die innere Funktion. Beispiel: $$Text.Format ('PartitionKey Eq \\' My_pkey_filter_value\\' und RowKey Seite \\' {0:yyyy-MM-TT HH: mm:}\\'', Time.AddHours (SliceStart -6)). In diesem Beispiel, beachten Sie, dass **$$** Präfix ist nicht für die Funktion **Time.AddHours** verwendet. 
  

