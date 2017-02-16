<properties
   pageTitle="Upgrade von einem eigenständigen Dienst Fabric Cluster unter Windows Server | Microsoft Azure"
   description="Aktualisieren Sie den Dienst Fabric Code und/oder Konfiguration, die einen eigenständigen Dienst Fabric Cluster, einschließlich festlegen Cluster Update-Modus ausgeführt wird."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/10/2016"
   ms.author="chackdan"/>


# <a name="upgrade-your-standalone-service-fabric-cluster-on-windows-server"></a>Aktualisieren Sie Ihrer eigenständigen Dienst Fabric Cluster unter Windows Server

> [AZURE.SELECTOR]
- [Azure Cluster](service-fabric-cluster-upgrade.md)
- [Eigenständige Cluster](service-fabric-cluster-upgrade-windows-server.md)

Bei allen modernen Systemen ist Erstellen eines Konzepts für bedingten-Taste, um langfristiges Erfolg Ihres Produkts eintritt. Ein Dienst Fabric-Cluster ist eine Ressource, die Sie besitzen. Dieser Artikel beschreibt, wie Sie sicherstellen, dass der Cluster stets unterstützten Versionen der Dienst Textur Code und Konfigurationen ausgeführt werden soll.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Steuern der Fabric-Version, die für Ihren Cluster ausgeführt wird

Sie können Ihre Cluster Dienstupdates Fabric herunterladen, wenn Microsoft eine neue Version veröffentlicht, oder lassen die wählen Sie eine unterstützte Fabric Version Ihrer Cluster sein muss, den gewünschten festlegen. 

Zu diesem Zweck die Cluster-Konfiguration "FabricClusterAutoupgradeEnabled" auf true oder false festlegen.


