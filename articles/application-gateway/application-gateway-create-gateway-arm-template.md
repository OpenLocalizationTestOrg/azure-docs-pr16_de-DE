
<properties
   pageTitle="Erstellen Sie einen Gateway mit Azure Ressourcenmanager Vorlagen | Microsoft Azure"
   description="Diese Seite enthält Anweisungen für ein Gateway Azure-Anwendung zu erstellen, mit der Vorlage Azure Ressourcenmanager"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="create-an-application-gateway-by-using-the-azure-resource-manager-template"></a>Erstellen Sie einen Gateway mit der Vorlage Azure Ressourcenmanager

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-gateway-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-gateway-arm.md)
- [Azure klassischen PowerShell](application-gateway-create-gateway.md)
- [Azure Ressourcenmanager Vorlage](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway ist eine Ebene 7 Lastenausgleich. Es bietet Failover, Performance-routing HTTP-Anfragen zwischen verschiedenen Servern, ob sie in der Cloud oder lokal sind. Application Gateway bietet viele der Anwendung Übermittlung Controller (ADC)-Funktionen, einschließlich HTTP-Lastenausgleich, Sitzung Cookie-basierten Zugehörigkeit, Auslagern, benutzerdefinierte Gesundheit Prüfpunkte, Unterstützung für mehrere Website und viele andere Secure Sockets Layer (SSL). Eine vollständige Liste der unterstützten Funktionen finden Sie auf [Anwendung Gateway (Übersicht)](application-gateway-introduction.md)

Sie erfahren, wie Sie herunterladen und Ändern einer vorhandenen Ressourcenmanager Azure-Vorlage von GitHub und die Vorlage GitHub, PowerShell und die CLI Azure bereitstellen.

Wenn Sie einfach die Vorlage Azure Ressourcenmanager direkt aus GitHub ohne Änderungen bereitstellen, fahren Sie zum Bereitstellen einer Vorlage von GitHub.

## <a name="scenario"></a>Szenario

In diesem Szenario werden Sie folgende Aufgaben ausführen:

- Erstellen Sie einen Gateway mit zwei Instanzen aus.
- Erstellen Sie ein virtuelles Netzwerk mit dem Namen VirtualNetwork1 mit reservierte CIDR 10.0.0.0/16 einen Textblock ein.
- Erstellen Sie ein Subnetz bezeichnet Appgatewaysubnet, die 10.0.0.0/28 als deren CIDR-Block verwendet.
- Einrichten von zwei konfiguriert zuvor den Datenverkehr Back-End-IP-Adressen für die Webserver Lastenausgleich werden soll. In diesem Beispiel Vorlage werden die Back-End-IP-Adressen 10.0.1.10 und 10.0.1.11.

>[AZURE.NOTE] Diese Einstellungen sind die Parameter für diese Vorlage. Zum Anpassen der Vorlage können Sie Regeln, die Zuhörer und die SSL, der die azuredeploy.json angezeigt wird, ändern.

![Szenario](./media/application-gateway-create-gateway-arm-template/scenario.png)

## <a name="download-and-understand-the-azure-resource-manager-template"></a>Herunterladen Sie und verstehen Sie die Ressourcenmanager Azure-Vorlage

Sie können die vorhandene Ressourcenmanager Azure-Vorlage zum Erstellen eines virtuellen Netzwerks und zwei Subnetzen aus GitHub nehmen Sie alle Änderungen, die Sie möglicherweise möchten und danach wiederverwenden herunterladen. Verwenden Sie hierzu die folgenden Schritte aus:

1. Navigieren Sie zu der [Anwendungsgateway zu erstellen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-application-gateway-create).
2. Klicken Sie auf **azuredeploy.json**, und klicken Sie dann auf **Rohstoffe**.
3. Speichern Sie die Datei in einem lokalen Ordner auf Ihrem Computer.
4. Wenn Sie mit Azure Ressourcenmanager Vorlagen vertraut sind, fahren Sie mit Schritt 7.
5. Öffnen Sie die Datei, die Sie gespeichert haben, und prüfen Sie den Inhalt unter **Parameter** in Zeile 5. Azure Ressourcenmanager Vorlagenparameter Geben Sie einen Platzhalter für Werte, die während der Bereitstellung ausgefüllt werden können.

  	| Parameter | Beschreibung |
  	|---|---|
  	| **Speicherort** | Azure Region, wo das Anwendungsgateway erstellt wird |
  	| **VirtualNetwork1** | Namen für das neue virtuelle Netzwerk |
  	| **addressPrefix** | Adresse Speicherplatz für das virtuelle Netzwerk, im CIDR-format |
  	| **ApplicationGatewaysubnet** | Namen für die Anwendung Gateway Subnetz |
  	| **subnetPrefix** | Für die Anwendung Gateway Subnetz CIDR-Blocks |
  	| **SKUName** | SKU Instanzgröße |
  	| **Kapazität** | Anzahl der Instanzen |
  	| **backendaddress1** | IP-Adresse des ersten Webserver |
  	| **backendaddress2** | IP-Adresse des zweiten Webserver |


    >[AZURE.IMPORTANT] Azure Ressourcenmanager Vorlagen in GitHub verwaltet können im Laufe der Zeit ändern. Stellen Sie sicher, dass die Vorlage aktivieren, bevor Sie ihn verwenden.

6. Überprüfen Sie den Inhalt unter **Ressourcen** und beachten Sie Folgendes:

    - **Typ**. Typ der Ressource, die von der Vorlage erstellt wird. In diesem Fall ist der ein **Microsoft.Network/applicationGateways**, die einen Gateway darstellt.
    - **Namen**. Namen für die Ressource. Beachten Sie die Verwendung von **[parameters('applicationGatewayName')]**, was bedeutet, dass der Name als Eingabe von Ihnen oder einer Parameterdatei während der Bereitstellung bereitgestellt wird.
    - **Eigenschaften**. Liste der Eigenschaften für die Ressource. Mit dieser Vorlage verwendet die virtuelles Netzwerk und öffentliche IP-Adresse während der Erstellung der Anwendung-Gateway ein.

7. Navigieren Sie zu [https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-create/](https://github.com/Azure/azure-quickstart-templates/blob/master/101-application-gateway-create).
8. Klicken Sie auf **Azuredeploy-paremeters.json**, und klicken Sie dann auf **Rohstoffe**.
9. Speichern Sie die Datei in einem lokalen Ordner auf Ihrem Computer.
10. Öffnen Sie die Datei, die Sie gespeichert haben, und bearbeiten Sie die Werte für die Parameter. Verwenden Sie die folgenden Werte für die Bereitstellung des Anwendung Gateways in diesem Szenario beschrieben.

        {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        {
        "location" : {
        "value" : "West US"
        },
        "addressPrefix": {
        "value": "10.0.0.0/16"
        },
        "subnetPrefix": {
        "value": "10.0.0.0/24"
        },
        "skuName": {
        "value": "Standard_Small"
        },
        "capacity": {
        "value": 2
        },
        "backendIpAddress1": {
        "value": "10.0.1.10"
        },
        "backendIpAddress2": {
        "value": "10.0.1.11"
        }
        }

11. Speichern Sie die Datei ein. Sie können die Parameter und JSON-Vorlage mit JSON Überprüfung Onlinetools wie [JSlint.com](http://www.jslint.com/)testen.

## <a name="deploy-the-azure-resource-manager-template-by-using-powershell"></a>Bereitstellen der Vorlage Ressourcenmanager Azure mithilfe der PowerShell

Wenn Sie noch keine Erfahrung Azure PowerShell haben, finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) und folgen Sie den Anweisungen zum Melden Sie sich bei Azure, und wählen Sie Ihr Abonnement.

### <a name="step-1"></a>Schritt 1

    Login-AzureRmAccount

### <a name="step-2"></a>Schritt 2

Überprüfen Sie die Abonnements für das Konto ein.

    Get-AzureRmSubscription

Sie werden aufgefordert, Ihre Anmeldeinformationen nicht authentifiziert.

### <a name="step-3"></a>Schritt 3

Wählen Sie aus, welche Ihrer Azure Abonnements verwenden.


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Schritt 4


Erstellen Sie eine Ressourcengruppe bei Bedarf mithilfe des Cmdlets **New-AzureResourceGroup** . Im folgenden Beispiel erstellen Sie eine Ressourcengruppe mit der Bezeichnung AppgatewayRG ostasiatischen US-Speicherort aus.

    New-AzureRmResourceGroup -Name AppgatewayRG -Location "East US"

Führen Sie das Cmdlet **Neu-AzureRmResourceGroupDeployment** aus, um das neue virtuelle Netzwerk bereitstellen, mit dem vorstehenden Vorlage und Parameterdateien, die Sie heruntergeladen haben und geändert.

    New-AzureRmResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
        -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json

## <a name="deploy-the-azure-resource-manager-template-by-using-the-azure-cli"></a>Bereitstellen Sie die Vorlage Ressourcenmanager Azure mithilfe der CLI Azure

Wenn Sie die Vorlage Ressourcenmanager Azure bereitstellen, die Sie mithilfe von Azure CLI heruntergeladen haben, führen Sie die folgenden Schritte aus:

### <a name="step-1"></a>Schritt 1

Wenn Sie noch keine Erfahrung Azure CLI haben, finden Sie unter [Installieren und Konfigurieren der CLI Azure](../xplat-cli-install.md) und folgen den Anweisungen auf den Punkt, in dem Sie Ihre Azure-Konto und Ihr Abonnement auswählen.

### <a name="step-2"></a>Schritt 2

Führen Sie den Befehl **Azure Config Modus** Ressourcenmanager im Modus wechseln, wie unten dargestellt.

    azure config mode arm

Hier ist die erwartete Ausgabe für den oben angegebenen Befehl aus:

    info:   New mode is arm

### <a name="step-3"></a>Schritt 3

Falls erforderlich, führen Sie den Befehl **Azure Gruppe erstellen** zum Erstellen einer neuen Ressourcengruppe, wie unten dargestellt. Beachten Sie die Ausgabe des Befehls an. Der Liste angezeigt, nachdem die Ausgabe wird, die Parameter verwendet erläutert. Weitere Informationen zu Ressourcengruppen finden Sie auf [Azure Ressourcenmanager Übersicht](../azure-resource-manager/resource-group-overview.md).

    azure group create -n appgatewayRG -l eastus

**-n (oder – Name)**. Namen für die neue Ressourcengruppe. In diesem Szenario ist es *AppgatewayRG*.

**-l (oder – Speicherort)**. Azure Region, wo die neue Ressourcengruppe erstellt wird. In diesem Szenario ist es *Eastus*.

### <a name="step-4"></a>Schritt 4

Führen Sie das Cmdlet **Bereitstellung Azure Gruppe erstellen** , um das neue virtuelle Netzwerk mithilfe der Vorlage und Parameter-Dateien heruntergeladen, und geändert, oberhalb bereitstellen. Der Liste angezeigt, nachdem die Ausgabe wird, die Parameter verwendet erläutert.

    azure group deployment create -g appgatewayRG -n TestAppgatewayDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

## <a name="deploy-the-azure-resource-manager-template-by-using-click-to-deploy"></a>Bereitstellen der Vorlage Ressourcenmanager Azure mithilfe von klicken Sie auf bereitstellen

Klicken Sie auf bereitstellen, ist eine weitere Möglichkeit zum Verwenden von Vorlagen Azure Ressourcenmanager. Es ist eine einfache Möglichkeit zur Verwendung von Vorlagen mit dem Azure-Portal.

### <a name="step-1"></a>Schritt 1

Wechseln Sie zu [erstellen, ein Gateway mit öffentliche IP-Adresse](https://azure.microsoft.com/documentation/templates/101-application-gateway-public-ip/).

### <a name="step-2"></a>Schritt 2

Klicken Sie auf **Bereitstellen für Azure**.

![Bereitstellen für Azure](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)

### <a name="step-3"></a>Schritt 3

Füllen Sie die Parameter für die Bereitstellungsvorlage im Portal aus, und klicken Sie auf **OK**.

![Parameter](./media/application-gateway-create-gateway-arm-template/ibiza1.png)

### <a name="step-4"></a>Schritt 4

Wählen Sie die **Vertragsbedingungen** , und klicken Sie auf **kaufen**.

### <a name="step-5"></a>Schritt 5

Klicken Sie auf das benutzerdefinierte Bereitstellung Blade klicken Sie auf **Erstellen**.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie SSL-Verschiebung konfigurieren möchten, finden Sie unter [konfigurieren ein Gateway für SSL Auslagern](application-gateway-ssl.md).

Wenn Sie einen Gateway zur Verwendung mit einem internen Lastenausgleich konfigurieren möchten, finden Sie unter [erstellen ein Gateway mit einem internen Lastenausgleich (ILB)](application-gateway-ilb.md).

Wenn Sie weitere Informationen zum Laden Lastenausgleich Optionen im Allgemeinen möchten, besuchen Sie:

- [Azure Lastenausgleich](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Datenverkehr-Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
