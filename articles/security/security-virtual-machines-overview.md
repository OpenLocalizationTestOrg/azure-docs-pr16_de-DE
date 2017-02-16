<properties
   pageTitle="Übersicht über die Sicherheit von Azure-virtuellen Computern | Microsoft Azure"
   description=" Azure-virtuellen Computern bieten Ihnen die Flexibilität von Virtualisierung ohne zu kaufen und verwalten die physische Hardware, die den virtuellen Computern ausgeführt wird.  In diesem Artikel bietet einen Überblick über die wichtigsten Azure Sicherheitsfeatures, die mit Azure virtuellen Computern verwendet werden können. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-virtual-machines-security-overview"></a>Azure-virtuellen Computern Sicherheit (Übersicht)

Azure-virtuellen Computern können Sie eine Vielzahl von Lösungen auf eine Weise agiles computing bereitstellen. Mit der Unterstützung für Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP und Azure BizTalk-Dienste können Sie alle Arbeitsbelastung und eine beliebige Sprache auf nahezu jedem Betriebssystem bereitstellen.

Ein Azure-virtuellen Computern bietet Ihnen die Flexibilität von Virtualisierung ohne zu kaufen und verwalten die physische Hardware, die den virtuellen Computern ausgeführt wird.  Sie können erstellen und Ihre Programme für die Sicherstellung, dass Ihre Daten geschützt und sichere in unseren sehr sicheren Rechenzentren bereitstellen.

Mit Azure können Sie mit erhöhter Sicherheit kompatiblen Lösungen, die erstellen:

- Ihre virtuellen Computer vor Viren und Schadsoftware zu schützen
- Verschlüsseln Sie Ihre sensiblen Daten
- Sicherer Netzwerkdatenverkehr
- Identifizieren und Erkennen von Risiken
- Compliance-Anforderungen entsprechen

Ziel dieses Artikels ist es, einen Überblick über das Herzstück Azure Sicherheitsfeatures bieten, die mit virtuellen Computern verwendet werden können. Wir bieten auch Links zu Artikeln, die näher jedes Feature, sodass Sie mehr erfahren können.  

Die wichtigsten Azure-virtuellen Computern Sicherheitsfunktionen, die in diesem Artikel behandelt werden:

- Modul
- Hardware Security Module
- Virtuellen Computern Datenträger-Verschlüsselung
- Sichern von virtuellen Computern
- Wiederherstellung Azure-Website
- Virtuelle Netzwerke
- Sicherheit Richtlinienmanagement und reporting
- Compliance

## <a name="antimalware"></a>Modul

Mit Azure können Sie Modul Software Sicherheitsanbieter wie Microsoft, Symantec, Trend Micro, McAfee und Kaspersky bösartige Dateien, Adware und ausgefeilte Ihren virtuellen Computern gewarnt. Finden Sie unter Weitere Abschnitt unten, um Artikel auf Partner Lösungen zu finden.

Microsoft Antimalware für Azure-Cloud-Diensten und virtuellen Computern ist eine Schutz in Echtzeit-Funktion, die zu identifizieren und Entfernen von Viren, Spyware und anderer bösartiger Software.  Microsoft Antimalware bietet konfigurierbare Benachrichtigungen, wenn bekannt, dass bösartige oder unerwünschte Software versucht, installiert oder auf Ihren Azure-Systemen ausgeführt.

Microsoft Antimalware ist eine Single-Agent-Lösung für Applikationen und Mandanten Umgebungen, die im Hintergrund ohne personenbezogenen Eingriff ausgeführt werden sollen. Sie können Schutz basierend auf den Anforderungen Ihrer Auslastung, mit entweder grundlegende secure standardmäßig oder erweiterte benutzerdefinierte Konfiguration, einschließlich Modul für die Überwachung bereitstellen.

Wenn Sie bereitstellen und Microsoft Antimalware aktivieren, stehen die folgenden Core-Funktionen zur Verfügung:

- Schutz in Echtzeit - Monitore Aktivität in der Cloud Services und auf virtuellen Computern zu erkennen und Schadsoftware Ausführung blockieren.
- Geplantes Scannen - führt regelmäßig gezielte Scannen zum Ermitteln von Schadsoftware, einschließlich der Programme aktiv ausgeführt.
- Behebung von Schadsoftware - Ausführung automatische von Aktionen auf erkannten Schadsoftware, z. B. löschen oder bösartige Dateien isolieren und Bereinigen von bösartiger Registrierungseinträge.
- Signatur-Updates - Installationen die neuesten Schutzsignaturen (Virusdefinitionen) zum Schutz zu gewährleisten wird automatisch auf dem neuesten Stand auf einem zuvor festgelegten Häufigkeit.
- Modul-Engine – automatisch aktualisiert die Microsoft Antimalware-Engine aktualisiert.
- Modul Plattform – automatisch aktualisiert die Microsoft Antimalware Plattform aktualisiert.
- Aktive Schutz - Berichte zu Azure werden Metadaten zu erkannten Risiken und verdächtigen Ressourcen schnelle Antwort sicherzustellen und in Echtzeit synchroner Signatur Übermittlung über die Microsoft aktiven Schutz System (MAPS) ermöglicht.
- Beispiele für die Berichterstattung – bietet und Berichte Beispiele zum Dienst Microsoft Antimalware besser optimieren den Dienst und Problembehandlung aktivieren.
- Ausschlüsse – ermöglicht die Anwendung und Dienstadministratoren bestimmte Dateien, Prozesse, konfigurieren und Laufwerke diese Schutz und anderen Gründen der Leistung und Scannen ausgeschlossen werden.
- Modul Ereignis Websitesammlungs - Einträge der Dienststatus Modul, verdächtige Aktivitäten und von Behebungsaktionen im Ereignisprotokoll Betriebssystem und sammelt diese in die Kundenkontos Azure-Speicher.

Weitere Informationen: Weitere Informationen zum Modul Software zum Schützen Ihrer virtuellen Computern finden Sie unter:

