<properties 
    pageTitle="Netzwerk-Architekturübersicht über App-Service-Umgebungen" 
    description="Übersicht über Architektur Netzwerk Suchtopologie OfApp Service-Umgebungen." 
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

# <a name="network-architecture-overview-of-app-service-environments"></a>Netzwerk-Architekturübersicht über App-Service-Umgebungen

## <a name="introduction"></a>Einführung ##
App-Service-Umgebungen werden stets innerhalb einer Subnetz eines [virtuelles Netzwerk] erstellt[ virtualnetwork] -apps, die in einer App-Service-Umgebung ausführen können Kommunikation mit privaten befindet sich innerhalb der gleichen virtuellen Netzwerk Suchtopologie Endpunkte.  Da Teile ihrer Infrastruktur virtuelles Netzwerk Kunden sperren können, ist es wichtig, den vorstehend beschriebenen Netzwerk Kommunikation Zahlungen zu verstehen, die mit einer App-Service-Umgebung auftreten.

## <a name="general-network-flow"></a>Standard-Netzwerk-Fluss ##
 
Wenn eine App-Service-Umgebung (ASE) eine öffentliche virtuelle IP-Adresse für apps verwendet, eintreffen alle eingehender Datenverkehr auf diesen öffentlichen VIP.  Dies umfasst HTTP und HTTPS-Datenverkehr für apps und anderen Datenverkehr für FTP, remote Debuggen Funktionalität und Azure Management Vorgänge an.  Eine vollständige Liste der die Ports (erforderlichen und optionalen), die auf der öffentlichen VIP verfügbaren finden Sie im Artikel zur [Steuerung der eingehenden Verkehr] [ controllinginboundtraffic] zu einer App-Service-Umgebung. 

App-Service-Umgebungen unterstützen auch laufenden apps, die nur für eine interne Adresse virtuelle Netzwerk, auch als ILB (internen Lastenausgleich) Adresse bezeichnet gebunden sind.  Klicken Sie auf eine ILB aktiviert ASE, HTTP und HTTPS-Datenverkehr für apps sowie remote Debuggen Aufrufe, werden in der Adresse ILB.  Für die am häufigsten verwendeten ILB-ASE Konfigurationen wird FTP-/ FTPS Datenverkehr auch auf die Adresse ILB werden.  Jedoch Azure Management Vorgänge weiterhin an der öffentlichen VIP von einer ILB Ports 454/455 Datenfluss werden aktiviert ASE.

Im nachstehenden Diagramm zeigt einen Überblick über die verschiedenen eingehenden und ausgehenden Netzwerk Zahlungen für eine App-Service-Umgebung, in denen die apps an eine öffentliche virtuelle IP-Adresse gebunden sind:

![Allgemeine Netzwerk Zahlungen][GeneralNetworkFlows]

Eine App-Service-Umgebung können mit einer Vielzahl von privaten Kunden Endpunkte kommunizieren.  Beispielsweise können ausführen in der App-Service-Umgebung apps mit Datenbank-Servern, die auf IaaS virtuellen Computern in der gleichen virtuellen Netzwerk Suchtopologie verbinden.

>[AZURE.IMPORTANT] Betrachten im Netzplandiagramm, werden die "andere berechnen Ressourcen" in einem anderen Subnetz als die App-Service-Umgebung bereitgestellt. Bereitstellen von Ressourcen in demselben Subnetz mit der ASE blockiert Konnektivität zwischen ASE und diese Ressourcen (mit Ausnahme von bestimmter innerhalb-ASE routing). Bereitstellen Sie in ein anderes Subnetz stattdessen (in der gleichen VNET). Die App-Service-Umgebung werden dann eine Verbindung herstellen. Es ist keine zusätzliche Konfiguration erforderlich.

App-Service-Umgebungen Kommunikation mit Sql-Datenbank und Azure-Speicher auch Ressourcen zum Verwalten von und Arbeiten mit einer App-Service-Umgebung erforderlich sind.  Einige der Sql und Speicher Ressourcen, mit denen ein App-Service-Umgebung kommuniziert befinden sich in derselben Region als die App-Service-Umgebung, während andere Personen in remote Azure Regionen befinden.  Ausgehende Verbindungen mit dem Internet ist daher immer für eine App-Service-Umgebung ordnungsgemäß erforderlich. 

Da eine App-Service-Umgebung in einem Subnetz bereitgestellt wird, können Sicherheitsgruppen Netzwerk eingehenden Datenverkehr an das Subnetz steuern verwendet werden.  Details zum Steuern eingehenden Verkehr zu einer App-Service-Umgebung finden Sie im folgenden [Artikel][controllinginboundtraffic].

Details zum ausgehende Verbindung mit dem Internet in einer App-Service-Umgebung ermöglichen finden Sie im folgenden Artikel über das Arbeiten mit [Express-Routing][ExpressRoute].  Die gleiche Weise, in dem Artikel beschriebenen angewendet wird, bei der Arbeit mit der Konnektivität zwischen Standorten und mit Erzwungene Tunnel.

## <a name="outbound-network-addresses"></a>Ausgehende Netzwerkadressen ##
Wenn eine App-Service-Umgebung ausgehende Anrufe vornimmt, wird eine IP-Adresse immer ausgehende Anrufe zugeordnet.  Die bestimmte IP-Adresse, die verwendet wird, hängt davon ab, ob der Endpunkt aufgerufen wird nur innerhalb der Suchtopologie virtuelles Netzwerk oder außerhalb der Suchtopologie virtuelles Netzwerk befindet.

