<properties
    pageTitle="Veröffentlichen eine app zu einem remote Cluster mit Visual Studio | Microsoft Azure"
    description="Erfahren Sie, wie eine Anwendung zu einem Remotedienst Fabric Cluster mithilfe von Visual Studio veröffentlichen."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="publish-an-application-to-a-remote-cluster-by-using-visual-studio"></a>Veröffentlichen einer Anwendungs zu einem remote Cluster mithilfe von Visual Studio

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Die Azure Service Fabric-Erweiterung für Visual Studio bietet eine einfache, wiederholbare und Skripts geeignet Möglichkeit, eine Anwendung zu einem Dienst Fabric Cluster veröffentlichen.

## <a name="the-artifacts-required-for-publishing"></a>Die Elemente, für die Veröffentlichung erforderlich

### <a name="deploy-fabricapplicationps1"></a>Bereitstellen von FabricApplication.ps1

Dies ist ein Powershellskript, einen veröffentlichen Profilpfad als Parameter für Veröffentlichung Dienst Fabric Applikationen verwendet. Da dieses Skript Teil Ihrer Anwendung ist, können Sie Willkommen bei Ihrer Anwendung je nach Bedarf ändern.

### <a name="publish-profiles"></a>Veröffentlichen von Benutzerprofilen

Ein Ordner in der Fabric Service-Anwendung mit dem Namen **PublishProfiles** enthält XML-Dateien, in denen wichtige Informationen für die Veröffentlichung von einer Anwendungs wie gespeichert:

- Dienst Fabric Cluster Verbindungsparameter
- Pfad für eine Anwendung parameter
- Upgradeeinstellungen

Standardmäßig wird eine Anwendung enthalten zwei Profile veröffentlichen: Local.xml und Cloud.xml. Sie können mehrere Profile durch Kopieren und Einfügen eine der Standarddateien hinzufügen.

### <a name="application-parameter-files"></a>Parameter-Dateien der Anwendung

Ein Ordner in der Fabric Service-Anwendung mit dem Namen **ApplicationParameters** enthält XML-Dateien für benutzerdefinierte Anwendung Manifesten Parameterwerte. Anwendungsmanifestdateien können parametrisierte sein, damit Sie verschiedene Werte für die Bereitstellung Einstellungen verwenden können. Weitere Informationen zum Parametrisieren Ihrer Anwendungs finden Sie unter [Verwalten von mehreren Umgebungen Service-Struktur](service-fabric-manage-multiple-environment-app-configuration.md).

>[AZURE.NOTE] Erstellen Sie für Akteur-Dienste das Projekt zuerst vor versucht, bearbeiten Sie die Datei in einem Editor oder über das Dialogfeld veröffentlichen. Dies liegt daran Teil der Manifestdateien bei der Erstellung generiert wird.

## <a name="to-publish-an-application-by-using-the-publish-service-fabric-application-dialog-box"></a>So veröffentlichen Sie eine Anwendung mithilfe des Dialogfelds veröffentlichen Fabric Anwendungsdienst

So veröffentlichen Sie eine Anwendung über das Dialogfeld **Dienst Fabric-Anwendung veröffentlichen** von Visual Studio-Dienst Fabric Tools bereitgestellten führen Sie die folgenden Schritten vor.

1. Wählen Sie im Kontextmenü des Projekts Dienst Fabric Anwendung **veröffentlichen...** im Dialogfeld **Veröffentlichen Anwendungsdienst Fabric** anzeigen zu können.

    ![Die ** im Dialogfeld veröffentlichen Dienst Fabric Anwendung **][0]

    Die Datei, die in der **Zielliste Profil** Dropdown-Listenfeld ausgewählt ist, in dem alle Einstellungen, außer **Manifest Versionen**, gespeichert sind. Sie können ein vorhandenes Profil wiederverwenden oder einen neuen erstellen, indem Sie Sie in der **Zielliste Profil** Dropdown-Listenfeld **< Profile verwalten... >** auswählen. Wenn Sie ein Veröffentlichungsprofil auswählen, werden deren Inhalt in den entsprechenden Feldern im Dialogfeld angezeigt. Um die Änderungen zu einem beliebigen Zeitpunkt zu speichern, wählen Sie den Link **Profil speichern** aus.    

