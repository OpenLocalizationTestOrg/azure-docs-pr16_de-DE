<properties
   pageTitle="Überwachung Azure SQL-Datenbank mit Ansichten Dynamic Management | Microsoft Azure"
   description="Erfahren Sie, wie erkennen und Analysieren von häufig auftretende Leistungsprobleme mithilfe von dynamischen Management Ansichten auf um Microsoft Azure SQL-Datenbank zu überwachen."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/20/2016"
   ms.author="carlrab"/>

# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Verwenden von dynamischen Management Ansichten Azure SQL-Datenbank für die Überwachung

Microsoft Azure SQL-Datenbank ermöglicht eine Teilmenge der dynamischen Management Ansichten Diagnose von Leistungsproblemen, die durch blockierte oder langer Abfragen, Ressourcenengpässe, beeinträchtigt Abfrage-Pläne, usw. verursacht werden kann. Dieses Thema enthält Informationen, wie Sie häufig auftretende Leistungsprobleme erkennen mithilfe von dynamischen Management Ansichten.

SQL-Datenbank unterstützt teilweise drei Kategorien von dynamischen Management Ansichten:

- Dynamische Management Datenbank-bezogene Ansichten.
- Dynamische Management Ausführung-bezogene Ansichten.
- Bezug auf die Transaktion dynamische Management-Ansichten.

Ausführliche Informationen zu dynamischen Management Sichten finden Sie unter [dynamische Management Ansichten und Funktionen (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) in der SQL Server-Onlinedokumentation.

## <a name="permissions"></a>Berechtigungen

In SQL-Datenbank erfordert eine dynamische Ansicht Abfragen **VIEW DATABASE STATE** Berechtigungen. Die **VIEW DATABASE STATE** -Berechtigung gibt Informationen über alle Objekte innerhalb der aktuellen Datenbank zurück.
Um einem bestimmten Datenbankbenutzer die **VIEW DATABASE STATE** -Berechtigungen zuweisen möchten, führen Sie die folgende Abfrage:

```GRANT VIEW DATABASE STATE TO database_user; ```

In einer lokalen SQL Server-Instanz zurückgeben dynamische Management Ansichten Zustand Serverinformationen. Informationen über die aktuelle logische Datenbank nur in SQL-Datenbank zurückgegeben.

## <a name="calculating-database-size"></a>Berechnen der Größe der Datenbank

Die folgende Abfrage gibt die Größe der Datenbank (in MB):

```
-- Calculates the size of the database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

Die folgende Abfrage gibt die Größe der einzelnen Objekte (in MB) in der Datenbank an:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Überwachen von Verbindungen

Die Ansicht [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) können zum Abrufen von Informationen über die Verbindungen mit einer bestimmten Azure SQL-Datenbankserver und die Details der jeweiligen Verbindung hergestellt. Darüber hinaus ist die Ansicht [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) hilfreich beim Abrufen von Informationen zu allen aktiven Benutzer Verbindungen und internen Aufgaben.
Die folgende Abfrage ruft Informationen über die aktuelle Verbindung an:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [AZURE.NOTE] Beim Ausführen von der **dm_exec_requests** und **sys.dm_exec_sessions Ansichten** **VIEW DATABASE STATE** -Berechtigung für die Datenbank haben, wird der Datenbank alle ausgeführte Sitzungen; Andernfalls wird nur die aktuelle Sitzung.

## <a name="monitoring-query-performance"></a>Überwachen der abfrageleistung

Ausführen von Abfragen langsam oder lange kann signifikante Systemressourcen belegen. In diesem Abschnitt wird veranschaulicht, wie dynamische Management-Ansichten verwenden, um einige allgemeine Abfrage Leistungsprobleme zu erkennen. Ein Bezug älterer aber immer noch hilfreich zur Behandlung dieses Problems, ist die [Problembehandlung bei Leistungsproblemen in SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) Artikel auf Microsoft TechNet.

### <a name="finding-top-n-queries"></a>Suchen die obersten N-Abfragen

Im folgende Beispiel gibt Informationen zu den oberen fünf Abfragen, die durchschnittliche CPU-Zeit Rangfolge. In diesem Beispiel aggregiert die Abfragen entsprechend Hashlänge Abfrage aus, damit logisch gleichwertig Abfragen nach deren kumulierte Ressourcenverbrauch gruppiert sind.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Überwachen von blockierten Abfragen

Langsam oder langer Abfragen können Sie eigene Notizen hinzufügen können übermäßiger Ressourcenverbrauch und zur Folge, dass blockierten Abfragen werden. Beeinträchtigt Anwendungsentwurf, fehlerhafte Abfrage-Pläne, die fehlende hilfreiche Indizes und usw., kann die Ursache für die Sperre sein. Die Ansicht sys.dm_tran_locks können um Informationen zu den aktuellen Tätigkeit ein Sperren in Ihrem SQL Azure-Datenbank zu erhalten. Beispielsweise Code, finden Sie unter [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) in der SQL Server-Onlinedokumentation.

### <a name="monitoring-query-plans"></a>Abfrage-Pläne für die Überwachung

Eine Abfrage nicht effizient planen kann auch CPU-Auslastung erhöhen. Im folgende Beispiel verwendet die [Sys. dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) Ansicht, um zu bestimmen, welche Abfrage die am häufigsten kumulative CPU verwendet.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Siehe auch

[Einführung in SQL-Datenbank](sql-database-technical-overview.md)
