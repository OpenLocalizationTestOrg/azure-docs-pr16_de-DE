<properties
   pageTitle="Schützen von Ihren virtuellen Computern im Sicherheitscenter Azure | Microsoft Azure"
   description="Dieses Dokument Adressen Empfehlungen im Azure-Sicherheitscenter, mit denen Sie Ihre virtuellen Computer schützen und Übereinstimmung mit Sicherheitsrichtlinien bleiben."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/25/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>Schützen von Ihren virtuellen Computern in Azure-Sicherheitscenter

Azure-Sicherheitscenter analysiert den Sicherheitszustand Azure Ressourcen. Beim Sicherheitscenter potenzieller Sicherheitslücken bezeichnet, erstellt Empfehlungen, die Sie schrittweise durch die Verfahren zum Konfigurieren der erforderlichen Steuerelemente.  Anwenden von Empfehlungen auf Azure Ressourcentypen: virtuellen Computern (virtuelle Computer), Netzwerken, SQL und Applikationen.

Dieser Artikel befasst sich Empfehlungen, die auf virtuellen Computern anwenden.  Virtueller Computer Empfehlungen im Mittelpunkt Datensammlung, Anwenden von System-Updates provisioning Modul, Verschlüsseln Ihrer virtuellen Computer Datenträger und vieles mehr.  Verwenden Sie in der folgenden Tabelle als Referenz, mit deren Hilfe Sie die Grundlagen der verfügbaren virtuellen Computer Empfehlungen und was wird jeweils tun, wenn Sie es anwenden.

## <a name="available-vm-recommendations"></a>Verfügbare virtueller Computer Empfehlungen

|Empfehlungen|Beschreibung|
|-----|-----|
|[Datensammlung für Abonnements aktivieren](security-center-enable-data-collection.md)|Empfiehlt, dass Sie die Datensammlung in der Sicherheitsrichtlinie für jede Ihrer Abonnements und alle virtuellen Computern (virtuelle Computer) in Ihrer Abonnements aktiviert.|
|[OS Schwachstellen kurzfristig zu beheben](security-center-remediate-os-vulnerabilities.md)|Empfiehlt, dass Sie Ihre Konfigurationen OS mit der Regeln für die empfohlene Konfiguration ausrichten z. B. zulassen nicht Kennwörter gespeichert werden.|
|[Anwenden von System-updates](security-center-apply-system-updates.md)|Empfiehlt, dass Sie fehlende System Sicherheitsupdates und wichtige Updates auf virtuellen Computern bereitstellen.|
|[Neu starten, wenn System-updates](security-center-apply-system-updates.md#reboot-after-system-updates)|Empfiehlt, dass Sie einen virtuellen zum Abschließen des Vorgangs der Anwendung System-Updates neu starten.|
|[Installieren der Endpunkt Schutz](security-center-install-endpoint-protection.md)|Empfiehlt die Bereitstellung von Modul-Programmen für virtuellen Computern (virtuelle nur für Windows-Computer).|
|[Beheben von Endpunkt Schutz Gesundheit Benachrichtigungen](security-center-resolve-endpoint-protection-health-alerts.md)|Empfiehlt, dass Sie Endpunkt Schutz Fehler beheben.|
|[Aktivieren der virtuellen Computer-Agents](security-center-enable-vm-agent.md)|Ermöglicht es Ihnen, die virtuellen Computern erfordern finden Sie unter der Agent virtueller Computer. Des virtuellen Computer-Agents muss auf virtuellen Computern installiert sein, um die Bereitstellung von Patch scannen, geplante Scannen und Modul-Programmen. Der virtuellen Computer Agent ist standardmäßig für virtuelle Computer installiert, die aus dem Azure Marketplace bereitgestellt werden. [Virtueller Computer-Agents und Erweiterungen – Teil 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) Artikel enthält Informationen zum Installieren des virtuellen Computer-Agents an.|
| [Anwenden von Datenträger-Verschlüsselung](security-center-apply-disk-encryption.md) |Empfiehlt, dass Sie Ihre virtuellen Computer Datenträger mit Azure Datenträger Verschlüsselung (Windows und Linux virtuellen Computern) verschlüsseln. Verschlüsselung wird für das Betriebssystem und die Daten Datenmengen Ihrer virtuellen Computers empfohlen.|
| [Aktualisieren Sie die Version des Betriebssystems](security-center-update-os-version.md) | Empfiehlt, dass Sie für Ihre Cloud-Dienst, um die neueste Version zur Verfügung für Ihre Familie OS Version des Betriebssystems (BS) aktualisieren.  Weitere Informationen zum Cloud Services finden Sie unter der [Cloud-Dienste (Übersicht)](../cloud-services/cloud-services-choose-me.md). |
| [Sicherheitsrisiko Bewertung nicht installiert](security-center-vulnerability-assessment-recommendations.md) | Empfiehlt, eine Sicherheitsrisiko Bewertung Lösung Ihrer virtuellen Computers zu installieren. |
| [Behebung von Schwachstellen](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Ermöglicht es Ihnen System und Anwendung Sicherheitslücken erkannt durch die Sicherheitsrisiko Bewertung Lösung Ihrer virtuellen Computers installiert angezeigt. |

## <a name="see-also"></a>Siehe auch

Weitere Informationen zum Empfehlungen, die für andere Typen von Azure gelten, probieren Sie Folgendes ein:

- [Schützen von Ihrer Anwendung in Sicherheitscenter Azure](security-center-application-recommendations.md)
- [Schützen Sie Ihr Netzwerk in Azure-Sicherheitscenter](security-center-network-recommendations.md)
- [Schützen von den SQL Azure-Dienst in Azure-Sicherheitscenter](security-center-sql-service-recommendations.md)

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes.
