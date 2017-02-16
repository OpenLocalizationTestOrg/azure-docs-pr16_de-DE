<properties
   pageTitle="Beschränken Sie den Zugriff über das Internet zugänglichen Endpunkte im Sicherheitscenter Azure | Microsoft Azure"
   description="Dieses Dokument wird gezeigt, wie der Azure-Sicherheitscenter Empfehlungen **schränken Sie den Zugriff über das Internet zugänglichen Endpunkt**implementieren."
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

# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Zugriff über das Internet zugänglichen Endpunkte im Sicherheitscenter Azure einschränken

Azure-Sicherheitscenter wird empfiehlt sich, dass Sie Zugriff über das Internet zugänglichen Endpunkte beschränken, wenn Ihr Netzwerk-Sicherheitsgruppen (NSGs) enthält eine oder mehrere eingehende Regeln, die Zugriff von "alle" Quelle IP-Adresse zulassen. Öffnen des Zugriffs auf "Beliebig" möglicherweise Angreifern Zugriff auf Ihre Ressourcen aktivieren. Sicherheitscenter wird empfiehlt sich, dass Sie diese eingehenden Regeln zum Einschränken des Zugriffs auf Quelle IP-Adressen, die Zugriff tatsächlich benötigen bearbeiten.

Diese Empfehlungen wird für alle nicht-Anschluss generiert, "alle" als Quelle enthält.

> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt. Dies ist keine schrittweise Anleitung.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlungen

1. Wählen Sie in der **Empfehlungen Blade** **schränken Sie den Zugriff über das Internet zugänglichen Endpunkt**ein.
![Schränken Sie Zugriff über das Internet gegenüberliegende Endpunkt ein][1]

2. Dadurch wird das Blade **schränken Sie den Zugriff über das Internet zugänglichen Endpunkt**geöffnet. Diese Blade Listen den virtuellen Computern (virtuellen Computern) mit eingehende Regeln, die ein Sicherheitsrisiko entsteht erstellen. Wählen Sie einen virtuellen Computer aus.
![Auswählen eines virtuellen Computers][2]

3. Das Blade **NSG** zeigt Sicherheitsgruppe Netzwerk, verwandte eingehende Regeln und den zugehörigen virtuellen Computer an. Wählen Sie mit dem Bearbeiten einer eingehenden Regel Fortfahren **Eingehende Regeln zu bearbeiten** .
![Netzwerk-Sicherheitsgruppe blade][3]

4. Wählen Sie auf das **eingehende Sicherheitsregeln** Blade eingehende Regel bearbeiten aus. Wählen Sie in diesem Beispiel **AllowWeb**uns aus.
![Eingehende Sicherheitsregeln][4]

  Beachten Sie, dass Sie auch **Regeln für das standardmäßige** zum Festlegen von Regeln für das standardmäßige enthalten sind, indem Sie alle NSGs finden Sie unter auswählen können. Die Regeln für die nicht gelöscht werden, aber, da sie eine geringere Priorität zugewiesen sind, können sie durch die Regeln, die Sie erstellen überschrieben werden. Weitere Informationen zu [Regeln für das standardmäßige](../virtual-network/virtual-networks-nsg.md#default-rules).
![Standardregeln][5]

5. Klicken Sie auf das Blade **AllowWeb** Bearbeiten der Eigenschaften der eingehenden Regel so, dass die **Quelle** einer IP-Adresse oder einen Block von IP-Adressen befindet. Weitere Informationen zu den Eigenschaften der eingehenden Regel finden Sie unter [NSG Regeln](../virtual-network/virtual-networks-nsg.md#nsg-rules).

  ![Eingehende Regel bearbeiten][6]

## <a name="see-also"></a>Siehe auch

In diesem Artikel wird gezeigt, wie implementiert Sicherheitscenter empfohlen "Über das Internet zugänglichen Endpunkt Zugriff einschränken". Weitere Informationen zum Aktivieren von NSGs und Regeln, probieren Sie Folgendes ein:

- [Was ist ein Netzwerk Sicherheit Gruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Zum Verwalten von NSGs über das Azure-portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md)– Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwalten der Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md)– erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Sicherheit Dienststatus überwachen im Sicherheitscenter Azure](security-center-monitoring.md)– erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md)– Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Überwachung partnerlösungen mit Azure-Sicherheitscenter](security-center-partner-solutions.md) – erfahren, wie Sie den Status des Ihrer partnerlösungen zu überwachen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md)– suchen häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/)– Sicherheit von Azure Neuigkeiten und Informationen erhalten.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
