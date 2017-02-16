<properties
   pageTitle="Schützen von SQL Azure-Dienst in Azure-Sicherheitscenter | Microsoft Azure"
   description="Dieses Dokument Adressen Empfehlungen im Sicherheitscenter Azure, die Ihnen helfen schützen SQL Azure-Dienst und unter Einhaltung der Sicherheitsrichtlinien bleiben."
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

# <a name="protecting-azure-sql-service-in-azure-security-center"></a>Schützen von SQL Azure-Dienst in Azure-Sicherheitscenter

Azure-Sicherheitscenter analysiert den Sicherheitszustand Azure Ressourcen. Beim Sicherheitscenter potenzieller Sicherheitslücken bezeichnet, erstellt Empfehlungen, die Sie schrittweise durch die Verfahren zum Konfigurieren der erforderlichen Steuerelemente.  Anwenden von Empfehlungen auf Azure Ressourcentypen: virtuellen Computern (virtuelle Computer), Netzwerken, SQL und Applikationen.

Dieser Artikel befasst sich Empfehlungen, die auf SQL Azure-Dienst anwenden.  Azure SQL-Dienst Empfehlungen im Mittelpunkt Aktivieren der Überwachung für SQL Azure-Server und Datenbanken und für die Verschlüsselung für SQL-Datenbanken.  Verwenden Sie in der folgenden Tabelle als Referenz, mit deren Hilfe Sie die Grundlagen der verfügbaren SQL-Dienst Empfehlungen und was wird jeweils tun, wenn Sie es anwenden.

## <a name="available-sql-service-recommendations"></a>Verfügbare SQL-Dienst Empfehlungen

|Empfehlungen|Beschreibung|
|-----|-----|
|[Aktivieren Sie die Überwachung von SQL server](security-center-enable-auditing-on-sql-servers.md)|Empfiehlt, dass Sie für SQL Azure-Server (SQL Azure-Dienst nicht nur; SQL, die auf Ihre virtuellen Computern gehören) Überwachung aktivieren.|
|[Aktivieren Sie die Datenbank SQL-Überwachung](security-center-enable-auditing-on-sql-databases.md)|Empfiehlt, dass Sie für SQL Azure-Datenbanken (SQL Azure-Dienst nicht nur; SQL, die auf Ihre virtuellen Computern gehören) Überwachung aktivieren.|
|[Aktivieren des transparente Verschlüsselung von Daten auf SQL-Datenbanken](security-center-enable-transparent-data-encryption.md)|Empfiehlt, dass Sie Verschlüsselung für SQL-Datenbanken (nur für SQL Azure Service) zu aktivieren.|

## <a name="see-also"></a>Siehe auch

Weitere Informationen zum Empfehlungen, die für andere Typen von Azure gelten, probieren Sie Folgendes ein:

- [Schützen von Ihren virtuellen Computern in Azure-Sicherheitscenter](security-center-virtual-machine-recommendations.md)
- [Schützen von Ihrer Anwendung in Sicherheitscenter Azure](security-center-application-recommendations.md)
- [Schützen Sie Ihr Netzwerk in Azure-Sicherheitscenter](security-center-network-recommendations.md)

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes.
