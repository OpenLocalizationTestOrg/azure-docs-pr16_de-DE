<properties 
    pageTitle="Service Bus und Python mit AMQP 1.0 | Microsoft Azure"
    description="Verwenden von Dienstbus aus Python mit AMQP."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-python-with-amqp-10"></a>Verwenden von Python Dienstbus mit AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton-Python ist eine Python Sprache Bindung zu Proton-C; d. h., Proton-Python als Wrapper um eine in c implementiert-Engine implementiert

## <a name="download-the-proton-client-library"></a>Herunterladen der Proton-Client-Bibliothek

Sie können aus [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html)Proton-C und deren zugeordneten Bindungen (einschließlich Python) herunterladen. Der Download erfolgt sehr in Form von Quellcode. Um den Code zu erstellen, Anweisungen Sie die im heruntergeladenen Paket enthalten.

Beachten Sie, dass zum Zeitpunkt der Erstellung dieses Dokuments, die SSL-Unterstützung in Proton-C nur für Linux-Betriebssystemen verfügbar ist. Da Azure-Dienstbus die Verwendung von SSL erforderlich ist, können Proton-C (und die Sprache Bindungen) nur verwendet werden, zu Linux zu diesem Zeitpunkt Dienstbus zugreifen. Arbeit an eine andere Proton-C mit SSL auf Windows aktivieren ist derzeit so Kontrollkästchen regelmäßig nach Updates suchen.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-python"></a>Arbeiten mit Dienstbus Warteschlangen, Themen und Python-Abonnements

Im folgende Code veranschaulicht, wie senden und Empfangen von Nachrichten aus einer Dienstbus messaging Entität.

### <a name="send-messages-using-proton-python"></a>Senden von Nachrichten mithilfe von Proton-Python

Der folgende Code veranschaulicht, wie das Senden eine Nachricht mit einem Dienst messaging Entität.

```
messenger = Messenger()
message = Message()
message.address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"

message.body = u"This is a text string"
messenger.put(message)
messenger.send()
```

### <a name="receive-messages-using-proton-python"></a>Empfangen von Nachrichten mit Proton-Python

Im folgende Code veranschaulicht, wie eine Nachricht von einer Dienstbus erhalten messaging Entität.

```
messenger = Messenger()
address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"
messenger.subscribe(address)

messenger.start()
messenger.recv(1)
message = Message()
if messenger.incoming:
      messenger.get(message)
messenger.stop()
```

## <a name="messaging-between-net-and-proton-python"></a>Zwischen .NET und Proton-Python Messaging

### <a name="application-properties"></a>Anwendungseigenschaften

#### <a name="proton-python-to-service-bus-net-apis"></a>Proton-Python zu Service Bus .NET-APIs

Proton-Python Nachrichten unterstützen Anwendungseigenschaften die folgenden Arten: **Int**, **long**, **Pufferzeiten**, **Uuid**, **Bool**, **Zeichenfolge**. Der folgende Python-Code veranschaulicht zum Festlegen von Eigenschaften für eine Nachricht mit jeder dieser Eigenschaftstypen.

```
message.properties[u"TestString"] = u"This is a string"    
message.properties[u"TestInt"] = 1
message.properties[u"TestLong"] = 1000L
message.properties[u"TestFloat"] = 1.5    
message.properties[u"TestGuid"] = uuid.uuid1()    
```

In der Dienst Bus .NET API werden Nachricht Anwendungseigenschaften in der **Properties** -Auflistung des [BrokeredMessage][]übertragen. Mit dem folgende Code veranschaulicht, wie die Anwendungseigenschaften von einer Nachricht von einem Client Python zu lesen.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}
```

In der folgenden Tabelle werden die Python Eigenschaft Datentypen die Typen von .NET zugeordnet.

| Python Eigenschaftentyp | Eigenschaftentyp .NET |
|----------------------|--------------------|
| Ganzzahl                  | Ganzzahl                |
| frei verschieben                | Doppelte             |
| lange                 | Int64              |
| UUID                 | GUID               |
| bool                 | bool               |
| Zeichenfolge               | Zeichenfolge             |

#### <a name="service-bus-net-apis-to-proton-python"></a>Dienstbus .NET APIs zu Proton-Python

Den [BrokeredMessage][] unterstützt Anwendungseigenschaften der folgenden Typen: **Byte**, **Sbyte**, **Zeichen**, **kurze**, **Ushort**, **Int**, **Uint**, **long**, **Ulong**, **Pufferzeiten**, **double**, **decimal**, **Bool**, **Guid**, **Zeichenfolge**, **Uri**, **DateTime**, **DateTimeOffset**, und **TimeSpan**. Der folgende .NET Code veranschaulicht zum Festlegen von Eigenschaften für ein [BrokeredMessage][] -Objekt, das mit jeder dieser Eigenschaft Typen.

```
message.Properties["TestByte"] = (byte)128;
message.Properties["TestSbyte"] = (sbyte)-22;
message.Properties["TestChar"] = (char) 'X';
message.Properties["TestShort"] = (short)-12345;
message.Properties["TestUshort"] = (ushort)12345;
message.Properties["TestInt"] = (int)-100;
message.Properties["TestUint"] = (uint)100;
message.Properties["TestLong"] = (long)-12345;
message.Properties["TestUlong"] = (ulong)12345;
message.Properties["TestFloat"] = (float)3.14159;
message.Properties["TestDouble"] = (double)3.14159;
message.Properties["TestDecimal"] = (decimal)3.14159;
message.Properties["TestBoolean"] = true;
message.Properties["TestGuid"] = Guid.NewGuid();
message.Properties["TestString"] = "Service Bus";
message.Properties["TestUri"] = new Uri("http://www.bing.com");
message.Properties["TestDateTime"] = DateTime.Now;
message.Properties["TestDateTimeOffSet"] = DateTimeOffset.Now;
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

