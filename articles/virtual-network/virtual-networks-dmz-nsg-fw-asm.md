<properties
   pageTitle="DMZ-Beispiel – erstellen eine DMZ zum Schützen von Applications mit einer Firewall und NSGs | Microsoft Azure"
   description="Erstellen eine DMZ mit einer Firewall und Netzwerk-Sicherheitsgruppen (NSG)"
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

# <a name="example-2--build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs"></a>Beispiel 2: erstellen eine DMZ zum Schützen von Applications mit einer Firewall und NSGs

[Kehren Sie zur Sicherheit Begrenzungslinie bewährte Methoden für die Seite][HOME]

In diesem Beispiel wird eine DMZ durch eine Firewall, vier Windows-Servern und Netzwerk Sicherheitsgruppen erstellen. Es werden auch die relevanten Befehle zum Bereitstellen von eines besseren Verständnis der einzelnen Schritte durchzuführen. Es gibt auch ein Abschnitt Datenverkehr Szenario einen detaillierten bereitstellen schrittweise wie Verkehr über die Sicherheitsebenen in der DMZ fortgesetzt wird. Schließlich ist Abschnitt in die Bezüge der vollständige Code und Anweisung-Umgebung, um zu testen, und experimentieren Sie mit verschiedenen Szenarios erstellen. 

![Eingehende DMZ mit NVA und NSG][1]

## <a name="environment-description"></a>Beschreibung der Umgebung
In diesem Beispiel ist ein Abonnement, die folgenden enthält:

- Zwei cloud Services: "FrontEnd001" und "BackEnd001"
- Ein virtuelles Netzwerk "CorpNetwork" mit unterschiedlichen zwei: "Front-End" und "Back-End"
- Eine einzelne Netzwerk-Sicherheitsgruppe, die auf beide Subnetze angewendet wird
- Eine virtuelle Netzwerkeinheit in diesem Beispiel wird eine Firewall Barracuda NextGen, mit dem Front-End-Subnetz verbunden
- Einen Windows-Server, der einen Anwendung Webserver iis01 ("Neu")
- Zwei Windows-Servern, die Anwendung wieder darstellen beenden Servers ("AppVM01", "AppVM02")
- Einen Windows-Server, der einen DNS-Server ("DNS01")

>[AZURE.NOTE] Obwohl in diesem Beispiel wird eine Barracuda NextGen Firewall verwendet wird, könnten viele der anderen virtuellen Netzwerkgeräte in diesem Beispiel verwendet werden.

Im Abschnitt "Verweise" unter ist ein PowerShell-Skript, die meisten der oben beschriebenen Umgebung zu erstellen, wird es. Erstellen der virtuellen Computern und virtuelle Netzwerke zwar durch das Beispielskript fertig sind nicht im ausführlich beschrieben in diesem Dokument.
 
Um die Umgebung zu erstellen:

  1.    Speichern Sie die Network Config XML-Datei im Abschnitt Verweise enthalten (aktualisiert mit Namen, Speicherort und IP-Adressen entsprechend das angegebene Szenario)
  2.    Aktualisieren Sie die Benutzervariablen in das Skript entsprechend die Umgebung, die das Skript ist gegen (Abonnements, Dienstnamen usw.) ausgeführt werden soll
  3.    Führen Sie das Skript in PowerShell

**Hinweis**: die Region erhält, der in der PowerShell-Skript muss die Region erhält, der in der XML-Netzwerk Konfigurationsdatei übereinstimmen.

Nachdem das Skript erfolgreich ausgeführt wird möglicherweise die folgenden Skript-Schritte ausgeführt werden:

1.  Richten Sie die Firewallregeln, ist dies in weiter unten im Abschnitt abgedeckt: Firewall-Regeln.
2.  Optional sind im Abschnitt Verweise zwei Skripts zum Einrichten der Webserver und der app-Server mit einer einfachen Webanwendung zum Testen mit dieser Konfiguration DMZ zulassen.

