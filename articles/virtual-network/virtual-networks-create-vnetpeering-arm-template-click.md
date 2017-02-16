<properties
   pageTitle="VNet Peering mit Ressourcenmanager Vorlagen erstellen | Microsoft Azure"
   description="Informationen Sie zum Erstellen eines virtuellen Netzwerks peering mithilfe der Vorlagen in Ressourcenmanager."
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-vnet-peering-using-resource-manager-templates"></a>Erstellen von VNet Peering Ressourcenmanager Vorlagen verwenden

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Zum Erstellen einer VNet mithilfe von Vorlagen Ressourcenmanager peering führen Sie die folgenden Schritte aus:

1. Wenn Sie noch keine Erfahrung Azure PowerShell haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) , und folgen Sie den Anweisungen, ganz nach Ende melden Sie sich bei Azure, und wählen Sie Ihr Abonnement.

    > [AZURE.NOTE] Der PowerShell-Cmdlets für die Verwaltung von VNet peering ist im Lieferumfang [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Der nachfolgende Text werden die Definition von einer Peeringverbindung VNet für VNet1, VNet2, basierend auf dem Szenario oben. Kopieren Sie den Inhalt unten, und klicken Sie auf eine Datei namens VNetPeeringVNet1.json gespeichert.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }

3. Weiter unten im Abschnitt werden die Definition von einer Peeringverbindung VNet für VNet2, VNet1, basierend auf dem Szenario oben.  Kopieren Sie den Inhalt unten, und klicken Sie auf eine Datei namens VNetPeeringVNet2.json gespeichert.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }

    Wie in der vorstehenden Vorlage gesehen, gibt es ein paar konfigurierbare Eigenschaften für VNet peering:

  	|Option|Beschreibung|Standard|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Unabhängig davon, ob der Adresse Speicherplatz einen Peer VNet in der Kategorie Virtual_network enthalten ist.|Ja|
  	|AllowForwardedTraffic|Gibt an, ob Verkehr nicht von einem hervorragendem VNet akzeptiert oder gelöscht wird.|Nein|
  	|AllowGatewayTransit|Ermöglicht den Peer VNet Schlüsselaufgaben VNet verwenden.|Nein|
  	|UseRemoteGateways|Verwenden der Peer des VNet Gateway. Der Peer VNet muss einen Gateway konfiguriert und AllowGatewayTransit ausgewählt. Sie können diese Option nicht verwenden, wenn Sie Gateway konfiguriert haben.|Nein|

    Jede Verknüpfung im VNet peering verfügt über die Eigenschaften oben. Sie können beispielsweise AllowVirtualNetworkAccess auf True für VNet Peeringverbindung VNet1 VNet2 festgelegt werden und für die VNet Peeringverbindung in die andere Richtung False festlegen.


4. Wenn Sie die Vorlagendatei bereitstellen, können Sie das Cmdlet "New-AzureRmResourceGroupDeployment" zum Erstellen oder aktualisieren die Bereitstellung ausführen. Weitere Informationen zur Verwendung von Vorlagen Ressourcenmanager Lizenzinformationen finden Sie in diesem [Artikel](../resource-group-template-deploy.md).

        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all

    > [AZURE.NOTE] Ersetzen Sie die Gruppe Name und Vorlage Ressourcendatei je nach Bedarf.

    Unter Beispiel oben beschriebenen Szenario basiert:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all

    Die Ausgabe zeigt:

        DeploymentName      : VNetPeeringVNet1
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all

    Die Ausgabe zeigt:

        DeploymentName      : VNetPeeringVNet2
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

