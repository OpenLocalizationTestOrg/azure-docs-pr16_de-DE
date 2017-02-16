<properties 
   pageTitle="Konfigurieren einer Punkt-zu-Standort VPN Gateway-Verbindung zu virtuelles Netzwerk mit dem Modell zur Bereitstellung von Ressourcenmanager und Azure-Portal | Microsoft Azure"
   description="Sicheres Herstellen einer Verbindung mit Ihrem Azure-virtuellen Netzwerk durch Erstellen einer Punkt-zu-Standort VPN-Gateway-Verbindungs Ressourcenmanager und Azure-Portal."
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
   ms.date="10/17/2016"
   ms.author="cherylmc" />

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Konfigurieren einer Punkt-zu-Standort-Verbindung zu einer VNet über das Azure-Portal

> [AZURE.SELECTOR]
- [Ressourcenmanager - Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Ressourcenmanager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klassische - Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Eine Punkt-zu-Standort (P2S)-Konfiguration können Sie eine sichere Verbindung aus einem einzelnen Clientcomputer zu einem virtuellen Netzwerk zu erstellen. Eine Verbindung P2S ist nützlich, wenn Sie zu Ihrer VNet von einem entfernten Standort, wie Start oder einer Konferenz, eine Verbindung herstellen möchten, oder wenn Sie nur wenige Clients haben, die Verbindung zu einem virtuellen Netzwerk benötigen. 

Ein VPN-Gerät oder eine öffentlich zugängliche IP-Adresse entwickelt erforderlich Punkt-zu-Standort-Verbindungen keine. Eine VPN-Verbindung wird hergestellt, indem Sie die Verbindung vom Clientcomputer starten. Weitere Informationen zu Punkt-zu-Standort Verbindungen finden Sie unter [VPN Gateway häufig gestellte Fragen](vpn-gateway-vpn-faq.md#point-to-site-connections) und der [Planung und Entwurf](vpn-gateway-plan-design.md).

In diesem Artikel führt Sie durch Erstellen einer VNet mit einer Punkt-zu-Standort-Verbindung im Bereitstellungsmodell Ressourcenmanager über das Azure-Portal.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Bereitstellungsmodelle und Methoden für P2S Verbindungen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

In der folgenden Tabelle werden die zwei Bereitstellungsmodelle und Methoden für die Bereitstellung verfügbar für P2S Konfigurationen. Wenn ein Artikel mit Konfigurationsschritte verfügbar ist, verknüpfen wir aus dieser Tabelle direkt an.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Grundlegende workflow 

![Punkt-zu-Website-Diagramm] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "Punkt-zu-Standort")

### <a name="a-nameexampleaexample-values"></a><a name="example"></a>Beispiel für Werte

- **Name: VNet1**
- **Leerzeichen zu beheben: 192.168.0.0/16**<br>In diesem Beispiel verwenden wir nur eine Adresse Leerzeichen ein. Sie können mehr als eine Adresse Speicherplatz für Ihr VNet verfügen.
- **Subnetnamen: Front-End**
- **Subnetzadressbereichs: 192.168.1.0/24**
- **Abonnement:** Wenn Sie mehr als ein Abonnement besitzen, stellen Sie sicher, dass Sie den richtigen Arbeitsbereich verwenden.
- **Ressourcengruppe: TestRG**
- **Standort: Ostasiatischen US**
- **GatewaySubnet: 192.168.200.0/24**
- **Virtuelle Netzwerk gatewayname: VNet1GW**
- **Gateway-Typ: VPN**
- **VPN-Typ: Routing-basierten**
- **Öffentliche IP-Adresse: VNet1GWpip**
- **Verbindungstyp: Punkt-zu-Standort**
- **Client Adresse Ressourcenpool: 172.16.201.0/24**<br>VPN-Clients, die Verbindung mit der VNet mithilfe dieser Punkt-zu-Standort-Verbindung erhalten eine IP-Adresse aus dem Pool Client-Adressen ein.

## <a name="before-beginning"></a>Bevor Sie beginnen

- Stellen Sie sicher, dass Sie ein Azure-Abonnement verfügen. Wenn Sie bereits über ein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Vorteile für Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder melden Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)von aktivieren.
    
## <a name="a-namecreatevnetapart-1---create-a-virtual-network"></a><a name="createvnet"></a>Teil 1 – erstellen Sie ein virtuelles Netzwerk

