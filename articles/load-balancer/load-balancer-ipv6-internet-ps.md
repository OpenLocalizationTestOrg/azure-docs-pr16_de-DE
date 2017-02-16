<properties
    pageTitle="Erstellen eines Internet gegenüberliegende Lastenausgleich mit IPv6 mithilfe der PowerShell für Ressourcenmanager | Microsoft Azure"
    description="Informationen Sie zum Erstellen eines Internet gegenüberliegende Lastenausgleich mit IPv6 mithilfe der PowerShell für Ressourcenmanager"
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    tags="azure-resource-manager"
    keywords="IPv6, Azure Lastenausgleich, zwei Stapel, öffentliche IP-Adresse, native ipv6, Mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Erste Schritte beim Erstellen eines Internet gegenüberliegende Lastenausgleich mit IPv6 mithilfe der PowerShell für Ressourcenmanager

> [AZURE.SELECTOR]
- [PowerShell](./load-balancer-ipv6-internet-ps.md)
- [Azure CLI](./load-balancer-ipv6-internet-cli.md)
- [Vorlage](./load-balancer-ipv6-internet-template.md)

Ein Azure Lastenausgleich ist ein Lastenausgleich Layer 4 (TCP, UDP). Lastenausgleich bietet hohe Verfügbarkeit durch eingehenden Datenverkehr zwischen Dienstinstanzen fehlerfrei, in der Cloud Services oder virtuellen Computern in einer Gruppe von laden Lastenausgleich verteilen. Azure Lastenausgleich können auch Dienste auf mehrere Ports, mehrere IP-Adressen oder beides präsentieren.

## <a name="example-deployment-scenario"></a>Beispiel für Bereitstellungsszenario

Das folgende Diagramm veranschaulicht den Lastenausgleich Lösung, die in diesem Artikel bereitgestellt werden.

