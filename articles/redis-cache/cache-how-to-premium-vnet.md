<properties 
    pageTitle="Konfigurieren von Virtual Network-Unterstützung für einen Premium Azure Redis Cache | Microsoft Azure" 
    description="Informationen Sie zum Erstellen und Verwalten von Virtual Network-Unterstützung für Ihre Premium Ebene Azure Redis Cache Instanzen" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="how-to-configure-virtual-network-support-for-a-premium-azure-redis-cache"></a>So konfigurieren Sie die virtuelle Netzwerk-Unterstützung für einen Premium Azure Redis Cache
Azure Redis Cache weist verschiedene Cache Angebote die Flexibilität bei der Wahl der Cachegröße und Funktionen, einschließlich der neuen Premium Stufe zur Verfügung zu stellen.

Die Azure Redis Cache Ebene Zusatzfunktionen gehören Cluster, Beibehaltung und Unterstützung für virtuelle Netzwerke (VNet). Eine VNet ist ein privates Netzwerk in der Cloud. Wenn eine Instanz Azure Redis Cache mit einem VNet konfiguriert ist, ist nicht öffentlich adressiert und kann nur von virtuellen Computern und Applikationen innerhalb der VNet zugegriffen werden. Dieser Artikel beschreibt, wie virtual Network-Unterstützung für eine Instanz der Premium Azure Redis Cache konfigurieren.

>[AZURE.NOTE] Azure Redis Cache unterstützt beide klassischen und Cloud VNets.

Informationen zu anderen Premium Cachefeatures finden Sie unter [Einführung in die Ebene Azure Redis Cache Premium](cache-premium-tier-intro.md).

