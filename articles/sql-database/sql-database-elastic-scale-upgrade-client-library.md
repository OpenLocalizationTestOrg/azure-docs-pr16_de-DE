< Eigenschaften
    
    pageTitle="Upgrade to the latest elastic database client library | Microsoft Azure" 
    description="Upgrade apps and libraries using Nuget" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>Upgraden einer app die neuesten flexible Datenbank-Client-Bibliothek

Neue Versionen der [flexible Datenbank-Client-Bibliothek](sql-database-elastic-database-client-library.md) sind durch NuGetand der NuGetPackage Manager-Benutzeroberfläche in Visual Studio verfügbar. Mehrfarbige enthalten Updates und Unterstützung für neue Funktionen der Clientbibliothek.

**Nach der neuesten Version:** Wechseln Sie zu [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Erneutes Erstellen der Anwendung mit der neuen Bibliothek sowie Ändern der Metadaten Ihrer vorhandenen Shard Karte Manager in Ihren Azure SQL-Datenbanken zur Unterstützung von neuen Features gespeichert.

Ausführen dieser Schritte in der Reihenfolge wird sichergestellt, dass die alte Clientbibliothek-Versionen nicht mehr vorhanden sind in Ihrer Umgebung Wenn Metadatenobjekte aktualisiert werden, was bedeutet, dass die alte Version Metadatenobjekte nach dem Upgrade erstellt werden, wird nicht.   

## <a name="upgrade-steps"></a>Upgrade vor

**1. upgrade Ihrer Anwendung.** In Visual Studio herunterladen und verweisen auf die neueste Version des Client-Bibliothek in alle Ihre Entwicklungsprojekte, die die Bibliothek verwenden; Klicken Sie dann neu erstellen und bereitstellen. 

 * Wählen Sie in der Visual Studio-Lösung **Tools**aus --> **NuGet Package Manager** -->  **NuGet-Pakete verwalten, für die Lösung**. 
 * (Visual Studio-2013) Klicken Sie im linken Bereich die Option **Updates**aus, und wählen Sie dann auf die Schaltfläche **Aktualisieren** , klicken Sie auf das Paket **Azure SQL Datenbank flexible skalieren Client-Bibliothek** , die im Fenster angezeigt wird.
 * (Visual Studio 2015) Legen Sie das Feld Filter ein Upgrade **verfügbar**. Wählen Sie das Paket aktualisieren aus, und klicken Sie auf die Schaltfläche **Aktualisieren** .
    
 
 * Erstellen und bereitstellen. 

**2. Ihre Skripts zu aktualisieren.** Wenn Sie mehrere Shards hinweg, [Laden Sie die neue Bibliotheksversion](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) verwalten, und kopieren Sie ihn in das Verzeichnis, aus dem Sie Skripts ausführen, **PowerShell** -Skripts verwenden. 

**3 aktualisieren Sie 3 Ihrem Dienst Teilen und zusammenführen.** Wenn Sie das flexible Teilen und Zusammenführen Datenbanktool verwenden, um sharded Daten, [herunterladen und Bereitstellen von die neueste Version des Tools](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)neu zu organisieren. Detaillierte Upgrade Schritte aus, für der Dienst gefunden werden kann [hier](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. upgrade Ihrer Shard Karte Manager-Datenbanken**. Aktualisieren Sie die Metadaten Ihrer Shard Karten in Azure SQL-Datenbank unterstützen.  Es gibt zwei Möglichkeiten, die Sie diesem Zweck können mithilfe der PowerShell oder c# aus. Beide Optionen sind nachstehend aufgeführt.

***Option 1: Upgrade Metadaten mithilfe der PowerShell***

1. Das neueste Befehlszeilendienstprogramm für NuGet aus [hier](http://nuget.org/nuget.exe) herunterladen und in einem Ordner speichern. 

2. Öffnen Sie ein Eingabeaufforderungsfenster, navigieren Sie zu dem Ordner, und geben Sie den Befehl aus:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`

3. Navigieren Sie zum Unterordner, enthält der neuen Client-DLL-Version, die Sie soeben, die beispielsweise heruntergeladen haben:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`

4. Laden Sie die Datenbank flexible Client Upgrade Scriptlet aus dem [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9), und speichern Sie es in denselben Ordner, der die DLL enthält.

5. Führen Sie aus diesem Ordner aus "PowerShell.\upgrade.ps1" über die Befehlszeile, und folgen Sie den Anweisungen.
 
***Option 2: Upgrade Metadaten mit c#***

Sie können auch Erstellen einer Visual Studio-Anwendungs, die Ihre ShardMapManager öffnet, durchläuft alle mehrere Shards hinweg und führt die Aktualisierung der Metadaten, indem Sie die Methoden [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) und [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) wie im folgenden Beispiel: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 
    
    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Diese Techniken für Metadaten Upgrades können ohne Schaden mehrmals angewendet werden. Beispielsweise, wenn eine ältere Clientversion versehentlich eine Shard erstellt, nachdem Sie bereits aktualisiert haben, können Sie ausführen Upgrade erneut über alle mehrere Shards hinweg, um sicherzustellen, dass die neueste Metadatenversion in der gesamten Infrastruktur vorhanden ist. 

**Hinweis:**  Neue Versionen der Clientbibliothek veröffentlicht bis-heute weiterhin in früheren Versionen von Metadaten Shard Karte Manager Azure SQL-Datenbank, und umgekehrt arbeiten.   Jedoch einige der neuen Features in der neuesten Client nutzen, Metadaten muss aktualisiert werden.   Beachten Sie, dass alle Benutzerdaten oder anwendungsspezifische Daten, die nur Objekte, die von der Shard Karte-Manager erstellt und verwendet Metadaten Upgrades nicht beeinträchtigt werden.  Und Applikationen weiterhin durch die oben beschriebenen Upgrade Abfolge ausgeführt werden. 

## <a name="elastic-database-client-version-history"></a>Flexible Datenbank-Client-Versionsverlauf 

Versionsverlauf finden Sie unter [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]  


<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png
 