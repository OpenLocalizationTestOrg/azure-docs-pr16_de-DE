<properties
    pageTitle="Azure-Speicher Skalierbarkeit und Leistungsziele | Microsoft Azure"
    description="Erfahren Sie mehr über die Ziele Skalierbarkeit und Leistung, für Azure-Speicher, einschließlich Kapazität, Rate Anfragen und eingehende und ausgehende Bandbreite für beide Speicherkonten Standard- und Premium. Grundlegendes zu Leistungsziele für Partitionen innerhalb jeder der Dienste Azure-Speicher."
    services="storage"
    documentationCenter="na"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn" />
<tags
    ms.service="storage"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage"
    ms.date="08/03/2016"
    ms.author="robinsh" />

# <a name="azure-storage-scalability-and-performance-targets"></a>Azure-Speicher Skalierbarkeit und Leistungsziele

## <a name="overview"></a>(Übersicht)

In diesem Thema werden die Themen Skalierbarkeit und Leistung für Microsoft Azure-Speicher. Eine Zusammenfassung der anderen Azure Grenzwerte finden Sie unter [Azure-Abonnement und Beschränkungen Service, Kontingente, und Einschränkungen](../azure-subscription-service-limits.md).

>[AZURE.NOTE] Alle Speicherkonten führen Sie auf der neuen flachen Netzwerk Suchtopologie und unterstützen der Skalierbarkeit und Leistung Ziele gegliederter unterhalb, unabhängig davon, wann sie erstellt wurden. Weitere Informationen und Skalierbarkeit flachen Netzwerkarchitektur Azure-Speicher finden Sie unter [Microsoft Azure-Speicher: A hochgradig verfügbar Cloud-Speicherdienst mit starken Konsistenz](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx).

>[AZURE.IMPORTANT] Die hier aufgeführten Skalierbarkeit und Leistung-Ziele High-End-Ziele sind, aber erreichbar sind. In allen Fällen, Rate Anfragen und Ihre Speicher erzielten Bandbreite Konto richtet sich nach der Größe von Objekten gespeichert, die Access-Muster ausgelastet, und den Typ der Arbeitsbelastung Ihrer Anwendung ausführt. Achten Sie darauf, dass zu testen des Diensts, um festzustellen, ob die Leistung Ihren Anforderungen entsprechen. Falls möglich, plötzliche Spitzen in der Kostensatz Verkehr zu vermeiden, und stellen Sie sicher, dass Datenverkehr partitionsübergreifend gut verteilt ist.

>Wenn eine Anwendung die maximal was eine Partition für Ihre Arbeitsbelastung bewältigen kann erreicht, beginnt Azure-Speicher Fehlercode 503 (Server beschäftigt) oder mit dem Fehlercode 500 (Vorgang Timeout) Antworten zurückgeben. In diesem Fall sollte die Anwendung eine exponentiellen Backoff Richtlinie für Wiederholungsversuche verwenden. Die exponentiellen Backoff ermöglicht die Laden auf Partition zu verkleinern und um Spitzen im Datenverkehr zu dieser Partition zu erleichtern.

Die Anforderungen Ihrer Anwendung, die größer sind als der Skalierbarkeit Ziele eines einzelnen Speicher-Kontos, können Sie erstellen Ihrer Anwendung, um mehrere Speicherkonten verwenden und Datenobjekte über diese Speicherkonten hinweg partitionieren. Informationen für die Volumenlizenz finden Sie unter [Azure Speicher Preise](https://azure.microsoft.com/pricing/details/storage/) .


## <a name="scalability-targets-for-blobs-queues-tables-and-files"></a>Skalierbarkeit Ziele für Blobs, Queues, Tabellen und Dateien

[AZURE.INCLUDE [azure-storage-limits](../../includes/azure-storage-limits.md)]

## <a name="scalability-targets-for-virtual-machine-disks"></a>Skalierbarkeit Ziele für Datenträger virtuellen Computern

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../../includes/azure-storage-limits-vm-disks.md)]

Weitere Details finden Sie unter [Windows virtueller Computer Größen](../virtual-machines/virtual-machines-windows-sizes.md) oder [Linux virtueller Computer Schriftgrade](../virtual-machines/virtual-machines-linux-sizes.md) .

### <a name="standard-storage-accounts"></a>Standard Speicherkonten

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../../includes/azure-storage-limits-vm-disks-standard.md)]

### <a name="premium-storage-accounts"></a>Premium Speicher-Konten

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../../includes/azure-storage-limits-vm-disks-premium.md)]

