<properties
    pageTitle="Überwachen und Verwalten einer Datenbank flexible Ressourcenpool mit c# | Microsoft Azure"
    description="Verwenden Sie C#-Datenbank Entwicklungstechniken, um eine SQL Azure-Datenbank flexible Datenbank Ressourcenpool verwalten."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-cx23"></a>Überwachen Sie und verwalten Sie einer Datenbank flexible Ressourcenpool mit C & #x 23; 

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


Informationen Sie zum Verwalten einer [Datenbank flexible Ressourcenpool](sql-database-elastic-pool.md) mit C & #x 23;. 

>[AZURE.NOTE] Viele neue Funktionen der SQL-Datenbank werden nur unterstützt, wenn Sie das [Modell zur Bereitstellung von Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md), verwenden, damit Sie immer die neuesten verwenden sollten **Azure SQL-Datenbank-Management-Bibliothek für .NET ([Dokumente](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Die ältere [klassischen Bereitstellung Modell basierenden Bibliotheken](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) werden unterstützt, nur, zur Abwärtskompatibilität, damit es empfiehlt sich, dass Sie die neueren Ressourcenmanager Grundlage Bibliotheken verwenden.

Um die Schritte in diesem Artikel ausführen zu können, benötigen Sie die folgenden Elemente:

- Flexible Ressourcenpool (Pool, die Sie verwalten möchten) Um ein Ressourcenpool zu erstellen, finden Sie unter [Erstellen einer Datenbank flexible Ressourcenpool mit c#](sql-database-elastic-pool-create-csharp.md).
- Visual Studio. Eine kostenlose Kopie von Visual Studio finden Sie auf der Seite [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) .


## <a name="move-a-database-into-an-elastic-pool"></a>Verschieben einer Datenbank in eine flexible Ressourcenpool

Sie können eine eigenständige Datenbank in oder aus einem Ressourcenpool verschieben.  

    // Retrieve current database properties.

    currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

    // Configure create or update parameters with existing property values, override those to be changed.
    DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentDatabase.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
            Collation = currentDatabase.Properties.Collation,
        }
    };

    // Update the database.
    var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", updatePooledDbParameters);

## <a name="list-databases-in-an-elastic-pool"></a>Liste der Datenbanken in einer flexible Ressourcenpool

Um alle Datenbanken in einem Ressourcenpool abzurufen, rufen Sie die [ListDatabases](https://msdn.microsoft.com/library/microsoft.azure.management.sql.elasticpooloperationsextensions.listdatabases) -Methode aus.

    //List databases in the elastic pool
    DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases("resourcegroup-name", "server-name", "ElasticPool1");
    Console.WriteLine("Databases in Elastic Pool {0}", "server-name.ElasticPool1");
    foreach (Database db in dbListInPool)
    {
        Console.WriteLine("  Database {0}", db.Name);
    }

## <a name="change-performance-settings-of-a-pool"></a>Ändern der Einstellungen der Leistung von einem Ressourcenpool

Abrufen von vorhandenen Ressourcenpool Eigenschaften. Ändern Sie die Werte aus, und führen Sie die Methode CreateOrUpdate.

    var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

    // Configure create or update parameters with existing property values, override those to be changed.
    ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = currentPool.Location,
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = currentPool.Properties.Edition,
            DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
            DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
            Dtu = (int)currentPool.Properties.Dtu,
            StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
        }
    };

    newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);


## <a name="latency-of-elastic-pool-operations"></a>Wartezeit flexible Ressourcenpool Vorgänge

- Ändern die min eDTUs pro Datenbank oder Max eDTUs pro Datenbank in der Regel führt in fünf Minuten oder weniger.
- Zeit zum Ändern der Größe der Ressourcenpool (eDTUs), hängt von der kombinierte Größe aller Datenbanken in dem Pool ab. Mittelwert der Änderungen 90 Minuten oder weniger pro 100 GB. Wenn beispielsweise der gesamte Speicherplatz aller Datenbanken in dem Pool 200 GB, klicken Sie dann die erwarteten Wartezeit ist, ändern den Ressourcenpool eDTU pro Pool 3 Stunden oder weniger.




## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Verbindungsfehler und andere Probleme-Datenbank](sql-database-develop-error-messages.md).
- [SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure Ressource Management-APIs](https://msdn.microsoft.com/library/azure/dn948464.aspx)
- [Erstellen eines neuen Datenbank flexible Pools mit c#](sql-database-elastic-pool-create-csharp.md)
- [Wann sollte eine Datenbank flexible Ressourcenpool werden verwendet?](sql-database-elastic-pool-guidance.md)
- Finden Sie unter [Skalierung heraus mit Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md): Verwenden Sie flexible Datenbanktools Skalierung, Verschieben von Daten, Abfrage, oder erstellen Sie Transaktionen.

