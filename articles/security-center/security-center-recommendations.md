<properties
   pageTitle="Verwalten der Sicherheit Empfehlungen im Sicherheitscenter Azure | Microsoft Azure"
   description="Dieses Dokument führt Sie durch, wie Empfehlungen im Sicherheitscenter Azure Organisieren Ihrer Azure Ressourcen zu schützen und unter Einhaltung des Sicherheitsrichtlinien bleiben."
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

# <a name="managing-security-recommendations-in-azure-security-center"></a>Verwalten der Sicherheit Empfehlungen im Sicherheitscenter Azure

Dieses Dokument führt Sie durch Verwendung von Empfehlungen Azure-Sicherheitscenter um zu Azure Ressourcen zu schützen.

> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt.  Dies ist keine schrittweise Anleitung.

## <a name="what-are-security-recommendations"></a>Was sind die Sicherheit Empfehlungen?
Sicherheitscenter analysiert regelmäßig den Sicherheitsstatus Ihrer Azure Ressourcen. Beim Sicherheitscenter potenzieller Sicherheitslücken bezeichnet, erstellt Empfehlungen. Die Empfehlungen führen Sie durch die Verfahren zum Konfigurieren der erforderlichen Steuerelemente.

## <a name="implementing-security-recommendations"></a>Implementieren der Sicherheit Empfehlungen

### <a name="set-recommendations"></a>Festlegen von Empfehlungen

In [Festlegen von Sicherheitsrichtlinien in Azure Sicherheitscenter](security-center-policies.md)lernen, wie Sie:

- Konfigurieren von Sicherheitsrichtlinien.
- Datensammlung aktivieren.
- Wählen Sie aus der Empfehlungen als Teil der Sicherheitsrichtlinie angezeigt.

Aktuelle Richtlinie Empfehlungen im Mittelpunkt System-Updates, geplante Regeln, Modul Programme [Netzwerk Sicherheitsgruppen](../virtual-network/virtual-networks-nsg.md) Subnetze und Netzwerk-Schnittstellen, SQL-Datenbank Überwachung, SQL-Datenbank als transparent Daten Verschlüsselung und Web-Anwendung Firewalls.  [Einrichten von Sicherheitsrichtlinien für die](security-center-policies.md) enthält eine Beschreibung der einzelnen Optionen empfohlen.

### <a name="monitor-recommendations"></a>Monitor Empfehlungen
Nach dem Einrichten einer Sicherheitsrichtlinie, analysiert Sicherheitscenter den Sicherheitsstatus Ihrer Ressourcen zur Identifizierung möglicher Sicherheitslücken. Die **Empfehlungen** -Kachel auf das **Sicherheitscenter** Blade können Sie die Gesamtzahl der Empfehlungen vom Sicherheitscenter identifiziert.

![Empfehlungen-Kachel][1]

Um die Details der einzelnen Empfehlungen anzuzeigen:

1. Klicken Sie auf die **Kachel Empfehlungen** auf das **Sicherheitscenter** Blade. Das Blade **Empfehlungen** wird geöffnet.

Die Empfehlungen werden in einem Tabellenformat angezeigt, wobei jede Zeile eine bestimmte Empfehlungen darstellt. Die Spalten in dieser Tabelle sind:

- **Beschreibung**: erläutert empfohlen und was muss ausgeführt werden, um ihn zu beheben.
- **Ressource**: Listet die Ressourcen dieser empfohlen gilt.
- **BUNDESSTAAT**: Beschreibt den aktuellen Status des empfohlen:
    - **Öffnen**: empfohlen noch noch nicht berücksichtigt wurde.
    - **In Bearbeitung**: empfohlen aktuell auf die Ressourcen angewendet wird, und von Ihnen ist keine Aktion erforderlich.
    - **Gelöst**: empfohlen bereits abgeschlossen wurde (in diesem Fall die Linie wird abgeblendet).
- **Schwere**: Beschreibt die Schwere der dieser bestimmten empfohlen:
    - **Hohe**: ein Sicherheitsrisiko vorhanden ist, mit einer aussagekräftigen Ressource (beispielsweise eine Anwendung, eines virtuellen Computers oder eine Netzwerksicherheitsgruppe) und Aufmerksamkeit erfordert.
    - **Mittel**: ein Sicherheitsrisiko vorhanden ist, und sind kritisch oder zusätzliche Schritte erforderlich, um sie zu entfernen oder zur Durchführung eines Prozesses.
    - **Niedrig**: ein Sicherheitsrisiko vorhanden ist, sollte berücksichtigt werden, aber nicht sofortige Aufmerksamkeit erfordert. (Standardmäßig niedrige Empfehlungen werden nicht angezeigt, aber Sie können über niedrig empfohlene filtern, wenn sie angezeigt werden sollen.)

Verwenden Sie in der folgenden Tabelle als Referenz, mit deren Hilfe Sie die Grundlagen der verfügbaren Empfehlungen und was wird jeweils tun, wenn Sie es anwenden.

