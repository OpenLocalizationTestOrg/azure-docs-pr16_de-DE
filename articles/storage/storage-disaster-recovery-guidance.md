<properties
    pageTitle="Was zu tun ist bei einem Ausfall Azure-Speicher | Microsoft Azure"
    description="Was zu tun ist bei einem Ausfall Azure-Speicher"
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>


# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>Vorgehensweise bei einem Azure-Speicher Ausfall

Bei Microsoft Arbeit wir harte um sicherzustellen, dass unserer Dienste immer verfügbar sind. Manchmal ist es erzwingt außerhalb unserer wirken sich auf uns Möglichkeiten, die den Ausfall eines Dienstes ungeplanten in einem oder mehreren Bereichen verursachen. Damit Sie diese seltenen Vorkommen helfen können, stellen wir die folgende auf hoher Ebene Anleitung für Azure Storage Services.

## <a name="how-to-prepare"></a>So bereiten Sie vor 

Es ist entscheidend, für jeden Kunden So bereiten Sie ihre eigenen solchen Plans vor. Der Leistung zum Wiederherstellen nach einem Speicher-Ausfall in der Regel umfasst sowohl Mitarbeiter und automatisierte Prozeduren und Ihre Programme in einem funktionsfähige Zustand reaktivieren möchten. Lizenzinformationen finden Sie in der Azure-Dokumentation unten, um Ihr eigenes Plan zur Wiederherstellung zu erstellen:

-   [Wiederherstellung und hohe Verfügbarkeit für Azure applications](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)

-   [Technische Anleitung Azure Stabilität](../resiliency/resiliency-technical-guidance.md)

