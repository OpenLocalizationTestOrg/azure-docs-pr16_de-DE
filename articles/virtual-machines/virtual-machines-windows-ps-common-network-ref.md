<properties
    pageTitle="Gemeinsames Netzwerk PowerShell Befehle für virtuelle Computer | Microsoft Azure"
    description="Allgemeine PowerShell-Befehle, die Ihnen helfen gestartet, erstellen ein virtuelles Netzwerk und die zugehörigen Ressourcen für virtuelle Computer."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="common-network-azure-powershell-commands-for-vms"></a>Allgemeine Netzwerk Azure PowerShell-Befehle für virtuellen Computern

Wenn Sie einen virtuellen Computer erstellen möchten, müssen Sie ein [virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md) erstellen oder Informationen zu einem vorhandenen virtuellen Netzwerk, in dem der virtuellen Computer hinzugefügt werden kann. In der Regel beim Erstellen eines virtuellen Computers müssen Sie auch erwägen Sie das Erstellen von der in diesem Artikel beschriebenen Ressourcen.

Informationen zum Installieren der neuesten Version von Azure PowerShell, Ihr Abonnement auswählen, und melden Sie sich bei Ihrem Konto finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .

## <a name="create-network-resources"></a>Erstellen von Netzwerk-Ressourcen

Aufgabe | Befehl 
-------------- | -------------------------
Erstellen von Konfigurationen Subnetz | $subnet1 = [New-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) -Namen "Subnet_name" - AddressPrefix XX. X.X.X/XX<BR>$subnet2 = New-AzureRmVirtualNetworkSubnetConfig-Namen "Subnet_name" - AddressPrefix XX. X.X.X/XX<BR><BR>Eine typische Netzwerk möglicherweise ein Subnetz für eine [Internet gegenüberliegende Lastenausgleich](../load-balancer/load-balancer-internet-overview.md) und ein separates Subnetz für eine [interne Lastenausgleich](../load-balancer/load-balancer-internal-overview.md). |
Erstellen Sie ein virtuelles Netzwerk | $vnet = [New-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) -Resource_group_name"Name"Virtual_network_name"- ResourceGroupName"-Speicherort "Location_name" - AddressPrefix XX. X.X.X/XX-Subnetz $subnet1, $subnet2
Test für einen eindeutigen Domänennamen | [Test-AzureRmDnsAvailability](https://msdn.microsoft.com/library/mt619419.aspx) - DomainQualifiedName "Domain_name"-Speicherort "Location_name"<BR><BR>Sie können einen DNS-Domänennamen für eine [öffentliche IP-Ressource](../virtual-network/virtual-network-ip-addresses-overview-arm.md), angeben, die eine Zuordnung für domainname.location.cloudapp.azure.com zu der öffentlichen IP-Adresse in die Azure verwaltet DNS-Server erstellt. Der Name kann nur Buchstaben, Zahlen und Bindestriche enthalten. Die ersten und letzten Zeichens muss einen Buchstaben oder Zahl und dem Domänennamen, müssen in die Azure Position eindeutig sein. Wenn **Wahr** zurückgegeben wird, ist der vorgeschlagenen Namen global eindeutig.
Erstellen einer öffentlichen IP-Adresse | $pip = [New-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) -Namen "Ip_address_name" - ResourceGroupName "Resource_group_name" - DomainNameLabel "Domain_name"-Speicherort "Location_name" - AllocationMethod Dynamic<BR><BR>Die öffentliche IP-Adresse verwendet, den Namen der Domäne, den Sie zuvor geprüft und von der Front-End-Konfiguration des Lastenausgleich verwendet.
Erstellen einer Front-End-IP-Konfigurations | $frontendIP = [New-AzureRmLoadBalancerFrontendIpConfig](https://msdn.microsoft.com/library/mt603510.aspx) -Namen "Frontend_ip_name" - Öffentl.IP $pip<BR><BR>Die Front-End-Konfiguration umfasst die öffentliche IP-Adresse, die Sie zuvor für eingehende Netzwerkverkehr erstellt haben.
Erstellen einer Back-End-Adresse Ressourcenpool | $beAddressPool = [New-AzureRmLoadBalancerBackendAddressPoolConfig](https://msdn.microsoft.com/library/mt603791.aspx) -Namen "Backend_pool_name"<BR><BR>Gibt die interne Adressen für die Back-End-des Lastenausgleich, die über ein Netzwerk-Benutzeroberfläche zugegriffen werden.
Erstellen einer Abfrage | $healthProbe = [New-AzureRmLoadBalancerProbeConfig](https://msdn.microsoft.com/library/mt603847.aspx) -Namen "Probe_name" - RequestPath 'HealthProbe.aspx'-Protokoll http-Anschluss 80 - IntervalInSeconds 15 - ProbeCount 2<BR><BR>Enthält Gesundheit Prüfpunkte zum Überprüfen der Verfügbarkeit von virtuellen Computern Instanzen in die Back-End-Adresse Ressourcenpool verwendet.
Erstellen einer Regel für den Lastenausgleich | $lbRule = [New-AzureRmLoadBalancerRuleConfig](https://msdn.microsoft.com/library/mt619391.aspx) -Namen HTTP - FrontendIpConfiguration $frontendIP - BackendAddressPool $beAddressPool-$healthProbe Prüfpunkt-Protokoll Tcp - FrontendPort 80 - BackendPort 80<BR><BR>Enthält Regeln, die mit einem öffentlichen Port auf dem Lastenausgleich zu einem Port in die Back-End-Adresspool zuweisen.
Erstellen einer Regel für eingehenden NAT | $inboundNATRule = [New-AzureRmLoadBalancerInboundNatRuleConfig](https://msdn.microsoft.com/library/mt603606.aspx) -Namen "Rule_name" - FrontendIpConfiguration $frontendIP-Protokoll TCP - FrontendPort 3441 - BackendPort 3389<BR><BR>Enthält Regeln, die mit einem öffentlichen Port auf dem Lastenausgleich zu einem Port für einen bestimmten virtuellen Computer in die Back-End-Adresse Ressourcenpool zuzuordnen.
Erstellen Sie ein Lastenausgleich | $loadBalancer = "Resource_group_name" [New-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt619450.aspx) - ResourceGroupName-Namen "Load_balancer_name"-Speicherort "Location_name" - FrontendIpConfiguration $frontendIP - InboundNatRule $inboundNATRule - LoadBalancingRule $lbRule - BackendAddressPool $beAddressPool-Prüfpunkt $healthProbe
Erstellen Sie eine Netzwerkschnittstelle | $nic1 = "Resource_group_name" [New-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) - ResourceGroupName-Name der Netzwerkschnittstelle"Name"-Speicherort "Location_name" - Priv.IP-Adresse XX. X.X.X-Subnetz subnet2 - LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] LoadBalancerInboundNatRule - $loadBalancer.InboundNatRules[0]<BR><BR>Erstellen Sie eine Netzwerkschnittstelle, die mit der öffentlichen IP-Adresse und virtuelles Netzwerk Subnetz gehören, die Sie zuvor erstellt haben.
    
## <a name="get-information-about-network-resources"></a>Erhalten von Informationen zu Netzwerk-Ressourcen

Aufgabe | Befehl 
-------------- | -------------------------
Liste virtuelle Netzwerke | [Get-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603515.aspx) - ResourceGroupName "Resource_group_name"<BR><BR>Zeigt eine Liste aller virtuelle Netzwerke in der Ressourcengruppe.
Erhalten von Informationen zu einem virtuellen Netzwerk | Get-AzureRmVirtualNetwork-Resource_group_name"Name"Virtual_network_name"- ResourceGroupName"
Liste der Subnetze in einem Netzwerk virtuelle | Get-AzureRmVirtualNetwork-Name "Virtual_network_name" - ResourceGroupName "Resource_group_name" & #124; Wählen Sie Subnets
Erhalten von Informationen zu einem Subnetz | [Get-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603817.aspx) -Namen "Subnet_name" - VirtualNetwork $vnet<BR><BR>Ruft Informationen über das Subnetz in das angegebene virtuelle Netzwerk an. Der Wert $vnet stellt das Get-AzureRmVirtualNetwork zurückgegebene Objekt.
Liste IP-Adressen | [Get-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619342.aspx) - ResourceGroupName "Resource_group_name"<BR><BR>Listet die öffentlichen IP-Adressen in der Ressourcengruppe an.
Liste Lastenausgleich | [Get-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603668.aspx) - ResourceGroupName "Resource_group_name"<BR><BR>Listet die Lastenausgleich in der Ressourcengruppe an.
Netzwerk-Schnittstellen auflisten | [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) - ResourceGroupName "Resource_group_name"<BR><BR>Listet die Netzwerk-Schnittstellen in der Ressourcengruppe an.
Erhalten von Informationen zu einer Netzwerk-Benutzeroberfläche | Get-AzureRmNetworkInterface-Resource_group_name"Name"Name der Netzwerkschnittstelle"- ResourceGroupName"<BR><BR>Ruft Informationen zu einer bestimmten Schnittstelle ab.
Abrufen der IP-Konfigurations der Netzwerk-Schnittstellen | [Get-AzureRmNetworkInterfaceIPConfig](https://msdn.microsoft.com/library/mt732618.aspx) -Namen "Ipconfiguration_name" - NetworkInterface $nic<BR><BR>Ruft Informationen über die IP-Konfiguration der angegebenen Schnittstelle ab. Der Wert $nic stellt das Get-AzureRmNetworkInterface zurückgegebene Objekt.

## <a name="manage-network-resources"></a>Verwalten von Netzwerk-Ressourcen

Aufgabe | Befehl 
-------------- | -------------------------
Hinzufügen von einem Subnetz zu einem virtuellen Netzwerk | [Hinzufügen von AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt603722.aspx) - AddressPrefix XX. X.X.X/XX-Namen "Subnet_name" - VirtualNetwork $vnet<BR><BR>Fügt einem Subnetz zu einem vorhandenen virtuellen Netzwerk an. Der Wert $vnet stellt das Get-AzureRmVirtualNetwork zurückgegebene Objekt.
Löschen eines virtuellen Netzwerks | [Entfernen-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt619338.aspx) -Resource_group_name"Name"Virtual_network_name"- ResourceGroupName"<BR><BR>Entfernt das angegebene virtuelle Netzwerk aus der Ressourcengruppe.
Löschen einer Netzwerk-Benutzeroberfläche | [Entfernen-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt603836.aspx) -Resource_group_name"Name"Name der Netzwerkschnittstelle"- ResourceGroupName"<BR><BR>Entfernt die angegebene Netzwerk-Benutzeroberfläche aus der Ressourcengruppe an.
Löschen eines Lastenausgleichsmoduls | [Entfernen-AzureRmLoadBalancer](https://msdn.microsoft.com/library/mt603862.aspx) -Resource_group_name"Name"Load_balancer_name"- ResourceGroupName"<BR><BR>Entfernt den angegebene Lastenausgleich aus der Ressourcengruppe an.
Löschen einer öffentlichen IP-Adresse | [Entfernen-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt619352.aspx)-Resource_group_name"Name"Ip_address_name"- ResourceGroupName"<BR><BR>Entfernt die angegebene öffentliche IP-Adresse aus der Ressourcengruppe an.

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie die Netzwerkschnittstelle, die Sie soeben, wann erstellte Sie [ein virtuellen Computers zu erstellen](virtual-machines-windows-ps-create.md).
- Erfahren Sie, wie Sie [ein virtuellen Computers mit mehreren Netzwerkschnittstellen erstellen](../virtual-network/virtual-networks-multiple-nics.md)können.
