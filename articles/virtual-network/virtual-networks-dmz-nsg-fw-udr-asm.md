<properties
   pageTitle="DMZ-Beispiel – erstellen eine DMZ zum Schützen von Netzwerken durch eine Firewall, UDR und NSG | Microsoft Azure"
   description="Erstellen einer DMZ mit einer Firewall, benutzerdefinierte Routing (UDR) und Netzwerk-Sicherheitsgruppen (NSG)"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor;sivae"/>

# <a name="example-3--build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg"></a>Beispiel 3 – erstellen eine DMZ Netzwerken durch eine Firewall, UDR und NSG schützen

[Kehren Sie zur Sicherheit Begrenzungslinie bewährte Methoden für die Seite][HOME]

In diesem Beispiel wird eine DMZ durch eine Firewall, vier Windows-Servern, Benutzer Routing definiert, IP-Weiterleitung und Netzwerk Sicherheitsgruppen erstellen. Es werden auch die relevanten Befehle zum Bereitstellen von eines besseren Verständnis der einzelnen Schritte durchzuführen. Es gibt auch ein Abschnitt Datenverkehr Szenario einen detaillierten bereitstellen schrittweise wie Verkehr über die Sicherheitsebenen in der DMZ fortgesetzt wird. Schließlich ist Abschnitt in die Bezüge der vollständige Code und Anweisung-Umgebung, um zu testen, und experimentieren Sie mit verschiedenen Szenarios erstellen. 

![Bidirektionale DMZ mit NVA, NSG und UDR][1]

## <a name="environment-setup"></a>Einrichten der Umgebung
In diesem Beispiel ist ein Abonnement, die folgenden enthält:

- Drei cloud Services: "SecSvc001", "FrontEnd001" und "BackEnd001"
- Ein virtuelles Netzwerk "CorpNetwork" mit unterschiedlichen drei: "SecNet", "Front-End" und "Back-End"
- Eine virtuelle Netzwerkeinheit in diesem Beispiel wird eine Firewall, mit dem SecNet Subnetz verbunden
- Einen Windows-Server, der einen Anwendung Webserver iis01 ("Neu")
- Zwei Windows-Servern, die Anwendung wieder darstellen beenden Servers ("AppVM01", "AppVM02")
- Einen Windows-Server, der einen DNS-Server ("DNS01")

Im Abschnitt "Verweise" unter ist ein PowerShell-Skript, die meisten der oben beschriebenen Umgebung zu erstellen, wird es. Erstellen der virtuellen Computern und virtuelle Netzwerke zwar durch das Beispielskript fertig sind nicht im ausführlich beschrieben in diesem Dokument.

Um die Umgebung zu erstellen:

  1.    Speichern Sie die Network Config XML-Datei im Abschnitt Verweise enthalten (aktualisiert mit Namen, Speicherort und IP-Adressen entsprechend das angegebene Szenario)
  2.    Aktualisieren Sie die Benutzervariablen in das Skript entsprechend die Umgebung, die das Skript ist gegen (Abonnements, Dienstnamen usw.) ausgeführt werden soll
  3.    Führen Sie das Skript in PowerShell

**Hinweis**: die Region erhält, der in der PowerShell-Skript muss die Region erhält, der in der XML-Netzwerk Konfigurationsdatei übereinstimmen.

Nachdem das Skript erfolgreich ausgeführt wird möglicherweise die folgenden Skript-Schritte ausgeführt werden:

1.  Richten Sie die Firewallregeln, ist dies in weiter unten im Abschnitt abgedeckt: Regelbeschreibung Firewall.
2.  Optional sind im Abschnitt Verweise zwei Skripts zum Einrichten der Webserver und der app-Server mit einer einfachen Webanwendung zum Testen mit dieser Konfiguration DMZ zulassen.

Nachdem Sie das Skript erfolgreich ausgeführt wurde die Firewall Regeln abgeschlossen sein müssen, ist dies im Abschnitt abgedeckt: Firewall-Regeln.

## <a name="user-defined-routing-udr"></a>Benutzerdefinierte Routing (UDR)
Standardmäßig werden die folgenden System leitet wie folgt definiert:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

Die VNETLocal ist immer die definierten Adresse prefix(es) von der VNet für dieses bestimmte Netzwerk (ie er ändert sich von VNet zu VNet je nachdem, wie jede bestimmte VNet definiert ist). Die verbleibende System leitet sind statisch und wie oben Standard.

Wie für Priorität leitet über der längsten Präfix Übereinstimmung (LPM) Methode verarbeitet werden, daher in der Tabelle die genauesten Routing gelten für eine gegebene Zieladresse.

Daher möchten (beispielsweise auf dem Server DNS01, 10.0.2.4) für das lokale Netzwerk (10.0.0.0/16) bestimmte Verkehr über die VNet den Zielort aufgrund der 10.0.0.0/16 Routing geleitet werden. Kurzum, ist für 10.0.2.4, die 10.0.0.0/16 Routing der genauesten weiterleiten, obwohl die 10.0.0.0/8 und 0.0.0.0/0 auch konnte gelten, aber, da sie kleiner sind bestimmte Weise wirken sich nicht auf diesen Datenverkehr. Somit würde der Datenverkehr an 10.0.2.4 einer nächsten Abschnitt von der lokalen VNet verfügen und einfach an das Ziel weiterleiten.

Wenn Datenverkehr war beispielsweise für 10.1.1.1 vorgesehen, die 10.0.0.0/16 Routing würde nicht anwenden möchten, aber die 10.0.0.0/8 der genauesten wäre, und sich dies der Datenverkehr abgelegt ("Schwarz holed"), da die nächsten Abschnitte Null ist. 

Wenn das Ziel nicht an die Präfixe Null oder die Präfixe VNETLocal anwenden, und klicken Sie dann Sie mindestens bestimmte führen würde weiterzuleiten, 0.0.0.0/0 und sich mit dem Internet weitergeleitet werden, als die nächsten Abschnitte und somit out Azure des Internet Kante.

Es gibt zwei identische Präfixe in der Tabelle weiterleiten, ist vor der Rangfolge basierend auf dem leitet "Quelle" Attribut:

1.  "VirtualAppliance" = ein Benutzer definiert Routing manuell zur Tabelle hinzugefügt.
2.  "VPNGateway" = eine dynamische Routing (BGP bei Verwendung mit Hybrid Netzwerke), ein Netzwerkprotokoll dynamische hinzugefügt, diese leitet können geändert werden über einen Zeitraum wie das dynamische Protokoll automatisch Änderungen in hervorragendem Netzwerk widerspiegeln
3.  "Standard" = das System weitergeleitet, die lokale VNet und statische Einträge wie in der obigen Routingtabelle dargestellt.

>[AZURE.NOTE] Sie können jetzt Benutzer definiert Routing (UDR) mit ExpressRoute und VPN-Gateways verwenden, so erzwingen Sie ausgehenden und eingehenden Cross lokale Datenverkehr an eine virtuelle Netzwerk-Einheit (NVA) weitergeleitet werden.

#### <a name="creating-the-local-routes"></a>Erstellen der lokalen leitet

In diesem Beispiel werden zwei routing-Tabellen benötigt, jeweils eines für die Front-End- und Back-End-Subnetze. Jede Tabelle wird mit statischen leitet für das angegebene Subnetz geeignet geladen. In diesem Beispiel wird weist jede Tabelle drei leitet:

1. Datenverkehr im lokalen Subnetz mit keine der nächsten Abschnitte definiert werden, um den Datenverkehr im lokalen Subnetz umgehen die Firewall zulassen
2. Virtuelle Netzwerkdatenverkehr mit einer nächsten Abschnitte als Firewall definiert, überschreibt diese Eigenschaft aus die Standard-Regel, die lokale VNet Verkehr direkt weiterleiten zulässt
3. Alle übrigen Datenverkehr (0/0) mit den nächsten definiert als Firewall eines Abschnitts

Nachdem Sie die Weiterleitung Tabellen erstellt werden, sind sie an Subnetze gebunden. Für das Front-End-Subnetz sollte routing-Tabelle, einmal erstellt und mit dem Subnetz gebunden wie folgt aussehen:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


In diesem Beispiel werden die folgenden Befehle verwendet, um die Routingtabelle erstellen, eine benutzerdefinierte Routing hinzufügen und dann an einem Subnetz Binden der Routingtabelle (Notiz; alle Elemente unterhalb Anfang mit einem Dollarzeichen (z. B.: $BESubnet) sind benutzerdefinierte Variablen vom Skript im Abschnitt "Daten" dieses Dokuments):

1.  Zunächst muss die base routing-Tabelle erstellt werden. Dieser Ausschnitt zeigt die Erstellung der Tabelle, für die Back-End-Subnetz an. Im Skript wird eine entsprechende Tabelle auch für das Front-End-Subnetz erstellt.

        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"

