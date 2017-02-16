<properties
   pageTitle="Implementieren einer Hybrid Netzwerkarchitektur mit Azure und lokalen VPN | Microsoft Azure"
   description="Wie Sie eine sichere Website-zu-Standort Netzwerkarchitektur implementieren, die ein Azure-virtuellen Netzwerk und einem lokalen Netzwerk verbunden über ein VPN umfasst."
   services=""
   documentationCenter="na"
   authors="RohitSharma-pnp"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="roshar"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-and-on-premises-vpn"></a>Implementieren einer Hybrid Netzwerkarchitektur mit Azure und lokalen VPN

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel enthält eine Reihe von Methoden zum Erweitern einer lokalen Netzwerk auf Azure über eine Website-zu-Standort virtuelles privates Netzwerk (VPN). Der Datenfluss zwischen dem lokalen Netzwerk und ein Azure virtuelles Netzwerk (VNet) durch einen Tunnel IPSec VPN. Diese Architektur eignet sich für Hybrid Applications die folgende Merkmale aufweist:

- Teile der Anwendung ausführen lokal, während andere Personen in Azure ausführen.

- Den Datenverkehr zwischen lokalen Hardware und die Cloud ist zu rechnen transparent sein, oder es ist von Vorteil, etwas erweiterten Wartezeit für die Flexibilität und Leistung der Cloud Handel.

- Im erweiterte Netzwerk bildet einen geschlossenen System. Es ist kein direkter Pfad aus dem Internet an den Azure VNet.

- Benutzer eine Verbindung herstellen mit dem lokalen Netzwerk zur Nutzung der Dienste in Azure gehostet wird. Die Verbindung zwischen dem lokalen Netzwerk und in Azure ausgeführten Dienste ist für Benutzer transparent.

Beispiele für Szenarien, die dieses Profil anpassen:

- Line-of-Business Applications innerhalb einer Organisation verwendet, in dem Teil der Funktionalität wurde in der Cloud migriert wurde.

- Entwicklung/Testlabor Auslastung.

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle: [Azure Ressourcenmanager] [ resource-manager-overview] und klassischen. Dieser Plan verwendet Ressourcenmanager, in dem Bereitstellung neuer Microsoft empfiehlt.

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm hervorgehoben, die Komponenten in dieser Architektur:

> Ein Visio-Dokument, das diese Architekturdiagramm enthält wird unter der [Microsoft download Center]herunterladen[visio-download]. Dieses Diagramm befindet sich in der "Hybrid Netzwerk - VPN" Seite.

![[0]][0]

- **Lokale Netzwerk.** Dies ist ein Netzwerk von Computern und Geräten, die über ein privates LAN-Netzwerk ausführen, die innerhalb einer Organisation verbunden.

- **VPN-Anwendung.** Dies ist ein Gerät oder den Dienst, der externe Konnektivität mit dem lokalen Netzwerk bereitstellt. Die VPN-Anwendung möglicherweise auf einem Gerät oder eine Software-Lösung, wie etwa das Routing und Service (RAS) in Windows Server 2012 sind möglich.

> [AZURE.NOTE] Eine Liste der unterstützten VPN-Geräte und Informationen zum ausgewählten VPN-Geräte für das Herstellen einer Verbindung mit einer Azure VPN-Gateway konfigurieren, finden Sie unter den Anweisungen für das entsprechende Gerät in der [Liste der von Azure unterstützten VPN-Geräte][vpn-appliance].

- **Mehrstufige Cloudanwendung.** Dies ist die Anwendung in Azure gehostet wird. Es möglicherweise mehrere Ebenen mit mehreren unterschiedlichen über Azure Lastenausgleich verbunden sind. Der Datenverkehr in jedem Subnetz kann Regeln mit [Azure Netzwerk Sicherheitsgruppen (NSGs)]definiert sein,[azure-network-security-group]. Weitere Informationen finden Sie unter [Erste Schritte mit Microsoft Azure-Sicherheit][getting-started-with-azure-security].

> [AZURE.NOTE] In diesem Artikel werden die Cloudanwendung als einzelne Einheit. Finden Sie unter [Ausführen einer mehrstufige Architektur auf Azure] [ implementing-a-multi-tier-architecture-on-Azure] ausführliche Informationen.

- **[Virtuelle Netzwerk (VNet)][azure-virtual-network].** Die Cloudanwendung und die Komponenten für das Azure VPN Gateway befinden sich in der gleichen VNet.

