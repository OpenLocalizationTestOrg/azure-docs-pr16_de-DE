In diesem Schritt testen Sie die Verfügbarkeit Gruppe Zuhörer mithilfe einer Clientanwendung auf demselben Netzwerk ausgeführt.

Beachten Sie die folgenden Anforderungen, Client-Konnektivität:

- Clientverbindungen mit dem Zuhörer müssen aus Maschinen stammen, die in einem anderen Clouddienst als die, hostet die Verfügbarkeit von AlwaysOn Replikate befinden.

- Wenn die AlwaysOn Replikate in unterschiedlichen Subnetzen sind, müssen Clients angeben "MultisubnetFailover = True" in der Verbindungszeichenfolge. Dies führt parallele Verbindungsversuche auf Replikate in verschiedenen Subnetzen. Beachten Sie, dass dieses Szenario eine Kreuz-Region AlwaysOn Availability Group Bereitstellung enthält.

Ein Beispiel dafür ist eine Verbindung zu der Zuhörer von einem den virtuellen Computern in der gleichen Azure VNet (jedoch keine, die ein Replikat hostet). Eine einfache Möglichkeit zum Abschließen dieser Test ist versuchen, eine Verbindung mit der Verfügbarkeit Gruppe Zuhörer SSMS. Eine weitere einfache Methode besteht darin [SQLCMD.exe](https://technet.microsoft.com/library/ms162773.aspx) wie folgt ausführen:

    sqlcmd -S "<ListenerName>,<EndpointPort>" -d "<DatabaseName>" -Q "select @@servername, db_name()" -l 15

> [AZURE.NOTE] Ist der Wert EndpointPort 1433, ist es nicht erforderlich, um es in den Anruf anzugeben. Der vorherige Anruf wird davon ausgegangen, dass der Clientcomputer dieselbe Domäne Mitglied ist und der Anrufer Berechtigungen für die Datenbank mithilfe der Windows-Authentifizierung gewährt wurde.

Beim Testen der Zuhörer unbedingt tritt ein Fehler auf der Verfügbarkeit Gruppe, um sicherzustellen, dass Clients über Failovers mit dem Zuhörer herstellen können.