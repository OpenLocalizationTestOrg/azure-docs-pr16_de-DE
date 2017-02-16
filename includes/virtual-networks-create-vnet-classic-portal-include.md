## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>So erstellen Sie eine VNet Azure-Portal

Führen Sie zum Erstellen einer VNet basierend auf dem oben genannten Szenario die folgenden Schritte aus.

1. Mithilfe eines Browsers und navigieren Sie zu http://manage.windowsazure.com und, falls notwendig, melden Sie sich mit Ihrem Azure-Konto.
2. Klicken Sie auf **neu** > **NETWORK SERVICES** > **VIRTUAL NETWORK** > **Benutzerdefinierte zu erstellen** , wie in der folgenden Abbildung gezeigt.

    ![Erstellen von VNet-Portal](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure1.gif)

3. Klicken Sie auf der Seite **Virtuelle Netzwerk-Details** Geben Sie den **Namen** der VNet, wählen Sie die **Position**aus, und klicken Sie dann auf den Pfeil in der unteren rechten Ecke der Seite, um zur nächsten Schritt 2. Die folgende Abbildung zeigt die Einstellungen für dieses Szenario.

    ![Detailseite virtuelles Netzwerk](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure2.png)

4. Geben Sie den Namen und die IP-Adresse für bis zu 9 DNS-Server verwenden, klicken Sie auf der Seite **DNS-Server und VPN-Konnektivität** . Wenn Sie einen DNS-Server nicht angeben, wird Ihre VNet interne naming Auflösung Auflösung von Azure bereitgestellten verwendet. Für dieses Szenario wird DNS-Server nicht konfiguriert.
5. Wenn Sie Ihre VNet Punkt-zu-Standort VPN Zugriff gewähren möchten, aktivieren Sie das Kontrollkästchen **Konfigurieren eines Punkt-zu-Standort VPN** . Wenn Sie ein Punkt-zu-Standort VPN nicht konfiguriert ist, können Sie es zu Ihrer VNet jederzeit nach der Erstellung hinzufügen. Für dieses Szenario wird ein Punkt-zu-Standort VPN nicht konfiguriert.
6. Wenn Sie die Website-zu-Standort VPN-Konnektivität zwischen Ihrer VNet und eine andere VNet oder aus Ihrem lokalen Netzwerk angeben möchten, aktivieren Sie das Kontrollkästchen **Konfigurieren einer Standort-zu-Standort VPN** , und geben Sie an, ob Sie **ExpressRoute** oder Notiz und den Namen des Netzwerks zum Herstellen einer Verbindung mit verwenden möchten. Wenn ein Standort-zu-Standort VPN nicht konfiguriert ist, können Sie es zu Ihrer VNet zu einem beliebigen Zeitpunkt nach der Erstellung hinzufügen. Für dieses Szenario wird ein Standort-zu-Standort VPN nicht konfiguriert.
7. Klicken Sie auf den Pfeil in der unteren rechten Ecke der Seite, um zu Schritt 3 wechseln, den folgende Abbildung, die Einstellungen für dieses Szenario zeigt.

    ![DNS-Server und VPN Connectivity-Seite](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure3.png)

8. Klicken Sie auf der Seite **Virtuellen Netzwerk Adresse Leerzeichen** unter **Erste IP-** *10.0.0.0* so ändern Sie den Abstand der VNet Adresse klicken Sie auf, und geben Sie dann die erste Adresse Leerzeichen, die Sie verwenden möchten. Geben Sie für dieses Szenario *192.168.0.0*aus. 
9. Wählen Sie unter **CIDR (Adresse COUNT)** die Anzahl von Bits für die Subnetzmaske ein. Wählen Sie für dieses Szenario *16 (65536)*ein.
10. Klicken Sie unter **SUBNETZE**klicken Sie auf *Subnetz-1* , und geben Sie das Subnetz ein, falls notwendig. In diesem Szenario benennen Sie sie in der *Front-End*aus.

    >[AZURE.NOTE] Wenn Sie außerhalb der im Textfeld den Wert für ein Subnetz klicken Sie auf nicht werden können, um den Namen zu bearbeiten, wenn das Subnetz erneut. Fügen Sie beheben, Sie das Subnetz entfernen, indem Sie auf die Schaltfläche X rechts daneben müssen, dann ein neues Subnetz, wie in Schritt 13 unter beschrieben.

11. Geben Sie unter **Erste IP-** für das erste Subnetz die erste IP-Adresse für das Subnetz ein. Geben Sie für dieses Szenario *192.168.1.0*aus.
12. Wählen Sie unter **CIDR (Adresse COUNT)** die Anzahl von Bits für die Subnetzmaske für das erste Subnetz ein. Wählen Sie für dieses Szenario *24 (256)*ein.
13. Klicken Sie bei Bedarf auf **Subnetz hinzufügen** , um ein neues Subnetz, hinzuzufügen. Hinzuzufügen Sie für dieses Szenario ein Subnetz, und wiederholen Sie die Schritte 10 bis 12 so konfigurieren Sie die VNet wie in der folgenden Abbildung gezeigt.

    ![Virtuelle Netzwerk Adresse Leerzeichen Seite](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure4.png)

14. Klicken Sie auf die Schaltfläche Häkchen auf der unteren rechten Ecke der Seite, um die VNet zu erstellen. Nach ein paar Sekunden werden in der Liste der verfügbaren VNets, Ihre VNet angezeigt, wie in der folgenden Abbildung gezeigt.

    ![Neues virtuelles Netzwerk](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure5.png)