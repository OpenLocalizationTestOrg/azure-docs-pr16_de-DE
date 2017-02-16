
<properties
   pageTitle="Erste Schritte beim Erstellen eines Internet gegenüberliegende Lastenausgleich im Modell über das klassische Azure-Portal zur klassischen Bereitstellung | Microsoft Azure"
   description="Informationen Sie zum Erstellen eines Internet gegenüberliegende Lastenausgleich im Modell zur klassischen Bereitstellung über das klassische Azure-portal"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a>Erste Schritte beim Erstellen eines Internet gegenüberliegende Lastenausgleich (klassisch) in der klassischen Azure-portal

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell klassischen. Sie können auch an, [wie ein Lastenausgleich mit Azure Ressourcenmanager gegenüberliegende Internet erstellt werden](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Einrichten eines Internet zugänglichen Lastenausgleich für virtuellen Computern

Saldo Netzwerkverkehr über den virtuellen Computern der Cloud-Dienst aus dem Internet laden, müssen Sie einen Satz mit Lastenausgleich erstellen. Dieses Verfahren setzt voraus, dass Sie die virtuellen Computer bereits erstellt haben und dass sie alle innerhalb der gleichen Cloud-Dienst sind.

**So konfigurieren Sie eine Gruppe mit Lastenausgleich für virtuellen Computern**

1. Im Portal Azure klassischen klicken Sie auf **virtuellen Computern**, und klicken Sie dann auf den Namen eines virtuellen Computers mit Lastenausgleich festlegen.

2. Klicken Sie auf die **Endpunkte**, und klicken Sie dann auf **Hinzufügen**.

3. Klicken Sie auf der Seite **hinzufügen einen Endpunkt eines virtuellen Computers** auf den Pfeil nach rechts.

4. Klicken Sie auf der Seite **Details der den Endpunkt angeben** :

    * Geben Sie einen Namen für den Endpunkt **Namen**oder wählen Sie den Namen aus der Liste der vordefinierten Endpunkte für allgemeine Protokolle.
    * Wählen Sie im Feld **Protokoll**das Protokoll erforderlich, indem Sie den Typ des Endpunkts, TCP oder UDP, je nach Bedarf.
    * Geben Sie die Portnummern die gewünschte des virtuellen Computers verwenden, je nach Bedarf in **öffentlichen und privaten Ports**. Können die privaten Port und die Firewall-Regeln des virtuellen Computers um zu leiten Sie den Datenverkehr auf eine Weise, die für die Anwendung geeignet ist. Private Anschluss kann den öffentlichen Port identisch sein. Für einen Endpunkt für Datenverkehr im Web (HTTP), können Sie beispielsweise Port 80 an den öffentlichen und privaten Port zuweisen.

5. Wählen Sie **Erstellen einer Gruppe mit Lastenausgleich**aus, und klicken Sie dann auf den Pfeil nach rechts.

6. Klicken Sie auf der Seite **Konfigurieren mit Lastenausgleich festlegen** Geben Sie einen Namen für den Lastenausgleich ein, und weisen Sie die Werte für den Lastenausgleich Azure Prüfpunkt Verhalten. Der Lastenausgleich verwendet Prüfpunkte, um festzustellen, ob die virtuellen Computer in der Gruppe mit Lastenausgleich eingehenden Datenverkehr empfangen zur Verfügung stehen.

7. Klicken Sie auf das Häkchen, um den Lastenausgleich Endpunkt zu erstellen. **Ja,** werden in der Spalte **mit Lastenausgleich SetName** Seitenrand **Endpunkte** des virtuellen Computers angezeigt.

8. Im Portal klicken Sie auf **virtuellen Computern**, klicken Sie auf den Namen eines weiteren virtuellen Computers Lastenausgleich festlegen, klicken Sie auf die **Endpunkte**, und klicken Sie dann auf **Hinzufügen**.

9. Klicken Sie auf der Seite **hinzufügen einen Endpunkt eines virtuellen Computers** klicken Sie auf **Add Endpunkt zu einem bestehenden Satz von Lastenausgleich**, wählen Sie den Namen der Gruppe mit Lastenausgleich, und klicken Sie dann auf den Pfeil nach rechts.

10. Klicken Sie auf der Seite **Geben Sie die Details des Endpunkts** Geben Sie einen Namen für den Endpunkt, und klicken Sie dann auf das Häkchen.

Wiederholen Sie für den zusätzlichen virtuellen Computern mit Lastenausgleich festlegen die Schritte 8 bis 10.



## <a name="next-steps"></a>Nächste Schritte

[Erste Schritte zum Konfigurieren einer internen Lastenausgleich](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurieren eines laden Lastenausgleich Verteilung Modus](load-balancer-distribution-mode.md)

[Konfigurieren von Einstellungen zur im Leerlauf TCP Timeout für Ihre Lastenausgleich](load-balancer-tcp-idle-timeout.md)

