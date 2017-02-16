<properties 
   pageTitle="ExpressRoute Problembehandlung Leitfaden – erste ARP Tabellen | Microsoft Azure"
   description="Diese Seite enthält Anweisungen zur ARP Abrufen von Tabellen für eine Verbindung ExpressRoute"
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carolz"
   editor="tysonn"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="ganesr"/>

#<a name="expressroute-troubleshooting-guide---getting-arp-tables-in-the-resource-manager-deployment-model"></a>Leitfaden ExpressRoute Problembehandlung – erste ARP Tabellen im Bereitstellungsmodell Ressourcenmanager

> [AZURE.SELECTOR]
[PowerShell - Ressourcenmanager](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell - Klassisch](expressroute-troubleshooting-arp-classic.md)

In diesem Artikel führt Sie durch die Schritte, um die ARP-Tabellen für Ihre ExpressRoute Verbindung zu. 

>[AZURE.IMPORTANT] Dieses Dokument soll Ihnen helfen diagnostizieren und Beheben von Problemen mit einfachen. Es ist kein Ersatz für Microsoft-Support werden soll. Wenn Sie nicht zur Lösung des Problems mit der Anleitung unten beschriebenen können, müssen Sie ein Ticket Support mit [Microsoft support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) öffnen.

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adresse Auflösung Protocol (ARP) und ARP-Tabellen
Mit einer Auflösung von ARP (Address Protocol) ist ein in [RFC 826 zum](https://tools.ietf.org/html/rfc826)Layer 2-Protokoll. ARP dient zum Zuordnen der Ethernet-Adresse (MAC) eine IP-Adresse.

Die ARP-Tabelle enthält eine Zuordnung der IPv4-Adresse und MAC-Adresse für einen bestimmten peering. Die für eine ExpressRoute Verbindung peering ARP-Tabelle enthält die folgenden Informationen für jede Schnittstelle (primären und sekundären)

1. Zuordnung von lokalen Router Schnittstelle IP-Adresse in die MAC-Adresse
2. Zuordnung ExpressRoute Router Schnittstelle IP-Adresse in die MAC-Adresse
3. Alter der Zuordnung

ARP-Tabellen können Layer 2-Konfiguration zu überprüfen und grundlegende Problembehandlung der Ebene 2 Netzwerkkonnektivitätsprobleme vor. 

ARP Beispieltabelle: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Im folgende Abschnitt erläutert, wie Sie die ARP-Tabellen, die von den ExpressRoute Kante Routern gesehen anzeigen können. 

## <a name="prerequisites-for-learning-arp-tables"></a>Erforderliche Komponenten zum Erlernen von ARP-Tabellen

Stellen Sie sicher, dass Sie die folgenden haben, bevor Sie weiteren Fortschritt

 - Eine gültige ExpressRoute-Verbindung mit mindestens eine peering konfiguriert. Die Verbindung muss vom Anbieter Connectivity vollständig konfiguriert sein. Sie (oder Ihrem Anbieter Connectivity) müssen Sie mindestens eines der Peerings (Azure private, Azure öffentlichen und Microsoft) auf dieser Verbindung konfiguriert haben.
 - IP-Adressbereiche zum Konfigurieren der Peerings (Azure private, Azure öffentlichen und Microsoft) verwendet. Überprüfen Sie die IP-Adresse Zuordnung Beispiele [ExpressRoute routing Anforderungen Seite](expressroute-routing.md) zu verstehen, wie Schnittstellen auf der Seite, und klicken Sie auf der Seite ExpressRoute IP-Adressen zugeordnet werden. Sie erhalten Informationen von der Peeringliste Konfiguration, indem der [ExpressRoute Peeringliste Konfigurationsseite](expressroute-howto-routing-arm.md)überprüfen.
 - Informationen aus Ihrem Team Netzwerke / Connectivity-Anbieter auf die MAC-Adressen der Schnittstellen für diese IP-Adressen verwendet.
 - Sie müssen das neueste PowerShell-Modul für Azure (Version 1,50 oder höher).

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>Abrufen der ARP Tabellen für Ihre ExpressRoute-Verbindung
Dieser Abschnitt enthält Anweisungen auf können Sie die ARP-Tabellen pro peering mithilfe der PowerShell anzeigen. Sie oder Ihr Connectivity-Anbieter muss die peering vor weiteren voranschreiten konfiguriert haben. Jede Verbindung verfügt über zwei Pfade (primären und sekundären). Sie können die ARP-Tabelle für jeden Pfad unabhängig überprüfen.

### <a name="arp-tables-for-azure-private-peering"></a>ARP Tabellen für Azure private peering
Das folgende Cmdlet stellt das ARP Tabellen für Azure private peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary
        
        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Beispiel für die Ausgabe ist für einen der Pfade unten dargestellt.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP Tabellen für Azure öffentlichen peering
Das folgende Cmdlet stellt das ARP Tabellen für Azure öffentlichen peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary
        
        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Beispiel für die Ausgabe ist für einen der Pfade unten dargestellt.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Für Microsoft peering ARP-Tabellen
Das folgende Cmdlet stellt das ARP Tabellen für Microsoft peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary
        
        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Beispiel für die Ausgabe ist für einen der Pfade unten dargestellt.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>So verwenden Sie diese Informationen
Die ARP-Tabelle mit einer peering kann verwendet werden, um zu bestimmen Layer 2-Konfiguration und Konnektivität zu überprüfen. Dieser Abschnitt enthält einen Überblick darüber, wie ARP Tabellen unter verschiedenen Szenarios aussehen werden.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>Wenn eine Verbindung in Betrieb Bundesstaat (erwarteten State) wird ARP-Tabelle

 - Die ARP-Tabelle kann es sich um einen Eintrag für die lokale Seite mit einer gültigen IP-Adresse und MAC-Adresse und eine ähnliche Eintrag für die Microsoft-Seite sind. 
 - Das letzte Oktett der lokale IP-Adresse wird immer eine ungerade Zahl sein.
 - Das letzte Oktett der Microsoft-IP-Adresse wird immer eine gerade Zahl sein.
 - Klicken Sie auf der Microsoft-Seite für alle 3 Peerings (primäre / sekundäre) wird dieselbe MAC-Adresse angezeigt. 


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP Tabelle, wenn lokale / Connectivity Anbieterseite weist Probleme

 - Nur ein Eintrag wird in der ARP Tabelle angezeigt. Dadurch wird die Zuordnung zwischen dem MAC-Adresse und in der Microsoft-Seite verwendete IP-Adresse angezeigt. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Öffnen Sie eine Supportanfrage Connectivity Anbieter solche Probleme Debuggen aus. 


### <a name="arp-table-when-microsoft-side-has-problems"></a>Wenn Microsoft auf Probleme besitzt ARP-Tabelle

 - Sie sehen keine ARP-Tabelle, die für eine peering an, wenn es Probleme auf der Microsoft-Seite liegen angezeigt. 
 -  Öffnen einer Support-Ticket mit [Microsoft unterstützen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Geben Sie an, dass Sie ein Problem mit der Ebene 2 Connectivity haben. 

## <a name="next-steps"></a>Nächste Schritte

 - Überprüfen Sie die Layer 3 Konfigurationen für Ihre ExpressRoute-Verbindung
     - Abrufen von Routing Zusammenfassung, um den Status der BGP Sitzungen zu ermitteln 
     - Abrufen von Routingtabelle, um zu bestimmen, welche Präfixe über ExpressRoute bekannt gegeben werden
 - Überprüfen Sie die Datenübertragung durch Bytes in / out-überprüfen
 - Wenn weiterhin Probleme auftreten, öffnen Sie mit [Unterstützung von Microsoft](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Support-Ticket.