Wenn Sie diese Konfiguration als Übung erstellen, können Sie auf die [Beispielwerte](#example)verweisen.

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

### <a name="2-add-additional-address-space-and-subnets"></a>2. Fügen Sie zusätzliche Adressbereichs und Subnetze hinzu

Sie können weitere Adressbereichs und Subnetze zu Ihrem VNet hinzufügen, nachdem er erstellt wurde.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

### <a name="3-create-a-gateway-subnet"></a>3 Erstellen Sie 3 ein Gateway Subnetz

Vor dem Verbinden Ihrer virtuellen Netzwerks zu einem Gateway, müssen Sie zuerst das Gateway Subnetz für das virtuelle Netzwerk erstellen, dem Sie eine Verbindung herstellen möchten. Falls möglich, empfiehlt es sich, erstellen Sie ein Gateway Subnetz einen Textblock CIDR /28 oder /27 verwenden, um genügend IP-Adressen, um zusätzliche zukünftigen Konfiguration Bedürfnisse bereitstellen zu können.

Die Screenshots in diesem Abschnitt werden als ein Bezug Beispiel bereitgestellt. Achten Sie darauf, dass die Adresse GatewaySubnet Bereichs, die entspricht mit den erforderlichen Werten für Ihre Konfiguration verwenden.

#### <a name="to-create-a-gateway-subnet"></a>So erstellen Sie ein Gateway Subnetz

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="a-namednsa4-specify-a-dns-server-optional"></a><a name="dns"></a>4 Geben Sie 4 die DNS-Server (optional)

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="a-namecreategwapart-2---create-a-virtual-network-gateway"></a><a name="creategw"></a>Teil 2 – Erstellen eines Gateways virtuelles Netzwerk

Punkt-zu-Standort Verbindungen erfordern die folgenden Einstellungen:

- Gateway-Typ: VPN
- VPN-Typ: Routing-basierten

### <a name="to-create-a-virtual-network-gateway"></a>Erstellen eines Gateways virtuelles Netzwerk

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="a-namegeneratecertapart-3---generate-certificates"></a><a name="generatecert"></a>Teil 3: generieren Zertifikate

Zertifikate werden von Azure verwendet, um VPN-Clients für Punkt-zu-Standort VPN authentifizieren. Sie exportieren als Base-64 CER-Datei aus entweder ein Zertifikat einer Stammzertifizierungsstelle durch eine Enterprise-Lösung Zertifikat generiert oder ein selbst signiertes Root Zertifikat x. 509-Zertifikat für Öffentliche Daten (nicht der private Schlüssel). Sie dann importieren die Zertifikat für Öffentliche Daten aus dem Stammordner Zertifikat in Azure. Darüber hinaus müssen Sie ein Client-Zertifikat aus dem Stammordner Zertifikat für Clients generieren. Jeder Client, der mit dem virtuellen Netzwerk über eine Verbindung P2S eine Verbindung herstellen möchte ein Clientzertifikat muss installiert sein, die aus dem Stammordner Zertifikat generiert wurde.

### <a name="a-namegetcera1-obtain-the-cer-file-for-the-root-certificate"></a><a name="getcer"></a>1. cer-Datei für das Zertifikat Quadratwurzel zu erhalten

Wenn Sie eine Lösung Enterprise verwenden, können Sie Ihre vorhandene Kette des Zertifikats. Wenn Sie ein Unternehmen Zertifizierungsstelle Lösung verwenden, können Sie ein selbst signiertes Stamm Zertifikat erstellen. Eine Methode zum Erstellen eines selbstsignierten Zertifikats ist Makecert.

- Wenn Sie ein Zertifikat Enterprise-System verwenden, beziehen Sie die CER-Datei für das Stamm-Zertifikat, das Sie verwenden möchten. 

- Wenn Sie eine Zertifikat-Lösung Enterprise nicht verwenden, müssen Sie ein Zertifikat selbstsignierten Stamm zu generieren. Für Windows 10 Schritten können Sie für die [Arbeit mit Quadratwurzel von selbstsignierten Zertifikaten für Punkt-zu-Standort Konfigurationen](vpn-gateway-certificates-point-to-site.md)verweisen.

1. Um ein Zertifikat eine CER-Datei erhalten öffnen Sie **certmgr.msc** , und suchen Sie das Zertifikat aus. Mit der rechten Maustaste in des Stamm-selbst signiertes Zertifikats, klicken Sie auf **Alle Aufgaben**, und klicken Sie dann auf **Exportieren**. Daraufhin wird ein **Zertifikat Export-Assistenten**.

2. Im Assistenten klicken Sie auf **Weiter**, wählen Sie **Nein, privaten Schlüssel nicht exportieren**aus, und klicken Sie dann auf **Weiter**.

3. Klicken Sie auf der Seite **Format der zu exportierenden Datei** wählen **Base-64-codierte x. 509 (. CER).** Klicken Sie dann auf **Weiter**. 

4. Klicken Sie auf die **Datei zu exportieren**, **Navigieren** Sie zu dem Speicherort, den Sie das Zertifikat exportieren möchten. Dateinamen Sie für den **Dateinamen ein**den Zertifikat aus. Klicken Sie dann auf **Weiter**.

5. Klicken Sie auf **Fertig stellen** , um das Zertifikat zu exportieren.

### <a name="a-namegenerateclientcerta2-generate-a-client-certificate"></a><a name="generateclientcert"></a>2. erstellen Sie ein Clientzertifikat

Sie können entweder ein eindeutige Zertifikat generieren, für jeden Client, der eine Verbindung herstellt, oder Sie können das gleiche Zertifikat auf mehreren Clients. Der Vorteil bei Generieren von eindeutigen Client-Zertifikate ist die Möglichkeit, ein einzelnes Zertifikat widerrufen, falls erforderlich. Andernfalls, wenn alle ist das gleiche Clientzertifikat verwenden, und Sie feststellen, dass Sie das Zertifikat für einen Client Sperren müssen, müssen Sie zum Generieren und installieren neue Zertifikate für alle Clients, die das Zertifikat zum Authentifizieren verwenden.

- Wenn Sie eine Zertifikat-Lösung Enterprise verwenden, erstellen Sie ein Clientzertifikat mit allgemeine Namensformat Wert 'name@yourdomain.com', und nicht das Format 'Domänenname\Benutzername'. 

- Wenn Sie ein selbst signiertes Zertifikat verwenden, finden Sie unter [Arbeiten mit selbstsignierten Stammzertifikaten für Punkt-zu-Standort-Konfigurationen](vpn-gateway-certificates-point-to-site.md) , um ein Client-Zertifikat generieren.

### <a name="a-nameexportclientcerta3-export-the-client-certificate"></a><a name="exportclientcert"></a>3 das Client-Zertifikat exportieren

Ein Client-Zertifikat ist zur Authentifizierung erforderlich. Exportieren Sie nach dem Generieren des Clientzertifikats, es aus. Das Clientzertifikat, das Sie exportieren wird später auf jedem Clientcomputer installiert.

1. Wenn Sie ein Client-Zertifikat exportieren möchten, können Sie *certmgr.msc*verwenden. Mit der rechten Maustaste in des Client-Zertifikats, das Sie exportieren, klicken Sie auf **Alle Aufgaben**, und klicken Sie dann auf **Exportieren**möchten.
2. Exportieren Sie das Client-Zertifikat mit dem privaten Schlüssel. Dies ist eine *PFX* -Datei. Vergewissern Sie sich zum Aufzeichnen oder vergessen Sie nicht das Kennwort (Schlüssel), das Sie für dieses Zertifikat festlegen.

## <a name="a-nameaddresspoolapart-4---add-the-client-address-pool"></a><a name="addresspool"></a>Teil 4: Fügen Sie dem Pool Client-Adressen hinzu.

1. Nachdem das Gateway virtuelles Netzwerk erstellt wurde, navigieren Sie zum Abschnitt **Einstellungen** des virtuellen Netzwerks Gateway Blades. Klicken Sie im Abschnitt **Einstellungen** auf **Punkt-zu-Standort-Konfiguration** um das Blade **Konfiguration** zu öffnen.

    ![zeigen Sie auf der Website blade] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png "zeigen Sie auf der Website blade")

