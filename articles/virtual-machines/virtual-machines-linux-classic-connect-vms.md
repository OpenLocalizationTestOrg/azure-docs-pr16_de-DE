<properties
    pageTitle="Verbinden von Linux virtuellen Computern in einen Cloud-Dienst | Microsoft Azure"
    description="Verbinden Sie Linux virtuellen Computer mit dem Bereitstellungsmodell klassischen mit einer Azure-Cloud-Dienst oder virtuelles Netzwerk erstellt."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cynthn"/>

# <a name="connect-linux-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Verbinden von Linux virtuellen Computern erstellt, mit dem Bereitstellungsmodell klassischen mit einem virtuellen Netzwerk oder Cloud-Dienst

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Virtuelle Linux-Computer mit dem Bereitstellungsmodell klassischen erstellt werden immer in einen Cloud-Dienst platziert. Cloud-Dienst fungiert als Container und stellt einen eindeutigen öffentlichen DNS-Namen, eine öffentliche IP-Adresse und eine Reihe von Endpunkten in der virtuelle Computer über das Internet zugreifen. Cloud-Dienst in einem Netzwerk virtuelle werden kann, aber nicht erforderlich ist. Sie können auch die [Windows-virtuellen Computern mit einem virtuellen Netzwerk oder Cloud-Dienst herstellen](virtual-machines-windows-classic-connect-vms.md).

Wenn Sie ein Clouddienst in einem virtuellen Netzwerk nicht zur Verfügung, es einen *eigenständigen* Cloud-Dienst genannt. Den virtuellen Computern in einem eigenständigen Cloud-Dienst können nur mit den anderen virtuellen Computern öffentlichen DNS-Namen mit anderen virtuellen Computern kommunizieren, und der Datenverkehr bewegt sich über das Internet. Ist ein Cloud-Dienst in einem virtuellen Netzwerk, den virtuellen Computern dieses Cloud-Dienst mit anderen virtuellen Computern in das virtuelle Netzwerk kommunizieren kann, ohne den Datenverkehr über das Internet sendet.

Setzen Sie Ihre virtuellen Computer in der gleichen eigenständigen Cloud-Dienst, können Sie immer noch den Lastenausgleich und Verfügbarkeit Mengen angezeigt werden. Details finden Sie unter [den Lastenausgleich virtuellen Computern](virtual-machines-linux-load-balance.md) und [Verwalten der Verfügbarkeit von virtuellen Computern](virtual-machines-linux-manage-availability.md). Jedoch nicht den virtuellen Computern in Subnetzen organisieren oder einen eigenständigen Cloud-Dienst mit Ihrem lokalen Netzwerk verbinden. Hier ist ein Beispiel:

[AZURE.INCLUDE [virtual-machines-common-classic-connect-vms](../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Nächste Schritte

Nach dem Erstellen eines virtuellen Computers ist es eine gute Idee, [einen Datenträger hinzufügen](virtual-machines-linux-classic-attach-disk.md) , sodass Ihre Dienste und Auslastung einen Speicherort zum Speichern von Daten haben. 



