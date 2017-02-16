<properties
   pageTitle="Ausführen von Windows virtuellen Computern in mehreren Regionen hohen Verfügbarkeit | Bezug auf Architektur | Microsoft Azure"
   description="Informationen zum Bereitstellen von virtuellen Computern in mehreren Regionen auf Azure für hohen Verfügbarkeit und Stabilität."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/21/2016"
   ms.author="mwasson"/>

# <a name="running-windows-vms-in-multiple-regions-for-high-availability"></a>Ausführen von Windows virtuellen Computern in mehreren Regionen hohen Verfügbarkeit

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Ausführen von Linux virtuellen Computern in mehreren Regionen hohen Verfügbarkeit](guidance-compute-multiple-datacenters-linux.md)
- [Ausführen von Windows virtuellen Computern in mehreren Regionen hohen Verfügbarkeit](guidance-compute-multiple-datacenters.md)

In diesem Artikel empfehlen wir eine Reihe von Methoden zum Ausführen von Windows-virtuellen Computern (virtuellen Computern) in mehrere Bereiche Azure Verfügbarkeit und eine robuste Wiederherstellungsinfrastruktur zu erzielen.

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle: [Ressourcenmanager] [ resource groups] und klassischen. In diesem Artikel wird Ressourcenmanager, in dem Bereitstellung neuer Microsoft empfiehlt verwendet.

Eine Architektur mit mehreren Region kann höheren Verfügbarkeit als bereitstellen für eine einzelne Region bereitstellen. Wenn ein regionalen Ausfall wirkt sich auf die primäre Region, verwenden Sie [Den Datenverkehr Manager] [ traffic-manager] über die Region ein anderes sekundäre fehlschlägt. Diese Architektur kann auch hilfreich, wenn eine einzelne Subsystem der Anwendung schlägt fehl. 

Es gibt mehrere allgemeine Verfahren zum Erreichen einer hohen Verfügbarkeit über Data Center:

- Aktiv/Passiv mit tollen Standby. Datenverkehr geht an eine Region, während die anderen wartet auf Standby. Virtuellen Computern in der Region sekundäre sind zugewiesenen und laufenden jederzeit an.

- Aktiv/Passiv mit kalt Standby. Die gleiche, aber virtuellen Computern in der Region sekundäre werden nicht zugeordnet, bis für Failover erforderlich. Dieser Ansatz Kosten kleiner ausgeführt werden, allerdings werden in der Regel mehr Zeiten bei einem Fehler.

- Aktiv/aktiv. Beide Regionen aktiv, und Serviceanfragen sind Lastenausgleich dazwischen. Wenn eine Datacenter nicht mehr verfügbar ist, wird diese außerhalb des Drehung übernommen. 

Diese Architektur befasst sich aktiv/passiv wichtiges Standby-Modus, mit dem Datenverkehr-Manager für Failover. Beachten Sie, dass Sie eine kleine Anzahl von virtuellen Computern für wichtiges Standby bereitstellen und dann Skalieren je nach Bedarf.

## <a name="architecture-diagram"></a>Architekturdiagramm

Das folgende Diagramm basiert auf der Architektur in [Zuverlässigkeit hinzufügen, um eine mehrstufige Architektur auf Azure](guidance-compute-n-tier-vm.md)angezeigt.

> Ein Visio-Dokument, das diese Architekturdiagramm enthält wird unter der [Microsoft download Center]herunterladen[visio-download]. Bei diesem Diagramm handelt, klicken Sie auf "berechnen - Multi Region (Windows) Seite.

[! [0]][0]

- **Primären und sekundären Regionen**. Diese Architektur verwendet zwei Bereiche, um höhere Verfügbarkeit zu erzielen. Eine ist der primäre Region. Bei normalen Vorgängen wird Netzwerkverkehr an die primäre Region weitergeleitet. Aber wenn, die nicht mehr verfügbar ist, den Datenverkehr an die sekundäre Region weitergeleitet wird.

