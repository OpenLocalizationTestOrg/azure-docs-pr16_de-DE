<properties
   pageTitle="Erstellen eines Internet gegenüberliegende Lastenausgleich in Ressourcenmanager mithilfe einer Vorlage | Microsoft Azure"
   description="Informationen Sie zum Erstellen eines Internet gegenüberliegende Lastenausgleich in Ressourcenmanager mithilfe einer Vorlage"
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

# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a>Erstellen eines Internet gegenüberliegende Lastenausgleich mithilfe einer Vorlage

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Modell zur Bereitstellung von Ressourcenmanager. Sie können auch [Informationen zum Erstellen eines Internet gegenüberliegende Lastenausgleich Modell zur klassischen Bereitstellung verwenden](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Stellen Sie die Vorlage mithilfe von klicken Sie auf bereitstellen bereit.

Die Beispielvorlage verfügbar im öffentlichen Repository verwendet eine Parameterdatei, die mit der standardmäßigen Werte verwendet, um die oben beschriebenen Szenario generieren. Zum Bereitstellen dieser Vorlage, klicken Sie auf bereitstellen, führen Sie [diesen Link](http://go.microsoft.com/fwlink/?LinkId=544801)verwenden, klicken Sie auf **Bereitstellen in Azure**, ersetzen Sie den Parameter Standardwerte bei Bedarf, und folgen Sie den Anweisungen im Portal.

## <a name="deploy-the-template-by-using-powershell"></a>Stellen Sie die Vorlage mithilfe der PowerShell bereit.

Um die Vorlage bereitzustellen, die Sie mithilfe der PowerShell heruntergeladen haben, führen Sie die folgenden Schritte aus.

1. Wenn Sie noch keine Erfahrung Azure PowerShell haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../../articles/powershell-install-configure.md) , und folgen Sie den Anweisungen, ganz nach Ende melden Sie sich bei Azure, und wählen Sie Ihr Abonnement.

2. Führen Sie das Cmdlet **Neu-AzureRmResourceGroupDeployment** aus, um eine Ressourcengruppe mithilfe der Vorlage erstellen.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Stellen Sie die Vorlage mithilfe der CLI Azure bereit.

Die Vorlage für die Bereitstellung mithilfe der Azure CLI führen Sie die folgenden Schritte aus.

1. Wenn Sie noch keine Erfahrung Azure CLI haben, finden Sie unter [Installieren und Konfigurieren der CLI Azure](../../articles/xplat-cli-install.md) , und folgen Sie den Anweisungen auf den Punkt, in dem Sie Ihre Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie den Befehl **Azure Config Modus** Ressourcenmanager im Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Hier ist die erwartete Ausgabe für den oben angegebenen Befehl aus:

        info:    New mode is arm

3. Im Browser navigieren Sie zu [Der Vorlage Schnellstart](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules), kopieren Sie den Inhalt der Json-Datei, und fügen Sie in einer neuen Datei in Ihrem Computer. In diesem Szenario möchten Sie die Werte unter in einer Datei namens **c:\lb\azuredeploy.parameters.json**kopieren.
4. Führen Sie das Cmdlet **Bereitstellung Azure Gruppe erstellen** , um neue Lastenausgleich mithilfe der Vorlage und Parameter-Dateien heruntergeladen, und geändert, oberhalb bereitstellen. Der Liste angezeigt, nachdem die Ausgabe wird, die Parameter verwendet erläutert.

        azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' -e 'c:\lb\azuredeploy.parameters.json'

## <a name="next-steps"></a>Nächste Schritte

[Erste Schritte zum Konfigurieren einer internen Lastenausgleich](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurieren eines laden Lastenausgleich Verteilung Modus](load-balancer-distribution-mode.md)

[Konfigurieren von Einstellungen zur im Leerlauf TCP Timeout für Ihre Lastenausgleich](load-balancer-tcp-idle-timeout.md)