2.  Nachdem die Routingtabelle erstellt wurde, können bestimmte benutzerdefinierte leitet hinzugefügt werden. Diese snipped werden alle Datenverkehr (0.0.0.0/0) durch das virtuelle Gerät weitergeleitet (einer Variablen zu speichern, $VMIP [0], wird verwendet, um die IP-Adresse zugewiesen, wenn die virtuelle Anwendung zuvor im Skript erstellte übergeben). Im Skript wird eine entsprechende Regel auch in der Front-End-Tabelle erstellt.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

3. Eintrag oben Routing wird der Standard "0.0.0.0/0" Routing, aber die standardmäßigen 10.0.0.0/16 Regel noch vorhandenen überschrieben Datenverkehr innerhalb der VNet zum Weiterleiten von direkt an das Ziel und die virtuelle Netzwerk-Anwendung nicht zulassen möchten. Muss korrekt dieses Verhalten der Regel folgen hinzugefügt werden.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

4. Es gibt an dieser Stelle eine der Auswahlmöglichkeiten vorgenommen werden. Mit den oben angegebenen zwei weitergeleitet werden alle Datenverkehr an die Firewall für Bewertung gerade Datenverkehr in einem einzelnen Subnetz weitergeleitet. Möglicherweise wünschenswert sein, doch um den Datenverkehr in einem Subnetz ohne Einbeziehung von der Firewall weiterleiten eine dritte lokal zu ermöglichen, spezieller Regel hinzugefügt werden kann. Diese Routing Staaten, die eine Adresse destine für das lokale Subnetz nur kann weiterleiten es direkt (NextHopType = VNETLocal).

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal

5.  Schließlich muss mit routing-Tabelle erstellt und mit einem benutzerdefinierten leitet aufgefüllt, die Tabelle jetzt mit einem Subnetz gebunden werden. Im Skript ist die front-End-Routing-Tabelle mit dem Front-End-Subnetz auch gebunden. So sieht das Skript Bindung, für die Back-End-Subnetz aus.

        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
           -SubnetName $BESubnet `
           -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>IP-Weiterleitung
Eine Companion-Funktion zu UDR, ist die IP-Weiterleitung. Dies ist eine Einstellung für eine virtuelle Anwendung, die nicht speziell an die Einheit adressierte Datenverkehr erhalten und leiten dann die Datenverkehr den ultimate-Zielort kann.

Als Beispiel wenn Datenverkehr von AppVM01 auf dem Server DNS01 anfordert weitergeleitet UDR dies an den Firewall. Mit IP-Weiterleitung aktiviert werden der Datenverkehr für das Ziel DNS01 (10.0.2.4) durch die Einheit (10.0.0.4) akzeptiert und dann den ultimate-Zielort (10.0.2.4) weitergeleitet. Ohne IP-Weiterleitung für die Firewall aktiviert würde Datenverkehr nicht von der Anwendung akzeptiert werden, obwohl die Routingtabelle die Firewall als den nächsten Abschnitt hat. 

>[AZURE.IMPORTANT] Es ist entscheidend, denken Sie daran, die IP-Weiterleitung in Verbindung mit Benutzer definiert Routing aktivieren.

IP-Weiterleitung Einrichten eines einzelnen Befehls ist und zum Zeitpunkt der Erstellung virtueller Computer ausgeführt werden kann. Für den Datenfluss in diesem Beispiel wird der Codeausschnitt zum Ende des Skripts und mit den Befehlen UDR gruppiert:

1.  Rufen Sie die Instanz virtueller Computer, die in diesem Fall ist der virtuelle Anwendung, die Firewall, und aktivieren Sie die IP-Weiterleitung (Notiz; ein beliebiges Element in Rot Anfang mit einem Dollarzeichen (z. B.: $VMName[0]) ist eine benutzerdefinierte Variable vom Skript im Abschnitt "Daten" dieses Dokuments. Die Wert 0 in Klammern, [0], steht für den ersten virtuellen Computer in der Matrix von virtuellen Maschinen, die für das Beispielskript ohne Änderung möglich, der erste virtuellen Computer (virtueller Computer 0) muss die Firewall):

        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | `
           Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Netzwerk-Sicherheitsgruppen (NSG)
In diesem Beispiel wird eine Gruppe NSG erstellt, und klicken Sie dann mit einer einzelnen Regel geladen. Diese Gruppe gebunden ist dann nur für die Front-End- und Back-End-Subnetze (nicht die SecNet). Die folgende Regel wird deklarativ erstellt:

1.  Alle Datenverkehr (alle Ports) aus dem Internet an den gesamten VNet (alle Subnetze) wird verweigert.

Obwohl in diesem Beispiel NSGs verwendet werden, ist das Hauptfenster Zweck als sekundäre Schutz vor falsche manuelle Konfiguration. Wir alle eingehenden Datenverkehr aus dem Internet entweder der Front-End blockieren möchten, oder Back-End-Subnetze, Datenverkehr sollte nur über das Subnetz SecNet an die Firewall Datenfluss (und dann, wenn geeignete bei der Front-End oder einer Back-End-Subnetze). Plus würde mit den UDR Regeln an Ort, Datenverkehr, die in der Front-End- oder Back-End-Subnetze aufgenommen werden konnten, an der Firewall (Dank UDR) weitergeleitet werden. Die Firewall sieht dies als eine asymmetrische Fluss und legen Sie den ausgehenden Datenverkehr würden. Es gibt also drei Sicherheitsebenen Schutz der Front-End- und Back-End-Subnets; 1) auf der FrontEnd001 und BackEnd001 keine geöffneten Endpunkte cloud Services, 2) NSGs Datenverkehr aus dem Internet, 3) der Firewall ablegen asymmetrische Datenverkehr verweigern.

Eine interessanten Punkts hinsichtlich der Netzwerk-Sicherheitsgruppe in diesem Beispiel ist, dass sie nur eine Regel, abgebildet, enthält die besteht darin, die Internet-Datenverkehr auf das gesamte virtuelle Netzwerk abzulehnen, die im Subnetz Sicherheit einschließen möchten. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
        from the Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

Jedoch, da die NSG nur an die Front-End- und Back-End-Subnetze gebunden ist, die Regel ist nicht verarbeitet auf den Datenverkehr an das Subnetz Sicherheit eingehenden. Daher, obwohl die Regel NSG kein Internet-Verkehr zu einer beliebigen Adresse auf die VNet lautet, da Sie die NSG nie mit dem Subnetz Sicherheit gebunden war, fließt den Datenverkehr an das Subnetz Sicherheit.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName
    
    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Firewall-Regeln
Klicken Sie auf die Firewall müssen Regeln weiterleiten erstellt werden. Da die Firewall blockieren oder Weiterleitung alle eingehenden, ausgehenden und innerhalb-VNet Datenverkehr ist sind viele Firewall-Regeln erforderlich. Darüber hinaus werden alle eingehender Datenverkehr Security Service öffentliche IP-Adresse (auf andere Ports), drücken Sie durch die Firewall verarbeitet werden soll. Es wird empfohlen, die logischen Zahlungen vor dem Einrichten der Subnetze in einem Diagramm und Firewallregeln zur Vermeidung später überarbeiten. Die folgende Abbildung zeigt eine logische Ansicht der Firewall-Regeln in diesem Beispiel:
 
![Logische Ansicht der Firewall-Regeln][2]

>[AZURE.NOTE] Basierend auf das Netzwerk virtuelle Einheit verwendet, werden die Management-Ports variieren. In diesem Beispiel wird eine Barracuda NextGen Firewall verwiesen wird, wird die Ports 22, 801 und 807 verwendet. Finden Sie in der Dokumentation des Herstellers Einheit genügt die genauen Ports für die Verwaltung von dem verwendeten Gerät verwendet.

### <a name="logical-rule-description"></a>Logische Regelbeschreibung
Im obigen logisches Diagramm wird das Sicherheit Subnetz nicht angezeigt, da die Firewall der einzige Ressource in diesem Subnetz ist und Firewall-Regeln, und wie diese logisch zulassen oder verweigern Datenverkehr Zahlungen und nicht den tatsächlichen weitergeleitete Pfad dieses Diagramm angezeigt wird. Darüber hinaus die externen Ports für der Datenverkehr RDP sind höher aktiviert einem Bereich Ports (8014 – 8026) und etwas mit den letzten beiden Oktetts der lokale IP-Adresse Lesbarkeit ausrichten ausgewählt wurden (z. B. lokale Serveradresse 10.0.1.4 ist externen Anschluss 8014 zugeordnet), jedoch eine höhere-Konflikte verursachende Ports verwendet werden können.

In diesem Beispiel 7 Typen von Regeln benötigt, werden diese Regeltypen wie folgt beschrieben:

