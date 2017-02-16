<properties
    pageTitle="Azure Ressourcenmanager-basierten PowerShell für Azure Web App-Befehle | Microsoft Azure"
    description="Erfahren Sie, wie die neuen Ressourcenmanager Azure-basierten PowerShell-Befehlen mithilfe von Azure Web Apps verwalten."
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

# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a>Mithilfe von Azure Ressource-Manager-basierten PowerShell Azure Web Apps verwalten#

> [AZURE.SELECTOR]
- [Azure CLI](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure PowerShell](app-service-web-app-azure-resource-manager-powershell.md)

Mit Microsoft Azure PowerShell Version 1.0.0 wurden neue Befehle hinzugefügt, die dem Benutzer die Möglichkeit geben können, Ressourcenmanager Azure-basierten PowerShell-Befehle verwenden, um Web Apps verwalten.

Weitere Informationen zum Verwalten von Ressourcengruppen, finden Sie unter [Verwenden von Azure PowerShell Azure Ressourcenmanager](../powershell-azure-resource-manager.md). 

Informationen über die vollständige Liste der Parameter und Optionen für den PowerShell-Cmdlets finden Sie unter den [Vollständigen Cmdlet Verweis von Web App Azure Ressourcenmanager-basierten PowerShell-Cmdlets](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Verwalten von App-Service-Pläne ##

### <a name="create-an-app-service-plan"></a>Erstellen eines App-Serviceplan ###
Verwenden Sie zum Erstellen einer app-Serviceplan des Cmdlets **New-AzureRmAppServicePlan** ein.

Im folgenden sind Beschreibungen verschiedener Parameter:

-   **Name**: Name des der app-Serviceplan.
-   **Standort**: service Plan-Speicherort.
-   **ResourceGroupName**: Ressourcengruppe, die der neu erstellten app Serviceplan enthält.
-   **Stufe**: die gewünschten Preisgestaltung Ebene (Standardwert ist kostenlos, anderen Optionen sind freigegeben, Basic, Standard und Premium)
-   **WorkerSize**: die Größe des Kollegen (Standard ist klein, wenn der Parameter Ebene als Basic, Standard oder Premium angegeben wurde. Weitere Optionen sind Mittel und groß.)
-   **NumberofWorkers**: die Anzahl der Mitarbeiter in der app-service Plan (Standardwert ist 1). 

Beispiel mit diesem Cmdlet verwenden:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Erstellen eines App-Serviceplan in einer App-Service-Umgebung ###
Verwenden Sie zum Erstellen einer app-Serviceplan in einer app-Service-Umgebung denselben Befehl **Neu AzureRmAppServicePlan** Befehl mit zusätzlichen Parameter, um die ASE des und des ASE Ressourcengruppennamen anzugeben.

Beispiel mit diesem Cmdlet verwenden:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Wenn Sie weitere Informationen zur app-Service-Umgebung überprüfen Sie [Einführung App-Service-Umgebung](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Liste vorhandenen App Service-Pläne ###

Verwenden Sie Cmdlet " **Get-AzureRmAppServicePlan** ", um die Liste der vorhandenen app-Service-Pläne.

Zum Auflisten aller app Dienstpläne unter Ihrem Abonnement verwenden Sie zu können: 

    Get-AzureRmAppServicePlan

Zum Auflisten aller app Dienstpläne unter einer bestimmten Ressourcengruppe zu verwenden:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

Um einen bestimmten app-Serviceplan erhalten möchten, verwenden Sie ein:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Konfigurieren einer vorhandenen App Dienst planen ###

Um die Einstellungen für eine vorhandene app-Serviceplan zu ändern, verwenden Sie das Cmdlet " **Set-AzureRmAppServicePlan** " ein. Sie können die Ebene, Worker Größe und Anzahl von Arbeitskräften ändern. 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Eine App Serviceplan Skalierung ####

Zum Skalieren einer vorhandenen App Dienst planen, verwenden Sie Folgendes:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a>Ändern der Größe Worker Planen einer App-Service ####

Verwenden Sie zum Ändern der Größe des Kollegen in einer vorhandenen App Dienst planen:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a>Ändern die Ebene eines Plans für App-Dienst ####

Verwenden Sie zum Ändern der Ebene von einer vorhandenen App Dienst planen:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Löschen einer vorhandenen App Dienst planen ###

Zum Löschen einer vorhandenen app-Serviceplan müssen alle zugeordneten Web apps verschoben oder zuerst gelöscht werden. Verwenden dann das **Entfernen AzureRmAppServicePlan** Cmdlet können Sie die app-Serviceplan löschen.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Verwalten von App-Dienst Web Apps ##

### <a name="create-a-web-app"></a>Erstellen Sie eine Web-App ###

Verwenden Sie zum Erstellen einer Web app das Cmdlet " **New-AzureRmWebApp** " ein.

Im folgenden sind Beschreibungen verschiedener Parameter:

- **Name**: Namen für das Web app.
- **AppServicePlan**: Namen für den Dienstplan verwendet, um das Web app zu hosten.
- **ResourceGroupName**: Ressourcengruppe, die der App-Serviceplan hostet.
- **Standort**: der Web app-Speicherort.

Beispiel mit diesem Cmdlet verwenden:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>Erstellen Sie eine Web-App in einer App-Service-Umgebung ###

Zum Erstellen einer Web-app in einer App-Service-Umgebung (ASE). Verwenden Sie denselben Befehl **Neu-AzureRmWebApp** mit zusätzlichen Parameter, um anzugeben, den Namen ASE und den Namen der Ressource Gruppe, zu der der ASE gehört.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Wenn Sie weitere Informationen zur app-Service-Umgebung überprüfen Sie [Einführung App-Service-Umgebung](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Löschen einer vorhandenen Web App ###

So löschen Sie eine vorhandene Web app können Sie das Cmdlet **AzureRmWebApp entfernen** , müssen Sie den Namen des Web app und den Namen der Ressource Gruppe angeben.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Liste vorhandenen Web Apps ###

Verwenden Sie das Cmdlet " **Get-AzureRmWebApp** ", um die Liste der vorhandenen Web apps.

Zum Auflisten aller Web apps unter Ihrem Abonnement verwenden Sie zu können:

    Get-AzureRmWebApp

Zum Auflisten aller Web apps unter einer bestimmten Ressourcengruppe zu verwenden:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

Verwenden Sie eine bestimmte Web app, um:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Konfigurieren einer vorhandenen Web App ###

Um die Einstellungen und Konfigurationen für eine vorhandene Web app zu ändern, verwenden Sie das Cmdlet " **Set-AzureRmWebApp** " ein. Eine vollständige Liste der Parameter aktivieren Sie das [Cmdlet-Referenz-link](https://msdn.microsoft.com/library/mt652487.aspx)

Beispiel (1): Verwenden Sie dieses Cmdlet Verbindungszeichenfolgen ändern

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

Beispiel (2): Hinzufügen oder Ändern von Einstellungen für die app

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


Beispiel (3): Legen Sie die Web app auf 64-Bit-Modus ausgeführt werden

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a>Ändern des Status einer vorhandenen Web App ###

#### <a name="restart-a-web-app"></a>Starten einer Web app ####

Um eine Web app neu zu starten, müssen Sie die Namen und Ressourcen Gruppe des Web app angeben.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Beenden einer Web app ####

Um eine Web app zu beenden, müssen Sie die Namen und Ressourcen Gruppe des Web app angeben.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Starten einer Web-app ####

Um eine Web app starten, müssen Sie die Namen und Ressourcen Gruppe des Web app angeben.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Verwalten von Benutzerprofilen Web App für die Veröffentlichung ###

Jede Web app hat, die verwendet werden kann, veröffentlichen Sie Ihre apps für die Veröffentlichung Profil, mehrere Vorgänge ausgeführt werden können, klicken Sie auf Profile veröffentlichen.

#### <a name="get-publishing-profile"></a>Erste Veröffentlichung Profil ####

Verwenden Sie das Profil für die Veröffentlichung einer Web App, um:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Dieser Befehl gibt dem publishing Profil, um die Befehlszeile als auch die Ausgabe das Veröffentlichung Profil in eine Textdatei.

#### <a name="reset-publishing-profile"></a>Zurücksetzen der Veröffentlichung Profil ####

Um beide Zurücksetzen des Kennworts für die Veröffentlichung FTP und Web Bereitstellen einer Web App verwenden:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Verwalten von Zertifikaten für Web App ###

Informationen zum Verwalten von Zertifikaten für Web app finden Sie unter [SSL-Zertifikate Bindung mithilfe der PowerShell](app-service-web-app-powershell-ssl-binding.md)


### <a name="next-steps"></a>Nächste Schritte ###
- Informationen über die Unterstützung von Azure Ressourcenmanager PowerShell finden Sie unter [mithilfe von Azure PowerShell Azure Ressourcenmanager.](../powershell-azure-resource-manager.md)
- Informationen zur App-Service-Umgebungen finden Sie unter [Einführung in die App-Service-Umgebung.](app-service-app-service-environment-intro.md)
- Weitere Informationen zum Verwalten von App-Dienst SSL-Zertifikate mithilfe der PowerShell, finden Sie unter [SSL-Zertifikate Bindung mithilfe der PowerShell.](app-service-web-app-powershell-ssl-binding.md)
- Informationen über die vollständige Liste der Ressourcenmanager Azure-basierten PowerShell-Cmdlets für Azure Web Apps finden Sie unter [Azure-Cmdlet Bezug der Web Apps Azure Ressourcenmanager PowerShell-Cmdlets.](https://msdn.microsoft.com/library/mt619237.aspx)
- - Weitere Informationen zum Verwalten von App-Verwaltungsdienst CLI verwenden, finden Sie unter [Using Azure Resource Manager-Based XPlat CLI für Azure Web App](app-service-web-app-azure-resource-manager-xplat-cli.md)
