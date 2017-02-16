## <a name="quick-steps"></a>QuickSteps 

Im Artikel wird vorausgesetzt, dass Sie Ihr Abonnement im Portal angemeldet haben, und ein virtuellen Computers mit verfügbaren Bilder mit dem Ressourcenmanager Bereitstellungsmodell erstellt. Gehen Sie folgendermaßen vor, nach dem Start des virtuellen Computers ausgeführt.

1.  Zeigen Sie die Einstellungen des virtuellen Computers auf dem Portal an, und klicken Sie auf die öffentliche IP-Adresse.

    ![Suchen Sie nach IP-Ressourcen](./media/virtual-machines-common-portal-create-fqdn/locatePublicIP.PNG)

2.  Beachten Sie, dass der DNS-Namen für die öffentliche IP-Adresse leer ist. Klicken Sie auf **Konfiguration** für die öffentliche IP-Karte.

    ![Einstellungen ip](./media/virtual-machines-common-portal-create-fqdn/settingsIP.PNG)

3.  Geben Sie die gewünschte Bezeichnung für DNS und Konfiguration **Speichern** .

    ![Geben Sie die Bezeichnung für dns](./media/virtual-machines-common-portal-create-fqdn/dnsNameLabel.PNG)

    Öffentliche IP-Ressource zeigt jetzt diese neuen DNS-Beschriftung auf deren Blade aus.

4.  Schließen Sie die Blades öffentliche IP-Adresse, und kehren Sie zum virtuellen Computern vorher in das Portal. Stellen Sie sicher, dass der DNS-Name/FQDN neben die IP-Adresse für die öffentliche IP-Ressource angezeigt wird.

    ![FQDN wird erstellt.](./media/virtual-machines-common-portal-create-fqdn/fqdnCreated.PNG)