- Externe Regeln (für eingehenden Datenverkehr):
  1.    Firewall Management Regel: Diese Regel App umleiten kann Datenverkehr an die Management-Ports der Einheit virtuelle Netzwerk übergeben.
  2.    RDP Regeln (bei jeder WindowsServer): Diese vier Regeln (eine für jede Server) werden die Verwaltung von den einzelnen Servern über RDP zulassen. Dies kann auch in eine Regel je nach den Funktionen der verwendeten Netzwerk virtuelle Einheit zusammengefasst.
  3.    Regeln für die Anwendung Datenverkehr: Es gibt zwei Anwendung Datenverkehr Regeln, die erste für die front-End-Web-Verkehr und die Sekunde für die Back-End-Datenverkehr (z.B. Webserver mit Datenebene). Die Konfiguration dieser Regeln werden abhängig von der Netzwerkarchitektur (der Ihre Server platziert sind) und Datenverkehr Zahlungen (welcher Richtung die Datenverkehr Zahlungen und welche ports verwendet werden).
      - Die erste Regel, den eigentlichen Anwendungsdatenverkehr erreichen, den Anwendungsserver zulassen. Während die anderen Regeln für Sicherheit, Verwaltung usw. zulassen, sind Anwendungsregeln an, was externe Benutzer oder Dienste die Anwendung(en) zugreifen können. In diesem Beispiel gibt es ein einzelnen Webserver auf Port 80, daher wird die Regel für eine einzelne Firewall-Anwendung eingehenden Datenverkehr leiten Sie an die externe IP-Adresse der Web-Servern internen IP-Adresse. Die Sitzung umgeleitet Datenverkehr würde an den internen Server NAT gelegt.
      - Die zweite Regel der Anwendung Datenverkehr ist die Back-End-Regel, die dem Webserver mit den AppVM01-Server (aber nicht AppVM02) sprechen über einen beliebigen Port zulässt.
- Interner Regeln (für den Datenverkehr für innerhalb-VNet)
  4.    Ausgehende Internet Regel: mit dieser Regel wird Datenverkehr von jedes Netzwerk Übergabe an den ausgewählten Netzwerken zulassen. Diese Regel ist in der Regel eine Standardregel bereits auf die Firewall, aber deaktiviert. In diesem Beispiel sollte diese Regel aktiviert sein.
  5.    DNS-Regel: Mit dieser Regel kann nur DNS-Einträge (Port 53) Datenverkehr an den DNS-Server zu übergeben. Für diese Umgebung, die meisten den Datenverkehr aus der Front-End in die Back-End-blockiert wird, kann diese Regel speziell DNS aus einem beliebigen lokalen Subnetz an.
  6.    Subnetz zu Subnetz Regel: mit dieser Regel wird an, damit alle Server in die Back-End-Subnetz Verbindung zu einem beliebigen Server auf dem front-End-Subnetz (aber nicht umgekehrt).
- Fail Regel (für den Datenverkehr, die keines der obigen Plug-Ins entsprechen nicht):
  7.    Verweigern alle Datenverkehr Regel: Dies sollten immer die endgültige Regel (in Bezug auf die Priorität) und als solche a Datenverkehr fließt einen Fehler in der vorherigen Regeln entsprechen, die mit dieser Regel gelöscht wird. Dies ist eine Standardregel und in der Regel aktiviert ist, werden in der Regel keine Änderungen benötigt.

>[AZURE.TIP] Der zweite Anwendung Datenverkehr Regel einen beliebigen Anschluss ist zulässig für einfach in diesem Beispiel, in einem realen Szenario den Port genauesten und Adressbereiche zum Verringern der Angriffen gegen diese Regel verwendet werden sollte.

<br />

>[AZURE.IMPORTANT] Nachdem alle oben genannten Regeln erstellt werden, ist es wichtig, um zu überprüfen, die Priorität der einzelnen Regeln, um sicherzustellen, dass Datenverkehr zulässig oder verweigert wie gewünscht wird. In diesem Beispiel werden die Regeln Priorität aus. Es ist einfach, aus der Firewall aufgrund falsch bestellten Regeln gesperrt werden. Mindestens stellen Sie sicher, dass die Verwaltung für die Firewall selbst immer den absoluten Regel mit höchster Priorität ist.

### <a name="rule-prerequisites"></a>Voraussetzungen für die Regel
Eine Vorbedingung für die Ausführung der Firewalls virtuellen Computern sind öffentliche Endpunkte. Für die Firewall Datenverkehr Verarbeitungszeit müssen die entsprechende öffentliche Endpunkte geöffnet wird. Es gibt drei Typen von Datenverkehr in diesem Beispiel; 1) Management Datenverkehr Steuerelement die Firewall und Firewall-Regeln, 2) RDP-Datenverkehr in die Windows-Servern und 3) Anwendung Datenverkehr steuern. Dies sind die drei Spalten Datenverkehrstypen in der oberen Hälfte der logischen Ansicht über die Firewallregeln.

>[AZURE.IMPORTANT] Hier eine Key Takeway besteht darin, denken Sie daran, dass **der gesamte** Datenverkehr durch die Firewall versehen werden. Also mit Remotedesktop auf dem Server iis01 neu, werden, obwohl es in Front-End-Cloud-Dienst und der Front-End-Subnetz ist zum Zugreifen auf diesem Server wir müssen, um die Firewall auf Anschluss 8014 RDP, und lassen Sie Firewall, um die Anfrage RDP intern an den iis01 neu RDP-Anschluss weiterleiten. Azure-Portal "Verbinden" Schaltfläche funktionieren nicht, da kein direkter RDP den Pfad für iis01 neu ist (soweit im Portal sichtbar sind). Dies bedeutet, dass alle Verbindungen aus dem Internet auf den Dienst Sicherheit und einen Anschluss, z. B. secscv001.cloudapp.net:xxxx (Referenz obigen Diagramm für die Zuordnung von externer Anschluss und interne IP-Adresse und den Port) beziehen soll.

Ein Endpunkt kann geöffnet werden, entweder zum Zeitpunkt der Erstellung virtueller Computer oder Posten erstellen, wie im Beispielskript fertig ist, und in dieser Codeausschnitt abgebildet (Notiz; alle Anfang Element mit einem Dollarzeichen (z. B.: $VMName[$i]) ist eine benutzerdefinierte Variable vom Skript im Abschnitt "Daten" dieses Dokuments. "$I" in Klammern, [$i], stellt die Matrix Zahl eines bestimmten virtuellen Computers in einem Array von virtuellen Computern):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

Zwar nicht klar hier gezeigten aufgrund der Verwendung von Variablen, aber die Endpunkte **nur** Sicherheit Cloud-Dienst geöffnet. Dies ist, um sicherzustellen, dass alle eingehender Datenverkehr behandelt wird (weitergeleitet, NAT vorkam, verworfen) durch die Firewall.

Management-Client auf einem PC verwalten die Firewall, und erstellen die erforderlichen Konfigurationen installiert werden müssen. Finden Sie unter den Hersteller Dokumentation aus Ihrem Firewall (oder andere NVA) zum Verwalten des Geräts ein. Der Rest der in diesem Abschnitt und im nächsten Abschnitt Firewall Regeln erstellen, wird die Konfiguration der Firewall selbst, bis der Lieferanten Management-Client (d. h. nicht Azure-Portal oder PowerShell) beschreiben.

