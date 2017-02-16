<properties 
    pageTitle="Mit dem Wiederherstellung-Manager Shard Karte Probleme beheben | Microsoft Azure" 
    description="Verwenden Sie die RecoveryManager-Klasse zum Lösen von Problemen mit Shard Karten" 
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
    ms.date="05/05/2016" 
    ms.author="ddove"/>

# <a name="using-the-recoverymanager-class-to-fix-shard-map-problems"></a>Mithilfe der Klasse RecoveryManager Shard Karte Probleme beheben

Die Klasse [RecoveryManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.aspx) stellt ADO.Net Applications die Möglichkeit, leichter zu erkennen und beheben alle Inkonsistenzen zwischen der globalen Shard Karte (g/m2) und die lokale Shard Karte (LSM) in einer Umgebung sharded Datenbank. 

Die g/m2 und LSM verfolgen die Zuordnung der einzelnen Datenbanken in einer sharded-Umgebung. Gelegentlich, tritt ein Seitenumbruch zwischen den g/m2 und der LSM ein. Verwenden Sie in diesem Fall die RecoveryManager-Klasse zu erkennen und reparieren den Seitenumbruch.

Die RecoveryManager-Klasse ist Teil der [flexible Datenbank-Client-Bibliothek](sql-database-elastic-database-client-library.md). 


![Shard Karte][1]


Ausdruck Definitionen finden Sie unter [Tools-Glossar flexible Datenbank](sql-database-elastic-scale-glossary.md). Um zu verstehen, wie die **ShardMapManager** zum Verwalten von Daten in einer sharded Lösung verwendet wird, finden Sie unter [Shard Management zuordnen](sql-database-elastic-scale-shard-map-management.md).


## <a name="why-use-the-recovery-manager"></a>Gründe für die Verwendung des Wiederherstellung-Managers

In einer sharded Datenbank-Umgebung gibt es einen Mandanten pro Datenbank und viele Datenbanken pro Server. Es kann auch viele Server in der Umgebung sein. Jede Datenbank wird in der Zuordnung Shard zugeordnet, damit Anrufe an den richtigen Server und die Datenbank weitergeleitet werden können. Datenbanken nach einem **Sharding Schlüssel**erfasst werden, und jedes Shard einen **Zellbereich Schlüsselwerte**zugeordnet ist. Beispielsweise kann ein Schlüssel Sharding die Namen der Kunden von "D" bis "F" dargestellt werden Das Zuweisen von allen mehrere Shards hinweg (QuickInfos Datenbanken) und deren Zuordnung Bereiche in der **globalen Shard Karte (g/m2)**enthalten sind. Jede Datenbank enthält auch ein Schema enthalten ist, klicken Sie auf die Shard Bereiche – Dies wird als die **lokale Shard zuordnen (LSM)**bezeichnet. Wenn eine app mit einem Shard verbunden ist, wird die Zuordnung mit der app für den schnellen Abruf zwischengespeichert. Die LSM wird verwendet, um zwischengespeicherte Daten zu überprüfen. 

Die g/m2 und LSM kann aus folgenden Gründen nicht synchron sein:

1. Das Löschen einer Shard, deren Bereich anscheinend nicht mehr verwendet oder Umbenennen von einem Shard werden. Löschen einer Shard Ergebnisse in eine **verwaiste Shard Zuordnung**an. Similary, eine umbenannte Datenbank kann eine Zuordnung verwaiste Shard verursachen. Je nach den Zweck der Änderung die Shard entfernt werden müssen, oder der Speicherort Shard aktualisiert werden soll. Zum Wiederherstellen einer gelöschten Datenbank finden Sie unter [Wiederherstellen einer Datenbank zu einem bestimmten Zeitpunkt vorherigen, Wiederherstellen einer gelöschte Datenbank, oder aus einem Data Center Ausfall wiederherstellen](sql-database-troubleshoot-backup-and-restore.md).
2. Ein Geo-Failover-Ereignis eintritt. Um den Vorgang fortzusetzen, muss eine aktualisieren den Servernamen und den Datenbanknamen Shard Karte-Manager in der Anwendung und aktualisieren Sie die Shard Zuordnung Details für alle mehrere Shards hinweg in einer Karte Shard. Bei einem Geo Failover sollte solche Wiederherstellungslogik innerhalb des Workflows Failover automatisieren. Automatisieren von Wiederherstellungsaktionen ermöglicht eine frictionless Verwaltbarkeit für Datenbanken Geo aktiviert und vermieden werden manuelle personenbezogenen Aktionen.
3. Entweder ein Shard oder die Datenbank ShardMapManager wird eine zuvor Punkt in Zeit wiederhergestellt.

