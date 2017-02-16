<properties 
    pageTitle="So AMQP 1.0 API Bus Java-Dienst verwenden | Microsoft Azure" 
    description="So verwenden Sie Java Message Service (JMS) mit Azure-Dienstbus und erweiterte Nachricht Queuing Protodol (AMQP) 1.0." 
    services="service-bus" 
    documentationCenter="java" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Verwenden Sie die API Java Message Service (JMS) mit Dienstbus und AMQP 1.0

Der erweiterte Message Queuing-Protokoll (AMQP) 1.0 ist ein effizienten, zuverlässigen und auf niedriger Ebene messaging-Protokoll, mit denen Sie robuste, Plattformen messaging Applications erstellen.

Unterstützung für AMQP 1.0 in Dienstbus bedeutet, Sie können die queuing verwenden und Veröffentlichen/Abonnieren von vermittelten messaging Features über einen Zellbereich Plattformen über eine effiziente binäre Protokoll. Darüber hinaus können Sie Applikationen durch eine Mischung von Sprachen, Framework und Betriebssysteme erstellte Komponenten besteht aus erstellen.

In diesem Artikel wird erläutert, wie Dienstbus messaging-Funktionen (Warteschlangen und Themen Veröffentlichen/Abonnieren) Verwenden von Java-Anwendungen mit der beliebte Java Message Service (JMS) API standard. Es gibt eine [Companion Artikel](service-bus-dotnet-advanced-message-queuing.md) , die erläutert, wie auch mithilfe der Service Bus .NET-API verwenden. Diese beiden Führungslinien können zusammen Sie Informationen zu Plattformen messaging AMQP 1.0 verwenden.

## <a name="get-started-with-service-bus"></a>Erste Schritte mit Dienstbus

