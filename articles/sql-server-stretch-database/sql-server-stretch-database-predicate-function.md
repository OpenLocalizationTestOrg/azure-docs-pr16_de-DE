<properties
    pageTitle="Wählen Sie Zeilen mithilfe einer Filterfunktion (gedehnt Datenbank) migrieren | Microsoft Azure"
    description="Informationen Sie zum Auswählen von Zeilen mithilfe einer Filterfunktion migrieren."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="douglasl"/>

# <a name="select-rows-to-migrate-by-using-a-filter-function-stretch-database"></a>Wählen Sie Zeilen mithilfe einer Filterfunktion (gedehnt Datenbank) migrieren

Wenn Sie kalte Daten in einer separaten Tabelle speichern möchten, können Sie die Datenbank gedehnt, um die gesamte Tabelle migrieren konfigurieren. Wenn die Tabelle wichtiges und kalte Daten enthält, andererseits, können Sie eine Filterfunktion, markieren Sie die Zeilen migrieren angeben. Das Filterprädikat ist eine Inline-Tabelle\-bewertet (Funktion). In diesem Thema beschrieben, wie Sie eine Tabelle Inline schreiben\-bewertet Funktion zum Auswählen von Zeilen zu migrieren.

>   [AZURE.NOTE] Wenn Sie eine Filterfunktion schlecht ausführt bereitstellen, führt Datenmigration schlecht. Dehnen Datenbank wendet die Filterfunktion in die Tabelle mit dem Operator CROSS anwenden.

Wenn Sie eine Filter-Funktion nicht angeben, wird die gesamte Tabelle migriert.

Wenn Sie die Datenbank aktivieren für gedehnt-Assistenten ausführen, können Sie eine gesamte Tabelle migrieren oder können Sie eine einfache Filter-Funktion im Assistenten angeben. Wenn Sie einen anderen Typ von Filter-Funktion verwenden Sie zum Auswählen von Zeilen migrieren möchten, führen Sie eine der folgenden Aktionen aus.

-   Beenden Sie den Assistenten, und führen Sie die ALTER TABLE-Anweisung gedehnt für die Tabelle zu aktivieren und eine Filterfunktion anzugeben.

-   Führen Sie die ALTER TABLE-Anweisung, um eine Filterfunktion anzugeben, nachdem Sie den Assistenten zu beenden.

Die Syntax der ALTER TABLE zum Hinzufügen einer Funktion wird später in diesem Artikel beschrieben.