Anweisungen für den Download durch Clients und Herstellen einer Verbindung mit den in diesem Beispiel verwendete Barracuda finden Sie hier: [Barracuda von Administrator](https://techlib.barracuda.com/NG61/NGAdmin)

Nach der Anmeldung auf die Firewall, aber davor Firewall-Regeln erstellen, gibt es zwei vorbereitende Objektklassen, die die Regeln einfacher erstellen vornehmen können. Netzwerk & Service-Objekte.

In diesem Beispiel sollten drei benannten Netzwerkobjekte definierten (eine für die Front-End-Subnetz und die Back-End-Subnetz, auch ein Netzwerkobjekt für die IP-Adresse des DNS-Servers). So erstellen Sie einen benannten Netzwerk; beginnend mit dem Barracuda NG Admin-Client-Dashboard, navigieren Sie zur Konfigurationsregisterkarte, klicken Sie im Abschnitt Konfiguration Betrieb Regelsatz auf, dann klicken Sie auf "Netzwerke" im Menü Firewall-Objekte, dann klicken Sie auf neu, klicken Sie im Menü Bearbeiten Netzwerken. Das Netzwerkobjekt kann nun erstellt werden durch hinzufügen den Namen und das Präfix:

![Erstellen Sie ein Objekt Front-End-Netzwerk][3]
 
Dadurch wird eine benannte Netzwerk für das Front-End-Subnetz erstellt, für die Back-End-Subnetz sowie eine ähnliche Objekt erstellt werden soll. Nun können die Subnetze einfacher anhand des Namens in der Firewall-Regeln verwiesen werden.

Für die DNS-Server-Objekt:

![Erstellen eines DNS-Server-Objekts][4]

Dieser Bezug der einzelnen IP-Adresse wird in einer Regel für die DNS-Einträge in das Dokument später genutzt werden.

Die zweite vorbereitende Objekte sind Dienste. Dies stellt die RDP Verbindung Ports für jeden Server dar. Da das vorhandene RDP Dienstobjekt an den RDP Standardport gebunden ist, können 3389, neue Dienste erstellt werden, die von den externen Ports (8014-8026) Datenverkehr zulässt. Die neuen Ports konnte auch an den vorhandenen RDP Dienst hinzugefügt werden, aber zum Steigern der Demo, eine einzelne Regel für jeden Server erstellt werden kann. So erstellen Sie eine neue RDP Regel für einen Server; beginnend mit dem Barracuda NG Admin-Client-Dashboard, navigieren Sie zur Konfigurationsregisterkarte klicken Sie im Abschnitt Betriebskonfiguration auf Regelsatz, und klicken Sie dann auf "Dienstleistungen" im Menü Firewall-Objekte, einen Bildlauf nach unten der Liste der Dienste und wählen Sie den Dienst "RDP". Mit der rechten Maustaste, und wählen Sie kopieren, und klicken Sie dann mit der rechten Maustaste ein, und wählen Sie einfügen aus. Es ist jetzt ein RDP-Copy1-Service-Objekt, das bearbeitet werden kann. Mit der rechten Maustaste RDP-Copy1, und wählen Sie bearbeiten, das Objekt Dienst bearbeiten pop-Fenster bis wie hier dargestellt wird:

![Kopieren der standardmäßigen RDP Regel][5]

Die Werte können dann zum Darstellen des RDP-Diensts für einen bestimmten Server bearbeitet werden. Für AppVM01 die obige Standard RDP Regel geändert werden sollte, um eine neue Service Name, Beschreibung und externen RDP Port verwendet, die im Diagramm Abbildung 8 wiederzugeben (Notiz: die Ports sind die standardmäßig RDP 3389 an den externen Port für diesen bestimmten Server verwendeten geändert, bei AppVM01 ist der externe Port 8025) der geänderte Dienst abgebildet :

![AppVM01 Regel][6]
 
Dieses Verfahren muss zum Erstellen von RDP Diensten für die verbleibenden Server wiederholt werden. AppVM02, DNS01 und iis01 neu. Die Erstellung der folgenden Dienste wird die Regel Erstellung einfacher und noch deutlicher im nächsten Abschnitt stellen.

>[AZURE.NOTE] Ein RDP-Dienst für die Firewall ist zwei Gründen nicht erforderlich. 1) erste die Firewall virtueller Computer ein Bild Linux-basierte, ist sodass SSH auf Anschluss 22 werden, für die Verwaltung von virtuellen Computern anstelle von RDP und 2) Anschluss 22 verwendet würde und zwei Management-Ports dürfen in die erste Management-Regel, die nachfolgend beschriebenen, um Management-Konnektivität zu ermöglichen.

### <a name="firewall-rules-creation"></a>Erstellung der Firewall-Regeln
Es gibt drei Typen von Firewall-Regeln, die in diesem Beispiel verwendete, diese distinct Symbole haben:

Die Anwendung umleiten Regel: ![Umleiten Anwendungssymbol][7]

Die Ziel-NAT Regel: ![Ziel NAT-Symbol][8]

Die Regel Pass: ![Symbol übergeben][9]

Weitere Informationen zu dieser Regeln finden Sie in der Barracuda-Website.

Um die folgenden Regeln erstellen (oder vorhandenen Regeln für das standardmäßige überprüfen), beginnend mit dem Dashboard Barracuda NG Admin-Client, navigieren Sie zur Konfigurationsregisterkarte, klicken Sie in der Konfiguration Betrieb Abschnitt auf Regelsatz. Einem Raster bezeichnet, wird auf dieser Firewall "Primär Regeln" die vorhandenen aktivierte und deaktivierten Regeln angezeigt. In der oberen rechten Ecke dieses Rasters ist eine kleine, Grün "+" Schaltfläche, klicken Sie auf diese Option, um eine neue Regel erstellen (Notiz: Ihre Firewall möglicherweise "gesperrt" Änderungen, wenn Sie eine Schaltfläche "Sperren" markiert angezeigt wird und Sie zum Erstellen oder Bearbeiten von Regeln klicken Sie auf diese Schaltfläche, um den Regelsatz "Entsperren" und Bearbeitung erlauben erfolgreiche). Wenn Sie eine vorhandene Regel bearbeiten möchten, wählen Sie die Regel, mit der rechten Maustaste, und wählen Sie Regel bearbeiten aus.

Nachdem Sie Ihre Regeln erstellt und/oder geändert werden, an die Firewall abgelegt und dann aktiviert werden müssen, wenn dies nicht der Fall die Regel Änderungen werden wirksam. Der Vorgang Pushbenachrichtigungen und-Aktivierung wird unterhalb der Details Regel Beschreibungen beschrieben.

Die Einzelheiten jede Regel, die in diesem Beispiel Abschluss erforderlich sind wie folgt beschrieben:

- **Regel für Firewall-Verwaltung**: Diese App umleiten Regel lässt Datenverkehr an die Management-Ports die virtuelle Netzwerk-Anwendung, in diesem Beispiel wird eine Barracuda NextGen Firewall übergeben. Die Management-Ports sind 801, 807 und 22 bei Bedarf an. Die internen und externen Ports sind gleich (d. h. keine Port Übersetzung). Diese Regel, SETUP-Management-ACCESS, ist eine Standardregel und (in Barracuda NextGen Firewall Version 6.1) standardmäßig aktiviert.

    ![Firewall Management-Regel][10]

>[AZURE.TIP] Die Quelle Adresse Leerzeichen in diese Regel ist vorhanden, wenn die Management IP-Adressbereiche bekannt sind, diesen Bereich würde auch reduzieren verringert die Angriffsfläche auf Management-Ports.

- **RDP Regeln**: Diese Ziel NAT-Regeln, die Verwaltung von den einzelnen Servern über RDP zulassen.
Es gibt vier kritisch (Felder) erforderlich, um diese Regel zu erstellen:
  1.    Quelle –, damit RDP überall den Bezug "Beliebig" wird in das Feld Quelle verwendet werden.
  2.    Service – verwenden Sie das entsprechende Service-Objekt, das weiter oben, in diesem Fall "AppVM01 RDP" erstellt, die externen Ports der Server lokale IP-Adresse und Port 3386 (die RDP Standardport) weiterleiten. Diese bestimmten Regel ist für RDP Zugriff auf AppVM01.
  3.    Ziel – sollte der *lokalen Port an die Firewall*, "Bestehenden 1 lokale IP-" oder eth0 sein, wenn statische IP-Adressen verwenden. Die englische Anzahl (eth0, eth1 usw.) kann abweichen, wenn Ihre Netzwerkeinheit mehrere lokale Schnittstellen verfügt. Dies ist der Port aus die Firewall versenden ist (möglicherweise identisch mit den Port empfangen), das Ziel des eigentliche weitergeleitete wird im Feld Zielliste.
  4.    Umleitung – teilt den in diesem Abschnitt der virtuellen Anwendung, wo schließlich diesen Datenverkehr umleiten. Die einfachste Umleitung besteht darin, die IP- und den Port im Feld Zielliste (optional) setzen. Wenn kein Port verwendet wird der Ziel-Port auf eingehenden Anfragen werden (ie keine Übersetzung) verwendet, wenn ein Port den Port vorgesehen ist werden auch NAT würde zusammen mit der-IP-Adresse.

    ![Firewall RDP-Regel][11]

    Insgesamt vier RDP Regeln müssen erstellt werden: 

  	|   Regelname    | Server  |   Dienst   |  Zielliste  |
  	|----------------|---------|-------------|---------------|
  	| RDP-zu-iis01 neu   | IIS01 NEU   | IIS01 NEU RDP   | 10.0.1.4:3389 |
  	| RDP-zu-DNS01   | DNS01   | DNS01 RDP   | 10.0.2.4:3389 |
  	| RDP-zu-AppVM01 | AppVM01 | AppVM01 RDP | 10.0.2.5:3389 |
  	| RDP-zu-AppVM02 | AppVM02 | AppVm02 RDP | 10.0.2.6:3389 |
  
>[AZURE.TIP] Den Bereich der Datenquellen und Dienst Felder Eingrenzung wird die Angriffen verringern. Der am häufigsten begrenzten Interessentenkreis, mit der Funktionen kann sollte verwendet werden.

