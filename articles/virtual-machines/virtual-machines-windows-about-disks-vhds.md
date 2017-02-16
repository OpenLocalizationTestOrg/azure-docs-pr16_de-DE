<properties
    pageTitle="Informationen zu Festplatten und virtuelle Festplatten für Windows virtuellen Computern | Microsoft Azure"
    description="Lernen Sie die Grundlagen der Datenträger und in Azure-virtuellen Computern virtuellen Festplatten für Windows."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="about-disks-and-vhds-for-azure-virtual-machines"></a>Informationen zu Festplatten und virtuelle Festplatten für Azure-virtuellen Computern

Wie mit einem anderen Computer verwenden virtuellen Computern in Azure Datenträger als einen Ort aus, um ein Betriebssystem,-Anwendungen und Daten zu speichern. Alle Azure-virtuellen Computern müssen mindestens zwei Datenträger – einen Windows-Betriebssystem und eine temporäre Datenträger. Der Datenträger Betriebssystem wird von einem Bild erstellt, und sowohl den Datenträger Betriebssystem und Bild sind virtuelle Festplatten (virtuelle Festplatten) in einem Konto Azure-Speicher gespeichert. Virtuellen Computern kann auch einen oder mehrere Daten Datenträger verfügen, die auch als virtuelle Festplatten gespeichert werden. In diesem Artikel wird auch für [Linux virtuellen Computern](virtual-machines-linux-about-disks-vhds.md)verfügbar.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



## <a name="operating-system-disk"></a>Betriebssystem-Laufwerk

Jeder virtuellen Computern verfügt über eine angefügte Betriebssystem-Laufwerk. Es wurde als SATA-Laufwerk registriert und als Laufwerk C: standardmäßig gekennzeichnet. Dieser Festplatte befindet sich eine maximale Kapazität von 1023 Gigabyte (GB). 

##<a name="temporary-disk"></a>Temporärer Speicherplatz

Der temporäre Datenträger wird automatisch für Sie erstellt. Der temporäre Datenträger ist standardmäßig und es zum Speichern von pagefile.sys als Laufwerk D: gekennzeichnet. 

Die Größe des temporären Datenträgers hängt von der Größe des virtuellen Computers. Weitere Informationen finden Sie unter [Größe für Windows virtuellen Computern](virtual-machines-windows-sizes.md).

>[AZURE.WARNING] Speichern Sie Daten nicht auf dem temporären Datenträger aus. Ermöglicht eine temporäre Speicherung für Applikationen und Prozessen, und nur Daten wie Seite oder austauschen-Dateien gespeichert werden soll. Um diesen Datenträger zu einem anderen Laufwerkbuchstaben zuordnen möchten, finden Sie unter [Ändern Sie den Buchstaben des dem temporären Windows-Datenträger](virtual-machines-windows-classic-change-drive-letter.md).