Weitere Informationen zu Azure SQL-Datenbank flexible Datenbanktools Geo-Replikation und wiederherstellen, finden Sie die folgenden: 

* [Übersicht: Cloud Business Continuity und Datenbank Wiederherstellung mit SQL-Datenbank](sql-database-business-continuity.md) 
* [Erste Schritte mit flexible Datenbanktools](sql-database-elastic-scale-get-started.md)  
* [ShardMap Verwaltung](sql-database-elastic-scale-shard-map-management.md)

## <a name="retrieving-recoverymanager-from-a-shardmapmanager"></a>Abrufen von RecoveryManager aus einer ShardMapManager 

Der erste Schritt besteht im Erstellen einer RecoveryManager-Instanz. Die [Methode GetRecoveryManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getrecoverymanager.aspx) gibt das Wiederherstellung-Manager für die aktuelle Instanz von [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) . Akzeptieren, um alle Inkonsistenzen benachrichtigt, in der Shard-Karte zu beheben, müssen Sie zuerst die RecoveryManager für die Karte bestimmten Shard abrufen. 

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString,  
             ShardMapManagerLoadPolicy.Lazy);
             RecoveryManager rm = smm.GetRecoveryManager(); 

In diesem Beispiel wird die RecoveryManager aus der ShardMapManager Initialisierung. Die mit einem ShardMap ShardMapManager ist auch noch Initialisierung. 

Da dieser Anwendungscode die Shard Karte selbst bearbeitet, sollten die Anmeldeinformationen in der Factory-Methode (im obigen Beispiel, SmmConnectionString) verwendet Anmeldeinformationen, die Berechtigungen Lese-und Schreibzugriff auf die g/m2 Datenbank optimiert die Verbindungszeichenfolge aufweisen. Diese Anmeldeinformationen unterscheiden sich in der Regel von Anmeldeinformationen verwendet, um für das routing von Daten-abhängige Verbindungen zu öffnen. Weitere Informationen finden Sie unter [Verwenden von Anmeldeinformationen im Datenbankclient flexible](sql-database-elastic-scale-manage-credentials.md).

## <a name="removing-a-shard-from-the-shardmap-after-a-shard-is-deleted"></a>Entfernen eines Shard die ShardMap nach dem Löschen einer shard

