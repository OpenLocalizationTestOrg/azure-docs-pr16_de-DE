<properties 
    pageTitle="So steuern eingehenden Verkehr zu einer App-Service-Umgebung" 
    description="Informationen Sie zum Konfigurieren von Netzwerk Sicherheitsregeln, um den eingehenden Verkehr zu einer App-Service-Umgebung zu steuern." 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>So steuern eingehenden Verkehr zu einer App-Service-Umgebung

## <a name="overview"></a>(Übersicht) ##
Eine App-Service-Umgebung erstellt werden können in **entweder** ein Ressourcenmanager Azure-virtuellen Netzwerk, Modellieren **oder** eine klassische Bereitstellung [virtuelles Netzwerk][virtualnetwork].  Ein neues virtuelles Netzwerk und neues Subnetz können gleichzeitig definiert werden, wenn, die eine App-Service-Umgebung erstellt wird.  Alternativ kann eine App-Service-Umgebung in einer bereits vorhandenen virtuelles Netzwerk und bereits vorhandenen Subnetz erstellt werden.  Mit der letzten Änderung in Juni 2016 kann ASEs jetzt in virtuelle Netzwerke bereitgestellt werden, die entweder öffentlichen Adressbereiche oder RFC1918 Adresse Leerzeichen (d. h. verwenden Private Adressen).  Weitere Informationen über das Erstellen einer App-Service-Umgebung [zum Erstellen einer App-Service-Umgebung]finden Sie unter[HowToCreateAnAppServiceEnvironment].

Eine App-Service-Umgebung müssen immer in einem Subnetz erstellt werden, da ein Subnetz eine Begrenzungslinie Netzwerk stellt die eingehenden Datenverkehr hinter übergeordneten Geräte und Dienste sperren, sodass HTTP und HTTPS-Datenverkehr nur von bestimmten übergeordneten IP-Adressen akzeptiert wird verwendet werden können.

Eingehende und ausgehende Netzwerkverkehr in einem Subnetz wird mithilfe einer [Netzwerksicherheitsgruppe]gesteuert[NetworkSecurityGroups]. Steuerung des eingehenden Datenverkehrs erfordert, dass Netzwerk Sicherheitsregeln in einer Netzwerksicherheitsgruppe erstellen und dann die Sicherheit Netzwerk zuweisen Gruppieren im Subnetz, enthält die App-Service-Umgebung.

Nachdem eine Netzwerksicherheitsgruppe mit einem Subnetz zugeordnet ist, eingehender Datenverkehr in die App-Service-Umgebung apps ist zulässig/blockiert basierend auf zulassen und verweigern in der Netzwerk-Sicherheitsgruppe definierte Regeln.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="network-ports-used-in-an-app-service-environment"></a>Network Ports, die in einer App-Service-Umgebung ##
Bevor Sie Sperren eingehenden Netzwerkdatenverkehr mit einer Netzwerksicherheitsgruppe, ist es den Satz von erforderlichen und optionalen Network Ports, die von einer App-Service-Umgebung beachtet.  Schließen versehentlich deaktiviert Datenverkehr für einige Ports kann Verlust der Funktionalität in einer App-Service-Umgebung führen.

Im folgenden finden eine Liste der Ports, indem Sie eine App-Service-Umgebung verwendet werden. Alle Ports sind **TCP**, sofern nicht anders klar angegeben:

