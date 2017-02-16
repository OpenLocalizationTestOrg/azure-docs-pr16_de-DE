<properties
   pageTitle="Azure Service Fabric Wiederherstellung | Microsoft Azure"
   description="Azure Service-Struktur bietet die Funktionen, die alle Arten von Datenverluste behandelt. In diesem Artikel werden die Arten von Problemen, die auftreten können und wie diese behandelt."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="seanmck"/>

# <a name="disaster-recovery-in-azure-service-fabric"></a>Wiederherstellung in Azure Service Architektur

Ein wichtiger Bestandteil Vorführen einer hoher Verfügbarkeit Cloud-Anwendungs müssen Sie sicherstellen, dass alle andere Arten von Fehlern überstehen, einschließlich der Empfänger, die sich außerhalb des Steuerelements befinden. Dieser Artikel beschreibt das physische Layout einer Azure Service Fabric Cluster im Kontext mögliche Datenverluste und enthält Anleitungen zum Umgang mit solchen Datenverluste zu beschränken oder das Risiko von Ausfallzeiten oder Datenverluste zu unterdrücken.

## <a name="physical-layout-of-service-fabric-clusters-in-azure"></a>Physisches Layout Dienst Stoffbahn Cluster in Azure

Um das Risiko, dass verschiedene Arten von Fehlern zu verstehen, ist es nützlich zu wissen, wie Cluster physisch in Azure angeordnet werden.

Wenn Sie einen Dienst Fabric Cluster in Azure erstellen, müssen Sie einen Bereich aus, in dem sie gehostet werden. Die Azure-Infrastruktur stellt dann die Ressourcen für diesen Cluster innerhalb des Bereichs liegen, allem die Anzahl der virtuellen Computern (virtuellen Computern) angefordert. Betrachten wir genauer wie und wo diese virtuellen Computern bereitgestellt werden.

### <a name="fault-domains"></a>Fehlerstrukturanalyse-Domänen

Standardmäßig werden die virtuellen Computern im Cluster gleichmäßig auf logischen Gruppen bekannt als Fehlerstrukturanalyse Domänen (FDs), die die Computer basierend auf mögliche Fehler in der Hosthardware segmentieren verteilt. Insbesondere wenn zwei virtuellen Computern in zwei unterschiedlichen FDs befinden, können Sie sicher sein, dass sie den gleichen Quell- oder Netzwerk Switch nicht gemeinsam nutzen. Daher wirkt einer lokalen Netzwerk oder Ausfall Auswirkungen ein virtueller Computer andere, gleicht Service-Struktur, bis die Arbeitslast des Computers reagiert nicht innerhalb der Cluster neu verteilen sich nicht.

Sie können das Layout von Ihren Cluster über Fehlerstrukturanalyse Domänen mithilfe der Cluster Karte in [Dienst Fabric Explorer](service-fabric-visualizing-your-cluster.md)bereitgestellten darstellen:

![Fehlerstrukturanalyse-Domänen in Dienst Fabric Explorer hinweg Knoten][sfx-cluster-map]

>[AZURE.NOTE] Die anderen Achse in der Tabelle Cluster zeigt Upgrade Domänen, die Knoten basierend auf der geplanten Wartungsaktivitäten logisch zu gruppieren. Dienst Fabric Cluster in Azure werden immer über fünf Upgrade Domänen angeordnet.

### <a name="geographic-distribution"></a>Geografische Verteilung

Es gibt zurzeit [26 Azure Regionen der ganzen Welt][azure-regions], mit mehreren mehr angekündigt. Eine einzelne Region kann eine oder mehrere physische Daten Centers je nach Bedarf und die Verfügbarkeit von geeignete Speicherorten zwischen Faktoren enthalten. Beachten Sie jedoch, dass auch in Bereiche, die mehrere physische Daten Centers enthalten, es ist keine Garantie, dass im Cluster virtuellen Computern über diese physischen Speicherorte gleichmäßig verteilt werden. Tatsächlich um, sind derzeit alle virtuellen Computern für einen angegebenen Cluster innerhalb einer einzelnen physischen Website nach der Bereitstellung.