Dieses Handbuch setzt voraus, dass bereits einen Dienstbus Namespace mit einer Warteschlange mit dem Namen **Warteschlange1**. Wenn Sie nicht der Fall ist, können Sie mit der [Azure-Portal](https://portal.azure.com) [den Namespace und Warteschlange erstellen](service-bus-create-namespace-portal.md) . Weitere Informationen zum Erstellen von Dienstbus Namespaces und Warteschlangen finden Sie unter [Servicebuswarteschlangen verwenden](service-bus-dotnet-get-started-with-queues.md).

> [AZURE.NOTE] Partitionierte Warteschlangen und Themen unterstützen auch AMQP. Weitere Informationen finden Sie unter [partitionierte messaging Einheiten](service-bus-partitioning.md) und [AMQP 1.0-Unterstützung für Dienstbus aufgeteilt Warteschlangen und Themen](service-bus-partitioned-queues-and-topics-amqp-overview.md).

## <a name="downloading-the-amqp-10-jms-client-library"></a>Herunterladen der AMQP 1.0 JMS-Client-Bibliothek

Informationen darüber, wo Sie die neueste Version von der Apache Qpid JMS AMQP 1.0-Client-Bibliothek herunterladen können finden Sie auf [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).

Sie müssen die folgenden vier JAR-Dateien aus dem Apache Qpid JMS AMQP 1.0 Verteilung Archiv Java-KLASSENPFAD hinzufügen beim Erstellen und Ausführen von Applications JMS mit Dienstbus:

- Geronimo Jms\_1.1\_1.0.jar-Spezifikation
- qpid-amqp-1-0-Client-[Version].jar
- qpid-amqp-1-0-Client-JMS-[Version].jar
- qpid-amqp-1-0-Common-[Version].jar

## <a name="coding-java-applications"></a>Codieren von Applications Java

### <a name="java-naming-and-directory-interface-jndi"></a>Java Naming and Directory Interface (JNDI)

JMS verwendet Java Naming and Directory Interface (JNDI), um eine Trennung zwischen logischen Namen und physischen Namen zu erstellen. Zwei Arten von JMS-Objekten werden mit JNDI aufgelöst: ConnectionFactory und Ziel. JNDI verwendet eine Anbietermodell, in dem anderen Verzeichnis-Services, um die Namen mit einer Auflösung von Aufgaben behandeln eingebunden werden kann. Formatieren von Apache Qpid JMS AMQP 1.0 Bibliothek im Lieferumfang von einer einfachen Eigenschaften Datei-basierten JNDI-Anbieter, die so konfiguriert ist, mithilfe einer der folgenden Eigenschaftsdatei:

```
# servicebus.properties - sample JNDI configuration
    
# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
    
# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="configure-the-connectionfactory"></a>Konfigurieren der ConnectionFactory

Der Eintrag verwendet, um eine **ConnectionFactory** in den Qpid Eigenschaften Datei JNDI-Anbieter definieren weist das folgende Format:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Wo **[Jndi_name]** und **[ConnectionURL]** folgende Bedeutung haben:

- **[Jndi_name]**: der logische Name der der ConnectionFactory. Dies ist der Name, der in die Java-Anwendung, die mithilfe der Methode JNDI IntialContext.lookup() aufgelöst werden.
- **[ConnectionURL]**: A-URL, die JMS-Bibliothek mit den erforderlichen Informationen in der Makler AMQP bereitstellt.

Das Format von der **ConnectionURL** lautet wie folgt aus:

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
Wo **[Namespace]**, **[SASPolicyName]** und **[SASPolicyKey]** haben folgende Bedeutung:

- **[Namespace]**: der Dienstbus Namespace.
- **[SASPolicyName]**: der Warteschlange freigegeben Access Richtlinie Signaturnamen.
- **[SASPolicyKey]**: der Warteschlange freigegeben Access Signatur Richtlinie-Taste.

> [AZURE.NOTE] Sie müssen URL codiert das Kennwort manuell. Eine hilfreiche URL-codierte Programm ist unter [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp)verfügbar.

#### <a name="configure-destinations"></a>Konfigurieren der Ziele

Der Eintrag verwendet, um eine Adresse in den Qpid Eigenschaften Datei JNDI-Anbieter zu definieren weist das folgende Format:

```
queue.[jndi_name] = [physical_name]
```

oder

```
topic.[jndi_name] = [physical_name]
```

Wo **[Jndi\_Name]** und **[physische\_Name]** haben folgende Bedeutung:

- **[Jndi_name]**: der logische Name des Ziels. Dies ist der Name, der in die Java-Anwendung, die mithilfe der Methode JNDI IntialContext.lookup() aufgelöst werden.
- **[Physical_name]**: der Name der Dienstbus Entität, der die Anwendung gesendet oder Empfangen von Nachrichten.

> [AZURE.NOTE] Wenn von einer Dienstbus Thema Abonnement zu empfangen, sollten im JNDI angegebene physische Name den Namen des Themas. Wenn das permanente Abonnement im JMS-Anwendungscode erstellt wurde, wird der Namen des Abonnements bereitgestellt. Der [Dienst Bus AMQP 1.0 Developer's Guide](service-bus-amqp-dotnet.md) enthält weitere Informationen zum Arbeiten mit Dienstbus Thema Abonnements von JMS an.

### <a name="write-the-jms-application"></a>Schreiben Sie die JMS-Anwendung

Es gibt keine spezielle APIs oder bei Verwendung von JMS mit Dienstbus erforderlichen Optionen aus. Es gibt jedoch einige Einschränkungen, die später behandelt werden. Als ist bei jeder Anwendung JMS erstes erforderlichen Konfiguration der Umgebung JNDI, eine **ConnectionFactory** und Ziele beheben können.

#### <a name="configure-the-jndi-initialcontext"></a>Konfigurieren der JNDI InitialContext

Die JNDI-Umgebung ist Hashtable der Konfigurationsinformationen an den Konstruktor der Klasse javax.naming.InitialContext übergeben konfiguriert. Die beiden erforderlichen Elemente in der Hashtable den Klassennamen der anfänglichen Kontext Fabrik und die Provider-URL sind. Der folgende code zeigt, wie die Verwendung die Qpid Eigenschaftsdatei JNDI-Umgebung konfigurieren basierend JNDI Anbieter mit einer Eigenschaftsdatei mit dem Namen **servicebus.properties**.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Eine einfache JMS-Anwendung mithilfe einer Warteschlange Dienstbus

Das folgende Beispielprogramm JMS TextMessages an eine Warteschlange Dienstbus mit dem JNDI logischen Namen der Warteschlange sendet und empfängt die Nachrichten zurück.

```
// SimpleSenderReceiver.java
    
import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;
    
public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();
    
    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);
    
        // Look up ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");
    
        // Create Connection
        connection = cf.createConnection();
    
        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);
    
        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }
    
    public static void main(String[] args) {
        try {
    
            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }
    
            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] to send a message. Type 'exit' + [enter] to quit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));
    
            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }
    
    public void close() throws JMSException {
        connection.close();
    }
    
    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}   
```

### <a name="run-the-application"></a>Führen Sie die Anwendung

Ausführen der Anwendung die Ausgabe des Formulars:

```
> java SimpleSenderReceiver
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318
    
Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483
    
Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Plattform-messaging zwischen JMS und .NET

In diesem Handbuch habe gezeigt, wie senden und Empfangen von Nachrichten an und von Dienstbus JMS verwenden. Einer der Hauptvorteile von AMQP 1.0 ist jedoch Applications zu erstellende von Komponenten in verschiedenen Sprachen mit Nachrichten, die zur vollständigen Genauigkeit und zuverlässig ausgetauscht geschrieben werden können.