- 454: von Azure-Infrastruktur für die Verwaltung und Wartung der App-Service-Umgebungen über SSL verwendet **Port erforderlich** .  Verkehr zu diesem Port nicht blockiert.  Dieser Port ist immer an den öffentlichen VIP von einer ASE gebunden.
- 455: von Azure-Infrastruktur für die Verwaltung und Wartung der App-Service-Umgebungen über SSL verwendet **Port erforderlich** .  Verkehr zu diesem Port nicht blockiert.  Dieser Port ist immer an den öffentlichen VIP von einer ASE gebunden.
- 80: Standard Port für eingehenden HTTP-Verkehr zu apps in App Service-Pläne in einer App-Service-Umgebung ausgeführt.  Klicken Sie auf eine ASE ILB aktiviert ist dieser Port auf die Adresse der ASE ILB gebunden.
- 443: Standard Port für den eingehenden SSL-Verkehr zu apps in App Service-Pläne in einer App-Service-Umgebung ausgeführt.  Klicken Sie auf eine ASE ILB aktiviert ist dieser Port auf die Adresse der ASE ILB gebunden.
- 21: Steuerelement Channel für FTP.  Dieser Port kann sicheres blockiert werden, wenn FTP nicht verwendet wird.  Klicken Sie auf eine ASE ILB aktiviert kann diesen Anschluss auf die Adresse ILB für eine ASE gebunden werden.
- 990: Steuerelement-Kanal für FTPS.  Dieser Port kann sicheres blockiert werden, wenn FTPS nicht verwendet wird.  Klicken Sie auf eine ASE ILB aktiviert kann diesen Anschluss auf die Adresse ILB für eine ASE gebunden werden.
- 10001-10020: Datenkanäle für FTP.  Wie Sie sich mit dem Steuerelement-Kanal, können diese Ports sicheres blockiert werden, wenn FTP nicht verwendet wird.  Klicken Sie auf eine ASE ILB aktiviert kann diesen Anschluss an die ASE des ILB Adresse gebunden werden.
- 4016: remote Debuggen mit Visual Studio 2012 verwendet.  Dieser Port kann sicheres blockiert werden, wenn das Feature nicht verwendet wird.  Klicken Sie auf eine ASE ILB aktiviert ist dieser Port auf die Adresse der ASE ILB gebunden.
- 4018: remote Debuggen mit Visual Studio 2013 verwendet.  Dieser Port kann sicheres blockiert werden, wenn das Feature nicht verwendet wird.  Klicken Sie auf eine ASE ILB aktiviert ist dieser Port auf die Adresse der ASE ILB gebunden.
- 4020: remote Debuggen mit Visual Studio 2015 verwendet.  Dieser Port kann sicheres blockiert werden, wenn das Feature nicht verwendet wird.  Klicken Sie auf eine ASE ILB aktiviert ist dieser Port auf die Adresse der ASE ILB gebunden.

## <a name="outbound-connectivity-and-dns-requirements"></a>Ausgehende Verbindungen und DNS-Anforderungen ##
Für eine App-Service-Umgebung ordnungsgemäß funktioniert ist es ausgehenden Zugriff zu verschiedenen Endpunkten erforderlich. Eine vollständige Liste der externen Endpunkte eines ASE untersuchten befindet sich im Abschnitt "Netzwerkkonnektivität erforderlich" dieses Artikels [Netzwerkkonfiguration für ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) .

App-Service-Umgebungen erfordern eine gültige DNS-Infrastruktur für das virtuelle Netzwerk konfiguriert.  Wenn aus irgendeinem Grund die DNS-Konfiguration geändert wird, nachdem ein App-Service-Umgebung erstellt wurde, können Entwickler erzwingen, dass ein App-Service-Umgebung, um die neue DNS-Konfiguration zu übernehmen.  Auslösen eines parallelen Umgebung Neustarts mithilfe des Symbols "Neu starten" am oberen Rand der App-Service-Umgebung Management vorher in der [Azure-Portal] ansässig[ NewPortal] bewirkt, dass die Umgebung für die neue DNS-Konfiguration abholen.

Es wird auch empfohlen, dass alle benutzerdefinierten DNS-Server auf den Vnet Setup im Voraus vor dem Erstellen einer App-Service-Umgebung sein.  Wenn ein virtuelles Netzwerk DNS-Konfiguration geändert wird, während der Erstellung einer App-Service-Umgebung, führt, die mit der App-Service-Umgebung Erstellung fehlschlägt.  In einer ähnlichen Vein Wenn Sie ein benutzerdefinierter DNS-Server vorhanden ist, klicken Sie auf das Ende eines Gateways VPN, und der DNS-Server ist nicht erreichbar oder nicht verfügbar ist, fehl der App-Service-Umgebung Erstellungsprozess außerdem.

## <a name="creating-a-network-security-group"></a>Erstellen einer Sicherheitsgruppe Netzwerk ##
Vollständige Details wie Netzwerk Arbeit Sicherheitsgruppen die folgenden [Informationen]finden Sie unter[NetworkSecurityGroups].  Die Details unten auf Touch hervorgehoben Netzwerk-Sicherheitsgruppen, mit Schwerpunkt auf Konfigurieren und Anwenden einer Netzwerksicherheitsgruppe mit einem Subnetz, die einer App-Service-Umgebung enthält.

