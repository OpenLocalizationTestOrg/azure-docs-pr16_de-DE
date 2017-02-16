<properties
   pageTitle="Erstellen und Verwalten von einem eigenständigen Azure Service Fabric Cluster | Microsoft Azure"
   description="Erstellen und verwalten einen Azure Service Fabric Cluster auf alle Computer (physische oder virtuelle) ausgeführt Windows Server, sei es lokal oder in einer beliebigen Cloud."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="dkshir;chackdan"/>


# <a name="create-and-manage-a-cluster-running-on-windows-server"></a>Erstellen und Verwalten von einem Cluster auf Windows Server

Azure Service Fabric können Dienst Fabric Cluster auf jedem virtuellen Computern oder Computern unter Windows Server zu erstellen. Dies bedeutet bereitstellen und Ausführen Dienst Fabric Applikationen in jeder Umgebung, die eine Reihe von Windows Server-Computern verbundener enthält, sei es lokal oder mit einem Cloudanbieter. Fabric-Dienst bietet ein Setup-Paket zum Dienst Fabric Cluster bezeichnet das eigenständige Windows Server-Paket zu erstellen.

In diesem Artikel führt Sie durch die Schritte zum Erstellen von eines Clusters mithilfe der eigenständigen Pakets Dienst Architektur auf lokale, obwohl sie einfach für jede andere Umgebung wie andere Cloud-Anbieter angepasst werden kann.

