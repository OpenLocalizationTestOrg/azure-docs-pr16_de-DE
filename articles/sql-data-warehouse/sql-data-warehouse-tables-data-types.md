<properties
   pageTitle="Datentypen für Tabellen in SQL Data Warehouse | Microsoft Azure"
   description="Erste Schritte mit Datentypen für Azure SQL-Data Warehouse-Tabellen."
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
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="data-types-for-tables-in-sql-data-warehouse"></a>Datentypen für Tabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [(Übersicht)][]
- [Datentypen][]
- [Verteilen][]
- [Index][]
- [Partition][]
- [Statistik][]
- [Temporäre][]

Die am häufigsten verwendeten SQL Data Warehouse unterstützt Datentypen.  Es folgt eine Liste der Datentypen von SQL Data Warehouse unterstützt werden.  Weitere ausführliche Informationen zum Datentyp unterstützen Sie, finden Sie unter [Tabelle erstellen][].

|**Unterstützte Datentypen**|||
|---|---|---|
[bigint][]|[Dezimalzahl][]|[smallint][]|
[Binärzahl][]|[frei verschieben][]|[smallmoney][]|
[Bit][]|[Ganzzahl][]|[sysname][]|
[Zeichen][]|[Geld][]|[Zeit][]|
[Datum][]|[NCHAR][]|[tinyint][]|
["DateTime"][]|[nvarchar][]|[uniqueidentifier][]|
[datetime2][]|[Real][]|[varbinary][]|
[DateTimeOffset][]|[smalldatetime][]|[varchar][]|


## <a name="data-type-best-practices"></a>Bewährte Methoden des Datentyps

 Wenn Sie Ihre Spaltentypen definieren, werden mit den kleinsten Datentyp mit Unterstützung für Ihre Daten Abfrage Leistung. Dies ist besonders wichtig für Zeichen und VARCHAR-Spalten. Wenn der längste Wert in einer Spalte 25 Zeichen ist, können definieren Sie Ihre Spalte wie VARCHAR(25). Vermeiden Sie alle Zeichenspalten zu einem großen standardmäßige Länge definieren. Darüber hinaus definieren Sie Spalten als VARCHAR, wenn dies alles ist, die erforderlich ist, anstatt [NVARCHAR][]zu verwenden.  Verwenden Sie NVARCHAR(4000) oder vom Datentyp VARCHAR(8000) nach Möglichkeit statt NVARCHAR(MAX) oder "nvarchar(max)", "varchar(max)" an.

## <a name="polybase-limitation"></a>Polybase Einschränkung

Wenn Sie Ihre Tabellen laden Polybase verwenden, definieren Sie Tabellen, damit die maximale mögliche Zeilenlänge, einschließlich der vollständigen Länge aller Spalten variabler Länge, nicht 32.767 Bytes überschreitet.  Während Sie eine Zeile mit Daten variabler Länge definieren können, die diese Breite überschreiten und Laden von Zeilen mit BCP können, werden Sie nicht Polybase verwenden diese Daten geladen sein.  Polybase Unterstützung für Breite Zeilen wird bald hinzugefügt werden.

## <a name="unsupported-data-types"></a>Nicht unterstützte Datentypen

Wenn Sie die Datenbank aus einer anderen SQL-Plattform wie SQL Azure-Datenbank migrieren, wie Sie migrieren, können einige Datentypen auftreten, die nicht auf SQL Data Warehouse unterstützt werden.  Nachstehend sind nicht unterstützte Datentypen als auch einige Alternativen, anstelle von nicht unterstützten Datentypen verwendet werden können.