- **Anwendung Datenverkehr Regeln**: Es gibt zwei Anwendung Datenverkehr Regeln, die erste für die front-End-Web-Verkehr und die Sekunde für die Back-End-Datenverkehr (z.B. Webserver mit Datenebene). Diese Regeln werden abhängig von der Netzwerkarchitektur (der Ihre Server platziert sind) und Datenverkehr Zahlungen (welcher Richtung die Datenverkehr Zahlungen und welche ports verwendet werden).

    Zuerst behandelt, wird die front-End-Regel für Datenverkehr im Web ist:

    ![Firewall-Web-Regel][12]
 
    Mit dieser Regel Ziel NAT ermöglicht den eigentlichen Anwendungsdatenverkehr Anwendungsserver erreicht haben. Während die anderen Regeln für Sicherheit, Verwaltung usw. zulassen, sind Anwendungsregeln an, was externe Benutzer oder Dienste die Anwendung(en) zugreifen können. In diesem Beispiel gibt es ein einzelnen Webserver auf Port 80, daher wird die Regel für die einzelnen Firewall Anwendung eingehenden Datenverkehr leiten Sie an die externe IP-Adresse der Web-Servern internen IP-Adresse.

    **Hinweis**:, dass es wurde keine Port in das Feld Zielliste zugewiesen, daher der eingehende Port 80 (oder 443 für den ausgewählten Dienst) wird in die Umleitung von dem Webserver verwendet werden. Der Webserver auf einem anderen Port überwacht, beispielsweise port 8080 verwenden, das Feld Zielliste konnte zu 10.0.1.4:8080 für den Port Umleitung sowie dürfen aktualisiert werden.

    Die nächsten Anwendung Datenverkehr Regel handelt es sich um die Back-End-Regel, die dem Webserver mit den AppVM01-Server (aber nicht AppVM02) sprechen über alle Dienst zulässt:

    ![Firewall AppVM01 Regel][13]

    Mit dieser Regel Pass ermöglicht eine IIS-Server im Front-End-Subnetz erreichen der AppVM01 (IP-Adresse 10.0.2.5) auf einen beliebigen Anschluss, verwenden Sie jedes Protokoll zu Access-Daten, die von der Webanwendung benötigt.

    In diesem Screenshot einer "\<explizit Ziel\>" wird im Zielfeld verwendet, um 10.0.2.5 als Ziel zu kennzeichnen. Möglicherweise entweder explizit benannte Siehe oder Netzwerk-Objekts (wie in der erforderlichen Komponenten für die DNS-Server ausgeführt wurde). Dies ist auf der Administrator der Firewall, die welche Methode verwendet wird. Doppelklicken Sie auf 10.0.2.5 als eine Explict Desitnation hinzufügen möchten, klicken Sie auf der ersten leeren Zeile unter \<explizit Ziel\> , und geben Sie die Adresse in das Fenster, das eingeblendet wird.

    Mit dieser Regel übergeben ist keine NAT erforderlich, da dies interner Datenverkehr ist, damit die Verbindung-Methode "Kein SNAT" festgelegt werden können.

    **Hinweis**: der Quelle Netzwerk in dieser Regel wird eine Ressource im Front-End-Subnetz, wenn es nur eine oder eine bestimmte bekannte Anzahl von Webservern werden, eine Ressource Network-Objekt konnte erstellt werden um genauer zu diesen genauen IP-Adressen statt der gesamten Front-End-Subnetz sein.

>[AZURE.TIP] Diese Regel verwendet des Diensts "Beliebiger", damit die Anwendung Stichprobe einfacher zu nutzen, damit können auch ICMPv4 (ping) in eine einzelne Regel. Dies ist jedoch nicht empfohlen. Die Ports und Protokolle ("Services") sollte auf ein Minimum möglichen eingegrenzt werden, die Anwendung Vorgangs die Angriffen über diese Grenze verringern ermöglicht.

<br />

>[AZURE.TIP] Auch wenn diese Regel einen explizit Ziel Bezug verwendeten angezeigt wird, sollte ein einheitliches Verfahren in der Konfiguration der Firewall verwendet werden. Es wird empfohlen, dass das benannte Netzwerk-Objekt in der gesamten für die Lesbarkeit und Flexibilität verwendet werden. Die explizite Ziel wird ausschließlich zum Anzeigen einer alternative Referenzmethode hier verwendet und sollte nicht im Allgemeinen (vor allem für komplexe Konfigurationen).

- **Ausgehend von Internet-Regel**: übergeben Sie diese Regel lässt Datenverkehr aus einer beliebigen Quellnetzwerk an den ausgewählten Ziel Netzwerken übergeben. Diese Regel ist eine Standardregel normalerweise bereits auf dem Firewall Barracuda NextGen, aber deaktiviert ist. Mit der rechten Maustaste auf diese Regel können Sie den Befehl Regel aktivieren zugreifen. Die Regel, die hier gezeigten wurde geändert, um das Attribut Quelle diese Regel zwei lokale Subnetze, die erstellt wurden als Verweise im Abschnitt vorbereitende dieses Dokuments hinzufügen.

    ![Ausgehende Firewall-Regel][14]

- **DNS-Regel**: übergeben Sie diese Regel kann nur DNS-(Port 53) Datenverkehr an den DNS-Server zu übergeben. Für diese Umgebung, die meisten den Datenverkehr aus der Front-End in die Back-End-blockiert wird, kann diese Regel speziell DNS.

    ![Firewall DNS-Regel][15]

    **Hinweis**: In dieser Bildschirm Screenshot der Verbindungsmethode enthalten ist. Da diese Regel für interne IP-Adresse für interne IP-Adresse Datenverkehr ist, keine NATing ist erforderlich, diese Verbindungsmethode ist auf festlegen "Kein SNAT" für diese Regel Pass.

- **Subnetz zu Subnetz Regel**: übergeben Sie diese Regel ist eine Standardregel, die geändert werden, um alle Server im Back-End-Subnetz Verbindung zu einem beliebigen Server im front-End-Subnetz können und aktiviert wurde. Mit dieser Regel wird alle internen Datenverkehr, damit die Verbindungsmethode keine SNAT festgelegt werden können.

    ![Firewall innerhalb VNet Regel][16]

    **Hinweis**: das Kontrollkästchen bidirektionale nicht aktiviert ist (noch in den meisten Regeln aktiviert ist), dies ist von Bedeutung für diese Regel darin, dass sie diese Regel "einseitig" macht, eine Verbindung aus dem Back-End-Subnetz der front-End-Netzwerk, aber nicht umgekehrt initiiert werden kann. Wenn dieses Kontrollkästchen aktiviert wurde, würde diese Regel bidirektionale Datenverkehr, aktivieren, die von unserem logisches Diagramm nicht gewünscht wird.

- **Regel für alle Datenverkehr verweigern**: Diese sollte die endgültige Regel (in Bezug auf die Priorität) werden und als solche, wenn eine Datenfluss fehlschlägt keines entsprechend zu der vorangehenden Regeln werden Sie von dieser Regel gelöscht werden. Dies ist eine Standardregel und in der Regel aktiviert ist, werden in der Regel keine Änderungen benötigt. 

    ![Firewall verweigern Regel][17]

>[AZURE.IMPORTANT] Nachdem alle oben genannten Regeln erstellt werden, ist es wichtig, um zu überprüfen, die Priorität der einzelnen Regeln, um sicherzustellen, dass Datenverkehr zulässig oder verweigert wie gewünscht wird. In diesem Beispiel werden die Regeln in der Reihenfolge an, die sie im Raster primär der Weiterleitung Regeln im Client Management Barracuda angezeigt werden sollen.

## <a name="rule-activation"></a>Regel Aktivierung
Mit der Regelsatz der Spezifikation des Diagramms Logik geändert muss der Regelsatz die Firewall geladen und dann aktiviert werden.

![Aktivierung der Firewall-Regel][18]
 
Sind Sie ein Cluster von Schaltflächen in der oberen rechten Ecke des Management-Clients. Klicken Sie auf die Schaltfläche "Änderungen senden", um die geänderten Regeln an die Firewall zu senden, und klicken Sie auf die Schaltfläche "Aktivieren".
 
Bei der Aktivierung von der Firewall Regelsatz Abschluss dieses Beispiel-Umgebung erstellen.

## <a name="traffic-scenarios"></a>Datenverkehr Szenarien
>[AZURE.IMPORTANT] Eine wichtige Takeway besteht darin, denken Sie daran, dass **der gesamte** Datenverkehr durch die Firewall versehen werden. Also mit Remotedesktop auf dem Server iis01 neu, werden, obwohl es in Front-End-Cloud-Dienst und der Front-End-Subnetz ist zum Zugreifen auf diesem Server wir müssen, um die Firewall auf Anschluss 8014 RDP, und lassen Sie Firewall, um die Anfrage RDP intern an den iis01 neu RDP-Anschluss weiterleiten. Azure-Portal "Verbinden" Schaltfläche funktionieren nicht, da kein direkter RDP den Pfad für iis01 neu ist (soweit im Portal sichtbar sind). Dies bedeutet, dass alle Verbindungen aus dem Internet auf den Dienst Sicherheit und einen Anschluss, z. B. secscv001.cloudapp.net:xxxx beziehen soll.

