<properties
   pageTitle="Migrieren von SQL-Code in SQL Data Warehouse | Microsoft Azure"
   description="Tipps für die Migration von SQL-Code in Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
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
   ms.date="08/02/2016"
   ms.author="lodipalm;barbkess;sonyama;jrj"/>

# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a>Migrieren von SQL-Code in SQL Data Warehouse

Bei der Migration von Codes aus einer anderen Datenbank nach SQL Data Warehouse müssen Sie wahrscheinlich Änderungen an Ihrer Datenbank vornehmen. Einige Features SQL Data Warehouse können die Leistung beträchtlich erhöhen, wie sie entwickelt verteilt ausgelegt sind. Sind jedoch um Leistung und Skalierung beizubehalten, einige Features auch nicht verfügbar.

## <a name="common-t-sql-limitations"></a>Allgemeine T-SQL-Einschränkungen

Die folgende Liste enthält eine Übersicht über die am häufigsten verwendeten Features, die nicht in Azure SQL-Data Warehouse unterstützt werden. Zu problemumgehungen für die nicht unterstützte Features finden Sie unter den Links:

- [ANSI-Verknüpfungen Updates][]
- [ANSI-Verknüpfungen auf Löschen][]
- [Seriendruck-Anweisung][]
- Datenbank-Cross joins
- [Cursor][]
- [WÄHLEN SIE AUS. IN][]
- [EINFÜGEN VON... MITARBEITER][]
- OUTPUT-Klausel
- Inline benutzerdefinierte Funktionen
- mit mehreren Anweisungen Funktionen
- [häufig verwendete Tabellenausdrücke](#Common-table-expressions)
- [rekursive allgemeine Tabellenausdrücke (CTE)] (#Recursive-common-table-expressions-(CTE)
- CLR-Funktionen und Prozeduren
- $partition (Funktion)
- Tabellenvariablen
- Tabelle Werteparameter
- verteilte Transaktionen
- Commit / Arbeit zurücksetzen
- Transaktion speichern
- Ausführung Kontexten (AS ausführen)
- [Group by-Klausel mit Rollup / cube / Sätze Gruppierungsoptionen][]
- [Verschachtelungsebenen jenseits 8][]
- [Aktualisieren der Ansichten][]
- [Verwenden der SELECT-Anweisung für Variable Zuordnung][]
- [Nein MAX-Datentyp für die dynamische SQL-Zeichenfolgen][]

Glücklicherweise können die meisten dieser Nachteile nicht umgangen werden. Erläuterungen werden in den relevanten Entwicklung Artikeln weiter oben erwähnten bereitgestellt.

## <a name="supported-cte-features"></a>Unterstützte CTE features

Häufig verwendete Tabellenausdrücke (CTEs) werden in SQL Data Warehouse teilweise unterstützt.  Die folgenden CTE-Features werden derzeit unterstützt:

- Ein CTE kann in einer SELECT-Anweisung angegeben werden muss.
- Ein CTE kann in einer Ansicht erstellen-Anweisung angegeben werden.
- Ein CTE kann in einer Anweisung erstellen Tabelle AS auswählen (CTAS) angegeben werden.
- Ein CTE kann in einer Anweisung erstellen REMOTE Tabelle AS auswählen (CRTAS) angegeben werden.
- Ein CTE kann in einer Anweisung erstellen externe Tabelle AS auswählen (CETAS) angegeben werden.
- Eine entfernte Tabelle kann aus einem CTE verwiesen werden.
- Eine externe Tabelle kann aus einem CTE verwiesen werden.
- Mehrere CTE Abfragedefinitionen können in einem CTE definiert werden.

## <a name="cte-limitations"></a>CTE Einschränkungen

Häufig verwendete Tabellenausdrücke haben einige Einschränkungen SQL Data Warehouse einschließlich:

- Ein CTE muss einer SELECT-Anweisung folgen. Einfügen, aktualisieren, löschen, und der SERIENDRUCK-Anweisungen werden nicht unterstützt.
- Ein allgemeiner Tabellenausdruck, der Bezüge an sich selbst (rekursive allgemeine Tabellenausdruck) enthält, wird nicht unterstützt (siehe unten im Abschnitt).
- Angeben von mehr als eins mit-Klausel in einen CTE ist nicht zulässig. Beispielsweise einer CTE_query_definition eine Unterabfrage enthält, kann nicht die Unterabfrage eine geschachtelte mit-Klausel enthalten, die einem anderen CTE definiert.
- ORDER BY-Klausel kann nicht in die CTE_query_definition außer verwendet werden, wenn eine TOP-Klausel angegeben ist.
- Wenn ein CTE in einer Anweisung, die Teil eines Stapels ist verwendet wird, muss die Anweisung davor durch ein Semikolon folgen.
- Bei Verwendung in Anweisungen durch Sp_prepare vorbereitet verhält sich CTEs auf die gleiche Weise wie andere SELECT-Anweisungen in PDW aus. Jedoch wenn CTEs als Teil von Sp_prepare vorbereitet CETAS verwendet werden, kann das Verhalten von SQL Server und anderen PDW Anweisungen aufgrund der Art und Weise zurückstellen Bindung für Sp_prepare implementiert. Wenn Sie wählen, dass Verweise, die CTE eine falsche Spalte verwendet wird, die in CTE nicht vorhanden ist, wird der Sp_prepare ohne Ermitteln des Fehlers übergeben, aber der Fehler stattdessen während Sp_execute ausgelöst.

## <a name="recursive-ctes"></a>Rekursive CTEs

Rekursive CTEs werden in SQL Data Warehouse nicht unterstützt.  Die beste Vorgehensweise besteht darin Teile aufzuteilen und die Migraion der rekursive CTE kann etwas abgeschlossen werden die in mehrere Schritte. Normalerweise können Sie eine Schleife verwenden und füllen eine temporäre Tabelle aus, wie Sie die rekursive Abfragen zwischenzeitlichen durchlaufen. Sobald die temporäre Tabelle ausgefüllt wird können Sie dann die Daten als ein einzelnes Resultset zurückgeben. Ein ähnlicher Ansatz wurde verwendet, um lösen `GROUP BY WITH CUBE` in die [group by-Klausel mit Rollup / cube / Sätze Gruppierungsoptionen][] Artikel.

## <a name="unsupported-system-functions"></a>Nicht unterstützte Systemfunktionen

Es gibt auch einige Systemfunktionen, die nicht unterstützt werden. Einige der wichtigsten zugewiesenen Aufgaben, die finden Sie in der Regel möglicherweise, in Data Warehouse verwendet werden:

- 'NEWSEQUENTIALID()'
- @@NESTLEVEL()
- @@IDENTITY()
- @@ROWCOUNT()
- ROWCOUNT_BIG
- ERROR_LINE()

Einige dieser Probleme können umgangen werden.

## <a name="rowcount-workaround"></a>@@ROWCOUNTDieses Problem zu umgehen

Fehlende Unterstützung für umgehen @@ROWCOUNT, erstellen Sie eine gespeicherte Prozedur, die die Zeilenanzahl der letzte aus sys.dm_pdw_request_steps abgerufen werden, und führen Sie dann `EXEC LastRowCount` nach einer DML-Anweisung.

```sql
CREATE PROCEDURE LastRowCount AS
WITH LastRequest as 
(   SELECT TOP 1    request_id
    FROM            sys.dm_pdw_exec_requests
    WHERE           session_id = SESSION_ID()
    AND             resource_class IS NOT NULL
    ORDER BY end_time DESC
),
LastRequestRowCounts as
(
    SELECT  step_index, row_count
    FROM    sys.dm_pdw_request_steps
    WHERE   row_count >= 0
    AND     request_id IN (SELECT request_id from LastRequest)
)
SELECT TOP 1 row_count FROM LastRequestRowCounts ORDER BY step_index DESC
;
```

## <a name="next-steps"></a>Nächste Schritte
Eine vollständige Liste der alle unterstützten T-SQL-Anweisungen werden soll finden Sie unter [Transact-SQL-Themen][].

<!--Image references-->

<!--Article references-->
[ANSI-Verknüpfungen Updates]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[ANSI-Verknüpfungen auf Löschen]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[Seriendruck-Anweisung]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[EINFÜGEN VON... MITARBEITER]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Transact-SQL-Themen]: ./sql-data-warehouse-reference-tsql-statements.md

[Cursor]: ./sql-data-warehouse-develop-loops.md
[WÄHLEN SIE AUS. IN]: ./sql-data-warehouse-develop-ctas.md#selectinto
[Group by-Klausel mit Rollup / cube / Sätze Gruppierungsoptionen]: ./sql-data-warehouse-develop-group-by-options.md
[Verschachtelungsebenen jenseits 8]: ./sql-data-warehouse-develop-transactions.md
[Aktualisieren der Ansichten]: ./sql-data-warehouse-develop-views.md
[Verwenden der SELECT-Anweisung für Variable Zuordnung]: ./sql-data-warehouse-develop-variable-assignment.md
[Nein MAX-Datentyp für die dynamische SQL-Zeichenfolgen]: ./sql-data-warehouse-develop-dynamic-sql.md

<!--MSDN references-->

<!--Other Web references-->
