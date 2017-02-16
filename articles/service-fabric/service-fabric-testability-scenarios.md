<properties
   pageTitle="Chaos und Failover Tests | Microsoft Azure"
   description="Verwenden der Dienst Textur testen Chaos Test- und Failover Szenarien zum Auslösen von Fehlern durch, und vergewissern Sie sich die Zuverlässigkeit Ihrer Dienste."
   services="service-fabric"
   documentationCenter=".net"
   authors="motanv"
   manager="rsinha"
   editor="toddabel"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="motanv"/>

# <a name="testability-scenarios"></a>Prüfbarkeit Szenarien
Große verteilte Systeme wie Cloud-Infrastruktur grundsätzlich unzuverlässigen sind. Azure Service-Struktur ermöglicht Entwicklern die auszuführenden auf unzuverlässigen Infrastruktur Dienste schreiben. Zum Schreiben von hoher Qualität Services müssen Entwickler solche unzuverlässigen Infrastruktur zum Testen der Stabilität ihrer Dienste auslösen können.

Fehlerstrukturanalyse Analysis-Dienst bietet Entwickler die Möglichkeit zum Auslösen Fehlerstrukturanalyse-Aktionen, die Dienste in Anwesenheit Fehlern zu testen. Jedoch erhalten zielgerichtete simulierte Fehlern Sie bisher nur. Um das Testen Weitere ergreifen, können die Testszenarien in Dienst Architektur: einen Testanruf Chaos und eine Failovertestes. Diese Szenarios simulieren fortlaufende überlappende Fehlern ordnungsgemäß und fehlerhaftes, über das Cluster über längere Zeit. Nachdem Sie ein Test mit dem Zins und Art von Fehlern konfiguriert ist, kann es über C#-APIs oder PowerShell zum Generieren von Fehlern in Cluster und dem Dienst gestartet werden.

>[AZURE.WARNING] ChaosTestScenario wird durch eine weitere robuste, Service-basierte Chaos ersetzt. Lizenzinformationen finden Sie im Artikel neue [Chaos gesteuert](service-fabric-controlled-chaos.md) , weitere Details.

## <a name="chaos-test"></a>Chaos testen
Das Chaos Szenario generiert Fehlern über den gesamten Dienst Fabric Cluster an. Das Szenario komprimiert Fehlern in Monaten oder Jahren, um ein paar Stunden in der Regel angezeigt. Die Kombination von überlappende Fehlern mit hoher Fehlerstrukturanalyse Rate findet Ausnahmefällen, die andernfalls verloren gehen. Dies führt zu einem erheblichen Verbesserung der Codequalität des Diensts.

### <a name="faults-simulated-in-the-chaos-test"></a>Fehler in der Chaos Test eines simulierten
 - Starten Sie einen Knoten
 - Starten eines Pakets bereitgestellten code
 - Entfernen eines Replikats
 - Starten eines Replikats
 - Verschieben eines primären Replikats (optional)
 - Verschieben einer sekundären Replikat (optional)

Die Teststatistik eines Chaos führt mehrere Iterationen von Fehlern und Cluster Validierungen für die angegebene Zeitspanne an. Der Zeitaufwand für den Cluster konstant und für die Überprüfung erfolgreich ist, lässt sich konfigurieren. Das Szenario schlägt fehl, wenn Sie eine einzelne Setupfehler in Cluster Überprüfung treffen.

Betrachten Sie beispielsweise einen Testanruf für eine Stunde mit bis zu drei gleichzeitige Fehlern ausgeführt. Der Test werden drei Fehlern auslösen, und überprüfen Sie die Integrität Cluster. Der Test durchlaufen im vorherigen Schritt aus, bis Cluster fehlerhaften wird oder 1 Stunde übergibt. Wenn der Cluster in einer beliebigen Iteration fehlerhaft wird, h. nicht innerhalb eines konfigurierten Zeitraums stabilisieren, der Test schlägt fehl, mit Ausnahme. Diese Ausnahme gibt an, dass etwas schiefgelaufen und weitere Untersuchung benötigt.

