<properties
   pageTitle="Erstellen eines virtuelles Netzwerks mit einer Website-zu-Standort VPN-Gateway-Verbindung über das klassische Azure-Portal | Microsoft Azure"
   description="Erstellen einer VNet mit einer S2S VPN-Gateway-Verbindung für Cross lokale und das Bereitstellungsmodell klassischen verwenden möchten."
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-classic-portal"></a>Erstellen einer VNet mit einer Website-zu-Standort-Verbindung über das klassische Azure-portal

> [AZURE.SELECTOR]
- [Ressourcenmanager - Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Ressourcenmanager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klassische - klassischen-Portal](vpn-gateway-site-to-site-create.md)

In diesem Artikel führt Sie durch die Erstellung eines virtuellen Netzwerks und eine Website-zu-Standort VPN-Gateway-Verbindung mit Ihrem lokalen Netzwerk mit dem klassischen Bereitstellungsmodell und das klassische Portal aus. Website-zu-Standort-Verbindungen für Cross lokale und Hybrid verwendet werden können Konfigurationen.

![Website-zu-Standort-Diagramm] (./media/vpn-gateway-site-to-site-create/site2site.png "Website-zu-Standort")


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Bereitstellungsmodelle und Methoden für die Verbindungen zwischen Standorten

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

In der folgenden Tabelle zeigt die aktuell verfügbare Bereitstellung-Modelle und Methoden für die Website-zu-Standort-Konfigurationen. Wenn ein Artikel mit Konfigurationsschritte verfügbar ist, verknüpfen wir aus dieser Tabelle direkt an.

[AZURE.INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Zusätzliche Konfigurationen 

Wenn Sie VNets miteinander zu verbinden möchten, finden Sie unter [Konfigurieren einer VNet-VNet - Verbindung für das Bereitstellungsmodell klassischen](virtual-networks-configure-vnet-to-vnet-connection.md). Wenn Sie eine Verbindung zwischen Standorten eine VNet hinzufügen, die bereits eine Verbindung möchten, finden Sie unter [Hinzufügen einer S2S-Verbindung zu einem VNet mit einer vorhandenen VPN-Gateway-Verbindung](vpn-gateway-multi-site.md).
 
## <a name="before-you-begin"></a>Vorbemerkung

Stellen Sie sicher, dass Sie über Folgendes verfügen, bevor Sie beginnen Konfiguration.

- Ein kompatibles VPN-Gerät und eine Person, die sie konfigurieren kann. Finden Sie unter [VPN-Geräte](vpn-gateway-about-vpn-devices.md). Wenn Sie nicht mit Ihrem Gerät VPN konfigurieren vertraut sind, oder klicken Sie mit der IP-Adresse, die Bereiche in ansässig vertraut sind, Ihre lokal Netzwerkkonfiguration, müssen Sie mit einer anderen Person zu koordinieren, die für Sie diese Details abgerufen werden können.

- Eine extern ausgerichteten öffentlichen IP-Adresse für Ihr Gerät VPN. Diese IP-Adresse kann sich nicht hinter einem NAT befinden

- Ein Azure-Abonnement. Wenn Sie bereits über ein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Vorteile für Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder melden Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)von aktivieren.


## <a name="a-namecreatevnetacreate-your-virtual-network"></a><a name="CreateVNet"></a>Erstellen von virtuellen Netzwerks