## <a name="scalability-targets-for-azure-resource-manager"></a>Skalierbarkeit Ziele für Azure Ressourcenmanager

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="partitions-in-azure-storage"></a>Partitionen in Azure-Speicher

Jedes Objekt, das Daten enthält, die in Azure-Speicher (Blobs, Nachrichten, Elemente und Dateien) gespeichert ist auf eine Partition gehört, und wird durch einen Partitionsschlüssel identifiziert. Die Partition bestimmt, wie Azure-Speicher Laden von Blobs, Nachrichten, Elemente und Dateien auf Servern, um den Datenverkehr dieser Objekte Anforderungen Salden. Der Partitionsschlüssel ist eindeutig und wird verwendet, um einen Blob, eine Nachricht oder eine Entität zu suchen.

Der obigen Tabelle in Listen [Skalierbarkeit Ziele für Standard-Speicher-Konten](#standard-storage-accounts) die Leistungsziele für eine einzelne Partition für jeden Dienst.

Einfluss auf den Lastenausgleich und Skalierbarkeit für jede der Speicherdienste wie folgt:

- **BLOBs**: der Partitionsschlüssel für ein Blob ist Kontonamen + + Containername der Blob. Dies bedeutet, dass jede Blob eine eigenen Partition kann Wenn Belastung der Blob erfordert. BLOBs können auf mehrere Server verteilt werden, um den Zugriff darauf zu skalieren, aber ein einzelnes Blob kann nur von einem einzigen Server bereitgestellt werden. Während Blobs logisch Blob Container gruppiert werden können, gibt es keine vorherigen Auswirkungen aus diesem gruppieren.

- **Dateien**: der Partitionsschlüssel für eine Datei + Name der Datei freigeben Name-Konto ist. Dies bedeutet, dass alle Dateien in einem freigegebenen Ordner auch eine einzelne Partition aufweisen.

- **Nachrichten**: die Partitionsschlüssel für eine Nachricht ist die Kontonamen + Name der Warteschlange, damit alle Nachrichten in einer Warteschlange in einer einzelnen Partition gruppiert sind und von einem einzigen Server bereitgestellt werden. Von anderen Servern zur Lastenausgleich für jedoch viele Warteschlangen möglicherweise ein Speicherkonto besitzen, können unterschiedliche Warteschlange verarbeitet werden.

- **Einheiten**: der Partitionsschlüssel für eine Entität ist Konto + Name der Tabelle + Taste, wobei der Partitionsschlüssel der Wert der erforderlichen User defined **PartitionKey** -Eigenschaft für die Entität ist partitionieren. Alle Elemente mit demselben Schlüsselwert Partition in derselben Partition gruppiert sind und von der gleichen Partitionsserver bereitgestellt werden. Dies ist ein wichtiger Punkt, beim Entwerfen Ihrer Anwendung zu verstehen. Die Anwendung sollten Skalierbarkeit Vorteile der Verteilen von Personen auf mehreren Partitionen mit Daten Access Vorteile von Personen in einer Einzelpartition Gruppierung abstimmen.  

Ein wesentlicher Vorteil zum Gruppieren von einer Gruppe von Personen in einer Tabelle in eine einzelne Partition ist, dass es möglich ist, Operationen atomaren Stapel mehreren Personen in der gleichen Partition, da eine Partition auf einem einzelnen Server vorhanden ist. Daher, wenn Sie Stapel Vorgänge auf eine Gruppe von Elementen ausführen möchten, sollten Sie sie mit der gleichen Partitionsschlüssel gruppieren. 

Andererseits, Einheiten, in der gleichen werden, Tabelle, aber andere Partition Schlüssel besitzen können werden Lastenausgleich auf verschiedene Server, wodurch es möglich ist, dass breiter skalierbar.

Detaillierte Empfehlungen für das Erstellen eines Konzepts Partitionierungsstrategie für Tabellen gefunden werden können [hier](https://msdn.microsoft.com/library/azure/hh508997.aspx).

## <a name="see-also"></a>Siehe auch

- [Preise Details Speicher](https://azure.microsoft.com/pricing/details/storage/)
- [Azure-Abonnement und Beschränkungen Service, Kontingente und Einschränkungen](../azure-subscription-service-limits.md)
- [Premium Speicher: Leistungsstarke Storage für Auslastung Azure-virtuellen Computern](storage-premium-storage.md)
- [Replikation Azure-Speicher](storage-redundancy.md)
- [Microsoft Azure-Speicher Leistung und Skalierbarkeit Checkliste](storage-performance-checklist.md)
- [Microsoft Azure-Speicher: Einen hoch verfügbaren Speicherplatz Clouddienst mit signifikante Konsistenz](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)
