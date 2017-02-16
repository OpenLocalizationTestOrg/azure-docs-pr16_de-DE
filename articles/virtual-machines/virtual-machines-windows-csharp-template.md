<properties
    pageTitle="Bereitstellen ein virtuellen Computers mit c# und eine Vorlage Ressourcenmanager | Microsoft Azure"
    description="Erfahren Sie, wie Sie c# und Ressourcenmanager Vorlage verwenden, um eine Azure-virtuellen Computer bereitstellen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="davidmu"/>

# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Bereitstellen einer Azure-virtuellen Computern mit c# und Ressourcenmanager Vorlage

Ressourcengruppen und Vorlagen verwenden, können Sie alle Ressourcen, die nicht trennen verwalten, die Ihrer Anwendung zu unterstützen. In diesem Artikel wird gezeigt, wie das Visual Studio und C#-Authentifizierung eingerichtet haben, erstellen Sie eine Vorlage verwenden, und klicken Sie dann bereitstellen Azure Ressourcen, die mit der Vorlage, die Sie erstellt werden können.

Zuerst müssen sicherstellen, dass dies Schritten einrichten:

- Installieren Sie [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- Überprüfen Sie die Installation von [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) oder [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855)
- Abrufen einer [Authentifizierungstoken](../resource-group-authenticate-service-principal.md)
- Erstellen einer Ressourcengruppe von [Azure PowerShell](../resource-group-template-deploy.md), [Azure CLI](../resource-group-template-deploy-cli.md)oder [Azure-Portal](../resource-group-template-deploy-portal.md)an.

Es dauert etwa 30 Minuten, führen Sie folgende Schritte aus.
    
## <a name="step-1-create-the-visual-studio-project-the-template-file-and-the-parameters-file"></a>Schritt 1: Erstellen der Visual Studio-Projekt, die Vorlagendatei und die Parameterdatei

### <a name="create-the-template-file"></a>Erstellen Sie die Vorlagendatei

Eine Vorlage Ressourcenmanager Azure ermöglicht es Ihnen das Bereitstellen und Verwalten von Azure Ressourcen zusammen. Die Vorlage ist eine Beschreibung JSON Ressourcen und zugehörigen Bereitstellungsparameter aufgelistet.

Visual Studio führen Sie folgende Schritte aus:

1. Klicken Sie auf **Datei** > **neue** > **Projekt**.

2. In **Vorlagen** > **C#-**, wählen Sie **Console-Anwendung**, geben Sie den Namen und Speicherort des Projekts, und klicken Sie dann auf **OK**.

3. Mit der rechten Maustaste im Explorer-Lösung des Namens des Projekts, klicken Sie auf **Hinzufügen** > **Neues Element**.

4. Klicken Sie auf Web, wählen Sie JSON-Datei aus, geben Sie *VirtualMachineTemplate.json* für den Namen ein, und klicken Sie dann auf **Hinzufügen**.

5. Fügen Sie in der öffnende und schließende Klammern der Datei VirtualMachineTemplate.json das Element erforderlichen Schema und die erforderlichen ContentVersion-Element hinzu:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
        }

