<properties 
   pageTitle="Routing steuern und virtuelle Einheiten, die über die CLI Azure im Bereitstellungsmodell klassischen verwenden | Microsoft Azure"
   description="Erfahren Sie, wie routing in VNets mithilfe der Azure CLI im Bereitstellungsmodell klassischen steuern"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-the-azure-cli"></a>Routing steuern und virtuelle Einheiten (klassisch) werden über die CLI Azure verwenden

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell klassischen. Sie können auch das [Steuerelement routing und virtuelle Einheiten im Bereitstellungsmodell Ressourcenmanager verwenden](virtual-network-create-udr-arm-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Die folgenden Beispiel Azure CLI Befehle erwarten eine einfache Umgebung bereits erstellten basierend auf dem Szenario oben. Wenn die Befehle ausführen, wie sie in diesem Dokument angezeigt werden soll, erstellen Sie die Umgebung, die in [einer VNet (klassische) verwenden die CLI Azure erstellen](virtual-networks-create-vnet-classic-cli.md)angezeigt.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Erstellen der UDR für das front-End-Subnetz
Gehen Sie wie folgt vor um die Routingtabelle und Routing für das front-End-Subnetz basierend auf dem oben genannten Szenario erforderlich erstellen.

1. Führen Sie die **`azure config mode`** auf zur klassischen Ansicht wechseln.

        azure config mode asm

    Ergebnis:

        info:    New mode is asm

3. Führen Sie die **`azure network route-table create`** Befehl zum Erstellen einer Routingtabelle für das front-End-Subnetz.

        azure network route-table create -n UDR-FrontEnd -l uswest

    Ergebnis:

        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK

    Parameter:
    - **-l (oder – Speicherort)**. Azure Region, in dem das neue NSG erstellt wird. In diesem Szenario *Westus*.
    - **-n (oder – Name)**. Namen für die neue NSG. In diesem Szenario *NSG-Front-End*.

4. Führen Sie die **`azure network route-table route set`** Befehl zum Erstellen einer Routing in der Routingtabelle erstellt über alle Datenverkehr auf dem Back-End Subnetz (192.168.2.0/24) **FW1** virtueller Computer (192.168.0.4) zu senden.

        azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4

    Ergebnis:

        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK

    Parameter:
    - **-r (oder – Routing Tabellenname)**. Name der Tabelle weiterleiten an die Routing hinzugefügt wird. In diesem Szenario *UDR-Front-End*.
    - **-ein (oder – Adresse-Präfix)**. Adresspräfix für das Subnetz, in dem Pakete an gerichtet sind. In diesem Szenario *192.168.2.0/24*.
    - **t-(oder – weiter-Abschnitte-Typ)**. Typ des Objekts Datenverkehr gesendet werden. Mögliche Werte sind *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*oder *keiner*.
    - **-p (oder – weiter Abschnitte Ip-Adresse**). IP-Adresse für den nächsten Abschnitt. In diesem Szenario *192.168.0.4*.

5. Führen Sie die **`azure network vnet subnet route-table add`** Befehl zum Zuordnen der Routingtabelle mit der **Front-End** -Subnetz über erstellt.

        azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd

    Ergebnis:

        info:    Executing command network vnet subnet route-table add
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK 

    Parameter:
    - **t-(oder – Vnet-Name)**. Name der VNet, in das Subnetz befindet. In diesem Szenario *TestVNet*.
    - **- n (oder – Subnetz-Name**. Name des im Subnetz der Routingtabelle hinzugefügt werden. In diesem Szenario *Front-End*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Erstellen der UDR für die Back-End-Subnetz
Gehen Sie wie folgt vor um die Routingtabelle und Routing für die Back-End-Subnetz basierend auf dem oben genannten Szenario erforderlich erstellen.

3. Führen Sie die **`azure network route-table create`** Befehl zum Erstellen einer Routingtabelle für die Back-End-Subnetz.

        azure network route-table create -n UDR-BackEnd -l uswest

4. Führen Sie die **`azure network route-table route set`** Befehl zum Erstellen einer Routing in der Routingtabelle erstellt über alle Datenverkehr auf dem front-End Subnetz (192.168.1.0/24) **FW1** virtueller Computer (192.168.0.4) zu senden.

        azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4

5. Führen Sie die **`azure network vnet subnet route-table add`** Befehl zum Zuordnen der Routingtabelle mit dem **Back-End-** Subnetz über erstellt.

        azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd

