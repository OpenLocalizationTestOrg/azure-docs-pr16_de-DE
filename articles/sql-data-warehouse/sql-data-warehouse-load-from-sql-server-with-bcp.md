<properties
   pageTitle="Laden Sie die Daten aus SQL Server in Azure SQL-Data Warehouse (Bcp) | Microsoft Azure"
   description="Für eine kleine Datengröße verwendet Bcp Daten aus SQL Server in flachen Dateien exportieren und importieren Sie die Daten direkt in Azure SQL-Data Warehouse ein."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>Laden Sie die Daten aus SQL Server in Azure SQL-Data Warehouse (flachen Dateien)

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [bcp](sql-data-warehouse-load-from-sql-server-with-bcp.md)

Für kleine Datasets können Sie Bcp-Befehlszeilenprogramm zum Exportieren von Daten aus SQL Server, und Laden Sie es direkt an Azure SQL-Data Warehouse verwenden.

In diesem Lernprogramm verwenden Sie Bcp zu:

- Exportieren einer Tabelle aus aus SQL Server mithilfe der Bcp Befehl (oder erstellen Sie eine einfache Beispieldatei)
- Importieren Sie die Tabelle aus einer Flatfile in SQL Data Warehouse.
- Erstellen Sie Statistiken, klicken Sie auf die Daten geladen.

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## <a name="before-you-begin"></a>Vorbemerkung

### <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm durchgehen, müssen Sie folgende Aktionen ausführen:

- Eine SQL Data Warehouse-Datenbank
- Bcp-Befehlszeilenprogramm installiert
- Das Sqlcmd Befehlszeilendienstprogramm installiert

Sie können die Dienstprogramme Bcp und Sqlcmd vom [Microsoft Download Center][]herunterladen.

### <a name="data-in-ascii-or-utf-16-format"></a>Daten in ASCII- oder UTF-16-format

Wenn Sie in diesem Lernprogramm für Ihre eigenen Daten versuchen, müssen die Daten ASCII- oder UTF-16-Codierung verwendet, da Bcp UTF-8 nicht unterstützt. 

PolyBase UTF-8 unterstützt, aber nicht noch UTF-16 unterstützt. Beachten Sie, dass wenn Bcp mit PolyBase kombiniert werden sollen müssen Sie transformieren die Daten in UTF-8 aus, nach dem Export aus SQL Server ist. 


## <a name="1-create-a-destination-table"></a>1. erstellen Sie 1. eine Zieltabelle

Definieren einer Tabelle in SQL Data Warehouse, die die Zieltabelle für die Last werden sollen. Die Spalten in der Tabelle müssen die Daten in jeder Zeile in der Datendatei entsprechen.

Erstellen einer Tabelle, öffnen Sie ein Eingabeaufforderungsfenster und sqlcmd.exe verwenden, um den folgenden Befehl ausführen:


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


## <a name="2-create-a-source-data-file"></a>2. Erstellen einer Quelldatendatei

Öffnen Sie Editor und kopieren Sie die folgenden Zeilen mit Daten in eine neue Textdatei, und speichern Sie diese Datei in einem lokalen Verzeichnis temp, C:\Temp\DimDate2.txt. Diese Daten werden im ASCII-Format.

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

(Optional) Um Ihre eigenen Daten aus einer SQL Server-Datenbank zu exportieren, öffnen Sie ein Eingabeaufforderungsfenster, und führen Sie den folgenden Befehl aus. Ersetzen Sie durch Ihre eigenen Daten Tabellenname, ServerName, Datenbankname, Benutzername und Kennwort ein.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-the-data"></a>3 Laden Sie 3 die Daten
Um die Daten zu laden, öffnen Sie ein Eingabeaufforderungsfenster, und führen Sie den folgenden Befehl aus, die Werte für Servername, Datenbank-Name, Benutzername und Kennwort durch Ihre eigenen Daten ersetzt werden.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Verwenden Sie diesen Befehl, um zu überprüfen, dass die Daten ordnungsgemäß geladen wurde

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Die Ergebnisse sollte wie folgt aussehen:

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

## <a name="4-create-statistics"></a>4 Statistiken erstellen

SQL Data Warehouse unterstützt noch nicht Support automatisch erstellen oder AutoUpdate-Statistik. Um die optimale abfrageleistung zu gelangen, ist es wichtig, nach dem ersten Laden oder wesentlichen Änderungen in den Daten Vorkommen Statistiken für alle Spalten aller Tabellen erstellen. Eine ausführliche Erläuterung von Statistiken finden Sie unter [Statistics][]. 

Führen Sie den folgenden Befehl zum Erstellen von Statistiken auf die Tabelle neu geladen.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5 Exportieren von Daten aus SQL Data Warehouse
Für aufbereiten können Sie die Daten exportieren, die Sie soeben wieder aus SQL Data Warehouse geladen.  Der Befehl Exportieren ist genau identisch exportieren aus SQL Server.

Es gibt jedoch eine Unterschied zwischen den Ergebnissen. Da in verteilten Standorten in SQL Data Warehouse, die Daten gespeichert werden, wenn Sie Daten exportieren schreibt jeder Knoten berechnen sie Daten in die Ausgabedatei. Die Reihenfolge der Daten in der Ausgabedatei ist zu rechnen, anders als die Reihenfolge der Daten in die Eingabe Datei.

### <a name="export-a-table-and-compare-exported-results"></a>Exportieren einer Tabelle und Vergleichen von exportierte Ergebnisse

Wenn Sie die exportierten Daten anzeigen möchten, öffnen Sie ein Eingabeaufforderungsfenster, und führen Sie diesen Befehl verwenden Ihrer eigenen Parameter. ServerName ist der Name Ihres logische Azure SQL-Servers.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Sie können überprüfen, dass die Daten einwandfrei exportiert wurden, indem Sie die neue Datei zu öffnen. Die Daten in der Datei sollte mit den folgenden Text übereinstimmen, aber wahrscheinlich in einer anderen Reihenfolge sortiert werden:

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

### <a name="export-the-results-of-a-query"></a>Exportieren Sie die Ergebnisse einer Abfrage

Die Funktion **Queryout** Bcp können Sie die Ergebnisse einer Abfrage die gesamte Tabelle exportieren exportiert werden. 

## <a name="next-steps"></a>Nächste Schritte
Übersicht über Laden finden Sie unter [Laden von Daten in SQL Data Warehouse][].
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Entwicklung von SQL Data Warehouse][].
Weitere Informationen zum Erstellen einer Tabelle auf SQL Data Warehouse finden Sie unter [Übersicht über die Tabelle][] oder [die Syntax der Tabelle erstellen][] .

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
