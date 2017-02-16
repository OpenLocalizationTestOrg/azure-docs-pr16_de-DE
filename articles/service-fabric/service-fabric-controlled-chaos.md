<properties
   pageTitle="Auslösen von Chaos in Dienst Fabric Cluster | Microsoft Azure"
   description="Verwenden von Fehlerinjektion und Cluster Analysis Service-APIs Chaos im Cluster verwalten."
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
   ms.date="09/19/2016"
   ms.author="motanv"/>

# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Auslösen Sie gesteuert Chaos in Dienst Fabric Cluster
Umfangreiche verteilte Systeme wie Cloud-Infrastruktur grundsätzlich unzuverlässigen sind. Azure Service-Struktur ermöglicht Entwicklern zuverlässigen Dienste auf einer unzuverlässigen Infrastruktur schreiben. Zum Schreiben von robuste Services müssen Entwickler Fehlern gegen solche unzuverlässigen Infrastruktur zum Testen der Stabilität ihrer Dienste auslösen können.

Die Fehlerinjektion und Cluster Analysis Services (auch bekannt als die Fehlerstrukturanalyse Analysis Services) bietet Entwickler die Möglichkeit zum Auslösen Fehlerstrukturanalyse Aktionen Dienste zu testen. Sie erhalten aber zielgerichtete simulierte Fehlern nur so weit. Um die Tests Weitere ergreifen, können Sie Chaos.

Chaos simuliert fortlaufende, überlappende Fehlern (sicheres und fehlerhaftes) über das Cluster über längere Zeit. Nachdem Sie mit der Zinssatz sowie die Art von Fehlern Chaos konfiguriert haben, können Sie starten und beenden es über C#-APIs oder PowerShell Fehlern in Cluster und dem Dienst generiert werden.

Während der Ausführung Chaos werden verschiedene Ereignisse, die den Status der Ausführung im Moment erfassen. Beispielsweise enthält eine ExecutingFaultsEvent allen Fehlern, die in der jeweiligen Iteration ausgeführt werden. Eine ValidationFailedEvent enthält die Details eines Fehlers, das während der Validierung Cluster gefunden wurde. Rufen Sie die GetChaosReportAsync-API, um den Bericht von Chaos ausgeführt zu erhalten.

## <a name="faults-induced-in-chaos"></a>Fehler in Chaos induzierten
Chaos Fehlern über den gesamten Dienst Fabric Cluster generiert und komprimiert Fehlern, die in Monaten oder Jahren in ein paar Stunden angezeigt werden. Die Kombination von überlappende Fehlern mit hoher Fehlerstrukturanalyse Rate findet Ecke Fälle, die andernfalls verloren gehen. Dieser Übung Chaos führt zu einem erheblichen Verbesserung der Codequalität des Diensts.

Chaos hervorruft Fehlern aus den folgenden Kategorien:

 - Starten Sie einen Knoten
 - Starten eines Pakets bereitgestellten code
 - Entfernen eines Replikats
 - Starten eines Replikats
 - Verschieben eines primären Replikats (konfigurierbare)
 - Verschieben einer sekundären Replikat (konfigurierbare)

Mehrere Iterationen Chaos wird ausgeführt. Jede Iteration besteht aus Fehlern und Cluster Überprüfung für die angegebene Periode zurück. Sie können den Zeitaufwand für den Cluster konstant und für die Überprüfung erfolgreich konfigurieren. Wenn ein Fehler bei der Validierung Cluster gefunden wird, wird Chaos generiert und weiterhin besteht eine ValidationFailedEvent mit der UTC-Zeitstempel und der Fehlerdetails für.

Betrachten Sie beispielsweise eine Instanz der Chaos, die in eine Stunde mit bis zu drei gleichzeitige Fehlern ausgeführt festgelegt werden. Chaos hervorruft drei Fehlern und überprüft dann die Integrität Cluster. Durchläuft Sie im vorherigen Schritt, bis er explizit beendet über die StopChaosAsync-API oder 1 Stunde wird übergibt. Wenn der Cluster in einer beliebigen Iteration fehlerhaften wird (d. h., es ist nicht stabilisieren innerhalb eines konfigurierten Zeitraums), Chaos generiert eine ValidationFailedEvent. Dieses Ereignis gibt an, dass etwas schiefgelaufen und unter Weitere Untersuchung Umständen.

In ihrer aktuellen Form hervorruft Chaos nur sichere Fehlern durch. Dies bedeutet, dass keine externen Fehler, Quorum Verlust oder Datenverlust nicht eintritt.

## <a name="important-configuration-options"></a>Konfigurationsoptionen für wichtige
 - **TimeToRun**: Gesamtzeit dieser Chaos ausgeführt, bevor sie mit Erfolg abgeschlossen wurde. Sie können Chaos beenden, bevor er für den Zeitraum TimeToRun über die StopChaos-API ausgeführt wurde.
 - **MaxClusterStabilizationTimeout**: die maximale Zeitspanne für den Cluster fehlerfrei vorgesehen ist vor dem erneuten Überprüfen warten. Diese warten besteht darin, Einsatz entlasten Cluster gedrückt, während sie wiederhergestellt wird. Die Prüfungen, die ausgeführt werden:
    - Wenn die Cluster Integrität zulässig
    - Wenn der Dienststatus zulässig
    - Wenn die Zieldatenbank Größe festgelegt wird für die Servicepartition erreicht.
    - Dass keine InBuild Replikate vorhanden sind.
 - **MaxConcurrentFaults**: die maximale Anzahl von gleichzeitigen Fehlern, die in jeder Iteration induzierten sind. Höhere Ordnung in die Nummer, die kürzere ist. Dadurch komplexere Failovers und Übergang Kombinationen. Chaos wird sichergestellt, dass keine externen Fehler, es ist kein Quorum Verlust oder Datenverlust, unabhängig davon, wie hoch ein Wert dieser Konfiguration hat.
 - **EnableMoveReplicaFaults**: aktiviert oder deaktiviert die Fehlern, die dazu führen, die primäre oder sekundäre Replikate dass zu verschieben. Diese Fehler sind standardmäßig deaktiviert.
 - **WaitTimeBetweenIterations**: die Zeitdauer zwischen Iterationen, d. h., nach dem eine Funktion Runden von Fehlern und entsprechende Überprüfung warten.
 - **WaitTimeBetweenFaults**: die Zeitdauer zwischen zwei aufeinander folgenden Fehlern in einer Iteration warten.

## <a name="how-to-run-chaos"></a>Ausführen von Chaos
**C#:**

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```
**PowerShell:**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```
