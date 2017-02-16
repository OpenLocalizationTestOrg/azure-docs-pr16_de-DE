<properties
   pageTitle="Schützen Ihres Netzwerks im Sicherheitscenter Azure | Microsoft Azure"
   description="Dieses Dokument Adressen Empfehlungen im Sicherheitscenter Azure, mit denen Sie schützen Sie Ihr Netzwerk Azure und unter Einhaltung der Sicherheitsrichtlinien bleiben."
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

# <a name="protecting-your-network-in-azure-security-center"></a>Schützen Sie Ihr Netzwerk in Azure-Sicherheitscenter

Azure-Sicherheitscenter analysiert den Sicherheitszustand Azure Ressourcen. Beim Sicherheitscenter potenzieller Sicherheitslücken bezeichnet, erstellt Empfehlungen, die Sie schrittweise durch die Verfahren zum Konfigurieren der erforderlichen Steuerelemente.  Anwenden von Empfehlungen auf Azure Ressourcentypen: virtuellen Computern (virtuelle Computer), Netzwerken, SQL und Applikationen.

Dieser Artikel befasst sich Empfehlungen, die mit Ihrem Netzwerk anwenden.  Netzwerk Empfehlungen im Mittelpunkt nächsten Generation Firewalls, Netzwerk-Sicherheitsgruppen, Konfigurieren von Regeln für eingehenden Datenverkehr und mehr.  Verwenden Sie in der folgenden Tabelle als Referenz, mit deren Hilfe Sie die Grundlagen der verfügbaren Netzwerk Empfehlungen und was wird jeweils tun, wenn Sie es anwenden.

## <a name="available-network-recommendations"></a>Verfügbare Netzwerk Empfehlungen

|Empfehlungen|Beschreibung|
|-----|-----|
|[Fügen Sie eine Firewall der nächste Generation](security-center-add-next-generation-firewall.md)|Empfiehlt, dass Sie von einem Microsoft-Partner eine nächsten Generation Firewall (NGFW) eins hinzufügen, um Ihre Sicherheitsmaßnahmen zu vergrößern.|
|[Routing-Verkehr über NGFW nur](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only)|Sie sollten Netzwerk Sicherheit Gruppe (NSG) Regeln konfigurieren, die eingehenden Datenverkehr an Ihre virtuellen Computer durch Ihre NGFW erzwingen.|
|[Aktivieren Sie Netzwerk Sicherheitsgruppen auf Subnetze oder virtuellen Computern](security-center-enable-network-security-groups.md)|Empfehlen Ihnen NSGs auf Subnets oder virtuellen Computern aktivieren.|
|[Schränken Sie Zugriff über das Internet gegenüberliegende Endpunkt ein](security-center-restrict-access-through-internet-facing-endpoints.md)|Sollten Sie eingehenden Datenverkehr Regeln für NSGs konfigurieren.|

## <a name="see-also"></a>Siehe auch

Weitere Informationen zum Empfehlungen, die für andere Typen von Azure gelten, probieren Sie Folgendes ein:

- [Schützen von Ihren virtuellen Computern in Azure-Sicherheitscenter](security-center-virtual-machine-recommendations.md)
- [Schützen von Ihrer Anwendung in Sicherheitscenter Azure](security-center-application-recommendations.md)
- [Schützen von den SQL Azure-Dienst in Azure-Sicherheitscenter](security-center-sql-service-recommendations.md)

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes.
