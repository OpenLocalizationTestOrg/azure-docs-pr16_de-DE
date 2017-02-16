<properties
   pageTitle="Hinzufügen oder Entfernen von Knoten zu einem eigenständigen Dienst Fabric Cluster | Microsoft Azure"
   description="Informationen Sie zum Hinzufügen oder Entfernen von Knoten zu einem Cluster Azure Service Fabric auf einer physischen oder virtuellen Computern unter Windows Server, die lokal sein könnten oder in einer beliebigen Cloud."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="dkshir;chackdan"/>


# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Hinzufügen oder Entfernen von Knoten zu einem eigenständigen Dienst Fabric Cluster auf Windows Server

Nachdem Sie [Ihre eigenständigen Dienst Fabric Cluster auf Computern mit Windows Server erstellt](service-fabric-cluster-creation-for-windows-server.md)haben, können Ihre geschäftliche Anforderungen ändern, sodass Sie müssen möglicherweise hinzufügen oder Entfernen von mehreren Knoten zum Cluster. Dieser Artikel enthält ausführliche Schritte aus, um dies zu erreichen.


## <a name="add-nodes-to-your-cluster"></a>Hinzufügen von Knoten zum cluster

1. Vorbereiten der virtuellen Computer/Computer Ihren Cluster hinzufügen, indem Sie die Schritte im Abschnitt [Vorbereiten der Computer ab, die erforderlichen Komponenten für die Clusterbereitstellung entsprechen](service-fabric-cluster-creation-for-windows-server.md#preparemachines) erwähnt werden soll.
2. Planen Sie, welche Fehlerstrukturanalyse und Upgrade Domäne, die Sie zum Hinzufügen von diesem virtuellen Computer/Computer zu abgelegt werden.
3. Remotedesktop (RDP) in den virtuellen Computer/Computer, die Sie zum Cluster hinzufügen möchten.
4. Kopieren oder mit diesem virtuellen Computer/Computer [eigenständigen Paket für Dienst Fabric für Windows Server herunterladen](http://go.microsoft.com/fwlink/?LinkId=730690) und Entzippen Sie ihn das Paket.
5. Führen Sie Powershell als Administrator aus, und navigieren Sie zum Speicherort des Pakets extrahiert.
6. Führen Sie *AddNode.ps1* Powershell mit den Parametern zur Beschreibung des neuen Knotens hinzufügen. Im folgenden Beispiel wird einen neuen Knoten namens VM5, mit dem Typ NodeType0, IP-Adresse 182.17.34.52 in UD1 und FD1 hinzugefügt. Die *ExistingClusterConnectionEndPoint* ist eine Verbindungsendpunkt für einen Knoten bereits in den vorhandenen Cluster. Für diesen Endpunkt können Sie die IP-Adresse von *allen* Knoten im Cluster auswählen.

```
.\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain FD1 -AcceptEULA

```

## <a name="remove-nodes-from-your-cluster"></a>Entfernen von Knoten aus Ihrem cluster

1. Je nach der Reliablity Ebene für den Cluster ausgewählt ist können Sie die ersten n (3/5/7/9) Knoten vom Typ primären Knoten nicht entfernen
2. Ausführen von RemoveNode Befehl in einem Entwickler Cluster wird nicht unterstützt.
2. Remotedesktop (RDP) in den virtuellen Computer/Computer, die Sie aus dem Cluster entfernen möchten.
2. Kopieren oder [eigenständigen Paket für Dienst Fabric für Windows Server herunterladen](http://go.microsoft.com/fwlink/?LinkId=730690) und Entzippen Sie ihn das Paket mit diesem virtuellen Computer/Computer.
3. Führen Sie Powershell als Administrator aus, und navigieren Sie zum Speicherort des Pakets extrahiert.
4. Führen Sie in der PowerShell *RemoveNode.ps1* ein. Im folgenden Beispiel entfernt den aktuellen Knoten aus dem Cluster. Die *ExistingClientConnectionEndpoint* ist eine Client-Verbindung-Endpunkt für jeden beliebigen Knoten, der im Cluster bleiben. Wählen Sie die IP-Adresse und den Endpunktport von *allen* **anderen Knoten** im Cluster an. Diese **anderen Knoten** wird die Cluster-Konfiguration für den entfernten Knoten wiederum aktualisiert. 

```
.\RemoveNode.ps1 -ExistingClientConnectionEndpoint 182.17.34.50:19000
```

Bitte beachten Sie, dass auch nach einem Knoten entfernt, es angezeigt wird möglicherweise als unten in Abfragen und umfassende. Dies ist ein bekannter Fehler und wird eine bevorstehende Version behoben werden. 


## <a name="next-steps"></a>Nächste Schritte
- [Konfiguration von Einstellungen für eigenständigen Windows-cluster](service-fabric-cluster-manifest.md)
- [Sichern von einem eigenständigen Cluster mit Windows-Sicherheit unter Windows](service-fabric-windows-cluster-windows-security.md)
- [Sichern von einem eigenständigen Cluster mit X509 unter Windows von Zertifikaten](service-fabric-windows-cluster-x509-security.md)
- [Erstellen eines eigenständigen Dienst Fabric Clusters mit Azure-virtuellen Computern unter Windows](service-fabric-cluster-creation-with-windows-azure-vms.md)
