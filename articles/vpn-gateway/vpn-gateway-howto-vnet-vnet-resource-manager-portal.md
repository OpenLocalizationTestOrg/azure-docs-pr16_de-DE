<properties
   pageTitle="Verbinden mit dem Modell zur Bereitstellung von Ressourcenmanager und Azure-Portal VNets | Microsoft Azure"
   description="Erstellen Sie eine VPN-Gateway-Verbindung zwischen VNets mithilfe von Ressourcenmanager und Azure-Portal an."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vnet-to-vnet-connection-using-the-azure-portal"></a>Konfigurieren einer VNet-VNet-Verbindung mit dem Azure-portal

> [AZURE.SELECTOR]
- [Ressourcenmanager - Azure-Portal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Ressourcenmanager - PowerShell](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klassische - klassischen-Portal](virtual-networks-configure-vnet-to-vnet-connection.md)


In diesem Artikel führt Sie durch die Schritte zum Erstellen einer Verbindungs zwischen VNets im Bereitstellungsmodell Ressourcenmanager mithilfe von VPN-Gateway und Azure-Portal an.

Wenn Sie das Portal Azure Verbindung virtuelle Netzwerke verwenden, muss die VNets im selben-Abonnement. Wenn Ihre virtuelle Netzwerke in anderen Abonnements sind, können Sie diese weiterhin mithilfe der [PowerShell](vpn-gateway-vnet-vnet-rm-ps.md) Schritte verbinden.

![V2V-Diagramm](./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Bereitstellungsmodelle und Methoden für VNet-VNet-Verbindungen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

In der folgenden Tabelle zeigt die aktuell verfügbare Bereitstellung-Modelle und Methoden für VNet-VNet-Konfigurationen. Wenn ein Artikel mit Konfigurationsschritte verfügbar ist, verknüpfen wir aus dieser Tabelle direkt an.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>VNet peering

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>Informationen zu VNet-VNet-Verbindungen

Herstellen einer Verbindung ein virtuelles Netzwerk zu einem anderen virtuellen Netzwerk ähnelt (VNet VNet) eine VNet in einer lokalen Website verbinden. Beide Typen Connectivity mit einer Azure VPN Gateway können einen sicheren Tunnel mit IPSec-/IKE bereitstellen. Die VNets, die Sie eine Verbindung herstellen kann in unterschiedlichen Regionen oder in anderen Abonnements sein.

Sie können auch VNet-VNet-Kommunikation mit mehreren Standorten Konfigurationen kombinieren. Diese Berechtigung ermöglicht, die Sie einführen Netzwerktopologien, die kombinieren Cross lokale Konnektivität mit zwischen virtuelle Netzwerkkonnektivität, wie in der folgenden Abbildung dargestellt:

![Informationen zu Verbindungen] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/aboutconnections.png "Informationen zu Verbindungen")

### <a name="why-connect-virtual-networks"></a>Warum keine Verbindung herstellen mit virtuelle Netzwerke?

Möglicherweise virtuelle Netzwerke aus den folgenden Gründen eine Verbindung herstellen möchten:

- **Cross Region Geo-Redundanz und Geo-Anwesenheitsstatus**
    - Sie können eigene Geo-Replikation oder die Synchronisierung mit sichere Konnektivität einrichten ohne Überblick über das Internet zugänglichen Endpunkte.
    - Mit Azure Datenverkehr Manager und Lastenausgleich können Sie über mehrere Azure Regionen hoch verfügbaren Arbeitsbelastung mit Geo-Redundanz einrichten. Beispiel für eine wichtige besteht darin, SQL immer auf mit Verfügbarkeit Gruppen verteilen auf mehrere Azure Regionen einzurichten.

- **Regionale mit mehreren Ebenen Applikationen mit Isolation oder administrative Grenze**
    - Innerhalb derselben Region können Sie mit mehreren Ebenen Applikationen mit mehreren virtuellen Netzwerken aufgrund von Isolation oder Verwaltungsaufwand miteinander verbunden einrichten.

Weitere Informationen zu VNet-VNet-Verbindungen finden Sie im [VNet-VNet – häufig gestellte Fragen](#faq) am Ende dieses Artikels.

### <a name="a-namevaluesaexample-settings"></a><a name="values"></a>Beispiel-Einstellungen

Wenn Sie diese Schritte als Übung verwenden, können Sie die Konfiguration Beispielwerte verwenden. Aus Gründen der Beispiel verwenden wir mehrere Adressbereiche für jede VNet ein. VNet-VNet-Konfigurationen erfordern jedoch nicht mehrere Adresse Leerzeichen.

**Werte für TestVNet1:**

- VNet Name: TestVNet1
- Leerzeichen zu beheben: 10.11.0.0/16
    - Subnetnamen: Front-End
    - Subnetzadressbereichs: 10.11.0.0/24
- Ressourcengruppe: TestRG1
- Standort: Ostasiatischen US
- Leerzeichen zu beheben: 10.12.0.0/16
    - Subnetnamen: Back-End-
    - Subnetzadressbereichs: 10.12.0.0/24
- Subnetz gatewayname: GatewaySubnet (Dies wird automatisch eingetragen im Portal)
    - Gateway Subnetzadressbereichs: 10.11.255.0/27
- DNS-Server: Verwenden Sie die IP-Adresse Ihres DNS-Servers
- Virtuelle Netzwerk gatewayname: TestVNet1GW
- Gateway-Typ: VPN
- VPN-Typ: Routing-basierten
- SKU: Wählen Sie die zu verwendende Gateway-SKU
- Namen von öffentlichen IP-Adresse: TestVNet1GWIP
- Werte für die Verbindung:
    - Name: TestVNet1toTestVNet4
    - Freigegebene Schlüssel: Sie können den freigegebenen Schlüssel selbst erstellen. In diesem Beispiel verwenden wir abc123. Wichtig ist, wenn Sie die Verbindung zwischen der VNets erstellen, der Wert entsprechen muss.

**Werte für TestVNet4:**

- VNet Name: TestVNet4
- Leerzeichen zu beheben: 10.41.0.0/16
    - Subnetnamen: Front-End
    - Subnetzadressbereichs: 10.41.0.0/24
- Ressourcengruppe: TestRG1
- Standort: Westen US
- Leerzeichen zu beheben: 10.42.0.0/16
    - Subnetnamen: Back-End-
    - Subnetzadressbereichs: 10.42.0.0/24
- GatewaySubnet Name: GatewaySubnet (Dies wird automatisch eingetragen im Portal)
    - GatewaySubnet Adressbereichs: 10.41.255.0/27
- DNS-Server: Verwenden Sie die IP-Adresse Ihres DNS-Servers
- Virtuelle Netzwerk gatewayname: TestVNet4GW
- Gateway-Typ: VPN
- VPN-Typ: Routing-basierten
- SKU: Wählen Sie die zu verwendende Gateway-SKU
- Namen von öffentlichen IP-Adresse: TestVNet4GWIP
- Werte für die Verbindung:
    - Name: TestVNet4toTestVNet1
    - Freigegebene Schlüssel: Sie können den freigegebenen Schlüssel selbst erstellen. In diesem Beispiel wird abc123 verwendet. Wichtig ist, wenn Sie die Verbindung zwischen der VNets erstellen, der Wert entsprechen muss.


## <a name="a-namecreatvneta1-create-and-configure-testvnet1"></a><a name="CreatVNet"></a>1. erstellen und Konfigurieren von TestVNet1

Wenn Sie bereits eine VNet haben, stellen Sie sicher, dass die Einstellungen mit den VPN-Gatewayentwurf kompatibel sind. Achten Sie besonders an alle Subnetze, die mit anderen Netzwerken überlappen möglicherweise. Wenn sich überlappenden Subnetze funktionieren die Verbindung ordnungsgemäß nicht. Wenn Ihre VNet mit den korrekten Einstellungen so konfiguriert ist, können Sie die Schritte im Abschnitt [Geben Sie einen DNS-Server](#dns) beginnen.

### <a name="to-create-a-virtual-network"></a>So erstellen ein virtuelles Netzwerk

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="a-namesubnetsa2-add-additional-address-space-and-create-subnets"></a><a name="subnets"></a>2. hinzufügen zusätzliche Adressbereichs und Erstellen von Subnetzen

Sie können zusätzliche Adresse Abstand und Subnetze erstellen, nachdem Sie Ihre VNet erstellt wurde.
[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)]

## <a name="a-namegatewaysubneta3-create-a-gateway-subnet"></a><a name="gatewaysubnet"></a>3 Erstellen Sie 3 ein Gateway Subnetz

Vor dem Verbinden Ihrer virtuellen Netzwerks zu einem Gateway, müssen Sie zuerst das Gateway Subnetz für das virtuelle Netzwerk erstellen, dem Sie eine Verbindung herstellen möchten. Falls möglich, empfiehlt es sich, erstellen Sie ein Gateway Subnetz einen Textblock CIDR /28 oder /27 verwenden, um genügend IP-Adressen, um zusätzliche zukünftigen Konfiguration Bedürfnisse bereitstellen zu können.

Wenn Sie diese Konfiguration als Übung erstellen, finden Sie in diese [Einstellungen Beispiel](#values) , wenn Ihr Subnetz Gateway zu erstellen.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

### <a name="to-create-a-gateway-subnet"></a>So erstellen Sie ein Gateway Subnetz

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="a-namednsservera4-specify-a-dns-server-optional"></a><a name="DNSServer"></a>4 Geben Sie 4 einen DNS-Server (optional)

Wenn Sie mit einer namensauflösung von für die virtuellen Computern haben, die in Ihrem VNets bereitgestellt werden möchten, sollten Sie einen DNS-Server angeben.

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]


## <a name="a-namevnetgatewaya5-create-a-virtual-network-gateway"></a><a name="VNetGateway"></a>5. Erstellen eines Gateways virtuelles Netzwerk

In diesem Schritt erstellen Sie das Gateway virtuelles Netzwerk für Ihre VNet. Dieser Schritt kann bis zu 45 Minuten in Anspruch nehmen. Wenn Sie diese Konfiguration als Übung erstellen, können Sie an den [Einstellungen Beispiel](#values)verweisen.

### <a name="to-create-a-virtual-network-gateway"></a>Erstellen eines Gateways virtuelles Netzwerk

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


## <a name="a-namecreatetestvnet4a6-create-and-configure-testvnet4"></a><a name="CreateTestVNet4"></a>6. erstellen und Konfigurieren von TestVNet4

Nachdem Sie TestVNet1 konfiguriert haben, erstellen Sie TestVNet4 durch den vorherigen Schritten, ersetzen die Werte, mit denen der TestVNet4 wiederholen. Sie müssen nicht warten, bis das Gateway virtuelles Netzwerk für TestVNet1 erstellen, bevor Sie TestVNet4 abgeschlossen ist. Wenn Sie Ihre eigenen Werte verwenden, stellen Sie sicher, dass die Adresse Leerzeichen mit einer der VNets nicht, die Sie überlappen in eine Verbindung herstellen möchten.


## <a name="a-nametestvnet1connectiona7-configure-the-testvnet1-connection"></a><a name="TestVNet1Connection"></a>7 konfigurieren Sie 7 die Verbindung TestVNet1

Wenn die virtuelle Netzwerkgateways für TestVNet1 und TestVNet4 abgeschlossen haben, können Sie virtuelle Netzwerks Gateway-Verbindungen erstellen. In diesem Abschnitt erstellen Sie eine Verbindung von VNet1 zu VNet4.

1. Navigieren Sie in **alle Ressourcen**mit dem Gateway virtuelles Netzwerk für Ihre VNet. Beispielsweise **TestVNet1GW**. Klicken Sie auf **TestVNet1GW** , um das Gateway-Blade virtuelles Netzwerk zu öffnen.

    ![Verbindungen blade] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/settings_connection.png "Verbindungen blade")

2. Klicken Sie auf **+ Add** um das Blade **Verbindung hinzufügen** zu öffnen.

3. Geben Sie in der Blade **Verbindung hinzufügen** in das Namensfeld einen Namen für die Verbindung aus. Beispielsweise **TestVNet1toTestVNet4**.

    ![Name der Verbindung] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/v1tov4.png "Name der Verbindung")