- **[Azure VPN-Gateway][azure-vpn-gateway].** Der VPN-Gateway-Dienst können Sie die VNet mit dem lokalen Netzwerk über ein VPN-Anwendung zu verbinden. Weitere Informationen finden Sie unter [Verbinden mit einem lokalen Netzwerk mit einem Microsoft Azure-virtuellen Netzwerk][connect-to-an-Azure-vnet]. Das Gateway VPN umfasst die folgenden Elemente:

  - **Virtuelle Netzwerk-Gateway.** Dies ist eine Ressource, die eine virtuelle VPN-Anwendung für die VNet enthält. Es ist verantwortlich für das routing des Datenverkehrs vom lokalen Netzwerk zu den VNet.

  - **Lokales Netzwerk-Gateway.** Dies ist ein Modell für die lokale VPN-Anwendung. Netzwerkverkehr aus der Cloudanwendung auf dem lokalen Netzwerk wird über dieses Gateway geleitet.

  - **Verbindung.** Die Verbindung verfügt über gemeinsame Eigenschaften, die den Verbindungstyp (IPSec) und die Taste für die lokale VPN-Anwendung freigegeben Datenverkehr Verschlüsselung angeben.

  - **Gateway Subnetz.** Virtuelle Netzwerk-Gateway ist in einem eigenen Subnetz, frei, die für die verschiedenen Anforderungen, unterliegt, wie im Abschnitt Empfehlungen beschrieben.

- **Interner Lastenausgleich.** Netzwerkverkehr von VPN-Gateway wird mit der Cloudanwendung über eine interne Lastenausgleich weitergeleitet. Lastenausgleich befindet sich in der Front-End-Subnetz der Anwendung.

## <a name="recommendations"></a>Empfehlungen

Azure bietet viele Ressourcentypen, sodass diese Architektur Bezug genommen kann und anderen Ressourcen viele verschiedene Arten bereitgestellt. Wir haben eine Ressourcenmanager Azure-Vorlage, um die Verweis-Architektur zu installieren, die diese Empfehlungen folgt bereitgestellt. Wenn Sie zum Erstellen Ihrer eigenen Bezug Architektur auswählen, sollten Sie diese Empfehlungen folgen, es sei denn, Sie haben eine bestimmte Anforderung, die nicht empfohlen passen.

### <a name="vnet-and-gateway-subnet"></a>VNet und dem Gateway Subnetz

Erstellen einer Azure VNet für die Komponenten in der Cloud gedrückt. Der Adresse der Azure VNet Speicherplatz muss groß genug für die Adressen, die vom virtuellen Computern und Subnetze in der VNet verwendet wird. Sicherstellen Sie, dass der VNet Adresse Speicherplatz ausreichend Platz für Wachstum hat, wenn zusätzliche virtuellen Computern wahrscheinlich zukünftig erforderlich sind. Der Adresse Speicherplatz der VNet muss nicht mit dem lokalen Netzwerk überschneiden. Beispielsweise wird das Diagramm oben die Adresse Leerzeichen 10.20.0.0/16 für die VNet verwendet.

Erstellen eines Namens _GatewaySubnet_, mit dem eine Adresse Zellbereich /27 Subnetz. Das Gateway virtuelles Netzwerk dieses Subnetz erforderlich ist, und das Zuweisen von 32 Adressen zu diesem Subnetz hilft zu verhindern, dass Sie in Zukunft gegenüber den möglichen Gateway Größe Einschränkungen ausgeführt. Platzieren von diesem Subnetz in der Mitte der Adresse Speicherplatz zu vermeiden. Empfiehlt sich das ist, um den Abstand Adresse für das Gateway Subnetz am oberen Ende des Adressbereichs VNet festzulegen. Im Diagramm gezeigten Beispiel wird die 10.20.255.224/27 verwendet.  Eine schnelle Vorgehensweise zum Berechnen der CIDR sieht wie folgt aus:

1. Festlegen der Variablen Bits in der Adresse Speicherplatz der VNet 1, auf die Bits von dem Gateway Subnetz verwendet wird, und legen Sie die restlichen Bits auf 0 an.

2. Konvertieren in eine Dezimalzahl resultierenden Bits, und es als einer Adresse Speicherplatz mit dem Präfix Länge Satz, um die Größe der Gateways Subnetz express.

Beispielsweise wird die Schritt #1 erneut anwenden 10.20.0b11111111.0b11100000 für eine VNet mit eine IP-Adresse ein Zellbereich 10.20.0.0/16.  Konvertieren, die in eine Dezimalzahl und es als einer Adresse Leerzeichen 10.20.255.224/27 ergibt Ausdrücken

> [AZURE.WARNING] Stellen Sie mit dem Gateway Subnetz nicht anderen virtuellen Computern oder Rolleninstanzen. Darüber hinaus weisen Sie eine NSG zu diesem Subnetz, wie es das Gateway zum Absturz verursacht.

### <a name="virtual-network-gateway"></a>Virtuelle Netzwerk-gateway

Zuweisen einer öffentlichen IP-Adresse für das Gateway virtuelles Netzwerk.

