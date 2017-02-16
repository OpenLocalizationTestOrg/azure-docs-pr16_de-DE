<properties 
    pageTitle="Daten abhängige routing | Microsoft Azure" 
    description="Verwenden Sie die ShardMapManager-Klasse in .NET apps für Daten-abhängige routing, ein Feature von flexible Datenbanken für SQL Azure-Datenbank" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

#<a name="data-dependent-routing"></a>Daten abhängige routing

**Daten abhängige routing** ist die Möglichkeit, die Daten in einer Abfrage verwenden, um die Anforderung zu einer geeigneten Datenbank weiterzuleiten. Dies ist eine grundlegende Muster beim Arbeiten mit Datenbanken sharded. Der Anforderungskontext möglicherweise auch die Anfrage weitergeleitet verwendet werden, insbesondere dann, wenn die Taste Sharding nicht als Teil der Abfrage ist. Jede bestimmte Abfrage oder Transaktion in einer Anwendung mit Daten abhängige routing ist auf den Zugriff auf eine einzelne Datenbank pro Anforderung beschränkt. Für die Azure SQL-Datenbank flexible Tools wird diese routing der **[Klasse ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)** in ADO.NET Applications erzielt.

Die Anwendung muss nicht zum Nachverfolgen von verschiedenen Verbindungszeichenfolgen oder anderen Segmente von Daten in der Umgebung sharded zugeordneten DB-Speicherorte. Stattdessen geöffnet wird, die [Shard Karte Manager](sql-database-elastic-scale-shard-map-management.md) Verbindungen auf die richtigen Datenbanken Wenn erforderlich, basierend auf der Registerkarte Daten in der Shard Karte und den Wert des Schlüssels Sharding, die das Ziel der Anforderung der Anwendung ist. Der Schlüssel ist normalerweise der *Customer_id*, *Tenant_id*, *Date_key*oder einige andere grundlegende Parameter der Datenbankanfrage ist bestimmten Kennzeichen). 

Weitere Informationen finden Sie unter [Skalieren von SQL Server mit Daten abhängige Routing](https://technet.microsoft.com/library/cc966448.aspx).

## <a name="download-the-client-library"></a>Herunterladen der Clientbibliothek

Um die Klasse zu gelangen, installieren Sie die [Flexible Datenbank-Client-Bibliothek](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)aus. 

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a>Verwenden eine ShardMapManager in einer Daten abhängige routing-Anwendung 

Die **ShardMapManager** sollte bei der Initialisierung, verwenden den Anruf Factory **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**Applikationen instanziiert werden. In diesem Beispiel werden sowohl in einem **ShardMapManager** einer bestimmten **ShardMap** , die darin enthaltenen Initialisierung. Dieses Beispiel zeigt die Methoden GetSqlShardMapManager und [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) .

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, 
                      ShardMapManagerLoadPolicy.Lazy);
    RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 

### <a name="use-lowest-privilege-credentials-possible-for-getting-the-shard-map"></a>Verwenden Sie zum Abrufen der Karte Shard niedrigste Berechtigung Anmeldeinformationen möglich

Wenn eine Anwendung nicht Shard Karte selbst Bearbeiten von ist, sollte die Anmeldeinformationen in der Factory-Methode verwendet nur schreibgeschützt Berechtigungen der **Globalen Shard Karte** Datenbank verfügen. Diese Anmeldeinformationen unterscheiden sich in der Regel von Anmeldeinformationen verwendet, um Verbindungen mit der Shard Karte-Manager zu öffnen. Siehe auch [Anmeldeinformationen verwendet, um die flexible Datenbank-Client-Bibliothek zugreifen](sql-database-elastic-scale-manage-credentials.md). 

## <a name="call-the-openconnectionforkey-method"></a>Rufen Sie die OpenConnectionForKey-Methode

Die ** [ShardMap.OpenConnectionForKey Methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx))** gibt eine ADO.NET-Verbindung zur Ausgabe von Befehlen zur entsprechenden Datenbank basierend auf dem Wert des Parameters **Schlüssel** bereit. Shard Informationen werden in der Anwendung durch die **ShardMapManager**, zwischengespeichert, damit diese Anfragen nicht in der Regel eine Datenbank nachschlagen für die Datenbank **Globale Shard Karte** beinhalten. 

    // Syntax: 
    public SqlConnection OpenConnectionForKey<TKey>(
        TKey key,
        string connectionString,
        ConnectionOptions options
    )


* **Key** -Parameter wird als ein Schlüssel zum Nachschlagen in der Karte Shard verwendet, um die entsprechende Datenbank für die Anforderung zu bestimmen. 

* **ConnectionString** wird verwendet, um nur die Anmeldeinformationen des Benutzers für die gewünschte Verbindung zu übergeben. Keine Datenbank- oder Servernamens in dieser *ConnectionString* enthalten sind, da die Methode die Datenbanken und Server mithilfe der **ShardMap**bestimmt wird. 

* Wenn eine Umgebung, in dem Shard Maps, möglicherweise ändern und Zeilen möglicherweise wechseln zu anderen Datenbanken als Ergebnis teilen oder Zusammenführen Vorgänge sollten **ConnectionOptions.Validate** der **[ConnectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)** festgelegt werden. Dies umfasst eine kurze Abfrage in der lokalen Shard Karte auf das Ziel Database-(nicht in der globalen Shard Karte), bevor Sie die Verbindung zur Anwendung übermittelt wird. 

Fehlschlagen die Validierung gegen die lokale Shard Karte (d. h., dass der Cache falsch ist) Abfrage der Shard Karte-Manager die globale Shard-Karte, um den richtigen neuen Wert für die Suche zu erhalten, aktualisieren Sie den Cache, und erhalten und die entsprechenden datenbankverbindung zurückzukehren. 