Im nächste Abschnitt wird erläutert, die meisten der Skripts Aussagen relativ zum Netzwerk Sicherheitsgruppen.

## <a name="network-security-groups-nsg"></a>Netzwerk-Sicherheitsgruppen (NSG)
In diesem Beispiel wird eine Gruppe NSG erstellt, und klicken Sie dann mit sechs Regeln geladen. 

>[AZURE.TIP] Im Allgemeinen sollten Sie bestimmten "Zulassen" Regeln zuerst erstellen und die eher generischen "Verweigern" Regeln und dann nach Nachname. Das zugewiesene Priorität bestimmt, welcher die Regeln werden ausgewertet ersten. Nachdem Sie den Datenverkehr auf einer bestimmten Regel anzuwendende gefunden wird, werden keine weiteren Regeln ausgewertet. NSG Regeln können in entweder in die Richtung eingehenden oder ausgehenden (hinsichtlich der im Subnetz) anwenden.

Deklarativ, sind die folgenden Regeln für eingehenden Datenverkehr erstellt wird:

1.  Internen DNS-Verkehr (Port 53) ist zulässig.
2.  RDP-Datenverkehr (Port 3389) aus dem Internet zu einem beliebigen virtuellen Computer ist zulässig.
3.  HTTP-Datenverkehr (Port 80) aus dem Internet an den NVA (Firewall) ist zulässig.
4.  Alle Verkehr (alle Ports) von iis01 neu zu AppVM1 ist zulässig.
5.  Alle Datenverkehr (alle Ports) aus dem Internet auf das ganze verweigert VNet (beide Subnetze)
6.  Alle Datenverkehr (alle Ports) aus dem Front-End-Subnetz mit dem Back-End-Subnetz verweigert

Mit dieser Regeln gebunden für jedes Subnetz, wenn eine HTTP-Anforderung aus dem Internet an den Webserver, beide Regeln 3 eingehenden wurde (zulassen) und 5 (ablehnen) anwenden möchten, aber da Regel 3 eine höhere Priorität hat nur sie anwenden möchten, und Regel 5 nicht ins Spiel kommen würde. Daher würde die HTTP-Anforderung an die Firewall gewährt werden. Wenn die gleiche Datenverkehr an den Server DNS01 zu erreichen versucht hat, Regel 5 (verweigern) wäre das erste anwenden und der Datenverkehr zulässig wäre nicht auf dem Server zu übergeben. Regel 6 (verweigern) blockiert des Front-End-Subnetzes aus ein Gespräch mit dem Back-End-Subnetz (mit Ausnahme der zulässige Datenverkehr in Regeln 1 und 4), die auf diese Weise dem Back-End-Netzwerk im Fall die Webanwendung auf der Front-End, der Angreifer Zugriff auf die Back-End-"geschützte" Netzwerk (nur für die Ressourcen bereitgestellt, die auf dem Server AppVM01) zugänglich gemacht würde immer geschützt.

Es gibt eine standardmäßige ausgehende Regel, die Datenverkehr, mit dem Internet zulässt. In diesem Beispiel haben wir zuzulassen ausgehenden Datenverkehr und alle ausgehenden Regeln nicht ändern. Zum Sperren von Datenverkehr in beide Richtungen, Benutzer Routing definiert ist erforderlich, wurde untersucht, in einem anderen Beispiel, bei dem können im [Hauptfenster Sicherheit Begrenzungslinie Dokument]gefundenen[HOME].

Die oben genannten erwähnt NSG Regeln sind sehr ähnlich NSG Regeln in [Beispiel 1: Erstellen einer einfachen DMZ mit NSGs][Example1]. Überprüfen Sie die NSG Beschreibung in diesem Dokument für eine ausführliche Betrachtung jede Regel NSG und seine Attribute an.

## <a name="firewall-rules"></a>Firewall-Regeln
Management-Client auf einem PC verwalten die Firewall, und erstellen die erforderlichen Konfigurationen installiert werden müssen. Finden Sie unter den Hersteller Dokumentation aus Ihrem Firewall (oder andere NVA) zum Verwalten des Geräts ein. Die Konfiguration der Firewall selbst, bis der Lieferanten Management-Client (d. h. nicht Azure-Portal oder PowerShell) wird in der Rest der in diesem Abschnitt beschrieben.

Anweisungen für den Download durch Clients und Herstellen einer Verbindung mit den in diesem Beispiel verwendete Barracuda finden Sie hier: [Barracuda von Administrator](https://techlib.barracuda.com/NG61/NGAdmin)

Klicken Sie auf die Firewall müssen Regeln weiterleiten erstellt werden. Da in diesem Beispiel wird nur den Internetdatenverkehr in gebundene an die Firewall, und klicken Sie dann auf den Webserver weiterleiten, ist nur eine Weiterleitung NAT Regel erforderlich. Klicken Sie auf die in diesem Beispiel verwendete Barracuda NextGen-Firewall wäre die Regel eine Regel Ziel NAT ("Ziel NAT") auf diese Pakete geleitet.

Wenn Sie die folgende Regel erstellen (oder vorhandenen Regeln für das standardmäßige überprüfen), beginnend mit dem Dashboard Barracuda NG Admin-Client, navigieren Sie zur Konfigurationsregisterkarte, klicken Sie in der Konfiguration Betrieb Abschnitt auf Regelsatz. Einem Raster bezeichnet, wird auf der Firewall "Primär Regeln" die vorhandenen aktivierte und deaktivierten Regeln angezeigt. In der oberen rechten Ecke dieses Rasters ist eine kleine, Grün "+" Schaltfläche, klicken Sie auf diese Option, um eine neue Regel erstellen (Notiz: Ihre Firewall möglicherweise "gesperrt" Änderungen, wenn Sie eine Schaltfläche gekennzeichnet "Sperren", und Sie zum Erstellen oder Bearbeiten von Regeln klicken Sie auf diese Schaltfläche, um den Regelsatz "Entsperren" und Bearbeitung erlauben erfolgreiche angezeigt). Wenn Sie eine vorhandene Regel bearbeiten möchten, wählen Sie die Regel, mit der rechten Maustaste, und wählen Sie Regel bearbeiten aus.

Erstellen Sie eine neue Regel, und geben Sie einen Namen ein, beispielsweise "WebTraffic". 

Das Ziel NAT Regelsymbol sieht wie folgt aus: ![Ziel NAT-Symbol][2]

Die Regel selbst sieht ungefähr wie folgt aus:

![Firewall-Regel][3]

Hier eine Adresse, die die Firewall Treffer eingehende HTTP (Port 80 oder 443 für HTTPS) zu erreichen versucht wird, die Firewall "DHCP1 lokale IP-" Benutzeroberfläche gesendet und an den Webserver mit der IP-Adresse des 10.0.1.5 umgeleitet werden. Da der Datenverkehr auf Port 80 eingehenden und ausgehenden mit dem Webserver, auf Port 80 ist wurde keine Änderung Port erforderlich. Jedoch hätte die Zielliste 10.0.1.5:8080 Wenn unsere Webserver Port 8080 somit übersetzen den eingehenden Port 80 auf der Firewall für eingehende Port 8080 auf dem Webserver überwacht.

Eine Verbindungsmethode sollte auch, für die Ziel-Regel aus dem Internet, erhält, werden "Dynamische SNAT" am besten geeignet ist. 

Obwohl nur eine Regel erstellt wurde, ist es wichtig, dessen Priorität korrekt festgelegt ist. Ist im Raster alle Regeln auf die Firewall diese neue Regel an den Fuß (unterhalb der Regel "BLOCKALL"), wird es nie ins Spiel kommen. Stellen Sie sicher, dass die neu erstellte Regel für Web-Verkehr über die Regel BLOCKALL ist.

Sobald die Regel erstellt, muss mit dem Firewall abgelegt und dann aktiviert, wenn dies nicht der Fall ist die Regel Änderung nicht wirksam. Der Prozess Pushbenachrichtigungen und-Aktivierung wird im nächsten Abschnitt beschrieben.

## <a name="rule-activation"></a>Regel Aktivierung
Mit der Regelsatz geändert, damit diese Regel hinzufügen muss der Regelsatz die Firewall geladen und aktiviert werden.

![Aktivierung der Firewall-Regel][4]

Sind Sie ein Cluster von Schaltflächen in der oberen rechten Ecke des Management-Clients. Klicken Sie auf die Schaltfläche "Änderungen senden", um die geänderten Regeln an die Firewall zu senden, und klicken Sie auf die Schaltfläche "Aktivieren".

Bei der Aktivierung von der Firewall Regelsatz Abschluss dieses Beispiel-Umgebung erstellen. Optional können die Skripts Beitrag erstellen im Abschnitt Verweise zum Hinzufügen einer Anwendung zu dieser Umgebung testen Ausführen der unter den Datenverkehr Szenarien.

>[AZURE.IMPORTANT] Es ist entscheidend, erkennen, dass Sie den Webserver nicht direkt erreicht werden. Wenn ein Browser eine HTTP-Seite aus FrontEnd001.CloudApp.Net anfordert, übergibt der HTTP-Endpunkt (Port 80) diesen Datenverkehr die Firewall nicht den Webserver. Klicken Sie dann die Firewall gemäß der Regel erstellt, über NATs, die an den Webserver anfordern.

## <a name="traffic-scenarios"></a>Datenverkehr Szenarien

#### <a name="allowed-web-to-web-server-through-firewall"></a>(Zulässig) Web auf Webserver durch die Firewall
1.  Internet Benutzer Anfragen HTTP-Seite aus FrontEnd001.CloudApp.Net (Internet gegenüberliegende Cloud Service)
2.  Cloud-Dienst übergibt-Verkehr über öffnen Endpunkt auf Port 80 an lokale Firewall-Schnittstelle 10.0.1.4:80
3.  Front-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.    NSG Regel 1 (DNS) nicht anwenden, wechseln zur nächsten Regel
  2.    NSG Regel 2 (RDP) nicht anwenden, wechseln zur nächsten Regel
  3.    NSG Regel 3 (Internet zur Firewall) gilt, den Datenverkehr ist eine zulässige, Anhalten Regel Verarbeitung
4.  Datenverkehr Treffer interne IP-Adresse der Firewall (10.0.1.4)
5.  Firewall Weiterleitungsregel finden Sie unter Dies ist Port 80 Datenverkehr, leitet es an den Webserver iis01 neu
6.  Iis01 neu hört für Datenverkehr im Web, empfängt diese Anforderung und Verarbeitung der Anforderung beginnt
7.  Iis01 neu fordert den SQL-Server auf AppVM01 Informationen
8.  Keine Regeln für ausgehenden Front-End-Subnetz ist Datenverkehr zulässig.
9.  Das Back-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.    NSG Regel 1 (DNS) nicht anwenden, wechseln zur nächsten Regel
  2.    NSG Regel 2 (RDP) nicht anwenden, wechseln zur nächsten Regel
  3.    NSG Regel 3 (Internet zur Firewall) nicht anwenden, wechseln zur nächsten Regel
  4.    NSG Regel 4 (iis01 neu zu AppVM01) wendet sich an den Datenverkehr ist zulässig, Regel Verarbeitung beenden
10. AppVM01 der SQL-Abfrage empfängt und beantwortet
11. Da es keine ausgehenden Regeln in die Back-End-Subnetz sind ist die Antwort zulässig.
12. Front-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.    Es gibt keine NSG Regel, die gilt für eingehende Datenverkehr aus dem Back-End-Subnetz mit dem Front-End-Subnetz, damit keines der NSG Anwenden von Regeln
  2.    Die Standard-System-Regel zulassen des Datenverkehrs zwischen Subnetzen würde dieses Datenverkehrs gestatten der Datenverkehr zulässig ist.
13. IIS-Server empfängt die Antwort SQL schließt die HTTP-Antwort und sendet an das jeweilige
14. Da dies eine Sitzung NAT von der Firewall ist, ist das Antwortziel (Anfangs) für die Firewall
15. Die Firewall empfängt die Antwort vom Webserver und die Nachricht an den Internet-Benutzer
16. Da es gibt keine ausgehenden Regeln die Antwort im Front-End-Subnetz zulässig ist und der Benutzer Internet empfängt der Webseite angefordert.

#### <a name="allowed-rdp-to-backend"></a>(Zulässig) RDP zu Back-End-
1.  Serveradministrator Internet anfordert RDP-Sitzung zu AppVM01 auf BackEnd001.CloudApp.Net:xxxxx, wo Xxxxx zufällig zugewiesenen Anschluss für RDP zu AppVM01 Zahl (der zugeordnete Port kann Azure-Portal oder über PowerShell gefunden werden)
2.  Da die Firewall nur die Adresse FrontEnd001.CloudApp.Net überwacht wird, es spielt keine Rolle mit diesem Datenfluss
3.  Back-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.    NSG Regel 1 (DNS) nicht anwenden, wechseln zur nächsten Regel
  2.    NSG Regel 2 (RDP) gilt, den Datenverkehr ist eine zulässige, Anhalten Regel Verarbeitung
4.  Anwenden von Regeln für das standardmäßige keine Regeln für ausgehenden und wiederholten Datenverkehr ist zulässig.
5.  RDP-Sitzung ist aktiviert.
6.  AppVM01 aufgefordert, Benutzernamenkennwort

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Zulässig) Nachschlagen von Web Server DNS-Einträge auf DNS-server
1.  Web-Server, iis01 neu, einem am www.data.gov Datenfeed, Anforderungen, jedoch muss die Adresse aufgelöst werden.
2.  Die Konfiguration für die Listen VNet DNS01 (10.0.2.4 in die Back-End-Subnetz) als primärer DNS-Server, iis01 neu sendet die DNS-Anforderung an DNS01
3.  Keine Regeln für ausgehenden Front-End-Subnetz ist Datenverkehr zulässig.
4.  Back-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.     NSG Regel 1 (DNS) gilt, den Datenverkehr ist eine zulässige, Anhalten Regel Verarbeitung
5.  DNS-Server empfängt die Anforderung
6.  DNS-Server verfügt nicht über die Adresse Cache und gefragt werden, einen Stamm-DNS-Server im Internet
7.  Keine Regeln für ausgehenden Back-End-Subnetz ist Datenverkehr zulässig.
8.  Internet-DNS-Server reagiert, da dieser Sitzung intern eingeleitet wurde, wird die Antwort zulässig.
9.  DNS-Server speichert die Antwort zwischen und reagiert auf die ursprüngliche Anfrage wieder iis01 neu
10. Keine Regeln für ausgehenden Back-End-Subnetz ist Datenverkehr zulässig.
11. Front-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.    Es gibt keine NSG Regel, die gilt für eingehende Datenverkehr aus dem Back-End-Subnetz mit dem Front-End-Subnetz, damit keines der NSG Anwenden von Regeln
  2.    Die Standard-System-Regel zulassen des Datenverkehrs zwischen Subnetzen würde diesen Datenverkehr zulassen, damit der Datenverkehr zugelassen wird
