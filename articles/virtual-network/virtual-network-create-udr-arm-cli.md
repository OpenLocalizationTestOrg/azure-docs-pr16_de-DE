<properties 
   pageTitle="Routing steuern und verwenden Sie virtuelle Einheiten in Ressourcenmanager mithilfe der CLI Azure | Microsoft Azure"
   description="Informationen Sie zum Steuern routing und virtuelle Einheiten, die über die CLI Azure verwenden"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="create-user-defined-routes-udr-in-the-azure-cli"></a>Erstellen benutzerdefinierter leitet (UDR) in der Azure CLI

[AZURE.INCLUDE [virtual-network-create-udr-arm-selectors-include.md](../../includes/virtual-network-create-udr-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Modell zur Bereitstellung von Ressourcenmanager. Sie können auch [UDRs im Bereitstellungsmodell klassischen erstellen](virtual-network-create-udr-classic-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Die folgenden Beispiel Azure CLI Befehle erwarten eine einfache Umgebung bereits erstellten basierend auf dem Szenario oben. Wenn die Befehle ausführen, wie sie in diesem Dokument angezeigt werden soll, zuerst erstellen Sie Umgebung für die Bereitstellung von [dieser Vorlage](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), klicken Sie auf **Bereitstellen in Azure**, ersetzen Sie den Parameter Standardwerte bei Bedarf, und folgen Sie den Anweisungen im Portal.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Erstellen der UDR für das front-End-Subnetz
Gehen Sie wie folgt vor um die Routingtabelle und Routing für das front-End-Subnetz basierend auf dem oben genannten Szenario erforderlich erstellen.

3. Führen Sie die **`azure network route-table create`** Befehl zum Erstellen einer Routingtabelle für das front-End-Subnetz.

        azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest

    Ergebnis:

        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK

    Parameter:
    - **-g (oder – Ressourcengruppe)**. Name der Ressourcengruppe, wo die UDR erstellt werden soll. In diesem Szenario *TestRG*.
    - **-l (oder – Speicherort)**. Azure Region, in dem die neue UDR erstellt wird. In diesem Szenario *Westus*.
    - **-n (oder – Name)**. Namen für die neue UDR. In diesem Szenario *UDR-Front-End*.

4. Führen Sie die **`azure network route-table route create`** Befehl zum Erstellen einer Routing in der Routingtabelle erstellt über alle Datenverkehr auf dem Back-End Subnetz (192.168.2.0/24) **FW1** virtueller Computer (192.168.0.4) zu senden.

        azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4

    Ergebnis:

        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK

    Parameter:
    - **-r (oder – Routing Tabellenname)**. Name der Tabelle weiterleiten an die Routing hinzugefügt wird. In diesem Szenario *UDR-Front-End*.
    - **-ein (oder – Adresse-Präfix)**. Adresspräfix für das Subnetz, in dem Pakete an gerichtet sind. In diesem Szenario *192.168.2.0/24*.
    - **y-(oder – weiter-Abschnitte-Typ)**. Typ des Objekts Datenverkehr gesendet werden. Mögliche Werte sind *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*oder *keiner*.
    - **-p (oder – weiter Abschnitte Ip-Adresse**). IP-Adresse für den nächsten Abschnitt. In diesem Szenario *192.168.0.4*.

5. Führen Sie die **`azure network vnet subnet set`** Befehl zum Zuordnen der Routingtabelle mit der **Front-End** -Subnetz über erstellt.

        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd

    Ergebnis:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

    Parameter:
    - **-e (oder – Vnet-Name)**. Name der VNet, in das Subnetz befindet. In diesem Szenario *TestVNet*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Erstellen der UDR für die Back-End-Subnetz
Gehen Sie wie folgt vor um die Routingtabelle und Routing für die Back-End-Subnetz basierend auf dem oben genannten Szenario erforderlich erstellen.

1. Führen Sie die **`azure network route-table create`** Befehl zum Erstellen einer Routingtabelle für die Back-End-Subnetz.

        azure network route-table create -g TestRG -n UDR-BackEnd -l westus

4. Führen Sie die **`azure network route-table route create`** Befehl zum Erstellen einer Routing in der Routingtabelle erstellt über alle Datenverkehr auf dem front-End Subnetz (192.168.1.0/24) **FW1** virtueller Computer (192.168.0.4) zu senden.

        azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4

5. Führen Sie die **`azure network vnet subnet set`** Befehl zum Zuordnen der Routingtabelle mit dem **Back-End-** Subnetz über erstellt.

        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd

## <a name="enable-ip-forwarding-on-fw1"></a>Aktivieren Sie die IP-Weiterleitung auf FW1
Um IP-Weiterleitung in die untersuchten **FW1**NIC zu aktivieren, führen Sie die folgenden Schritte aus.

1. Führen Sie die **`azure network nic show`** Befehl aus, und notieren Sie den Wert für die **Weiterleitung von IP aktivieren**. Es sollte *false*festgelegt werden.

        azure network nic show -g TestRG -n NICFW1

    Ergebnis:
        
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK

2. Führen Sie die **`azure network nic set`** Befehl aus, um die IP-Weiterleitung zu aktivieren.

        azure network nic set -g TestRG -n NICFW1 -f true

    Ergebnis:

        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK

    Parameter:

    - **f-(oder – aktivieren-Ip-Weiterleitung)**. *true* oder *false*.
