<properties
   pageTitle="Netzwerk-Sicherheitsgruppen in Azure Sicherheitscenter aktivieren | Microsoft Azure"
   description="Dieses Dokument wird gezeigt, wie empfohlen Azure-Sicherheitscenter **Aktivieren Netzwerk Sicherheitsgruppen**implementieren."
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

# <a name="enable-network-security-groups-in-azure-security-center"></a>Netzwerk-Sicherheitsgruppen in Azure-Sicherheitscenter aktivieren

Azure-Sicherheitscenter wird empfiehlt sich, dass Sie eine Netzwerksicherheitsgruppe (NSG) aktivieren, wenn Sie noch nicht aktiviert ist. NSGs enthalten eine Liste mit Access Control Liste (ACL) Regeln, die zulassen oder verhindern Netzwerkverkehr auf Ihre Instanzen virtueller Computer in einem virtuellen Netzwerk an. NSGs können entweder Subnets oder einzelne Instanzen von virtuellen Computer in diesem Subnetz zugeordnet werden. Wenn eine NSG ein Subnetz zugeordnet ist, wenden Sie ACL-Regeln auf alle Instanzen virtueller Computer in diesem Subnetz aus. Darüber hinaus kann den Datenverkehr in einem einzelnen virtuellen Computer beschränkt werden weitere durch Zuordnen einer NSG direkt an diesen virtuellen Computer. Weitere finden Sie unter Informationen [Neuigkeiten ein Netzwerk Sicherheit Gruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)

Wenn Sie keinen NSGs aktiviert präsentieren Sicherheitscenter zwei Empfehlungen Ihnen: Aktivieren von Netzwerk-Sicherheitsgruppen auf Subnetze und Netzwerk-Sicherheitsgruppen auf virtuellen Computern aktivieren. Sie auswählen, welche Ebene, Subnetz oder virtueller Computer, um NSGs anzuwenden.


> [AZURE.NOTE] Dieses Dokument wird den Dienst mithilfe einer Beispiel-bereitstellungs eingeführt.  Dies ist keine schrittweise Anleitung.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlungen

1. Wählen Sie in das Blade **Empfehlungen** in Subnetzen oder auf virtuellen Computern **Netzwerk Sicherheitsgruppen aktivieren** aus.
![Aktivieren Sie Netzwerk-Sicherheitsgruppen][1]

2. Daraufhin wird das Blade **Konfigurieren fehlende Netzwerk Sicherheitsgruppen** für Subnetze oder virtuellen Computern, je nach empfohlen, die Sie ausgewählt haben. Wählen Sie ein Subnetz oder eines virtuellen Computers so konfigurieren Sie eine NSG auf.

  ![Konfigurieren von NSG für Subnetz][2]

  ![NSG für virtuellen Computer konfigurieren][3]
3. Wählen Sie eine vorhandene NSG aus, oder klicken Sie auf das **Netzwerk-Sicherheitsgruppe auswählen** Blade wählen Sie zum Erstellen einer neuen NSG.

  ![Wählen Sie Netzwerk-Sicherheitsgruppe][4]

Wenn Sie eine neue NSG erstellt haben, folgen Sie den Schritten [zum Verwalten von NSGs über das Azure-Portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md) Erstellen einer NSG und Sicherheitsregeln festlegen.

## <a name="see-also"></a>Siehe auch

In diesem Artikel gezeigt, wie man Sicherheitscenter empfohlen "Netzwerk Sicherheitsgruppen aktivieren" für Subnetze oder virtuellen Computern implementieren. Erfahren Sie mehr über das Aktivieren der NSGs, probieren Sie Folgendes ein:

- [Was ist ein Netzwerk Sicherheit Gruppe (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Zum Verwalten von NSGs über das Azure-portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

Weitere Informationen zum Sicherheitscenter, probieren Sie Folgendes ein:

- [Einrichten von Sicherheitsrichtlinien für die in Azure Sicherheitscenter](security-center-policies.md) – Informationen zum Konfigurieren von Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheit Empfehlungen im Sicherheitscenter Azure](security-center-recommendations.md) – erfahren Sie, wie Empfehlungen im Zusammenhang mit Azure Ressourcen zu schützen.
- [Sicherheit Dienststatus in Azure Sicherheitscenter Überwachung](security-center-monitoring.md) – erfahren, wie Sie die Integrität des Azure Ressourcen zu überwachen.
- [Verwalten von und Beantworten von Sicherheitshinweisen im Sicherheitscenter Azure](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Beantworten von Sicherheitshinweisen.
- [Überwachung partnerlösungen mit Azure-Sicherheitscenter](security-center-partner-solutions.md) – erfahren, wie Sie den Status des Ihrer partnerlösungen zu überwachen.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – suchen häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Sicherheit von Azure Neuigkeiten und Informationen erhalten.

<!--Image references-->
[1]: ./media/security-center-enable-nsg/enable-nsg.png
[2]:./media/security-center-enable-nsg/configure-nsg-for-subnet.png
[3]: ./media/security-center-enable-nsg/configure-nsg-for-vm.png
[4]: ./media/security-center-enable-nsg/choose-nsg.png