Erstellen Sie des Gateways virtuelles Netzwerk in das Gateway Subnetz, und weisen sie die neu zugewiesene öffentliche IP-Adresse. Verwenden Sie den Gateway-Typ, die Ihren Anforderungen am besten passt und die aktiviert ist, indem Sie Ihre VPN-Anwendung:

Erstellen eines [Gateways Policy-basierte] [ policy-based-routing] Wenn Sie genau steuern können, wie Anfragen weitergeleitet werden müssen. anhand von Kriterien Richtlinie, z. B. Adresspräfixe. Policy-basierte Gateways statisches routing verwenden, und funktionieren nur mit Standorten Verbindungen.

Erstellen eines [Gateways Routing-basierten] [ route-based-routing] wenn Verbindung mit dem lokalen Netzwerk RRAS mit mehreren Standorte oder Cross-Region Verbindungen unterstützen oder implementieren VNet-VNet-Verbindungen (einschließlich weitergeleitet, die mehrere VNets durchlaufen). Routing-basierten Gateways verwenden Sie dynamisches routing für direkte Datenverkehr zwischen Netzwerken. Sie können Fehlern im Netzwerkpfad besser als statische weitergeleitet, tolerieren, da diese alternativen leitet ausprobieren können. Routing-basierten Gateways können auch den Verwaltungsaufwand reduzieren, da leitet möglicherweise nicht manuell aktualisiert werden, wenn Netzwerkadressen ändern müssen.

Eine Liste der unterstützten VPN-Geräte, finden Sie unter [Informationen zu VPN-Geräten für Standort-zu-Standort VPN-Gateway Verbindungen][vpn-appliances].

> [AZURE.NOTE] Nachdem das Gateway erstellt wurde, können Sie zwischen verschiedenen Arten von Gateway ohne zu löschen und neu erstellen des Gateways nicht ändern.

Wählen Sie die Azure VPN Gateway-SKU, die Ihren Anforderungen Durchsatz am ehesten entspricht. Azure VPN-Gateway steht in drei SKUs in der folgenden Tabelle angezeigt. Jede SKU bietet verschiedene Durchsatz, [Gebühren sind erreichen] [ azure-gateway-charges] basierend auf den Zeitraum, die das Gateway bereitgestellt wird und zur Verfügung.

| SKU | VPN Durchsatz | Max IPSec-Tunnel|
|-----|----------------|------------------|
| Grundlegende | 100 MB/s | 10 |
| Standard | 100 MB/s | 10 |
| Hohe Leistung | 200 MB | 30 |

> [AZURE.NOTE] Die grundlegende SKU ist nicht mit Azure ExpressRoute kompatibel. Sie können [die SKU ändern] [ changing-SKUs] nach das Gateway erstellt wurde.

Erstellen von Weiterleitungsregeln für das Gateway Subnetz die direkte Anwendungsdatenverkehr aus des Gateways auf dem internen Lastenausgleich, statt Anfragen direkt an den virtuellen Computern übergeben, die die Anwendung implementieren.

### <a name="on-premises-network-connection"></a>Lokale Netzwerk-Verbindung

Erstellen eines Gateways für lokales Netzwerk. Geben Sie die öffentliche IP-Adresse der lokalen VPN-Anwendung, und der Adressraum des lokalen Netzwerks ein. Beachten Sie, dass die lokale VPN-Anwendung muss eine öffentliche IP-Adresse verfügen, die vom lokalen Netzwerkgateway in Azure VPN-Gateway zugegriffen werden kann. Das Gerät VPN kann hinter einem NAT-Gerät nicht gefunden werden.

Erstellen Sie eine Verbindung zwischen Standorten für das Gateway virtuelles Netzwerk und das Gateway lokales Netzwerk. Wählen Sie den Standort-zu-Standort (IPSec) Verbindungstyp aus, und geben Sie den freigegebenen Schlüssel. Standort-zu-Standort-Verschlüsselung mit dem Azure VPN Gateway basiert auf das IPSec-Protokoll, vorinstallierter Schlüssel für die Authentifizierung verwenden. Beim Erstellen der Azure VPN-Gateway, geben Sie die Taste. Sie müssen die lokale mit demselben Schlüssel ausgeführt VPN-Anwendung konfigurieren. Andere Authentifizierungsverfahren werden derzeit nicht unterstützt.

Sicherstellen Sie, dass die lokale, routing-Infrastruktur für die Adressen in der Azure VNet am Gerät VPN vorgesehen Anfragen weiterleiten konfiguriert ist.

Öffnen Sie alle Ports, die von der Cloudanwendung "im lokalen Netzwerk erforderlich.

Testen Sie die Verbindung aus, um sicherzustellen, dass ein:

- Die lokale VPN-Anwendung leitet den Datenverkehr ordnungsgemäß mit der Cloudanwendung über das Azure VPN Gateway weiter.

- Der VNet leitet den Datenverkehr ordnungsgemäß wieder auf dem lokalen Netzwerk.

- Unzulässigen Datenverkehr in beiden Richtungen ist richtig blockiert.

## <a name="scalability-considerations"></a>Aspekte Skalierbarkeit

Eingeschränkte vertikale Skalierbarkeit erzielen Sie durch Verschieben von Grundlagen "oder" Standard VPN Gateway SKUs hohe Leistung VPN SKU.

VNets, die eine große Anzahl von VPN-Datenverkehr erwarten, sollten Sie die verschiedenen Auslastung in separaten kleinere VNets verteilen und Konfigurieren eines Gateways VPN für jede von ihnen.

Sie können die VNet entweder horizontal oder vertikal aufteilen. Verschieben Sie, mit dem horizontal aufgeteilt, einige Instanzen virtueller Computer aus jeder Kategorie in der der neue VNet Subnetze. Das Ergebnis ist, dass jede VNet dieselbe Struktur und Funktionalität verfügt. Entwerfen Sie, mit dem vertikal aufgeteilt, einzelnen Ebenen, um die Funktionalität in unterschiedliche logische Bereiche (z. B. Behandlung Bestellungen, fakturieren, Kunden Konto Management usw.) aufzuteilen. Einzelnen Funktionsbereichen kann dann in einem eigenen VNet platziert werden.

Repliziert einen lokalen Active Directory-Domänencontroller in der VNet und Implementierung von DNS-Einträge in der VNet können dazu beitragen um einige des Datenverkehrs Sicherheits- und administrative parallelen aus lokalen in der Cloud zu verringern.

## <a name="availability-considerations"></a>Verfügbarkeit Aspekte

Wenn Sie müssen Sie sicherstellen, dass das lokale Netzwerk Azure VPN-Gateway verfügbar bleibt, Sie können implementieren Sie einen Failovercluster für das lokale VPN-Gateway.

Wenn Ihre Organisation mehrere lokale Websites verfügt, erstellen Sie [mit mehreren Standort Verbindungen] [ vpn-gateway-multi-site] auf eine oder mehrere Azure VNets. Dieser Ansatz erfordert dynamisches routing (Routing-basiert), müssen Sie sicherstellen, dass die lokale VPN-Gateway dieses Feature unterstützt.

[Vereinbarung zum SERVICELEVEL für VPN-Gateway] finden Sie unter[ sla-for-vpn-gateway] Details die Option VPN Gateway Vereinbarung zum SERVICELEVEL.

## <a name="manageability-considerations"></a>Verwaltbarkeit Aspekte

Überwachen von Diagnoseinformationen aus lokalen VPN-Geräte. Dieser Vorgang hängt von den Features von VPN-Anwendung. Beispielsweise, wenn Sie Windows Server 2012 der Routing und RAS-Dienst verwenden, Sie sollten [RRAS Protokollierung]aktivieren[rras-logging].

Verwenden Sie die [Diagnose Azure VPN-Gateway] [ gateway-diagnostic-logs] zum Erfassen von Informationen zu Netzwerkkonnektivitätsprobleme vor. Diese Protokolle können zum Nachverfolgen von Informationen, wie z. B. Quell- und Ziele der Verbindung Besprechungsanfragen, welches Protokoll verwendet wurde, und wie die Verbindung hergestellt wurde (oder warum Fehler beim) verwendet werden.

Überwachen Sie die Protokolle der Betrieb des Gateways VPN Azure mithilfe der Überwachungsprotokolle Azure-Portal zur Verfügung. Separate Protokolle stehen für lokales Netzwerkgateways, das Netzwerkgateway Azure und die Verbindung zur Verfügung. Diese Informationen kann verwendet werden, um mit dem Gateway vorgenommenen Änderungen nachverfolgen und ist nützlich, wenn ein zuvor funktionsfähige Gateway nicht funktioniert aus irgendeinem Grund mehr.

![[2]][2]

Überwachen Sie der Konnektivität sowie verfolgen Sie Connectivity fehlgeschlagene Ereignisse zu. Sie können ein Paket Überwachung, wie z. B. [Nagios] [ nagios] zu erfassen und diese Informationen zu melden.

## <a name="security-considerations"></a>Zur Sicherheit

Erstellt einen anderen freigegebenen Registrierungsschlüssel für jede VPN-Gateway. Verwenden eines freigegebenen starken Schlüssels Bruteforce-Angriffen standhält.

> [AZURE.NOTE] Sie können nicht aktuell, Azure-Taste Tresor mit vorinstallierten Schlüsseln Azure VPN-Gateway verwenden.

