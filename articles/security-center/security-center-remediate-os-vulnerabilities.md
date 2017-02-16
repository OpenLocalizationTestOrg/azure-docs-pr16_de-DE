<properties
   pageTitle="Behebung von OS Sicherheitsrisiko in Azure-Sicherheitscenter | Microsoft Azure"
   description="Dieses Dokument wird gezeigt, wie Azure-Sicherheitscenter empfohlen **OS beseitigen Schwachstellen**implementieren."
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

# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Behebung von OS Sicherheitsrisiko in Azure-Sicherheitscenter

Azure-Sicherheitscenter analysiert täglich virtuellen Computern (virtueller Computer) Betriebssystem (BS) für Konfigurationen, die konnte den virtuellen Computer stellen schneller zu Angriffen und Konfiguration Änderungen an diesen Angriffsmethoden empfiehlt. Weitere Informationen über die spezifischen Konfigurationen überwacht wird finden Sie unter der [Liste empfohlene Konfiguration Regeln](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Sicherheitscenter empfiehlt Schwachstellen beheben, wenn Ihre virtuellen Computers OS Konfiguration nicht die empfohlene Konfigurationsregeln übereinstimmt.

> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt.  Dies ist keine schrittweise Anleitung.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlungen

1. Wählen Sie das Blade **Empfehlungen** **Behebung von OS Schwachstellen**.
![OS Schwachstellen kurzfristig zu beheben][1]

    Das **OS beseitigen Schwachstellen** Blade wird geöffnet und Listen Ihrer virtueller Computer mit OS Konfigurationen, die nicht die empfohlene Konfigurationsregeln entsprechen.  Für jeden virtuellen Computer kennzeichnet das Blade:

   - **Fehler beim Regeln** – die Anzahl der Regeln, die Fehler bei der OS-Konfiguration des virtuellen Computers.
   - **Letzte SCAN-Zeit** – Datum und Uhrzeit, dass Sicherheitscenter zuletzt OS-Konfiguration des virtuellen Computers überprüft.
   - **BUNDESSTAAT** – im aktuellen Zustand das Sicherheitsrisiko:

        - Öffnen: Das Sicherheitsrisiko wurde nicht noch nicht behoben
        - Wird ausgeführt: Derzeit das Sicherheitsrisiko angewendet wird, die von Ihnen ist keine Aktion erforderlich
        - Gelöst: Wurde bereits das Sicherheitsrisiko berücksichtigt (wenn das Problem gelöst wurde, der Eintrag ist abgeblendet)
  - **Schwere** – alle festgelegt sind, dass eine schwere niedrig, d. h. ein Sicherheitsrisiko sollte berücksichtigt werden, jedoch sind keine sofortige Aufmerksamkeit erforderlich.

Wählen Sie einen virtuellen Computer aus. Eine Blade für diesen virtuellen Computer wird geöffnet und zeigt die Regeln, die fehlgeschlagen sind.
   ![Von Konfigurationsregeln, die fehlgeschlagen sind][2]

Wählen Sie eine Regel aus. In diesem Beispiel können wählen Sie **Kennwort muss Komplexität Anforderungen entsprechen**. Eine Blade wird geöffnet, die fehlerhafte Regel und den Einfluss beschrieben. Überprüfen Sie die Details, und beachten Sie, wie das Betriebssystem von Konfigurationen angewendet werden sollen.
  ![Eine Beschreibung für die fehlerhafte Regel][3]

  Sicherheitscenter verwendet allgemeine Konfiguration Enumeration (CCE), um die eindeutigen Bezeichner für Konfigurationsregeln zuweisen. Klicken Sie auf diese Blade wird die folgende Informationen bereitgestellt:

  - NAME – Name der Regel
  - SCHWERE – CCE schwere Wert kritisch, wichtig oder Warnung
  - CCIED – CCE Eindeutiger Bezeichner für die Regel
  - Beschreibung – Beschreibung der Regel
  - Sicherheitsrisiko – Erläuterung Sicherheitsrisiko oder Risiko Wenn Regel nicht übernommen wird
  - EINFLUSS – Geschäftliche Vorteile beim Regel angewendet wird
  - – Erwarteter Wert erwartet, wenn das Sicherheitscenter die Konfiguration virtueller Computer OS gegen die Regel analysiert
  - Regel Betrieb – Regel Vorgang untersuchten Sicherheitscenter während Analyse der Konfiguration virtueller Computer OS gegen die Regel
  - TATSÄCHLICHE Wert – Der Rückgabewert nach Analyse der Konfiguration virtueller Computer OS gegen die Regel
  - AUSWERTUNGSERGEBNIS –-Ergebnis Analyse: übergeben, Fail


## <a name="see-also"></a>Siehe auch

In diesem Artikel wird gezeigt, wie implementiert Sicherheitscenter empfohlen "Remediate OS Vulnerabilities". Sie können die Konfiguration Regelsatzes überprüfen [können](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Sicherheitscenter verwendet CCE (Allgemeine Konfiguration Enumeration), um die eindeutigen Bezeichner für Konfigurationsregeln zuweisen. Finden Sie weitere Informationen der Website [CCE](http://cce.mitre.org) aus.

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md) – erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Sicherheit Dienststatus in Azure Sicherheitscenter Überwachung](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Überwachung partnerlösungen mit Azure-Sicherheitscenter](security-center-partner-solutions.md) – erfahren, wie Sie den Status des Ihrer partnerlösungen zu überwachen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Blog suchen Beiträge zu Azure Sicherheit und Kompatibilität.

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
