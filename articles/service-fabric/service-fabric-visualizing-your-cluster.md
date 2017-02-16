<properties
   pageTitle="Visualisieren den Cluster mithilfe der Dienst Fabric Explorer | Microsoft Azure"
   description="Dienst Fabric Explorer ist ein webbasierten Tool zum Überprüfen und Verwalten von Applications Cloud und Knoten in einem Microsoft Azure Service Fabric Cluster."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/22/2016"
   ms.author="seanmck"/>

# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>Visualisieren Sie Ihre Cluster mit Service Fabric-Explorer

Dienst Fabric Explorer ist ein webbasierten Tool zum Überprüfen und Verwalten von Applications und Knoten in einem Cluster Azure Service Fabric. Dienst Fabric Explorer wird direkt in Cluster gehostet wird, damit er immer verfügbar ist, unabhängig davon, wo Ihre Cluster ausgeführt wird.

## <a name="connect-to-service-fabric-explorer"></a>Herstellen einer Verbindung Dienst Fabric Explorer mit

Wenn Sie die Anweisungen zum [Vorbereiten Ihrer Entwicklungsumgebung](service-fabric-get-started.md)befolgt haben, können Sie auf Ihrem lokalen Cluster Service Fabric Explorer starten, durch Navigieren zur Http://localhost:19080/Explorer.

>[AZURE.NOTE] Wenn Sie Internet Explorer mit Service Fabric-Explorer zum Verwalten von eines remote Clusters verwenden werden, müssen Sie einige Internet Explorer-Einstellungen zu konfigurieren. Um sicherzustellen, dass alle Informationen lädt **Tools**richtig, wechseln Sie > **Ansichtsoptionen Kompatibilität** und **Intranet-Websites in der Ansicht Kompatibilität anzeigen**deaktivieren.

## <a name="understand-the-service-fabric-explorer-layout"></a>Grundlegendes zu den Dienst Fabric Explorer-layout

Sie können mithilfe der Strukturansicht auf der linken Seite über Dienst Fabric Explorer navigieren. Im Stammverzeichnis der Struktur Übersicht über das Dashboard Cluster Cluster, einschließlich eine Zusammenfassung der Anwendung und Knoten Dienststatus.

