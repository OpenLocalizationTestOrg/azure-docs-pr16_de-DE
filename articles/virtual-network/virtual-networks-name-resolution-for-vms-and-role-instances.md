<properties 
   pageTitle="Mit einer Auflösung von virtuellen Computern und Rolleninstanzen"
   description="Benennen Sie die Auflösung Szenarien für Azure IaaS, Hybrid Lösungen, zwischen verschiedenen Cloud Services, Active Directory und verwenden Ihre eigenen DNS-server "
   services="virtual-network"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="telmos" />

# <a name="name-resolution-for-vms-and-role-instances"></a>Mit einer namensauflösung von für virtuellen Computern und Rolleninstanzen

Je nachdem, wie Sie Azure hosten IaaS, PaaS und Hybrid Lösungen verwenden müssen Sie die virtuellen Computern und Rolleninstanzen, die Sie erstellen, um kommunizieren dürfen. Auch wenn diese Kommunikation mithilfe von IP-Adressen vorgenommen werden kann, ist es viel einfacher Namen zu verwenden, die können einfach gedacht und kann nicht ändern. 

Wenn Rolleninstanzen und virtuellen Computern in Azure gehostet Domänennamen in internen IP-Adressen auflösen müssen, können sie eine der beiden Methoden verwenden:

- [Mit einer Auflösung von Azure bereitgestellten Namen](#azure-provided-name-resolution)

- [Verwenden Ihre eigenen DNS-Server mit einer Auflösung von Namen](#name-resolution-using-your-own-dns-server) (die möglicherweise Abfragen an die Azure bereitgestellten DNS-Server weiterleiten) 

Der Typ des namensauflösung, die Sie verwenden, hängt davon ab, wie Ihre virtuellen Computern und Rolleninstanzen kommunizieren müssen.

**Die folgende Tabelle zeigt die Szenarien und den entsprechenden Namen mit einer Auflösung von Lösungen:**

| **Szenario** | **Lösung** | **Suffix** |
|--------------|--------------|----------|
| Mit einer namensauflösung von zwischen Rolleninstanzen oder virtuellen Computern, die sich in der gleichen Cloud-Dienst oder virtuelles Netzwerk befindet | [Mit einer Auflösung von Azure bereitgestellten Namen](#azure-provided-name-resolution)| Hostname oder FQDN |
| Mit einer namensauflösung von zwischen Rolleninstanzen oder virtuelle Computer befindet sich in verschiedenen virtuellen Netzwerken | Kunden verwaltete DNS-Weiterleitung Abfragen zwischen Vnets für die Auflösung von Azure (DNS-Proxy).  finden Sie unter [mit einer Auflösung von Namen mithilfe Ihrer eigenen DNS-server](#name-resolution-using-your-own-dns-server)| Nur FQDN |
| Lokaler Computer und Service-Namen aus Rolleninstanzen oder virtuellen Computern in Azure Auflösung | Kunden verwaltet DNS-Server (z. B. lokal Domänencontroller, lokalen schreibgeschützt Domänencontroller oder einer sekundären DNS synchronisiert mit einer Übertragung der Zone).  Finden Sie unter [mit einer Auflösung von Namen mithilfe Ihrer eigenen DNS-server](#name-resolution-using-your-own-dns-server)|Nur FQDN |
| Mit einer Auflösung von Azure Hostnamen aus lokalen Computern | Weiterleitung von Abfragen an einen Kunden verwaltete DNS-Proxyserver in den entsprechenden Vnet, leitet der Proxyserver Abfragen in Azure für Auflösung. Finden Sie unter [mit einer Auflösung von Namen mithilfe Ihrer eigenen DNS-server](#name-resolution-using-your-own-dns-server)| Nur FQDN |
| Reverse-DNS-für internen IP-Adressen | [Verwenden Ihre eigenen DNS-Server mit einer namensauflösung von](#name-resolution-using-your-own-dns-server) | n/v |
| Zwischen virtuellen Computern oder Rolleninstanzen befindet sich unter anderen Cloud-Dienst in ein virtuelles Netzwerk nicht mit einer namensauflösung von| Nicht verfügbar. Konnektivität zwischen virtuellen Computern und Rolleninstanzen in anderen Cloud Services wird außerhalb eines virtuellen Netzwerks nicht unterstützt.| n/v |



## <a name="azure-provided-name-resolution"></a>Mit einer Auflösung von Azure bereitgestellten Namen

Zusammen mit einer Auflösung von öffentlichen DNS-Namen bietet Azure mit einer Auflösung von internen Namen für virtuellen Computern und Rolleninstanzen, die innerhalb der gleichen virtuellen Netzwerk oder Cloud-Dienst befinden.  In einen Cloud-Service-virtuellen Computern/Instanzen freigeben das gleiche DNS-Suffix (, damit die Hostname alleine ausreichend ist), aber klassischen virtuelle Netzwerke anderen Cloud Services haben verschiedene DNS-Suffixe, damit der FQDN zum Auflösen von Namen zwischen verschiedenen Cloud Services erforderlich ist.  In virtuelle Netzwerke im Bereitstellungsmodell Ressourcenmanager das DNS-Suffix konsistent im virtuellen Netzwerk (, damit Sie der vollqualifizierten Domänennamen nicht mehr benötigt wird) und DNS-Namen können sowohl NICs und virtuellen Computern zugewiesen werden. Obwohl Azure bereitgestellten namensauflösung keine Konfiguration erforderlich ist, ist es nicht die entsprechende Option für die in allen Bereitstellungsszenarien wie in der Tabelle oben angezeigt.

> [AZURE.NOTE] Wenn Web- und Worker Rollen können Sie auch die internen IP-Adressen der Rolleninstanzen auf der Grundlage der Rolle Namen und die Instanz Anzahl mithilfe der Azure Service Management REST-API zugreifen. Weitere Informationen finden Sie unter [Service Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/ee460799.aspx).

### <a name="features-and-considerations"></a>Features und Aspekte

**Features:**

- Center für erleichterte Bedienung: ist keine Konfiguration erforderlich, damit Sie mit einer Auflösung von Azure bereitgestellten Namen verwenden können.

- Dienst mit einer Auflösung von Azure bereitgestellten Namen ist hoch verfügbar, speichern Sie die müssen Cluster Ihrer eigenen DNS-Server erstellen und verwalten.

- Kann in Verbindung mit Ihrer eigenen DNS-Server verwendet werden, um lokal und Azure Hostnamen aufzulösen.

- Mit einer namensauflösung von werden zwischen Rolle Instanzen/virtuellen Computern innerhalb derselben Cloud-Dienst ohne Notwendigkeit einen vollqualifizierten Domänennamen bereitgestellt.

- Mit einer namensauflösung von werden zwischen virtuellen Computern in virtuelle Netzwerke bereitgestellt, die das Modell Ressourcenmanager zur Bereitstellung, ohne Notwendigkeit den vollqualifizierten Domänennamen verwenden. Virtuelle Netzwerke im Bereitstellungsmodell klassischen erfordern den vollqualifizierten Domänennamen beim Auflösen von Namen in anderen Cloud-Dienste. 

- Hostnamen, die Bereitstellung Ihrer am besten beschreiben können anstatt arbeiten mit Namen automatisch generiert.

**Aspekte:**

- Das Azure erstellte DNS-Suffix kann nicht geändert werden.

- Sie können eigene Einträge nicht manuell registrieren.

- WINS- und NetBIOS werden nicht unterstützt. (Die virtuellen Computern in Windows-Explorer nicht angezeigt werden.)

- Hostnamen muss DNS-kompatible (müssen sie nur 0-9, a-Z verwenden und '-', und kann nicht mit beginnen oder enden eines ' – '. Siehe RFC 3696 Abschnitt 2.)

- DNS-Abfragedatenverkehr für jeden virtuellen Computer beschränkt. Dies sollte nicht die meisten Applikationen auswirken.  Sicherstellen Sie Anforderung begrenzungsebene beobachtet wird, dass clientseitige Zwischenspeichern aktiviert ist.  Weitere Informationen hierzu finden Sie unter [optimale Nutzung von Azure bereitgestellten namensauflösung](#Getting-the-most-from-Azure-provided-name-resolution).

- Virtuellen Computern nur in der ersten 180 Cloud Services sind für jedes virtuelle Netzwerk in einem Bereitstellungsmodell klassischen registriert. Dies gilt nicht für virtuelle Netzwerke in Ressourcenmanager Bereitstellungsmodellen.


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Die optimale Nutzung Azure bereitgestellten namensauflösung
**Clientseitige Zwischenspeichern:**

Nicht alle DNS-Abfrage muss im Netzwerk gesendet werden.  Clientseitige Zwischenspeichern hilft Wartezeit reduzieren und Stabilität bei Network ahnen verbessern, indem Sie die Auflösung von periodischen DNS-Abfragen aus einem lokalen Cache.  DNS-Einträge enthalten eine Time To Live (TTL) der können den Cache zum Speichern von des Eintrags so lange beeinträchtigt Aktualität aufzeichnen, damit clientseitige Zwischenspeichern in den meisten Fällen geeignet ist.

Standardmäßig Windows DNS-Client hat einen integrierten DNS-Cache.  Einige Linux Distros Zwischenspeichern standardmäßig nicht enthalten ist, wird empfohlen, dass eine für jede Linux VM (nach dem Überprüfen, dass es noch kein lokaler Cache) hinzugefügt werden.

Es gibt eine Reihe von anderen DNS-Einträge zwischenspeichern Pakete verfügbar, z. B. Dnsmasq, hier werden die Schritte zur Installation von Dnsmasq auf die am häufigsten verwendeten Distros:

- **Ubuntu (verwendet Resolvconf)**:
    - Installieren Sie nur das Paket Dnsmasq ("Sudo apt-Get-Installation Dnsmasq").
- **SUSE (verwendet Netconf)**:
    - Installieren Sie das Paket Dnsmasq ("Sudo Zypper Installation Dnsmasq") 
    - Aktivieren Sie den Dienst Dnsmasq ("Systemctl aktivieren dnsmasq.service") 
    - Starten Sie den Dienst Dnsmasq ("Systemctl Start dnsmasq.service") 
    - Bearbeiten "/ usw./Sysconfig/Network/Config", und ändern Sie NETCONFIG_DNS_FORWARDER = "" auf "Dnsmasq"
    - Aktualisieren von resolv.conf ("Netconfig Update"), um den Cache festlegen als die lokale DNS-Auflösung
- **OpenLogic (NetworkManager verwendet)**:
    - Installieren Sie das Paket Dnsmasq ("Sudo Yum installieren Dnsmasq")
    - Aktivieren Sie den Dienst Dnsmasq ("Systemctl aktivieren dnsmasq.service")
    - Starten Sie den Dienst Dnsmasq ("Systemctl Start dnsmasq.service")
    - Hinzufügen von "vorangestellt Domain Name Servers 127.0.0.1;", "/etc/dhclient-eth0.conf"
    - Starten Sie den Netzwerkdienst ("Service Netzwerk Neustart"), um den Cache als die lokale DNS-Auflösung festlegen

> [AZURE.NOTE] Das Paket 'Dnsmasq' ist nur eine der vielen DNS-Caches für Linux verfügbar.  Bevor Sie es verwenden, überprüfen Sie die Verwendbarkeit für Ihre Bedürfnisse und, die keine anderen Cache installiert ist.

**Clientseitige Wiederholungsversuche:**

DNS ist hauptsächlich ein UDP-Protokoll.  Wie das UDP-Protokoll Nachrichtenübermittlung sichergestellt ist nicht, wird in der DNS-Protokoll selbst Logik Wiederholungsversuche behandelt.  Jeder DNS-Client (Betriebssystem) kann ein Logik für verschiedene Wiederholungsversuche je nach der Einstellung für Ersteller aufweisen:

 - Windows-Betriebssysteme Wiederholung nach 1 zweites, und klicken Sie dann erneut nach einem anderen 2, 4 und einen anderen 4 Sekunden. 
 - Die standardmäßigen Linux Setup Wiederholungsversuche nach 5 Sekunden.  Es wird empfohlen, diese auf 1 zweiten Intervallen 5 Mal wiederholen ändern.  

So überprüfen Sie die aktuellen Einstellungen auf einer Linux VM, 'Katze /etc/resolv.conf', und achten Sie auf die Zeile 'Optionen', z. B.:

    options timeout:1 attempts:5

Die Datei resolv.conf ist in der Regel automatisch generiert und dürfen nicht bearbeitet werden.  Die einzelnen Schritte für das Hinzufügen der Zeile 'Optionen' variieren je nach Distro:

- **Ubuntu** (verwendet Resolvconf):
    - Fügen Sie die Optionen Linie ' / etc/resolveconf/resolv.conf.d/head' 
    - Führen Sie 'Resolvconf -u' aktualisieren
- **SUSE** (verwendet Netconf):
    - Hinzufügen von 'Timeout:1 Versuche: 5' in der NETCONFIG_DNS_RESOLVER_OPTIONS = "" Parameter in '/ usw./Sysconfig/Network/Config' 
    - Ausführen von 'Netconfig Update' aktualisieren
- **OpenLogic** (verwendet NetworkManager):
    - 'Echo "Optionen Timeout:1 Versuche: 5" ' hinzufügen ' / etc/NetworkManager/dispatcher.d/11-dhclient' 
    - Ausführen 'Netzwerk Restart-service' aktualisieren

## <a name="name-resolution-using-your-own-dns-server"></a>Verwenden Ihre eigenen DNS-Server mit einer namensauflösung von
Es gibt eine Reihe von Situationen, wobei Indexeigenschaften mit einer Auflösung von Namen über die Funktionen von Azure, beispielsweise wenn Active Directory-Domänen wechseln Sie möglicherweise verwenden, oder wenn Sie mit einer Auflösung von DNS-zwischen virtuelle Netzwerke (Vnets) erforderlich ist.  Um diese Szenarios verwischen, bietet Azure die Möglichkeit, damit Sie Ihre eigenen DNS-Server verwenden.  

DNS-Server in einem virtuellen Netzwerk können DNS-Abfragen an den Azure rekursive Server Problembehebung Hostnamen innerhalb dieses virtuellen Netzwerks weiterleiten.  Beispielsweise können eine Ausführung in Azure Domain Controller (DC) Antworten auf DNS-Abfragen für ihre Domänen und alle anderen Abfragen an Azure weiterleiten.  Dadurch wird die virtuelle Computer Ihre lokal Ressourcen (über dem DC) und die bereitgestellten Azure Hostnamen (über die Weiterleitung) sehen.  Zugriff auf den Azure rekursive Server wird über die virtuelle IP-Adresse 168.63.129.16 bereitgestellt.

DNS-Weiterleitung auch ermöglicht zwischen Vnet DNS-Auflösung und Ihre Computer lokal bereitgestellte Azure Hostnamen auflösen.  Um einen virtuellen Computers Hostname zu beheben, der DNS-Server virtueller Computer muss sich in der gleichen virtuellen Netzwerk befinden und für Vorwärts Hostname Abfragen an Azure konfiguriert werden.  Wie das DNS-Suffix in jeder Vnet abweicht, können Sie bedingte Weiterleitungsregeln, DNS-Abfragen an die richtige Vnet für Auflösung senden.  Die folgende Abbildung zeigt zwei Vnets und ein lokal Netzwerk zwischen Vnet DNS-Auflösung verwenden diese Methode ausführen.  Eine Beispiel für DNS-Weiterleitung ist im [Katalog Azure Schnellstart Vorlagen](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) und [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder)zur Verfügung.

![Zwischen Vnet DNS-Einträge](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Bei Verwendung von Azure bereitgestellten Namen, mit einer Auflösung von einer internen DNS-Suffix (*. internal.cloudapp.net) für jeden virtuellen Computer mithilfe von DHCP bereitgestellt.  Hostname Auflösung wird dadurch, wie die Einträge Hostname in der Zone internal.cloudapp.net sind.  Wenn Sie Ihre eigenen Namen mit einer Auflösung von Lösung verwenden zu können, wird das Suffix IDNS nicht auf virtuellen Computern angegeben werden, da es mit anderen DNS-Architekturen (wie Domänenverbund Szenarien) beeinträchtigt.  Stattdessen bieten wir ein Platzhalters nicht funktioniert (reddog.microsoft.com).  

Falls erforderlich, kann das internen DNS-Suffix mithilfe der PowerShell oder der API ermittelt werden:

-  Für virtuelle Netzwerke in Ressourcenmanager Bereitstellungsmodellen steht das Suffix über die Ressource [Netzwerk Benutzeroberflächen-Karte](https://msdn.microsoft.com/library/azure/mt163668.aspx) oder über das Cmdlet " [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) ".    
-  In klassischen Bereitstellungsmodellen, das Suffix steht über den Anruf [Erhalten Deployment API](https://msdn.microsoft.com/library/azure/ee460804.aspx) oder über die [Get-AzureVM-Debuggen](https://msdn.microsoft.com/library/azure/dn495236.aspx) Cmdlet.


Wenn die Weiterleitung von Abfragen in Azure nicht Ihren Anforderungen entsprechend, müssen Sie Ihre eigenen DNS-Lösung bereitzustellen.  Ihre DNS-Lösung müssen:

-  Eine entsprechende Hostname-Auflösung, z.B. über [DDNS](virtual-networks-name-resolution-ddns.md).  Notiz, wenn DDNS verwenden, müssen Sie möglicherweise Deaktivieren des DNS-Eintrag Managers während des Azure DHCP-Leases sehr lange und Aufräumen sind, möglicherweise DNS-Einträge vorzeitig entfernen. 
-  Bieten Sie entsprechende rekursive Auflösung an, damit mit einer Auflösung von externen Domänennamen ein.
-  Zugänglich (TCP und UDP auf Port 53) von Clients, die sie dient und auf das Internet zugreifen.
-  Vor Zugriff geschützt werden, aus dem Internet, zum Einschränken von externen Agents ausgehenden dieser Risiken.

> [AZURE.NOTE] Zur Optimierung der Systemleistung bei Verwendung Azure-virtuellen Computern als DNS-Server IPv6 deaktiviert werden sollen, und eine [Instanz Ebene öffentliche IP-Adresse](virtual-networks-instance-level-public-ip.md) den einzelnen DNS-Server virtueller Computer zugewiesen werden sollen.  Wenn Sie Windows Server als Ihr DNS-Server verwenden, stehen [in diesem Artikel](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx) zusätzliche Leistungsanalyse und Optimierungen.


### <a name="specifying-dns-servers"></a>Festlegen von DNS-Servern

Wenn Sie Ihre eigenen DNS-Server verwenden zu können, bietet Azure die Möglichkeit zum Angeben mehrerer DNS-Server pro virtuelles Netzwerk oder Netzwerk-Benutzeroberfläche (Ressourcenmanager) / cloud-Dienst (klassisch).  DNS-Server für eine Cloud-Service-Network-Benutzeroberfläche angegebenen erhalten Vorrang gegenüber den für das virtuelle Netzwerk angegeben.

> [AZURE.NOTE] Verbindungseigenschaften Netzwerk, wie DNS-Server IP-Adressen, dürfen nicht direkt in die Windows-virtuellen Computern bearbeitet werden, wie sie während der Dienst gelöscht erhalten möglicherweise lösen, wenn der virtuelle Netzwerkadapter ersetzt wird. 


Wenn Sie das Modell zur Bereitstellung von Ressourcenmanager verwenden, können DNS-Server im Portal API/Vorlagen ([Vnet](https://msdn.microsoft.com/library/azure/mt163661.aspx), [Nic](https://msdn.microsoft.com/library/azure/mt163668.aspx)) oder PowerShell ([Vnet](https://msdn.microsoft.com/library/mt603657.aspx), [Nic](https://msdn.microsoft.com/library/mt619370.aspx)) angegeben werden muss.

Wenn Sie das Bereitstellungsmodell klassischen verwenden, können DNS-Server für das virtuelle Netzwerk im Portal oder [im Datei *-Netzwerkkonfiguration* ](https://msdn.microsoft.com/library/azure/jj157100)angegeben werden muss.  Für Clouddienste werden die DNS-Server über [die *Dienstkonfiguration* -Datei](https://msdn.microsoft.com/library/azure/ee758710) oder in PowerShell ([Neu-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)) angegeben.

> [AZURE.NOTE] Wenn Sie die DNS-Einstellungen für einen virtuellen Netzwerk/virtuelle Computer, die bereits bereitgestellt ist ändern, müssen Sie jedes betroffenen virtuellen Computer für die Änderungen wirksam werden neu starten.


## <a name="next-steps"></a>Nächste Schritte

Ressourcenmanager Modell zur Bereitstellung von:

- [Erstellen oder Aktualisieren eines virtuellen Netzwerks](https://msdn.microsoft.com/library/azure/mt163661.aspx)
- [Erstellen oder Aktualisieren von Network Interface card](https://msdn.microsoft.com/library/azure/mt163668.aspx)
- [Neue AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx)
- [Neue AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx)

 
Klassische Bereitstellungsmodell:

- [Die Konfigurationsschema Azure Service](https://msdn.microsoft.com/library/azure/ee758710)
- [Virtuelle Netzwerk-Konfigurationsschema](https://msdn.microsoft.com/library/azure/jj157100)
- [Konfigurieren eines virtuellen Netzwerks mithilfe einer Netzwerk-Konfigurationsdatei](virtual-networks-using-network-configuration-file.md) 

