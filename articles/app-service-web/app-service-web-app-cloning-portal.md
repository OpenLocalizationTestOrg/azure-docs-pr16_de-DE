<properties
    pageTitle="Web App Klonen mit Azure-Portal"
    description="Erfahren Sie, wie Ihre Web Apps neue Web Apps mit Azure-Portal klonen."
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
    ms.date="03/08/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Klonen Azure-Portal mit Azure App-Verwaltungsdienst-App#

Das Klonen Feature in [Azure App Dienst Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) können Sie ganz einfach vorhandenen Web apps zu einer neu erstellten app in einem anderen Bereich oder in der gleichen Region klonen. Dadurch werden die Kunden, die eine Reihe von apps in verschiedener Regionen schnell und einfach bereitstellen.

Klonen App gibt es zurzeit unterstützt nur Premium Ebene app-Service-Pläne. Das neue Feature verwendet die gleichen Einschränkungen als Web Apps Sicherung Feature, finden Sie unter [einer Web app im App-Verwaltungsdienst Azure sichern](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 


## <a name="cloning-an-existing-app"></a>Klonen einer vorhandenen App ##

Die Web-app muss im Modus **Premium** in Reihenfolge für Sie zum Erstellen einer datenbeschriftungsreihe für das Web app ausgeführt werden.

1. Öffnen Sie im [Portal Azure](https://portal.azure.com/)-Web app Blade aus.
2. Klicken Sie auf **Extras**. Klicken Sie dann in das Blade **Tools** auf **Datenbeschriftungsreihe App**.

    ![][1]

    > [AZURE.NOTE]
    > Wenn das Web app im Modus **Premium** noch nicht vorhanden ist, erhalten Sie eine Meldung mit den unterstützten Modi für die app klonen. An diesem Punkt müssen Sie die Option zum **Aktualisieren**auswählen.
    
3. Geben Sie einen Namen der neuen Web app, Ressourcengruppe und Planen der App-Dienst, in der **App Klonen** Blade. Außerdem werden der Benutzer wählen, ob eine Anzahl von Quelle Web app-Einstellungen Klonen oder nicht.

    ![][2]

4. Nach dem Klicken auf **Erstellen** wird die Plattform ordnungsgemäß Verhalten, zum Erstellen einer datenbeschriftungsreihe der Quelle Web app.

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonen einer vorhandenen App zu einer App-Service-Umgebung##

In der **App Klonen** Blade haben der Kunden die Möglichkeit, eine app Ressourcenpool in einer bestehenden Umgebung der App-Dienst auswählen.

## <a name="current-restrictions"></a>Aktuelle Einschränkungen ##

Diese Funktion ist derzeit in der Vorschau, wir arbeiten zum Hinzufügen neuer Funktionen, die über einen Zeitraum, in der folgenden Liste sind die bekannten Einschränkungen auf die aktuelle Unterstützung der app Klonen Azure-Portal:

- Azure Datenverkehr Manager Einstellungen werden nicht geklont.
- Automatische skalierungseinstellungen werden nicht geklont.
- Zusätzliche Terminplan Einstellungen werden nicht geklont.
- VNET Einstellungen werden nicht geklont.
- App Einsichten sind nicht automatisch auf die Ziel-Web app eingerichtet
- Einfache autorisierende Einstellungen werden nicht geklont.
- Kudu Erweiterung werden nicht geklont.
- Tipp Regeln werden nicht geklont.
- Datenbankinhalt werden nicht geklont.


### <a name="references"></a>Verweise ###
- [Web App Klonen mithilfe der PowerShell](app-service-web-app-cloning.md)
- [Sichern einer Web-app in Azure-App-Verwaltungsdienst](web-sites-backup.md)
- [So erstellen Sie eine App-Service-Umgebung](app-service-web-how-to-create-an-app-service-environment.md)
- [Erstellen Sie eine Web-app in einer App-Service-Umgebung](app-service-web-how-to-create-a-web-app-in-an-ase.md)
- [Einführung in die App-Service-Umgebung](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png