1. Melden Sie sich im [Azure klassischen Portal](https://manage.windowsazure.com/).

2. Klicken Sie in der unteren linken Ecke des Bildschirms auf **neu**. Klicken Sie im Navigationsbereich klicken Sie auf **Netzwerkdienste**, und klicken Sie dann auf **Virtuelles Netzwerk**. Klicken Sie auf **Benutzerdefinierte erstellen** , um den Konfigurations-Assistenten zu starten.

3. Um Ihre VNet zu erstellen, geben Sie Ihre Konfiguration Einstellungen auf den folgenden Seiten aus:

## <a name="a-namedetailsavirtual-network-details-page"></a><a name="Details"></a>Detailseite virtuelles Netzwerk

Geben Sie die folgenden Informationen ein:

- **Name**: Namen für das virtuelle Netzwerk. Beispielsweise *EastUSVNet*. Dieser virtuelle Netzwerkname wird verwendet werden, wenn Sie Ihre Instanzen virtuellen Computern und PaaS bereitstellen, damit Sie nicht den Namen zu kompliziert machen möchten.
- **Standort**: die Position direkt bezieht sich auf den Standort (Bereich), werden Ihre Ressourcen (virtuelle Computer) befinden soll. Wählen Sie diesem Speicherort angenommen, Sie gegebenenfalls die virtuellen Computern, die Sie für diese virtuelle Netzwerk physisch im *Südostasiatischen US*befinden bereitstellen. Sie können nicht ändern die Region Ihres Netzwerks virtuelle zugeordnet, nachdem Sie es erstellt haben.

## <a name="a-namednsadns-servers-and-vpn-connectivity-page"></a><a name="DNS"></a>DNS-Server und VPN-Konnektivität Seite

Geben Sie die folgenden Informationen ein, und klicken Sie dann auf den nächsten Pfeil in der unteren rechten Ecke.

- **DNS-Server**: Geben Sie den DNS-Servernamen und die IP-Adresse ein, oder wählen Sie einen zuvor registrierten DNS-Server aus dem Kontextmenü. Diese Einstellung werden keine DNS-Server erstellt. Sie können Sie die DNS-Server angeben, die Sie für die Auflösung von Namen für diese virtuelle Netzwerk verwenden möchten.
- **Konfigurieren von Website-zu-Standort VPN**: Aktivieren Sie das Kontrollkästchen für das **Konfigurieren eines Standort-zu-Standort VPN**.
- **Lokales Netzwerk**: ein lokales Netzwerk darstellt, Ihre physischen lokalen Speicherort. Sie können auswählen, ein lokales Netzwerk, das Sie zuvor erstellt haben, oder Sie können ein neues lokales Netzwerk erstellen. Wenn Sie wählen Sie aus ein lokales Netzwerk verwendet, das Sie zuvor erstellt haben, wechseln Sie zur Konfigurationsseite **Lokalen Netzwerken** und stellen Sie sicher, dass die VPN-Gerät IP-Adresse (öffentlich zugänglichen IPv4-Adresse) für das VPN-Gerät richtig ist.

## <a name="a-nameconnectivityasite-to-site-connectivity-page"></a><a name="Connectivity"></a>Konnektivität zwischen Standorten-Seite

Wenn Sie ein neues lokales Netzwerk erstellen, sehen Sie die Seite **Standorten Konnektivität** . Wenn Sie möchten ein lokales Netzwerk verwenden, das Sie zuvor erstellt haben, diese Seite des Assistenten nicht angezeigt wird und Sie können mit dem nächsten Abschnitt auf verschieben.

Geben Sie die folgenden Informationen ein, und klicken Sie dann auf den Pfeil nach rechts.

-   **Name**: Netzwerk-Website für der Namen, die Sie Ihren lokalen (lokal) anrufen möchten.
-   **VPN-Gerät IP-Adresse**: die öffentlich zugänglichen IPv4-Adresse von Ihrem lokalen VPN-Gerät, die Sie verwenden, um die Verbindung mit Azure. Das Gerät VPN kann sich nicht hinter einem NAT befinden
-   **Adressbereichs**: Starten IP-Adresse und CIDR (Anzahl der Adressen) enthalten. Sie geben die Adresse range(s), die Sie über das Gateway virtuelles Netzwerk auf Ihrem lokalen lokalen Speicherort gesendet werden sollen. Liegt eine IP-Zieladresse innerhalb der Bereiche, die Sie hier angeben, wird es über das Gateway virtuelles Netzwerk weitergeleitet.
-   **Hinzufügen von Adressbereichs**: Wenn Sie mehrere Adressbereiche, die über das Gateway virtuelles Netzwerk gesendet werden soll verfügen, geben Sie jede zusätzliche Adressbereich. Sie können hinzufügen oder Entfernen von Bereichen finden Sie auf der Seite **Lokale Netzwerk** .

## <a name="a-nameaddressavirtual-network-address-spaces-page"></a><a name="Address"></a>Virtuelle Netzwerk Adresse Leerzeichen Seite

Geben Sie im Adressbereich, den Sie für Ihre virtuelle Netzwerk verwenden möchten. Hierbei handelt es sich um die dynamischen IP-Adressen (DIPS), die den virtuellen Computern und andere Rolleninstanzen, die Sie in diesem virtuellen Netzwerk bereitstellen zugewiesen werden.

Es ist besonders wichtig, wählen Sie einen Bereich, der nicht überlappt mit keine Bereiche, die für das lokale Netzwerk verwendet werden. Sie müssen Ihr Netzwerkadministrator abgestimmt. Machen Sie einen Bereich von IP-Adressen aus Ihrem lokalen Netzwerk Adresse Space für Ihre Verwendung für Ihre virtuelle Netzwerk möglicherweise Ihr Netzwerkadministrator müssen.

Geben Sie die folgenden Informationen ein, und klicken Sie dann auf das Häkchen unten rechts in Ihrem Netzwerk konfigurieren.

- **Adressbereichs**: Starten IP-Adresse und die Anzahl der Adressen enthalten. Stellen Sie sicher, dass die Adresse Leerzeichen, die Sie angeben sich die Adressräume überschneiden nicht, die Sie in Ihrem lokalen Netzwerk besitzen.
- **Hinzufügen von Subnetz**: erste IP-einschließen und Adresse zählen. Weitere Subnetze sind nicht erforderlich, aber Sie möchten ein separates Subnetz für virtuelle Computer zu erstellen, die statischen DIPS. Oder möglicherweise möchten Sie Ihre virtuelle Computer in einem Subnetz haben, die von der anderen Rolleninstanzen getrennt ist.
- **Hinzufügen Gateway Subnetz**: Klicken Sie auf das Gateway Subnetz hinzufügen. Das Gateway Subnetz wird nur für das Gateway virtuelles Netzwerk verwendet und für diese Konfiguration erforderlich ist.

Klicken Sie auf das Häkchen klicken Sie auf den unteren Rand der Seite und virtuellen Netzwerks beginnt mit dem erstellen. Wenn es abgeschlossen ist, sehen Sie sich, dass **erstellt** auf der Seite **Netzwerke** im klassischen Azure-Portal unter **Status** aufgelistet. Nachdem der VNet erstellt wurde, können Sie Ihr virtuelles Netzwerk-Gateway konfigurieren.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="a-namevnetgatewayaconfigure-your-virtual-network-gateway"></a><a name="VNetGateway"></a>Konfigurieren des virtuelle Netzwerk-Gateways

Konfigurieren des Netzwerkgateways virtuelle, um eine sichere Website-zu-Standort-Verbindung zu erstellen. Finden Sie unter [Konfigurieren eines Gateways virtuelles Netzwerk im klassischen Azure-Portal](vpn-gateway-configure-vpn-gateway-mp.md).

## <a name="next-steps"></a>Nächste Schritte

Nachdem die Verbindung abgeschlossen ist, können Sie Ihre virtuelle Netzwerke virtuellen Computern hinzufügen. Finden Sie in der Dokumentation [virtuellen Computern](https://azure.microsoft.com/documentation/services/virtual-machines/) für Weitere Informationen.
