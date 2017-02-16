<properties
    pageTitle="Azure Ressourcenmanager-basierten Plattformen Befehlszeilentools für Azure Web App | Microsoft Azure"
    description="Erfahren Sie, wie Sie mit den neuen Ressourcenmanager Azure-basierten Plattformen Befehlszeilentools zum Verwalten Ihrer Azure Web Apps."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="aelnably"/>

# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-web-app"></a>Mithilfe von Azure Ressource-Manager-basierte XPlat CLI für Azure Web App#

> [AZURE.SELECTOR]
- [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Mit der neuen Version Microsoft Azure Plattformen Line Tools 0.10.5 wurden neue Befehle hinzugefügt. Diese Befehle gewähren Sie dem Benutzer die Möglichkeit, Ressourcenmanager Azure-basierten PowerShell-Befehle verwenden, um Web Apps verwalten.

Weitere Informationen zum Verwalten von Ressourcengruppen, finden Sie unter [der Azure CLI zum Verwalten von Azure Ressourcen und Ressourcengruppe verwenden](../xplat-cli-azure-resource-manager.md). 


## <a name="managing-app-service-plans"></a>Verwalten von App-Service-Pläne ##

### <a name="create-an-app-service-plan"></a>Erstellen eines App-Serviceplan ###
Verwenden Sie zum Erstellen einer app-Serviceplan des Befehls **Azure Appserviceplan erstellen** ein.

Im folgenden sind Beschreibungen verschiedener Parameter:

-   **– Ressourcengruppe**: Ressourcengruppe, die der neu erstellten app Serviceplan enthält.
-   **– Name**: von der app-Serviceplan Namen.
-   **– Speicherort**: app-Plan Speicherort.
-   **– Ebene**: die gewünschten Preisgestaltung Sku (die Optionen sind: F1 (kostenlos). D1 (freigegeben). B1 (grundlegende klein), B2 (grundlegende Mittel) und B3 (Basic großen). S1 (Standard klein), S2 (Standard Mittel) und S3 (Standard groß). P1 (Premium klein), P2 (Premium Mittel), und P3 (Premium großen).)
-   **– Instanzen**: die Anzahl der Mitarbeiter in der app-service Plan (Standardwert ist 1).

Beispiel mit diesem Cmdlet verwenden:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

### <a name="list-existing-app-service-plans"></a>Liste vorhandenen App Service-Pläne ###

Verwenden Sie Befehl **Azure Appserviceplan Liste** , um die Liste der vorhandenen app-Service-Pläne.

Zum Auflisten aller app Dienstpläne unter einer bestimmten Ressourcengruppe zu verwenden:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

Um einen bestimmten app-Serviceplan zu gelangen, verwenden Sie Befehl **Azure Appserviceplan anzeigen** :

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Konfigurieren einer vorhandenen App Dienst planen ###

Um die Einstellungen für eine vorhandene app-Serviceplan zu ändern, verwenden Sie den Befehl **Azure Appserviceplan Config** . Sie können die Sku, und die Anzahl der Worker ändern. 

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Eine App Serviceplan Skalierung ####

Zum Skalieren einer vorhandenen App Dienst planen, verwenden Sie Folgendes:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a>Ändern der SKUs eines Plans für App-Dienst ####

Zum Ändern einer vorhandenen App planen Service Sku verwenden Sie zu können:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Löschen einer vorhandenen App Dienst planen ###

Zum Löschen einer vorhandenen app-Serviceplan müssen alle zugeordneten Web apps verschoben oder zuerst gelöscht werden. Klicken Sie dann Befehls des **Azure Webapp löschen** , dass Sie die app-Serviceplan löschen können.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-web-apps"></a>Verwalten von App-Dienst Web Apps ##

### <a name="create-a-web-app"></a>Erstellen Sie eine Web-App ###

Zum Erstellen einer Web app verwenden Sie den Befehl **Azure Webapp erstellen** .

Im folgenden sind Beschreibungen verschiedener Parameter:

- **– Name**: Namen für das Web app.
- **– Plan**: Namen für den Dienstplan verwendet, um das Web app zu hosten.
- **– Ressourcengruppe**: Ressourcengruppe, die der App-Serviceplan hostet.
- **– Speicherort**: die Web app-Speicherort.

Beispiel mit diesem Cmdlet verwenden:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-web-app"></a>Löschen einer vorhandenen Web App ###

So löschen Sie eine vorhandene Web app können Sie den Befehl **Löschen Azure Webapp** verwenden, müssen Sie den Namen des Web app und den Namen der Ressource Gruppe angeben.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-web-apps"></a>Liste vorhandenen Web Apps ###

Um die Liste der vorhandenen Web apps können verwenden Sie den Befehl **Azure Webapp Liste** .

Zum Auflisten aller Web apps unter einer bestimmten Ressourcengruppe zu verwenden:

    azure webapp list --resource-group ContosoAzureResourceGroup

Verwenden Sie eine bestimmte Web app, um den Befehl **Azure Webapp anzeigen** .

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-web-app"></a>Konfigurieren einer vorhandenen Web App ###

Um die Einstellungen und Konfigurationen für eine vorhandene Web app zu ändern, verwenden Sie den Befehl **set Config Azure Webapp** .

Beispiel (1): Ändern der Php Version einer Web App 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

Beispiel (2): Hinzufügen oder Ändern von Einstellungen für die app

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

Wissen, was andere Konfiguration werden kann geändert, mithilfe des Befehls **Azure Webapp Config set -h** .

### <a name="change-the-state-of-an-existing-web-app"></a>Ändern des Status einer vorhandenen Web App ###

#### <a name="restart-a-web-app"></a>Starten einer Web app ####

Um eine Web app neu zu starten, müssen Sie die Namen und Ressourcen Gruppe des Web app angeben.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Beenden einer Web app ####

Um eine Web app zu beenden, müssen Sie die Namen und Ressourcen Gruppe des Web app angeben.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Starten einer Web-app ####

Um eine Web app starten, müssen Sie die Namen und Ressourcen Gruppe des Web app angeben.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Verwalten von Benutzerprofilen Web App für die Veröffentlichung ###

Jede Web app enthält ein publishing Profil, das zum Veröffentlichen von Ihrer apps verwendet werden kann.

#### <a name="get-publishing-profile"></a>Erste Veröffentlichung Profil ####

Verwenden Sie das Profil für die Veröffentlichung einer Web App, um:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

Dieser Befehl gibt die Veröffentlichung Profil-Benutzernamen und Ihr Kennwort an die Befehlszeile.

### <a name="manage-web-app-hostnames"></a>Verwalten von Hostnamen Web App ###

Zum Verwalten von Hostname Bindungen für Ihre Web app verwenden Sie den Befehl **Azure Webapp Config Hostnamen**  

#### <a name="list-hostname-bindings"></a>Liste Hostname Bindungen ####

Zum Abrufen der aktuellen Hostname Bindungen für eine Web app verwenden Sie zu können:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Hostname Bindungen hinzufügen ####

Verwenden Sie, um eine Web app Hostname Bindungen hinzuzufügen:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Hostname Bindungen löschen ####

Zum Löschen von Hostname Bindungen verwenden:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

### <a name="next-steps"></a>Nächste Schritte ###
- Informationen über die Unterstützung von Azure Ressourcenmanager CLI finden Sie unter [mit dem Azure Ressourcen und Ressourcengruppe Verwalten der CLI Azure.](../xplat-cli-azure-resource-manager.md)
- Weitere Informationen zum Verwalten von App-Verwaltungsdienst mithilfe der PowerShell, finden Sie unter [Using Azure Resource Manager-Based PowerShell zum Verwalten von Azure Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)
