<properties
    pageTitle="Erstellen ein virtuellen Computers in der klassischen Portal | Microsoft Azure"
    description="Erstellen Sie einen Windows-Computer in der klassischen Azure-Portal an."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="cynthn"/>

# <a name="create-a-virtual-machine-running-windows-in-the-azure-classic-portal"></a>Erstellen einer virtuellen Computern unter Windows im klassischen Azure-Portal

> [AZURE.SELECTOR]
- [Azure klassischen-portal](virtual-machines-windows-classic-tutorial.md)
- [PowerShell: Klassischen Bereitstellung](virtual-machines-windows-classic-create-powershell.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie Sie mithilfe der **neuen Azure-Portal** [führen folgende Schritte aus, die mit dem Modell zur Bereitstellung von Ressourcenmanager](virtual-machines-windows-hero-tutorial.md) . 

In diesem Lernprogramm erfahren Sie, wie eine Azure-virtuellen Computern (virtueller Computer) unter Windows im klassischen Azure-Portal zu erstellen. Wir als Beispiel für ein Bild von Windows Server verwenden, aber das ist nur eines der Bilder, die Azure bietet viele. Beachten Sie, dass Ihr Bild Auswahlmöglichkeiten für Ihr Abonnement abhängig sind. Beispielsweise möglicherweise Windows-desktop-Images für MSDN-Abonnenten zur Verfügung.

In diesem Abschnitt wird gezeigt, wie Sie die Option **Aus der Katalog** in der klassischen Azure-Portal zu verwenden, um die Erstellung des virtuellen Computers. Diese Option bietet weitere Konfiguration Auswahlmöglichkeiten als die Option **Symbolleiste erstellen** . Wenn Sie einen virtuellen Computer zu einem virtuellen Netzwerk teilnehmen möchten, müssen Sie die Option **Aus der Katalog** verwenden.

Sie können auch auf virtuellen Computern mit [Ihren eigenen Bildern](virtual-machines-windows-classic-createupload-vhd.md)erstellen. Weitere Informationen zu dieser und andere Methoden finden Sie unter [verschiedene Methoden zum Erstellen von einem Windows-Computer](virtual-machines-windows-creation-choices.md).



## <a name="video-walkthrough"></a>Video Exemplarische Vorgehensweise

Hier ist eine exemplarische Vorgehensweise dieses Lernprogramms aus.

[AZURE.VIDEO creating-a-windows-vm-on-microsoft-azure-classic-portal]

## <a id="createvirtualmachine"> </a>Des virtuellen Computers zu erstellen

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie in das neue Azure-Portal [erstellen ein virtuellen Computers verwenden das Modell zur Bereitstellung von Ressourcenmanager](virtual-machines-windows-hero-tutorial.md) . 

- Melden Sie sich am virtuellen Computer an. Anweisungen finden Sie unter, [Melden Sie sich bei einem virtuellen Computern unter Windows Server](virtual-machines-windows-classic-connect-logon.md).

- Fügen Sie einen Datenträger zum Speichern von Daten. Sie können anfügen sowohl leere Datenträger und Datenträger, die Daten enthalten. Anweisungen finden Sie unter [Anfügen einen Datenträger auf einem Windows-Computer mit dem Bereitstellungsmodell klassischen erstellt](virtual-machines-windows-classic-attach-disk.md).
