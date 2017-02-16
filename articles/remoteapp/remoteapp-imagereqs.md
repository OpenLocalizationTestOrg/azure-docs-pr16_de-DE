
<properties
    pageTitle="Azure RemoteApp Bild Anforderungen | Microsoft Azure"
    description="Erfahren Sie mehr über die Anforderungen für das Erstellen von Bildern mit Azure RemoteApp verwendet werden"
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



# <a name="requirements-for-azure-remoteapp-images"></a>Anforderungen für Azure RemoteApp Bilder

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp verwendet ein Windows Server 2012 R2 Bild, um alle Programme zu hosten, die Sie für Ihre Benutzer freigeben möchten. Um ein benutzerdefiniertes Bild zu erstellen, können Sie mit einer vorhandenen Bild oder [Erstellen Sie einen neuen](remoteapp-create-custom-image.md)starten.

> [AZURE.TIP] Wussten Sie schon, dass Ihr Abonnement Azure RemoteApp auf ein Bild Windows Server 2012 R2 im Katalog Azure-virtuellen Computer Sie zugreifen kann, mit denen Sie ein eigenes Vorlagenbild erstellen? [Testen Sie diese Funktionen](remoteapp-image-on-azurevm.md).  


Sind die Anforderungen für das Bild, das für die Verwendung mit Azure RemoteApp hochgeladen werden kann:


- Benutzerdefinierte Applikationen speichern nicht Daten lokal auf das Bild. Diese Bilder sind statusfrei und sollten nur Applikationen enthalten.
- Das Bild enthält keine Daten, die verloren gehen können.
- Die Bildgröße sollte ein Vielfaches der MB sein. Wenn Sie versuchen, ein Bild hochladen, die ein Vielfaches nicht ist, tritt ein Fehler des Uploads.
- Die Bildgröße muss 127 GB oder zu verkleinern.
- Es muss eine virtuelle Festplatte-Datei (VHDX Dateien werden derzeit nicht unterstützt).
- Die virtuelle Festplatte darf eine Generation 2 virtuellen Computern.
- Die virtuelle Festplatte möglich fester Größe oder Dyn. Eine dynamisch erweiterte virtuelle Festplatte wird empfohlen, weil sie weniger Zeit als eine feste Größe virtuelle Festplatte Datei in Azure hochladen annimmt.
- Der Datenträger muss mit der Master Boot Record (MBR) formatiert werden: Initialisierung werden. Die Tabelle (GPT)-Partitionsstil GUID wird nicht unterstützt.
- Die virtuelle Festplatte muss mit eine einzige Installation von Windows Server 2012 R2 enthalten. Sie können mehrere Datenmengen, aber nur ein, die eine Installation von Windows enthält enthalten.
- Die Rolle des Remote Desktop Session Host (RDSH) und das Feature für die Desktopdarstellung müssen installiert sein.
- Die Rolle des Remotedesktop-Verbindungsbroker muss *nicht* installiert werden.
- Die Datei System VERSCHLÜSSELNDE muss deaktiviert werden.
- Das Bild muss mit den Parametern SYSPREPed **/oobe / generalize/shutdown** (nicht den **/mode:vm** Parameter verwenden).
- Hochladen der virtuellen Festplatte aus einer Momentaufnahmekette wird nicht unterstützt.

Weitere Informationen zum Erstellen von Bildern für Azure RemoteApp finden Sie unter [erstellen ein Bilds Azure RemoteApp](remoteapp-imageoptions.md) .
