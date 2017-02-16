## <a name="virtual-network-basics"></a>Virtuelle Netzwerk-Grundlagen

### <a name="what-is-an-azure-virtual-network-vnet"></a>Was ist ein Azure virtuelles Netzwerk (VNet)?

Sie können VNets bereitstellen verwenden virtuelle private Netzwerke (VPN) in Azure und zu verwalten, optional Verknüpfen der VNets mit anderen VNets in Azure oder mit Ihrem lokalen IT-Infrastruktur Hybrid oder übergreifend lokale Lösungen zu erstellen. Jede VNet erstellten verfügt über eine eigene CIDR blockieren, und in anderen Netzwerken VNets und lokale verknüpft werden, solange die CIDR-Blocks keine Konflikte erzeugen, gehen Sie wie folgt. Sie können auch DNS Server Settings für VNets-Steuerelemente und Segmentierung der VNet in Subnetzen.

Verwenden Sie VNets an:

- Erstellen Sie ein dediziertes privates Cloud nur virtuelles Netzwerk

    Manchmal ist keine Sie für Ihre Lösung eine Cross lokale Konfiguration erforderlich. Wenn Sie eine VNet erstellen, können Ihrer Dienste und virtuellen Computern innerhalb der VNet direkt und sicher in der Cloud kommunizieren. Dies hält Datenverkehr sicher innerhalb der VNet, aber weiterhin ermöglicht es Ihnen, die Endpunkt-Verbindungen für die virtuellen Computern und Dienste, die Internet-Kommunikation als Teil der Lösung erfordern konfigurieren.

- Sichere erweitern Sie Ihrer Data center

    Mit VNets können Sie herkömmliche Standort-zu-Standort (S2S) VPN, um Ihre Datenzentrumskapazität sicher skalieren erstellen. S2S VPN verwenden IPSEC, um eine sichere Verbindung zwischen Ihrem corporate VPN-Gateway und Azure bereitzustellen.

- Aktivieren der Hybriden Cloud-Szenarien

    VNets bieten Ihnen die Flexibilität, einen Zellbereich Hybriden Cloudszenarien unterstützen. Sie können sicher cloudbasierten Applikationen auf einen beliebigen lokalen System wie Mainframecomputer und Unix-Systemen herstellen.

### <a name="how-do-i-know-if-i-need-a-virtual-network"></a>Wie feststellen kann ich, ob ich ein virtuelles Netzwerk benötigen?

Besuchen Sie die [Virtuelle Network (Übersicht)](../articles/virtual-network/virtual-networks-overview.md) , um eine Entscheidungstabelle, mit denen Sie die beste Option Netzwerk Entwurf für Sie entscheiden, finden Sie unter.

### <a name="how-do-i-get-started"></a>Wie Schritte ich?