**Hinweis:** Netzwerk-Sicherheitsgruppen grafisch mithilfe der [Azure-Portal](https://portal.azure.com) konfiguriert oder über Azure PowerShell.

Netzwerk-Sicherheitsgruppen werden zuerst als eigenständige Entität zugeordnet ein Abonnement erstellt. Da Netzwerk Sicherheitsgruppen in einer Azure Region erstellt wurden, müssen sicherstellen Sie, dass die Netzwerk-Sicherheitsgruppe in derselben Region als die App-Service-Umgebung erstellt wird.

Das folgende Beispiel veranschaulicht, erstellen eine Netzwerksicherheitsgruppe:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Nachdem eine Netzwerksicherheitsgruppe erstellt wurde, werden eine oder mehrere Netzwerk Sicherheitsregeln hinzugefügt.  Da Satz von Regeln im Laufe der Zeit ändern kann, empfiehlt es sich das Nummerierungsschema für Regel Prioritäten verwendet, um zusätzliche Regeln über einen Zeitraum einfügen erleichtern space.

Das folgende Beispiel zeigt eine Regel, die ausdrücklich gewährt Zugriff auf die Management-Ports, die von der Azure-Infrastruktur verwalten und Warten einer App-Service-Umgebung erforderlich.  Beachten Sie, dass alle Management Verkehr über SSL fließt und durch Client-Zertifikate gesichert wird, daher, obwohl die Ports geöffnet werden von allen als Azure-Infrastruktur nicht zugegriffen werden.


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    

Beim Zugriff auf port 80 und 443 ein App-Service-Umgebung hinter übergeordneten Geräte oder Dienste "ausgeblendet" blockieren, müssen Sie die übergeordneten IP-Adresse kennen.  Beispielsweise, wenn Sie eine Web-Anwendung Firewall (WAF) verwenden, die WAF besitzen eine eigene IP-Adresse (oder Adressen) die wann verwendet wird, als Proxy für Datenverkehr auf eine untergeordnete App Service-Umgebung.  Sie benötigen diese IP-Adresse in die *SourceAddressPrefix* -Parameter der Regel ein Netzwerk-Sicherheit zu verwenden.

Im folgenden Beispiel ist die eingehenden Verkehr von einer bestimmten übergeordneten IP-Adresse explizit zulässig.  Die Adresse *1.2.3.4* wird als Platzhalter für die IP-Adresse von einer übergeordneten WAF verwendet.  Ändern Sie den Wert entsprechend die Adresse, die von der übergeordneten Gerät oder einen bestimmten Dienst verwendet.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
Gegebenenfalls FTP-Unterstützung können die folgenden Regeln als Vorlage verwendet werden, den Zugriff auf den FTP-Steuerelement Port und die Daten Channel Ports gewähren.  Da FTP ein Protokoll dynamische ist, Sie zum Weiterleiten von FTP-Verkehr über ein traditionelle HTTP-/HTTPS-Firewall oder des Proxyservers Gerät möglicherweise nicht.  In diesem Fall müssen Sie zum Festlegen der *SourceAddressPrefix* auf einen anderen Wert – beispielsweise die IP-Adresse Zellbereich Entwickler oder Bereitstellung Maschinen, die auf der FTP-Clients ausgeführt werden. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Hinweis:** Port des Bereichs der Channel während der Preview-Zeitraum kann geändert werden.)

Wenn remote Debuggen mit Visual Studio verwendet wird, führen Sie die folgenden Regeln vor, wie den Zugriff gewähren.  Es gibt eine separate Regel für jede unterstützte Version von Visual Studio, da jeder Version für das Debuggen Remoteprozeduraufruf einen anderen Anschluss verwendet.  Wie bei FTP-Zugriff, möglicherweise remote Debuggen Datenverkehr nicht korrekt über einen herkömmlichen WAF oder Proxygerät angeordnet.  Stattdessen kann die IP-Adresse von Entwicklertools Computern ausgeführt werden Visual Studio Bereich der *SourceAddressPrefix* festgelegt werden.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-to-a-subnet"></a>Zuweisen einer Sicherheitsgruppe Netzwerk mit einem Subnetz ##
Eine Netzwerksicherheitsgruppe verfügt über eine Standard Sicherheitsregel, die wodurch Zugriff auf alle externen Datenverkehr verweigert wird.  Das Ergebnis aus den oben beschriebenen Netzwerk Sicherheitsregeln und der Standard-Sicherheitsregel Sperren eingehenden Datenverkehrs kombinieren, wird dieser nur Datenverkehr von Quellbilds, Bereiche, die eine Aktion *Zulassen* zugeordnet können Sie den Datenverkehr an apps unter in einer App-Service-Umgebung senden.

