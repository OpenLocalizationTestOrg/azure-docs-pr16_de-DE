<properties 
    pageTitle="Abfragen mit mehreren Shard | Microsoft Azure" 
    description="Ausführen von Abfragen über mehrere Shards hinweg mithilfe der flexible Datenbank-Client-Bibliothek." 
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
    ms.date="04/12/2016" 
    ms.author="torsteng"/>

# <a name="multi-shard-querying"></a>Mehrere Shard Abfragen

## <a name="overview"></a>(Übersicht)

Mit den [flexible Datenbanktools](sql-database-elastic-scale-introduction.md)können Sie die Datenbank sharded Lösungen erstellen. **Abfragen mit mehreren Shard** wird für Aufgaben wie das Websitesammlung/Melden von Daten verwendet werden, die eine Abfrage, die gestreckt wird über mehrere mehrere Shards hinweg ausgeführt werden müssen. (Vergleichen Sie dies mit [Daten-abhängige routing](sql-database-elastic-scale-data-dependent-routing.md), der alle Arbeit für einen einzelnen Shard durchgeführt.) 

## <a name="overview"></a>(Übersicht)

1. Abrufen einer [**RangeShardMap**](https://msdn.microsoft.com/library/azure/dn807318.aspx) oder [**ListShardMap**](https://msdn.microsoft.com/library/azure/dn807370.aspx) mithilfe der [**TryGetRangeShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), die [**TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)oder die [**GetShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) -Methode. Finden Sie unter [**Erstellen einer ShardMapManager**](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) und dem [**erhalten von RangeShardMap ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Erstellen Sie ein **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)** Objekt ein.
2. Erstellen einer **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**an. 
3. Legen Sie die **[CommandText-Eigenschaft](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)** auf einen T-SQL-Befehl ein.
3. Führen Sie den Befehl, indem Sie die **[ExecuteReader-Methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**aufrufen.
4. Anzeigen der Ergebnisse der **[Klasse MultiShardDataReader](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**verwenden. 

## <a name="example"></a>Beispiel

Der folgende Code veranschaulicht die Verwendung von Shard mit mehreren Abfragen mithilfe einer angegebenen **ShardMap** mit dem Namen *MyShardMap*. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                } 
           } 
    } 

 
Ein Key Unterschied ist der Erstellung von Verbindungen mit mehreren Shard. Wobei **SqlConnection** für eine einzelne Datenbank ausgeführt wird, wird der **MultiShardConnection** eine ***Sammlung von mehrere Shards hinweg*** als Eingabe. Füllen Sie diese Sammlung von mehrere Shards hinweg aus einer Shard Zuordnung an. Die Abfrage wird dann auf die Auflistung von **UNION ALL** Semantik so ein einzelnes Ergebnis für insgesamt zusammen mit mehrere Shards hinweg ausgeführt. Optional kann die Ausgabe mithilfe der **ExecutionOptions** -Eigenschaft auf Befehl der Namen der Stelle, an der die Zeile stammt Shard hinzugefügt werden. 

Beachten Sie den Anruf an **myShardMap.GetShards()**. Diese Methode ruft alle mehrere Shards hinweg aus der Zuordnung Shard und bietet eine einfache Möglichkeit zum Ausführen einer Abfrage in allen relevanten Datenbanken. Die zurückgegebene Auflistung von mehrere Shards hinweg für eine Abfrage mit mehreren Shard raffiniertes werden kann weiter nach Durchführung einer LINQ-Abfrage über die Sammlung von den Anruf an **myShardMap.GetShards()**. In Kombination mit der Richtlinie Teilergebnisse wurde die aktuelle Videofunktionen in Abfragen mit mehreren Shard entwickelt entwickelt auch für Zehntel bis zu mehreren hundert mehrere Shards hinweg.

Eine Einschränkung mit Abfragen mit mehreren Shard gibt es zurzeit die fehlende Überprüfung für mehrere Shards hinweg und Shardlets, die abgefragt werden. Während der Daten-abhängige routing überprüft, ob eine Shard angegebenen Teil der Karte Shard zum Zeitpunkt der Abfragen ist, führen Shard mit mehreren Abfragen mit dieser Prüfung nicht. Dies kann führen zu Shard mit mehreren Abfragen auf Datenbanken, die von der Karte Shard entfernt wurden.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Mehrere Shard Abfragen und Teilen und Zusammenführen Vorgänge

Abfragen mit mehreren Shard überprüfen nicht, ob der abgefragte Datenbank Shardlets im laufenden Betrieb von Teilen und Zusammenführen teilnehmen. (Siehe [Skalierung mithilfe des Tools für die Datenbank flexible Teilen und Zusammenführen](sql-database-elastic-scale-overview-split-and-merge.md).) Dies kann zu Inkonsistenzen führen, in dem Zeilen aus der gleichen Shardlet für mehrere Datenbanken in einer Abfrage mit mehreren Shard anzeigen. Achten Sie diese Einschränkungen und berücksichtigen Sie Ausgleichsmodus laufenden Betrieb von Teilen und Zusammenführen und Änderungen an der Karte Shard beim Ausführen von Abfragen mit mehreren Shard.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Siehe auch
**[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)** Klassen und Methoden.


Verwalten Sie mehrere Shards hinweg mithilfe der [flexible Datenbank-Client-Bibliothek](sql-database-elastic-database-client-library.md). Enthält einen Namespace mit dem Namen [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) , die die Möglichkeit zum Abfragen von mehreren mehrere Shards hinweg mit einer einzigen Abfrage und Ergebnis bereitstellt. Es stellt eine Abstraktion abfragende über eine Auflistung von mehrere Shards hinweg. Darüber hinaus alternativen Ausführungsrichtlinien, insbesondere Teilergebnisse Fehler behandelt, wenn Sie über mehrere Shards viele hinweg Abfragen.  

 