Stellen Sie sicher, dass die lokale VPN-Anwendung eine Verschlüsselungsmethode verwendet, die [mit dem Azure VPN Gateway]ist[vpn-appliance-ipsec]. Für das routing von Policy-basierte unterstützt Azure VPN-Gateway die AES256, AES128 und 3DES Algorithmen für die Verschlüsselung. Routing-basierten Gateways unterstützen AES256 und 3DES.

Wenn Ihre lokalen VPN-Anwendung in einem Netzwerk Umfang eine Firewall zwischen dem Shape-Netzwerk und im Internet ist, möglicherweise müssen Sie [zusätzliche Firewall-Regeln] konfigurieren[ additional-firewall-rules] um die Website-zu-Standort VPN-Verbindung zu ermöglichen.

Wenn die Anwendung in den VNet Daten mit dem Internet sendet, sollten Sie sich [Implementieren eines erzwungenen Tunnel] [ forced-tunneling] zum gesamten Internet gerichtete Verkehr über das lokale Netzwerk weiterleiten. Dieser Ansatz ermöglicht es Ihnen zu überwachender ausgehende Besprechungsanfragen, die von der Anwendung aus der lokalen Infrastruktur vorgenommen.

> [AZURE.NOTE] Erzwungene Tunnel Verbindung zu Azure-Dienste (beispielsweise der Speicherdienst) auswirken kann und die Windows-Lizenz-Manager.

## <a name="troubleshooting-considerations"></a>Aspekte zur Problembehandlung

> [AZURE.NOTE] Besuchen Sie die Routing und Remote Access-Blog allgemeine Informationen zur [Problembehandlung bei häufigen VPN zusammenhängenden][troubleshooting-vpn-errors].

Wenn Sie den Datenverkehr kann nicht die VPN-Verbindung zu durchlaufen, überprüfen Sie Folgendes:

### <a name="on-premises-vpn-appliance"></a>Lokale VPN-Anwendung:

**Ist die lokale VPN-Anwendung ordnungsgemäß funktionsfähig?**

Überprüfen eines Protokolldateien, die durch die VPN-Anwendung für Fehler sowie Fehler generiert. Die Position der diese Informationen variieren gemäß Ihrer Einheit. Beispielsweise, wenn Sie Windows Server 2012 RRAS verwenden, können den folgenden PowerShell-Befehl Sie Ereignisinformationen für den Dienst RRAS Fehler angezeigt:

```
Get-EventLog -LogName System -EntryType Error -Source RemoteAccess | Format-List -Property *
```

Die _Nachricht_ -Eigenschaft für jeden Eintrag enthält eine Beschreibung des Fehlers. Beispiele für allgemeine sind:

- Unfähigkeit zum Verbinden, möglicherweise aufgrund einer falschen IP-Adresse für das Gateway Azure VPN, in der Benutzeroberfläche RRAS VPN Netzwerkkonfiguration angegeben:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {41, 3, 0, 0}
Index              : 14231
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: The network connection between your computer and
                     the VPN server could not be established because the remote server is not responding. This could
                     be because one of the network devices (e.g, firewalls, NAT, routers, etc) between your computer
                     and the remote server is not configured to allow VPN connections. Please contact your
                     Administrator or your service provider to determine which device may be causing the problem.
Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, The network connection between
                     your computer and the VPN server could not be established because the remote server is not
                     responding. This could be because one of the network devices (e.g, firewalls, NAT, routers, etc)
                     between your computer and the remote server is not configured to allow VPN connections. Please
                     contact your Administrator or your service provider to determine which device may be causing the
                     problem.}
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:26:02 PM
TimeWritten        : 3/18/2016 1:26:02 PM
UserName           :
Site               :
Container          :
```

- Die falschen freigegebenen Schlüssel in der Benutzeroberfläche RRAS VPN Netzwerkkonfiguration angegeben wird:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {233, 53, 0, 0}
Index              : 14245
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: IKE authentication credentials are unacceptable

Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, IKE authentication credentials are
                     unacceptable
                     }
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:34:22 PM
TimeWritten        : 3/18/2016 1:34:22 PM
UserName           :
Site               :
Container          :
```

Ereignisprotokollinformationen Versuche Verbindung über den RRAS-Dienst mithilfe des folgenden PowerShell-Befehls zur Verfügung:

```
Get-EventLog -LogName Application -Source RasClient | Format-List -Property *
```

Bei einem Ausfall zu verbinden wird dieses Protokoll Fehler enthalten, die ähnlich wie der folgende aus:

```
EventID            : 20227
MachineName        : on-prem-vm
Data               : {}
Index              : 4203
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : CoId={B4000371-A67F-452F-AA4C-3125AA9CFC78}: The user SYSTEM dialed a connection named
                     AzureGateway which has failed. The error code returned on failure is 809.
Source             : RasClient
ReplacementStrings : {{B4000371-A67F-452F-AA4C-3125AA9CFC78}, SYSTEM, AzureGateway, 809}
InstanceId         : 20227
TimeGenerated      : 3/18/2016 1:29:21 PM
TimeWritten        : 3/18/2016 1:29:21 PM
UserName           :
Site               :
Container          :
```

**Ist die Option VPN Einheit ordnungsgemäß routing des Datenverkehrs über das Azure VPN Gateway?**

Verwenden ein Tools, wie z. B. [PsPing] [ psping] zur Überprüfung der Konnektivität und routing über VPN-Gateway. Führen Sie beispielsweise zum Testen der Konnektivität aus einer lokalen Computer auf einen Webserver befindet sich auf der VNet den folgenden Befehl (ersetzen `<<web-server-address>>` mit der Adresse des Web-Servers):

```
PsPing -t <<web-server-address>>:80
```

Wenn auf der lokalen Computer Datenverkehr an den Webserver weiterleiten kann, sollte ähnlich wie der folgende Ausgabe angezeigt werden:

```
D:\PSTools>psping -t 10.20.0.5:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.0.5:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.0.5:80 (warmup): 6.21ms
Connecting to 10.20.0.5:80: 3.79ms
Connecting to 10.20.0.5:80: 3.44ms
Connecting to 10.20.0.5:80: 4.81ms

  Sent = 3, Received = 3, Lost = 0 (0% loss),
  Minimum = 3.44ms, Maximum = 4.81ms, Average = 4.01ms
```

Wenn auf der lokalen Computer mit dem angegebenen Ziel kommunizieren kann, wird wie folgt angezeigt:

```
D:\PSTools>psping -t 10.20.1.6:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.1.6:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.1.6:80 (warmup): This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80:
  Sent = 3, Received = 0, Lost = 3 (100% loss),
  Minimum = 0.00ms, Maximum = 0.00ms, Average = 0.00ms
```

**Ermöglicht die lokale Firewall VPN Datenverkehr passieren? Haben die richtigen Ports geöffnet wurde?**

Stellen Sie sicher, dass die lokale VPN-Anwendung eine Verschlüsselungsmethode verwendet, die [mit dem Azure VPN Gateway]ist[vpn-appliance].

Für das routing von Policy-basierte unterstützt Azure VPN-Gateway die AES256, AES128 und 3DES Algorithmen für die Verschlüsselung. Routing-basierten Gateways unterstützen AES256 und 3DES.

### <a name="azure-vnet-gateway"></a>Azure VNet Gateway:

Untersuchen Sie [Diagnoseprotokolle Azure VPN-Gateway] [ gateway-diagnostic-logs] auf potenzielle Probleme.

**Sind Azure VPN-Gateway und lokale VPN-Anwendung mit demselben freigegebenen Authentifizierungsschlüssel konfiguriert?**

Sie können vom Azure VPN-Gateway mit den folgenden Befehl aus Azure CLI gespeicherten freigegebenen Schlüssel anzeigen:

```
azure network vpn-connection shared-key show <<resource-group>> <<vpn-connection-name>>
```

Mithilfe des Befehls für Ihre lokalen VPN-Anwendung geeignet anzeigen den freigegebenen Schlüssel für die Anwendung konfiguriert.

Überprüfen Sie, ob das _GatewaySubnet_ Subnetz gedrückt, dass das Azure VPN Gateway nicht mit einer NSG verknüpft ist.

Sie können die Subnetdetails mit den folgenden Befehl aus Azure CLI anzeigen:

```
azure network vnet subnet show -g <<resource-group>> -e <<vnet-name>> -n GatewaySubnet
```

Vergewissern Sie sich es keine Datenfeld _Sicherheitsgruppe Netzwerk-Id_benannt wird. Das folgende Beispiel zeigt die Ergebnisse für eine Instanz von der _GatewaySubnet_ , die eine zugeordnete NSG (_VPN-Gateway-Group_) enthält. Dies kann dazu führen, dass das Gateway ordnungsgemäß funktioniert, wenn es Regeln für die diese NSG sind verhindern:

```
C:\>azure network vnet subnet show -g profx-prod-rg -e profx-vnet -n GatewaySubnet
    info:    Executing command network vnet subnet show
    + Looking up virtual network "profx-vnet"
    + Looking up the subnet "GatewaySubnet"
    data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/virtualNetworks/profx-vnet/subnets/GatewaySubnet
    data:    Name                            : GatewaySubnet
    data:    Provisioning state              : Succeeded
    data:    Address prefix                  : 10.20.3.0/27
    data:    Network Security Group id       : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/networkSecurityGroups/VPN-Gateway-Group
    info:    network vnet subnet show command OK
```