- [Microsoft-Modul für Azure-Cloud-Diensten und virtuellen Computern](../security/azure-security-antimalware.md)
- [Bereitstellen von Modul Lösungen auf Azure-virtuellen Computern](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [So installieren und Konfigurieren von Trend Micro Tiefe Security als auf einen virtuellen Windows-Dienst](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [So installieren und Konfigurieren von Symantec Endpunkt Schutz auf einen Windows-virtuellen](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Neue Modul Optionen zum Schutz von Azure-virtuellen Computern – McAfee Endpunkt Schutz](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)
- [Sicherheit Lösungen, in dem Azure Marketplace](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Hardware-Sicherheit Modul

Verschlüsselung und Authentifizierung können nicht Sicherheit verbessern, es sei denn, die Tasten selbst geschützt sind. Sie können die Verwaltung und Sicherheit Ihrer kritischen Kennwörter und Schlüssel in Azure-Taste Tresor gespeichert werden vereinfachen. Taste Tresor bietet die Option zum Speichern Ihrer Tasten in Hardware Security Module (HSMs) zertifiziert, FIPS 140-2 Ebene 2 Standards. Schlüssel Ihrer SQL Server für die Sicherung oder [transparente Verschlüsselung](https://msdn.microsoft.com/library/bb934049.aspx) können alle im Schlüssel Tresor mit allen Tasten oder geheimen Daten aus einer Anwendung gespeichert werden. Berechtigungen und Zugriff auf die geschützten Elemente werden durch [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)verwaltet.

Weitere Informationen:

- [Was ist die Taste Tresor Azure?](../key-vault/key-vault-whatis.md)
- [Erste Schritte mit Azure-Taste Tresor](../key-vault/key-vault-get-started.md)
- [Azure-Taste Tresor blog](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Virtuellen Computern Datenträger-Verschlüsselung

Azure Datenträger Verschlüsselung ist eine neue Funktion, die Sie Ihre Windows und Linux Azure-virtuellen Computern Datenträger verschlüsseln kann. Azure Datenträger Verschlüsselung verwendet die Branche [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Standardfunktion von Windows und die Funktion [dm-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux um Lautstärke Verschlüsselung für das Betriebssystem und Festplatten mit den Daten zu ermöglichen.

Die Lösung ist integriert mit Azure-Taste Tresor, mit deren Hilfe Sie steuern und Verwalten der Schlüssel zur Verschlüsselung und vertrauliche Informationen in Ihrem Abonnement Key Tresor und dabei sicherstellen, dass alle Daten im virtuellen Computern Laufwerke zum Rest in Azure-Speicher verschlüsselt werden.

Weitere Informationen:

- [Azure Datenträger Verschlüsselung für Windows und Linux IaaS virtuellen Computern](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
- [Azure Datenträger Verschlüsselung für Linux und Windows-virtuellen Computern](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
- [Verschlüsseln einer virtuellen Computern](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Sichern von virtuellen Computern

Azure Sicherung ist eine skalierbare Lösung, die die Anwendungsdaten mit 0 (null) Investitionen und minimale Geschäftsjahre Kosten zu schützen. Anwendungsfehler können Ihre Daten zu beschädigen, und personenbezogenen Fehler können Fehler in Ihre Apps vorstellen. Mit Azure Sicherung sind Ihre virtuellen Computern unter Windows und Linux geschützt.

Weitere Informationen:

- [Was ist eine Sicherung Azure?](../backup/backup-introduction-to-azure-backup.md)
- [Azure Sicherung Learning Path](https://azure.microsoft.com/documentation/learning-paths/backup/)
- [Azure Sicherung Service – häufig gestellte Fragen](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Wiederherstellung Azure-Website

Ein wichtiger Bestandteil Ihrer Organisation BCDR Strategie ist herauszufinden, wie corporate Auslastung und apps aufrechterhalten und ausgeführt, wenn Ausfall ausgeführt werden. Azure Website Wiederherstellung hilft Replikation, Failover und Wiederherstellung Auslastung und apps koordinieren, damit sie sind verfügbar von einem zweiten Standort Wenn gewohnten Standort befinden, nach unten geht.

Website-Wiederherstellung:

- **Vereinfacht strategische BCDR** – Website Wiederherstellung erleichtert die Replikation, Failover und Wiederherstellung mehrerer Business Auslastung und apps von einem einzigen Ort zu behandeln. Website Wiederherstellung koordiniert Replikation und Failover, aber nicht Ihre Anwendungsdaten abzufangen oder haben Sie alle Informationen zu erhalten.
- **Flexible Replikation stellt** – Website Wiederherstellung verwenden können Auslastung unter Hyper-V-virtuellen Computern, VMware virtuellen Computern und Windows/Linux physischen Servern repliziert.
- **Unterstützt Failover und Wiederherstellung** – Wiederherstellung Website bietet Testfailovers Disaster Wiederherstellung einen Drilldown Herstellung Umgebungen nicht unterstützt. Sie können auch mit einem Null-Datenverlust für erwarteten Ausfall geplanten Failovers oder nicht geplanten Failover mit minimalen Datenverlust (je nach Häufigkeit Replikation) für unerwartete Datenverluste ausführen. Nach einem Failover können Sie Failback primären Websites aus. Website Wiederherstellung bietet Wiederherstellung-Pläne, die Skripts und Azure Automatisierung Arbeitsmappen enthalten sein können, damit Sie Failover und Wiederherstellung von Applications mit mehreren Ebenen anpassen können.
- **Sekundäre Datacenter eliminiert** – können auf einer Website sekundäre lokalen oder Azure repliziert. Verwenden von Azure als Ziel für die Wiederherstellung entfällt, Kosten und Komplexität der Verwaltung einer Website sekundären. Replizierte Daten werden in Azure Storage gespeichert.
- **Ermöglichen die Integration mit vorhandenen BCDR Technologien** – Website Wiederherstellung Partner mit anderen Anwendung BCDR Features. Website Wiederherstellung können Sie die SQL Server-Back-End des Unternehmens Auslastung schützen. Dies umfasst systemeigene Unterstützung für SQL Server AlwaysOn das Failover der Verfügbarkeit von Gruppen verwalten.

Weitere Informationen:

- [Was ist die Website Wiederherstellung Azure?](../site-recovery/site-recovery-overview.md)
- [Wie funktioniert der Wiederherstellung Azure-Website?](../site-recovery/site-recovery-components.md)
- [Was Auslastung durch Azure Website Wiederherstellung geschützt werden?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtuelle Netzwerke

Virtuellen Computern benötigen Netzwerkkonnektivität. Um die Anforderung zu unterstützen, erfordert Azure virtuellen Computern mit einer Azure-virtuellen Netzwerk verbunden sein. Ein Azure-virtuellen Netzwerk ist eine logische erstellen, die auf der Struktur physischen Azure Netzwerk aufgebaut. Jedes logische virtuelle Azure-Netzwerk wird von allen anderen Azure virtuellen Netzwerken isoliert. Diese Isolation hilft sicherzustellen, dass in Ihrem Bereitstellungen Netzwerkverkehr nicht mit anderen Kunden Microsoft Azure zugegriffen werden kann.

Weitere Informationen:

- [Übersicht über die Sicherheit von Azure Netzwerk](security-network-overview.md)
- [Virtuelle Network (Übersicht)](../virtual-network/virtual-networks-overview.md)
- [Netzwerkfeatures und Partnerschaften für Enterprise-Szenarien](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Sicherheit-Management und reporting

Azure-Sicherheitscenter hilft Ihnen zu verhindern, erkennen und Beantworten von Risiken und stellt, dass Sie die Transparenz, und steuern, die Sicherheit Ihrer Azure Ressourcen erhöht. Es bietet integrierte Sicherheit Überwachung und Policy Management über Ihre Azure-Abonnements, hilft Angriffen, die andernfalls aufgefallen und funktioniert mit einem ausgedehnten System von Lösungen Sicherheit erkennen.

Azure-Sicherheitscenter unterstützt Sie beim Optimieren und Überwachen des virtuellen Computers Sicherheit durch:

- Bereitstellen von virtuellen Computern [Mittelpunkt](../security-center/security-center-recommendations.md) wie System-Updates anwenden, ACLs Endpunkte konfigurieren, Modul aktivieren, Sicherheitsgruppen Netzwerk aktivieren und Verschlüsselung Festplatten anwenden.
- Überwachen des Status Ihrer virtuellen Maschinen

Weitere Informationen:

- [Einführung in Azure-Sicherheitscenter](../security-center/security-center-intro.md)
- [Häufig gestellte Fragen zu Azure-Sicherheitscenter](../security-center/security-center-faq.md)
- [Planen von Azure-Sicherheitscenter und Vorgänge](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Compliance

Azure-virtuellen Computern ist zertifiziert für FISMA, FedRAMP, HIPAA, PCI DSS Ebene1 und anderen wichtigen Compliance-Programme. Diese Zertifizierung vereinfacht für eigene Azure Applications zu Vorschriften zu erfüllen und für Ihr Unternehmen eine Vielzahl von nationalen und internationalen gesetzlichen Vorschriften behoben.

Weitere Informationen:

- [Microsoft-Trust Center: Compliance](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
- [Vertrauenswürdigen Cloud: Microsoft Azure-Sicherheit, Datenschutz und Regelkonformität](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
