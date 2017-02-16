
<properties
   pageTitle="Dienst Fabric Clustersicherheit: Clientrollen | Microsoft Azure"
   description="In diesem Artikel werden die zwei Clientrollen und die zugehörigen Berechtigungen zur Verfügung gestellt, die die Rollen."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="coreysa"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>



# <a name="role-based-access-control-for-service-fabric-clients"></a>Rollenbasierte Access Steuerelement für Fabric Service-clients

Azure Service-Struktur unterstützt zwei Arten von anderen Access-Steuerelement für Clients, die mit einem Dienst Fabric Cluster verbunden sind: "Unternehmensadministrator" und Benutzer. Access-Steuerelement ermöglicht den Cluster-Administrator, den Zugriff auf bestimmte Clustervorgänge für verschiedene Gruppen von Benutzern, Verbesserung der Sicherheit von Cluster begrenzen.  

**Administratoren** haben Vollzugriff auf Verwaltungsfunktionen (/ Lese-Funktionen, einschließlich). Standardmäßig haben **Benutzer** nur Lesezugriff Verwaltungsfunktionen (z. B. Abfragefunktionen), und die Möglichkeit, Anwendungen und Dienste zu beheben.

Sie angeben die beiden Clientrollen (Administrator und Client) zum Zeitpunkt der Clustererstellung können, indem Sie für jede separate Zertifikate. Details zum Einrichten von einem sicheren Dienst Fabric Cluster finden Sie unter [Service Fabric Clustersicherheit](service-fabric-cluster-security.md) .


## <a name="default-access-control-settings"></a>Standardeinstellungen für Access-Steuerelement


Der Administrator Access Steuerelementtyp verfügt über Vollzugriff auf alle FabricClient APIs. Sie können alle liest und schreibt auf dem Dienst Fabric Cluster, einschließlich der folgenden Vorgänge ausführen:


### <a name="application-and-service-operations"></a>Anwendung und Service-Vorgänge
* **CreateService**: Erstellung service                           
* **CreateServiceFromTemplate**: service Erstellung aus Vorlage                             
* **UpdateService**: service Updates                            
* **' DeleteService '**: service löschen                           
* **ProvisionApplicationType**: Anwendung Typ bereitgestellt                           
* ****: Erstellen der Anwendung                               
* **DeleteApplication**: Löschen der Anwendung                           
* **UpgradeApplication**: Starten oder unterbrechen Upgrades der Anwendung                             
* **UnprovisionApplicationType**: Anwendung Typ bekannt                           
* **MoveNextUpgradeDomain**: Fortsetzen des Upgrades der Anwendung mit einer Domäne explizit aktualisieren                           
* **ReportUpgradeHealth**: Fortsetzen des Upgrades der Anwendung mit den Fortschritt der aktuellen Aktualisierung                          
* **ReportHealth**: reporting Dienststatus                            
* **PredeployPackageToNode**: vor-API                         
* **CodePackageControl**: einen Neustart von Code-Paketen                          
* **RecoverPartition**: Wiederherstellen einer Partition                          
* **RecoverPartitions**: Partitionen wiederherstellen                          
* **RecoverServicePartitions**: Wiederherstellen den Servicepartitionen                           
* **RecoverSystemPartitions**: Dienst Systempartitionen wiederherstellen                             


