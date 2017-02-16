<properties 
    pageTitle="So AMQP 1.0 API Bus .NET-Dienst verwenden | Microsoft Azure" 
    description="Erfahren Sie, wie erweiterte Nachricht Queuing Protodol (AMQP) 1.0 API Bus Azure .NET Dienst verwenden." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-amqp-10-with-the-service-bus-net-api"></a>Verwenden von AMQP 1.0 mit der Bus .NET API

Der erweiterte Message Queuing-Protokoll (AMQP) 1.0 ist ein effizienten, zuverlässigen und auf niedriger Ebene messaging-Protokoll, mit denen Sie erstellen robuste, Plattform-messaging Applications.

Unterstützung für AMQP 1.0 in Dienstbus bedeutet, Sie können die queuing verwenden und Veröffentlichen/Abonnieren von vermittelten messaging Features über einen Zellbereich Plattformen über eine effiziente binäre Protokoll. Darüber hinaus können Sie Applikationen durch eine Mischung von Sprachen, Framework und Betriebssysteme erstellte Komponenten besteht aus erstellen.

In diesem Artikel wird erläutert, wie Dienstbus vermittelte messaging verwenden der Features (Warteschlangen und Themen Veröffentlichen/Abonnieren) aus der Bus .NET API verwenden. Es gibt einen [anderen Artikel aus](service-bus-java-how-to-use-jms-api-amqp.md) , die erläutert, wie Sie vorgehen müssen dieselbe mithilfe der standardmäßigen Java Message Service (JMS)-APIs. Diese beiden Führungslinien können zusammen Sie Informationen zu Plattformen messaging AMQP 1.0 verwenden.

## <a name="get-started-with-service-bus"></a>Erste Schritte mit Dienstbus

