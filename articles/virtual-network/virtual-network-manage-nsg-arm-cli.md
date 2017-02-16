<properties 
   pageTitle="Verwendung der Azure-CLI in Ressourcenmanager NSGs verwalten | Microsoft Azure"
   description="Informationen Sie zum Verwalten von vorhandenen NSGs mithilfe der Azure CLI in Ressourcenmanager"
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
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-the-azure-cli"></a>Verwendung der CLI Azure NSGs verwalten

[AZURE.INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="retrieve-information"></a>Abrufen von Informationen

Sie können Ihre vorhandene NSGs anzeigen, Regeln für eine vorhandene NSG abrufen und finden Sie heraus, welche Ressourcen ein NSG zugeordnet ist.

### <a name="view-existing-nsgs"></a>Vorhandene NSGs anzeigen

Um die Liste der NSGs in einer bestimmten Ressourcengruppe anzeigen möchten, führen Sie die `azure network nsg list` Befehl wie unten dargestellt. 

    azure network nsg list --resource-group RG-NSG

Erwartetes Ergebnis:

    info:    Executing command network nsg list
    + Getting the network security groups
    data:    Name          Location
    data:    ------------  --------
    data:    NSG-BackEnd   westus
    data:    NSG-FrontEnd  westus
    info:    network nsg list command OK
         
### <a name="list-all-rules-for-an-nsg"></a>Alle Regeln für ein NSG Liste

Um von einer NSG benannte **NSG-Front-End-**Regeln anzeigen möchten, führen Sie die `azure network nsg show` Befehl wie unten dargestellt. 

    azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd

Erwartetes Ergebnis:
    
    info:    Executing command network nsg show
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Name                            : NSG-FrontEnd
    data:    Type                            : Microsoft.Network/networkSecurityGroups
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    Tags                            : displayName=NSG - Front End
    data:    Security group rules:
    data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
    data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
    data:    rdp-rule                       Internet           *            *               3389              Tcp       Inbound    Allow   100
    data:    web-rule                       Internet           *            *               80                Tcp       Inbound    Allow   101
    data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000
    data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001
    data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500
    data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000
    data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001
    data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500
    info:    network nsg show command OK

>[AZURE.NOTE] Sie können auch `azure network nsg rule list --resource-group RG-NSG --nsg-name NSG-FrontEnd` die Regeln aus der **NSG-Front-End** NSG aufgelistet.

### <a name="view-nsg-associations"></a>Ansicht NSG Zuordnungen

Um anzuzeigen, was die NSG **NSG-Front-End ist-** Ressourcen zuordnen, führen Sie die `azure network nsg show` Befehl wie unten dargestellt. Beachten Sie, dass der einzige Unterschied die Verwendung des Parameters **– Json** ist.

    azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json

Suchen Sie die Eigenschaften **NetworkInterfaces** und **Subnetze** wie unten dargestellt:

    "networkInterfaces": [],
    ...
    "subnets": [
        {
            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
        }
    ],
    ...

Im Beispiel oben im NSG ist nicht mit einem beliebigen Netzwerk-Schnittstellen (NICs) verknüpft, und es mit einem Subnetz mit dem Namen **Front-End**verknüpft ist.

## <a name="manage-rules"></a>Verwalten von Regeln

Sie können Hinzufügen von Regeln zu einer vorhandenen NSG, bearbeiten vorhandene Regeln und Entfernen von Regeln.

### <a name="add-a-rule"></a>Hinzufügen einer Regel

Um einer Regel, da der **eingehenden** Datenverkehr an Port **443** aus einem beliebigen Computer zu **NSG-Front-End** NSG hinzuzufügen, führen Sie die `azure network nsg rule create` Befehl wie unten dargestellt.

    azure network nsg rule create --resource-group RG-NSG \
        --nsg-name NSG-FrontEnd \
        --name allow-https \
        --description "Allow access to port 443 for HTTPS" \
        --protocol Tcp \
        --source-address-prefix * \
        --source-port-range * \
        --destination-address-prefix * \
        --destination-port-range 443 \
        --access Allow \
        --priority 102 \
        --direction Inbound     

Erwartetes Ergebnis:

    info:    Executing command network nsg rule create
    + Looking up the network security rule "allow-https"
    + Creating a network security rule "allow-https"
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access to port 443 for HTTPS
    data:    Source IP                       : *
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule create command OK

### <a name="change-a-rule"></a>Ändern einer Regel

Führen Sie zum Ändern der Regel oben erstellte zulassen von eingehendem Datenverkehr aus dem **Internet** nur die `azure network nsg rule set` Befehl wie unten dargestellt.

    azure network nsg rule set --resource-group RG-NSG \
        --nsg-name NSG-FrontEnd \
        --name allow-https \
        --source-address-prefix Internet

Erwartetes Ergebnis:

    info:    Executing command network nsg rule set
    + Looking up the network security group "NSG-FrontEnd"
    + Setting a network security rule "allow-https"
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access to port 443 for HTTPS
    data:    Source IP                       : Internet
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule set command OK

### <a name="delete-a-rule"></a>Löschen einer Regel

Führen Sie zum Löschen der Regel erstellt, die über die `azure network nsg rule delete` Befehl wie unten dargestellt.

    azure network nsg rule delete --resource-group RG-NSG \
        --nsg-name NSG-FrontEnd \
        --name allow-https \
        --quiet

>[AZURE.NOTE] Die **--Stiller** Parameter stellt sicher, müssen Sie nicht den Löschvorgang zu bestätigen.

Erwartetes Ergebnis:

    info:    Executing command network nsg rule delete
    + Looking up the network security group "NSG-FrontEnd"
    + Deleting network security rule "allow-https"
    info:    network nsg rule delete command OK

## <a name="manage-associations"></a>Verwalten von Zuordnungen

Sie können eine NSG mit Subnetzen und NICs zuordnen. Sie können auch eine NSG aus jeder Ressource Zuordnung aufzuheben, um Sie verbunden ist.

### <a name="associate-an-nsg-to-a-nic"></a>Zuordnen einer NSG an einen Netzwerkadapter

Wenn **NSG-Front-End** NSG zu **TestNICWeb1** NIC verknüpfen möchten, führen Sie die `azure network nic set` Befehl wie unten dargestellt.

    azure network nic set --resource-group RG-NSG \
        --name TestNICWeb1 \
        --network-security-group-name NSG-FrontEnd

Erwartetes Ergebnis:

    info:    Executing command network nic set
    + Looking up the network interface "TestNICWeb1"
    + Looking up the network security group "NSG-FrontEnd"
    + Updating network interface "TestNICWeb1"
    + Looking up the network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Network security group          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Virtual machine                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-nic"></a>Heben Sie eine NSG aus einen Netzwerkadapter Zuweisung

Um die **NSG-Front-End** NSG aus **TestNICWeb1** NIC Zuordnung aufzuheben, führen Sie die `azure network nic set` Befehl wie unten dargestellt.

    azure network nic set --resource-group RG-NSG --name TestNICWeb1 --network-security-group-id ""

>[AZURE.NOTE] Beachten Sie die "" (leer) Wert für den **Netzwerk-Sicherheit-Gruppe-Id** -Parameter. Dies ist, wie Sie eine Zuordnung zu einer NSG zu entfernen. Sie können nicht mit dem **Netzwerk-Sicherheit-Gruppennamen** Parameter auch vornehmen.

Erwartetes Ergebnis:

    info:    Executing command network nic set
    + Looking up the network interface "TestNICWeb1"
    + Updating network interface "TestNICWeb1"
    + Looking up the network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Virtual machine                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-subnet"></a>Heben Sie eine NSG aus einem Subnetz Zuweisung

Um die aus dem **Front-End** -Subnetz **NSG-Front-End** NSG Zuordnung aufzuheben, führen Sie die `azure network vnet subnet set` Befehl wie unten dargestellt.

    azure network vnet subnet set --resource-group RG-NSG \
        --vnet-name TestVNet \
        --name FrontEnd \
        --network-security-group-id ""

Erwartetes Ergebnis:

    info:    Executing command network vnet subnet set
    + Looking up the subnet "FrontEnd"
    + Setting subnet "FrontEnd"
    + Looking up the subnet "FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:    Type                            : Microsoft.Network/virtualNetworks/subnets
    data:    ProvisioningState               : Succeeded
    data:    Name                            : FrontEnd
    data:    Address prefix                  : 192.168.1.0/24
    data:    IP configurations:
    data:      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
    data:      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
    data:
    info:    network vnet subnet set command OK

### <a name="associate-an-nsg-to-a-subnet"></a>Zuordnen einer NSG mit einem Subnetz

Um **NSG-Front-End** NSG erneut mit dem **FronEnd** Subnetz zuzuordnen, führen Sie die `azure network vnet subnet set` Befehl wie unten dargestellt.

    azure network vnet subnet set --resource-group RG-NSG \
        --vnet-name TestVNet \
        --name FrontEnd \
        --network-security-group-name NSG-FronEnd

>[AZURE.NOTE] Der Befehl über funktioniert nur, da die **NSG-Front-End** -NSG in derselben Ressourcengruppe als das virtuelle Netzwerk **TestVNet**ist. Wenn die NSG in einer anderen Ressourcengruppe ist, müssen Sie verwenden die **– Netzwerk-Sicherheit-Gruppe-Id** Parameter stattdessen, und geben Sie die vollständige-Id für die NSG. Sie können die Id unter **Azure Netzwerk Nsg anzeigen – RG-NSG – Namen NSG-Front-End – Json-Ressourcengruppe** ausgeführt, und suchen Sie nach der Eigenschaft **Id** abrufen. 

Erwartetes Ergebnis:

        info:    Executing command network vnet subnet set
        + Looking up the subnet "FrontEnd"
        + Looking up the network security group "NSG-FrontEnd"
        + Setting subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
        data:      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
        data:
        info:    network vnet subnet set command OK

## <a name="delete-an-nsg"></a>Löschen einer NSG

Sie können nur eine NSG löschen, wenn es nicht auf eine beliebige Ressource zugeordnet sind. Zum Löschen einer NSG führen Sie die folgenden Schritte aus.

1. Um zu einer NSG zugeordneten Ressourcen zu überprüfen, führen Sie die `azure network nsg show` wie in der [Ansicht NSGs Zuordnungen](#View-NSGs-associations)dargestellt.
2. Wenn die NSG alle NICs zugeordnet ist, führen Sie die `azure network nic set` Siehe [Dissociate einer NSG aus einen Netzwerkadapter](#Dissociate-an-NSG-from-a-NIC) für die einzelnen NICs. 
3. Wenn die NSG alle Subnetz zugeordnet ist, führen Sie die `azure network vnet subnet set` Siehe [Dissociate einer NSG aus einem Subnetz](#Dissociate-an-NSG-from-a-subnet) für jedes Subnetz.
4. Führen Sie zum Löschen der NSG der `azure network nsg delete` Befehl wie unten dargestellt.

        azure network nsg delete --resource-group RG-NSG --name NSG-FrontEnd --quiet

    Erwartetes Ergebnis:

        info:    Executing command network nsg delete
        + Looking up the network security group "NSG-FrontEnd"
        + Deleting network security group "NSG-FrontEnd"
        info:    network nsg delete command OK

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren der Protokollierung](virtual-network-nsg-manage-log.md) für NSGs.