2. Geben Sie im Abschnitt **Verbindungsendpunkt** einer lokalen oder einer Remotedatenbank Dienst Fabric Cluster des Veröffentlichen-Endpunkt an. Zum Hinzufügen oder ändern den Verbindungsendpunkt, klicken Sie auf der **Verbindungsendpunkt** Dropdown-Liste. Die Liste zeigt verfügbar Dienst Fabric Cluster Verbindungsendpunkte in denen Sie veröffentlichen können basierend auf Ihrer Azure-Abonnements. Beachten Sie, dass, wenn Sie sich nicht bereits Visual Studio angemeldet sind, Sie aufgefordert werden können.

    Verwenden Sie das Dialogfeld Auswahl Cluster aus dem Satz von Abonnements zur Verfügung und Cluster auswählen.

    ![Die ** im Dialogfeld Wählen Sie Dienst Fabric Cluster **][1]

    >[AZURE.NOTE] Wenn Sie auf einen beliebigen Endpunkt (beispielsweise ein Cluster Partei) veröffentlichen möchten, finden Sie unter der **Veröffentlichung auf einen beliebigen Cluster Endpunkt** unten angezeigt.

    Nachdem Sie einen Endpunkt auswählen, überprüft Visual Studio die Verbindung mit dem ausgewählten Dienst Fabric Cluster aus. Wenn Cluster nicht sicher ist, können Visual Studio sofort zu verbinden. Wenn der Cluster sicher ist, müssen Sie ein Zertifikat auf Ihrem lokalen Computer, bevor Sie fortfahren zu installieren. Weitere Informationen finden Sie unter [So konfigurieren Sie die sichere Verbindungen](service-fabric-visualstudio-configure-secure-connections.md) . Wenn Sie fertig sind, wählen Sie die Schaltfläche **OK** . Der ausgewählte Cluster wird im Dialogfeld **Dienst Fabric-Anwendung veröffentlichen** .

3. Navigieren Sie in der **Anwendung Parameterdatei** Dropdown-Listenfeld in einer Anwendung Parameter-Datei. Parameter-Datei einer Anwendung enthält benutzerdefinierte Werte für Parameter in die Anwendungsmanifestdatei ein. Hinzufügen oder Ändern eines Parameters, wählen die Schaltfläche **Bearbeiten** . Geben Sie an, oder ändern Sie den Wert des Parameters im Raster **Parameter** . Wenn Sie fertig sind, wählen Sie die Schaltfläche **Speichern** .

    ![Die ** Dialogfeld bearbeiten Parameter **][2]

4. Verwenden Sie das Kontrollkästchen **Aktualisieren Sie die Anwendung** , um anzugeben, dass diese Aktion veröffentlichen ist ein Upgrade an. Upgrade veröffentlichen Aktionen unterscheiden sich von Normal Aktionen veröffentlichen. Eine Liste der Unterschiede finden Sie unter [Service Fabric Anwendung aktualisieren](service-fabric-application-upgrade.md) . Wählen Sie den Link **Upgradeeinstellungen konfigurieren** Upgradeeinstellungen um zu konfigurieren. Upgrade Parameter-Editor wird angezeigt. Finden Sie unter [Konfigurieren des Upgrades einer Fabric Service-Anwendung](service-fabric-visualstudio-configure-upgrade.md) Weitere Informationen zum Upgrade-Parameter.

5. Wählen Sie aus der **Manifest Versionen...** Schaltfläche zum Anzeigen des Dialogfelds **Versionen bearbeiten** . Sie müssen Anwendung und Dienstversionen für ein Upgrade auf stattfinden aktualisieren. Finden Sie unter [Fabric Service-Anwendung upgrade Lernprogramm](service-fabric-application-upgrade-tutorial.md) erfahren Sie, wie Anwendung und dem Versionen Einfluss einer Upgradevorgang manifest.

    ![Die ** Dialogfeld bearbeiten Versionen **][3]

    Wenn die Anwendung und Dienstversionen semantische Versioning wie 1.0.0 oder numerische Werte in das Format des 1.0.0.0 verwenden, die Option **Anwendung und Dienstversionen automatisch zu aktualisieren** . Wenn wählen Sie diese Option, die den Dienst und Anwendung Versionsnummern werden automatisch aktualisiert, sobald eine Code, Config oder Daten Version Verpacken wird aktualisiert. Wenn Sie die Versionen manuell zu bearbeiten lieber, deaktivieren Sie das Kontrollkästchen, um dieses Feature deaktivieren.

    >[AZURE.NOTE] Erstellen Sie für alle Einträge Paket für ein Projekt Akteur angezeigt werden zuerst das Projekt, um die Einträge in der Dienst Manifest Dateien generiert.

