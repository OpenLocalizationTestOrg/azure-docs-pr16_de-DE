
<properties
    pageTitle="Migrieren von Benutzerdaten aus Azure RemoteApp | Microsoft Azure"
    description="Erfahren Sie, wie Ihre Benutzerdaten ein-und Azure RemoteApp migrieren."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="how-to-migrate-data-into-and-out-of-azure-remoteapp"></a>Wie Sie Daten in die und aus Azure RemoteApp migrieren.

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Viele verschiedene Tools und Methoden können Sie [Benutzerdaten](remoteapp-upd.md) in die und aus Azure RemoteApp übertragen. Hier sind einige Methoden aus:

- Kopieren Sie und fügen Sie ein, verwenden die gemeinsame Nutzung der Zwischenablage
- Kopieren von Dateien und Daten auf einem Dateiserver
- Kopieren Sie die Dateien in OneDrive for Business über einen browser
- Kopieren Sie die Dateien mithilfe der Umleitung

>[AZURE.NOTE] 
> Können nicht aktiviert werden die OneDrive für Unternehmen oder Verbraucher synchronisieren-Agents – diese in Azure RemoteApp [werden nicht unterstützt](remoteapp-onedrive.md) .

## <a name="use-copy-and-paste-in-file-explorer"></a>Verwenden Sie kopieren und Einfügen im Datei-Explorer

Kopieren und Einfügen über die Zwischenablage ist in RemoteApp Bereitstellungen [standardmäßig](remoteapp-redirection.md)aktiviert. Auf diese Weise können Benutzer Dateien zwischen ihrer lokalen PC- und RemoteApp apps zu kopieren. Häufig im normalen Lauf der Verwendung von apps in RemoteApp Benutzer gespeichert Dateien zu ihren UPDs - verschieben, dass die Daten von RemoteApp einfach ist:

1. [Veröffentlichen von Datei-Explorer als eine app](remoteapp-publish.md) in einer Websitesammlung RemoteApp. (Beachten Sie, dass dies eine administrative Aufgabe ist.)
2. Fordern Sie die Benutzer aus, zu die Datei-Explorer-app starten, die Sie veröffentlicht und zu verwenden, kopieren und Einfügen von Dateien in ihre UPD sowohl daraus.

## <a name="upload-files-and-data-to-a-file-server-by-using-standard-network-file-copy"></a>Hochladen von Dateien und Daten auf einem Dateiserver mithilfe der standard-Netzwerk-Datei kopieren

Häufig Organisationen verwenden Dateiserver speichern allgemeine Daten. Wenn Sie den Namen oder Speicherort kennen, können Benutzer Navigieren im lokale Netzwerk für den Server und ihre Dateien, kopieren Sie chatten, wie sie über hat. Erneut sollten Sie Datei-Explorer auf RemoteApp veröffentlichen, und klicken Sie dann Ihre Benutzer freigeben.

>[AZURE.NOTE] 
> Der Dateiserver müssen im Netzwerk weitergeleitet, die in RemoteApp bereitgestellt wurde.

## <a name="copy-files-to-onedrive-for-business"></a>Kopieren Sie Dateien auf OneDrive for Business
Obwohl Sie die OneDrive for Business-Sync-Agent in RemoteApp können nicht aktiviert haben, können Sie weiterhin Dateien aus Ihrem UPD zu OneDrive for Business über einen Browser kopieren. 

1. Veröffentlichen Sie Datei-Explorer auf RemoteApp, und teilen Sie Benutzern den Zugriff auf Dateien über die app. 
2. Es ist die einfachste Möglichkeit zum Dateien übertragen, wenn sie komprimiert werden, damit Benutzer eine ZIP-Datei erstellen sollten, die alle Dateien auf OneDrive for Business verschieben enthält.
3. Bitten Sie die Benutzer wechseln Sie zu Office 365-Portal, und wechseln Sie zu OneDrive und die ZIP-Datei hochladen.

## <a name="copy-files-by-using-drive-redirection"></a>Kopieren Sie die Dateien mithilfe von Laufwerk Umleitung

Wenn Sie die [Umleitung Laufwerk](remoteapp-redirection.md)aktiviert haben, haben Sie bereits ein zugeordnetes Laufwerk für Ihre Benutzer erstellt. In diesem Fall können sie zip-ihre Dateien auf das Laufwerk umgeleitet, und klicken Sie dann auf den lokalen Computer speichern.