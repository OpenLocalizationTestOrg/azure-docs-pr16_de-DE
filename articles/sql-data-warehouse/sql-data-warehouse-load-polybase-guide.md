<properties
   pageTitle="Leitfaden für die Verwendung von PolyBase in SQL Data Warehouse | Microsoft Azure"
   description="Richtlinien und Empfehlungen für PolyBase in SQL Data Warehouse Szenarien verwenden."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Leitfaden für die Verwendung von PolyBase in SQL Data Warehouse

Dieses Handbuch bietet praktische Informationen zur Verwendung von PolyBase in SQL Data Warehouse.

Um anzufangen, finden Sie unter das [Laden von Daten mit PolyBase][] Lernprogramm.


## <a name="rotating-storage-keys"></a>Drehen von Speicher Tasten

Sie werden von Zeit zu Zeit Zugriffstaste in Ihrem BLOB-Speicher aus Sicherheitsgründen ändern möchten.

Die am häufigsten elegante Methode zum Ausführen dieser Aufgabe ist ein Vorgang bekannt als "Drehen die Tasten" ausgewählt haben. Möglicherweise haben Sie bemerkt, dass Sie zwei Speicherschlüssel für Ihr Konto der Blob-Speicher haben. Dies ist, damit Sie einen Übergang ausführen kann

Drehen Ihre Azure-Speicher Konto Schlüssel ist einfachen drei Schritten

1. Erstellen der zweiten Datenbank ausgelegte Anmeldeinformationen basierend auf den sekundären Speicher Zugriffstaste
2. Erstellen Sie zweite basierend auf diese neuen Anmeldeinformationen externen Datenquelle
3. Löschen und die externen Tabelle(n) auf der neuen externen Datenquelle erstellen

Wenn Sie migriert haben alle Ihre externen Tabellen in der neuen externen Datenquelle und dann können Sie die bereinigen Aufgaben ausführen:

1. Legen Sie die erste externen Datenquelle
2. Erste Datenbank ablegen ausgelegte Anmeldeinformationen basierend auf der primären Speicher Zugriffstaste
3. Melden Sie sich bei Azure und neu generieren der primären Zugriffstaste für eine erneute Verwendung bereit.

## <a name="query-azure-blob-storage-data"></a>Abfragen von Daten Azure Blob-Speicher
Abfragen für externe Tabellen verwenden einfach den Namen der Tabelle, als ob es einer relationalen Tabelle wurde.

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [AZURE.NOTE] Der Fehler *"Abfrage abgebrochen – das Maximum ablehnen Schwellenwert beim Lesen aus einer externen Quelle erreicht wurde"*kann eine Abfrage auf einer externen Tabelle fehl. Dies zeigt an, dass die externen Daten mit *geänderter* Datensätze enthält. Ein Datensatz ist 'dirty' betrachtet, wenn die tatsächlichen Daten Typen/Anzahl von Spalten die Spaltendefinitionen der externen Tabelle nicht übereinstimmen oder die Daten in der angegebenen externen Dateiformat entsprechen nicht. Um dieses Problem zu beheben, stellen Sie sicher, dass Ihre externe Tabelle und Definitionen für externe Dateiformate richtig sind und diese Definitionen die externen Daten entspricht. Für den Fall, dass eine Untermenge der Datensätze mit externen Daten ist verschmutzt, können Sie diese Einträge für Ihre Abfragen mithilfe der Optionen ablehnen in externe Tabelle DDL erstellen Ablehnen auswählen.


## <a name="load-data-from-azure-blob-storage"></a>Laden Sie Daten aus Azure Blob-Speicher
In diesem Beispiel werden die Daten aus Azure Blob-Speicher in SQL Data Warehouse-Datenbank geladen.

Speichern von Daten direkt entfernt die Daten durchstellen Zeit für Abfragen. Speichern von Daten mit einem Index Columnstore verbessert die Leistung von Abfragen für Analysis-Abfragen nach bis zu 10 X.

In diesem Beispiel wird die CREATE TABLE AS SELECT-Anweisung, um Daten zu laden. Die neue Tabelle erbt die Spalten, die mit dem Namen in der Abfrage an. Es erbt die Datentypen der Spalten aus der externen Tabellendefinition.

CREATE TABLE AS SELECT ist hochgradig effektiv Transact-SQL-Anweisung, die die Daten parallel zu den berechnen Knoten des SQL Data Warehouse zu laden.  Es wurde für die parallele Verarbeitung (MPP)-Engine Analytics Plattform System ursprünglich entwickelt und ist nun in SQL Data Warehouse.

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

Finden Sie unter [Wählen Sie die Tabelle AS erstellen (Transact-SQL)][].

## <a name="create-statistics-on-newly-loaded-data"></a>Klicken Sie auf neu laden Daten Statistiken erstellen

