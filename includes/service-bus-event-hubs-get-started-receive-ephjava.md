## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Empfangen von Nachrichten mit EventProcessorHost in Java

EventProcessorHost ist eine Java-Klasse, die vereinfacht Empfang von Ereignissen Ereignis Hubs durch Verwalten von beständigen Kontrollpunkten empfängt Parallel aus diesen Hubs Ereignis. Mit EventProcessorHost können Sie Ereignisse über mehrere Empfänger aufteilen auch, wenn in unterschiedlichen Knoten gehostet. In diesem Beispiel wird gezeigt, wie EventProcessorHost für einen einzelnen Empfänger verwendet werden soll.

###<a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto

Um EventProcessorHost verwenden zu können, müssen Sie ein [Speicher Azure-Konto][]verfügen:

1. Melden Sie sich bei der [Azure klassischen Portal][], und klicken Sie auf **neu** am unteren Rand des Bildschirms.

2. Klicken Sie auf **Data Services**, und klicken Sie dann **Speicher**dann **Schnell erstellen**, und geben Sie einen Namen für Ihr Speicherkonto. Wählen Sie die gewünschte Region aus, und klicken Sie dann auf **Speicher-Konto erstellen**.

    ![][11]

3. Klicken Sie auf der neu erstellten Speicherkonto, und klicken Sie dann auf **Zugriffstasten verwalten**:

    ![][12]

    Kopieren Sie die primäre Zugriffstaste später in diesem Lernprogramm verwenden.

###<a name="create-a-java-project-using-the-eventprocessor-host"></a>Erstellen eines Java-Projekts mithilfe der EventProcessor-Host

Die Java-Client-Bibliothek für Ereignis Hubs steht zur Verwendung in Maven Projekte aus dem [Zentralen Repository Maven][Maven Package], und verwenden die folgende Abhängigkeit Deklaration in Ihre Projektdatei Maven verwiesen werden:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Für unterschiedliche Arten von Umgebungen erstellen, können Sie explizit die neueste JAR-Dateien aus dem [Zentralen Repository Maven] abrufen[ Maven Package] oder [Release Verteilung](https://github.com/Azure/azure-event-hubs/releases)Punkt auf GitHub.  

1. Das folgende Beispiel erstellen Sie zuerst ein neues Maven Projekt für eine Anwendung Console-Verwaltungsshell in Ihrer bevorzugten Java-Entwicklungsumgebung ein. Die Klasse wird aufgerufen ```ErrorNotificationHandler```.     

    ``` Java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```

2. Verwenden Sie den folgenden Code zum Erstellen einer neuen Klasse mit dem Namen ```EventProcessor```.

    ```Java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;

    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
                
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```

3. Erstellen Sie eine endgültige Klasse mit der Bezeichnung ```EventProcessorSample```, mit dem folgenden Code.

    ```Java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;

    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";

            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
            
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
            
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
            
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }

            System.out.println("Press enter to stop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
                
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
            
            System.out.println("End of sample");
        }
    }
    ```

4. Ersetzen Sie die folgenden Felder mit den Werten verwendet, wenn Sie das Ereignis Hub und Speicher-Konto erstellt haben.

    ``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";

    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";

    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [AZURE.NOTE] In diesem Lernprogramm verwendet eine einzelne Instanz von EventProcessorHost. Um Durchsatz zu erhöhen, empfiehlt es sich, dass Sie mehrere Instanzen von EventProcessorHost ausgeführt werden. In diesen Fällen koordinieren die verschiedenen Instanzen automatisch miteinander um Lastenausgleich die empfangenen Ereignisse. Wenn Sie mehrere Empfänger auf jeder Prozess *Alle* Ereignisse möchten, müssen Sie das **ConsumerGroup** Konzept verwenden. Wenn Ereignisse von verschiedenen Computern zu empfangen, ist dies möglicherweise sinnvoll, den Namen für die EventProcessorHost Instanzen basierend auf dem Computer (oder die Rollen) angeben, in dem sie bereitgestellt werden.

<!-- Links -->
[Event Hubs overview]: ../articles/event-hubs/event-hubs-overview.md
[Azure-Speicher-Konto]: ../articles/storage/storage-create-storage-account.md
[Azure klassischen-portal]: http://manage.windowsazure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png

