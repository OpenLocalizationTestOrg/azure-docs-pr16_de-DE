<properties
    pageTitle="Informationen zu Festplatten und virtuelle Festplatten für Linux virtuellen Computern | Microsoft Azure"
    description="Lernen Sie die Grundlagen der Datenträger und virtuellen Festplatten für in Azure-virtuellen Computern Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="cynthn"/>

# <a name="about-disks-and-vhds-for-azure-virtual-machines"></a>Informationen zu Festplatten und virtuelle Festplatten für Azure-virtuellen Computern

Wie mit einem anderen Computer verwenden virtuellen Computern in Azure Datenträger als einen Ort aus, um ein Betriebssystem,-Anwendungen und Daten zu speichern. Alle Azure-virtuellen Computern müssen mindestens zwei Datenträger – ein Linux Betriebssystem (im Falle einer Linux VM) und einen temporären Datenträger. Der Datenträger Betriebssystem wird von einem Bild erstellt, und sowohl die Betriebssystem-Datenträger und das Bild befinden sich tatsächlich virtuelle Festplatten (virtuelle Festplatten) in einem Konto Azure-Speicher gespeichert. Virtuellen Computern kann auch einen oder mehrere Daten Datenträger verfügen, die auch als virtuelle Festplatten gespeichert werden. In diesem Artikel wird auch für [Windows-virtuellen Computern](virtual-machines-windows-about-disks-vhds.md)verfügbar.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

