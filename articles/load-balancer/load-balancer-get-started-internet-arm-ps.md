<properties
   pageTitle="Erstellen eines Internet zugänglichen Lastenausgleich in Ressourcenmanager mithilfe der PowerShell | Microsoft Azure"
   description="Erfahren Sie, wie ein Lastenausgleich internetfähigen in Ressourcenmanager mithilfe der PowerShell erstellen"
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

# <a name="a-nameget-startedacreating-an-internet-facing-load-balancer-in-resource-manager-by-using-powershell"></a><a name="get-started"></a>Erstellen eines Internet zugänglichen Lastenausgleich in Ressourcenmanager mithilfe der PowerShell

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Modell zur Bereitstellung von Ressourcenmanager. Sie können auch die [Informationen zum Erstellen eines Internet zugänglichen Lastenausgleich anhand des Modells klassischen Bereitstellung](load-balancer-get-started-internet-classic-cli.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-by-using-azure-powershell"></a>Bereitstellen der Lösung mit Azure PowerShell

Die folgenden Verfahren erläutert, wie ein Lastenausgleich internetfähigen mithilfe von Azure Ressourcenmanager mit PowerShell erstellen. Mit Azure Ressourcenmanager, jeder Ressource wird erstellt und konfiguriert einzeln, und setzen Sie dann zusammen, um ein Lastenausgleich zu erstellen.

Erstellen und konfigurieren Sie die folgenden Objekte, um ein Lastenausgleich bereitstellen müssen:

- Front-End-IP-Konfiguration: öffentliche (PIP)-IP-Adressen für eingehende Netzwerkdatenverkehr enthält.
- Back-End-Adresse Ressourcenpool: Netzwerk-Schnittstellen (NICs) enthält, für den virtuellen Computern Netzwerkdatenverkehr aus dem Lastenausgleich zu erhalten.
- Regeln für Lastenausgleich: enthält Regeln, die mit einem öffentlichen Port auf dem Lastenausgleich zu einem Port in die Back-End-Adresse Ressourcenpool zuordnen.
- Eingehende Regeln NAT: enthält Regeln, die einen Port für einen bestimmten virtuellen Computer im Adresspool Back-End-einen öffentlichen Anschluss auf dem Lastenausgleich zuordnen möchten.
- Prüfpunkte: enthält Gesundheit Prüfpunkte zum Überprüfen der Verfügbarkeit von virtuellen Computern Instanzen in die Back-End-Adresse Ressourcenpool verwendet.

Weitere Informationen finden Sie unter [Azure Ressourcenmanager für Lastenausgleich zu unterstützen](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Einrichten von PowerShell Ressourcenmanager verwenden

Stellen Sie sicher, dass Sie die neueste Version des Moduls Azure Ressourcenmanager für PowerShell haben:

1. Melden Sie sich bei Azure.

        Login-AzureRmAccount

    Geben Sie Ihre Anmeldeinformationen ein, wenn Sie dazu aufgefordert werden.

2. Überprüfen Sie die Abonnements für das Konto ein.

        Get-AzureRmSubscription

3. Wählen Sie aus, welche Ihrer Azure Abonnements verwenden.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Erstellen Sie eine Ressourcengruppe aus. (Überspringen Sie diesen Schritt, wenn Sie eine vorhandene Ressourcengruppe verwenden.)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Erstellen Sie ein virtuelles Netzwerk und eine öffentliche IP-Adresse für den Front-End-IP-pool

1. Erstellen Sie ein Subnetz und ein virtuelles Netzwerk.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Erstellen einer Azure öffentlichen IP-Adressen-Ressource mit dem Namen **PublicIP**von einem Front-End-IP-Ressourcenpool mit den DNS-Namen **loadbalancernrp.westus.cloudapp.azure.com**verwendet werden soll. Mit dem folgende Befehl wird der statischen Verteilung verwendet.

        $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' –AllocationMethod Static -DomainNameLabel loadbalancernrp

    >[AZURE.IMPORTANT]Lastenausgleich verwendet die Bezeichnung Domäne für die öffentliche IP-Adresse als Präfix für den vollqualifizierten Domänennamen ein. Dies unterscheidet sich von klassischen Bereitstellungsmodell und, das Cloud-Dienst als Lastenausgleich FQDN verwendet.
    >In diesem Beispiel ist der FQDN **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Erstellen einer Front-End-IP-Pool und einem Ressourcenpool Back-End-Adresse

1. Erstellen eines Front-End-IP-Pools mit dem Namen **Pfd-Front-End** , die die Ressource **PublicIp** verwendet.

        $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP

2. Erstellen einer Back-End-Adresse Ressourcenpool **Pfd-Back-End-**Namens an.

        $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>Erstellen von Regeln, eine laden Lastenausgleich Regel, einen Prüfpunkt und ein Lastenausgleich NAT

In diesem Beispiel wird die folgenden Elemente:

- Eine Regel NAT alle eingehenden Datenverkehr auf Anschluss 3441 Anschluss 3389 übersetzen
- Eine Regel NAT alle eingehenden Datenverkehr auf Anschluss 3442 Anschluss 3389 übersetzen
- Eine Regel Prüfpunkt So überprüfen Sie den Status auf einer Seite mit dem Namen **HealthProbe.aspx**
- Eine Regel Lastenausgleich laden alle eingehenden Datenverkehr an Port 80 an Port 80 auf die Adressen in die Back-End-Ressourcenpool Saldo
- Ein Lastenausgleich, die alle diese Objekte verwendet.

Gehen Sie folgendermaßen vor:

1. Erstellen Sie die NAT-Regeln.

        $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

        $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

2. Erstellen Sie einen Prüfpunkt Dienststatus. Es gibt zwei Möglichkeiten, um einen Prüfpunkt konfigurieren:

    HTTP-Prüfpunkt

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    Prüfpunkt TCP

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2

3. Erstellen einer laden Lastenausgleich Regel.

        $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80

4. Erstellen Sie mithilfe der zuvor erstellten Objekte Lastenausgleich.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe

## <a name="create-nics"></a>Erstellen von NICs

Erstellen von Netzwerk-Schnittstellen (oder vorhandene bearbeiten), und ordnen Sie diese dann NAT Regeln, laden Lastenausgleich Regeln und Prüfpunkte:

1. Erhalten Sie das virtuelle Netzwerk und einem Netzwerksubnetz virtuelle, in denen die NICs erstellt werden müssen.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Erstellen Sie einen Netzwerkadapter mit dem Namen **Pfd nic1 werden**, und ordnen sie die erste Regel NAT und dem Pool erste (und einzige) Back-End-Adressen.

        $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]

3. Erstellen Sie einen Netzwerkadapter mit dem Namen **Pfd nic2 sein**, und die zweite Regel NAT und dem Pool erste (und einzige) Back-End-Adressen zuordnen.

        $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]

