<properties 
   pageTitle="Konfigurieren eine Punkt-zu-Standort VPN-Gateway-Verbindung zu virtuellen verwendet das Modell zur Bereitstellung von Ressourcenmanager | Microsoft Azure"
   description="Sicheres Herstellen einer Verbindung mit Ihrem Azure-virtuellen Netzwerk durch Erstellen einer Punkt-zu-Standort VPN-Gateway-Verbindungs."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>Konfigurieren einer Punkt-zu-Standort-Verbindung zu einem VNet mithilfe der PowerShell

> [AZURE.SELECTOR]
- [Ressourcenmanager - Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Ressourcenmanager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klassische - Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Eine Punkt-zu-Standort (P2S)-Konfiguration können Sie eine sichere Verbindung aus einem einzelnen Clientcomputer zu einem virtuellen Netzwerk zu erstellen. Eine Verbindung P2S ist nützlich, wenn Sie zu Ihrer VNet von einem entfernten Standort, wie Start oder einer Konferenz, eine Verbindung herstellen möchten, oder wenn Sie nur wenige Clients haben, die Verbindung zu einem virtuellen Netzwerk benötigen. 

Ein VPN-Gerät oder eine öffentlich zugängliche IP-Adresse entwickelt erforderlich Punkt-zu-Standort-Verbindungen keine. Eine VPN-Verbindung wird hergestellt, indem Sie die Verbindung vom Clientcomputer starten. Weitere Informationen zu Punkt-zu-Standort Verbindungen finden Sie unter [VPN Gateway häufig gestellte Fragen](vpn-gateway-vpn-faq.md#point-to-site-connections) und der [Planung und Entwurf](vpn-gateway-plan-design.md). 

In diesem Artikel führt Sie durch die Erstellung einer VNet mit einer Punkt-zu-Standort-Verbindung in das Modell zur Bereitstellung von Ressourcenmanager mithilfe der PowerShell.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Bereitstellungsmodelle und Methoden für P2S Verbindungen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

In der folgenden Tabelle werden die zwei Bereitstellungsmodelle und Methoden für die Bereitstellung verfügbar für P2S Konfigurationen. Wenn ein Artikel mit Konfigurationsschritte verfügbar ist, verknüpfen wir aus dieser Tabelle direkt an.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Grundlegende workflow 

![Punkt-zu-Website-Diagramm] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "Punkt-zu-Standort")

In diesem Szenario erstellen Sie ein virtuelles Netzwerk mit einer Punkt-zu-Standort-Verbindung. Die Anweisungen auch hilft Ihnen Zertifikate, generieren, die für diese Konfiguration erforderlich sind. Eine P2S-Verbindung besteht aus den folgenden Elemente: A VNet mit einem VPN-Gateway, eine Root Zertifikat CER-Datei (öffentlicher Schlüssel), ein Client-Zertifikat und die VPN-Konfiguration auf dem Client. 

Wir verwenden die folgenden Werte für diese Konfiguration aus. Wir legen Sie die Variablen im Abschnitt [1](#declare) dieses Artikels. Sie können entweder gehen Sie vor, wie eine exemplarische Vorgehensweise und verwenden Sie die Werte, ohne sie zu ändern, oder ändern diese entsprechend Ihrer Umgebung. 

### <a name="a-nameexampleaexample-values"></a><a name="example"></a>Beispiel für Werte

- **Name: VNet1**
- **Speicherplatz zu beheben: 192.168.0.0/16** und **10.254.0.0/16**<br>In diesem Beispiel verwenden wir mehrere Adressbereichs um zu veranschaulichen, dass diese Konfiguration mit mehreren Adresse Leerzeichen eingesetzt werden kann. Mehreren Adresse Leerzeichen sind jedoch nicht für dieser Konfiguration erforderlich.
- **Subnetnamen: Front-End**
    - **Subnetzadressbereichs: 192.168.1.0/24**
- **Subnetnamen: Back-End-**
    - **Subnetzadressbereichs: 10.254.1.0/24**
- **Subnetnamen: GatewaySubnet**<br>Den Namen Subnetz *GatewaySubnet* ist für das Gateway VPN entwickelt obligatorisch.
    - **Subnetzadressbereichs: 192.168.200.0/24** 
- **VPN-Client Adresse Ressourcenpool: 172.16.201.0/24**<br>VPN-Clients, die Verbindung mit der VNet mithilfe dieser Punkt-zu-Standort-Verbindung erhalten eine IP-Adresse aus dem Pool VPN-Client.
- **Abonnement:** Wenn Sie mehr als ein Abonnement besitzen, stellen Sie sicher, dass Sie den richtigen Arbeitsbereich verwenden.
- **Ressourcengruppe: TestRG**
- **Standort: Ostasiatischen US**
- **DNS-Server: IP Address** des DNS-Servers ein, die für die namensauflösung verwendet werden soll.
- **GW Namen: Vnet1GW**
- **Öffentlicher IP-Name: VNet1GWPIP**
- **VpnType: RouteBased**

## <a name="before-beginning"></a>Bevor Sie beginnen

- Stellen Sie sicher, dass Sie ein Azure-Abonnement verfügen. Wenn Sie bereits über ein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Vorteile für Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder melden Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/)von aktivieren.
    
- Installieren Sie die neueste Version von Azure Ressourcenmanager PowerShell-Cmdlets. Weitere Informationen zum Installieren der PowerShell-Cmdlets finden Sie unter [Informationen zum Installieren und konfigurieren Azure PowerShell](../powershell-install-configure.md) . Bei der Arbeit mit PowerShell für diese Konfiguration stellen Sie sicher, dass Sie als Administrator ausführen. 

## <a name="a-namedeclareapart-1---log-in-and-set-variables"></a><a name="declare"></a>Teil 1 - Log in und festgelegten Variablen

In diesem Abschnitt anmelden und die Werte für diese Konfiguration verwendete deklarieren. Die deklarierte Werte werden in der Stichprobe Skripts verwendet. Ändern Sie die Werte entsprechend Ihrer eigenen Umgebung. Alternativ können Sie verwenden die deklarierte Werte und durchlaufen Sie die Schritte als Übung.

1. Melden Sie sich in der PowerShell-Konsole Ihrem Azure-Konto an. Dieses Cmdlet fordert Sie für die Anmeldeinformationen für Ihr Konto Azure. Nach der Anmeldung lädt diese Einstellungen für Ihr Konto, damit sie für Azure PowerShell verfügbar sind.

        Login-AzureRmAccount 

2. Abrufen einer Liste von Azure-Abonnements.

        Get-AzureRmSubscription

3. Geben Sie das Abonnement, das Sie verwenden möchten. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. Deklarieren Sie die Variablen, die verwendet werden soll. Verwenden Sie im folgende Beispiel wird die Werte für eigene bei Bedarf ersetzen.

        $VNetName  = "VNet1"
        $FESubName = "FrontEnd"
        $BESubName = "Backend"
        $GWSubName = "GatewaySubnet"
        $VNetPrefix1 = "192.168.0.0/16"
        $VNetPrefix2 = "10.254.0.0/16"
        $FESubPrefix = "192.168.1.0/24"
        $BESubPrefix = "10.254.1.0/24"
        $GWSubPrefix = "192.168.200.0/26"
        $VPNClientAddressPool = "172.16.201.0/24"
        $RG = "TestRG"
        $Location = "East US"
        $DNS = "8.8.8.8"
        $GWName = "VNet1GW"
        $GWIPName = "VNet1GWPIP"
        $GWIPconfName = "gwipconf"
        
## <a name="a-nameconfigurevnetapart-2---configure-a-vnet"></a><a name="ConfigureVNet"></a>Teil 2 – Konfigurieren eines VNet 

1. Erstellen Sie eine Ressourcengruppe aus.

        New-AzureRmResourceGroup -Name $RG -Location $Location

2. Erstellen Sie die Subnetz Konfigurationen für das virtuelle Netzwerk, und benennen sie *Front-End*, *Back-End-*und *GatewaySubnet*. Diese Präfixe muss Teil des Adressbereichs VNet, die Sie deklariert.

        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix

3. Erstellen Sie das virtuelle Netzwerk an. Der DNS-Server angegebenen sollten einen DNS-Server, der die Namen für die Ressourcen beheben können, die, denen Sie eine Verbindung herstellen. In diesem Beispiel wird eine öffentliche IP-Adresse verwendet. Achten Sie darauf, dass Sie Ihre eigenen Werte verwenden.
    
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS

4. Geben Sie die Variablen für das virtuelle Netzwerk aus, die, das Sie erstellt haben.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

5. Anfordern einer dynamisch zugewiesene öffentliche IP Address. Diese IP-Adresse ist erforderlich für das Gateway ordnungsgemäß funktioniert. Sie verbinden später des Gateways auf die Gateway IP-Konfiguration.
        
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

## <a name="a-namecertificatesapart-3---certificates"></a><a name="Certificates"></a>Teil 3 - Zertifikate

Zertifikate werden von Azure verwendet, um VPN-Clients für Punkt-zu-Standort VPN authentifizieren. Sie exportieren als Base-64 CER-Datei aus entweder ein Zertifikat einer Stammzertifizierungsstelle durch eine Enterprise-Lösung Zertifikat generiert oder ein selbst signiertes Root Zertifikat x. 509-Zertifikat für Öffentliche Daten (nicht der private Schlüssel). Sie dann importieren die Zertifikat für Öffentliche Daten aus dem Stammordner Zertifikat in Azure. Darüber hinaus müssen Sie ein Client-Zertifikat aus dem Stammordner Zertifikat für Clients generieren. Jeder Client, der mit dem virtuellen Netzwerk über eine Verbindung P2S eine Verbindung herstellen möchte ein Clientzertifikat muss installiert sein, die aus dem Stammordner Zertifikat generiert wurde.

### <a name="a-namecera1-obtain-the-cer-file-for-the-root-certificate"></a><a name="cer"></a>1. cer-Datei für das Zertifikat Quadratwurzel zu erhalten

Sie müssen die öffentliche Zertifikat-Daten für das Zertifikat der Stamm abzurufen, die Sie verwenden möchten.

- Wenn Sie ein Zertifikat Enterprise-System verwenden, beziehen Sie die CER-Datei für das Stamm-Zertifikat, das Sie verwenden möchten. 

- Wenn Sie eine Zertifikat-Lösung Enterprise nicht verwenden, müssen Sie ein Zertifikat selbstsignierten Stamm zu generieren. Für Windows 10 Schritten können Sie für die [Arbeit mit Quadratwurzel von selbstsignierten Zertifikaten für Punkt-zu-Standort Konfigurationen](vpn-gateway-certificates-point-to-site.md)verweisen.


1. Um ein Zertifikat eine CER-Datei erhalten öffnen Sie **certmgr.msc** , und suchen Sie das Zertifikat aus. Mit der rechten Maustaste in des Stamm-selbst signiertes Zertifikats, klicken Sie auf **Alle Aufgaben**, und klicken Sie dann auf **Exportieren**. Daraufhin wird ein **Zertifikat Export-Assistenten**.

2. Im Assistenten klicken Sie auf **Weiter**, wählen Sie **Nein, privaten Schlüssel nicht exportieren**aus, und klicken Sie dann auf **Weiter**.

3. Klicken Sie auf der Seite **Format der zu exportierenden Datei** wählen **Base-64-codierte x. 509 (. CER).** Klicken Sie dann auf **Weiter**. 

4. Klicken Sie auf die **Datei zu exportieren**, **Navigieren** Sie zu dem Speicherort, den Sie das Zertifikat exportieren möchten. Dateinamen Sie für den **Dateinamen ein**den Zertifikat aus. Klicken Sie dann auf **Weiter**.

5. Klicken Sie auf **Fertig stellen** , um das Zertifikat zu exportieren.

### <a name="a-namegeneratea2-generate-the-client-certificate"></a><a name="generate"></a>2. das Client-Zertifikat generieren

Als Nächstes wird die Clientzertifikate. Sie können entweder ein eindeutige Zertifikat generieren, für jeden Client, der eine Verbindung herstellt, oder Sie können das gleiche Zertifikat auf mehreren Clients. Der Vorteil bei Generieren von eindeutigen Client-Zertifikate ist die Möglichkeit, ein einzelnes Zertifikat widerrufen, falls erforderlich. Andernfalls, wenn alle ist das gleiche Clientzertifikat verwenden, und Sie feststellen, dass Sie das Zertifikat für einen Client Sperren müssen, müssen Sie zum Generieren und installieren neue Zertifikate für alle Clients, die das Zertifikat zum Authentifizieren verwenden. Auf jedem Clientcomputer später in dieser Übung werden die Clientzertifikate installiert.

- Wenn Sie eine Zertifikat-Lösung Enterprise verwenden, generieren ein Client-Zertifikat mit der gemeinsamen Wert Namensformat 'name@yourdomain.com', statt der NetBIOS 'Domaene\benutzername' formatieren. 

- Wenn Sie ein selbst signiertes Zertifikat verwenden, finden Sie unter [Arbeiten mit selbstsignierten Stammzertifikaten für Punkt-zu-Standort-Konfigurationen](vpn-gateway-certificates-point-to-site.md) , um ein Client-Zertifikat generieren.

### <a name="a-nameexportclientcerta3-export-the-client-certificate"></a><a name="exportclientcert"></a>3 das Client-Zertifikat exportieren

Ein Client-Zertifikat ist zur Authentifizierung erforderlich. Exportieren Sie nach dem Generieren des Clientzertifikats, es aus. Das Clientzertifikat, das Sie exportieren wird später auf jedem Clientcomputer installiert.

1. Wenn Sie ein Client-Zertifikat exportieren möchten, können Sie *certmgr.msc*verwenden. Mit der rechten Maustaste in des Client-Zertifikats, das Sie exportieren, klicken Sie auf **Alle Aufgaben**, und klicken Sie dann auf **Exportieren**möchten.
2. Exportieren Sie das Client-Zertifikat mit dem privaten Schlüssel. Dies ist eine *PFX* -Datei. Vergewissern Sie sich zum Aufzeichnen oder vergessen Sie nicht das Kennwort (Schlüssel), das Sie für dieses Zertifikat festlegen.

### <a name="a-nameuploada4-upload-the-root-certificate-cer-file"></a><a name="upload"></a>4. Root Zertifikat CER-Datei hochladen

Deklarieren Sie die Variable für Ihr Zertifikatsname, den Wert durch ein eigenes ersetzen:

        $P2SRootCertName = "Mycertificatename.cer"

Die Zertifikate mit öffentlichem Daten für das Stammzertifikat Azure hinzufügen. Sie können Dateien für bis zu 20 Stammzertifikate hochladen. Sie nicht den privaten Schlüssel für das Zertifikat der Stamm in Azure hochladen. Sobald die CER-Datei hochgeladen wird, von Azure-Clients authentifizieren, die mit dem virtuellen Netzwerk verwendet. 

Ersetzen Sie den Dateipfad durch ein eigenes, und führen Sie dann auf die Cmdlets.
    
        $filePathForCert = "C:\cert\Mycertificatename.cer"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="a-namecreategatewayapart-4---create-the-vpn-gateway"></a><a name="creategateway"></a>Teil 4: Erstellen des Gateways VPN

Konfigurieren Sie und erstellen Sie für Ihr VNet virtuelle Netzwerk-Gateway. Die *-GatewayType* muss **Vpn** und die *VpnType -* **RouteBased**werden müssen. Dies kann bis zu 45 Minuten dauern.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="a-nameclientconfigapart-5---download-the-vpn-client-configuration-package"></a><a name="clientconfig"></a>Teil 5 - Download VPN-Client-Konfigurations-Paket

Herstellen einer Verbindung mit Azure mit P2S Clients müssen sowohl ein Client-Zertifikat und einem VPN-Client-Konfiguration-Paket installiert. VPN-Client-Konfigurationspakete stehen für Windows-Clients. Das VPN-Client-Paket enthält Informationen, um die Option VPN-Clientsoftware konfigurieren, die in Windows integriert ist und bezieht sich nur auf die Option VPN, die Sie in eine Verbindung herstellen möchten. Das Paket keine zusätzlichen Software installieren. Finden Sie die [Option VPN Gateway häufig gestellte Fragen zu](vpn-gateway-vpn-faq.md#point-to-site-connections) für Weitere Informationen.

1. Nachdem das Gateway erstellt wurde, können Sie das Client-Konfigurations-Paket herunterladen. In diesem Beispiel wird das Paket für 64-Bit-Clients heruntergeladen. Wenn Sie den 32-Bit-Client herunterladen möchten, ersetzen Sie 'Amd64' mit 'X86'. Sie können auch den VPN-Client mithilfe des Azure-Portals herunterladen.

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

2. Das PowerShell-Cmdlet gibt eine URL-Verknüpfung. Dies ist ein Beispiel für welche die zurückgegebene URL sieht wie folgt aus:

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"

3. Kopieren Sie und fügen Sie den Link, der auf einen Webbrowser, um das Paket herunterzuladen zurückgegeben wird. Installieren Sie das Paket klicken Sie dann auf dem Clientcomputer. Wenn Sie ein Popup SmartScreen erhalten möchten, klicken Sie auf **Weitere Informationen anzuzeigen**, klicken Sie dann **trotzdem ausführen** , um das Paket zu installieren.

4. Navigieren Sie zu **Deren Einstellungen im Netzwerk** , und klicken Sie auf **VPN**, auf dem Clientcomputer. Sie sehen die Verbindung aufgeführt. Es zeigt den Namen des virtuellen Netzwerk, dem eine Verbindung herstellt und Looks ähnlich wie in diesem Beispiel: 

    ![VPN-client] (./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png "VPN-client")


## <a name="a-nameclientcertificateapart-6---install-the-client-certificate"></a><a name="clientcertificate"></a>Teil 6 - Clientzertifikat installieren

Jeder Clientcomputer müssen ein Client-Zertifikat für die Authentifizierung. Wenn Sie das Client-Zertifikat installieren zu können, benötigen Sie das Kennwort, das erstellt wurde, wenn das Client-Zertifikat exportiert wurde.

1. Kopieren Sie die PFX-Datei auf dem Clientcomputer.
2. Doppelklicken Sie auf die PFX-Datei, um ihn zu installieren. Ändern Sie den Installationsspeicherort nicht.

## <a name="a-nameconnectapart-7---connect-to-azure"></a><a name="connect"></a>Teil 7: Herstellen einer Verbindung Azure mit

1. Navigieren Sie zu VPN-Verbindungen, und suchen Sie die VPN-Verbindung, die Sie erstellt haben, zum Verbinden mit Ihrer VNet, auf dem Clientcomputer. Es wird als denselben Namen wie virtuelle Netzwerk bezeichnet. Klicken Sie auf **Verbinden**. Möglicherweise eine Popup-Meldung angezeigt, die bei der Verwendung von des Zertifikats verweist. Wenn dies der Fall ist, klicken Sie auf **Weiter** , um erhöhten verwenden. 

2. Klicken Sie auf der Seite **Verbindung** Status auf **Verbinden** , um die Verbindung zu starten. Wenn Sie einen **Zertifikat auswählen** -Bildschirm angezeigt wird, stellen Sie sicher, dass das Client-Zertifikat, das angezeigt, die Sie verwenden wird, um eine Verbindung herstellen möchten. Wenn es nicht der Fall ist, verwenden Sie den Dropdown-Pfeil, um das richtige Zertifikat auszuwählen, und klicken Sie dann auf **OK**.

    ![VPN-Client-Verbindung] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "VPN-Client-Verbindung")

3. Nun sollte die Verbindung hergestellt werden.

    ![Verbindung hergestellt] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Verbindung hergestellt")

## <a name="a-nameverifyapart-8---verify-your-connection"></a><a name="verify"></a>Teil 8: Überprüfen Sie die Verbindung

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

## <a name="a-nameaddremovecertato-add-or-remove-a-trusted-root-certificate"></a><a name="addremovecert"></a>Hinzufügen oder entfernen ein vertrauenswürdiges Zertifikat

Zertifikate werden zum Authentifizieren des VPN-Clients für Punkt-zu-Standort VPN verwendet. Die folgenden Schritte führen Sie durch das Hinzufügen und Entfernen von Zertifikaten aus. Wenn Sie eine Datei Base64-codierte x. 509 (.) Azure hinzufügen, werden Sie ein anderer Benutzer Azure Root Zertifikat vertrauen, das die Datei darstellt. 

Sie können hinzufügen oder Entfernen von Zertifikate der vertrauenswürdigen Stammzertifizierungsstelle mithilfe der PowerShell oder der Azure-Portal. Wenn Sie dazu das Azure-Portal verwenden möchten, navigieren Sie zu Ihrer **virtuelle Netzwerk-Gateway > Einstellungen > Punkt-zu-Standort Konfiguration > Root-Zertifikate**. Die folgenden Schritte führen Sie durch die folgenden Aufgaben mithilfe der PowerShell. 

### <a name="add-a-trusted-root-certificate"></a>Fügen Sie ein vertrauenswürdiges Zertifikat hinzu.

Sie können bis zu 20 vertrauenswürdiges Zertifikat CER-Dateien in Azure hinzufügen. Gehen Sie folgendermaßen vor, um ein Zertifikat einer Stammzertifizierungsstelle hinzuzufügen.

1. Erstellen Sie und bereiten Sie des neuen Stammzertifikats vor, die in Azure hinzugefügt wird. Öffentlichen Schlüssel exportieren, als Base-64 x. 509 (. CER), und öffnen Sie es mit einem Text-Editor. Kopieren Sie nur den Abschnitt unten gezeigten. 
 
    Kopieren Sie die Werte aus, wie im folgenden Beispiel gezeigt:

    ![Zertifikat] (./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png "Zertifikat")
    
2. Geben Sie als Variable Zertifikatsname und wichtige Informationen ein. Ersetzen Sie die Informationen durch eigene, wie im folgenden Beispiel:

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"


3. Fügen Sie das neue Root Zertifikat hinzu. Sie können nur jeweils ein Zertifikat hinzufügen.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2


4. Sie können überprüfen, dass das neue Zertifikat ordnungsgemäß über das folgende Cmdlet hinzugefügt wurde.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

### <a name="to-remove-a-trusted-root-certificate"></a>So entfernen Sie ein vertrauenswürdiges Zertifikat

Sie können aus Azure vertrauenswürdiges Zertifikat entfernen. Wenn Sie ein vertrauenswürdiges Zertifikat entfernen, werden die Clientzertifikate, die aus dem Stammordner Zertifikat generiert wurden nicht mehr in Azure über Punkt-zu-Standort eine Verbindung herstellen können. Wenn Sie Clients eine Verbindung herstellen möchten, müssen sie ein neues Clientzertifikat zu installieren, das aus einem Zertifikat generiert wird, der in Azure vertraut ist.

1. Um ein vertrauenswürdiges Zertifikat entfernen möchten, ändern Sie im folgende Beispiel:

    Deklarieren Sie die Variablen.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. Entfernen Sie das Zertifikat ein.

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
 
3. Verwenden Sie das folgende Cmdlet aus, um Stellen Sie sicher, dass das Zertifikat erfolgreich entfernt wurde. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

## <a name="a-namerevokeato-manage-the-list-of-revoked-client-certificates"></a><a name="revoke"></a>Zum Verwalten der Liste der gesperrten Client-Zertifikate

Sie können Client-Zertifikate widerrufen. Der Sperrliste können Sie basierend auf den einzelnen Client-Zertifikate Punkt-zu-Standort-Konnektivität Selektives verweigern. Wenn Sie eine Root Zertifikat CER aus Azure entfernen, widerrufen wird den Zugriff für alle Client-Zertifikate generiert/angemeldet durch das gesperrte Root Zertifikat. Wenn Sie ein bestimmtes Client-Zertifikat nicht im Stammverzeichnis, widerrufen möchten können Sie dies tun. Auf diese Weise, die die anderen Zertifikate, die aus dem Stammordner Zertifikat generiert wurden weiterhin werden sein gültig.

Meist besteht darin, das Zertifikat der Stamm zum Verwalten des Zugriffs auf Team oder Organisationsebene, während der Verwendung von gesperrten Client-Zertifikate für abgestimmte Access-Steuerelement auf einzelne Benutzer zu verwenden.

### <a name="revoke-a-client-certificate"></a>Ein Client-Zertifikat widerrufen

1. Abrufen des Fingerabdrucks des Clientzertifikats zu widerrufen.

        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "‎ef2af033d0686820f5a3c74804d167b88b69982f"

2. Hinzufügen des Fingerabdrucks zur Liste der gesperrten Fingerabdruck an.

        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

3. Stellen Sie sicher, dass der Fingerabdruck der Sperrliste hinzugefügt wurde. Sie müssen eine Fingerabdruck nacheinander hinzufügen.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### <a name="reinstate-a-client-certificate"></a>Reaktivieren eines Client-Zertifikats

Sie können ein Client-Zertifikat reaktivieren, indem Sie den Fingerabdruck aus der Liste der gesperrten Client-Zertifikate entfernen.

1.  Entfernen Sie den Fingerabdruck aus der Liste der gesperrten Client Fingerabdruck des Zertifikats an.

        Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

2. Überprüfen Sie, ob der Fingerabdruck aus der Liste der gesperrten entfernt wird.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## <a name="next-steps"></a>Nächste Schritte

Sie können einen virtuellen Computer mit Ihrem Netzwerk virtuelle hinzufügen. Schritte finden Sie unter [Erstellen eines virtuellen Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .