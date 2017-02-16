<properties
   pageTitle="Laden Sie die Daten aus einer CSV-Datei in Azure SQL-Databaase (Bcp) | Microsoft Azure"
   description="Für eine kleine Datengröße verwendet Bcp zum Importieren von Daten in SQL Azure-Datenbank aus."
   services="sql-database"
   documentationCenter="NA"
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/13/2016"
   ms.author="carlrab"/>


# <a name="load-data-from-csv-into-azure-sql-data-warehouse-flat-files"></a>Laden Sie die Daten aus CSV in Azure SQL-Data Warehouse (flachen Dateien)

Bcp-Befehlszeilenprogramm können Daten aus einer CSV-Datei in SQL Azure-Datenbank importieren.

## <a name="before-you-begin"></a>Vorbemerkung

### <a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm durchgehen, müssen Sie folgende Aktionen ausführen:

- Zuweisen einer logischen Azure SQL-Datenbank-Server und Datenbank
- Bcp-Befehlszeilenprogramm installiert
- Das Sqlcmd Befehlszeilendienstprogramm installiert

Sie können die Dienstprogramme Bcp und Sqlcmd vom [Microsoft Download Center][]herunterladen.

### <a name="data-in-ascii-or-utf-16-format"></a>Daten in ASCII- oder UTF-16-format

Wenn Sie in diesem Lernprogramm für Ihre eigenen Daten versuchen, müssen die Daten ASCII- oder UTF-16-Codierung verwendet, da Bcp UTF-8 nicht unterstützt. 

## <a name="1-create-a-destination-table"></a>1. erstellen Sie 1. eine Zieltabelle

Definieren einer Tabelle in der SQL-Datenbank wie die Zieltabelle. Die Spalten in der Tabelle müssen die Daten in jeder Zeile in der Datendatei entsprechen.

So erstellen Sie eine Tabelle, öffnen Sie ein Eingabeaufforderungsfenster und sqlcmd.exe verwenden, um den folgenden Befehl ausführen:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
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


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie eine SQL Server-Datenbank migrieren, finden Sie unter [Migration von SQL Server-Datenbank](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
