<properties
   pageTitle="Laden Sie die Beispieldaten in SQL Data Warehouse | Microsoft Azure"
   description="Laden Sie die Beispieldaten in SQL Data Warehouse"
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
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="load-sample-data-into-sql-data-warehouse"></a>Laden Sie die Beispieldaten in SQL Data Warehouse

Führen Sie diese einfache Schritte zum Laden und die Adventure Works-Beispieldatenbank Abfragen aus. Zuerst verwenden diese Skripts Sqlcmd SQL ausführen, um Tabellen und Ansichten erstellen. Nachdem Tabellen erstellt wurden, werden die Skripts Bcp verwenden, um Daten zu laden.  Wenn Sie noch Sqlcmd und Bcp installiert haben, folgen Sie diesen Links, [Bcp][] installieren und [Sqlcmd zu installieren][].

##<a name="load-sample-data"></a>Laden von Beispieldaten

1. Die [Adventure Works Stichprobe Skripts für SQL Data Warehouse][] Zip-Datei herunterladen.

2. Extrahieren Sie die Dateien aus heruntergeladenen Zip in ein Verzeichnis auf Ihrem lokalen Computer an.

3. Bearbeiten Sie der extrahierten Datei aw_create.bat, und legen Sie die folgenden Variablen am oberen Rand der Datei gefunden.  Achten Sie darauf, dass Sie keine Leerzeichen zwischen verlassen der "=" und der Parameter.  Nachstehend sind Beispiele für die Darstellung Ihrer Änderungen möglicherweise ein.

    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```

4. Führen Sie die bearbeiteten aw_create.bat aus einem Windows-Befehlszeilenprompt.  Achten Sie darauf, dass Sie sich im Verzeichnis befinden, in dem Sie Ihre bearbeiteten Version des aw_create.bat gespeichert.
Dieses Skript wird...
    * Legen Sie alle Adventure Works-Tabellen oder Sichten, die bereits in der Datenbank vorhanden sind.
    * Erstellen der Adventure Works-Tabellen und Ansichten
    * Laden Sie alle Adventure Works-Tabellen mit den bcp
    * Überprüfen Sie die Zeilenanzahl für jede Adventure Works-Tabelle
    * Erfassen von Statistiken auf jeder Spalte für jede Adventure Works-Tabelle


##<a name="query-sample-data"></a>Abfrage-Beispieldaten

Nachdem Sie einige Beispieldaten in Ihr SQL Data Warehouse geladen haben, können Sie schnell ein paar Abfragen ausgeführt werden.  Zum Ausführen einer Abfrage, Herstellen einer Verbindung mit der neu erstellten Adventure Works-Datenbank in Azure SQL-DW mit Visual Studio und SSDT, wie in der [Abfrage mit Visual Studio][] -Dokument beschrieben.

Beispiele für einfache select-Anweisung, um sämtliche Informationen der Mitarbeiter zu erhalten:

```sql
SELECT * FROM DimEmployee;
```

Beispiel für eine komplexe Abfrage über Konstrukte, wie z. B. GROUP BY die Gesamtmenge für alle Umsätze für jeden Tag einsehen:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Beispiel für eine SELECT-Anweisung mit einer WHERE-Klausel vor einem bestimmten Datum herausfiltern Bestellungen aus:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

SQL Data Warehouse unterstützt fast alle T-SQL-Konstrukte die SQL Server unterstützt.  Alle Unterschiede sind in unseren [Migrieren von Code][] -Dokumentation beschrieben.

## <a name="next-steps"></a>Nächste Schritte
Jetzt, da Sie können Sie einige Abfragen mit Beispieldaten ausprobieren geführt haben, schauen Sie sich so [entwickeln][], [Laden][]oder in SQL Data Warehouse [Migrieren][] .

<!--Image references-->

<!--Article references-->
[Migrieren von]: sql-data-warehouse-overview-migrate.md
[Entwickeln]: sql-data-warehouse-overview-develop.md
[Beim Laden]: sql-data-warehouse-overview-load.md
[Abfrage mit Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Migrieren von code]: sql-data-warehouse-migrate-code.md
[Installieren von bcp]: sql-data-warehouse-load-with-bcp.md
[Installieren von sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works Stichprobe Skripts für SQL Datawarehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