### <a name="cluster-operations"></a>Clustervorgänge
* **ProvisionFabric**: MSI und/oder Cluster manifest bereitgestellt                             
* **UpgradeFabric**: Starten Cluster-Upgrades                          
* **UnprovisionFabric**: MSI und/oder Cluster manifest bekannt                         
* **MoveNextFabricUpgradeDomain**: fortsetzen Cluster Upgrades mit einer Domäne explizit aktualisieren                             
* **ReportFabricUpgradeHealth**: Cluster Upgrades mit den Fortschritt der aktuellen Aktualisierung fortsetzen                            
* **StartInfrastructureTask**: Starten von Infrastruktur Aufgaben                            
* **FinishInfrastructureTask**: Abschließen Infrastruktur Aufgaben                          
* **InvokeInfrastructureCommand**: Infrastruktur Befehle zur Verwaltung des Vorgangs                              
* **ActivateNode**: Aktivieren von einem Knoten                           
* **DeactivateNode**: können einen Knoten                           
* **DeactivateNodesBatch**: können mehrere Knoten                             
* **RemoveNodeDeactivations**: Zurücksetzen Deaktivierung auf mehreren Knoten                             
* **GetNodeDeactivationStatus**: Überprüfen des Status der Deaktivierung                           
* **NodeStateRemoved**: reporting Knotenstatus entfernt                            
* **ReportFault**: reporting Fehlerstrukturanalyse                          
* **FileContent**: Bild Store Client Dateiübertragung (extern zu Cluster)                           
* **FileDownload**: Bild Store Client Datei herunterladen Einleitung (außerhalb Cluster)                           
* **InternalList**: Bild Client Datei Liste Speichervorgang (intern)                           
* **Löschen**: Store Client Löschvorgang Bild                           
* **Hochladen**: Bild Client Upload-Speichervorgang                           
* **NodeControl**: starten, beenden und Neustarten von Knoten                             
* **MoveReplicaControl**: Replikate von einem Knoten auf einen anderen verschieben                          

### <a name="miscellaneous-operations"></a>Verschiedene Operationen
* **Ping**: Client Pings                            
* **Abfrage**: alle Abfragen zulässig.
* **NameExists**: URI Vorhandensein Prüfungen benennen                           



Der Benutzer Access-Steuerelementtyp ist standardmäßig, beschränkt den folgenden Schritten: 

* **EnumerateSubnames**: URI Enumeration benennen                             
* **EnumerateProperties**: benennen die Eigenschaft Enumeration                          
* **PropertyReadBatch**: Eigenschaft benennt die Vorgänge lesen                            
* **GetServiceDescription**: Long-Umfrage Dienst Benachrichtigungen und Lesebereich service Beschreibungen                           
* **ResolveService**: Antrag webbasierten Dienst Auflösung                            
* **ResolveNameOwner**: Beheben von naming URI Besitzer                          
* **ResolvePartition**: Auflösen von Systemdiensten                           
* **ServiceNotifications**: Ereignis-basierten Dienst Benachrichtigungen                           
* **GetUpgradeStatus**: Abrufen der Anwendung Upgradestatus                          
* **GetFabricUpgradeStatus**: Abrufen des Upgradestatus Cluster                            
* **InvokeInfrastructureQuery**: Abfragen Infrastruktur Aufgaben                          
* **Liste**: Bild Store Client-Datei Liste Vorgang                          
* **ResetPartitionLoad**: Laden für Failovereinheit zurücksetzen                            
* **ToggleVerboseServicePlacementHealthReporting**: Umschalten reporting zu ausführlichen Dienst Platzierung Dienststatus                             

Das Access-Steuerelement Administrator hat auch Zugriff auf die vorherige Vorgänge.

## <a name="changing-default-settings-for-client-roles"></a>Ändern der Standardeinstellungen für Clientrollen

In der Manifestdatei Cluster können Sie Administrator-Funktionen, die an den Client bereitstellen, falls erforderlich. Sie können die Standardeinstellungen ändern, indem Sie die Option **Einstellungen Fabric** während der [Clustererstellung](service-fabric-cluster-creation-via-portal.md), und die vorherigen Einstellungen in den **Namen**, **Administrator**, **Benutzer-**und **Wertfeldern** bereitstellen.

## <a name="next-steps"></a>Nächste Schritte

[Dienst Fabric Cluster-Sicherheit](service-fabric-cluster-security.md)

[Dienst Fabric Clustererstellung](service-fabric-cluster-creation-via-portal.md)