Nachdem eine Netzwerksicherheitsgruppe mit Sicherheitsregeln gefüllt wird, muss es an das Subnetz mit der App-Service-Umgebung zugewiesen werden soll.  Der Befehl Zuordnung verweist auf sowohl den Namen des virtuellen Netzwerks, in dem sich die App-Service-Umgebung befindet, als auch den Namen der im Subnetz, in dem die App-Service-Umgebung erstellt wurde.  

Das folgende Beispiel zeigt eine Netzwerk-Sicherheitsgruppe zu einer Subnetz und virtuelle Netzwerk zugewiesen wird:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Nachdem die Netzwerk Sicherheit gruppenzuordnung erfolgreich ist (die Zuordnung ist eine zeitintensive Vorgänge und kann einige Minuten dauern), nur eingehender Datenverkehr *Zulassen* Regeln entsprechen apps in der App-Service-Umgebung erfolgreich erreicht wird.

Vollständigkeit im folgenden Beispiel wird veranschaulicht, wie entfernen, und ordnen Sie die Netzwerk-Sicherheitsgruppe aus dem Subnetz somit Failoverservern:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Besonderheiten für explizite IP-SSL ##
Wenn eine app so konfiguriert ist, mit einer explizite IP-SSL Adresse (gilt für ASEs, die nur eine öffentliche VIP haben), verwenden Sie die standardmäßige IP-Adresse die App-Service-Umgebung, sondern HTTP- und HTTPS Datenverkehr Zahlungen in das Subnetz über einen anderen Satz von Ports als Ports 80 und 443.

Das einzelne Paar der von jeder SSL-IP-Adresse verwendeten Ports finden Sie in der Portalseite Benutzeroberfläche aus der App-Dienst Umgebung Details UX Blade.  Wählen Sie "alle Einstellungen"--> "IP-Adressen".  Das Blade "IP-Adressen" zeigt eine Tabelle aller explizit konfiguriertes SSL-IP-Adressen für die App-Service-Umgebung, sowie das spezielle Port-Paar, mit dem HTTP und HTTPS-Datenverkehr jedes SSL-IP-Adresse zugeordnet ist.  Es ist dieser Port-Paar, die beim Konfigurieren von Regeln in einer Netzwerksicherheitsgruppe für die Parameter DestinationPortRange verwendet werden soll.

Wenn eine app auf einem ASE mit IP-SSL konfiguriert ist, wird externe Kunden werden nicht angezeigt und müssen nicht über die speziellen Port Paar Zuordnung zu sorgen.  Den Datenverkehr in der apps fließt normalerweise in der konfigurierten SSL-IP-Adresse.  Die Übersetzung in die speziellen Portpaar geschieht automatisch intern beim endgültigen Abschnitt der Weiterleitung Datenverkehr in das Subnetz mit der ASE. 

## <a name="getting-started"></a>Erste Schritte

Um mit der App-Service-Umgebungen anzufangen, finden Sie unter [Einführung in die App-Service-Umgebung][IntroToAppServiceEnvironment]

Alle Artikel und wie-des für App-Service-Umgebungen in der [Infodatei für Service-Umgebungen Anwendung](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Details um sicher miteinander zu verbinden, die Back-End-Ressource aus einer App-Service-Umgebung apps finden Sie unter [sichere Verbindung mit Back-End-Ressourcen aus einer App-Service-Umgebung][SecurelyConnecttoBackend]

Weitere Informationen über die App-Verwaltungsdienst Azure-Plattform finden Sie unter [Azure-App-Verwaltungsdienst][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[SecurelyConnecttoBackend]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->
 
