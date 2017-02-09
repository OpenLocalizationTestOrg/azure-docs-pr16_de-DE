<properties
    pageTitle="Web App Klonen mithilfe der PowerShell"
    description="Erfahren Sie, wie Ihre Web Apps neue Web Apps mithilfe der PowerShell klonen."
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
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-powershell"></a>Mithilfe der PowerShell Klonen Azure App-Verwaltungsdienst-App#

Mit der Veröffentlichung von Microsoft Azure PowerShell Version 1.1.0 wurde eine neue Option zu neu-AzureRMWebApp hinzugefügt, die dem Benutzer zu einer vorhandenen Web-App zu einer neu erstellten app in einem anderen Bereich oder in der gleichen Region Klonen gewähren möchten. Dadurch werden die Kunden, die eine Reihe von apps in verschiedener Regionen schnell und einfach bereitstellen.

Klonen App gibt es zurzeit unterstützt nur Premium Ebene app-Service-Pläne. Das neue Feature verwendet die gleichen Einschränkungen als Web Apps Sicherung Feature, finden Sie unter [einer Web app im App-Verwaltungsdienst Azure sichern](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Um weitere Informationen zur Verwendung Azure Ressourcenmanager Azure PowerShell basierend auf Überprüfen Cmdlets für die Verwaltung von Web Apps [Azure Ressourcenmanager Grundlage PowerShell-Befehlen für Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="cloning-an-existing-app"></a>Klonen einer vorhandenen App ##

Szenario: Einer vorhandenen Web-app in der Region Süden zentralen uns der Benutzer möchte den Inhalt aus, um eine neue Web-app in der Region US North Central klonen. Dies kann mithilfe der Ressourcenmanager Azure-Version des PowerShell-Cmdlets zum Erstellen einer neuen Web-app mit der Option - SourceWebApp erfolgen.

Kennen den Namen der Ressource-Gruppe, der die Quelle Web app enthält, können wir den folgenden PowerShell-Befehl Sie um der Quelle Web-app Informationen (in diesem Fall Quelle-Webapp genannt) zu erhalten:

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Zum Erstellen einer neuen App Service planen, können wir neu AzureRmAppServicePlan Befehl wie im folgenden Beispiel verwenden.

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Mithilfe des Befehls New-AzureRmWebApp können wir erstellen das neue Web-app in der Region US North Central und Einbindung an einer vorhandenen Premium Schicht App Dienst planen, darüber hinaus können wir verwenden derselben Ressourcengruppe als die Quelle Web app oder eine neue Ressourcengruppe definieren, das folgende Beispiel veranschaulicht, die:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

Zu eine vorhandenen Web-app, einschließlich aller zugehörigen Bereitstellung Klonen Steckplätze, benötigt der Benutzer den IncludeSourceWebAppSlots-Parameter verwenden, der folgende PowerShell-Befehl veranschaulicht die Verwendung dieses Parameters mit dem Befehl neu-AzureRmWebApp:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots $true

Um eine vorhandene Web app innerhalb derselben Region klonen, zum Erstellen einer neuen Ressourcengruppe und eine neue app-Verwaltungsdienst der Benutzer muss in der gleichen Region planen und dann mit dem folgenden PowerShell-Befehl Klonen des Web app

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonen einer vorhandenen App zu einer App-Service-Umgebung ##

Szenario: Einer vorhandenen Web-app in der Region Süden zentralen uns der Benutzer möchte den Inhalt aus, um eine neue Web-app zu einer vorhandenen App Dienst Umgebung (ASE) Klonen.

Kennen den Namen der Ressource-Gruppe, der die Quelle Web app enthält, können wir den folgenden PowerShell-Befehl Sie um der Quelle Web-app Informationen (in diesem Fall Quelle-Webapp genannt) zu erhalten:

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Die ASE den Namen und den Namen der Ressource Gruppe, zu der der ASE gehört bekannt sein, der Benutzer kann mithilfe des Befehls New-AzureRmWebApp das neuen Web app in den vorhandenen ASE erstellen, das folgende Beispiel veranschaulicht, die:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

Der Parameter Speicherort legacy Grund erforderlich ist, aber bei der Erstellung einer app in einer ASE wird ignoriert. 

## <a name="cloning-an-existing-app-slot"></a>Klonen einer vorhandenen App Slot ##

Szenario: Der Benutzer möchte einen vorhandenen Web App Slot, um entweder eine neue Web App oder einen neuen Web App Slot klonen. Die neue Web-App kann in derselben Region als den ursprünglichen Web App Slot oder in einem anderen Bereich.

Kennen den Namen der Ressource-Gruppe, der die Quelle Web app enthält, können wir den folgenden PowerShell-Befehl Sie der Quelle Web app Slot Informationen (in diesem Fall Quelle-Webappslot bezeichnet) zu Web App Quelle-Webapp verknüpft:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

Das folgende Beispiel veranschaulicht, erstellen eine datenbeschriftungsreihe von der Quelle Web app eine neue Web app:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Konfigurieren von Datenverkehr Manager während eine App Klonen ##

Mit mehreren Region Web apps erstellen und Konfigurieren von Azure Datenverkehr Manager, um den Datenverkehr an alle diese Web apps weiterleiten, ist ein n wichtiger Punkt, um sicherzustellen, dass Kunden apps sind hochgradig verfügbar, wenn Klonen eine vorhandenen Web app müssen Sie die Option zum Herstellen einer Verbindung entweder ein neues Profil der Datenverkehr Manager oder ein vorhandenes mit beide Web apps - Beachten Sie, dass nur Azure Ressourcenmanager Version von Datenverkehr Manager unterstützt wird.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Erstellen ein neues Profil für den Datenverkehr Manager während eine App Klonen ###

Szenario: Der Benutzer möchte Klonen eine Web-app zu einer anderen Region beim Konfigurieren eines Azure Ressourcenmanager Datenverkehr-Manager-Profil, die beide Web apps enthalten. Das folgende Beispiel veranschaulicht, erstellen eine datenbeschriftungsreihe von der Quelle Web app eine neue Web app beim Konfigurieren eines neuen Datenverkehr Manager Profils:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Hinzufügen von neuen Klonen Web App erstellt ein vorhandenes Datenverkehr-Manager-Profil ###

Szenario: Der Benutzer bereits ein Azure Ressourcenmanager Datenverkehr-Manager-Profil, das er beide Web apps als Endpunkte hinzufügen möchten. Dazu müssen Sie zuerst die vorhandene Datenverkehr Manager Profil-Id zusammenstellen und benötigen wir die Abonnement-Id, Ressourcengruppennamen und der vorhandenen Profilnamen Datenverkehr-Manager.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Veranschaulicht, nachdem ich die Datenverkehr-Manager-Id, die folgenden einer Klonen der Quelle Web app zu einer neuen Web-app erstellen, während ein vorhandenes Datenverkehr Manager Profil hinzugefügt:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Aktuelle Einschränkungen ##

Diese Funktion ist derzeit in der Vorschau, wir arbeiten zum Hinzufügen neuer Funktionen, die über einen Zeitraum, in der folgenden Liste sind die bekannten Einschränkungen auf die aktuelle Version der app Klonen:

- Automatische skalierungseinstellungen werden nicht geklont.
- Zusätzliche Terminplan Einstellungen werden nicht geklont.
- VNET Einstellungen werden nicht geklont.
- App Einsichten sind nicht automatisch auf die Ziel-Web app eingerichtet
- Einfache autorisierende Einstellungen werden nicht geklont.
- Kudu Erweiterung werden nicht geklont.
- Tipp Regeln werden nicht geklont.
- Datenbankinhalt werden nicht geklont.


### <a name="references"></a>Verweise ###
- [Azure Ressourcenmanager Grundlage PowerShell-Befehlen für Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
- [Web App Klonen mit Azure-Portal](app-service-web-app-cloning-portal.md)
- [Sichern einer Web-app in Azure-App-Verwaltungsdienst](web-sites-backup.md)
- [Azure Ressourcenmanager Support für Azure Datenverkehr Manager Preview](../../articles/traffic-manager/traffic-manager-powershell-arm.md)
- [Einführung in die App-Service-Umgebung](app-service-app-service-environment-intro.md)
- [Mithilfe der Azure PowerShell mit Azure Ressourcenmanager](../powershell-azure-resource-manager.md)