6. [Parameter](../resource-group-authoring-templates.md#parameters) sind nicht immer erforderlich, aber sie bieten eine Möglichkeit, um Werte einzugeben, wenn die Vorlage bereitgestellt wird. Fügen Sie das Element Parameter und deren untergeordnete Elemente nach dem Element ContentVersion hinzu:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

7. In einer Vorlage können [Variablen](../resource-group-authoring-templates.md#variables) verwendet werden, um anzugeben, Werte, die häufig geändert werden können oder Werte, die aus einer Kombination von Parameterwerte erstellt werden müssen. Fügen Sie das Element Variablen nach dem Abschnitt Parameter hinzu:

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

8. [Ressourcen](../resource-group-authoring-templates.md#resources) wie des virtuellen Computers, das virtuelle Netzwerk und Speicher-Konto werden in der Vorlage weiter definiert. Fügen Sie im Ressourcenabschnitt nach dem Variablenabschnitt hinzu:

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
              "name": "myvnet1",
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
      
9. Speichern Sie die Vorlagendatei, die Sie erstellt haben.

### <a name="create-the-parameters-file"></a>Erstellen Sie die Parameterdatei

Wenn Sie Werte für die Ressourcenparameter angeben, die in der Vorlage definiert wurden, erstellen Sie eine Parameterdatei, die die Werte enthält, die verwendet werden, wenn die Vorlage bereitgestellt wird. Visual Studio führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste im Explorer-Lösung des Namens des Projekts, klicken Sie auf **Hinzufügen** > **Neues Element**.

2. Klicken Sie auf Web, wählen Sie JSON-Datei aus, geben Sie *Parameters.json* für den Namen ein, und klicken Sie dann auf **Hinzufügen**.

3. Öffnen Sie die Datei parameters.json, und fügen Sie dieses Inhaltstyps JSON:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] In diesem Artikel erstellt einen virtuellen Computer mit einer Version des Betriebssystems Windows Server. Weitere Informationen zum Auswählen von anderen Bildern finden Sie unter [Navigieren und select Azure-virtuellen Computern Bilder mit Windows PowerShell und Azure CLI](virtual-machines-linux-cli-ps-findimage.md).

4. Speichern Sie die Parameterdatei, die Sie erstellt haben.

## <a name="step-2-install-the-libraries"></a>Schritt 2: Installieren Sie die Bibliotheken

NuGet-Pakete sind die einfachste Möglichkeit, die Bibliotheken zu installieren, die Sie benötigen, um dieses Lernprogramms fertig zu stellen. Sie benötigen die Azure Management Ressourcenbibliothek und Azure Active Directory-Authentifizierung Library, um die Ressourcen zu erstellen. Um diese Bibliotheken in Visual Studio zu erhalten, führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste im Explorer-Lösung des Projektnamens, klicken Sie auf **NuGet-Pakete verwalten**, und klicken Sie dann auf Durchsuchen.

2. Typ *Active Directory* in das Suchfeld ein, klicken Sie auf **Installieren** , für das Paket Active Directory-Authentifizierungsbibliothek, und folgen Sie dann die Anweisungen zur Installation des Pakets.

4. Wählen Sie am oberen Rand der Seite **Vorabversion enthalten**. Typ *Microsoft.Azure.Management.ResourceManager* in das Suchfeld ein, klicken Sie auf **Installieren** , für die Microsoft Azure Ressource Management Bibliotheken, und folgen Sie dann die Anweisungen, um das Paket zu installieren.

Jetzt sind Sie bereit sind, verwenden die Bibliotheken zum Erstellen einer Anwendung.

## <a name="step-3-create-the-credentials-that-are-used-to-authenticate-requests"></a>Schritt 3: Erstellen der Anmeldeinformationen, mit denen Anfragen authentifizieren

Die Anwendung Azure Active Directory erstellt, und die Authentifizierungsbibliothek installiert ist. Jetzt formatieren Sie die Anwendungsinformationen in Anmeldeinformationen, die zum Authentifizieren Besprechungsanfragen in Azure-Manager verwendet werden.

1. Öffnen Sie die Datei Program.cs für das Projekt, das Sie erstellt haben, und fügen Sie diese Anweisungen an den Anfang der Datei verwenden:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Rest;
        using System.IO;

2.  Fügen Sie diese Methode zur Klasse Programm das Token abrufen, das erforderlich ist, um die Anmeldeinformationen zu erstellen:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token.");
          }
          return token;
        }

    Ersetzen Sie {Client-Id} mit dem Bezeichner des Azure Active Directory-Anwendung {Client-geheim} zusammen mit der Tastenkombination für die AD-Anwendung, und klicken Sie auf {Mandanten-Id} mit den Mandanten Bezeichner für Ihr Abonnement. Sie können die Mandanten-Id durch Ausführen der Get-AzureRmSubscription suchen. Sie können die Zugriffstaste mithilfe des Azure-Portals suchen.

3. Um die Anmeldeinformationen zu erstellen, fügen Sie diesen Code zur Methode Main in der Datei Program.cs aus:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Speichern Sie die Datei Program.cs.

## <a name="step-4-deploy-the-template"></a>Schritt 4: Bereitstellen der Vorlage

In diesem Schritt verwenden Sie die Ressourcengruppe aus, die Sie zuvor erstellt haben, aber Sie können auch eine Ressourcengruppe mithilfe der [ResourceGroup](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.models.resourcegroup.aspx) und den [ResourceManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx) Klassen erstellen.

1. Fügen Sie Variablen für die Main-Methode der Klasse Programm, um die Namen der Ressourcen, die Sie zuvor erstellt haben, den Namen für die Bereitstellung und Ihr Abonnement-ID angeben:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var deploymentName = "deployment name";

    Ersetzen Sie den Wert der Gruppenname mit dem Namen der Ressourcengruppe ein. Ersetzen Sie den Wert von DeploymentName durch den Namen, den Sie für die Bereitstellung verwenden möchten. Sie können die Abonnement-ID durch Ausführen der Get-AzureRmSubscription suchen.

2. Fügen Sie diese Methode zur Klasse Programm, um die Ressourcen der Ressourcengruppe bereitstellen, mit der Vorlage, die Sie definiert:

        public static async Task<DeploymentExtended> CreateTemplateDeploymentAsync(
          TokenCredentials credential,
          string groupName,
          string deploymentName,
          string subscriptionId)
        {
          Console.WriteLine("Creating the template deployment...");
          var deployment = new Deployment();
          deployment.Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            Template = File.ReadAllText("..\\..\\VirtualMachineTemplate.json"),
            Parameters = File.ReadAllText("..\\..\\Parameters.json")
          };
          var resourceManagementClient = new ResourceManagementClient(credential) 
            { SubscriptionId = subscriptionId };
          return await resourceManagementClient.Deployments.CreateOrUpdateAsync(
            groupName,
            deploymentName,
            deployment);
        }

    Wenn Sie die Vorlage von einem Speicherkonto bereitstellen möchten, können Sie die Template-Eigenschaft durch die Eigenschaft TemplateLink ersetzen.

3. Zum Aufrufen der Methode, die Sie soeben hinzugefügt haben, fügen Sie diesen Code zur Methode Main:

        var dpResult = CreateTemplateDeploymentAsync(
          credential,
          groupName,
          deploymentName,
          subscriptionId);
        Console.WriteLine(dpResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

## <a name="step-5-delete-the-resources"></a>Schritt 5: Löschen der Ressourcen

Da Ihnen für Ressourcen, die in Azure berechnet werden, ist es immer empfiehlt sich das Löschen von Ressourcen, die nicht mehr benötigt werden. Sie müssen nicht jede Ressource aus einer Ressourcengruppe separat zu löschen. Löschen der Ressourcengruppe und alle zugeordneten Ressourcen werden automatisch gelöscht.

1.  Zum Löschen der Ressourcengruppe fügen Sie diese Methode zur Programmklasse aus:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.  Zum Aufrufen der Methode, die Sie soeben hinzugefügt haben, fügen Sie diesen Code zur Methode Main:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

##<a name="step-6-run-the-console-application"></a>Schritt 6: Führen Sie die Anwendung Konsole

1.  Führen Sie die Console-Anwendung, klicken Sie auf in Visual Studio **Starten** , und melden Sie sich an Azure AD mit den gleichen Anmeldeinformationen, die Sie mit Ihrem Abonnement verwenden.

2.  Drücken Sie die **EINGABETASTE** , nachdem der Status akzeptiert wird angezeigt.

    Es sollte ungefähr fünf Minuten für diese Console-Anwendung vollständig vom Anfang ausführen dauern, bis z. Bevor Sie beginnen, Löschen von Ressourcen EINGABETASTE, könnten Sie einige Minuten dauern, zur Überprüfung der Erstellung der Ressourcen Azure-Portal aus, bevor Sie sie löschen.

3. Um den Status der Ressourcen anzuzeigen, wechseln Sie zu der Überwachungsprotokolle Azure-Portal:

    ![Durchsuchen von Überwachungsprotokollen Azure-Portal](./media/virtual-machines-windows-csharp-template/crpportal.png)

## <a name="next-steps"></a>Nächste Schritte

- Wenn es Probleme mit der Bereitstellung gab, wäre im nächsten Schritt eigenständig [Problembehandlung Ressource Gruppe Bereitstellungen mit Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md).
- Erfahren Sie, wie des virtuellen Computers zu verwalten, die Sie erstellt haben, indem Sie [Verwalten virtuellen Computern Azure Ressourcenmanager und PowerShell verwenden](virtual-machines-windows-csharp-manage.md).