4. Aktivieren Sie die NICs.

        $backendnic1

    Erwartetes Ergebnis:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
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
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. Verwenden der `Add-AzureRmVMNetworkInterface` Cmdlet die NICs anderen virtuellen Computern zuweisen.

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Anleitung zum Erstellen eines virtuellen Computers und einen Netzwerkadapter zuweisen finden Sie unter [Erstellen einer Azure-virtuellen Computer mithilfe der PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md).

## <a name="add-the-network-interface-to-the-load-balancer"></a>Hinzufügen der Schnittstelle an den Lastenausgleich

1. Abrufen von Lastenausgleich aus Azure.

    Laden Sie laden Lastenausgleich Ressource in einer Variablen zu speichern, (Wenn Sie die noch nicht getan haben). Die Variable heißt **$lb**. Verwenden Sie die gleichen Namen aus der laden Lastenausgleich Ressource, die Sie zuvor erstellt haben.

        $lb= get-azurermloadbalancer –name NRP-LB -resourcegroupname NRP-RG

2. Laden Sie die Back-End-Konfiguration auf eine Variable ein.

        $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb

3. Laden der bereits erstellten Schnittstelle in einer Variablen zu speichern. Der Variablenname ist **$nic**. Die Benutzeroberfläche Netzwerkname ist der identisch aus dem früheren Beispiel.

        $nic =get-azurermnetworkinterface –name lb-nic1-be -resourcegroupname NRP-RG

4. Ändern Sie die Back-End-Konfiguration auf der Schnittstelle.

        $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend

5. Speichern Sie das Netzwerk Benutzeroberflächen-Objekt.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Nachdem eine Netzwerk-Benutzeroberfläche zum Laden Lastenausgleich Back-End-Pool hinzugefügt wurde, wird die empfangen Netzwerkverkehr basierend auf den Lastenausgleich Regeln für diese laden Lastenausgleich Ressource gestartet.

## <a name="update-an-existing-load-balancer"></a>Aktualisieren einer vorhandenen Lastenausgleich

1. Mithilfe den Lastenausgleich aus dem früheren Beispiel ein Laden Lastenausgleich Objekt die Variable **$slb** mit zuweisen `Get-AzureLoadBalancer`.

        $slb = get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

2. Im folgenden Beispiel fügen Sie eine Regel für eingehende NAT – mithilfe von Port81 im Front-End-Pool und den Port 8181 für die Back-End-Ressourcenpool – an einer vorhandenen Lastenausgleich hinzu.

        $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP

3. Speichern Sie die neue Konfiguration mithilfe von `Set-AzureLoadBalancer`.

        $slb | Set-AzureRmLoadBalancer

## <a name="remove-a-load-balancer"></a>Entfernen eines Lastenausgleichsmoduls

Verwenden Sie den Befehl `Remove-AzureLoadBalancer` zum Löschen einer zuvor erstellten Lastenausgleich, die mit dem Namen **NRP Projektor** in einer Ressourcengruppe aufgerufen **NRP-RG**.

    Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG

>[AZURE.NOTE] Optionalen Schalter können **-Force** die Aufforderung zum Löschen zu vermeiden.

## <a name="next-steps"></a>Nächste Schritte

[Erste Schritte zum Konfigurieren einer internen Lastenausgleich](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurieren eines laden Lastenausgleich Verteilung Modus](load-balancer-distribution-mode.md)

[Konfigurieren von Einstellungen zur im Leerlauf TCP Timeout für Ihre Lastenausgleich](load-balancer-tcp-idle-timeout.md)
