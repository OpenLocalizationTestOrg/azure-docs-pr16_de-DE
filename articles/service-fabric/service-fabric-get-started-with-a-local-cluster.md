<properties
   pageTitle="Erste Schritte mit bereitstellen und Aktualisieren von apps auf Ihrem lokalen Cluster | Microsoft Azure"
   description="Richten Sie eine lokale Dienst Fabric Cluster, Bereitstellen einer vorhandenen Anwendungs darauf, und aktualisieren Sie die Anwendung."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="ryanwi;mikhegn"/>

# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a>Erste Schritte mit bereitstellen und Aktualisieren von Applications auf Ihrem lokalen cluster
Die Azure Service Fabric SDK enthält eine vollständige lokale Entwicklung-Umgebung, die Sie verwenden können, um schnell mit bereitstellen und Verwalten von Applications auf einem lokalen Cluster anzufangen. In diesem Artikel erstellen Sie einen lokalen Cluster, Bereitstellen eine vorhandene Anwendung darauf und aktualisieren Sie die Anwendung, um eine neue Version, alle von Windows PowerShell.

> [AZURE.NOTE] In diesem Artikel wird vorausgesetzt, dass Sie bereits [Einrichten Ihrer Entwicklungsumgebung](service-fabric-get-started.md).

## <a name="create-a-local-cluster"></a>Erstellen Sie einen lokalen cluster
Ein Dienst Fabric Cluster stellt eine Reihe von Hardware-Ressourcen, denen Sie Applikationen zum bereitstellen können. In der Regel ein Cluster an einer beliebigen Stelle von fünf auf Tausende von Autos besteht aus. Der Dienst Fabric SDK enthält jedoch eine Cluster-Konfiguration, die auf einem einzigen Computer ausgeführt werden kann.

Es ist wichtig zu verstehen, dass der Dienst Fabric lokale Cluster keine Emulator und Simulator ist. Führt den gleichen Plattformcode, der in einer Umgebung mit mehreren Computern Cluster befindet. Der einzige Unterschied ist, dass sie die Plattform Prozesse ausgeführt wird, die über fünf Autos auf einem Computer normal verteilt sind.

Das SDK bietet zwei Methoden zum Einrichten eines lokalen Clusters: ein Windows PowerShell-Skript und die lokale Cluster Manager System über Taskleiste app. In diesem Lernprogramm verwenden wir das PowerShell-Skript aus.

> [AZURE.NOTE] Wenn Sie einen lokalen Cluster bereits erstellt haben, indem Sie eine Anwendung von Visual Studio bereitstellen, können Sie diesen Abschnitt überspringen.


1. Starten Sie ein neues Fenster mit PowerShell als Administrator an.

2. Führen Sie das Setup-Skript Cluster SDK Ordner aus:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```

    Cluster-Setup dauert ein paar Minuten. Nach Abschluss der Installation sollte ähnlich wie die Ausgabe angezeigt werden:

    ![Cluster Setup Ausgabe][cluster-setup-success]

    Sie können nun versuchen, eine Anwendung zum Cluster bereitzustellen.

## <a name="deploy-an-application"></a>Bereitstellen einer Anwendung
Der Dienst Fabric SDK enthält eine umfangreiche Menge von Framework und Developer Tools für Applikationen erstellen. Wenn Sie lernen, wie Sie die Anwendungen in Visual Studio erstellen möchten, finden Sie unter [Erstellen Ihrer ersten Fabric Service-Anwendung in Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).

In diesem Lernprogramm verwenden wir eine vorhandene Beispiel-Anwendung (WordCount genannt), damit wir uns auf die Aspekte beim Verwalten von der Plattform – einschließlich Bereitstellung, Überwachung und Upgrade konzentrieren können.


1. Starten Sie ein neues Fenster mit PowerShell als Administrator an.

2. Importieren des Dienst Fabric SDK PowerShell-Moduls.

    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```