In ihrer aktuellen Form hervorruft der Fehlerstrukturanalyse Generation-Engine im Chaos Test nur sichere Fehlern durch. Dies bedeutet, dass in Abwesenheit von externen Fehlern, die eine Quorum oder Datenverlust nie stattfindet.

### <a name="important-configuration-options"></a>Konfigurationsoptionen für wichtige
 - **TimeToRun**: Gesamtzeit, die der Test ausgeführt wird, vor dem Erfolg beenden. Der Test kann zuvor anstelle eines Überprüfungsfehler Fertig stellen.
 - **MaxClusterStabilizationTimeout**: Maximum Zeitspanne für den Cluster fehlerfrei vorgesehen ist, bevor der Test fehlschlägt warten. Prüfungen durchgeführt werden, ob der Dienststatus Cluster OK ist, Dienststatus ist in Ordnung, die Größe des Target Replikat festlegen, wird für die Servicepartition erreicht und keine InBuild Replikate vorhanden sind.
 - **MaxConcurrentFaults**: maximale Anzahl von gleichzeitigen Fehlern in jeder Iteration induzierten. Die höhere Zahl, die kürzere der Test, was daher komplexere Failovers und Übergang Kombinationen. Der Test garantiert in Abwesenheit von externen Fehlern nicht vorliegen einer Quorum oder Datenverlust, unabhängig davon, wie hoch diese Konfiguration ist.
 - **EnableMoveReplicaFaults**: aktiviert oder deaktiviert die Fehlern, die das Verschieben von der primären oder sekundären Replikaten verursachen. Diese Fehler sind standardmäßig deaktiviert.
 - **WaitTimeBetweenIterations**: Zeitdauer zwischen Iterationen, d. h. nach einer Funktion Runden von Fehlern und entsprechende Überprüfung warten.

### <a name="how-to-run-the-chaos-test"></a>Wie den Chaos Test ausgeführt
C#-Beispiel

```csharp
using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";

        Console.WriteLine("Starting Chaos Test Scenario...");
        try
        {
            RunChaosTestScenarioAsync(clusterConnection).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Chaos Test Scenario did not complete: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Chaos Test Scenario completed.");
        return 0;
    }

    static async Task RunChaosTestScenarioAsync(string clusterConnection)
    {
        TimeSpan maxClusterStabilizationTimeout = TimeSpan.FromSeconds(180);
        uint maxConcurrentFaults = 3;
        bool enableMoveReplicaFaults = true;

        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);

        // The chaos test scenario should run at least 60 minutes or until it fails.
        TimeSpan timeToRun = TimeSpan.FromMinutes(60);
        ChaosTestScenarioParameters scenarioParameters = new ChaosTestScenarioParameters(
          maxClusterStabilizationTimeout,
          maxConcurrentFaults,
          enableMoveReplicaFaults,
          timeToRun);

        // Other related parameters:
        // Pause between two iterations for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenIterations = TimeSpan.FromSeconds(30);
        // Pause between concurrent actions for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenFaults = TimeSpan.FromSeconds(10);

        // Create the scenario class and execute it asynchronously.
        ChaosTestScenario chaosScenario = new ChaosTestScenario(fabricClient, scenarioParameters);

        try
        {
            await chaosScenario.ExecuteAsync(CancellationToken.None);
        }
        catch (AggregateException ae)
        {
            throw ae.InnerException;
        }
    }
}
```

