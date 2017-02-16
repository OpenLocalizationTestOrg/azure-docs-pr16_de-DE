<properties
   pageTitle="Konfigurieren eines virtuellen Netzwerk und dem Gateway für ExpressRoute im Portal klassischen | Microsoft Azure"
   description="In diesem Artikel führt Sie durch das Einrichten eines virtuellen Netzwerks für ExpressRoute mit dem klassischen Bereitstellungsmodell und klassischen-Portal an."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="09/20/2016"
   ms.author="cherylmc"/>

# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a>Erstellen eines virtuellen Netzwerks für ExpressRoute im klassischen-Portal

Die Schritte in diesem Artikel führt Sie durch die Konfiguration von einem virtuellen Netzwerk und eines Gateways virtuelles Netzwerk zur Verwendung mit ExpressRoute mit dem klassischen Bereitstellungsmodell und das klassische Portal.

Wenn Sie Anweisungen für das Modell zur Bereitstellung von Ressourcenmanager suchen, können Sie die folgenden Artikeln: [Erstellen eines virtuellen Netzwerks mithilfe der PowerShell](../virtual-network/virtual-networks-create-vnet-arm-ps.md) und [Hinzufügen einer VPN-Gateway zu einem Ressourcenmanager VNet für ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).

**Informationen zu Datenmodellen Azure-Bereitstellung**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="create-a-classic-vnet-and-gateway"></a>Erstellen eines klassischen VNet und Gateways