Für diese Szenarios sollte die folgenden Firewallregeln angeordnet werden:

1.  Firewall-Verwaltung
2.  RDP zu iis01 neu
3.  RDP zu DNS01
4.  RDP zu AppVM01
5.  RDP zu AppVM02
6.  App Datenverkehr im Web
7.  App-Verkehr zu AppVM01
8.  Ausgehende mit dem Internet
9.  Front-End zu DNS01
10. Innerhalb-Subnetz Datenverkehr (Back-End front-End)
11. Alles ablehnen

Der tatsächliche Firewall Regelsatz haben wahrscheinlich viele andere Regeln zusätzlich zu diesen, die Regeln an jedem angegebenen Firewall können auch andere Priorität Zahlen als hier nicht aufgeführt sind. Diese Liste und die zugehörigen Zahlen sind Relevanz zwischen nur diese 11 Regeln und dem sie die relative Priorität bereitstellen. Zählung ausnehmen; Klicken Sie auf die ist-Firewall möglicherweise die "RDP zu iis01 neu" Regel Zahl 5 sein, aber solange sie unterhalb der Regel "Firewall-Verwaltung" ist und über die Regel "RDP-DNS01" es Absicht, diese Liste ausrichten möchten. Die Liste wird auch als Hilfe für die unter Szenarien gleicht aus Platzgründen; z. B. "FW Regel 9 (DNS)". Auch aus Platzgründen die vier RDP Regeln gemeinsam aufgerufen, "die RDP Regeln" bei das Szenario Datenverkehr nicht im Zusammenhang mit RDP ist.

Auch Denken Sie daran, dass Netzwerk Sicherheitsgruppen in-situ sind für eingehenden Datenverkehrs in der Front-End- und Back-End-Subnetzen.

#### <a name="allowed-internet-to-web-server"></a>(Zulässig) Internet auf Webserver
1.  Internet Benutzer Anfragen HTTP-Seite aus SecSvc001.CloudApp.Net (Internet gegenüberliegende Cloud Service)
2.  Cloud-Dienst übergibt-Verkehr über öffnen Endpunkt auf Port 80 zu Firewall-Benutzeroberfläche 10.0.0.4:80
3.  Keine NSG Sicherheit Subnetz zugewiesen, also System NSG Regeln Netzwerkverkehr Firewall zulassen
4.  Datenverkehr Treffer interne IP-Adresse der Firewall (10.0.1.4)
5.  Firewall beginnt Regel Verarbeitung:
  1.    FW-Regel 1 (FW-Verwaltung) nicht anwenden, wechseln zur nächsten Regel
  2.    FW-Regeln 2 bis 5 (RDP Regeln) nicht anwenden, wechseln zur nächsten Regel
  3.    FW Regel 6 (App: Web) gilt, den Datenverkehr zulässig ist, Firewall NATs er 10.0.1.4 (iis01 neu)
6.  Das Front-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.    NSG Regel 1 (Internet blockieren) gilt nicht für (diesen Datenverkehr wurde NAT möchten, indem Sie die Firewall, sodass die Quellbilds jetzt die Firewall auf die Sicherheit Subnetz also und für die Front-End-Subnetz NSG "lokalen" Datenverkehr werden sichtbar und somit zulässig ist), wechseln zur nächsten Regel
  2.    Regeln für das standardmäßige NSG Subnetz zu Subnetz Datenverkehr zulassen, Datenverkehr ist zulässig, NSG Regel Verarbeitung beenden
7.  Iis01 neu hört für Datenverkehr im Web, empfängt diese Anforderung und Verarbeitung der Anforderung beginnt
8.  Iis01 neu Versuche, initiiert eine FTP-Sitzung zu AppVM01 Back-End-Subnetz
9.  Die UDR Routing Front-End-Subnetz macht der Firewall den nächsten Abschnitt
10. Keine Regeln für ausgehenden Front-End-Subnetz ist Datenverkehr zulässig.
11. Firewall beginnt Regel Verarbeitung:
  1.    FW-Regel 1 (FW-Verwaltung) nicht anwenden, wechseln zur nächsten Regel
  2.    FW-Regel 2 bis 5 (RDP Regeln) nicht anwenden, wechseln zur nächsten Regel
  3.    FW Regel 6 (App: Web) nicht anwenden, wechseln zur nächsten Regel
  4.    FW Regel 7 (App: Back-End-) gilt, den Datenverkehr ist zulässig, Firewall leitet den Datenverkehr in 10.0.2.5 (AppVM01)
12. Das Back-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.    NSG Regel 1 (Internet blockieren) nicht anwenden, wechseln zur nächsten Regel
  2.    Regeln für das standardmäßige NSG Subnetz zu Subnetz Datenverkehr zulassen, Datenverkehr ist zulässig, NSG Regel Verarbeitung beenden
13.  AppVM01 empfängt die Anforderung und leitet die Sitzung und reagiert
14. Die UDR Routing Back-End-Subnetz macht der Firewall den nächsten Abschnitt
15. Da es gibt keine ausgehenden NSG Regeln in die Back-End-Subnetz die Antwort ist zulässig.
16. Da dies den Datenverkehr auf eine eingerichteten Sitzung zurückgeben ist übergibt die Firewall die Antwort an den Webserver (iis01 neu)
17. Front-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.    NSG Regel 1 (Internet blockieren) nicht anwenden, wechseln zur nächsten Regel
  2.    Regeln für das standardmäßige NSG Subnetz zu Subnetz Datenverkehr zulassen, Datenverkehr ist zulässig, NSG Regel Verarbeitung beenden
18. IIS-Server empfängt die Antwort, wird die Transaktion mit AppVM01 abgeschlossen und führt anschließend die HTTP-Antwort erstellen, wird diese HTTP-Antwort an das jeweilige gesendet.
19. Da es gibt keine ausgehenden NSG Regeln die Antwort im Front-End-Subnetz ist zulässig.
20. HTTP-Antwort, da dies die Antwort auf eine definierte NAT Sitzung ist, wird durch die Firewall akzeptiert und Treffer die firewall
21. Die Firewall leitet die Antwort an den Internet-Benutzer
22. Da es gibt keine ausgehenden empfängt NSG Regeln oder UDR Abschnitte in der Front-End-Subnetz, die die Antwort zulässig ist, und der Benutzer Internet der Webseite angefordert.

#### <a name="allowed-internet-rdp-to-backend"></a>(Zulässig) Internet RDP zu Back-End-
1.  Serveradministrator Internet anfordert RDP-Sitzung zu AppVM01 über SecSvc001.CloudApp.Net:8025, wo finde ich 8025 die Benutzer zugewiesene Port-Nummer für die Firewall-Regel "RDP-AppVM01"
2.  Cloud-Dienst übergibt Verkehr über den Endpunkt geöffneten Port 8025 auf Firewall-Schnittstelle für 10.0.0.4:8025
3.  Keine NSG Sicherheit Subnetz zugewiesen, also System NSG Regeln Netzwerkverkehr Firewall zulassen
4.  Firewall beginnt Regel Verarbeitung:
  1.    FW-Regel 1 (FW-Verwaltung) nicht anwenden, wechseln zur nächsten Regel
  2.    FW-Regel 2 (RDP IIS) nicht anwenden, wechseln zur nächsten Regel
  3.    FW-Regel 3 (RDP DNS01) nicht anwenden, wechseln zur nächsten Regel
  4.    FW-Regel 4 (RDP AppVM01) wendet sich an, den Datenverkehr zulässig ist, Firewall NATs er 10.0.2.5:3386 (RDP-Anschluss AppVM01)
5.  Das Back-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.    NSG Regel 1 (Internet blockieren) gilt nicht für (diesen Datenverkehr wurde NAT möchten, indem Sie die Firewall, sodass die Quellbilds jetzt die Firewall auf die Sicherheit Subnetz also und für die Back-End-Subnetz NSG "lokalen" Datenverkehr werden sichtbar und somit zulässig ist), wechseln zur nächsten Regel
  2.    Regeln für das standardmäßige NSG Subnetz zu Subnetz Datenverkehr zulassen, Datenverkehr ist zulässig, NSG Regel Verarbeitung beenden
