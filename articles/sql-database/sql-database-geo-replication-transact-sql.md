<properties
    pageTitle="Konfigurieren von Geo-Replikation für Azure-SQL­Datenbank mit Transact-SQL | Microsoft Azure"
    description="Konfigurieren von Geo-Replikation für mit Transact-SQL Azure SQL-Datenbank"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="NA"
    ms.date="10/13/2016"
    ms.author="carlrab"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-transact-sql"></a>Konfigurieren von Geo-Replikation für mit Transact-SQL Azure SQL-Datenbank

> [AZURE.SELECTOR]
- [(Übersicht)](sql-database-geo-replication-overview.md)
- [Azure-Portal](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

In diesem Artikel wird das Konfigurieren von aktiven Geo-Replikation für eine Azure SQL-Datenbank mit Transact-SQL veranschaulicht.

Um System durch Verwendung von Transact-SQL starten zu können, finden Sie unter [Einleiten einer geplanten oder ungeplanten Failover für mit Transact-SQL Azure SQL-Datenbank](sql-database-geo-replication-failover-transact-sql.md).

>[AZURE.NOTE] Aktive Geo-Replikation (lesbaren sekundäre) steht für alle Datenbanken auf allen Ebenen der Dienst zur Verfügung. In April 2017 nicht lesbaren sekundäre Typ gelöscht werden, und vorhandene nicht lesbaren Datenbanken, um lesbare sekundäre automatisch aktualisiert werden.

So konfigurieren Sie aktiven Geo-Replikation mithilfe von Transact-SQL, benötigen Sie Folgendes:

- Ein Azure-Abonnement.
- Eine logische Azure SQL-Datenbankserver <MyLocalServer> und einer SQL-Datenbank <MyDB> – die primäre Datenbank, die repliziert werden soll.
- Ein oder mehrere logische Azure SQL-Datenbank-Servern < MySecondaryServer(n) > - die logischen Server, die den Partnerservern, werden in denen Sie sekundäre Datenbanken erstellt werden.
- Ein Benutzername, der DBManager auf dem primären, haben Db_ownership der, dass Sie Geo Replikation, werden lokale Datenbank und den Partner-Servern, dem Sie Geo-Replikation konfigurieren, DBManager sein.
- SQL Server Management Studio (SSMS)

> [AZURE.IMPORTANT] Es wird empfohlen, dass Sie immer die neueste Version von Management Studio verwenden, um mit Microsoft Azure und SQL-Datenbank-Updates synchronisiert werden. [Aktualisieren von SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="add-secondary-database"></a>Sekundäre Datenbank hinzufügen

**ALTER DATABASE** -Anweisung können um eine sekundäre Geo repliziert-Datenbank auf einem Partnerserver zu erstellen. Klicken Sie auf die master-Datenbank des Servers mit der Datenbank repliziert werden, führen Sie diese Anweisung. Die Datenbank Geo repliziert (die "primäre Datenbank") haben denselben Namen wie die Datenbank, die repliziert und kann, standardmäßig sind die gleichen Dienstalter wie die primäre Datenbank. Die sekundäre Datenbank nicht lesbaren oder gelesen werden kann, und eine einzelne Datenbank oder eine flexible Databbase werden kann. Weitere Informationen finden Sie unter [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) und [Dienst Ebenen](sql-database-service-tiers.md).
Nachdem die sekundäre Datenbank erstellt haben und die sich befinden, werden Daten asynchrone aus der primären Datenbank repliziert beginnen. Die folgenden Schritte beschreiben, wie so konfigurieren Sie Geo-Replikation mithilfe von Management Studio. Schritte zum Erstellen von nicht lesbaren und lesbar entweder mit einer einzelnen Datenbank oder eine Datenbank flexible sekundäre stehen zur Verfügung.

> [AZURE.NOTE] Der Befehl schlägt fehl, wenn Sie eine Datenbank auf dem Server angegebene Partner mit demselben Namen wie die primäre Datenbank vorhanden ist.


### <a name="add-non-readable-secondary-single-database"></a>Hinzufügen von nicht lesbaren sekundären (einzelne Datenbank)

Verwenden Sie die folgenden Schritte aus, um eine nicht lesbaren sekundäre als einzelne Datenbank zu erstellen.

1. Verwenden die Version 13.0.600.65 oder höher von SQL Server Management Studio.

     > [AZURE.IMPORTANT] Laden Sie die [neueste](https://msdn.microsoft.com/library/mt238290.aspx) Version von SQL Server Management Studio. Es wird empfohlen, dass Sie immer die neueste Version von Management Studio verwenden, um mit Updates Azure-Portal synchron bleiben.


2. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **System-Datenbanken** , mit der rechten Maustaste auf **master**, und klicken Sie dann auf **Neue Abfrage**.

3. Verwenden Sie die folgende **ALTER DATABASE** -Anweisung, um eine lokale Datenbank in einer Geo-Replikation mit einer nicht lesbaren sekundäre Datenbank auf MySecondaryServer1 primären umwandeln, in dem MySecondaryServer1 den Servernamen geeignet ist.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer1> WITH (ALLOW_CONNECTIONS = NO);

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.


### <a name="add-readable-secondary-single-database"></a>Hinzufügen von lesbare sekundären (einzelne Datenbank)
Gehen Sie folgendermaßen vor, um eine lesbare sekundäre als einzelne Datenbank zu erstellen.

1. Schließen Sie in Management Studio an Ihre logischen Azure SQL-Datenbankserver.

2. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **System-Datenbanken** , mit der rechten Maustaste auf **master**, und klicken Sie dann auf **Neue Abfrage**.

3. Verwenden Sie die folgende **ALTER DATABASE** -Anweisung, um eine lokale Datenbank ein Geo-Replikation primären mit einer lesbaren sekundäre Datenbank auf einem sekundären Server zu speichern.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.



### <a name="add-non-readable-secondary-elastic-database"></a>Hinzufügen von nicht lesbaren sekundären (Flexible Datenbank)

Verwenden Sie die folgenden Schritte aus, um eine nicht lesbaren sekundäre als flexible Datenbank zu erstellen.

1. Schließen Sie in Management Studio an Ihre logischen Azure SQL-Datenbankserver.

2. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **System-Datenbanken** , mit der rechten Maustaste auf **master**, und klicken Sie dann auf **Neue Abfrage**.

3. Verwenden Sie die folgende **ALTER DATABASE** -Anweisung, um eine lokale Datenbank in einer Geo-Replikation mit einer nicht lesbaren sekundäre Datenbank auf einem sekundären Server in einem Pool flexible primären zu machen.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer3> WITH (ALLOW_CONNECTIONS = NO
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool1));

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.



### <a name="add-readable-secondary-elastic-database"></a>Hinzufügen von lesbare sekundären (Flexible Datenbank)
Verwenden Sie die folgenden Schritte aus, um einem lesbaren sekundären als flexible Datenbank zu erstellen.

1. Schließen Sie in Management Studio an Ihre logischen Azure SQL-Datenbankserver.

2. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **System-Datenbanken** , mit der rechten Maustaste auf **master**, und klicken Sie dann auf **Neue Abfrage**.

3. Verwenden Sie die folgende **ALTER DATABASE** -Anweisung, um eine lokale Datenbank in einer Geo-Replikation mit einer lesbaren sekundäre Datenbank auf einem sekundären Server in einem Pool flexible primären zu machen.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.



## <a name="remove-secondary-database"></a>Sekundäre Datenbank entfernen

**ALTER DATABASE** -Anweisung können Sie um die Replikation Zusammenarbeit zwischen einer sekundären Datenbank und primärem dauerhaft zu beenden. Klicken Sie auf die master-Datenbank auf dem primäre Datenbank befindet diese Anweisung ausgeführt. Nach Beendigung der Beziehung wird die sekundäre Datenbank eine reguläre Lese-und Schreibzugriff-Datenbank. Wenn die Verbindung zu sekundäre Datenbank fehlerhaft ist der Befehl erfolgreich ausgeführt wird, aber des sekundären werden erst Lese-und Schreibzugriff, nach der Wiederherstellung Connectivity. Weitere Informationen finden Sie unter [ALTER DATABASE (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) und [Dienst Ebenen](sql-database-service-tiers.md).

Verwenden Sie die folgenden Schritte aus, um sekundären Geo repliziert aus einer Geo Replikationspartnerschaft entfernen.

1. Schließen Sie in Management Studio an Ihre logischen Azure SQL-Datenbankserver.

2. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **System-Datenbanken** , mit der rechten Maustaste auf **master**, und klicken Sie dann auf **Neue Abfrage**.

3. Verwenden Sie die folgende **ALTER DATABASE** -Anweisung, um eine sekundäre Geo repliziert zu entfernen.

        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.

## <a name="monitor-geo-replication-configuration-and-health"></a>Gesundheit und Monitor Geo-Replikation über den Computer

Überwachung Aufgaben aufnehmen der Konfiguration Geo-Replikation für die Überwachung und Daten Replikationszustand überwachen.  Die **sys.dm_geo_replication_links** Management dynamische Ansicht können in der Datenbank master, um Informationen zu allen vorhandenen Replikation Links für jede Datenbank auf die logischen Azure SQL-Datenbankserver zurückzukehren. Diese Ansicht enthält eine Zeile für jeden der Replikation Verknüpfung zwischen primären und sekundären Datenbanken. Die **sys.dm_replication_link_status** Management dynamische Ansicht können eine Zeile für jede Azure SQL-Datenbank zurückgegeben, die derzeit in einem Replikation Replikation Link aktiviert ist. Dies umfasst primäre und sekundäre Datenbanken. Wenn mehr als eine fortlaufende Replikation Verknüpfung für eine bestimmte primäre Datenbank vorhanden ist, enthält diese Tabelle eine Zeile für jede der Beziehungen. Die Sicht ist in allen Datenbanken, einschließlich des logischen Masters erstellt. Diese Ansicht im logischen Master Abfragen werden jedoch eine leere Menge zurückgegeben. Die **sys.dm_operation_status** Management dynamische Ansicht können den Status für alle Datenbankvorgänge, einschließlich des Status der Replikation Links angezeigt. Weitere Informationen finden Sie unter [sys.geo_replication_links (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm_geo_replication_link_status (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/mt575504.aspx)und [sys.dm_operation_status (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/dn270022.aspx).

Gehen Sie folgendermaßen vor, um eine Zusammenarbeit Geo-Replikation überwachen.

1. Schließen Sie in Management Studio an Ihre logischen Azure SQL-Datenbankserver.

2. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **System-Datenbanken** , mit der rechten Maustaste auf **master**, und klicken Sie dann auf **Neue Abfrage**.

3. Verwenden Sie die folgende Anweisung, um alle Datenbanken mit Geo-Replikation Links anzuzeigen.

        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM [sys].geo_replication_links;

4. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.
5. Öffnen Sie den Ordner Datenbanken, erweitern Sie den Ordner **System-Datenbanken** , mit der rechten Maustaste auf **MyDB**, und klicken Sie dann auf **Neue Abfrage**.
6. Verwenden Sie die folgende Anweisung zum Anzeigen der Replikation-fällt ab und letzten Replikation meiner sekundäre Datenbanken von MyDB dauern.

        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status

7. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.
8. Verwenden Sie die folgende Anweisung, um die letzte Datenbank MyDB zugeordneten Geo-Replikation-Vorgänge anzuzeigen.

        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC

9. Klicken Sie auf **Ausführen** , um die Abfrage auszuführen.

## <a name="upgrade-a-non-readable-secondary-to-readable"></a>Aktualisieren von einem nicht lesbaren sekundären auf lesbar

In April 2017 nicht lesbaren sekundäre Typ gelöscht werden, und vorhandene nicht lesbaren Datenbanken, um lesbare sekundäre automatisch aktualisiert werden. Wenn Sie nicht lesbaren sekundäre heute und aktualisieren Sie sie, um gelesen werden sollen, können Sie die folgenden einfachen Schritte für jedes sekundären verwenden.

> [AZURE.IMPORTANT] Es gibt keine Self-service-Methode der in-situ-Upgrade von einem nicht lesbaren sekundären zu lesbar. Wenn Sie Ihre nur sekundäre ablegen, klicken Sie dann bleibt die primäre Datenbank ungeschützte, bis die neue sekundäre vollständig synchronisiert wird. Wenn der Anwendung Vereinbarung zum SERVICELEVEL erfordert, dass die primäre immer geschützt ist, sollten Sie eine sekundäre parallele in einem anderen Server erstellen, bevor Sie das Upgrade der oben aufgeführten Schritte. Beachten Sie, dass jede primär von 4 sekundäre Datenbanken verwalten kann.


1. Zunächst Verbinden mit dem *sekundären* Server, und legen Sie die nicht lesbaren sekundäre Datenbank ab:  
        
        DROP DATABASE <MyNonReadableSecondaryDB>;

2. Jetzt eine Verbindung mit dem *primären* Server und hinzufügen einen neuen lesbaren sekundären

        ALTER DATABASE <MyDB>
            ADD SECONDARY ON SERVER <MySecondaryServer> WITH (ALLOW_CONNECTIONS = ALL);




## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu aktiven Geo-Replikation finden Sie unter - [Active Geo-Replikation](sql-database-geo-replication-overview.md)
- Eine Übersicht über Business-Continuity und Szenarien finden Sie unter [Übersicht über die Business continuity](sql-database-business-continuity.md)