## <a name="basic-requirements-for-the-filter-function"></a>Grundlegende Anforderungen für die Filter-Funktion
Die Tabelle Inline\-Werte für eine Datenbank gedehnt Filterprädikat erforderlichen Funktion sieht wie im folgenden Beispiel aus.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datatype1, @column2 datatype2 [, ...n])
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE <predicate>
```
Bezeichner für Spalten aus der Tabelle werden die Parameter für die Funktion müssen.

Schema Bindung ist erforderlich, um zu verhindern, dass Spalten, die von der Filterfunktion aus, die gelöscht oder geändert verwendet werden.

### <a name="return-value"></a>Rückgabewert
Wenn die Funktion eine nicht gibt\-ein leeres Ergebnis, die Zeile ist berechtigt migriert werden. Andernfalls \- d. h., wenn die Funktion ein Ergebnis nicht \- die Zeile ist nicht berechtigt, migriert werden.

### <a name="conditions"></a>Bedingungen
Die &lt; *Prädikat* &gt; kann von einer Bedingung oder mehrerer Bedingungen, die mit der logische und-Operator beigetreten bestehen.

```
<predicate> ::= <condition> [ AND <condition> ] [ ...n ]
```
Jeder Bedingung kann wiederum einer primitiven Bedingung oder mehrere primitiven Konditionen, die mit dem logischen Operator OR beigetreten bestehen.

```
<condition> ::= <primitive_condition> [ OR <primitive_condition> ] [ ...n ]
```

### <a name="primitive-conditions"></a>Primitiven Bedingungen
Eine Bedingung primitive kann eine der folgenden Vergleiche ausführen.

```
<primitive_condition> ::=
{
<function_parameter> <comparison_operator> constant
| <function_parameter> { IS NULL | IS NOT NULL }
| <function_parameter> IN ( constant [ ,...n ] )
}
```

-   Vergleichen Sie einen Funktionsparameter ein konstanter Ausdruck ein. Beispielsweise `@column1 < 1000`.

    Hier ein Beispiel, die überprüft, ob der Wert von *einer Datumsspalte* &lt; 1/1/2016.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
    GO

    ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Wenden Sie für einen Funktionsparameter Operators ISTNULL oder ist nicht NULL ein.

-   Verwenden Sie den Operator IN ein Funktionsparameter zu einer Liste von konstanten Werten vergleichen.

    Hier ein Beispiel, die überprüft, ob der Wert von einer *Lieferung\_Status* Spalte ist `IN (N'Completed', N'Returned', N'Cancelled')`.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

### <a name="comparison-operators"></a>Vergleichsoperatoren
Die folgenden Vergleichsoperatoren werden unterstützt.

`<, <=, >, >=, =, <>, !=, !<, !>`

```
<comparison_operator> ::= { < | <= | > | >= | = | <> | != | !< | !> }
```

### <a name="constant-expressions"></a>Konstante Ausdrücke
Die Konstanten, die Sie in einer Filterfunktion verwenden, können alle deterministisch Ausdruck sein, der ausgewertet werden kann, wenn Sie die Funktion definieren. Konstante Ausdrücke können die folgenden Elemente enthalten.

-   Literalen. Beispielsweise `N’abc’, 123`.

-   Algebraische Ausdrücke. Beispielsweise `123 + 456`.

-   Deterministische Funktionen. Beispielsweise `SQRT(900)`.

-   Deterministisch Konvertierungen, die Umwandlung oder konvertieren verwenden. Beispielsweise `CONVERT(datetime, '1/1/2016', 101)`.

### <a name="other-expressions"></a>Andere Ausdrücke
Sie können die BETWEEN und nicht BETWEEN Operatoren verwenden, wenn die resultierende Funktion den Regeln, die hier beschriebenen entspricht, nachdem Sie den BETWEEN ersetzen und nicht zwischen Operatoren mit den entsprechenden und-und oder-Ausdrücke.

Sie können keine Unterabfragen verwenden oder nicht\-deterministische Funktionen wie ZUFALLSZAHL () oder GETDATE().

## <a name="add-a-filter-function-to-a-table"></a>Hinzufügen einer Filter-Funktion zu einer Tabelle
Hinzufügen eine Filter-Funktion zu einer Tabelle durch Ausführen der **ALTER TABLE** -Anweisung und Angeben einer vorhandenen Inline-Tabelle\-bewertet Funktion als Wert des der **FILTER\_Prädikat** Parameter. Beispiel:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
) )
```
Nachdem Sie die Funktion zur Tabelle als Prädikat binden, sind die folgenden Punkte true.

-   Die nächste Mal Datenmigration auftritt, nur die Zeilen, die für die gibt die Funktion eine nicht\-leerer Wert migriert werden.

-   Die Spalten, die von der Funktion verwendet werden Schema gebunden. Sie können diese Spalten nicht ändern, solange eine Tabelle die Funktion als deren Filterprädikat verwendet wird.

Die Tabelle Inline können nicht gelöscht werden\-Funktion bewertet, solange eine Tabelle die Funktion als deren Filterprädikat verwendet wird.

>   [AZURE.NOTE] Um die Leistung der Filterfunktion erstellen Sie einen Index, klicken Sie auf die Spalten, die von der Funktion verwendet.

