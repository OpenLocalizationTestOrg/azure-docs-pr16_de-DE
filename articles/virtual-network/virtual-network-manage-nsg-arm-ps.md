<properties 
   pageTitle="Verwalten von NSGs mithilfe der PowerShell in Ressourcenmanager | Microsoft Azure"
   description="Erfahren Sie, wie bereits vorhandene NSGs mithilfe der PowerShell in Ressourcenmanager verwalten"
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

# <a name="manage-nsgs-using-powershell"></a>Verwalten von NSGs mithilfe der PowerShell

[AZURE.INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a>Abrufen von Informationen

Sie können Ihre vorhandene NSGs anzeigen, Regeln für eine vorhandene NSG abrufen und finden Sie heraus, welche Ressourcen ein NSG zugeordnet ist.

### <a name="view-existing-nsgs"></a>Vorhandene NSGs anzeigen
Um alle vorhandenen NSGs in einem Abonnement anzuzeigen, führen Sie die `Get-AzureRmNetworkSecurityGroup` Cmdlet wie unten dargestellt.

    Get-AzureRmNetworkSecurityGroup

Erwartetes Ergebnis:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
                            
    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


Um die Liste der NSGs in einer bestimmten Ressourcengruppe anzeigen möchten, führen Sie die `Get-AzureRmNetworkSecurityGroup` Cmdlet wie unten dargestellt. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG

Erwartetes Ergebnis:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
         
### <a name="list-all-rules-for-an-nsg"></a>Alle Regeln für ein NSG Liste

Um von einer NSG benannte **NSG-Front-End-**Regeln anzeigen möchten, führen Sie die `Get-AzureRmNetworkSecurityGroup` Cmdlet wie unten dargestellt. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules

Erwartetes Ergebnis:
    
    Name                     : rdp-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound
    
    Name                     : web-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

>[AZURE.NOTE] Sie können auch `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` die Regeln für die von der **NSG-Front-End** NSG aufgelistet.

### <a name="view-nsgs-associations"></a>NSGs Zuordnungen anzeigen

Um anzuzeigen, was die NSG **NSG-Front-End ist-** Ressourcen zuordnen, führen Sie die `Get-AzureRmNetworkSecurityGroup` Cmdlet wie unten dargestellt.

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd

Suchen Sie nach der **NetworkInterfaces** und **Subnetze** Eigenschaften wie unten dargestellt:

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

Im Beispiel oben im NSG ist nicht mit einem beliebigen Netzwerk-Schnittstellen (NICs) verknüpft, und es mit einem Subnetz mit dem Namen **Front-End**verknüpft ist.

## <a name="manage-rules"></a>Verwalten von Regeln

Sie können Hinzufügen von Regeln zu einer vorhandenen NSG, bearbeiten vorhandene Regeln und Entfernen von Regeln.

### <a name="add-a-rule"></a>Hinzufügen einer Regel

Gehen Sie wie folgt vor um eine Regel zulassen **eingehenden** Datenverkehrs von einem beliebigen Computer zu **NSG-Front-End** NSG an Port **443** hinzuzufügen.

1. Ausführen der `Get-AzureRmNetworkSecurityGroup` -Cmdlet zum Abrufen der vorhandenen NSG und in einer Variablen zu speichern, wie unten dargestellt.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Führen Sie die `Add-AzureRmNetworkSecurityRuleConfig` Cmdlet, wie unten dargestellt.

        Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange * `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Um die an den NSG vorgenommenen Änderungen zu speichern, führen Sie die `Set-AzureRmNetworkSecurityGroup` Cmdlet wie unten dargestellt.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Erwartetes Ergebnis nur die Sicherheitsregeln mit:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a>Ändern einer Regel

Führen Sie zum Ändern der eingehenden Datenverkehr aus dem **Internet** nur zulassen oben erstellten Regel die folgenden Schritte aus.

1. Ausführen der `Get-AzureRmNetworkSecurityGroup` -Cmdlet zum Abrufen der vorhandenen NSG und in einer Variablen zu speichern, wie unten dargestellt.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Führen Sie die `Set-AzureRmNetworkSecurityRuleConfig` Cmdlet, wie unten dargestellt.

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange Internet `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Um die an den NSG vorgenommenen Änderungen zu speichern, führen Sie die `Set-AzureRmNetworkSecurityGroup` Cmdlet wie unten dargestellt.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Erwartetes Ergebnis nur die Sicherheitsregeln mit:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a>Löschen einer Regel

1. Ausführen der `Get-AzureRmNetworkSecurityGroup` -Cmdlet zum Abrufen der vorhandenen NSG und in einer Variablen zu speichern, wie unten dargestellt.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Führen Sie die `Remove-AzureRmNetworkSecurityRuleConfig` Cmdlet, wie unten dargestellt.

        Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule

3. Um die an den NSG vorgenommenen Änderungen zu speichern, führen Sie die `Set-AzureRmNetworkSecurityGroup` Cmdlet wie unten dargestellt.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Erwartetes Ergebnis mit nur die Sicherheitsregeln, beachten Sie, die die **Https-Regel** nicht mehr aufgeführt wird:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a>Verwalten von Zuordnungen

Sie können eine NSG mit Subnetzen und NICs zuordnen. Sie können auch eine NSG aus jeder Ressource Zuordnung aufzuheben, um Sie verbunden ist.

### <a name="associate-an-nsg-to-a-nic"></a>Zuordnen einer NSG an einen Netzwerkadapter

Gehen Sie wie folgt vor um **NSG-Front-End** NSG zu **TestNICWeb1** NIC zuzuordnen.

1. Ausführen der `Get-AzureRmNetworkSecurityGroup` -Cmdlet zum Abrufen der vorhandenen NSG und in einer Variablen zu speichern, wie unten dargestellt.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Ausführen der `Get-AzureRmNetworkInterface` -Cmdlet zum Abrufen der vorhandenen NIC und in einer Variablen zu speichern, wie unten dargestellt.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. Festlegen Sie die Eigenschaft **NetworkSecurityGroup** die **NIC** Variable auf den Wert der Variablen **NSG** , wie unten dargestellt.

        $nic.NetworkSecurityGroup = $nsg

4. Um die an die NIC vorgenommenen Änderungen zu speichern, führen Sie die `Set-AzureRmNetworkInterface` Cmdlet wie unten dargestellt.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Erwarteten Ergebnis nur die **NetworkSecurityGroup** Eigenschaft mit:

        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a>Heben Sie eine NSG aus einen Netzwerkadapter Zuweisung

Um die **NSG-Front-End** NSG aus **TestNICWeb1** NIC Zuordnung aufzuheben, führen Sie die folgenden Schritte aus.

1. Ausführen der `Get-AzureRmNetworkSecurityGroup` -Cmdlet zum Abrufen der vorhandenen NSG und in einer Variablen zu speichern, wie unten dargestellt.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Ausführen der `Get-AzureRmNetworkInterface` -Cmdlet zum Abrufen der vorhandenen NIC und in einer Variablen zu speichern, wie unten dargestellt.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. Legen Sie die Eigenschaft **NetworkSecurityGroup** der Variablen **NIC** auf **$null**, wie unten gezeigt.

        $nic.NetworkSecurityGroup = $null

4. Um die an die NIC vorgenommenen Änderungen zu speichern, führen Sie die `Set-AzureRmNetworkInterface` Cmdlet wie unten dargestellt.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Erwarteten Ergebnis nur die **NetworkSecurityGroup** Eigenschaft mit:

        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a>Heben Sie eine NSG aus einem Subnetz Zuweisung

Um die aus dem **Front-End** -Subnetz **NSG-Front-End** NSG Zuordnung aufzuheben, führen Sie die folgenden Schritte aus.

1. Ausführen der `Get-AzureRmVirtualNetwork` -Cmdlet zum Abrufen der vorhandenen VNet und in einer Variablen zu speichern, wie unten dargestellt.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Ausführen der `Get-AzureRmVirtualNetworkSubnetConfig` -Cmdlet zum Abrufen des **Front-End** -Subnetzes und in einer Variablen zu speichern, wie unten dargestellt.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

3. Legen Sie die Eigenschaft **NetworkSecurityGroup** der Variablen **Subnetz** auf **$null**, wie unten gezeigt.

        $subnet.NetworkSecurityGroup = $null

4. Um die mit dem Subnetz vorgenommenen Änderungen zu speichern, führen Sie die `Set-AzureRmVirtualNetwork` Cmdlet wie unten dargestellt.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Erwartetes Ergebnis nur die Eigenschaften der **Front-End** -Subnetz mit. Beachten Sie, dass keiner Eigenschaft für **NetworkSecurityGroup**zur Verfügung:

            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-to-a-subnet"></a>Zuordnen einer NSG mit einem Subnetz

Gehen Sie wie folgt vor um **NSG-Front-End** NSG mit dem Subnetz **FronEnd** zuzuordnen.

1. Ausführen der `Get-AzureRmVirtualNetwork` -Cmdlet zum Abrufen der vorhandenen VNet und in einer Variablen zu speichern, wie unten dargestellt.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Ausführen der `Get-AzureRmVirtualNetworkSubnetConfig` -Cmdlet zum Abrufen des **Front-End** -Subnetzes und in einer Variablen zu speichern, wie unten dargestellt.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

1. Ausführen der `Get-AzureRmNetworkSecurityGroup` -Cmdlet zum Abrufen der vorhandenen NSG und in einer Variablen zu speichern, wie unten dargestellt.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

3. Legen Sie die Eigenschaft **NetworkSecurityGroup** der Variablen **Subnetz** auf **$null**, wie unten gezeigt.

        $subnet.NetworkSecurityGroup = $nsg

4. Um die mit dem Subnetz vorgenommenen Änderungen zu speichern, führen Sie die `Set-AzureRmVirtualNetwork` Cmdlet wie unten dargestellt.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Erwartetes Ergebnis nur die **NetworkSecurityGroup** Eigenschaft der **Front-End** -Subnetz mit:

        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a>Löschen einer NSG

Sie können nur eine NSG löschen, wenn es nicht auf eine beliebige Ressource zugeordnet sind. Zum Löschen einer NSG führen Sie die folgenden Schritte aus.

1. Um zu einer NSG zugeordneten Ressourcen zu überprüfen, führen Sie die `azure network nsg show` wie in der [Ansicht NSGs Zuordnungen](#View-NSGs-associations)dargestellt.
2. Wenn die NSG alle NICs zugeordnet ist, führen Sie die `azure network nic set` Siehe [Dissociate einer NSG aus einen Netzwerkadapter](#Dissociate-an-NSG-from-a-NIC) für die einzelnen NICs. 
3. Wenn die NSG alle Subnetz zugeordnet ist, führen Sie die `azure network vnet subnet set` Siehe [Dissociate einer NSG aus einem Subnetz](#Dissociate-an-NSG-from-a-subnet) für jedes Subnetz.
4. Führen Sie zum Löschen der NSG der `Remove-AzureRmNetworkSecurityGroup` Cmdlet wie unten dargestellt.

        Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force

    >[AZURE.NOTE] Die **-Force** Parameter stellt sicher, müssen Sie nicht den Löschvorgang zu bestätigen.

## <a name="next-steps"></a>Nächste Schritte

- [Aktivieren der Protokollierung](virtual-network-nsg-manage-log.md) für NSGs.