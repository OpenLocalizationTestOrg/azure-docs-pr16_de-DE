<properties
    pageTitle="Bereitstellen einer Web app MSDeploy mit Hostname und Ssl Zertifikat verwenden"
    description="Verwenden einer Vorlage Ressourcenmanager Azure bereitstellen eine Web app verwenden MSDeploy und Einrichten von benutzerdefinierten Hostname und ein Zertifikat einer Zertifizierungsstelle"
    services="app-service\web"
    manager="erikre"
    documentationCenter=""
    authors="jodehavi"
    />

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="john.dehavilland"/>

# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a>Bereitstellen einer Web app mit MSDeploy, benutzerdefinierte Hostname und SSL-Zertifikat

Dieses Handbuch führt durch Erstellen einer End-to-End-bereitstellungs für eine Azure Web App, Nutzung von MSDeploy sowie zum Hinzufügen einer benutzerdefinierten Hostname und ein SSL-Zertifikat in die Cloud-Vorlage.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Authoring Azure Ressourcenmanager Vorlagen](../resource-group-authoring-templates.md).

###<a name="create-sample-application"></a>Beispiel-Anwendung erstellen

Sie können eine ASP.NET Web-Anwendung bereitgestellt. Der erste Schritt besteht im Erstellen einer einfachen Web-Anwendungs (oder können Sie festlegen, verwenden Sie eine vorhandene, – in diesem Fall können Sie diesen Schritt überspringen).

Visual Studio 2015 zu öffnen, und wählen Sie Datei > Neues Projekt. Wählen Sie im daraufhin angezeigten Dialogfeld Web > ASP.NET Web-Anwendung. Klicken Sie unter Vorlagen wählen Sie Web, und wählen Sie die MVC-Vorlage aus. Wählen Sie den _Authentifizierungstyp ändern_ zu _Keine Authentifizierung_. Dies ist nur in der Stichprobe Anwendung so einfach wie möglich machen.

An diesem Punkt haben Sie eine einfache ASP.Net Web app bereit sind, das als Teil der Bereitstellung verwenden.

###<a name="create-msdeploy-package"></a>MSDeploy-Paket erstellen

Nächstes wird zum Erstellen des Pakets, um das Web app Azure bereitstellen. Hierzu speichern Sie das Projekt, und führen Sie Folgendes über die Befehlszeile:

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

Dadurch wird unter dem Ordner PackageLocation ein ZIP-Pakets erstellt. Die Anwendung kann nun bereitgestellt werden, die können Sie jetzt Auschecken einer Vorlage Azure Ressourcenmanager erledigen erstellen.

###<a name="create-arm-template"></a>Cloud-Vorlage erstellen
Zunächst betrachten wir zu Beginn einer Standardvorlage Cloud, die Erstellen einer Webanwendung und einem Hostinganbieter Plan (Notiz, die Parameter und Variablen aus Platzgründen nicht angezeigt werden).

    {
        "name": "[parameters('appServicePlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "apiVersion": "2014-06-01",
        "dependsOn": [ ],
        "tags": {
            "displayName": "appServicePlan"
        },
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "sku": "[parameters('appServicePlanSKU')]",
            "workerSize": "[parameters('appServicePlanWorkerSize')]",
            "numberOfWorkers": 1
        }
    },
    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        }
    }

Als Nächstes müssen Sie die Web app Ressource um eine geschachtelte MSDeploy Ressource schalten ändern. Dies ermöglicht zu Bezug Ihnen das Paket zuvor erstellten und feststellen, Azure Ressourcenmanager MSDeploy zur Bereitstellung des Pakets auf die WebApp Azure verwenden. Die nachstehende Abbildung zeigt die Microsoft.Web/sites Ressource mit geschachtelten MSDeploy Ressource aus:

    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        },
        "resources": [
            {
                "name": "MSDeploy",
                "type": "extensions",
                "location": "[resourceGroup().location]",
                "apiVersion": "2015-08-01",
                "dependsOn": [
                    "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
                ],
                "tags": {
                    "displayName": "webDeploy"
                },
                "properties": {
                    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                        "IIS Web Application Name": "[variables('webAppName')]"
                    }
                }
            }
        ]
    }

Nun werden Sie feststellen, dass die Ressource MSDeploy eine Eigenschaft **PackageUri** annimmt, die wie folgt definiert ist:

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

Diese **PackageUri** dauert dem Speicher Konto Uri, die Sie hochladen, mit dem Speicherkonto verweist, Ihre Postleitzahl Paket zu. Ressourcenmanager Azure nutzen Sie [Access Signaturen freigegeben](../storage/storage-dotnet-shared-access-signature-part-1.md) , um das Paket lokal aus dem Speicherkonto ziehen Sie nach unten, wenn Sie die Vorlage bereitstellen. Dieses Verfahren wird über eine PowerShell Skript, wird das Paket hochladen und rufen Sie die Azure Management-API zum Erstellen der Schlüssel, erforderlich, und übergeben Sie diese in der Vorlage als Parameter (*_artifactsLocation* und *_artifactsLocationSasToken*) automatische werden. Sie müssen zum Definieren von Parametern für den Ordner und Filename das Paket unter dem Speichercontainer geladen wird.

