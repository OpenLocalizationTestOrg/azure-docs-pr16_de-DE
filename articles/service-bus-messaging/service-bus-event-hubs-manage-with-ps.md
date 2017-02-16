<properties
    pageTitle="Mithilfe von PowerShell Dienstbus und Ereignis Hubs Ressourcen verwalten | Microsoft Azure"
    description="Erstellen und Verwalten von Dienstbus und Ereignis Hubs Ressourcen mithilfe der PowerShell"
    services="service-bus,event-hubs"
    documentationCenter=".NET"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="use-powershell-to-manage-service-bus-and-event-hubs-resources"></a>Mithilfe von PowerShell Dienstbus und Ereignis Hubs Ressourcen verwalten

Microsoft Azure PowerShell ist eine scripting-Umgebung, die Sie zum Steuern und Automatisierung der Bereitstellung und Verwaltung von Azure Services verwenden können. Dieser Artikel beschreibt, wie Sie mithilfe von PowerShell bereitstellen und Verwalten von Dienstbus Elemente wie Namespaces, Warteschlangen und Ereignis Hubs mithilfe einer lokalen Azure PowerShell-Konsole.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen, benötigen Sie Folgendes:

- Ein Azure-Abonnement. Azure ist ein Abonnement-basierten Plattform. Weitere Informationen dazu, wie Sie ein Abonnement erhalten finden Sie unter [Optionen erwerben][], [bietet Mitglied][]oder [kostenloses Konto][].

- Computer mit Azure PowerShell. Anweisungen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][].

- Allgemeine Kenntnisse von PowerShell-Skripts, NuGet-Pakete und .NET Framework.

## <a name="include-a-reference-to-the-net-assembly-for-service-bus"></a>Schließen Sie einen Verweis auf die für Dienstbus

Es gibt eine begrenzte Anzahl von PowerShell-Cmdlets für die Verwaltung von Dienstbus verfügbar. Zum Bereitstellen von Personen, die nicht über die vorhandene Cmdlets verfügbar gemacht werden, können Sie den Kunden .NET verweisen auf das [Service Bus NuGet-Paket]für Dienstbus aus innerhalb von PowerShell verwenden.

Vergewissern Sie sich, dass das Skript **Microsoft.ServiceBus.dll** gefunden kann die mit NuGet-Paket installiert wird. Um flexible werden, führt das Skript folgende Schritte aus:

1. Bestimmt den Pfad, in dem es aufgerufen wird.
2. Den Pfad durchläuft, bis er einen Ordner namens findet `packages`. Dieser Ordner wird erstellt, bei der Installation von NuGet-Pakete.
3. Rekursiv Suchbegriffe die `packages` Ordner für eine Assembly mit dem Namen **Microsoft.ServiceBus.dll**.
4. Verweist auf die Assembly, sodass die Typen zur späteren Verwendung zur Verfügung stehen.

So sieht wie folgt vor in einem Powershellskript implementiert werden:

```powershell

try
{
    # IMPORTANT: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}

```

## <a name="provision-a-service-bus-namespace"></a>Bereitstellen einer Dienstbus namespace

Bei der Arbeit mit Dienstbus Namespaces es gibt zwei Cmdlets anstelle von .NET SDK können: [Get-AzureSBNamespace][] und [Neu-AzureSBNamespace][].

In diesem Beispiel erstellt ein paar lokale Variablen in das Skript. `$Namespace` and `$Location`.

- `$Namespace`ist der Name der des Dienstbus Namespace mit dem wir arbeiten möchten.
- `$Location`kennzeichnet das Data Center, in dem wir den Namespace bereitgestellt wird.
- `$CurrentNamespace`Speichert den Verweis-Namespace, den wir abrufen (oder erstellen).

In einem tatsächlichen Skript `$Namespace` und `$Location` als Parameter übergeben werden kann.

Dieser Teil des Skripts geschieht Folgendes:

