<properties
   pageTitle="Entwickeln Sie U-SQL-Skripts mit dem Datentools für Visual Studio | Azure"
   description="Erfahren Sie, wie Daten dem Tools für Visual Studio, so entwickeln und U-SQL-Testskripts zu installieren. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-u-sql-language"></a>Lernprogramm: Erste Schritte mit Azure Daten dem Analytics U-SQL-Sprache

U-SQL ist eine Sprache, die die Vorteile von SQL mit der ausdrucksbasierte Leistungsfähigkeit von eigenem Code zum Verarbeiten aller Daten bei einer beliebigen Skalierung vereint. U-SQL skalierbare verteilte Abfrage-Funktion können Sie effizient Analysieren von Daten im Store und über relationalen speichern, wie etwa SQL Azure-Datenbank.  Sie können den Prozess unstrukturierte Daten durch Anwenden einer Schema auf lesen, fügen Sie benutzerdefinierte Logik und des UDFs und enthält Erweiterbarkeit um fein detaillierte Kontrolle wie bei ausführen aktivieren. Erfahren Sie mehr über die zugrunde liegende U-SQL-Konzept verdeutlicht, finden Sie in [Visual Studio Blogbeitrag](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Es gibt einige Unterschiede zu ANSI SQL oder T-SQL ein. Beispielsweise verfügen über seine Schlüsselwörter wie SELECT in GROSSBUCHSTABEN sein.

Es handelt sich Typ System und Ausdruck Sprache in der select-Klausel, wo Prädikate usw. in c# sind.
Dies bedeutet die Datentypen sind die C#-Typen die Datentypen verwendet C#-NULL-Semantik, und folgen Sie den Vergleichsvorgänge innerhalb eines Prädikats C#-Syntax (z. B. einer == "Foo").  Das auch bedeutet, dass die Werte vollständige .NET Objekte, sodass Sie einfach jede Methode verwenden, um die Steuerung auf das Objekt (z.B. "f o o o". Split(' ')).

Weitere Informationen finden Sie unter [U-SQL-Referenz](http://go.microsoft.com/fwlink/p/?LinkId=691348).

###<a name="prerequisites"></a>Erforderliche Komponenten

Sie müssen abschließen [Lernprogramm: Entwickeln U-SQL-Skripts mit dem Datentools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

Im Lernprogramm haben Sie ein Projekt Daten dem Analytics mit den folgenden U-SQL-Skript ausgeführt:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();

Dieses Skript haben keine Schritte Transformation. Liest aus der Quelldatei **SearchLog.tsv**aufgerufen, es schematizes, und gibt das Rowset wieder in eine Datei namens **SearchLog-ersten-u-sql.csv**.

Beachten Sie neben den Datentyp des Felds Dauer ein Fragezeichen im Feld ein. Dies bedeutet, dass das Feld "Dauer" null sein könnte.

Einige grundlegende Konzepte und das Skript-Schlüsselwörter:

- **Rowset Variablen**: jeder Abfrageausdruck, der einen Rowset erzeugt eine Variable zugewiesen werden kann. U-SQL folgt das T-SQL-Variable naming Muster, z. B. **@searchlog** im Skript.
    Hinweis die Zuordnung jeglichen Datenausführungsverhinderung. Lediglich erhält den Namen des Ausdrucks und bietet Ihnen die Möglichkeit, Surfen ansammeln komplexere Ausdrücke.
- **EXTRAHIEREN** , gibt Ihnen die Möglichkeit, ein Schema auf gelesen definieren. Das Schema wird durch einen Spaltennamen und C#-Typ ein Paar pro Spalte angegeben. Es wird eine so genannte **extrahieren**, beispielsweise **Extractors.Tsv()** zum Extrahieren von Tsv-Dateien verwendet. Sie können benutzerdefinierte Extraktionsprogramme entwickeln.
- **Ausgabe** ein Rowsets akzeptiert und serialisiert es. Die Outputters.Csv() ausgeben eine CSV-Datei in der angegebenen Position ein. Sie können auch benutzerdefinierte Outputters entwickeln.
- Beachten Sie, dass die beiden Pfade relative Pfade sind. Sie können auch absolute Pfade verwenden.  Beispielsweise

        adl://<ADLStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Sie müssen den absoluten Pfad verwenden, Zugriff auf die Dateien in der verknüpften Speicher-Konten.  Die Syntax für Dateien, die in verknüpften Speicher Azure-Konto gespeichert ist:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure Blob-Container mit öffentlichen Blobs oder öffentlichen Container Zugriffsberechtigungen werden derzeit nicht unterstützt.

## <a name="use-scalar-variables"></a>Verwenden von skalaren Variablen

Skalare Variablen können ebenfalls Sie um Ihre Skripts Wartung zu erleichtern. Das vorherige U-SQL-Skript kann auch wie folgt geschrieben werden:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Transformieren rowsets

Verwenden Sie **Wählen Sie aus** , um Rowsets Transformieren:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

Die WHERE-Klausel [boolescher Ausdruck](https://msdn.microsoft.com/library/6a71f45d.aspx)verwendet wird. C#-Ausdruckssprache können Sie eigene Ausdrücken und Funktionen führen. Sie können auch durchführen komplexer Filtern durch Kombinieren mit logischen Konjunktionen (und) und Disjunctions (oder).

Das folgende Skript verwendet die DateTime.Parse() Methode und Konjunktion.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datatime.csv"
        USING Outputters.Csv();

Beachten Sie, dass die zweite Abfrage auf das Ergebnis des ersten Rowsets betrieben und somit das Ergebnis eine Kombination der beiden Filter ist ein. Sie können auch einen Variablennamen wiederverwenden und die Namen lexikalisch ausgelegte sind.

## <a name="aggregate-rowsets"></a>Aggregieren rowsets

U-SQL bietet Ihnen die vertrauten **ORDER BY**, **GROUP BY** und Aggregationen.

Die folgende Abfrage findet die gesamte Dauer pro Region, und klicken Sie dann im oberen Bereich 5 Dauer in Reihenfolge gibt aus.

U-SQL Rowsets beibehalten deren Reihenfolge für die nächste Abfrage nicht. Um ein Ergebnis zu sortieren, müssen Sie auf diese Weise ORDER BY die OUTPUT-Anweisung wie folgt hinzufügen:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();
    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

U-SQL ORDER BY-Klausel muss mit der Abruf-Klausel in einer SELECT-Ausdruck kombiniert werden.

Probleme U-SQL-Klausel kann verwendet werden, um die Ausgabe zu Gruppen einzuschränken, die die HAVING-Bedingung erfüllen:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

## <a name="join-data"></a>Teilnehmen an Daten

U-SQL bietet allgemeine Join-Operatoren innere Verknüpfung, links/rechts/vollständige ÄUßERE Verknüpfung, HALB Join-und für die Teilnahme an nicht nur Tabellen, sondern auf Rowsets (auch jene gefertigt aus Dateien).

Das folgende Skript verknüpft die Searchlog mit einer Ankündigung Eindruck Log und datumuhrzeitschlüssel der Werbung für die Abfragezeichenfolge für ein angegebenes Datum zurück.

    @adlog =
        EXTRACT UserId int,
                Ad string,
                Clicked int
        FROM "/Samples/Data/AdsLog.tsv"
        USING Extractors.Tsv();

    @join =
        SELECT a.Ad, s.Query, s.Start AS Date
        FROM @adlog AS a JOIN <insert your DB name>.dbo.SearchLog1 AS s
                        ON a.UserId == s.UserId
        WHERE a.Clicked == 1;

    OUTPUT @join   
        TO "/output/Searchlog-join.csv"
        USING Outputters.Csv();


U-SQL unterstützt nur die ANSI-kompatible Join-Syntax: Prädikat Rowset1 Verknüpfung Rowset2 auf. Die alte Syntax von Rowset1, wobei Rowset2 Prädikat wird nicht unterstützt.
Das Prädikat in einer Verknüpfung verfügt über eine Verknüpfung Gleichheit und kein Ausdruck sein. Wenn Sie einen Ausdruck verwenden möchten, fügen sie zu einem vorherigen Rowset select-Klausel hinzu. Wenn Sie einen anderen Vergleich Aufgaben ausführen möchten, können Sie sie in der WHERE-Klausel verschieben.


## <a name="create-databases-table-valued-functions-views-and-tables"></a>Erstellen von Datenbanken, Tabellenwertfunktionen, Ansichten und Tabellen

U-SQL können Sie Daten im Kontext einer Datenbank und Schema zu verwenden. Daher müssen Sie nicht immer aus lesen oder Schreiben von Dateien.

Jeder U-SQL-Skript wird mit einem Standarddatenbank (Master) und Standardschema (DBO) als den standardmäßigen Kontext ausgeführt. Sie können eigene Datenbank und/oder Schema erstellen. Um den Kontext zu ändern, verwenden Sie die **verwenden** -Anweisung, um den Kontext ändern.


### <a name="create-a-table-valued-function-tvf"></a>Erstellen einer Tabellenwertfunktion (Tabellenwertfunktion)

In der vorherigen U-SQL-Skript wiederholt Sie extrahiert werden, lesen in der gleichen Quelldatei verwenden. U-SQL-Tabellenwertfunktion können Sie die für eine spätere Verwendung umfassen.   

Das folgende Skript erstellt eine Tabellenwertfunktion aufgerufen *Searchlog()* in der Standarddatenbank und Schema:

    DROP FUNCTION IF EXISTS Searchlog;

    CREATE FUNCTION Searchlog()
    RETURNS @searchlog TABLE
    (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
    )
    AS BEGIN
    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();
    RETURN;
    END;

Das folgende Skript wird gezeigt, wie die Tabellenwertfunktion definiert das vorherige Skript verwenden:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM Searchlog() AS S
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/SerachLog-use-tvf.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-views"></a>Erstellen von Ansichten

Wenn Sie nur eine Abfrageausdruck, die Sie abstrakte möchten, und führen sie parametrisieren möchten verfügen, können Sie eine Ansicht anstelle einer Tabellenwertfunktion erstellen.

Das folgende Skript erstellt eine Ansicht mit der Bezeichnung *SearchlogView* in der Standarddatenbank und Schema:

    DROP VIEW IF EXISTS SearchlogView;

    CREATE VIEW SearchlogView AS  
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

Das folgende Skript zeigt die definierte Ansicht verwenden:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchlogView
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-use-view.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-tables"></a>Erstellen von Tabellen

Relationale Datenbanktabelle ähnlich, ermöglicht U-SQL zum Erstellen einer Tabelle mit einem vordefinierten Schema oder erstellen Sie eine Tabelle und das Schema aus der Abfrage, die die Tabelle (auch bekannt als CREATE TABLE AS SELECT oder CTAS) auffüllt.

Das folgende Skript Erstellen einer Datenbank und zwei Tabellen:

    DROP DATABASE IF EXISTS SearchLogDb;
    CREATE DATABASE SeachLogDb
    USE DATABASE SearchLogDb;

    DROP TABLE IF EXISTS SearchLog1;
    DROP TABLE IF EXISTS SearchLog2;

    CREATE TABLE SearchLog1 (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string,

                INDEX sl_idx CLUSTERED (UserId ASC)
                    PARTITIONED BY HASH (UserId)
    );

    INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

    CREATE TABLE SearchLog2(
        INDEX sl_idx CLUSTERED (UserId ASC)
                PARTITIONED BY HASH (UserId)
    ) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here


### <a name="query-tables"></a>Abfragetabellen

Sie können die Tabellen (erstellt in das vorherige Skript) auf die gleiche Weise wie Sie eine Abfrage über die Datendateien Abfragen. Statt ein Rowset mit EXTRAHIEREN zu erstellen, können Sie jetzt nur auf den Namen der Tabelle verweisen.

Das zuvor von Ihnen verwendete Transformation-Skript wird geändert, um die Informationen aus den Tabellen:

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchLogDb.dbo.SearchLog2
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @res
        TO "/output/Searchlog-query-table.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Beachten Sie, dass Sie derzeit nicht ausgeführt eine SELECT-Anweisung auf eine Tabelle in einem Skript das Skript werden können, in dem Sie die Tabelle erstellen.


##<a name="conclusion"></a>Abschluss

Was im Lernprogramm verdeckt ist, ist nur einen kleinen Teil des SQL-U. Aufgrund des Umfangs dieses Lernprogramms kann nicht es alles, wie behandeln:

- Verwenden Sie CROSS Entpacken Teile Zeichenfolgen, Matrizen und Karten in Zeilen anwenden.
- Steuerung partitionierte Datenmengen (Dateisätze und partitionierten Tabellen).
- Entwickeln Sie benutzerdefinierte Operatoren Extraktionsprogramme, Outputters, Prozessoren, User defined Aggregatoren in c#.
- Verwenden Sie U-SQL Windowing Funktionen.
- Verwalten von U-SQL-Code mit Ansichten, Tabellenwertfunktionen und gespeicherte Prozeduren.
- Führen Sie auf Ihrer Verarbeitung Knoten beliebigen benutzerdefinierten Code ein.
- Verbinden Sie mit Azure SQL-Datenbanken und vereinheitlichen Abfragen über sie und Ihre Daten U-SQL- und Azure Daten Lake.

## <a name="see-also"></a>Siehe auch

- [Übersicht über Microsoft Azure-Daten Lake Analytics](data-lake-analytics-overview.md)
- [Entwickeln Sie U-SQL-Skripts mit dem Datentools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Verwenden von U-SQL-Fenster für Azure Daten dem Analytics Aufträge](data-lake-analytics-use-window-functions.md)
- [Überwachen Sie und Behandeln von Problemen mit Azure Daten dem Analytics Aufträge mithilfe von Azure-Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

## <a name="let-us-know-what-you-think"></a>Teilen Sie uns Ihre Meinung

- [Fordern Sie ein feature](http://aka.ms/adlafeedback)
- [Anfordern von Hilfe in den Foren](http://aka.ms/adlaforums)
- [Feedback zu U-SQL](http://aka.ms/usqldiscuss)
