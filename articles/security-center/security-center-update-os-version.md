<properties
   pageTitle="OS Updateversion im Sicherheitscenter Azure | Microsoft Azure"
   description="In diesem Artikel wird gezeigt, wie Azure-Sicherheitscenter empfohlen **Update OS Version**implementiert wird."
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

# <a name="update-os-version-in-azure-security-center"></a>Aktualisieren Sie die Version des Betriebssystems in Azure-Sicherheitscenter

Für virtuelle Computer (virtuelle Computer) in der Cloud Services wird Azure-Sicherheitscenter empfiehlt sich, dass das Betriebssystem (BS) aktualisiert werden, wenn eine neuere Version zur Verfügung stehen.  Nur Cloud services Web und Worker Rollen in Herstellung, Steckplätze überwacht werden, ausgeführt.

> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt.  Dies ist keine schrittweise Anleitung.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlungen

1. Wählen Sie das Blade **Empfehlungen** **Betriebssystemversion aktualisieren**.
![Aktualisieren Sie die Version des Betriebssystems][1]

2. Dadurch wird das Blade **Update OS Version**geöffnet. Folgen Sie den Schritten in dieser Blade Betriebssystemversion aktualisieren aus.

## <a name="see-also"></a>Siehe auch

In diesem Artikel wird gezeigt, wie implementiert Sicherheitscenter empfohlen "Update OS Version". Weitere Informationen zum Cloud-Diensten und aktualisieren die Version des Betriebssystems für einen Clouddienst finden Sie unter:

- [Cloud-Dienste (Übersicht)](../cloud-services/cloud-services-choose-me.md)
- [So aktualisieren Sie einen Clouddienst](../cloud-services/cloud-services-update-azure-service.md)
- [So konfigurieren Sie Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md)

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md) – erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Sicherheit Dienststatus in Azure Sicherheitscenter Überwachung](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Überwachung partnerlösungen mit Azure-Sicherheitscenter](security-center-partner-solutions.md) – erfahren, wie Sie den Status des Ihrer partnerlösungen zu überwachen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Sicherheit von Azure Neuigkeiten und Informationen erhalten.

<!--Image references-->
[1]: ./media/security-center-update-os-version/update-os-version.png
