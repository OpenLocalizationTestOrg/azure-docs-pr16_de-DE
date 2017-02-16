<properties
    pageTitle="Erstellen ein virtuellen Computers mit einer Vorlage Ressourcenmanager | Microsoft Azure"
    description="Verwenden einer Vorlage Ressourcenmanager und PowerShell auf einfache Weise einen neuen Windows virtuellen Computer erstellen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-with-a-resource-manager-template"></a>Erstellen von einem Windows-Computer mit einer Vorlage Ressourcenmanager

Dieser Artikel bietet Ihnen einen Einstieg zu einer Vorlage Azure Ressourcenmanager und zeigt, wie Sie PowerShell verwenden, um diese bereitstellen. Die Vorlage wird einer einzelnen virtuellen Computern unter Windows Server in einem neuen virtuellen Netzwerk mit einem einzelnen Subnetz bereitgestellt.

Es sollte ungefähr 20 Minuten dauern, die Schritte in diesem Artikel ausführen.

> [AZURE.IMPORTANT] Wenn Sie Ihre virtuellen Computer Teil einer Verfügbarkeit festlegen möchten, fügen Sie dieses zum Satz, wenn Sie den virtuellen Computer erstellen. Gibt es zurzeit keine Möglichkeit zum Hinzufügen eines virtuellen Computers zu einer Verfügbarkeit festzulegen, nachdem er erstellt wurde.

## <a name="step-1-create-the-template-file"></a>Schritt 1: Erstellen der Vorlagendatei

Sie können eine eigenen Vorlage mithilfe der Informationen in [Azure Ressourcenmanager Authoring-Vorlagen](../resource-group-authoring-templates.md)erstellen. Sie können auch Vorlagen bereitstellen, die auf Grundlage von [Azure Schnellstart Vorlagen](https://azure.microsoft.com/documentation/templates/)für Sie erstellt wurden.

1. Öffnen Sie Ihre bevorzugten Texteditor und fügen Sie der erforderlichen Schemaelement und die erforderlichen ContentVersion-Element hinzu:

        {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
        }
2. [Parameter](../resource-group-authoring-templates.md#parameters) sind nicht immer erforderlich, aber sie bieten eine Möglichkeit, um Werte einzugeben, wenn die Vorlage bereitgestellt wird. Fügen Sie das Element Parameter und deren untergeordnete Elemente nach dem Element ContentVersion hinzu:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

3. In einer Vorlage können [Variablen](../resource-group-authoring-templates.md#variables) verwendet werden, um anzugeben, Werte, die häufig geändert werden können oder Werte, die aus einer Kombination von Parameterwerte erstellt werden müssen. Fügen Sie das Element Variablen nach dem Abschnitt Parameter hinzu:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }
        
4. [Ressourcen](../resource-group-authoring-templates.md#resources) wie des virtuellen Computers, das virtuelle Netzwerk und Speicher-Konto werden in der Vorlage weiter definiert. Fügen Sie im Ressourcenabschnitt nach dem Variablenabschnitt hinzu:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvn1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
          
    >[AZURE.NOTE] In diesem Artikel erstellt einen virtuellen Computer mit einer Version des Betriebssystems Windows Server. Weitere Informationen zum Auswählen von anderen Bildern finden Sie unter [Navigieren und select Azure-virtuellen Computern Bilder mit Windows PowerShell und Azure CLI](virtual-machines-linux-cli-ps-findimage.md).  
            
2. Speichern Sie die Vorlagendatei als *VirtualMachineTemplate.json*ein.

## <a name="step-2-create-the-parameters-file"></a>Schritt 2: Erstellen der Parameterdatei

Wenn Sie Werte für die Ressourcenparameter angeben, die in der Vorlage definiert wurden, erstellen Sie eine Parameterdatei, die die Werte enthält, die verwendet werden, wenn die Vorlage bereitgestellt wird.

1. Kopieren Sie im Text-Editor eine neue Datei mit *Parameters.json*dieses Inhaltstyps JSON:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Finden Sie weitere Informationen zu [Benutzername und Kennwort Anforderungen](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm).

2. Speichern Sie die Parameterdatei ein.

## <a name="step-3-install-azure-powershell"></a>Schritt 3: Installieren Sie Azure PowerShell

Informationen zum Installieren der neuesten Version von Azure PowerShell, Ihr Abonnement auswählen, und melden Sie sich bei Ihrem Konto finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) .

## <a name="step-4-create-a-resource-group"></a>Schritt 4: Erstellen einer Ressourcengruppe

In einer [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md)müssen alle Ressourcen bereitgestellt werden.

1. Erhalten Sie eine Liste der verfügbaren Speicherorte aus, in dem Ressourcen erstellt werden können.

        Get-AzureRmLocation | sort DisplayName | Select DisplayName

2. Ersetzen Sie den Wert von **$locName** durch einen Speicherort aus der Liste, beispielsweise **Zentralen US**ein. Erstellen Sie die Variable ein.

        $locName = "location name"
        
3. Ersetzen Sie den Wert von **$rgName** mit dem Namen der neuen Ressourcengruppe ein. Erstellen der Variablen und der Ressourcengruppe.

        $rgName = "resource group name"
        New-AzureRmResourceGroup -Name $rgName -Location $locName
        
    Sie sollten ungefähr wie im folgenden Beispiel sehen:
    
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{subscription-id}/resourceGroups/myrg1

## <a name="step-5-create-the-resources-with-the-template-and-parameters"></a>Schritt 5: Erstellen der Ressourcen mit der Vorlage und Parameter

Ersetzen Sie den Wert von **$templateFile** durch den Pfad und den Namen der Vorlagendatei an. Ersetzen Sie den Wert von **$parameterFile** durch den Pfad und den Namen der Parameterdatei ein. Die Variablen erstellen, und klicken Sie dann die Vorlage bereitstellen. 

        $templateFile = "template file"
        $parameterFile = "parameter file"
        New-AzureRmResourceGroupDeployment -ResourceGroupName $rgName -TemplateFile $templateFile -TemplateParameterFile $parameterFile

    You will see something like this:

        DeploymentName    : VirtualMachineTemplate
        ResourceGroupName : myrg1
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2016 8:11:37 PM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            adminUsername    String                     mytestacct1
                            adminPassword    SecureString

        Outputs           :

>[AZURE.NOTE] Sie können auch Vorlagen und Parameter von einem Konto Azure-Speicher bereitstellen. Weitere Informationen finden Sie unter [Verwenden von Azure PowerShell mit Azure-Speicher](../storage/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Nächste Schritte

- Falls Probleme mit der Bereitstellung aufgeführt sind, wäre im nächsten Schritt die [Problembehandlung Ressource Gruppe Bereitstellungen mit Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md) eigenständig
- Erfahren Sie, wie des virtuellen Computers zu verwalten, die Sie erstellt haben, indem Sie [Verwalten virtuellen Computern Azure Ressourcenmanager und PowerShell verwenden](virtual-machines-windows-ps-manage.md).
