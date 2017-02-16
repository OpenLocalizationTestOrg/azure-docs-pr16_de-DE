<properties
    pageTitle="Verwalten von Dienstbus mit PowerShell | Microsoft Azure"
    description="Verwalten von Dienstbus mit PowerShell-Skripts"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="sethm"/>

# <a name="manage-service-bus-with-powershell"></a>Verwalten von Dienstbus mit PowerShell

## <a name="overview"></a>(Übersicht)

Microsoft Azure PowerShell handelt es sich um eine scripting-Umgebung, die Sie verwenden können, um zu steuern und Automatisierung der Bereitstellung und Verwaltung von Ihrer Auslastung in Azure. Dieser Artikel beschreibt, wie Sie mithilfe von PowerShell bereitstellen und Verwalten von Dienstbus Elemente wie Namespaces, Warteschlangen und Ereignis Hubs mithilfe einer lokalen Azure PowerShell-Konsole.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie in diesem Artikel beginnen, müssen Sie die folgenden Vorkenntnisse verfügen:

- Ein Azure-Abonnement. Azure ist ein Abonnement-basierten Plattform. Weitere Informationen dazu, wie Sie ein Abonnement erhalten finden Sie unter [Optionen erwerben][], [Bietet Mitglied][]oder [Kostenlose Testversion][].

- Computer mit Azure PowerShell. Anweisungen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][].

- Allgemeine Kenntnisse von PowerShell-Skripts, NuGet-Pakete und .NET Framework.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Einen Verweis auf die für Dienstbus einschließlich

Es gibt eine begrenzte Anzahl von PowerShell-Cmdlets für die Verwaltung von Dienstbus verfügbar. Um die Bereitstellung von Personen, die nicht über die vorhandene Cmdlets verfügbar gemacht werden, können Sie den .NET Client für Dienstbus im [Dienst Bus NuGet-Paket][]verwenden.

Vergewissern Sie sich, dass das Skript **Microsoft.ServiceBus.dll** gefunden kann die mit NuGet-Paket installiert wird. Um flexible werden, führt das Skript folgende Schritte aus:

1. Bestimmt den Pfad, an dem sie aufgerufen wurde.
2. Den Pfad durchläuft, bis er einen Ordner namens findet `packages`. Dieser Ordner wird erstellt, bei der Installation von NuGet-Pakete.
3. Rekursiv Suchbegriffe die `packages` Ordner für eine Assembly mit dem Namen **Microsoft.ServiceBus.dll**.
4. Verweist auf die Assembly, sodass die Typen zur späteren Verwendung zur Verfügung stehen.

So sieht wie folgt vor in einem Powershellskript implementiert werden:

```
try
{
    # WARNING: Make sure to reference the latest version of Microsoft.ServiceBus.dll
    Write-Output "Adding the [Microsoft.ServiceBus.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.ServiceBus.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.ServiceBus.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error "Could not add the Microsoft.ServiceBus.dll assembly to the script. Make sure you build the solution before running the provisioning script."
}
```

## <a name="provision-a-service-bus-namespace"></a>Bereitstellen einer Dienstbus namespace

Zwei PowerShell-Cmdlets unterstützen Dienstbus Namespace Vorgänge an. Statt die APIs von .NET SDK können Sie [Get-AzureSBNamespace][] und [Neu-AzureSBNamespace][]verwenden.

In diesem Beispiel erstellt ein paar lokale Variablen in das Skript. `$Namespace` and `$Location`.

- `$Namespace`ist der Name des Dienstbus Namespace mit dem wir arbeiten möchten.
- `$Location`Gibt das Data Center, in dem das Skript Namespace Vorschriften.
- `$CurrentNamespace`Speichert den Verweis-Namespace, den das Skript abruft (oder erstellt).

In einem tatsächlichen Skript `$Namespace` und `$Location` als Parameter übergeben werden kann.

Dieser Teil des Skripts führt die folgenden Aufgaben:

1. Versucht, einen Namespace Dienstbus mit dem angegebenen Namen abzurufen.
2. Wenn der Namespace gefunden wird, meldet es, was gefunden wurde.
3. Der Namespace nicht gefunden wird, erstellt den Namespace und anschließend neu erstellten Namespace abgerufen.

    ```
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
        New-AzureSBNamespace -Name $Namespace -Location $Location -CreateACSNamespace $false -NamespaceType Messaging
        $CurrentNamespace = Get-AzureSBNamespace -Name $Namespace
        Write-Host "The [$Namespace] namespace in the [$Location] region has been successfully created."
    }
    ```

