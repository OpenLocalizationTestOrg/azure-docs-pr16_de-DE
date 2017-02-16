<properties
   pageTitle="Beheben Endpunkt Schutz Gesundheit Benachrichtigungen in Azure-Sicherheitscenter | Microsoft Azure"
   description="Dieses Dokument wird gezeigt, wie Azure Sicherheitscenter empfohlen **beheben Endpunkt Schutz Gesundheit Benachrichtigungen**implementieren."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Beheben von Endpunkt Schutz Gesundheit Benachrichtigungen in Azure-Sicherheitscenter

Azure-Sicherheitscenter wird empfiehlt sich, dass Sie erkannt Endpunkt Schutz Gesundheit Benachrichtigungen beheben.  Sicherheitscenter können Sie sehen, welche virtuellen Computer (virtuellen Computern) Endpunkt Schutz Fehlern und wie viele Fehler haben.

> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt. Dies ist keine schrittweise Anleitung.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlungen

1. Wählen Sie in der **Empfehlungen Blade** **beheben Endpunkt Schutz Gesundheit Benachrichtigungen**aus.
![Beheben von Endpunkt Schutz Gesundheit Benachrichtigungen][1]

2. Daraufhin wird das Blade **Endpunkt Schutz Ausfall** der virtuellen Computern mit Fehlern und die Anzahl der Fehler für jeden virtuellen Computer aufgelistet sind. Wählen Sie einen virtuellen Computer aus der Liste aus.
![Fehler bei der Endpunkt Schutz][2]

3. Eine **Liste mit Fehlern** Blade wird für den ausgewählten virtuellen Computer, eine Liste der Fehler angezeigt. Wählen Sie aus der Liste, um weitere ein Fehler aus. Daraufhin wird eine Blade mit Informationen über den ausgewählten Fehler.
![Liste mit Fehlern][3]
  ![Fehler bei der Veranstaltung][4]

## <a name="see-also"></a>Siehe auch

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md)– Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwalten der Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md)– erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Sicherheit Dienststatus überwachen im Sicherheitscenter Azure](security-center-monitoring.md)– erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md)– Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Überwachung partnerlösungen mit Azure-Sicherheitscenter](security-center-partner-solutions.md) – erfahren, wie Sie den Status des Ihrer partnerlösungen zu überwachen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md)– suchen häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/)– Sicherheit von Azure Neuigkeiten und Informationen erhalten.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
