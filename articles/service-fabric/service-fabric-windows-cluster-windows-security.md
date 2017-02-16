<properties
   pageTitle="Sichern von einem Cluster mit Windows-Sicherheit unter Windows unter | Microsoft Azure"
   description="Informationen Sie zum Konfigurieren der Knoten-zu-Knoten und Client-zu-Knoten-Sicherheit auf einem eigenständigen Cluster mit Windows-Sicherheit unter Windows ausgeführt."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="secure-a-standalone-cluster-on-windows-using-windows-security"></a>Sichern von einem eigenständigen Cluster mit Windows-Sicherheit unter Windows

Um unbefugten Zugriff auf einen Dienst Fabric Cluster zu verhindern müssen Sie es, Sichern besonders, wenn sie Produktionsarbeitslasten daran ausgeführt hat. In diesem Artikel wird beschrieben, wie Knoten-zu-Knoten und Client-zu-Knoten Sicherheit mithilfe der Windows-Sicherheit in der Datei *ClusterConfig.JSON* konfigurieren und Schritt Sicherheit Konfigurieren des [eigenständigen Cluster erstellen, auf dem Windows](service-fabric-cluster-creation-for-windows-server.md)entspricht. Weitere Informationen darüber, wie der Dienst Fabric Windows-Sicherheit verwendet finden Sie unter [Cluster Sicherheitsszenarien](service-fabric-cluster-security.md).

>[AZURE.NOTE]
Sie sollten Ihre Auswahl Sicherheit-Knoten Wertpapiers, sorgfältig, da es kein Upgrade Cluster aus eine Sicherheitsoption in ein anderes ist. Ändern der Auswahl Sicherheit müssten vollständige Cluster neu zu erstellen.

## <a name="configure-windows-security"></a>Konfigurieren Sie die Windows-Sicherheit
Die Stichprobe *ClusterConfig.Windows.JSON* Konfigurationsdatei heruntergeladen haben, mit der [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) eigenständigen Cluster-Paket enthält eine Vorlage zum Konfigurieren der Windows-Sicherheit.  Windows-Sicherheit wird im Abschnitt **Eigenschaften** konfiguriert:

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

|**Einstellung für die Konfiguration**|**Beschreibung**|
|-----------------------|--------------------------|
|ClusterCredentialType|Windows-Sicherheit ist aktiviert, indem Sie den Parameter **ClusterCredentialType** auf *Windows*.|
|ServerCredentialType|Windows-Sicherheit für Clients ist aktiviert, indem Sie den Parameter **ServerCredentialType** auf *Windows*. Dies zeigt an, dass die Clients von der Cluster, zu Kopien innerhalb einer Active Directory-Domäne ausgeführt werden.|
|WindowsIdentities|Enthält die Identitäten Cluster und Client.|
|ClusterIdentity|Konfigurieren von Knoten Sicherheit. Eine kommagetrennte Liste der Gruppe verwaltete Dienstkonten oder Computernamen an.|
|ClientIdentities|Konfigurieren von Client-zu-Knoten-Sicherheit. Ein Array von Client-Benutzerkonten.|
|Identität|Die Clientidentität eines Domänenbenutzers.|
|IsAdmin|True gibt an, dass es sich bei der Domänenbenutzer Administrator Client, für den Client Benutzerzugriff falsch zugreifen können.|

[Knoten Knoten Sicherheit](service-fabric-cluster-security.md#node-to-node-security) wird durch die Einstellung **ClusterIdentity**konfiguriert. Um Trust Beziehungen zwischen Knoten zu erstellen, müssen sie miteinander aufmerksam gemacht werden. Hierfür kann auf zwei verschiedene Arten: Geben Sie die Gruppe verwaltete Dienstkonto, die alle Knoten im Cluster enthält, oder geben Sie die Domäne Knoten Identitäten aller Knoten im Cluster. Es wird dringend empfohlen, mit der [Gruppe verwaltete-Dienstkonto (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) Ansatz, besonders für größere Cluster (mehr als 10 Knoten) oder Cluster, die wahrscheinlich vergrößert oder verkleinert.
Dieser Ansatz ermöglicht Knoten hinzugefügt oder aus der gMSA ohne Änderungen an den Cluster Manifest entfernt werden sollen. Dieser Ansatz erfordert keine die Erstellung einer Gruppe für die Domäne für die Clusteradministratoren gewährt wurde Zugriffsrechte zum Hinzufügen und Entfernen von Mitgliedern. Weitere Informationen finden Sie unter [Erste Schritte mit der Gruppe verwaltete Dienstkonten](http://technet.microsoft.com/library/jj128431.aspx).

[Client Knoten Sicherheit](service-fabric-cluster-security.md#client-to-node-security) wird mithilfe von **ClientIdentities**konfiguriert. Um Vertrauensstellung zwischen einem Client und Cluster einrichten zu können, müssen Sie konfigurieren Cluster welche Client Identitäten erhalten, die sie vertrauen kann. Dies kann auf zwei verschiedene Arten: Geben Sie die Domäne Gruppe Benutzer, die eine Verbindung herstellen können, oder geben Sie die Domäne Knoten Benutzer, die eine Verbindung herstellen können. Dienst Fabric unterstützt zwei Arten von anderen Access-Steuerelement für Clients, die mit einem Dienst Fabric Cluster verbunden sind: "Unternehmensadministrator" und Benutzer. Access-Steuerelement ermöglicht das Cluster-Administrator, den Zugriff auf bestimmte Arten von Clustervorgänge für verschiedene Gruppen von Benutzern, Verbesserung der Sicherheit von Cluster begrenzen.  Administratoren haben Vollzugriff auf Verwaltungsfunktionen (/ Lese-Funktionen, einschließlich). Benutzer haben standardmäßig nur Lesezugriff Verwaltungsfunktionen (z. B.-Abfragefunktionen, die), und die Möglichkeit, Anwendungen und Dienste zu beheben.

Im folgenden Beispiel **Sicherheit** Abschnitt konfiguriert Windows-Sicherheit und gibt an, dass der Computer in *ServiceFabric/clusterA.contoso.com* Cluster gehören und *CONTOSO\usera* Administrator Client zugreifen können:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
        "IsAdmin": true
        }]
    }
},
```

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren der Windows-Sicherheit in der Datei *ClusterConfig.JSON* , fortsetzen Sie, den Erstellungsprozess Cluster in [Erstellen eines eigenständigen Cluster Windows ausgeführt werden](service-fabric-cluster-creation-for-windows-server.md).

Weitere Informationen finden Sie unter-Knoten Sicherheit, Client-zu-Knoten-Sicherheit und rollenbasierte Access-Steuerelement [Cluster Sicherheitsszenarien](service-fabric-cluster-security.md).

Beispiele für die Verbindung mithilfe der PowerShell oder FabricClient finden Sie unter [Verbinden mit einem sicheren Cluster](service-fabric-connect-to-secure-cluster.md) .
