<properties
    pageTitle="Was zu tun ist, wenn eine Azure dienststörung wirkt sich auf die Azure-virtuellen Computern | Microsoft Azure"
    description="Erfahren Sie, was zu tun ist, wenn eine Azure dienststörung wirkt sich auf die Azure-virtuellen Computern."
    services="virtual-machines"
    documentationCenter=""
    authors="kmouss"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="virtual-machines"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-virtual-machines"></a>Was zu tun ist, wenn eine Azure dienststörung wirkt sich auf die Azure-virtuellen Computern

Bei Microsoft Arbeit wir harte um sicherzustellen, dass unserer Dienste immer zur Verfügung stehen, wenn Sie benötigt. Erzwingt außerhalb unserer Kontrolle auswirken uns manchmal Möglichkeiten, die ungeplanten Ausfällen verursachen.

Microsoft stellt einen Dienst Ebene Vertrag SERVICELEVEL für seine Services als Verpflichtung für Verfügbarkeit und Verbindungsproblemen. Der Vereinbarung zum SERVICELEVEL für einzelne Azure-Dienste finden Sie unter [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

Azure haben bereits viele integrierte Plattformfunktionen, die hoch verfügbare Anwendungen zu unterstützen. Lesen Sie für Weitere Informationen zu dieser Dienste [Wiederherstellung und hohe Verfügbarkeit für Azure Applications](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)aus.

Dieser Artikel behandelt ein Szenario zur Wiederherstellung nach true, wenn eine gesamte Region ein Ausfall aufgrund großer natürlich Datenverlust oder weit verbreitet dienstunterbrechung auftritt. Hierbei handelt es sich um seltene vorkommen, aber Sie müssen die Möglichkeit, dass eine derzeit nicht zur Verfügung für eine gesamte Region vorbereiten. Wenn Sie eine ganze Region einer dienststörung auftritt, wäre die lokal redundanten Kopien Ihrer Daten vorübergehend nicht verfügbar. Wenn Sie Geo-Replikation aktiviert haben, werden drei zusätzliche Kopien Ihrer Azure-Speicher Blobs und Tabellen in einem anderen Bereich gespeichert. Im Falle einer abgeschlossen regionalen Ausfall oder eine in dem die primäre Region nicht wiederhergestellt werden kann, ordnet Azure alle DNS-Einträge in der Region Geo repliziert.

>[AZURE.NOTE]Achten Sie darauf, dass Sie verfügen nicht über die Kontrolle über dieses Verfahren und nur für Region organisationsweite Ausfällen tritt. Daher müssen Sie auch auf andere anwendungsspezifische Strategien in der höchsten Ebene über die Verfügbarkeit von erzielen verlassen. Weitere Informationen finden Sie im Abschnitt auf [Daten Strategien für die Wiederherstellung](../resiliency/resiliency-disaster-recovery-azure-applications.md#data-strategies-for-disaster-recovery)ein.

Damit Sie diese seltenen Vorkommen helfen können, stellen wir die folgende Anleitung für Azure-virtuellen Computern im Fall einer dienststörung des gesamten Bereichs, wo Ihre Azure-virtuellen Computern Anwendung bereitgestellt wird.

##<a name="option-1-wait-for-recovery"></a>Option 1: Warten auf Wiederherstellung
In diesem Fall ist keine Aktion Ihrerseits erforderlich. Wissen Sie, dass wir sorgfältig arbeiten, um die Verfügbarkeit von Diensten wiederherzustellen. Sie können den aktuellen Dienststatus auf unsere [Azure-Dienststatus-Dashboard](https://azure.microsoft.com/status/)anzeigen.

>[AZURE.NOTE]Dies ist die beste Option, wenn Sie keine Azure Website Wiederherstellung, virtuellen Computern sichern, Lesezugriff Geo redundante Speicher oder Geo redundante Speicher vor der Unterbrechung eingerichtet haben. Wenn Sie, Geo redundante Speicherung oder Lesezugriff Geo redundante Speicher für das Speicherkonto eingerichtet haben, in dem Ihre virtuellen Festplatten (virtuelle Festplatten) für virtuellen Computer gespeichert sind, können Sie zum Wiederherstellen der Basis-Image virtuelle Festplatte suchen und versuchen, einen neuen virtuellen Computer es bereitstellen. Dies ist eine bevorzugte Option nicht, da es gibt keine Garantie der Synchronisierung von Daten, es sei denn, Azure-virtuellen Computer Sicherung oder Wiederherstellung der Azure-Website verwendet werden. Daher wird diese Option nicht unbedingt arbeiten.

Für Kunden, die direkten Zugriff auf virtuellen Computern wünschen, stehen zwei Optionen zur Verfügung.  

>[AZURE.NOTE]Achten Sie darauf, dass beide der folgenden Optionen die Möglichkeit, Daten verloren haben.     

##<a name="option-2-restore-a-vm-from-a-backup"></a>Option 2: Stellen eines virtuellen Computers aus einer Sicherung wieder her.
Für Kunden, die eine Sicherung virtueller Computer konfiguriert haben, können Sie den virtuellen Computer ab dem Punkt Sicherung und Wiederherstellung wiederherstellen.

Zum Wiederherstellen eines neuen virtuellen Computers aus Azure Sicherung finden Sie unter [virtuelle Computer in Azure wiederherstellen](../backup/backup-azure-restore-vms.md).

Damit Sie für die Sicherungsdatei Azure-virtuellen Computern-Infrastruktur planen können, finden Sie unter [Planen einer Sicherung virtueller Computer-Infrastruktur Azure](../backup/backup-azure-vms-introduction.md).

##<a name="option-3-initiate-a-failover-by-using-azure-site-recovery"></a>Option 3: Aufstellen eines Failovers mithilfe von Azure Website Wiederherstellung
Wenn Sie Azure Website Wiederherstellung für die Arbeit mit der betroffenen Azure-virtuellen Computern konfiguriert haben, können Sie Ihre virtuellen Computern aus sich wiederherstellen. Diese Replikationen können auf Azure oder lokal befinden. In diesem Fall können Sie einen neuen virtuellen Computer von der vorhandenen Replikation erstellen. Um Ihre virtuellen Computern aus einer Kopie Azure Website Wiederherstellung wiederherstellen möchten, finden Sie unter [Migrieren von Azure IaaS virtuellen Computern zwischen Azure Regionen mit Azure Website Wiederherstellung](../site-recovery/site-recovery-migrate-azure-to-azure.md).

>[AZURE.NOTE]Obwohl Azure-virtuellen Computern Betriebssystem und Datenträger mit Daten zu einer sekundären virtuellen, repliziert werden, wenn sie in einer Geo redundante Speicherung oder Geo redundante Lesezugriff Speicherkonto sind, wird jede virtuelle Festplatte unabhängig voneinander repliziert. Diese Stufe der Replikation nicht Konsistenz über die replizierten virtuellen Festplatten sichergestellt ist. Wenn Ihre Anwendung und/oder Datenbanken mit diesen Datenfestplatten Abhängigkeiten voneinander abhängig sind, ist es nicht sichergestellt, dass alle virtuellen Festplatten als eine Momentaufnahme repliziert werden. Es ist auch nicht sichergestellt, dass das virtuelle Festplatte Replikat Geo redundante Speicherung oder Lesezugriff Geo redundante Speicher eine einheitliche Momentaufnahme der Anwendung, starten den virtuellen Computer führt.

##<a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Implementieren eines Wiederherstellung und Strategie für hohe Verfügbarkeit finden Sie unter [Wiederherstellung und hohe Verfügbarkeit für Azure Applications](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Um ein detailliertes technischen Verständnis der Funktionen, die eine Cloud entwickeln, finden Sie unter [Azure Stabilität technische Anleitung](../resiliency/resiliency-technical-guidance.md).

Sichern von virtuellen Computern finden Sie unter [Azure-virtuellen Computern sichern](../backup/backup-azure-vms.md).

Erfahren Sie, wie Azure Website Wiederherstellung koordinieren und Schutz Ihrer physischen (und virtuellen) unter Windows und Linux Maschinen, die auf VMWare und Hyper-V virtuelle Computer ausgeführt werden, finden Sie unter [Azure Website Wiederherstellung](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)automatisieren.

Wenn die Anweisungen sind nicht deaktivieren oder wenn Sie Microsoft, um die Aktionen in Ihrem Auftrag ausführen möchten, wenden Sie sich an den [Kundensupport](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