> [AZURE.NOTE] Wenn Sie eine kurze haben, helfen Sie uns, in der Dokumentation Azure Linux virtueller Computer zu verbessern, indem Sie die Teilnahme an dieser [Umfrage schnelle](https://aka.ms/linuxdocsurvey) Ihrer Person aufführen. Jeder Antwort hilft uns helfen Ihnen, die Sie Ihre Arbeit vermitteln.

## <a name="operating-system-disk"></a>Betriebssystem-Laufwerk

Jeder virtuellen Computern verfügt über eine angefügte Betriebssystem-Laufwerk. Es wurde als SATA-Laufwerk registriert und als Laufwerk C: standardmäßig gekennzeichnet. Dieser Festplatte befindet sich eine maximale Kapazität von 1023 Gigabyte (GB). 

##<a name="temporary-disk"></a>Temporärer Speicherplatz

Der temporäre Datenträger wird automatisch für Sie erstellt. Auf virtuellen Computern Linux der Datenträger ist in der Regel /dev/sdb und formatiert und zum /mnt/resource vom Linux Agent Azure bereitgestellt.

Die Größe des temporären Datenträgers hängt von der Größe des virtuellen Computers. Weitere Informationen finden Sie unter [Größe für Linux virtuellen Computern](virtual-machines-linux-sizes.md).

>[AZURE.WARNING] Speichern Sie Daten nicht auf dem temporären Datenträger aus. Ermöglicht eine temporäre Speicherung für Applikationen und Prozessen, und nur Daten wie Seite oder austauschen-Dateien gespeichert werden soll. 

Weitere Informationen darüber, wie Azure temporären Speicherplatz verwendet finden Sie unter [Grundlegendes zu den temporären Laufwerk auf Microsoft Azure virtuellen Computern](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Datenträger

Einen Datenträger handelt es sich um eine virtuelle Festplatte, der mit einem virtuellen Computer zum Speichern von Anwendungsdaten oder andere Daten, die Sie beibehalten müssen angeschlossen ist. Daten Datenträger als SCSI-Laufwerke registriert sind, und es werden gekennzeichnet, mit einem Buchstaben, den Sie auswählen.  Jeder Datenträger verfügt über eine maximale Kapazität von 1023 GB. Die Größe des virtuellen Computers bestimmt, wie viele Datenlaufwerke erkannt und den Typ der Speicher zuordnen können Sie zum Hosten Datenträger verwenden können.

>[AZURE.NOTE] Weitere Details zu belasten virtuellen Computern finden Sie unter [Größe für Linux virtuellen Computern](virtual-machines-linux-sizes.md).

Azure erstellt einen Datenträger Betriebssystem beim Erstellen eines virtuellen Computers von einem Bild. Wenn Sie ein Bild, der Datenträger Daten enthält verwenden, wird bei der Erstellung des virtuellen Computers auch Azure Festplatten mit den Daten erstellt. Andernfalls fügen Sie nach dem Erstellen des virtuellen Computers Daten Datenträger hinzu.

Sie können Daten Datenträger eines virtuellen Computers zu einem beliebigen Zeitpunkt, durch **Anfügen** der Datenträger virtuellen Computer hinzufügen. Sie können eine virtuelle Festplatte verwenden, die Sie kopiert haben, um Ihr Speicherkonto oder die Azure für Sie erstellt oder geladen haben. Anfügen einen Datenträger ordnet die virtuelle Festplatte Datei von Ihrem Speicherkonto des virtuellen Computers, indem Sie eine 'verleasen' auf die virtuelle Festplatte ein, damit es von Speicher gelöscht werden kann, während sie immer noch verbunden ist.

## <a name="about-vhds"></a>Informationen zu virtuellen Festplatten

Die virtuellen Festplatten in Azure verwendet werden VHD-Dateien als Seitenblobs in einem standard oder Premium Speicherkonto in Azure gespeichert. Details zu Seitenblobs finden Sie unter [Grundlegendes zu blockieren Blobs und Seitenblobs](https://msdn.microsoft.com/library/ee691964.aspx). Details zu Premium-Speicher finden Sie unter [Premium Speicher: leistungsstarke Storage für Azure-virtuellen Computern Auslastung](../storage/storage-premium-storage.md).

Azure unterstützt das Format der Festplatte virtuelle Festplatte an. Feste Format ordnet den logischen Datenträger linear innerhalb der Datei, damit dieser Datenträger Versatz X am Blob-Offset X gespeichert ist. Eine Fußzeile am Ende der Blob enthält eine Beschreibung die Eigenschaften der virtuellen Festplatte. Feste Format häufig verschwendet Platz, da die meisten Datenträger großen nicht verwendete Bereiche haben. Wird jedoch gespeichert Azure VHD-Dateien in einer gering gefüllten Formats, damit Sie die Vorteile der feste und dynamische Datenträger zur gleichen Zeit erhalten. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit virtuellen Festplatten](https://technet.microsoft.com/library/dd979539.aspx).

Alle VHD-Dateien in Azure, die Sie als Quelle zu verwenden, um Laufwerke oder Bilder erstellen möchten, sind schreibgeschützt. Wenn Sie eine Festplatte oder ein Bild erstellen, erstellt Azure Kopien der VHD-Dateien. Diese Kopien ist schreibgeschützt oder lesen-und-schreiben, je nachdem, wie Sie die virtuelle Festplatte verwenden.

Beim Erstellen eines virtuellen Computers von einem Bild erstellt Azure einen Datenträger für die virtuellen Computern, die eine Kopie der Quelldatei VHD ist. Zum Schutz vor unbeabsichtigtes Löschvorgang platziert Azure ein verleasen auf eine beliebige Quelle VHD-Datei, die zum Erstellen eines Bilds, einer Betriebssystem-Laufwerk oder einen Datenträger verwendet wird.

Bevor Sie eine Datenquelle .vhd-Datei löschen können, müssen Sie die verleasen zu entfernen, indem Sie den Datenträger oder das Bild löschen. Um eine .vhd-Datei löschen, die von einem virtuellen Computer als Betriebssystem-Laufwerk verwendet wird, können Sie Löschen des virtuellen Computers, der Datenträger Betriebssystem und die Quelle VHD-Datei auf einmal durch Löschen des virtuellen Computers und löschen alle Zusammenhang Festplatten. Jedoch Löschen einer VHD-Datei, die Quelle für einen Datenträger ist erfordert mehrere Maßnahmen, die in einer Reihenfolge festlegen – trennen Sie den Datenträger aus des virtuellen Computers, löschen, und löschen Sie dann die VHD-Datei.

>[AZURE.WARNING] Wenn Sie eine Datenquelle .vhd-Datei aus dem Speicher löschen oder Ihr Speicherkonto löschen, kann nicht Microsoft die Daten für Sie wiederherzustellen.


## <a name="troubleshooting"></a>Behandlung von Problemen
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Nächste Schritte

-  [Fügen Sie einen Datenträger](virtual-machines-linux-add-disk.md) zusätzlichen Speicher für Ihre virtuellen Computer hinzugefügt.
-  [Konfigurieren Software RAID](virtual-machines-linux-configure-raid.md) Redundanzgründen.
-  [Erfassen eines Linux virtuellen Computers](virtual-machines-linux-classic-capture-image.md) , damit Sie schnell zusätzliche virtuellen Computern bereitstellen können.


