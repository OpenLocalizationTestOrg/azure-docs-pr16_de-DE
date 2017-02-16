<properties
   pageTitle="So Datenverlust auf Dienst Fabric Services aufrufen | Microsoft Azure"
   description="Beschreibt, wie der Datenverlust api"
   services="service-fabric"
   documentationCenter=".net"
   authors="LMWF"
   manager="rsinha"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="lemai"/>
   
# <a name="how-to-invoke-data-loss-on-services"></a>So rufen Sie Daten verloren gehen, klicken Sie auf Dienste

>[AZURE.WARNING] Dieses Dokument beschrieben, wie Sie Daten verloren in Ihrer Dienste und sollte mit Vorsicht verwendet werden.

## <a name="introduction"></a>Einführung
Sie können Datenverlust bei einem Teil der Dienst Fabric Service durch einen StartPartitionDataLossAsync() aufrufen.  Diese api verwendet die Fehlerinjektion und Analysis Services für die Arbeitsschritte zum Datenverlust Umständen auftreten.

## <a name="using-the-fault-injection-and-analysis-service"></a>Verwendung der Fehlerinjektion und Analysis Services

Die Fehlerinjektion und Analysis Services unterstützt derzeit die folgenden APIs in der nachstehenden Tabelle ein.  Rechts neben dem Diagramm zeigt das entsprechende PowerShell-Cmdlet.  Näheres Msdn-Dokumentation für jede API für Weitere Informationen zu einzelnen.

|           C#-API                    |         PowerShell-Cmdlets                      |
|-------------------------------------|-----------------------------------------------:|
|[StartPartitionDataLossAsync] [dl]   |[Start-ServiceFabricPartitionDataLoss] [psdl]   |
|[StartPartitionQuorumLossAsync] [ql] |[Start-ServiceFabricPartitionQuorumLoss] [psql] |
|[StartPartitionRestartAsync] [rp]    |[Start-ServiceFabricPartitionRestart] [psrp]    |

## <a name="conceptual-overview-of-running-a-command"></a>Konzeptionelle Übersicht über einen Befehl ausführen

Die Fehlerinjektion und Analysis Services verwendet ein asynchrones Modell, in dem Sie den Befehl mit einem API, als die API "Start" in diesem Dokument bezeichnet, und dann überprüft den Fortschritt diesen Befehl mit einem "" GetProgress "" API, bis einen terminal Zustand erreicht ist oder bis Sie sie stornieren.
Rufen Sie die API "Start" für die entsprechende-API, um einen Befehl zu starten.  Diese API gibt, wann die Fehlerinjektion und Analysis Services hat die Anfrage angenommen.  Jedoch, es gibt keine wie weit ein Befehl ausgeführt wurde, oder selbst wenn sie noch nicht begonnen hat.  Akzeptieren, um den Fortschritt eines Befehls zu überprüfen, rufen Sie die "" GetProgress "" API, das die "Start"-API, die zuvor Namens entspricht.  Die "" GetProgress "" API gibt Objekt gibt den aktuellen Status des Befehls in seine Eigenschaft Zustand zurück.  Ein Befehl endlos bis ausgeführt wird:

1.  Es wird erfolgreich abgeschlossen.  Wenn Sie in diesem Fall ""GetProgress "" daran aufrufen, wird den Fortschritt Zustand des Objekts abgeschlossen.
2.  Es wird einen schwerwiegenden Fehler auftritt.  Wenn Sie in diesem Fall ""GetProgress "" daran aufrufen, wird der Fortschritt Zustand des Objekts fehlerhaft werden
3.  Sie über die [CancelTestCommandAsync] Abbrechen [ cancel] API oder [Beenden-ServiceFabricTestCommand]  [ cancelps] PowerShell-Cmdlet.  Wenn Sie in diesem Fall ""GetProgress "" daran aufrufen, werden den Fortschritt Zustand des Objekts entweder storniert oder ForceCancelled, je nach Argument für die API.  Finden Sie in der Dokumentation [CancelTestCommandAsync]  [ cancel] für weitere Details.


## <a name="details-of-running-a-command"></a>Details einen Befehl ausführen

Rufen Sie akzeptieren, um einen Befehl zu beginnen, die API beginnen mit der erwarteten Argumente ein.  Alle-APIs verfügen über ein Guid Argument mit dem Namen OperationId.  Sie sollten das Argument OperationId verfolgen, da sie zum Überwachen dieser Befehl verwendet wird.  Dies muss in der "" GetProgress"" API akzeptieren, um den Befehl überwachen übergeben.  Die OperationId muss eindeutig sein.

Nach dem erfolgreichen Aufrufen der Start-API, sollte der API "GetProgress" in einer Schleife aufgerufen werden erst nach Abschluss des Objekts zurückgegebenen Fortschritt State-Eigenschaft.  Alle [FabricTransientException des]  [ fte] und OperationCanceledExceptions wiederholt werden soll.
Der Befehl mit einen terminal Zustand (erledigt, Faulted oder abgebrochen) erreicht hat, wird die zurückgegebene Fortschritt des Objekts Result-Eigenschaft zusätzliche Informationen angezeigt.  Wenn der Status abgeschlossen ist, wird Result.SelectedPartition.PartitionId die Partitions-Id enthalten, die ausgewählt wurden.  Result.Exception werden null.  Wenn der Status fehlerhaft ist, müssen Result.Exception den Grund auf das Fehlerstrukturanalyse einfügen und Analysis Services fehlerhaft den Befehl.  Result.SelectedPartition.PartitionId haben die Partitions-Id, die ausgewählt wurde.  In einigen Fällen der Befehl möglicherweise nicht weit genug die Zeichenfolge haben, eine Partition auszuwählen.  In diesem Fall wird der PartitionId 0 sein.  Wenn der Status abgebrochen wird, werden Result.Exception null.  Wie der Fall Faulted Result.SelectedPartition.PartitionId haben die Partitions-Id, die ausgewählt wurde, aber wenn der Befehl dazu nicht weit genug verlaufen ist, wird es 0 sein.  Sehen Sie auch unter dem folgenden Beispiel.

Der folgende Beispielcode veranschaulicht, wie starten, und klicken Sie dann auf einen Befehl zum Datenverlust auf einer bestimmten Partition Fortschritt überprüfen.

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // The id of the target partition inside the target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.        

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                // In a terminal state .Result.SelectedPartition.PartitionId will have the chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}'", operationId, progress.Result.Exception);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(5.0d)).ConfigureAwait(false);
        }
    }
```

Im folgenden Beispiel wird gezeigt, wie mithilfe der PartitionSelector eine zufällige Teil einer angegebenen Dienst ausgewählt werden:

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have the Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }
            catch (Exception e)
            {
                Console.WriteLine("Unexpected exception '{0}'", e);
                throw;
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                Console.WriteLine("Printing progress.Result:");
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);

                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}', SelectedPartition {2}", operationId, progress.Result.Exception, progress.Result.SelectedPartition);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }
    }
```

## <a name="history-and-truncation"></a>Verlauf und Nachkommastellen erhalten bleiben sollen

Wenn Sie ein Befehl einen terminal Zustand erreicht hat, bleibt deren Metadaten Fehlerinjektion und Analysis Services für eine bestimmte Uhrzeit, bevor sie entfernt werden wird, um Platz zu sparen.  Wenn ""GetProgress "" mithilfe der OperationId eines Befehls aufgerufen wird, nachdem es entfernt wurde, wird eine FabricException mit einer der KeyNotFound Fehlercode zurückgegeben.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx
