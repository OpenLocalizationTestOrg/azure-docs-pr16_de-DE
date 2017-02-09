## <a name="send-messages-to-event-hubs"></a>Senden von Nachrichten an Ereignis Hubs

Die Java-Client-Bibliothek für Ereignis Hubs steht zur Verwendung in Maven Projekte aus dem [Maven zentralen Repository gespeichert](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22), und verwenden die folgende Abhängigkeit Deklaration in Ihre Projektdatei Maven verwiesen werden:    

``` XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```
 
Für unterschiedliche Arten von Umgebungen erstellen können Sie die neueste JAR-Dateien aus dem [Maven zentralen Repository gespeichert](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) oder [Release Verteilung](https://github.com/Azure/azure-event-hubs/releases)Punkt auf GitHub explizit abrufen.  

Importieren Sie für eine einfache Ereignis Publisher das Paket *com.microsoft.azure.eventhubs* für das Ereignis Hubs-Client-Klassen und das Paket *com.microsoft.azure.servicebus* für Programm Klassen wie allgemeine Ausnahmen, die mit dem Azure-Dienstbus messaging Client freigegeben wurden. 

Das folgende Beispiel erstellen Sie zuerst ein neues Maven Projekt für eine Anwendung Console-Verwaltungsshell in Ihrer bevorzugten Java-Entwicklungsumgebung ein. Die Klasse wird aufgerufen ```Send```.     

``` Java

import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Ersetzen Sie den Namespace und den Namen des Ereignisses Hub mit den Werten verwendet, wenn Sie das Ereignis Hub erstellt haben.

``` Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Erstellen Sie dann ein Ereignis im singular durch Umwandlung in einer Zeichenfolge in deren UTF-8-Byte-Codierung ein. Wir dann eine neue Instanz von Ereignis Hubs Client aus der Verbindungszeichenfolge erstellen und senden Sie die Nachricht.   

``` Java 
                
    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);
    
    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 
