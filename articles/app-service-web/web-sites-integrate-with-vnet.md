<properties 
    pageTitle="Integrieren von app mit einer Azure-virtuellen Netzwerk" 
    description="Zeigt, wie Sie eine Verbindung mit einem neuen oder vorhandenen Azure virtuelle Netzwerk eine app im App-Verwaltungsdienst Azure" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor="cephalin"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="ccompy"/>

# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Die app in einer Azure virtuelle Netzwerk integrieren #

Dieses Dokument beschreibt die App-Verwaltungsdienst Azure virtuelles Netzwerk-Integration und zeigt, wie mit apps in [Azure-App-Verwaltungsdienst](http://go.microsoft.com/fwlink/?LinkId=529714)einrichten.  Wenn Sie mit Azure-virtuellen Netzwerken (VNETs) nicht vertraut sind, ist dies eine Funktion, mit der Sie zahlreiche Ressourcen Azure in ein nicht-Internet geroutet Netzwerk platziert, die Steuerung des Zugriffs auf.  Diese Netzwerke können klicken Sie dann auf auf lokale Netzwerke mithilfe einer Vielzahl von VPN-Technologien verbunden sein.  Weitere Informationen zu Azure-virtuellen Netzwerken beginnen mit den Informationen hier lernen: [Azure-virtuellen Netzwerk Übersicht][VNETOverview].  

Die App-Verwaltungsdienst Azure verfügt über zwei Formen.  

1. Die mit mehreren Mandanten Systeme, die das gesamte Spektrum Preise Pläne unterstützen
1. Die App-Service-Umgebung (ASE) Premium-Funktion die in Ihrem VNET bereitstellt.  

Dieses Dokument durchläuft VNET Integration und nicht für App-Service-Umgebung ab.  Wenn Sie weitere Informationen zu den ASE Feature, und klicken Sie dann mit den Informationen hier starten möchten: [Einführung in die App-Service-Umgebung][ASEintro].

VNET Integration Ihren Web app Zugriff auf Ressourcen in Ihrem Netzwerk virtuelle bietet jedoch nicht die Berechtigung privaten-Zugriffs auf Web app aus dem virtuellen Netzwerk.  Zugriff auf die privaten Website ist nur verfügbar, wenn eine ASE mit einem internen laden Lastenausgleich (ILB) konfiguriert ist.  Detaillierte Informationen zur Verwendung von einer ILB ASE, beginnen Sie mit folgenden Artikel: [Erstellen und Verwenden einer ILB ASE][ILBASE]. 

Ein gängiges Szenario, in dem Sie, VNET Integration verwenden würden, ist Access aus der Web-app mit einer Datenbank oder einer auf einer virtuellen Computer im Netzwerk Azure virtuelle Webdienste aktivieren.  Mit VNET Integration brauchen Sie einen öffentlichen Endpunkt verfügbar machen, für Applikationen Ihrer virtuellen Computers kann jedoch stattdessen die privaten geroutet nicht Internet-Adressen verwenden.  

Das Feature VNET Integration:

- erfordert ein Standard- oder Premium Preise plan 
- Arbeiten mit Classic(V1) oder Ressource Manager(V2) VNET 
- TCP und UDP unterstützt
- funktioniert mit Web, Mobile und API-apps
- ermöglicht eine app zu nur 1 VNET gleichzeitig eine Verbindung herstellen
- bis zu 5 VNETs in einer App-Dienst Planen mit integriert werden können 
- ermöglicht die gleichen VNET von mehreren apps in einer App-Dienst planen verwendet werden soll
- unterstützt ein 99,9 % Vereinbarung zum SERVICELEVEL aufgrund einer Abhängigkeit von dem Gateway VNET

Es gibt einige Aktionen, die VNET Integration einschließlich nicht unterstützt:

- Bereitstellen eines Laufwerks
- AD-integration 
- NetBios
- Zugriff auf die Website als "Privat"

### <a name="getting-started"></a>Erste Schritte ###
Hier sind einige Dinge zu beachten vor dem Verbinden Ihrer Web-app zu einem virtuellen Netzwerk aus:

- VNET Integration funktioniert nur mit apps in **Standard** oder **Premium** Preise planen.  Wenn Sie des Features aktivieren, und klicken Sie dann Ihre App-Serviceplan ein nicht unterstütztes Preisgestaltung Plan skalieren verlieren Ihrer apps deren Verbindungen die VNETs verwendeten.  
- Wenn Ihr Ziel virtuelle Netzwerk bereits vorhanden ist, müssen sie Punkt-zu-Standort VPN mit dynamischen routing-Gateway aktiviert werden, bevor sie eine app verbunden werden kann.  Sie können keine Punkt-zu-Standort Virtuelle Private Netzwerk (VPN) aktivieren, wenn Ihr Gateway mit statisches routing konfiguriert ist.
- Die VNET muss im selben Abonnement als Ihre App-Dienst Plan(ASP).  
- Der apps, die mit einer VNET integriert sind, werden die DNS verwendet, die für die VNET angegeben ist.
- Standardmäßig werden der integrierte apps nur Datenverkehr in Ihrem VNET basierend auf den Strecken, die in Ihrem VNET definiert sind.  


## <a name="enabling-vnet-integration"></a>Aktivieren der VNET-Integration ##

Dieses Dokument konzentriert hauptsächlich Azure-Portal zur Integration VNET verwenden.  Führen Sie zum Aktivieren der VNET Integration-App mithilfe der PowerShell der erfahren Sie, wie hier: [Verbinden der app zu Ihrer virtuellen Netzwerk mithilfe der PowerShell][IntPowershell].

Sie haben die Möglichkeit, Ihre app mit einem neuen oder vorhandenen virtuellen Netzwerk verbinden.  Wenn Sie als Teil der Integration klicken Sie dann neben dem einfach die VNET erstellen ein neues Netzwerk erstellen, wird dynamischer routing-Gateway für Sie vorkonfiguriert und zeigen Sie auf der Website VPN wird aktiviert sein.  

>[AZURE.NOTE] Konfigurieren einer neuen virtuelles Netzwerkintegration kann einige Minuten dauern.  

Aktivieren VNET Integration Öffnen der app-Einstellungen, und wählen Sie dann auf Netzwerk.  Die Benutzeroberfläche, die angezeigt wird, bietet drei Netzwerke Auswahlmöglichkeiten.  Mit diesem Leitfaden wird nur in VNET Integration durch Hybrid Verbindungen und App Dienst Umgebungen werden weiter unten in diesem Dokument erläutert.  

Wenn die app nicht in der richtigen Plan Preise ist ermöglicht die Benutzeroberfläche Art Ihres Plans zu einem höheren Preisgestaltung Plan Ihrer Wahl skalieren.


![][1]
 
###<a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>Aktivieren der VNET Integration mit einer bereits vorhandenen VNET###
Die VNET Integration Benutzeroberfläche können Sie aus einer Liste mit Ihrer VNETs auswählen.  Die klassischen VNETs wird anzeigt, dass sie beispielsweise das Wort "Klassischen" in Klammern neben dem Namen des VNET.  So, dass der Ressourcenmanager VNETs zuerst aufgelistet sind, wird die Liste sortiert.  Im unten gezeigten Bild können Sie sehen, dass nur eine VNET ausgewählt werden kann.  Es gibt mehrere Gründe, dass eine VNET einschließlich abgeblendet sein wird:

- die VNET wird in ein anderes Abonnement, dem Ihr Konto auf zugreifen kann
- die VNET hat keine Punkt zur Website aktiviert
- die VNET hat keinen dynamischen routing-gateway


![][2]

Zum Aktivieren der Integration klicken Sie einfach auf die VNET integriert werden soll.  Nachdem Sie die VNET auswählen, wird die app für die Änderungen wirksam werden automatisch gestartet.  

##### <a name="enable-point-to-site-in-a-classic-vnet"></a>Zeigen Sie auf der Website in einem klassischen VNET aktivieren #####
Ihre VNET verfügt nicht über ein Gateway noch Punkt zur Website müssen Sie die erste einrichten.  Wechseln Sie zu diesem Zweck für eine klassische VNET zum [Azure-Portal] [ AzurePortal] und die Liste der virtuelle Networks(classic) aufzurufen.  Sie möchten hier klicken Sie auf das Netzwerk zu integrieren, und klicken Sie auf die große Feld unter Essentials VPN-Verbindungen bezeichnet.  Von hier aus können Sie Ihre Website VPN und sogar Erstellen eines Gateways haben, zeigen erstellen.  Nachdem Sie durch den Punkt zu Website mit Gateway Erstellung Erfahrung werden etwa 30 Minuten, bevor er bereit ist.  

![][8]

##### <a name="enabling-point-to-site-in-a-resource-manager-vnet"></a>Zeigen Sie auf der Website in einem Ressourcenmanager VNET aktivieren #####

Konfigurieren eines VNET Ressourcenmanager mit eines Gateways, und zeigen Sie auf der Website Sie PowerShell verwenden können, wie beschrieben müssen [Konfigurieren einer Punkt-zu-Standort-Verbindung zu einem virtuellen Netzwerk mithilfe der PowerShell][V2VNETP2S].  Die Benutzeroberfläche zum Ausführen dieser Funktion ist noch nicht verfügbar. 

### <a name="creating-a-pre-configured-vnet"></a>Erstellen einer vorkonfigurierten VNET ###
Wenn Sie eine neue VNET, die mit einem Gateway konfiguriert ist und Punkt-zu-Standort erstellen möchten, hat der App-Verwaltungsdienst networking-Benutzeroberfläche die Möglichkeit, allerdings nur für ein Ressourcenmanager VNET erledigen.  Wenn Sie eine klassische VNET mit Gateway und Punkt-zu-Standort erstellen möchten, müssen Sie dies manuell vornehmen, über die Benutzeroberfläche Netzwerke. 

Um eine Ressourcenmanager VNET über die VNET Integration Benutzeroberfläche zu erstellen, wählen Sie **Neues virtuelles Netzwerk erstellen** , und geben Sie die:

- Virtuelle Netzwerknamen
- Adressblock virtuelles Netzwerk
- Subnetnamen
- Subnetz Adressblock
- Adressblock Gateway
- Punkt-zu-Standort des Adressblocks

Wenn Sie möchten, dass diese VNET Verbindung zu einer anderen Netzwerks sollten Sie vermeiden IP-Adressbereichs, die mit diesen Netzwerken überlappt auswählen.  

>[AZURE.NOTE] Ressourcenmanager VNET Erstellung mit einem Gateway dauert etwa 30 Minuten und aktuell wird nicht Integrieren der VNET-App.  Nach der Erstellung der VNET mit dem Gateway müssen Sie wieder zum Ihre app VNET Integration UI, und wählen Sie Ihre neue VNET.

![][3]

Azure VNETs werden normalerweise innerhalb privates Netzwerkadressen erstellt.  Standardmäßig ist die Integration von VNET leitet Feature sämtlichen Datenverkehr für diese IP-Adressbereiche in Ihrer VNET.  Der privaten IP-Adressbereiche sind:

- 10.0.0.0/8 – Dies ist der gleiche wie 10.0.0.0 - 10.255.255.255
- 172.16.0.0/12 - entspricht 172.16.0.0 - 172.31.255.255 
- 192.168.0.0/16 - entspricht 192.168.0.0 - 192.168.255.255
 
Die VNET Adresse Leerzeichen muss in CIDR-Notation angegeben werden.  Wenn Sie mit CIDR-Notation nicht vertraut sind, ist dies eine Methode zum Angeben der Adressblöcke mit eine IP-Adresse und eine ganze Zahl, die die Netzwerkmaske darstellt. Eine Kurzübersicht sollten Sie, dass 10.1.0.0/24 256 Adressen und 10.1.0.0/25 128 Adressen wäre.  Eine IPv4-Adresse mit einem /32 wäre nur 1 Adresse.  

Wenn Sie die DNS-Serverinformationen hier festgelegt werden, die für Ihre VNET festgelegt werden.  Nach der Erstellung der VNET können Sie diese Informationen aus der Benutzerfunktionalität VNET bearbeiten.

Beim Erstellen einer klassischen VNET mithilfe der VNET Integration-Benutzeroberfläche wird in der gleichen Ressourcengruppe als Ihre app ein VNET erstellt. 

## <a name="how-the-system-works"></a>Funktionsweise des ##
Im Hintergrund erstellt dieses Feature auf Punkt-zu-Standort VPN-Technologie Verbindung Ihre app zu Ihrer VNET ein.   Apps in Azure-App-Verwaltungsdienst verfügen über ein mit mehreren Mandanten Systemarchitektur, die ausschließt, eine app direkt in einem VNET bereitgestellt, als mit virtuellen Computern erledigt ist.  Durch Erstellen von Punkt-zu-Standort-Technologie beschränken wir Netzwerkzugriff auf nur des virtuellen Computers Hostinganbieter die app aus.  Zugriff auf das Netzwerk ist außerdem auf diesen Hosts app eingeschränkt, damit Ihre apps nur die Netzwerke zugreifen können, die Sie für den Zugriff auf diese konfigurieren.  

![][4]
 
Wenn Sie einen DNS-Server mit Ihrem Netzwerk virtuelle konfiguriert haben, müssen Sie IP-Adressen verwenden.  Beim Verwenden von IP-Adressen, denken Sie daran, dass die wichtige Vorteile für dieses Feature ist, dass Sie damit die privaten Adressen innerhalb des privaten Netzwerks verwenden können.  Wenn Sie festlegen, dass Ihre app nach Zeitphasen bis zum Verwenden der öffentliche IP-Adressen für eine Ihrer virtuellen Computer, und Sie nicht zur Verfügung VNET Integration Feature und kommunizieren über das Internet.


##<a name="managing-the-vnet-integrations"></a>Verwalten der Integrations VNET##

Die Möglichkeit zum Verbinden und trennen zu einer VNET ist eine app Ebene.  Vorgänge, die Integration von VNET über mehrere apps beeinflussen können sind eine Ebene ASP.  Über die Benutzeroberfläche, die Ebene der app angezeigt wird, können Sie Details auf Ihre VNET erhalten.  Die meisten denselben Informationen wird auch auf der Ebene ASP angezeigt.  

![][5]

Von der Seite Netzwerkstatus-Funktion können Sie sehen, wenn Ihre app zu Ihrer VNET angeschlossen ist.  Ist Ihr Gateway VNET nach unten für würde diese dann Grund als nicht verbundenen anzeigen.  

Die Informationen, die Sie jetzt in der app zur Verfügung haben, die Ebene VNET Integration Benutzeroberfläche die Detailinformationen identisch ist, die Sie von der ASP-Seite zu gelangen.  Es folgen einige diese Elemente:

- VNET Name - dieser Link öffnet die im Netzwerk-Benutzeroberfläche
- Speicherort - wird diese den Speicherort der VNET angezeigt.  Es ist möglich, die in einer VNET in einem anderen Speicherort zu integrieren.
- Zertifizierungsstatus - Zertifikate verwendet, um die VPN-Verbindung zwischen der app und die VNET secure vorhanden sind.  Dieser enthält einen Test, um sicherzustellen, dass sie synchron sind.
- Gatewaystatus - der Gateways nach unten aus irgendeinem Grund sollten kann Ihre app Ressourcen in der VNET nicht zugreifen.  
- VNET Adressbereichs – Dies ist die IP-Adresse Speicherplatz für Ihre VNET.  
- Zeigen Sie auf Website Adresse Abstand – Dies ist der, zeigen Sie auf der Website IP-Adresse Speicherplatz für Ihre VNET.  Die app wird als aus einem der IP-Adressen in diesem Adressbereich gab Kommunikation angezeigt.  
- Website für die Website Adresse Speicherplatz - Sie können zu anderen Websites VPN verwenden, um Ihre VNET auf lokale Ressourcen oder anderen VNETs verbinden.  Sie sollten verfügen, die so konfiguriert ist, und klicken Sie dann die IP-Adressbereiche mit definiert werden, dass VPN-Verbindung hier angezeigt wird.
- DNS-Server – Wenn Sie die DNS-Server mit Ihrem VNET konfiguriert ist, und klicken Sie dann hier aufgeführten haben.
- IP-Adressen an die VNET - dort weitergeleitet werden eine Liste der IP-Adressen, dass Ihre VNET routing weist für definiert ist.  Diese Adressen werden hier angezeigt.  

Der einzige Vorgang, die, den Sie in der app-Ansicht von Ihrem VNET Integration ergreifen können, besteht darin, Ihre app von der VNET zu trennen, sie momentan verbunden ist.  Klicken Sie hierzu einfach auf Trennen oben.  Diese Aktion ändert sich nicht auf Ihre VNET aus.  Die VNET und seine Konfiguration einschließlich der Gateways bleibt unverändert.  Wenn Sie dann Ihre VNET löschen möchten, müssen Sie zuerst die Ressourcen darin einschließlich der Gateways löschen.  

Die App-Dienst planen Ansicht verfügt über eine Reihe von zusätzlichen Operationen.  Es ist auch anders als die app zugreifen.  Zum Erreichen Öffnen der ASP Networking-Benutzeroberfläche einfach Ihrer ASP-Benutzeroberfläche, und führen Sie einen Bildlauf nach unten.  Es gibt ein Element der Benutzeroberfläche Feature Netzwerkstatus bezeichnet.  Es kann einige kleinere Details, um Ihre VNET Integration erzielt werden.  Klick auf diese Benutzeroberfläche Öffnen der Benutzeroberfläche Netzwerk Feature Status.  Wenn Sie "Click here to verwalten" klicken Sie dann auf Öffnen Sie Benutzeroberfläche, die die VNET Integration in dieser ASP-Listen.

![][6]

Die Position des ASP ist ratsam, denken Sie daran, wenn die Speicherorte der im VNETs betrachten, die Sie in integrieren.  Wenn die VNET in einem anderen Speicherort befindet, können Sie sehr viel eher Wartezeitprobleme auftreten.  

Die VNETs integriert ist eine Erinnerung auf wie vielen VNETs, dass Ihre apps integriert sind in dieser ASP und wie viele Sie haben.  

Um jede VNET hinzugefügten Details anzuzeigen, klicken Sie einfach auf die VNET, die Sie interessiert sind.  Zusätzlich zu den Details wurden, die bereits zuvor erwähnt, dass Sie auch eine Liste der apps in dieser ASP angezeigt werden, die die VNET verwendet wird.  

Es gibt zwei primäre Aktionen, in Bezug auf Aktionen.  Die erste ist die Möglichkeit, leitet hinzufügen, die Datenverkehr verlassen der app in Ihrem VNET steuern.  Die zweite Aktion ist die Möglichkeit, Zertifikate und Netzwerkinformationen zu synchronisieren.

![][7]

**Routing** Wie bereits zuvor erwähnt sind die weitergeleitet, die in Ihrem VNET definiert sind, was verwendet wird, für den Datenverkehr in Ihrem VNET aus der app zu leiten.  Es gibt einige Verwendungsmöglichkeiten, obwohl die Stelle, an der Kunden zusätzliche ausgehenden Datenverkehr aus einer app in den VNET und für die sie diese Funktion senden möchten bereitgestellt werden.  Was geschieht mit den Datenverkehr, nachdem das nach Zeitphasen bis zum wie der Kunden ihre VNET konfiguriert ist.  

**Zertifikate** Der Status des Zertifikats widerspiegeln ein Häkchen durchgeführt werden vom Dienst App zu überprüfen, dass die Zertifikate, die für die VPN-Verbindung wir verwenden dennoch hilfreich sind.  Wenn VNET Integration aktiviert, klicken Sie dann ist die ersten Integration zu dieser VNET von apps, die in dieser ASP, es dies ein erforderlichen Austausch von Zertifikaten für die Sicherheit der Verbindung zu gewährleisten.  Sowie die Zertifikate erhalten wir die DNS-Konfiguration, leitet und andere ähnliche Dinge, die im Netzwerk zu beschreiben.
Wenn diese Zertifikate oder Netzwerkinformationen geändert wird, müssen Sie "Netzwerk synchronisieren" klicken.  **Hinweis**: Wenn Sie "Netzwerk synchronisieren" klicken, und klicken Sie dann Sie eine kurze Ausfall in Konnektivität zwischen der app und Ihre VNET verursacht.  Während die app nicht neu gestartet wird, könnte der Verlust der Konnektivität Ihrer Website nicht ordnungsgemäß.  

##<a name="accessing-on-premise-resources"></a>Zugreifen auf lokale Ressourcen auf##

Einer der Vorteile der Funktion für die Integration von VNET heißt wenn Ihre VNET mit Ihrem auf lokale Netzwerk mit einem Standort zu Standort VPN angeschlossen ist, und klicken Sie dann Ihre apps auf auf lokale Ressourcen aus der app zugreifen können.  Damit dies funktioniert, obwohl Sie möglicherweise Ihrer auf lokale VPN-Gateway mit der leitet für Ihre, zeigen Sie auf der Website IP müssen-Bereich.  Wenn das Standort zu Standort VPN zuerst haben eingerichtet sollten konfigurieren verwendeten Skripts auf weitergeleitet, einschließlich Ihrer, zeigen Sie auf der Website VPN einrichten.  Wenn Sie den Punkt Standort VPN nach dem Hinzufügen der Ihr VPN zu anderen Websites erstellen, müssen Sie die Arbeitspläne manuell aktualisieren.  Details zum erledigen variiert pro Gateway und sind nicht alle hier beschriebenen.  

>[AZURE.NOTE] Während die VNET-Integration mit einem Standort zu Standort VPN für den Zugriff auf lokale Ressourcen auf funktionieren funktionieren sie aktuell nicht mit einer ExpressRoute VPN dasselbe.  Dies trifft, wenn Sie mit einer klassischen oder Ressourcenmanager VNET integrieren.  Wenn Sie über ein VPN ExpressRoute Ressourcen zugreifen müssen dann können eine ASE Sie die in Ihrem VNET ausgeführt werden kann. 

##<a name="pricing-details"></a>Informationen zur Preisgestaltung##
Es gibt ein paar Preise Nuancen, die Sie beim Verwenden des Features für die Integration von VNET bewusst sein sollten.  Es gibt 3 verwandte Gebühren für die Verwendung dieses Features:

- ASP Preise Ebene Anforderungen
- Datenübertragung Kosten
- VPN-Gateway Kosten.

Für Ihre apps dieses Feature verwenden können müssen sie ein Standard- oder Planen von Premium App Service sein.  Sie können weitere Details anzeigen, klicken Sie auf diese Kosten hier: [App Preisen][ASPricing]. 

Aufgrund der Art Punkt-zu-Website VPN sind behandelt, immer stehen Ihnen eine Gebühr für ausgehende Daten über die Verbindung VNET Integration, selbst wenn die VNET in derselben Data Center ist.  Was sehen diese Gebühren sind sehen Sie hier: [Daten übertragen Preise Details][DataPricing].  

Das letzte Element ist die Kosten der Gateways VNET.  Wenn Sie die Gateways nach etwas anderem wie zu anderen Websites VPN benötigen dann Zahlen Sie Gateways zur Unterstützung der Funktion VNET Integration für.  Klicken Sie auf diese Kosten hier Details vorhanden sind: [VPN Gateway Preise][VNETPricing].  

##<a name="troubleshooting"></a>Behandlung von Problemen##

Das Feature problemlos eingerichtet werden nicht bedeuten, dass Ihre Erfahrungen Problem kostenlosen gehört.  Sollte der Begegnung sind Probleme beim Zugriff auf Ihre gewünschten Endpunkt es einige Dienstprogramme, die Sie zum Testen der Verbindung von der app-Konsole verwenden können.  Es gibt zwei Console-Erfahrung, die Sie verwenden können.  Eine ist aus der Kudu-Konsole und die andere Konsole, die Sie im Portal Azure erreichen können.  Um die Konsole Kudu aus der app zu gelangen-Gehe zu Extras > Kudu.  Dies ist der gezeigt, [Sitename] identisch. scm.azurewebsites.net.  Wechseln Sie zur Registerkarte Console Debuggen, nachdem die einfach wird geöffnet.  Um zur Azure Portals gehosteten Konsole und dann aus der app zu erhalten-Konsole Gehe zu Extras >.  


####<a name="tools"></a>Tools####

Die Tools Ping, Nslookup und Tracert funktionieren über die Verwaltungskonsole aufgrund Sicherheit Einschränkungen nicht.  Zum Ausfüllen der void dort wurden zwei separate Tools hinzugefügt.  Um festzustellen, ob der DNS-Funktionalität haben wir ein Tool namens nameresolver.exe hinzugefügt.  Die Syntax lautet:

    nameresolver.exe hostname [optional: DNS Server]

Nameresolver können Sie die Hostnamen überprüfen, von denen Ihre app abhängig.  Auf diese Weise können Sie testen, wenn Sie vielleicht keinen Zugriff auf Ihre DNS-Server oder nichts falsch mit Ihren DNS-Server konfiguriert haben.

Dem nächste Tool können Sie zum Testen TCP-Verbindung zu einer Kombination aus Host und Anschluss.  Dieses Programm heißt tcpping.exe und die Syntax lautet:

    tcpping.exe hostname [optional: port]

Dieses Tool informiert, dass automatisch ein, wenn Sie führt die gleiche Aufgabe, die Sie mit der ICMP erhalten jedoch eines bestimmten Hosts und Ports erreichen Ping-Programm basiert.  Die ICMP-Ping informiert Sie, ob Ihre Host von befindet.  Mit Tcpping finden Sie heraus, wenn Sie einen bestimmten Anschluss auf einem Server zugreifen können.  


####<a name="debugging-access-to-vnet-hosted-resources"></a>Ressourcen für das Debuggen Zugriff auf VNET gehostet####

Es gibt eine Reihe von Dingen, die Ihre app eines bestimmten Hosts und Ports Eingang verhindern können.  In den meisten Fällen ist es eine der drei Schritte:

- **Es ist eine Firewall auf die Art und Weise**  Wenn Sie eine Firewall verarbeitet haben und drücken Sie das TCP-Timeout.  In diesem Fall ist, die 21 Sekunden.  Verwenden Sie das Tool Tcpping, um die Verbindung testen.  TCP Zeitlimit können viele Merkmale jenseits von Firewalls der Fall sein, aber es starten.  
- **DNS-Einträge nicht zugegriffen werden**  Das DNS-Timeout ist 3 Sekunden pro DNS-Server.  Wenn Sie 2 DNS-Server verfügen heißt 6 Sekunden.  Verwenden Sie Nameresolver, um festzustellen, ob die DNS ausgeführt wird.  Denken Sie daran, dass Sie Nslookup können nicht wie verwenden, die nicht die DNS-Einträge mit Ihrer VNET konfiguriert ist.
- **Ungültige P2S IP-Bereich** Der, zeigen Sie auf der Website IP-Bereich in die RFC 1918 privaten IP-Adressbereiche werden muss (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255) im Bereich verwendet IP-Adressen, die außerhalb von und Dinge funktioniert nicht.  

Wenn diese Elemente Ihr Problem nicht beantwortet haben, suchen Sie zuerst nach der einfachen Aspekten wie:  

- Werden das Gateway wird als wird oben im Portal angezeigt?
- Werden Zertifikate führen Sie als synchron angezeigt?
- Ändern jemand die Netzwerkkonfiguration, ohne ein "Synchronisieren Netzwerk" in den betroffenen Sicherheitskopie ausführen? 

Nach unten ist Ihr Gateway schalten Sie ihn nach oben.  Wenn Ihre Zertifikate nicht synchron sind dann wechseln Sie zu der Ansicht ASP-Integration Ihrer VNET, und drücken Sie "Netzwerk synchronisieren".  Wenn Sie davon ausgehen, dass eine Änderung an der Konfiguration VNET wurde und Sie es nicht synchronisieren möchten, mit Ihrer Sicherheitskopie dann Ihre VNET Integration der ASP Ansicht wechseln, und drücken Sie "Netzwerk synchronisieren" Just-in als Erinnerung, dadurch wird eine kurze einem Dienstausfall mit Ihrer VNET Verbindung und Ihre apps.  

Wenn diese fein ist, müssen Sie eine kurze Anmerkung Feldcodes:

- Gibt es eine beliebige andere apps VNET Integration zum Erreichen von Ressourcen in der gleichen VNET verwenden? 
- Können Sie wechseln Sie zu der app-Konsole und verwenden Tcpping zum Erreichen von allen anderen Ressourcen in Ihrer VNET?  

Wenn eine vorstehend genannten Punkte zutreffen dann Ihre VNET Integration fein ist und das Problem an anderer Stelle.  Dies ist die Stelle, an der es erhält eine größere Herausforderung sein, da es keine einfache Möglichkeit sehen, warum Sie einen Host: Port erreichen kann.  Einige Ursachen gehören:

- Sie haben eine Firewall von Ihren Host Zugriff auf den Port Anwendung von Ihrem, zeigen Sie auf der Website IP-Bereich zu verhindern.  Subnetze häufig kreuzt erfordert Public-Zugriff.
- Ihr Ziel-Host ist nach unten
- die Anwendung wird nach unten
- Das war der falschen IP- oder hostname
- die Anwendung hört auf einem anderen Port als erwartet.  Sie können dies überprüfen, indem auf dem Host vertraut, und verwenden "Netstat-aon" aus der Befehlszeilenprompt.  Hieraus sehen welche Prozess Sie ID was port Abhören ist.  
- Ihr Netzwerk Sicherheitsgruppen sind in einer Art und Weise so konfiguriert, dass sie Zugriff auf Ihre Anwendungshost und den Port aus Ihrem, zeigen Sie auf der Website IP-Bereich verhindern

Denken Sie daran, dass Sie welche IP in sozusagen Website IP-Bereich nicht kennen, der Ihre app verwendet werden, sodass Sie aus dem gesamten Bereich zugreifen dürfen müssen.  

Debuggen zusätzliche Schritte umfassen:

- Melden Sie sich an einen anderen virtuellen Computer in Ihrer VNET, und versuchen Sie, Ihre Ressourcen Host: Port von dort aus zu erreichen.  Es gibt einige TCP Ping Dienstprogramme, Sie für diesen Zweck können oder auch Telnet verwenden können, wenn werden müssen.  Der Zweck hier ist einfach feststellen, ob Connectivity aus diesem anderen virtuellen Computer ist. 
- Rufen Sie Anwendung einer anderen virtuellen Computer und Testen der Zugriff auf diesen Host und den Port von der Konsole aus der app auf  
####<a name="on-premise-resources"></a>Klicken Sie auf lokale Ressourcen####
Wenn Ihre Ressourcen lokal kann nicht erreicht haben, und klicken Sie dann als Erstes sollten ist, wenn Sie eine Ressource in Ihrer VNET erreichen können.  Wenn die arbeitet, sind die nächsten Schritte ziemlich einfach.  Eines virtuellen Computers in Ihrem VNET müssen Sie versuchen, die auf lokale Anwendung erreicht haben.  Sie können Telnet oder einer TCP Ping-Programm verwenden.  Ihre virtuellen Computer kann nicht erreicht haben, die auf lokale Ressource ab, und vergewissern Sie sich zuerst funktioniert die VPN-Verbindung zu anderen Websites.  Wenn es arbeitet, aktivieren Sie dann die gleichen Interessen, die zuvor erwähnt, sowie auf lokale Gateway-Konfiguration und Status.  

Jetzt, wenn Ihre VNET gehostet virtueller Computer kann Ihrem auf lokale System erreichen, aber Ihre app nicht möglich, dann ist der Grund wahrscheinlich eine der folgenden Aktionen aus:
- Ihre leitet sind nicht mit Ihrem, zeigen Sie auf der Website IP-Adressbereiche in Ihr auf lokale Gateway konfiguriert.
- Ihr Netzwerk Sicherheitsgruppen sind für Ihre, zeigen Sie auf der Website IP-Bereich Zugriff blockieren.
- Ihre auf lokale Firewalls sind Datenverkehr von sozusagen Website IP-Bereich blockieren.
- ein Benutzer definiert Route(UDR) wurden auf Ihre VNET, die Ihre Punkt für die Website, auf der Grundlage von Datenverkehr verhindert Ihre auf lokale Netzwerk erreicht

## <a name="hybrid-connections-and-app-service-environments"></a>Hybrid-Verbindungen und App-Service-Umgebungen##
Es gibt 3 Features, mit denen Zugriff auf Ressourcen VNET gehostet werden.  Sie sind:

- VNET-Integration
- Hybrid-Verbindungen
- App-Service-Umgebungen

Hybrid-Verbindungen müssen Sie einen Relay-Agent aufgerufen, die in Verbindung Manager(HCM) in Ihrem Netzwerk installieren.  Die HCM muss mit Azure und auch an Ihrer Anwendung verbunden werden können.  Diese Lösung eignet sich besonders aus einer remote-Netzwerkzugriff wie Netzwerks auf lokale oder sogar mit einem weiteren Cloud gehostet Netzwerk aus, da es nicht, einen Internet zugänglich Endpunkt erforderlich ist.  Die HCM, die nur unter Windows ausgeführt wird und Sie können bis zu 5 Instanzen zur Gewährleistung hoher Verfügbarkeit ausgeführt haben.  Hybrid-Verbindungen unterstützt nur TCP durch und jeder Endpunkt HC, die an einer Kombination aus bestimmten Host: Anschluss anzupassen.  

Das Feature für die App-Service-Umgebung können Sie eine Instanz des Diensts Azure-App in Ihrem VNET ausführen.  Auf diese Weise Ihre apps Zugriff auf Ressourcen in Ihrem VNET ohne jede zusätzliche Schritte können.  Einige der anderen Vorteile einer App-Service-Umgebung ist, dass Sie 8 Core dedizierter Arbeitskräften mit 14 GB RAM verwendet werden können.  Ein weiterer Vorteil ist, dass Sie die Skalierung des Systems an Ihre Anforderungen anpassen können.  Im Gegensatz zu den mit mehreren Mandanten-Umgebung, in dem Ihre ASP Größe beschränkt ist, steuern in einem ASE wie viele Ressourcen, die Sie an das System gewähren möchten.  In Bezug auf den Fokus Netzwerk dieses Dokuments ist jedoch der Maßnahmen, die Sie mit einer ASE erhalten, die Sie mit VNET Integration nicht, dass es mit einer ExpressRoute VPN arbeiten kann.  

Während der vorhanden ist, dass einige Groß-/Kleinschreibung Überlappung verwenden, können keine der folgenden Features alle anderen ersetzen.  Wissen, welche Funktion zur Verwendung mit Ihren Anforderungen verknüpft ist, und wie wird es verwenden möchten.  Beispiel:

- Wenn Sie ein Entwickler sind und einfach zu eine Website in Azure ausgeführt und haben sie die Datenbank auf dem Computer unter Ihrem Schreibtisch zugreifen möchten ist die einfachste Möglichkeit zum Verwenden von Hybrid Verbindungen.  
- Wenn Sie eine große Organisation sind, die eine große Anzahl von setzen möchte Webeigenschaften im öffentlichen cloud, und diese in Ihrem eigenen Netzwerk verwalten, und klicken Sie dann mit der App-Service-Umgebung übernommen werden sollen.  
- Wenn Sie haben, eine Reihe von App-Dienst gehostet apps und Ressourcen in Ihrer VNET zugreifen, und klicken Sie dann VNET Integration die beste Wahl ist einfach möchten.  

Neben den verwenden Fällen es gibt einige Vereinfachung Zusammenhang Aspekte.  Wenn Ihre VNET bereits mit dem auf lokale Netzwerk dann VNET Integration verbunden ist oder eine App-Service-Umgebung eine einfache Möglichkeit ist, klicken Sie auf lokale Ressourcen nutzen.  Wenn Ihre VNET nicht mit Ihrem auf lokale Netzwerk angeschlossen ist, ist es viel mehr Verwaltungsaufwand zum Einrichten eines VPN zu anderen Websites mit verglichen Ihrer VNET andererseits, mit der Installation von der HCM.  

Über die Funktionsunterschiede hinaus bestehen auch Preise Unterschiede.  Das Feature für die App-Service-Umgebung ist ein Premium-Service-Angebot aber besonders Netzwerk bietet Optionen zur Konfiguration sowie weitere großartige Features.  VNET Integration mit Standard oder Premium Sicherheitskopie verwendet werden kann und eignet sich optimal für sichere verbraucht Ressourcen in Ihrer VNET aus den App-Dienst für mehrere Mandanten.  Hybrid Verbindungen hängt aktuell eine BizTalk Konto das verfügt Ebenen, die kostenlose beginnen und dann schrittweise teurer Preise basierend auf den Betrag ein, die Sie benötigen.  Wenn es jedoch in vielen Netzwerken arbeiten geht, ist es keine anderen Features wie Hybrid Verbindungen, die Sie Zugriff auf Ressourcen in weit mehr als 100 separate Netzwerke aktivieren können.    


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[IntPowershell]: http://azure.microsoft.com/documentation/articles/app-service-vnet-integration-powershell/
[ASEintro]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