6.  AppVM01 ist RDP Datenverkehr überwachen und reagiert
7.  Anwenden von Regeln für das standardmäßige keine Regeln für ausgehenden NSG und wiederholten Datenverkehr ist zulässig.
8.  UDR leitet ausgehenden Datenverkehr an die Firewall als den nächsten Abschnitt
9.  Da dies den Datenverkehr auf eine eingerichteten Sitzung zurückgeben ist übergibt die Firewall die Antwort an den Benutzer internet
10. RDP-Sitzung ist aktiviert.
11. AppVM01 aufgefordert, Benutzernamenkennwort

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Zulässig) Nachschlagen von Web Server DNS-Einträge auf DNS-server
1.  Web-Server, iis01 neu, einem am www.data.gov Datenfeed, Anforderungen, jedoch muss die Adresse aufgelöst werden.
2.  Die Konfiguration für die Listen VNet DNS01 (10.0.2.4 in die Back-End-Subnetz) als primärer DNS-Server, iis01 neu sendet die DNS-Anforderung an DNS01
3.  UDR leitet ausgehenden Datenverkehr an die Firewall als den nächsten Abschnitt
4.  Keine ausgehenden NSG Regeln mit dem Front-End-Subnetz gebunden sind, wird Datenverkehr zugelassen werden.
5.  Firewall beginnt Regel Verarbeitung:
  1.    FW-Regel 1 (FW-Verwaltung) nicht anwenden, wechseln zur nächsten Regel
  2.    FW-Regel 2 bis 5 (RDP Regeln) nicht anwenden, wechseln zur nächsten Regel
  3.    FW Regeln 6 und 7 (App Regeln) nicht anwenden, wechseln zur nächsten Regel
  4.    FW Regel 8 (zum Internet) nicht anwenden, wechseln zur nächsten Regel
  5.    FW Regel 9 (DNS) wendet sich an den Datenverkehr ist zulässig, Firewall leitet den Datenverkehr in 10.0.2.4 (DNS01)
6.  Das Back-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.    NSG Regel 1 (Internet blockieren) nicht anwenden, wechseln zur nächsten Regel
  2.    Regeln für das standardmäßige NSG Subnetz zu Subnetz Datenverkehr zulassen, Datenverkehr ist zulässig, NSG Regel Verarbeitung beenden
7.  DNS-Server empfängt die Anforderung
8.  DNS-Server verfügt nicht über die Adresse Cache und gefragt werden, einen Stamm-DNS-Server im Internet
9.  UDR leitet ausgehenden Datenverkehr an die Firewall als den nächsten Abschnitt
10. Keine ausgehenden NSG Regeln Back-End-Subnetz, ist den Datenverkehr zulässig.
11. Firewall beginnt Regel Verarbeitung:
  1.    FW-Regel 1 (FW-Verwaltung) nicht anwenden, wechseln zur nächsten Regel
  2.    FW-Regel 2 bis 5 (RDP Regeln) nicht anwenden, wechseln zur nächsten Regel
  3.    FW Regeln 6 und 7 (App Regeln) nicht anwenden, wechseln zur nächsten Regel
  4.     FW Regel 8 (zum Internet) gilt, den Datenverkehr ist zulässig, Sitzung ist SNAT, Stamm-DNS-Server im Internet
12. Internet-DNS-Server reagiert, da dieser Sitzung über die Firewall initiiert wurde, wird die Antwort von der Firewall akzeptiert
13. Wie folgt ein eingerichteten Sitzung ist, leitet die Firewall die Antwort an den auslösenden Server, DNS01
14. Das Back-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.    NSG Regel 1 (Internet blockieren) nicht anwenden, wechseln zur nächsten Regel
  2.    Regeln für das standardmäßige NSG Subnetz zu Subnetz Datenverkehr zulassen, Datenverkehr ist zulässig, NSG Regel Verarbeitung beenden
15. Der DNS-Server empfängt und speichert die Antwort zwischen, und klicken Sie dann die ursprüngliche Anfrage wieder iis01 neu beantwortet
16. Die UDR Routing Back-End-Subnetz macht der Firewall den nächsten Abschnitt
17. Keine ausgehenden NSG Regeln in die Back-End-Subnetz vorhanden ist, wird Datenverkehr zugelassen werden.
18. Dies ist eine eingerichteten Sitzung auf die Firewall, die Antwort wird durch die Firewall wieder auf dem IIS-Server weitergeleitet.
19. Front-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.    Es gibt keine NSG Regel, die gilt für eingehende Datenverkehr aus dem Back-End-Subnetz mit dem Front-End-Subnetz, damit keines der NSG Anwenden von Regeln
  2.    Die Standard-System-Regel zulassen des Datenverkehrs zwischen Subnetzen würde diesen Datenverkehr zulassen, damit der Datenverkehr zugelassen wird
20. Iis01 neu empfängt die Antwort von DNS01

#### <a name="allowed-backend-server-to-frontend-server"></a>(Zulässig) Back-End-Server und Front-End-server
1.  Ein Administrator angemeldet AppVM02 über RDP fordert eine Datei direkt vom Server iis01 neu über Windows-Datei-explorer
2.  Die UDR Routing Back-End-Subnetz macht der Firewall den nächsten Abschnitt
3.  Da es gibt keine ausgehenden NSG Regeln in die Back-End-Subnetz die Antwort ist zulässig.
4.  Firewall beginnt Regel Verarbeitung:
  1.    FW-Regel 1 (FW-Verwaltung) nicht anwenden, wechseln zur nächsten Regel
  2.    FW-Regel 2 bis 5 (RDP Regeln) nicht anwenden, wechseln zur nächsten Regel
  3.    FW Regeln 6 und 7 (App Regeln) nicht anwenden, wechseln zur nächsten Regel
  4.    FW Regel 8 (zum Internet) nicht anwenden, wechseln zur nächsten Regel
  5.    FW Regel 9 (DNS) nicht anwenden, wechseln zur nächsten Regel
  6.    FW-Regel-10 (innerhalb-Subnetz) gilt, Datenverkehr ist zulässig, Firewall übergibt den Datenverkehr an 10.0.1.4 (iis01 neu)
5.  Front-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.    NSG Regel 1 (Internet blockieren) nicht anwenden, wechseln zur nächsten Regel
  2.    Regeln für das standardmäßige NSG Subnetz zu Subnetz Datenverkehr zulassen, Datenverkehr ist zulässig, NSG Regel Verarbeitung beenden
6.  Sofern ordnungsgemäße Authentifizierung und Autorisierung, iis01 neu die Anforderung akzeptiert und reagiert
7.  Die UDR Routing Front-End-Subnetz macht der Firewall den nächsten Abschnitt
8.  Da es gibt keine ausgehenden NSG Regeln die Antwort im Front-End-Subnetz ist zulässig.
9.  Wie hier eine vorhandene Sitzung auf die Firewall diese Antwort zulässig ist und die Firewall gibt die Antwort auf AppVM02
10. Back-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.    NSG Regel 1 (Internet blockieren) nicht anwenden, wechseln zur nächsten Regel
  2.    Regeln für das standardmäßige NSG Subnetz zu Subnetz Datenverkehr zulassen, Datenverkehr ist zulässig, NSG Regel Verarbeitung beenden
11. AppVM02 empfängt die Antwort

#### <a name="denied-internet-direct-to-web-server"></a>(Verweigert) Internet direkt an Webserver
1.  Internet-Benutzer versucht, den Webserver, iis01 neu, über den Dienst FrontEnd001.CloudApp.Net zugreifen
2.  Da keine Endpunkte für HTTP-Datenverkehr geöffnet sind, wird dies würde über den Cloud-Dienst nicht bestanden und würde nicht erreicht haben, ab dem server
3.  Wenn die Endpunkte aus irgendeinem Grund geöffnet und Sie sind möchten die NSG (Internet blockieren) im Front-End-Subnetz diesen Datenverkehr blockiert.
4.  Schließlich würde der Front-End-Subnetz UDR Routing alle ausgehenden Datenverkehr die Firewall als den nächsten Abschnitt aus iis01 neu senden, und die Firewall sehen Sie dies als asymmetrische Datenverkehr und legen Sie die ausgehende Antwort, es gibt also mindestens drei unabhängige Sicherheitsebenen zwischen dem Internet und iis01 neu über deren Cloud-Dienst nicht autorisiert/unangemessene Access verhindern möchten.

#### <a name="denied-internet-to-backend-server"></a>(Verweigert) Internet und Back-End-Server
1.  Internet-Benutzer versucht, eine Datei auf AppVM01 über die BackEnd001.CloudApp.Net Dienst zugreifen
2.  Da keine Endpunkte für Dateifreigabe geöffnet sind, wird dies würde Cloud-Dienst nicht bestanden und würde nicht erreicht haben, ab dem server
3.  Wenn die Endpunkte aus irgendeinem Grund geöffnet und Sie sind möchten die NSG (Internet blockieren) diesen Datenverkehr blockiert.
4.  Schließlich würde die UDR Routing alle ausgehenden Datenverkehr die Firewall als den nächsten Abschnitt aus AppVM01 senden, und die Firewall sehen Sie dies als asymmetrische Datenverkehr und legen Sie die ausgehende Antwort, es gibt also mindestens drei unabhängige Sicherheitsebenen zwischen dem Internet und AppVM01 über deren Cloud-Dienst nicht autorisiert/unangemessene Access verhindern möchten.

