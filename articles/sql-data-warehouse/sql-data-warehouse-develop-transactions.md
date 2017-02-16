<properties
   pageTitle="Transaktionen in SQL Datawarehouse | Microsoft Azure"
   description="Tipps für die Implementierung von Transaktionen in Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
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
   ms.date="07/31/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="transactions-in-sql-data-warehouse"></a>Transaktionen in SQL Datawarehouse

Wie erwartet, unterstützt SQL Data Warehouse Transaktionen als Teil der Datawarehouse Arbeitsbelastung an. Sind jedoch um sicherzustellen, dass die Leistung von SQL Data Warehouse bei verwaltet werden einige Features beschränkt im Vergleich zu SQL Server. In diesem Artikel werden die Unterschiede und die anderen Listen. 

## <a name="transaction-isolation-levels"></a>Transaktionsisolationsebenen
SQL Data Warehouse implementiert ACID-Transaktionen. Die Isolation der Transaktionen Unterstützung ist jedoch auf `READ UNCOMMITTED` und kann nicht geändert werden. Sie können eine Reihe von Methoden zum geänderter Daten zu verhindern, wenn dies nicht gewünscht für Sie ist Codierung implementieren. Die am häufigsten verwendeten Methoden nutzen CTAS sowohl Tabelle Partition wechseln (häufig als den verschiebbaren Fenstermuster bezeichnet), um zu verhindern, dass Benutzer Abfragen von Daten, die noch erstellt werden. Ansichten, die vor dem Filtern der Daten ist ebenfalls ein beliebte Ansatz.  

## <a name="transaction-size"></a>Transaktionsgröße
Eine einzelne Daten Änderung Transaktion ist in Größe begrenzt. Die Beschränkung wird heute "pro Verteilung" angewendet. Daher kann die gesamte Zuteilung berechnet werden, mit der Verteilungsanzahl multipliziert die Beschränkung. Zum Teilen ungefähren der maximalen Anzahl von Zeilen in der Transaktion die Verteilung Linienende durch die Gesamtgröße der einzelnen Zeilen. Erwägen Sie für Spalten mit variabler Länge eine Spalte average Länge aufzeichnen, anstatt die maximale Größe zu verwenden.

In der Tabelle unter den folgenden Annahmen wurden vorgenommen:

* Gleichmäßige Verteilung der Daten aufgetreten. 
* Die durchschnittliche Zeilenlänge beträgt 250 Byte

| [DWU][]    | Begrenzen Sie pro Verteilung (GiB) | Anzahl der Verteilung | MAX Transaktionsgröße (GiB) | # Zeilen pro Verteilung | Max Zeilen pro Transaktion |
| ------ | -------------------------- | ----------------------- | -------------------------- | ----------------------- | ------------------------ |
| DW100  |  1                         | 60                      |   60                       |   4,000,000             |    240,000,000           |
| DW200  |  1,5                       | 60                      |   90                       |   6,000,000             |    360,000,000           |
| DW300  |  2,25                      | 60                      |  135                       |   9,000,000             |    540,000,000           |
| DW400  |  3                         | 60                      |  180                       |  12,000,000             |    720,000,000           |
| DW500  |  3,75                      | 60                      |  225                       |  15,000,000             |    900,000,000           |
| DW600  |  4.5                       | 60                      |  Um 270 Grad                       |  18,000,000             |  1,080,000,000           |
| DW1000 |  7.5                       | 60                      |  450                       |  30,000,000             |  1,800,000,000           |
| DW1200 |  9                         | 60                      |  540                       |  36,000,000             |  2,160,000,000           |
| DW1500 | 11,25                      | 60                      |  675                       |  45,000,000             |  2,700,000,000           |
| DW2000 | 15                         | 60                      |  900                       |  60.000.000             |  3,600,000,000           |
| DW3000 | 22,5                       | 60                      |  1.350                     |  90,000,000             |  5,400,000,000           |
| DW6000 | 45                         | 60                      |  2.700                     | 180,000,000             | 10,800,000,000           |

Pro Transaktion oder Vorgang wird das Größenlimit Transaktion angewendet. Es ist nicht über alle gleichzeitigen Transaktionen angewendet wird. Daher darf jede Transaktion dieses Datenmenge in das Protokoll schreiben. 

Optimieren und die Menge der Daten in das Protokoll geschrieben minimieren Lizenzinformationen finden Sie im Artikel [best Practices für Transaktionen][] .

> [AZURE.WARNING] Die maximale Transaktionsgröße kann nur für HASH erreicht werden oder sogar ROUND_ROBIN verteilt Tabellen, wo finde ich die Seitenansicht der Daten. Wenn die Transaktion auf die Daten in einer Weise verzerrt schreiben ist die Beschränkung wahrscheinlich vor der maximale Transaktionsgröße erreicht werden.
<!--REPLICATED_TABLE-->

## <a name="transaction-state"></a>Transaktionszustand
SQL Data Warehouse wird die XACT_STATE()-Funktion verwendet, um eine fehlgeschlagene Transaktion unter Verwendung des Werts-2 zu melden. Dies bedeutet, dass die Transaktion fehlgeschlagen ist und für das Zurücksetzen nur markiert ist

> [AZURE.NOTE] Die Verwendung von-2 von der Funktion XACT_STATE, um eine fehlgeschlagene Transaktion kennzeichnen stellt anderes Verhalten mit SQL Server. SQL Server verwendet den Wert-1 zur Darstellung einer Transaktions dauerhaften Commit ausgeführt werden. SQL Server tolerieren kann einige Fehler innerhalb einer Transaktion ohne, als dauerhaften Commit ausgeführt werden markiert werden soll. Beispielsweise `SELECT 1/0` würde tritt ein Fehler auf, aber nicht erzwingen, dass eine Transaktion in einem Zustand dauerhaften Commit ausgeführt werden. SQL Server erlaubt liest auch in der Transaktion dauerhaften Commit ausgeführt werden. Jedoch können SQL Data Warehouse Aktion Sie nicht. Falls ein Fehler, innerhalb einer Transaktion SQL Data Warehouse auftritt es wird automatisch im Zustand-2 einzugeben, und Sie werden nicht in der vorzunehmen, eine weitere Anweisungen auswählen, bis die Anweisung rückgängig gemacht wurde. Es ist daher wichtig, überprüfen, dass die Anwendungscode um festzustellen, ob es XACT_STATE() während verwendet möglicherweise Code Änderungen vornehmen muss.

Beispielsweise wird möglicherweise in SQL Server eine Transaktion angezeigt werden, die sieht wie folgt aus:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Wenn Sie den Code lassen ungeändert über werden, erhalten Sie folgende Fehlermeldung angezeigt:

Msg 111233, Ebene 16, Status 1, Zeile 1 111233; Die aktuelle Transaktion wurde abgebrochen, und haben ausstehenden Änderungen rückgängig gemacht wurde. Ursache: Eine Transaktion in einem Zustand zurücksetzen nur wurde nicht explizit vor einer DDL, DML oder SELECT-Anweisung zurückgesetzt.

Sie erhalten auch nicht die Ausgabe der Funktionen ERROR_ *.

In SQL Data Warehouse muss der Code etwas geändert werden:

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();
        
        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

Erwartet wird jetzt beobachtet werden. Der Fehler in der Transaktion verwaltet wird, und die Funktionen ERROR_ * Werte angeben, wie erwartet.

Geändert wurde lediglich, die die `ROLLBACK` der Transaktion hatten, bevor Sie die Fehlerinformationen in das Lesen auftritt der `CATCH` blockieren.

## <a name="errorline-function"></a>Error_Line() (Funktion)
Es ist auch zu beachten, dass SQL Data Warehouse nicht implementiert oder die Funktion ERROR_LINE() unterstützen. Wenn Sie dies in Ihrer Code enthalten müssen Sie entfernen, um mit SQL Data Warehouse kompatibel sein. Verwenden Sie stattdessen Abfrage Etiketten in Ihrem Code entsprechenden Funktionalität implementiert wird. Lizenzinformationen finden Sie im Artikel [Bezeichnung][] für weitere Details zu diesem Feature.

## <a name="using-throw-and-raiserror"></a>AUSLÖSEN und RAISERROR verwenden
AUSLÖSEN ist die weitere moderne Implementierung zum Auslösen von Ausnahmen in SQL Data Warehouse RAISERROR wird ebenfalls unterstützt. Es gibt einige Unterschiede, die im Wert Beachtung jedoch sind.

- Benutzerdefinierte Fehlermeldungen, die Zahlen im Bereich 100,000-150.000 ausgelösten Ausnahme werden kann
- RAISERROR Fehlermeldungen sind bei 50.000 fest.
- Verwendung der sys.messages wird nicht unterstützt.

## <a name="limitiations"></a>Limitiations
SQL Data Warehouse verfügt über ein paar andere Einschränkungen, die sich auf Transaktionen beziehen.

Sie sind wie folgt:

- Keine verteilten Transaktionen
- Keine geschachtelten Transaktionen zulässig
- Kein speichern Punkte zulässig.
- Keine benannten Transaktionen
- Keine markierten Transaktionen
- Keine Unterstützung für DDL wie `CREATE TABLE` innerhalb eines Benutzers definiert Transaktion

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Transaktionen zu optimieren, finden Sie unter [best Practices für Transaktionen][].  Weitere Informationen zu anderen SQL Data Warehouse best Practices finden Sie unter [bewährte Methoden für SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[development overview]: ./sql-data-warehouse-overview-develop.md
[Bewährte Methoden für Transaktionen.]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Bewährte Methoden für SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[BESCHRIFTUNG]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