## <a name="dealing-with-failures"></a>Umgang mit Fehlern

Es gibt verschiedene Arten von Fehlern, die Ihre Cluster, jede mit einem eigenen Reduzierung auswirken können. Wir sehen sich diese in der Reihenfolge der Wahrscheinlichkeit ausgeführt.

### <a name="individual-machine-failures"></a>Einzelne Computer-Fehlern

Wie erwähnt, wird von einzelne Computer Fehlern, in dem virtuellen Computer oder in der Hardware oder Software, die sie innerhalb einer Domäne Fehlerstrukturanalyse Hostinganbieter keine Risiko auf eigene darstellen. Fabric Service wird in der Regel innerhalb von Sekunden den Ausfall erkennen und Antworten, entsprechend basierend auf den Status der Cluster. Wenn Sie der Knoten als Host für die primäre Replikate für eine Partition verwendet wurde, gewählt wird eine neue primäre z. B. aus der sekundäre-Partitionsreplikaten. Wenn Azure den Fehler beim Computer sichern bringt, wird es treten Sie dem Cluster automatisch und seinen Anteil an die Arbeitsbelastung erneut ausführen.

### <a name="multiple-concurrent-machine-failures"></a>Mehrere gleichzeitige Computer-Fehlern

Während der Fehlerstrukturanalyse Domänen das Risiko von Fehlern gleichzeitige Computer beträchtlich verringern, ist immer die Möglichkeit nach mehreren zufällige Fehlern, um gleichzeitig mehrere Computer in einem Cluster zu bringen.

Im Allgemeinen, solange eine Mehrzahl der Knoten sind verfügbar, weiterhin der Cluster ausgeführt werden, obgleich am unteren Kapazität als dynamische Replikate in eine kleinere Menge Autos verpackt erste und weniger statusfreie Instanzen stehen zu laden.

#### <a name="quorum-loss"></a>Quorum Verlust

Wenn die Mehrzahl der Replikate für eine dynamische Servicepartition wechseln nach unten, wird dieser Partition Status auf "Quorum Verlust". An diesem Punkt stoppt Fabric Service wird in dieser Partition, um sicherzustellen, dass der Zustand konsistent und zuverlässig bleibt schreibt. Wir werden wirksam, auswählen einen Zeitraum nicht verfügbar sind, um sicherzustellen, dass Clients nicht informiert werden, dass ihre Daten gespeichert wurde, wenn war wirklich nicht annehmen. Beachten Sie, wenn Sie bis zum Lesen von sekundären Replikaten für diesen Dienst dynamische zulassen abonniert haben, Sie weiterhin können auf, das Lesen von Vorgängen in diesem Zustand auszuführen. Eine Partition bleibt in Quorum Verlust angezeigt, bis eine ausreichende Anzahl von Replikaten der-oder diejenige oder der Cluster-Administrator das System, um das Verschieben auf mithilfe der [Reparatur ServiceFabricPartition API]erzwingt[repair-partition-ps].

>[AZURE.WARNING] Eine Reparaturaktion durchgeführt werden, während primäre Replikat nicht verfügbar ist, führt zu Datenverlust.

System Services können Quorum Verlust, auch mit der Auswirkung wird speziell für den betreffenden Dienst beeinträchtigt werden. Quorum Verlust im naming Service wird beispielsweise mit einer namensauflösung von, beeinflussen, während Quorum Verlust der Failover-Manager-Dienst Erstellung einer neuen Dienst und Failovers blockiert wird. Beachten Sie, dass im Gegensatz zu für Ihre eigenen Dienste, versucht, System Services reparieren *nicht* empfohlen. In diesem Fall empfiehlt es sich einfach warten, bis die nach-unten Replikate zurückzukehren.

#### <a name="minimizing-the-risk-of-quorum-loss"></a>Minimieren des Risikos Quorum Verlust

Sie können das Risiko Quorum Verlust minimieren, indem Sie vergrößern Ziel Replikat Festlegen des Diensts. Es empfiehlt sich, die Anzahl der Replikationen betrachten Sie müssen in Bezug auf die Anzahl der nicht verfügbaren Knoten, die, denen Sie am tolerieren können, nachdem bleiben Sie verfügbar für schreibt, planmäßigen dieser Anwendung denken, oder Cluster Upgrades umso Knoten vorübergehend nicht verfügbar sind, sowie Hardware-Fehlern.

Erwägen Sie die folgenden Beispiele unter der Voraussetzung, dass Sie Ihre Dienste, damit eine MinReplicaSetSize drei, den kleinsten Wert für die Herstellung Services empfohlen konfiguriert haben. Mit einer TargetReplicaSetSize von drei (ein primärer und zwei sekundäre) ein Hardware-Fehlers bei einer Aktualisierung (zwei Replikate unten) führt zu Fehlern im Quorum Verlust und der Dienst werden erst schreibgeschützt. Sie können auch wenn Sie fünf Replikate haben, möchten Sie möglicherweise zwei auftretende Fehler während des Upgrades (drei Replikate nach unten) zu verarbeiten, wie die restlichen beiden Replikate weiterhin ein Quorum innerhalb der minimalen Replikatgruppe bilden können.

### <a name="data-center-outages-or-destruction"></a>Data Center Ausfall eines oder Beseitigung

In Ausnahmefällen können physische Data Center Verlust der Power oder Netzwerk-Konnektivität vorübergehend nicht verfügbar machen. In diesen Fällen Ihren Cluster Service Fabric und Applikationen ebenso werden nicht zur Verfügung, aber Ihre Daten werden beibehalten. Für Cluster, die in Azure ausführen, können Sie Updates auf Ausfall auf der [Workflowstatusseite Azure]anzeigen[azure-status-dashboard].

Im weitgehend Ereignis, dass eine gesamte physische Data Center gelöscht wird verloren alle Dienst Fabric Cluster es gehostet, zusammen mit ihren Status.

Zum Schutz gegen diese Möglichkeit ist entscheidend, um regelmäßig zu einem Geo redundante Speicher [Sichern Ihrer Zustand](service-fabric-reliable-services-backup-restore.md) , und stellen Sie sicher, dass Sie die Möglichkeit zum Wiederherstellen überprüft haben. Wie oft eine Sicherungskopie werden abhängig von der Wiederherstellung Punkt Ziel (RPO). Auch wenn Sie nicht vollständig sichern und Wiederherstellen noch implementiert haben, sollten Sie einen Ereignishandler für Implementieren der `OnDataLoss` Ereignis, damit Sie sich anmelden können, wenn er auftritt, als folgt:

```c#
protected virtual Task<bool> OnDataLoss(CancellationToken cancellationToken)
{
  ServiceEventSource.Current.ServiceMessage(this, "OnDataLoss event received.");
  return Task.FromResult(true);
}
```


### <a name="software-failures-and-other-sources-of-data-loss"></a>Software-Fehlern und anderen Quellen Verlust von Daten

Als Ursache von Datenverlust sind Code Grafikhardware in Services, human Fehlern in der Ausführung und Sicherheitsverstöße häufiger als weit verbreitet Data Center Fehlern. In allen Fällen die Wiederherstellungsstrategie ist jedoch identisch: Optimieren von regelmäßigen Sicherungen aller dynamische Dienste, und probieren Sie Ihre Möglichkeit zum Wiederherstellen dieser Status.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie verschiedene mit [Prüfbarkeit Framework](service-fabric-testability-overview.md) simulieren
- Lesen Sie weitere Ressourcen zur Wiederherstellung und hoher Verfügbarkeit. Microsoft hat eine große Datenmenge Anleitungen zu diesen Themen veröffentlicht wird. Während einige Dokumente auf bestimmte Techniken zur Verwendung in anderen Produkten beziehen, enthalten sie viele allgemeine bewährte Methoden, die ebenfalls im Dienst Fabric Kontext angewendet werden können:
 - [Verfügbarkeitscheckliste](../best-practices-availability-checklist.md)
 - [Eine Disaster Wiederherstellung Drillup ausführen](../sql-database/sql-database-disaster-recovery-drills.md)
 - [Wiederherstellung und hohe Verfügbarkeit für Azure applications][dr-ha-guide]


<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
