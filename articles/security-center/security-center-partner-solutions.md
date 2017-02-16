<properties
   pageTitle="Verwalten von partnerlösungen im Sicherheitscenter Azure | Microsoft Azure"
   description="Dieses Dokument führt Sie durch so Azure-Sicherheitscenter Monitor auf einen Blick können Sie der Status des Ihrer partnerlösungen mit Ihrem Abonnement Azure integriert."
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
   ms.date="10/26/2016"
   ms.author="terrylan"/>

# <a name="monitoring-partner-solutions-with-azure-security-center"></a>Überwachen von partnerlösungen mit Azure-Sicherheitscenter

Dieses Dokument führt Sie durch so den Status des Ihrer partnerlösungen in Azure-Sicherheitscenter zu überwachen.

> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt. Dies ist keine schrittweise Anleitung.

## <a name="monitoring-partner-solutions"></a>Überwachen von partnerlösungen

Die Kachel **partnerlösungen** auf das **Sicherheitscenter** Blade kann Monitor auf einen Blick der Status des Ihrer partnerlösungen mit Ihrem Abonnement Azure integriert.
![Kachel partnerlösungen][1]

Die Kachel " **partnerlösungen** " zeigt die Anzahl von partnerlösungen und eine Zusammenfassung für diese Lösungen Status.

Der **STATUS** der Lösung eines Partners kann sein:

- Geschützte (Grün) – kein Problem Gesundheit vorhanden ist.
- Fehlerhaft (Rot) - Dienststatus Problem, die sofortige Aufmerksamkeit erforderlich sind.
- Beendet (Orange) reporting – die Lösung verfügt über deren Status reporting beendet.
- Unbekannten Schutzstatus (Orange) – ist die Integrität des die Lösung, die zu diesem Zeitpunkt aufgrund einer ausgefallenen Verfahren zum Hinzufügen einer neuen Ressource zur vorhandenen Lösung unbekannt.
- Nicht gemeldet (grau) – die Lösung noch nicht alles gemeldet noch eine Lösung Status möglicherweise, wenn sie nur verbunden wurde und weiterhin Bereitstellen von.

Es gibt keine Lösungen mit Ihrem Abonnement integriert wird die Kachel angegeben werden, dass es keine Lösungen gibt. Markieren die Kachel **partnerlösungen** ermöglicht Ihnen das Blade **Empfehlungen** zum Bereitstellen von partnerlösungen-Sicherheit zu öffnen.
![Keine partnerlösungen][2]

So zeigen Sie den Integritätsstatus Ihrer partnerlösungen an:

1. Wählen Sie die Kachel **partnerlösungen** aus. Eine Blade wird geöffnet, und eine Liste der partnerlösungen mit Sicherheitscenter verbunden.
![Partnerlösungen][3]

2. Wählen Sie eine Lösung Partner aus. In diesem Beispiel können die Lösung **F5-WAF2** wählen.  Eine Blade wird mit, dass der Status des Partner-Lösung und der Lösung des Ressourcen zugeordnet ist. Wählen Sie **Lösung Console** um den Partner Verwaltungsoption für diese Lösung zu öffnen.
![Details zum Partner-Lösung][4]

3. Kehren Sie zu der **F5-WAF2** Blade, und wählen Sie **Link-app**. Das **Verknüpfen von Applications** Blade wird geöffnet. Hier können Sie Ressourcen zur Lösung Partner verbinden.
![Verknüpfen von Ressourcen in Partner-Lösung][5]

## <a name="see-also"></a>Siehe auch
In diesem Dokument wurden Sie eine Einführung in die Kachel **Partnerlösungen** im Sicherheitscenter. Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwalten der Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md) – erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Sicherheit Dienststatus überwachen im Sicherheitscenter Azure](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – häufig gestellte Fragen zur Verwendung des Dienstes suchen.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Sicherheit von Azure Neuigkeiten und Informationen erhalten.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/no-partner-solutions-to-display.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
