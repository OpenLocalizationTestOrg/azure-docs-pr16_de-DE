<properties
    pageTitle="Welche Auslastung können Sie mit Azure Website Wiederherstellung schützen?"
    description="Azure Website Wiederherstellung Loss Ihre Auslastung und Applikationen durch die Replikation, Failover- und der lokalen virtuellen Computern und physischen Servern Azure oder einer Website sekundäre lokalen koordinieren"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>

# <a name="what-workloads-can-you-protect-with-azure-site-recovery"></a>Welche Auslastung können Sie mit Azure Website Wiederherstellung schützen?


In diesem Artikel werden die Auslastung und Anwendungen, die mit dem Dienst Azure Website Wiederherstellung repliziert werden können.

Posten Sie Kommentare oder Fragen am Ende dieses Artikels oder im [Azure Wiederherstellung Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>(Übersicht)

Organisationen benötigen eine Business Continuity- und Disaster Wiederherstellung (BCDR) Strategie beibehalten Auslastung und Daten sicher und verfügbar sind bei geplanten und ungeplanten Ausfällen und so früh wie möglich um reguläre Arbeitsbedingungen wiederherstellen.

Wiederherstellung Website ist eine Azure-Dienst, der zur strategische BCDR beiträgt. Verwenden die Website Wiederherstellung, können Sie anwendungsspezifische Replikation in der Cloud oder an einem sekundären Standort bereitstellen. Führen Sie, ob Ihre apps Windows sind oder Linux-basierten, laufenden auf physischen Servern, VMware oder Hyper-V, Sie Website Wiederherstellung verwenden können, um die Replikation koordinieren, Disaster Wiederherstellung testen, und Ausführen Failovers und Failback.


Website Wiederherstellung integriert mit Microsoft-Anwendungen, einschließlich SharePoint, Exchange, Dynamics, SQL Server und Active Directory. Microsoft arbeitet außerdem eng mit führenden Anbietern, einschließlich Oracle, SAP, IBM und Red Hat. Sie können die Replikation auf Basis app-von-app anpassen.

## <a name="why-use-site-recovery-for-application-replication"></a>Gründe für die Verwendung Website Wiederherstellung für die Anwendungsreplikation?

Website Wiederherstellung trägt zur Anwendung Ebene Schutz und Wiederherstellung wie folgt ein:

- App-unabhängige, Replikation für alle Auslastung auf einem unterstützten Computer bereitstellen.
- In der Nähe synchroner Replikation mit RPOs so niedrig wie 30 Sekunden dauern, bis Sie den Bedürfnissen der wichtigsten Geschäfts-apps.
- App-konsistente Momentaufnahmen für einzelne oder mehrere Ebenen Applikationen.
- Integration in SQL Server AlwaysOn und Zusammenarbeit mit anderen Anwendung Ebene Replikation Technologien, einschließlich Active Directory-Replikation SQL AlwaysOn, Exchange-Datenbank Verfügbarkeit Gruppen (DAGs) und Oracle Data Guard.
- Flexible Wiederherstellung Pläne, mit denen Sie einen gesamte Anwendungsstapel mit einem einzigen Klick wiederherstellen und einschließen, um externe Skripts und manuelle Aktionen in den Plan einschließen.
- Erweitertes Netzwerk-Management in Website Wiederherstellung und Azure app Netzwerk Anforderungen, einschließlich der Möglichkeit zum IP-Adressen reservieren vereinfachen konfigurieren Lastenausgleich und Integration mit Azure Datenverkehr Manager für niedrig RTO Netzwerk Serverarrays.
-  Eine Rich-Automatisierungsbibliothek, die einsatzbereit, anwendungsspezifische Skripts bereitstellt, die heruntergeladen und Wiederherstellung Pläne integriert werden können.



## <a name="workload-summary"></a>Zusammenfassung Arbeitsbelastung

Website Wiederherstellung kann eine beliebige app auf einem unterstützten Computer repliziert werden. Darüber hinaus haben wir mit Produktteams zusätzliche app spezifische Tests ausführen zu können.

**Arbeitsbelastung** | **Hyper-V virtuellen Computern an einem sekundären Standort repliziert** | **Hyper-V virtuelle Computer auf Azure repliziert** | **Virtuelle VMware-Computer an einem sekundären Standort repliziert** | **Virtuelle VMware-Computer auf Azure repliziert**
---|---|---|---|---
Active Directory, DNS | Y | Y | Y | Y
Web apps (IIS, SQL) | Y | Y | Y | Y
System Center Operations Manager | Y | Y | Y | Y
SharePoint | Y | Y | Y | Y
SAP<br/><br/>SAP-Website in Azure für nicht Cluster repliziert | Y (von Microsoft getestet) | Y (von Microsoft getestet) | Y (von Microsoft getestet) | Y (von Microsoft getestet)
Exchange (nicht so) | Y | Demnächst | Y | Y
Remote Desktop-VDI | Y | Y | Y | N/V
Linux (Betriebssystem und apps) | Y (von Microsoft getestet) | Y (von Microsoft getestet) | Y (von Microsoft getestet) | Y (von Microsoft getestet)
Dynamics AX | Y | Y | Y | Y
Dynamics CRM | Y | Demnächst | Y | Demnächst
Oracle | Y (von Microsoft getestet) | Y (von Microsoft getestet) | Y (von Microsoft getestet) | Y (von Microsoft getestet)
Windows-Dateiserver | Y | Y | Y | Y


## <a name="replicate-active-directory-and-dns"></a>Repliziert Active Directory und DNS

Zuweisen einer Active Directory und DNS-Infrastruktur sind für die meisten Enterprise-apps unverzichtbar. Während der Wiederherstellung müssen Sie schützen und Wiederherstellen von dieser Infrastrukturkomponenten vor dem Wiederherstellen Ihrer Auslastung und apps.

Website Wiederherstellung können zum Erstellen eines Plans abgeschlossen automatisierte zur Wiederherstellung für Active Directory und DNS. Beispielsweise, wenn Sie möchten ein Fehler auftreten, SharePoint und SAP-von einer primären an einem sekundären Standort, können Sie Einrichten eines Plans für die Wiederherstellung, die über Active Directory zuerst schlägt fehl, und dann einen Wiederherstellungsplan Weitere app-spezifische über die andere apps fehlschlägt, die auf Active Directory basieren.

[Weitere](site-recovery-active-directory.md) Informationen zum Schutz von Active Directory und DNS.

## <a name="protect-sql-server"></a>Schützen von SQLServer

SQL Server stellt eine Data Services Grundlage für Data Services für viele Geschäfts-apps in einem lokalen Data Center.  Website Wiederherstellung kann zusammen mit SQL Server HA/DR-Technologien verwendet werden, zum Schutz von mehreren Ebenen Enterprise-apps, die SQL Server verwenden. Website Wiederherstellung bietet:

- Ein einfacher und kostengünstiger Disaster Wiederherstellung Lösung für SQL Server. Mehrere Versionen und Editionen von SQL Server eigenständigen Server und Cluster, Azure oder an einem sekundären Standort repliziert werden soll.  
- Integration in SQL AlwaysOn Verfügbarkeit Gruppen Failover und Failback mit Azure Website Wiederherstellung Wiederherstellung Pläne verwalten.
- End-to-End-Wiederherstellung Pläne für die alle Ebenen in einer Anwendung, einschließlich der SQL Server-Datenbanken.
- Skalierung der SQL Server für Höchstwert lädt mit Website Wiederherstellung, diese in IaaS virtuellen Computers größere in Azure "Trennen".
- Der SQL Server-Wiederherstellung einfach testen. Sie können zum Analysieren von Daten und Compliance-Prüfungen ausführen, ohne zu beeinträchtigen Ihrer Umgebung für die Herstellung Testfailovers ausführen.

[Weitere](site-recovery-sql.md) Informationen zum Schutz von SqlServer.

##<a name="protect-sharepoint"></a>Schützen von SharePoint

Azure Website Wiederherstellung hilft, SharePoint-Bereitstellungen wie folgt schützen:

- Die müssen und die zugehörige Infrastrukturkosten für einen Standby-Farm für die Wiederherstellung vermieden werden. Verwenden Sie Website Wiederherstellung, um eine gesamte Farm (Web, app und Datenbank Ebenen) in Azure oder an einem sekundären Standort repliziert.
- Vereinfacht die Bereitstellung der Anwendung und Verwaltung. Updates auf dem primären Standort bereitgestellt werden automatisch repliziert und somit nach Failover und Wiederherstellung einer Farm an einem sekundären Standort verfügbar sind. Auch Grundlinie die Management-Komplexität und Kosten im Zusammenhang mit eine Standby-Farm auf dem neuesten Stand.
- Vereinfacht die SharePoint-Anwendungsentwicklung und Tests durch Erstellen einer Kopie der Herstellung-wie bei Bedarf Replikat Umgebung zum Testen und Debuggen.
- Vereinfacht Übergang in die Cloud zum Migrieren von SharePoint-Bereitstellungen zu Azure mithilfe von Website-Wiederherstellung.

[Weitere](https://gallery.technet.microsoft.com/SharePoint-DR-Solution-f6b4aeae) Informationen zum Schutz von SharePoint.


## <a name="protect-dynamics-ax"></a>Schützen von Dynamics AX

Azure Website Wiederherstellung hilft, Ihre Dynamics AX ERP-Lösung von schützen:

- Orchestriert Replikation der gesamten ANGEHÖREN Umgebung (Web und AOS Ebenen, Datenbank Ebenen, SharePoint) Azure oder einem sekundären Standort.
- Vereinfachen der Migration von ANGEHÖREN Bereitstellungen in der Cloud (Azure).
- Vereinfachen ANGEHÖREN Anwendungsentwicklung und Tests durch Erstellen einer Kopie der Herstellung-wie bei Bedarf, zum Testen und Debuggen.

[Weitere](https://gallery.technet.microsoft.com/Dynamics-AX-DR-Solution-b2a76281) Informationen zum Schutz von dynamischen AX.

## <a name="protect-rds"></a>Schützen von RDS

Remote Desktop Services (RDS) ermöglicht virtual desktop Infrastructure (VDI), Sitzung-basierten Desktops und Anwendungen, ermöglichen, dass Benutzer überall arbeiten. Mit Azure Website Wiederherstellung können Sie folgende Aktionen ausführen:

- Verwaltete oder nicht verwaltete gepoolte virtuelle Desktops auf einer Website sekundäre und remote-Anwendungen und Sitzungen zu einer sekundären oder Azure repliziert.
- Hier ist was repliziert werden können:

**RDS** | **Hyper-V virtuellen Computern an einem sekundären Standort repliziert** | **Hyper-V virtuelle Computer auf Azure repliziert** | **Virtuelle VMware-Computer an einem sekundären Standort repliziert** | **Virtuelle VMware-Computer auf Azure repliziert** | **Physische Server auf einem sekundären Standort repliziert** | **Physische Server auf Azure repliziert**
---|---|---|---|---|---|---
**Gepoolte Virtual Desktop (nicht verwaltete)** | Ja | Nein | Ja | Nein | Ja | Nein
**Der gepoolten Virtual Desktop (verwaltete und ohne UPD)** | Ja | Nein | Ja | Nein | Ja | Nein
**Remote-Anwendungen und Desktop Sitzungen (ohne UPD)** | Ja | Ja | Ja | Ja | Ja | Ja


[Weitere](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb) Informationen zum Schützen von RDS.


## <a name="protect-exchange"></a>Schützen von Exchange

Website Wiederherstellung schützen Exchange, wie folgt:

- Für kleine Exchange-Bereitstellungen, z. B. ein einzelnes oder eigenständigen-Server können Sie Website Wiederherstellung repliziert und über ein Fehler auftreten, Azure oder an einem sekundären Standort.
- Für größere Bereitstellungen integriert Exchange DAGS Website Wiederherstellung.
- Exchange-DAGs sind die empfohlenen Lösung für die Exchange-Wiederherstellung in einem Unternehmen.  Website Wiederherstellung Wiederherstellung Pläne können DAGs, um so Failover websiteübergreifend koordinieren einbeziehen.


[Weitere](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6) Informationen zum Schutz von Exchange.

## <a name="protect-sap"></a>Schützen von SAP

Verwenden Sie Website Wiederherstellung Ihrer SAP-Bereitstellung wie folgt geschützt:

- Aktivieren des Schutzes für die gesamte SAP-Bereitstellung, da die Bereitstellung von verschiedenen Ebenen Azure oder einem sekundären Standort repliziert.
- Cloud-Migration mithilfe von Website-Wiederherstellung Ihrer SAP-Umgebung zu Azure Migrieren zu vereinfachen.
- SAP-Entwicklung zu vereinfachen und testen, durch Erstellen einer produktionsähnlichen kopieren bei Bedarf zum Testen und Debuggen von Applications.

[Weitere](http://aka.ms/asr-sap) Informationen zum Schutz von SAP.

## <a name="next-steps"></a>Nächste Schritte

[Bereiten Sie für die Bereitstellung der Website Wiederherstellung vor](site-recovery-best-practices.md) 