Verwenden Sie **ConnectionOptions.None** nur bei Shard Zuordnung Änderungen nicht erwartet werden, während eine Anwendung online ist. In diesem Fall die zwischengespeicherten Werte können als immer in Ordnung, und der Anruf zusätzliche Roundtrip Überprüfung in die Zieldatenbank sicheres übersprungen werden kann. Die verringert den Datenverkehr im Datenbank. Die **ConnectionOptions** möglicherweise auch über einen Wert festgelegt werden, in einer Konfigurationsdatei, um anzugeben, ob Sharding Änderungen erwartet werden oder während einer bestimmten Zeitspanne nicht.  

In diesem Beispiel wird den Wert eine ganze Zahl Schlüssels **CustomerID**, mit einem **ShardMap** -Objekt mit dem Namen **CustomerShardMap**.  

    int customerId = 12345; 
    int newPersonId = 4321; 

    // Connect to the shard for that customer ID. No need to call a SqlConnection 
    // constructor followed by the Open method.
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, 
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
    { 
        // Execute a simple command. 
        SqlCommand cmd = conn.CreateCommand(); 
        cmd.CommandText = @"UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID"; 

        cmd.Parameters.AddWithValue("@customerID", customerId); 
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId); 
        cmd.ExecuteNonQuery(); 
    }  

Die Methode **OpenConnectionForKey** gibt eine neue bereits geöffneten Verbindung mit der richtigen Datenbank ein. Auf diese Weise genutzt Verbindungen nutzen noch ADO.Net Verbindungspooling. Solange Transaktionen und Besprechungsanfragen von einer Shard nacheinander erfüllt werden können, sollte dies der einzige Änderung in einer Anwendung, die bereits mit ADO.Net erforderlich sein. 

Die **[OpenConnectionForKeyAsync Methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)** ist ebenfalls verfügbar, wenn eine Anwendung asynchrone Programmierung mit ADO.Net macht verwenden. Das Verhalten ist die Daten abhängige routing von ADO entspricht. Netto s **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688(v=vs.110).aspx)** Methode.

## <a name="integrating-with-transient-fault-handling"></a>Integration mit vorübergehenden Fehlerbehandlung 

Bewährte Methode bei der Entwicklung Daten-Access-Applikationen in der Cloud wird sichergestellt, dass vorübergehenden Fehler abgefangen werden, indem Sie die app, und die Vorgänge mehrmals wiederholt werden, bevor ein Fehler ausgelöst. Vorübergehende Fehlerbehandlung für Applikationen Cloud wird bei der [Behandlung von vorübergehenden Fehlerstrukturanalyse](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx)erläutert. 
 
Vorübergehende Fehlerbehandlung kann mit den Daten abhängige Routing Muster natürlich gleichzeitig verwendet werden. Die wichtige Anforderung besteht darin, wiederholen Sie die gesamte Data Access Anforderung, einschließlich des Zeitraums mit **verwenden** , die die Daten-abhängige Weiterleitung Verbindung ermittelt. Im oben genannten Beispiel kann neu geschrieben werden, wie folgt (hervorgehobener Notiz ändern). 

### <a name="example--data-dependent-routing-with-transient-fault-handling"></a>Beispiel – mit vorübergehenden Fehlerbehandlung routing abhängige Daten 

<pre><code>int customerId = 12345; 
int newPersonId = 4321; 

<span style="background-color:  #FFFF00">Configuration.SqlRetryPolicy.ExecuteAction(() =&gt; </span> 
<span style="background-color:  #FFFF00">    { </span>
        // Connect to the shard for a customer ID. 
        using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId,  
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
        { 
            // Execute a simple command 
            SqlCommand cmd = conn.CreateCommand(); 

            cmd.CommandText = @&quot;UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID&quot;; 

            cmd.Parameters.AddWithValue(&quot;@customerID&quot;, customerId); 
            cmd.Parameters.AddWithValue(&quot;@newPersonID&quot;, newPersonId); 
            cmd.ExecuteNonQuery(); 

            Console.WriteLine(&quot;Update completed&quot;); 
        } 
<span style="background-color:  #FFFF00">    }); </span> 
</code></pre>


Zum vorübergehenden Fehlerbehandlung implementieren notwendigen Pakete werden automatisch heruntergeladen werden, beim Erstellen der Anwendungs flexible Datenbank Stichprobe. Darüber hinaus stehen Pakete separat in [Enterprise Library - vorübergehende Fehlerstrukturanalyse Handling Application Block](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)zur Verfügung. Verwenden Sie die Version 6.0 oder höher. 

## <a name="transactional-consistency"></a>Konsistenz von Transaktionen 

Transaktionen Eigenschaften sind für alle Vorgänge lokal für ein Shard garantiert. Führen Sie beispielsweise über Daten-abhängige Weiterleitung Übermittelte Transaktionen innerhalb des Gültigkeitsbereichs von der Ziel-Shard für die Verbindung ein. Zu diesem Zeitpunkt, es gibt keine Funktionen, die für mehrere Verbindungen in eine Transaktion eintragen und daher es gibt keine Transaktionen Garantie für Vorgänge über mehrere Shards hinweg ausgeführt.

## <a name="next-steps"></a>Nächste Schritte
Trennen einer Shard oder einer Shard erneut an, finden Sie unter [Verwenden der Klasse RecoveryManager Shard Karte Probleme beheben](sql-database-elastic-database-recovery-manager.md)

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
