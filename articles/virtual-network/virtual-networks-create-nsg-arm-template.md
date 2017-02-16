<properties
   pageTitle="So erstellen Sie NSGs in der Cloud-Modus mithilfe einer Vorlage | Microsoft Azure"
   description="Informationen Sie zum Erstellen und Bereitstellen von NSGs in der Cloud mithilfe einer Vorlage"
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
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-using-a-template"></a>So erstellen Sie NSGs mithilfe einer Vorlage

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Modell zur Bereitstellung von Ressourcenmanager. Sie können auch [NSGs im Bereitstellungsmodell klassischen erstellen](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

## <a name="nsg-resources-in-a-template-file"></a>NSG Ressourcen in einer Vorlagendatei

Sie können angezeigt und die [Beispielvorlage](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json)herunterladen.

Im folgenden Abschnitt zeigt die Definition der front-End NSG, basierend auf dem Szenario oben.

      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('frontEndNSGName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "NSG - Front End"
      },
      "properties": {
        "securityRules": [
          {
            "name": "rdp-rule",
            "properties": {
              "description": "Allow RDP",
              "protocol": "Tcp",
              "sourcePortRange": "*",
              "destinationPortRange": "3389",
              "sourceAddressPrefix": "Internet",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 100,
              "direction": "Inbound"
            }
          },
          {
            "name": "web-rule",
            "properties": {
              "description": "Allow WEB",
              "protocol": "Tcp",
              "sourcePortRange": "*",
              "destinationPortRange": "80",
              "sourceAddressPrefix": "Internet",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 101,
              "direction": "Inbound"
            }
          }
        ]
      }

Wenn die NSG mit dem front-End-Subnetz zuordnen möchten, müssen Sie die Definition Subnetz in der Vorlage ändern, und verwenden Sie die Verweis-Id für die NSG.

        "subnets": [
          {
            "name": "[parameters('frontEndSubnetName')]",
            "properties": {
              "addressPrefix": "[parameters('frontEndSubnetPrefix')]",
              "networkSecurityGroup": {
                "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('frontEndNSGName'))]"
              }
            }
          }, ...

Beachten Sie die gleiche geleisteten für die Back-End-NSG und die Back-End-Subnetz in der Vorlage aus.

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Bereitstellen der Vorlage Cloud mithilfe klicken Sie auf bereitstellen

Die Beispielvorlage verfügbar im öffentlichen Repository verwendet eine Parameterdatei, die mit der standardmäßigen Werte verwendet, um die oben beschriebenen Szenario generieren. Zum Bereitstellen dieser Vorlage, klicken Sie auf bereitstellen, führen Sie [diesen Link](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG)verwenden, klicken Sie auf **Bereitstellen in Azure**, ersetzen Sie den Parameter Standardwerte bei Bedarf, und folgen Sie den Anweisungen im Portal.

## <a name="deploy-the-arm-template-by-using-powershell"></a>Bereitstellen der Vorlage Cloud mithilfe der PowerShell

Um die Cloud-Vorlage bereitstellen, die Sie mithilfe der PowerShell heruntergeladen haben, führen Sie die folgenden Schritte aus.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Wenn Sie noch keine Erfahrung Azure PowerShell haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) , und folgen Sie den Anweisungen, ganz nach Ende melden Sie sich bei Azure, und wählen Sie Ihr Abonnement.

3. Ausführen der **`New-AzureRmResourceGroup`** -Cmdlet zum Erstellen einer Ressourcengruppe mithilfe der Vorlage.

        New-AzureRmResourceGroup -Name TestRG -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'

    Erwartetes Ergebnis:

        ResourceGroupName : TestRG
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        Resources         :
                            Name                Type                                     Location
                            ==================  =======================================  ========
                            sqlAvSet            Microsoft.Compute/availabilitySets       westus  
                            webAvSet            Microsoft.Compute/availabilitySets       westus  
                            SQL1                Microsoft.Compute/virtualMachines        westus  
                            SQL2                Microsoft.Compute/virtualMachines        westus  
                            Web1                Microsoft.Compute/virtualMachines        westus  
                            Web2                Microsoft.Compute/virtualMachines        westus  
                            TestNICSQL1         Microsoft.Network/networkInterfaces      westus  
                            TestNICSQL2         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb1         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb2         Microsoft.Network/networkInterfaces      westus  
                            NSG-BackEnd         Microsoft.Network/networkSecurityGroups  westus  
                            NSG-FrontEnd        Microsoft.Network/networkSecurityGroups  westus  
                            TestPIPSQL1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPSQL2         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb2         Microsoft.Network/publicIPAddresses      westus  
                            TestVNet            Microsoft.Network/virtualNetworks        westus  
                            testvnetstorageprm  Microsoft.Storage/storageAccounts        westus  
                            testvnetstoragestd  Microsoft.Storage/storageAccounts        westus  

        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG

## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>Stellen Sie die Cloud-Vorlage mithilfe der CLI Azure bereit.

Gehen Sie wie folgt vor die Cloud-Vorlage für die Bereitstellung mithilfe der CLI Azure.

1. Wenn Sie noch keine Erfahrung Azure CLI haben, finden Sie unter [Installieren und Konfigurieren der CLI Azure](../xplat-cli-install.md) , und folgen Sie den Anweisungen auf den Punkt, in dem Sie Ihre Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie die **`azure config mode`** Befehl Ressourcenmanager im Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Hier ist die erwartete Ausgabe für den oben angegebenen Befehl aus:

        info:    New mode is arm

4. Ausführen der **`azure group deployment create`** -Cmdlet zum Bereitstellen von den neuen VNet mithilfe der Vorlage und Parameter-Dateien heruntergeladen und über geändert. Der Liste angezeigt, nachdem die Ausgabe wird, die Parameter verwendet erläutert.

        azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' -e 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'

    Erwartetes Ergebnis:

        info:    Executing command group create
        info:    Getting resource group TestRG
        info:    Creating resource group TestRG
        info:    Created resource group TestRG
        info:    Initializing template configurations and parameters
        info:    Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:    
        info:    group create command OK

    - **-n (oder – Name)**. Name der Ressourcengruppe erstellt werden.
    - **-l (oder – Speicherort)**. Azure Region, in die Ressourcengruppe erstellt werden soll.
    - **f-(oder – Vorlage-Datei)**. Pfad zu Ihrem Cloud-Vorlagendatei.
    - **-e (oder – Parameter-Datei)**. Der Pfad zur Datei Parameter Cloud.