Die [Methode DetachShard](https://msdn.microsoft.com/library/azure/dn842083.aspx) trennt die angegebenen Shard aus der Zuordnung Shard und löscht Zuordnungen der Shard zugeordnet.  

* Der Parameter Speicherort ist Shard Standort, insbesondere Servernamen und den Datenbanknamen, von der Shard getrennt werden. 
* Der Parameter ShardMapName ist der Name Shard zuordnen. Diese Option ist nur erforderlich sind, wenn mehrere Shard Karten vom gleichen Shard Karte Manager verwaltet werden. Optional. 

**Wichtig**: Verwenden Sie dieses Verfahren nur, wenn Sie sicher, dass der Bereich für sind die aktualisierte Zuordnung leer ist. Die oben genannten Methoden prüfen nicht Daten für den Bereich verschoben wird, damit die Prüfungen in Ihren Code aufnehmen möchten, empfiehlt es sich.

In diesem Beispiel werden mehrere Shards hinweg aus der Zuordnung Shard entfernt. 

    rm.DetachShard(s.Location, customerMap); 

Die Zuordnung der Shard Position in der g/m2 vor dem Löschen der Shard. Da die Shard gelöscht wurde, wird davon ausgegangen, dies beabsichtigt war, und der wichtigsten Sharding Bereich ist nicht mehr verwendet. Wenn dies nicht der Fall ist, können Sie den Punkt in Time-Wiederherstellung ausführen. Wiederherstellen eines der Shard aus einer früheren Point-in-Time. (In diesem Fall lesen Sie den Abschnitt unten, um Shard Inkonsistenzen zu erkennen.) Zum Wiederherstellen, finden Sie unter [Wiederherstellen einer Datenbank zu einem bestimmten Zeitpunkt vorherigen, Wiederherstellen einer gelöschte Datenbank, oder aus einem Data Center Ausfall wiederherstellen](sql-database-troubleshoot-backup-and-restore.md).

Da, dass die Datenbank Löschung absichtlich ausgeführt wurde angenommen wird, ist die endgültige administrative Aufräumen Aktion Eintrags, der die Shard im Shard Karte Manager zu löschen. Dies verhindert, dass die Anwendung versehentlich Schreiben von Daten in einem Bereich, die nicht erwartet wird.

## <a name="to-detect-mapping-differences"></a>Um die Zuordnung Unterschiede zu ermitteln 

Die [Methode DetectMappingDifferences](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.detectmappingdifferences.aspx) markiert und gibt einen von der Shard-Karten (lokale oder globale) als Quelle der Wahrheitswert und gleicht Zuordnungen auf beide Shard Karten (g/m2 und LSM).

    rm.DetectMappingDifferences(location, shardMapName);

* Der *Speicherort* gibt den Servernamen und den Datenbanknamen. 
* Der Parameter *ShardMapName* ist der Name des Shard zuordnen. Diese Option ist nur erforderlich, wenn mehrere Shard Karten vom gleichen Shard Karte Manager verwaltet werden. Optional. 

## <a name="to-resolve-mapping-differences"></a>Zuordnung Differenzen auflösen

Die [ResolveMappingDifferences Methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.resolvemappingdifferences.aspx) wählt eine Option aus der Shard-Karten (lokale oder globale) als Quelle der Wahrheitswert und gleicht Zuordnungen auf beide Shard Karten (g/m2 und LSM).

    ResolveMappingDifferences (RecoveryToken, MappingDifferenceResolution.KeepShardMapping);
   
* Der Parameter *RecoveryToken* Listet die Unterschiede im Zuordnungen das g/m2 und der LSM für die bestimmte Shard. 

* Der [MappingDifferenceResolution Enumeration](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.mappingdifferenceresolution.aspx) dient dazu die Methode zum Beheben des Unterschieds zwischen den Shard Zuordnungen anzugeben. 
* **MappingDifferenceResolution.KeepShardMapping** empfiehlt sich, wenn der LSM enthält die genaue Zuordnung und daher die Zuordnung in der Shard verwendet werden soll. Dies ist normalerweise der Groß-/Kleinschreibung fällt ein: die Shard jetzt auf einem neuen Server gespeichert ist. Da die Shard zuerst aus der g/m2 (mithilfe der Methode RecoveryManager.DetachShard) entfernt werden muss, ist eine Zuordnung auf der g/m2 nicht mehr vorhanden. Daher, die die LSM muss verwendet werden, um die Zuordnung Shard wieder herzustellen.

## <a name="attach-a-shard-to-the-shardmap-after-a-shard-is-restored"></a>Anfügen einer Shard zu den ShardMap nach der Wiederherstellung eines shard 

Die [AttachShard Methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.recovery.recoverymanager.attachshard.aspx) fügt der angegebenen Shard an die Karte Shard. Dann erkennt Inkonsistenzen Karte Shard und im Zuordnungen entsprechend der Shard zum Zeitpunkt der Wiederherstellung Shard aktualisiert. Es wird davon ausgegangen, dass die Datenbank entsprechend den ursprünglichen Datenbanknamen (vor Wenn der Shard wiederhergestellt wurde), auch umbenannt wird seit dem Punkt in Anzeigedauer Wiederherstellung Standardeinstellungen in eine neue Datenbank mit dem Zeitstempel angefügt. 

    rm.AttachShard(location, shardMapName) 

* Der Parameter *Speicherort* ist den Servernamen und den Datenbanknamen, der die anzufügende Shard. 

* Der Parameter *ShardMapName* ist der Name Shard zuordnen. Diese Option ist nur erforderlich sind, wenn mehrere Shard Karten vom gleichen Shard Karte Manager verwaltet werden. Optional. 

In diesem Beispiel wird die Karte Shard die zuletzt aus einer zuvor Punkt in Zeit wiederhergestellt wurde ein Shard hinzugefügt. Da der Shard (nämlich die Zuordnung für die Shard in der LSM) wiederhergestellt wurde, ist es mit dem Eintrag Shard in der g/m2 potenziell inkonsistenten. Außerhalb dieses Beispielcode wurde die Shard wiederhergestellt und in den ursprünglichen Namen der Datenbank umbenannt. Da es wiederhergestellt wurde, wird davon ausgegangen, dass die Zuordnung in der LSM den vertrauenswürdigen Zuordnung ist. 

    rm.AttachShard(s.Location, customerMap); 
    var gs = rm.DetectMappingDifferences(s.Location); 
      foreach (RecoveryToken g in gs) 
       { 
       rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
       } 

## <a name="updating-shard-locations-after-a-geo-failover-restore-of-the-shards"></a>Aktualisieren von Shard Speicherorte nach einem Geo-Failover (Wiederherstellen), der die mehrere Shards hinweg

Fällt eine Geo-die sekundäre Datenbank schreiben verfügbar gemacht und neuen primären Datenbank wird. Der Namen des Servers und potenziell auf die Datenbank (je nach Konfiguration), kann von der ursprünglichen primären abweichen. Daher müssen die Zuordnungseinträge für die Shard g/m2 und LSM konstant bleiben. Wenn die Datenbank auf einem anderen Namen oder Speicherort oder zu einem früheren Zeitpunkt wiederhergestellt wird, kann dies auf ähnliche Weise Inkonsistenzen in den Shard Karten führen. Die Shard Karte Manager übernimmt die Verteilung der geöffneten Verbindungen mit der richtigen Datenbank. Verteilung basiert auf der Registerkarte Daten in der Shard Karte und den Wert des Schlüssels Sharding, die das Ziel der Anforderung der Anwendung ist. Nach einem Geo-Failover muss diese Informationen mit genau Servername, Datenbankname und Shard Zuordnung der wiederhergestellte Datenbank aktualisiert werden. 

## <a name="best-practices"></a>Bewährte Methoden

Geo-Failover und Wiederherstellung sind Vorgänge, die von einem Administrator Cloud der Anwendung absichtlich Nutzung eines Azure SQL-Datenbanken Business Continuity Features in der Regel verwaltet. Business Continuity-Planung erfordert, Prozesse, Verfahren und Maßnahmen, um sicherzustellen, dass Business Vorgänge ohne Unterbrechung fortgesetzt werden können. Die verfügbaren Methoden basiert als Teil der Klasse RecoveryManager innerhalb dieser Workflow verwendet werden soll, um sicherzustellen, dass die g/m2 und LSM auf dem neuesten Stand gehalten werden nach der Wiederherstellungsaktion. Es gibt 5 grundlegenden Schritte ordnungsgemäß sicherzustellen, dass die g/m2 und LSM wirken sich die genaue Informationen nach einem Failover Ereignis aus. Der Anwendungscode Schritte ausführen kann in vorhandenen Tools und den Workflow integriert werden. 

1. Rufen Sie die RecoveryManager aus der ShardMapManager. 
2. Trennen Sie die alte Shard aus der Shard Karte.
3. Fügen Sie der neuen Shard an Shard Karte, einschließlich der neuen Shard Position ein.
4. Erkennen von Inkonsistenzen in die Zuordnung zwischen dem g/m2 und LSM. 
5. Beheben Sie Unterschiede zwischen der g/m2 und der LSM, die LSM vertrauen. 

In diesem Beispiel führt die folgenden Schritte aus:
1. Entfernt die Shard Speicherorte vor der Failoverereignisses widerspiegeln Shard Karte mehrere Shards hinweg.
2. Fügt mehrere Shards hinweg an die neuen Shard Speicherorten (der Parameter "Configuration.SecondaryServer" ist aber den gleichen Datenbanknamen den Namen des neuen Servers) über die entsprechenden Shard Karte an.
3. Ruft die Wiederherstellung Token durch Zuordnung Unterschiede zwischen der g/m2 und der LSM für jede Shard erkennen. 
4. Die Inkonsistenzen werden durch die Zuordnung aus der LSM der einzelnen Shard vertrauen aufgelöst. 

    Varianz mehrere Shards hinweg = Smm. GetShards();  Foreach (Shard s in mehrere Shards hinweg) {Wenn (s.Location.Server == Configuration.PrimaryServer) {ShardLocation SlNew = neue ShardLocation (Configuration.SecondaryServer, s.Location.Database) 
        
          rm.DetachShard(s.Location); 
        
          rm.AttachShard(slNew); 
        
          var gs = rm.DetectMappingDifferences(slNew); 
    
          foreach (RecoveryToken g in gs) 
            { 
               rm.ResolveMappingDifferences(g, MappingDifferenceResolution.KeepShardMapping); 
            } 
        } 
    } 



[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Image references-->
[1]: ./media/sql-database-elastic-database-recovery-manager/recovery-manager.png
 
