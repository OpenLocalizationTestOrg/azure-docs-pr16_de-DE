<properties
   pageTitle="Dienst Fabric App Upgrade mithilfe der PowerShell | Microsoft Azure"
   description="In diesem Artikel führt durch die Erfahrung der Bereitstellen einer Fabric Service-Anwendung, ändern den Code und anschließend eine Aktualisierung mithilfe der PowerShell einführen."
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


# <a name="service-fabric-application-upgrade-using-powershell"></a>Dienst Fabric Anwendungsupgrade mithilfe der PowerShell

> [AZURE.SELECTOR]
- [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
- [Visual Studio](service-fabric-application-upgrade-tutorial.md)

<br/>

Die am häufigsten verwendeten, und es wird empfohlen Upgrade der überwachten paralleles Update.  Azure Service-Struktur überwacht die Integrität der zu aktualisierenden Anwendung anhand einer Reihe von Gesundheit Richtlinien. Nachdem Sie eine Domäne aktualisieren (UD) durchgeführt wurde, Dienst Fabric wertet die Anwendung Integrität und setzt den Vorgang mit den nächsten Domäne aktualisieren oder schlägt die Aktualisierung je nach den Richtlinien Gesundheit.

Ein Upgrade überwachte Anwendung kann mithilfe der verwalteten oder systemeigenen APIs, PowerShell oder REST durchgeführt werden. Anweisungen zur Durchführung einer Aktualisierung mit Visual Studio finden Sie unter [Upgrade Ihrer Anwendung mit Visual Studio](service-fabric-application-upgrade-tutorial.md).

Mit Service Fabric überwacht parallele Updates kann der Anwendungsadministrator die Gesundheit Auswertung Richtlinie konfiguriert werden, die Fabric Service verwendet, um festzustellen, ob die Anwendung fehlerfrei ist. Darüber hinaus kann der Administrator konfigurieren die Aktion, die durchgeführt werden, wenn die Auswertung Gesundheit (beispielsweise wie folgt ein automatisches Zurücksetzen.) schlägt fehl Dieser Abschnitt erläutert schrittweise eines überwachten Upgrades für eine den SDK-Beispielen, die PowerShell verwendet.

## <a name="step-1-build-and-deploy-the-visual-objects-sample"></a>Schritt 1: Erstellen und Bereitstellen der Stichprobe visuelle Objekte


Erstellen Sie und veröffentlichen Sie die Anwendung, indem Sie mit der rechten Maustaste auf das Anwendungsprojekt, **VisualObjectsApplication,** und markieren den Befehl **Veröffentlichen** .  Weitere Informationen finden Sie unter [upgrade Lernprogramm Fabric Service-Anwendung](service-fabric-application-upgrade-tutorial.md).  Alternativ können Sie PowerShell verwenden, die Anwendung bereitstellen.

> [AZURE.NOTE] Bevor Sie den Dienst Fabric-Befehlen in PowerShell verwendet werden kann, müssen Sie zunächst mit dem Cluster Herstellen der `Connect-ServiceFabricCluster` Cmdlet. Auf ähnliche Weise wird davon ausgegangen, dass der Cluster bereits auf Ihrem lokalen Computer eingerichtet wurde. Finden Sie im Artikel zum [Einrichten von Ihrem Dienst Fabric Entwicklungsumgebung](service-fabric-get-started.md)aus.

Den PowerShell-Befehl **Kopieren-ServiceFabricApplicationPackage** können Sie nach dem Erstellen des Projekts in Visual Studio um das Anwendungspaket an die ImageStore zu kopieren. Im nächsten Schritt wird die Anwendung der Dienst Fabric Laufzeit mithilfe des Cmdlets **Register-ServiceFabricApplicationPackage** registrieren. Der letzte Schritt wird eine Instanz der Anwendung gestartet wird, mithilfe des Cmdlets **New-ServiceFabricApplication** .  Diese drei Schritte sind analog zur Verwendung von in Visual Studio des Menüelements **Bereitstellen** .

Nun können Sie [Dienst Fabric-Explorer, um den Cluster und die Anwendung anzuzeigen](service-fabric-visualizing-your-cluster.md). Die Anwendung hat einen Webdienst, der zum in Internet Explorer navigiert werden kann, indem Sie Sie in der Adressleiste [http://localhost: 8081/Visualobjects](http://localhost:8081/visualobjects) eingeben.  Einige unverankerte visuelle Objekte verschieben, klicken Sie im Bildschirm sollte angezeigt werden.  **Get-ServiceFabricApplication** können Sie außerdem um den Anwendungsstatus zu überprüfen.

## <a name="step-2-update-the-visual-objects-sample"></a>Schritt 2: Aktualisieren der Stichprobe visuelle Objekte

Sie möglicherweise feststellen, dass mit der Version, die in Schritt 1 bereitgestellt wurde, die visuellen Objekte nicht gedreht werden. Wir aktualisieren diese Anwendung auf eine Stelle, an der die visuellen Objekte auch drehen.

Wählen Sie das Projekt VisualObjects.ActorService innerhalb der VisualObjects Lösung, und öffnen Sie die Datei StatefulVisualObjectActor.cs. In dieser Datei, navigieren Sie zu der Methode `MoveObject`, kommentieren `this.State.Move()`, und entfernen Sie die Kommentarzeichen `this.State.Move(true)`. Diese Änderung drehen Sie die Objekte nach der Dienst durchgeführt wurde.

Wir müssen außerdem die Datei *ServiceManifest.xml* (unter "PackageRoot") des Projekts **VisualObjects.ActorService**zu aktualisieren. Aktualisieren Sie die *CodePackage* und die Service-Version 2.0, und die zugehörigen Zeilen in der Datei *ServiceManifest.xml* ein.
Die Option Visual Studio *Manifest Dateien bearbeiten* können, nachdem Sie mit der rechten auf die Lösung Maustaste, um die Manifestdatei Änderungen vorzunehmen.


Nachdem Sie die Änderungen vorgenommen werden, sollte das Manifest wie folgt aussehen (hervorgehobenen Bereiche zeigen die Änderungen):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Jetzt wird die *ApplicationManifest.xml* -Datei (finden Sie unter dem Projekt **VisualObjects** unter die Lösung **VisualObjects** ) auf Version 2.0 des Projekts **VisualObjects.ActorService** aktualisiert. Darüber hinaus wird die Version der Anwendung von 1.0.0.0 auf 2.0.0.0 aktualisiert. Die *ApplicationManifest.xml* sollte wie im folgenden Codeausschnitt aussehen:

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```


Erstellen Sie nun das Projekt, indem nur das Projekt **ActorService** , und klicken Sie dann mit der rechten Maustaste auswählen und die Option **Erstellen** in Visual Studio. Wenn Sie **Alles neu erstellen**ausgewählt haben, sollten Sie die Versionen für alle Projekte, da der Code geändert haben, würde aktualisieren. Nächste, lassen Sie uns Packen die aktualisierte Anwendung starten, indem Sie mit der rechten Maustaste auf ***VisualObjectsApplication***, wählen im Menü Service Fabric und wählen Sie **Paket**. Diese Aktion erstellt eine Anwendungspaket, das bereitgestellt werden kann.  Ihre aktualisierte Anwendung ist jetzt bereitgestellt werden.


## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>Schritt 3: Bestimmen der Richtlinien Gesundheit und Durchführen eines Upgrades Parameter

Vertrautmachen Sie mit der [Anwendung upgrade Parameter](service-fabric-application-upgrade-parameters.md) und der [Upgradeprozess](service-fabric-application-upgrade.md) zum Abrufen eines umfassenden Überblick über die verschiedenen Upgrade Parameter, Timeouts und Gesundheit Kriterium angewendet. Für diese exemplarische Vorgehensweise ist das Dienst Gesundheit Auswertung Kriterium auf den Standardwert festgelegt (und empfohlen) Werte, was bedeutet, dass alle Dienste und Instanzen _fehlerfrei_ nach dem Upgrade werden soll.  

Lassen Sie uns erhöhen jedoch die *HealthCheckStableDuration* auf 60 Sekunden (damit die Dienste fehlerfrei für mindestens 20 Sekunden sind, bevor das Upgrade auf die nächste Update Domäne fortgesetzt wird).  Lassen Sie uns Einstellen der *UpgradeDomainTimeout* 1200 Sekunden sein und die *UpgradeTimeout* 3000 Sekunden sein.

Lassen Sie uns auch legen Sie abschließend die *UpgradeFailureAction* rückgängig gemacht. Diese Option erfordert Service-Struktur, bis die Anwendung auf die vorherige Version zurücksetzen zu machen, falls während des Upgrades Probleme auftreten. Auf diese Weise beim Starten des Upgrades (in Schritt 4), werden die folgenden Parameter angegeben:

FailureAction = zurücksetzen

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000


## <a name="step-4-prepare-application-for-upgrade"></a>Schritt 4: Vorbereiten der Anwendung für das Upgrade vor

Nun ist die Anwendung erstellt und aktualisiert werden kann. Wenn Sie als Administrator an, und geben Sie **Get-ServiceFabricApplication**um ein PowerShell-Fenster zu öffnen, sollte es informiert Sie darüber, dass es Anwendungstyp 1.0.0.0 der **VisualObjects** ist, das bereitgestellt wurde, ist.  

Klicken Sie unter den folgenden relativen Pfad ist das Anwendungspaket gespeichert, wo Sie den Dienst Fabric SDK - *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*nicht komprimiert. Sie sollten einen Ordner "Paket" in dem Verzeichnis, suchen das Anwendungspaket gespeichert ist. Überprüfen Sie die Zeitstempel, um sicherzustellen, dass es auf den neuesten Stand ist (möglicherweise müssen die Pfade entsprechend auch ändern).

Jetzt aktualisierte Anwendungspaket uns an den Dienst Fabric ImageStore (die Anwendungspakete Speicherorte nach Fabric Service) zu kopieren. Der Parameter *ApplicationPackagePathInImageStore* Hinweis Dienst Fabric, dass das Anwendungspaket zur Hand haben. Wir haben die aktualisierte Anwendung setzen Sie in "VisualObjects\_Version 2" mit den folgenden Befehl aus (möglicherweise müssen zum Ändern von Pfaden wieder ordnungsgemäß).

```powershell
Copy-ServiceFabricApplicationPackage  -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package
-ImageStoreConnectionString fabric:ImageStore   -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

Im nächsten Schritt wird in dieser Anwendung mit Service-Fabric zu erfassen, das mit dem folgenden Befehl ausgeführt werden kann:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Wenn der Befehl nicht erfolgreich, ist es wahrscheinlich benötigen Sie alle Dienste neu zu erstellen. Wie in Schritt2 angegeben ist, müssen Sie möglicherweise auch Ihre Webdienst-Version zu aktualisieren.

## <a name="step-5-start-the-application-upgrade"></a>Schritt 5: Starten Sie die Aktualisierung der Anwendung

Jetzt sind wir schon so starten Sie die Anwendung Aktualisierung mit den folgenden Befehl aus:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


Der Name der Anwendung ist die gleiche wie in der Datei *ApplicationManifest.xml* beschrieben wurde. Dienst Fabric verwendet diesen Namen um zu identifizieren, welche Anwendung durchgeführt erste wurde. Wenn Sie die Zeitüberschreitungswerte zu kurz sein festlegen, können Sie eine Fehlermeldung auftreten, die das Problem angibt. Finden Sie im Abschnitt zur Problembehandlung, oder erhöhen Sie die Timeouts.

Jetzt die Anwendung Upgrade Erträge, können Sie es mit dem Dienst Fabric Explorer überwachen oder mithilfe der folgenden PowerShell Befehl: **Get-ServiceFabricApplicationUpgrade Fabric: / VisualObjects**.

In wenigen Minuten der Status, die Sie haben mithilfe des vorhergehenden PowerShell-Befehls sollte besagen: alle Update-Domänen aktualisiert wurden (abgeschlossen). Und finden Sie, dass die visuellen Objekte im Browserfenster gestartet haben drehen!

Sie können das Upgrade von Version 2 zu Version 3 oder von Version 2 Version 1 als Übung versuchen. Verschieben von Version 2 zu Version 1 gilt auch ein Upgrade. Experimentieren Sie mit Timeouts und Gesundheit Richtlinien, um sich mit ihnen vertraut machen möchten. Wenn Sie zu einem Azure Cluster bereitstellen, müssen die Parameter ordnungsgemäß festgelegt werden. Es ist sinnvoll, um die Zeitüberschreitungswerte konservativ festlegen.


## <a name="next-steps"></a>Nächste Schritte

[Aktualisieren Ihrer Anwendung mit Visual Studio](service-fabric-application-upgrade-tutorial.md) führt Sie durch eine Anwendung Aktualisierung mit Visual Studio.

Steuern Sie, wie eine Anwendung upgrades mithilfe von [upgrade-Parameter](service-fabric-application-upgrade-parameters.md).

Machen Sie Ihre Upgrades der Anwendung kompatibel, indem Sie lernen, wie Sie [Datenserialisierung](service-fabric-application-upgrade-data-serialization.md)verwenden.

Informationen Sie zum erweiterten Funktionen, die während des Upgrades Ihrer Anwendungs von Verweisen auf [Erweiterte Themen](service-fabric-application-upgrade-advanced.md)verwenden.

Beheben von häufig auftretenden Problemen in Upgrades der Anwendung anhand der Schritte in der [Problembehandlung Anwendungsupgrades](service-fabric-application-upgrade-troubleshooting.md).