![Laden Lastenausgleich Szenario](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

In diesem Szenario erstellen Sie die folgenden Azure Ressourcen:

- eine Internet zugänglichen Lastenausgleich mit einer IPv4 und IPv6 öffentlichen IP-Adresse
- zwei laden Lastenausgleich Regeln zum Zuordnen der öffentlichen VIPs an die Endpunkte als "Privat"
- eine Verfügbarkeit festlegen, enthält die zwei virtuellen Computern
- zwei virtuellen Computern (virtuellen Computern)
- eine virtuelle Schnittstelle für jeden virtuellen Computer mit zugewiesenen IPv4 und IPv6-Adressen

## <a name="deploying-the-solution-using-the-azure-powershell"></a>Bereitstellen der Lösung mithilfe der PowerShell Azure

Die folgenden Schritte anzeigen zum Erstellen einer gegenüberliegende Lastenausgleich mit Azure Ressourcenmanager mit PowerShell Internet Mit Azure Ressourcenmanager, jeder Ressource wird erstellt und konfiguriert einzeln dann sich zusammen, um eine Ressource zu erstellen.

Um ein Lastenausgleich bereitzustellen, erstellen und konfigurieren Sie die folgenden Objekte:

- Front-End-IP-Konfiguration - enthält öffentliche IP-Adressen für eingehende Netzwerkdatenverkehr.
- Back-End-Adresse Ressourcenpool - enthält Netzwerk-Schnittstellen (NICs) für den virtuellen Computern Netzwerkdatenverkehr aus dem Lastenausgleich zu erhalten.
- Regel Lastenausgleich - enthält Regeln, die mit einem öffentlichen Port auf dem Lastenausgleich zu Port Pool Back-End-Adressen zuzuordnen.
- Eingehende Regeln NAT - Regeln, die Verknüpfung mit einem öffentlichen Ports auf dem Lastenausgleich an einen Anschluss für einen bestimmten virtuellen Computer in dem Pool Back-End-Adressen enthält.
- Untersucht - Dienststatus Prüfpunkte verwendet, um die Verfügbarkeit der Instanzen von virtuellen Computern in dem Pool Back-End-Adressen enthält.

Weitere Informationen finden Sie unter [Azure Ressourcenmanager für Lastenausgleich zu unterstützen](load-balancer-arm.md).

## <a name="set-up-powershell-to-use-resource-manager"></a>Einrichten von PowerShell Ressourcenmanager verwenden

Stellen Sie sicher, dass Sie die neueste Version des Moduls Azure Ressourcenmanager für PowerShell verfügen.

1. Melden Sie sich bei Azure

        Login-AzureRmAccount

    Geben Sie Ihre Anmeldeinformationen ein, wenn Sie dazu aufgefordert werden.

2. Überprüfen Sie die Abonnements für das Konto

        Get-AzureRmSubscription

3. Wählen Sie aus, welche Ihrer Azure Abonnements verwenden.

        Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'

4. Erstellen einer Ressourcengruppe (Überspringen dieser Schritt, wenn eine vorhandene Ressourcengruppe verwenden)

        New-AzureRmResourceGroup -Name NRP-RG -location "West US"

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a>Erstellen Sie ein virtuelles Netzwerk und eine öffentliche IP-Adresse für den Front-End-IP-pool

1. Erstellen Sie ein virtuelles Netzwerk mit einem Subnetz.

        $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
        $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet

2. Azure öffentliche IP-Adresse (PIP) Ressourcen für die Front-End-IP-Adresse Ressourcenpool zu erstellen.

        $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
        $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6

    >[AZURE.IMPORTANT] Lastenausgleich verwendet die Bezeichnung Domäne für die öffentliche IP-Adresse als Präfix für den vollqualifizierten Domänennamen ein. In diesem Beispiel werden die Fully *lbnrpipv4.westus.cloudapp.azure.com* und *lbnrpipv6.westus.cloudapp.azure.com*.

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Erstellen einer Front-End-IP-Konfigurationen und einem Back-End-Adresse Ressourcenpool

1. Erstellen Sie die Konfiguration der Front-End-Adresse, die die öffentliche IP-Adressen verwendet, die Sie erstellt haben.

        $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
        $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6

2. Back-End-Adresspool anlegen.

        $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
        $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"


## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>Erstellen von Pfd Regeln, NAT Regeln, einen Prüfpunkt und ein Lastenausgleich

In diesem Beispiel wird die folgenden Elemente:

- eine Regel NAT alle eingehenden Datenverkehr auf Anschluss 443 Anschluss 4443 übersetzen.
- eine Auslastung Lastenausgleich Regel alle eingehenden Datenverkehr an Port 80 an Port 80 auf die Adressen in die Back-End-Ressourcenpool zu verteilen.
- eine Auslastung Lastenausgleich Regel RDP-Verbindung mit den virtuellen Computern Port 3389 zuzulassen.
- eine Regel Prüfpunkt So überprüfen Sie den Status auf einer Seite mit dem Namen *HealthProbe.aspx* oder einem Dienst im Anschluss 8080
- ein Lastenausgleich, die alle diese Objekte verwendet.

1. Erstellen Sie die NAT-Regeln.

        $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
        $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443

2. Erstellen Sie einen Prüfpunkt Dienststatus. Es gibt zwei Möglichkeiten, um einen Prüfpunkt konfigurieren:

    HTTP-Prüfpunkt

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    oder TCP Prüfpunkt

        $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
        $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2


    In diesem Beispiel werden wir die Prüfpunkte TCP verwenden.

3. Erstellen einer laden Lastenausgleich Regel.

        $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
        $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389

4. Erstellen Sie mithilfe der zuvor erstellten Objekte Lastenausgleich.

        $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule

## <a name="create-nics-for-the-back-end-vms"></a>Erstellen von NICs für die Back-End-virtuellen Computern

1. Erhalten der virtuelle Netzwerk und virtuelle Netzwerk-Subnetz, in denen die NICs erstellt werden müssen.

        $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
        $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet

2. Erstellen Sie für den virtuellen Computern IP-Konfigurationen und NICs.

        $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
        $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
        $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

        $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
        $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
        $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'

## <a name="create-virtual-machines-and-assign-the-newly-created-nics"></a>Erstellen von virtuellen Computern und Zuweisen der neu erstellten NICs

Weitere Informationen zum Erstellen eines virtuellen Computers finden Sie unter [Erstellen und einem Windows-Computer mit Ressourcenmanager und Azure PowerShell vorkonfiguriert](..\virtual-machines\virtual-machines-windows-ps-create.md)

1. Erstellen Sie ein Konto Verfügbarkeit festlegen und Speicher

        New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
        $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
        New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName $LRS
        $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'

2. Jeder virtueller Computer erstellen und Zuweisen von vorhergehenden NICs erstellt

        $mySecureCredentials= Get-Credential -Message “Type the username and password of the local administrator account.”

        $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
        $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
        $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

        $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
        $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
        $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
        $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
        $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
        New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2

## <a name="next-steps"></a>Nächste Schritte

[Erste Schritte zum Konfigurieren einer internen Lastenausgleich](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurieren eines laden Lastenausgleich Verteilung Modus](load-balancer-distribution-mode.md)

[Konfigurieren von Einstellungen zur im Leerlauf TCP Timeout für Ihre Lastenausgleich](load-balancer-tcp-idle-timeout.md)