In diesem Artikel wird vorausgesetzt, dass Sie bereits einen Dienstbus Namespace mit einer Warteschlange mit dem Namen "Warteschlange1". Wenn Sie nicht der Fall ist, können Sie den Namespace und die Warteschlange mithilfe der [Azure-Portal][]erstellen. Weitere Informationen zum Erstellen von Dienstbus Namespaces und Warteschlangen finden Sie unter [Erste Schritte mit Servicebuswarteschlangen](service-bus-dotnet-get-started-with-queues.md#1-create-a-namespace-using-the-Azure-portal).

## <a name="download-the-service-bus-sdk"></a>Herunterladen der Dienstbus SDK

Unterstützung für AMQP 1.0 steht in Service Bus SDK Version 2.1 oder höher. Sie können die neuesten Bits NuGet unter [http://nuget.org/packages/WindowsAzure.ServiceBus/](http://nuget.org/packages/WindowsAzure.ServiceBus/)herunterladen.

## <a name="code-net-applications"></a>Code .NET applications

Standardmäßig kommuniziert Service Bus .NET Clientbibliothek Dienstbus Dienst mit einer dedizierten SOAP-Protokoll ein. Wenn Sie statt der standardmäßigen AMQP 1.0 verwendet erfordert Protokoll explizite Konfiguration auf die Verbindungszeichenfolge Dienstbus wie im nächsten Abschnitt beschrieben. Als diese Änderung unverändert Anwendungscode im Grunde Wenn AMQP 1.0 verwenden.

In der aktuellen Version gibt es ein paar API-Features, die nicht unterstützt werden, wenn Sie AMQP verwenden. Diese nicht unterstützte Features werden später im Abschnitt [nicht unterstützte Features und Einschränkungen](#unsupported-features-and-restrictions)aufgeführt. Einige erweiterte Konfiguration Einstellungen können beim Sie auch eine andere Bedeutung AMQP verwenden. Keiner der diese Einstellungen werden in diesem Artikel verwendet, aber weitere Details stehen im [Dienst Bus AMQP Übersicht](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

### <a name="configure-via-appconfig"></a>Konfigurieren Sie über App.config

Es wird empfohlen, dass die Anwendung der App-Konfiguration verwenden, um die Einstellungen zu speichern. Für Applikationen Dienstbus können Sie App.config Dienstbus **ConnectionString**gespeichert. Diese Anwendung verwendet App.config auch auf den Namen des messaging Entität, mit deren Hilfe Dienstbus speichern.

Eine Beispiel-App ist hier dargestellt:

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
            <add key="EntityName" value="queue1" />
    </appSettings>
</configuration>
```

### <a name="configure-the-service-bus-connection-string"></a>Konfigurieren der Verbindungszeichenfolge Dienstbus

Der Wert der Einstellung **Microsoft.ServiceBus.ConnectionString** ist die Verbindungszeichenfolge Dienstbus, mit dem die Verbindung zu Dienstbus konfigurieren. Das Format ist wie folgt aus:

```
Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp
```

Wo `[namespace]` und `[SAS key]` vom [Azure-Portal][]abgerufen werden. Weitere Informationen finden Sie unter [Servicebuswarteschlangen verwenden] [].

Bei der Verwendung von AMQP mit die Verbindungszeichenfolge angefügt `;TransportType=Amqp`, weist die Client-Bibliothek, deren Verbindung zu Dienstbus herzustellen AMQP 1.0 verwenden.

### <a name="configure-the-entity-name"></a>Konfigurieren der Name des Objekts

Diese Anwendung verwendet die `EntityName` im Abschnitt **AppSettings** von der App auf den Namen der Warteschlange zu konfigurieren, mit der die Anwendung Nachrichten austauscht, festlegen.

### <a name="a-simple-net-application-using-a-service-bus-queue"></a>Eine einfache .NET Anwendung mithilfe einer Warteschlange Dienstbus

Im folgenden Beispiel wird gesendet und Empfangen von Nachrichten an und von einer Warteschlange Dienstbus.

```
// SimpleSenderReceiver.cs
    
using System;
using System.Configuration;
using System.Threading;
using Microsoft.ServiceBus.Messaging;
    
namespace SimpleSenderReceiver
{
    class SimpleSenderReceiver
    {
        private static string connectionString;
        private static string entityName;
        private static Boolean runReceiver = true;
        private MessagingFactory factory;
        private MessageSender sender;
        private MessageReceiver receiver;
        private MessageListener messageListener;
        private Thread listenerThread;
    
        static void Main(string[] args)
        {
            try
            {
                if ((args.Length > 0) && args[0].ToLower().Equals("sendonly"))
                    runReceiver = false;
                    
                string ConnectionStringKey = "Microsoft.ServiceBus.ConnectionString";
                string entityNameKey = "EntityName";
                entityName = ConfigurationManager.AppSettings[entityNameKey];
                connectionString = ConfigurationManager.AppSettings[ConnectionStringKey];
                SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
    
                Console.WriteLine("Press [enter] to send a message. " +
                    "Type 'exit' + [enter] to quit.");
    
                while (true)
                {
                    string s = Console.ReadLine();
                    if (s.ToLower().Equals("exit"))
                    {
                        simpleSenderReceiver.Close();
                        System.Environment.Exit(0);
                    }
                    else
                        simpleSenderReceiver.SendMessage();
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Caught exception: " + e.Message);
            }
        }
    
        public SimpleSenderReceiver()
        {
            factory = MessagingFactory.CreateFromConnectionString(connectionString);
            sender = factory.CreateMessageSender(entityName);
    
            if (runReceiver)
            {
                receiver = factory.CreateMessageReceiver(entityName);
                messageListener = new MessageListener(receiver);
                listenerThread = new Thread(messageListener.Listen);
                listenerThread.Start();
            }
        }
    
        public void Close()
        {
            messageListener.RequestStop();
            listenerThread.Join();
            factory.Close();
        }
    
        private void SendMessage()
        {
            BrokeredMessage message = new BrokeredMessage("Test AMQP message from .NET");
            sender.Send(message);
            Console.WriteLine("Sent message with MessageID = " 
                + message.MessageId);
        }

    }
    
    public class MessageListener
    {
        private MessageReceiver messageReceiver;
        public MessageListener(MessageReceiver receiver)
        {
            messageReceiver = receiver;
        }
    
        public void Listen()
        {
            while (!_shouldStop)
            {
                try
                {
                    BrokeredMessage message = 
                        messageReceiver.Receive(new TimeSpan(0, 0, 10));
                    if (message != null)
                    {
                        Console.WriteLine("Received message with MessageID = " + 
                            message.MessageId);
                        message.Complete();
                    }
                }
                catch (Exception e)
                {
                    Console.WriteLine("Caught exception: " + e.Message);
                    break;
                }
            }
        }
    
        public void RequestStop()
        {
            _shouldStop = true;
        }
    
        private volatile bool _shouldStop;
    }
}
```

### <a name="run-the-application"></a>Führen Sie die Anwendung

Ausführen der Anwendung die Ausgabe des Formulars:

```
> SimpleSenderReceiver.exe
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
Received message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
    
Sent message with MessageID = d00e2e088f06416da7956b58310f7a06
Received message with MessageID = d00e2e088f06416da7956b58310f7a06
    
Received message with MessageID = f27f79ec124548c196fd0db8544bca49
Sent message with MessageID = f27f79ec124548c196fd0db8544bca49
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Plattform-messaging zwischen JMS und .NET

Dieser erläutert, wie zum Senden von Nachrichten unter Verwendung von .NET mit Service und wie diese mit .NET Nachrichten empfangen werden sollen. Einer der Hauptvorteile von AMQP 1.0 ist jedoch Applications zu erstellende von Komponenten in verschiedenen Sprachen mit Nachrichten, die zur vollständigen Genauigkeit und zuverlässig ausgetauscht geschrieben werden können.

Mit den oben beschriebenen Beispiel .NET Anwendung und eine ähnliche Java-Anwendung entnommen eine Companion Anleitung, [wie die API mit Dienstbus & AMQP 1.0 Java Message Service (JMS) verwenden](service-bus-java-how-to-use-jms-api-amqp.md), ist es möglich, zum Austauschen von Nachrichten zwischen .NET und Java. 

Weitere Informationen zu den Details der Plattformen messaging Dienstbus und AMQP 1.0 verwenden finden Sie unter der [Dienst Bus AMQP 1.0 Übersicht](service-bus-amqp-overview.md).

### <a name="jms-to-net"></a>JMS zu .NET

Um zu veranschaulichen, JMS .NET Messaging:

* Starten Sie die Anwendung .NET Beispiel ohne Befehlszeilenargumente.
* Starten der Stichprobe Java-Anwendungs mit Argument "Sendonly" Befehlszeile. In diesem Modus, erhalten die Anwendung keine Nachrichten aus der Warteschlange, wird es nur senden.
* Drücken Sie mehrmals die **EINGABETASTE** , in die Java-Konsole Anwendung, die unterbindet Nachrichten gesendet werden.
* Diese Nachrichten werden durch die Anwendung .NET empfangen.

### <a name="output-from-jms-application"></a>Ausgaben JMS-Anwendung

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

### <a name="output-from-net-application"></a>Die Ausgabe von .NET Anwendung

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

## <a name="net-to-jms"></a>.NET zu JMS

Um zu veranschaulichen, .NET JMS-Messaging:

* Die Anwendung .NET Stichproben mit Argument Befehlszeile "Sendonly" beginnen. In diesem Modus, erhalten die Anwendung keine Nachrichten aus der Warteschlange, wird es nur senden.
* Starten Sie die Java-Beispiel-Anwendung ohne Befehlszeilenargumente.
* Drücken Sie mehrmals die **EINGABETASTE** , in die .NET Anwendung Konsole, mit die Hilfe unterbindet Nachrichten gesendet werden.
* Diese Nachrichten werden durch die Java-Anwendung empfangen.

#### <a name="output-from-net-application"></a>Ausgaben .NET Anwendung

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

Die folgenden Features der .NET Service Bus-API werden derzeit nicht unterstützt, wenn Sie AMQP verwenden:

 * Transaktionen.
 * Senden Sie per durchstellen Ziel

Weitere Informationen finden Sie unter [nicht unterstützte Features, Einschränkungen, und unterschieden im Verhalten](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel gezeigt, wie .NET messaging vermittelte Dienstbus-Features (Warteschlangen und Themen Veröffentlichen/Abonnieren) zugreifen AMQP 1.0 und der Bus .NET API verwenden.

Sie können auch von anderen Sprachen, einschließlich Java, C, Python und PHP Service Bus AMQP 1.0 verwenden. Mithilfe dieser Sprachen erstellte Komponenten können Nachrichten zuverlässig und bei vollständiger Genauigkeit mit AMQP 1.0 in Dienstbus austauschen. Weitere Informationen finden Sie unter der [Dienst Bus AMQP Übersicht](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie einen Überblick über Dienstbus und AMQP mit .NET gelesen haben, finden Sie unter den folgenden Links für Weitere Informationen.

* [AMQP 1.0-Unterstützung in Azure-Dienstbus](service-bus-amqp-overview.md)
* [So verwenden Sie die API Java Message Service (JMS) mit Dienstbus & AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
* [So verwenden Sie Servicebuswarteschlangen](service-bus-dotnet-get-started-with-queues.md)
 
[Azure-portal]: https://portal.azure.com
