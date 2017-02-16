<properties
    pageTitle="Mit PowerShell automatisieren Dienst Fabric anwendungsverwaltung | Microsoft Azure"
    description="Bereitstellen, aktualisieren, testen und Entfernen von Applications Dienst Fabric mithilfe der PowerShell."
    services="service-fabric"
    documentationCenter=".net"
    authors="rwike77"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-fabric"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="ryanwi"/>

# <a name="automate-the-application-lifecycle-using-powershell"></a>Automatisieren von Lebenszyklus der Anwendung, die mithilfe der PowerShell

Viele Aspekte des [Lebenszyklus Fabric Service-Anwendung](service-fabric-application-lifecycle.md) können automatisierte werden.  In diesem Artikel veranschaulicht, wie PowerShell verwenden, um allgemeine Aufgaben für die Bereitstellung, Upgrade, entfernen und den Test Azure Service Fabric Applications automatisieren.  Verwaltete und HTTP-APIs für die Verwaltung der app stehen ebenfalls zur Verfügung. [App-Lebenszyklus](service-fabric-application-lifecycle.md) für Weitere Informationen finden Sie unter.  

## <a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie auf die Aufgaben im Artikel verschieben, müssen Sie unbedingt:

+ Machen Sie sich mit den Dienst Fabric Konzepten [Technische Übersicht Dienst Stoffbahn](service-fabric-technical-overview.md)beschrieben.
+ [Installieren der Laufzeit, SDK, und Tools](service-fabric-get-started.md), die auch das **ServiceFabric** PowerShell-Modul installieren.
+ [Ausführung des Skripts PowerShell aktivieren](service-fabric-get-started.md#enable-powershell-script-execution).
+ Starten eines lokalen Clusters an.  Starten Sie ein neues Fenster der PowerShell als Administrator aus, und führen Sie dann das Cluster Setup-Skript, aus dem Ordner SDK:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
+ Bevor Sie eine beliebige PowerShell-Befehlen in diesem Artikel ausführen, zuerst Herstellen einer Verbindung mit dem lokalen Dienst Fabric Cluster [**Verbinden-ServiceFabricCluster**](https://msdn.microsoft.com/library/azure/mt125938.aspx)mit:`Connect-ServiceFabricCluster localhost:19000`
+ Die folgenden Aufgaben erfordern eine Version 1-Anwendung-Paket bereitstellen und einem Version 2 Anwendungspaket für das Upgrade vor. Laden Sie die [ **WordCount** -Beispiel-Anwendung](http://aka.ms/servicefabricsamples) (befindet sich in den Beispielen überfordert). Erstellen Sie und Packen Sie die Anwendung in Visual Studio (Rechtsklick auf **WordCount** auch select- **Paket**). Kopieren Sie das Paket Version 1 in `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` auf `C:\Temp\WordCount`. Kopieren `C:\Temp\WordCount` auf `C:\Temp\WordCountV2`, erstellen die Version 2 Anwendungspakets für das Upgrade vor. Open `C:\Temp\WordCountV2\ApplicationManifest.xml` in einem Text-Editor. Ändern Sie im **ApplicationManifest** Element das Attribut **ApplicationTypeVersion** aus "1.0.0" auf "2.0.0", um die Versionsnummer der app zu aktualisieren. Speichern Sie die geänderte ApplicationManifest.xml-Datei ein.

## <a name="task-deploy-a-service-fabric-application"></a>Aufgabe: Bereitstellen einer Fabric Service-Anwendungs

Nachdem Sie haben erstellt und die Anwendung verpackt (oder das Anwendungspaket heruntergeladen), können Sie die Anwendung in einem lokalen Dienst Fabric Cluster bereitstellen. Die Bereitstellung umfasst das Anwendungspaket hochladen, Registrieren des Anwendungstyps und Instanz der Anwendung zu erstellen. Verwenden Sie die Anweisungen in diesem Abschnitt, um eine neue Anwendung zu einem Cluster bereitzustellen.

### <a name="step-1-upload-the-application-package"></a>Schritt 1: Hochladen der Anwendungspakets
Hochladen der Anwendungspakets auf dem Bild Store wird in einen Speicherort auf interne Dienst Fabric-Komponenten zugreifen können.  Das Anwendungspaket enthält notwendigen Anwendungsmanifest, Dienst Manifeste und Code, Konfiguration und Daten Pakete aus, auf die Anwendung und Dienstinstanzen erstellen.  Der Befehl [**Kopieren-ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125905.aspx) lädt das Paket hoch. Beispiel:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-the-application-type"></a>Schritt 2: Registrieren des Anwendungstyps
Registrieren des Anwendungspakets macht den Anwendungstyp und die Version deklariert im Anwendungsmanifest verfügbar für die Verwendung. Das System liest das Paket hochgeladen, die in Schritt 1, vergewissern Sie sich das Paket (entspricht dem [**Test-ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125950.aspx) lokal ausgeführt), der Inhalt des Pakets verarbeiten und verarbeitete Paket an einem Systemspeicherort internen kopieren.  Führen Sie das [**Register-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) -Cmdlet ein:

```powershell
Register-ServiceFabricApplicationType WordCount
```
Um alle Anwendungstypen im Cluster registriert sehen zu können, führen Sie das Cmdlet " [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/azure/mt125871.aspx) ":

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-the-application-instance"></a>Schritt 3: Erstellen der Anwendungsinstanz
Mit einer beliebigen Anwendung Typversion, die mithilfe des Befehls [**New-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125913.aspx) erfolgreich registriert wurde, kann eine Anwendung instanziiert werden. Der Name der einzelnen Anwendung wird am deklariert, Zeit bereitstellen und muss beginnen Sie mit der **Fabric:** -Schemanamen und für jede Anwendungsinstanz eindeutig sein. Den Namen der Anwendung eingeben und die Anwendung Typversion werden in der Datei **ApplicationManifest.xml** des Anwendungspakets deklariert. Wenn Sie alle Standarddienste im Anwendungsmanifest von den Zielanwendungstyp definiert wurden, werden die zu diesem Zeitpunkt erstellt.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

Der [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx) -Befehl listet alle Anwendungsinstanzen, die erfolgreich, zusammen mit den allgemeinen Status erstellt wurden. Der [**Get-ServiceFabricService**](https://msdn.microsoft.com/library/azure/mt125889.aspx) -Befehl listet alle Instanzen der Dienst, die innerhalb einer bestimmten Anwendungsinstanz erfolgreich erstellt wurden. Standarddienste (falls vorhanden) werden aufgelistet.

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Aufgabe: Aktualisieren einer Fabric Service-Anwendung
Sie können eine zuvor bereitgestellte Fabric Service-Anwendung mit einem aktualisierten Anwendungspaket aktualisieren. Diese Aufgabe die WordCount-Anwendung, die in bereitgestellt wurde aktualisiert "Aufgabe: Bereitstellen eine Fabric Service-Anwendung." Weitere Informationen finden Sie über [Fabric Service-Anwendung zu aktualisieren](service-fabric-application-upgrade.md) .

Um die Punkte in diesem Beispiel einfach zu halten, wurde nur die Versionsnummer der Anwendung im WordCountV2 Anwendung Verpacken in die erforderlichen Komponenten erstellt wurden aktualisiert. Ein weitere realistisches Szenario beinhaltet Ihre Dateien Service, Code, Konfiguration oder Daten aktualisieren und Neuerstellen und Packen der Anwendung mit den aktualisierten Versionsnummern.  

### <a name="step-1-upload-the-updated-application-package"></a>Schritt 1: Hochladen der aktualisierten Anwendung-Pakets
Die WordCount Version 1-Anwendung ist für ein Upgrade bereit. Wenn Sie ein PowerShell-Fenster als Administrator und Typ [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx)öffnen, sehen Sie diese Version 1.0.0 vom Typ WordCount Anwendung bereitgestellt wird.  

Kopieren Sie jetzt das Paket aktualisierten Anwendung zum Dienst Fabric Bild Store (die Anwendungspakete vom Dienst Fabric gespeichert sind). Der Parameter **ApplicationPackagePathInImageStore** informiert Dienst Fabric Stelle, an der sie das Anwendungspaket finden können. Mit dem folgende Befehl kopiert das Anwendungspaket in **WordCountV2** im Speicher Bild:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-the-updated-application-type"></a>Schritt 2: Registrieren des aktualisierten Anwendungstyps
Im nächsten Schritt wird die neue Version der Anwendung mit Fabric Service, registrieren, die mithilfe des Cmdlets [**Register-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) durchgeführt werden können:

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-the-upgrade"></a>Schritt 3: Starten des Upgrades
Verschiedene Upgrade Parameter, Zeitlimit und Gesundheit Kriterien können Upgrades der Anwendung angewendet. Durch die [Anwendung upgrade Parameter](service-fabric-application-upgrade-parameters.md) und [Upgradeprozess](service-fabric-application-upgrade.md) Dokumente Weitere Informationen zu lesen. Alle Dienste und Instanzen sollten _fehlerfrei_ nach dem Upgrade.  Legen Sie die **HealthCheckStableDuration** auf 60 Sekunden (damit die Dienste fehlerfrei für mindestens 20 Sekunden sind, bevor das Upgrade auf die nächste Upgrade Domäne fortgesetzt wird).  Einstellen der **UpgradeDomainTimeout** 1200 Sekunden und die **UpgradeTimeout** auf 3000 Sekunden. Legen Sie schließlich die **UpgradeFailureAction** auf **Zurücksetzen**, dem angefordert wird, dass Fabric Service die Anwendung, um die vorherige Version zurückgesetzt, wenn Fehler bei der Aktualisierung auftreten.

Nun können Sie die Aktualisierung der Anwendung mithilfe des Cmdlets [**Start-ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125975.aspx) :

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Beachten Sie, dass der Name der Anwendung den Namen der Anwendung zuvor bereitgestellte v1.0.0 identisch ist (Fabric: / WordCount). Dienst Fabric verwendet diesen Namen um zu identifizieren, welche Anwendung durchgeführt erste wurde. Wenn Sie die Zeitüberschreitungswerte zu kurz sein festlegen, können eine Timeout-Fehlermeldung auftreten, die das Problem angibt. Finden Sie unter [Behandeln von Anwendungsupgrades](service-fabric-application-upgrade-troubleshooting.md)oder erhöhen Sie die Timeouts.

### <a name="step-4-check-upgrade-progress"></a>Schritt 4: Überprüfen des Aktualisierungsvorgangs
Sie können den Fortschritt Aktualisierung Anwendung mit [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)oder mithilfe des Cmdlets [**Get-ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125988.aspx) überwachen:

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

In wenigen Minuten, das Cmdlet " [Get-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/azure/mt125988.aspx) " zeigt an, dass alle Upgrade Domänen aktualisiert wurden (abgeschlossen).

## <a name="task-test-a-service-fabric-application"></a>Aufgabe: Testen einer Fabric Service-Anwendungs

Zum Schreiben von hoher Qualität Services müssen Entwickler auslösen, unzuverlässigen Infrastruktur Fehlern durch die Stabilität ihrer Dienste testen können. Dienst Fabric erhalten Entwickler die Möglichkeit zum Auslösen Fehlerstrukturanalyse-Aktionen und Dienste in Anwesenheit von Fehlern mithilfe der Chaos und Failover Testszenarien testen.  Lesen Sie weitere Informationen [Prüfbarkeit Übersicht](service-fabric-testability-overview.md) .

### <a name="step-1-run-the-chaos-test-scenario"></a>Schritt 1: Ausführen das Chaos Test-Szenario
Das Chaos Testszenario generiert Fehlern über den gesamten Dienst Fabric Cluster an. Das Szenario komprimiert Fehlern im Allgemeinen über Monate oder Jahre, um ein paar Stunden angezeigt. Die Kombination von überlappende Fehlern mit hoher Fehlerstrukturanalyse Satz findet Ecke Fälle, in denen andernfalls verpasst werden möchten. Im folgende Beispiel wird das Chaos Testszenario 60 Minuten ausgeführt.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-the-failover-test-scenario"></a>Schritt 2: Ausführen das Failover Test-Szenario
Testen Sie das Failover Szenario Ziele einer bestimmten Servicepartition statt der gesamten Cluster, und er bewirkt, dass andere Dienste nicht betroffen. Das Szenario durchläuft eine Abfolge von simulierten Fehlern bei der Validierung Service, während Ihre Geschäftslogik ausgeführt wird. Ein Fehler bei der Validierung Dienst gibt ein Problem, das weiter Untersuchung an. Die Failovertestes hervorruft nur eine Fehlerstrukturanalyse nacheinander im Gegensatz zu dem Chaos Testszenario, das mehreren Fehlern auslösen kann. Im folgenden Beispiel wird die Failovertestes 60 Minuten anhand der Textur: / WordCount/WordCountService-Dienst.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Aufgabe: Entfernen einer Fabric Service-Anwendung
Sie können eine Instanz einer bereitgestellten Anwendung löschen, entfernen den bereitgestellte Anwendungstyp aus dem Cluster und entfernen Sie das Anwendungspaket aus der ImageStore.

### <a name="step-1-remove-an-application-instance"></a>Schritt 1: Entfernen Sie eine Instanz der Anwendung
Wenn eine Instanz der Anwendung nicht mehr benötigt wird, können Sie es dauerhaft mithilfe des Cmdlets [**Entfernen-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125914.aspx) entfernen. Hierdurch werden automatisch auch alle Dienste, die mit der Anwendung, dauerhaft entfernt alle Dienststatus gehören entfernt. Dieser Vorgang kann nicht rückgängig gemacht und Anwendungsstatus nicht wiederhergestellt werden.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-the-application-type"></a>Schritt 2: Aufheben der Registrierung des Anwendungstyps
Wenn Sie eine bestimmte Version ein Anwendungstyp nicht mehr erforderlich ist, wird die Registrierung aufheben Sie mithilfe des Cmdlets [**' Registrierung aufheben '-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125885.aspx) . Aufheben der Registrierung nicht verwendeter Arten frei Speicherplatz durch die Anwendungspaket im Speicher Bild verwendet. Ein Anwendungstyp kann aufgehoben werden, solange keine Programme instanziiert davor oder ausstehend Upgrades der Anwendung darauf verweisen sind.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-the-application-package"></a>Schritt 3: Entfernen des Anwendungspakets
Nachdem der Anwendungstyp aufgehoben wird, kann das Anwendungspaket mithilfe des Cmdlets [**Entfernen-ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt163532.aspx) aus dem Bild Speicher entfernt werden.

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Nächste Schritte
[Lebenszyklus Fabric Service-Anwendung](service-fabric-application-lifecycle.md)

[Bereitstellen einer Anwendung](service-fabric-deploy-remove-applications.md)

[Anwendungsupgrade](service-fabric-application-upgrade.md)

[Azure Service Fabric-cmdlets](https://msdn.microsoft.com/library/azure/mt125965.aspx)

[Azure Service Fabric Prüfbarkeit cmdlets](https://msdn.microsoft.com/library/azure/mt125844.aspx)
