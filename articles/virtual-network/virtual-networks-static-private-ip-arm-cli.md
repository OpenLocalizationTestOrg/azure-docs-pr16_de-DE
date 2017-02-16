<properties 
   pageTitle="So legen Sie eine statische private IP-Adresse in die Cloud-Modus mithilfe der CLI | Microsoft Azure"
   description="Grundlegendes zu statischen IP-Adressen (DIPs) und wie sie in der Cloud-Modus mithilfe der CLI verwaltet"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
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

# <a name="how-to-set-a-static-private-ip-address-in-azure-cli"></a>So legen Sie eine statische private IP-Adresse in Azure CLI

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Modell zur Bereitstellung von Ressourcenmanager. Sie können auch [statische private IP-Adresse in das Bereitstellungsmodell klassischen verwalten](virtual-networks-static-private-ip-classic-cli.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Die folgenden Beispiel Azure CLI Befehle erwarten eine einfache-Umgebung, die bereits erstellt haben. Wenn die Befehle ausführen, wie sie in diesem Dokument angezeigt werden soll, erstellen Sie zuerst der testumgebung beschrieben, die in [einer Vnet erstellen](virtual-networks-create-vnet-arm-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>So geben Sie beim Erstellen eines virtuellen Computers eine statische private IP-Adresse
Zum Erstellen eines virtuellen Computers mit dem Namen *DNS01* in der *Front-End* -Subnetz von einer VNet mit dem Namen *TestVNet* mit einem statischen privaten IP-Adresse des *192.168.1.101*führen Sie die folgenden Schritte aus:

1. Wenn Sie noch keine Erfahrung Azure CLI haben, finden Sie unter [Installieren und Konfigurieren der CLI Azure](../xplat-cli-install.md) , und folgen Sie den Anweisungen auf den Punkt, in dem Sie Ihre Azure-Konto und Ihr Abonnement auswählen.

2. Führen Sie den Befehl **Azure Config Modus** Ressourcenmanager im Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Erwartetes Ergebnis:

        info:    New mode is arm

3. Führen Sie das **Erstellen von Azure Netzwerk öffentlichen Ip -** um eine öffentliche IP-Adresse für den virtuellen Computer zu erstellen. Der Liste angezeigt, nachdem die Ausgabe wird, die Parameter verwendet erläutert.

        azure network public-ip create -g TestRG -n TestPIP -l centralus
    
    Erwartetes Ergebnis:

        info:    Executing command network public-ip create
        + Looking up the public ip "TestPIP"
        + Creating public ip address "TestPIP"
        + Looking up the public ip "TestPIP"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
        data:    Name                            : TestPIP
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Dynamic
        data:    Idle timeout                    : 4
        info:    network public-ip create command OK

    - **-g (oder – Ressourcengruppe)**. Name der Ressourcengruppe die öffentliche IP-Adresse wird in erstellt werden.
    - **-n (oder – Name)**. Name des die öffentliche IP-Adresse.
    - **-l (oder – Speicherort)**. Azure Region, in dem die öffentliche IP-Adresse erstellt wird. In diesem Szenario *Centralus*.

3. Führen Sie so erstellen Sie einen Netzwerkadapter mit einer statischen privaten IP **Azure Netzwerk Nic erstellen** Befehl aus Der Liste angezeigt, nachdem die Ausgabe wird, die Parameter verwendet erläutert.

        azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd

    Erwartetes Ergebnis:

        + Looking up the network interface "TestNIC"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC"
        + Looking up the network interface "TestNIC"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
        data:    Name                            : TestNIC
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

    - **-ein (oder – Private Ip-Adresse)**. Statische private IP-Adresse für die Netzwerkkarte.
    - **m-(oder – Subnetz-Vnet-Name)**. Name der VNet, in dem die NIC erstellt wird.
    - **-k (oder – Subnetz-Name)**. Name des im Subnetz, in dem die NIC erstellt wird.

4. Führen Sie den Befehl **Azure-virtuellen Computer erstellen** , um den virtuellen Computer mit dem öffentlichen IP- und NIC oben erstellten erstellen. Der Liste angezeigt, nachdem die Ausgabe wird, die Parameter verwendet erläutert.

        azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd

    Erwartetes Ergebnis:

        info:    Executing command vm create
        + Looking up the VM "DNS01"
        info:    Using the VM Size "Standard_A1"
        warn:    The image "bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2" will be used for VM
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account vnetstorage
        + Looking up the NIC "TestNIC"
        info:    Found an existing NIC "TestNIC"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in the NIC "TestNIC"
        info:    Found public ip parameters, trying to setup PublicIP profile
        + Looking up the public ip "TestPIP"
        info:    Found an existing PublicIP "TestPIP"
        info:    Configuring identified NIC IP configuration with PublicIP "TestPIP"
        + Updating NIC "TestNIC"
        + Looking up the NIC "TestNIC"
        + Creating VM "DNS01"
        info:    vm create command OK

    - **y-(oder – os-Typ)**. Typ des Betriebssystems für den virtuellen Computer *Windows* oder *Linux*.
    - **f-(oder –-Namen)**. Name der NIC der virtuellen Computer verwendet wird.
    - **-i (oder – öffentlichen-Ip-Name)**. Name der öffentliche IP-Adresse, die der virtuellen Computer verwendet wird.
    - **F-(oder – Vnet-Name)**. Name der VNet, in der virtuellen Computer erstellt werden soll.
    - **-j (oder – Vnet-Subnetz-Name)**. Name des im Subnetz, in der virtuellen Computer erstellt werden soll.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Informationen zu Vorgehensweisen abrufen statische private IP-Adresse für einen virtuellen Computer

Führen Sie den folgenden Befehl aus Azure CLI und beachten Sie die Werte für *Private IP-zuordnen-Methode* und *Private IP-Adresse*, zum Anzeigen der statischen privaten IP-Adresse-Informationen für den virtuellen Computer mit dem oben genannten Skript erstellt:

    azure vm show -g TestRG -n DNS01

Erwartetes Ergebnis:

    info:    Executing command vm show
    + Looking up the VM "DNS01"
    + Looking up the NIC "TestNIC"
    + Looking up the public ip "TestPIP
    data:    Id                              :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    ProvisioningState               :Succeeded
    data:    Name                            :DNS01
    data:    Location                        :centralus
    data:    Type                            :Microsoft.Compute/virtualMachines
    data:
    data:    Hardware Profile:
    data:      Size                          :Standard_A1
    data:
    data:    Storage Profile:
    data:      Source image:
    data:        Id                          :/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/services/images/bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
    data:
    data:      OS Disk:
    data:        OSType                      :Windows
    data:        Name                        :cli08d7bd987a0112a8-os-1441774961355
    data:        Caching                     :ReadWrite
    data:        CreateOption                :FromImage
    data:        Vhd:
    data:          Uri                       :https://vnetstorage2.blob.core.windows.net/vhds/cli08d7bd987a0112a8-os-1441774961355vhd
    data:
    data:    OS Profile:
    data:      Computer Name                 :DNS01
    data:      User Name                     :adminuser
    data:      Windows Configuration:
    data:        Provision VM Agent          :true
    data:        Enable automatic updates    :true
    data:
    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-90-1A-A8
    data:          Provisioning State        :Succeeded
    data:          Name                      :TestNIC
    data:          Location                  :centralus
    data:            Private IP alloc-method :Static
    data:            Private IP address      :192.168.1.101
    data:            Public IP address       :40.122.213.159
    info:    vm show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>So entfernen Sie eine statische private IP-Adresse eines virtuellen Computers
Sie können keine statische private IP-Adresse aus einen Netzwerkadapter in Azure CLI für Ressourcenmanager entfernen. Sie müssen erstellen einen neuen Netzwerkadapter, der eine dynamische IP-Adresse verwendet wird, entfernen die vorherige NIC aus dem virtuellen Computer, und fügen Sie anschließend die neue Netzwerkkarte den virtuellen Computer. Zum Ändern der NIC für die Int virtueller Computer verwendet führen eh Befehle oben, Sie die folgenden Schritte aus.
    
1. Führen Sie den Befehl **Azure Netzwerk Nic erstellen** , um einen neuen Netzwerkadapter mit dynamischen IP-Zuordnung zu erstellen. Beachten Sie, wie Sie nicht die IP-Adresse diesmal angeben müssen.

        azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd

    Erwartetes Ergebnis:

        info:    Executing command network nic create
        + Looking up the network interface "TestNIC2"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC2"
        + Looking up the network interface "TestNIC2"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
        data:    Name                            : TestNIC2
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.6
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

2. Führen Sie den Befehl **Azure-virtuellen Computer festlegen** , so ändern Sie die NIC durch den virtuellen Computer verwendet.

        azure vm set -g TestRG -n DNS01 -N TestNIC2

    Erwartetes Ergebnis:

        info:    Executing command vm set
        + Looking up the VM "DNS01"
        + Looking up the NIC "TestNIC2"
        + Updating VM "DNS01"
        info:    vm set command OK

3. Wenn beispielsweise, führen Sie den Befehl **Azure Netzwerk Nic löschen** , so löschen Sie die alte Netzwerkkarte.

        azure network nic delete -g TestRG -n TestNIC --quiet

    Erwartetes Ergebnis:

        info:    Executing command network nic delete
        + Looking up the network interface "TestNIC"
        + Deleting network interface "TestNIC"
        info:    network nic delete command OK

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>So fügen Sie eine statische private IP-Adresse zu einer vorhandenen virtuellen Computer
Um eine statische private IP-Adresse die von den virtuellen Computer erstellt, mit dem oben genannten Skript verwendete NIC hinzuzufügen, führen Sie den folgenden Befehl aus:

    azure network nic set -g TestRG -n TestNIC2 -a 192.168.1.101

Erwartetes Ergebnis:

    info:    Executing command network nic set
    + Looking up the network interface "TestNIC2"
    + Updating network interface "TestNIC2"
    + Looking up the network interface "TestNIC2"
    data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
    data:    Name                            : TestNIC2
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : centralus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-90-29-25
    data:    Enable IP forwarding            : false
    data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    IP configurations:
    data:      Name                          : NIC-config
    data:      Provisioning state            : Succeeded
    data:      Private IP address            : 192.168.1.101
    data:      Private IP Allocation Method  : Static
    data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

## <a name="next-steps"></a>Nächste Schritte

- Informationen Sie zu [Reservierte öffentliche IP-](virtual-networks-reserved-public-ip.md) Adressen.
- Informationen Sie zu Adressen [Instanz Ebene öffentlichen IP-(ILPIP)](virtual-networks-instance-level-public-ip.md) .
- Wenden Sie sich an die [reservierte IP-REST-APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).
