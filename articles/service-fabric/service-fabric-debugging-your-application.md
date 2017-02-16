<properties
   pageTitle="Die Anwendung in Visual Studio debuggen | Microsoft Azure"
   description="Verbessern Sie die Zuverlässigkeit und Leistung Ihrer Dienste durch entwickeln und Debuggen sie in Visual Studio auf einem lokalen Entwicklung Cluster an."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/21/2016"
   ms.author="vturecek;mikhegn"/>

# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Debuggen der Fabric Service-Anwendung mit Visual Studio

## <a name="debug-a-local-service-fabric-application"></a>Debuggen einer lokalen Fabric Service-Anwendungs

Sie können Zeit und Geld sparen bereitstellen und Debuggen Ihrer Anwendung Azure Service Fabric in einem lokalen Computer Entwicklung Cluster. Visual Studio kann bereitstellen die Anwendung auf dem lokalen Cluster und verbinden den Debugger automatisch für alle Instanzen der Anwendung.

1. Starten Sie einen lokale Entwicklung Cluster anhand der Schritte im Thema [Einrichten Ihrer Entwicklungsumgebung Dienst Fabric](service-fabric-get-started.md).

2. Drücken Sie **F5,** oder klicken Sie auf **Debuggen** > **Debuggen starten**.

    ![Starten Sie das Debuggen einer Anwendungs][startdebugging]

3. Legen Sie durch Klicken auf Befehle im Menü **Debuggen** Haltepunkte in Ihren Code und Schritt durch die Anwendung.

    > [AZURE.NOTE] Visual Studio fügt an alle Instanzen der Anwendung. Während Sie Code schrittweisen sind, möglicherweise von mehreren Prozessen in gleichzeitigen Sitzungen resultierender Haltepunkte abrufen. Versuchen Sie die Haltepunkte deaktivieren, nachdem sie, indem Sie jede fortzuschreiten auf die Thread-ID bedingte machen oder mithilfe einer Diagnoseprotokollen Ereignisse treffen sind.

4. Das Fenster **Diagnostic Ereignisse** wird automatisch geöffnet, damit diagnostic Ereignisse in Echtzeit angezeigt werden können.

    ![Diagnose Ereignisse in Echtzeit anzeigen][diagnosticevents]

5. Sie können auch im Fenster **Diagnostic Ereignisse** in der Cloud-Explorer öffnen.  Klicken Sie unter **Dienst Fabric**mit der rechten Maustaste in einen beliebigen Knoten, und wählen Sie **Ansicht Streaming Spuren**aus.

    ![Öffnen Sie das Fenster diagnostic Ereignisse][viewdiagnosticevents]

    Wenn Sie Ihre lässt sich auf einen bestimmten Dienst oder eine Anwendung filtern möchten, aktivieren Sie einfach streaming Spuren auf bestimmten Dienst oder einer Anwendung.