4. Für **Verbindungstyp**. Wählen Sie **VNet-zu-VNet** aus der Dropdownliste aus.

5. Der **erste virtuelle Netzwerkgateway** Feldwert wird automatisch ausgefüllt, da Sie diese Verbindung vom Gateway angegebenen virtuelles Netzwerk erstellen.

6. Das **zweite virtuelle Netzwerk-Gateway** -Feld ist als virtuelles Netzwerk-Gateway der VNet, die Sie eine Verbindung mit erstellen möchten. Klicken Sie auf, **Wählen Sie ein anderes virtuelles Netzwerk-Gateway aus** , um das Blade **virtuelle Netzwerk-Gateway auswählen** zu öffnen.

    ![Verbindung hinzufügen] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/add_connection.png "Hinzufügen einer Verbindung")

7. Zeigen Sie die aufgelisteten virtuelles Netzwerkgateways auf diese Blade an Beachten Sie, dass nur virtuelle Netzwerkgateways, die im Rahmen Ihres Abonnements sind aufgelistet werden. Wenn Sie das Verbinden mit einem virtuellen Netzwerkgateway, der nicht im Rahmen Ihres Abonnements möchten, verwenden Sie den [PowerShell-Artikel](vpn-gateway-vnet-vnet-rm-ps.md). 

