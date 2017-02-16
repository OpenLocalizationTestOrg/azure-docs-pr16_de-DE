<properties
   pageTitle="Gespeicherten Prozeduren in SQL Data Warehouse | Microsoft Azure"
   description="Tipps für die Implementierung von gespeicherter Prozeduren in Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
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
   ms.date="06/30/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="stored-procedures-in-sql-data-warehouse"></a>Gespeicherte Prozeduren in SQL Data Warehouse

SQL Data Warehouse unterstützt viele Transact-SQL-Features in SQL Server gefunden. Es gibt wichtiger Maßstab sich bestimmte Features, die wir nutzen, um die Leistung Ihrer Lösung maximieren möchten.

Sind jedoch zum Verwalten der Skalierung und Leistung des SQL-Data Warehouse es auch einige Features und Funktionen, die enthalten Unterschieden im Verhalten und andere Personen, die nicht unterstützt werden.

In diesem Artikel wird erläutert, wie gespeicherte Prozeduren in SQL Data Warehouse implementieren.

## <a name="introducing-stored-procedures"></a>Einführung in gespeicherten Prozeduren
Gespeicherte Prozeduren eignen sich hervorragend zum SQL-Code kapseln; speichern es in der Nähe der Daten im Datawarehouse. Indem Sie den Code in verwaltbare Einheiten encapsulating Hilfe gespeicherte Prozeduren Entwickler die dazugehörigen Lösungen modularisiert; Erleichterung größer Re-usability Code. Jede gespeicherte Prozedur kann auch Parameter, um auch flexibler zu gestalten akzeptieren.

SQL Data Warehouse bietet eine vereinfachte und optimierte gespeicherte Prozedur-Implementierung. Der größte Unterschied im Vergleich zu SQL Server ist, dass die gespeicherte Prozedur nicht vorab kompilierte Code. In Lager Daten sind wir in der Regel weniger betreffenden mit der Kompilierung. Es ist wichtiger, dass der Code der gespeicherten Prozedur beim Betrieb gegen große Datenmengen ordnungsgemäß optimiert ist. Das Ziel besteht, Stunden, Minuten und Sekunden nicht Millisekunden zu speichern. Es empfiehlt sich daher mehr als Container für SQL-Logik gespeicherten Prozeduren vorstellen.     

Wenn SQL Data Warehouse Ihre gespeicherte Prozedur ausführt werden SQL-Anweisungen, übersetzten und optimiert zur Laufzeit analysiert. Bei diesem Vorgang wird jede Anweisung in verteilten Abfragen umgewandelt. Der Abfrage übermittelten unterscheidet sich der SQL-Code, der für die Daten tatsächlich ausgeführt wird.

## <a name="nesting-stored-procedures"></a>Schachtelungsebenen gespeicherten Prozeduren
Beim gespeicherte Prozeduren rufen Sie anderen gespeicherten Prozeduren oder dynamisches Sql ausführen, und klicken Sie dann die innere gespeicherte Prozedur oder Code aufrufen gesagt, dass er geschachtelt werden.

SQL Data Warehouse unterstützt maximal 8 Verschachtelungsebenen. Dies ist etwas anders als SQL Server. Die verschachteln Ebene in SQL Server beträgt 32.

Der obersten Ebene gespeicherte Prozedur Anruf entspricht, um das Verschachteln von Ebene 1

```sql
EXEC prc_nesting
```
Wenn die gespeicherte Prozedur macht auch eine andere Mitarbeiter anrufen dann erhöht dies die verschachteln Ebene 2
```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Wenn die zweite Prozedur dann einige dynamisches Sql ausgeführt wird, diese vergrößert wird, die Ebene verschachteln mit 3
```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Hinweis SQL Data Warehouse unterstützt derzeit keine @@NESTLEVEL. Sie müssen eine Ihrer Ebene verschachteln selbst nachverfolgen. Es ist wahrscheinlich nicht Sie den Ebene 8 verschachteln Grenzwert erreichen müssen Sie jedoch in diesem Fall wird erneut Code arbeiten und "reduzieren", damit er innerhalb dieses Zeitraums passt.

## <a name="insertexecute"></a>EINFÜGEN VON... AUSFÜHREN
SQL Data Warehouse lässt nicht das Resultset einer gespeicherten Prozedur mit einer INSERT-Anweisung nutzen. Es gibt jedoch eine weitere Möglichkeit, die Sie verwenden können.

Lizenzinformationen finden Sie im folgenden Artikel auf [temporäre Tabellen] für ein Beispiel dafür, wie Sie dies tun.

## <a name="limitations"></a>Einschränkungen

Es gibt einige Aspekte des gespeicherten Transact-SQL-Prozeduren, die nicht in SQL Data Warehouse implementiert werden.

Sie sind:

- temporäre gespeicherte Prozeduren
- nummerierte gespeicherte Prozeduren
- Erweiterte gespeicherte Prozeduren
- Gespeicherte CLR-Prozeduren
- Verschlüsselungsoption
- Replikationsoption
- Tabellenwertfunktionen Parameter
- schreibgeschützte Parameter
- Standardparameter
- Ausführungskontexte
- Anweisung zurückgeben

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Entwicklung][].

<!--Image references-->

<!--Article references-->
[temporäre Tabellen]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Übersicht über die Entwicklung]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