**Gibt den virtuellen Computern in der Azure VNet so konfiguriert, dass Datenverkehr aus außerhalb der VNet? Überprüfen Sie alle NSG Regeln mit unterschiedlichen mit diesen virtuellen Computern verknüpft ist.**

Sie können alle NSG Regeln mit den folgenden Befehl aus Azure CLI anzeigen:

```
azure network nsg show -g <<resource-group>> -n <<nsg-name>>
```

**Wurde das Azure VPN Gateway getrennt?**

Den folgenden Azure PowerShell-Befehl können den aktuellen Status der Azure VPN-Verbindung zu überprüfen. Die `<<connection-name>>` Parameter ist der Name der Azure VPN-Verbindung, die das Gateway virtuelles Netzwerk und das lokale Gateway verknüpft.

```
Get-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> - ResourceGroupName <<resource-group>>
```

Die folgenden Codeausschnitte markieren Sie die Ausgabe generiert wird, wenn das Gateway ist (das erste Beispiel) verbunden und getrennt (das zweite Beispiel):

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : Connected
EgressBytesTransferred     : 55254803
IngressBytesTransferred    : 32227221
ProvisioningState          : Succeeded
...
```

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection2 -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : NotConnected
EgressBytesTransferred     : 0
IngressBytesTransferred    : 0
ProvisioningState          : Succeeded
...
```

### <a name="host-vm-configuration-network-bandwidth-utilization-and-application-performance"></a>Virtueller Computer Hostkonfiguration, Netzwerk Bandbreite Auslastung und Leistung der Anwendung:

Stellen Sie sicher, dass die Firewall im Gast-Betriebssystem ausgeführt wird, klicken Sie auf den Azure-virtuellen Computern, in dem Subnetz zulässigen Verkehr über die lokale IP-Adressbereiche zulässig ordnungsgemäß konfiguriert sind.

**Ist der Umfang des Datenverkehrs nahe am Grenzwert für die Bandbreite Azure VPN-Gateway verfügbar?**

Tools hängt von den Funktionen, die für Ihre VPN-Anwendung, die lokal ausgeführt. Wenn Sie Windows Server 2012 RRAS verwenden, können Sie beispielsweise Performance Monitor verwenden, zum Nachverfolgen von Datenmengen erhalten haben und über die VPN-Verbindung übertragen werden; Wählen Sie das Objekt _Virtuelles Total_ mit Indikatoren _Bytes empfangen/s_ und _Gesendet_ :

![[3]][3]

Vergleichen Sie die Ergebnisse mit der Bandbreite VPN-Gateway (100 MB/s für Basic und Standard-SKUs und 200 MB für das hohe Leistung SKU) verfügbar:

![[4]][4]

**Sind eines virtuellen Computer in der Azure VNet langsam ausgeführt? Sind, die sie überlastet, es ausreichend von, damit die Last bewältigt, alle Lastenausgleich ordnungsgemäß konfiguriert sind, werden?**

[Erfassen und Analysieren von Diagnoseinformationen]setzt[azure-vm-diagnostics]. Sie können die Ergebnisse der Azure-Portal mit untersuchen, aber viele Drittanbieter-Tools stehen auch zur Verfügung, die können ermöglichen detaillierte Einblicke in die Leistungsdaten.

**Ist die Anwendung, die effiziente Cloud Ressourcen verwenden?**

Urkunde Anwendungscode ausführen jedes virtuellen Computers zu bestimmen, ob Applikationen die optimale Nutzung von Ressourcen erstellen möchten. Tools wie [Anwendung Einsichten] können[ application-insights] helfen, die folgenden Aufgaben ausführen.

## <a name="solution-deployment"></a>Lösung-Bereitstellung

Wenn Sie eine vorhandene lokale Infrastruktur mit einer VPN-Anwendung bereits konfiguriert haben, können Sie die Verweis-Architektur bereitstellen, mit folgenden Schritten:

