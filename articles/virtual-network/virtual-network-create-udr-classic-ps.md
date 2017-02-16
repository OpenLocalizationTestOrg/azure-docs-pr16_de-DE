<properties 
   pageTitle="Routing steuern und virtuelle Einheiten mithilfe der PowerShell im Bereitstellungsmodell klassischen verwenden | Microsoft Azure"
   description="Erfahren Sie, wie routing in VNets mithilfe der PowerShell im Bereitstellungsmodell klassischen steuern"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Steuern Sie der routing und verwenden virtuelle Einheiten (klassisch) mithilfe der PowerShell

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell klassischen.

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Im Beispiel oben beschriebenen Szenario Azure PowerShell unten aufgeführten Befehle eine einfache-Umgebung, die bereits erstellt erwarten Grundlage. Wenn die Befehle ausführen, wie sie in diesem Dokument angezeigt werden soll, erstellen Sie die Umgebung, die in [einer VNet (klassische) mithilfe der PowerShell erstellen](virtual-networks-create-vnet-classic-netcfg-ps.md)angezeigt.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Erstellen der UDR für das front-End-Subnetz
Gehen Sie wie folgt vor um die Routingtabelle und Routing für das front-End-Subnetz basierend auf dem oben genannten Szenario erforderlich erstellen.

3. Ausführen der **`New-AzureRouteTable`** -Cmdlet zum Erstellen einer Routingtabelle für das front-End-Subnetz.

        New-AzureRouteTable -Name UDR-FrontEnd `
            -Location uswest `
            -Label "Route table for front end subnet"

    Ergebnis:

        Name         Location   Label                          
        ----         --------   -----                          
        UDR-FrontEnd West US    Route table for front end subnet

4. Ausführen der **`Set-AzureRoute`** -Cmdlet zum Erstellen einer Routing in der Routingtabelle erstellt über alle Datenverkehr auf dem Back-End Subnetz (192.168.2.0/24) **FW1** virtueller Computer (192.168.0.4) zu senden.
    
        Get-AzureRouteTable UDR-FrontEnd `
            |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

    Ergebnis:

        Name     : UDR-FrontEnd
        Location : West US
        Label    : Route table for frontend subnet
        Routes   : 
                   Name                 Address Prefix    Next hop type        Next hop IP address
                   ----                 --------------    -------------        -------------------
                   RouteToBackEnd       192.168.2.0/24    VirtualAppliance     192.168.0.4  

5. Führen Sie die **`Set-AzureSubnetRouteTable`** Cmdlet zum Zuordnen der Routingtabelle mit der **Front-End** -Subnetz über erstellt wurden.

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName FrontEnd `
            -RouteTableName UDR-FrontEnd
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Erstellen der UDR für die Back-End-Subnetz
Gehen Sie wie folgt vor um die Routingtabelle und Routing für die Back-End-Subnetz basierend auf dem oben genannten Szenario erforderlich erstellen.

3. Ausführen der **`New-AzureRouteTable`** -Cmdlet zum Erstellen einer Routingtabelle für die Back-End-Subnetz.

        New-AzureRouteTable -Name UDR-BackEnd `
            -Location uswest `
            -Label "Route table for back end subnet"

4. Ausführen der **`Set-AzureRoute`** -Cmdlet zum Erstellen einer Routing in der Routingtabelle erstellt über alle Datenverkehr auf dem front-End Subnetz (192.168.1.0/24) **FW1** virtueller Computer (192.168.0.4) zu senden.

        Get-AzureRouteTable UDR-BackEnd `
            |Set-AzureRoute -RouteName RouteToFrontEnd -AddressPrefix 192.168.1.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

5. Führen Sie die **`Set-AzureSubnetRouteTable`** Cmdlet zum Zuordnen der Routingtabelle über erstellt, mit dem **Back-End-** Subnetz.

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName BackEnd `
            -RouteTableName UDR-BackEnd

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>Aktivieren Sie die IP-Weiterleitung des FW1 virtuellen Computers
Um IP-Weiterleitung auf dem FW1 virtuellen Computer zu aktivieren, führen Sie die folgenden Schritte aus.

1. Ausführen der **`Get-AzureIPForwarding`** -Cmdlet zum Überprüfen des Status IP-Weiterleitung

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Get-AzureIPForwarding

    Ergebnis:

        Disabled

2. Führen Sie die **`Set-AzureIPForwarding`** Befehl aus, um die IP-Weiterleitung für die *FW1* virtuellen Computer aktivieren.

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Set-AzureIPForwarding -Enable
