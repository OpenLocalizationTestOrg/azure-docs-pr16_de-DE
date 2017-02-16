<properties
   pageTitle="Erstellen Sie einen eigenständigen Cluster mit Azure-virtuellen Computern unter Windows | Microsoft Azure"
   description="Informationen Sie zum Erstellen und Verwalten eines Azure Service Fabric Clusters auf Azure-virtuellen Computern unter Windows Server."
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
   ms.date="08/05/2016"
   ms.author="dkshir;chackdan"/>



# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>Erstellen Sie einen eigenständigen Dienst Fabric Cluster mit drei Knoten mit Azure-virtuellen Computern unter Windows Server

In diesem Artikel wird beschrieben, wie einen Cluster auf Windows basierende Azure-virtuellen Computern (virtuelle Computer), verwenden das Installationsprogramm des eigenständigen Dienst Fabric für Windows Server zu erstellen. Dies ist eine spezielle Groß-/Kleinschreibung von [Erstellen und Verwalten von einem Cluster unter Windows Server](service-fabric-cluster-creation-for-windows-server.md) wo sind die virtuellen Computern [Azure-virtuellen Computern unter Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md), jedoch [keiner Azure cloudbasierten Dienst Fabric Cluster](service-fabric-cluster-creation-via-portal.md)erstellt werden. Der Unterschied ist, dass eigenständigen Dienst Fabric Cluster erstellt werden, indem Sie die folgenden Schritte wird vollständig von Ihnen verwaltet, während der Azure cloudbasierten Dienst Fabric Cluster verwaltet und vom Dienst Fabric Ressourcenanbieter durchgeführt werden.


## <a name="steps-to-create-the-standalone-cluster"></a>Schritte zum Erstellen des eigenständigen Clusters

1. Melden Sie sich bei der Azure-Portal an, und erstellen Sie einen neuen Windows Server 2012 R2 Datacenter virtuellen Computer in einer Ressourcengruppe. Lesen Sie den Artikel, [erstellen einen virtuellen Computer Windows Azure-Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md) , weitere Details.
2. Fügen Sie einige weitere Windows Server 2012 R2 Datacenter virtuellen Computern in derselben Ressourcengruppe ein. Stellen Sie sicher, dass jede der virtuellen Computern dieselben Administrator-Benutzernamen und Kennwort ein, wenn Sie erstellt hat. Nach der Erstellung alle drei virtuellen Computern im gleichen virtuellen Netzwerk sollte angezeigt werden.
3. Verbinden mit jeder der der virtuellen Computern und Deaktivieren der Windows Firewall mit dem [Server-Manager, lokalen Server Dashboard](https://technet.microsoft.com/library/jj134147.aspx). Dadurch wird sichergestellt, dass der Netzwerkdatenverkehr zwischen den Computern kommunizieren kann. Während Sie auf jedem Computer angeschlossen ist, erhalten Sie die IP-Adresse, öffnen ein Eingabeaufforderungsfenster und geben `ipconfig`. Alternativ können Sie die IP-Adresse der einzelnen Computer finden Sie unter, indem Sie die Ressource virtuelles Netzwerk für die Ressourcengruppe Azure-Portal auswählen.
4. Herstellen einer Verbindung eine der virtuellen Computern mit und Testen Sie, ob Sie die anderen zwei virtuellen Computern anpingen können.
5. Verbinden mit einer der virtuellen Computern und [eigenständigen Dienst Fabric Paket für Windows Server herunterladen](http://go.microsoft.com/fwlink/?LinkId=730690) in einen neuen Ordner auf dem Computer, und das Paket zu extrahieren.
6. Öffnen Sie die *ClusterConfig.Unsecure.MultiMachine.json* -Datei in Editor, und bearbeiten Sie die einzelnen Knoten mit den drei IP-Adressen der Computer. Ändern Sie des Clusternamens oben, und speichern Sie die Datei.  Nachfolgend finden Sie ein Beispiel für das Cluster Manifest teilweise.

    ```
    {
        "name": "TestCluster",
        "clusterManifestVersion": "1.0.0",
        "apiVersion": "2015-01-01-alpha",
        "nodes": [
        {
            "nodeName": "vm0",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "vm2",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
    ],
    ```

7. Öffnen Sie ein [PowerShell ISE Fenster](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise)an. Navigieren Sie zu dem Ordner, wo Sie das heruntergeladene eigenständigen Installer-Paket extrahiert und die Manifestdatei Cluster gespeichert. Führen Sie den folgenden PowerShell-Befehl ein.

    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
    ```

8. Die PowerShell ausführen, eine Verbindung mit jedem Computer aus, und erstellen Sie einen Cluster sollte angezeigt werden. Nachdem Sie über eine Minute um Sie prüfen können, ob der Cluster funktionsfähig ist durch Herstellen einer Verbindung zum Dienst Fabric-Explorer auf eine der IP-Adresse des Computers z. B. mithilfe von `http://10.7.0.5:19080/Explorer/index.html`. Da hierbei handelt es sich um einen eigenständigen Cluster mit Azure-virtuellen Computern, können sie secure Sie müssen [Zertifikate mit den Azure-virtuellen Computern bereitstellen](service-fabric-windows-cluster-x509-security.md) oder Einrichten von einem Computer unter einem [Windows Server Active Directory (AD) Controller für Windows-Authentifizierung](service-fabric-windows-cluster-windows-security.md), wie Sie lokal tun würden.


## <a name="next-steps"></a>Nächste Schritte
- [Erstellen von eigenständigen Dienst Fabric Cluster unter Windows Server oder Linux](service-fabric-deploy-anywhere.md)
- [Hinzufügen oder Entfernen von Knoten zu einem eigenständigen Dienst Fabric cluster](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Konfiguration von Einstellungen für eigenständigen Windows-cluster](service-fabric-cluster-manifest.md)
- [Sichern von einem eigenständigen Cluster mit Windows-Sicherheit unter Windows](service-fabric-windows-cluster-windows-security.md)
- [Sichern von einem eigenständigen Cluster mit X509 unter Windows von Zertifikaten](service-fabric-windows-cluster-x509-security.md)
