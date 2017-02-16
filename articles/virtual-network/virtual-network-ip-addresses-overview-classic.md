<properties
   pageTitle="Öffentliche und private adressieren IP (klassisch) in Azure | Microsoft Azure"
   description="Informationen Sie zu öffentlichen und privaten IP-Adressen in Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/11/2016"
   ms.author="jdial" />

# <a name="ip-addresses-classic-in-azure"></a>IP-Adressen (klassisch) in Azure
Sie können Azure Ressourcen zur Kommunikation mit anderen Azure Ressourcen, Ihrem lokalen Netzwerk und im Internet IP-Adressen zuweisen. Es gibt zwei Arten von IP-Adressen in Azure können: öffentlichen und privaten.

Öffentliche IP-Adressen werden für die Kommunikation mit dem Internet, einschließlich Azure öffentlich zugängliche Dienste verwendet.

Private IP-Adressen werden für die Kommunikation innerhalb einer Azure virtuelles Netzwerk (VNet), einen Clouddienst und Ihrem lokalen Netzwerk verwendet, wenn Sie ein VPN-Gateway oder ExpressRoute-Verbindung zu Ihrem Netzwerk in Azure erweitern verwenden.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [Führen Sie diese Schritte, die mit dem Modell zur Bereitstellung von Ressourcenmanager](virtual-network-ip-addresses-overview-arm.md).

## <a name="public-ip-addresses"></a>Öffentliche IP-Adressen
Öffentliche IP-Adressen zulassen Azure Ressourcen zur Kommunikation mit dem Internet und Azure öffentlich zugängliche Dienste wie [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure Ereignis Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL-Datenbanken](../sql-database/sql-database-technical-overview.md)und [Azure-Speicher](../storage/storage-introduction.md).

Eine öffentliche IP-Adresse, die die folgenden Ressourcentypen zugeordnet ist:

- Cloud-Dienste
- IaaS virtuellen Computern (virtuellen Computern)
- PaaS Rolleninstanzen
- VPN-gateways
- Anwendungsgateways

