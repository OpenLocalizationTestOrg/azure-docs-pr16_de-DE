<properties
   pageTitle="Zuweisen von Variablen in SQL Data Warehouse | Microsoft Azure"
   description="Tipps für das Zuweisen von Variablen von Transact-SQL Azure SQL-Data Warehouse für die Entwicklung von Lösungen."
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

# <a name="assign-variables-in-sql-data-warehouse"></a>Zuweisen von Variablen in SQL Data Warehouse
Variablen in SQL Data Warehouse sind set mithilfe der `DECLARE` Anweisung oder die `SET` Anweisung.

Alle der folgenden sind absolut gültige Möglichkeiten, um den Wert einer Variablen festzulegen:

## <a name="setting-variables-with-declare"></a>Festlegen von Variablen mit DECLARE

Variablen mit DECLARE ist eine der am häufigsten flexiblen Methoden, um einen Variablen Wert in SQL Data Warehouse festlegen.

```sql
DECLARE @v  int = 0
;
```

Sie können auch DECLARE verwenden, um jeweils mehr als eine Variable zu festzulegen. Sie können keine `SELECT` oder `UPDATE` Zweck:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Sie können keine initialisieren und eine Variable in der gleichen DECLARE-Anweisung. Erläutern Sie den Punkt im folgenden Beispiel ist als **nicht** zulässig @p1 ist Initialisierung sowohl in der gleichen DECLARE-Anweisung verwendet werden. Dies führt zu einem Fehler.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Festlegen von Werten mit festlegen
Ist eine sehr allgemeine Methode zum Festlegen einer einzelnen Variablen zu speichern.

Alle in den folgenden Beispielen gibt gültige zum Festlegen einer Variablen mit festlegen:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Sie können eine Variable nur jeweils mit festlegen festlegen. Sind jedoch als über gesehen zusammengesetzte Operatoren zulässig sind.

## <a name="limitations"></a>Einschränkungen
Sie können nicht für Variable Zuordnung aktivieren oder UPDATE verwenden.


## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie unter [Übersicht über die Entwicklung][].

<!--Image references-->

<!--Article references-->
[Übersicht über die Entwicklung]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