![Dienst Fabric Explorer Cluster dashboard][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a>Die Cluster Layout anzeigen

Knoten in einem Cluster Service Fabric über ein zweidimensionales Raster Fehlerstrukturanalyse Domänen gesetzt und Durchführen eines Upgrades Domänen. So stellen Sie sicher, dass Ihre Programme verfügbar in Anwesenheit Hardware-Fehlern und Upgrades der Anwendung bleiben. Sie können anzeigen, wie der aktuelle Cluster angeordnet wird mithilfe der Karte Cluster.

![Dienst Fabric Explorer Cluster Karte][sfx-cluster-map]

### <a name="view-applications-and-services"></a>Ansicht Anwendungen und Dienste

Cluster enthält zwei Unterstrukturen: eine für Applikationen sowie eine weitere Knoten.

Sie können die Ansicht Anwendung verwenden, zur Navigation im Dienst-Fabric logische Hierarchie: Applications, Services, Partitionen und Replikate.

Im folgenden Beispiel besteht aus zwei Dienste **MyStatefulService** und **Webdienst** **die Anwendung** . Da **MyStatefulService** dynamische ist, enthält sie eine Partition mit einer primären und zwei sekundäre Replikate. Im Gegensatz dazu WebSvcService staatenlos ist und eine einzelne Instanz enthält.

![Dienst Fabric Explorer-Anwendung-Ansicht][sfx-application-tree]

Jede Ebene der Struktur zeigt das Hauptfenster relevanten Informationen über das Element an. Beispielsweise können Sie den Integritätsstatus und die Version für einen bestimmten Dienst anzeigen.

![Dienst Fabric Explorer Essentials-Bereich][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a>Anzeigen des Cluster-Knoten

Die Knotenansicht zeigt das physische Layout der Cluster. Für einen angegebenen Knoten können Sie überprüfen, welche Applikationen Code auf diesem Knoten bereitgestellt haben. Genauer gesagt, können Sie sehen, welche Replikate aktuell noch ausgeführt werden.

## <a name="actions"></a>Aktionen

Dienst Fabric Explorer bietet eine schnelle Möglichkeit, Aktionen auf Knoten, Anwendungen und Dienste in Ihrem Cluster aufzurufen.

Angenommen, um eine Instanz der Anwendung zu löschen, wählen Sie die Anwendung einfach aus der Strukturansicht auf der linken Seite, und wählen Sie **Aktionen** > **Anwendung löschen**.

![Löschen einer Anwendung im Dienst Fabric-Explorer][sfx-delete-application]

>[AZURE.TIP] Sie können die gleichen Aktionen ausführen, indem Sie auf die Auslassungspunkte neben jedes Element.

Die folgende Tabelle enthält die Aktionen für jede Person zur Verfügung:

| **Entität** | **Aktion** | **Beschreibung** |
| ------ | ------ | ----------- |
| Anwendungstyp | Aufheben der Bereitstellung Typ | Die Cluster Bild Store entfernt das Anwendungspaket. Erfordert alle Applikationen dieses Typs zuerst entfernt werden. |
| Anwendung | Löschen Sie die Anwendung | Löschen der Anwendungs, einschließlich von alle Dienste und deren Status (falls vorhanden) an.  |
| Dienst | Dienst löschen | Löschen Sie den Dienst und Zustand (falls vorhanden). |
| Knoten | Aktivieren | Aktivieren Sie den Knoten. |
|| Deaktivieren (anhalten) | Zeigen Sie den Knoten im aktuellen Zustand. Services weiterhin ausgeführt, jedoch Service Fabric vorausschauende verschiebt nicht alles auf, oder deaktivieren sie es sei denn, sie benötigt wird, um einem Dienstausfall oder Daten Inkonsistenzen zu verhindern. Diese Aktion ist in der Regel verwendet, um aktivieren Debuggen Dienste auf einem bestimmten Knoten, um sicherzustellen, dass während der Prüfung nicht verschoben werden. |
|| Deaktivieren (Neustart) | Verschieben von sicheres alle in-Memory-Dienste einen Knoten deaktiviert, und schließen Sie beständigen Dienste. In der Regel verwendet, wenn den Hostprozesse oder den Computer neu gestartet werden müssen. |
|| Deaktivieren Sie (Entfernen Daten) | Schließen Sie sicheres alle Dienste auf dem Knoten ausgeführt werden, nachdem Sie ausreichend freier Replikate erstellt. In der Regel verwendet, wenn ein Knoten (oder zumindest deren Speicherung) wird dauerhaft aus Commission genommen wird. |
|| Entfernen von Knoten Zustand | Entfernen Sie Kenntnisse auf einem Knoten Replikate aus dem Cluster. In der Regel verwendet, wenn ein bereits ausgefallenen Knoten als nicht behebbarer eingestuft wird. |

Da viele Aktionen destruktive sind, können Sie aufgefordert, Ihrer Absicht zu bestätigen, bevor die Aktion abgeschlossen ist.

>[AZURE.TIP] Jede Aktion, die über den Dienst Fabric Explorer ausgeführt werden kann, kann auch über PowerShell oder eine REST-API Automatisierung aktivieren durchgeführt werden.

Sie können auch Service Fabric Explorer verwenden, um neue Anwendungsinstanzen für einen bestimmten Anwendungstyp und Version erstellen. Wählen Sie in der Strukturansicht den Anwendungstyp aus, und klicken Sie auf den Link **Erstellen app Instanz** neben der Version, die Sie im rechten Bereich möchten.

![Erstellen eine Instanz der Anwendung im Dienst Fabric-Explorer][sfx-create-app-instance]

>[AZURE.NOTE] Anwendungsinstanzen erstellt bis Dienst Fabric Explorer können nicht aktuell parametrisierte werden. Mithilfe von Standardwerten für Parameter werden erstellt.

## <a name="connect-to-a-remote-service-fabric-cluster"></a>Verbinden Sie mit einem remote-Dienst Fabric cluster

Da Service Fabric Explorer webbasierten und im Cluster ausgeführt wird, zugegriffen werden sie über einen beliebigen Browser, solange Sie die Cluster Endpunkt kennen und über ausreichende Berechtigungen zum darauf zugreifen.

### <a name="discover-the-service-fabric-explorer-endpoint-for-a-remote-cluster"></a>Ermitteln des Dienst Fabric Explorer-Endpunkts für einen remote cluster

Um für einen angegebenen Cluster Service Fabric Explorer erreicht haben, zeigen Sie einfach im Browser:

http://&lt;Ihr Cluster Endpunkt&gt;: 19080/Explorer

Die vollständige URL steht auch im Bereich Essentials Cluster des Portals Azure.

### <a name="connect-to-a-secure-cluster"></a>Verbinden Sie mit einem sicheren cluster

Sie können ClientAccess zum Dienst Fabric Cluster entweder mit Zertifikaten oder mit Azure Active Directory (AAD) steuern.

Wenn Sie versuchen, auf einem sicheren Cluster eine Verbindung zum Dienst Fabric Explorer, werden Sie entweder eine Client-Zertifikat oder melden Sie sich mit AAD, je nach Konfiguration des Cluster darstellen erforderlich sein.

## <a name="next-steps"></a>Nächste Schritte

- [Prüfbarkeit (Übersicht)](service-fabric-testability-overview.md)
- [Verwalten von Ihrem Dienst Fabric Applications in Visual Studio](service-fabric-manage-application-in-visual-studio.md)
- [Bereitstellung von Fabric Service-Anwendung mithilfe der PowerShell](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