12. Iis01 neu empfängt die Antwort von DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(Zulässig) Web Access-Datei auf AppVM01 Server
1.  Iis01 neu gefragt werden, für eine Datei auf AppVM01
2.  Keine Regeln für ausgehenden Front-End-Subnetz ist Datenverkehr zulässig.
3.  Das Back-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.    NSG Regel 1 (DNS) nicht anwenden, wechseln zur nächsten Regel
  2.    NSG Regel 2 (RDP) nicht anwenden, wechseln zur nächsten Regel
  3.    NSG Regel 3 (Internet zur Firewall) nicht anwenden, wechseln zur nächsten Regel
  4.    NSG Regel 4 (iis01 neu zu AppVM01) wendet sich an den Datenverkehr ist zulässig, Regel Verarbeitung beenden
4.  AppVM01 empfängt die Anforderung und reagiert mit Datei (vorausgesetzt, dass ein Zugriff autorisiert ist.)
5.  Da es keine ausgehenden Regeln in die Back-End-Subnetz sind ist die Antwort zulässig.
6.  Front-End-Subnetz beginnt eingehende Regel Verarbeitung:
  1.    Es gibt keine NSG Regel, die gilt für eingehende Datenverkehr aus dem Back-End-Subnetz mit dem Front-End-Subnetz, damit keines der NSG Anwenden von Regeln
  2.    Die Standard-System-Regel zulassen des Datenverkehrs zwischen Subnetzen würde dieses Datenverkehrs gestatten der Datenverkehr zulässig ist.
