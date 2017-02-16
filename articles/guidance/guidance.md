
<properties
   pageTitle="Azure Anleitungen | Muster und Methoden | Microsoft Azure"
   description="Empfohlene Vorgehensweisen und Leitfaden für Azure"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="christb"/>

# <a name="azure-guidance"></a>Azure Anleitungen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Das Microsoft Muster & Methoden-Team ist Teil des Kunden Ihren Azure Teams. Unsere Zweck ist Entwickler, Architekten und IT-Experten auf der Microsoft Azure-Plattform erfolgreich durchgeführt werden können. Wir entwickeln Leitfäden, die optimale Methoden zum Erstellen von Cloudlösungen auf Azure zeigt.

## <a name="checklists"></a>Checklisten

Diese Listen sind eine Kurzübersicht über den grundlegenden Aspekten der Verfügbarkeit und Skalierbarkeit überprüfen. 

- [Verfügbarkeitscheckliste][AvailabilityChecklist] 

    Eine Zusammenfassung der empfohlenen Methoden für die Sicherstellung der Verfügbarkeit.

- [Checkliste Skalierbarkeit][ScalabilityChecklist]

    Eine Zusammenfassung der empfohlenen Methoden für das Entwerfen und implementieren skalierbare Services und datenverwaltung behandeln.

## <a name="best-practices-articles"></a>Bewährte Methoden Artikel

Dieser Artikel enthält eine ausführliche Erläuterung von wichtige Konzepte im Zusammenhang mit der Cloud computing. 

- [API-Design][APIDesign] 

    Eine Erläuterung der Entwurfsprobleme beim Entwerfen einer Webs-API zu berücksichtigen.

- [API-Implementierung][APIImplementation] 

    Eine Reihe von empfohlene Vorgehensweisen für das Implementieren und eine Web-API veröffentlichen.

