<properties
   pageTitle="Laden Sie die Daten aus SQL Server in Azure SQL-Data Warehouse (PolyBase) | Microsoft Azure"
   description="Verwendet Bcp So exportieren Sie Daten aus SQL Server in flachen Dateien, AZCopy zum Importieren von Daten in Azure Blob-Speicher und PolyBase, um die Daten in Azure SQL-Data Warehouse Aufnahme an."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-azcopy"></a>Laden Sie die Daten aus SQL Server in Azure SQL-Data Warehouse (AZCopy)

Verwenden Sie Bcp und AZCopy Befehlszeile Dienstprogramme, um Daten aus SQL Server in Azure Blob-Speicher laden. Verwenden Sie dann die Daten in Azure SQL-Data Warehouse laden PolyBase oder Azure Data Factory. 


## <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm durchgehen, müssen Sie folgende Aktionen ausführen:

- Eine SQL Data Warehouse-Datenbank
- Die Bcp Befehlszeilendienstprogramm installiert
- Die SQLCMD Befehlszeilendienstprogramm installiert

>[AZURE.NOTE] Sie können die Dienstprogramme Bcp und Sqlcmd vom [Microsoft Download Center][]herunterladen.

## <a name="import-data-into-sql-data-warehouse"></a>Importieren von Daten in SQL Data Warehouse

Sie werden in diesem Lernprogramm erstellen Sie eine Tabelle in Azure SQL-Data Warehouse und Importieren von Daten in der Tabelle.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Schritt 1: Erstellen einer Tabelle in Azure SQL-Data Warehouse

Verwenden Sie über eine Befehlszeile Sqlcmd zum Ausführen der folgenden Abfrage zum Erstellen einer Tabelle auf Ihre Instanz ein:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```

>[AZURE.NOTE] Weitere Informationen zum Erstellen einer Tabelle auf SQL Data Warehouse und den verfügbaren Optionen in der WITH-Klausel finden Sie unter [Übersicht über die Tabelle][] oder [die Syntax der Tabelle erstellen][] .

### <a name="step-2-create-a-source-data-file"></a>Schritt 2: Erstellen einer Quelldatendatei

Öffnen Sie Editor und kopieren Sie die folgenden Zeilen mit Daten in eine neue Textdatei, und speichern Sie diese Datei in einem lokalen Verzeichnis temp, C:\Temp\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

> [AZURE.NOTE] Es ist wichtig, denken Sie daran, dass die bcp.exe die UTF-8-Codierung Datei nicht unterstützt. Verwenden Sie ASCII-Dateien oder UTF-16-codierte Dateien Wenn bcp.exe verwenden.

### <a name="step-3-connect-and-import-the-data"></a>Schritt 3: Verbinden und Importieren von Daten
Bcp können Sie eine Verbindung und importieren Sie die Daten mit dem folgenden Befehl ersetzen die Werte nach Bedarf:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Sie können überprüfen, dass die Daten durch Ausführen der folgenden Abfrage mit Sqlcmd geladen wurde:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Dies sollte die folgenden Ergebnisse zurück:

DateId |CalendarQuarter |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Schritt 4: Erstellen von Statistiken für Ihre Daten neu geladen

Azure SQL-Data Warehouse unterstützt noch nicht automatisch Support erstellen oder auto-Statistiken aktualisieren. Um die optimale Leistung bei Ihrer Abfragen zu erhalten, es ist wichtig, Statistiken für alle Spalten aller Tabellen, nach dem ersten Laden erstellt werden oder wesentlichen Änderungen in den Daten auftreten. Eine ausführliche Erläuterung von Statistiken finden Sie unter der [Statistik][] in der Gruppe Entwicklung von Themen. Es folgt ein kurzes Beispiel Statistiken auf die Tabelle in diesem Beispiel geladen erstellen

Führen Sie die folgenden Aussagen CREATE STATISTICS von eine Aufforderung Sqlcmd:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Exportieren von Daten aus SQL Data Warehouse
In diesem Lernprogramm erstellen Sie eine Datei aus einer Tabelle in SQL Data Warehouse. Exportieren Sie die Daten weiter oben erstellten werden wir in einer neuen Datendatei DimDate2_export.txt bezeichnet.

### <a name="step-1-export-the-data"></a>Schritt 1: Exportieren der Daten

Mit dem Programm Bcp, können Sie verbinden und Exportieren von Daten mit dem folgenden Befehl ersetzen die Werte nach Bedarf:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Sie können überprüfen, dass die Daten einwandfrei exportiert wurden, indem Sie die neue Datei zu öffnen. Die Daten in der Datei sollte mit den folgenden Text übereinstimmen:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

>[AZURE.NOTE] Aufgrund der Art des verteilten Systemen kann die Reihenfolge der Daten ist möglicherweise nicht gleich über die Data Warehouse SQL-Datenbanken. Eine weitere Möglichkeit besteht darin, verwenden die Funktion **Queryout** Bcp zum Schreiben einer Abfrage extrahiert werden, anstatt die gesamte Tabelle exportieren.

## <a name="next-steps"></a>Nächste Schritte
Übersicht über Laden finden Sie unter [Laden von Daten in SQL Data Warehouse][].
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Entwicklung von SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->

[Laden von Daten in SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[Übersicht über die Entwicklung von SQL Data Warehouse]: ./sql-data-warehouse-overview-develop.md
[Tabelle (Übersicht)]: ./sql-data-warehouse-tables-overview.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[Die Syntax der Tabelle erstellen]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
