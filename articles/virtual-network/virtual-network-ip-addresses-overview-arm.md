<properties
   pageTitle="Öffentliche und private IP-Adressen in Azure Ressourcenmanager | Microsoft Azure"
   description="Informationen Sie zu öffentlichen und privaten IP-Adressen in Azure Ressourcenmanager"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="ip-addresses-in-azure"></a>IP-Adressen in Azure
Sie können Azure Ressourcen zur Kommunikation mit anderen Azure Ressourcen, Ihrem lokalen Netzwerk und im Internet IP-Adressen zuweisen. Es gibt zwei Arten von IP-Adressen, die Sie in Azure verwenden können:

- **Öffentliche IP-Adressen**: für die Kommunikation mit dem Internet, einschließlich Azure öffentlich zugängliche Dienste verwendet
- **Private IP-Adressen**: für die Kommunikation in einem Azure-virtuellen Netzwerk (VNet) verwendet, und Ihrem lokalen Netzwerk, wenn Sie ein VPN-Gateway oder ExpressRoute-Verbindung zu Ihrem Netzwerk in Azure erweitern verwenden.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](virtual-network-ip-addresses-overview-classic.md).

Wenn Sie mit dem Bereitstellungsmodell klassischen vertraut sind, überprüfen Sie die [Unterschiede bei IP-Adressen zwischen Classic und Ressourcenmanager](virtual-network-ip-addresses-overview-classic.md#Differences-between-Resource-Manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Öffentliche IP-Adressen
Öffentliche IP-Adressen zulassen Azure Ressourcen zur Kommunikation mit dem Internet und Azure öffentlich zugängliche Dienste wie [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure Ereignis Hubs](https://azure.microsoft.com/services/event-hubs/), [SQL-Datenbanken](../sql-database/sql-database-technical-overview.md)und [Azure-Speicher](../storage/storage-introduction.md).

In Azure Ressourcenmanager, ist eine [öffentliche IP-](resource-groups-networking.md#public-ip-address) Adresse einer Ressource, die ihre eigenen Eigenschaften enthält. Sie können eine öffentliche IP-Adressenressource eine oder mehrere der folgenden Ressourcen zuordnen:

- Virtuellen Computern (virtueller Computer)
- Internet zugänglichen Lastenausgleich
- VPN-gateways
- Anwendungsgateways

### <a name="allocation-method"></a>Die Verteilungsmethode
Es gibt zwei Methoden, in denen eine IP-Adresse einer *öffentlichen* IP-Ressource - *dynamische* oder *statische*zugewiesen ist. Die Methode der Standard-Verteilung ist *dynamische*, wobei eine IP-Adresse **nicht** zum Zeitpunkt der seiner Erstellung zugeordnet ist. Die öffentliche IP-Adresse ist stattdessen zugeordnet wird, wenn Sie die zugeordnete Ressource (wie eine Lastenausgleich virtuellen Computers oder laden) starten (oder erstellen). Wenn Sie die Ressource beenden (oder löschen), wird die IP-Adresse freigegeben. Dadurch wird die IP-Adresse zu ändern, wenn Sie beenden, und starten Sie eine Ressource aus.

Um sicherzustellen, dass die IP-Adresse für die zugeordnete Ressource unverändert bleibt, können Sie die Methode der Verteilung explizit *statische*festlegen. In diesem Fall wird sofort eine IP-Adresse zugewiesen. Es wird nur beim Löschen der Ressource oder ändern die Methode der Verteilung in *dynamischen*freigegeben.

>[AZURE.NOTE] Selbst wenn Sie die Methode der Verteilung auf *statischen*festgelegt haben, können nicht Sie die tatsächliche IP-Adresse der *öffentlichen IP-Ressource*zugeordneten angeben. Stattdessen ruft aus einem Pool verfügbarer IP-Adressen in den Azure Speicherort zugewiesen, die in die Ressource erstellt wird.

Statische öffentliche IP-Adressen werden in den folgenden Szenarien häufig verwendet:

- Endbenutzer müssen zur Kommunikation mit Ressourcen Azure-Firewall-Regeln zu aktualisieren.
- DNS-namensauflösung, in denen eine Änderung der IP-Adresse benötigen würde A-Einträge aktualisieren.
- Azure Ressourcen kommunizieren mit anderen apps oder Dienste, die eine IP-Adresse basierende Sicherheitsmodell verwenden.
- Sie verwenden eine IP-Adresse verknüpfte SSL-Zertifikate.

>[AZURE.NOTE] Die Liste der IP-Adressbereiche aus denen öffentliche IP-Adressen (Dynamic/statisch) Azure Ressourcen zugeordnet sind, wird bei [Azure Datacenter IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653)veröffentlicht.

### <a name="dns-hostname-resolution"></a>Mit einer Auflösung von DNS-hostname
Sie können eine Bezeichnung DNS-Domäne ein, für eine öffentliche IP-Ressource angeben, die eine Zuordnung für *Domainnamelabel*erstellt. *Speicherort*. cloudapp.azure.com der öffentlichen IP-Adresse in die Azure verwaltet DNS-Server. Beispielsweise wenn Sie eine öffentliche IP-Ressource mit **"Contoso"** als eine *Domainnamelabel* in den **USA Westen** erstellen Azure *Speicherort*, den vollständigen Domänennamen (FULLY Qualified Name) **contoso.westus.cloudapp.azure.com** zu der öffentlichen IP-Adresse der Ressource aufgelöst. Sie können diese vollqualifizierten Domänennamen verwenden, zum Erstellen eines benutzerdefinierten Domäne CNAME-Eintrags zu der öffentlichen IP-Adresse in Azure zeigt.

>[AZURE.IMPORTANT] Bezeichnung für jede Domäne erstellt muss in die Azure Position eindeutig sein.  

### <a name="virtual-machines"></a>Virtuellen Computern
Sie können eine öffentliche IP-Adresse mit einem [Windows](../virtual-machines/virtual-machines-windows-about.md) oder [Linux](../virtual-machines/virtual-machines-linux-about.md) virtueller Computer zuordnen, indem Sie seine **Schnittstelle**zuweisen. Im Falle einer mehrere Netzwerk-Schnittstelle virtueller Computer können Sie es an die *primäre* Netzwerk-Schnittstelle zuweisen. Sie können entweder eine dynamische oder eine statische öffentliche IP-Adresse eines virtuellen Computers zuweisen.

### <a name="internet-facing-load-balancers"></a>Internet zugänglichen Lastenausgleich
Sie können eine [Azure Lastenausgleich](../load-balancer/load-balancer-overview.md), indem Sie die **Front-End** -Konfiguration von laden Lastenausgleich zuweisen eine öffentliche IP-Adresse zuordnen. Diese öffentliche IP-Adresse dient als ein Lastenausgleich virtuelle IP-Adresse. Sie können entweder eine dynamische oder eine statische öffentliche IP-Adresse ein Front-End-Lastenausgleich zuweisen. Sie können ein Lastenausgleich Front-End-, die womit [Multi-VIP](../load-balancer/load-balancer-multivip.md) Szenarien wie einer Umgebung mit mehreren Mandanten mit SSL-basierte Websites können, auch mehrere öffentliche IP-Adressen zuweisen.

### <a name="vpn-gateways"></a>VPN-gateways
[Azure VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) wird verwendet, um ein Azure-virtuellen Netzwerk (VNet) mit anderen Azure VNets oder mit einem lokalen Netzwerk verbunden sein. Sie müssen zuweisen eine öffentliche IP-Adresse auf die **IP-Konfiguration** , um ihn zur Kommunikation mit remote-Netzwerkzugriff zu aktivieren. Derzeit können Sie nur eine *dynamische* öffentliche IP-Adresse zu einem VPN-Gateway zuweisen.

### <a name="application-gateways"></a>Anwendungsgateways
Sie können eine öffentliche IP-Adresse mit einem Azure- [Anwendungsgateway](../application-gateway/application-gateway-introduction.md)zuordnen, indem Sie den Gateway **Front-End** -Konfiguration zuweisen. Diese öffentliche IP-Adresse dient als eine VIP Lastenausgleich. Derzeit können Sie nur eine *dynamische* öffentliche IP-Adresse zu einer Gateway Front-End-Anwendungskonfiguration zuweisen.

### <a name="at-a-glance"></a>Auf einen Blick
Die nachfolgenden Tabelle wird aufgezeigt, die bestimmte Eigenschaft bis zu der eine öffentliche IP-Adresse zugeordnet werden, kann auf oberster Ebene einer Ressourcenansicht hinzu, und die möglichen Zuteilung Methoden (dynamisch oder statisch), die verwendet werden können.

|Ressourcen auf oberster Ebene|IP-Adresse association|Dynamische|Statische|
|---|---|---|---|
|Virtuellen Computern|Netzwerk-Benutzeroberfläche|Ja|Ja|
|Lastenausgleich|Front-End-Konfiguration|Ja|Ja|
|VPN-gateway|Gateway-IP-Konfiguration|Ja|Nein|
|Anwendungsgateway|Front-End-Konfiguration|Ja|Nein|

## <a name="private-ip-addresses"></a>Private IP-Adressen
Private IP-Adressen zulassen Azure Ressourcen zur Kommunikation mit anderen Ressourcen in einem [virtuellen Netzwerk](virtual-networks-overview.md) oder einem lokalen Netzwerk über ein VPN-Gateway oder ExpressRoute-Verbindung, ohne eine Internet erreichbar IP-Adresse.

Im Bereitstellungsmodell Azure Ressourcenmanager gibt es eine private IP-Adresse für die folgenden Typen von Azure Ressourcen:

- Virtuellen Computern
- Interner Lastenausgleich (ILBs)
- Anwendungsgateways

### <a name="allocation-method"></a>Die Verteilungsmethode
Eine private IP-Adresse wird aus der Adresse Zellbereich im Subnetz zugewiesen, der die Ressource zugeordnet ist. Bereich von dem Subnetz selbst ist Bestandteil der VNets Adressbereichs.

Es gibt zwei Methoden, in denen eine private IP-Adresse zugeordnet ist: *dynamische* oder *statische*. Methode der Standard-Verteilung ist *dynamische*, wo die IP-Adresse automatisch aus der Ressource Subnetz (mit DHCP) zugeordnet. Wenn Sie beenden, und starten die Ressource, kann diese IP-Adresse ändern.

Sie können die Methode der Verteilung auf *statischen* um sicherzustellen, dass die IP-Adresse unverändert bleibt festlegen. In diesem Fall müssen Sie auch eine gültige IP-Adresse angeben, die der Ressource Subnetz gehört.

Statische private IP-Adressen werden häufig für verwendet:

- Virtuellen Computern, die als Domänencontroller oder DNS-Server fungieren.
- Ressourcen, die Firewall-Regeln mit IP-Adressen erfordern.
- Ressourcen, die von anderen apps/Ressourcen über eine IP-Adresse zugegriffen werden.

### <a name="virtual-machines"></a>Virtuellen Computern
Eine private IP-Adresse wird die **Schnittstelle** einer [Windows](../virtual-machines/virtual-machines-windows-about.md) oder [Linux](../virtual-machines/virtual-machines-linux-about.md) virtueller Computer zugewiesen. Bei einer mehrere Netzwerk-Schnittstelle virtueller Computer wird jede Schnittstelle eine private IP-Adresse zugewiesen. Sie können die Methode der Verteilung als dynamisch oder statisch für einen Netzwerkadapter angeben.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Internen DNS-Hostname Auflösung (für virtuelle Computer)
Alle Azure-virtuellen Computern sind standardmäßig mit [Azure verwaltet DNS-Server](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) konfiguriert, es sei denn, Sie explizit benutzerdefinierten DNS-Server konfigurieren. Diese DNS-Server stellen internen Namen mit einer Auflösung von virtuellen Computern, die sich innerhalb der gleichen VNet befinden.

Beim Erstellen eines virtuellen Computers ist eine Zuordnung für die Hostname seiner privaten IP-Adresse die Azure verwaltet DNS-Server hinzugefügt. Bei einer mehrere Netzwerk-Schnittstelle virtueller Computer wird die Hostname der privaten IP-Adresse der primären Schnittstelle zugeordnet.

Virtuellen Computern mit Azure verwaltet DNS-Server konfiguriert werden die Hostnamen von allen virtuellen Computern innerhalb ihrer VNet ihren privaten IP-Adressen auflösen.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Interner Lastenausgleich (ILB) und Anwendungsgateways
Sie können eine private IP-Adresse der **front-End** -Konfiguration von einer [Internen Lastenausgleich Azure](../load-balancer/load-balancer-internal-overview.md) (ILB) oder ein [Azure Application Gateway](../application-gateway/application-gateway-introduction.md)zuweisen. Diese private IP-Adresse fungiert als internen Endpunkt, Zugriff nur für die Ressourcen in einem virtuellen Netzwerk (VNet) und der remote Netzwerke mit den VNet verbunden sind. Sie können entweder eine dynamische oder statische private IP-Adresse der front-End-Konfiguration zuweisen.

### <a name="at-a-glance"></a>Auf einen Blick
Die nachfolgenden Tabelle wird aufgezeigt, die bestimmte Eigenschaft bis zu der eine private IP-Adresse zugeordnet werden, kann auf oberster Ebene einer Ressourcenansicht hinzu, und die möglichen Zuteilung Methoden (dynamisch oder statisch), die verwendet werden können.

|Ressourcen auf oberster Ebene|IP-Adresse association|Dynamische|Statische|
|---|---|---|---|
|Virtuellen Computern|Netzwerk-Benutzeroberfläche|Ja|Ja|
|Lastenausgleich|Front-End-Konfiguration|Ja|Ja|
|Anwendungsgateway|Front-End-Konfiguration|Ja|Ja|

## <a name="limits"></a>Grenzwerte

Die IP-Adressen auferlegt Grenzwerte sind in sämtlicher [dateigrößenbeschränkungen für Netzwerke](azure-subscription-service-limits.md#networking-limits) in Azure angegeben. Diese Grenzwerte sind je nach Region und pro Abonnement. Sie können Sie zum Erhöhen der standardmäßigen Grenzwerte bis zu den Höchstgrenzen bei entsprechend Ihren geschäftlichen Anforderungen [an den Support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

## <a name="pricing"></a>Preise

Öffentliche IP-Adressen möglicherweise eine nominal aufzulisten. Überprüfen Sie die [IP-Adresse Preise](https://azure.microsoft.com/pricing/details/ip-addresses) Seite, um weitere Informationen zur IP-Adresse in Azure Preise.

## <a name="next-steps"></a>Nächste Schritte
- [Bereitstellen eines virtuellen Computers mit einer statischen öffentliche IP-Adresse, mit dem Azure-portal](virtual-network-deploy-static-pip-arm-portal.md)
- [Bereitstellen eines virtuellen Computers mit einer statischen öffentliche IP-Adresse mithilfe einer Vorlage](virtual-network-deploy-static-pip-arm-template.md)
- [Bereitstellen eines virtuellen Computers mit einer statischen privaten IP-Adresse über das Azure-portal](virtual-networks-static-private-ip-arm-pportal.md)