Azure SQL-Data Warehouse unterstützt noch nicht automatisch Support erstellen oder auto-Statistiken aktualisieren.  Um die optimale Leistung bei Ihrer Abfragen zu erhalten, es ist wichtig, Statistiken für alle Spalten aller Tabellen, nach dem ersten Laden erstellt werden oder wesentlichen Änderungen in den Daten auftreten.  Eine ausführliche Erläuterung von Statistiken finden Sie unter der [Statistik][] in der Gruppe Entwicklung von Themen.  Es folgt ein kurzes Beispiel Statistiken auf die Tabelle in diesem Beispiel geladen erstellen.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-to-azure-blob-storage"></a>Exportieren von Daten in Azure Blob-Speicher
In diesem Abschnitt wird gezeigt, wie Daten aus SQL Data Warehouse in Azure Blob-Speicher exportieren. In diesem Beispiel wird die erstellen externe Tabelle AS SELECT also hochgradig effektiv Transact-SQL-Anweisung so exportieren Sie die Daten parallel aus allen Knoten berechnen.

Im folgenden Beispiel wird eine externe Tabelle Weblogs2014 Spaltendefinitionen und Daten aus Dbo verwenden. Blogs Tabelle. Die Definition der externen Tabelle ist in SQL Data Warehouse gespeichert, und die Ergebnisse der SELECT-Anweisung im Verzeichnis "/ Archivieren/log2014 /" von der Datenquelle angegebene Blob Container zum exportiert werden. Die Daten werden in der angegebenen Datei im Textformat exportiert.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```


## <a name="working-around-the-polybase-utf-8-requirement"></a>Um die Anforderung PolyBase UTF-8 arbeiten
Bei präsentieren PolyBase unterstützt Datendateien, die UTF-8-codierte wurden geladen. Wie UTF-8 verwendet die gleiche Zeichen Codierung wie ASCII-PolyBase wird auch Unterstützung beim Laden der Daten, die ASCII-codierte ist. Jedoch PolyBase unterstützt keine anderen Zeichen Codierung, z. B. UTF-16 / Unicode oder erweiterte ASCII-Zeichen. Erweiterte ASCII umfasst Zeichen mit Akzenten, wie etwa die Umlaut in Deutsch häufig also an.

Wenn Sie diese Anforderung umgehen, ist die beste Antwort erneut Schreibvorgangs UTF-8-Codierung ist.

Es gibt verschiedene Möglichkeiten zur Verfügung. Nachfolgend finden zwei Vorgehensweisen mithilfe der Powershell aus:

### <a name="simple-example-for-small-files"></a>Einfaches Beispiel für kleine Dateien

Es folgt eine einfache zeilenweise Powershell-Skript, die die Datei erstellt wird.

```PowerShell
Get-Content <input_file_name> -Encoding Unicode | Set-Content <output_file_name> -Encoding utf8
```

Und so eine einfache Möglichkeit sieht, um die Daten neu zu codieren ist es jedoch keinesfalls am effizientesten. Im folgenden streaming EA-Beispiel ist sehr viel schneller und dasselbe Ergebnis erzielt.

### <a name="io-streaming-example-for-larger-files"></a>EA Streaming Beispiel für größere Dateien

Im folgenden Beispiel ist komplexer, aber wie sie die Zeilen von Daten aus der Datenquelle verfügen, um es zu adressieren streamt ist sehr viel effizienter. Wenden Sie dieses Verfahren für größere Dateien ein.

```PowerShell
#Static variables
$ascii = [System.Text.Encoding]::ASCII
$utf16le = [System.Text.Encoding]::Unicode
$utf8 = [System.Text.Encoding]::UTF8
$ansi = [System.Text.Encoding]::Default
$append = $False

#Set source file path and file name
$src = [System.IO.Path]::Combine("C:\input_file_path\","input_file_name.txt")

#Set source file encoding (using list above)
$src_enc = $ansi

#Set target file path and file name
$tgt = [System.IO.Path]::Combine("C:\output_file_path\","output_file_name.txt")

#Set target file encoding (using list above)
$tgt_enc = $utf8

$read = New-Object System.IO.StreamReader($src,$src_enc)
$write = New-Object System.IO.StreamWriter($tgt,$append,$tgt_enc)

while ($read.Peek() -ne -1)
{
    $line = $read.ReadLine();
    $write.WriteLine($line);
}
$read.Close()
$read.Dispose()
$write.Close()
$write.Dispose()
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Verschieben von Daten in SQL Data Warehouse finden Sie unter [Übersicht über die Migration von Daten][].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Laden Sie die Daten mit PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md
[Übersicht über die Migration von Daten]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[Erstellen externer-DATEIFORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026) .aspx [Erstellen einer EXTERNEN Tabelle (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspxx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[Erstellen Sie die Tabelle AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
