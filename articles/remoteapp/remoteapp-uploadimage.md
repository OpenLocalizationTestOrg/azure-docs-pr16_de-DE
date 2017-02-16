
<properties
    pageTitle="Ein benutzerdefiniertes Bild für Azure RemoteApp hochladen | Microsoft Azure"
    description="Erfahren Sie, wie Sie ein benutzerdefiniertes Bild für Azure RemoteApp hochladen."
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="ericor" />



# <a name="upload-a-custom-image-for-azure-remoteapp"></a>Ein benutzerdefiniertes Bild für Azure RemoteApp hochladen

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Jetzt, da Sie benutzerdefinierte Vorlagenbild erstellt haben oder es mit den Änderungen aktualisiert haben, müssen Sie das Bild in Ihre Bildbibliothek Azure RemoteApp hochladen. Gehen Sie folgendermaßen vor.


## <a name="before-you-start"></a>Bevor Sie beginnen

1.      Stellen Sie sicher, dass Ihr benutzerdefinierte Bild im [Bild Anforderungen](remoteapp-imagereqs.md) und die [Anwendung Anforderungen](remoteapp-appreqs.md)erfüllt.
2.      Installieren Sie das [Azure PowerShell-Modul](../powershell-install-configure.md).

## <a name="step-by-step-on-how-to-upload-custom-image"></a>Klicken Sie auf benutzerdefiniertes Bild Hochladen von Schritt für Schritt

1.      Öffnen Sie Azure-Verwaltungsportal zu, und navigieren Sie zu der Seite RemoteApp.
2.      Klicken Sie auf **Hochladen** , am unteren Rand der Seite, auf der Registerkarte **Vorlage Bilder** .
4.      Geben Sie einen Anzeigenamen für das Bild, und geben Sie den Speicherort für das Konto an. Stellen Sie sicher, dass der Speicherort ist am selben Speicherort wie Ihre Sammlung RemoteApp oder an einem Speicherort, in dem Sie einen erstellen möchten.
5.      Wenn Sie dazu aufgefordert werden, laden Sie das Skript auf Ihrem lokalen Computer.
6.      Kopieren Sie der Befehlsparameter in das Textfeld in der Zwischenablage.
7.      Öffnen einer erhöhten Windows PowerShell-Fenster an.
8.      Navigieren Sie aus dem erhöhten Windows PowerShell-Fenster in dem Verzeichnis, in dem Sie das Skript heruntergeladen haben.
9.      Fügen Sie den kopierten Befehl, und drücken Sie die **EINGABETASTE**.

    Während des Uploads beginnt und Dauer möglicherweise hängen von vielen Faktoren, einschließlich Ihrer netzwerkgeschwindigkeit und die Größe des Bilds

11.    Wenn der Upload nicht auf dem aufgrund von Netzwerk Unterbrechung oder Dinge erfolgreich ist, können Sie immer während des Uploads fortsetzen, die Sie begonnen haben. Um einen Upload fortzusetzen, führen Sie das Skript erneut mithilfe derselben Befehlszeile.

> [AZURE.WARNING] Ändern Sie das Upload-Skript nie. Spezifische Tests wurden implementiert, um sicherzustellen, dass das Bild im Bild Anforderungen und die Anwendung Anforderungen erfüllt.

## <a name="common-problems"></a>Häufig auftretende Probleme

- Stellen Sie sicher, dass Sie Windows PowerShell nicht Azure PowerShell verwenden. Sie müssen das Azure PowerShell-Modul zu installieren, weil bestimmte Module während des Upload-Vorgangs erforderlich sind.
- Ändern das Skript nie, Validierungen gibt es erleichtern.
- Wenn die Datei virtuelle Festplatte während des Uploads gesperrt wird, kopieren Sie die Datei oder Verschieben an einen neuen Speicherort und Versuch Uploads erneut. Möglicherweise gibt es einige Windows-Prozess, der den Upload verhindert wird.  
