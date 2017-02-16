<properties
   pageTitle="ExpressRoute zur Problembehandlung Leitfaden: erste ARP Tabellen | Microsoft Azure"
   description="Diese Seite enthält Anweisungen zum Abrufen von Tabellen für eine Verbindung ExpressRoute für des ARP."
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

# <a name="expressroute-troubleshooting-guide-getting-arp-tables-in-the-classic-deployment-model"></a>ExpressRoute zur Problembehandlung Leitfaden: erste ARP Tabellen im Bereitstellungsmodell klassischen

> [AZURE.SELECTOR]
[PowerShell - Ressourcenmanager](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell - Klassisch](expressroute-troubleshooting-arp-classic.md)

In diesem Artikel führt Sie durch die Schritte zum Abrufen von Tabellen mit einer Auflösung von ARP (Address Protocol) für Ihre Azure ExpressRoute Verbindung.

>[AZURE.IMPORTANT] Dieses Dokument soll Ihnen helfen diagnostizieren und Beheben von Problemen mit einfachen. Es ist kein Ersatz für Microsoft-Support werden soll. Wenn Sie das Problem lösen können mithilfe der folgenden Anleitung, öffnen Sie eine Supportanfrage mit [Microsoft Azure-Hilfe + Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Adresse Auflösung Protocol (ARP) und ARP-Tabellen
ARP ist ein Layer 2-Protokoll, das in [RFC 826](https://tools.ietf.org/html/rfc826)definiert ist. ARP wird verwendet, um eine IP-Adresse eine Ethernet-Adresse (MAC-Adresse) zugeordnet wird.

Eine ARP Tabelle enthält eine Gegenüberstellung der IPv4-Adresse und MAC-Adresse für einen bestimmten peering. Die ARP-Tabelle für eine ExpressRoute Verbindung peering bietet die folgende Informationen für jede Schnittstelle (primären und sekundären):

1. Zuordnen einer lokalen Router Schnittstelle IP-Adresse an eine MAC-Adresse
2. Zuordnen einer ExpressRoute Router Schnittstelle IP-Adresse an eine MAC-Adresse
3. Das Alter der Zuordnung

ARP-Tabellen können Layer 2-Konfiguration überprüfen und Behandeln von Problemen mit grundlegenden Ebene 2 Connectivity helfen.

Es folgt ein Beispiel für eine Tabelle ARP:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Der folgende Abschnitt enthält Informationen dazu, wie die ARP-Tabellen anzeigen möchten, die von den ExpressRoute Kante Routern angezeigt werden.

## <a name="prerequisites-for-using-arp-tables"></a>Voraussetzung für die Verwendung von ARP-Tabellen

Stellen Sie sicher, dass Sie die folgenden haben, bevor Sie fortfahren:

 - Eine gültige ExpressRoute-Verbindung, die mit mindestens eine peering konfiguriert ist. Die Verbindung muss vom Anbieter Connectivity vollständig konfiguriert sein. Sie (oder Ihrem Anbieter Connectivity) müssen Sie mindestens eines der Peerings (Azure private, Azure öffentlichen oder Microsoft) auf dieser Verbindung konfigurieren.

 - IP-Adressbereiche, die zum Konfigurieren der Peerings (Microsoft und Azure private, Azure öffentlichen) verwendet werden. Überprüfen Sie die IP-Adresse Zuordnung Beispiele [ExpressRoute routing Anforderungen Seite](expressroute-routing.md) zu verstehen, wie Schnittstellen auf Ihre Aise, und klicken Sie auf der Seite ExpressRoute IP-Adressen zugeordnet werden. Sie erhalten Informationen zur Konfiguration Peeringliste, indem der [ExpressRoute Peeringliste Konfigurationsseite](expressroute-howto-routing-classic.md)überprüfen.

 - Informationen aus Ihrem Netzwerke Team oder Connectivity-Anbieter die MAC-Adressen der Schnittstellen, die für diese IP-Adressen verwendet werden.

 - Das neueste Windows PowerShell-Modul für Azure (Version 1,50 oder höher).

## <a name="arp-tables-for-your-expressroute-circuit"></a>ARP Tabellen für Ihre ExpressRoute-Verbindung
Dieser Abschnitt enthält Anweisungen dazu, wie Sie die ARP-Tabellen für jeden mithilfe der PowerShell peering anzeigen. Bevor Sie fortfahren, muss Sie oder Ihrem Anbieter Connectivity der peering konfigurieren. Jede Verbindung verfügt über zwei Pfade (primären und sekundären). Sie können die ARP-Tabelle für jeden Pfad unabhängig überprüfen.

### <a name="arp-tables-for-azure-private-peering"></a>ARP Tabellen für Azure private peering
Das folgende Cmdlet bietet die ARP Tabellen für Azure private peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Es folgt eine Ausgabe von einem der Pfade:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP Tabellen für Azure öffentlichen peering:
Das folgende Cmdlet bietet die ARP Tabellen für Azure öffentlichen peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Es folgt eine Ausgabe von einem der Pfade:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Es folgt eine Ausgabe von einem der Pfade:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Für Microsoft peering ARP-Tabellen
Das folgende Cmdlet bietet die ARP Tabellen für Microsoft peering:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Beispiel für die Ausgabe ist für einen der Pfade unten dargestellt:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>So verwenden Sie diese Informationen
Schicht 2-Konfiguration und Konnektivität überprüft, kann die ARP-Tabelle mit einer peering verwendet werden. Dieser Abschnitt enthält einen Überblick über die Darstellung von ARP-Tabellen in anderen Szenarien.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>Wenn eine Verbindung in einem Betrieb (erwartet) Zustand ist ARP-Tabelle

 - Die ARP-Tabelle hat einen Eintrag für die lokale Seite mit einer gültigen IP-Adresse und MAC-Adresse und eine ähnliche Eintrag für die Microsoft-Seite.
 - Das letzte Oktett der lokale IP-Adresse ist immer eine ungerade Zahl.
 - Das letzte Oktett der Microsoft-IP-Adresse ist immer eine gerade Zahl an.
 - Dieselbe MAC-Adresse angezeigt wird, klicken Sie auf der Microsoft-Seite für alle drei Peerings (primäre/sekundäre).


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>Wenn es sich um lokale oder, wenn der Konnektivität Anbieterseite Probleme hat ARP-Tabelle

 Nur ein Eintrag wird in der ARP Tabelle angezeigt. Es zeigt die Zuordnung zwischen die MAC-Adresse und die IP-Adresse, die auf der Seite Microsoft verwendet wird.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Wenn ein Problem wie folgt auftreten, öffnen Sie eine Supportanfrage mit Ihren Connectivity-Anbieter, um es zu beheben.


### <a name="arp-table-when-the-microsoft-side-has-problems"></a>Wenn Sie die Microsoft-Seite Probleme besitzt ARP-Tabelle

 - Sie sehen keine ARP-Tabelle, die für eine peering an, wenn es Probleme auf der Microsoft-Seite liegen angezeigt.
 -  Öffnen Sie eine Supportanfrage mit [Microsoft Azure-Hilfe + Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Geben Sie an, dass Sie ein Problem mit Layer 2-Konnektivität verfügen.

## <a name="next-steps"></a>Nächste Schritte

 - Schicht 3 Konfigurationen für Ihre ExpressRoute Verbindung zu überprüfen:
     - Abrufen einer Routing Zusammenfassung, um den Status der BGP Sitzungen zu ermitteln.
     - Abrufen einer Routingtabelle, um zu bestimmen, welche Präfixe über ExpressRoute bekannt gegeben werden.
 - Überprüfen Sie die Datenübertragung durch Bytes ein-und überprüfen.
 - Öffnen Sie eine Supportanfrage mit [Microsoft Azure-Hilfe + Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , wenn Sie weiterhin Probleme auftreten.
