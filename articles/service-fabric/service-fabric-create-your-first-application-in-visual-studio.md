<properties
   pageTitle="Erstellen Ihrer erste Fabric Service-Anwendung in Visual Studio | Microsoft Azure"
   description="Erstellen, bereitstellen und Debuggen einer Fabric Service-Anwendung mit Visual Studio"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/26/2016"
   ms.author="ryanwi"/>


# <a name="create-your-first-azure-service-fabric-application"></a>Erstellen Sie Ihrer erste Azure Service Fabric-Anwendung

> [AZURE.SELECTOR]
- [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
- [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)

Der Dienst Fabric SDK enthält ein Add-In für Visual Studio, die Tools und Vorlagen für erstellen, bereitstellen und Debuggen Dienst Fabric Applikationen enthält. In diesem Thema führt Sie durch das Verfahren zum Erstellen der ersten Anwendung in Visual Studio.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen, stellen Sie sicher, dass Sie [Einrichten Ihrer Entwicklungsumgebung](service-fabric-get-started.md)haben.

## <a name="video-walkthrough"></a>Video Exemplarische Vorgehensweise

Das folgende Video führt durch die Schritte in diesem Lernprogramm:

>[AZURE.VIDEO creating-your-first-service-fabric-application-in-visual-studio]

## <a name="create-the-application"></a>Erstellen Sie die Anwendung

Eine Fabric Service-Anwendung kann einen oder mehrere Dienste, jede mit einer bestimmten Rolle in Vorführung der Anwendungsfunktionalität enthalten. Mithilfe der Assistent Neues Projekt können Sie ein Anwendungsprojekt zusammen mit der ersten Dienstprojekt erstellen. Sie können weitere Dienste später hinzufügen.

1. Starten Sie Visual Studio als Administrator an.

2. Klicken Sie auf **Datei > Neues Projekt > Cloud > Service Fabric Anwendung**.

3. Nennen Sie die Anwendung, und klicken Sie auf **OK**.

    ![Dialogfeld "Neues Projekt" in Visual Studio][1]

4. Wählen Sie auf der nächsten Seite **Stateful** als erste Diensttyp in Ihrer Anwendung aufnehmen möchten. Nennen Sie es, und klicken Sie auf **OK**.

    ![Dialogfeld "neuen Service" in Visual Studio][2]

    >[AZURE.NOTE] Weitere Informationen zu den Optionen finden Sie unter [Service Fabric programming Übersicht über das Objektmodell](service-fabric-choose-framework.md).

    Visual Studio erstellt die Anwendungsprojekt und das Dienstprojekt dynamische und zeigt sie im Explorer-Lösung.

    ![Lösung-Explorer nach der Erstellung der dynamische Service-Anwendung][3]

    Projekt für die Anwendung sind keine Code enthalten. Stattdessen verweist es auf einer Gruppe von Projekten Dienst. Darüber hinaus enthält sie drei andere Arten von Inhalten aus:

    - **Veröffentlichen von Profilen**: Verwaltung von Tools Einstellungen für die verschiedenen Umgebungen verwendet wird.

    - **Skripts**: ein PowerShell-Skript für die Bereitstellung von/Upgrade Ihrer Anwendung enthält. Visual Studio verwendet das Skript im Hintergrund von Visual Studio. Das Skript kann auch direkt in der Befehlszeile aufgerufen werden.

    - **Definition der Anwendung**: enthält das Anwendungsmanifest unter *ApplicationPackageRoot*. Verknüpfte Anwendung Parameter-Dateien sind unter *ApplicationParameters*, die die Anwendung definieren und ermöglichen es Ihnen, die speziell für eine bestimmte Umgebung konfigurieren.

    Eine Übersicht über den Inhalt des Projekts Service finden Sie unter [Erste Schritte mit den Diensten von zuverlässigen](service-fabric-reliable-services-quick-start.md).

## <a name="deploy-and-debug-the-application"></a>Bereitstellen und Debuggen der Anwendung

Jetzt, da Sie eine Anwendung haben, versuchen Sie es ausführen.

1. Drücken Sie F5 in Visual Studio bereitstellen die Anwendung für das Debuggen aus.

    >[AZURE.NOTE] Bereitstellen von dauert eine Weile beim ersten als Visual Studio ist für die Entwicklung einen lokalen Cluster erstellen. Ein lokaler Cluster führt den gleichen Plattformcode, den Sie erstellen, klicken Sie auf in einer Umgebung mit mehreren Computern Cluster nur auf einem einzelnen Computer an. Der Status Cluster zeigt im Ausgabefenster Visual Studio.

    Wenn der Cluster fertig ist, erhalten Sie eine Benachrichtigung aus lokalen Cluster System Anwendung im Infobereich Manager im Lieferumfang des SDK enthalten.

    ![Benachrichtigung über Taskleiste zum lokalen Cluster des Systems][4]

2. Einmal schlägt der Anwendung gestartet wird, Visual Studio automatisch die Diagnose-Ereignisanzeige, wo Sie Spur Ausgabe vom Dienst sehen können.

    ![Diagnose Ereignisse-viewer][5]

    Wenn die Dienstvorlage dynamische einfach die Nachrichten anzeigen der Zähler Wert im Erhöhen der `RunAsync` Methode zum MyStatefulService.cs.

3. Erweitern Sie eines der Ereignisse, um weitere Informationen zur, einschließlich des Knotens, in dem der Code ausgeführt wird. In diesem Fall ist es _Node_2, obwohl es auf Ihrem Computer unterscheiden.

    ![Details zum Diagnostic Ereignisse-viewer][6]

    Der lokale Cluster enthält fünf Knoten auf einem einzigen Computer gehostet. Es ähnelt einen Cluster mit fünf Knoten, wo Knoten auf unterschiedlichen Computern sind. Sehen wir uns nach einem Knoten auf dem lokalen Cluster unten, um den Verlust von einem Computer zu reproduzieren, während der Visual Studio-Debugger zur gleichen Zeit nutzen.

    >[AZURE.NOTE] Die Anwendungsereignisse, die durch die Projektvorlage ausgegeben verwenden Sie die darin enthaltenen `ServiceEventSource` Class. Weitere Informationen finden Sie unter [So überwachen und Dienste lokal diagnostizieren](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

4. Project an die Klasse in Ihrem Dienst, Suchen von StatefulService (z. B. MyStatefulService) abgeleitet wird und Einrichten einer fortzuschreiten in der ersten Zeile von der `RunAsync` Methode.

    ![Fortzuschreiten in dynamische Service RunAsync-Methode][7]

5. Mit der rechten Maustaste in der lokale Cluster Manager System über Taskleiste app, und wählen Sie **Lokalen Cluster verwalten** , um Dienst Fabric Explorer zu starten.

    ![Starten Sie Dienst Fabric Explorer aus dem lokalen Cluster-Manager][systray-launch-sfx]

    Dienst Fabric Explorer bietet eine visuelle Darstellung der Cluster – beispielsweise die Gruppe der Anwendung, die es und Festlegen der physischen Knoten, die sie zusammen bilden bereitgestellt. Weitere Informationen zum Dienst Fabric Explorer finden Sie unter [Ihren Cluster Visualisierung](service-fabric-visualizing-your-cluster.md).

6. Erweitern Sie im linken Bereich **Cluster > Knoten** , und suchen Sie den Knoten, in dem der Code ausgeführt wird.

7. Klicken Sie auf **Aktionen > deaktivieren (Neustart)** simulieren einen Computer neu zu starten. (Beachten Sie, dass Sie auch aus dem Kontextmenü in der Listenansicht Knoten im linken Bereich deaktivieren können.)

    ![Beenden eines Knotens im Dienst Fabric-Explorer][sfx-stop-node]

    Vorübergehend auftreten Ihrer fortzuschreiten Treffer in Visual Studio, wie die Berechnung Sie auf einem Knoten nahtlos Aktionen wurden über zu einem anderen ausgeführt.

8. Zurück zu den Diagnoseprotokollen Ereignisse-Viewer, und beachten Sie die Nachrichten. Der Zähler wurde erhöht, Fortsetzung, obwohl die Ereignisse tatsächlich von einem anderen Knoten stammen.

    ![Diagnose Ereignisse Viewer nach failover][diagnostic-events-viewer-detail-post-failover]

## <a name="switch-cluster-mode"></a>Switch-Cluster-Modus

Standardmäßig ist der lokale Entwicklung Cluster konfiguriert als einen Cluster mit 5 Knoten nützlich für das Debuggen von Diensten über mehrere Knoten bereitgestellt wird ausgeführt. Bereitstellen einer Anwendung zum Cluster Entwicklung 5 Knoten kann jedoch einige Zeit dauern. Code Änderungen schnell zu durchlaufen, ohne Ihre app auf 5 Knoten, ausgeführt werden soll, können Sie Cluster Entwicklung in Knoten 1-Modus wechseln. Zum Ausführen von Codes auf einem Cluster mit einem Knoten mit der rechten Maustaste auf den lokalen Cluster-Manager in der Taskleiste, und wählen Sie **1-Knoten-Cluster-Modus wechseln >**.  

![Switch-Cluster-Modus][switch-cluster-mode]

Wenn Sie ändern Clustermodus Entwicklung Cluster werden zurückgesetzt, und alle Programme, die nach der Bereitstellung oder auf dem Cluster ausgeführt werden entfernt.

## <a name="cleaning-up"></a>Bereinigen

  Bevor Sie Nachbereitung, ist es wichtig, denken Sie daran, dass der lokale Cluster sehr realen ist. Beenden des Debuggers entfernt Ihrer Anwendungsinstanz und hebt den Anwendungstyp. Cluster befindet sich jedoch im Hintergrund ausführen. Sie haben mehrere Optionen Cluster verwaltet:

  1. Fahren Sie Cluster, aber behalten Sie die Anwendungsdaten und auf, klicken Sie auf **Lokale Cluster beenden** in der Taskleiste-app System.

  2. Um den Cluster vollständig löschen möchten, klicken Sie auf **Lokale Cluster entfernen** in der Taskleiste-app System. Diese Option führt zu Fehlern in einer anderen langsam Bereitstellung das nächste Mal Sie drücken Sie F5 in Visual Studio. Löschen Sie den Cluster nur, wenn Sie nicht mit dem lokalen Cluster für einige Zeit möchten oder wenn Sie für das Freigeben von Ressourcen benötigen.

## <a name="next-steps"></a>Nächste Schritte

- Informationen Sie zum Erstellen eines [Cluster in Azure](service-fabric-cluster-creation-via-portal.md) oder einem [eigenständigen Cluster unter Windows](service-fabric-cluster-creation-for-windows-server.md).
- Versuchen Sie, einen Dienst mithilfe der [Zuverlässigen Services](service-fabric-reliable-services-quick-start.md) oder [Zuverlässigen Akteuren](service-fabric-reliable-actors-get-started.md) programming Modelle zu erstellen.
- Erfahren Sie, wie Sie Ihre mit dem Internet mit einem [Web-Dienst front-End](service-fabric-add-a-web-frontend.md)-Dienste verfügbar machen können.
- Eine [hands-on-Übung](https://msdnshared.blob.core.windows.net/media/2016/07/SF-Lab-Part-I.docx) durchgehen statusfreien Dienst erstellen, Konfigurieren von Berichten für die Überwachung und Gesundheit und führen Sie eine Aktualisierung der Anwendung.

<!-- Image References -->

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog.png
[2]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
[3]: ./media/service-fabric-create-your-first-application-in-visual-studio/solution-explorer-stateful-service-template.png
[4]: ./media/service-fabric-create-your-first-application-in-visual-studio/local-cluster-manager-notification.png
[5]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer.png
[6]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail.png
[7]: ./media/service-fabric-create-your-first-application-in-visual-studio/runasync-breakpoint.png
[sfx-stop-node]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-deactivate-node.png
[systray-launch-sfx]: ./media/service-fabric-create-your-first-application-in-visual-studio/launch-sfx.png
[diagnostic-events-viewer-detail-post-failover]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail-post-failover.png
[sfe-delete-application]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-delete-application.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