Der Endpunkt aufgerufen wird ist **außerhalb** von der Suchtopologie virtuelles Netzwerk, klicken Sie dann die ausgehende (Adresse QuickInfos der ausgehenden NAT), die verwendet wird die öffentliche VIP des App-Service-Umgebung.  Diese Adresse kann in der Portal-Benutzeroberfläche für die App-Service-Umgebung in Eigenschaften Blade gefunden werden.
 
![Ausgehende IP-Adresse][OutboundIPAddress]

Diese Adresse kann auch für ASEs ermittelt werden, die nur eine öffentliche VIP aufweisen, durch Erstellen einer app in der App-Service-Umgebung verwenden und anschließend auf die Adresse des app Ausführen einer *Nslookup* . Die resultierende IP-Adresse wird sowohl die öffentlichen VIP als auch die App Service-Umgebung ausgehende NAT Adresse.

Wenn der Endpunkt aufgerufen wird **innerhalb** von der Suchtopologie virtuelles Netzwerk befindet, werden die ausgehende Adresse der app einen die interne IP-Adresse der Ressource einzelne berechnen die app ausgeführt.  Es ist jedoch nicht beständige Zuordnung von virtuellen Netzwerk internen IP-Adressen auf apps.  Apps können über die unterschiedlichen berechnen Ressourcen und dem Pool der verfügbaren berechnen, die Ressourcen in einer App-Service-Umgebung nicht ändern können aufgrund Skalierung Vorgänge navigieren.

Da eine App-Service-Umgebung immer in einem Subnetz befindet, sind Sie jedoch sichergestellt, dass die interne IP-Adresse einer berechnen Ressource Ausführen einer app immer im Bereich von CIDR das Subnetz liegen wird.  Wenn abgestimmte ACLs oder Netzwerk Sicherheitsgruppen zum Sichern des Zugriffs mit anderen Endpunkten innerhalb des virtuellen Netzwerks verwendet werden, muss die Subnetz-Bereich, der die App-Service-Umgebung enthält daher Zugriff erteilt werden.

Das folgende Diagramm veranschaulicht diese Konzepte ausführlicher:

![Ausgehende Netzwerkadressen][OutboundNetworkAddresses]

Im obigen Diagramm:

- Da die öffentliche VIP des App-Service-Umgebung 192.23.1.2 ist, ist, die die ausgehende IP-Adresse verwendet, wenn die Endpunkte "Internet" aufrufen.
- Der im enthaltenden Subnetz für die App-Service-Umgebung CIDR liegt 10.0.1.0/26.  Andere Endpunkte innerhalb derselben virtuelles Netzwerkinfrastruktur werden Anrufe von apps als Ursprung an einer beliebigen Stelle in diesem Adressbereich angezeigt.

## <a name="calls-between-app-service-environments"></a>Anrufe zwischen Umgebungen App-Dienst ##
Ein Szenario komplexeres kann auftreten, wenn Sie mehrere App Service-Umgebungen im gleichen virtuellen Netzwerk bereitstellen, und nehmen ausgehende Anrufe von einer App-Service-Umgebung in eine andere App-Service-Umgebung.  Diese Typen von cross App-Service-Umgebung Anrufe auch als "Internet" Anrufe behandelt werden sollen.

Die folgende Abbildung zeigt ein Beispiel für eine mehrstufige Architektur mit apps auf eine App-Service-Umgebung (z. B. "Vorderseite Tür-" Web apps) Aufrufen von apps für eine zweite App-Service-Umgebung (z. B. interne Back-End-API apps sollen nicht aus dem Internet zugänglich sein). 

![Anrufe zwischen Umgebungen App-Dienst][CallsBetweenAppServiceEnvironments] 

Im obigen Beispiel enthält die App-Service-Umgebung "ASE eine" ausgehende IP-Adresse 192.23.1.2.  Wenn eine app ausgeführt wird, klicken Sie auf diese App-Service-Umgebung macht ein ausgehender Anruf zu einer app ausgeführt wird, klicken Sie auf einer zweiten App Dienst Umgebung ("ASE zwei") im gleichen virtuellen Netzwerk, den ausgehenden Anruf zur Verfügung stehen, die als einen Videoanruf "Internet" behandelt werden.  Daher den Datenverkehr auf das zweite kommen App-Service-Umgebung wird angezeigt als 192.23.1.2 (d. h. nicht Subnetz Bereich von der ersten App-Service-Umgebung) stammt.

Obwohl beide App-Service-Umgebungen in der gleichen Azure Region ansässig sind, der Datenverkehr bleibt in den regionalen Azure Netzwerk und werden nicht über das öffentliche Internet physisch Datenfluss, Anrufe zwischen verschiedenen Umgebungen der App-Service wie "Internet"-Aufrufe, behandelt werden.  Daher können Sie eine Netzwerksicherheitsgruppe im Subnetz des zweiten App-Service-Umgebung verwenden, dürfen nur eingehende Anrufe aus der ersten App Dienst Umgebung (192.23.1.2 ist, dessen ausgehende IP-Adresse), wodurch sichergestellt wird sicheren Kommunikation zwischen den App-Service-Umgebung.

## <a name="additional-links-and-information"></a>Weitere Links und Informationen ##
Alle Artikel und wie-des für App-Service-Umgebungen in der [Infodatei für Service-Umgebungen Anwendung](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Details auf eingehende von App-Service-Umgebungen verwendeten Ports und mit Sicherheitsgruppen Netzwerk eingehenden Datenverkehr steuern steht [hier][controllinginboundtraffic].

Details zur Verwendung von Benutzer definiert leitet ausgehende Zugriff auf das Internet Service-Umgebungen App steht in diesem [Artikel]gewähren[ExpressRoute]. 


<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[ExpressRoute]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

