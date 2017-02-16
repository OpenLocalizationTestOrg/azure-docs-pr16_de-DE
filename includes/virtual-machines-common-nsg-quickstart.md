Sie einen Port öffnen oder erstellen einen Endpunkt, mit einem virtuellen Computer (virtueller Computer) in Azure durch Erstellen eines Filters Netzwerk in einem Subnetz oder virtueller Computer-Schnittstelle. Platzieren Sie diese Filter für eingehenden und ausgehenden Datenverkehr zu steuern, auf einer Netzwerk-Sicherheitsgruppe angefügt, die der Ressource, die den Datenverkehr empfängt.

Verwenden Sie uns ein gängiges Beispiel der Web-Verkehr auf Port 80 ein. Nachdem Sie einen virtuellen Computer, die so konfiguriert ist haben, dass dienen port Webanfragen auf den standard TCP 80 (Denken Sie daran, um die entsprechenden Dienste und Öffnen des virtuellen Computers als auch alle OS Firewall-Regeln), Sie:

1. Erstellen Sie eine Sicherheitsgruppe Netzwerk ein.
2. Erstellen Sie eine eingehende Regel zulassen des Datenverkehrs mit:
  - Zielbereich Port "80"
  - Port Quellbereich von "*" (alle Quellport gleicht)
  - Prioritätswert kleiner 65,500 (auf Verweigern werden höher in Priorität als den standardmäßigen alle erfassen eingehende Regel)
3. Ordnen der Netzwerk-Sicherheitsgruppe virtuellen Computer oder die Subnetz gehören.
    
Sie können komplexe Netzwerkkonfigurationen zum Sichern Ihrer Umgebung mit Sicherheitsgruppen Netzwerk und Regeln erstellen. Beispiel verwendet nur eine oder zwei Regeln, die HTTP-Verkehr oder remote-Verwaltung zu ermöglichen. Weitere Informationen finden Sie im Abschnitt [' Weitere Informationen '](#more-information-on-network-security-groups) oder [Neuigkeiten einer Sicherheitsgruppe Netzwerk?](../articles/virtual-network/virtual-networks-nsg.md)