PowerShell

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```


## <a name="failover-test"></a>Failovertestes

Das Failover-Test-Szenario ist eine Version von dem Chaos Test-Szenario, das Ziel eine bestimmten Servicepartition hat. Sie überprüft die Auswirkung der Failover auf eine bestimmte Servicepartition hinterlassen Sie andere Dienste nicht betroffen. Nachdem sie mit Informationen das Ziel und andere Parameter so konfiguriert ist, wird es als clientseitige Tool, die entweder C#-APIs oder PowerShell wird verwendet, um für eine Servicepartition Fehlern generieren ausgeführt. Das Szenario durchläuft eine Abfolge von Fehlern durch eine simulierten und Überprüfung Service, während Ihre Geschäftslogik für die Seite, die eine Arbeitsbelastung ausgeführt wird. Ein Fehler bei der Validierung Dienst gibt ein Problem, das weiter Untersuchung an.

### <a name="faults-simulated-in-the-failover-test"></a>Fehler in der Failovertestes eines simulierten
- Starten einer bereitgestellten Code-Paket, wo die Partition gehostet wird
- Entfernen einer primären/sekundären Replikat oder statusfreie Instanz
- Neu starten eines sekundären primären Replikats (Wenn eine dauerhaften Service)
- Verschieben eines primären Replikats
- Verschieben einer sekundären Replikat
- Starten Sie die Partition neu.

Die Failovertestes hervorruft einen ausgewählten Fehler und kann die Überprüfung klicken Sie dann auf den Dienst, um sicherzustellen, dass deren Stabilität. Des Failovertestes hervorruft mehreren Fehlern im Chaos Test nur eine Fehlerstrukturanalyse nacheinander im Gegensatz zum möglich. Wenn die Servicepartition nicht innerhalb der konfigurierten Timeout nach jeder Fehlerstrukturanalyse stabilisieren, schlägt fehl. Der Test hervorruft nur sichere Fehlern durch. Dies bedeutet, dass liegen keine externe Fehler, ein Quorum oder Datenverlust nicht ausgeführt wird.

### <a name="important-configuration-options"></a>Konfigurationsoptionen für wichtige
 - **PartitionSelector**: Ansichtsauswahl-Objekt, das die Partition angibt, die als Ziel angegeben werden muss.
 - **TimeToRun**: Gesamtzeit, die vor dem Beenden der Test ausgeführt wird.
 - **MaxServiceStabilizationTimeout**: Maximum Zeitspanne für den Cluster fehlerfrei vorgesehen ist, bevor der Test fehlschlägt warten. Die Prüfungen, die ausgeführt werden, ob der Dienststatus OK ist, wird die Größe des Target Replikat festlegen für alle Partitionen erreicht und keine InBuild Replikate vorhanden sind.
 - **WaitTimeBetweenFaults**: Zeitdauer zwischen jedem Zyklus Fehlerstrukturanalyse und Validierung warten.

### <a name="how-to-run-the-failover-test"></a>Ausführen des Failovertestes

**C#**

```csharp
using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Chaos Test Scenario...");
        try
        {
            RunFailoverTestScenarioAsync(clusterConnection, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Chaos Test Scenario did not complete: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Chaos Test Scenario completed.");
        return 0;
    }

    static async Task RunFailoverTestScenarioAsync(string clusterConnection, Uri serviceName)
    {
        TimeSpan maxServiceStabilizationTimeout = TimeSpan.FromSeconds(180);
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);

        // The chaos test scenario should run at least 60 minutes or until it fails.
        TimeSpan timeToRun = TimeSpan.FromMinutes(60);
        FailoverTestScenarioParameters scenarioParameters = new FailoverTestScenarioParameters(
          randomPartitionSelector,
          timeToRun,
          maxServiceStabilizationTimeout);

        // Other related parameters:
        // Pause between two iterations for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenIterations = TimeSpan.FromSeconds(30);
        // Pause between concurrent actions for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenFaults = TimeSpan.FromSeconds(10);

        // Create the scenario class and execute it asynchronously.
        FailoverTestScenario failoverScenario = new FailoverTestScenario(fabricClient, scenarioParameters);

        try
        {
            await failoverScenario.ExecuteAsync(CancellationToken.None);
        }
        catch (AggregateException ae)
        {
            throw ae.InnerException;
        }
    }
}
```


**PowerShell**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/SampleApp/SampleService"

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindSingleton
```