6. Wenn Sie damit fertig sind angeben alle notwendigen Einstellungen, wählen Sie die Schaltfläche **Veröffentlichen** Ihrer Anwendung mit dem ausgewählten Dienst Fabric Cluster veröffentlichen aus. Die Einstellungen, die Sie angegeben haben, die auf Veröffentlichungsprozesses angewendet werden.

## <a name="publish-to-an-arbitrary-cluster-endpoint-including-party-clusters"></a>Veröffentlichen Sie auf einem beliebigen Cluster Endpunkt (einschließlich Cluster Partei)

Visual Studio Veröffentlichung Erfahrung ist für die Veröffentlichung auf einem Ihrer Abonnements Azure zugeordnet remote-Cluster optimiert. Jedoch ist es möglich, zu veröffentlichen, um beliebige Endpunkte (z. B. Dienst Fabric Partei Cluster) durch das Veröffentlichungsprofil XML-direkt zu bearbeiten. Wie oben beschrieben, zwei veröffentlichen Profile werden von Standard -**Local.xml** und **Cloud.xml**– bereitgestellt, die Sie aber Willkommen bei zusätzliche Profile für die verschiedenen Umgebungen erstellen. Beispielsweise sollten Sie zum Erstellen eines Profils für das Veröffentlichen zur Party Cluster, vielleicht mit der Bezeichnung **Party.xml**.

Wenn Sie eine Verbindung zu einem ungeschützte Cluster herstellen, erforderlich ist, lediglich den Endpunkt Cluster Verbindung, wie `partycluster1.eastus.cloudapp.azure.com:19000`. In diesem Fall würde der Verbindungsendpunkt im Profil veröffentlichen ungefähr wie folgt aussehen:

```XML
<ClusterConnectionParameters ConnectionEndpoint="partycluster1.eastus.cloudapp.azure.com:19000" />
```

  Wenn Sie eine Verbindung zu einem gesicherten Cluster herstellen, müssen Sie auch Geben Sie die Details des Clientzertifikats aus dem lokalen Speicher für Authentifizierung verwendet werden soll. Weitere Informationen hierzu finden Sie unter [Konfigurieren von secure Verbindungen zu einem Dienst Fabric Cluster](service-fabric-visualstudio-configure-secure-connections.md).

  Nachdem Sie Ihr Veröffentlichungsprofil eingerichtet haben, können Sie es im Dialogfeld veröffentlichen verweisen, wie unten dargestellt.

  ![Neues Profil veröffentlichen in veröffentlichen (Dialogfeld)][4]

  Beachten Sie, dass in diesem Fall das neues Profil veröffentlichen auf eine der Standard-Anwendung Parameterdateien verweist. Dies ist geeignet, wenn Sie die gleichen Anwendungskonfiguration auf eine Anzahl von Umgebungen veröffentlichen möchten. In Fällen, wo sollen verschiedene Konfigurationen für jede Umgebung, die Sie veröffentlichen möchten, würde hingegen sinnvoll ist, erstellen eine entsprechende Anwendung Parameterdatei stellen werden.

## <a name="next-steps"></a>Nächste Schritte

Automatisieren des Veröffentlichungsvorgangs in einer Umgebung mit fortlaufender Integration von Informationen finden Sie unter [Einrichten von Dienst Fabric fortlaufende Integration](service-fabric-set-up-continuous-integration.md).


[0]: ./media/service-fabric-publish-app-remote-cluster/PublishDialog.png
[1]: ./media/service-fabric-publish-app-remote-cluster/SelectCluster.png
[2]: ./media/service-fabric-publish-app-remote-cluster/EditParams.png
[3]: ./media/service-fabric-publish-app-remote-cluster/EditVersions.png
[4]: ./media/service-fabric-publish-app-remote-cluster/publish-to-party-cluster.png
