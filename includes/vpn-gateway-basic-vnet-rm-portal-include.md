Um eine VNet im Bereitstellungsmodell Ressourcenmanager mithilfe des Azure-Portals zu erstellen, führen Sie die folgenden Schritte aus. Die Screenshots dienen als Beispiele für. Achten Sie darauf, dass Sie die Werte durch ein eigenes ersetzen. Weitere Informationen zum Arbeiten mit virtuelle Netzwerke finden Sie unter der [Virtuelle Network (Übersicht)](../articles/virtual-network/virtual-networks-overview.md).

1. Mithilfe eines Browsers und navigieren Sie zu der [Azure-Portal](http://portal.azure.com) und, falls notwendig, melden Sie sich mit Ihrem Azure-Konto.

2. Klicken Sie auf **neu**. Geben Sie im Feld **Suchen die Marketplace** "Virtuelle Netzwerk" aus. Suchen Sie **Virtuelle Netzwerk** aus der zurückgegebenen Liste, und klicken Sie auf, um das **Virtuelle Netzwerk** Blade zu öffnen.

    ![Virtuelle Netzwerk suchen Ressource blade] (./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "Suchen nach virtuelles Netzwerk Ressource blade")

3. Im unteren Bereich des Blades virtuelle Netzwerk aus der Liste **Wählen Sie ein Bereitstellungsmodell** **Ressourcenmanager**wählen Sie aus, und klicken Sie dann auf **Erstellen**.


    ![Wählen Sie Ressourcenmanager] (./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png "Wählen Sie Ressourcenmanager")

4. Konfigurieren Sie die VNet-Einstellungen, auf das Blade **virtuelles Netzwerk erstellen** . Wenn Sie die Felder ausfüllen, wird das rote Ausrufezeichen einem grünen Häkchen werden, wenn die in das Feld eingegebenen Zeichen gültig sind.

    ![Feldüberprüfung] (./media/vpn-gateway-basic-vnet-rm-portal-include/checkmark300.png "Feldüberprüfung")

5. Ähnlich wie im folgenden Beispiel wird, sieht das Blade **virtuelles Netzwerk erstellen** aus. Möglicherweise gibt es Werte, die automatisch ausgefüllt werden. In diesem Fall ersetzen Sie die Werte durch ein eigenes ein.

    ![Erstellen virtuelle Netzwerk blade] (./media/vpn-gateway-basic-vnet-rm-portal-include/createvnet300.png "Erstellen virtuelle Netzwerk blade")

6. **Name**: Geben Sie den Namen für das virtuelle Netzwerk.

7. **Adressbereichs**: Geben Sie den Abstand Adresse. Wenn Sie mehrere Adresse Leerzeichen hinzugefügt haben, fügen Sie der ersten Adresse Leerzeichen hinzu. Sie können zusätzliche Adresse Leerzeichen nach dem Erstellen der VNet später hinzufügen.
 
8. **Subnetnamen**: Fügen Sie die Subnetnamen und Subnetzadressbereichs. Sie können weitere Subnetze später hinzufügen, nach dem Erstellen der VNet.

10. **Abonnements**: Stellen Sie sicher, dass das Abonnement aufgeführt richtig ist. Sie können Abonnements mithilfe der Dropdown-Liste ändern.

11. **Ressourcengruppe**: Wählen Sie eine vorhandene Ressourcengruppe oder einen neuen erstellen, indem Sie einen Namen für die neue Ressourcengruppe eingeben. Wenn Sie eine neue Gruppe erstellen, benennen Sie die Ressourcengruppe entsprechend Ihren Werten für geplanten Konfiguration. Weitere Informationen zu Ressourcengruppen finden Sie auf [Azure Ressourcenmanager Übersicht](resource-group-overview.md#resource-groups).

12. **Standort**: Wählen Sie den Speicherort für Ihre VNet aus. Die Position bestimmt, wo die Ressourcen, die Sie für diese VNet bereitstellen gespeichert werden soll.

13. Wählen Sie **Pin zum Dashboard** , wenn Sie möchten, kann der VNet auf dem Dashboard wiederzufinden, und klicken Sie dann auf **Erstellen**.
    
    ![PIN zum dashboard] (./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png "PIN zum dashboard")

14. Nachdem Sie auf **Erstellen**, sehen Sie eine Kachel auf des Dashboards, die den Fortschritt Ihrer VNet widerspiegeln wird. Die Kachel ändert sich während der VNet erstellt wird.

    ![Erstellen von virtuelles Netzwerk-Kachel] (./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png "Erstellen von virtuelles Netzwerk-Kachel")