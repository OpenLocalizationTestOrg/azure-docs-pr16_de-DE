<properties
   pageTitle="Öffnen eines virtuellen Computers mithilfe der PowerShell-Ports | Microsoft Azure"
   description="Informationen Sie zum Öffnen eines Ports / erstellen einen Endpunkt auf Ihrem Windows virtueller Computer mit dem Azure Ressource-Manager-Bereitstellung Modus und Azure-PowerShell"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Öffnen von Ports und Endpunkte eines virtuellen Computers in Azure mithilfe der PowerShell
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Symbolleiste Befehle
Zum Erstellen einer Sicherheitsgruppe Netzwerk und ACL Regeln benötigen Sie [die neueste Version von Azure PowerShell installiert](../powershell-install-configure.md). Sie können auch an, [Führen Sie diese Schritte im Portal Azure verwenden](virtual-machines-windows-nsg-quickstart-portal.md).

Melden Sie sich bei Ihrem Konto Azure aus:

```powershell
Login-AzureRmAccount
```

Ersetzen Sie in den folgenden Beispielen Beispiel Parameternamen durch Ihre eigenen Werte ein. Beispiel für Parameternamen enthalten `myResourceGroup`, `myNetworkSecurityGroup`, und `myVnet`.

Erstellen einer Regel. Im folgenden Beispiel wird eine Regel namens `myNetworkSecurityGroupRule` zu TCP-Datenverkehr an Port 80 zulässt:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" -Access "Allow" -Protocol "Tcp" -Direction "Inbound" `
    -Priority "100" -SourceAddressPrefix "Internet" -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 80
```

Als Nächstes erstellen Sie Ihr Netzwerk-Sicherheitsgruppe, und weisen Sie die HTTP-Regel, die Sie gerade wie folgt erstellt haben. Im folgenden Beispiel wird eine Netzwerk-Sicherheitsgruppe mit dem Namen `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNetworkSecurityGroup" -SecurityRules $httprule
```

Nun lassen Sie uns Ihre Netzwerk-Sicherheitsgruppe mit einem Subnetz zuweisen. Das folgende Beispiel weist ein vorhandenes virtuelles Netzwerk mit dem Namen `myVnet` der Variablen `$vnet`:

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Ordnen Sie Ihr Netzwerk-Sicherheitsgruppe Ihrem Subnetz gehören. Im folgende Beispiel ordnet das Subnetz mit dem Namen `mySubnet` mit Ihrem Netzwerk-Sicherheitsgruppe:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Aktualisieren Sie schließlich Ihre virtuelle Netzwerke in der Reihenfolge, damit die Änderungen wirksam werden:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Weitere Informationen zum Netzwerk-Sicherheitsgruppen
Hier die Symbolleiste Befehle ermöglichen Ihnen, Einstieg und die Verwendung mit den Datenverkehr an Ihre virtuellen Computer entdeckt aufzurufen. Netzwerk-Sicherheitsgruppen enthalten zahlreiche hervorragende Features und Genauigkeit zum Steuern des Zugriffs auf Ihre Ressourcen. Weitere Informationen zum [Erstellen einer Sicherheitsgruppe Netzwerk und hier ACL-Regeln](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Sie können als Teil der Ressourcenmanager Azure Vorlagen Sicherheitsgruppen Netzwerk und ACL-Regeln definieren. Weitere Informationen zum [Erstellen von Netzwerk-Sicherheitsgruppen mit Vorlagen](../virtual-network/virtual-networks-create-nsg-arm-template.md)finden.

Wenn Sie mit Port-Weiterleitung einen eindeutigen externen Port mit einem internen Anschluss Ihrer virtuellen Computers zuordnen müssen, verwenden Sie ein Lastenausgleich und Regeln (Netzwerkadressübersetzung). Angenommen, möchten Sie verfügbar machen TCP-8080 extern und Port 80 eines virtuellen Computers TCP gerichtete Datenverkehr haben. Weitere Informationen zum [Erstellen eines Internet zugänglichen Lastenausgleich](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Nächste Schritte
In diesem Beispiel haben Sie eine einfache Regel HTTP-Verkehr zulassen erstellt. Finden Sie Informationen zum Erstellen von ausführlichere Umgebungen in den folgenden Artikeln:

- [Azure Ressourcenmanager (Übersicht)](../azure-resource-manager/resource-group-overview.md)
- [Was ist ein Netzwerk Sicherheit Gruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Übersicht über die Azure Ressourcenmanager für Lastenausgleich](../load-balancer/load-balancer-arm.md)