Weitere Informationen darüber, wie Azure temporären Speicherplatz verwendet finden Sie unter [Grundlegendes zu den temporären Laufwerk auf Microsoft Azure virtuellen Computern](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Datenträger

Einen Datenträger handelt es sich um eine virtuelle Festplatte, der mit einem virtuellen Computer zum Speichern von Anwendungsdaten oder andere Daten, die Sie beibehalten müssen angeschlossen ist. Daten Datenträger als SCSI-Laufwerke registriert sind, und es werden gekennzeichnet, mit einem Buchstaben, den Sie auswählen.  Jeder Datenträger verfügt über eine maximale Kapazität von 1023 GB. Die Größe des virtuellen Computers bestimmt, wie viele Datenlaufwerke erkannt und den Typ der Speicher zuordnen können Sie zum Hosten Datenträger verwenden können.

>[AZURE.NOTE] Weitere Informationen zu virtuellen Computern Kapazität finden Sie unter [Größe für Windows virtuellen Computern](virtual-machines-windows-sizes.md).

Azure erstellt einen Datenträger Betriebssystem beim Erstellen eines virtuellen Computers von einem Bild. Wenn Sie ein Bild, der Datenträger Daten enthält verwenden, wird bei der Erstellung des virtuellen Computers auch Azure Festplatten mit den Daten erstellt. Andernfalls fügen Sie nach dem Erstellen des virtuellen Computers Daten Datenträger hinzu.

Sie können Daten Datenträger eines virtuellen Computers zu einem beliebigen Zeitpunkt, durch **Anfügen** der Datenträger virtuellen Computer hinzufügen. Sie können eine virtuelle Festplatte verwenden, die Sie kopiert haben, um Ihr Speicherkonto oder die Azure für Sie erstellt oder geladen haben. Anfügen einen Datenträger ordnet die Datei virtuelle Festplatte den virtuellen Computer durch das Platzieren eines 'verleasen' der virtuellen Festplatte, damit es von Speicher gelöscht werden kann, während sie immer noch verbunden ist.

## <a name="about-vhds"></a>Informationen zu virtuellen Festplatten

Die virtuellen Festplatten in Azure verwendet werden VHD-Dateien als Seitenblobs in einem standard oder Premium Speicherkonto in Azure gespeichert. Weitere Informationen zu Seitenblobs finden Sie unter [Grundlegendes zu blockieren Blobs und Seitenblobs](https://msdn.microsoft.com/library/ee691964.aspx). Weitere Informationen zu Premium Speicher finden Sie unter [Premium Speicher: leistungsstarke Storage für Azure-virtuellen Computern Auslastung](../storage/storage-premium-storage.md).

Azure unterstützt das Format der Festplatte virtuelle Festplatte an. Das geometrisch-Format ordnet den logischen Datenträger linear innerhalb der Datei, damit dieser Datenträger Versatz X am Blob-Offset X gespeichert ist. Eine Fußzeile am Ende der Blob enthält eine Beschreibung die Eigenschaften der virtuellen Festplatte. Häufig space die Abfälle-Format, da die meisten Datenträger großen nicht verwendete Bereiche haben. Wird jedoch gespeichert Azure VHD-Dateien in einer gering gefüllten Formats, damit Sie die Vorteile der feste und dynamische Datenträger zur gleichen Zeit erhalten. Weitere Informationen finden Sie unter [Erste Schritte mit virtuellen Festplatten](https://technet.microsoft.com/library/dd979539.aspx).

Alle VHD-Dateien in Azure, die Sie als Quelle zu verwenden, um Laufwerke oder Bilder erstellen möchten, sind schreibgeschützt. Wenn Sie eine Festplatte oder ein Bild erstellen, erstellt Azure Kopien der VHD-Dateien. Diese Kopien ist schreibgeschützt oder lesen-und-schreiben, je nachdem, wie Sie die virtuelle Festplatte verwenden.

Beim Erstellen eines virtuellen Computers von einem Bild erstellt Azure einen Datenträger für die virtuellen Computern, die eine Kopie der Quelldatei VHD ist. Zum Schutz vor unbeabsichtigtes Löschvorgang platziert Azure ein verleasen auf eine beliebige Quelle VHD-Datei, die zum Erstellen eines Bilds, einer Betriebssystem-Laufwerk oder einen Datenträger verwendet wird.

Bevor Sie eine Datenquelle .vhd-Datei löschen können, müssen Sie die verleasen zu entfernen, indem Sie den Datenträger oder das Bild löschen. Sie können den virtuellen Computern, die Betriebssystem-Laufwerk, löschen und die Quelle VHD-Datei auf einmal durch Löschen des virtuellen Computers und löschen alle Zusammenhang Datenträger. Löschen einer VHD-Datei, die Quelle für einen Datenträger ist erfordert jedoch mehrere Maßnahmen, die in einer Reihenfolge festlegen. Zuerst trennen Sie den Datenträger aus dem virtuellen Computer, und klicken Sie dann löschen, und löschen Sie dann die VHD-Datei.

>[AZURE.WARNING] Wenn Sie eine Datenquelle .vhd-Datei aus dem Speicher löschen oder Ihr Speicherkonto löschen, kann nicht Microsoft die Daten für Sie wiederherzustellen.



## <a name="next-steps"></a>Nächste Schritte
-  [Fügen Sie einen Datenträger](virtual-machines-windows-attach-disk-portal.md) zusätzlichen Speicher für Ihre virtuellen Computer hinzugefügt.
-  [Hochladen eines Bilds virtuellen Windows-Computer in Azure](virtual-machines-windows-upload-image.md) , beim Erstellen eines neuen virtuellen Computers verwendet.
-  [Ändern Sie den Buchstaben des dem temporären Windows-Datenträger](virtual-machines-windows-classic-change-drive-letter.md) , sodass die Anwendung Laufwerk D: für Daten verwenden kann.
