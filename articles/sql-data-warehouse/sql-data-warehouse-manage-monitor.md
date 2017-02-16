<properties
   pageTitle="Überwachen Sie Ihre Arbeitsbelastung mit DMVs | Microsoft Azure"
   description="Informationen Sie zum Überwachen der Arbeitsbelastung DMVs verwenden."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/08/2016"
   ms.author="sonyama;barbkess"/>

# <a name="monitor-your-workload-using-dmvs"></a>Überwachen Sie Ihre Arbeitsbelastung DMVs verwenden

Dieser Artikel beschreibt, wie mithilfe der dynamischen Management (DMVs) Ihrer Arbeitsbelastung überwachen und Ausführung der Abfrage in Azure SQL-Data Warehouse ermitteln.

## <a name="permissions"></a>Berechtigungen

Die DMVs in diesem Artikel abgefragt werden soll, benötigen Sie eine Ansicht Datenbank BUNDESSTAAT oder Steuerelement Berechtigung aus. Gewähren VIEW DATABASE STATE ist normalerweise die bevorzugte Berechtigung aus, wie viel eingeschränkter ist.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>Überwachen der Verbindung

Alle Benutzernamen in SQL Data Warehouse werden in [sys.dm_pdw_exec_sessions][]protokolliert.  Diese DMV enthält die letzte 10.000 Benutzernamen.  Die Nummer ist der Primärschlüssel und sequenziell für jedes neue Anmeldung zugeordnet ist.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Überwachen der Ausführung von Abfragen

Alle Abfragen, die auf SQL Data Warehouse ausgeführt werden in [sys.dm_pdw_exec_requests][]protokolliert.  Diese DMV enthält die letzten 10.000 Abfragen ausgeführt werden.  Die Request_id eindeutig identifiziert jede Abfrage und den Primärschlüssel für diese DMV ist.  Die Request_id wird für jedes neue Abfrage sequenziell zugewiesen und ist das Präfix QID, steht für die Abfrage-ID.  Diese DMV für eine angegebene Nummer Abfragen zeigt alle Abfragen für eine bestimmte Anmeldung.

>[AZURE.NOTE] Gespeicherte Prozeduren verwenden mehrerer anfordern IDs.  Anforderung IDs werden sequenziell zugewiesen. 

Hier sind die folgenden Schritte aus der abfrageausführung-Pläne und Zeiten für eine bestimmte Abfrage um.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>Schritt 1: Ermitteln der Abfrage, die Sie ermitteln möchten.