>[AZURE.NOTE] Dieses eigenständigen Windows Server-Paket enthält möglicherweise Elemente, die derzeit in der Vorschau werden und nicht für kommerzielle Nutzung unterstützt. Der Liste der Features, die in der Vorschau sind, finden Sie unter "Vorschau-Features in diesem Paket enthalten." Sie können auch [Herunterladen einer Kopie der Endbenutzer-Lizenzvertrag](http://go.microsoft.com/fwlink/?LinkID=733084) jetzt.


<a id="getsupport"></a>
## <a name="get-support-for-the-service-fabric-standalone-package"></a>Anfordern von Unterstützung für den Dienst Fabric eigenständigen Paket

- Fragen Sie die Community zu diesem Dienst Fabric eigenständigen-Paket für Windows Server im [Forum Azure Service Fabric](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).

- Öffnen Sie ein Ticket [Professional Support für Dienst Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146 ).  Weitere Informationen zu [Professional Support von Microsoft](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).

<a id="downloadpackage"></a>
## <a name="download-the-service-fabric-standalone-package"></a>Dienst Fabric eigenständigen Paket herunterladen


[Herunterladen des eigenständigen Pakets für Dienst Fabric für Windows Server 2012 R2 und höher](http://go.microsoft.com/fwlink/?LinkId=730690), mit dem Namen Microsoft.Azure.ServiceFabric.WindowsServer. &lt;Version&gt;ZIP.


Im Download-Paket finden Sie die folgenden Dateien:

|**Dateiname**|**Kurze Beschreibung**|
|-----------------------|--------------------------|
|MicrosoftAzureServiceFabric.cab|Die CAB-Datei, die Binärdateien enthält, die auf jedem Computer im Cluster bereitgestellt werden.|
|ClusterConfig.Unsecure.DevCluster.json|Eine Cluster-Konfiguration Beispieldatei, die die Einstellungen für eine ungeschützte, drei Knoten, einem einzelnen Computer (oder virtuellen Computern) Entwicklung Cluster, einschließlich der Informationen für die einzelnen Knoten im Cluster enthält. |
|ClusterConfig.Unsecure.MultiMachine.json|Ein Cluster Konfiguration Beispieldatei, die die Einstellungen für einen Cluster ungeschützte, mehrere Computer (oder virtuellen Computer), einschließlich der Informationen für jeden Computer im Cluster enthält.|
|ClusterConfig.Windows.DevCluster.json|Eine Cluster-Konfiguration Beispieldatei, die enthält alle Einstellungen für eine sichere und drei Knoten, einem einzelnen Computer (oder virtuellen Computern) Entwicklung Cluster, einschließlich der Informationen für die einzelnen Knoten, die im Cluster ist. Cluster wird mithilfe der [Windows-Identitäten](https://msdn.microsoft.com/library/ff649396.aspx)gesichert.|
|ClusterConfig.Windows.MultiMachine.json|Ein Cluster Konfiguration Beispieldatei, die alle Einstellungen für einen sicheren, mehrere Computer (oder virtuellen Computer) Cluster mit Windows-Sicherheit, einschließlich der Informationen für jeden Computer, der im sicheren Cluster enthält. Cluster wird mithilfe der [Windows-Identitäten](https://msdn.microsoft.com/library/ff649396.aspx)gesichert.|
|ClusterConfig.x509.DevCluster.json|Ein Cluster Konfiguration Beispieldatei, die alle Einstellungen für eine sichere und drei Knoten, einem einzelnen Computer (oder virtuellen Computern) Entwicklung Cluster, einschließlich der Informationen für die einzelnen Knoten im Cluster enthält. Cluster ist gesicherter mit X509 Zertifikate.|
|ClusterConfig.x509.MultiMachine.json|Eine Cluster-Konfiguration Beispieldatei, die alle Einstellungen für den Cluster secure, mit mehreren Computer (oder virtuellen Computer), einschließlich der Informationen für die einzelnen Knoten im sicheren Cluster enthält. Cluster ist gesicherter mit X509 Zertifikate.|
|EULA_ENU.txt|Der Lizenzbedingungen für die Verwendung von Microsoft Azure Service Fabric eigenständige Windows Server-Paket. Sie können [eine Kopie des EULA herunterladen](http://go.microsoft.com/fwlink/?LinkID=733084) jetzt.|
|Readme.txt|Ein Link zur die Versionsinformationen und Standardinstallation Anweisungen. Es ist eine Teilmenge der den Anweisungen in diesem Dokument.|
|CreateServiceFabricCluster.ps1|Ein Powershellskript, den mit den Einstellungen in ClusterConfig.json Cluster erstellt.|
|RemoveServiceFabricCluster.ps1|Ein Powershellskript, das einen Cluster mit den Einstellungen in ClusterConfig.json entfernt.|
|ThirdPartyNotice.rtf |Hinweis der Software von Drittanbietern, die im Paket enthalten ist.|
|AddNode.ps1|Hinzufügen eines Knotens zu einem vorhandenen eines PowerShell-Skript bereitgestellt Cluster.|
|RemoveNode.ps1|Entfernen eines Knotens aus einem vorhandenen eines PowerShell-Skript bereitgestellt Cluster.|
|CleanFabric.ps1|Ein PowerShell-Skript zum Bereinigen von einer eigenständiges Dienst Fabric Installation Deaktivieren der aktuellen Computer. Vorherige MSI-Installationen sollte mit ihren eigenen zugeordneten Deinstallationsprogramme entfernt werden.|
|TestConfiguration.ps1|Ein PowerShell-Skript zum Analysieren der Infrastruktur in der Cluster.json angegeben sind.|


## <a name="plan-and-prepare-your-cluster-deployment"></a>Planen und Vorbereiten der bereitstellungs cluster
Führen Sie die folgenden Schritte aus, bevor Sie Ihren Cluster erstellen.

### <a name="step-1-plan-your-cluster-infrastructure"></a>Schritt 1: Planen Sie Ihrer Infrastruktur cluster
Sie sind dabei, erstellen einen Dienst Fabric Cluster auf Computern, die Sie besitzen, damit Sie entscheiden können, welche Arten von Fehlern Cluster überstehen soll. Beispielsweise müssen Sie Stromleitungen oder internetverbindungen bereitgestellt hat diesen Computern trennen? Darüber hinaus sollten Sie die physische Sicherheit dieser Computer ein. Wo befinden die Computer, und wer benötigt Zugriff darauf? Nachdem Sie diese Entscheidungen haben, können Sie logisch die Computern der verschiedenen Fehlerstrukturanalyse-Domänen zuordnen (siehe Schritt 4). Die Planung für Herstellung Cluster Infrastruktur ist etwas komplexer als für Cluster testen.

<a id="preparemachines"></a>
### <a name="step-2-prepare-the-machines-to-meet-the-prerequisites"></a>Schritt 2: Vorbereiten der Computer ab, die erforderlichen Komponenten entsprechen.
Erforderliche Komponenten für jeden Computer, die Sie zum Cluster hinzufügen möchten:

- Mindestens 16 GB RAM wird empfohlen.
- Mindestens 40 GB verfügbarer Speicherplatz wird empfohlen.
- Eine 4 Core oder größer CPU wird empfohlen.
- Die Verbindung zu einem sicheren Netzwerk oder Netzwerke für alle Computer.
- Windows Server 2012 R2 oder Windows Server 2012 (Sie müssen [KB2858668](https://support.microsoft.com/kb/2858668) installiert haben).
- [.NET Framework 4.5.1 oder höher](https://www.microsoft.com/download/details.aspx?id=40773), vollständige installieren.
- [Windows PowerShell 3.0](https://msdn.microsoft.com/powershell/scripting/setup/installing-windows-powershell).
- Der [Dienst RemoteRegistry](https://technet.microsoft.com/library/cc754820) sollte auf allen Computern ausgeführt werden.

Der Clusteradministrator bereitstellen und Konfigurieren des Clusters muss auf jedem der Computer über [Administratorrechte](https://social.technet.microsoft.com/wiki/contents/articles/13436.windows-server-2012-how-to-add-an-account-to-a-local-administrator-group.aspx) verfügen. Sie können nicht auf einem Domain Controller Dienst Fabric installieren.

### <a name="step-3-determine-the-initial-cluster-size"></a>Schritt 3: Legen Sie die ursprüngliche Clustergröße
Jeder Knoten in einem eigenständigen Dienst Fabric Cluster hat der Dienst-Textur Runtime bereitgestellt und ist ein Mitglied der Cluster. In einer normalen Herstellung Bereitstellung ist ein Knoten pro OS Instanz (physische oder virtuelle). Die Größe der Zuordnungseinheiten wird durch Ihre geschäftliche Anforderungen bestimmt. Jedoch müssen Sie die Größe der minimalen Zuordnungseinheiten von drei Knoten (oder virtuellen Computern) verfügen.
Aus Gründen der Entwicklung haben Sie mehr als einen Knoten auf einem bestimmten Computer. In einer Umgebung Herstellung unterstützt Dienst Fabric nur ein Knoten pro physischen oder virtuellen Computern an.

### <a name="step-4-determine-the-number-of-fault-domains-and-upgrade-domains"></a>Schritt 4: Ermitteln der Anzahl der Fehlerstrukturanalyse Domänen und Aktualisieren von Domänen
*Fehlerstrukturanalyse-Domäne* (FD) ist eine physische Einheit des Fehlers und direkt bezieht sich auf die physische Infrastruktur in die Data Centers. Eine Domäne Fehlerstrukturanalyse besteht aus Hardwarekomponenten (Computern, Schalter, Netzwerke und mehr), die einen einzelnen Punkt des Fehlers freigeben. Zwar keine 1:1-Zuordnung zwischen Fehlerstrukturanalyse Domänen und Racks, kann grob sprechen, den einzelnen Shapes für Gestelle eine Domäne Fehlerstrukturanalyse angesehen werden. Wenn die Knoten im Cluster in Betracht ziehen, wird dringend empfohlen, dass die Knoten auf mindestens drei Fehlerstrukturanalyse Domänen verteilt werden.

Wenn Sie in ClusterConfig.json FDs angeben möchten, können Sie den Namen für die einzelnen FD auswählen. Dienst Fabric unterstützt hierarchische FDs, damit Sie Ihre Infrastruktur Suchtopologie in diese spiegeln können.  Beispielsweise gelten die folgenden FDs:

- "FaultDomain": "fd: / Room1/Rack 1/Machine1"
- "FaultDomain": "fd: / FD1"
- "FaultDomain": "fd: / Room1/Rack 1/PDU1/M1"


*Aktualisieren einer Domäne* (UD) ist eine logische Einheit von Knoten. Bei Service Fabric koordiniert Upgrades (ein Anwendungsupgrade oder eines Upgrades Cluster) werden alle Knoten in einem UD nach unten übernommen, um die Aktualisierung durchführen, während Sie Knoten in anderen UDs Anfragen dienen zur Verfügung stehen. Firmware-Upgrades, die Sie auf Ihrem Computer ausführen UDs, nicht beachtet, sodass Sie diese eine tun müssen nacheinander Arbeitsplatz.

Die einfachste Möglichkeit, diese Konzepte anzustellen ist FDs als Maßeinheit ungeplanten Ausfall und UDs als Maßeinheit der geplanten Wartung zu berücksichtigen.

Wenn Sie in ClusterConfig.json UDs angeben möchten, können Sie den Namen für die einzelnen UD auswählen. Beispielsweise gelten die folgenden Namen:

- "UpgradeDomain": "UD0"
- "UpgradeDomain": "UD1A"
- "UpgradeDomain": "DomainRed"
- "UpgradeDomain": "Blau"

Weitere ausführliche Informationen zum Upgrade und Fehlerstrukturanalyse-Domänen finden Sie unter [einem Dienst Fabric Cluster beschreiben](service-fabric-cluster-resource-manager-cluster-description.md).

### <a name="step-5-download-the-service-fabric-standalone-package-for-windows-server"></a>Schritt 5: Dienst Fabric eigenständigen Paket für Windows Server herunterladen
[Dienst Fabric eigenständigen Paket für Windows Server herunterladen](http://go.microsoft.com/fwlink/?LinkId=730690) und Entzippen Sie ihn das Paket, das mit einem Bereitstellung Computer, die nicht Bestandteil der Cluster ist oder mit einem der Computer, die Ihren Cluster verwendet werden soll. Sie können den entpackt Ordner umbenennen `Microsoft.Azure.ServiceFabric.WindowsServer`.

<a id="createcluster"></a>
## <a name="create-your-cluster"></a>Erstellen Sie Ihren cluster

Nachdem Sie die Planung und Vorbereitung Schritte gegangen sind, sind Sie bereit sind, Ihren Cluster zu erstellen.

### <a name="step-1-modify-cluster-configuration"></a>Schritt 1: Ändern der Clusterkonfiguration
Der Cluster wird in ClusterConfig.json beschrieben. Details auf die Abschnitte in dieser Datei finden Sie unter [Konfiguration von Einstellungen für eigenständigen Windows Cluster](service-fabric-cluster-manifest.md).
Öffnen Sie eine der ClusterConfig.json Dateien aus dem Paket, die, das Sie heruntergeladen haben, und ändern Sie die folgenden Einstellungen:

<!--Loc Comment: Please, check that line 129 the clause has been modified to "that you use as placement constraints" instead of using "you are used as placement constraints"-->

|**Einstellung für die Konfiguration**|**Beschreibung**|
|-----------------------|--------------------------|
|**NodeTypes**|Knotentypen können Sie Ihre Clusterknoten in verschiedenen Gruppen zu trennen. Ein Cluster muss mindestens eine NodeType verfügen. Alle Knoten in der Gruppe stehen die folgenden Merkmale gemeinsam: <br> **Name** – Dies ist der Name der Knoten Typ. <br>**Endpunkt-Ports** – Hierbei handelt es sich um verschiedene benannten Endpunkte (Ports), die diese Art von Knoten zugeordnet sind. Sie können beliebig viele Port verwenden, die Sie wünschen, solange sie nicht ein anderer Teil dieses Manifest Konflikt stehen, und werden nicht bereits von einer anderen Anwendung auf dem Computer/virtuellen Computer ausführen. <br> **Platzierungseigenschaften** – diese beschreiben Eigenschaften für diesen Knoten, die Sie als Platzierung Einschränkungen für die Dienste oder Ihrer Dienste verwenden. Diese Eigenschaften sind benutzerdefinierter Schlüssel-Wert-Paare, die zusätzliche Metadaten für einen angegebenen Knoten bereitstellen. Beispiele für Eigenschaften des Knotens wäre, ob der Knoten verfügt, eine Festplatte oder Grafikkarte, die Anzahl der Laufwerke in deren Festplatte Kerne sowie andere physischen Eigenschaften. <br> **Kapazität** - Kapazität definieren, den Namen und die Menge an eine bestimmte Ressource, die ein bestimmter Knoten enthält Knoten für Ernährung verfügbar. Beispielsweise möglicherweise einen Knoten definieren, dass sie Kapazität für eine Metrik namens "MemoryInMb" aufweist und 2048 MB verfügbar standardmäßig hat. Diese Kapazität werden zur Laufzeit verwendet, um sicherzustellen, dass die Dienste, die bestimmte Ressourcen beanspruchen auf den Knoten platziert werden, denen diese Ressourcen, die in den erforderlichen Mengen verfügbar sind.<br>**IsPrimary** – Wenn Sie mehrere NodeType definiert sicherstellen, dass nur eine haben wird, festgelegt mit dem Wert *true*, auf Primär, wo die Dienste ausgeführt. Alle anderen Typen von Knoten sollte der Wert *false* festgelegt werden|
|**Knoten**|Dies sind die Details für jeden einzelnen Knoten, die den Cluster (Knotentyp, Name des Knotens, IP-Adresse, Fehlerstrukturanalyse-Domäne und Upgrade Domäne des Knotens) gehören. Die Computer Cluster auf hier die IP-Adressen aufgelistet werden müssen erstellt werden soll. <br> Wenn Sie die IP-Adresse für alle Knoten verwenden, wird dann ein Cluster ein-Feld erstellt, die Sie zu Testzwecken verwenden können. Verwenden Sie ein-Feld Cluster nicht für die Bereitstellung von Produktionsarbeitslasten aus.|


### <a name="step-2-run-the-testconfiguration-script"></a>Schritt 2: Führen Sie das Skript TestConfiguration

Das Skript TestConfiguration überprüft Ihre Infrastruktur definierten in cluster.json, um sicherzustellen, dass die erforderlichen Berechtigungen zugewiesen sind, die Computer miteinander verbunden sind und andere Attribute definiert wurden, sodass die Bereitstellung erfolgreich ausgeführt werden kann. Es ist im Wesentlichen eine Minisymbolleiste ExBPA. Wir weiterhin über einen Zeitraum, eine sichere wird dieses Tool Weitere Validierungen hinzufügen.

Dieses Skript kann auf jedem Computer ausgeführt werden, die Administratorzugriff auf alle Computer enthält, die als Knoten in der Clusterkonfigurationsdatei aufgeführt sind. Der Computer, auf dem dieses Skript ausgeführt wird, klicken Sie auf hat keinen Teil der Cluster sein.

```powershell

PS C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer> .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json
Trace folder already exists. Traces will be written to existing trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
Running Best Practices Analyzer...
Best Practices Analyzer completed successfully.


LocalAdminPrivilege        : True
IsJsonValid                : True
IsCabValid                 : True
RequiredPortsOpen          : True
RemoteRegistryAvailable    : True
FirewallAvailable          : True
RpcCheckPassed             : True
NoConflictingInstallations : True
FabricInstallable          : True
Passed                     : True


```

### <a name="step-3-run-the-create-cluster-script"></a>Schritt 3: Führen Sie das Erstellen Cluster-Skript
Nachdem Sie die Clusterkonfiguration im Dokument JSON geändert und hinzugefügt haben, alle Knoteninformationen hinzu, führen die Clustererstellung *CreateServiceFabricCluster.ps1* PowerShell-Skript aus dem Paketordner und in den Pfad zu der Konfigurationsdatei JSON übergeben. Wenn dies abgeschlossen ist, akzeptieren Sie den Endbenutzer-Lizenzvertrag.

Dieses Skript kann auf jedem Computer ausgeführt werden, die Administratorzugriff auf alle Computer enthält, die als Knoten in der Clusterkonfigurationsdatei aufgeführt sind. Der Computer, auf dem dieses Skript ausgeführt wird, klicken Sie auf hat keinen Teil der Cluster sein.

```
#Create an unsecured local development cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```
```
#Create an unsecured multi-machine cluster

.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -AcceptEULA
```

>[AZURE.NOTE] Die Bereitstellungsprotokolle sind lokal auf dem virtuellen Computer/Computer verfügbar, die Sie der CreateServiceFabricCluster PowerShell ausgeführt haben. Sie sind in einem Unterordner namens DeploymentTraces unter dem Ordner, in dem Sie den PowerShell-Befehl ausgeführt haben. Um festzustellen, ob der Dienst Fabric mit einem Computer ordnungsgemäß bereitgestellt wurde, können Sie die installierten Dateien im Verzeichnis C:\ProgramData finden und FabricHost.exe und Fabric.exe Prozesse ausgeführt im Task-Manager angezeigt werden können.

### <a name="step-4-connect-to-the-cluster"></a>Schritt 4: Herstellen einer Verbindung Cluster mit

Verbinden mit einem sicheren Cluster finden Sie unter [Service Fabric Cluster sichere Verbindung](service-fabric-connect-to-secure-cluster.md).

Führen Sie zum Verbinden mit einem unsicheren Cluster den folgenden PowerShell-Befehl aus:

```powershell

Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>

Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000

```
### <a name="step-5-bring-up-service-fabric-explorer"></a>Schritt 5: Rufen Sie Dienst Fabric-Explorer auf

Nachdem Sie mit dem Cluster mit Service Fabric Explorer entweder direkt von einem Computer mit Http://localhost:19080/Explorer/index.html oder Remote mit http://< verbinden können*IPAddressofaMachine*>: 19080/Explorer/index.html.



## <a name="add-and-remove-nodes"></a>Hinzufügen und Entfernen von Knoten

Sie können hinzufügen oder Entfernen von Knoten zum Cluster Service Fabric eigenständigen Anforderungen Ihres Unternehmens ändern. Die detaillierten Schritte finden Sie unter [Hinzufügen oder Entfernen von Knoten zu einem Dienst Fabric eigenständigen Cluster](service-fabric-cluster-windows-server-add-remove-nodes.md) .


## <a name="remove-a-cluster"></a>Entfernen von einem cluster

Um einen Cluster zu entfernen, führen Sie das *RemoveServiceFabricCluster.ps1* PowerShell-Skript aus dem Paketordner aus, und den Pfad zur Konfigurationsdatei JSON übergeben. Optional können Sie einen Speicherort für das Protokoll des Löschvorgangs angeben.

Dieses Skript kann auf jedem Computer ausgeführt werden, die Administratorzugriff auf alle Computer enthält, die als Knoten in der Clusterkonfigurationsdatei aufgeführt sind. Der Computer, auf dem dieses Skript ausgeführt wird, klicken Sie auf hat keinen Teil der Cluster sein.

```
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json   
```


<a id="telemetry"></a>
## <a name="telemetry-data-collected-and-how-to-opt-out-of-it"></a>Werden Daten gesammelt und wie Sie daraus Suchbegriffen

Standardmäßig sammelt das Produkt werden auf der Dienst Fabric Nutzung zur Optimierung des Produkts. Die beste Methode Analyzer, der als Teil der Setup überprüft die Konnektivität zu [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1)ausgeführt wird. Wenn es nicht erreichbar ist, schlägt die Einrichtung es sei denn, Sie außerhalb des werden Suchbegriffen.

  1. Der Verkaufspipeline werden versucht, die folgenden Daten zu [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) einmal täglich hochladen. Es ist eine bestmögliche hochladen und hat keine Auswirkung auf die Clusterfunktionalität. Die werden nur von den Knoten, die das Failover ausgeführt wird gesendet wurde primären Manager. Keine anderen Knoten versenden werden.

  2. Die telemetrieprotokoll besteht aus den folgenden:

-            Anzahl der Dienste
-            Anzahl der ServiceTypes
-            Anzahl von Applications
-            Anzahl der ApplicationUpgrades
-            Anzahl der FailoverUnits
-            Anzahl der InBuildFailoverUnits
-            Anzahl der UnhealthyFailoverUnits
-            Anzahl der Replikate
-            Anzahl der InBuildReplicas
-            Anzahl der StandByReplicas
-            Anzahl der OfflineReplicas
-            CommonQueueLength
-            QueryQueueLength
-            FailoverUnitQueueLength
-            CommitQueueLength
-            Anzahl der Knoten
-            IsContextComplete: Wahr/falsch
-            ClusterId: Dies ist eine GUID zufällig generiert für jeden cluster
-            ServiceFabricVersion
-             IP-Adresse des virtuellen Computers oder der Computer, von denen die werden hochgeladen wird


Um telemetrieprotokoll zu deaktivieren, fügen Sie den folgenden *Eigenschaften* in der Cluster Config: *EnableTelemetry: falsch*.

<a id="previewfeatures"></a>
## <a name="preview-features-included-in-this-package"></a>In diesem Paket enthaltenen Features von Preview

Keine.

>[AZURE.NOTE] Mit dem neuen [GA eigenständigen Cluster für Windows Server-Version (Version 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), Sie können Ihren Cluster in zukünftigen Versionen manuell oder automatisch aktualisiert. Da dieses Feature nicht auf der Preview-Versionen zur Verfügung steht, müssen Sie zum Erstellen eines Clusters mithilfe der GA-Version und zum Migrieren Ihre Daten und Applikationen aus dem Preview Cluster. Achten Sie auf Weitere Details zu diesem Feature.


## <a name="next-steps"></a>Nächste Schritte
- [Konfiguration von Einstellungen für eigenständigen Windows-cluster](service-fabric-cluster-manifest.md)
- [Hinzufügen oder Entfernen von Knoten zu einem eigenständigen Dienst Fabric cluster](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Erstellen eines eigenständigen Dienst Fabric Clusters mit Azure-virtuellen Computern unter Windows](service-fabric-cluster-creation-with-windows-azure-vms.md)
- [Sichern von einem eigenständigen Cluster mit Windows-Sicherheit unter Windows](service-fabric-windows-cluster-windows-security.md)
- [Sichern von einem eigenständigen Cluster mit X509 unter Windows von Zertifikaten](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