Als Nächstes müssen Sie in eine andere geschachtelte Ressource zum Einrichten der Hostname Bindungen zur Nutzung einer benutzerdefinierten Domänennamens hinzufügen. Zuerst müssen Sie sicherstellen, dass Sie der Besitzer der Hostnamens und Einrichten von Azure überprüft werden, dass Sie es - besitzen finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens in Azure-App-Dienst](web-sites-custom-domain-name.md). Danach wird, können Sie die folgenden Ihrer Vorlage unter Abschnitt Ressource Microsoft.Web/sites hinzufügen:

    {
        "apiVersion": "2015-08-01",
        "type": "hostNameBindings",
        "name": "www.yourcustomdomain.com",
        "dependsOn": [
            "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
        ],
        "properties": {
            "domainId": null,
            "hostNameType": "Verified",
            "siteName": "variables('webAppName')"
        }
    }

Schließlich müssen Sie eine andere obersten Ebene Ressource, Microsoft.Web/certificates hinzufügen. Diese Ressource enthält das SSL-Zertifikat und werden auf der gleichen Ebene wie der Web app und Hostinganbieter Plan vorhanden sind.

    {
        "name": "[parameters('certificateName')]",
        "apiVersion": "2014-04-01",
        "type": "Microsoft.Web/certificates",
        "location": "[resourceGroup().location]",
        "properties": {
            "pfxBlob": "pfx base64 blob",
            "password": "some pass"
        }
    }

Sie müssen ein gültiges SSL-Zertifikat akzeptieren, um diese Ressource eingerichtet haben. Nachdem Sie das gültige Zertifikat haben, müssen Sie die Pfx-Bytes als base64 Zeichenfolge extrahieren. Eine Möglichkeit, dies zu extrahieren besteht darin, den folgenden PowerShell-Befehl verwenden:

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

Sie könnten diese dann als Parameter zur Bereitstellung Cloud Vorlage übergeben.

An diesem Punkt ist die Cloud-Vorlage bereit.

###<a name="deploy-template"></a>Bereitstellen der Vorlage

Die letzten Schritte sind und dies überhaupt in eine vollständige End-to-End-Bereitstellung Stück. Zu vereinfachen Bereitstellung Sie das Skript **Bereitstellen-AzureResourceGroup.ps1** PowerShell nutzen können, das beim Erstellen eines Projekts Azure Ressourcengruppe in Visual Studio unterstützen Sie beim Upload von alle Elemente in der Vorlage erforderlich hinzugefügt wird. Daher müssen Sie ein Speicherkonto erstellt haben, die Sie im Voraus verwenden möchten. In diesem Beispiel habe ich ein Speicherkonto freigegebenen für die package.zip hochgeladen werden erstellt. Das Skript wird AzCopy, um das Paket mit dem Speicherkonto hochladen nutzen. Sie Ihre Ordnerspeicherort Element übergeben, und das Skript wird automatisch alle Dateien in diesem Verzeichnis auf den benannten Speichercontainer hochladen. Nach dem Aufrufen bereitstellen-AzureResourceGroup.ps1 müssen Sie dann die SSL-Bindungen zum Zuordnen der benutzerdefinierten Hostnamens mit dem SSL-Zertifikat aktualisieren.

Die folgende PowerShell zeigt die vollständige Bereitstellung der bereitstellen aufrufen-AzureResourceGroup.ps1:

    #Set resource group name
    $rgName = "Name-of-resource-group"

    #call deploy-azureresourcegroup script to deploy web app

    .\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation "East US" `
                                    -ResourceGroupName $rgName `
                                    -UploadArtifacts `
                                    -StorageAccountName "name-of-storage-acct-for-package" `
                                    -StorageAccountResourceGroupName "resource-group-name-storage-acct" `
                                    -TemplateFile "web-app-deploy.json" `
                                    -TemplateParametersFile "web-app-deploy-parameters.json" `
                                    -ArtifactStagingDirectory "C:\path\to\packagefolder\"

    #update web app to bind ssl certificate to hostname. This has to be done after creation above.

    $cert = Get-PfxCertificate -FilePath C:\path\to\certificate.pfx

    $ar = Get-AzureRmResource -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -ApiVersion 2014-11-01

    $props = $ar.Properties

    $props.HostNameSslStates[2].'SslState' = 1
    $props.HostNameSslStates[2].'thumbprint' = $cert.Thumbprint
    $props.hostNameSslStates[2].'toUpdate' = $true

    Set-AzureRmResource -ApiVersion 2014-11-01 -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -PropertyObject $props

An diesem Punkt Ihrer Anwendung sollte wurde bereitgestellt haben und Sie sollten möglicherweise über https://www.yourcustomdomain.com zu durchsuchen