3. Erstellen Sie ein Verzeichnis zum Speichern von der Anwendungs, die Sie herunterladen und bereitstellen, z. B. C:\ServiceFabric.

    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```

4. An die Position [die Anwendung WordCount herunterladen](http://aka.ms/servicefabric-wordcountapp) haben Sie erstellt.  Hinweis: Microsoft Edge Browser speichert die Datei mit der Erweiterung *ZIP* .  Ändern Sie die Erweiterung in *.sfpkg*ein.

5. Verbinden Sie mit dem lokalen Cluster:

    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```

6. Erstellen Sie eine neue Anwendung das SDKs Befehl "Bereitstellung" mit einen Namen und einen Pfad für das Anwendungspaket.

    ```powershell  
  Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```

    Wenn alles reibungslos funktioniert, sollten Sie folgende Ausgabe sehen:

    ![Bereitstellen einer Anwendung auf dem lokalen cluster][deploy-app-to-local-cluster]

7. Wenn Sie um die Anwendung in Aktion zu sehen, starten Sie den Browser, und navigieren Sie zu [Http://localhost:8081/wordcount/index.html](http://localhost:8081/wordcount/index.html)aus. Sie sollten angezeigt werden:

    ![Bereitgestellte Anwendung UI][deployed-app-ui]

    Die Anwendung WordCount ist sehr einfach. Er enthält clientseitige JavaScript-Code zum Generieren zufälliger fünfstelligen "Wörter" werden dann mit der Anwendung über ASP.NET Web-API weitergeleitet werden. Ein dynamische Dienst verfolgt die Anzahl der Wörter berücksichtigt. Sie sind unterteilt, basierend auf dem ersten Zeichen des Worts. Sie können den Quellcode für die app WordCount im [überfordert Beispiele](https://azure.microsoft.com/documentation/samples/service-fabric-dotnet-getting-started/)finden.

    Die Anwendung, die wir bereitgestellt enthält vier Partitionen an. Damit Anfangsbuchstaben von A bis G Wörter in die erste Partition gespeichert werden, beginnend mit H bis N Wörter werden in der zweiten Partition gespeichert, usw.

## <a name="view-application-details-and-status"></a>Anzeigen von Details zu Anwendung und status
Jetzt, da wir die Anwendung bereitgestellt haben, sehen wir uns einige der app-Details in der PowerShell.

1. Abfrage alle bereitgestellte Anwendungen auf dem Cluster an:

    ```powershell
    Get-ServiceFabricApplication
    ```

    Unter der Voraussetzung, dass Sie nur die WordCount app bereitgestellt haben, sehen Sie etwa an:

    ![Alle bereitgestellten Applications in PowerShell Abfragen][ps-getsfapp]

2. Wechseln Sie auf die nächste Ebene, indem Sie die Gruppe von Diensten, die in der Anwendung WordCount enthalten sind.

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Liste der Dienste für die Anwendung in PowerShell][ps-getsfsvc]

    Die Anwendung besteht aus zwei Dienste – das Web-front-End und dynamische Dienst, der die Wörter verwaltet werden.

3. Schließlich, sehen Sie sich die Liste der Partitionen für WordCountService:

    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```

    ![Anzeigen der Servicepartitionen PowerShell][ps-getsfpartitions]

    Festlegen der Befehle, die Sie verwendet, wie alle Dienst Fabric PowerShell-Befehle haben sind für alle Cluster, die Verbindung zu, lokalen oder einem Remotecomputer möglicherweise verfügbar.

    Für eine visuelle Möglichkeit Interaktion mit dem Cluster können Sie das Tool für die webbasierten Dienst Fabric Explorer durch Navigieren zur [Http://localhost:19080/Explorer](http://localhost:19080/Explorer) im Browser.

    ![Anzeigen von Details zu Anwendung im Dienst Fabric-Explorer][sfx-service-overview]

    > [AZURE.NOTE] Weitere Informationen zum Dienst Fabric Explorer finden Sie in der [Visualisierung Ihrer Clusters mit Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).

## <a name="upgrade-an-application"></a>Aktualisieren einer Anwendung
Fabric-Dienst bietet keine Ausfallzeiten Upgrades durch die Integrität der Anwendung überwachen, während sie sich über den Cluster ein für ein. Führen Sie uns ein einfaches Upgrade der Anwendung WordCount.

Die neue Version der Anwendung nun zählt nur Wörter, die mit einem Vokal beginnen. Während des Upgrades ein für ab, sehen wir zwei Änderungen in das Verhalten der Anwendung. Zunächst sollte der Zins, immer umfangreicher die Anzahl die wird, verlangsamen, da weniger Wörter werden gezählt wird. Zweites, sollten, da die erste Partition zwei Vokale (A und E weist), und alle anderen Partitionen nur einen einzelnen enthalten, deren Anzahl später beginnen an die anderen hinter sich gelassen.

1. [WordCount Version 2 Paket herunterladen](http://aka.ms/servicefabric-wordcountappv2) , an der gleichen Stelle, an der Sie das Paket Version 1 heruntergeladen haben.

2. Kehren Sie zu der PowerShell-Fenster zurück, und das SDKs Upgrade-Befehl verwenden, um die neue Version im Cluster zu registrieren. Klicken Sie dann bei einer Aktualisierung der Textur: / WordCount-Anwendung.

    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```

    Finden Sie unter Ausgabe in PowerShell, die Looks ähnlich wie der folgende als das Upgrade beginnt.

    ![Aktualisieren des Vorgangsfortschritts in PowerShell][ps-appupgradeprogress]

3. Während des Upgrades vorangehenden ist, können Sie deren Status aus dem Dienst Fabric Explorer überwachen einfacher. Starten Sie ein Browserfenster, und navigieren Sie zu [Http://localhost:19080/Explorer](http://localhost:19080/Explorer). Erweitern von **Applications** in der Strukturansicht auf der linken Seite, und klicken Sie dann **WordCount**, wählen Sie aus, und klicken Sie abschließend **Fabric: / WordCount**. Der Registerkarte Essentials sehen Sie den Status des Upgrades, während sie durch die Cluster Upgrade Domänen fortschreitet.

    ![Aktualisieren des Vorgangsfortschritts im Dienst Fabric-Explorer][sfx-upgradeprogress]

    Wie das Upgrade über jede Domäne durchgeführt werden können, werden integritätsprüfungen ausgeführt, um sicherzustellen, dass die Anwendung ordnungsgemäß verhält.

4. Wenn Sie erneut, die frühere Abfrage für die Gruppe von Diensten in der Textur ausführen: / WordCount-Anwendung, Mitteilung, die die Version des WordCountService geändert aber die Version von WordCountWebService konnten nicht:

    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```

    ![Abfrage Anwendungsdienste nach dem upgrade][ps-getsfsvc-postupgrade]

    Dadurch wird hervorgehoben, wie Dienst Fabric Upgrades der Anwendung verwaltet. Er berührt nur die Gruppe der Dienste (oder Code/Konfiguration Pakete innerhalb dieser Dienste), geändert haben, wodurch der Prozess des Upgrades schneller und die Zuverlässigkeit.

5. Schließlich an den Browser zu sehen, das Verhalten der neuen Version der Anwendung zurück. Wie erwartet, wird die Anzahl die langsamer fortschreitet, und die erste Partition endet mit etwas mehr des Datenträgers.

    ![Zeigen Sie die neue Version der Anwendung im browser][deployed-app-ui-v2]

## <a name="cleaning-up"></a>Bereinigen

Bevor Sie Nachbereitung, ist es wichtig, denken Sie daran, dass der lokale Cluster real ist. Applikationen weiterhin im Hintergrund ausgeführt, bis Sie sie entfernen.  Je nach Art Ihrer Apps kann eine app laufende erheblichen Ressourcen auf Ihrem Computer einnehmen. Sie haben mehrere Optionen zum Verwalten von Applications und Cluster:

1. Führen Sie folgende Schritte aus, um eine einzelne Anwendung und alle darin enthaltenen Daten zu entfernen:

    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```

    Oder löschen Sie die Anwendung aus der Dienst Fabric Explorer im Menü **Aktionen** oder in der Anwendung Listenansicht im linken Bereich des Kontextmenüs.

    ![Löschen einer Anwendung ist Dienst Fabric-Explorer][sfe-delete-application]

2. Nach dem Löschen der Anwendungs aus dem Cluster, können Sie Versionen 1.0.0 und 2.0.0 vom Typ Anwendung WordCount aufgehoben werden. Löschen entfernt die Anwendungspakete, einschließlich des Codes und Konfiguration, die Cluster Bild Store.

    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```

    Oder, im Dienst Fabric-Explorer, wählen Sie **Das Aufheben der Bereitstellung** der Anwendung.

3. Fahren Sie Cluster, aber behalten Sie die Anwendungsdaten und auf, klicken Sie auf **Lokale Cluster beenden** in der Taskleiste-app System.

4. Um den Cluster vollständig löschen möchten, klicken Sie auf **Lokale Cluster entfernen** in der Taskleiste-app System. Diese Option führt zu Fehlern in einer anderen langsam Bereitstellung das nächste Mal Sie drücken Sie F5 in Visual Studio. Entfernen Sie den lokalen Cluster nur, wenn Sie nicht für einige Zeit verwenden möchten, oder wenn Sie für das Freigeben von Ressourcen müssen.

## <a name="1-node-and-5-node-cluster-mode"></a>Knoten 1 und 5 Knoten Cluster-Modus

Bei der Arbeit mit dem lokalen Cluster zum Entwickeln von Applications häufig finden Sie selbst ausführen Schreiben von Code, für das Debuggen, Ändern von Code, Symbolleiste Iterationen Debuggen usw.. Wenn Sie dieses Verfahren optimieren, der lokale Cluster in zwei Modi ausgeführt werden kann: 1 oder 5 Knoten. Beide Modi Cluster haben ihre Vorteile.
5 Knoten Clustermodus können Sie für die Arbeit mit einem Cluster real. Arbeiten mit weitere Instanzen und Replikate Ihrer Dienste und Failover-Szenarien testen können.
1 Knoten Cluster-Modus ist optimiert führen Sie die schnelle Bereitstellung und Registrierung von Diensten, damit Sie schnell Code mithilfe der Dienst Fabric Runtime überprüfen können.

Sowohl 1 Knoten cluster-Modus und 5 Knoten Cluster-Modus ist kein Emulator oder Simulator. Führt den gleichen Plattformcode, der in einer Umgebung mit mehreren Computern Cluster befindet.

> [AZURE.NOTE] Diese Funktion steht in SDK Version 5.2 und höher.

Um den Clustermodus zu einem 1 Knotencluster zu ändern, verwenden Sie den Dienst Fabric lokale Cluster-Manager oder mithilfe der PowerShell wie folgt:

1. Starten Sie ein neues Fenster mit PowerShell als Administrator an.

2. Führen Sie das Setup-Skript Cluster SDK Ordner aus:

    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```

    Cluster-Setup dauert ein paar Minuten. Nach Abschluss der Installation sollte ähnlich wie die Ausgabe angezeigt werden:
    
    ![Cluster Setup Ausgabe][cluster-setup-success-1-node]

Wenn Sie den Dienst Fabric lokale Cluster-Manager verwenden:

![Switch-Cluster-Modus][switch-cluster-mode]

> [AZURE.WARNING] Beim Ändern von Clustermodus der aktuelle Cluster von Ihrem System entfernt wird, und ein neuer Cluster erstellt wird. Die Daten, die Sie in der Cluster gespeichert haben, müssen wird gelöscht werden, wenn Sie Clustermodus ändern.

## <a name="next-steps"></a>Nächste Schritte
- Jetzt, da Sie bereitgestellt und einige vorgefertigten Applikationen durchgeführt haben, können Sie [versuchen, eine eigene in Visual Studio erstellen](service-fabric-create-your-first-application-in-visual-studio.md).
- Alle Aktionen ausgeführt werden, auf dem lokalen Cluster in diesem Artikel können auf einer [Azure Cluster](service-fabric-cluster-creation-via-portal.md) auch ausgeführt werden.
- Das Upgrade, das wir in diesem Artikel durchgeführt wurde grundlegende. Finden Sie in der [Dokumentation upgrade](service-fabric-application-upgrade.md) erfahren Sie mehr über die Leistungsfähigkeit und Flexibilität der Dienst Fabric Upgrades.

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
