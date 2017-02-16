<properties
   pageTitle="Erstellen von Azure Service Fabric Cluster unter Windows Server und Linux | Microsoft Azure"
   description="Dienst Fabric Cluster unter Windows Server und Linux, was bedeutet, dass Sie bereitstellen können werden und Host Service Fabric Applikationen an einer beliebigen Stelle ausführen, können Sie Windows Server oder Linux ausführen."
   services="service-fabric"
   documentationCenter=".net"
   authors="Chackdan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="chackdan"/>

# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Erstellen von Dienst Fabric Cluster unter Windows Server oder Linux

Azure Service-Struktur ermöglicht die Erstellung der Dienst Fabric Cluster auf jedem virtuellen Computern oder Computern unter Windows Server oder Linux. Dies bedeutet, dass Sie sind in der Lage, bereitstellen und Dienst Fabric Applikationen in jeder Umgebung ausführen, müssen Sie eine Reihe von Windows Server oder Linux auf Computern, die miteinander verbunden sind, werden sie lokale, Microsoft Azure oder mit einem Cloudanbieter.

##<a name="create-service-fabric-clusters-on-azure"></a>Erstellen Sie auf Azure Service Fabric Cluster

Erstellen eines Clusters Azure ist entweder über eine Ressource Model-Vorlage oder Azure-Portal abgeschlossen. Weitere Informationen finden Sie [mithilfe einer Vorlage Ressourcenmanager Cluster Service Fabric erstellen](service-fabric-cluster-creation-via-arm.md) oder [Erstellen Sie einen Dienst Fabric Cluster vom Azure-Portal](service-fabric-cluster-creation-via-portal.md) .

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Unterstützte Betriebssysteme für Azure-Cluster

Sie können auf virtuellen Computern mit diesen Betriebssystemen Cluster zu erstellen:

* Windows Server 2012 R2
* Windows Server 2016 (nach der als allgemein verfügbar angekündigt wird)
* Linux Ubuntu 16.04 (in public Preview-Version) 


##<a name="create-service-fabric-standalone-clusters-on-premise-or-with-any-cloud-provider"></a>Erstellen Sie Dienst Fabric eigenständigen Cluster, lokal oder mit einem Cloudanbieter

Dienst Fabric bietet ein Paket installieren Sie eigenständigen Dienst Fabric Cluster lokalen erstellen oder auf eine beliebige Cloud-Anbieter

Weitere Informationen zum Einrichten von eigenständigen Dienst Fabric Cluster unter Windows Server finden Sie unter [Service Fabric Cluster Creation für Windows Server](service-fabric-cluster-creation-for-windows-server.md)

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>Alle Cloud-Bereitstellungen im Vergleich zu lokale Bereitstellungen
Der Vorgang zum Erstellen eines Dienst Fabric Clusters lokalen ähnelt der Vorgang des Erstellens eines Clusters auf eine beliebige Cloud mit einer Reihe von virtuellen Computern Ihrer Wahl. Die ersten Schritte zur Bereitstellung der virtuellen Computern unterliegt der Cloudanbieter oder lokalen Umgebung, die Sie verwenden. Nachdem Sie eine Reihe von virtuellen Computern mit Netzwerkkonnektivität dazwischen aktiviert haben, klicken Sie dann die Schritte zum Einrichten des Dienst Fabric-Pakets, bearbeiten Sie die Clustereinstellungen, und führen Sie die Clustererstellung und Management Skripts sind identisch. Dadurch wird sichergestellt, dass Ihr Wissen und Erfahrung zu betreiben und Verwalten von Service Fabric Cluster übertragbar ist, wenn Sie neue Hostinganbieter Umgebungen als Ziel auswählen.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Vorteile der eigenständigen Dienst Fabric Cluster erstellen
* Sie können frei wählen alle Cloud-Anbieter, um Ihren Cluster zu hosten.
* Dienst Fabric Applications, einmal geschrieben wurde, können in mehreren Hostinganbieter Umgebungen mit minimalen keine Änderungen ausgeführt werden.
* Kenntnisse über das Erstellen von Fabric Service führt über aus einem hosting-Umgebung in eine andere.
* Ausführen und Verwalten von Service Fabric Betrieb Erfahrung Cluster ausführt über von einer Umgebung in eine andere.
* Umfassende Kunden kommen wird durch das Hosten Umgebung Einschränkungen ungebundener.
* Ein zusätzliches Maß an Zuverlässigkeit und Schutz vor weit verbreitet Ausfall vorhanden ist, da Sie die Dienste in eine andere Bereitstellung Umgebung verschieben können über Wenn Datenanbieter Center oder Cloud einer gesperrten verfügt.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Unterstützte Betriebssysteme für eigenständigen Cluster
Sie können Cluster auf virtuellen Computern oder Computern, die mit diesen Betriebssystemen zu erstellen:

* Windows Server 2012 R2
* Windows Server 2016 (nach der als allgemein verfügbar angekündigt wird)
* Linux (in Kürze verfügbar)

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Vorteile von über eigenständigen, die Cluster für den Dienst Fabric auf Azure Service Fabric Cluster erstellt lokale

Dienst Fabric Cluster auf Azure erhalten Sie, dass die Vorteile gegenüber der lokalen Wahl können, wenn Sie keine bestimmte Anforderungen für Cluster Ausführungsort, haben, und es wird empfohlen, dass Sie auf Azure ausgeführt werden. Klicken Sie auf Azure stellen wir Integration mit anderen Azure Funktionen und Diensten, wodurch Vorgänge und Verwaltung von Cluster vereinfachen und die Zuverlässigkeit wird.

* **Azure-Portal:** Azure-Portal erleichtert die Cluster erstellen und verwalten.

* **Azure Ressourcenmanager:** Verwenden von Azure Ressourcenmanager einfache Verwaltung aller Ressourcen, die vom Cluster verwendet werden, als Einheit und vereinfacht Kosten verfolgen und Abrechnung.
* **Dienst Fabric Cluster als Azure Ressource** Ein Dienst Fabric Cluster ist eine Ressource Cloud, damit Sie ihn wie andere Ressourcen Cloud Azure modellieren können.
* **Integration in Azure Infrastruktur** Dienst Fabric koordiniert mit der zugrunde liegenden Azure Infrastruktur für OS, Netzwerk- und andere Upgrades zur Verbesserung der Verfügbarkeit und Zuverlässigkeit Ihrer Programme.  
* **Diagnose:** Auf Azure stellen wir Integration mit Azure Diagnose und Log Analytics aus.
* **Automatische Skalierung:** Für Cluster auf Azure stellen wir integrierte Funktionen, die aufgrund von virtuellen Computern skalieren-setzt die automatische Skalierung aus. Lokale und anderen Cloud-Umgebungen müssen Sie eigene automatische Skalierung Feature oder Skala mit manuell die APIs, die Fabric Service macht für die Skalierung Cluster erstellen.

## <a name="next-steps"></a>Nächste Schritte
Erstellen Sie einen Cluster auf virtuellen Computern oder Computern unter Windows Server: [Dienst Fabric Cluster Creation für Windows Server](service-fabric-cluster-creation-for-windows-server.md)

Erstellen Sie einen Cluster auf virtuellen Computern oder Computern unter Linux: [Dienst Architektur auf Linux](service-fabric-linux-overview.md)