Mit den oben beschriebenen Beispiel JMS-Anwendung und eine ähnliche .NET Anwendung entnommen einer Führungslinie Companion, [So AMQP 1.0 API .NET .NET Service Bus verwenden](service-bus-dotnet-advanced-message-queuing.md), können Sie Nachrichten zwischen .NET und Java austauschen. 

Weitere Informationen zu den Details der Plattformen messaging Dienstbus und AMQP 1.0 verwenden finden Sie unter der [Service Bus AMQP 1.0 Developer's Guide](service-bus-amqp-dotnet.md).

### <a name="jms-to-net"></a>JMS zu .NET

Um zu veranschaulichen, JMS .NET Messaging:

* Starten Sie die Anwendung .NET Beispiel ohne Befehlszeilenargumente.
* Starten der Stichprobe Java-Anwendungs mit Argument "Sendonly" Befehlszeile. In diesem Modus, erhalten die Anwendung keine Nachrichten aus der Warteschlange, wird es nur senden.
* Drücken Sie mehrmals die **EINGABETASTE** , in die Java-Konsole Anwendung, die unterbindet Nachrichten gesendet werden.
* Diese Nachrichten werden durch die Anwendung .NET empfangen.

#### <a name="output-from-jms-application"></a>Ausgaben JMS-Anwendung

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>Die Ausgabe von .NET Anwendung

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-to-jms"></a>.NET zu JMS

Um zu veranschaulichen, .NET JMS-Messaging:

* Starten der Anwendungs .NET Stichproben mit Argument "Sendonly" Befehlszeile. In diesem Modus, erhalten die Anwendung keine Nachrichten aus der Warteschlange, wird es nur senden.
* Starten Sie die Anwendung Java Beispiel ohne Befehlszeilenargumente.
* Drücken Sie mehrmals die **EINGABETASTE** , in die .NET Anwendung Konsole, mit die Hilfe unterbindet Nachrichten gesendet werden.
* Diese Nachrichten werden durch die Java-Anwendung empfangen.

#### <a name="output-from-net-application"></a>Die Ausgabe von .NET Anwendung

```
> SimpleSenderReceiver.exe sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3  
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>Ausgaben JMS-Anwendung

```
> java SimpleSenderReceiver 
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Nicht unterstützte Features und Einschränkungen

Die folgenden Einschränkungen bei der Verwendung von JMS über AMQP 1.0 mit Dienstbus, nämlich vorhanden ist:

* Pro **Sitzung**ist nur eine **MessageProducer** oder **MessageConsumer** zulässig. Wenn Sie mehrere **MessageProducers** oder **MessageConsumers** in einer Anwendung erstellen müssen, erstellen Sie eine dedizierte **Sitzung** für jede von ihnen.
* Veränderliche Thema Abonnements werden derzeit nicht unterstützt.
* **MessageSelectors** werden derzeit nicht unterstützt.
* Temporäre Ziele, d. h., **TemporaryQueue**, **TemporaryTopic** werden derzeit nicht unterstützt zusammen mit den **QueueRequestor** und **TopicRequestor** APIs, die sie verwenden.
* Durchgeführte Sitzungen und verteilte Transaktionen werden nicht unterstützt.

## <a name="summary"></a>Zusammenfassung

Diese schrittweise Anleitung gezeigt, wie vermittelte Dienstbus-Funktionen (Warteschlangen und Themen Veröffentlichen/Abonnieren) von Java verwenden beliebte JMS-API und AMQP 1.0 verwenden.

Sie können auch Service Bus AMQP 1.0 aus anderen Sprachen, einschließlich .NET, C, Python und PHP verwenden. Komponenten, die mit diesen verschiedenen Sprachen erstellt können exchange-Nachrichten zuverlässig und bei vollständiger Genauigkeit mithilfe der AMQP 1.0 unterstützen in Dienstbus. Weitere Informationen finden Sie im [Service Bus AMQP 1.0 Developer's Guide](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Nächste Schritte

* [AMQP 1.0-Unterstützung in Azure-Dienstbus](service-bus-amqp-overview.md)
* [Verwenden von AMQP 1.0 mit der Bus .NET API](service-bus-dotnet-advanced-message-queuing.md)
* [Bus AMQP 1.0 Service Developer's Guide](service-bus-amqp-dotnet.md)
* [So verwenden Sie Servicebuswarteschlangen](service-bus-dotnet-get-started-with-queues.md)
* [Java-Entwicklercenter](/develop/java/).



 