6. Die Ereignisse, die angezeigt werden können, die automatisch generierte **ServiceEventSource.cs** -Datei und werden vom Anwendungscode bezeichnet.

    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```

7. Das Fenster **Diagnostic Ereignisse** unterstützt filtern, Anhalten und Prüfen der Ereignisse in Echtzeit an.  Der Filter ist eine einfache Zeichenfolge suchen nach Ereignisnachricht einschließlich deren Inhalt.

    ![Filtern Sie, Anhalten Sie und fortsetzen Sie oder prüfen Sie der Ereignisse in Echtzeit][diagnosticeventsactions]

8. Debuggen von Diensten ist wie für das Debuggen einer anderen Anwendungs. Sie werden normalerweise Haltepunkte über Visual Studio für das Debuggen einfach festlegen. Obwohl zuverlässigen Websitesammlungen in mehreren Knoten repliziert, können sie weiterhin IEnumerable implementieren. Dies bedeutet, dass Sie die Ergebnisse anzeigen in Visual Studio verwenden können beim Debuggen sehen, was Sie innerhalb gespeichert haben. Festlegen Sie eine fortzuschreiten einfach an einer beliebigen Stelle im Code.

    ![Starten Sie das Debuggen einer Anwendungs][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Debuggen einer remote Fabric Service-Anwendungs

Wenn Ihre Fabric Service-Anwendung in einem Dienst Fabric Cluster in Azure ausgeführt werden, können Sie diese direkt in Visual Studio Remote-Debuggen.

> [AZURE.NOTE] Die Funktion erfordert [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) und [Azure SDK für .NET 2.9](https://azure.microsoft.com/downloads/).    

<!-- -->
> [AZURE.WARNING] Remote Debuggen für Test-/Szenarien und nicht zu aufgrund der Einfluss auf die laufenden Applications in Herstellung-Umgebungen verwendet werden soll.

1. Navigieren Sie zu Ihren Cluster in der **Cloud-Explorer**, mit der rechten Maustaste, und wählen Sie **Debuggen aktivieren**

    ![Aktivieren Sie das Debuggen Remoteprozeduraufruf][enableremotedebugging]

    Dadurch wird die Vorgehensweise zum Aktivieren der remote Debuggen Erweiterung auf Ihre Clusterknoten sowie erforderlichen Netzwerkkonfigurationen Projektstart-.

2. Mit der rechten Maustaste Cluster-Knoten in der **Cloud-Explorer**, und wählen Sie **Debugger anfügen**

    ![Fügen Sie debugger][attachdebugger]

3. Klicken Sie im Dialogfeld **an den Prozess anhängen** wählen Sie den Prozess, den Sie debuggen möchten, und klicken Sie auf **Anfügen**

    ![Wählen Sie Prozess][chooseprocess]

    Der Name des Prozesses, die Sie anfügen, möchten gleich den Namen des Assemblynamens Ihrer Service-Projekt.

    Das Debuggen wird auf alle Knoten den Prozess ausführt.
    - In den Fall, in dem Sie einen statusfreien Dienst debuggen, werden alle Instanzen des Diensts auf allen Knoten Teil der Sitzung Debuggen.
    - Wenn Sie einen dynamische Dienst debuggen, werden nur die primäre Kopie einer beliebigen Partition aktiv und können daher da vom Debugger. Wenn während der Sitzung Debuggen primäre Replikat bewegt wird, sind die Verarbeitung die Replikation Teil der Sitzung Debuggen weiterhin.
    - Um nur entsprechenden oder Instanzen von einen bestimmten Dienst abzufangen, können Sie bedingte Haltepunkte Partition oder Instanz nur aufgehoben werden muss.

    ![Bedingte fortzuschreiten][conditionalbreakpoint]

    > [AZURE.NOTE] Wir unterstützen derzeit keine Debuggen von einem Dienst Fabric Cluster mit mehreren Instanzen des Diensts ausführbare gleichnamigen.

4. Nachdem Sie die Anwendung Debuggen beenden möchten, können die remote Debuggen Erweiterung deaktivieren, indem Sie mit der rechten Maustaste in die **Cloud Explorer** Cluster und wählen Sie **Debuggen deaktivieren**

    ![Deaktivieren Sie das Debuggen Remoteprozeduraufruf][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Streaming Spuren von einem remote Clusterknoten

Sie können außerdem zu Spuren Stream direkt von einer remote Cluster-Knoten in Visual Studio. Dieses Feature können Sie zum Stream ETW Verfolgen von Ereignissen, auf einen Dienst Fabric Cluster-Knoten direkt in Visual Studio gefertigt.

> [AZURE.NOTE] Die Funktion erfordert [Service Fabric SDK 2.0](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) und [Azure SDK für .NET 2.9](https://azure.microsoft.com/downloads/).

<!-- -->
> [AZURE.WARNING]Streaming Spuren für Test-/Szenarien und nicht zu aufgrund der Einfluss auf die laufenden Applications in Herstellung-Umgebungen verwendet werden soll.
> In einem Szenario Herstellung sollten Sie verlassen auf Weiterleiten von Ereignissen verwenden Azure-Diagnose.

1. Navigieren Sie zu Ihren Cluster in der **Cloud-Explorer**, mit der rechten Maustaste, und wählen Sie **Streaming Spuren aktivieren**

    ![Aktivieren des remote streaming auf][enablestreamingtraces]

    Dadurch wird die Vorgehensweise zum Aktivieren der streaming auf Erweiterung auf Ihre Clusterknoten sowie erforderlichen Netzwerkkonfigurationen Projektstart-.

2. Erweitern Sie das Element **Knoten** in der **Cloud Explorer**der rechten Maustaste auf den gewünschten streamen Spuren aus, und wählen **Ansicht Streaming auf** Knoten

    ![Remote streaming Spuren anzeigen][viewremotestreamingtraces]

    Wiederholen Sie Schritt 2 für beliebig viele Knoten Spuren von angezeigt werden sollen. Jeder Knoten Stream wird in einem speziellen Fenster angezeigt.

    Sie können nun sehen die Spuren vom Dienst Fabric und Ihrer Dienste ausgegeben. Wenn Sie die Ereignisse aus, um nur eine bestimmte Anwendung anzeigen filtern möchten, geben Sie einfach den Namen der Anwendung im Filter.

    ![Anzeigen von Spuren streaming][viewingstreamingtraces]

4. Sobald Sie streaming Spuren aus Ihrem Cluster fertig sind, können Sie remote streaming auf, indem Sie mit der rechten Maustaste in die **Cloud Explorer** Cluster deaktivieren und wählen Sie **Streaming auf Deaktivieren**

    ![Deaktivieren Sie remote streaming auf][disablestreamingtraces]

## <a name="next-steps"></a>Nächste Schritte

- [Test einen Dienst Fabric-Dienst](service-fabric-testability-overview.md).
- [Verwalten von Ihrem Dienst Fabric Anwendungen in Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
