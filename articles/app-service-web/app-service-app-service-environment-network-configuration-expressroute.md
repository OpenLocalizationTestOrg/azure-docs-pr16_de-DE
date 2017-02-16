<properties 
    pageTitle="Netzwerk-Konfigurationsdetails für das Arbeiten mit Express-Routing" 
    description="Netzwerk-Konfigurationsdetails zum Ausführen von App-Service-Umgebungen in einer virtuellen Netzwerken mit einer ExpressRoute Verbindung verbunden ist." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="stefsch"/>   

# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>Netzwerk-Konfigurationsdetails für App-Service-Umgebungen mit ExpressRoute 

## <a name="overview"></a>(Übersicht) ##
Kunden können eine Verbindung herstellen einer [Azure ExpressRoute] [ ExpressRoute] Verbindung, deren virtuelle Infrastruktur, daher Erweitern ihrer lokalen Netzwerk in Azure.  Eine App-Service-Umgebung kann in einem Subnetz dieses [virtuelles Netzwerk] erstellt werden[ virtualnetwork] Infrastruktur.  Apps für die App-Service-Umgebung ausgeführt können sichere Verbindungen mit Back-End-Ressourcen zugegriffen werden dann nur über die ExpressRoute Verbindung herstellen.  

Eine App-Service-Umgebung erstellt werden können in **entweder** ein Ressourcenmanager Azure-virtuellen Netzwerk, Modellieren **oder** eine klassische Bereitstellung virtuelles Netzwerk.  Mit der letzten Änderung in Juni 2016 kann ASEs jetzt auch in virtuelle Netzwerke bereitgestellt werden, die entweder öffentlichen Adressbereiche oder RFC1918 Adresse Leerzeichen (d. h. verwenden Private Adressen). 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="required-network-connectivity"></a>Netzwerkkonnektivität erforderlich ##
Es gibt Network Connectivity Anforderungen für App-Service-Umgebungen, die ursprünglich nicht in einem virtuellen Netzwerk bei einer Verbindung zu einer ExpressRoute erfüllt sein kann.  App-Service-Umgebungen erfordern alle der folgenden ordnungsgemäße funktionieren:


-  Ausgehende Netzwerkkonnektivität zum Azure-Speicher auf beide Ports 80 und 443 weltweit Endpunkte.  Dies umfasst die Endpunkte befindet sich in der gleichen Region wie die App-Service-Umgebung als auch Speicher Endpunkte in **anderen** Azure Regionen ansässig.  Azure Speicher Endpunkte zu beheben, klicken Sie unter den folgenden DNS-Domänen: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* und *file.core.windows.net*.  
-  Ausgehende Netzwerkkonnektivität zum Dienst auf Port 445 Azure-Dateien.
-  Ausgehende Netzwerkkonnektivität zu Sql DB Endpunkte, die sich in derselben Region als die App-Service-Umgebung befindet.  SQL-DB Endpunkte zu beheben, klicken Sie unter den folgenden Domäne: *database.windows.net*.  Öffnen des Zugriffs auf Ports 1433, 11000-11999 und 14000-14999 setzt.  Weitere Informationen hierzu finden Sie [in diesem Artikel auf Sql-Datenbank V12 Portverwendung](../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
-  Ausgehende Netzwerkkonnektivität an die Azure Management Ebene Endpunkte (sowohl ASM und Cloud Endpunkte).  Ausgehende Verbindungen sowohl *management.core.windows.net* und *management.azure.com*umfasst. 
-  Ausgehende Netzwerkkonnektivität *ocsp.msocsp.com*, *mscrl.microsoft.com* und *crl.microsoft.com*.  Dies ist erforderlich, um SSL-Funktionen zu unterstützen.
-  Die DNS-Konfiguration für das virtuelle Netzwerk muss aufgelöst aller Endpunkte und Domänen in der früheren Punkte angegeben ist.  Wenn diese Endpunkte aufgelöst werden können, tritt ein App-Service-Umgebung Erstellung Versuche und vorhandenen App Dienst Umgebungen als fehlerhaft gekennzeichnet.
-  Ausgehender Zugriff auf den Port 53 ist für die Kommunikation mit DNS-Server erforderlich.
-  Wenn Sie ein benutzerdefinierter DNS-Server auf das Ende eines Gateways VPN vorhanden ist, muss der DNS-Server aus dem Subnetz, enthält die App-Service-Umgebung erreichbar sein. 
-  Der Netzwerkpfad ausgehenden kann nicht über internen corporate Proxys Reisen, noch können sofort zu lokalen geschickt werden.  Auf diese Weise wird die effektive NAT-Adresse des ausgehenden Netzwerkverkehr aus der App-Service-Umgebung.  Ändern der Adresse einer App Service-Umgebung ausgehenden Netzwerkverkehr NAT Konnektivitätsfehlern auf viele der oben aufgeführten Endpunkte bewirkt, dass.  Dadurch wird in der App-Service-Umgebung Erstellung Versuche, ebenso wie zuvor fehlerfrei App-Service-Umgebungen, die als fehlerhaft gekennzeichnet werden.  
-  Eingehende Netzwerkzugriff auf erforderliche Ports, für die App-Service-Umgebungen gewährt werden müssen, wie in diesem [Artikel]beschrieben[requiredports].

Die DNS-Anforderungen können erfüllt sein, indem sichergestellt wird eine gültige DNS-Infrastruktur konfiguriert ist, und für das virtuelle Netzwerk verwaltet werden.  Wenn Sie aus irgendeinem Grund die DNS-Konfiguration geändert wird, nachdem ein App-Service-Umgebung erstellt wurde, können Entwickler eine App Service-Umgebung, um die neue DNS-Konfiguration abholen erzwingen.  Auslösen eines parallelen Umgebung Neustarts mithilfe des Symbols "Neu starten" am oberen Rand der App-Service-Umgebung Management vorher in der [Azure-Portal] ansässig[ NewPortal] bewirkt, dass die Umgebung, um die neue DNS-Konfiguration zu übernehmen.

Die Access-Anforderungen eingehenden Netzwerk können durch Konfigurieren einer [Netzwerksicherheitsgruppe] erfüllt sein[ NetworkSecurityGroups] auf die App Service-Umgebung Subnetz zu den erforderlichen Zugriff zulassen in diesem [Artikel]beschriebenen[requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Aktivieren von ausgehenden Netzwerkkonnektivität für eine App-Service-Umgebung##
Standardmäßig Ankündigung eine neu erstellte ExpressRoute Verbindung eines Standard-Routing, die ausgehende Verbindung mit dem Internet ermöglicht.  Mit dieser Konfiguration werden einer App-Service-Umgebung können mit anderen Azure Endpunkten verbunden.

Werden jedoch eine gemeinsame Kundenkonfiguration, erzwingt ausgehenden Internet-Datenverkehr stattdessen lokalen, eigene Standard-Routing (0.0.0.0/0) definiert wird.  Dieser Datenfluss verletzt ausnahmslos App-Service-Umgebungen, da ausgehende Datenverkehr entweder gesperrten lokalen ist oder NAT auf ein nicht erkanntes Satz von Adressen, die nicht mehr mit verschiedenen Azure Endpunkten arbeiten möchten.

Die Lösung besteht darin, ein (oder mehrere) benutzerdefinierte leitet (UDRs) im Subnetz zu definieren, die die App-Service-Umgebung enthält.  Eine UDR definiert Subnetz-spezifische weitergeleitet, die statt der Standard-Routing berücksichtigt werden.

Falls möglich, empfiehlt es sich die folgende Konfiguration verwendet:

- Die Konfiguration ExpressRoute Ankündigung 0.0.0.0/0 und standardmäßig erzwingen, dass alle ausgehenden Datenverkehr lokalen Tunnel.
- Die UDR angewendet, die mit dem Subnetz, enthält die App-Service-Umgebung definiert 0.0.0.0/0 mit dem nächsten Abschnitt Typ Internet (ein Beispiel dafür ist weiter unten in diesem Artikel).

Der kombinierte Effekt Schritte ist, dass die Subnetzebene UDR die ExpressRoute Erzwungene Tunnel, Vorrang wird somit ausgehenden Zugriff auf das Internet aus der App-Service-Umgebung.

> [AZURE.IMPORTANT] Die Arbeitspläne in einer UDR **muss** definiert werden spezifischen Vorrang vor alle leitet durch die Konfiguration ExpressRoute angekündigt.  Im folgenden Beispiel Adressbereichs wesentlichen 0.0.0.0/0 verwendet und als solche kann potenziell versehentlich überschrieben werden durch Routing Werbung spezifischere Adressbereiche verwenden.
>
>App-Service-Umgebungen werden nicht unterstützt, auf denen ExpressRoute Konfigurationen dieser **leitet aus dem öffentlichen Peeringliste Pfad an den privaten Peeringliste Pfad Cross ankündigen**.  ExpressRoute Konfigurationen öffentlichen peering konfiguriert, denen erhalten Routing Werbung von Microsoft für eine große Gruppe von Microsoft Azure IP-Adressbereiche.  Wenn diese Adressbereiche auf den privaten Peeringliste Pfad Cross angekündigt sind, ist das Ergebnis Ende alle ausgehenden Netzwerkpakete aus der App-Dienst Umgebung Subnetz Erzwingen eines Kunden die lokale Infrastruktur geschickt werden können, die an.  Diese Netzwerk-Verkehr wird mit App-Service-Umgebungen derzeit nicht unterstützt.  Eine Lösung für dieses Problem ist das Cross-Werbung leitet aus dem öffentlichen Peeringliste Pfad an den privaten Peeringliste Pfad zu beenden.

Hintergrundinformationen auf benutzerdefinierte Strecken steht in dieser [Übersicht][UDROverview].  

Details zum Erstellen und Konfigurieren von benutzerdefinierten leitet steht in diesem [Wie zu Leitfaden][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>Beispiel für eine UDR Konfiguration für eine App-Service-Umgebung ##

**Erforderliche Komponenten**

1. Installieren Sie von der [Seite Azure-Downloads] Azure Powershell[ AzureDownloads] (vom Juni 2015 oder höher).  Unter "Befehlszeile Tools" befindet sich ein Link "Installieren" unter "Windows Powershell", die die neuesten Powershell-Cmdlets installiert werden soll.

2. Es wird empfohlen, dass ein eindeutiges Subnetz Exklusivmodus, indem Sie eine App-Service-Umgebung erstellt wird.  Dadurch wird sichergestellt, dass die UDRs angewendet, die mit dem Subnetz nur ausgehenden Datenverkehr für die App-Service-Umgebung geöffnet werden.
3. **Wichtig**: Stellen Sie die App-Service-Umgebung nicht erst **nach** die folgenden Konfigurationsschritte gefolgt sind.  Dadurch wird sichergestellt, dass ausgehende Netzwerkkonnektivität verfügbar ist, bevor Sie versuchen, eine App-Service-Umgebung bereitstellen.

**Schritt 1: Erstellen einer benannten Routingtabelle**

Der folgende Codeausschnitt erstellt eine Routingtabelle, die so genannte "DirectInternetRouteTable" in der Region Westen uns Azure:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**Schritt 2: Erstellen Sie eine oder mehrere leitet in der Tabelle weiterleiten**

Sie benötigen mindestens leitet der Routing-Tabelle hinzufügen, um ausgehenden Zugriff auf das Internet aktivieren.  

Es wird empfohlen für ausgehenden Zugriff auf das Internet konfigurieren zum Definieren einer Routing für 0.0.0.0/0 wie unten dargestellt.
  
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Denken Sie daran, die 0.0.0.0/0 ist eine umfassende Adressbereichs und als solche wird durch spezifischere Adressbereiche angekündigt durch die ExpressRoute überschrieben werden.  Zum erneuten durchlaufen früheren empfohlen sollte eine UDR mit einer Routing 0.0.0.0/0 in Verbindung mit einer ExressRoute Konfiguration verwendet werden, denen nur 0.0.0.0/0 auch anbietet. 

Alternativ können Sie eine umfassende und aktualisierte Liste CIDR Bereiche unter Verwenden von Azure herunterladen.  Die XML-Datei mit allen Azure IP-Adressbereiche steht im [Microsoft Download Center][DownloadCenterAddressRanges].  

Beachten Sie jedoch, dass diese Bereiche im Laufe der Zeit ändern anzuzeigen, das periodisch manuelle Updates für die benutzerdefinierte leitet synchron bleiben daher erfordert.  Auch, da es standardmäßig oberen maximal 100 leitet in einer einzelnen UDR ist, müssen Sie die Azure IP-Adressbereiche in die Beschränkung 100 Routing passt "zusammenfassen" angekündigt planmäßigen Beachten Sie, dass UDR leitet müssen genauer sein als die Arbeitspläne definiert durch Ihre ExpressRoute.  


**Schritt 3: Verbinden der Routingtabelle mit dem Subnetz, enthält die App-Service-Umgebung**

Der letzte Konfigurationsschritt ist zum Zuordnen der Routingtabelle mit dem Subnetz, in dem die App-Service-Umgebung bereitgestellt werden.  Mit dem folgende Befehl ordnet die "DirectInternetRouteTable" auf "ASESubnet", die später eine App-Service-Umgebung enthalten sollen.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**Schritt 4: Abschließende Schritte**

Sobald die Routingtabelle mit dem Subnetz gebunden ist, wird empfohlen, zuerst testen und bestätigen den gewünschten Effekt.  Beispielsweise Bereitstellen eines virtuellen Computers in das Subnetz und zu bestätigen, dass:


- Weiter oben erwähnten ausgehender Datenverkehr an Azure und nicht Azure Endpunkte in diesem Artikel **nicht** ab der Verbindung ExpressRoute entdeckt wird.  Es ist sehr wichtig, um dieses Verhalten zu überprüfen, da ist ausgehender Datenverkehr aus dem Subnetz weiterhin wird erzwungen lokal App-Service-Umgebung Tunnel, die Erstellung immer fehlschlägt. 
- Weiter oben erwähnten DNS-Suchvorgänge für die Endpunkte alle korrekt aufgelöst werden. 

Nachdem Sie die oben beschriebenen Schritte bestätigt werden, müssen Sie die virtuellen Computern gelöscht werden, da das Subnetz muss "leer" zum Zeitpunkt verwendet, wenn die App-Service-Umgebung erstellt wird.
 
Fahren Sie mit dem Erstellen einer App-Service-Umgebung!

## <a name="getting-started"></a>Erste Schritte
Alle Artikel und wie-des für App-Service-Umgebungen in der [Infodatei für Service-Umgebungen Anwendung](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Um mit der App-Service-Umgebungen anzufangen, finden Sie unter [Einführung in die App-Service-Umgebung][IntroToAppServiceEnvironment]

Weitere Informationen über die App-Verwaltungsdienst Azure-Plattform finden Sie unter [Azure-App-Verwaltungsdienst][AzureAppService].

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureDownloads]: http://azure.microsoft.com/en-us/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[NewPortal]:  https://portal.azure.com
 

<!-- IMAGES -->
