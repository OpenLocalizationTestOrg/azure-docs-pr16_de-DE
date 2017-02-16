<properties
   pageTitle="Aktualisieren Ihrer Websitesammlung Azure RemoteApp | Microsoft Azure"
   description="Erfahren Sie, wie Ihre Sammlung Azure RemoteApp aktualisieren"
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="update-a-collection-in-azure-remoteapp"></a>Aktualisieren einer Websitesammlungs in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Es kommt der Zeitpunkt, zwangsläufig, wenn Sie den apps oder das Bild in Ihrer Websitesammlung Azure RemoteApp aktualisieren müssen. Wenn Sie eines der Bilder enthalten, die mit Ihrem Abonnement Azure RemoteApp in einem Cloud oder Hybrid Auflistung verwenden alle Updates erfolgt mit Azure RemoteApp Kopien, sodass Sie einfach ruhen lassen können.

Wenn Sie ein benutzerdefiniertes Bild (entweder, dass Sie ganz neu erstellt oder, dass Sie eine unserer Bilder geändert haben erstellte) verwenden werden, sind Sie jedoch für das Bild und apps verwalten. Wenn Sie das Bild oder keines der apps darin enthaltenen aktualisieren müssen, müssen Sie zum Erstellen einer neuen, aktualisierten Version des Bilds, und Ersetzen Sie das vorhandene Bild in Ihrer Websitesammlung mit dieser neuen aktualisierten Bild.

Ja, wie gehen Sie zu Ihrer Sammlung aktualisieren? Es ist ziemlich einfach:

1. Aktualisieren Sie das Bild, das Sie in der Websitesammlung verwendet haben. Zuweisen Sie einer beliebigen Korrekturen oder Updates, die erforderlich sind, und klicken Sie dann unter einem neuen Namen speichern.
2. [Hochladen](remoteapp-uploadimage.md) oder [Importieren](remoteapp-image-on-azurevm.md) , die auf RemoteApp Bild.
3. Klicken Sie nun auf der Seite Websitesammlungen auf **Aktualisieren**.
4. Wählen Sie das neue Bild aus der **Vorlage** Bildliste aus.
4. Hier wird das Webpart knifflig – Sie müssen entscheiden, wie Sie allen Benutzern behandelt, die derzeit in der Auflistung eine app verwenden. Sie haben die folgenden Optionen aus:
    - **Gewähren Sie Benutzern 60 Minuten nach der Aktualisierung**. Sobald die Aktualisierung abgeschlossen ist, wird Azure RemoteApp eine Meldung angezeigt, zu jedem aktiven Benutzer informieren zum Speichern ihrer Arbeit und melden Sie sich ab und melden Sie sich wieder an. Nach 60 Minuten werden jedem aktiven Benutzer gesendet, die nicht, deaktivieren angemeldet haben automatisch abgemeldet. Benutzer können sofort wieder anmelden.
    - **Melden Sie sich Benutzer sofort**. Sobald die Aktualisierung abgeschlossen ist, melden Sie alle Benutzer automatisch ohne Warnung aus. Wenn Sie diese Option auswählen, können Benutzer Daten verloren gehen. Allerdings können sie bei der app sofort wieder herstellen.

1. Klicken Sie auf das Häkchen, um die Aktualisierung zu starten.