1. Versucht, einen Namespace Dienstbus mit dem angegebenen Namen abzurufen.
2. Wenn der Namespace gefunden wird, meldet es, was gefunden wurde.
3. Der Namespace nicht gefunden wird, erstellt den Namespace und anschließend neu erstellten Namespace abgerufen.

    ``` powershell

    $Namespace = "MyServiceBusNS"
    $Location = "West US"

    # Query to see if the namespace currently exists
    $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

    # Check if the namespace already exists or needs to be created
    if ($CurrentNamespace)
    {
        Write-Output "The namespace [$Namespace] already exists in the [$($CurrentNamespace.Region)] region."
    }
    else
    {
        Write-Host "The [$Namespace] namespace does not exist."
        Write-Output "Creating the [$Namespace] namespace in the [$Location] region..."
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```
Um anderen Personen Dienstbus bereitstellen, erstellen Sie eine Instanz von der `NamespaceManager` Objekt aus dem SDK. Das Cmdlet " [Get-AzureSBAuthorizationRule][] " können Sie eine Autorisierungsregel abzurufen, die zum Bereitstellen einer Verbindungszeichenfolge verwendet wird. In diesem Beispiel speichert einen Verweis auf die `NamespaceManager` Instanz fest, der `$NamespaceManager` Variable. Das Skript später verwendet `$NamespaceManager` zur Bereitstellung von anderen Personen.

    ``` powershell
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    # Create the NamespaceManager object to create the Event Hub
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
    ```

## <a name="provisioning-other-service-bus-entities"></a>Bereitstellung von anderen Personen Dienstbus

Um die Bereitstellung von anderen Personen, wie etwa Warteschlangen, Themen und Ereignis Hubs, können Sie die [.NET API für Dienstbus][]verwenden. Ausführlichere Beispiele für andere Personen, einschließlich werden am Ende dieses Artikels verwiesen wird.

### <a name="create-an-event-hub"></a>Erstellen Sie einen Ereignis-Hub

Dieser Teil des Skripts erstellt vier weitere lokalen Variablen. Diese Variablen werden verwendet, um Instanziieren einer `EventHubDescription` Objekt. Das Skript führt Folgendes aus:

1. Verwenden der `NamespaceManager` Objekt, das Kontrollkästchen, um festzustellen, ob das Ereignis Hub identifizierten `$Path` vorhanden ist.
2. Wenn sie nicht vorhanden ist, Erstellen einer `EventHubDescription` und übergeben, um die `NamespaceManager` Class `CreateEventHubIfNotExists` Methode.
3. Nachdem festgestellt, dass das Ereignis Hub verfügbar ist, Erstellen einer Gruppe mit Consumer `ConsumerGroupDescription` und `NamespaceManager`.

    ``` powershell

    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"

    # Check if the Event Hub already exists
    if ($NamespaceManager.EventHubExists($Path))
    {
        Write-Output "The [$Path] event hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] event hub in the [$Namespace] namespace: PartitionCount=[$PartitionCount] MessageRetentionInDays=[$MessageRetentionInDays]..."
        $EventHubDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.EventHubDescription -ArgumentList $Path
        $EventHubDescription.PartitionCount = $PartitionCount
        $EventHubDescription.MessageRetentionInDays = $MessageRetentionInDays
        $EventHubDescription.UserMetadata = $UserMetadata
        $EventHubDescription.Path = $Path
        $NamespaceManager.CreateEventHubIfNotExists($EventHubDescription);
        Write-Output "The [$Path] event hub in the [$Namespace] namespace has been successfully created."
    }

    # Create the consumer group if it doesn't exist
    Write-Output "Creating the consumer group [$ConsumerGroupName] for the [$Path] event hub..."
    $ConsumerGroupDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.ConsumerGroupDescription -ArgumentList $Path, $ConsumerGroupName
    $ConsumerGroupDescription.UserMetadata = $ConsumerGroupUserMetadata
    $NamespaceManager.CreateConsumerGroupIfNotExists($ConsumerGroupDescription);
    Write-Output "The consumer group [$ConsumerGroupName] for the [$Path] event hub has been successfully created."
    ```

### <a name="create-a-queue"></a>Erstellen einer Warteschlange

