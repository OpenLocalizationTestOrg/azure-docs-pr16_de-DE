<properties 
    pageTitle="Hinzufügen eines Shard mit flexible Datenbanktools | Microsoft Azure" 
    description="Festlegen, wie flexible skalieren-APIs verwenden, um neue mehrere Shards hinweg zu einer Shard hinzufügen." 
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

# <a name="adding-a-shard-using-elastic-database-tools"></a>Hinzufügen eines Shard mit flexible Datenbanktools

## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Hinzufügen eines Shard einen neuen Bereich oder Schlüssel  

Applikationen müssen häufig einfach Hinzufügen neuer mehrere Shards hinweg, um Daten zu verarbeiten, die möglicherweise von neuen Schlüsseln oder Key Bereiche, für ein Shard Schema, die bereits vorhanden ist. Angenommen, eine Anwendung sharded nach Mandanten ID möglicherweise müssen Sie eine neue Shard für einen neuen Mandanten bereitstellen oder Daten sharded monatlich möglicherweise benötigen Sie eine neue Shard nach der Bereitstellung vor Beginn eines jeden neuen Monat. 

Wenn Sie der neue Bereich der Schlüsselwerte nicht Teil einer vorhandenen Zuordnung ist, ist es sehr einfach, die neue Shard hinzufügen, und ordnen Sie die neuen Product Key oder einen Bereich aus, um die Shard. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Beispiel: Hinzufügen eines Shard und des Bereichs zu einer vorhandenen Shard-Zuordnung
In diesem Beispiel die [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) der [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0})) Methoden, verwendet und erstellt eine Instanz der Klasse [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) . Im folgenden Beispiel eine Datenbank mit dem Namen **sample_shard_2** und alle notwendigen Schemaobjekte innerhalb er erstellt wurden zum halten Bereich [300, 400).  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Alternativ können Sie Powershell zum Erstellen einer neuen Shard Karte-Manager. Beispiel für steht [hier](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).
## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Hinzufügen eines Shard für einen leeren Bereich des eines vorhandenen Bereichs  

In einigen Fällen zugeordnet noch einen Bereich eines Shard und es teilweise mit Daten ausgefüllt, aber jetzt anstehende Daten an einer anderen Shard weitergeleitet werden sollen. Angenommen, Sie Shard durch die Anzahl der Tage und bereits zugewiesenen 50 Tagen einer Shard müssen, aber am Tag 24, zukünftige Daten in einer anderen Shard landen werden soll. Das flexible- [Tool Teilen und Zusammenführen](sql-database-elastic-scale-overview-split-and-merge.md) können diesen Vorgang ausführen, aber ist das Verschieben von Daten nicht erforderlich (beispielsweise Daten für den Bereich von Tagen [25, 50), d. h., Tag 25 50 ausschließlich, einschließlich ist noch nicht vorhanden) Sie können dies vollständig mit dem Ausführen die Shard Karte Management-APIs direkt.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Beispiel: Teilen eines Bereichs, und den leeren Teil einer neu hinzugefügten Shard zuweisen

Eine Datenbank namens "sample_shard_2" sowie alle erforderlichen Schemaobjekte innerhalb er erstellt wurden.  

 
    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
    
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**Wichtig**: Verwenden Sie dieses Verfahren nur, wenn Sie sicher, dass der Bereich für sind die aktualisierte Zuordnung leer ist.  Die oben genannten Methoden prüfen nicht Daten für den Bereich verschoben wird, damit die Prüfungen in Ihren Code aufnehmen möchten, empfiehlt es sich.  Im Bereich verschobene Zeilen vorhanden sind, wird die Verteilung der tatsächlichen Daten nicht die aktualisierten Shard Karte überein. Verwenden Sie das [Tool Teilen und Zusammenführen](sql-database-elastic-scale-overview-split-and-merge.md) zum Ausführen des Vorgangs stattdessen in diesen Fällen ein.  


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
