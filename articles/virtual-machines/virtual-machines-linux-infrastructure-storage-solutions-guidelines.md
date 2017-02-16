<properties
    pageTitle="Speicher Lösungen Richtlinien | Microsoft Azure"
    description="Lernen Sie die wichtigsten Entwurf und Implementierung von Richtlinien für die Bereitstellung von Lösungen von Speicher in Azure-Infrastrukturdiensten aus."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="storage-infrastructure-guidelines"></a>Richtlinien für Speicher-Infrastruktur

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Dieser Artikel befasst sich Grundlegendes zu Speicher Anforderungen und Entwurf für optimale virtuellen Computern (virtueller Computer) Leistung erreichen.


## <a name="implementation-guidelines-for-storage"></a>Von Implementierungsrichtlinien für Speicher

Entscheidungen:

- Benötigen Sie Standard oder Premium Speicherplatz für Ihre Arbeitsbelastung verwenden?
- Benötigen Sie Festplattenstriping Datenträger größer als 1023 GB erstellen?
- Benötigen Sie Festplattenstriping optimale Leistung von e/a für Ihre Arbeitsbelastung erzielen?
- Welche Menge Storage Kontotypen müssen Sie Ihre IT-Arbeitsbelastung oder Infrastruktur hosten?

Aufgaben:

- Überprüfen Sie die e/a-Auslastung des Applications, die Sie bereitstellen und planen die entsprechende Anzahl und Typ von Speicherkonten.
- Erstellen von Speicherkonten mit Ihrer Benennungskonvention festlegen. Sie können die CLI Azure oder im Portal verwenden.


## <a name="storage"></a>Speicher

Azure-Speicher ist ein wichtiger Bestandteil bereitstellen und Verwalten von virtuellen Computern (virtuelle Computer) und Applikationen. Azure-Speicher bietet Dienste zum Speichern von Daten, unstrukturierte Daten und Nachrichten, und es ist auch Bestandteil der entsprechenden virtuellen Computern unterstützenden Infrastruktur.

Stehen zwei Arten von Speicherkonten für die Unterstützung von virtuellen Computern zur Verfügung:

- Standard-Speicher Konten gewähren Sie Zugriff auf Blob-Speicher (verwendet zum Speichern von Azure-virtuellen Computer Datenträger), Tabellenspeicher, Warteschlangenspeicher und Dateispeicher erhalten.
- [Premium Speicher](../storage/storage-premium-storage.md) Konten vorführen leistungsfähige und niedrig Wartezeiten Datenträger Unterstützung für e/a-stark Auslastung, wie z. B. MongoDB Sharded Cluster. Premium Speicher unterstützt derzeit nur Datenträger Azure-virtuellen Computer an.

Azure erstellt virtuelle Computer mit einer Betriebssystem Festplatten, eine temporäre und NULL oder mehr optionale Daten Datenträger. Die Betriebssystem-Datenträger und Datenlaufwerke auf sind Azure Seitenblobs, während der temporäre Speicherplatz auf dem Knoten lokal gespeichert wird, in dem auf der Computer verfügbar ist. Achten Sie beim Entwerfen von Applications nur diese temporären Speicherplatz für nicht beständige Daten verwendet, wie der virtuellen Computer zwischen Hosts während eines Ereignisses Wartung migriert werden kann. Auf dem temporären Datenträger gespeicherten Daten wäre verloren.

Zuverlässigkeit und hohe Verfügbarkeit erfolgt über die zugrunde liegenden Azure Storage-Umgebung, um sicherzustellen, dass die Daten weiterhin für nicht geplanten Wartung oder Hardware-Fehlern geschützt ist. Beim Entwerfen Ihrer Umgebung Azure Storage können Sie Speicher virtueller Computer repliziert auswählen:

- lokal innerhalb eines bestimmten Azure Datencenters
- für Azure Rechenzentren innerhalb eines bestimmten Bereichs
- für Azure Rechenzentren über verschiedener Regionen.

