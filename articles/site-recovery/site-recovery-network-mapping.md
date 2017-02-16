<properties
    pageTitle="Vorbereiten für Schutz von Hyper-V-virtuellen Computern mit VMM in Azure Website Wiederherstellung Netzwerk-Zuordnung | Microsoft Azure"
    description="Netzwerk-Zuordnung Replikation Hyper-V virtuellen Computern aus einem lokalen Rechenzentrum Azure oder einem sekundären Standort einrichten."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>


# <a name="prepare-network-mapping-for-hyper-v-virtual-machine-protection-with-vmm-in-azure-site-recovery"></a>Bereiten Sie für Schutz von Hyper-V-virtuellen Computern mit VMM in Azure Website Wiederherstellung Netzwerk-Zuordnung vor

Azure Website Wiederherstellung beiträgt zu Ihrer Business Continuity- und Disaster Wiederherstellung (BCDR) Strategie durch Replikation, Failover und Wiederherstellung von virtuellen Computern und physischen Servern orchestriert.

In diesem Artikel werden die Netzwerk-Zuordnung, die Einstellungen im Netzwerk optimal konfigurieren, wenn Sie Hyper-V-virtuellen Computern repliziert Website Wiederherstellung verwenden, hilft in VMM Wolken zwischen zwei lokalen Datencenter oder zwischen einer lokalen Datacenter und Azure gespeichert. Beachten Sie, dass, wenn Sie Hyper-V virtuelle Computer ohne eine VMM-Cloud repliziert sind oder Replikation virtuelle VMware-Computer oder physischen Servern sind nicht in diesem Artikel relevant ist.

Posten Sie Kommentare oder Fragen am Ende dieses Artikels oder im [Azure Wiederherstellung Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>(Übersicht)

Netzwerk-Zuordnung wird verwendet, wenn Hyper-V-virtuellen Computern Azure oder einem sekundären Datencenter, repliziert Hyper-V Replica oder SAN Replikation Azure Website Wiederherstellung bereitgestellt wird.

- **Replikation von Hyper-V virtuelle Computer in VMM Wolken zwischen zwei lokalen Rechenzentren**– Netzwerk-Zuordnung Karten zwischen virtuellen Computer Netzwerken auf eine Quelle VMM-Server und virtueller Computer Netzwerken auf einem Ziel VMM-Server die folgenden Schritte durchführen:

    - **Verbinden mit virtuellen Computern nach Failover**– stellt sicher, dass virtuellen Computern mit entsprechenden Netzwerken nach Failover verbunden werden soll. Replikat virtuellen Computers wird mit dem Zielnetzwerk verbunden werden, die mit dem Netzwerk Datenquelle zugeordnet ist.
    - **Ort Replikat virtuellen Computern auf Hostservern**– Replikat virtuellen Computern auf Hyper-V Hostservern optimal zu platzieren. Replikat virtuellen Computern wird auf Hosts platziert werden, die die zugeordnete virtueller Computer Netzwerke zugreifen können.
    - **Keine Netzwerk-Zuordnung**– Wenn Sie Netzwerk-Zuordnung nicht konfiguriert haben, wird nicht repliziert virtuellen Computern mit allen Netzwerken virtuellen Computer verbunden sein, nach Failover.

- **Hyper-V repliziert virtuellen Computern in einer lokalen VMM in Azure cloud**– Netzwerk-Zuordnung Karten zwischen Netzwerken virtueller Computer, auf dem Server der Quelle VMM und Adressieren Azure Netzwerke, um die folgenden Schritte ausführen:
    - **Verbinden von virtuellen Computern nach Failover**– alle Maschinen, welche Failover im selben Netzwerk kann miteinander, unabhängig von der Wiederherstellung planen sie befinden sich in eine Verbindung herstellen.
    - **Netzwerk-Gateway**– Wenn Sie ein Netzwerk-Gateway am Ziel Azure Netzwerk eingerichtet ist, virtuellen Computern können die Verbindung zu anderen lokalen virtuellen Computern.
    - **Keine Netzwerk-Zuordnung**– Wenn Sie Netzwerk-Zuordnung zu konfigurieren nicht nur virtuellen Computern dieser Failover im gleichen Wiederherstellungsplan wird Verbindung miteinander nach Fail über in Azure hergestellt werden.


## <a name="network-mapping-example"></a>Beispiel für Netzwerk-Zuordnung

Netzwerk-Zuordnung kann zwischen virtuellen Computer Netzwerken auf zwei VMM-Servern oder auf einem einzelnen, wenn zwei Websites vom gleichen Server verwaltet werden VMM-Server konfiguriert werden. Wenn Zuordnung ordnungsgemäß konfiguriert ist und die Replikation aktiviert ist, ein virtuellen Computers zu primärer Speicherort wird mit einem Netzwerk verbunden sein, und sein Replikat am Zielort wird deren zugeordnete Netzwerk verbunden sein.

Wenn Netzwerke ordnungsgemäß in VMM, eingerichteten Wenn Sie einem Ziel virtueller Computer Netzwerk bei Network Zuordnung auswählen, werden die VMM Quelle Wolken, die das Quelle virtueller Computer-Netzwerk verwenden, zusammen mit den verfügbaren Ziel virtueller Computer Netzwerken auf die Ziel-Wolken angezeigt, die für den Schutz verwendet werden.

Hier ist ein Beispiel, um dieses Verfahren zu veranschaulichen. Nachstehend wird eine Organisation mit zwei Standorten in New York und Chicago.

**Speicherort** | **VMM-server** | **Virtueller Computer Netzwerken** | **Zugeordneten**
---|---|---|---
Berlin | VMM-NewYork| VMNetwork1-NewYork | VMNetwork1-Chicago zugeordneten
 |  | VMNetwork2-NewYork | Nicht zugeordnete
Chicago | VMM-Chicago| VMNetwork1-Chicago | VMNetwork1-NewYork zugeordneten
 | | VMNetwork2-Chicago | Nicht zugeordnete

Mit diesem Beispiel:

- Beim Erstellen ein Replikat virtuellen Computers für alle virtuellen Computern, die mit VMNetwork1-NewYork verbunden ist, wird er mit VMNetwork1-Chicago verbunden werden.
- Wenn ein Replikat virtuellen Computern für VMNetwork2-NewYork oder VMNetwork2-Chicago erstellt wurde, wird er keines Netzwerk verbunden.

So sieht wie VMM Wolken in unserer Organisation (Beispiel) und die logischen Netzwerke Wolken zugeordnet eingerichtet sind.

### <a name="cloud-protection-settings"></a>Cloud Schutz Einstellungen

**Geschützte cloud** | **Schützen von cloud** | **Logisches Netzwerk (New York)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>NV</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>
SilverCloud2 | <p>NV</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>

### <a name="logical-and-vm-network-settings"></a>Logisch und virtuellen Computer Netzwerkeinstellungen

**Speicherort** | **Logisches Netzwerk** | **Zugeordneten virtuellen Computer Netzwerk**
---|---|---
Berlin | LogicalNetwork1-NewYork | VMNetwork1-NewYork
Chicago | LogicalNetwork1-Chicago | VMNetwork1-Chicago
 | LogicalNetwork2Chicago | VMNetwork2-Chicago

### <a name="target-networks"></a>Ziel Netzwerken

Basierend auf diese Einstellungen, wenn Sie das Ziel virtueller Computer Netzwerk auswählen, zeigt die folgende Tabelle die Optionen, die zur Verfügung stehen.

**Wählen Sie aus** | **Geschützte cloud** | **Schützen von cloud** | **Zielnetzwerk verfügbar**
---|---|---|---
VMNetwork1-Chicago | SilverCloud1 | SilverCloud2 | Verfügbar
 | GoldCloud1 | GoldCloud2 | Verfügbar
VMNetwork2-Chicago | SilverCloud1 | SilverCloud2 | Nicht verfügbar
 | GoldCloud1 | GoldCloud2 | Verfügbar



## <a name="multiple-subnets"></a>Mehrere Subnetze

Wenn das Zielnetzwerk verfügt über mehrere Subnetze und von diesen Subnetzen denselben Namen wie dem Subnetz hat, auf dem sich die Quelle virtuellen Computern befindet, wird dann Replikat virtuellen Computers mit diesem Ziel Subnetz nach Failover verbunden sein. Ist kein Ziel Subnetz mit einem übereinstimmenden Namen, wird der virtuellen Computern mit dem ersten Subnetz im Netzwerk verbunden.


### <a name="failback"></a>Failback

Um anzuzeigen, was passiert, wenn Failback (reverse Replikation), wird angenommen, dass VMNetwork1-NewYork VMNetwork1-Chicago, mit den folgenden Einstellungen zugeordnet ist.


**Virtuellen Computern** | **Virtueller Computer-Netzwerk verbunden ist.**
---|---
VM1 | VMNetwork1-Netzwerk
VM2 (Kopie VM1) | VMNetwork1-Chicago

Mit diesen Einstellungen was passiert in verschiedene mögliche Szenarios.

**Szenario** | **Ergebnis**
---|---
Keine Änderungen in den Eigenschaften des virtuellen Computer-2 nach Failover. | Virtueller Computer-1 bleibt mit dem Quellnetzwerk verbunden.
Netzwerkeigenschaften des virtuellen Computer-2 werden nach Failover geändert und getrennt wird. | Virtueller Computer-1 wird getrennt.
Netzwerkeigenschaften des virtuellen Computer-2 werden nach Failover geändert und mit VMNetwork2-Chicago verbunden ist. | Wenn VMNetwork2-Chicago zugeordnet wird, wird virtueller Computer-1 getrennt.
Zuordnung VMNetwork1 Chicago Netzwerk wird geändert. | Virtueller Computer-1 wird mit dem Netzwerk zugeordnet jetzt VMNetwork1-Chicago verbunden sein.


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun ein besseres Verständnis der Netzwerk-Zuordnung, [Erste Schritte mit der Bereitstellung der Website Wiederherstellung](site-recovery-best-practices.md)stehen Ihnen.
