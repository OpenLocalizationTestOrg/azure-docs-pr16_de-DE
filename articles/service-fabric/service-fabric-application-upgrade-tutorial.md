 <properties
   pageTitle="Dienst Fabric app Upgrade Lernprogramm | Microsoft Azure"
   description="In diesem Artikel durchläuft zur Darstellung von Bereitstellen einer Fabric Service-Anwendung, ändern den Code und anschließend ein Upgrade mit Visual Studio einführen."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>


# <a name="service-fabric-application-upgrade-tutorial-using-visual-studio"></a>Dienst Fabric Anwendung Upgrade Lernprogramm verwenden Visual Studio

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
- [Visual Studio](service-fabric-application-upgrade-tutorial.md)

<br/>

Azure Service-Struktur vereinfacht das Aktualisieren einer Cloud-Anwendung durch, um sicherzustellen, dass nur geänderte Services aktualisiert werden und Anwendung Zustand während der Aktualisierung überwacht wird. Es setzt auch automatisch wieder die Anwendung auf die vorherige Version ausführt, wenn Probleme. Dienst Fabric Anwendungsupgrades sind *Ausfallzeiten 0 (null)*, da die Anwendung ohne Ausfallzeiten aktualisiert werden kann. In diesem Lernprogramm beschrieben, wie Sie ein paralleles Update von Visual Studio ausführen.


## <a name="step-1-build-and-publish-the-visual-objects-sample"></a>Schritt 1: Erstellen und Veröffentlichen der Stichprobe visuelle Objekte

Laden Sie zunächst die Anwendung [Visuelle Objekte](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Actors/VisualObjects) von GitHub. Anschließend erstellen Sie und veröffentlichen Sie die Anwendung indem Sie mit der rechten Maustaste auf die Anwendung Project **VisualObjects**, und der Befehl " **Veröffentlichen** " in das Menüelement Fabric Dienst auswählen.

![Kontextmenü für eine Fabric Service-Anwendung][image1]

Ein Popup bringt **Veröffentlichen** auswählen, und Sie können das **Zielprofil** auf **PublishProfiles\Local.xml**festlegen. Das Fenster sollte wie folgt aussehen, bevor Sie klicken Sie auf **Veröffentlichen**.

![Veröffentlichen einer Fabric Service-Anwendungs][image2]