2. **Adresse Ressourcenpool** wird dem Pool von IP-Adressen, von denen Clients, die eine Verbindung herstellen eine IP-Adresse empfangen werden. Fügen Sie dem Adresspool hinzu, und klicken Sie dann auf **Speichern**.

    ![Client Adresse Ressourcenpool] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/addresspool.png "Client Adresse Ressourcenpool")

## <a name="a-nameuploadfileapart-5---upload-the-root-certificate-cer-file"></a><a name="uploadfile"></a>Teil 5 - Upload Root Zertifikat CER-Datei

Nachdem das Gateway erstellt wurde, können Sie die CER-Datei für ein vertrauenswürdiges Zertifikat in Azure hochladen. Sie können Dateien für bis zu 20 Stammzertifikate hochladen. Sie nicht den privaten Schlüssel für das Zertifikat der Stamm in Azure hochladen. Sobald die CER-Datei hochgeladen wird, von Azure-Clients authentifizieren, die mit dem virtuellen Netzwerk verwendet.

1. Navigieren Sie zu der **Punkt-zu-Standort-Konfiguration** Blade. Sie werden im Abschnitt **Root Zertifikat** von diesem Blade CER-Dateien hinzufügen.

    ![zeigen Sie auf der Website blade] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcert.png "zeigen Sie auf der Website blade")