1. Klicken Sie mit der rechten Maustaste auf die Schaltfläche unten, und wählen Sie entweder "Link öffnen in neuer Registerkarte" oder "Link in neuem Fenster öffnen":  
[![Bereitstellen für Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn%2Fazuredeploy.json)

2. Warten Sie für den Link zur Azure-Portal zu öffnen, und klicken Sie dann gehen Sie folgendermaßen vor: 
    - Der Name der **Ressourcengruppe** bereits in der Parameterdatei definiert ist, also wählen Sie **Neu erstellen** und geben Sie `ra-hybrid-vpn-rg` in das Textfeld ein.
    - Wählen Sie die Region im Dropdownfeld **Speicherort** aus.
    - Bearbeiten Sie die **Vorlage Stamm-Uri** oder die **Parameter Stamm-Uri** Textfelder nicht.
    - Überprüfen Sie die allgemeinen Geschäftsbedingungen, und klicken Sie auf das Kontrollkästchen **ich die allgemeinen Geschäftsbedingungen oben erwähnten zustimmen** .
    - Klicken Sie auf die Schaltfläche **Einkauf** .

3. Warten Sie für die Bereitstellung ausführen.

## <a name="next-steps"></a>Nächste Schritte

- [Installieren zusätzliche Domänencontroller für lokalen Active Directory-Domäne mithilfe von virtuellen Computern in einer Azure VNet][installing-ad].

- [Richtlinien für die Bereitstellung von Windows Server Active Directory auf Azure-virtuellen Computern][deploying-ad].

- [Erstellen eines DNS-Servers in einer VNet][creating-dns].

- [Konfigurieren und Verwalten von DNS-Einträge in einer VNet][configuring-dns].

- [Lokale Stormshield Sicherheit Netzwerkgeräte über ein VPN mit][stormshield].

- [Implementieren einer Hybrid Netzwerkarchitektur mit Azure ExpressRoute][expressroute].

<!-- links -->

[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[arm-templates]: ../resource-group-authoring-templates.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-portal]: ../azure-portal/resource-group-portal.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[azure-virtual-network]: ../virtual-network/virtual-networks-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: https://azure.microsoft.com/services/vpn-gateway/
[azure-gateway-charges]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[azure-network-security-group]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[vpn-gateway-multi-site]: ../vpn-gateway/vpn-gateway-multi-site.md
[policy-based-routing]: https://en.wikipedia.org/wiki/Policy-based_routing
[route-based-routing]: https://en.wikipedia.org/wiki/Static_routing
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[sla-for-vpn-gateway]: https://azure.microsoft.com/support/legal/sla/vpn-gateway/v1_0/
[additional-firewall-rules]: https://technet.microsoft.com/library/dn786406.aspx#firewall
[nagios]: https://www.nagios.org/
[azure-vpn-gateway-diagnostics]: http://blogs.technet.com/b/keithmayer/archive/2014/12/18/diagnose-azure-virtual-network-vpn-connectivity-issues-with-powershell.aspx
[ping]: https://technet.microsoft.com/library/ff961503.aspx
[tracert]: https://technet.microsoft.com/library/ff961507.aspx
[psping]: http://technet.microsoft.com/sysinternals/jj729731.aspx
[nmap]: http://nmap.org
[changing-SKUs]: https://azure.microsoft.com/blog/azure-virtual-network-gateway-improvements/
[gateway-diagnostic-logs]: http://blogs.technet.com/b/keithmayer/archive/2015/12/07/step-by-step-capturing-azure-resource-manager-arm-vnet-gateway-diagnostic-logs.aspx
[troubleshooting-vpn-errors]: https://blogs.technet.microsoft.com/rrasblog/2009/08/12/troubleshooting-common-vpn-related-errors/
[rras-logging]: https://www.petri.com/enable-diagnostic-logging-in-windows-server-2012-r2-routing-and-remote-access
[create-on-prem-network]: https://technet.microsoft.com/library/dn786406.aspx#routing
[create-azure-vnet]: ../virtual-network/virtual-networks-create-vnet-classic-cli.md
[azure-vm-diagnostics]: https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/
[application-insights]: ../application-insights/app-insights-overview-usage.md
[forced-tunneling]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-forced-tunneling/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[vpn-appliances]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[installing-ad]: ../active-directory/active-directory-install-replica-active-directory-domain-controller.md
[deploying-ad]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[creating-dns]: https://blogs.msdn.microsoft.com/mcsuksoldev/2014/03/04/creating-a-dns-server-in-azure-iaas/
[configuring-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[stormshield]: https://azure.microsoft.com/marketplace/partners/stormshield/stormshield-network-security-for-cloud/
[vpn-appliance-ipsec]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec-parameters
[expressroute]: ./guidance-hybrid-network-expressroute.md
[naming conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/deploy-reference-architecture.sh
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetwork.parameters.json
[virtualNetworkGateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetworkGateway.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/blueprints/hybrid-network-vpn.png "Struktur einer Hybrid Tag im lokalen Netzwerk und cloud-Infrastruktur"
[1]: ./media/guidance-hybrid-network-vpn/partitioned-vpn.png "Eine VNet zur Verbesserung der Skalierbarkeit Partitionierung"
[2]: ./media/guidance-hybrid-network-vpn/audit-logs.png "Überwachungsprotokolle Azure-Portal"
[3]: ./media/guidance-hybrid-network-vpn/RRAS-perf-counters.png "Leistungsindikatoren für die Überwachung VPN-Netzwerkverkehr"
[4]: ./media/guidance-hybrid-network-vpn/RRAS-perf-graph.png "Beispiel für VPN-Netzwerk Leistung graph"