5. Nach Abschluss die Bereitstellung können Sie das Cmdlet unten, um den Status Peeringliste anzeigen ausführen:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2

    Die Ausgabe zeigt:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : VNet101
        VirtualNetworkName  : VNet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Nachdem peering in diesem Szenario eingerichtet wurde, sollten Sie die Verbindungen aus einem beliebigen virtuellen Computers zu einem beliebigen virtuellen Computern in beiden VNets einleiten sein. Standardmäßig AllowVirtualNetworkAccess wahr ist, und VNet peering stellt die richtigen ACLs zum Zulassen der Kommunikation zwischen VNets bereit. Sie können weiterhin Netzwerk Sicherheit Gruppe (NSG) Regeln, um die Verbindung zwischen bestimmten Subnets oder virtuellen Computern die genaue Kontrolle von Access zwischen zwei virtuelle Netzwerke blockieren anwenden.  Weitere Informationen zum Erstellen von Regeln NSG von Lizenzinformationen finden Sie in diesem [Artikel](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Um eine VNet über Abonnements peering zu erstellen, führen Sie die folgenden Schritte aus:

1. Anmelden bei Azure mit Berechtigungen A-Benutzer des Kontos in A-Abonnement, und führen Sie das folgende Cmdlet aus:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

    Dies ist nicht erforderlich, peering hergestellt werden, auch wenn Benutzer einzeln Peeringliste Anfragen für ihre jeweiligen Vnets heraufstufen, solange die Anforderungen entsprechen. Hinzufügen eines Benutzers Berechtigungen von den anderen VNet als Benutzer in der lokalen VNet vereinfacht die Einrichtung führen.

2. Melden Sie sich bei Azure mit berechtigten Benutzer-B-Konto für Abonnement-B, und führen Sie das folgende Cmdlet aus:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. In s Benutzer-A-Sitzung, die mit diesem Cmdlet ausführen:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all

    So sieht wie die JSON-Datei definiert ist.  

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }

4. Führen Sie in der Benutzer-B-Sitzung das folgende Cmdlet aus:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all

    So sieht wie die JSON-Datei definiert wird:

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }

    Nachdem peering in diesem Szenario eingerichtet wurde, sollten Sie die Verbindungen aus einem beliebigen virtuellen Computers zu einem beliebigen virtuellen Computern der beiden VNets über anderen Abonnements initiieren sein.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. In diesem Szenario können Sie die Beispielvorlage Herstellen der VNet peering bereitstellen.  Sie müssen zum Festlegen der AllowForwardedTraffic-Eigenschaft auf True, wodurch die virtuelle Netzwerk-Anwendung in der hervorragendem VNet senden und Empfangen von Datenverkehr.

    Hier sind die Vorlage zum Erstellen einer VNet von HubVNet zu VNet1 peering aus. Beachten Sie, dass AllowForwardedTraffic auf False festgelegt ist.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }

2. Hier sind die Vorlage zum Erstellen einer VNet von VNet1 zu HubVnet peering aus. Beachten Sie, dass AllowForwardedTraffic ist true.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }


3. Nachdem peering hergestellt wurde, finden Sie in diesem [Artikel](virtual-network-create-udr-arm-ps.md) zum Definieren von User defined routes(UDR) umgeleitet VNet1 Verkehr über eine virtuelle Einheit seine Funktionen verwenden. Wenn Sie die Adresse des nächste Abschnitts im Routing angeben, können Sie die IP-Adresse des virtuellen Geräts in der Peer VNet HubVNet festlegen.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Um eine peering zwischen virtuelle Netzwerke aus verschiedenen Bereitstellungsmodellen zu erstellen, führen Sie die folgenden Schritte aus:
1. Der Text wird unten zeigt die Definition eines VNet Peeringverbindung für VNET1 zu VNET2 in diesem Szenario. Nur einen einzigen Link ist ein klassisches virtuelles Netzwerk mit einem Azure Ressource-Manager virtuelle Netzwerk peer erforderlich.

    Achten Sie darauf, dass Sie in Ihrem Abonnement-ID für die, in der klassischen virtuelles Netzwerk oder VNET2 befindet setzen und ändern Sie MyResouceGroup auf den Namen der entsprechenden Ressource.

    {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "ContentVersion": "1.0.0.0", "Parameter": {}, "Variablen": {}, "Ressourcen": [{"ApiVersion": "2016-06-01", "Typ": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings", "name": "VNET1/LinkToVNET2", "Speicherort": "[ResourceGroup () .location]", "Eigenschaften": {"AllowVirtualNetworkAccess": WAHR, "AllowForwardedTraffic": false "AllowGatewayTransit": false "UseRemoteGateways": false "RemoteVirtualNetwork": {"Id": "[ResourceId (' Microsoft.ClassicNetwork/VirtualNetworks', 'VNET2')]"}}}]}

2. Um die Vorlagendatei bereitzustellen, führen Sie das folgende Cmdlet zum Erstellen oder aktualisieren die Bereitstellung.

        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all

        Output shows:

        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

3. Nachdem die Bereitstellung hergestellt wurde, können Sie das folgende Cmdlet aus, um den Peeringliste Zustand anzuzeigen ausführen:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2

        Output shows:

        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

Nachdem peering zwischen einer klassischen VNet und einem Ressourcenmanager VNet eingerichtet wurde, sollten Sie von jedem virtuellen Computer in VNET1 Verbindungen, um alle virtuellen Computern VNET2 und umgekehrt initiieren sein.