### <a name="passing-column-names-to-the-filter-function"></a>Übergabe Spaltennamen an die Filterfunktion
Wenn Sie einer Tabelle eine Filterfunktion zuweisen, geben Sie die Spaltennamen für die Filterfunktion mit einem Namen 1-Webpart übergeben. Wenn Sie angeben, ein dreiteiligen Namen, wenn Sie die Spalte übergeben benennt, nachfolgende Abfragen für die gedehnt\-aktiviert Tabelle schlägt fehl.

Wenn Sie einen dreiteiligen Spaltennamen angeben, wie im folgenden Beispiel dargestellt, beispielsweise die Anweisung erfolgreich ausgeführt wird, aber nachfolgende Abfragen der Tabelle schlägt fehl.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(dbo.SensorTelemetry.ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

Geben Sie stattdessen die Filter-Funktion mit einer Chi-Webpart Spaltenname ein, wie im folgenden Beispiel gezeigt.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON  (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

## <a name="a-nameaddafterwizaadd-a-filter-function-after-running-the-wizard"></a><a name="addafterwiz"></a>Fügen Sie eine Filterfunktion nach Ausführen des Assistenten  

Wenn Sie möchten eine Funktion verwenden, die Sie im Assistenten zum **Aktivieren von Datenbanken für gedehnt** erstellen können, können Sie ausführen ALTER TABLE-Anweisung, um eine Funktion anzugeben, nachdem Sie den Assistenten zu beenden. Bevor Sie eine Funktion anwenden können, müssen Sie jedoch zum Beenden der Datenmigrations, die bereits abgehalten wird, und fügen Sie die wieder migrierte Daten. (Weitere Informationen zu warum dies erforderlich ist, finden Sie unter [Ersetzen eine vorhandenen Filterfunktion](#replacePredicate).  

1. Umkehren der Richtung Migration und wieder zurück, die die Daten bereits migriert abrufen. Sie können keine diesen Vorgang abbrechen, nach dem Start. Sie Kosten auf Azure auch für ausgehende Datenübertragung anfallen \(Ausgang\). Weitere Informationen finden Sie unter [wie Azure Preise funktioniert](https://azure.microsoft.com/pricing/details/data-transfers/).  

    ```tsql  
    ALTER TABLE <table name>  
         SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;   
    ```  

2. Warten Sie für die Migration zu beenden. Sie können den Status in **Gedehnt Datenbank Monitor** aus SQL Server Management Studio überprüfen, oder Sie können die Ansicht **sys.dm_db_rda_migration_status** Abfragen. Weitere Informationen finden Sie unter [Überwachen und Problembehandlung bei der Migration von Daten](sql-server-stretch-database-monitor.md) oder [sys.dm_db_rda_migration_status](https://msdn.microsoft.com/library/dn935017.aspx).  

3. Erstellen Sie die Filter-Funktion, die Sie der Tabelle zuweisen möchten.  

4. Fügen Sie die Funktion auf die Tabelle, und starten Sie Migration von Daten in Azure neu.  

    ```tsql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE  
            (           
                FILTER_PREDICATE = <predicate>,  
                MIGRATION_STATE = OUTBOUND  
            )  
        );   
    ```  

## <a name="filter-rows-by-date"></a>Filtern von Zeilen nach Datum
Im folgende Beispiel werden die Zeilen, in denen die Spalte " **Datum** " einen Wert früher als 1 Januar 2016 enthält, migriert.

```tsql
-- Filter by date
--
CREATE FUNCTION dbo.fn_stretch_by_date(@date datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @date < CONVERT(datetime2, '1/1/2016', 101)
GO
```

## <a name="filter-rows-by-the-value-in-a-status-column"></a>Filtern von Zeilen durch den Wert in einer Statusspalte
Im folgende Beispiel migriert Zeilen, in die Spalte **Status** der angegebenen Werte enthält.

```tsql
-- Filter by status column
--
CREATE FUNCTION dbo.fn_stretch_by_status(@status nvarchar(128))
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @status IN (N'Completed', N'Returned', N'Cancelled')
GO
```

## <a name="filter-rows-by-using-a-sliding-window"></a>Filtern von Zeilen mit einem verschiebbaren Fenster
Lassen Sie zum Filtern von Zeilen mit einem verschiebbaren Fenster, beachten Sie die folgenden Anforderungen für die Filterfunktion.

-   Die Funktion muss deterministisch sein. Sie können nicht daher eine Funktion erstellen, die automatisch das verschiebbaren Fenster mit der Zeit neu berechnet.

-   Die Funktion verwendet die Bindung des Schemas. Daher aktualisieren Sie nicht einfach die Funktion "an der Stelle" jeden Tag, indem Sie **ALTER FUNCTION** zum Verschieben des verschiebbaren Fensters.

Beginnen Sie mit einem Filter-Funktion, wie im folgenden Beispiel, in dem Zeilen migriert werden, in dem die **SystemEndTime** Spalte einen Wert früher als 1 Januar 2016 enthält.

```tsql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING  
AS  
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2016-01-01T00:00:00', 101) ;
```

Filter-Funktion auf die Tabelle anwenden

```tsql
ALTER TABLE <table name>
SET (
        REMOTE_DATA_ARCHIVE = ON
                (
                        FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160101 (SysEndTime)
                                , MIGRATION_STATE = OUTBOUND
                )
        )
;
```

Wenn Sie das verschiebbaren Fenster aktualisieren möchten, führen Sie die folgenden Elemente.

1.  Erstellen Sie eine neue Funktion, die angibt, das neue verschiebbaren Fenster an. Im folgende Beispiel markiert Datumsangaben vor 2 Januar 2016 anstelle von 1 Januar 2016.

2.  Ersetzen Sie die vorherige Filterfunktion durch den neuen durch einen **ALTER TABLE**, wie im folgenden Beispiel dargestellt.

3. Legen Sie optional die vorherigen Filter-Funktion, die Sie nicht mehr verwenden, einen **Ablegen (Funktion)**. (Dieser Schritt ist nicht im Beispiel angezeigt.)

```tsql
BEGIN TRAN
GO
        /*(1) Create new predicate function definition */
        CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160102(@systemEndTime datetime2)
        RETURNS TABLE
        WITH SCHEMABINDING
        AS
        RETURN SELECT 1 AS is_eligible
               WHERE @systemEndTime < CONVERT(datetime2,'2016-01-02T00:00:00', 101)
        GO

        /*(2) Set the new function as the filter predicate */
        ALTER TABLE <table name>
        SET
        (
               REMOTE_DATA_ARCHIVE = ON
               (
                       FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160102(SysEndTime),
                       MIGRATION_STATE = OUTBOUND
               )
        )
COMMIT ;
```

## <a name="more-examples-of-valid-filter-functions"></a>Weitere Beispiele für gültige Filterfunktionen

-   Im folgende Beispiel werden zwei primitive Bedingungen mithilfe des logischen und-Operators kombiniert.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate((@column1 datetime, @column2 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
      WHERE @column1 < N'20150101' AND @column2 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date, shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Im folgende Beispiel werden mehrere Konditionen und eine deterministische Konvertierung mit konvertieren verwendet.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example1(@column1 datetime, @column2 int, @column3 nvarchar)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)AND (@column2 < -100 OR @column2 > 100 OR @column2 IS NULL)AND @column3 IN (N'Completed', N'Returned', N'Cancelled')
    GO
    ```

-   Im folgende Beispiel wird die mathematische Operatoren und Funktionen verwendet.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example2(@column1 float)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < SQRT(400) + 10
    GO
    ```

-   Im folgende Beispiel wird die BETWEEN und nicht BETWEEN Operatoren verwendet. Diese Verwendung ist gültig, da die resultierende Funktion den Regeln, die hier beschriebenen entspricht, nachdem Sie den BETWEEN ersetzen und nicht zwischen Operatoren mit den entsprechenden und-und oder-Ausdrücke.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example3(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 BETWEEN 0 AND 100
                AND (@column2 NOT BETWEEN 200 AND 300 OR @column1 = 50)
    GO
    ```
    Die vorherige Funktion entspricht die folgende Funktion, nachdem Sie die BETWEEN und nicht BETWEEN Operatoren durch die entsprechende und-und oder-Ausdrücke ersetzen.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example4(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 >= 0 AND @column1 <= 100AND (@column2 < 200 OR @column2 > 300 OR @column1 = 50)
    GO
    ```

## <a name="examples-of-filter-functions-that-arent-valid"></a>Beispiele für Filterfunktionen, die nicht gültig sind

-   Die folgende Funktion ist ungültig, da sie eine nicht enthält\-deterministische Konvertierung.

    ```tsql
    CREATE FUNCTION dbo.fn_example5(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016')
    GO
    ```

-   Die folgende Funktion ist ungültig, da sie eine nicht enthält\-deterministisch Funktionsaufrufs.

    ```tsql
    CREATE FUNCTION dbo.fn_example6(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < DATEADD(day, -60, GETDATE())
    GO
    ```

-   Die folgende Funktion ist nicht zulässig, da sie eine Unterabfrage enthält.

    ```tsql
    CREATE FUNCTION dbo.fn_example7(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (SELECT SupplierID FROM Supplier WHERE Status = 'Defunct'))
    GO
    ```

-   Die folgenden Funktionen sind nicht gültig da Ausdrücke, die algebraische Operatoren verwenden oder erstellt\-in Funktionen muss eine Konstante auswerten, wenn Sie die Funktion definieren. Sie können keine Spaltenverweise algebraische Ausdrücke oder Funktion Anrufe enthalten.

    ```tsql
    CREATE FUNCTION dbo.fn_example8(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 % 2 =  0
    GO

    CREATE FUNCTION dbo.fn_example9(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE SQRT(@column1) = 30
    GO
    ```

-   Die folgende Funktion ist nicht zulässig, da es die Regeln, die hier beschriebenen verletzt, nachdem Sie den Operator BETWEEN mit der entsprechende Ausdruck und ersetzt.

    ```tsql
    CREATE FUNCTION dbo.fn_example10(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 BETWEEN 1 AND 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```
    Die vorherige Funktion entspricht der folgenden Funktion, nachdem Sie den Operator BETWEEN mit der entsprechende Ausdruck und ersetzen. Diese Funktion ist nicht zulässig, da primitive Bedingungen nur den logischen Operator oder verwenden können.

    ```tsql
    CREATE FUNCTION dbo.fn_example11(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 >= 1 AND @column1 <= 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```

## <a name="how-stretch-database-applies-the-filter-function"></a>Wie gilt gedehnt Datenbank Filter-Funktion
Dehnen Datenbank gilt für die Tabelle die Filterfunktion und bestimmt berechtigte Zeilen mit dem Operator CROSS anwenden. Beispiel:

```tsql
SELECT * FROM stretch_table_name CROSS APPLY fn_stretchpredicate(column1, column2)
```
Wenn die Funktion eine nicht gibt\-ein leeres Ergebnis für die Zeile, die Zeile ist berechtigt migriert werden.

## <a name="a-namereplacepredicateareplace-an-existing-filter-function"></a><a name="replacePredicate"></a>Ersetzen einer vorhandenen Filterfunktion
Sie können eine zuvor festgelegten Filter-Funktion ersetzen, indem Sie erneut die **ALTER TABLE** -Anweisung ausführen und einen neuen Wert für die **FILTER\_Prädikat** Parameter. Beispiel:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate2(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
```
Die neue Inline-Tabelle\-Werte Funktion weist die folgenden Anforderungen.

-   Die neue Funktion muss kleiner als die vorherige Funktion eingeschränkt sein.

-   Alle Operatoren, die in der alten Funktion bestanden müssen in der neuen Funktion vorhanden sein.

-   Die neue Funktion kann keine Operatoren enthalten, die in der alten Funktion nicht vorhanden ist.

-   Die Reihenfolge der Operator Argumente kann nicht ändern.

-   Nur konstante Werte, die Bestandteil von einer `<, <=, >, >=` Vergleich auf eine Weise, die die Funktion weniger einschränkenden macht geändert werden kann.

### <a name="example-of-a-valid-replacement"></a>Beispiel für einen gültigen Ersatz
Wird davon ausgegangen Sie, dass die folgende Funktion der aktuellen Filterprädikat ist.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_old (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
Die folgende Funktion ist kein gültiger Ersatz aus, da die neue Datumskonstante (die später Stichtagsdatum angibt) die Funktion weniger einschränkenden macht.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_new (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '2/1/2016', 101)
            AND (@column2 < -50 OR @column2 > 50)
GO
```

### <a name="examples-of-replacements-that-arent-valid"></a>Beispiele für Ersatz, die nicht gültig sind
Die folgende Funktion ist kein gültige Ersatz, da die neue Datumskonstante (die eine zuvor Stichtagsdatum angibt) die Funktion weniger einschränkenden stellen nicht.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_1 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
Die folgende Funktion ist kein gültiger Ersatz, weil eine der Vergleichsoperatoren entfernt wurde.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_2 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -50)
GO
```
Die folgende Funktion ist kein gültiger Ersatz, weil eine neue Bedingung mit dem logischen Operator hinzugefügt wurde.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_3 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
            AND (@column2 <> 0)
GO
```

## <a name="remove-a-filter-function-from-a-table"></a>Entfernen einer Tabelle eine Filter-Funktion
Um die gesamte Tabelle anstelle der ausgewählten Zeilen zu migrieren, entfernen Sie die vorhandene Funktion durch Festlegen **FILTER\_Prädikat** auf Null. Beispiel:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = NULL,
    MIGRATION_STATE = <desired_migration_state>
) )
```
Nachdem Sie die Filter-Funktion entfernt haben, werden alle Zeilen in der Tabelle für die Migration berechtigt. Sie können nicht daher eine Filterfunktion für später der gleichen Tabelle angeben, es sei denn, Sie wieder alle remote-Daten für die Tabelle aus Azure zuerst importieren. Diese Einschränkung ist vorhanden, um die Situation zu vermeiden, Zeilen, die nicht berechtigt, für die Migration, sind Wenn Sie eine neue Filterfunktion bereitstellen bereits in Azure migriert wurden.

## <a name="check-the-filter-function-applied-to-a-table"></a>Aktivieren Sie das zu einer Tabelle angewendete Filterfunktion
Um den Filter-Funktion angewendet, die zu einer Tabelle zu überprüfen, öffnen Sie die Katalogansicht **sys.remote\_Daten\_Archiv\_Tabellen** und überprüfen Sie den Wert, der die **Filter\_Prädikat** Spalte. Wenn der Wert null ist, wird die gesamte Tabelle für Archivierung berechtigt. Weitere Informationen finden Sie unter [sys.remote_data_archive_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="security-notes-for-filter-functions"></a>Sicherheit Notizen für Filterfunktionen  
Eine beschädigte Konto mit Db_owner-Berechtigungen kann folgende Aktionen ausführen.  

-   Erstellen und Anwenden einer Tabellenwertfunktion, die große Mengen von Serverressourcen verbraucht oder in einer DOS-resultierender längere wartet.  

-   Erstellen und Anwenden einer Tabellenwertfunktion, die es ermöglicht, den Inhalt einer Tabelle abzuleiten, für die der Benutzer explizit Lesezugriff verweigert wurde.  

## <a name="see-also"></a>Siehe auch

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