## <a name="why-vnet"></a>Warum VNet?
Bereitstellung von [Azure-virtuellen Netzwerk (VNet)](https://azure.microsoft.com/services/virtual-network/) bietet verbesserte Sicherheit und Isolation für Ihren Azure Redis Cache als auch Subnetze, Steuerelement-Richtlinien und weitere Features zum Einschränken des Zugriffs auf Azure Redis Cache weiter.

## <a name="virtual-network-support"></a>Virtuelle Netzwerk-support
Unterstützung für virtuelle Netzwerke (VNet) wird auf der **Neuen Redis Cache** Blade während der Erstellung des Caches für konfiguriert. 

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Nachdem Sie eine Ebene Preise Premium ausgewählt haben, können Sie Azure Redis Cache VNet Integration durch Auswahl eines VNet, der in derselben Abonnement und Speicherort wie dem Cache konfigurieren. Um eine neue VNet verwenden möchten, erstellen sie zuerst gemäß die Anweisungen in [Erstellen Sie ein virtuelles Netzwerk über das Azure-Portal](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) oder [Erstellen Sie ein virtuelles Netzwerk (klassisch) mithilfe des Azure-Portals](../virtual-network/virtual-networks-create-vnet-classic-portal.md) und dann an die **Neue Redis Cache** Blade erstellen und Konfigurieren des Caches Premium zurückzukehren.

Um die VNet für Ihren neuen Cache zu konfigurieren, klicken Sie in das **Neue Redis Cache** Blade **Virtuelle Netzwerk** , und wählen Sie die gewünschten VNet aus der Dropdownliste aus.

![Virtuelles Netzwerk][redis-cache-vnet]

Wählen Sie das gewünschte Subnetz aus der Dropdownliste **Subnetz** aus, und geben Sie die gewünschte **statische IP-Adresse**. Das Feld **statische IP-Adresse** ist optional, wenn Sie eine klassische VNet verwenden, und wenn keine angegeben ist, wird eine aus dem ausgewählten Subnetz ausgewählt.

>[AZURE.IMPORTANT] Wenn eine Azure Redis Cache auf einer Cloud VNet bereitstellen zu können, muss der Cache in einem dedizierten Subnetz, die keine anderen Ressourcen eine Ausnahme bilden jedoch Azure Redis Cache Instanzen enthält. Wenn versucht wird, eine Cloud VNet mit einem Subnetz einen Azure Redis Cache bereitstellen, enthält weitere Ressourcen, die Bereitstellung fehlschlägt.

![Virtuelles Netzwerk][redis-cache-vnet-ip]

>[AZURE.IMPORTANT] Die ersten vier Adressen in einem Subnetz sind reserviert und können nicht verwendet werden. Weitere Informationen finden Sie unter [gibt es Einschränkungen mit IP-Adressen in diesen Subnetzen?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Nachdem der Cache erstellt wurde, können Sie die Konfiguration für die VNet durch Klicken auf **Virtuelle Netzwerk** aus dem Blade **Einstellungen** anzeigen.

![Virtuelles Netzwerk][redis-cache-vnet-info]


Geben Sie den Hostnamen des Ihren Cache zum Verbinden mit Ihrer Azure Redis Cacheinstanz bei Verwendung eines VNet, in der Verbindungszeichenfolge wie im folgenden Beispiel dargestellt.

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5premium.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## <a name="azure-redis-cache-vnet-faq"></a>Azure Redis Cache VNet häufig gestellte Fragen

Die folgende Liste enthält Antworten auf häufig gestellte Fragen zu den Azure Redis Cache Skalierung.

-   [Was sind einige der häufigsten falsche Konfiguration Probleme mit Azure Redis Cache und VNets?](#what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets)
-   [Kann ich mit einem Cache standard oder grundlegende VNets verwenden?](#can-i-use-vnets-with-a-standard-or-basic-cache)
-   [Warum schlägt erstellen einen Redis Cache in einigen Subnetze jedoch nicht andere fehl?](#why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others)
-   [Funktionieren alle Cachefeatures beim Hosten von eines Caches in einem VNET?](#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)


## <a name="what-are-some-common-misconfiguration-issues-with-azure-redis-cache-and-vnets"></a>Was sind einige der häufigsten falsche Konfiguration Probleme mit Azure Redis Cache und VNets?

Wenn in einer VNet Azure Redis Cache gehostet wird, werden die Ports in der folgenden Tabelle verwendet. Wenn diese Ports blockiert werden, kann der Cache nicht ordnungsgemäß. Haben Sie eine oder mehrere der folgenden Ports blockiert ist die am häufigsten verwendeten falsche Konfiguration Problem wenn Azure Redis Cache in einem VNet verwenden.

| Ports     | Richtung        | Transportregel-Protokoll | Zweck                                                                           | Remote-IP-                           |
|-------------|------------------|--------------------|-----------------------------------------------------------------------------------|-------------------------------------|
| 80, 443     | Ausgehende         | TCP                | Redis Abhängigkeiten auf Azure Speicher/PKI (Internet)                                | *                                   |
| 53          | Ausgehende         | TCP/UDP            | Redis Abhängigkeiten von DNS-Einträge (Internet/VNet)                                         | *                                   |
| 6379, 6380  | Eingehende          | TCP                | Clientkommunikation mit Redis, Azure Lastenausgleich                               | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 8443        | Ein-/ausgehende | TCP                | Implementierungsdetails für Redis                                                   | VIRTUAL_NETWORK                     |
| 8500        | Eingehende          | TCP/UDP            | Azure Lastenausgleich                                                              | AZURE_LOADBALANCER                  |
| 10221-10231 | Ein-/ausgehende | TCP                | Implementierungsdetails für Redis (können remote Endpunkt auf VIRTUAL_NETWORK beschränken) | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 13000-13999 | Eingehende          | TCP                | Client-Kommunikation auf Redis Cluster Azure Lastenausgleich                      | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 15000-15999 | Eingehende          | TCP                | Client-Kommunikation auf Redis Cluster Azure Lastenausgleich                      | VIRTUAL_NETWORK, AZURE_LOADBALANCER |
| 16001       | Eingehende          | TCP/UDP            | Azure Lastenausgleich                                                              | AZURE_LOADBALANCER                  |
| 20226       | Eingehende + ausgehende | TCP                | Implementierungsdetails für Redis Cluster                                          | VIRTUAL_NETWORK                     |


Es gibt Network Connectivity Anforderungen für Azure Redis Cache, die ursprünglich nicht in einem virtuellen Netzwerk erfüllt sein kann. Azure Redis Cache erfordert alle der folgenden Elemente, um ordnungsgemäß funktionieren, wenn Sie innerhalb eines virtuellen Netzwerks verwendet.

-  Ausgehende Netzwerkkonnektivität zum Azure-Speicher Endpunkte weltweit. Dies umfasst die Endpunkte befindet sich in der gleichen Region wie die Instanz Azure Redis Cache sowie Speicher Endpunkte in der **anderen** Azure Regionen ansässig. Azure Speicher Endpunkte zu beheben, klicken Sie unter den folgenden DNS-Domänen: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net*und *file.core.windows.net*. 
-  Ausgehende Netzwerkkonnektivität *ocsp.msocsp.com*, *mscrl.microsoft.com* und *crl.microsoft.com*. Diese Konnektivität ist erforderlich, um SSL-Funktionen zu unterstützen.
-  Die DNS-Konfiguration für das virtuelle Netzwerk muss aufgelöst aller Endpunkte und Domänen in der früheren Punkte angegeben ist. Diese DNS-Anforderungen können erfüllt sein, indem sichergestellt wird eine gültige DNS-Infrastruktur konfiguriert ist, und für das virtuelle Netzwerk verwaltet werden.



### <a name="can-i-use-vnets-with-a-standard-or-basic-cache"></a>Kann ich mit einem Cache standard oder grundlegende VNets verwenden?

VNets können nur mit Premium Caches verwendet werden.

### <a name="why-does-creating-a-redis-cache-fail-in-some-subnets-but-not-others"></a>Warum schlägt erstellen einen Redis Cache in einigen Subnetze jedoch nicht andere fehl?

Wenn Sie einen Azure Redis Cache zu einer Cloud VNet bereitstellen, muss der Cache in einem dedizierten Subnetz, die keine anderen Ressourcentyp enthält. Wenn versucht wird, eine Azure Redis Cache für eine Cloud VNet Subnetz bereitstellen, enthält weitere Ressourcen, die Bereitstellung fehlschlägt. Löschen Sie die vorhandenen Ressourcen innerhalb der Subnetz aus, bevor Sie einen neuen Cache für Redis erstellen können.

Sie können mehrere Arten von Ressourcen zu einem klassischen VNet bereitstellen, solange Sie genügend IP-Adressen verfügbar haben.

### <a name="do-all-cache-features-work-when-hosting-a-cache-in-a-vnet"></a>Funktionieren alle Cachefeatures beim Hosten von eines Caches in einem VNET?

Wenn Ihr Cache Teil einer VNET ist, können nur Clients in der VNET den Cache zugreifen. Die folgenden Cache Management Features funktionieren nicht daher zu diesem Zeitpunkt.

-   Redis Console - da Redis Console den Redis cli.exe Client auf virtuellen Computern, die nicht Bestandteil Ihrer VNET sind gehostet verwendet, es kann keine Verbindung herstellen in Ihren Cache.


## <a name="use-expressroute-with-azure-redis-cache"></a>Verwenden von ExpressRoute mit Azure Redis Cache

Kunden können eine Verbindung [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) ihre Infrastruktur virtuelles Netzwerk Verbindung somit deren lokalen Netzwerk in Azure verlängern. 

Standardmäßig Ankündigung eine neu erstellte ExpressRoute Verbindung eines Standard-Routing, die ausgehende Verbindung mit dem Internet ermöglicht. Mit dieser Konfiguration sind Clientanwendungen mit anderen Azure Endpunkten einschließlich Azure Redis Cache eine Verbindung herstellen können.

Werden jedoch eine gemeinsame Kundenkonfiguration, erzwingt ausgehenden Internet-Datenverkehr stattdessen lokalen, eigene Standard-Routing (0.0.0.0/0) definiert wird. Dieser Datenfluss verletzt ausnahmslos Verbindungen mit Azure Redis Cache, da ausgehende Datenverkehr entweder gesperrten lokalen ist oder NAT auf ein nicht erkanntes Satz von Adressen, die nicht mehr mit verschiedenen Azure Endpunkten arbeiten möchten.

Die Lösung besteht darin, ein (oder mehrere) User defined leitet (UDRs) im Subnetz zu definieren, die den Azure Redis Cache enthält. Eine UDR definiert Subnetz-spezifische weitergeleitet, die statt der Standard-Routing berücksichtigt werden.

Falls möglich, empfiehlt es sich die folgende Konfiguration verwendet:

- Die Konfiguration ExpressRoute Ankündigung 0.0.0.0/0 und standardmäßig erzwingen, dass alle ausgehenden Datenverkehr lokalen Tunnel.
- Die UDR angewendet, die an das Subnetz mit Azure Redis Cache definiert 0.0.0.0/0 mit dem nächsten Abschnitt Typ Internet (ein Beispiel dafür ist weiter unten in diesem Artikel).

Die kombinierte Schritten ergibt sich, dass die Subnetzebene UDR Vorrang vor der ExpressRoute Erzwungene Tunnel, hat ausgehenden Zugriff auf das Internet aus dem Azure Redis Cache wodurch sichergestellt wird.

Zwar die Verbindung zu einer Instanz Azure Redis Cache aus einer lokalen Anwendung mithilfe der ExpressRoute ist kein typische Verwendungsszenario Leistung aus Gründen (zur Optimierung der Systemleistung Azure Redis Cache Clients in derselben Region als Azure Redis Cache werden soll) in diesem Szenario, die der Netzwerkpfad ausgehenden internen corporate Proxys passieren kann nicht, noch können sofort zu lokalen geschickt werden. Auf diese Weise wird die effektive NAT-Adresse des ausgehenden Netzwerkverkehr aus dem Azure Redis Cache. Ändern der Adresse einer Azure Redis Cache NAT versucht Instanz des ausgehenden Netzwerkverkehr Konnektivitätsfehlern auf viele der oben aufgeführten Endpunkte. Dadurch Versuche Azure Redis Cache erstellen.

**Wichtig:**  Die Arbeitspläne in einer UDR **muss** definiert werden spezifischen Vorrang vor alle leitet durch die Konfiguration ExpressRoute angekündigt. Im folgende Beispiel verwendet den Bereich der allgemeinen 0.0.0.0/0-Adresse und als solche kann potenziell versehentlich überschrieben werden durch Routing Werbung spezifischere Adressbereiche verwenden.

**Sehr wichtig:**  Azure Redis Cache ist nicht mit ExpressRoute Konfigurationen unterstützt die **falsch Cross - leitet aus dem öffentlichen Peeringliste Pfad an den privaten Peeringliste Pfad Ankündigung**. ExpressRoute Konfigurationen öffentlichen peering konfiguriert, denen erhalten Routing Werbung von Microsoft für eine große Gruppe von Microsoft Azure IP-Adressbereiche. Wenn diese Adressbereiche fälschlicherweise als übergreifend auf den privaten Peeringliste Pfad angekündigt sind, ist das Ergebnis, dass alle ausgehenden Pakete aus der Redis Azure-Cache-Instanz Subnetz falsch als Erzwingen eines Kunden die lokale Infrastruktur geschickt werden. Diese Netzwerkfluss verletzt Azure Redis Cache. Die Lösung des Problems ist das Cross-Werbung leitet aus dem öffentlichen Peeringliste Pfad an den privaten Peeringliste Pfad zu beenden.

Hintergrundinformationen zum User defined leitet steht in dieser [Übersicht](../virtual-network/virtual-networks-udr-overview.md). 

Weitere Informationen zu ExpressRoute finden Sie unter [ExpressRoute – technische Übersicht](../expressroute/expressroute-introduction.md)

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie, wie mehr Premium Cachefeatures verwenden.

-   [Einführung in die Ebene Azure Redis Cache Premium](cache-premium-tier-intro.md)





  
<!-- IMAGES -->

[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png

[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png

[redis-cache-vnet-info]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-info.png

