<properties
   pageTitle="Agent im Sicherheitscenter Azure-virtuellen Computer aktivieren | Microsoft Azure"
   description="Dieses Dokument wird gezeigt, wie empfohlen Azure-Sicherheitscenter **Aktivieren virtueller Computer Agent**implementieren."
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

# <a name="enable-vm-agent-in-azure-security-center"></a>Agent im Sicherheitscenter Azure-virtuellen Computer aktivieren

Der Agent virtueller Computer muss in Reihenfolge [Datensammlung](security-center-enable-data-collection.md)aktivieren auf virtuellen Computern (virtuellen Computern) installiert sein.  Azure-Sicherheitscenter können Sie anzeigen, welche virtuellen Computern erfordern des virtuellen Computer-Agents und werden empfohlen, des virtuellen Computer-Agents auf diesen virtuellen Computern zu aktivieren.

Der virtuellen Computer Agent ist standardmäßig für virtuelle Computer installiert, die aus dem Azure Marketplace bereitgestellt werden. [Virtueller Computer-Agents und Erweiterungen – Teil 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) Artikel enthält Informationen zum Installieren des virtuellen Computer-Agents an.


> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt. Dies ist keine schrittweise Anleitung.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlungen

1. Wählen Sie in der **Empfehlungen Blade** **Virtueller Computer-Agent aktivieren**aus.
![Aktivieren der virtuellen Computer-Agents][1]

2. Daraufhin wird das Blade **Virtueller Computer Agent fehlende oder nicht reagiert**. Diese Blade Listet die virtuellen Computern, die der virtuellen Computer-Agent erfordern. Folgen Sie den Anweisungen auf dem Blade zum Installieren des virtuellen Computer-Agents aus.
![Virtueller Computer-Agent ist nicht vorhanden][2]

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
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
