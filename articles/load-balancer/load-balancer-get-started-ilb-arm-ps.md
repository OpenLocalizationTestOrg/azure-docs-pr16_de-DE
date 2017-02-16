<properties
   pageTitle="Erstellen Sie eine interne Lastenausgleich mithilfe der PowerShell in Ressourcenmanager | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer internen Lastenausgleich mithilfe der PowerShell in Ressourcenmanager"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-using-powershell"></a>Erstellen Sie eine interne Lastenausgleich mithilfe der PowerShell

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]


Die folgenden Schritte erläutert, wie eine interne Lastenausgleich mit Azure Ressourcenmanager mit PowerShell zu erstellen. Mit Azure Ressourcenmanager, sind die Elemente zum Erstellen eines internen Lastenausgleichsmoduls einzeln konfiguriert und dann Bildung ein Lastenausgleich.

Sie müssen erstellen und konfigurieren Sie die folgenden Objekte zum Bereitstellen eines Lastenausgleichsmoduls:

- Front-End-IP-Konfiguration – wird die private IP-Adresse für eingehenden Netzwerkverkehr konfiguriert
- Back-End-Adresse Ressourcenpool - wird die die empfangen den Lastenausgleich Datenverkehr aus Pool front-End-IP-Netzwerk-Schnittstellen konfigurieren
- Regeln - Quell- und lokale Port-Konfiguration für den Lastenausgleich den Lastenausgleich.
- Untersucht – den Dienststatus Status Prüfpunkt für die Instanzen des virtuellen Computers konfiguriert.
- Eingehende Regeln NAT – konfiguriert die Regeln um direkt auf eine der Instanzen virtuellen Computern zuzugreifen.

Sie können weitere Informationen zu laden Lastenausgleich Komponenten mit Azure Ressourcenmanager bei [Azure Ressourcenmanager Unterstützung für Lastenausgleich](load-balancer-arm.md)herunterladen.

Die folgenden Schritte erläutert, wie ein Lastenausgleich zwischen zwei virtuellen Computern konfigurieren.


## <a name="setup-powershell-to-use-resource-manager"></a>Einrichten von PowerShell Ressourcenmanager verwenden

Stellen Sie sicher, Sie haben die neueste Version des Moduls den Azure für PowerShell, und PowerShell Setup ordnungsgemäß zu Ihrem Abonnement Azure zugreifen.

### <a name="step-1"></a>Schritt 1

        Login-AzureRmAccount

### <a name="step-2"></a>Schritt 2

Überprüfen Sie die Abonnements für das Konto

        Get-AzureRmSubscription

Sie werden zum Authentifizieren mit Ihrer Anmeldeinformationen aufgefordert.<BR>

### <a name="step-3"></a>Schritt 3

Wählen Sie aus, welche Ihrer Azure Abonnements verwenden. <BR>


        Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="create-resource-group-for-load-balancer"></a>Erstellen von Ressourcengruppe für Lastenausgleich

### <a name="step-4"></a>Schritt 4

Erstellen einer neuen Ressourcengruppe (Überspringen dieser Schritt, wenn eine vorhandene Ressourcengruppe verwenden)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

Azure Ressourcenmanager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dies wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Stellen Sie sicher, dass alle Befehle zum Erstellen eines Lastenausgleichsmoduls derselben Ressourcengruppe verwendet werden.

Im oben genannten Beispiel erstellt eine Ressourcengruppe "NRP-RG" und einen Speicherort "" Westen "USA" bezeichnet.

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Erstellen von virtuellen Netzwerk und eine private IP-Adresse für front-End-IP-pool


### <a name="step-1"></a>Schritt 1

Erstellt ein Subnetz für das virtuelle Netzwerk und Variablen $backendSubnet zugewiesen

    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24

Erstellen Sie ein virtuelles Netzwerk an:

    $vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

Das virtuelle Netzwerk erstellt und fügt das Subnetz Pfd Subnetz werden mit dem virtuellen Netzwerk NRPVNet und Variablen $vnet zugewiesen



## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Erstellen von Front-End-IP-Pool und Back-End-Adresse Ressourcenpool

Einrichten einer front-End-IP-Ressourcenpool Datenverkehr, für der eingehende laden Lastenausgleich Netzwerk Datenverkehr und Back-End-Adresspool erhalten die Last verteilt.

### <a name="step-1"></a>Schritt 1

Erstellen eines front-End-IP-Pools mit der privaten IP-Adresse 10.0.2.5 für das Subnetz 10.0.2.0/24, das Endpunkt des eingehenden Datenverkehr werden.

    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id

### <a name="step-2"></a>Schritt 2

Richten Sie einen Back-End-Adresspool verwendet, um eingehenden Datenverkehr von front-End-IP-Ressourcenpool zu erhalten:

    $beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"


## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>Erstellen von Pfd, NAT Regeln, Prüfpunkt und Lastenausgleich

Nach dem Pool front-End-IP- und die Back-End-Adresse Ressourcenpool erstellt haben, müssen Sie die Regeln erstellen, die zu laden Lastenausgleich Ressource gehören soll:

### <a name="step-1"></a>Schritt 1

    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

     $lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80


Im oben genannten Beispiel ist die folgenden Elemente erstellen:

- NAT-Regel an, die alle eingehender Datenverkehr an Port 3441 Port 3389 gehen.
- eine zweite NAT Regel der gesamte eingehender Datenverkehr an Port 3442 Port 3389 gehen.
- eine Auslastung Lastenausgleich Regel welche geladen wird Saldo alle eingehenden Datenverkehr auf Öffentliche Port 80 an lokalen Anschluss 80 in dem Pool Back-End-Adressen.
- eine Prüfpunkt Regel, die um zu den Status für den Pfad "HealthProbe.aspx überprüfen"



### <a name="step-2"></a>Schritt 2

Addieren aller Objekte (NAT, laden Lastenausgleich Regeln, Prüfpunkt Konfigurationen) Lastenausgleich zu erstellen:

    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe


## <a name="create-network-interfaces"></a>Erstellen von Netzwerk-Schnittstellen

Nach dem Erstellen des internen Lastenausgleich, müssen Sie definieren, welche Netzwerkschnittstellen eingehenden Netzwerkverkehr Lastenausgleich NAT-Regeln und Prüfpunkt empfangen soll. Die Schnittstelle in diesem Fall einzeln konfiguriert ist und höher zu einer virtuellen Computern zugewiesen werden kann.


### <a name="step-1"></a>Schritt 1


Abrufen der Ressource virtuelles Netzwerk und Subnetz Netzwerkschnittstellen erstellt werden:

    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet


Dieser Schritt erstellt Netzwerk-Schnittstelle, die zum Laden Lastenausgleich Back-End-Pool angehören und ordnen Sie die erste Regel NAT für RDP für diese Netzwerkschnittstelle:

    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

### <a name="step-2"></a>Schritt 2

Erstellen Sie eine zweite Netzwerkschnittstelle namens Pfd Nic2 werden:

Dieser Schritt erstellt eine zweite Netzwerkschnittstelle, auf den gleichen laden Lastenausgleich Back-End-Pool zuweisen und die zweite Regel NAT zuordnen für RDP erstellt:

     $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

Das Ergebnis wird Folgendes angezeigt:

    $backendnic1

Erwartetes Ergebnis:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
                           "PublicIpAddress": {
                             "Id": null
                           },
                           "LoadBalancerBackendAddressPools": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                             }
                           ],
                           "LoadBalancerInboundNatRules": [
                             {
                               "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                             }
                           ],
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>Schritt 3

Verwenden Sie den Befehl hinzufügen-AzureRmVMNetworkInterface die NIC virtuellen Computers zuweisen.

Sie können die Schritt-für-Schritt-Anweisungen zum Erstellen eines virtuellen Computers und weisen einen Netzwerkadapter folgen der Dokumentation gefunden: [Erstellen einer Azure-virtuellen Computer mithilfe der PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md).

Wenn Sie bereits eine virtuellen Computern erstellt haben, können Sie die Netzwerk-Benutzeroberfläche mit den folgenden Schritten hinzufügen:

#### <a name="step-1"></a>Schritt 1

Laden Sie laden Lastenausgleich Ressource in einer Variablen zu speichern, (Wenn Sie die noch nicht getan haben). Die verwendete Variable heißt $lb und die gleichen Namen aus der laden Lastenausgleich Ressource über erstellt.

    $lb= Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG

#### <a name="step-2"></a>Schritt 2

Laden Sie die Back-End-Konfiguration auf eine Variable ein.

    $backend= Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

#### <a name="step-3"></a>Schritt 3

Laden der bereits erstellten Schnittstelle in einer Variablen zu speichern. Der Variablenname verwendet wird, $ NIC. Die Benutzeroberfläche Netzwerkname verwendet ist gleich aus dem oben genannten Beispiel.

    $nic=Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG

#### <a name="step-4"></a>Schritt 4

Ändern Sie die Back-End-Konfiguration auf der Schnittstelle.

    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

#### <a name="step-5"></a>Schritt 5

Speichern Sie das Netzwerk Benutzeroberflächen-Objekt.

    Set-AzureRmNetworkInterface -NetworkInterface $nic

Nachdem eine Netzwerk-Benutzeroberfläche zum Laden Lastenausgleich Back-End-Pool hinzugefügt wurde, wird die empfangen Netzwerkverkehr basierend auf den Lastenausgleich Regeln für diese laden Lastenausgleich Ressource gestartet.

## <a name="update-an-existing-load-balancer"></a>Aktualisieren einer vorhandenen Lastenausgleich


### <a name="step-1"></a>Schritt 1

Verwenden den Lastenausgleich aus dem oben genannten Beispiel, laden Lastenausgleich Objekt Variablen mit Get-AzureRmLoadBalancer $slb zuweisen

    $slb=get-azureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

### <a name="step-2"></a>Schritt 2

Im folgenden Beispiel fügen Sie eine neue NAT eingehende Regel Port81 im front-End und den Port 8181 für die Back-End-Pool an einer vorhandenen Lastenausgleich

    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp


### <a name="step-3"></a>Schritt 3

Speichern Sie die neue Konfiguration festlegen-AzureLoadBalancer verwenden

    $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Entfernen eines Lastenausgleichsmoduls

Verwenden Sie den Befehl Entfernen-AzureRmLoadBalancer zum Löschen einer zuvor erstellten Lastenausgleich, die mit dem Namen "NRP Projektor" in einer Ressourcengruppe "NRP-RG" bezeichnet

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] Sie können das optionale wechseln - Force, um die Aufforderung zum Löschen zu vermeiden.



## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren eines laden Lastenausgleich Verteilung Modus](load-balancer-distribution-mode.md)

[Konfigurieren von Einstellungen zur im Leerlauf TCP Timeout für Ihre Lastenausgleich](load-balancer-tcp-idle-timeout.md)