#### <a name="denied-frontend-server-to-backend-server"></a>(Verweigert) Front-End-Server und Back-End-Server
1.  Nehmen Sie an iis01 neu gefährdet wurde und bösartigen Code beim Scannen die Back-End-Subnetz Server ausgeführt wird.
2.  Der Front-End-Subnetz UDR weiterleiten möchten alle ausgehenden Datenverkehr iis01 neu an die Firewall als den nächsten Abschnitt senden. Dies ist nicht für ein Element, das durch den betroffenen virtuellen Computer geändert werden können.
3.  Die Firewall würde den Datenverkehr verarbeiten, wenn die Anforderung wurde AppVM01 oder der DNS-Server für die DNS-suchen, die Datenverkehr durch die Firewall (aufgrund FW Regeln 7 und 9) potenziell gewährt werden konnte. Alle anderen Datenverkehr würde durch FW Regel 11 (alle verweigern) blockiert.
4.  Wenn Sie erweiterte Erkennung auf die Firewall aktiviert wurde (die wird nicht in diesem Dokument behandelt, finden Sie unter Dokumentation des Herstellers für Ihre bestimmte Netzwerkeinheit erweiterte Funktionen Bedrohung) gerade Datenverkehr, die durch die grundlegende Weiterleitungsregeln in diesem Dokument beschrieben zulässig sind konnte verhindert werden, wenn der Datenverkehr enthalten bekannte Signaturen oder Mustern, die eine Regel hochentwickelter gekennzeichnet.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Verweigert) Nachschlagen von Internet-DNS-Einträge auf DNS-server
1.  Internet-Benutzer versucht, einen internen DNS-Eintrag auf DNS01 über BackEnd001.CloudApp.Net Dienst nachschlagen 
2.  Da keine Endpunkte DNS-Verkehr geöffnet sind, wird dies würde über den Cloud-Dienst nicht bestanden und würde nicht erreicht haben, ab dem server
3.  Wenn die Endpunkte aus irgendeinem Grund geöffnet und Sie sind möchten die NSG Regel (Internet blockieren) im Front-End-Subnetz diesen Datenverkehr blockiert.
4.  Schließlich würde die Back-End-Subnetz UDR Routing alle ausgehenden Datenverkehr die Firewall als den nächsten Abschnitt aus DNS01 senden, und die Firewall sehen Sie dies als asymmetrische Datenverkehr und legen Sie die ausgehende Antwort, es gibt also mindestens drei unabhängige Sicherheitsebenen zwischen dem Internet und DNS01 über deren Cloud-Dienst nicht autorisiert/unangemessene Access verhindern möchten.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Verweigert) Internet auf SQL-Zugriff über die Firewall
1.  Internet-Benutzers anfordert SQL-Daten aus SecSvc001.CloudApp.Net (Internet gegenüberliegende Cloud Service)
2.  Da keine Endpunkte für SQL geöffnet sind, wird dies würde Cloud-Dienst nicht bestanden und würde nicht erreicht haben, ab der firewall
3.  Wenn SQL-Endpunkte aus irgendeinem Grund geöffnet und Sie sind möchten die Firewall Regel Verarbeitung beginnen:
  1.    FW-Regel 1 (FW-Verwaltung) nicht anwenden, wechseln zur nächsten Regel
  2.    FW-Regeln 2 bis 5 (RDP Regeln) nicht anwenden, wechseln zur nächsten Regel
  3.    FW Regel 6 und 7 (Anwendungsregeln) nicht anwenden, wechseln zur nächsten Regel
  4.    FW Regel 8 (zum Internet) nicht anwenden, wechseln zur nächsten Regel
  5.    FW Regel 9 (DNS) nicht anwenden, wechseln zur nächsten Regel
  6.    FW-Regel-10 (innerhalb-Subnetz) nicht anwenden, wechseln zur nächsten Regel
  7.    FW-Regel 11 (alle verweigern) gilt, Datenverkehr ist blockiert, Anhalten Regel Verarbeitung


## <a name="references"></a>Verweise
### <a name="main-script-and-network-config"></a>Hauptfenster Skripts und Netzwerk Config
Speichern Sie das vollständige Skript in einer PowerShell-Skriptdatei aus. Speichern Sie das Netzwerk Config in einer Datei namens "NetworkConf2.xml" ein.
Ändern Sie die benutzerdefinierte Variablen nach Bedarf. Führen Sie das Skript, und führen Sie die Firewall-Regel-Setup-Anweisung oben.

#### <a name="full-script"></a>Vollständiges Skript
Dieses Skript wird, basierend auf die benutzerdefinierte Variablen an:

1.  Verbinden Sie mit einem Azure-Abonnement
2.  Erstellen eines neuen Kontos mit Speicher
3.  Erstellen einer neuen VNet und drei Subnetze definierten in der Netzwerk-Konfigurationsdatei
4.  Erstellen von fünf virtuellen Computern Firewall 1 und 4 WindowsServer virtuellen Computern
5.  Konfigurieren von UDR einschließlich:
  1.    Erstellen zwei neue Routing-Tabellen
  2.    Leitet den Tabellen hinzufügen
  3.    Binden von Tabellen in der jeweiligen subnets
6.  Aktivieren Sie die IP-Weiterleitung auf der NVA
7.  Konfigurieren von NSG einschließlich:
  1.    Erstellen einer NSG
  2.    Hinzufügen einer Regel
  3.    Binden der NSG zu den entsprechenden Subnetzen

Diese PowerShell-Skript sollte lokal auf ausgeführt werden, dass eine Internet PC oder Server verbunden.

>[AZURE.IMPORTANT] Wenn dieses Skript ausgeführt wird, es möglicherweise Warnungen oder anderen informativen Nachrichten, die in PowerShell pop. Nur Fehlermeldungen in Rot sind Anlass zur Sorge.

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - IP Forwading from the FireWall out to the internet
       - User Defined Routing FrontEnd and BackEnd Subnets to the NVA
    
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind the appropriate
      layer(s) of protection. This script serves as an example of some of the techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and the appropriate protections needed, and then effectively
      implement those protections.
    
      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4
     
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4
     
      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6
    
    #>
    
    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()
    
    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section
    
      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    
      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"
    
      # Service Details
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be the NVA.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"
    
    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #
    
      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop
    
      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}
    
      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop
    
    # Validation
    $FatalError = $false
    
    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}
    
    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "The SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use" -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}
    
    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}
    
    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}
    
    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop
    
    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: All traffic goes through the firewall, so we'll need to set up all ports here.
                #       Also, the firewall will be redirecting traffic to a new IP and Port in a forwarding
                #       rule, so all of these endpoint have the same public and local port and the firewall
                #       will do the mapping, NATing, and/or redirection as declared in the firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }
    
    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan
    
      # Create the Route Tables
        Write-Host "Creating the Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"
    
      # Add Routes to Route Tables
        Write-Host "Adding Routes to the Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal
    
      # Assoicate the Route Tables with the Subnets
        Write-Host "Binding Route Tables to the Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName
    
     # Enable IP Forwarding on the Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
    
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rule
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
      # Assign the NSG to two Subnets
        # The NSG is *not* bound to the Security Subnet. The result
        # is that internet traffic flows only to the Security subnet
        # since the NSG bound to the Frontend and Backback subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
    

#### <a name="network-config-file"></a>Konfigurationsdatei Netzwerk
Fügen Sie einer Verknüpfung zu dieser Datei, um die Variable $NetworkConfigFile im obigen Skript hinzu, und speichern Sie diese XML-Datei mit aktualisierten Speicherort.

    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Beispiel für Anwendungsskripts
Wenn Sie die Anwendung einer Stichprobe für diese und andere Beispiele DMZ installieren möchten, eine bereitgestellt wurde unter folgendem Link: [Beispiel-Anwendung-Skript][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Bidirektionale DMZ mit NVA, NSG und UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Logische Ansicht der Firewall-Regeln"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Erstellen Sie ein Objekt Front-End-Netzwerk"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Erstellen eines DNS-Server-Objekts"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Kopieren der standardmäßigen RDP Regel"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 Regel"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Redirect Anwendungssymbol"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Ziel NAT-Symbol"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Pass-Symbol"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Firewall Management-Regel"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "Firewall RDP-Regel"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Firewall-Web-Regel"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Firewall AppVM01 Regel"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Ausgehende Firewall-Regel"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "Firewall DNS-Regel"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Firewall innerhalb VNet Regel"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Firewall verweigern Regel"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Aktivierung der Firewall-Regel"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