8. Klicken Sie auf die virtuelle Netzwerkgateway, dem Sie in eine Verbindung herstellen möchten.
 
9. Geben Sie im Feld **freigegebene Schlüssel** einen freigegebenen Schlüssel für die Verbindung aus. Sie können generieren oder Schlüssel zu erstellen. In einer Verbindung zwischen Standorten würde der Schlüssel, die, den Sie verwenden, sein genau für Ihr Gerät lokalen und Ihre virtuelle Gateway Netzwerkverbindung identisch. Das Konzept entspricht, es sei denn, statt eine Verbindung mit einem Gerät VPN, und Sie die Verbindung herstellen, ein anderes virtuelles Netzwerkgateway.

    ![Freigegebene Schlüssel] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/sharedkey.png "Freigegebene Schlüssel")

10. Klicken Sie auf **OK** am unteren Rand der Blade um die Änderungen zu speichern.

## <a name="a-nametestvnet4connectiona8-configure-the-testvnet4-connection"></a><a name="TestVNet4Connection"></a>8 konfigurieren Sie 8 die Verbindung TestVNet4

Erstellen Sie anschließend eine Verbindung von TestVNet4 zu TestVNet1. Verwenden Sie die gleiche Methode, mit dem Sie die Verbindung von TestVNet1 zur TestVNet4 zu erstellen. Stellen Sie sicher, dass Sie den gleichen gemeinsamen Schlüssel verwenden.

## <a name="a-nameverifyconnectiona9-verify-your-connection"></a><a name="VerifyConnection"></a>9. Überprüfen Sie die Verbindung

Überprüfen Sie die Verbindung aus. Führen Sie für jede virtuelle Netzwerk-Gateway folgende Schritte aus:

1. Suchen Sie das Blade für das Netzwerkgateway virtuelle aus. Beispielsweise **TestVNet4GW**. 
2. Klicken Sie auf das virtuelle Netzwerk Gateway Blade klicken Sie auf **Verbindungen** , um das Blade Verbindungen für das Gateway virtuelles Netzwerk anzuzeigen.

Anzeigen der Verbindungen, und überprüfen Sie den Status. Nachdem die Verbindung erstellt wurde, werden Sie **erfolgreich** und **verbunden** als die Statuswerte angezeigt.

![Wurde erfolgreich abgeschlossen] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/connected.png "Wurde erfolgreich abgeschlossen")

Sie können jede Verbindung getrennt, um weitere Informationen über die Verbindung anzeigen doppelklicken.

![Grundlagen] (./media/vpn-gateway-howto-vnet-vnet-resource-manager-portal/essentials.png "Grundlagen")

## <a name="a-namefaqavnet-to-vnet-faq"></a><a name="faq"></a>VNet-VNet – häufig gestellte Fragen

Häufig gestellte Fragen zu Details für Weitere Informationen zu VNet-VNet-Verbindungen anzeigen.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]

## <a name="next-steps"></a>Nächste Schritte

Nachdem die Verbindung abgeschlossen ist, können Sie Ihre virtuelle Netzwerke virtuellen Computern hinzufügen. Schritte finden Sie unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