```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

Aus den vorherigen Abfrageergebnissen **Notiz die ID anfordern** der Abfrage, die Sie ermitteln möchten.

Abfragen in den Status " **angehalten** " sind aufgrund von Parallelitätsgrenzen in der Warteschlange wird. Diese Abfragen werden auch in der sys.dm_pdw_waits wartet Abfrage mit einem Typ von UserConcurrencyResourceType angezeigt. Weitere Details auf Parallelitätsgrenzen finden Sie unter [Parallelität und Arbeitsbelastung Management][] . Abfragen können auch anderen Gründen wie für Objektsperren warten.  Wenn Ihre Abfrage für eine Ressource wartet, finden Sie unter [Investigating Abfragen warten auf Ressourcen][] weiter unten in diesem Artikel.

Verwenden Sie zur Vereinfachung der Suche nach einer Abfrage in der Tabelle sys.dm_pdw_exec_requests [Etikett][] einen Kommentar zu Ihrer Abfrage zuweisen, die in der Ansicht sys.dm_pdw_exec_requests nachgeschlagen werden kann.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>Schritt 2: Ermitteln des Abfrage-Plans

Verwenden Sie die ID anfordern, um die Abfrage des verteilten SQL (DSQL) Plan aus [sys.dm_pdw_request_steps][]abzurufen.

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Wenn Sie ein Plan DSQL dauert länger als erwartet wird, kann die Ursache einer komplexen Plan mit vielen DSQL Schritten oder nur eine sehr lange dauert Schritt müssen.  Ist der Plan vielen Schritten mit mehreren verschieben Vorgängen, erwägen Sie das Optimieren Ihrer Tabelle Verteilung zum Verschieben von Daten zu verringern. Der [Verteilung der Tabelle][] Artikel wird erläutert, warum die Daten in einer Abfrage zu lösen verschoben werden müssen und erläutert einige Verteilungsstrategien zum Verschieben von Daten zu minimieren.

Weitere Details zu einem einzigen Schritt, der *Operation_type* Spalte neben dem abfrageschritt, langer ermitteln und notieren Sie den **Schritt Index**:

- Fahren Sie mit Schritt 3a für **SQL-Vorgänge**: OnOperation, RemoteOperation, ReturnOperation.
- Fahren Sie mit Schritt 3 b für **Vorgänge Verschieben von Daten**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>Schritt 3a: Ermitteln SQL auf die verteilten Datenbanken

Verwenden Sie die ID anfordern und der Schritt Index zum Abrufen von Details aus [sys.dm_pdw_sql_requests][], die Informationen der Ausführung des abfrageschritts auf alle verteilten Datenbanken enthält.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Wenn der abfrageschritt ausgeführt wird, kann [DBCC PDW_SHOWEXECUTIONPLAN][] zum Abrufen von des SQL Server geschätzten Plans aus dem Cache für SQL Server-Plan für den Schritt ausführen, klicken Sie auf eine bestimmte Verteilung verwendet werden.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>Schritt 3: Ermitteln der Verlagerung von Daten auf die verteilten Datenbanken

Verwenden Sie die ID anfordern und der Schritt Index zum Abrufen von Informationen zu einem Daten Bewegung Schritt auf jede Verteilung aus [sys.dm_pdw_dms_workers][]ausgeführt.

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

- Überprüfen Sie die Spalte *Total_elapsed_time* , um festzustellen, ob eine bestimmte Verteilung wesentlich länger als die andere für das Verschieben von Daten ausführt.
- Aktivieren Sie für die Verteilung langer der Spalte *Rows_processed* , um festzustellen, ob die Anzahl der Zeilen, die vom die Verteilung verschoben beträchtlich größer als die andere ist. Wenn dies der Fall ist, kann dies der zugrunde liegenden Daten Schiefe hindeuten.

Wenn die Abfrage ausgeführt wird, kann [DBCC PDW_SHOWEXECUTIONPLAN][] zum Abrufen von des SQL Server geschätzten Plans aus dem Cache für SQL Server-Plan für das aktuell ausgeführte SQL Schritt innerhalb einer bestimmten Verteilung verwendet werden.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>
## <a name="monitor-waiting-queries"></a>Überwachen Sie warten Abfragen

Wenn Sie feststellen, dass Ihre Abfrage nicht Fortschritte erzielt wird, da sie eine Ressource wartet, hier eine Abfrage, die alle Ressourcen zeigt, dass eine Abfrage wartet.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Wenn die Abfrage aktiv auf Ressourcen aus einer anderen Abfrage wartet, wird der Status **AcquireResources**sein.  Wenn die Abfrage alle erforderlichen Ressourcen aufweist, wird der Status **gewährt**sein.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu DMVs finden Sie unter [Systemansichten][] .
Weitere Informationen über bewährte Verfahren finden Sie [bewährte Methoden für SQL Data Warehouse][]

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[Bewährte Methoden für SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Systemansichten]: ./sql-data-warehouse-reference-tsql-system-views.md
[Tabelle Verteilung]: ./sql-data-warehouse-tables-distribute.md
[Parallelität und Arbeitsbelastung management]: ./sql-data-warehouse-develop-concurrency.md
[Untersuchung läuft Abfragen für Ressourcen warten]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[Sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[Sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[Sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[Sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[Sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[BESCHRIFTUNG]: https://msdn.microsoft.com/library/ms190322.aspx