7.  IIS-Server empfängt die Datei

#### <a name="denied-web-direct-to-web-server"></a>(Verweigert) Web direkt an Webserver
Da die Webserver, iis01 neu, und die Firewall in der gleichen Cloud-Dienst werden Teilen sie die öffentliche zugänglichen IP-Adresse aus. Daher würde alle HTTP-Datenverkehr an die Firewall geleitet. Während die Anfrage erfolgreich dienen sollte, es kann nicht direkt an den Webserver wechseln, jedoch übergeben, wie, durch die Firewall zuerst vorgesehen. Finden Sie im erste Szenario in diesem Abschnitt für den Datenfluss aus.

#### <a name="denied-web-to-backend-server"></a>(Verweigert) Web auf Back-End-Server
1.  Internet-Benutzer versucht, eine Datei auf AppVM01 über die BackEnd001.CloudApp.Net Dienst zugreifen
2.  Da keine Endpunkte für Dateifreigabe geöffnet sind, wird dies würde Cloud-Dienst nicht bestanden und würde nicht erreicht haben, ab dem server
3.  Wenn die Endpunkte aus irgendeinem Grund geöffnet und Sie sind möchten NSG Regel 5 (Internet zur VNet) diesen Datenverkehr blockiert.

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(Verweigert) Nachschlagen von Web DNS-Einträge auf DNS-server
1.  Internet-Benutzer versucht, einen internen DNS-Eintrag auf DNS01 über den Dienst BackEnd001.CloudApp.Net nachschlagen
2.  Da keine Endpunkte für DNS geöffnet sind, wird dies würde Cloud-Dienst nicht bestanden und würde nicht erreicht haben, ab dem server
3.  Wenn die Endpunkte aus irgendeinem Grund geöffnet und Sie sind würde NSG Regel 5 (Internet zur VNet) diesen Datenverkehr blockieren (Hinweis: Diese Regel 1 (DNS) zwei Gründen nicht anwenden möchten, zuerst die Quellbilds im Internet ist, diese Regel gilt nur für die lokale VNet als Quelle, auch hier eine Zulassungsregel, damit es nie Datenverkehr verweigern möchten)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(Verweigert) Web SQL-Zugriff über die Firewall
1.  Internet-Benutzers anfordert SQL-Daten aus FrontEnd001.CloudApp.Net (Internet gegenüberliegende Cloud Service)
2.  Da keine Endpunkte für SQL geöffnet sind, wird dies würde Cloud-Dienst nicht bestanden und würde nicht erreicht haben, ab der firewall
3.  Wenn die Endpunkte aus irgendeinem Grund geöffnet sind und Sie beginnt das Front-End-Subnetz eingehende Regel Verarbeitung:
  1.    NSG Regel 1 (DNS) nicht anwenden, wechseln zur nächsten Regel
  2.    NSG Regel 2 (RDP) nicht anwenden, wechseln zur nächsten Regel
  3.    NSG Regel 2 (Internet zur Firewall) gilt, den Datenverkehr ist eine zulässige, Anhalten Regel Verarbeitung
