<properties
   pageTitle="Konfigurieren von secure Verbindungen vom Dienst Fabric Cluster unterstützt | Microsoft Azure"
   description="Informationen Sie zum Verwenden von Visual Studio sichere Verbindungen konfiguriert werden, die von der Azure Service Fabric Cluster unterstützt werden."
   services="service-fabric"
   documentationCenter="na"
   authors="cawaMS"
   manager="paulyuk"
   editor="tglee" />

<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="10/08/2015"
   ms.author="cawaMS" />

# <a name="configure-secure-connections-to-a-service-fabric-cluster-from-visual-studio"></a>Konfigurieren von secure Verbindungen zu einem Dienst Fabric Cluster aus Visual Studio

Erfahren Sie, wie Sie mit Visual Studio sicher einen Cluster Azure Service Fabric mit so konfiguriert, dass Steuerelement-Richtlinien zugreifen.

## <a name="cluster-connection-types"></a>Typen von Cluster-Verbindung

Zwei Arten von Verbindungen werden vom Cluster Azure Service Fabric unterstützt: **nicht sichere** Verbindungen und **Zertifikaten basierende X509** sichere Verbindungen. (Für Dienst Fabric Cluster gehostet lokalen, **Windows** und **dSTS** Authentifizierung werden ebenfalls unterstützt). Sie müssen den Verbindungstyp Cluster konfigurieren, wenn der Cluster erstellt wird. Nachdem sie erstellt wurde, kann der Verbindungstyp geändert werden.

Die Tools für Visual Studio-Dienst Fabric unterstützt alle Authentifizierungsarten zum Herstellen einer Verbindung zu einem Cluster für die Veröffentlichung an. Anweisungen zum Einrichten eines sicheren Dienst Fabric Clusters finden Sie unter [Einrichten von einem Dienst Fabric Cluster vom Azure-Portal](service-fabric-cluster-creation-via-portal.md) .

## <a name="configure-cluster-connections-in-publish-profiles"></a>Konfigurieren von Clusterverbindungen in Profile veröffentlichen

Wenn Sie ein Projekt Dienst Fabric von Visual Studio zu veröffentlichen, verwenden Sie im Dialogfeld **Dienst Fabric-Anwendung veröffentlichen** einen Azure Service Fabric Cluster durch Klicken auf die Schaltfläche **Wählen Sie** im Abschnitt **Endpunkt Verbindung** auswählen. Sie können melden Sie sich bei Ihrem Konto Azure und wählen Sie dann einen vorhandenen Cluster unter Ihrer Abonnements.

![Die ** im Dialogfeld veröffentlichen Dienst Fabric Anwendung ** wird verwendet, um eine Verbindung Dienst Fabric konfigurieren.][publishdialog]

Klicken Sie im Dialogfeld **Wählen Sie Dienst Fabric Cluster** überprüft automatisch die Cluster-Verbindung. Validierung erfolgreich verläuft, bedeutet dies, dass das System die richtigen Zertifikate installiert haben hat, um eine sichere Verbindung zum Cluster herstellen, oder Ihren Cluster nicht sicher ist. Überprüfungsfehler können Probleme nicht auf Ihrem System zum Verbinden mit einem sicheren Cluster ordnungsgemäß konfiguriert oder Netzwerkproblemen verursacht werden.

![In der ** wählen Sie Dienst Fabric Cluster ** im Dialogfeld konfigurieren Sie eine vorhandene Verbindung der Dienst Fabric Cluster oder erstellen und konfigurieren Sie eine neue Cluster-Verbindung.][selectsfcluster]

### <a name="to-connect-to-a-secure-cluster"></a>Die Verbindung zu einem sicheren cluster

1.  Stellen Sie sicher, dass Sie eines der Clientzertifikate zugreifen können, die der Zielcluster vertrauenswürdig einstuft. Das Zertifikat ist in der Regel als Personal Information Exchange (PFX-Datei) Datei freigegeben. Finden Sie unter [Einrichten von einem Dienst Fabric Cluster vom Azure-Portal](service-fabric-cluster-creation-via-portal.md) so konfigurieren Sie den Server auf einem Client Zugriff gewähren.

2.  Installieren Sie das vertrauenswürdige Zertifikat. Hierzu Doppelklicken Sie auf die PFX-Datei oder verwenden Sie das PowerShell-Skript importieren-PfxCertificate, um die Zertifikate zu importieren. Installieren Sie das Zertifikat zum **Zertifikat: \LocalMachine\My**ein. Es ist OK, um alle Standardeinstellungen beim Importieren des Zertifikats zu übernehmen.

3.  Wählen Sie den Befehl **veröffentlichen...** im Kontextmenü des Projekts zum Öffnen des Dialogfelds **Azure-Anwendung veröffentlichen** , und wählen Sie dann den gewünschten Cluster aus. Das Tool automatisch die Verbindung aufgelöst und speichert die sichere Verbindungsparameter im Profil veröffentlichen.

4.  [Optional]: You can edit the publish profile to specify a secure cluster connection.

    Da Sie manuell die veröffentlichen Profil XML-Datei die Zertifikatinformationen angeben bearbeiten, werden Sie sicher, dass der Name des Zertifikats Store zu beachten, speichern Sie Speicherort und Fingerabdruck des Zertifikats. Sie müssen Sie einen Namen und Speicherort speichern diese Werte für das Zertifikat des Store bereitstellen. Finden Sie unter [wie: Abrufen des Fingerabdrucks eines Zertifikats](https://msdn.microsoft.com/library/ms734695(v=vs.110).aspx) für Weitere Informationen.

    Die Parameter *ClusterConnectionParameters* können die PowerShell-Parameter verwenden, bei der Verbindung mit dem Dienst Fabric Cluster angeben. Gültige Parameter sind diejenigen, indem Sie das Cmdlet verbinden-ServiceFabricCluster akzeptiert werden. Lesen Sie [Verbinden-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) für eine Liste der verfügbaren Parameter ein.

    Wenn Sie zu einem remote Cluster veröffentlichen möchten, müssen Sie die entsprechenden Parameter für diese bestimmte Cluster angeben. Im folgenden finden ein Beispiel für das Herstellen einer Verbindung mit einem unsicheren Cluster:

    `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`

    Hier ein Beispiel für das Herstellen einer Verbindung mit einer X509 secure Cluster basierenden Zertifikats:

    ```
    <ClusterConnectionParameters
    ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000"
    X509Credential="true"
    ServerCertThumbprint="0123456789012345678901234567890123456789"
    FindType="FindByThumbprint"
    FindValue="9876543210987654321098765432109876543210"
    StoreLocation="CurrentUser"
    StoreName="My" />
    ```

5.  Bearbeiten Sie alle anderen notwendigen Einstellungen wie Upgrade Parameter und Dateispeicherort Anwendungsparameter, und veröffentlichen Sie die Anwendung im Dialogfeld **Veröffentlichen Fabric Anwendungsdienst** klicken Sie dann in Visual Studio.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Zugriff auf Dienst Fabric Cluster finden Sie unter [Ihren Cluster mit Service Fabric Explorer Visualisierung](service-fabric-visualizing-your-cluster.md).

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