>[AZURE.NOTE] Vergewissern Sie sich immer eine Version unterstützte Dienst Fabric Cluster beibehalten. Wie und wir die Version eine neue Version des Diensts Fabric ankündigen, wird die vorherige Version nach mindestens 60 Tage von diesem Zeitpunkt für das Ende des Supports markiert. Neuen Versionen werden bekannt gegebenen [auf den Dienst Fabric-Teamblog](https://blogs.msdn.microsoft.com/azureservicefabric/ ). Wählen Sie dann ist die neue Version steht. 


Sie können Ihren Cluster nur, wenn Sie eine Konfiguration der Herstellung-Schreibweise Knoten arbeiten, wo jeden Dienst Fabric Knoten auf einem separaten physischen oder virtuellen Computern reserviert auf die neue Version aktualisieren. Wenn Sie einen Cluster Entwicklung haben, wobei es mehrere Dienst Fabric Knoten auf einer einzelnen physischen oder virtuellen Computern gibt, müssen Sie Ihren Cluster beenden und mit der neuen Version neu erstellen.


Es gibt zwei unterschiedlichen Workflows für ein Upgrade Ihrer Cluster auf die aktuellsten oder eine unterstützte Service Fabric Version aus. Platzhalter für Cluster, für die Verbindung zu die neueste Version automatisch herunterladen und den zweiten für Cluster, die keine Verbindung zu laden Sie die neueste Version der Dienst Fabric sind.

### <a name="upgrade-the-clusters-with-connectivity-to-download-the-latest-code-and-configuration"></a>Aktualisieren Sie die Cluster mit Verbindung zu den neuesten Code und Konfiguration herunterladen 

Mit den folgenden Schritten so aktualisieren Sie Ihre Cluster auf eine unterstützte Version, wenn Ihre Clusterknoten Internet-Verbindung zu [http://download.microsoft.com](http://download.microsoft.com) verfügen. 

Cluster, die Verbindung zu [http://download.microsoft.com](http://download.microsoft.com)haben, überprüfen wir regelmäßig für die Verfügbarkeit von neuen Dienst Fabric Versionen.


Wenn eine neue Version der Dienst Fabric verfügbar ist, wird das Paket lokal in der Cluster heruntergeladen und nach der Bereitstellung für das Upgrade vor. Darüber hinaus stellen um den Kunden über diese neue Version zu informieren, das System eine explizite Cluster Gesundheit Warnung ähnlich wie der folgende:

"Die aktuelle Cluster Version [Version Nr.] Support endet [Datum].", nachdem der Cluster die neueste Version ausgeführt wird, geht die Warnung abwesend.


#### <a name="cluster-upgrade-workflow"></a>Upgrade Cluster-Workflow.
 
Sobald Sie die Cluster Health Warnung angezeigt wird, müssen Sie wie folgt vorgehen:

1. Verbinden Sie mit dem Cluster aus einem beliebigen Computer mit Administratorzugriff auf alle Computer, die als Knoten im Cluster aufgelistet werden. Der Computer, auf dem dieses Skript ausgeführt wird, klicken Sie auf hat keinen Teil der cluster

    ```powershell

    ###### connect to the secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Abrufen der Liste der Dienst Fabric-Versionen, denen aktualisiert werden kann

    ```powershell

    ###### Get the list of available service fabric versions 
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    Sie sollten eine Ausgabe ähnlich wie folgt erhalten:

    ![Abrufen von Fabric Versionen][getfabversions]

3. Starten eines Deaktivieren eines Upgrades Cluster auf einen der Versionen, die unter Verwendung des [Start-ServiceFabricClusterUpgrade PowerShell Cmd](https://msdn.microsoft.com/library/mt125872.aspx) verfügbar ist

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Sie können den Fortschritt des Upgrades oder durch Ausführen des folgenden Power Shell-Befehls auf Dienst Fabric Explorer überwachen.

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the Start-ServiceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Nachdem Sie die Probleme, die das Zurücksetzen geführt haben behoben haben, müssen Sie das Upgrade erneut, initiieren, indem Sie die gleichen Schritte als vor.


### <a name="upgrade-the-clusters-with-uno-connectivityu-to-download-the-latest-code-and-configuration"></a>Aktualisieren Sie die Cluster mit <U>keine Konnektivität</u> zum Herunterladen der neuesten Code und Konfiguration

Mit den folgenden Schritten so aktualisieren Sie Ihre Cluster auf eine unterstützte Version, wenn die Cluster Knoten **haben keine** Verbindung zum Internet zu [http://download.microsoft.com](http://download.microsoft.com) 


>[AZURE.NOTE]Wenn Sie einen Cluster, der nicht Internet verbunden ist ausgeführt werden, müssen Sie den Dienst Fabric-Teamblog zu einer neuen Version benachrichtigt zu werden, überwachen. Platzieren Sie alle Cluster Gesundheit Warnung Sie davon benachrichtigen das System **nicht unterstützt** .  

1. Ändern der Cluster-Konfigurations, damit die folgende Eigenschaft auf falsch festgelegt.

        "fabricClusterAutoupgradeEnabled": false,

und Starten eines Deaktivieren eines Upgrades Konfiguration. Verwendungsdetails finden Sie unter [Start-ServiceFabricClusterUpgrade PS Cmd](https://msdn.microsoft.com/library/mt125872.aspx) . Cluster ist manifest die Version, die Sie in der clusterConfig.JSON aufweisen. Vergewissern Sie sich vor dem Projektstart der Konfiguration Aktualisierung aktualisieren.

```powershell

    Start-ServiceFabricClusterUpgrade [-Config] [-ClusterConfigVersion] -FailureAction Rollback -Monitored 

```

#### <a name="cluster-upgrade-workflow"></a>Upgrade Cluster-Workflow.
 


1. Laden Sie die neueste Version des Pakets aus [Erstellen Dienst Fabric Cluster für WindowsServer](service-fabric-cluster-creation-for-windows-server.md) -Dokument 


1. Verbinden Sie mit dem Cluster aus einem beliebigen Computer mit Administratorzugriff auf alle Computer, die als Knoten im Cluster aufgelistet werden. Der Computer, auf dem dieses Skript ausgeführt wird, klicken Sie auf hat keinen Teil der cluster 

    ```powershell

    ###### connect to the cluster
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3" 
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. Kopieren Sie das heruntergeladene Paket in den Cluster Bild-Speicher.

    ```powershell

    ###### Get the list of available service fabric versions 
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file including the path to it> -ImageStoreConnectionString "fabric:ImageStore"

    ###### Here is a filled out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"


    ```

2. Registrieren Sie sich das kopierte Paket 

    ```powershell

    ###### Get the list of available service fabric versions 
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of the .cab file> 

    ###### Here is a filled out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```


3. Starten eines Deaktivieren eines Upgrades Cluster auf einen der Versionen, die verfügbar ist. 

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback
    
    ```
Sie können den Fortschritt des Upgrades oder durch Ausführen des folgenden Power Shell-Befehls auf Dienst Fabric Explorer überwachen.

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    If the cluster health policies are not met, the upgrade is rolled back. You can specify custom health policies at the time for the start-serviceFabricClusterUpgrade command refer to [this document](https://msdn.microsoft.com/library/mt125872.aspx) for details. 

Nachdem Sie die Probleme, die das Zurücksetzen geführt haben behoben haben, müssen Sie das Upgrade erneut, initiieren, indem Sie die gleichen Schritte als vor.



## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie einige der [Dienst Fabric Cluster Fabric Einstellungen](service-fabric-cluster-fabric-settings.md) anpassen
- Erfahren Sie, wie Sie [Ihren Cluster ein-und skalieren](service-fabric-cluster-scale-up-down.md)
- Erfahren Sie mehr über [Upgrades der Anwendung](service-fabric-application-upgrade.md)

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