4.  Datenverkehr Treffer interne IP-Adresse der Firewall (10.0.1.4)
5.  Firewall weist keine Weiterleitungsregeln für SQL und legt den Datenverkehr ab

## <a name="conclusion"></a>Abschluss
Dies ist eine relativ direkt weiterleiten Verfahren zum Schutz Ihrer Anwendungs durch eine Firewall und das Back-End-Subnetz aus eingehenden Datenverkehr isolieren.

Wenn Sie weitere Beispiele und einen Überblick über die Sicherheitsgrenzen Netzwerken finden Sie [hier][HOME].

## <a name="references"></a>Verweise
### <a name="main-script-and-network-config"></a>Hauptfenster Skripts und Netzwerk Config
Speichern Sie das vollständige Skript in einer PowerShell-Skriptdatei aus. Speichern Sie das Netzwerk Config in einer Datei namens "NetworkConf2.xml" ein.
Ändern Sie die benutzerdefinierte Variablen nach Bedarf. Führen Sie das Skript, und führen Sie die Firewall-Regel-Setup-Anweisung oben.

#### <a name="full-script"></a>Vollständiges Skript
Dieses Skript wird, basierend auf die benutzerdefinierte Variablen an:

1.  Verbinden Sie mit einem Azure-Abonnement
2.  Erstellen eines neuen Kontos mit Speicher
3.  Erstellen einer neuen VNet und zwei Subnetzen in der Netzwerk-Konfigurationsdatei definierten
4.  Erstellen von 4 Windows Server virtuellen Computern
5.  Konfigurieren von NSG einschließlich:
  - Erstellen einer NSG
  - Füllen es mit Regeln
  - Binden der NSG zu den entsprechenden Subnetzen