Die folgenden Schritte durch Erstellen einer klassischen VNet und ein Gateway virtuelles Netzwerk. Wenn Sie bereits eine klassische VNet haben, finden Sie im Abschnitt [Konfigurieren einer vorhandenen klassischen VNet](#config) in diesem Artikel.

1. Melden Sie sich im [Azure klassischen Portal](http://manage.windowsazure.com).

2. Klicken Sie in der unteren linken Ecke des Bildschirms auf **neu**. Klicken Sie im Navigationsbereich klicken Sie auf **Netzwerkdienste**, und klicken Sie dann auf **Virtuelles Netzwerk**. Klicken Sie auf **Benutzerdefinierte erstellen** , um den Konfigurations-Assistenten zu starten.

3. Geben Sie auf der Seite **Details für virtuelle Netzwerk** Folgendes ein:

    - **Name** – Name virtuellen Netzwerks. Dieser virtuelle Netzwerkname wird verwendet werden, wenn Sie Ihre Instanzen virtuellen Computern und PaaS bereitstellen, damit Sie nicht den Namen zu kompliziert machen möchten.
    - **Speicherort** – der Speicherort direkt bezieht sich auf den Standort (Bereich), werden Ihre Ressourcen (virtuelle Computer) befinden soll. Wählen Sie diesem Speicherort angenommen, Sie gegebenenfalls die virtuellen Computern, die Sie für diese virtuelle Netzwerk physisch im südostasiatischen US befinden bereitstellen. Sie können nicht ändern die Region Ihres Netzwerks virtuelle zugeordnet, nachdem Sie es erstellt haben.

4. Klicken Sie auf der Seite **DNS-Server und VPN-Konnektivität** Geben Sie die folgenden Informationen ein, und klicken Sie dann auf den nächsten Pfeil in der unteren rechten Ecke. 

    - **DNS-Server** - Geben Sie den DNS-Servernamen und die IP-Adresse ein, oder wählen Sie einen zuvor registrierten DNS-Server aus dem Kontextmenü. Diese Einstellung werden keine DNS-Server erstellt. Sie können Sie die DNS-Server angeben, die Sie für die Auflösung von Namen für diese virtuelle Netzwerk verwenden möchten.
    - **Konnektivität zwischen Standorten** – aktivieren Sie das Kontrollkästchen für das **Konfigurieren eines Standort-zu-Standort VPN**.
    - **ExpressRoute** – aktivieren Sie das Kontrollkästchen **ExpressRoute verwenden**. Diese Option wird nur angezeigt, wenn Sie das **Konfigurieren einer Standort-zu-Standort VPN**ausgewählt haben.
    - **Lokales Netzwerk** - Sie sind erforderlich, um eine lokale Netzwerk-Website für ExpressRoute haben. Allerdings werden bei einer Verbindung ExpressRoute die Adresspräfixe für lokales Netzwerk-Website angegebenen ignoriert. In diesem Fall werden die Adresspräfixe an Microsoft durch die Verbindung ExpressRoute angekündigt für das routing verwendet werden.<BR>Wenn Sie bereits über ein lokales Netzwerk für die Verbindung ExpressRoute erstellt haben, können Sie ihn aus der Dropdownliste auswählen. Wenn dies nicht der Fall ist, wählen Sie **ein neues lokales Netzwerk-angeben**.

5. Die **Konnektivität zwischen Standorten** Seite angezeigt wird, wenn Sie ausgewählt haben, um ein neues lokales Netzwerk im vorherigen Schritt anzugeben. Um Ihr lokales Netzwerk zu konfigurieren, geben Sie die folgenden Informationen ein, und klicken Sie dann auf den Pfeil nach rechts. 

    - **Name** – den Namen, die Sie Ihren lokalen (lokal) anrufen möchten network Website an.
    - **Adressbereichs** – einschließlich erste IP- und CIDR (Anzahl der Adressen). Sie können alle Adressbereichs angeben, solange Sie ihn mit den Adressbereich für Ihr Netzwerk virtuelle überlappt. In der Regel Dies würde die Adressbereiche für Ihre lokale Netzwerke angeben, aber bei ExpressRoute, diese Einstellungen nicht verwendet werden. Diese Einstellung ist jedoch erforderlich, im lokale Netzwerk zu erstellen, wenn Sie das klassische Portal verwenden.
    - **Hinzufügen von Adressbereichs** - diese Einstellung ist nicht relevant für ExpressRoute.


6. Klicken Sie auf der Seite **Virtuellen Netzwerk Adresse Leerzeichen** Geben Sie die folgenden Informationen ein, und klicken Sie dann auf das Häkchen unten rechts in Ihrem Netzwerk konfigurieren. 

    - **Adressbereichs** – einschließlich IP-Adresse und Adresse zählen beginnen. Stellen Sie sicher, dass die Adresse Leerzeichen, die Sie angeben sich die Adressräume überschneiden nicht, die Sie in Ihrem lokalen Netzwerk besitzen.
    - **Hinzufügen Subnetz** - erste IP- und die Anzahl der Adressen einschließlich. Weitere Subnetze sind nicht erforderlich.
    - **Hinzufügen Gateway Subnetz** - klicken Sie auf das Gateway Subnetz hinzufügen. Das Gateway Subnetz wird nur für das Gateway virtuelles Netzwerk verwendet und für diese Konfiguration erforderlich ist.<BR>Das Gateway Subnetz CIDR (Anzahl der Adressen) für ExpressRoute muss /28 oder größere (/ 27, / 26 usw..). Auf diese Weise können für genügend IP-Adressen in diesem Subnetz die Konfiguration entwickelt zulassen. Wenn Sie das Kontrollkästchen, um ExpressRoute, verwenden Sie ausgewählt haben, gibt im Portal im Portal klassischen ein Gateway Subnetz mit /28.  Sie können nicht die Anzahl der CIDR-Adresse in der klassischen Portal anpassen. Das Gateway Subnetz wird als **Gateway** im Portal klassischen angezeigt, obwohl die real Name der Gateways Subnetz gehören, die erstellt wird tatsächlich **GatewaySubnet**ist. Sie können diesen Namen mithilfe der PowerShell oder der Azure-Portal anzeigen.

7. Klicken Sie auf das Häkchen klicken Sie auf den unteren Rand der Seite und virtuellen Netzwerks beginnt mit dem erstellen. Wenn es abgeschlossen ist, sehen Sie sich, dass **erstellt** unter **Status** auf der Seite **Netzwerke** im Portal klassischen aufgelistet.

## <a name="a-namegwacreate-the-gateway"></a><a name="gw"></a>Erstellen des Gateways

1. Klicken Sie auf der Seite **Netzwerke** klicken Sie auf das virtuelle Netzwerk aus, das Sie soeben erstellt haben und dann auf **Dashboard** am oberen Rand der Seite.

2. Klicken Sie am unteren Rand der Seite **Dashboard** klicken Sie auf **Gateway erstellen** , und wählen Sie **Dynamisches Routing**. Klicken Sie auf **Ja** , um zu bestätigen, dass Sie einen Gateway erstellen möchten.

3. Wenn das Gateway beginnt mit der Erstellung, sehen Sie die Vermietung einer Nachricht, die Sie wissen, dass das Gateway gestartet wurde. Es dauert bis zu 45 Minuten für das Gateway zu erstellen.

4. Verknüpfen Sie Ihr Netzwerk mit eine Verbindung. Folgen Sie den Anweisungen im Artikel [VNets zu ExpressRoute Schaltkreise verknüpfen](expressroute-howto-linkvnet-classic.md).

## <a name="a-nameconfigaconfigure-an-existing-classic-vnet-for-expressroute"></a><a name="config"></a>Konfigurieren einer vorhandenen klassischen VNet für ExpressRoute

Wenn Sie bereits eine klassische VNet verfügen, können Sie ihn in der klassischen Portal eine Verbindung zu ExpressRoute konfigurieren. Die Einstellungen sind identisch mit den Abschnitten oben, also durch diese Abschnitte mit den erforderlichen Einstellungen vertraut zu lesen. Wenn Sie eine gleichzeitig vorhandener ExpressRoute/Website-zu-Standort-Verbindung erstellen möchten, finden Sie die Schritte [in diesem Artikel](expressroute-howto-coexist-classic.md) . Sie unterscheiden sich die Schritte in diesem Artikel zur Verfügung.
 
1. Sie müssen im lokale Netzwerk erstellen, bevor Sie die restlichen Ihre Einstellungen VNet aktualisieren. Klicken Sie auf **neu** , um ein neues lokales Netzwerk zu erstellen, die erforderlich ist, wenn ExpressRoute über das klassische Portal konfigurieren, **>** **Network Services** **>** **Virtuelles Netzwerk** **>** **Lokales Netzwerk hinzufügen**. Folgen Sie den Schritten des Assistenten zum Erstellen des lokalen Netzwerks ein.

2. Verwenden Sie die Seite **Konfigurieren** zu den Rest der Einstellungen für Ihre VNet aktualisieren und die VNet mit dem lokalen Netzwerk zugeordnet werden soll.

3. Nach dem Konfigurieren der Einstellungen, wechseln Sie zum [Erstellen des Gateways](#gw) Abschnitt in diesem Artikel, um das Gateway zu erstellen.


## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie virtuellen Computern mit Ihrem Netzwerk virtuelle hinzufügen möchten, finden Sie unter [virtuellen Computern learning Pfade](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
- Wenn Sie weitere Informationen zur ExpressRoute möchten, lesen Sie die [ExpressRoute Übersicht](expressroute-introduction.md).


 