Führen Sie ein Häkchen Namespace wie im vorherigen Abschnitt, um eine Warteschlange oder ein Thema zu erstellen.

    if ($NamespaceManager.QueueExists($Path))
    {
        Write-Output "The [$Path] queue already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] queue in the [$Namespace] namespace..."
        $QueueDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.QueueDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $QueueDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $QueueDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $QueueDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $QueueDescription.EnableBatchedOperations = $EnableBatchedOperations
        $QueueDescription.EnableDeadLetteringOnMessageExpiration = $EnableDeadLetteringOnMessageExpiration
        $QueueDescription.EnableExpress = $EnableExpress
        $QueueDescription.EnablePartitioning = $EnablePartitioning
        $QueueDescription.ForwardDeadLetteredMessagesTo = $ForwardDeadLetteredMessagesTo
        $QueueDescription.ForwardTo = $ForwardTo
        $QueueDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        if ($LockDuration -gt 0)
        {
            $QueueDescription.LockDuration = [System.TimeSpan]::FromSeconds($LockDuration)
        }
        $QueueDescription.MaxDeliveryCount = $MaxDeliveryCount
        $QueueDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $QueueDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        $QueueDescription.RequiresSession = $RequiresSession
        if ($EnablePartitioning)
        {
            $QueueDescription.SupportOrdering = $False
        }
        else
        {
            $QueueDescription.SupportOrdering = $SupportOrdering
        }
        $QueueDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateQueue($QueueDescription);
    }

### <a name="create-a-topic"></a>Erstellen Sie ein Thema

    if ($NamespaceManager.TopicExists($Path))
    {
        Write-Output "The [$Path] topic already exists in the [$Namespace] namespace."
    }
    else
    {
        Write-Output "Creating the [$Path] topic in the [$Namespace] namespace..."
        $TopicDescription = New-Object -TypeName Microsoft.ServiceBus.Messaging.TopicDescription -ArgumentList $Path
        if ($AutoDeleteOnIdle -ge 5)
        {
            $TopicDescription.AutoDeleteOnIdle = [System.TimeSpan]::FromMinutes($AutoDeleteOnIdle)
        }
        if ($DefaultMessageTimeToLive -gt 0)
        {
            $TopicDescription.DefaultMessageTimeToLive = [System.TimeSpan]::FromMinutes($DefaultMessageTimeToLive)
        }
        if ($DuplicateDetectionHistoryTimeWindow -gt 0)
        {
            $TopicDescription.DuplicateDetectionHistoryTimeWindow = [System.TimeSpan]::FromMinutes($DuplicateDetectionHistoryTimeWindow)
        }
        $TopicDescription.EnableBatchedOperations = $EnableBatchedOperations
        $TopicDescription.EnableExpress = $EnableExpress
        $TopicDescription.EnableFilteringMessagesBeforePublishing = $EnableFilteringMessagesBeforePublishing
        $TopicDescription.EnablePartitioning = $EnablePartitioning
        $TopicDescription.IsAnonymousAccessible = $IsAnonymousAccessible
        $TopicDescription.MaxSizeInMegabytes = $MaxSizeInMegabytes
        $TopicDescription.RequiresDuplicateDetection = $RequiresDuplicateDetection
        if ($EnablePartitioning)
        {
            $TopicDescription.SupportOrdering = $False
        }
        else
        {
            $TopicDescription.SupportOrdering = $SupportOrdering
        }
        $TopicDescription.UserMetadata = $UserMetadata
        $NamespaceManager.CreateTopic($TopicDescription);
    }

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel bereitgestellt eines einfachen Workflows für die Bereitstellung von Dienstbus Personen mithilfe der PowerShell. Obwohl es eine begrenzte Anzahl von PowerShell-Cmdlets gibt für die Verwaltung von Dienstbus verfügbar können Personen, durch einen Verweis auf die Assembly Microsoft.ServiceBus.dll, praktisch jedem Vorgang messaging sondern mithilfe der .NET Client-Bibliotheken, die Sie auch in einem Powershellskript ausführen können.

Ausführlichere Beispiele stehen in diese Blogbeiträge zur Verfügung:

- [So erstellen Sie Dienstbus Warteschlangen, Themen und Abonnements mithilfe eines PowerShell-Skript](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [So erstellen Sie eine Service Bus Namespace und ein Ereignis Hub mithilfe eines PowerShell-Skript](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Es gibt auch einige vorgefertigten Skripts zum Download zur Verfügung:

- [Reaktionsgruppendienst Bus PowerShell-Skripts](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Anchors-->

[Language Pack für-Optionen]: http://azure.microsoft.com/pricing/purchase-options/
[Mitglied bietet.]: http://azure.microsoft.com/pricing/member-offers/
[kostenloses Konto]: http://azure.microsoft.com/pricing/free-trial/
[Service Bus NuGet-Paket]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Neue AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[.NET API für Dienstbus]: https://msdn.microsoft.com/en-us/library/azure/mt419900.aspx
[Installieren und Konfigurieren von Azure PowerShell]: ../powershell-install-configure.md
