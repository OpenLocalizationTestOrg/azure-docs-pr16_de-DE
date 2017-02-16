<properties
   pageTitle="Verwenden von Etiketten Urkunde Abfragen in SQL Data Warehouse | Microsoft Azure"
   description="Tipps zur Verwendung von Etiketten Urkunde Abfragen in Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a>Verwenden von Etiketten Urkunde Abfragen in SQL Data Warehouse
SQL Data Warehouse unterstützt das Konzept Abfrage Etiketten. Prüfen Sie vor dem Wechsel in einer beliebigen Tiefe wir ein Beispiel für eine aus:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Diese letzte Zeile tags die Zeichenfolge "Meine Abfrage Bezeichnung" auf die Abfrage. Dies ist besonders hilfreich, denn damit die Bezeichnung durch die DMVs Abfrage können ist. Dadurch wird mit einem Problem Abfragen aufzufinden und identifizieren Fortschritt über eine ETL ausführen.

Eine gute Benennungskonvention hilft wirklich hier aus. Beispielsweise wie im folgenden ' Projekt: Verfahren: Anweisung: Kommentar ' würde dazu beitragen, die Abfrage in dem gesamten Code in Datenquellen-Steuerelements eindeutig identifizieren.

Suchen Sie nach der Bezeichnung können Sie die folgende Abfrage aus, die die von dynamischen Verwaltungsansichten verwendet:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [AZURE.NOTE] Es ist wichtig, dass Sie bei der Abfrage eckigen Klammern oder die Bezeichnung Word in doppelte Anführungszeichen umbrechen. Beschriftung ist ein reserviertes Wort und wird einen Fehler verursacht, wenn Sie nicht als Trennzeichen wurden.


## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Entwicklung][].

<!--Image references-->

<!--Article references-->
[Übersicht über die Entwicklung]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