-   [Azure Website Wiederherstellung-Dienst](https://azure.microsoft.com/services/site-recovery/)

-   [Azure Speicherreplikation](storage-redundancy.md)

-   [Azure Sicherung-Dienst](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>So erkennen 

Es wird empfohlen den Status Azure Service ermitteln zum Abonnieren der [Azure-Dienststatus-Dashboard](https://azure.microsoft.com/status/).

## <a name="what-to-do-if-a-storage-outage-occurs"></a>Was zu tun ist, wenn ein Ausfall Speicher erfolgt

Wenn eine oder mehrere Storage Services bei einem oder mehreren Bereichen vorübergehend nicht verfügbar sind, stehen zwei Optionen zur Auswahl. Wenn Sie direkten Zugriff auf Ihre Daten wünschen, erwägen Sie die Option 2.

### <a name="option-1-wait-for-recovery"></a>Option 1: Warten auf Wiederherstellung

In diesem Fall ist keine Aktion Ihrerseits erforderlich. Wir arbeiten sorgfältig, um die Verfügbarkeit Azure Service wiederherzustellen. Sie können den Dienststatus auf dem [Azure-Dienststatus-Dashboard](https://azure.microsoft.com/status/)überwachen.

### <a name="option-2-copy-data-from-secondary"></a>Option 2: Kopieren von Daten aus einer sekundären

Wenn Sie [Lesezugriff Geo redundante Speicher (RAS-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (empfohlen) für Ihre Speicherkonten ausgewählt haben, haben gelesen Sie Zugriff auf Ihre Daten aus der zweiten Region. Sie können Tools verwenden, z. B. [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md)und [Verschieben von Daten Azure Bibliothek](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) Kopieren von Daten aus der zweiten Region in ein anderes Speicherkonto in einen Bereich unimpacted, und zeigen Sie dann auf Ihre Anwendungen zu diesem Speicherkonto für beide lesen und Verfügbarkeit schreiben.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>Was Sie erwartet, wenn ein Speicher ausgeführt wird

Wenn Sie [Geo redundante Speicher (GRS)](storage-redundancy.md#geo-redundant-storage) oder [Lesezugriff Geo redundante Speicher (RAS-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (empfohlen) ausgewählt haben, werden Azure-Speicher von Daten dauerhaften in beiden Regionen (primären und sekundären) beibehalten. In beide Bereiche verwaltet Azure-Speicher ständig mehrere Replikate Ihrer Daten aus.

Wenn Sie ein regionaler Ausfall wirkt sich auf Ihrem primären Region, zuerst versuchen wir, den Dienst in diesem Bereich wiederherstellen. Abhängig von der Art der dem Datenverlust und deren Nachteile, in einigen Fällen seltenen wir zum Wiederherstellen der primären Region möglicherweise nicht. Wir wird zu diesem Zeitpunkt einen Geo-Failover ausgeführt. Die Datenreplikation Cross-Region ist eine asynchrone Prozess eine kurzen Verzögerung umfassen kann, damit es möglich ist, dass Änderungen, die noch nicht in den Bereich sekundäre repliziert wurde verloren. Sie können die ["Uhrzeit der letzten Synchronisierung" Ihres Kontos Speicher](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) um Details zu erhalten, klicken Sie auf den Replikationsstatus Abfragen.

Verschiedene Elemente in Bezug auf die Speicherung Geo Failover-Oberfläche:

-   Geo Speicherfailover wird nur durch das Team Azure-Speicher ausgelöst werden – keine Kunden Aktion erforderlich ist.

-   Ihre vorhandene Speicher-Endpunkte für Blobs, Tabellen, Warteschlangen und Dateien bleiben nach dem Failover; der DNS-Eintrag müssen aktualisiert werden, um von der primären Region zu der sekundäre Region wechseln.

-   Vor und während des Geo-Failovers brauchen Sie sich Ihr Speicherkonto aufgrund von den Einfluss der dem Datenverlust Schreibzugriff, aber Sie können weiterhin aus der sekundären lesen, wenn Ihr Speicherkonto als RAS-GRS konfiguriert wurde.

-   Wenn das Geo-Failover abgeschlossen wurde, und die DNS-Änderungen verteilt, werden der Lese- und Schreibzugriff auf Ihr Speicherkonto fortgesetzt. Sie können Abfragen, ["letzte Geo Failoverzeit" Ihres Kontos Speicher](https://msdn.microsoft.com/library/azure/ee460802.aspx) , um weitere Details zu erhalten.

-   Nach dem Failover Ihr Speicherkonto voll funktionsfähiges, aber in einen "beeinträchtigt" Status, wie er gehostet wird in einem eigenständigen Bereich mit keine Geo-Replikation möglich. Um dieses Risiko auszuschalten, wir wiederherstellen die ursprüngliche primäre Region und führen Sie dann eine Geo-Failback zum Wiederherstellen des ursprünglichen Zustands. Ist die ursprüngliche primäre Region nicht verfügbar ist, wird eine andere sekundäre Region zugeordnet werden.
Weitere Informationen hierzu der Infrastruktur Azure-Speicher Geo Replikation Lizenzinformationen finden Sie im Artikel im Speicher-Teamblog zu [Optionen für Hardwareredundanz und RAS-GRS](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

##<a name="best-practices-for-protecting-your-data"></a>Bewährte Methoden zum Schutz von Daten

Es gibt einige empfohlenen Vorgehensweisen sichern Sie Ihre Daten in regelmäßigen Abständen aus.

-   Virtueller Computer Datenträger – mit [Sicherung Azure Service](https://azure.microsoft.com/services/backup/) können Sie die von Ihrer Azure-virtuellen Computern verwendete virtueller Computer Datenträger sichern.

-   Blockieren blobs – Erstellen einer [Momentaufnahme](https://msdn.microsoft.com/library/azure/hh488361.aspx) aller Blob blockieren oder kopieren Sie die Blobs in einem anderen Ordner in einer anderen Region [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md)oder in der [Bibliothek Azure Verschieben von Daten](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)zu berücksichtigen.

-   Tabellen – verwenden [AzCopy](storage-use-azcopy.md) , um die Daten in ein anderes Speicherkonto in einem anderen Region exportieren.

-   Dateien – mit [AzCopy](storage-use-azcopy.md) oder [Azure PowerShell](storage-powershell-guide-full.md) können um Ihre Dateien an ein anderes Speicherkonto in einem anderen Bereich zu kopieren.