Besuchen Sie [die virtuelle Netzwerk-Dokumentation](https://azure.microsoft.com/documentation/services/virtual-network/) zur Seite Erste Schritte. Diese Seite enthält Links zu häufig verwendete Konfigurationsschritte sowie Informationen, die Ihnen hilft, die Dinge zu verstehen, die Sie beim Entwerfen von Ihrem Netzwerks virtuellen berücksichtigen müssen.

### <a name="what-services-can-i-use-with-vnets"></a>Verwenden auf welche Dienste kann ich mit VNets?

VNets können mit einer Vielzahl von verschiedenen Azure-Diensten, wie z. B. Cloud Services (PaaS), virtuellen Computern und Web Apps verwendet werden. Es gibt jedoch einige Dienste, die auf einer VNet nicht unterstützt werden. Überprüfen Sie den bestimmten Dienst zu verwenden, und stellen Sie sicher, dass es kompatibel ist.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Kann ich VNets ohne Cross lokale Verbindung werden verwendet?

Ja. Sie können einer VNet ohne Standorten Konnektivität verwenden. Dies ist besonders hilfreich, wenn die Domänencontroller und SharePoint-Farmen in Azure ausgeführt werden soll.

## <a name="virtual-network-configuration"></a>Virtuelle Netzwerkkonfiguration

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Welche Tools werden verwendet, um eine VNet zu erstellen?

Sie können die folgenden Tools verwenden, erstellen oder Konfigurieren eines virtuellen Netzwerks:

- Azure-Portal (für Classic und VNets Ressourcenmanager).

- Eine Netzwerk-Konfigurationsdatei (Netcfg – für nur klassische VNets). Finden Sie unter [Konfigurieren eines virtuellen Netzwerks mit einer Konfigurationsdatei Netzwerk](../articles/virtual-network/virtual-networks-using-network-configuration-file.md).

- PowerShell (für Classic und VNets Ressourcenmanager).

- Azure CLI (für Classic und VNets Ressourcenmanager).

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Welche Adressbereiche kann ich in meinem VNets verwenden?

Sie können öffentliche IP-Adressbereiche und einen beliebigen IP-Adressbereich in [RFC 1918](http://tools.ietf.org/html/rfc1918)definiert.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Kann ich in meinem VNets öffentliche IP-Adressen verwenden?

Ja. Weitere Informationen zu öffentlichen IP-Adressbereiche finden Sie unter [öffentliche IP-Adresse Adresse Leerzeichen in einem virtuellen Netzwerk (VNet)](../articles/virtual-network/virtual-networks-public-ip-within-vnet.md). Beachten Sie, dass Ihre öffentliche IP-Adressen nicht direkt aus dem Internet zugänglich sind beibehalten.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-virtual-network"></a>Gibt es eine Beschränkung der Anzahl der Subnetze in meinem virtuelle Netzwerk?

Es gibt keine Beschränkung für die Anzahl der Subnetze, die Sie innerhalb einer VNet verwenden. Alle Subnets des müssen innerhalb des virtuellen Netzwerk Adresse vollständig enthalten sein und sollten nicht miteinander überlappen.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Gibt es Einschränkungen auf IP-Adressen in diesen Subnetzen verwenden?

Azure reserviert einige IP-Adressen in jedem Subnetz. Die vor- und Nachnamen IP-Adressen der Subnetze sind für Protokoll-Konformität mit 3 weitere Adressen für Azure Dienste verwendet reserviert.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Wie können kleine und wie große VNets und Subnetze werden?

Das kleinste Subnetz, die, das wir Unterstützung, ist eine /29 und der größte ist eine /8 (mit CIDR Subnetzdefinitionen).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Kann ich meine VLANs an Azure bringen VNets verwenden?

Nein. VNets sind Layer-3 überlagert an. Eine beliebige Semantik Layer-2 unterstützt Azure nicht.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Kann ich auf meinem VNets und Subnetze benutzerdefinierte routing Richtlinien angeben?

Ja. Sie können die Benutzer definiert Routing (UDR) verwenden. Weitere Informationen zu UDR finden Sie auf [Benutzer definiert ist und die IP-Weiterleitung](../articles/virtual-network/virtual-networks-udr-overview.md).

### <a name="do-vnets-support-multicast-or-broadcast"></a>Unterstützt gehen Sie wie folgt VNets Multicast- oder übertragenen?

Nein. Wir unterstützen keine Multicast- oder übertragenen.

### <a name="what-protocols-can-i-use-within-vnets"></a>Welche Protokolle können in VNets werden verwendet?

Standard-IP-basierte Protokolle innerhalb VNets können. Multicast, übertragenen, IP-IP eingekapselte Pakete und Pakete generische Routing Encapsulation (GRE) werden jedoch in VNets blockiert. Standard-Protokolle, die Arbeit enthalten:

- TCP
- UDP
- ICMP

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Kann ich meine Standardrouter innerhalb einer VNet anpingen?

Nein.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Kann ich mithilfe von Tracert Connectivity diagnostizieren?

Nein.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Kann ich nach dem Erstellen der VNet Subnetze hinzuzufügen?

Ja. Subnetze können zu VNets zu einem beliebigen Zeitpunkt hinzugefügt werden, solange die Subnetzadresse nicht Teil einer anderen Subnetz in der VNet ist.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Kann ich ändern die Größe des Meine Subnetz, nachdem ich sie erstellt haben?

Sie können hinzufügen, entfernen, erweitern oder einem Subnetz verkleinern, wenn es sind keine virtuellen Computern oder Dienste darin mithilfe der PowerShell-Cmdlets oder die Datei NETCFG bereitgestellt. Sie können auch hinzufügen, entfernen, erweitern oder keine Präfixe verkleinern, solange die Subnetze, die virtuellen Computern oder Dienste enthalten sind von der Änderung nicht betroffen.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Kann ich Subnetze ändern, nachdem ich sie erstellt hat?

Ja. Sie können hinzufügen, entfernen und ändern die CIDR Blöcke von einer VNet verwendet.

### <a name="can-i-connect-to-the-internet-if-i-am-running-my-services-in-a-vnet"></a>Kann ich mit dem Internet verbinden, wenn ich meine Dienste in einer VNet verwende?

Ja. Alle Dienste, die innerhalb einer VNet bereitgestellt können mit dem Internet verbinden. Jeder Cloud-Dienst bereitgestellt in Azure verfügt über eine öffentlich adressiert VIP zugewiesen. Sie müssen zum Definieren von Endpunkten für PaaS Rollen und Endpunkten für virtuellen Computern zum Aktivieren dieser Dienste Verbindungen aus dem Internet annehmen.

### <a name="do-vnets-support-ipv6"></a>Unterstützen VNets IPv6?

Nein. Sie können keine IPv6 mit VNets zu diesem Zeitpunkt verwenden.

### <a name="can-a-vnet-span-regions"></a>Kann eine VNet Regionen umfassen?

Nein. Eine VNet ist auf eine einzelne Region beschränkt.

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Kann ich eine VNet mit einer anderen VNet in Azure verbinden?

Ja. Sie können VNet zu VNet Kommunikation mithilfe von REST-APIs oder Windows PowerShell erstellen. Sie können auch VNets über VNet Peering verbinden. Anzeigen von mehr Details zur peering [hier.](../articles/virtual-network/virtual-network-peering-overview.md)

## <a name="name-resolution-dns"></a>Mit einer namensauflösung von (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Was sind meine DNS-Optionen für VNets?

Entscheiden Sie anhand der Tabelle auf der Seite [Mit einer Auflösung von Namen für virtuellen Computern und Rolleninstanzen](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) zu Sie schrittweise durch alle verfügbaren DNS-Optionen.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Kann ich die DNS-Server für eine VNet angeben?

Ja. Sie können DNS-Server IP-Adressen in den Einstellungen VNet angeben. Dies wird als die Standard-DNS-Server für alle virtuellen Computern in der VNet angewendet.

### <a name="how-many-dns-servers-can-i-specify"></a>Wie viele DNS-Server kann ich angeben?

Sie können bis zu 12 DNSServers angeben.

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Kann ich meine DNS-Server ändern, nachdem ich das Netzwerk erstellt haben?

Ja. Sie können die Liste der DNS-Server für Ihre VNet zu einem beliebigen Zeitpunkt ändern. Wenn Sie die Liste der DNS-Server ändern, müssen Sie jede der virtuellen Computern in Ihrer VNet in Reihenfolge nach ihnen zu den neuen DNS-Server neu zu starten.


### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Was ist Azure bereitgestellte DNS und mit VNets arbeiten?

Azure bereitgestellte DNS ist eine mit mehreren Mandanten DNS-Dienstleistung von Microsoft. Azure registriert alle virtuellen Computern und Rolleninstanzen in diesem Dienst. Dieser Service bietet namensauflösung Hostname für virtuellen Computern und Rolleninstanzen innerhalb derselben Cloud-Dienst und über den vollqualifizierten Domänennamen für virtuellen Computern und Rolleninstanzen in der gleichen VNet an.

> [AZURE.NOTE] Es ist eine Einschränkung zu diesem Zeitpunkt auf die ersten 100 Clouddienste in das virtuelle Netzwerk für Cross-Mandanten namensauflösung mit Azure bereitgestellte DNS ein. Diese Einschränkung gilt nicht, wenn Sie Ihre eigenen DNS-Server verwenden sind.

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>Kann ich setzen Meine DNS-Einstellungen eines pro virtuellen Computers außer Kraft / service Basis?

Ja. Sie können auf Basis pro Cloud-Dienst zum Außerkraftsetzen von Standardeinstellungen Netzwerk DNS-Server festlegen. Wir empfehlen jedoch Sie Netzwerk-Wide DNS so weit wie möglich verwenden.

### <a name="can-i-bring-my-own-dns-suffix"></a>Kann ich meinen eigenen DNS-Suffix bringen?

Nein. Sie können keine benutzerdefinierten DNS-Suffix für Ihre VNets angeben.

## <a name="vnets-and-vms"></a>VNets und virtuellen Computern

### <a name="can-i-deploy-vms-to-a-vnet"></a>Kann ich eine VNet virtuellen Computern bereitstellen?

Ja.

### <a name="can-i-deploy-linux-vms-to-a-vnet"></a>Kann ich eine VNet Linux virtuellen Computern bereitstellen?

Ja. Sie können eine beliebige Distro Linux von Azure unterstützt bereitstellen.

### <a name="what-is-the-difference-between-a-public-vip-and-an-internal-ip-address"></a>Was ist der Unterschied zwischen einer öffentlichen VIP und eine interne IP-Adresse?

- Eine interne IP-Adresse ist eine IP-Adresse, die einzelnen virtuellen Computer in einem VNet von DHCP zugewiesen ist. Es ist nicht public-Facing. Wenn Sie eine VNet erstellt haben, wird die interne IP-Adresse aus dem Bereich zugewiesen, die Sie in den Einstellungen Subnetz Ihrer VNet angegeben haben. Wenn Sie nicht über eine VNet verfügen, wird eine interne IP-Adresse dennoch zugewiesen. Die interne IP-Adresse bleibt mit dem virtuellen Computer für Lebensdauer, es sei denn, die virtuellen Computer freigegeben ist.

- Eine öffentliche VIP ist die öffentliche IP-Adresse, die in der Cloud-Dienst oder laden Lastenausgleich zugeordnet ist. Es ist nicht direkt an Ihre VM NIC. zugewiesen. Die VIP bleibt mit der Cloud-Dienst, den sie, bis alle virtuellen Computern zugewiesen wurde, als Cloud-Dienst freigegeben oder gelöscht werden. An diesem Punkt, wird er freigegeben.

### <a name="what-ip-address-will-my-vm-receive"></a>Welche IP-Adresse, meine virtueller Computer erhalten?

- **Interne IP-Adresse –** Wenn Sie eine VNet ein virtuellen Computers bereitstellen, erhält der virtuellen Computer eine interne IP-Adresse aus einem Pool von internen IP-Adressen, die Sie angeben. Kommunizieren Sie virtuellen Computern innerhalb der VNets mithilfe von internen IP-Adressen. Obwohl Azure eine dynamische interne IP-Adresse zuweist, können Sie eine statische Adresse für Ihre virtuellen Computer anfordern. Weitere Informationen zum statischen internen IP-Adressen finden Sie auf [wie eine statische internen IP-Adresse festgelegt](../articles/virtual-network/virtual-networks-reserved-private-ip.md).

- **VIP-** Ihre virtuellen Computer ist auch eine VIP, zugeordnet, obwohl eine VIP nie direkt auf den virtuellen Computer zugewiesen ist. Eine VIP ist eine öffentliche IP-Adresse, die in der Cloud-Service zugeordnet werden kann. Sie können, klicken Sie optional eine VIP für Ihre Cloud-Dienst reservieren.

- **ILPIP-** Sie können auch eine Instanz Ebene öffentliche IP-Adresse (ILPIP) konfigurieren. ILPIPs sind direkt mit dem virtuellen Computer, statt die Cloud-Dienst verbunden sind. Weitere Informationen zum ILPIPs finden Sie auf [Instanz Ebene öffentlichen IP-Übersicht](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).

### <a name="can-i-reserve-an-internal-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Kann ich eine interne IP-Adresse für einen virtuellen reservieren, die ich zu einem späteren Zeitpunkt erstellen?

Nein. Sie können keine interne IP-Adresse reservieren. Wenn eine interne IP-Adresse zur Verfügung steht wird es eine Instanz virtuellen Computers oder Rolle der DHCP-Server zugewiesen werden. Diese virtuellen Computers möglicherweise oder möglicherweise nicht die gewünschte die interne IP-Adresse zugewiesen werden soll. Sie können jedoch die interne IP-Adresse von einem bereits erstellten virtuellen Computer alle verfügbaren internen IP-Adresse ändern.

### <a name="do-internal-ip-addresses-change-for-vms-in-a-vnet"></a>Gehen Sie wie folgt internen IP-Adressen ändern für virtuellen Computern in einer VNet?

Ja. Interne IP-Adressen bleiben mit dem virtuellen Computer für ihre Lebensdauer, es sei denn, der virtuellen Computer freigegeben ist. Wenn ein virtuellen Computers freigegeben ist, wird die interne IP-Adresse freigegeben, es sei denn, Sie eine statische interne IP-Adresse für Ihre virtuellen Computer definiert. Wenn Sie der virtuellen Computer ist einfach beendet (und nicht in den Status **beendet (Deallocated)**setzen) bleibt die IP-Adresse zugewiesen an den virtuellen Computer.

### <a name="can-i-manually-assign-ip-addresses-to-nics-in-vms"></a>Kann ich IP-Adressen manuell zu Netzwerkkarten in virtuellen Computern zuweisen?

Nein. Sie müssen keine Benutzeroberflächen-Eigenschaften von virtuellen Computern ändern. Alle Änderungen können zu verlieren Konnektivität zu den virtuellen Computer führen.

### <a name="what-happens-to-my-ip-addresses-if-i-shut-down-a-vm"></a>Was geschieht mit meinem IP-Adressen, wenn ich einen virtuellen beenden?

Nichts. Die IP-Adressen (VIP öffentlichen und internen IP-Adresse) verbleibt mit der Cloud-Dienst oder virtueller Computer.

> [AZURE.NOTE] Wenn Sie den virtuellen Computer einfach beenden möchten, verwenden Sie nicht im Verwaltungsportal dazu. Aktuell ist, wird die Schaltfläche war(en) des virtuellen Computers freigeben.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-re-deploying"></a>Kann ich virtuellen Computern von einem Subnetz in ein anderes Subnetz in einer VNet ohne erneut bereitstellen wechseln?

Ja. Weitere Informationen finden Sie [hier](../articles/virtual-network/virtual-networks-move-vm-role-to-subnet.md).

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Kann ich eine statische MAC-Adresse für meine virtuellen Computer konfigurieren?

Nein. MAC-Adresse kann nicht statisch konfiguriert werden.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-it-has-been-created"></a>Bleibt die MAC-Adresse unverändert für meine virtuellen Computer nach dem Erstellen?

Ja, die MAC-Adresse erhalten bleibt für einen virtuellen Computer, obwohl Sie der virtuellen Computer ist (freigegeben) beendet und neu gestartet.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Kann ich eine Verbindung mit dem Internet eines virtuellen Computers in einer VNet herstellen?

Ja. Alle Dienste, die innerhalb einer VNet bereitgestellt können mit dem Internet verbinden. Darüber hinaus gibt jeder bereitgestellt in Azure-Cloud-Dienst eine öffentlich adressierbare VIP zugewiesen. Sie müssen Eingabewerte Endpunkte für PaaS Rollen und die Endpunkte für virtuelle Computer zum Aktivieren dieser Dienste Verbindungen aus dem Internet annehmen definieren.

## <a name="vnets-and-services"></a>VNets und-Diensten

### <a name="what-services-can-i-use-with-vnets"></a>Verwenden auf welche Dienste kann ich mit VNets?

Sie können nur berechnen Services innerhalb VNets verwenden. Berechnen Sie, dass die Dienste auf Cloud Services (Web und Worker Rollen) und virtuellen Computern beschränkt sind.

### <a name="can-i-use-web-apps-with-virtual-network"></a>Kann ich Web Apps mit virtuelles Netzwerk verwenden?

Ja. Sie können Web Apps innerhalb einer VNet mit ASE (App-Service-Umgebung) bereitstellen. Hinzufügen zu, dass Web Apps sicher können eine Verbindung herstellen Sie und Zugriff auf Ressourcen in der Azure-VNet, wenn Sie den Punkt-zu-Standort für Ihre VNet konfiguriert haben. Weitere Informationen finden Sie unter:


- [Erstellen von Web Apps in einer App-Service-Umgebung](../articles/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)

- [Web Apps virtuelles Netzwerk-Integration](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)

- [Verwenden von VNet Integration und Hybrid Verbindungen mit Web Apps](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/)

- [Integrieren einer Web app mit einer Azure-virtuellen Netzwerk](../articles/app-service-web/web-sites-integrate-with-vnet.md)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Kann Clouddiensten mit Web- und Worker Rollen (PaaS) in einem VNet werden bereitgestellt?

Ja. Sie können in VNets PaaS-Dienste bereitstellen.

### <a name="how-do-i-deploy-paas-roles-to-a-vnet"></a>Wie bereit Stelle ich PaaS Rollen in einem VNet?

Hierzu können Sie den Namen des VNet und der Rolle /subnet Zuordnungen im Abschnitt Konfiguration Netzwerk, der Ihre Dienstkonfiguration angeben. Sie müssen nicht die Binärdateien zu aktualisieren.

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Kann ich meine Dienste ein-und VNets verschieben?

Nein. Ein-und VNets Services kann nicht verschoben werden. Sie müssen das Löschen und erneut bereitstellen des Diensts in einem anderen VNet zu verschieben.

## <a name="vnets-and-security"></a>VNets und Sicherheit

### <a name="what-is-the-security-model-for-vnets"></a>Was ist das Sicherheitsmodell für VNets?

VNets sind vollständig isoliert aus einer und anderen Diensten in der Azure-Infrastruktur gehostet wird. Eine VNet ist eine Begrenzungslinie vertrauen.

### <a name="can-i-define-acls-or-nsgs-on-my-vnets"></a>Können ACLs oder NSGs für meine VNets werden definiert?

Nein. Sie können keine ACLs oder NSGs zu VNets zuordnen. Allerdings ACLs definiert werden, klicken Sie auf Eingabe Endpunkte virtuellen Computern, die für einen VNets bereitgestellt wurden, und NSGs kann Subnets oder NICs zugeordnet werden.

### <a name="is-there-a-vnet-security-whitepaper"></a>Gibt es ein VNet Sicherheit Whitepaper?

Ja. Sie können es herunterladen [können](http://go.microsoft.com/fwlink/?LinkId=386611).

## <a name="apis-schemas-and-tools"></a>-APIs, Schemas und Tools

### <a name="can-i-manage-vnets-from-code"></a>Kann ich VNets vom Code verwalten?

Ja. REST-APIs können VNets und Cross lokale Connectivity verwalten. Weitere Informationen finden Sie [hier](http://go.microsoft.com/fwlink/?LinkId=296833).

### <a name="is-there-tooling-support-for-vnets"></a>Ist es an der Unterstützung für VNets?

Ja. Sie können für eine Vielzahl von Plattformen PowerShell und Befehlszeile Tools verwenden. Weitere Informationen finden Sie [hier](http://go.microsoft.com/fwlink/?LinkId=317721).
