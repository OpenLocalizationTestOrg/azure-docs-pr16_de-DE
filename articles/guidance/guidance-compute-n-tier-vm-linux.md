<properties
   pageTitle="Ausführen von Linux virtuellen Computern für eine mehrstufige Architektur auf Azure | Microsoft Azure"
   description="So eine mehrstufige Architektur in Microsoft Azure Linux virtuellen Computern ausgeführt werden."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="mwasson"/>

# <a name="running-linux-vms-for-an-n-tier-architecture-on-azure"></a>Ausführen von Linux virtuellen Computern für eine mehrstufige Architektur auf Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]


> [AZURE.SELECTOR]
- [Für eine mehrstufige Architektur Azure Linux virtuellen Computern ausgeführt](guidance-compute-n-tier-vm-linux.md)
- [Windows-virtuellen Computern für eine mehrstufige Architektur auf Windows Azure ausgeführte](guidance-compute-n-tier-vm.md)

Dieser Artikel enthält eine Reihe von bewährten Methoden für die Ausführung von Linux virtuellen Computern (virtuellen Computern) für eine Anwendung mit einer mehrstufige Architektur. Es wird erstellt, die [mehrere virtuelle Computer auf Azure]-Artikel[multi-vm].

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle: [Ressourcenmanager] [ resource-manager-overview] und klassischen. In diesem Artikel wird Ressourcenmanager, in dem Bereitstellung neuer Microsoft empfiehlt verwendet.

## <a name="architecture-diagram"></a>Architekturdiagramm

Es gibt Variationen mehrstufige Architekturen aus. Die Unterschiede sollten nicht in den meisten Fällen, für die Zwecke Tips Rolle. In diesem Artikel wird vorausgesetzt, eine typische Ebene 3 Web app:

- **Webebene.** Übernimmt eingehende HTTP-Anfragen an. Antworten werden durch diese Ebene zurückgegeben.

- **Business Ebene.** Implementierung von Geschäftsprozessen und andere funktionsübergreifendes Logik für das System.

- **Datenbankebene.** Bietet beständige Datenspeicher Apache Cassandra für hohe Verfügbarkeit verwenden.

> Ein Visio-Dokument, das diese Architekturdiagramm enthält wird unter der [Microsoft download Center]herunterladen[visio-download]. Bei diesem Diagramm handelt, klicken Sie auf "berechnen - Multi Ebene (Linux) Seite.

![[0]][0]

- **Verfügbarkeit Sätze.** Erstellen einer [Verfügbarkeit festlegen] [ azure-availability-sets] für jede gestuft, und mindestens zwei virtuellen Computern in jede Ebene bereitstellen. Dieser Ansatz ist erforderlich, um die Verfügbarkeit [Vereinbarung zum SERVICELEVEL] erreichen[ vm-sla] für virtuelle Computer.

- **Subnetze.** Erstellen Sie ein separates Subnetz für jede Ebene an. Geben Sie die Adresse Bereichs- und Subnetz Maske [CIDR] -Notation verwenden. 

- **Lastenausgleich.** Verwenden eines [Internet zugänglichen Lastenausgleich] [ load-balancer-external] Sie eingehenden Internetdatenverkehr an der Webebene und eine [interne Lastenausgleich] verteilen[ load-balancer-internal] Netzwerkverkehr von der Webebene an der Business Ebene verteilen.

- **Jumpbox**. Eine _Jumpbox_, auch [Bastionhost], genannt ist ein virtueller Computer im Netzwerk, die Administratoren für die Verbindung mit den anderen virtuellen Computern verwenden. Die Jumpbox verfügt über eine NSG, die nur aus auf weißer Liste öffentliche IP-Adressen remote Verkehr zulässt. Die NSG sollte secure Shell (SSH) Datenverkehr werden kann.

- Für die **Überwachung**. Überwachen von Software wie [Nagios], [Zabbix]oder [Icinga] können Sie einen Einblick in die Antwortzeit, Verfügbarkeit virtueller Computer und den allgemeinen Zustand des Computers erteilen. Installieren von Software für die Überwachung auf einen virtuellen Computer, die in einem separaten Management Subnetz platziert wird.

- **NSGs**. Verwenden Sie [Sicherheitsgruppen Netzwerk] [ nsg] (NSGs) zum Einschränken der Netzwerkdatenverkehr innerhalb der VNet. In den hier gezeigten 3-Ebenen-Architektur annehmen die Datenbankebene angenommen, nicht Verkehr über das Web-front-End nur aus der Business Ebene und das Management Subnetz.

- **Apache Cassandra Datenbank**. Hohe Verfügbarkeit bei der Datenebene, bereitgestellt durch das Aktivieren der Replikation und Failover.

## <a name="recommendations"></a>Empfehlungen

Azure bietet viele Ressourcentypen, sodass diese Architektur Bezug genommen kann und anderen Ressourcen viele verschiedene Arten bereitgestellt. Wir haben eine Ressourcenmanager Azure-Vorlage, um die Verweis-Architektur zu installieren, die diese Empfehlungen folgt bereitgestellt. Wenn Sie auch erstellen eigene Bezug Architektur führen Sie diese Empfehlungen, wenn Sie eine bestimmte Anforderung verfügen, die nicht passen empfohlen.

### <a name="vnet--subnets"></a>VNet / Subnetze

Wenn Sie die VNet erstellen, weisen Sie genügend Speicherplatz für die Adresse für die Subnetze, die Sie benötigen. Geben Sie die [CIDR] -Notation mit VNet Adresse-Bereich und Subnetz Maske an. Verwenden Sie ein Leerzeichen Adresse, die in der Standardansicht [private IP-Adressblöcke]fällt[private-ip-space], welche 10.0.0.0/8, 172.16.0.0/12 und 192.168.0.0/16 sind.

Auswählen eines Adressbereichs, das nicht mit Ihrem Netzwerk lokal überlappt an, für den Fall, dass Sie später Einrichten eines Gateways zwischen den VNet und Ihrem Netzwerk lokal müssen. Nachdem Sie die VNet erstellt haben, können Sie Bereich nicht ändern.

Entwerfen Sie Subnetze mit Funktionalität und Sicherheit Anforderungen berücksichtigen. Alle virtuellen Computern innerhalb der gleichen Ebene oder Rolle sollte in demselben Subnetz, wechseln Sie eine Begrenzungslinie Sicherheit erfolgen kann. Weitere Informationen zu VNets und Subnetze entwerfen, finden Sie unter [Planen und Entwerfen Azure-virtuellen Netzwerken][plan-network].

Geben Sie den Abstand Adresse für das Subnetz für jedes Subnetz in CIDR-Notation. '10.0.0.0/24' erstellt beispielsweise einen Zellbereich 256 Adressen. (Virtuellen Computern können 251 dieser; fünf reserviert werden. Finden Sie unter dem [virtuelles Netzwerk häufig gestellte Fragen zu][vnet faq].) Stellen Sie sicher, dass die Adressbereiche über Subnetze überlappen nicht.

### <a name="load-balancers"></a>Lastenausgleich

Externe Lastenausgleich verteilt Internet den Datenverkehr in der Webebene an. Erstellen Sie eine öffentliche IP-Adresse für diese Lastenausgleich. Finden Sie unter [Erstellen einer Lastenausgleich Internet zugänglichen][lb-external-create].

Interner Lastenausgleich verteilt Netzwerkverkehr von der Webebene an die Schicht Business an. Diese Lastenausgleich eine private IP-Adresse verleihen, erstellen Sie eine Front-End-IP-Konfiguration, und das Subnetz für die Leiste Business zugeordnet. Finden Sie unter [Erste Schritte beim Erstellen eines internen Lastenausgleich][lb-internal-create].

### <a name="cassandra"></a>Cassandra

Wir empfehlen [DataStax Enterprise] [ datastax] bei Herstellung verwenden, aber diese Empfehlungen beziehen sich auf eine beliebige Cassandra Edition. Weitere Informationen zum Ausführen von DataStax in Azure, finden Sie unter [DataStax Bereitstellungshandbuch für Azure Unternehmen][cassandra-in-azure]. 

Setzen Sie die virtuellen Computern für einen Cluster Cassandra in einem Satz Verfügbarkeit, um sicherzustellen, dass die Replikate Cassandra über mehrere Fehlerstrukturanalyse Domänen verteilt sind und Aktualisieren von Domänen aus. Weitere Informationen zu Fehlerstrukturanalyse und Upgrade-Domänen, finden Sie unter [Verwalten der Verfügbarkeit von virtuellen Computern][availability-sets-manage]. 

Konfigurieren von 3 Fehlerstrukturanalyse Domänen (das Maximum) pro Verfügbarkeit festlegen. 

Konfigurieren von 18 Upgrade Domänen pro Verfügbarkeit festlegen. Dadurch werden die maximale Anzahl von Upgrade Domänen als über den Fehlerstrukturanalyse-Domänen weiterhin gleichmäßig verteilt werden kann.   

Konfigurieren von Knoten in den Shapes für Gestelle-fähige Modus. Fehlerstrukturanalyse-Domänen Gestelle in Zuordnen der `cassandra-rackdc.properties` Datei.

Sie benötigen kein Lastenausgleich vor dem Cluster. Der Client direkt auf einen Knoten im Cluster eine Verbindung herstellt.

### <a name="jumpbox"></a>Jumpbox

Platzieren Sie den Jumpbox aus, in der gleichen VNet als die anderen virtuellen Computern, aber in einem separaten Management Subnetz.

Erstellen Sie eine [öffentliche IP-Adresse] für die Jumpbox ein.

Verwenden Sie eine kleine virtueller Speicher für die Jumpbox, wie etwa Standard A1 ein.

Konfigurieren der NSGs für das Webebene, Business Ebene und Datenbank Ebene Subnetze damit administrative (SSH) Datenverkehr aus dem Subnetz Management passieren kann.

Um die Jumpbox zu sichern, erstellen Sie eine NSG und mit dem Subnetz Jumpbox anwenden. Hinzufügen einer NSG Regel, die SSH Verbindungen nur aus einer Gruppe auf weißer Liste öffentliche IP-Adressen zulässt.

Die NSG kann das Subnetz oder die Jumpbox NIC angefügt werden In diesem Fall wird empfohlen, es zu der Netzwerkkarte anfügen, sodass nur für die Jumpbox SSH Datenverkehr zulässig ist, auch wenn Sie anderen virtuellen Computern im selben Subnetz hinzufügen.

Lassen Sie nicht SSH-Zugriff aus dem öffentlichen Internet mit den virtuellen Computern, die die Anwendung Arbeitsbelastung ausgeführt werden. In diesem Fall muss alle SSH-Zugriff auf diese virtuellen Computern der Jumpbox überstehen. Ein Administrator anmeldet, das die Jumpbox, und klicken Sie dann in den anderen virtuellen Computer aus der Jumpbox protokolliert. Die Jumpbox ermöglicht SSH Datenverkehr aus dem Internet, aber nur von bekannten, auf weißer Liste IP-Adressen.

## <a name="availability-considerations"></a>Verfügbarkeit Aspekte

Setzen Sie jeden Stufe oder Rolle in einem separaten Verfügbarkeit Satz ein. Setzen Sie nicht virtuellen Computern aus unterschiedlichen Ebenen in der gleichen Verfügbarkeit festlegen. 

Auf der Datenbankstufe übersetzt gibt es mehrere virtuellen Computern automatisch in einer Datenbank mit hoher Verfügbarkeit nicht. Bei einer relationalen Datenbank müssen Sie in der Regel Replikation und Failover verwenden, um hohe Verfügbarkeit zu erreichen.  

Wenn Sie höheren Verfügbarkeit als die [Azure Vereinbarung zum SERVICELEVEL für virtuelle Computer] benötigen[ vm-sla] zur Verfügung stellt, die Anwendung über zwei Bereiche hinweg zu reproduzieren und Azure Datenverkehr Manager Failoververarbeitung verwenden. Weitere Informationen finden Sie unter [Ausführen des Linux virtuellen Computern in mehreren Regionen hohen Verfügbarkeit][multi-dc].  

## <a name="security-considerations"></a>Zur Sicherheit

Verwenden von NSG Regeln zum Einschränken des Datenverkehrs zwischen Ebenen. Angenommen, in der oben dargestellten 3-Ebenen-Architektur kommuniziert die Webebene direkt mit der Datenbankebene nicht. Um dies zu erzwingen, sollte die Datenbankebene eingehenden Datenverkehr aus dem Web Ebene Subnetz blockieren.  

  1. Erstellen Sie einer NSG, und ordnen Sie es an die Datenbank Ebene Subnetz.

  2. Hinzufügen einer Regel, die alle eingehenden Datenverkehr aus der VNet verweigert. (Verwenden der `VIRTUAL_NETWORK` Tag in der Regel.) 

  3. Fügen Sie eine Regel mit höherer Priorität, der aus dem Business Ebene Subnetz eingehenden Verkehr zulässt. Mit dieser Regel überschreibt die vorherige Regel, und ermöglicht die Business Ebene mit der Datenbankebene sprechen.

  4. Fügen Sie einer Regel, die aus der Datenbank Ebene Subnetz selbst eingehenden Datenverkehr zulässt. Mit dieser Regel ermöglicht die Kommunikation zwischen virtuellen Computern in der Datenbankebene, die für Datenbankreplikation und Failover benötigt wird.

  5. Fügen Sie eine Regel, die SSH Datenverkehr aus dem Subnetz Jumpbox zulässt. Mit dieser Regel kann Administratoren die Datenbankebene von der Jumpbox Verbindung.

  > [AZURE.NOTE] Eine NSG hat [Regeln für das standardmäßige] [ nsg-rules] , die alle eingehenden Verkehr von innerhalb der VNet zulassen. Diese Regeln können nicht gelöscht werden, aber Sie können sie durch Erstellen von Regeln mit höherer Priorität überschreiben.

Fügen Sie eine virtuelle Netzwerk-Anwendung (NVA) zum Erstellen einer DMZ zwischen dem öffentlichen Internet und dem Azure virtuelle Netzwerk hinzu. NVA ist eine allgemeine Bezeichnung für eine virtuelle Anwendung, die Aufgaben wie Firewall, Paket Prüfung, Überwachung, benutzerdefinierte routing oder einer Vielzahl von anderen Vorgängen im Zusammenhang mit dem Netzwerk ausführen kann. Weitere Informationen finden Sie unter [Implementierung einer zwischen Azure und im Internet][dmz].

## <a name="scalability-considerations"></a>Aspekte Skalierbarkeit

Der Lastenausgleich verteilen Netzwerkverkehr den Web und Business Ebenen. Horizontal skalieren Sie, indem Sie neue Instanzen von virtuellen Computer hinzufügen. Beachten Sie, dass Sie die Ebenen Web und Business unabhängig voneinander, zu basierend auf Laden skalieren können. Klicken Sie zum Verringern der möglicher Komplikationen zurückzuführen, dass die Clientzugehörigkeit beibehalten müssen sollten statusfreie die virtuellen Computern in der Webebene. Die virtuellen Computern, die die Geschäftslogik hosten, sollten auch statusfrei sein.

## <a name="manageability-considerations"></a>Verwaltbarkeit Aspekte

Vereinfachen Sie das Management des gesamten Systems mit zentralisierte Verwaltungstools wie [Azure Automatisierung][azure-administration], [Microsoft Operations Management Suite][operations-management-suite], [Verwaltungsangestellte][chef], oder [Marionette][puppet]. Diese Tools können Diagnose- und Gesundheit Informationen aus mehreren virtuelle Computer bieten einen Überblick über das System erfasst konsolidieren.

## <a name="solution-deployment"></a>Lösung-Bereitstellung

Eine Bereitstellung für eine Verweis-Architektur, die diese Empfehlungen implementiert steht auf [Github][github-folder]. Dieser Bezug Architektur enthält eine Stufe Web, Business und einer Datenebene.

1. Klicken Sie auf die Schaltfläche unten.  
[! ["Bereitstellen Sie für Azure"] [1]][2]

2. Nachdem Sie der Link im Portal Azure geöffnet, geben Sie die Werte gehen Sie folgendermaßen vor: 
  - Der Name der **Ressourcengruppe** bereits in der Parameterdatei definiert ist, also wählen Sie **Neu erstellen** und geben Sie `ra-ntier-sql-network-rg` in das Textfeld ein.
  - Wählen Sie die Region im Dropdownfeld **Speicherort** aus.
  - Bearbeiten Sie die **Vorlage Stamm-Uri** oder die **Parameter Stamm-Uri** Textfelder nicht.
  - Überprüfen Sie die allgemeinen Geschäftsbedingungen, und klicken Sie auf das Kontrollkästchen **ich die allgemeinen Geschäftsbedingungen oben erwähnten zustimmen** .
  - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Überprüfen Sie, dass Azure Portals Benachrichtigung für eine Nachricht die Bereitstellung abgeschlossen ist.

4. Die Parameterdateien enthalten eine hartcodierte Administrator-Benutzernamen und Kennwörter, und es wird dringend empfohlen, Sie beide auf alle virtuellen Computern sofort ändern. Klicken Sie auf jedes virtuellen Computers Azure-Portal, und klicken Sie dann klicken Sie in das Blade **Support + Problembehandlung** auf **Kennwort zurücksetzen** . Wählen Sie im Dropdownfeld **Modus** **Kennwort zurücksetzen** aus, und wählen Sie dann einen neuen **Benutzernamen** und **ein Kennwort**. Klicken Sie auf die Schaltfläche **Aktualisieren** , um den neuen Benutzernamen und das Kennwort beibehalten werden.

## <a name="next-steps"></a>Nächste Schritte

Um hohe Verfügbarkeit für diese Architektur Bezug zu erreichen, wir empfehlen die [Bereitstellung auf mehrere Bereiche][multi-dc].

<!-- links -->

[azure-administration]: ../automation/automation-intro.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[availability-sets-manage]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[Bastionhost]: https://en.wikipedia.org/wiki/Bastion_host
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-consistency-usage]: http://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-in-azure]: https://docs.datastax.com/en/datastax_enterprise/4.5/datastax_enterprise/install/installAzure.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[datastax]: http://www.datastax.com/products/datastax-enterprise
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters-linux.md
[multi-vm]: guidance-compute-multi-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[öffentliche IP-Adresse]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[0]: ./media/blueprints/compute-n-tier-linux.png "Mehrstufige Architektur mit Microsoft Azure"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier%2Fazuredeploy.json