Der folgende Python Code veranschaulicht, wie die Anwendungseigenschaften von einer Nachricht von einem Dienst Bus zu lesen.

```
if message.properties != None:
   for k,v in message.properties.items():         
         print "--   %s : %s (%s)" % (k, str(v), type(v))         
```

In der folgenden Tabelle werden die .NET Eigenschaft Datentypen die Typen von Python zugeordnet.

| Eigenschaftentyp .NET | Python Eigenschaftentyp | Notizen                                                                                                                                                               |
|--------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Byte-Zeichen               | Ganzzahl                  | -                                                                                                                                                                     |
| SByte              | Ganzzahl                  | -                                                                                                                                                                     |
| Zeichen               | Zeichen                 | Proton Python Klasse                                                                                                                                                 |
| kurze              | Ganzzahl                  | -                                                                                                                                                                     |
| ushort             | Ganzzahl                  | -                                                                                                                                                                     |
| Ganzzahl                | Ganzzahl                  | -                                                                                                                                                                     |
| uint               | Ganzzahl                  | -                                                                                                                                                                     |
| lange               | Ganzzahl                  | -                                                                                                                                                                     |
| ulong              | lange                 | Proton Python Klasse                                                                                                                                                 |
| frei verschieben              | frei verschieben                | -                                                                                                                                                                     |
| Doppelte             | frei verschieben                | -                                                                                                                                                                     |
| Dezimalzahl            | Zeichenfolge               | Dezimal wird mit Proton derzeit nicht unterstützt.                                                                                                                     |
| bool               | bool                 | -                                                                                                                                                                     |
| GUID               | UUID                 | Proton Python Klasse                                                                                                                                                 |
| Zeichenfolge             | Zeichenfolge               | -                                                                                                                                                                     |
| "DateTime"           | Zeitstempel            | Proton Python Klasse                                                                                                                                                 |
| DateTimeOffset     | DescribedType        | DateTimeOffset.UtcTicks AMQP Typ zugeordnet:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| TimeSpan           | DescribedType        | Timespan.Ticks AMQP Typ zugeordnet:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| URI                | DescribedType        | Uri.AbsoluteUri AMQP Typ zugeordnet:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-properties"></a>Standardeigenschaften

Die folgende Tabelle enthält die Zuordnung zwischen der standard-Nachricht Proton-Python und die [BrokeredMessage][] Nachricht standard-Eigenschaften.

#### <a name="proton-python-to-service-bus-net-apis"></a>Proton-Python zu Service Bus .NET-APIs

| Proton-Python        | Dienstbus .NET         | Notizen                                                     |
|----------------------|--------------------------|-----------------------------------------------------------|
| Dauerhaftes              | n/v                      | Dienstbus unterstützt nur dauerhafte Nachrichten.               |
| Priorität             | n/v                      | Dienstbus unterstützt nur eine Kernaussage Priorität.      |
| TTL                  | Message.TimeToLive       | Konvertierung; Proton-Python TTL ist in Millisekunden definiert. |
| erste\_Erwerber      | n/v                      | -                                                           |
| die Übermittlung\_zählen      | n/v                      | -                                                           |
| ID                   | Message.MessageID        | -                                                           |
| Benutzer\_Id             | n/v                      | -                                                           |
| Adresse              | Message.To               | -                                                           |
| Betreff              | Message.Label            | -                                                           |
| Antwort\_zu            | Message.ReplyTo          | -                                                           |
| Korrelationskoeffizienten\_Id      | Message.CorrelationID    | -                                                           |
| Inhalt\_Typ        | Message.ContentType      | -                                                           |
| Inhalt\_Codierung    | n/v                      | -                                                           |
| Ablauf\_Zeit         | n/v                      | -                                                           |
| Erstellung\_Zeit       | n/v                      | -                                                           |
| Gruppe\_Id            | Message.SessionId        | -                                                           |
| Gruppe\_Sequenz      | n/v                      | -                                                           |
| Antwort\_auf\_Gruppe\_Id | Message.ReplyToSessionId | -                                                           |
| Format               | n/v                      | -                                                           |

| Dienstbus .NET        | Proton                       | Notizen                                                     |
|-------------------------|------------------------------|-----------------------------------------------------------|
| ContentType             | Message.Content\_Typ        | -                                                           |
| CorrelationId           | Message.Correlation\_Id      | -                                                           |
| EnqueuedTimeUtc         | n/v                          | -                                                           |
| Beschriftung                   | Message.Subject              | -                                                           |
| Nachrichten-ID               | Message.ID                   | -                                                           |
| ReplyTo                 | Message.Reply\_zu            | -                                                           |
| ReplyToSessionId        | Message.Reply\_zu\_Gruppe\_Id | -                                                           |
| ScheduledEnqueueTimeUtc | n/v                          | -                                                           |
| SessionId               | Message.Group\_Id            | -                                                           |
| TimeToLive              | Message.TTL                  | Konvertierung; Proton-Python TTL ist in Millisekunden definiert. |
| An                      | Message.Address              | -                                                           |

## <a name="next-steps"></a>Nächste Schritte

Möchten Sie mehr erfahren? Besuchen Sie die folgenden Links:

- [Service Bus AMQP (Übersicht)]
- [AMQP in Dienstbus für WindowsServer]

[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP in Dienstbus für WindowsServer]: https://msdn.microsoft.com/library/dn574799.aspx

[Service Bus AMQP (Übersicht)]: service-bus-amqp-overview.md
