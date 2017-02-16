### <a name="determine-the-dns-name-of-the-virtual-machine"></a>Bestimmen Sie den DNS-Namen des virtuellen Computers

Zum Verbinden mit SQL Server-Datenbank-Engine von einem anderen Computer müssen Sie den Namen (DNS = Domain Name System) des virtuellen Computers kennen. (Dies ist der Name, der im Internet zur Identifizierung des virtuellen Computers verwendet. Sie können die IP-Adresse verwenden, aber die IP-Adresse ändert sich möglicherweise die beim Azure Ressourcen für Redundanz oder Wartung ausgeführt werden. Der DNS-Name wird unveränderliche, da es an eine neue IP-Adresse umgeleitet werden kann.)  

1. Wählen Sie im Portal Azure (oder aus dem vorherigen Schritt) **virtuellen Computern (klassische)**.

2. Wählen Sie aus der SQL-virtueller Computer.

2. Kopieren Sie den **DNS-Namen** für den virtuellen Computer auf das Blade **virtuellen Computern** .

    ![DNS-name](./media/virtual-machines-sql-server-connection-steps/sql-vm-dns-name.png)


### <a name="connect-to-the-database-engine-from-another-computer"></a>Verbinden Sie mit der Datenbank-Engine von einem anderen computer

1. Öffnen Sie auf einem Computer mit dem Internet verbunden ist SQL Server Management Studio aus.

2. Geben Sie im Dialogfeld **mit Server verbinden** oder **Verbinden mit Datenbank-Engine** in das Feld **Servername** den DNS-Namen des virtuellen Computers (festgelegt in den vorherigen Vorgang) und einer öffentlichen Endpunkt Port Zahl in das Format der *DNS-Name, Portnumber* wie **mysqlvm.cloudapp.net,57500**.

    ![Eine Verbindung mit SSMS](./media/virtual-machines-sql-server-connection-steps/33Connect-SSMS.png)

    Wenn Sie die Anzahl der öffentlichen Endpunkt Port vergessen Sie zuvor erstellt haben, können Sie es im Bereich **Endpunkte** des **virtuellen Computers** Blades finden.

    ![Öffentlicher Port](./media/virtual-machines-sql-server-connection-steps/sql-vm-port-number.png)

3. Wählen Sie im Feld **Authentifizierung** **SQL Server-Authentifizierung**ein.

5. Geben Sie im Feld **Benutzername** ein, die Sie in einer früheren Aufgabe erstellt Benutzername.

6. Geben Sie im Feld **Kennwort** das Kennwort für den Benutzernamen, den die in einer früheren Aufgabe erstellt.

7. Klicken Sie auf **Verbinden**.