- ** [Azure Datenverkehr Manager] [ traffic-manager] ** eingehende Anfragen an die primäre Region weitergeleitet. Wenn die Region nicht mehr verfügbar ist, wechselt Datenverkehr-Manager auf der Sekundärachse Region. Weitere Informationen finden Sie im Abschnitt [Datenverkehr Manager konfigurieren](#configuring-traffic-manager).

- **Gruppen für Ressourcen**. Erstellen Sie separate [Ressourcengruppen] [ resource groups] die primäre Region, der sekundäre Region, und für den Datenverkehr-Manager. Dadurch haben Sie die Möglichkeit zum Verwalten von jeder Region als eine einzelne Sammlung von Ressourcen. Sie könnten beispielsweise eine Region Aufzeichnen unten den anderen aus erneut bereitstellen. [Verknüpfen Sie die Ressourcengruppen][resource-group-links], sodass Sie eine Abfrage, um eine Liste aller Ressourcen für die Anwendung ausführen können.

- **VNets**. Erstellen einer separaten VNet für die einzelnen Regionen an. Stellen Sie sicher, dass die Adresse Leerzeichen nicht überschneiden. 

- **SQLServer immer auf Verfügbarkeit Gruppe**. Wenn Sie SQL Server verwenden, sollten Sie [SQL immer auf Availabilty Gruppen] [ sql-always-on] für eine hohe Verfügbarkeit. Erstellen einer einzelnen Verfügbarkeit Gruppe, die SQL Server-Instanzen in beiden Regionen enthält. Weitere Informationen finden Sie im Abschnitt [Konfigurieren der Verfügbarkeit von SQL Server immer auf Gruppe](#configuring-the-sql-server-alwayson-availability-group ).

> [AZURE.NOTE] Erwägen Sie auch [Azure SQL-Datenbank][azure-sql-db], die eine relationale Datenbank als Cloud-Dienst bietet. Mit SQL-Datenbank brauchen Sie Konfigurieren einer Gruppe Verfügbarkeit oder Failover verwalten.  

- **VPN-Gateways**: Erstellen eines [VPN-Gateway] [ vpn-gateway] in jeder VNet, und konfigurieren Sie eine [Verbindung VNet-VNet -][vnet-to-vnet], um zwischen den beiden VNets Netzwerkverkehr zu ermöglichen. Dies ist für die Gruppe SQL immer auf Verfügbarkeit erforderlich.

## <a name="recommendations"></a>Empfehlungen

Azure bietet viele Ressourcentypen, sodass diese Architektur Bezug genommen kann und anderen Ressourcen viele verschiedene Arten bereitgestellt. Wir haben eine Ressourcenmanager Azure-Vorlage, um die Verweis-Architektur zu installieren, die diese Empfehlungen folgt bereitgestellt. Wenn Sie auch erstellen eigene Bezug Architektur führen Sie diese Empfehlungen, wenn Sie eine bestimmte Anforderung verfügen, die nicht passen empfohlen.

### <a name="regional-pairing"></a>Landes-/ Verbindung

Jede Azure Region ist mit einer anderen Region innerhalb der gleichen Geography kombiniert. Wählen Sie in der Regel Regionen aus dem gleichen Landes-/ Paar (z. B. ostasiatischen US 2 und uns zentralen) aus. Auf diese Weise bietet folgende Vorteile:

- Ist eine umfassende einem Dienstausfall, Wiederherstellung von mindestens eine Region aus jeder Paar erhält.

- Geplanten Azure System-Updates werden, die für Regionen sequenziell Variationswebsites um mögliche Ausfallzeiten zu minimieren.

- Paare befinden sich in der gleichen "geography", die Daten vor-Ort-erfüllen.

Jedoch sicherzustellen, dass beide Regionen aller Azure Dienste für eine Anwendung erforderlich unterstützen (finden Sie unter [Services nach Region][services-by-region]). Weitere Informationen zu regionalen paarweise angegeben werden, finden Sie unter [Business Continuity- und Disaster Wiederherstellung (BCDR): Azure gepaart Regionen][regional-pairs].

### <a name="traffic-manager-configuration"></a>Konfiguration von Datenverkehr-Manager

Berücksichtigen Sie die folgenden Punkte, beim Konfigurieren Datenverkehr Manager für Ihr Szenario:

- **Routing.** Datenverkehr Manager unterstützt mehrere [routing Algorithmen][tm-routing]. Verwenden Sie für die in diesem Artikel beschriebenen Szenario _Priorität_ routing (vormals als _Failover_ routing bezeichnet). Mit dieser Einstellung sendet Datenverkehr Manager alle Anfragen an die primäre Region an, es sei denn, die primäre Region nicht mehr erreichbar ist. An diesem Punkt wechselt es automatisch auf der Sekundärachse Region. Finden Sie unter [Konfigurieren von Failover routing Methode][tm-configure-failover].

- **Gesundheit Prüfpunkt.** Datenverkehr-Manager verwendet eine HTTP (oder HTTPS) [Prüfpunkt] [ tm-monitoring] zum Überwachen der Verfügbarkeit der einzelnen Regionen angezeigt. Der Prüfpunkt überprüft für eine HTTP 200 Antwort für einen angegebenen URL-Pfad. Als bewährte Methode erstellen Sie einen Endpunkt, der den gesamten der Anwendung Integritätsberichte, und verwenden Sie diesen Endpunkt für den Systemzustand Prüfpunkt. Der Prüfpunkt andernfalls möglicherweise einen Endpunkt "fehlerfreien" melden, wenn kritische Teile der Anwendung tatsächlich Fehler aufgetreten sind. Weitere Informationen finden Sie unter [Dienststatus Endpunkt Überwachung Muster][health-endpoint-monitoring-pattern].   

Über den Datenverkehr Manager schlägt fehl, gibt es ein Zeitraum Wenn Clients die Anwendung, die mehrere Minuten sein kann erreichen kann. Zwei Faktoren wirken sich auf die gesamte Dauer:

- Der Dienststatus Prüfpunkt muss erkannt werden, dass das primäre Data Center nicht erreichbar ist.

- DNS-Server müssen die zwischengespeicherten DNS-Einträge für die IP-Adresse Aktualisieren der abhängt, die DNS-Time to live (TTL). Die Standard-TTL ist 300 Sekunden (5 Minuten), aber Sie können diesen Wert konfigurieren, wenn Sie das Profil Datenverkehr Manager erstellen.

Details finden Sie unter [Informationen zu den Datenverkehr Manager Überwachung][tm-monitoring]. 

Wenn Sie über den Datenverkehr Manager schlägt fehl, empfehlen wir eine manuelle Failback durchführen, anstatt weiß nicht automatisch wieder an. Stellen Sie sicher, dass alle Anwendung Teilsystemen zuerst fehlerfrei sind. Andernfalls können Sie eine Situation erstellen, in dem die Anwendung vorwärts und rückwärts zwischen Data Center spiegelt.

Standardmäßig schlägt Datenverkehr Manager automatisch zurück. Um dies zu verhindern, müssen senken Sie die Priorität der primären Region manuell nach einer Failoverereignisses. Nehmen Sie beispielsweise an die primäre Region ist Priorität 1 und des sekundären Priorität 2. Legen Sie nach einem Failover die primäre Region auf Priorität 3, um automatische Failback verhindern, dass ein. Wenn Sie bereit sind, wechseln Sie zurück sind, aktualisieren Sie die Priorität 1.

Der folgende Befehl aus Azure CLI aktualisiert die Priorität:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --priority 3
```    

Eine weitere Möglichkeit zur Vermeidung Flipflop besteht darin, den Endpunkt vorübergehend zu deaktivieren:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --status Disabled
```

Je nach der Ursache eines Failovers benötigen Sie möglicherweise zu Redploy die Ressourcen innerhalb eines Bereichs. Führen Sie bevor wieder fehlschlägt einen Test Einsatzbereitschaft aus. Der Test sollte Aspekten wie überprüfen:

- Virtuellen Computern sind ordnungsgemäß konfiguriert. (Alle erforderlichen Software installiert ist, IIS wird ausgeführt, usw.).

- Anwendung Teilsystemen sind fehlerfrei. 

- Testen der Funktionalität. (Zum Beispiel ist die Datenbankebene erreichbar ist von der Webebene).

### <a name="sql-server-always-on-configuration"></a>SQL Server immer auf Konfiguration

SQL Server immer auf Verfügbarkeit Gruppen erfordern einen Domänencontroller. Alle Knoten in der Gruppe Verfügbarkeit muss in der gleichen AD-Domäne. Die folgenden Punkte bieten Hinweise für eine Gruppe SQL Server immer auf Verfügbarkeit zu konfigurieren:

- Setzen Sie mindestens zwei Domain Controller in jeder Region ein. 

- Geben Sie jeder Domänencontroller eine statische IP-Adresse.

- Erstellen einer VNet-VNet-Verbindungs zum Aktivieren der Kommunikation zwischen den VNets an.

- Fügen Sie für jede VNet die IP-Adressen der Domänencontroller (über beide Regionen) in der Liste der DNS-Server hinzu. Sie können den folgenden CLI-Befehl verwenden. Weitere Informationen finden Sie unter [Verwalten von verwendeten DNS-Server nach einem virtuellen Netzwerk (VNet)][vnet-dns].

```bat
azure network vnet set --resource-group dc01-rg --name dc01-vnet --dns-servers "10.0.0.4,10.0.0.6,172.16.0.4,172.16.0.6"
```

- Erstellen eines [Windows Server-Failoverclustering] [ wsfc] Cluster (WSFC), die SQL Server-Instanzen in beiden Regionen enthält. 

- Erstellen einer SQL Server immer auf Verfügbarkeit Gruppe, die SQL Server-Instanzen in der primären und sekundären Regionen enthält. Die Schritte finden Sie unter [Erweitern immer auf Verfügbarkeit Gruppe Remote Azure Datacenter (PowerShell)](https://blogs.msdn.microsoft.com/sqlcat/2014/09/22/extending-alwayson-availability-group-to-remote-azure-datacenter-powershell/) . 

- Setzen Sie in der primären Region primäre Replikat.

- Setzen Sie eine oder mehrere sekundäre Replikate in der primären Region ein. Konfigurieren Sie diese, um synchroner Commit für die automatische Failover verwendet.

- Setzen Sie eine oder mehrere sekundäre Replikate in der sekundäre Region ein. Konfigurieren Sie diese, um *asynchrone* Commit ausführen, aus Gründen der Leistung verwenden. (Andernfalls müssen alle SQL-Transaktionen über das Netzwerk auf der Sekundärachse Region in einer Schleife warten.) 

> [AZURE.NOTE] Automatisches Failover unterstützt asynchrone Commit Replikate nicht. 

Weitere Informationen finden Sie unter [Ausführen des Windows virtuellen Computern für eine mehrstufige Architektur auf Azure](guidance-compute-n-tier-vm.md#SQL-AlwaysOn-Availability-Group).

## <a name="availability-considerations"></a>Verfügbarkeit Aspekte

Bei einer komplexen mehrstufige app müssen Sie möglicherweise nicht die gesamte Anwendung in der Region sekundäre repliziert. In diesem Fall möglicherweise nur ein kritische Subsystem repliziert werden, die zur Unterstützung der Geschäftskontinuität benötigt wird.

Datenverkehr-Manager ist ein Punkt möglicherweise Fehler im System. Wenn der Dienst fehlschlägt, können keine Clients Ihrer Anwendung während der Ausfall zugreifen. Überprüfen Sie den [Datenverkehr Manager Vereinbarung zum SERVICELEVEL][tm-sla], und feststellen, ob Datenverkehr Manager alleine mit Ihren geschäftlichen Anforderungen für eine hohe Verfügbarkeit entspricht. Wenn dies nicht der Fall ist, fügen Sie ein anderes Datenverkehr Management-Lösung als ein Failback hinzu. Wenn der Dienst-Manager Azure-Datenverkehr fehlschlägt, Ändern der CNAME-Einträge in DNS auf den Datenverkehr-Verwaltungsdienst verweisen. (Dieser Schritt muss manuell ausgeführt werden, und Ihrer Anwendung nicht verfügbar ist, bis die DNS-Änderungen weitergegeben werden). 

Für den SQL Server-Cluster gibt es zwei Failover-Szenarien zu berücksichtigen ist:

1. Alle Replikate SQL in der primären Region fehl. Dies kann beispielsweise während einer regionalen Ausfall erfolgen. In diesem Fall müssen Sie manuell über die Verfügbarkeit der Gruppe SQL fehl, obwohl Datenverkehr Manager automatisch über am front-End schlägt fehl. Folgen Sie den Schritten, [Führen Sie eine erzwungene Manueller Failover einer SQL Server-Verfügbarkeit Gruppe](https://msdn.microsoft.com/library/ff877957.aspx), die beschreibt, wie Sie einen erzwungene Failover mithilfe von SQL Server Management Studio, Transact-SQL oder PowerShell in SQL Server 2016 ausführen. 

    > [AZURE.WARNING] Mit einer erzwungenen Failover besteht das Risiko von Datenverlusten. Sobald die primäre Region wieder online ist, wird eine Momentaufnahme der Datenbank und [Tablediff] verwenden, um die Unterschiede zu ermitteln.

2. Datenverkehr-Manager kann über die sekundäre Region, aber die primäre SQL-Replikats bleibt jedoch verfügbar. Beispielsweise kann die Front-End-Ebene fehlgeschlagen ohne Auswirkung auf die SQL-virtuellen Computern. In diesem Fall Internetdatenverkehr an der sekundäre Region weitergeleitet wird, und die Region weiterhin an der primären SQL verbinden kann. Es werden jedoch höhere Wartezeit, da die SQL-Verbindungen über Regionen abgelegt werden. In diesem Fall sollten Sie wie folgt ein manuelles Failover durchführen: 

    - Wechseln Sie vorübergehend eines SQL-Replikats in der sekundäre Region auf *synchroner* Commit. Dadurch wird sichergestellt, dass es können nicht Datenverlust während des Failovers.

    - Fehl über ein, an die SQL. 
    
    - Wenn Sie wieder zur primären Region nicht, stellen Sie die Einstellung asynchrone Commit wieder her. 

## <a name="manageability-considerations"></a>Verwaltbarkeit Aspekte

Wenn Sie eine Bereitstellung aktualisieren, aktualisieren Sie eine Region jeweils, um das Risiko eines globalen Fehlers aus einer falschen Konfiguration oder ein Fehler in der Anwendung zu reduzieren.

Testen der Stabilität des Systems auf Fehler an. Hier sind einige häufige Fehlerszenarien zu testen:

- Fahren Sie virtueller Computer Instanzen aus.

- Druck Ressourcen wie etwa CPU- und.

- Trennen/Verzögerung Netzwerk.

- Abstürzen Sie Prozesse an.

- Am Zertifikate ab.

- Hardware-Fehlern zu reproduzieren.

- Beenden Sie den DNS-Dienst auf die Domänencontroller aus.

Messen Sie die Wiederherstellungszeiten, und stellen Sie sicher, dass sie Ihre Bedürfnisse zuschneiden. Kombinationen von Ausfall-Modi, sowie zu testen.

## <a name="next-steps"></a>Nächste Schritte

Dieser Reihe weist auf reines Cloud-Bereitstellungen konzentrieren können. Enterprise-Szenarien erfordern häufig einem Hybriden Netzwerk, Herstellen einer Verbindung von einem lokalen Netzwerk mit einer Azure-virtuellen Netzwerk. So erstellen Sie einem solchen Hybrid Netzwerk finden Sie unter [Implementieren eines Hybrid Netzwerkarchitektur mit Azure und lokalen VPN][hybrid-vpn].

<!-- Links -->

[azure-sla]: https://azure.microsoft.com/support/legal/sla/
[azure-sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[hybrid-vpn]: guidance-hybrid-network-vpn.md
[regional-pairs]: ../best-practices-availability-paired-regions.md
[resource groups]: ../azure-resource-manager/resource-group-overview.md
[resource-group-links]: ../resource-group-link-resources.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[sql-always-on]: https://msdn.microsoft.com/en-us/library/hh510230.aspx
[TableDiff]: https://msdn.microsoft.com/en-us/library/ms162843.aspx
[tm-configure-failover]: ../traffic-manager/traffic-manager-configure-failover-routing-method.md
[tm-monitoring]: ../traffic-manager/traffic-manager-monitoring.md
[tm-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[tm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/traffic-manager/v1_0/
[traffic-manager]: https://azure.microsoft.com/en-us/services/traffic-manager/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[vnet-to-vnet]: ../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[wsfc]: https://msdn.microsoft.com/en-us/library/hh270278.aspx
[0]: ./media/blueprints/compute-multi-dc.png "Hochgradig verfügbare Netzwerkarchitektur für Applikationen Azure mehrstufige"