<properties
    pageTitle="Was zu tun ist im Falle einer Azure service Unterbrechung, die Azure Cloud Services wirkt sich auf | Microsoft Azure"
    description="Erfahren Sie, was zu tun ist im Falle einer Unterbrechung Azure Service, die Azure Cloud Services auswirkt."
    services="cloud-services"
    documentationCenter=""
    authors="kmouss"
    manager="drewm"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="cloud-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Was zu tun ist im Falle einer Azure service Unterbrechung, die Azure Cloud Services wirkt sich auf

Bei Microsoft Arbeit wir harte um sicherzustellen, dass unserer Dienste immer zur Verfügung stehen, wenn Sie benötigt. Erzwingt außerhalb unserer Kontrolle auswirken uns manchmal Möglichkeiten, die ungeplanten Ausfällen verursachen.

Microsoft stellt einen Dienst Ebene Vertrag SERVICELEVEL für seine Services als Verpflichtung für Verfügbarkeit und Verbindungsproblemen. Der Vereinbarung zum SERVICELEVEL für einzelne Azure-Dienste finden Sie unter [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

Azure haben bereits viele integrierte Plattformfunktionen, die hoch verfügbare Anwendungen zu unterstützen. Lesen Sie für Weitere Informationen zu dieser Dienste [Wiederherstellung und hohe Verfügbarkeit für Azure Applications](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)aus.

Dieser Artikel behandelt ein Szenario zur Wiederherstellung nach true, wenn eine gesamte Region ein Ausfall aufgrund großer natürlich Datenverlust oder weit verbreitet dienstunterbrechung auftritt. Hierbei handelt es sich um seltene vorkommen, aber Sie müssen die Möglichkeit, dass eine derzeit nicht zur Verfügung für eine gesamte Region vorbereiten. Wenn Sie eine ganze Region einer dienststörung auftritt, wäre die lokal redundanten Kopien Ihrer Daten vorübergehend nicht verfügbar. Wenn Sie Geo-Replikation aktiviert haben, werden drei zusätzliche Kopien Ihrer Azure-Speicher Blobs und Tabellen in einem anderen Bereich gespeichert. Im Falle einer abgeschlossen regionalen Ausfall oder eine in dem die primäre Region nicht wiederhergestellt werden kann, ordnet Azure alle DNS-Einträge in der Region Geo repliziert.

>[AZURE.NOTE]Achten Sie darauf, dass Sie verfügen nicht über die Kontrolle über dieses Verfahren und nur für Datacenter organisationsweite Ausfällen tritt. Daher müssen Sie auch auf andere anwendungsspezifische Strategien in der höchsten Ebene über die Verfügbarkeit von erzielen verlassen. Weitere Informationen finden Sie im Abschnitt zu den [Daten Strategien für die Wiederherstellung](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md#DSDR). Wenn Sie Ihre eigenen Failover beeinflussen können möchten, sollten Sie erwägen Sie die Verwendung von [Lesezugriff Geo redundante Speicher (RAS-GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage)der eine schreibgeschützte Kopie der Daten in einem anderen Region erstellt.

Damit Sie diese seltenen Vorkommen helfen können, stellen wir die folgende Anleitung für Azure-virtuellen Computern (virtuellen Computern) im Falle einer dienststörung des gesamten Bereichs, wo eine Anwendung Azure-virtuellen Computer bereitgestellt wird.

##<a name="option-1-wait-for-recovery"></a>Option 1: Warten auf Wiederherstellung
In diesem Fall ist keine Aktion Ihrerseits erforderlich. Wissen Sie, dass Azure Teams sorgfältig arbeiten, um die Verfügbarkeit von Diensten wiederherzustellen. Sie können den aktuellen Dienststatus auf unsere [Azure-Dienststatus-Dashboard](https://azure.microsoft.com/status/)anzeigen.

>[AZURE.NOTE]Dies ist die beste Option, wenn ein Kunde noch nicht Azure Website Wiederherstellung einrichten oder verfügt über eine sekundäre Bereitstellung in einem anderen Bereich.

Für Kunden, die direkten Zugriff auf ihre bereitgestellten Clouddienste wünschen, stehen die folgenden Optionen zur Verfügung.

>[AZURE.NOTE]Achten Sie darauf, dass diese Optionen die Möglichkeit, Daten verloren haben.     

##<a name="option-2-re-deploy-your-cloud-service-configuration-to-a-new-region"></a>Option 2: Erneut bereitstellen Sie Ihrer Cloud Dienstkonfiguration neue Region

Wenn Sie den ursprünglichen Code verfügen, können Sie einfach nur die Anwendung, verbundene Konfiguration und zugeordneten Ressourcen zu einem neuen Cloud-Dienst in einer neuen Region erneut bereitstellen.  

Ausführliche Informationen zum Erstellen und Bereitstellen einer Cloud-Service-Anwendungs finden Sie unter [Erstellen und Bereitstellen eines Cloud-Diensts](./cloud-services-how-to-create-deploy-portal.md).

Je nach Ihrer Anwendung Datenquellen müssen Sie möglicherweise die Wiederherstellungsverfahren für die Datenquelle für die Anwendung zu aktivieren.
  * Datenquellen Azure-Speicher finden Sie unter [Azure-Speicher Replikation](../storage/storage-redundancy.md#read-access-geo-redundant-storage) überprüfen Sie die Optionen, die verfügbar sind basierend auf das ausgewählte Element Replikationsmodell für eine Anwendung.
  * SQL-Datenbank Quellen, finden Sie unter [Übersicht: Business Continuity und Datenbank Wiederherstellung mit SQL-Datenbank Cloud](../sql-database/sql-database-business-continuity.md) Überprüfen auf Grundlage die verfügbaren Optionen ausgewählten Replikationsmodell für eine Anwendung.

##<a name="option-3-use-a-backup-deployment-through-azure-traffic-manager"></a>Option 3: Verwenden Sie eine Sicherungskopie Bereitstellung über Azure Datenverkehr-Manager
Diese Option wird davon ausgegangen, dass Sie Ihre Anwendungslösung mit regionalen Wiederherstellung Punkte bereits entworfen haben. Sie können diese Option verwenden, wenn Sie bereits eine sekundäre Cloud Services-Anwendung-Bereitstellung haben, die in einem anderen Bereich ausgeführt wird und über einen Datenverkehr Manager Kanal verbunden sind. Aktivieren Sie in diesem Fall die Integrität des sekundärer Bereich ein. Wenn sie fehlerfrei ist, können Sie den Datenverkehr darauf über Azure Datenverkehr Manager umleiten. Bei dieser Vorgehensweise können Sie den Datenverkehr routing Methode und Reihenfolge Failoverkonfigurationen nutzen in Azure Datenverkehr Manager. Weitere Informationen finden Sie unter [Konfigurieren von Einstellungen für den Datenverkehr-Manager](../traffic-manager/traffic-manager-overview.md#how-to-configure-traffic-manager-settings).

![Lastenausgleich Azure-Cloud-Diensten über Regionen mit Azure Datenverkehr Manager](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

##<a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Implementieren eines Wiederherstellung und Strategie für hohe Verfügbarkeit finden Sie unter [Wiederherstellung und hohe Verfügbarkeit für Azure Applications](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Um ein detailliertes technischen Verständnis der Funktionen, die eine Cloud entwickeln, finden Sie unter [Azure Stabilität technische Anleitung](../resiliency/resiliency-technical-guidance.md).

Wenden Sie sich an den [Kundensupport](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade), wenn die Anweisungen sind nicht löschen, oder wenn Sie Microsoft, um die Aktionen in Ihrem Auftrag ausführen möchten.
