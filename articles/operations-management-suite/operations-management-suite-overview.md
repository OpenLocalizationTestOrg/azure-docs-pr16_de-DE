<properties
   pageTitle="Übersicht über die Vorgänge Management Suite (OMS) | Microsoft Azure"
   description="Microsoft Operations Management Suite (OMS) ist die Microsoft Cloud-basierte IT Management Lösung, die Sie verwalten und Schützen von Ihrem lokalen & cloud-Infrastruktur.  In diesem Artikel die verschiedenen Dienste, die im Lieferumfang von OMS bezeichnet und enthält Links zu ihren detaillierte Inhalt."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="bwren" />

# <a name="what-is-operations-management-suite-oms"></a>Was ist die Vorgänge Management Suite (OMS)?

Microsoft Operations Management Suite (OMS) ist die Microsoft Cloud-basierte IT Management Lösung, die Sie verwalten und Schützen von Ihrem lokalen & cloud-Infrastruktur.  Da OMS als einen cloudbasierten Dienst implementiert wird, können Sie es einsatzbereit schnell minimaler Investition in Infrastrukturdienste enthält.  Neue Features geleitet automatisch, speichern Sie die laufenden Wartung und Durchführen eines Upgrades Kosten.

Zusätzlich zur Bereitstellung wertvolle Dienste in einem eigenen können OMS System Center-Komponenten, wie z. B. System Center Operations Manager zum Erweitern Ihrer bestehenden Management Investitionen in der Cloud integrieren.  System Center und OMS können zusammen arbeiten, um eine vollständige Hybrid Verwaltungsoption bereitzustellen.

Die folgenden Abschnitte enthalten eine umfassende Beschreibung der anderen Wertbereiche OMS und den Diensten, die sie implementieren.  Sie können auf OMS Architektur für eine Übersicht der verschiedenen Komponenten OMS verweisen, bevor Sie die detaillierte Dokumentation für jede.


## <a name="insight-and-analyticsmediaoperations-management-suite-overviewicon-insight-analyticspng-insight-and-analytics"></a>![Einblicke und Analysen](media/operations-management-suite-overview/icon-insight-analytics.png) Einblicke und Analysen

[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) hilft Ihnen die sammeln, zu koordinieren, durchsuchen und wirken sich auf Log und Leistung von Daten durch Betriebssysteme und Anwendungen generiert. Sie erfahren in Echtzeit Betrieb Einsichten mithilfe von Such- und benutzerdefinierte Dashboards Millionen von Datensätzen über alle Auslastung und Servern unabhängig von deren Standort leicht zu analysieren.

Sie können Lösungen problemlos Log Analytics, die Daten gesammelt werden, um zu definieren und die Logik für die Analyse hinzufügen.  -Lösungen enthalten möglicherweise zusätzliche Funktionen sind, die automatisch mit minimalen Agents oder keine Konfiguration übermittelt wird.  Zusätzlich zur Verwendung der Analysetools von einzelnen Lösungen bereitgestellt, können Sie benutzerdefinierte Suchvorgänge über das gesamte Dataset durchführen, um Daten zwischen Betriebssysteme und Anwendungen zu koordinieren.  


## <a name="automation--controlmediaoperations-management-suite-overviewicon-automation-controlpng-automation--control"></a>![Automatisierung und Steuerelement](media/operations-management-suite-overview/icon-automation-control.png) Automatisierung und Steuerelement

Azure Automatisierung Automatisierung administrative Vorgänge mit [Runbooks](../automation/automation-runbook-types.md) , die auf der Grundlage der PowerShell und in der Cloud Azure ausgeführt wird.  Runbooks können Zugriff auf alle Produkten oder Dienstleistungen, die mit Ressourcen in anderen Wolken z. B. Amazon Web Services (AWS) einschließlich PowerShell verwaltet werden kann.  Runbooks können auch auf einem Server in Ihrem lokalen Datencenter zum Verwalten von lokaler Ressourcen ausgeführt werden.

Automatisierung Azure ermöglicht die Verwaltung der Konfiguration mit [PowerShell DSC](../automation/automation-dsc-overview.md).  Erstellen und Verwalten von DSC Ressourcen in Azure gehostet wird, und Cloud und lokale Systeme definieren und erzwingen automatisch ihre Konfiguration zuweisen.


## <a name="protection-and-recoverymediaoperations-management-suite-overviewicon-protection-recoverypng-protection-and-disaster-recovery"></a>![Schutz und Wiederherstellung](media/operations-management-suite-overview/icon-protection-recovery.png) Schutz und Wiederherstellung

[Azure Sicherung](http://azure.microsoft.com/documentation/services/backup) Ihrer Anwendung Datenschutz und stellt diese für Jahre, ohne alle Investitionen und mit minimalen Geschäftsjahre Kosten.  Sie können Daten aus physischen und virtuellen Windows-Servers sowie Auslastung wie SQL Server und SharePoint sichern.  Sie können auch durch System Center Data Protection Manager (DPM) verwendet werden geschützte Daten auf Azure für Redundanz und langfristig Speicher repliziert.

[Azure Website Wiederherstellung](http://azure.microsoft.com/documentation/services/site-recovery) beiträgt Ihrer Geschäftstätigkeit und Wiederherstellen (BCDR) nach Datenverlusten durch Replikation, Failover und Wiederherstellung lokalen Hyper-V-virtuellen Computern, VMware virtuellen Computern und physischen Windows/Linux Servern orchestriert. Sie können Autos auf einem sekundären Data Center repliziert oder Ihr Data Center erweitern, indem sie auf Azure repliziert. Website Wiederherstellung bietet auch einfache Failover und Wiederherstellung für Auslastung. Es Disaster Wiederherstellung Verfahren, wie etwa SQL Server AlwaysOn integriert und stellt Wiederherstellung Pläne für einfache Failover der Auslastung, die auf mehreren Computern gestuft werden.


## <a name="oms-security-and-compliancemediaoperations-management-suite-overviewicon-security-compliancepng-security-and-compliance"></a>![OMS Sicherheit und Kompatibilität](media/operations-management-suite-overview/icon-security-compliance.png) Sicherheit und Kompatibilität
Sicherheit und Kompatibilität können Sie erkennen, bewerten und Beheben von Sicherheit Risiken für Ihre Infrastruktur.  Dieser Features von OMS werden über mehrere Lösungen in Log Analytics implementiert, die Protokollieren von Daten und Konfiguration von Agent Systemen zur Unterstützung bei der Sicherstellung der laufenden Sicherheits Ihrer Umgebung zu analysieren.

- Die [Sicherheit und Audit Lösung](oms-security-getting-started.md ) sammelt und analysiert Sicherheitsereignisse auf verwalteten Systemen verdächtigen Aktivitäten zu identifizieren.
- Das [Modul Lösung](log-analytics-malware.md ) Berichte zum Status von Modul Schutz auf verwalteten Systemen.  
- Die Lösung System-Updates führt eine Analyse der Sicherheitsupdates und andere auf Ihren verwalteten Systemen wird aktualisiert, sodass Sie einfach Systeme identifizieren mit Anforderung der Patch.


## <a name="next-steps"></a>Nächste Schritte
- Informationen Sie zu [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics).
- Informationen Sie zu [Azure Automatisierung](../automation/automation-intro.md).
- Informationen Sie zu [Azure sichern](http://azure.microsoft.com/documentation/services/backup).
- Informationen Sie zu [Wiederherstellung Azure-Website](http://azure.microsoft.com/documentation/services/site-recovery).
