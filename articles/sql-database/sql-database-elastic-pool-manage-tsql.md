<properties 
    pageTitle="Erstellen, oder eine flexible Ressourcenpool mit T-SQL Azure SQL-Datenbank verschieben | Microsoft Azure" 
    description="Verwenden Sie T-SQL Azure SQL-Datenbank in eine flexible Ressourcenpool zu erstellen. Oder T-SQL verwenden, um die Datenbank ein-und Pools zu verschieben." 
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-transact-sql"></a>Überwachen Sie und verwalten Sie einer Datenbank flexible Ressourcenpool mit Transact-SQL  

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)

Verwenden Sie die [Datenbank erstellen (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/dn268335.aspx) und [Alter Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) Befehle, erstellen und Verschieben von Datenbanken in die und aus flexible Pools. Flexible Pool muss vorhanden sein, bevor Sie diese Befehle verwenden können. Diese Befehle wirken sich nur Datenbanken. Erstellung der neuen Pools und der Einstellung für den Ressourcenpool Eigenschaften (z. B. min und Max eDTUs) können nicht mit T-SQL-Befehle geändert werden.

## <a name="create-a-new-database-in-an-elastic-pool"></a>Erstellen einer neuen Datenbank in eine flexible Ressourcenpool
Verwenden Sie den Befehl Datenbank erstellen, mit der Option SERVICE_OBJECTIVE.   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in a pool named S3M100.

Alle Datenbanken in einer flexible Ressourcenpool erben die Dienstebene der flexible Ressourcenpool (Basic, Standard, Premium). 


## <a name="move-a-database-between-elastic-pools"></a>Verschieben einer Datenbank zwischen flexible pools
Verwenden Sie den Befehl ALTER DATABASE mit der ändern, und richten Sie Dienst\_Ziel Option als ELASTISCH\_POOL; Legen Sie den Namen auf den Namen der Ziel-Ressourcenpool.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move the database named db1 to a pool named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>Verschieben einer Datenbank in eine flexible Ressourcenpool 
Verwenden Sie den Befehl ALTER DATABASE mit der ändern, und richten Sie Dienst\_Ziel Option als ELASTIC_POOL; Legen Sie den Namen auf den Namen der Ziel-Ressourcenpool.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move the database named db1 to a pool named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>Verschieben einer Datenbank aus einer flexible Ressourcenpool
Verwenden Sie den Befehl ALTER DATABASE, und legen Sie die SERVICE_OBJECTIVE auf einen der Leistungsebenen (S0, S1 usw.).

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes the database into a stand-alone database with the service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>Liste der Datenbanken in einer flexible Ressourcenpool
Verwenden der [sys.database\_Service \_Ziele Ansicht](https://msdn.microsoft.com/library/mt712619) aller Datenbanken in einer flexible Ressourcenpool aufgelistet. Melden Sie sich an den Master-Datenbank in der Ansicht Abfragen.

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-a-pool"></a>Erste Verwendung Ressourcendaten für einen pool

Verwenden der [sys.elastic\_Pool \_Ressource \_Stats Ansicht](https://msdn.microsoft.com/library/mt280062.aspx) der Ressource: Einsatz Statistik eine flexible Ressourcenpool auf einem logischen untersuchen. Melden Sie sich an den Master-Datenbank in der Ansicht Abfragen.

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-an-elastic-database"></a>Abrufen von Ressource: Einsatz für eine flexible-Datenbank

Verwenden der [sys.dm\_ Db\_ Ressource\_Stats Ansicht](https://msdn.microsoft.com/library/dn800981.aspx) oder [sys.resource \_Stats Ansicht](https://msdn.microsoft.com/library/dn269979.aspx) der Ressource: Einsatz Statistik einer Datenbank in eine flexible Ressourcenpool zu untersuchen. Dieses Verfahren ähnelt der Ressource: Einsatz für jede einzelne Datenbank Abfragen.

## <a name="next-steps"></a>Nächste Schritte

Nach dem Erstellen einer Datenbank flexible Ressourcenpool, können Sie flexible Datenbanken im Pool verwalten, indem Sie flexible Aufträge erstellen. Flexible Aufträge zu laufenden T-SQL-Skripts auf eine beliebige Anzahl von Datenbanken im Pool erleichtern. Weitere Informationen finden Sie unter [flexible Datenbank Aufträge (Übersicht)](sql-database-elastic-jobs-overview.md). 

Finden Sie unter [Skalierung heraus mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md): Verwenden Sie flexible Datenbanktools Skalierung, Verschieben von Daten, Abfrage, oder erstellen Sie Transaktionen.
