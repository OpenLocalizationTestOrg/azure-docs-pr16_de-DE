<properties
   pageTitle="Sicherheit Kontaktdetails im Sicherheitscenter Azure bereitstellen | Microsoft Azure"
   description="Dieses Dokument wird gezeigt, wie Sicherheit Kontaktdetails im Sicherheitscenter Azure bereitstellen."
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
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="provide-security-contact-details-in-azure-security-center"></a>Bereitstellen von Sicherheit Kontaktdetails in Azure-Sicherheitscenter

Azure-Sicherheitscenter wird empfiehlt sich, dass Sie die Sicherheit Kontaktdetails für Ihr Abonnement Azure bereitstellen, wenn Sie noch nicht geschehen ist. Diese Informationen werden von Microsoft verwendet werden, mit Ihnen Kontakt aufnehmen, wenn das Microsoft Security Antwort Center (MSRC) erkennt, dass Ihre Kundendaten durch einen dritten ungesetzlichen oder nicht autorisierte zugegriffen wurde. MSRC führt select Sicherheit Überwachung der Azure Netzwerk- und Infrastruktur und empfängt Threat Intelligence und Missbrauch Beschwerden von Drittanbietern.

Klicken Sie auf das erste tägliche vorkommen, der eine Benachrichtigung und nur für Benachrichtigungen hoher Priorität wird eine e-Mail-Benachrichtigung gesendet. E-Mail-Einstellungen können nur für Abonnements Richtlinien konfiguriert werden. Ressourcengruppen innerhalb eines Abonnements werden diese Einstellungen übernehmen.

> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt.  Dies ist keine schrittweise Anleitung.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlungen

1. Wählen Sie das Blade **Empfehlungen** **Bereitstellen Sicherheit Details wenden Sie sich an**.
![Bereitstellen von Sicherheit Kontaktperson][1]

2. Das Blade **Bereitstellen Sicherheit, wenden Sie sich an Details**wird geöffnet. Wählen Sie aus der Azure-Abonnement auf Kontaktinformationen.
![Bereitstellen von Sicherheit Kontaktdetails][2]

3. Eine zweite **bieten Sicherheit Kontaktdetails** Blade wird geöffnet.

  - Geben Sie die Sicherheit Kontakt-e-Mail-Adressen durch Kommas getrennt ein. Es ist keiner hinsichtlich der Anzahl von e-Mail-Adressen, die Sie eingeben können.
  - Geben Sie eine internationale Telefonnummer oder Sozialversicherungsnummer an.
  - Aktivieren Sie die Option **e-Mails an mich senden zu Benachrichtigungen**zum Empfangen von e-Mails zu hoher Priorität Benachrichtigungen aus.
  - In der Zukunft haben Sie die Option zum Senden von e-Mail-Benachrichtigungen zu Abonnement Besitzer. Diese Option ist derzeit abgeblendet.
  - Wählen Sie **OK** zum Anwenden der Sicherheit Kontaktinformationen für Ihr Abonnement.

## <a name="see-also"></a>Siehe auch

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md) – erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Sicherheit Dienststatus in Azure Sicherheitscenter Überwachung](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Überwachung partnerlösungen mit Azure-Sicherheitscenter](security-center-partner-solutions.md) – erfahren, wie Sie den Status des Ihrer partnerlösungen zu überwachen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Sicherheit von Azure Neuigkeiten und Informationen erhalten.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