2. Stellen Sie sicher, dass Sie das Zertifikat der Stamm exportiert als Base-64 x. 509 (.) Datei. Sie müssen sie in diesem Format exportiert werden, dass das Zertifikat mit Text-Editor geöffnet werden kann.
3. Öffnen Sie das Zertifikat mit einem Texteditor wie Editor. Kopieren Sie nur den folgenden Abschnitt ein:

    ![Zertifikatsdaten] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png "Zertifikatsdaten")

4. Fügen Sie die Zertifikatsdaten in den **Öffentlichen Zertifikatsdaten** Abschnitt des Portals aus. Setzen Sie den Namen des Zertifikats in den Abstand **ein** , und klicken Sie dann auf **Speichern**. Sie können bis zu 20 vertrauenswürdiges Zertifikate hinzufügen.

    ![Zertifikat hochladen] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/uploadcert.png "Zertifikat hochladen")

## <a name="a-nameclientconfigapart-6---download-and-install-the-vpn-client-configuration-package"></a><a name="clientconfig"></a>Teil 6: herunterladen und installieren Sie das VPN-Client-Konfigurations-Paket

Herstellen einer Verbindung mit Azure mit P2S Clients müssen ein Client-Zertifikat, und einem VPN-Client-Konfiguration-Paket installiert. VPN-Client-Konfigurationspakete stehen für Windows-Clients. 

Das VPN-Client-Paket enthält Informationen, um die Option VPN-Clientsoftware konfigurieren, die in Windows integriert ist. Die Konfiguration gilt nur für die Option VPN, die Sie in eine Verbindung herstellen möchten. Das Paket keine zusätzlichen Software installieren. Finden Sie die [Option VPN Gateway häufig gestellte Fragen zu](vpn-gateway-vpn-faq.md#point-to-site-connections) für Weitere Informationen.

1. Klicken Sie auf der **Punkt-zu-Standort-Konfiguration** Blade, klicken Sie auf **herunterladen VPN-Client** , um das **Herunterladen von VPN-Client** -Blade zu öffnen.

    ![Herunterladen von VPN-client] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadclient.png "Herunterladen von VPN-client")

2. Wählen Sie das richtige Paket für Ihren Kunden ein, und klicken Sie auf **herunterladen**. Wählen Sie für 64-Bit-Clients **AMD64**aus. Wählen Sie für 32-Bit-Clients **X86**ein.

3. Installieren Sie das Paket auf dem Clientcomputer. Wenn Sie ein Popup SmartScreen erhalten möchten, klicken Sie auf **Weitere Informationen anzuzeigen**, klicken Sie dann **trotzdem ausführen** , um das Paket zu installieren.

4. Navigieren Sie zu **Deren Einstellungen im Netzwerk** , und klicken Sie auf **VPN**, auf dem Clientcomputer. Sie sehen die Verbindung aufgeführt. Es zeigt den Namen des virtuellen Netzwerk, dem eine Verbindung herstellt und Looks ähnlich wie in diesem Beispiel: 

    ![VPN-client] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png "VPN-client")

## <a name="a-nameinstallclientcertapart-7---install-the-client-certificate"></a><a name="installclientcert"></a>Teil 7 - Clientzertifikat installieren

Jeder Clientcomputer müssen ein Client-Zertifikat für die Authentifizierung. Wenn Sie das Client-Zertifikat installieren zu können, benötigen Sie das Kennwort, das erstellt wurde, wenn das Client-Zertifikat exportiert wurde.

1. Kopieren Sie die PFX-Datei auf dem Clientcomputer.
2. Doppelklicken Sie auf die PFX-Datei, um ihn zu installieren. Ändern Sie den Installationsspeicherort nicht.

## <a name="a-nameconnectapart-8---connect-to-azure"></a><a name="connect"></a>Teil 8: Herstellen einer Verbindung Azure mit

1. Navigieren Sie zu VPN-Verbindungen, und suchen Sie die VPN-Verbindung, die Sie erstellt haben, zum Verbinden mit Ihrer VNet, auf dem Clientcomputer. Es wird als denselben Namen wie virtuelle Netzwerk bezeichnet. Klicken Sie auf **Verbinden**. Möglicherweise eine Popup-Meldung angezeigt, die bei der Verwendung von des Zertifikats verweist. Wenn dies der Fall ist, klicken Sie auf **Weiter** , um erhöhten verwenden. 

2. Klicken Sie auf der Seite **Verbindung** Status auf **Verbinden** , um die Verbindung zu starten. Wenn Sie einen **Zertifikat auswählen** -Bildschirm angezeigt wird, stellen Sie sicher, dass das Client-Zertifikat, das angezeigt, die Sie verwenden wird, um eine Verbindung herstellen möchten. Wenn es nicht der Fall ist, verwenden Sie den Dropdown-Pfeil, um das richtige Zertifikat auszuwählen, und klicken Sie dann auf **OK**.

    ![VPN-Client 2] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN-Client-Verbindung")

3. Nun sollte die Verbindung hergestellt werden.

    ![VPN-Client 3] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "VPN-Client-Verbindung 2")

## <a name="a-nameverifyapart-9---verify-your-connection"></a><a name="verify"></a>Teil 9: Überprüfen Sie die Verbindung

1. Stellen Sie sicher, dass die Option VPN-Verbindung aktiv ist, wenn öffnen Sie ein erweitertes Eingabeaufforderungsfenster, und führen Sie *Ipconfig/All*.

2. Anzeigen der Ergebnisse an. Beachten Sie, dass die IP-Adresse, die Sie erhalten eine der Adressen in dem Pool Punkt-zu-Standort VPN Client Adressen, die Sie in der Konfiguration angegeben haben. Die Ergebnisse sollten etwa so:
    
        PPP adapter VNet1:
            Connection-specific DNS Suffix .:
            Description.....................: VNet1
            Physical Address................:
            DHCP Enabled....................: No
            Autoconfiguration Enabled.......: Yes
            IPv4 Address....................: 172.16.201.3(Preferred)
            Subnet Mask.....................: 255.255.255.255
            Default Gateway.................:
            NetBIOS over Tcpip..............: Enabled

## <a name="a-nameaddato-add-or-remove-trusted-root-certificates"></a><a name="add"></a>Hinzufügen oder Entfernen von Zertifikate der vertrauenswürdigen Stammzertifizierungsstelle

Sie können aus Azure vertrauenswürdiges Zertifikat entfernen. Wenn Sie ein vertrauenswürdiges Zertifikat entfernen, werden die Clientzertifikate, die aus dem Stammordner Zertifikat generiert wurden nicht mehr in Azure über Punkt-zu-Standort eine Verbindung herstellen können. Wenn Sie Clients eine Verbindung herstellen möchten, müssen sie ein neues Clientzertifikat zu installieren, das aus einem Zertifikat generiert wird, der in Azure vertraut ist.

Sie können die Liste der gesperrten Client-Zertifikate auf der **Punkt-zu-Standort-Konfiguration verwalten** Blade. Dies ist das Blade, das Sie hochladen [ein vertrauenswürdiges Zertifikat](#uploadfile)verwendet.

## <a name="a-namerevokeclientato-manage-the-list-of-revoked-client-certificates"></a><a name="revokeclient"></a>Zum Verwalten der Liste der gesperrten Client-Zertifikate

Sie können Client-Zertifikate widerrufen. Der Sperrliste können Sie basierend auf den einzelnen Client-Zertifikate Punkt-zu-Standort-Konnektivität Selektives verweigern. Wenn Sie eine Root Zertifikat CER aus Azure entfernen, widerrufen wird den Zugriff für alle Client-Zertifikate generiert/angemeldet durch das gesperrte Root Zertifikat. Wenn Sie ein bestimmtes Client-Zertifikat nicht im Stammverzeichnis, widerrufen möchten können Sie dies tun. Auf diese Weise, die die anderen Zertifikate, die aus dem Stammordner Zertifikat generiert wurden weiterhin werden sein gültig. 

Meist besteht darin, das Zertifikat der Stamm zum Verwalten des Zugriffs auf Team oder Organisationsebene, während der Verwendung von gesperrten Client-Zertifikate für abgestimmte Access-Steuerelement auf einzelne Benutzer zu verwenden.

Sie können die Liste der gesperrten Client-Zertifikate auf der **Punkt-zu-Standort-Konfiguration verwalten** Blade. Dies ist das Blade, das Sie hochladen [ein vertrauenswürdiges Zertifikat](#uploadfile)verwendet.


## <a name="next-steps"></a>Nächste Schritte

Sie können einen virtuellen Computer mit Ihrem Netzwerk virtuelle hinzufügen. Schritte finden Sie unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .