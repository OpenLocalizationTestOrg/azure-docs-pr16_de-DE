<properties
   pageTitle="Konfigurieren der Stabilität und Wiederherstellung auf Elasticsearch auf Azure"
   description="Aspekte, die im Zusammenhang mit der Stabilität und Wiederherstellung für Elasticsearch."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>
   
# <a name="configuring-resilience-and-recovery-on-elasticsearch-on-azure"></a>Konfigurieren der Stabilität und Wiederherstellung auf Elasticsearch auf Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel ist [Teil einer Serie](guidance-elasticsearch.md). 

Ein der wichtigsten Features von Elasticsearch wird unterstützt, die sie für Flexibilität bei ausgefallener Knoten und/oder Netzwerk Partition Ereignisse enthält. Die Replikation ist die offensichtlichsten Methode, in der Sie verbessern können, die Flexibilität von einem beliebigen Cluster, aktivieren Elasticsearch, um sicherzustellen, dass mehrere Kopien eines beliebigen Datenelements auf verschiedenen Knoten verfügbar ist, für den Fall, dass ein Knoten zugegriffen werden soll. Wenn ein Knoten vorübergehend nicht verfügbar ist, können andere Knoten mit Replikaten eines Daten aus dem fehlenden Knoten die fehlenden Daten dienen bis zur Behebung des Problems. Für den Fall eines Problems mehr Ausdruck fehlende Knoten durch ein neues ersetzt werden, und Elasticsearch kann die Daten auf dem neuen Knoten von den Replikaten wiederherstellen.

Hier werden wir die Stärke und Wiederherstellung Optionen mit Elasticsearch, wenn in Azure gehostet wird, und Beschreiben Sie einige wichtige Aspekte des ein Elasticsearch Cluster, den Sie berücksichtigen sollten, um die Wahrscheinlichkeit von Datenverlust und erweiterten Daten Wiederherstellungszeiten zu minimieren.

In diesem Artikel wird auch Stichprobe Tests, mit die durchgeführt wurden, um anzuzeigen, wie das System reagiert, wie es stellt wieder her, und die Effekte der verschiedenen Arten von Fehlern in einem Cluster Elasticsearch.

Ein Elasticsearch Cluster verwendet Replikate Verfügbarkeit und Verbessern der Leistung finden Sie hier. Replikate sollten auf verschiedenen virtuellen Computern aus der primären mehrere Shards hinweg gespeichert werden, die sie sich reproduzieren. Die Absicht ist, wenn der virtuellen Computer einen Datenknoten Hostinganbieter fehlschlägt oder nicht verfügbar ist mehr, das System mithilfe der virtuellen Computern fortsetzen kann, halten die Replikate funktionsfähiges.

## <a name="using-dedicated-master-nodes"></a>Verwenden von dedizierten master-Knoten

Ein Knoten in einem Cluster Elasticsearch gewählt wird als master-Knoten. Der Zweck dieses Knotens ist Cluster Management Vorgänge wie ausführen:

- Erkennen von ausgefallenen Knoten und Umstellung auf Replikate.

- Verschieben von mehrere Shards hinweg, Knoten Arbeitsbelastung abzuwägen.

- Mehrere Shards hinweg wiederherstellen, wenn ein Knoten wieder online geschaltet wird.

Sie sollten bietet dedizierte master-Knoten in kritischen Cluster und sicherstellen, dass es 3 dedizierte Knoten gibt, dessen Rolle nur besteht darin, master werden. Diese Konfiguration reduziert den Umfang der Ressource stark Arbeit, die diese Knoten ausführen müssen (ihnen nicht gespeichert Daten oder verarbeitet Abfragen) und Cluster Stabilität Leben können. Nur einen dieser Knoten gewählt werden wird, aber die anderen werden eine Kopie des Systemstatus enthalten und können übernehmen, sollte das markierte Master-Shape fehl.

## <a name="controlling-high-availability-with-azure--update-domains-and-fault-domains"></a>Steuern der hohen Verfügbarkeit mit Azure – Update und Fehlerstrukturanalyse-Domänen 

Andere virtuelle Computer können die gleiche physische Hardware freigeben. In einer Azure Datacenter den eines einzelnen Shapes für Gestelle kann eine Anzahl von virtuellen Computern hosten, und alle diese virtuellen Computern freigeben einer allgemeinen Power Quell- und Netzwerk wechseln. Ausfall ein einzelnes den Shapes für Gestelle Ebene kann eine Anzahl von virtuellen Computern daher auswirken. Azure verwendet des Konzepts der Fehlerstrukturanalyse Domänen zum Testen und dieses Risiko verteilt. Eine Domäne Fehlerstrukturanalyse entspricht ungefähr eine Gruppe von virtuellen Computern, die die gleichen den Shapes für Gestelle freigeben. Um sicherzustellen, dass ein Fehler den Shapes für Gestelle Ebene kein und den Knoten, halten gleichzeitig alle zugehörigen Replikate abstürzen, sollten Sie sicherstellen, dass die virtuellen Computern Fehlerstrukturanalyse Domänen verteilt werden.

