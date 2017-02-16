<properties
   pageTitle="Konfigurieren Sie den eigenständigen Cluster | Microsoft Azure"
   description="Dieser Artikel beschreibt, wie Sie Ihre eigenständigen oder privaten Dienst Fabric Cluster konfigurieren."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="dkshir"/>


# <a name="configuration-settings-for-standalone-windows-cluster"></a>Konfiguration von Einstellungen für eigenständigen Windows-cluster

Dieser Artikel beschreibt, wie einen eigenständigen Dienst Fabric Cluster mithilfe der Datei _**ClusterConfig.JSON**_ konfigurieren. Dieser Datei können Sie Informationen, wie z. B. Dienst Fabric Knoten und ihre IP-Adressen, die verschiedenen Arten von Knoten auf den Cluster, die Sicherheitskonfigurationen sowie der Suchtopologie Netzwerk im Hinblick auf Domänen Fehlerstrukturanalyse/Upgrade für Ihren Cluster eigenständigen angeben.

Wenn Sie [eigenständigen Dienst Fabric Paket herunterladen](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), werden einige Beispiele für die Datei ClusterConfig.JSON, werden heruntergeladen auf Ihren Computer arbeiten. Die Beispiele *DevCluster* in deren Namen Probleme hilft Ihnen einen Cluster mit allen drei Knoten auf dem gleichen Computer, wie logische Knoten zu erstellen. Aus den folgenden muss mindestens einen Knoten als primären Knoten markiert werden. Diese Cluster eignet sich für eine Entwicklung oder Test-Umgebung und wird als Herstellung Cluster nicht unterstützt. Die Beispiele Probleme *MultiMachine* in ihren Namen, helfen Ihnen, einen Herstellung Qualität Cluster, wobei jeder Knoten auf einem separaten Computer zu erstellen. Die Anzahl der primären Knoten für diese Cluster wird auf die [Zuverlässigkeit Ebene](#reliability)basieren.

1. *ClusterConfig.Unsecure.DevCluster.JSON* und *ClusterConfig.Unsecure.MultiMachine.JSON* zeigen, wie einen ungeschützte Test- oder Cluster Hilfethemas zu erstellen. 
    
2. *ClusterConfig.Windows.DevCluster.JSON* und *ClusterConfig.Windows.MultiMachine.JSON* zeigen, wie Test- oder Cluster, gesichert mithilfe der [Windows-Sicherheit](service-fabric-windows-cluster-windows-security.md)zu erstellen.

3. *ClusterConfig.X509.DevCluster.JSON* und *ClusterConfig.X509.MultiMachine.JSON* zeigen, wie Sie Test- oder Cluster, die mit sicherem erstellen [X509 Zertifikaten basierende Sicherheit](service-fabric-windows-cluster-x509-security.md). 


Untersuchen wir nun die verschiedenen Abschnitten der Datei _**ClusterConfig.JSON**_ als unter.

## <a name="general-cluster-configurations"></a>Allgemeine Cluster-Konfigurationen
Hierzu gehören die wesentlichen Cluster spezifischen Konfigurationen, wie in den JSON-Codeausschnitt unten dargestellt.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2015-01-01-alpha",

Sie können eine beliebige Anzeigename zum Dienst Fabric Cluster verleihen, indem Sie die Variable **Namen** zuweisen. Die **ClusterConfigurationVersion** ist die Versionsnummer der Ihren Cluster; Sie sollten sie jedes Mal, wenn Sie Ihrem Dienst Fabric Cluster aktualisieren erhöhen. Sie sollten die **ApiVersion** jedoch auf den Standardwert belassen.


<a id="clusternodes"></a>
## <a name="nodes-on-the-cluster"></a>Auf dem Cluster Knoten
Sie können die Knoten auf Ihrem Dienst Fabric Cluster konfigurieren, mithilfe des Abschnitts mit den **Knoten** , wie im folgenden Codeausschnitt dargestellt.

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

Ein Dienst Fabric Cluster muss mindestens 3 Knoten enthalten. Sie können in diesem Abschnitt gemäß Ihr Setup weitere Knoten hinzufügen. Die folgende Tabelle erläutert die Konfiguration Einstellungen für die einzelnen Knoten.

|**Knotenkonfiguration**|**Beschreibung**|
|-----------------------|--------------------------|
|nodeName|Sie können alle Anzeigenamen ein, der auf den Knoten verleihen.|
|IP-Adresse|Die IP-Adresse Ihres Knotens herausfinden, indem Sie ein Eingabeaufforderungsfenster öffnen und mit der Eingabe `ipconfig`. Beachten Sie die IPv4-Adresse ein, und weisen sie der Variablen **IP-Adresse** .|
|nodeTypeRef|Jeder Knoten kann einen anderen Knotentyp zugewiesen werden. Die [Knotentypen](#nodetypes) werden unten im Abschnitt definiert.|
|faultDomain|Fehlerstrukturanalyse-Domänen aktivieren Clusteradministratoren physischen Knoten definieren, die gleichzeitig aufgrund von freigegebenen physischen Abhängigkeiten fehlschlagen können.|
|upgradeDomain|Upgrade Domänen beschreiben Sätze von Knoten, die für den Dienst Fabric Upgrades zu etwa zur gleichen Zeit ausgeschaltet werden. Sie können welche Knoten zuweisen, welche Domänen Upgrade auswählen, wie er nicht durch eine physischen Anforderungen beschränkt werden.| 


## <a name="cluster-properties"></a>Clustereigenschaften

Im Abschnitt **Eigenschaften** in der ClusterConfig.JSON wird verwendet, um die Cluster wie folgt zu konfigurieren.

<a id="reliability"></a>
### <a name="reliability"></a>Zuverlässigkeit 
Im Abschnitt **ReliabilityLevel** definiert die Anzahl der Kopien der Dienste aufgeführt, die auf dem primären Knoten im Cluster ausgeführt werden können. Dies erhöht die Zuverlässigkeit dieser Dienste und somit Cluster. Sie können diese Variable entweder *Bronze*, *Silber*, *Gold* oder *Platinum* für 3, 5, 7 oder 9 Kopien dieser Dienste Hilfethemas festlegen. Nachfolgend ein Beispiel dazu finden Sie unter.

    "reliabilityLevel": "Bronze",
    
Beachten Sie, dass da ein primärer Knoten ein einzelnes Dokument von der Systemdienste ausgeführt wird, möchten Sie mindestens 3 primären Knoten für *Bronze*, 5 für *Silber*, 7 für *Gold* und 9 für *Platinum* Zuverlässigkeit Ebenen benötigen.


### <a name="diagnostics"></a>Diagnose
Im Abschnitt **DiagnosticsStore** können Sie Konfigurieren von Parametern zum Aktivieren der Diagnose und Problembehandlung bei Fehlern Knoten oder Cluster, wie im folgenden Codeausschnitt dargestellt. 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

Die **Metadaten** ist eine Beschreibung der Ihrer Cluster-Diagnose und gemäß Ihr Setup festgelegt werden kann. Diese Variablen Hilfen in ETW Überwachungsprotokolle, Absturzabbilder als auch Leistungsindikatoren sammeln. Lesen Sie [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) und [Spur ETW](https://msdn.microsoft.com/library/ms751538.aspx) für Weitere Informationen zu ETW Überwachungsprotokolle an. Alle einschließlich [Absturzabbilder](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) und [Leistungsindikatoren](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) Protokolle können zum **ConnectionString** Ordner auf Ihrem Computer geleitet werden. Sie können auch *AzureStorage* verwenden, zum Speichern von Diagnose. Nachfolgend finden Sie eine Stichprobe Codeausschnitt.

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Sicherheit 
Abschnitt **Sicherheit** durch ist für eine sichere eigenständigen Dienst Fabric Cluster erforderlich. Der folgende Ausschnitt zeigt einen Teil in diesem Abschnitt.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

Die **Metadaten** ist eine Beschreibung der sicheren Cluster und gemäß Ihr Setup festgelegt werden kann. Die **ClusterCredentialType** und **ServerCredentialType** bestimmen den Typ des Wertpapiers zurück, das den Cluster und den Knoten implementiert werden sollen. Sie können entweder *X509* für ein Wertpapier Zertifikat-basierten oder *Windows* für eine Azure-Active Directory-basierte Sicherheit festgelegt werden. Die restlichen Abschnitt **Sicherheit** durch basiert auf den Typ der Sicherheit. Informationen zum Ausfüllen der restlichen Abschnitt **Sicherheit** durch finden Sie [Zertifikate basierende Sicherheit in einem eigenständigen Cluster](service-fabric-windows-cluster-x509-security.md) oder [Windows-Sicherheit in einem eigenständigen Cluster](service-fabric-windows-cluster-windows-security.md) .


<a id="nodetypes"></a>
### <a name="node-types"></a>Knotentypen
Im Abschnitt **NodeTypes** beschreibt den Typ der Knoten, die Ihren Cluster hat. Für einen Cluster, muss mindestens ein Knotentyp angegeben werden, wie im folgenden Codeausschnitt dargestellt. 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

Der **Name** ist der angezeigte Name für diese Art von bestimmter Knoten. Erstellen Sie einen Knoten dieses Typs Knoten, weisen Sie der Anzeigename der Variablen **NodeTypeRef** für diesen Knoten, als erwähnten [Weiter oben](#clusternodes). Legen Sie für jeden Knoten die Verbindungsendpunkte, die verwendet werden soll. Sie können eine beliebige Anzahl Port für diese Verbindungsendpunkte auswählen, solange sie nicht mit einem beliebigen anderen Endpunkten in diesem Cluster in Konflikt stehen. In einem Cluster mit mehreren Knoten werden ein oder mehrere primäre Knoten (d. h. **IsPrimary** festgelegten *Wahr*), je nach der [**ReliabilityLevel**](#reliability). Lesen Sie [Dienst Fabric Cluster Kapazität Planungsaspekte beim](service-fabric-cluster-capacity.md) für Informationen **NodeTypes** und **ReliabilityLevel** Werte und wissen, was primären sind und die Typen nicht als Primärschlüssel Knoten. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Endpunkte verwendet, um die Knotentypen konfigurieren

- *ClientConnectionEndpointPort* wird vom Client für die Verbindung zum Cluster, bei Verwendung des Clients APIs verwendet. 
- *ClusterConnectionEndpointPort* ist den Port, an dem die Knoten miteinander kommunizieren.
- *LeaseDriverEndpointPort* wird vom Cluster verleasen Treiber verwendet, um herauszufinden, ob die Knoten noch aktiv sind. 
- *ServiceConnectionEndpointPort* wird die Anwendungen und Dienste auf einem Knoten bereitgestellt zur Kommunikation mit dem Dienst Fabric-Client auf einem bestimmten Knoten verwendet.
- *HttpGatewayEndpointPort* wird vom Dienst Fabric Explorer Verbindung mit dem Cluster verwendet.
- *EphemeralPorts* sind die [dynamischen Ports, die vom Betriebssystem verwendet](https://support.microsoft.com/kb/929851). Dienst Fabric wird einen Teil davon als Anwendungsports verwendet werden, und der verbleibenden wird für das Betriebssystem verfügbar. Es wird auch Buchstabenfolge an den vorhandenen Bereich des Betriebssystems, ordnen Sie, sodass für alle Verwendungszwecke Sie Bereiche, die in den JSON-Beispieldateien angegebenen verwenden können. Sie müssen, um sicherzustellen, dass der Unterschied zwischen der Start- und die Endzeit Ports mindestens 255 ist. 
- *ApplicationPorts* sind die Ports, die von den Dienst Fabric-Clientanwendungen verwendet werden. Diese sollten einen Teil der *EphemeralPorts*, genug zu bedecken der Anforderung zum Endpunkt für Anwendungen. Dienst Fabric wird diese bei jedem neuen Ports erforderlich sind, sowie übernehmen, öffnen die Firewall für diese Ports verwenden. 
- *ReverseProxyEndpointPort* ist ein optionaler reverse Proxyendpunkt. Weitere Informationen hierzu finden Sie unter [Service Fabric Reverse Proxy](service-fabric-reverseproxy.md) . 


### <a name="other-settings"></a>Andere Einstellungen
Im Abschnitt **FabricSettings** können Sie die Quadratwurzel Verzeichnisse für den Dienst Fabric Daten und Protokolle festlegen. Sie können diese nur während der anfänglichen Clustererstellung anpassen. Nachfolgend finden Sie ein Beispiel für Ausschnitt der in diesem Abschnitt.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

Es wird empfohlen, mit einem BS-Laufwerk als FabricDataRoot und FabricLogRoot, wie SFA Weitere Zuverlässigkeit gegen OS stürzt ab. Beachten Sie, dass, wenn Sie nur die Daten aus anpassen möchten, klicken Sie dann im Stammverzeichnis Log eine Ebene unterhalb des Stammverzeichnisses Daten platziert wird.


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie eine vollständige ClusterConfig.JSON Datei gemäß der eigenständigen Cluster Setup konfiguriert haben, können Sie Ihren Cluster bereitstellen, indem Sie die folgenden Artikel [erstellen einen Azure Service Fabric Cluster lokal oder in der Cloud](service-fabric-cluster-creation-for-windows-server.md) und passen Sie dann [Ihre Cluster mit Service Fabric Explorer Visualisierung](service-fabric-visualizing-your-cluster.md).


