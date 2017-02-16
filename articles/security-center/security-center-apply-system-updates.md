<properties
   pageTitle="Anwenden von System-Updates in Azure-Sicherheitscenter | Microsoft Azure"
   description="Dieses Dokument wird gezeigt, wie die Sicherheitscenter Azure Empfehlungen **System-Updates anwenden** und **neu starten, wenn System-Updates**implementieren."
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

# <a name="apply-system-updates-in-azure-security-center"></a>Anwenden von System-Updates in Azure-Sicherheitscenter

Azure-Sicherheitscenter überwacht täglich Windows und Linux-virtuellen Computern (virtuellen Computern) auf fehlende Betriebssystemupdates. Sicherheitscenter Ruft eine Liste der verfügbaren Sicherheitsupdates und wichtige Updates von Windows Update oder Windows Server Update Services (WSUS), je nachdem, welcher Dienst auf einen Windows-virtuellen konfiguriert.  Sicherheitscenter prüft in Linux Systeme auch für die neuesten Updates. Wenn Ihre virtuellen Computer eine Aktualisierung von System nicht angezeigt wird, wird das Sicherheitscenter empfiehlt System-Updates anzuwenden

> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt.  Dies ist keine schrittweise Anleitung.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlungen

1. Wählen Sie das Blade **Empfehlungen** **System-Updates anwenden**.
![Anwenden von System-updates][1]

2. Das **System-Updates anwenden** Blade wird geöffnet, und eine Liste der virtuellen Computern fehlen System-Updates. Wählen Sie einen virtuellen Computer aus.
![Auswählen eines virtuellen Computers][2]

3. Eine Blade wird eine Liste der fehlenden Updates für diesen virtuellen Computer anzeigt. Wählen Sie ein System aktualisieren. Wählen Sie in diesem Beispiel KB3156016 uns aus.
![Fehlende Sicherheitsupdates][3]

4. Führen Sie die Schritte in der **Sicherheitsupdate** Blade zum Anwenden der fehlende Updates aus.
![Sicherheitsupdate][4]

## <a name="reboot-after-system-updates"></a>Neu starten, wenn System-updates

5. Kehren Sie zu der Blade **Empfehlungen** zurück. Ein neuer Eintrag generiert wurde, nach der Anwendung von System-Updates aufgerufen **nach System-Updates neu starten**. Dieser Eintrag können Sie wissen, dass Sie den virtuellen Computer zum Abschließen des Vorgangs der Anwendung System-Updates neu starten müssen.
![Neu starten, wenn System-updates][5]

6. Wählen Sie aus, **nach der System-Updates neu starten**. Daraufhin wird **ein Neustart ausstehend System-Updates abgeschlossen ist** Blade mit einer Liste der virtuellen Computern, die Sie benötigen, um die übernehmen System Updates abzuschließen neu zu starten.
![Neustart ausstehend][6]

Starten Sie den virtuellen Computer aus Azure bis zum Abschluss der neu.

## <a name="see-also"></a>Siehe auch

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md) – erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Sicherheit Dienststatus in Azure Sicherheitscenter Überwachung](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Überwachung partnerlösungen mit Azure-Sicherheitscenter](security-center-partner-solutions.md) – erfahren, wie Sie den Status des Ihrer partnerlösungen zu überwachen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Blog suchen Beiträge zu Azure Sicherheit und Kompatibilität.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
