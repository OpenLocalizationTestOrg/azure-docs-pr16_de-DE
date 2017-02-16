<properties
    pageTitle="Fügen Sie einen Datenträger einer Linux VM | Microsoft Azure"
    description="Informationen zum neuen oder vorhandenen Daten Datenträger einer Linux VM im Azure-Portal mit dem Modell zur Bereitstellung von Ressourcenmanager anfügen."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cynthn"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-vm-in-the-azure-portal"></a>So fügen Sie einen Datenträger einer Linux VM Azure-Portal

In diesem Artikel wird gezeigt, wie ein Linux virtuellen Computern über das Portal Azure neuen und vorhandenen Datenträger anfügen. Sie können auch [einen Datenträger auf einen virtuellen Computer Windows Azure-Portal anfügen](virtual-machines-windows-attach-disk-portal.md). Bevor Sie dies tun, überprüfen Sie die folgenden Tipps:

- Die Größe des virtuellen Computers steuert, wie viele Daten Datenträger, die Sie anfügen können. Weitere Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).
- Wenn Premium Speicher verwenden möchten, benötigen Sie eine DS- oder GS-Serie virtuellen Computern. Sie können Datenträger aus sowohl Premium und Standard-Speicher-Konten mit diesen virtuellen Computern verwenden. Premium-Speicher steht in bestimmte Regionen zur Verfügung. Details finden Sie unter [Premium Speicher: leistungsstarke Storage für Azure-virtuellen Computern Auslastung](../storage/storage-premium-storage.md).
- Mit virtuellen Computern verbundenen Datenträger sind tatsächlich VHD-Dateien in einem Azure-Speicherkonto an. Weitere Informationen finden Sie unter [Datenträger und virtueller Festplatten für virtuelle Computer](virtual-machines-linux-about-disks-vhds.md).
- Für einen neuen Datenträger brauchen Sie es zuerst erstellt werden, da Azure erstellt wird, wenn Sie sie anzufügen.
- Für einen vorhandenen Datenträger muss die VHD-Datei in einem Konto Azure-Speicher verfügbar sein. Können Sie eine VHD-Datei, die bereits ist es, wenn er nicht zu einer anderen virtuellen Computern oder Hochladen eigener VHD-Datei mit dem Speicherkonto angeschlossen ist.


[AZURE.INCLUDE [virtual-machines-common-attach-disk-portal](../../includes/virtual-machines-common-attach-disk-portal.md)]

## <a name="next-steps"></a>Nächste Schritte

Nachdem der Datenträger hinzugefügt wurde, müssen Sie es für die Verwendung vorzubereiten. Im Betriebssystem des virtuellen Computers angezeigt: "How to: Initialisierung einen neuen Datenträger in Linux" in diesem [Artikel](virtual-machines-linux-classic-attach-disk.md#how-to-initialize-a-new-data-disk-in-linux).