Um anderen Personen Dienstbus bereitstellen, erstellen Sie eine Instanz der Klasse [NamespaceManager][] aus dem SDK ein.
Das Cmdlet " [Get-AzureSBAuthorizationRule][] " können Sie eine Autorisierungsregel abzurufen, die zum Bereitstellen einer Verbindungszeichenfolge verwendet wird. Speichern wir einen Verweis auf die `NamespaceManager` Instanz fest, der `$NamespaceManager` Variable. Wir verwenden `$NamespaceManager` weiter unten in das Skript zur Bereitstellung von anderen Personen.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the event hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager = [Microsoft.ServiceBus.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```

## <a name="provisioning-other-service-bus-entities"></a>Bereitstellung von anderen Personen Dienstbus

Verwenden Sie die [.NET API für Dienstbus][], um andere Einheiten, wie Warteschlangen, Themen und Ereignis Hubs, bereitstellen. Dieser Artikel befasst sich nur auf Ereignis Hubs, aber die Schritte für andere Personen ähneln. Darüber hinaus werden am Ende dieses Artikels Ausführlichere Beispiele für andere Personen, einschließlich verwiesen wird.

Dieser Teil des Skripts erstellt vier weitere lokalen Variablen. Diese Variablen werden verwendet, um Instanziieren einer `EventHubDescription` Objekt. Das Skript führt die folgenden Aufgaben:

1. Verwenden der `NamespaceManager` Objekt, das Kontrollkästchen, um festzustellen, ob das Ereignis Hub identifizierten `$Path` vorhanden ist.
2. Wenn es nicht vorhanden ist, erstellen Sie ein `EventHubDescription` und übergeben, um die `NamespaceManager` Class `CreateEventHubIfNotExists` Methode.
3. Nachdem festgestellt, dass das Ereignis Hub verfügbar ist, Erstellen einer Gruppe mit Consumer `ConsumerGroupDescription` und `NamespaceManager`.

    ```
    $Path  = "MyEventHub"
    $PartitionCount = 12
    $MessageRetentionInDays = 7
    $UserMetadata = $null
    $ConsumerGroupName = "MyConsumerGroup"
        
    # Check to see if the Event Hub already exists
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

## <a name="migrate-a-namespace-to-another-azure-subscription"></a>Migrieren eines Namespaces zu einem anderen Azure-Abonnement

Die folgende Abfolge von Befehlen wird einen Namespace aus einer Azure-Abonnement zu einem anderen verschoben. Zum Ausführen dieses Vorgangs der Namespace bereits aktiv sein muss, und der Benutzer Ausführen der PowerShell-Befehle muss ein Administrator auf Quell- und Zielliste Abonnements.

```
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel bereitgestellt eine allgemeine Gliederung für Dienstbus Personen mithilfe der PowerShell bereitgestellt. Nichts, dass Sie mit der .NET Client-Bibliotheken ausführen können, können Sie auch in einem Powershellskript ausführen.

Klicken Sie auf diese Blogbeiträge sind weitere Ausführliche Beispiele verfügbar:

- [So erstellen Sie Dienstbus Warteschlangen, Themen und Abonnements mithilfe eines PowerShell-Skript](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
- [So erstellen Sie eine Service Bus Namespace und ein Ereignis Hub mithilfe eines PowerShell-Skript](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Es gibt auch einige vorgefertigten Skripts zum Download zur Verfügung:
- [Reaktionsgruppendienst Bus PowerShell-Skripts](https://code.msdn.microsoft.com/Service-Bus-PowerShell-a46b7059)

<!--Link references-->
[Language Pack für-Optionen]: http://azure.microsoft.com/pricing/purchase-options/
[Mitglied Angebote]: http://azure.microsoft.com/pricing/member-offers/
[Kostenlose Testversion]: http://azure.microsoft.com/pricing/free-trial/
[Installieren und Konfigurieren von Azure PowerShell]: ../powershell-install-configure.md
[Service Bus NuGet-Paket]: http://www.nuget.org/packages/WindowsAzure.ServiceBus/
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[Neue AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx
[.NET API für Dienstbus]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.aspx
[NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
