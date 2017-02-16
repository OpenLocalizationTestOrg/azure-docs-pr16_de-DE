<properties 
    pageTitle="Sicheres Herstellen einer Verbindung mit Back-End-Ressourcen aus einer App-Service-Umgebung" 
    description="Informationen Sie zum sichere Verbinden mit Back-End-Ressourcen aus einer App-Service-Umgebung." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="stefsch"/>   

# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a>Sicheres Herstellen einer Verbindung mit Back-End-Ressourcen aus einer App-Service-Umgebung #

## <a name="overview"></a>(Übersicht) ##
Da eine App-Service-Umgebung immer erstellt wird in **entweder** ein Ressourcenmanager Azure-virtuellen Netzwerk, Modellieren **oder** eine klassische Bereitstellung [virtuelles Netzwerk][virtualnetwork], ausgehende Verbindungen aus einer App-Service-Umgebung zu anderen Back-End-Ressourcen können ausschließlich über das virtuelle Netzwerk Datenfluss.  Mit der letzten Änderung in Juni 2016 kann ASEs auch in virtuelle Netzwerke bereitgestellt werden, die entweder öffentlichen Adressbereiche oder RFC1918 Adresse Leerzeichen (d. h. verwenden Private Adressen).  

Möglicherweise gibt es beispielsweise einer SQL Server ausgeführt wird, in einem Cluster aus virtuellen Computern mit Port 1433 gesperrt.  Der Endpunkt möglicherweise ACLd nur Zugriff von anderen Ressourcen in der gleichen virtuellen Netzwerk zulassen.  

Klicken Sie ein weiteres Beispiel vertrauliche Endpunkte lokal ausgeführt werden können, und mit Azure über entweder [Zwischen Standorten] verbunden sein[ SiteToSite] oder [Azure ExpressRoute] [ ExpressRoute] Verbindungen.  Daher werden nur Ressourcen virtuelle Netzwerke verbunden, zu der Website-zu-Standort oder ExpressRoute Tunnel auf lokale Endpunkte zugreifen.

Für alle diese Szenarios werden apps für eine App-Service-Umgebung ausgeführt kann eine sichere Verbindung herstellen mit den verschiedenen Servern und Ressourcen.  Ausgehenden Datenverkehr von apps, die in einer App-Service-Umgebung ausgeführt werden, um private Endpunkte im gleichen virtuellen Netzwerk (oder mit dem gleichen virtuellen Netzwerk verbunden), wird nur Verkehr über das virtuelle Netzwerk.  Private Endpunkte ausgehender Datenverkehr fließt nicht über das öffentliche Internet.

Eine Einschränkung gilt für ausgehenden Datenverkehr aus einer App-Service-Umgebung an Endpunkte innerhalb eines virtuellen Netzwerks.  App-Service-Umgebungen können nicht erreichen Endpunkte des virtuellen Computern, die sich in **demselben** Subnetz wie die App-Service-Umgebung befindet.  Dies sollte normalerweise ein Problem nicht als App-Service-Umgebungen in einem Subnetz vorbehalten ausschließlich nur die App-Service-Umgebung bereitgestellt werden.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="outbound-connectivity-and-dns-requirements"></a>Ausgehende Verbindungen und DNS-Anforderungen ##
Für eine App-Service-Umgebung ordnungsgemäß funktioniert ist es ausgehenden Zugriff zu verschiedenen Endpunkten erforderlich. Eine vollständige Liste der externen Endpunkte eines ASE untersuchten befindet sich im Abschnitt "Netzwerkkonnektivität erforderlich" dieses Artikels [Netzwerkkonfiguration für ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) .

App-Service-Umgebungen erfordern eine gültige DNS-Infrastruktur für das virtuelle Netzwerk konfiguriert.  Wenn Sie aus irgendeinem Grund die DNS-Konfiguration geändert wird, nachdem ein App-Service-Umgebung erstellt wurde, können Entwickler eine App Service-Umgebung, um die neue DNS-Konfiguration abholen erzwingen.  Auslösen eines paralleles Umgebung Neustarts verwenden das Symbol "Restart" am oberen Rand der App-Service-Umgebung Management vorher in das Portal bewirkt, dass die Umgebung, um die neue DNS-Konfiguration zu übernehmen.

