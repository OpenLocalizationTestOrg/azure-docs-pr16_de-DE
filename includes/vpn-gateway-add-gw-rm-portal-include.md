1. Wechseln Sie im Portal auf **neu**. Geben Sie "Virtuelle Netzwerk-Gateway" in suchen. Suchen nach **virtuellen Netzwerk-Gateway** in das Feld Suchen zurückgegeben, und klicken Sie auf den Eintrag. Dadurch wird das **Erstellen virtuelle Netzwerk-Gateway** Blade geöffnet.
2. Klicken Sie auf **Erstellen** am unteren Rand der **virtuellen Netzwerk-Gateway** Blade. Das **Erstellen virtuelle Netzwerk-Gateway** Blade wird geöffnet. Füllen Sie die Werte für Ihr virtuelles Netzwerk-Gateway ein.

    ![Erstellen virtuelle Netzwerk Gateway Blade (Felder)] (./media/vpn-gateway-add-gw-rm-portal-include/createvnetgw300.png "Erstellen virtuelle Netzwerk Gateway Blade (Felder)")

3. **Name**: Name der Gateways. Dies ist nicht identisch mit einem Gateway Subnetz benennen. Es ist der Name des Gatewayobjekts, das Sie erstellen.

4. **Gateway Type**: Wählen Sie **VPN**. VPN-Gateways verwenden Sie den Gateway-Typ des virtuellen Netzwerk **VPN**. 

5. **VPN-Typ**: Wählen Sie den Typ von VPN, die für Ihre Konfiguration angegeben ist. Die meisten Konfigurationen erfordern einen Routing-basierten VPN-Typ.

6. **SKU**: Wählen Sie den Gateway SKU aus der Dropdownliste aus. Die SKUs aufgeführt, die in der Dropdownliste richtet sich nach dem VPN, die Sie auswählen.

7. **Standort**: Anpassen im Feld **Ort** , an die Position verweisen, wo Ihre virtuelle Netzwerk befindet.
 
8. Wählen Sie das virtuelle Netzwerk, das Sie dieses Gateway hinzufügen möchten. Klicken Sie auf **virtuellen Netzwerk** , um das Blade **auswählen ein virtuelles Netzwerk** zu öffnen. Wählen Sie die VNet aus. Wenn Sie Ihre VNet angezeigt werden, stellen Sie sicher, dass im Feld **Ort** den Bereich zeigt in dem sich Ihr virtuelle Netzwerk befindet.

9. Wählen Sie eine öffentliche IP-Adresse ein. Klicken Sie auf **öffentliche IP-Adresse** , um das Blade **auswählen öffentliche IP-Adresse** zu öffnen. Klicken Sie auf **+ neu erstellen** zum Öffnen der **öffentlichen IP-Adresse Karte erstellen**. Geben Sie einen Namen für Ihre öffentliche IP-Adresse ein. Diese Blade erstellt eine öffentliche IP-Adressobjekt, der eine öffentliche IP-Adresse dynamisch zugewiesen werden soll.<br>Klicken Sie auf **OK** , um die Änderungen zu speichern, um diese Blade.

10. **Abonnements**: Stellen Sie sicher, dass das richtige Abonnement aktiviert ist.

11. **Ressourcengruppe**: mit dieser Einstellung wird durch das virtuelle Netzwerk aus, die Sie auswählen, bestimmt. 

12. Passen Sie den **Speicherort** nicht, nachdem Sie die vorherigen Einstellungen angegeben haben.

13. Überprüfen Sie die Einstellungen aus. Sie können **an Dashboard anheften** am unteren Rand der Blade auswählen, wenn Sie Ihr Gateway auf dem Dashboard angezeigt werden soll.

14. Klicken Sie auf **Erstellen** , um mit dem Erstellen des Gateways beginnen. Die Einstellungen überprüft werden sollen, und sehen Sie "Bereitstellen von virtuellen Netzwerkgateway" auf dem Dashboard Kachel. Erstellen eines Gateways kann bis zu 45 Minuten dauern. Möglicherweise müssen Sie der Portalseite zum Anzeigen des abgeschlossenen Status aktualisieren.

    ![Bereitstellen von virtuellen Netzwerk-gateway] (./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png "Bereitstellen von virtuellen Netzwerk-gateway")

11. Nachdem das Gateway erstellt wurde, können Sie die IP-Adresse anzeigen, die darauf zugewiesen wurde, indem Sie das virtuelle Netzwerk im Portal. Das Gateway wird als ein angeschlossenes Gerät angezeigt. Sie können klicken Sie auf das verbundene Gerät (Schlüsselaufgaben virtuelles Netzwerk), um weitere Informationen anzuzeigen.



