<properties
    pageTitle="Erstellen von virtuellen Computern skalieren Sets mithilfe der PowerShell-Cmdlets | Microsoft Azure"
    description="Erste Schritte, erstellen und Verwalten von Ihrer ersten Azure virtuellen Computern skalieren Sätze aus Azure PowerShell-cmdlets"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="danielsollondon"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="danielsollondon"/>

# <a name="creating-virtual-machine-scale-sets-using-powershell-cmdlets"></a>Erstellen von virtuellen Computern skalieren Sets mithilfe der PowerShell-cmdlets

Dies ist ein Beispiel einer virtuellen Computern skalieren Set(VMSS) erstellen, es wird eine VMSS von 3 Knoten mit alle zugeordneten Netzwerke und Speicher erstellt.

## <a name="first-steps"></a>Erste Schritte
Stellen Sie sicher, Sie das neueste Azure PowerShell-Modul installiert haben, wird dies der PowerShell-Cmdlets zum Verwalten und Erstellen von VMSS erforderlich sind enthalten.
Klicken Sie auf die Befehlszeile Extras [hier](http://aka.ms/webpi-azps) für die neueste verfügbare Azure-Module.

Zum Suchen von VMSS Zusammenhang Cmdlets, verwenden Sie die zu durchsuchenden Zeichenfolge \*VMSS\*.

## <a name="creating-a-vmss"></a>Erstellen einer VMSS

##### <a name="create-resource-group"></a>Ressourcengruppe erstellen

```
$loc = 'westus';
$rgname = 'mynewrgwu';
  New-AzureRmResourceGroup -Name $rgname -Location $loc -Force;
```

##### <a name="create-storage-account"></a>Erstellen von Speicher-Konto

Festlegen von Speicher Kontotyp / name.

```
$stoname = 'sto' + $rgname;
$stotype = 'Standard_LRS';
 New-AzureRmStorageAccount -ResourceGroupName $rgname -Name $stoname -Location $loc -SkuName $stotype -Kind "Storage";

$stoaccount = Get-AzureRmStorageAccount -ResourceGroupName $rgname -Name $stoname;
```

#### <a name="create-networking-vnet--subnet"></a>Erstellen von Netzwerke (VNET / Subnetz)

##### <a name="subnet-specification"></a>Subnetzspezifikation

```
$subnetName = 'websubnet'
  $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix "10.0.0.0/24";
```

##### <a name="vnet-specification"></a>Spezifikation der VNET

```
$vnet = New-AzureRmVirtualNetwork -Force -Name ('vnet' + $rgname) -ResourceGroupName $rgname -Location $loc -AddressPrefix "10.0.0.0/16" -Subnet $subnet;
$vnet = Get-AzureRmVirtualNetwork -Name ('vnet' + $rgname) -ResourceGroupName $rgname;

#In this case below we assume the new subnet is the only one, note difference if you have one already or have adjusted this code to more than one subnet.
$subnetId = $vnet.Subnets[0].Id;
```

##### <a name="create-public-ip-resource-to-allow-external-access"></a>Erstellen von öffentlichen IP-Ressource, um den externen Zugriff zulassen

Dies wird gebunden werden, um die an den Lastenausgleich.

```
$pubip = New-AzureRmPublicIpAddress -Force -Name ('pubip' + $rgname) -ResourceGroupName $rgname -Location $loc -AllocationMethod Dynamic -DomainNameLabel ('pubip' + $rgname);
$pubip = Get-AzureRmPublicIpAddress -Name ('pubip' + $rgname) -ResourceGroupName $rgname;
```

##### <a name="create-and-configure-load-balancer"></a>Erstellen und Konfigurieren von Lastenausgleich

```
$frontendName = 'fe' + $rgname
$backendAddressPoolName = 'bepool' + $rgname
$probeName = 'vmssprobe' + $rgname
$inboundNatPoolName = 'innatpool' + $rgname
$lbruleName = 'lbrule' + $rgname
$lbName = 'vmsslb' + $rgname

#Bind Public IP to Load Balancer
$frontend = New-AzureRmLoadBalancerFrontendIpConfig -Name $frontendName -PublicIpAddress $pubip
```

##### <a name="configure-load-balancer"></a>Konfigurieren von Lastenausgleich
Back-End-Adresse Ressourcenpool Config erstellen, werden diese durch die NICs von den virtuellen Computern in VMSS freigegeben.

```
$backendAddressPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name $backendAddressPoolName
```

Laden angeglichene Prüfpunkt Port festlegen, Ändern der Einstellungen für eine Anwendung Bedarf.

```
$probe = New-AzureRmLoadBalancerProbeConfig -Name $probeName -RequestPath healthcheck.aspx -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
```

Erstellen von NAT-Regeln für direkte weitergeleitete Konnektivität (nicht Lastenausgleich) mit den virtuellen Computern in der VMSS über den Lastenausgleich, Notiz, die diese veranschaulicht, wie mit RDP, dies trifft nur für die Vorführung und interne VNET Methoden für RDP'ing mit diesen Servern verwendet werden sollte.

```
$frontendpoolrangestart = 3360
$frontendpoolrangeend = 3370
$backendvmport = 3389
$inboundNatPool = New-AzureRmLoadBalancerInboundNatPoolConfig -Name $inboundNatPoolName -FrontendIPConfigurationId `
$frontend.Id -Protocol Tcp -FrontendPortRangeStart $frontendpoolrangestart -FrontendPortRangeEnd $frontendpoolrangeend -BackendPort $backendvmport;
```

Erstellen Sie Regel, die laden verteilt, dieses Beispiel zeigt den Lastenausgleich Port 80-Anfragen Laden mit den Einstellungen aus den vorherigen Schritten.

```
$protocol = 'Tcp'
$feLBPort = 80
$beLBPort = 80

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name $lbruleName `
-FrontendIPConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -Protocol $protocol -FrontendPort $feLBPort -BackendPort $beLBPort `
-IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -Verbose;
```

Erstellen Sie mit der Konfiguration Lastenausgleich ein.

```
$actualLb = New-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname -Location $loc `
-FrontendIpConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -LoadBalancingRule $lbrule -InboundNatPool $inboundNatPool -Verbose;
```

Pfd Einstellungen aktivieren, und aktivieren Sie Port Konfigurationen Lastenausgleich, Notiz wird erst angezeigt, NAT eingehende Regeln des virtuellen Computers in der VMSS erstellt werden.

```
$expectedLb = Get-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname
```

##### <a name="configure-and-create-vmss"></a>Konfigurieren und Erstellen von VMSS

Notiz dieses Infrastruktur Beispiel zeigt, wie Sie Setup verteilen und Web-Verkehr über die VMSS skalieren, aber die hier angegebenen virtuellen Computern Bilder verfügen über keine Webdienste installiert.

```
#specify VMSS Name
$vmssName = 'vmss' + $rgname;

##specify VMSS specific details
$adminUsername = 'azadmin';
$adminPassword = "Password1234!";

$PublisherName = 'MicrosoftWindowsServer'
$Offer         = 'WindowsServer'
$Sku          = '2012-R2-Datacenter'
$Version       = 'latest'
$vmNamePrefix = 'winvmss'

$vhdContainer = "https://" + $stoname + ".blob.core.windows.net/" + $vmssName;

###add an extension
$extname = 'BGInfo';
$publisher = 'Microsoft.Compute';
$exttype = 'BGInfo';
$extver = '2.1';
```

Binden Sie NIC mit Lastenausgleich und Subnetz

```
$ipCfg = New-AzureRmVmssIPConfig -Name 'nic' `
-LoadBalancerInboundNatPoolsId $actualLb.InboundNatPools[0].Id `
-LoadBalancerBackendAddressPoolsId $actualLb.BackendAddressPools[0].Id `
-SubnetId $subnetId;
```

Erstellen von VMSS Config

```
#Specify number of nodes
$numberofnodes = 3

$vmss = New-AzureRmVmssConfig -Location $loc -SkuCapacity $numberofnodes -SkuName 'Standard_A2' -UpgradePolicyMode 'automatic' `
  	| Add-AzureRmVmssNetworkInterfaceConfiguration -Name $subnetName -Primary $true -IPConfiguration $ipCfg `
  	| Set-AzureRmVmssOSProfile -ComputerNamePrefix $vmNamePrefix -AdminUsername $adminUsername -AdminPassword $adminPassword `
  	| Set-AzureRmVmssStorageProfile -Name 'test' -OsDiskCreateOption 'FromImage' -OsDiskCaching 'None' `
    -ImageReferenceOffer $Offer -ImageReferenceSku $Sku -ImageReferenceVersion $Version `
    -ImageReferencePublisher $PublisherName -VhdContainer $vhdContainer `
  	| Add-AzureRmVmssExtension -Name $extname -Publisher $publisher -Type $exttype -TypeHandlerVersion $extver -AutoUpgradeMinorVersion $true
```

Erstellen von VMSS Config

```
New-AzureRmVmss -ResourceGroupName $rgname -Name $vmssName -VirtualMachineScaleSet $vmss -Verbose;
```

Nachdem Sie die VMSS erstellt haben. Sie können testen, Herstellen einer Verbindung mit den einzelnen virtuellen Computer mit RDP in diesem Beispiel:

```
VM0 : pubipmynewrgwu.westus.cloudapp.azure.com:3360
VM1 : pubipmynewrgwu.westus.cloudapp.azure.com:3361
VM2 : pubipmynewrgwu.westus.cloudapp.azure.com:3362
```