Jetzt können Sie **Veröffentlichen** im Dialogfeld klicken. [Dienst Fabric-Explorer, um den Cluster und die Anwendung anzeigen](service-fabric-visualizing-your-cluster.md)können. Die Anwendung visuelle Objekte enthält einen Webdienst, den Sie zu wechseln, indem Sie in der Adressleiste des Browsers eingeben [Http://localhost:8082/Visualobjects /](http://localhost:8082/visualobjects/) .  10 unverankerter visuellen Objekte, die auf dem Bildschirm bewegen sollte angezeigt werden.

## <a name="step-2-update-the-visual-objects-sample"></a>Schritt 2: Aktualisieren der Stichprobe visuelle Objekte

Sie möglicherweise feststellen, dass mit der Version, die in Schritt 1 bereitgestellt wurde, die visuellen Objekte nicht gedreht werden. Wir aktualisieren diese Anwendung auf eine Stelle, an der die visuellen Objekte auch drehen.

Wählen Sie das Projekt VisualObjects.ActorService in der VisualObjects Lösung, und öffnen Sie die Datei **VisualObjectActor.cs** . In dieser Datei, wechseln Sie zu der Methode `MoveObject`, kommentieren `visualObject.Move(false)`, und entfernen Sie die Kommentarzeichen `visualObject.Move(true)`. Diese Änderung Code dreht die Objekte nach der Dienst durchgeführt wurde.  **Jetzt ganz (neu nicht erstellen) die Lösung**, der die geänderten Projekte erstellt. Wenn Sie *Alles neu erstellen*ausgewählt haben, müssen Sie die Versionen für alle Projekte zu aktualisieren.

Wir benötigen außerdem Version unserer Anwendung. Wenn Sie die Version Änderungen vornehmen möchten, nachdem Sie mit der rechten auf das Projekt **VisualObjects Maustaste** , können Sie die Option Visual Studio **Manifest Versionen bearbeiten** . Durch Auswahl dieser Option Öffnet das Dialogfeld für Edition Versionen wie folgt:

![Klicken Sie im Dialogfeld versionsverwaltung][image3]

Aktualisieren Sie die Versionen für die geänderte Projekte und Code Softwarepaketen, zusammen mit der Anwendung Version 2.0.0. Nachdem Sie die Änderungen vorgenommen werden, sollte das Manifest wie folgt aussehen (fett Bereiche zeigen die Änderungen):

![Aktualisieren von Versionen][image4]

Visual Studio-Tools ausführen können automatischen Rollups Versionen nach Auswahl der **Anwendung und Dienstversionen automatisch zu aktualisieren**. Wenn Sie [SemVer](http://www.semver.org)verwenden, müssen Sie die Code und/oder Konfiguration Version des Pakets alleine zu aktualisieren, wenn diese Option ausgewählt ist.

Speichern Sie die Änderungen zu, und aktivieren Sie das **Upgrade der Anwendungs** nun.


## <a name="step-3--upgrade-your-application"></a>Schritt 3: Aktualisieren Sie Ihrer Anwendung

Vertrautmachen Sie mit der [Anwendung aktualisieren Parameter](service-fabric-application-upgrade-parameters.md) und der [Upgradeprozess](service-fabric-application-upgrade.md) auf erhalten einen umfassenden Überblick über die verschiedenen Upgrade Parameter, Timeouts und Gesundheit Kriterium, die angewendet werden kann. Für diese exemplarische Vorgehensweise wird das Dienst Gesundheit Auswertung Kriterium auf Standard (nicht überwacht Modus) festgelegt. Sie können diese Einstellungen konfigurieren, indem Sie **Upgradeeinstellungen konfigurieren** , und klicken Sie dann die Parameter nach Bedarf ändern.

Jetzt werden wir alle festlegen, um die Aktualisierung der Anwendung zu starten, indem Sie **Veröffentlichen**auswählen. Diese Option upgrades Ihrer Anwendung Version 2.0.0, in der die Objekte drehen. Dienst Fabric upgrades ein Update Domain nacheinander (einige Objekte werden zunächst aktualisiert gefolgt von anderen), und der Dienst weiterhin während des Upgrades zugegriffen werden. Zugriff auf den Dienst kann über Ihren Client (Browser) geprüft werden soll.  


Jetzt, wie die Aktualisierung der Anwendung durchgeführt werden können, können Sie es mit Service Fabric-Explorer, Überwachen mithilfe der Registerkarte **Aktualisierungen wird ausgeführt** , unter der Applications.

In wenigen Minuten alle aktualisieren Domänen (abgeschlossen) aktualisiert werden soll, und im Ausgabefenster Visual Studio sollten auch besagen, dass die Aktualisierung abgeschlossen ist. Und Sie sollten die suchen *Alle* , die die visuellen Objekte im Browserfenster jetzt drehen sind!

Sie versuchen, die Versionen ändern möchten, und Verschieben von Version 2.0.0 Version 3.0.0 als Übung oder sogar von Version 2.0.0 zurück zur Version 1.0.0. Experimentieren Sie mit Timeouts und Gesundheit Richtlinien, um sich mit ihnen vertraut machen möchten. Wenn Sie zu einem Azure Cluster im Gegensatz zu einem lokalen Cluster bereitstellen, möglicherweise die verwendeten Parameter unterschiedlich sein. Es empfiehlt sich, dass Sie die Zeitüberschreitungswerte konservativ festlegen.


## <a name="next-steps"></a>Nächste Schritte

[Aktualisieren Ihrer Anwendung mithilfe der PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) führt Sie durch eine Anwendung Aktualisierung mithilfe der PowerShell.

Steuern Sie, wie eine Anwendung mithilfe von [Parametern upgrade](service-fabric-application-upgrade-parameters.md)durchgeführt wurde.

Machen Sie Ihre Upgrades der Anwendung kompatibel, indem Sie lernen, wie Sie [Datenserialisierung](service-fabric-application-upgrade-data-serialization.md)verwenden.

Informationen Sie zum erweiterten Funktionen, die während des Upgrades Ihrer Anwendungs von Verweisen auf [Erweiterte Themen](service-fabric-application-upgrade-advanced.md)verwenden.

Beheben von häufig auftretenden Problemen in Upgrades der Anwendung anhand der Schritte in der [Problembehandlung Anwendungsupgrades](service-fabric-application-upgrade-troubleshooting.md).



[image1]: media/service-fabric-application-upgrade-tutorial/upgrade7.png
[image2]: media/service-fabric-application-upgrade-tutorial/upgrade1.png
[image3]: media/service-fabric-application-upgrade-tutorial/upgrade5.png
[image4]: media/service-fabric-application-upgrade-tutorial/upgrade6.png
