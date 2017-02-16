<properties
   pageTitle="Bereitstellen ein virtuellen Computers mit einer statischen öffentliche IP-Adresse mithilfe einer Vorlage in Ressourcenmanager | Microsoft Azure"
   description="Weitere Informationen zum Bereitstellen virtueller Computer mit einer statischen öffentliche IP-Adresse mithilfe einer Vorlage in Ressourcenmanager"
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
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-a-template"></a>Bereitstellen eines virtuellen Computers mit einer statischen öffentliche IP-Adresse mithilfe einer Vorlage

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-resources-in-a-template-file"></a>Öffentliche IP-Ressourcen in einer Vorlagendatei

Sie können angezeigt und die [Beispielvorlage](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json)herunterladen.

Im folgenden Abschnitt zeigt die Definition der öffentlichen IP-Ressource, basierend auf dem Szenario oben.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[variables('webVMSetting').pipName]",
        "location": "[variables('location')]",
        "properties": {
          "publicIPAllocationMethod": "Static"
        },
        "tags": {
          "displayName": "PublicIPAddress - Web"
        }
      },

Beachten Sie die **PublicIPAllocationMethod** -Eigenschaft, die auf *statischen*festgelegt ist. Diese Eigenschaft kann entweder *dynamische* (Standardwert) oder *statisch*sein. Wenn es auf statischen Garantien, die die öffentliche IP-Adresse zugewiesen nie geändert wird.

Weiter unten im Abschnitt zeigt die Zuordnung der öffentlichen IP-Adresse mit einer Netzwerk-Benutzeroberfläche an.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[variables('webVMSetting').nicName]",
        "location": "[variables('location')]",
        "tags": {
          "displayName": "NetworkInterface - Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
          "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
        ],
        "properties": {
          "ipConfigurations": [
            {
              "name": "ipconfig1",
              "properties": {
                "privateIPAllocationMethod": "Static",
                "privateIPAddress": "[variables('webVMSetting').ipAddress]",
                "publicIPAddress": {
                  "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
                },
                "subnet": {
                  "id": "[variables('frontEndSubnetRef')]"
                }
              }
            }
          ]
        }
      },

Beachten Sie die **Öffentl.IP** -Eigenschaft auf die **Id** des eine Ressource mit dem Namen **Variables('webVMSetting').pipName**zeigen. Das war der Name der öffentlichen IP-Ressource abgebildet.

Schließlich wird die Schnittstelle oben in der Eigenschaft **NetworkProfile** den virtuellen Computer zu erstellenden aufgeführt.

      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Stellen Sie die Vorlage mithilfe von klicken Sie auf bereitstellen bereit.

Die Beispielvorlage verfügbar im öffentlichen Repository verwendet eine Parameterdatei, die mit der standardmäßigen Werte verwendet, um die oben beschriebenen Szenario generieren. Zum Bereitstellen dieser Vorlage klicken Sie auf bereitstellen verwenden, klicken Sie auf **Bereitstellen in Azure** in der Datei Readme.md für die [virtuellen Computer mit statischen PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) -Vorlage. Ersetzen Sie die Werte der Parameter bei Bedarf, und geben Sie Werte für die leeren Parameter.  Folgen Sie den Anweisungen im Portal zum Erstellen eines virtuellen Computers mit einer statischen öffentlichen IP-Adresse ein.

## <a name="deploy-the-template-by-using-powershell"></a>Stellen Sie die Vorlage mithilfe der PowerShell bereit.

Um die Vorlage bereitzustellen, die Sie mithilfe der PowerShell heruntergeladen haben, führen Sie die folgenden Schritte aus.

1. Wenn Sie noch keine Erfahrung Azure PowerShell haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) , und folgen Sie den Anweisungen in die Schritte 1 bis 3.

2. Führen Sie in einer PowerShell-Konsole das Cmdlet **AzureRmResourceGroup neu** , um eine neue Ressourcengruppe bei Bedarf erstellen. Wenn Sie bereits eine Ressourcengruppe erstellt haben, wechseln Sie zu Schritt 3.

        New-AzureRmResourceGroup -Name PIPTEST -Location westus

    Erwartetes Ergebnis:

        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. Führen Sie in einer PowerShell-Konsole das Cmdlet **AzureRmResourceGroupDeployment neu** , um die Vorlage bereitzustellen.

        New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
            -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
            -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json

    Erwartetes Ergebnis:

        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : <Deployment date> <Deployment time>
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0

        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         

        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Stellen Sie die Vorlage mithilfe der CLI Azure bereit.

Die Vorlage für die Bereitstellung mithilfe der Azure CLI führen Sie die folgenden Schritte aus.

1. Wenn Sie noch keine Erfahrung Azure CLI haben, führen Sie die Schritte in der [Installieren und Konfigurieren der CLI Azure](../xplat-cli-install.md) -Artikel, und klicken Sie dann die Schritte zum Verbinden der CLI für Ihr Abonnement im Abschnitt "Verwenden von Azure Login interaktiv authentifizieren" im Artikel [mit einem Azure-Abonnement aus der Azure Line Interface (CLI Azure)](../xplat-cli-connect.md) .
2. Führen Sie den Befehl **Azure Config Modus** Ressourcenmanager im Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Hier ist die erwartete Ausgabe für den oben angegebenen Befehl aus:

        info:    New mode is arm

3. Öffnen Sie die [Parameterdatei](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), und wählen Sie deren Inhalt zu einer Datei in Ihrem Computer zu speichern. In diesem Beispiel werden die Parameter in einer Datei namens *parameters.json*gespeichert. Ändern die gewünschten Parameterwerte innerhalb der Datei, falls gewünscht, aber zumindest, es empfiehlt sich, dass Sie den Wert für den Parameter AdminPassword auf einen eindeutigen, komplexe Kennwort ändern.

4. Führen Sie das Cmdlet **Bereitstellung Azure Gruppe erstellen** , um die neue VNet mithilfe der Vorlage und Parameter-Dateien heruntergeladen, und geändert, oberhalb bereitstellen. Ersetzen Sie den folgenden Befehl <path> durch den Pfad zu die Datei gespeichert haben. 

        azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json

    Erwartete Ausgabe (Listen Parameterwerte verwendet):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/<Subscription ID>/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