> [AZURE.NOTE] Sie möchten die [klassischen und Ressourcenmanager Bereitstellungsmodelle](../azure-classic-rm.md) für Azure Ressourcen zu verstehen.

|Empfehlungen|Beschreibung|
|-----|-----|
|[Datensammlung für Abonnements aktivieren](security-center-enable-data-collection.md)|Empfiehlt, dass Sie in Ihrer Abonnements Datensammlung in der Sicherheitsrichtlinie für jede Ihrer Abonnements und alle virtuellen Computern (virtuellen Computern) aktivieren.|
|[OS Schwachstellen kurzfristig zu beheben](security-center-remediate-os-vulnerabilities.md)|Empfiehlt, dass Sie Ihre Konfigurationen OS mit der Regeln für die empfohlene Konfiguration ausrichten z. B. zulassen nicht Kennwörter gespeichert werden.|
|[Anwenden von System-updates](security-center-apply-system-updates.md)|Empfiehlt, dass Sie fehlende System Sicherheitsupdates und wichtige Updates auf virtuellen Computern bereitstellen.|
|[Neu starten, wenn System-updates](security-center-apply-system-updates.md#reboot-after-system-updates)|Empfiehlt, dass Sie einen virtuellen zum Abschließen des Vorgangs der Anwendung System-Updates neu starten.|
|[Hinzufügen einer Web-Anwendung Firewalls](security-center-add-web-application-firewall.md)|Empfiehlt, dass Sie eine Web-Anwendung Firewall (WAF) für Webendpunkte bereitstellen. Sie können mehrere Webanwendungen in Sicherheitscenter schützen, indem Sie diese Programme Bereitstellung Ihrer vorhandenen WAF hinzufügen. WAF Einheiten (erstellt mit dem Modell zur Bereitstellung von Ressourcenmanager) in einem separaten virtuellen Netzwerk bereitgestellt werden müssen. WAF Einheiten (erstellt mit dem Bereitstellungsmodell klassischen) sind bei der Verwendung einer Netzwerksicherheitsgruppe beschränkt. Diese Unterstützung wird in eine vollständig angepasste Bereitstellung von einer WAF Einheit (klassische) in der Zukunft erweitert werden. Sicherheitscenter wird empfiehlt sich, dass Sie eine WAF zum Schutz vor Angriffen, die auf Ihre Webanwendungen auf virtuellen Computern und auf App-Service-Umgebung (ASE) Hilfe bereitstellen. Weitere Informationen zum ASE finden Sie unter der [App-Umgebung Servicedokumentation](../app-service/app-service-app-service-environments-readme.md). |
|[Fertigstellen Schutz der Anwendung](security-center-add-web-application-firewall.md#finalize-application-protection)|Zum Abschließen der Konfigurations von einem WAF muss den Datenverkehr an die Einheit WAF umgeleitet werden. Folgen diesem Empfehlungen, führen Sie die notwendigen Setup Änderungen.|
|[Fügen Sie eine Firewall der nächste Generation](security-center-add-next-generation-firewall.md)|Empfiehlt, dass Sie von einem Microsoft-Partner eine nächsten Generation Firewall (NGFW) eins hinzufügen, um Ihre Sicherheitsmaßnahmen zu vergrößern.|
|[Routing-Verkehr über NGFW nur](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Sie sollten Netzwerk Sicherheit Gruppe (NSG) Regeln konfigurieren, die eingehenden Datenverkehr an Ihre virtuellen Computer durch Ihre NGFW erzwingen.|
|[Installieren der Endpunkt Schutz](security-center-install-endpoint-protection.md)|Empfiehlt die Bereitstellung von Modul-Programmen für virtuellen Computern (virtuelle nur für Windows-Computer).|
|[Beheben von Endpunkt Schutz Gesundheit Benachrichtigungen](security-center-resolve-endpoint-protection-health-alerts.md)|Empfiehlt, dass Sie Endpunkt Schutz Fehler beheben.|
|[Aktivieren Sie Netzwerk Sicherheitsgruppen auf Subnetze oder virtuellen Computern](security-center-enable-network-security-groups.md)|Empfehlen Ihnen NSGs auf Subnets oder virtuellen Computern aktivieren.|
|[Schränken Sie Zugriff über das Internet gegenüberliegende Endpunkt ein](security-center-restrict-access-through-internet-facing-endpoints.md)|Sollten Sie eingehenden Datenverkehr Regeln für NSGs konfigurieren.|
|[Aktivieren Sie die Überwachung von SQL server](security-center-enable-auditing-on-sql-servers.md)|Empfiehlt, dass Sie für SQL Azure-Server (SQL Azure-Dienst nicht nur; SQL, die auf Ihre virtuellen Computern gehören) Überwachung aktivieren.|
|[Aktivieren Sie die Datenbank SQL-Überwachung](security-center-enable-auditing-on-sql-databases.md)|Empfiehlt, dass Sie für SQL Azure-Datenbanken (SQL Azure-Dienst nicht nur; SQL, die auf Ihre virtuellen Computern gehören) Überwachung aktivieren.|
|[Aktivieren des transparente Verschlüsselung von Daten auf SQL-Datenbanken](security-center-enable-transparent-data-encryption.md)|Empfiehlt, dass Sie Verschlüsselung für SQL-Datenbanken (nur für SQL Azure Service) zu aktivieren.|
|[Aktivieren der virtuellen Computer-Agents](security-center-enable-vm-agent.md)|Ermöglicht es Ihnen, die virtuellen Computern erfordern finden Sie unter der Agent virtueller Computer. Des virtuellen Computer-Agents muss auf virtuellen Computern installiert sein, um die Bereitstellung von Patch scannen, geplante Scannen und Modul-Programmen. Der virtuellen Computer Agent ist standardmäßig für virtuelle Computer installiert, die aus dem Azure Marketplace bereitgestellt werden. [Virtueller Computer-Agents und Erweiterungen – Teil 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) Artikel enthält Informationen zum Installieren des virtuellen Computer-Agents an.|
| [Anwenden von Datenträger-Verschlüsselung](security-center-apply-disk-encryption.md) |Empfiehlt, dass Sie Ihre virtuellen Computer Datenträger mit Azure Datenträger Verschlüsselung (Windows und Linux virtuellen Computern) verschlüsseln. Verschlüsselung wird für das Betriebssystem und die Daten Datenmengen Ihrer virtuellen Computers empfohlen.|
|[Bereitstellen von Sicherheit Kontaktdetails](security-center-provide-security-contact-details.md) | Sicherheit zu gewährleisten empfiehlt Kontaktinformationen für jede Ihrer Abonnements an. Kontaktinformationen besteht eine e-Mail-Adresse und Telefonnummer. Die Informationen werden verwendet werden, mit Ihnen Kontakt aufnehmen, findet unser Sicherheitsteam, dass Ihre Ressourcen beeinträchtigt werden. |
| [Aktualisieren Sie die Version des Betriebssystems](security-center-update-os-version.md) | Empfiehlt, dass Sie für Ihre Cloud-Dienst, um die neueste Version zur Verfügung für Ihre Familie OS Version des Betriebssystems (BS) aktualisieren.  Um weitere Informationen zur Cloud Services finden Sie in der [Cloud-Dienste (Übersicht)](../cloud-services/cloud-services-choose-me.md). |
| [Sicherheitsrisiko Bewertung nicht installiert](security-center-vulnerability-assessment-recommendations.md) | Empfiehlt, eine Sicherheitsrisiko Bewertung Lösung Ihrer virtuellen Computers zu installieren. |
| [Behebung von Schwachstellen](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Ermöglicht es Ihnen System und Anwendung Sicherheitslücken erkannt durch die Sicherheitsrisiko Bewertung Lösung Ihrer virtuellen Computers installiert angezeigt. |

Sie können filtern und Empfehlungen beenden.

1. Klicken Sie auf das **Empfehlungen** Blade auf **Filter** . Das **Filters** Blade wird geöffnet, und wählen Sie die schwere und Status-Werte, die Sie sehen möchten.

    ![Filtern von Empfehlungen][2]

2. Wenn Sie feststellen, dass ein Empfehlungen nicht verfügbar ist, können Sie die Empfehlungen beenden und Filtern sie dann aus der Ansicht. Es gibt zwei Möglichkeiten, um eine Empfehlungen zu beenden. Eine Möglichkeit besteht darin, klicken Sie mit der rechten Maustaste auf ein Element, und wählen Sie dann auf **Schließen**. Die andere besteht darin, zeigen Sie auf ein Element auf die drei Punkte, die auf der rechten Seite angezeigt werden, und wählen Sie dann auf **Schließen**. Sie können ausgeblendete Empfehlungen anzeigen, indem Sie auf **Filtern**und dann **Dismissed**auswählen.

    ![Beenden der Empfehlungen][3]

### <a name="apply-recommendations"></a>Anwenden von Empfehlungen
Überprüfen Sie alle empfohlenen, entscheiden Sie, welche die Sie zuerst angewendet werden soll. Es empfiehlt sich, dass Sie die Bewertung schwere verwenden, wie der Hauptfenster Parameter für welche Empfehlungen ausgewertet werden soll, zuerst angewendet werden soll.

Markieren Sie in der Tabelle oben empfohlenen empfohlen und durchgehen Sie, es in ein Beispiel für ein Empfehlungen anwenden.

## <a name="see-also"></a>Siehe auch
In diesem Dokument wurden Sicherheit Empfehlungen im Sicherheitscenter vorgestellt. Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Sicherheit Dienststatus überwachen im Sicherheitscenter Azure](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Überwachen von partnerlösungen mit Azure-Sicherheitscenter](security-center-partner-solutions.md) – erfahren, wie Sie den Status des Ihrer partnerlösungen zu überwachen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – häufig gestellte Fragen zur Verwendung des Dienstes suchen.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Blogbeiträge zur Azure Sicherheit und Einhaltung von Vorschriften zu finden.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png