Auf ähnliche Weise können virtuellen Computern vom [Azure Fabric Controller](https://azure.microsoft.com/documentation/videos/fabric-controller-internals-building-and-updating-high-availability-apps/) absolviert werden nach unten und geplanten Wartung und Betriebssystem-Upgrades durchführen. Azure weist virtuellen Computern um Domänen zu aktualisieren. Wenn einer geplanten Wartung Ereignis eintritt, werden nur virtuellen Computern in einer einzelnen Update-Domäne jeweils beeinflusst. Virtuellen Computern in anderen Domänen aktualisieren verbleiben, bis die virtuellen Computern in der Domäne aktualisieren der Aktualisierung wieder online geschaltet sind. Daher müssen Sie sicherstellen, dass virtuellen Computern Hostinganbieter Knoten und sich zu anderen Update Domänen möglichst gehören.

> [AZURE.NOTE] Weitere Informationen zu Fehlerstrukturanalyse Domänen und Domänen aktualisieren finden Sie unter [Verwalten der Verfügbarkeit von virtuellen Computern][].

Sie können keine explizit ein virtuellen Computers zu einem bestimmten Update Domain und Fehlerstrukturanalyse-Domäne zuweisen. Beim Erstellen von virtuellen Computern, ist diese Zuordnung von Azure gesteuert. Allerdings können Sie angeben, dass virtuellen Computern als Teil eines Satzes Verfügbarkeit erstellt werden soll. Virtuellen Computern in demselben Satz Verfügbarkeit werden aktualisieren und Fehlerstrukturanalyse-Domänen verteilt. Virtuellen Computern manuell erstellt, erstellt Azure jedes Verfügbarkeit mit zwei Fehlerstrukturanalyse und fünf Update-Domänen einrichten. Virtuellen Computern an diese Domänen Fehlerstrukturanalyse zugeordnet sind, und aktualisieren Sie Domänen runden und ausschalten, wie weiteren virtuellen Computern, wie folgt bereitgestellt werden: 

| VIRTUELLER COMPUTER | Fehlerstrukturanalyse-Domäne | Domäne aktualisieren |
|----|--------------|---------------|
|  1 |            0 |             0 |
|  2 |            1 |             1 |
|  3 |            0 |             2 |
|  4 |            1 |             3 |
|  5 |            0 |             4 |
|  6 |            1 |             0 |
|  7 |            0 |             1 |

> [AZURE.IMPORTANT] Wenn Sie virtuelle Computer mit dem Azure Ressource-Manager erstellen, kann jeden Satz Verfügbarkeit bis zu 3 Fehlerstrukturanalyse und 20 Update-Domänen zugeordnet werden. Dies ist eine überzeugende Grund für die Verwendung der Ressourcenmanager.

Im Allgemeinen alle virtuellen Computern, die denselben Zweck, in demselben Satz Verfügbarkeit erfüllen, zu platzieren, aber Erstellen von anderen Verfügbarkeit Datensätze für virtuellen Computern, die verschiedene Funktionen ausführen. Setzt erstellen mindestens separaten Verfügbarkeit für Elasticsearch, dies bedeutet, dass Sie berücksichtigen sollten:

- Virtuellen Computern Datenknoten hosten.
- Virtuelle Computer hosten von Client-Knoten (Wenn Sie diese verwenden).
- Virtuellen Computern Hostinganbieter master-Knoten.

Darüber hinaus sollten Sie sicherstellen, dass jeder Knoten in einem Cluster der Domäne aktualisieren und die zugehörige Fehlerstrukturanalyse-Domäne bekannt ist. Diese Informationen können dabei helfen, um sicherzustellen, dass Elasticsearch nicht mehrere Shards hinweg und sich in der gleichen Fehlerstrukturanalyse erstellen und Aktualisieren von Domänen, die Möglichkeit, eine Shard und der zugehörigen aus außer Betrieb genommen wird, zur gleichen Zeit zu minimieren. Sie können einen Elasticsearch-Knoten, um die Hardware-Verteilung der Cluster durch Konfigurieren [Shard Zuteilung Präsenz](https://www.elastic.co/guide/en/elasticsearch/reference/current/allocation-awareness.html#allocation-awareness)annähernd konfigurieren. Beispielsweise könnten Sie ein Paar von benutzerdefinierten Knotenattribute namens *FaultDomain* und *UpdateDomain* in der Datei elasticsearch.yml wie folgt definieren:

```yaml
node.faultDomain: \${FAULTDOMAIN}
node.updateDomain: \${UPDATEDOMAIN}
```

In diesem Fall sind die Attribute mit den Werten frei, die im festgelegt der * \${FAULTDOMAIN}* und * \${UPDATEDOMAIN}* Umgebungsvariablen Wenn Elasticsearch gestartet wird. Sie müssen die folgenden Einträge zu der Datei Elasticsearch.yml, um anzugeben, dass *FaultDomain* und *UpdateDomain* Zuteilung Präsenz Attributen sind aus, und geben Sie die Datensätze der zulässigen Werte für diese Attribute hinzufügen:

```yaml
cluster.routing.allocation.awareness.force.updateDomain.values: 0,1,2,3,4
cluster.routing.allocation.awareness.force.faultDomain.values: 0,1
cluster.routing.allocation.awareness.attributes: updateDomain, faultDomain
```

Shard Zuteilung Präsenz können in Verbindung mit [Shard Zuteilung Filtern](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/shard-allocation-filtering.html#shard-allocation-filtering) explizit angeben, welche Knoten mehrere Shards hinweg für einen bestimmten Index gehostet werden können.

Wenn Sie über die Anzahl der Fehlerstrukturanalyse und Aktualisieren von Domänen in einem Satz Verfügbarkeit skalieren müssen, können Sie zusätzliche Verfügbarkeit gebildeten virtuellen Computern erstellen. Jedoch müssen Sie wissen, dass Knoten in anderen Verfügbarkeit von Sätzen für die Wartung gleichzeitig abgeschaltet werden können. Versuchen Sie, stellen Sie sicher, dass jede Shard und mindestens eines seiner Replikate in demselben Satz Verfügbarkeit enthalten sind.

> [AZURE.NOTE] Es gibt es zurzeit maximal 100 virtuellen Computern pro Verfügbarkeit festlegen. Weitere Informationen finden Sie unter [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen](../azure-subscription-service-limits.md).

### <a name="backup-and-restore"></a>Sichern und Wiederherstellen

Verwendung von Replikaten bietet keine vollständigen Schutz von schwerwiegenden Fehler (z. B. versehentlich löschen den gesamten Cluster). Sie sollten sicherstellen, dass Sie die Daten in einem Cluster regelmäßig gesichert und stehen Ihnen eine bewährten Strategie zum Wiederherstellen des Systems aus dieser Sicherungskopien.

Verwenden Sie die Elasticsearch [Momentaufnahme und Wiederherstellen von APIs](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html) : elastisch nicht diese zu begrenzen. >> zum Sichern und Wiederherstellen von Indizes. Momentaufnahmen können in einem freigegebenen Dateisystem gespeichert werden. Alternativ-Plug-Ins sind verfügbar, die können Momentaufnahmen geschrieben werden im Dateisystem Hadoop distributed (HDFS) ( [HDFS-Plug-Ins](https://github.com/elasticsearch/elasticsearch-hadoop/tree/master/repository-hdfs)) oder Azure-Speicher (das [Azure-Plug-Ins](https://github.com/elasticsearch/elasticsearch-cloud-azure#azure-repository)).

Berücksichtigen Sie beim Auswählen des Momentaufnahme Speichermechanismus die folgenden Punkte:

- [Azure Dateispeicher](https://azure.microsoft.com/services/storage/files/) können Sie ein freigegebenes Dateisystem implementieren, die von allen Knoten zugegriffen werden kann.

- Verwenden Sie das Plug-in HDFS nur, wenn Sie zusammen mit Hadoop Elasticsearch ausgeführt werden.

- Das Plug-in HDFS erfordert, dass der Java-Manager, die innerhalb der Elasticsearch Instanz des Java virtuellen Computers (JVM) deaktivieren Sie.

- Die HDFS-Plug-in unterstützt alle HDFS kompatiblen Dateisystem, vorausgesetzt, dass die richtige Hadoop-Konfiguration mit Elasticsearch verwendet wird.

  
## <a name="handling-intermittent-connectivity-between-nodes"></a>Behandeln von bei unterbrochener Verbindung zwischen Knoten

Netzwerkprobleme Probleme aufgeführt, virtueller Computer neu gestartet, nachdem eine routinemäßige Wartung im Datencenter und ähnlichen Ereignissen können dazu führen, dass Knoten vorübergehend nicht zugegriffen werden. In diesen Situationen, in dem das Ereignis wahrscheinlich kurzlebige ist, tritt auf der Aufwand für die mehrere Shards hinweg Qualifikationsprofilen zweimal in Quick Folge (einmal, wenn der Fehler erkannt wird und erneut wann der Knoten für das Master-Shape sichtbar sein) ein erheblichen Aufwand, die Leistung wirkt sich auf machen kann. Sie können verhindern, dass temporäre Knoten nicht zugegriffen werden kann bewirken, dass das Master-Shape zu neu zu verteilen Cluster durch Festlegen der *verzögert\_Timeout* -Eigenschaft eines Indexes oder für alle Indizes. Im folgenden Beispiel wird die Verzögerung auf 5 Minuten:

```http
PUT /_all/settings
{
    "settings": {
    "index.unassigned.node_left.delayed_timeout": "5m"
    }
}
```

Weitere Informationen finden Sie unter [verzögern Zuteilung, wenn ein Knoten verlässt](https://www.elastic.co/guide/en/elasticsearch/reference/current/delayed-allocation.html).

In einem Netzwerk, die zu Interruptions ist, können Sie auch die Parameter ändern, die konfigurieren ein Master-Shapes zu erkennen, wenn Sie einen anderen Knoten nicht mehr zugegriffen werden. Diese Parameter sind Bestandteil des Moduls [macht Suche](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-discovery-zen.html#modules-discovery-zen) mit Elasticsearch bereitgestellt, und Sie können sie in der Datei Elasticsearch.yml festlegen. Beispielsweise gibt der Parameter *discovery.zen.fd.ping.retries* an, wie oft ein master-Knoten versucht, einen anderen Knoten im Cluster, bevor Sie sich entscheiden, den sie fehlgeschlagen ist, Signal an. Für diesen Parameter zu 3 Standardwerte, doch können Sie es wie folgt ändern:

```yaml
discovery.zen.fd.ping_retries: 6
```

## <a name="controlling-recovery"></a>Steuern der Wiederherstellung

Wenn die Verbindung zu einem Knoten nach einem Fehler wiederhergestellt wird, werden alle mehrere Shards hinweg auf diesem Knoten müssen wiederhergestellt werden, um diese zu auf dem neuesten Stand. Standardmäßig stellt Elasticsearch mehrere Shards hinweg in der folgenden Reihenfolge wieder her:

- Nach dem Erstellungsdatum der reverse Index. Bevor Sie ältere Indizes werden neuere Indizes wiederhergestellt.

- Nach dem reverse Index an. Indizes, deren Namen, die alphanumerisch größer als andere sind, werden zuerst wiederhergestellt werden.

Wenn einige Indizes wichtiger als andere sind, aber diese Kriterien, können Sie die Rangfolge von Indizes überschreiben, durch Festlegen der Eigenschaft *index.priority* stimmen nicht überein. Indizes mit einem höheren Wert für diese Eigenschaft werden vor dem Indizes wiederhergestellt werden, die einen unteren Wert aufweisen:

```http
PUT low_priority_index
{
    "settings": {
        "index.priority": 1
    }
}

PUT high_priority_index
{
    "settings": {
        "index.priority": 10
    }
}
```

Weitere Informationen finden Sie unter [Index Wiederherstellung Prioritäten](https://www.elastic.co/guide/en/elasticsearch/reference/2.0/recovery-prioritization.html#recovery-prioritization).

Sie können den Wiederherstellungsprozess für einen oder mehrere Indizes mit Überwachen der * \_Wiederherstellung* API:

```http
GET /high_priority_index/_recovery?pretty=true
```

Weitere Informationen finden Sie unter [Indizes Wiederherstellung](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-recovery.html#indices-recovery).

> [AZURE.NOTE] Ein Cluster mit mehrere Shards hinweg, die Wiederherstellung erfordern haben einen Status des *gelben* um anzugeben, dass nicht alle mehrere Shards hinweg derzeit verfügbar sind. Wenn die mehrere Shards hinweg verfügbar sind, sollte der Status Cluster *grünen*wiederherstellen. Ein Cluster mit dem Status *Rot* zeigt an, dass eine oder mehrere mehrere Shards hinweg physisch, möglicherweise ist es erforderlich, um Daten aus einer Sicherung wiederherzustellen.

## <a name="preventing-split-brain"></a>Verhindern von Teilen Gehirn 

Einer geteilten Gehirn kann auftreten, wenn die Verbindungen zwischen Knoten fehl. Wenn ein master-Knoten eines Bereichs Cluster nicht erreichbar wird, eine Wahl statt im Netzwerksegment, das Erreichbarkeit bleibt, bis ein anderer Knoten werden erst das Master-Shape. In einem Cluster nicht ordnungsgemäß konfiguriert ist es möglich, für jeden Teil des Cluster verschiedenen Master-Shapes in Dateninkonsistenzen oder Beschädigung resultierender haben. Dieses Phänomen wird als eine *geteilte Gehirn*bezeichnet.

Sie können die Wahrscheinlichkeit von einem geteilten Gehirn reduzieren, indem Sie konfigurieren die *minimalen\_master\_Knoten* Eigenschaft des Moduls Suche, in der Datei elasticsearch.yml. Diese Eigenschaft gibt an, wie viele Knoten so aktivieren Sie die Wahl eines Master-Shapes verfügbar sein müssen. Im folgende Beispiel wird den Wert dieser Eigenschaft auf 2:

```yaml
discovery.zen.minimum_master_nodes: 2
```

Dieser Wert sollte den niedrigsten Großteil die Anzahl der Knoten festgelegt werden, die die Funktion erfüllen können. Beispielsweise, wenn Sie Ihren Cluster 3 master-Knoten, hat *minimalen\_master\_Knoten* auf 2 festgelegt werden. Wenn Sie 5 master-Knoten haben *minimalen\_master\_Knoten* sollte auf 3 festgelegt werden. Idealerweise sollten Sie eine ungerade Zahl von einem master-Knoten verfügen.

> [AZURE.NOTE] Es ist möglich, dass ein geteiltes Gehirn zu auftreten, wenn mehrere master-Knoten in demselben Cluster gleichzeitig gestartet werden. Während dieses Termins selten sind, können Sie es verhindern durch Starten von Knoten seriell mit einer kurzen Verzögerung (5 Sekunden) zwischen den einzelnen. 

## <a name="handling-rolling-updates"></a>Behandeln von parallelen updates

Wenn Sie ein Upgrade der Software auf Knoten selbst durchführen (beispielsweise auf eine neuere Version migrieren oder Durchführung eines Patches), müssen Sie auszuführende Arbeit zu den einzelnen Knoten, die erforderlich sind, erstellen sie offline Beibehaltung den Rest der Cluster zur Verfügung. Erwägen Sie in diesem Fall das folgen dieser Vorgehensweise implementieren. 

1. Stellen Sie sicher, dass die Shard erneuten Reservierung ausreichend verzögert ist, um zu verhindern, dass das markierte Master-Shape Qualifikationsprofilen mehrere Shards hinweg von einem fehlende Knoten über den Rest der Cluster. Standardmäßig Shard erneuten Reservierung für eine Minute verzögert ist, aber Sie können die Dauer erhöhen, ist ein Knoten wahrscheinlich für einen längeren Zeitraum nicht mehr verfügbar sein. Im folgende Beispiel wird die Verzögerung bis 5 Minuten erhöht:

    ```http
    PUT /_all/_settings
    {
        "settings": {
            "index.unassigned.node_left.delayed_timeout": "5m"
        }
    }
    ```

    > [AZURE.IMPORTANT] Sie können die Shard erneuten Reservierung auch vollständig deaktivieren, indem Sie der *cluster.routing.allocation.enable* im Cluster *keine*. Jedoch sollten Sie mithilfe dieser Ansatz, wenn neue Indizes sind wahrscheinlich erstellt werden, während der Knoten offline ist, wie dies Index Zuteilung zum Fehlschlagen in einem Cluster mit roten Status resultierender führen kann.

2. Beenden Sie Elasticsearch auf dem Knoten verwaltet werden. Wenn Elasticsearch als Dienst ausgeführt wird, können Sie den Prozess mithilfe eines Befehls Betriebssystem kontrolliert zu unterbrechen sein. Im folgende Beispiel wird gezeigt, wie angehalten Elasticsearch Dienst auf einem einzelnen Knoten Ubuntu ausgeführt wird:

    ```bash
    service elasticsearch stop
    ```

    Alternativ können Sie die war(en)-API direkt auf den Knoten verwenden:

    ```http
    POST /_cluster/nodes/_local/_shutdown
    ```

3. Führen Sie die notwendigen Wartung auf den Knoten

4. Starten Sie den Knoten neu, und warten Sie darauf, um dem Cluster beitreten.

5. Shard Zuordnung wieder zu aktivieren:

    ```http
    PUT /_cluster/settings
    {
        "transient": {
            "cluster.routing.allocation.enable": "all"
        }
    }
    ```

> [AZURE.NOTE] Wenn Sie mehr als einen Knoten beibehalten müssen, wiederholen Sie die Schritte 2&ndash;4 auf den einzelnen Knoten, bevor Sie Shard Zuteilung wieder aktivieren.

Falls dies möglich ist, beenden Sie neue Daten während dieses Vorgangs Indizierung. Dadurch wird das um Wiederherstellungszeit zu minimieren, wenn Knoten wieder online geschaltet sind, und treten Sie dem Cluster.

Automatische Updates Vorsicht vor Elemente wie die JVM (idealerweise deaktivieren automatische Updates für diese Elemente), besonders wenn Elasticsearch unter Windows ausgeführt. Java-Update-Agent kann die neueste Version von Java automatisch herunterladen, aber möglicherweise erfordern Elasticsearch neu gestartet werden, damit die Aktualisierung wirksam wird. Dadurch können unkoordiniert temporäre Verlust der Knoten, je nachdem, wie der Java-Update-Agent konfiguriert ist. Dies kann auch in anderen Instanzen von Elasticsearch im selben Cluster mit unterschiedlichen Versionen der Kompatibilitätsprobleme könnte JVM führen.

## <a name="testing-and-analyzing-elasticsearch-resilience-and-recovery"></a>Testen und Analysieren von Elasticsearch Stabilität und Wiederherstellung

In diesem Abschnitt werden eine Reihe von Tests, die ausgeführt wurden, um die Stabilität auszuwerten und Wiederherstellung einer Elasticsearch Cluster mit drei Daten und drei master-Knoten.

Die folgenden Szenarien wurden getestet: 

- Ausfall eines und Neustart mit keine Daten verloren. Ein Datenknoten beendet und nach 5 Minuten neu gestartet. Elasticsearch wurde nicht neu zuordnen fehlende mehrere Shards hinweg in diesem Intervall, sodass keine zusätzliche e/a in mehrere Shards hinweg bewegen anfallen konfiguriert. Wenn Sie der Knoten neu gestartet wurde, bringt des Wiederherstellungsvorgangs der mehrere Shards hinweg auf diesem Knoten wieder auf dem neuesten Stand.

- Ausfall eines Knotens mit schwerwiegenden Datenverlust. Ein Datenknoten beendet ist und die Daten, die ihn enthält ist zum Simulieren von schwerwiegenden Datenträgerfehler gelöscht wird. Der Knoten dann neu gestartet wird (nach 5 Minuten), effektiv fungiert als Ersatz für den ursprünglichen Knoten. Des Wiederherstellungsvorgangs muss neu erstellt die fehlenden Daten für diesen Knoten und kann mehrere Shards hinweg frei, die auf anderen Knoten verschieben umfassen.

- Ausfall eines und Neustart ohne Datenverlust, jedoch mit Shard erneuten Reservierung. Ein Datenknoten angehalten, und die mehrere Shards hinweg, die ihn enthält werden an andere Knoten zugewiesen. Der Knoten neu gestartet wird, und weitere erneuten Reservierung tritt auf, um den Cluster neu zu verteilen.

- Paralleles Updates an. Jeder Knoten im Cluster beendet und neu gestartet nach kurzer um Maschinen, die gerade neu gestartet nach einer softwareaktualisierung zu reproduzieren. Nur ein Knoten wird auf einem beliebigen Zeitpunkt gestoppt. Mehrere Shards hinweg werden nicht erneut zugeordnet, bevor ein Knoten verfügbar ist.

Jedes Szenario wurde unterliegen den gleichen Arbeitsbelastung einschließlich eine Mischung Aufnahme Datentasks, Aggregationen und Filterabfragen, während Knoten offline und wiederhergestellte geöffnet wurden. Das gleichzeitige Einfügen Vorgänge in die Arbeitsbelastung jede 1000 Dokumente gespeichert und gegen einen Index, während die Aggregationen durchgeführt wurden und Filterabfragen verwendet einen Index mit mehreren Millionen Dokumente. Dies wurde die Leistung von Abfragen, die aus der Massen fügt separat bewertet werden aktivieren. Jeder Index enthalten fünf mehrere Shards hinweg und eine Kopie an.

Den folgenden Abschnitten werden die Ergebnisse dieser Tests, beachtet werden alle Leistungsabfall während ein Knoten offline ist oder wiederherzustellende und Fehlern, die gemeldet wurden. Die Ergebnisse werden grafisch dargestellt Hervorhebung die Punkte, an denen ein oder mehrere Knoten fehlen und -Schätzung die Zeit für das System vollständig wiederherstellen und erzielen eine ähnliche Ebene der Leistung, die vor der Offlineschalten Knoten vorhanden ist.

> [AZURE.NOTE] Die Durchführung dieser Tests verwendet Test Kabelbäume sind online verfügbar. Sie können anpassen, und verwenden diese Kabelbäume zur Überprüfung der Stabilität und der Wiederherstellung Ihrer eigenen Cluster-Konfigurationen. Weitere Informationen finden Sie unter [die automatisierten Elasticsearch Stabilität Tests ausführen][].

## <a name="node-failure-and-restart-with-no-data-loss-results"></a>Ausfall eines und Neustart ohne Datenverlust: Ergebnisse

<!-- TODO; reformat this pdf for display inline --> 

Die Ergebnisse dieser Prüfung werden in der Datei [ElasticsearchRecoveryScenario1.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario1.pdf)angezeigt. Das Diagramm zeigt Performance-Profil der Arbeitsbelastung und physische Ressourcen für die einzelnen Knoten im Cluster. Ersten Teils der Diagramme anzeigen das System normalerweise für ungefähr 20 Minuten, an welcher Stelle Knoten 0 für 5 Minuten neu gestartet wird beendet wird ausgeführt. Die Statistik für einen weiteren 20 Minuten dargestellt werden; das System dauert etwa 10 Minuten wiederherstellen und stabilisieren. Dies ist die Transaktion Sätzen und Reaktionszeiten für die verschiedenen Auslastung dargestellt.

Beachten Sie die folgenden Punkte:

- Während der Prüfung wurden keine Fehler gemeldet. Keine Daten verloren gegangen, und alle Vorgänge erfolgreich abgeschlossen.

- Die Transaktion Sätzen für alle drei Arten von Vorgang (Massen einfügen, aggregieren Abfrage und Filtern Abfragen) gelöscht und die durchschnittliche Reaktionszeiten wurde erhöht, während der Knoten 0 offline war.

- Während der Wiederherstellung Periode, Transaktion Sätze und Reaktionszeiten für aggregieren Abfrage- und Filtern Abfragen wurden Vorgänge allmählich wiederhergestellt. Die Leistung für Massen einfügen, die für einen kurzen während vor dem Schwächen wiederhergestellt werden. Jedoch sind wahrscheinlich die Lautstärke des Indexes verwendet wird durch das Einfügen Massen wachsen bewirken, dass Daten, und die Transaktion Sätzen für diesen Vorgang verlangsamt, bevor Knoten 0 Offlineschalten angezeigt werden können.

- Die CPU-Auslastung Graph für Knoten 0 zeigt reduzierte Aktivität der Wiederherstellung Phase dieser aufgrund des erhöhten Datenträgers und Netzwerkaktivitäten, die nach der Wiederherstellungsverfahren, den Knoten verursacht hat zum aktuellen Stand mit Daten, die sie verpasst wurde, während sie offline ist, und aktualisieren die mehrere Shards hinweg, die ihn enthält.

- Die mehrere Shards hinweg für die Indizes werden nicht genau verteilt gleichmäßig über alle Knoten. Es gibt zwei Indizes mit 5 mehrere Shards hinweg und jedes, 1 Kopie ausführenden insgesamt 20 mehrere Shards hinweg. Zwei Knoten enthält daher 6 mehrere Shards hinweg, die anderen zwei 7 jedes gedrückt. Dies fällt in die CPU-Auslastung Diagramme während der anfänglichen 20-minütiges Periode, Knoten 0 kleiner als die anderen zwei beschäftigt ist. Nach Abschluss der Wiederherstellung anscheinend einige umsteigen auftreten, wie Knoten 2 angezeigt wird, die weitere leicht geladen Knoten vorgesehen ist.

    
## <a name="node-failure-with-catastrophic-data-loss-results"></a>Ausfall eines Knotens mit schwerwiegenden Datenverlust: Ergebnisse

<!-- TODO; reformat this pdf for display inline -->

Die Ergebnisse dieser Prüfung werden in der Datei [ElasticsearchRecoveryScenario2.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario2.pdf)dargestellt. Während der ersten Teils der Diagramme mit dem ersten Test, das System normalerweise für ungefähr 20 Minuten ausgeführt wird, wird an welchem Punkt Knoten 0 5 Minuten geschlossen. Während dieses Intervalls werden die Elasticsearch Daten auf diesem Knoten entfernt simulieren schwerwiegenden Datenverlust, bevor Sie neu gestartet wird. Vollständige Wiederherstellung wird 12-15 Minuten vor der Wiederherstellung werden der Ebenen der Leistung vor dem Test angezeigt wird.

Beachten Sie die folgenden Punkte:

- Während der Prüfung wurden keine Fehler gemeldet. Keine Daten verloren gegangen, und alle Vorgänge erfolgreich abgeschlossen.

- Die Transaktion Sätzen für alle drei Arten von Vorgang (Massen einfügen, aggregieren Abfrage und Filtern Abfragen) gelöscht und die durchschnittliche Reaktionszeiten wurde erhöht, während der Knoten 0 offline war. An diesem Punkt ähnelt das Performance-Profil des Tests im ersten Szenario. Dies ist keine überraschende als, bis zu diesem Punkt, die Szenarien sind gleich.

- Während des Zeitraums Wiederherstellung wurden Transaktion Sätze und Reaktionszeiten wiederhergestellt, obwohl dieses Zeitraums viel mehr Flüchtigkeit die Zahlen enthielt. Dies ist höchstwahrscheinlich aufgrund der zusätzlichen Aufwand, dass die Knoten im Cluster die Daten zum Wiederherstellen der fehlenden mehrere Shards hinweg zur Verfügung stellt durchführen. Diese zusätzlichen Aufwand ist in der CPU-Auslastung, Datenträgeraktivität und Netzwerk Aktivität Diagramme erkennbar.

- Die CPU-Auslastung Graph für Knoten 0 und 1 zeigt reduzierte Aktivität der Wiederherstellung Phase dies aufgrund der erhöhte Datenträger und Netzwerkaktivität des Wiederherstellungsvorgangs zurückzuführen ist. Im ersten Szenario nur der Knoten wiederherzustellende ausgestellt dieses Verhalten, aber in diesem Szenario ist es wahrscheinlich, dass die fehlenden Daten für Knoten 0 ein Großteil wird wiederhergestellt von Knoten 1.

- Die e/a-Aktivität für Knoten 0 wird tatsächlich im Vergleich zum ersten Szenario verringert. Ursache könnte die e/a-Effizienz einfach Kopieren der Daten für die Reihe von kleineren e/a-Anfragen erforderlich, um eine vorhandene Shard auf dem neuesten Stand zu bringen, statt eine gesamte Shard hierfür sein.

- Die Netzwerkaktivität für alle drei Knoten Aktivitätsschübe anzugeben, wie Daten gesendet und empfangen wird zwischen Knoten. Nur Knoten 0 wie viel Netzwerkaktivität, aber diese Aktivität ausgestellt anscheinend in Szenario 1 für einen längeren Zeitraum aufrechterhalten werden. In diesem Fall könnte dieser Unterschied aufgrund der Effizienz der gesamten Daten für ein Shard als die Reihe von kleinere Anfragen beim Wiederherstellen einer Shard erhalten, statt einer Anforderung übertragen.

## <a name="node-failure-and-restart-with-shard-reallocation-results"></a>Ausfall eines und Neustart mit Shard erneuten Reservierung: Ergebnisse

<!-- TODO; reformat this pdf for display inline -->

Die Datei [ElasticsearchRecoveryScenario3.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario3.pdf) veranschaulicht die Ergebnisse dieser Prüfung. Während der ersten Tests zurück, ersten Teils der Diagramme anzeigen auf das System normalerweise für ungefähr 20 Minuten ausgeführt, wird an welchem Punkt Knoten 0 5 Minuten geschlossen. Zu diesem Zeitpunkt versucht Elasticsearch Cluster der fehlende mehrere Shards hinweg neu zu erstellen und die mehrere Shards hinweg über den verbleibenden Knoten neu zu verteilen. Nach 5 Minuten Knoten 0 wieder online geschaltet, und erneut Cluster hat, die mehrere Shards hinweg neu zu verteilen. Leistung wird nach 12-15 Minuten wiederhergestellt.

Beachten Sie die folgenden Punkte:

- Während der Prüfung wurden keine Fehler gemeldet. Keine Daten verloren gegangen, und alle Vorgänge erfolgreich abgeschlossen.

- Die Transaktion Sätzen für alle drei Arten von Vorgang (Massen einfügen, aggregieren Abfrage und Filtern Abfragen) gelöscht und die durchschnittliche Reaktionszeiten deutlich erhöhten während der Knoten 0 offline war im Vergleich zum vorherigen beiden Prüfungen. Die Clusteraktivität höhere beim erneuten Erstellen der fehlende mehrere Shards hinweg und Qualifikationsprofilen Cluster aus, indem Sie die potenzieren Zahlen für Festplatte und das Netzwerk Aktivität für Knoten 1 und 2 dieses Zeitraums verfügbar ist.

- Während des Zeitraums nach Knoten 0 wieder online geschaltet ist bleiben Transaktion Sätze und Reaktionszeiten veränderliche.

- Die CPU-Auslastung und Datenträger Aktivität Diagramme für Knoten 0 zeigt sehr reduzierte initial Aktion der Wiederherstellung Phase. Dies liegt daran zu diesem Zeitpunkt Knoten 0 keine Daten fungiert. Nach einem Zeitraum von ungefähr 5 Minuten, birst der Knoten in der Aktion < RBC: Hierbei handelt es sich mich Sprachausgabe snort. Ich bin nicht mit eine bessere Möglichkeit, diese jedoch sagen in Kürze nach oben.  >> dargestellt durch die plötzlich erhöhen im Netzwerk, Datenträger und CPU-Aktivität. Dies wird in den meisten Fällen vom Cluster Verteilen von mehrere Shards hinweg über Knoten verursacht. Knoten 0 zeigt dann normale Aktivitäten.
  
## <a name="rolling-updates-results"></a>Paralleles Updates: Ergebnisse

<!-- TODO; reformat this pdf for display inline -->

Die Ergebnisse dieser Prüfung, in der Datei [ElasticsearchRecoveryScenario4.pdf](https://github.com/mspnp/azure-guidance/blob/master/figures/Elasticsearch/ElasticSearchRecoveryScenario4.pdf), anzeigen, wie jede Knoten ist offline und dann wieder von erneut nacheinander. Jeder Knoten ist für 5 Minuten neu gestartet wird abgeschaltet zu diesem Zeitpunkt der nächste Knoten in Sequenz beendet wird.

Beachten Sie die folgenden Punkte:

- Während der einzelnen Knoten neu gestartet wird, bleibt die Leistung im Hinblick auf Durchsatz und Antwort Zeiten einigermaßen selbst.

- Festplatten-Aktivität erhöht für die einzelnen Knoten für kurze Zeit wie sie wieder online geschaltet ist. Dies ist höchstwahrscheinlich aufgrund des Wiederherstellungsvorgangs parallelen weiterleiten Änderungen, die aufgetreten sind, während der Knoten unten war.

- Wenn ein Knoten offline gestellt, auftreten, Spitzen im Netzwerkaktivitäten in den verbleibenden Knoten. Spitzen auftreten auch auf, wenn ein Knoten neu gestartet wird.

- Nach der letzte Knoten wieder verwendet wird, gibt das System einen Zeitraum signifikante Volatilität aus. Dies wurde wahrscheinlich durch des Wiederherstellungsvorgangs zum Synchronisieren von Änderungen jeden Knoten, und stellen Sie sicher, dass alle Replikate und deren entsprechenden mehrere Shards hinweg konsistent sind Probleme verursacht. Diese leistungsgesteuert Ursachen aufeinanderfolgende Massen Vorgänge auf Timeout einfügen und nicht bestanden an einem Punkt. Der Fehler gemeldet jedem Fall wurden:

```
Failure -- BulkDataInsertTest17(org.apache.jmeter.protocol.java.sampler.JUnitSampler$AnnotatedTestCase): java.lang.AssertionError: failure in bulk execution:
[1]: index [systwo], type [logs], id [AVEg0JwjRKxX_sVoNrte], message [UnavailableShardsException[[systwo][2] Primary shard is not active or isn't assigned to a known node. Timeout: [1m], request: org.elasticsearch.action.bulk.BulkShardRequest@787cc3cd]]

```

Nachfolgende experimentieren angezeigt wurden, dass dieser Fehler, Einführung in eine Verzögerung von wenigen Minuten zwischen und Ausschalten der einzelnen Knoten beseitigt, sodass es wahrscheinlich zurückzuführen Konflikte des Wiederherstellungsvorgangs versuchen wurde, mehrere Knoten gleichzeitig wiederherzustellen und die Massen einfügen Vorgänge versuchen Tausende von neuen Dokumenten zu speichern.


## <a name="summary"></a>Zusammenfassung

Ausgeführten Tests angegeben, die:

- Elasticsearch wurde hochgradig flexibel in Bezug auf die am häufigsten verwendeten Fehlerarten wahrscheinlich in einem Cluster ausgeführt werden.

- Elasticsearch kann schnell wiederherstellen, wenn ein durchdachter Cluster unterliegen schwerwiegenden Datenverlust auf einem Knoten ist. Dies kann geschehen, wenn Sie zum Speichern von Daten in temporärer Speicher Elasticsearch konfigurieren und der Knoten anschließend nach einem Neustart reprovisioned ist. Diese Ergebnisse anzeigen, die auch in diesem Fall die Risiken Ihrer Verwendung temporärer Speicher am ehesten, durch die Leistungsvorteile, die diese Klasse Speicher bietet aufgewogen.

- In den ersten drei Szenarien aufgetreten keine Fehler in gleichzeitigen Massen einfügen, Aggregation und Filter Abfrage Auslastung ein Knoten offline und wiederhergestellte geöffnet wurde.

- Nur das letzte Szenario angegeben möglichem, und diese Verlust betroffen nur neue Daten hinzugefügt werden. Es empfiehlt in Clientanwendungen Durchführung Erfassung von Daten in diese Wahrscheinlichkeit verringern, indem Sie Wiederholung Einfügevorgänge, die nicht als der Typ des Fehlers gemeldet höchstwahrscheinlich vorübergehend sein wird.

- Die Ergebnisse des letzten Tests anzeigen auch, wenn Sie geplante Wartung der Knoten in einem Cluster durchführen, Dienstleistung Leistung, wenn Sie einige Minuten zwischen und ausschalten, einem Knoten und dem nächsten zulassen. In einer ungeplanten Situation (z. B. Wiederverwendung Knoten nach dem Durchführen einer Aktualisierung Betriebssystem Datencenter) müssen Sie weniger steuern, wie und wann Knoten nach unten erfasste und neu gestartet werden. Die Anzahl von Konflikten, die tritt auf, wenn Elasticsearch versucht, um den Status der Cluster wiederherzustellen, nach sequenziellen Knoten Ausfall Zeitlimit und Fehlern führen können. 

[Verwalten der Verfügbarkeit von virtuellen Computern]: ../articles/virtual-machines/virtual-machines-linux-manage-availability.md
[Ausführen der automatisierten Elasticsearch Stabilität Tests]: guidance-elasticsearch-running-automated-resilience-tests.md
