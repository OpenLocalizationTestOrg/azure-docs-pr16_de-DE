<properties 
    pageTitle="Verwalten von Anmeldeinformationen in der Datenbank flexible Client-Bibliothek | Microsoft Azure" 
    description="So legen Sie die richtige Ebene der Anmeldeinformationen, Administrator als schreibgeschützt für flexible Datenbank-apps" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Zugriff auf die Bibliothek flexible Datenbank Client verwendeten Anmeldeinformationen

[Flexible Datenbank-Client-Bibliothek](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) verwendet drei verschiedene Arten von Anmeldeinformationen, um die [Shard Karte Manager](sql-database-elastic-scale-shard-map-management.md)zuzugreifen. Je nach werden müssen verwenden Sie die Anmeldeinformationen mit den niedrigsten Zugriffsebene möglich aus.

* **Verwaltung von Anmeldeinformationen**: zum Erstellen oder Bearbeiten von a-Manager Shard-Karte. (Siehe [Glossar](sql-database-elastic-scale-glossary.md)). 
* **Anmeldeinformationen für den Zugriff**: Zugriff auf eine vorhandene Shard Karte Manager, um Informationen über mehrere Shards hinweg erhalten.
* **Anmeldeinformationen der Verbindung**: Verbindung zum mehrere Shards hinweg. 

Siehe auch [Verwalten von Datenbanken und Benutzernamen in SQL Azure-Datenbank](sql-database-manage-logins.md). 
 
## <a name="about-management-credentials"></a>Informationen zum Projektmanagement-Anmeldeinformationen

Management Anmeldeinformationen werden verwendet, um ein Objekt [**ShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) für Applikationen zu erstellen, mit denen Shard Karten bearbeitet wird. (Z. B. finden Sie unter [Hinzufügen eines Shard flexible Datenbanktools verwenden](sql-database-elastic-scale-add-a-shard.md) und [Daten abhängige routing](sql-database-elastic-scale-data-dependent-routing.md)) Der Benutzer der Clientbibliothek flexible skalieren erstellt die SQL-Benutzer und SQL-Benutzernamen und sichergestellt, dass jede die Lese-und Schreibberechtigungen auf die globale Shard Karte Datenbank und alle Shard Datenbanken gewährt wird. Diese Anmeldeinformationen werden zum Verwalten der globalen Shard Karte und die lokale Shard Karten, wenn Änderungen an der Karte Shard ausgeführt werden. Verwenden Sie die Anmeldeinformationen für die Verwaltung beispielsweise zum Erstellen des Shard Karte Manager-Objekts (bei [**GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)an: 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

Die Variable **SmmAdminConnectionString** ist eine Verbindungszeichenfolge, die die Verwaltung Anmeldeinformationen enthält. Die Benutzer-ID und das Kennwort bietet Lese-und Schreibzugriff Shard Karte Datenbank und einzelne mehrere Shards hinweg. Die Verwaltung Verbindungszeichenfolge enthält darüber hinaus den Servernamen und den Datenbanknamen zum Identifizieren der globalen Shard Karte Datenbank ein. So sieht eine typische Verbindungszeichenfolge für diesen Zweck:

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

Verwenden Sie keine Werte in Form von "username@server"—instead nur den Wert "Username" verwenden.  Dies liegt daran Anmeldeinformationen sowohl in einzelne mehrere Shards hinweg, die auf verschiedenen Servern möglicherweise die Shard Karte Manager-Datenbank arbeiten müssen.

## <a name="access-credentials"></a>Access-Anmeldeinformationen
  
Verwenden Sie eine Shard Karte-Manager in einer Anwendung, die nicht von Shard Karten verwalten beim Erstellen Anmeldeinformationen, die schreibgeschützt Berechtigungen auf der Karte Globale Shard aufweisen. Die Daten aus der globalen Shard Karte unter diese Anmeldeinformationen werden verwendet, für das [routing von Daten-abhängige](sql-database-elastic-scale-data-dependent-routing.md) und den Shard Karte Cache auf dem Client füllen. Die Anmeldeinformationen werden durch das gleiche Muster für den Anruf zu **GetSqlShardMapManager** bereitgestellt, wie oben gezeigt: 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

Beachten Sie die Verwendung von der **SmmReadOnlyConnectionString** , um die Verwendung von anderen Anmeldeinformationen für diesen Zugriff für Benutzer **ohne Administratorrechte** wiederzugeben: Diese Anmeldeinformationen sollte keine Schreibberechtigung auf der Karte Globale Shard bereit. 

## <a name="connection-credentials"></a>Anmeldeinformationen der Verbindung 

Bei Verwendung der [**OpenConnectionForKey**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) -Methode, um eine Shard mit einem Sharding Schlüssel verknüpften zuzugreifen, sind zusätzliche Anmeldeinformationen erforderlich. Diese Anmeldeinformationen müssen Berechtigungen für schreibgeschützten Zugriff auf die lokale Shard Karte Tabellen, die auf die Shard bereitstellen. Dies ist erforderlich, die Verbindung Validierung für das routing von Daten-abhängige auf die Shard. Dieser Codeausschnitt ermöglicht Zugriff auf Daten im Zusammenhang mit Daten abhängige routing an: 
 
    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

In diesem Beispiel enthält **SmmUserConnectionString** die Verbindungszeichenfolge für die Anmeldeinformationen des Benutzers an. Für SQL Azure DB sieht eine typische Verbindungszeichenfolge für Benutzeranmeldeinformationen aus: 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

Wie bei der Administrator-Anmeldeberechtigungen nicht in Form von Werten "username@server". Verwenden Sie stattdessen einfach "Username".  Beachten Sie auch, dass die Verbindungszeichenfolge keinen Servernamen und den Datenbanknamen enthält. Dies liegt daran der Anruf **OpenConnectionForKey** automatisch die Verbindung zu den richtigen Shard anhand des Schlüssels direkte wird. Daher sind den Datenbanknamen und den Servernamen nicht verfügbar. 

## <a name="see-also"></a>Siehe auch
[Verwalten von Datenbanken und Anmeldedaten in Azure SQL-Datenbank](sql-database-manage-logins.md)

[Sichern Ihrer SQL­Datenbank](sql-database-security.md)

[Erste Schritte mit flexible Datenbank Aufträge](sql-database-elastic-jobs-getting-started.md)

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 