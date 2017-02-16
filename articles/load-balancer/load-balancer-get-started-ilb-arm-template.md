<properties
   pageTitle="Erstellen Sie eine interne Lastenausgleich mithilfe einer Vorlage in Ressourcenmanager | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer internen Lastenausgleich mithilfe einer Vorlage in Ressourcenmanager"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-using-a-template"></a>Erstellen Sie eine interne Lastenausgleich mithilfe einer Vorlage

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Stellen Sie die Vorlage mithilfe von klicken Sie auf bereitstellen bereit.

Die Beispielvorlage verfügbar im öffentlichen Repository verwendet eine Parameterdatei, die mit der standardmäßigen Werte verwendet, um die oben beschriebenen Szenario generieren. Zum Bereitstellen dieser Vorlage, klicken Sie auf bereitstellen, führen Sie [diesen Link](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer)verwenden, klicken Sie auf **Bereitstellen in Azure**, ersetzen Sie den Parameter Standardwerte bei Bedarf, und folgen Sie den Anweisungen im Portal.

## <a name="deploy-the-template-by-using-powershell"></a>Stellen Sie die Vorlage mithilfe der PowerShell bereit.

Um die Vorlage bereitzustellen, die Sie mithilfe der PowerShell heruntergeladen haben, führen Sie die folgenden Schritte aus.

1. Wenn Sie noch keine Erfahrung Azure PowerShell haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../../articles/powershell-install-configure.md) , und folgen Sie den Anweisungen, ganz nach Ende melden Sie sich bei Azure, und wählen Sie Ihr Abonnement.
2. Die Parameterdatei auf die lokale Festplatte herunterladen.
3. Bearbeiten Sie die Datei, und speichern Sie es.
4. Führen Sie das Cmdlet **Neu-AzureRmResourceGroupDeployment** aus, um eine Ressourcengruppe mithilfe der Vorlage erstellen.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
            -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Stellen Sie die Vorlage mithilfe der CLI Azure bereit.

Die Vorlage für die Bereitstellung mithilfe der Azure CLI führen Sie die folgenden Schritte aus.

1. Wenn Sie noch keine Erfahrung Azure CLI haben, finden Sie unter [Installieren und Konfigurieren der CLI Azure](../../articles/xplat-cli-install.md) , und folgen Sie den Anweisungen auf den Punkt, in dem Sie Ihre Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie den Befehl **Azure Config Modus** Ressourcenmanager im Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Hier ist die erwartete Ausgabe für den oben angegebenen Befehl aus:

        info:    New mode is arm

3. Öffnen Sie die Parameterdatei, und wählen Sie deren Inhalt zu einer Datei in Ihrem Computer zu speichern. In diesem Beispiel wir die Parameterdatei *parameters.json*gespeichert.

4. Führen Sie den Befehl **Bereitstellung Azure Gruppe erstellen** , Bereitstellen von neuen internen Lastenausgleich mithilfe der Vorlage und Parameter-Dateien, die Sie heruntergeladen haben und über geändert. Der Liste angezeigt, nachdem die Ausgabe wird, die Parameter verwendet erläutert.

        azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren eines Quelle IP-Zugehörigkeit mit laden Lastenausgleich Verteilung-Modus](load-balancer-distribution-mode.md)

[Konfigurieren von Einstellungen zur im Leerlauf TCP Timeout für Ihre Lastenausgleich](load-balancer-tcp-idle-timeout.md)



