<properties
   pageTitle="Hinzufügen eine nächste Generation Firewall im Sicherheitscenter Azure | Microsoft Azure"
   description="Dieses Dokument wird gezeigt, wie die Sicherheitscenter Azure Empfehlungen **Hinzufügen eine nächsten Generation Firewall** und **Routing mit Eingangsport bis NGFW nur**implementieren."
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

# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Hinzufügen einer nächsten Generation Firewalls in Azure-Sicherheitscenter

Azure-Sicherheitscenter möglicherweise empfiehlt sich, dass Sie von einem Microsoft-Partner eine nächste Generation Firewall (NGFW eins) hinzufügen, um Ihre Sicherheitsmaßnahmen zu vergrößern. Dieses Dokument führt Sie durch ein Beispiel dazu.

> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt.  Dies ist keine schrittweise Anleitung.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlungen

1. Wählen Sie in das Blade **Empfehlungen** **Hinzufügen eine Firewall der nächsten Generation**aus.
![Fügen Sie eine Firewall der nächste Generation][1]

2. Wählen Sie in das **Hinzufügen einer nächsten Generation Firewall** Blade einen Endpunkt aus.
![Wählen Sie einen Endpunkt][2]

3. Eine zweite **Hinzufügen eine Firewall der nächsten Generation** Blade wird geöffnet. Sie können auch eine vorhandene Lösung verwenden, falls vorhanden, oder Sie können einen neuen erstellen. In diesem Beispiel stehen es keine vorhandenen Lösungen, damit wir eine neue NGFW erstellen können.
![Erstellen Sie neue nächsten Generation Firewall][3]

4. Wählen Sie zum Erstellen einer neuen NGFW einer Lösung aus der Liste der integrierten Partner zur Verfügung. In diesem Beispiel werden wir **Check Point**auswählen.
![Wählen Sie die nächste Generation Firewall-Lösung][4]

5. Das **Kontrollkästchen Punkt** Blade öffnet Sie Informationen über die Lösung Partner bereitgestellt. Wählen Sie in das Blade Informationen **Erstellen** aus.
![Firewall Informationen blade][5]

6. Das **Erstellen von virtuellen Computern** Blade wird geöffnet. Hier können Sie Informationen zum Einrichten eines virtuellen Computers (virtueller Computer) gedreht werden soll, der die NGFW ausgeführt werden kann erforderlich. Führen Sie die Schritte aus, und geben Sie die NGFW Informationen erforderlich. Wählen Sie OK, um Sie anzuwenden.
![Erstellen von virtuellen Computern um NGFW auszuführen.][6]

## <a name="route-traffic-through-ngfw-only"></a>Routing-Verkehr über NGFW nur

Kehren Sie zu der Blade **Empfehlungen** zurück. Ein neuer Eintrag wurde generiert, nachdem Sie eine NGFW über das Sicherheitscenter, hinzugefügt **Routing Verkehr über NGFW nur**aufgerufen. Nur, wenn Sie Ihre NGFW bis Sicherheitscenter installiert haben, wird dieser Empfehlungen erstellt. Wenn Sie Internet zugänglichen Endpunkte verfügen, wird Sicherheitscenter empfiehlt sich, dass Sie Netzwerk-Sicherheitsgruppe Regeln konfigurieren, die eingehenden Datenverkehr an Ihre virtuellen Computer durch Ihre NGFW erzwingen.

1. Wählen Sie in der **Empfehlungen Blade** **Routing Verkehr über NGFW nur**ein.
![Routing-Verkehr über NGFW nur][7]

2. Daraufhin wird das Blade **Datenverkehr durch NGFW nur** virtuellen Computern aufgeführt, die Sie den Datenverkehr in weiterleiten können. Wählen Sie einen virtuellen Computer aus der Liste aus.
![Auswählen eines virtuellen Computers][8]

3. Eine Blade für den ausgewählten virtuellen Computer wird geöffnet und verwandte eingehende Regeln. Eine Beschreibung bietet Ihnen weitere Informationen zu nächste Schritte. Wählen Sie mit dem Bearbeiten einer eingehenden Regel Fortfahren **Eingehende Regeln zu bearbeiten** . Es wird erwartet **Quelle** nicht auf **Alle** für das Internet zugänglichen Endpunkte verknüpft, mit der NGFW festgelegt ist. Weitere Informationen zu den Eigenschaften der eingehenden Regel finden Sie unter [NSG Regeln](../virtual-network/virtual-networks-nsg.md#nsg-rules).
![Konfigurieren von Regeln zum Einschränken des Zugriffs][9]
![eingehende Regel bearbeiten][10]

## <a name="see-also"></a>Siehe auch

Dieses Dokument wurde gezeigt, wie implementieren Sicherheitscenter empfohlen "Hinzufügen einer nächsten Generation Firewall". Weitere Informationen zu NGFWs und die Check Point-Partner-Lösung zu finden, probieren Sie Folgendes ein:

- [Nächsten Generation Firewall](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
- [Check Point vSEC](https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/)

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien.
- [Verwalten von Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md) – erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Sicherheit Dienststatus in Azure Sicherheitscenter Überwachung](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Überwachung partnerlösungen mit Azure-Sicherheitscenter](security-center-partner-solutions.md) – erfahren, wie Sie den Status des Ihrer partnerlösungen zu überwachen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Blog suchen Beiträge zu Azure Sicherheit und Kompatibilität.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
