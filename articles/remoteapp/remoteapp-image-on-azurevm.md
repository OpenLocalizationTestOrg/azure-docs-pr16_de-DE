<properties
    pageTitle="Erstellen ein Azure RemoteApp Bilds basierend auf einer Azure-virtuellen Computer | Microsoft Azure"
    description="Erfahren Sie, wie ein Bild für Azure RemoteApp erstellen, indem Sie mit einer Azure-virtuellen Computern starten."
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



# <a name="create-a-azure-remoteapp-image-based-on-an-azure-virtual-machine"></a>Erstellen eines Azure RemoteApp Bilds basierend auf einer Azure-virtuellen Computern

> [AZURE.IMPORTANT]
> Azure RemoteApp ist nicht mehr verwendet werden. Lesen Sie die Details der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Sie können Azure RemoteApp Bilder (die die apps, die freigegeben werden in Ihrer Websitesammlung, zu halten) aus einer Azure-virtuellen Computern erstellen. Können Sie auch festlegen, Abbild eines virtuellen Computers verwenden, die, das wir in der Bildergalerie Azure-virtuellen Computer hinzugefügt, die alle die Azure RemoteApp Bild erfüllt – können, die diesem Bild virtueller Computer als Ausgangspunkt für Ihre eigenen virtuellen Computer, wenn Sie möchten. Achten Sie einfach auf das Bild "Windows Server Remote Desktop Session Host" in der Bibliothek.

Es gibt zwei Schritte zum Erstellen eigener Bilder basierend auf einer Azure-virtuellen Computer – erstellen Sie das Bild aus, und klicken Sie dann auf Azure RemoteApp aus der Bibliothek Azure-virtuellen Computer hochladen aus.

## <a name="create-a-custom-image-based-on-an-azure-vm"></a>Erstellen Sie ein benutzerdefiniertes Bild basierend auf einer Azure-virtuellen Computer

Verwenden Sie folgende Schritte aus, um ein Bild basierend auf einer Azure-virtuellen Computer zu erstellen.

1. Erstellen einer Azure-virtuellen Computern an. Sie können die "Windows Server Remote Desktop Session Host" oder "Windows Server Remote Desktop Session Host mit Microsoft Office 365 ProPlus" Bilds aus der Bildergalerie Azure-virtuellen Computern verwenden. Diese Abbildung erfüllt alle Azure RemoteApp Vorlage Bild.

    Weitere Informationen finden Sie unter [Erstellen eines virtuellen Computers Windows ausgeführt](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

2. Verbinden mit dem virtuellen Computer installieren und Konfigurieren der apps, die Sie über RemoteApp freigeben möchten. Vergewissern Sie sich die zusätzlichen Windows Konfigurationen erforderlich, indem Sie Ihre apps ausführen.

    Weitere Informationen finden Sie unter [So melden Sie sich bei einem virtuellen Computern ausführen von Windows Server](../virtual-machines/virtual-machines-windows-classic-connect-logon.md).

3. Wenn Sie eines der Bilder Windows Server Remote Desktop Session Host verwenden, ist ein Skript enthalten Überprüfung, die sicherstellen, dass Ihre virtuellen Computer der RemoteApp Pre-reqs. erfüllt vorhanden Um das Skript ausführen möchten, doppelklicken Sie auf **ValidateRemoteAppImage** auf dem Desktop. Stellen Sie sicher, dass alle vom Skript gemeldeten Fehler, bevor Sie mit dem nächsten Schritt fort behoben werden.

4. SYSPREP generalize und erfassen das Bild. Weitere Informationen finden Sie in der [zum Erfassen von einem Windows-Computer als Vorlage verwenden](../virtual-machines/virtual-machines-windows-classic-capture-image.md) .



## <a name="import-the-image-into-the-azure-remoteapp-image-library"></a>Importieren Sie das Bild in der Bildbibliothek Azure RemoteApp

Gehen Sie folgendermaßen vor, um das neue Bild in Azure RemoteApp zu importieren:

1. Auf der Registerkarte **Vorlage Bilder** :
    - Wenn Sie keine vorhandene Bilder haben, klicken Sie auf **Hochladen oder importieren Sie eine Vorlagenbild**.
    - Wenn Sie bereits mindestens ein Bild haben, klicken Sie auf **+** in ein neues Bild hinzufügen möchten.

2. Wählen Sie **Importieren ein Bild aus Ihrer virtuellen Computern** Bibliothek aus, und klicken Sie dann auf **Weiter**.

3. Klicken Sie auf der nächsten Seite Wählen Sie aus der Liste der benutzerdefinierten Bild aus, und bestätigen Sie, dass Sie die Schritte aufgeführt, wenn Sie das Bild erstellt gefolgt. Klicken Sie auf **Weiter**.
4. Geben Sie einen Namen für das neue RemoteApp Bild, wählen Sie den Speicherort aus, und klicken Sie auf das Häkchen, um den Importvorgang zu starten.

> [AZURE.NOTE] Sie können von einem beliebigen Azure Standort unterstützt durch Azure virtuellen Computern an eine beliebige Azure Position von Azure RemoteApp unterstützt Bilder importieren. Der Import kann bis zu 25 Minuten dauern, je nach den jeweiligen Speicherorten.

Jetzt sind Sie bereit sind, Ihre neue Websitesammlung, entweder ein [Cloud](remoteapp-create-cloud-deployment.md) -Websitesammlung oder [Hybrid](remoteapp-create-hybrid-deployment.md), je nach Ihren Anforderungen erstellen.
