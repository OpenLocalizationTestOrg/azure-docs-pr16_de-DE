## <a name="how-to-create-a-vnet-using-the-azure-cli"></a>So erstellen Sie eine VNet mithilfe der Azure-CLI

Die CLI Azure können Sie um über die Befehlszeile von jedem Computer unter Windows, Linux oder OSX Azure Ressourcen zu verwalten. Zum Erstellen einer VNet mithilfe der Azure CLI führen Sie die folgenden Schritte aus.

1. Wenn Sie die CLI Azure nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der CLI Azure](../articles/xplat-cli-install.md) , und folgen Sie den Anweisungen auf den Punkt, in dem Sie Ihre Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie den Befehl **Azure Config Modus** Ressourcenmanager im Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Hier ist die erwartete Ausgabe für den oben angegebenen Befehl aus:

        info:    New mode is arm

3. Falls erforderlich, führen Sie die **Azure Gruppe erstellen** zum Erstellen einer neuen Ressourcengruppe wie unten dargestellt. Beachten Sie die Ausgabe des Befehls an. Der Liste angezeigt, nachdem die Ausgabe wird, die Parameter verwendet erläutert. Weitere Informationen zu Ressourcengruppen finden Sie auf [Azure Ressourcenmanager Übersicht](../articles/virtual-network/resource-group-overview.md#resource-groups).

        azure group create -n TestRG -l centralus

    Hier ist die erwartete Ausgabe für den oben angegebenen Befehl aus:

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **-n (oder – Name)**. Namen für die neue Ressourcengruppe. In diesem Szenario *TestRG*.
    - **-l (oder – Speicherort)**. Azure Region, in dem die neue Ressourcengruppe erstellt wird. In diesem Szenario *Centralus*.

4. Führen Sie den Befehl **Azure Netzwerk Vnet erstellen** , zum Erstellen einer VNet und einem Subnetz, aus, wie unten dargestellt. 

        azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l centralus

    Hier ist die erwartete Ausgabe für den oben angegebenen Befehl aus:

        info:    Executing command network vnet create
        + Looking up virtual network "TestVNet"
        + Creating virtual network "TestVNet"
        + Loading virtual network state
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet2
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK

    - **-g (oder – Ressourcengruppe)**. Name der Ressourcengruppe, wo die VNet erstellt werden soll. In diesem Szenario *TestRG*.
    - **-n (oder – Name)**. Namen der VNet erstellt werden. In diesem Szenario, *TestVNet*
    - **-ein (oder – Adresse-Präfixe)**. Liste der CIDR-Blocks nach dem VNet Adresse Leerzeichen verwendet. In diesem Szenario, *192.168.0.0/16*
    - **-l (oder – Speicherort)**. Azure Region, wo die VNet erstellt werden soll. In diesem Szenario *Centralus*.

5. Führen Sie den Befehl **Azure Netzwerk Vnet Subnetz erstellen,** erstellen Sie ein Subnetz wie unten dargestellt. Beachten Sie die Ausgabe des Befehls an. Der Liste angezeigt, nachdem die Ausgabe wird, die Parameter verwendet erläutert.

        azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24

    Hier ist die erwartete Ausgabe für den oben angegebenen Befehl aus:

        info:    Executing command network vnet subnet create
        + Looking up the subnet "FrontEnd"
        + Creating subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK

    - **-e (oder – Vnet-Name**. Name der VNet, wo das Subnetz erstellt wird. In diesem Szenario *TestVNet*.
    - **-n (oder – Name)**. Name der das neue Subnetz. In diesem Szenario *Front-End*.
    - **-ein (oder – Adresse-Präfix)**. Subnetz CIDR blockieren. Vier unserer Szenario, *192.168.1.0/24*.

6. Wiederholen Sie Schritt 5 zum Erstellen von anderen Subnets, oben, falls erforderlich. In diesem Szenario, führen Sie den Befehl unten im *Back-End-* Subnetz erstellen.

        azure network vnet subnet create -g TestRG -e TestVNet -n BackEnd -a 192.168.2.0/24

4. Führen Sie den Befehl **Azure Netzwerk Vnet anzeigen** , zum Anzeigen der Eigenschaften für die neue Vnet aus, wie unten dargestellt.

        azure network vnet show -g TestRG -n TestVNet

    Hier ist die erwartete Ausgabe für den oben angegebenen Befehl aus:

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK
