<properties
   pageTitle="Konfigurieren einer Punkt-zu-Standort VPN Gateway-Verbindung mit einem Azure virtuelle Netzwerk im Portal klassische | Microsoft Azure"
   description="Sicheres Herstellen einer Verbindung mit Ihrem Azure-virtuellen Netzwerk durch Erstellen einer Punkt-zu-Standort VPN-Gateway-Verbindungs."
   services="vpn-gateway"
   documentationCenter="na"
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
   ms.date="10/17/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-classic-portal"></a>Konfigurieren einer Punkt-zu-Standort-Verbindung zu einer VNet über das klassische-portal

> [AZURE.SELECTOR]
- [Ressourcenmanager - Azure-Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Ressourcenmanager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klassische - Azure-Portal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
- [Klassische - klassischen-Portal](vpn-gateway-point-to-site-create.md)

Die Konfiguration einer Punkt-zu-Standort (P2S) können Sie eine sichere Verbindung aus einem einzelnen Clientcomputer zu einem virtuellen Netzwerk zu erstellen. Eine Verbindung P2S ist nützlich, wenn Sie zu Ihrer VNet von einem entfernten Standort, wie Start oder einer Konferenz, eine Verbindung herstellen möchten, oder wenn Sie nur wenige Clients haben, die Verbindung zu einem virtuellen Netzwerk benötigen.

In diesem Artikel führt Sie durch die Erstellung einer VNet mit einer Punkt-zu-Standort-Verbindung im **Modell zur klassischen Bereitstellung** Verwenden des **klassischen-Portal**an.

Ein VPN-Gerät oder eine öffentlich zugängliche IP-Adresse entwickelt erforderlich Punkt-zu-Standort-Verbindungen keine. Eine VPN-Verbindung wird hergestellt, indem Sie die Verbindung vom Clientcomputer starten. Weitere Informationen zu-Punkt-zu-Standort-Verbindungen finden Sie unter [VPN Gateway häufig gestellte Fragen](vpn-gateway-vpn-faq.md#point-to-site-connections) und der [Planung und Entwicklung](vpn-gateway-plan-design.md).


### <a name="deployment-models-and-methods-for-p2s-connections"></a>Bereitstellungsmodelle und Methoden für P2S Verbindungen

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

In der folgenden Tabelle werden die zwei Bereitstellungsmodelle und Methoden für die Bereitstellung verfügbar für P2S Konfigurationen. Wenn ein Artikel mit Konfigurationsschritte verfügbar ist, verknüpfen wir aus dieser Tabelle direkt an.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 



## <a name="basic-workflow"></a>Grundlegende workflow

![Punkt-zu-Website-Diagramm] (./media/vpn-gateway-point-to-site-create/p2sclassic.png "Punkt-zu-Standort")
 
Die folgenden Schritte führen Sie durch die Schritte zum Erstellen einer secure Punkt-zu-Standort-Verbindungs zu einem virtuellen Netzwerk an. 

Die Konfiguration für eine Punkt-zu-Standort-Verbindung ist in vier Abschnitte unterteilt. Die Reihenfolge, in der Sie jeder dieser Abschnitte konfigurieren, ist wichtig. Nicht überspringen Sie die Schritte oder springen.

- **Abschnitt 1** Erstellen Sie ein virtuelles Netzwerk und VPN-Gateway ein.
- **Abschnitt 2** Erstellen Sie die Zertifikate für die Authentifizierung verwendet, und Laden Sie sie hoch.
- **Abschnitt 3** Exportieren und Ihre Clientzertifikate installieren.
- **Abschnitt 4** Konfigurieren des VPN-Clients.

## <a name="a-namevnetvpnasection-1---create-a-virtual-network-and-a-vpn-gateway"></a><a name="vnetvpn"></a>Abschnitt 1: Erstellen eines virtuellen Netzwerks und ein VPN-Gateway

### <a name="part-1-create-a-virtual-network"></a>Teil 1: Erstellen ein virtuelles Netzwerks

1. Melden Sie sich im [Azure klassischen Portal](https://manage.windowsazure.com/). Diese Schritte ausführen, das klassische-Portal nicht im Azure-Portal. Derzeit können keine P2S Verbindung mit dem Azure-Portal erstellt werden.

2. Klicken Sie in der unteren linken Ecke des Bildschirms auf **neu**. Klicken Sie im Navigationsbereich klicken Sie auf **Netzwerkdienste**, und klicken Sie dann auf **Virtuelles Netzwerk**. Klicken Sie auf **Benutzerdefinierte erstellen** , um den Konfigurations-Assistenten zu starten.

3. Klicken Sie auf der Seite **Virtuelle Netzwerkdetails** Geben Sie die folgenden Informationen ein, und klicken Sie dann auf den nächsten Pfeil in der unteren rechten Ecke.
    - **Name**: Namen für das virtuelle Netzwerk. Beispielsweise 'VNet1'. Dies ist der Name, die Sie beim Bereitstellen von virtuellen Computern für diese VNet verweisen werden.
    - **Standort**: die Position direkt bezieht sich auf den Standort (Bereich), werden Ihre Ressourcen (virtuelle Computer) befinden soll. Wählen Sie diesem Speicherort angenommen, Sie gegebenenfalls die virtuellen Computern, die Sie für diese virtuelle Netzwerk physisch im südostasiatischen US befinden bereitstellen. Sie können nicht ändern die Region Ihres Netzwerks virtuelle zugeordnet, nachdem Sie es erstellt haben.

4. Klicken Sie auf der Seite **DNS-Server und VPN-Konnektivität** Geben Sie die folgenden Informationen ein, und klicken Sie dann auf den nächsten Pfeil in der unteren rechten Ecke.
    - **DNS-Server**: Geben Sie den DNS-Servernamen und die IP-Adresse ein, oder wählen Sie einen zuvor registrierten DNS-Server aus dem Kontextmenü. Diese Einstellung werden keine DNS-Server erstellt. Sie können Sie die DNS-Server angeben, die Sie für die Auflösung von Namen für diese virtuelle Netzwerk verwenden möchten. Wenn Sie den standardmäßigen Azure Namen Auflösung-Dienst verwenden möchten, lassen Sie diesen Abschnitt leer.
    - **Konfigurieren von Punkt-zu-Standort VPN**: Aktivieren Sie das Kontrollkästchen.

5. Geben Sie auf der Seite **Punkt-zu-Standort-Konnektivität** des IP-Adressbereichs, von dem VPN-Clients eine IP-Adresse, wenn verbunden empfangen werden. Es gibt ein paar Rechtschreibregeln für die Adressbereiche, die Sie angeben können. Es ist wichtig, um sicherzustellen, dass es sich bei der Bereich, den Sie angeben, mit einem der Bereiche, die sich auf Ihrem lokalen Netzwerk befinden überlappt.

6. Geben Sie die folgenden Informationen ein, und klicken Sie dann auf den Pfeil nach rechts.
 - **Adressbereichs**: die Start-IP- und CIDR (Anzahl der Adressen) einzuschließen.
 - **Hinzufügen von Adressbereichs**: Adressbereichs nur hinzufügen, wenn sie für den Netzwerkentwurf erforderlich ist.

7. Geben Sie auf der Seite **Virtuellen Netzwerk Adresse Leerzeichen** Bereich, den Sie für Ihre virtuelle Netzwerk verwenden möchten. Hierbei handelt es sich um die dynamischen IP-Adressen (DIPS), die den virtuellen Computern und andere Rolleninstanzen, die Sie in diesem virtuellen Netzwerk bereitstellen zugewiesen werden.<br><br>Es ist besonders wichtig, wählen Sie einen Bereich, der nicht überlappt mit keine Bereiche, die für das lokale Netzwerk verwendet werden. Sie müssen Ihren Netzwerkadministrator koordinieren, die möglicherweise machen Sie einen Bereich von IP-Adressen aus Ihrem lokalen Netzwerk Adresse Space Sie für Ihre virtuelle Netzwerk verwenden müssen.

8. Geben Sie die folgenden Informationen ein, und klicken Sie dann auf das Häkchen, um mit dem Erstellen von virtuellen Netzwerks beginnen.
 - **Adressbereichs**: Fügen Sie den Bereich von internen IP-Adresse, die Sie für diese virtuelle Netzwerk, einschließlich der erste IP- und Count verwenden möchten. Es ist wichtig, wählen Sie einen Bereich, der nicht überlappt mit keine Bereiche, die für das lokale Netzwerk verwendet werden. 
 - **Hinzufügen von Subnetz**: Weitere Subnetze sind nicht erforderlich, aber Sie möchten ein separates Subnetz für virtuelle Computer zu erstellen, die statischen DIPS. Oder möglicherweise möchten Sie Ihre virtuelle Computer in einem Subnetz haben, die von der anderen Rolleninstanzen getrennt ist.
 - **Hinzufügen Gateway Subnetz**: das Gateway-Subnetz für ein Punkt-zu-Standort VPN erforderlich ist. Klicken Sie auf diese Option, um das Gateway Subnetz hinzuzufügen. Das Gateway Subnetz wird nur für das Gateway virtuelles Netzwerk verwendet.

9. Sobald virtuellen Netzwerks erstellt wurde, sehen Sie sich, dass **erstellt** auf der Seite Netzwerke im klassischen Azure-Portal unter **Status** aufgelistet. Nachdem Sie Ihr Netzwerk virtuelle erstellt wurde, können Sie dynamische Weiterleitung Schlüsselaufgaben erstellen.

### <a name="part-2-create-a-dynamic-routing-gateway"></a>Teil 2: Erstellen ein dynamisches Weiterleitung Gateways

Der Typ des Gateways muss als dynamisch konfiguriert sein. Statische Weiterleitung Gateways funktionieren nicht mit diesem Feature.

1. Klicken Sie im Azure klassischen-Portal auf der Seite **Netzwerke** auf das virtuelle Netzwerk aus, das Sie erstellt haben, und navigieren Sie zu der Seite " **Dashboard** ".

2. Klicken Sie auf **Gateway erstellen**, befindet sich am unteren Rand der Seite **Dashboard** . Wird eine Meldung angezeigt, mit der Frage **möchten Sie Erstellen eines Gateways für virtuelle Netzwerk "VNet1"**. Klicken Sie auf **Ja,** um mit dem Erstellen des Gateways beginnen. Es dauert ungefähr 15 Minuten, damit das Gateway zu erstellen.

## <a name="a-namegenerateasection-2---generate-and-upload-certificates"></a><a name="generate"></a>Abschnitt 2: generieren und Hochladen Zertifikate

Zertifikate werden zum Authentifizieren des VPN-Clients für Punkt-zu-Standort VPN verwendet. Können Sie ein Zertifikat einer Stammzertifizierungsstelle durch eine Enterprise-Lösung Zertifikat generiert, oder Sie können ein selbst signiertes Zertifikat verwenden. Sie können bis zu 20 Stammzertifikate in Azure hochladen. Sobald die CER-Datei hochgeladen wird, können Azure die darin enthaltene Informationen werden zum Authentifizieren von Clients, die ein Clientzertifikat installiert sein. Das Client-Zertifikat muss in das gleiche Zertifikat generiert werden, die die CER-Datei darstellt.

In diesem Abschnitt werden Sie die folgenden Aktionen ausführen:

- CER-Datei für ein Zertifikat einer Stammzertifizierungsstelle zu erhalten. Dies kann entweder ein selbst signiertes Zertifikat, oder Sie können auch das Zertifikat Enterprise verwenden.
- Laden Sie die CER-Datei in Azure hoch.
- Generieren Sie Clientzertifikate.

### <a name="a-namerootapart-1-obtain-the-cer-file-for-the-root-certificate"></a><a name="root"></a>Teil 1: CER-Datei für das Zertifikat Quadratwurzel zu erhalten

Wenn Sie ein Zertifikat Enterprise-System verwenden, beziehen Sie die CER-Datei für das Stamm-Zertifikat, das Sie verwenden möchten. In [Teil 3](#createclientcert)generieren Sie die Clientzertifikate aus dem Stammordner Zertifikat.

Wenn Sie eine Zertifikat-Lösung Enterprise nicht verwenden, müssen Sie ein Zertifikat selbstsignierten Stamm zu generieren. Für Windows 10 Schritten können Sie für die [Arbeit mit Quadratwurzel von selbstsignierten Zertifikaten für Punkt-zu-Standort Konfigurationen](vpn-gateway-certificates-point-to-site.md)verweisen. Im Artikel führt Sie durch die Verwendung von Makecert ein selbst signiertes Zertifikat generieren, und klicken Sie dann die CER-Datei exportieren.

### <a name="a-nameuploadapart-2-upload-the-root-certificate-cer-file-to-the-azure-classic-portal"></a><a name="upload"></a>Teil 2: Hochladen der Root Zertifikat CER-Datei zum klassischen Azure-portal

Hinzufügen eines vertrauenswürdigen Zertifikats in Azure. Wenn Sie eine Datei Base64-codierte x. 509 (.) Azure hinzufügen, werden Sie ein anderer Benutzer Azure Root Zertifikat vertrauen, das die Datei darstellt.

1. Im Azure klassischen-Portal auf der Seite **Zertifikate** für Ihre virtuelle Netzwerk klicken Sie auf **ein Zertifikat einer Stammzertifizierungsstelle hochladen**.

2. Suchen Sie auf der Seite **Zertifikat hochladen** nach der CER-Root Zertifikat, und klicken Sie dann auf das Häkchen.

### <a name="a-namecreateclientcertapart-3-generate-a-client-certificate"></a><a name="createclientcert"></a>Teil 3: Erstellen Sie ein Clientzertifikat

Als Nächstes wird die Clientzertifikate. Sie können entweder ein eindeutige Zertifikat generieren, für jeden Client, der eine Verbindung herstellt, oder Sie können das gleiche Zertifikat auf mehreren Clients verwenden. Der Vorteil bei Generieren von eindeutigen Client-Zertifikate ist die Möglichkeit, ein einzelnes Zertifikat widerrufen, falls erforderlich. Andernfalls, wenn alle ist das gleiche Clientzertifikat verwenden, und Sie feststellen, dass Sie das Zertifikat für einen Client Sperren müssen, müssen Sie zum Generieren und installieren neue Zertifikate für alle Clients, die das Zertifikat zum Authentifizieren verwenden.

- Wenn Sie eine Zertifikat-Lösung Enterprise verwenden, generieren ein Client-Zertifikat mit der gemeinsamen Wert Namensformat 'name@yourdomain.com', statt der NetBIOS 'Domaene\benutzername' formatieren. 

- Wenn Sie ein selbst signiertes Zertifikat verwenden, finden Sie unter [Arbeiten mit selbstsignierten Stammzertifikaten für Punkt-zu-Standort-Konfigurationen](vpn-gateway-certificates-point-to-site.md) , um ein Client-Zertifikat generieren.

## <a name="a-nameinstallclientcertasection-3---export-and-install-the-client-certificate"></a><a name="installclientcert"></a>Abschnitt 3: exportieren, und installieren Sie das Client-Zertifikat

Installieren Sie ein Client-Zertifikat auf jedem Computer, die Sie an das virtuelle Netzwerk eine Verbindung herstellen möchten. Ein Client-Zertifikat ist zur Authentifizierung erforderlich. Sie können die Installation von dem Client-Zertifikat automatisieren, oder Sie können manuell installieren. Die folgenden Schritte führen Sie durch das Exportieren und das Client-Zertifikat manuell installieren.

1. Wenn Sie ein Client-Zertifikat exportieren möchten, können Sie *certmgr.msc*verwenden. Mit der rechten Maustaste in des Client-Zertifikats, das Sie exportieren, klicken Sie auf **Alle Aufgaben**, und klicken Sie dann auf **Exportieren**möchten.
2. Exportieren Sie das Client-Zertifikat mit dem privaten Schlüssel. Dies ist eine *PFX* -Datei. Vergewissern Sie sich zum Aufzeichnen oder vergessen Sie nicht das Kennwort (Schlüssel), das Sie für dieses Zertifikat festlegen.
3. Kopieren Sie die *PFX* -Datei auf dem Clientcomputer. Doppelklicken Sie auf die *PFX* -Datei, um ihn zu installieren, auf dem Clientcomputer. Geben Sie das Kennwort, die bei der Anforderung aus. Ändern Sie den Installationsspeicherort nicht.

## <a name="a-namevpnclientconfigasection-4---configure-your-vpn-client"></a><a name="vpnclientconfig"></a>Abschnitt 4: konfigurieren den VPN-Client

Informationen zum Verbinden mit dem virtuellen Netzwerk müssen Sie auch einen VPN-Client konfigurieren. Der Client erfordert ein Client-Zertifikat, und die ordnungsgemäße Konfiguration der VPN-Client und herstellen. Führen Sie die folgenden Schritte aus, um ein VPN-Client zu konfigurieren, in Reihenfolge.

### <a name="part-1-create-the-vpn-client-configuration-package"></a>Teil 1: Erstellen das VPN-Client-Konfiguration-Paket

1. Im Azure klassischen-Portal auf der Seite " **Dashboard** " für Ihre virtuelle Netzwerk navigieren Sie zum schnellen Blick Menü rechts. Eine Liste der Client-Betriebssystemen, die unterstützt werden, finden Sie unter der [Punkt-zu-Standort-Verbindungen](vpn-gateway-vpn-faq.md#point-to-site-connections) Abschnitt die Option VPN Gateway häufig gestellte Fragen. Das VPN-Client-Paket enthält Informationen zum Konfigurieren der VPN-Client-Software in Windows integriert. Das Paket keine zusätzlichen Software installieren. Die Einstellungen gelten nur für das virtuelle Netzwerk aus, dem Sie in eine Verbindung herstellen möchten.<br><br>Wählen Sie das Downloadpaket, das das Client-Betriebssystem entspricht, auf denen es installiert werden:
 - Wählen Sie für 32-Bit-Clients **die 32-Bit-Client-VPN-Paket herunterladen**.
 - Wählen Sie für 64-Bit-Clients **die 64-Bit-Client-VPN-Paket herunterladen**.

2. Es dauert ein paar Minuten Ihr Clientpaket zu erstellen. Nachdem das Paket abgeschlossen ist, können Sie die Datei herunterladen. Die *.exe* -Datei, die Sie herunterladen kann sicher auf Ihrem lokalen Computer gespeichert werden.

3. Nachdem Sie generiert und das VPN-Client-Paket vom klassischen Azure-Portal herunterladen, können Sie das Client-Paket auf dem Clientcomputer installieren, von der Verbindung mit Ihrem Netzwerk virtuelle aus. Wenn Sie das VPN-Client-Paket auf mehreren Clientcomputern installieren möchten, stellen Sie sicher, dass sie jeweils auch ein Client-Zertifikat installiert haben.

### <a name="part-2-install-the-vpn-configuration-package-on-the-client"></a>Teil 2: Installieren der Paket VPN-Konfiguration auf dem client

1. Kopieren Sie die Konfigurationsdatei lokal auf dem Computer, den Sie verwenden möchten, eine Verbindung mit Ihrem Netzwerk virtuelle und doppelklicken Sie auf die .exe-Datei. 

2. Nachdem das Paket installiert wurde, können Sie die VPN-Verbindung starten. Das Konfigurationspaket ist nicht von Microsoft signiert. Sie möglicherweise das Paket mithilfe von Ihrer Organisation signierenden Service melden möchten, oder melden sich mit [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327). Es ist OK, um das Paket verwenden, ohne sich anzumelden. Jedoch, wenn das Paket signiert nicht zur Verfügung, wird eine Warnung angezeigt, wenn Sie das Paket installieren.

3. Navigieren Sie zu **Deren Einstellungen im Netzwerk** , und klicken Sie auf **VPN**, auf dem Clientcomputer. Sie sehen die Verbindung aufgeführt. Es wird der Name des virtuellen Netzwerks angezeigt, es wird Herstellen einer Verbindung mit und sieht ungefähr wie folgt: 

    ![VPN-client] (./media/vpn-gateway-point-to-site-create/vpn.png "VPN-client")


### <a name="part-3-connect-to-azure"></a>Teil 3: Verbinden mit Azure

1. Navigieren Sie zu VPN-Verbindungen, und suchen Sie die VPN-Verbindung, die Sie erstellt haben, zum Verbinden mit Ihrer VNet, auf dem Clientcomputer. Es wird als denselben Namen wie virtuelle Netzwerk bezeichnet. Klicken Sie auf **Verbinden**. Möglicherweise eine Popup-Meldung angezeigt, die bei der Verwendung von des Zertifikats verweist. Wenn dies der Fall ist, klicken Sie auf **Weiter** , um erhöhten verwenden. 

2. Klicken Sie auf der Statusseite **Verbindung** auf **Verbindung herstellen** , um die Verbindung zu starten. Wenn Sie einen **Zertifikat auswählen** -Bildschirm angezeigt wird, stellen Sie sicher, dass das Client-Zertifikat, das angezeigt, die Sie verwenden wird, um eine Verbindung herstellen möchten. Wenn es nicht der Fall ist, verwenden Sie den Dropdown-Pfeil, um das richtige Zertifikat auszuwählen, und klicken Sie dann auf **OK**.

    ![VPN-Client 2] (./media/vpn-gateway-point-to-site-create/clientconnect.png "VPN-Client-Verbindung")

3. Nun sollte die Verbindung hergestellt werden.

    ![VPN-Client 3] (./media/vpn-gateway-point-to-site-create/connected.png "VPN-Client-Verbindung 2")

### <a name="part-4-verify-the-vpn-connection"></a>Teil 4: Überprüfen Sie die Option VPN-Verbindung

1. Stellen Sie sicher, dass die Option VPN-Verbindung aktiv ist, wenn öffnen Sie ein erweitertes Eingabeaufforderungsfenster, und führen Sie *Ipconfig/All*.
2. Anzeigen der Ergebnisse an. Beachten Sie, dass die IP-Adresse, die Sie erhalten eine der Adressen innerhalb des Punkt-zu-Standort Connectivity Adressbereichs, dass Sie beim Erstellen Ihrer VNet angegeben ist. Die Ergebnisse sollten etwa so:

Beispiel:



    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## <a name="next-steps"></a>Nächste Schritte

Sie können mit Ihrem Netzwerk virtuelle virtuellen Computern hinzufügen. Informationen Sie [zum Erstellen eines benutzerdefinierten virtuellen Computers](../virtual-machines/virtual-machines-windows-classic-createportal.md).

Wenn Sie weitere Informationen zu virtuellen Netzwerken möchten, finden Sie unter der Seite [Virtuelle Netzwerk-Dokumentation](https://azure.microsoft.com/documentation/services/virtual-network/) .