### <a name="allocation-method"></a>Die Verteilungsmethode
Wenn Sie eine öffentliche IP-Adresse einer Ressource Azure zugewiesen werden muss, ist es *dynamisch* aus einem Ressourcenpool verfügbare öffentliche IP-Adresse innerhalb des Speicherorts Erstellung die Ressource zugeordnet. Wenn die Ressource beendet ist, wird diese IP-Adresse freigegeben. Falls der Cloud-Dienst, dies geschieht, wenn alle Rolleninstanzen angehalten werden, können die mithilfe einer *statischen* (reservierte) IP-Adresse vermieden werden (siehe [Cloud Services](#Cloud-services)).

>[AZURE.NOTE] Die Liste der IP-Adressbereiche aus der öffentliche IP-Adressen Azure Ressourcen zugeordnet sind, wird bei [Azure Datacenter IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653)veröffentlicht.

### <a name="dns-hostname-resolution"></a>Mit einer Auflösung von DNS-hostname
Wenn Sie einen Clouddienst oder einer VM IaaS erstellen, müssen Sie einen Cloud-Dienst DNS-Namen angeben, der alle Ressourcen Azure eindeutig ist. Dies erstellt eine Zuordnung in Azure verwaltet DNS-Server für die *DNS-Name*. cloudapp.net der öffentlichen IP-Adresse der Ressource. Wenn Sie einen Cloud-Dienst DNS-Namen von **Contoso**Cloud-Dienst erstellen, werden die vollständigen Domänennamen (FULLY Qualified Name) **contoso.cloudapp.net** beispielsweise in einer öffentlichen IP-Adresse des Cloud-Dienst aufgelöst. Sie können diese vollqualifizierten Domänennamen verwenden, zum Erstellen eines benutzerdefinierten Domäne CNAME-Eintrags zu der öffentlichen IP-Adresse in Azure zeigt.

### <a name="cloud-services"></a>Cloud-Dienste
Cloud-Dienst enthält immer eine öffentliche IP-Adresse als eine virtuelle IP-Adresse bezeichnet. Sie können Endpunkte erstellen, in einen Cloud-Dienst, in die VIP zu internen Ports auf virtuellen Computern und Rolleninstanzen in der Cloud-Dienst verschiedene Ports zugeordnet werden soll. 

Cloud-Dienst kann mehrere IaaS virtuellen Computern oder PaaS Rolleninstanzen, bis im selben Cloud-Dienst VIP alle zugänglicher enthalten. Sie können auch [mehrere VIPs auf einen Clouddienst](../load-balancer/load-balancer-multivip.md), zuweisen, womit Multi-VIP Szenarien wie mehrere Mandanten Umgebung mit SSL-basierte Websites können.

Sie können sicherstellen, dass die öffentliche IP-Adresse des Cloud-Dienst unverändert bleibt, auch wenn alle Rolleninstanzen angehalten sind, mithilfe einer *statische* öffentlichen IP-Adresse aus, als [Reservierte IP-Adresse](virtual-networks-reserved-public-ip.md)bezeichnet, zu verwenden. Sie können erstellen eine statische (reservierte) IP-Ressource in einer bestimmten Stelle und Zuweisen der Lizenz zu einem beliebigen Cloud-Dienst an diesem Speicherort. Sie können keine die tatsächliche IP-Adresse für die reservierte IP-Adresse angeben, er zugeordnet ist, aus dem Pool verfügbarer IP-Adressen in den Speicherort aus, die, den Sie erstellt wurde. Diese IP-Adresse wird nicht freigegeben, bis Sie explizit löschen.

Statische (reservierte) öffentliche IP-Adressen werden in den Szenarien, in einen Cloud-Service häufig verwendet:

- erfordert Firewall-Regeln von Endbenutzern eingerichtet werden.
- Abhängig von externen DNS-namensauflösung und eine dynamische IP-Adresse würde erfordern eine Einträge aktualisieren.
- Es verbraucht externe Webdienste die IP-basierte Sicherheitsmodell verwenden.
- verwendet SSL-Zertifikate einer IP-Adresse verknüpft.

>[AZURE.NOTE] Beim Erstellen eines klassischen virtuellen Computers ist ein Container *Cloud-Dienst* von Azure, erstellt die hat eine virtuelle IP-Adresse. Wenn die Erstellung über Portal fertig ist, ist einen Standardwert RDP oder SSH *Endpunkt* vom Portal konfiguriert, damit Sie auf den virtuellen Computer durch die Cloud-Dienst VIP zugreifen können. Diese Cloud-Dienst VIP kann die bietet effektiv einer reservierten IP-Adresse für die Verbindung zu den virtuellen Computer reserviert werden. Sie können zusätzliche Ports öffnen, indem Sie weitere Endpunkte konfigurieren.

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS virtuellen Computern und PaaS Rolleninstanzen
Sie können eine öffentliche IP-Adresse direkt an eine IaaS [virtueller Computer](../virtual-machines/virtual-machines-linux-about.md) oder PaaS Rolleninstanz in einen Clouddienst zuweisen. Dies wird als eine Instanz Ebene öffentlichen IP-Adresse ([ILPIP](virtual-networks-instance-level-public-ip.md)) bezeichnet. Diese öffentliche IP-Adresse kann nur dynamische sein.

>[AZURE.NOTE] Dies ist die VIP des Cloud-Dienst, ein Container für IaaS virtuellen Computern oder PaaS Rolleninstanzen ist, da Cloud-Dienst mehrere IaaS virtuellen Computern enthalten kann oder alle wird über die gleichen Cloud-Dienst VIP Rolleninstanzen PaaS abweicht.

### <a name="vpn-gateways"></a>VPN-gateways
Ein [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) kann verwendet werden, eine Azure VNet zu anderen Azure VNets oder lokalen Netzwerken verbinden. Ein VPN-Gateway wird eine öffentliche IP-Adresse *dynamisch*, der Kommunikation mit der remote-Netzwerkzugriff ermöglicht zugewiesen.

### <a name="application-gateways"></a>Anwendungsgateways
Ein Azure [Application Gateway](../application-gateway/application-gateway-introduction.md) kann für Layer7 den Lastenausgleich um Netzwerkdatenverkehr weiterzuleiten, basierend auf HTTP verwendet werden. Anwendungsgateway wird eine öffentliche IP-Adresse *dynamisch*, der als den Lastenausgleich VIP fungiert zugewiesen.

### <a name="at-a-glance"></a>Auf einen Blick
In der nachfolgenden Tabelle wird jede Ressourcenart mit möglichen Zuteilung Methoden (Dynamic/statisch) und Möglichkeit, mehrere öffentliche IP-Adressen zuweisen.

|Ressource|Dynamische|Statische|Mehrere IP-Adressen|
|---|---|---|---|
|Cloud-Dienst|Ja|Ja|Ja|
|IaaS VM oder PaaS Instanz der Rolle|Ja|Nein|Nein|
|VPN-gateway|Ja|Nein|Nein|
|Anwendungsgateway|Ja|Nein|Nein|

## <a name="private-ip-addresses"></a>Private IP-Adressen
Private IP-Adressen zulassen Azure Ressourcen zur Kommunikation mit anderen Ressourcen in einen Clouddienst oder [virtuelles Netzwerk](virtual-networks-overview.md)(VNet) oder lokalen Netzwerk (über ein VPN-Gateway oder ExpressRoute Verbindung), ohne eine Internet erreichbar IP-Adresse ein.

Klassische Azure-Bereitstellung-Modell kann eine private IP-Adresse den folgenden Azure Ressourcen zugewiesen werden:

- IaaS virtuellen Computern und PaaS Rolleninstanzen
- Interner Lastenausgleich
- Anwendungsgateway

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS virtuellen Computern und PaaS Rolleninstanzen
Virtuellen Computern (virtuelle Computer) mit dem Bereitstellungsmodell klassischen erstellt werden immer in einen Cloud-Dienst, ähnlich wie die Rolleninstanzen PaaS platziert. Das Verhalten der privaten IP-Adressen sind daher für diese Ressourcen ähnlich.

Es ist wichtig, beachten Sie, dass auf zwei Arten Cloud-Dienst bereitgestellt werden kann:

- Als *eigenständigen* Cloud-Dienst, wo es nicht in einem virtuellen Netzwerk ist.
- Als Teil eines virtuellen Netzwerks.

#### <a name="allocation-method"></a>Die Verteilungsmethode
Bei einem *eigenständigen* Cloud-Dienst erhalten Ressourcen eine private IP-Adresse reserviert *dynamisch* über die Azure Datacenter private IP-Adressenbereich. Es kann nur für die Kommunikation mit anderen virtuellen Computern innerhalb derselben Cloud-Dienst verwendet werden. Wenn die Ressource beendet und gestartet wird, kann diese IP-Adresse ändern.

Bei einer Cloud-Dienst bereitgestellt, die in einem virtuellen Netzwerk erhalten Sie Ressourcen private IP-Adressen von Bereich von den zugeordneten Subnetze (gemäß der Netzwerkkonfiguration) zugewiesen sind. Diese privaten IP-Adresse(n) kann für die Kommunikation zwischen allen virtuellen Computern innerhalb der VNet verwendet werden.

Darüber hinaus wird bei Cloud Services innerhalb einer VNet, eine private IP-Adresse *dynamisch* (mit DHCP) standardmäßig zugewiesen. Sie können ändern, wenn die Ressource beendet und gestartet ist. Um sicherzustellen, dass die IP-Adresse unverändert bleibt, müssen Sie die Methode der Verteilung auf *statischen*festgelegt, und geben Sie eine gültige IP-Adresse innerhalb des entsprechenden Adressbereichs.

Statische private IP-Adressen werden häufig für verwendet:

 - Virtuellen Computern, die als Domänencontroller oder DNS-Server fungieren.
 - Virtuellen Computern, die Firewall-Regeln mit IP-Adressen erfordern.
 - Virtuellen Computern mit Dienste zugegriffen werden, indem Sie andere apps über eine IP-Adresse.

#### <a name="internal-dns-hostname-resolution"></a>Mit einer Auflösung von internen DNS-hostname
Alle Azure-virtuellen Computern und PaaS Rolleninstanzen sind mit [Azure verwaltet DNS-Server](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) standardmäßig so konfiguriert, wenn Sie explizit benutzerdefinierten DNS-Server konfigurieren. Diese DNS-Server stellen internen Namen mit einer Auflösung von virtuellen Computern und Rolleninstanzen, die innerhalb der gleichen VNet oder Cloud-Dienst befinden.

Beim Erstellen eines virtuellen Computers ist eine Zuordnung für die Hostname seiner privaten IP-Adresse die Azure verwaltet DNS-Server hinzugefügt. Im Fall eines Multi-NIC virtuellen Computers wird die Hostname der privaten IP-Adresse des primären Netzwerkschnittstellenadapters zugeordnet. Diese Zuordnungsinformationen ist jedoch auf Ressourcen innerhalb der gleichen Cloud-Dienst oder VNet beschränkt.

Bei einem *eigenständigen* Cloud-Dienst werden Sie alle Instanzen von virtuellen Computern/Rolle innerhalb der gleichen Cloud-Dienst Hostnamen auflösen. Bei einem Cloud-Dienst innerhalb einer VNet werden Sie Hostnamen aller virtuellen Computern/Rolle Instanzen innerhalb der VNet auflösen.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interner Lastenausgleich (ILB) und Anwendungsgateways
Sie können eine private IP-Adresse der **front-End** -Konfiguration von einer [Internen Lastenausgleich Azure](../load-balancer/load-balancer-internal-overview.md) (ILB) oder ein [Azure Application Gateway](../application-gateway/application-gateway-introduction.md)zuweisen. Diese private IP-Adresse fungiert als internen Endpunkt, Zugriff nur für die Ressourcen in einem virtuellen Netzwerk (VNet) und der remote Netzwerke mit den VNet verbunden sind. Sie können entweder eine dynamische oder statische private IP-Adresse der front-End-Konfiguration zuweisen. Sie können auch mehrere private IP-Adressen Multi-vip Szenarien unterstützen zuweisen.

### <a name="at-a-glance"></a>Auf einen Blick
In der nachfolgenden Tabelle wird jede Ressourcenart mit möglichen Zuteilung Methoden (Dynamic/statisch) und Möglichkeit, mehrere private IP-Adressen zuweisen.

|Ressource|Dynamische|Statische|Mehrere IP-Adressen|
|---|---|---|---|
|Virtueller Computer (in einem *eigenständigen* Cloud-Dienst)|Ja|Ja|Ja|
|PaaS Rolleninstanz (in einem *eigenständigen* Cloud-Dienst)|Ja|Nein|Ja|
|Virtuellen Computers oder PaaS Rolleninstanz (in einer VNet)|Ja|Ja|Ja|
|Interner laden Lastenausgleich front-end|Ja|Ja|Ja|
|Anwendung Gateway front-end|Ja|Ja|Ja|

## <a name="limits"></a>Grenzwerte

In der nachfolgenden Tabelle zeigt die IP-vorgegebenen Beschränkung in Azure pro Abonnement adressieren. Sie können Sie zum Erhöhen der standardmäßigen Grenzwerte bis zu den Höchstgrenzen bei entsprechend Ihren geschäftlichen Anforderungen [an den Support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

||Standardmäßige Grenzwert|Obergrenze|
|---|---|---|
|Öffentliche IP-Adressen (dynamisch)|5|Wenden Sie sich an|
|Reservierte öffentliche IP-Adressen|20|Wenden Sie sich an|
|Öffentliche VIP pro Bereitstellung (Cloud-Dienst)|5|Wenden Sie sich an|
|Private VIP (ILB) pro Bereitstellung (Cloud-Dienst)|1|1|

Stellen Sie sicher, dass Sie sämtlicher [dateigrößenbeschränkungen für Netzwerke](azure-subscription-service-limits.md#networking-limits) in Azure lesen.

## <a name="pricing"></a>Preise

In den meisten Fällen sind öffentliche IP-Adressen kostenlos. Wird eine nominal belastet zusätzliche und/oder statische öffentliche IP-Adressen verwenden. Stellen Sie sicher, dass Sie die [Struktur für öffentliche IP-Adressen Preise](https://azure.microsoft.com/pricing/details/ip-addresses/)verstehen.

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Unterschiede zwischen Ressourcenmanager und klassischen Bereitstellungen
Es folgt eine Featurevergleich zwischen IP-Adressen Ressourcenmanager und das Bereitstellungsmodell klassischen.

||Ressource|Klassische|Ressourcenmanager|
|---|---|---|---|
|**Öffentliche IP-Adresse**|VIRTUELLER COMPUTER|Als eine ILPIP (nur dynamisch) bezeichnet|Als eine öffentliche IP-Adresse (dynamische oder statische) bezeichnet|
|||Eine VM IaaS oder eine Instanz der Rolle PaaS zugewiesen sind|Des virtuellen Computers NIC zugeordnet ist|
||Internet zugänglichen Lastenausgleich|VIP (dynamic) oder reservierte IP-(statisch) genannt|Als eine öffentliche IP-Adresse (dynamische oder statische) bezeichnet|
|||Zugewiesen an einen Clouddienst|Um den Lastenausgleich front-End-Config verknüpft ist|
||||
|**Private IP-Adresse**|VIRTUELLER COMPUTER|Als ein DIP bezeichnet|Als eine private IP-Adresse bezeichnet|
|||Eine VM IaaS oder eine Instanz der Rolle PaaS zugewiesen sind|Des virtuellen Computers NIC zugewiesen ist|
||Interner Lastenausgleich (ILB)|Die ILB (dynamische oder statische) zugewiesen|Zugewiesen an den ILBs front-End-Konfiguration (dynamische oder statische)|

## <a name="next-steps"></a>Nächste Schritte
- [Bereitstellen eines virtuellen Computers mit einer statischen privaten IP-Adresse](virtual-networks-static-private-ip-classic-pportal.md) über das klassische-Portal.
