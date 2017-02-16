<properties
   pageTitle="Schützen von Ihrer Anwendung in Sicherheitscenter Azure | Microsoft Azure"
   description="Dieses Dokument Adressen Empfehlungen im Sicherheitscenter Azure, mit denen Sie schützen Ihrer Azure Applications und unter Einhaltung der Sicherheitsrichtlinien bleiben."
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
   ms.date="08/04/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-applications-in-azure-security-center"></a>Schützen von Ihrer Anwendung in Sicherheitscenter Azure

Azure-Sicherheitscenter analysiert den Sicherheitszustand Azure Ressourcen. Beim Sicherheitscenter potenzieller Sicherheitslücken bezeichnet, erstellt Empfehlungen, die Sie schrittweise durch die Verfahren zum Konfigurieren der erforderlichen Steuerelemente.  Anwenden von Empfehlungen auf Azure Ressourcentypen: virtuellen Computern (virtuelle Computer), Netzwerken, SQL und Applikationen.

Dieser Artikel befasst sich Empfehlungen, die für Applikationen gelten.  Anwendung Empfehlungen im Mittelpunkt Bereitstellung von einer Web-Anwendung-Firewall.  Verwenden Sie in der folgenden Tabelle als Referenz, mit deren Hilfe Sie die Grundlagen der Anwendung verfügbaren Empfehlungen und was wird jeweils tun, wenn Sie es anwenden.

## <a name="available-application-recommendations"></a>Empfehlungen im Zusammenhang mit der Anwendung

|Empfehlungen|Beschreibung|
|-----|-----|
|[Hinzufügen einer Web-Anwendung Firewalls](security-center-add-web-application-firewall.md)|Empfiehlt, dass Sie eine Web-Anwendung Firewall (WAF) für Webendpunkte bereitstellen. Sie können mehrere Webanwendungen in Sicherheitscenter schützen, indem Sie diese Programme Bereitstellung Ihrer vorhandenen WAF hinzufügen. WAF Einheiten (erstellt mit dem Modell zur Bereitstellung von Ressourcenmanager) in einem separaten virtuellen Netzwerk bereitgestellt werden müssen. WAF Einheiten (erstellt mit dem Bereitstellungsmodell klassischen) sind bei der Verwendung einer Netzwerksicherheitsgruppe beschränkt. Diese Unterstützung wird in eine vollständig angepasste Bereitstellung von einer WAF Einheit (klassische) in der Zukunft erweitert werden.|
|[Fertigstellen Schutz der Anwendung](security-center-add-web-application-firewall.md#finalize-application-protection)|Zum Abschließen der Konfigurations von einer WAF muss den Datenverkehr an die Einheit WAF umgeleitet werden. Folgen diesem Empfehlungen, führen Sie die notwendigen Setup Änderungen.|

## <a name="see-also"></a>Siehe auch

Weitere Informationen zum Empfehlungen, die für andere Typen von Azure gelten, probieren Sie Folgendes ein:

- [Schützen von Ihren virtuellen Computern in Azure-Sicherheitscenter](security-center-virtual-machine-recommendations.md)
- [Schützen Sie Ihr Netzwerk in Azure-Sicherheitscenter](security-center-network-recommendations.md)
- [Schützen von den SQL Azure-Dienst in Azure-Sicherheitscenter](security-center-sql-service-recommendations.md)

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes.