Es wird auch empfohlen, dass alle benutzerdefinierten DNS-Server auf den Vnet Setup im Voraus vor dem Erstellen einer App-Service-Umgebung sein.  Wenn ein virtuelles Netzwerk DNS-Konfiguration geändert wird, während der Erstellung einer App-Service-Umgebung, führt, die mit der App-Service-Umgebung Erstellung fehlschlägt.  In einer ähnlichen Vein Wenn Sie ein benutzerdefinierter DNS-Server vorhanden ist, klicken Sie auf das Ende eines Gateways VPN, und der DNS-Server ist nicht erreichbar oder nicht verfügbar ist, fehl der App-Service-Umgebung Erstellungsprozess außerdem.

## <a name="connecting-to-a-sql-server"></a>Herstellen einer Verbindung mit einem SQLServer
Allgemeine SQL Server-Konfiguration weist einen Endpunkt Port 1433 Abhören:

![SQL Server-Endpunkt][SqlServerEndpoint]

Es gibt zwei Vorgehensweisen für den Datenverkehr an diesen Endpunkt einschränken:


- [Network Access Control Lists] [ NetworkAccessControlLists] (Netzwerk ACLs)

- [Netzwerk-Sicherheitsgruppen][NetworkSecurityGroups]


## <a name="restricting-access-with-a-network-acl"></a>Einschränken des Zugriffs mit einem Netzwerk ACL

Port 1433 kann mithilfe einer Access-Steuerelement Netzwerkliste gesichert werden.  Im Beispiel unten weißen Client Adressen innerhalb eines virtuellen Netzwerks stammen, und blockiert den Zugriff auf alle anderen Clients.

![Beispiel für eine Liste Network Access-Steuerelement][NetworkAccessControlListExample]

Alle Programme App Dienst Umgebung im gleichen virtuellen Netzwerk wie der SQL Server Verbindung zu die **internen VNet** IP-Adresse für den virtuellen SQL Server-Computer mit SQL Server-Instanz hergestellt werden.  

Im folgenden Beispiel Verbindungszeichenfolge verweist auf die SQL Server mithilfe der privaten IP-Adresse.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Obwohl des virtuellen Computers als auch einen öffentlichen Endpunkt aufweist, werden versucht, eine Verbindung mit der öffentlichen IP-Adresse aufgrund im Netzwerk ACL abgelehnt. 

## <a name="restricting-access-with-a-network-security-group"></a>Einschränken des Zugriffs mit einer Sicherheitsgruppe Netzwerk
Eine weitere Möglichkeit zum Sichern des Zugriffs ist mit einer Netzwerksicherheitsgruppe.  Netzwerk-Sicherheitsgruppen können einzelne virtuellen Computern oder ein Subnetz mit virtuellen Computern angewendet werden.

Zunächst muss eine Netzwerksicherheitsgruppe erstellt werden:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Einschränken des Zugriffs auf nur VNet internen Datenverkehr ist sehr einfach, mit einer Netzwerksicherheitsgruppe.  Die Regeln für die in einer Netzwerksicherheitsgruppe ermöglichen nur Zugriff von anderen Netzwerkclients im gleichen virtuellen Netzwerk an.

Als Ergebnis Sperren von Access mit SQL Server ist so einfach wie das Anwenden einer Netzwerksicherheitsgruppe mit seiner standardmäßigen Regeln entweder den virtuellen Computern unter SQL Server oder das Subnetz mit den virtuellen Computern.

Im folgenden Beispiel wird im enthaltenden Subnetz eine Netzwerksicherheitsgruppe gilt:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'
    
Das Ergebnis ist eine Reihe von Sicherheitsregeln, die externen Zugriff blockieren, wodurch VNet internen zugegriffen werden kann:

![Regeln für das standardmäßige Netzwerk Sicherheit][DefaultNetworkSecurityRules]


## <a name="getting-started"></a>Erste Schritte
Alle Artikel und wie-des für App-Service-Umgebungen in der [Infodatei für Service-Umgebungen Anwendung](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Um mit der App-Service-Umgebungen anzufangen, finden Sie unter [Einführung in die App-Service-Umgebung][IntroToAppServiceEnvironment]

Details zum Steuern der eingehenden Datenverkehrs an Ihre App-Service-Umgebung finden Sie unter [steuern eingehenden Verkehr zu einer App-Service-Umgebung][ControlInboundASE]

Weitere Informationen über die App-Verwaltungsdienst Azure-Plattform finden Sie unter [Azure-App-Verwaltungsdienst][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