- [Leitfaden für API-Sicherheit](https://github.com/mspnp/azure-guidance/blob/master/API-security.md) 

    Eine Erläuterung der Authentifizierung und Autorisierung Bedenken (z. B. token Typen, Autorisierung Protokolle, Autorisierung Zahlungen und Bedrohung Reduzierung).

- [Automatische Skalierung Anleitungen][AutoscalingGuidance] 

    Eine Zusammenfassung der Aspekte für die Skalierung Lösungen ohne die Notwendigkeit der manuellen Eingriff.

- [Hintergrund Aufträge Anleitungen][BackgroundJobsGuidance] 

    Eine Beschreibung der verfügbaren Optionen und empfohlene Vorgehensweisen für die Durchführung von Aufgaben, die in den Hintergrund, unabhängig von einem beliebigen Vordergrund- oder interaktiven Vorgänge durchgeführt werden soll.

- [Leitfaden für Content Delivery Network (CDN)][CDNGuidance] 

    Allgemeine Anleitung und empfiehlt sich für die Verwendung des CDN die Belastung Ihrer Anwendung minimieren und Maximieren Verfügbarkeit und Leistung.

- [Anleitungen Zwischenspeichern][CachingGuidance] 

    Eine Zusammenfassung der Verwendung von verbessern die Leistung und Skalierbarkeit eines Systems zum Zwischenspeichern.

- [Daten Partitioning Anleitungen][DataPartitioningGuidance]

    Strategien, dass Sie Daten verwenden können, zur Verbesserung der Skalierbarkeit, Konflikte verringern und Leistung zu optimieren.

- [Hinweise zur Überwachung und Diagnose][MonitoringandDiagnosticsGuidance] 

    Leitfaden zum verfolgen, wie Ihre Benutzer nutzen von Ihrem System, Ressourcen Auslastung verfolgen und Überwachen von Zustand und Leistung von Ihrem System in der Regel.

- [Empfohlene Benennungskonvention][naming-conventions] 

    Empfohlene Benennungskonvention für Azure Ressourcen.

- [Wiederholen Sie die allgemeinen Leitfaden][RetryGeneralGuidance] 

    Erläuterung der allgemeinen Konzepte für den Umgang mit vorübergehenden Fehler.

- [Wiederholen Sie Service-spezifische Leitfäden][RetryServiceSpecificGuidance]

    Eine Zusammenfassung der Features von "Wiederholen" für zahlreiche Azure Services, einschließlich Informationen dazu verwenden, anpassen oder erweitern das "Wiederholen" Verfahren für diesen Dienst.

## <a name="scenario-guides"></a>Szenario Führungslinien

- [Elasticsearch auf Windows Azure ausgeführte][elasticsearch] 
    
    Elasticsearch ist eine hochgradig skalierbare öffnen Source-Suchmaschine und die Datenbank. Eignet sich für Situationen, in denen schnelle Analyse und Suche nach Informationen in den großen Datasets erfordern. Dieser Leitfaden untersucht einige wichtige Aspekte zu berücksichtigen beim Entwerfen eines Elasticsearch Clusters aus.

- [Identitätsmanagement für Applikationen mandantenfähigen][identity-multitenant] 
    
    Tenancy ist eine Architektur, in dem mehrere Mandanten Freigeben der Anwendung jedoch voneinander isoliert sind. Dieser Leitfaden zeigt, wie Sie die Verwaltung von Benutzeridentitäten in einer mandantenfähigen mit [Azure Active Directory] Anwendung[ AzureAD] Anmeldung und Authentifizierung behandelt.
    
- [Zur Entwicklung von Lösungen für große Daten](https://msdn.microsoft.com/library/dn749874.aspx)

    Dieser Leitfaden erläutert die Verwendung von HDInsight für Szenarien wie iterative Untersuchung, die als ein Datawarehouse, für ETL-Prozesse und Integration in vorhandene BI-Systeme. Darüber hinaus Anleitungen Verständnis der Konzepte des big Data, Planen und Erstellen eines Konzepts big Data Lösungen und implementieren diese Lösungen.
    
## <a name="patterns"></a>Muster

- [Cloud Entwurfmustern: Nachschlagewerke Architektur Anleitungen für Applikationen Cloud](https://msdn.microsoft.com/library/dn568099.aspx)

    Cloud Entwurfmustern ist eine Bibliothek von entwurfmustern und Anleitungen verwandten Themen. Diese Hervorhebung der Vorteile der Anwendung Muster durch mit, wie jedes Blatt in der Cloud Anwendungsarchitekturen dargestellt werden kann.
    
- [Optimieren der Leistung für Applikationen Cloud](https://github.com/mspnp/performance-optimization)

    Diese Anleitung gilt einer Untersuchung von allgemeine Anti-Muster, die apps aus Auslastung Skalierung beeinträchtigen. Er enthält Beispiele acht Anti-Mustern und eine [Einführung in Analyse der Leistung](https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md) und einen Leitfaden für die [Bewertung der Leistung gegen wichtigen Kriterien](https://github.com/mspnp/performance-optimization/blob/master/Assessing-System-Performance-Against-KPI.md)aus.

## <a name="reference-architectures"></a>Referenz-Architekturen

Unsere Bezug Architekturen werden nach Szenario angeordnet.
Jede einzelne Architektur bietet empfohlene Vorgehensweisen und Schritte sowie eine ausführbare Komponente, die die Empfehlungen enthält.

Die aktuelle Bibliothek Bezug Architekturen ist unter [http://aka.ms/architecture](http://aka.ms/architecture)verfügbar.

## <a name="resiliency-guidance"></a>Stabilität Anleitungen

Die folgenden Themen beschreiben zum Entwerfen von Applications, die flexibel in auf Fehler in einer verteilten Cloud-Umgebung Bezug.   

- [Stabilität (Übersicht)][ResiliencyOvervew]

     So erstellen Sie auf der Azure-Plattform Applications, die von Fehlern wiederherstellen können, und fahren Sie mit der Funktion. Beschreibt einen strukturierten Ansatz zum Stabilität, basierend auf einem Design Implementierung, Bereitstellung und Vorgänge zu erzielen.

- [Stabilität Checkliste][resiliency-checklist]

    Eine Prüfliste mit empfohlenen, die Ihnen helfen Planen einer Vielzahl von Fehlermodi, die auftreten können.

- [Fehler bei der Modus Analyse][resiliency-fma] 

    Fehler bei der Modus Analyse (FMA) umfasst zum Erstellen von Stabilität in einem System, indem Sie möglicherweise Fehlerpunkte. Dieser Artikel enthält als Ausgangspunkt für den Prozess FMA einen Katalog von mögliche Fehlermodi und deren Problembehebungen an. 

<!-- links -->

[AzureAD]: https://azure.microsoft.com/documentation/services/active-directory/

[PerformanceOptimization]: https://github.com/mspnp/performance-optimization

[APIDesign]: ../best-practices-api-design.md
[APIImplementation]: ../best-practices-api-implementation.md
[AutoscalingGuidance]: ../best-practices-auto-scaling.md
[BackgroundJobsGuidance]: ../best-practices-background-jobs.md
[CDNGuidance]: ../best-practices-cdn.md
[CachingGuidance]: ../best-practices-caching.md
[DataPartitioningGuidance]: ../best-practices-data-partitioning.md
[MonitoringandDiagnosticsGuidance]: ../best-practices-monitoring.md
[RetryGeneralGuidance]: ../best-practices-retry-general.md
[RetryServiceSpecificGuidance]: ../best-practices-retry-service-specific.md
[RetryPolicies]: Retry-Policies.md
[ScalabilityChecklist]: ../best-practices-scalability-checklist.md
[AvailabilityChecklist]: ../best-practices-availability-checklist.md
[naming-conventions]: guidance-naming-conventions.md

<!-- guidance projects -->
[elasticsearch]: guidance-elasticsearch.md
[identity-multitenant]: guidance-multitenant-identity.md

<!-- reference architectures -->
[ref-arch-single-vm-windows]: guidance-compute-single-vm.md
[ref-arch-single-vm-linux]: guidance-compute-single-vm-linux.md
[ref-arch-multi-vm]: guidance-compute-multi-vm.md
[ref-arch-3-tier]: guidance-compute-3-tier-vm.md
[ref-arch-n-tier-windows]: guidance-compute-n-tier-vm.md
[ref-arch-n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[ref-arch-multi-dc-windows]: guidance-compute-multiple-datacenters.md
[ref-arch-multi-dc-linux]: guidance-compute-multiple-datacenters-linux.md

<!-- resiliency -->
[resiliency-fma]: guidance-resiliency-failure-mode-analysis.md
[resiliency-checklist]: guidance-resiliency-checklist.md
[ResiliencyOvervew]: guidance-resiliency-overview.md

