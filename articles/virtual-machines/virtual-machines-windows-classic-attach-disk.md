<properties
    pageTitle="Fügen Sie einen Datenträger an einen virtuellen | Microsoft Azure"
    description="Fügen Sie einen Datenträger auf einem Windows-Computer mit dem Bereitstellungsmodell klassischen erstellt, und es Initialisierung."
    services="virtual-machines-windows, storage"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="cynthn"/>

# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Fügen Sie einen Datenträger auf einem Windows-Computer mit dem Bereitstellungsmodell klassischen erstellt

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Wenn Sie das neue Portal verwenden möchten, finden Sie unter [So fügen Sie einen Datenträger in einen virtuellen Computer Windows Azure-Portal](virtual-machines-windows-attach-disk-portal.md).

Wenn Sie einen zusätzliche Datenträger benötigen, können Sie einen leeren Datenträger oder auf einer vorhandenen Festplatte mit Daten an einen virtuellen anfügen. In beiden Fällen sind die Datenträger VHD-Dateien, die sich in einem Konto Azure-Speicher befinden. Im Fall einer neuen Datenträger Nachdem Sie den Datenträger installieren müssen Sie auch zur Initialisierung, damit er für die Verwendung durch einen Windows-virtuellen bereit ist.

Finden Sie unter Weitere Details Datenträger [zu Datenträger und virtueller Festplatten für virtuelle Computer](virtual-machines-windows-about-disks-vhds.md).


[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

## <a name="initialize-the-disk"></a>Initialisierung des Datenträgers

1. Verbinden Sie mit des virtuellen Computers. Anweisungen finden Sie unter [So melden Sie sich mit einem virtuellen Computer unter Windows Server][logon].

2. Öffnen Sie nach der Anmeldung des virtuellen Computers **Server-Manager**. Wählen Sie im linken Bereich **Datei- und Speicher zur Verfügung**.

    ![Öffnen des Server-Managers](./media/virtual-machines-windows-classic-attach-disk/fileandstorageservices.png)

3. Erweitern Sie das Menü, und wählen Sie die **Festplatten**.

4. Der **Datenträger** Abschnitt listet die Datenträger. In den meisten Fällen müssen diese Festplatten 0, 1 und Datenträger 2. Datenträger 0 ist das Betriebssystem Datenträger, Datenträger 1 ist der temporären Datenträger und Datenträger 2 ist der Datenträger, die, den Sie soeben an den virtuellen Computer angefügt. Die neuen Daten Datenträgers wird die Partition als **unbekannt**aufgeführt. Mit der rechten Maustaste in des Datenträgers, und wählen Sie die **Initialisierung**.

5.  Sie werden benachrichtigt, dass alle Daten gelöscht werden bei der Initialisierung des Datenträgers ist. Klicken Sie auf **Ja,** um zu bestätigen Sie die Warnung und Initialisierung den Datenträger. Sobald Sie fertig sind, wird die Partition als **GPT**aufgeführt. Mit der rechten Maustaste erneut auf des Datenträgers, und wählen Sie **Neues Volume**.

6.  Führen Sie den Assistenten unter Verwendung der Standardwerte an. Wenn der Assistent fertig ist, Listen im Abschnitt **Datenmengen** das neue Volume aus. Der Datenträger ist jetzt online und bereit, um Daten zu speichern.

    ![Volumen-Initialisierung erfolgreich](./media/virtual-machines-windows-classic-attach-disk/newvolumecreated.png)

> [AZURE.NOTE] Die Größe der virtuellen Computer bestimmt, wie viele Datenträger, die Sie anfügen können. Weitere Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[So trennen Sie einen Datenträger aus einem Windows-Computer](virtual-machines-windows-classic-detach-disk.md)

[Informationen zu Festplatten und virtuelle Festplatten für virtuellen Computern](virtual-machines-linux-about-disks-vhds.md)

[logon]: virtual-machines-windows-classic-connect-logon.md