|Datentyp|Dieses Problem zu umgehen|
|---|---|
|[Geometrie][]|[varbinary][]|
|["geography"][]|[varbinary][]|
|[hierarchyid][]|[nvarchar][] (4000)|
|[Bild][ntext,text,image]|[varbinary][]|
|[Text][ntext,text,image]|[varchar][]|
|[ntext][ntext,text,image]|[nvarchar][]|
|[sql_variant][]|Spalte in mehrere stark eingegebenen Spalten aufteilen.|
|[Tabelle][]|Konvertieren Sie in temporäre Tabellen.|
|[Zeitstempel][]|Überarbeiten des Codes zum [datetime2][] verwenden und `CURRENT_TIMESTAMP` (Funktion).  Es werden nur Konstanten unterstützt als Standardeinstellungen, daher Current_timestamp kann nicht definiert werden als Default-Einschränkung. Wenn Sie eine Timestamp-Spalte eingegebenen Version Zeilenwerte migrieren müssen verwenden Sie [BINÄRE][](8) oder [VARBINARY][BINÄRE](8) für nicht NULL oder NULL Zeile Versionswerte.|
|[XML][]|[varchar][]|
|[benutzerdefinierte Inhaltstypen][]|Konvertieren Sie wieder in ihren systemeigenen Typen, soweit möglich|
|Standardwerte|Standardwerte Literalen und Konstanten nur unterstützt werden.  Nicht-deterministische Ausdrücke oder -Funktionen, wie `GETDATE()` oder `CURRENT_TIMESTAMP`, werden nicht unterstützt.|

Die folgenden SQL kann auf Ihrem aktuellen ausgeführt werden SQL-Datenbank in Spalten identifizieren die sind nicht durch Azure SQL-Data Warehouse unterstützt:

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter den Artikeln auf [Tabelle Übersicht][Übersicht], [Verteilen einer Tabelle][verteilen], [Indizieren einer Tabelle][Index], [eine-Tabelle teilen][Partition], [Tabellenstatistiken Verwalten von][Statistiken] und [Temporäre Tabellen][temporäre].  Weitere Informationen zu best Practices finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[(Übersicht)]: ./sql-data-warehouse-tables-overview.md
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[Verteilen]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistik]: ./sql-data-warehouse-tables-statistics.md
[Temporäre]: ./sql-data-warehouse-tables-temporary.md
[Bewährte Methoden für SQL Datawarehouse]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
[Tabelle erstellen]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[Binärzahl]: https://msdn.microsoft.com/library/ms188362.aspx
[Bit]: https://msdn.microsoft.com/library/ms177603.aspx
[Zeichen]: https://msdn.microsoft.com/library/ms176089.aspx
[Datum]: https://msdn.microsoft.com/library/bb630352.aspx
["DateTime"]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[DateTimeOffset]: https://msdn.microsoft.com/library/bb630289.aspx
[Dezimalzahl]: https://msdn.microsoft.com/library/ms187746.aspx
[frei verschieben]: https://msdn.microsoft.com/library/ms173773.aspx
[Geometrie]: https://msdn.microsoft.com/library/cc280487.aspx
["geography"]: https://msdn.microsoft.com/library/cc280766.aspx
[hierarchyid]: https://msdn.microsoft.com/library/bb677290.aspx
[Ganzzahl]: https://msdn.microsoft.com/library/ms187745.aspx
[Geld]: https://msdn.microsoft.com/library/ms179882.aspx
[NCHAR]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[Real]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[smallint]: https://msdn.microsoft.com/library/ms187745.aspx
[smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[sql_variant]: https://msdn.microsoft.com/library/ms173829.aspx
[sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[Tabelle]: https://msdn.microsoft.com/library/ms175010.aspx
[Zeit]: https://msdn.microsoft.com/library/bb677243.aspx
[Zeitstempel]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[uniqueidentifier]: https://msdn.microsoft.com/library/ms187942.aspx
[varbinary]: https://msdn.microsoft.com/library/ms188362.aspx
[varchar]: https://msdn.microsoft.com/library/ms186939.aspx
[XML]: https://msdn.microsoft.com/library/ms187339.aspx
[benutzerdefinierte Inhaltstypen]: https://msdn.microsoft.com/library/ms131694.aspx
