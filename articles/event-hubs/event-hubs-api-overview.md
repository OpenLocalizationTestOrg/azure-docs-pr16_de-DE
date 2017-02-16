<properties 
    pageTitle="Übersicht über die Azure Ereignis Hubs APIs | Microsoft Azure"
    description="Eine Übersicht über einige wichtige Ereignis Hubs .NET Client-API."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm" />

# <a name="event-hubs-api-overview"></a>Ereignis Hubs API (Übersicht)

In diesem Artikel werden einige der wichtigsten Ereignis Hubs .NET Client-APIs zusammengefasst. Es gibt zwei Kategorien: Verwaltung und Laufzeit-APIs. Laufzeit-APIs bestehen alle Vorgänge, die zum Senden und Empfangen einer Nachricht erforderlich sind. Management-Vorgängen können Sie einen Ereignis Hubs Entitätszustand durch erstellen, aktualisieren und Löschen von Personen zu verwalten.

Überwachung Szenarien umfassen sowohl Management und zur Laufzeit. Ausführliche Dokumentation zu den .NET APIs finden Sie unter die Bezüge [Service Bus.NET](https://msdn.microsoft.com/library/azure/mt419900.aspx) und [EventProcessorHost-API](https://msdn.microsoft.com/library/azure/mt445521.aspx) .

## <a name="management-apis"></a>Projektmanagement-APIs

Um die folgenden Management Vorgänge ausführen zu können, müssen Sie **Verwalten** von Berechtigungen für das Ereignis Hubs Namespace verfügen:

### <a name="create"></a>Erstellen

```
// Create the Event Hub
EventHubDescription ehd = new EventHubDescription(eventHubName);
ehd.PartitionCount = SampleManager.numPartitions;
namespaceManager.CreateEventHubAsync(ehd).Wait();
```

### <a name="update"></a>Aktualisieren

```
EventHubDescription ehd = await namespaceManager.GetEventHubAsync(eventHubName);

// Create a customer SAS rule with Manage permissions
ehd.UserMetadata = "Some updated info";
string ruleName = "myeventhubmanagerule";
string ruleKey = SharedAccessAuthorizationRule.GenerateRandomKey();
ehd.Authorization.Add(new SharedAccessAuthorizationRule(ruleName, ruleKey, new AccessRights[] {AccessRights.Manage, AccessRights.Listen, AccessRights.Send} )); 
namespaceManager.UpdateEventHubAsync(ehd).Wait();
```

### <a name="delete"></a>Löschen

```
namespaceManager.DeleteEventHubAsync("Event Hub name").Wait();
```

## <a name="run-time-apis"></a>Laufzeit-APIs

### <a name="create-publisher"></a>Erstellen von publisher

```
// EventHubClient model (uses implicit factory instance, so all links on same connection)
EventHubClient eventHubClient = EventHubClient.Create("Event Hub name");
```

### <a name="publish-message"></a>Veröffentlichen der Nachricht

```
// Create the device/temperature metric
MetricEvent info = new MetricEvent() { DeviceId = random.Next(SampleManager.NumDevices), Temperature = random.Next(100) };
EventData data = new EventData(new byte[10]); // Byte array
EventData data = new EventData(Stream); // Stream 
EventData data = new EventData(info, serializer) //Object and serializer 
    {
       PartitionKey = info.DeviceId.ToString()
    };

// Set user properties if needed
data.Properties.Add("Type", "Telemetry_" + DateTime.Now.ToLongTimeString());

// Send single message async
await client.SendAsync(data);
```

### <a name="create-consumer"></a>Erstellen von consumer

```
// Create the Event Hubs client
EventHubClient eventHubClient = EventHubClient.Create(EventHubName);

// Get the default consumer group
EventHubConsumerGroup defaultConsumerGroup = eventHubClient.GetDefaultConsumerGroup();

// All messages
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index);

// From one day ago
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index, startingDateTimeUtc:DateTime.Now.AddDays(-1));
                        
// From specific offset, -1 means oldest
EventHubReceiver consumer = await defaultConsumerGroup.CreateReceiverAsync(shardId: index,startingOffset:-1); 
```

### <a name="consume-message"></a>Nutzen Sie die Nachricht

```
var message = await consumer.ReceiveAsync();

// Provide a serializer
var info = message.GetBody<Type>(Serializer)
                                    
// Get a byte[]
var info = message.GetBytes(); 
msg = UnicodeEncoding.UTF8.GetString(info);
```

## <a name="event-processor-host-apis"></a>Ereignis Prozessor Host APIs

Diese APIs bieten Stabilität bei Arbeitsprozesse, die nicht verfügbar ist, werden möglicherweise durch mehrere Shards hinweg auf verfügbaren Worker verteilt.

```
// Checkpointing is done within the SimpleEventProcessor and on a per-consumerGroup per-partition basis, workers resume from where they last left off.
// Use the EventData.Offset value for checkpointing yourself, this value is unique per partition.

string eventHubConnectionString = System.Configuration.ConfigurationManager.AppSettings["Microsoft.ServiceBus.ConnectionString"];
string blobConnectionString = System.Configuration.ConfigurationManager.AppSettings["AzureStorageConnectionString"]; // Required for checkpoint/state

EventHubDescription eventHubDescription = new EventHubDescription(EventHubName);
EventProcessorHost host = new EventProcessorHost(WorkerName, EventHubName, defaultConsumerGroup.GroupName, eventHubConnectionString, blobConnectionString);
            host.RegisterEventProcessorAsync<SimpleEventProcessor>();

// To close
host.UnregisterEventProcessorAsync().Wait();   
```

Die Benutzeroberfläche [IEventProcessor](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.aspx) ist wie folgt definiert:

```
public class SimpleEventProcessor : IEventProcessor
{
    IDictionary<string, string> map;
    PartitionContext partitionContext;

    public SimpleEventProcessor()
    {
        this.map = new Dictionary<string, string>();
    }

    public Task OpenAsync(PartitionContext context)
    {
        this.partitionContext = context;

        return Task.FromResult<object>(null);
    }

    public async Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (EventData message in messages)
        {
            Process messages here
        }
        
        // Checkpoint when appropriate
        await context.CheckpointAsync();

    }


    public async Task CloseAsync(PartitionContext context, CloseReason reason)
    {
        if (reason == CloseReason.Shutdown)
        {
            await context.CheckpointAsync();
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Ereignis Hubs Szenarien finden Sie auf folgenden Links:

- [Was ist Azure Ereignis Hubs?](event-hubs-what-is-event-hubs.md)
- [Ereignis Hubs (Übersicht)](event-hubs-overview.md)
- [Ereignis Hubs programming guide](event-hubs-programming-guide.md)
- [Ereignis Hubs Codebeispielen](http://code.msdn.microsoft.com/site/search?query=event hub&f[0].Value=event hubs&f[0].Type=SearchText&ac=5)

Die Bezüge .NET API befinden sich hier:

- [Dienstbus und Ereignis Hubs .NET API Bezügen](https://msdn.microsoft.com/library/azure/mt419900.aspx)
- [Ereignis Prozessor Host-API-Referenz](https://msdn.microsoft.com/library/azure/mt445521.aspx)