Sie können [Weitere Informationen zu den Replikationsoptionen für hohen Verfügbarkeit](../storage/storage-introduction.md#replication-for-durability-and-high-availability)lesen.

Betriebssystem Festplatten und Datenfestplatten besitzen eine maximale Größe von 1023 Gigabyte (GB). Die maximale Größe eines Blob ist 1024 GB und müssen, die die Metadaten (Fußzeile) der Datei virtuelle Festplatte (ein GB ist 1024<sup>3</sup> Byte) enthalten. Logische Volume Manager (LVM) können Sie diese Beschränkung überschreiten, indem Verbindungspooling zusammen Datenträger Daten, wenn Sie Ihre virtuellen Computer logische Datenträger, die größer als 1023 GB präsentieren.

Es gibt einige Skalierbarkeit Beschränkungen beim Entwerfen der Bereitstellung Ihrer Azure-Speicher – finden Sie unter [Grenzwerte für Microsoft Azure-Abonnement und Dienst, Kontingente, und Einschränkungen](azure-subscription-service-limits.md#storage-limits) für weitere Details. Siehe auch [Azure-Speicher Skalierbarkeit und Leistung Ziele](../storage/storage-scalability-targets.md)aus.

Sie können für Anwendungsspeicher unstrukturierten Objektdaten wie Dokumente, Bilder, Sicherungskopien, Konfigurationsdaten, Protokolle usw. speichern. Verwenden von Blob-Speicher. Statt eine Anwendung wie auf einen virtuellen Datenträger, die den virtuellen Computer angefügt kann die Anwendung direkt in Azure Blob-Speicher zu schreiben. BLOB-Speicher bietet auch die Möglichkeit, [wichtiges und aussagekräftige Speicherebenen](../storage/storage-blob-storage-tiers.md) je nach Ihren Anforderungen Verfügbarkeit und Kosten Einschränkungen.


## <a name="striped-disks"></a>Verteilten Festplatten
Neben ermöglicht es Ihnen, Laufwerke in vielen Fällen mehr als 1023 GB erstellen optimiert mit Striping für Daten Datenträger Leistung von Dokumenten, indem mehrere Blobs in den Hintergrund des Speichers für ein einzelnes Volume. Fortgesetzt wird mit Striping, die für das Schreiben und Lesen von Daten aus einem einzelnen logischen Datenträger e/a Parallel.

Azure auferlegt Grenzwerte für die Anzahl der Datenträger Daten und Bandbreite zur Verfügung, abhängig von der Größe des virtuellen Computer an. Weitere Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).

Wenn Sie Festplattenstriping für Datenträger Azure-Daten verwenden, sollten Sie die folgenden Richtlinien:

- Daten Datenträger sollten immer die maximale Größe (1023 GB).
- Fügen Sie die maximale Daten Datenträger für die Größe des virtuellen Computer zulässig.
- Verwenden Sie LVM.
- Vermeiden Sie Optionen zum Zwischenspeichern Azure Datenträger (Zwischenspeichern Richtlinie = keine).

Weitere Informationen finden Sie unter [Konfigurieren von LVM einer Linux virtuellen Computers](virtual-machines-linux-configure-lvm.md).


## <a name="multiple-storage-accounts"></a>Mehrere Speicherkonten

Beim Entwerfen der Azure-Speicher-Umgebung können Sie mehrere Speicherkonten als die Anzahl der virtuellen Computern, die Sie bereitstellen erhöht. Dieser Ansatz hilft, die ein-/Ausgabe auf der zugrunde liegenden Azure Storage Infrastruktur optimale Leistung für Ihre virtuellen Computern und Applikationen verwalten verteilen. Wie Sie die Anwendungen, die Sie bereitstellen möchten entwerfen, sollten Sie über Konten Azure-Speicher die e/a-Anforderungen, die jeder virtuellen Computer ist und Saldo sich diese virtuellen Computern. Versuchen Sie, um zu vermeiden, gruppieren alle der hoher i/o anspruchsvolle virtuellen Computern in mit nur einem oder zwei Speicherkonten.

Weitere Informationen zu den e/a-Funktionen, die der anderen Optionen Azure-Speicher und einige empfohlen Sie Maximalwerte, finden Sie [Azure-Speicher Skalierbarkeit und Leistung Ziele](../storage/storage-scalability-targets.md).


## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 