Diese PowerShell-Skript sollte lokal auf ausgeführt werden, dass eine Internet PC oder Server verbunden.

>[AZURE.IMPORTANT] Wenn dieses Skript ausgeführt wird, es möglicherweise Warnungen oder anderen informativen Nachrichten, die in PowerShell pop. Nur Fehlermeldungen in Rot sind Anlass zur Sorge.


    <# 
     .SYNOPSIS
      Example of DMZ and Network Security Groups in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet (plus the NVA on the FrontEnd subnet)
       - Three Servers on the BackEnd Subnet
       - Network Security Groups to allow/deny traffic patterns as declared
      
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that can be used, but should not be used for all scenarios. You
      are responsible to assess your security needs and the appropriate protections
      needed, and then effectively implement those protections.
    
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       myFirewall - 10.0.1.4
       IIS01      - 10.0.1.5
     
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
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf2.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 1
        #       - The AppVM1 Server must be VM 2
        #       - The DNS server must be VM 4
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"
    
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
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
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
                # Note: Web traffic goes through the firewall, so we'll need to set up a HTTP endpoint.
                #       Also, the firewall will be redirecting web traffic to a new IP and Port in a
                #       forwarding rule, so the HTTP endpoint here will have the same public and local
                #       port and the firewall will do the NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                    # Note: A Remote Desktop endpoint is automatically created when each VM is created.
                }
            $i++
        }
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
        
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) to $($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
            -Protocol *
        
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *
    
        # Assign the NSG to the Subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
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
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Eingehende DMZ mit NSG"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Ziel NAT-Symbol"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Firewall-Regel"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Aktivierung der Firewall-Regel"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
