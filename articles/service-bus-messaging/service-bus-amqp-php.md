<properties 
    pageTitle="Service Bus und PHP mit AMQP 1.0 | Microsoft Azure"
    description="Verwenden von PHP Dienstbus mit AMQP an."
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

# <a name="using-service-bus-from-php-with-amqp-10"></a>Verwenden von PHP Dienstbus mit AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton-PHP ist eine PHP Sprache Bindung zu Proton-C; d. h., Proton-PHP als Wrapper um eine in c implementiert-Engine implementiert

## <a name="downloading-the-proton-client-library"></a>Herunterladen der Proton-Client-Bibliothek

Sie können aus [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html)Proton-C und deren zugeordneten Bindungen (einschließlich von PHP) herunterladen. Der Download erfolgt sehr in Form von Quellcode. Um den Code zu erstellen, Anweisungen Sie die in das heruntergeladene Paket enthalten sind.

> [AZURE.IMPORTANT] Zum Zeitpunkt der Erstellung dieses Dokuments steht die SSL-Unterstützung in Proton-C nur für Linux Betriebssysteme. Da Azure-Dienstbus die Verwendung von SSL erforderlich ist, können Proton-C (und die Sprache Bindungen) nur verwendet werden, zu Linux zu diesem Zeitpunkt Dienstbus zugreifen. So aktivieren Sie Proton-C mit SSL auf Windows derzeit wird, also Kontrollkästchen regelmäßig nach Updates suchen.

## <a name="working-with-service-bus-queues-topics-and-subscriptions-from-php"></a>Arbeiten mit Warteschlangen, Themen und Abonnements von PHP Dienstbus

Im folgende Code veranschaulicht, wie senden und Empfangen von Nachrichten aus einer Dienstbus messaging Entität.

### <a name="sending-messages-using-proton-php"></a>Senden von Nachrichten mithilfe von PHP-Proton

Der folgende Code veranschaulicht, wie das Senden eine Nachricht mit einem Dienst messaging Entität.

```
$messenger = new Messenger();
$message = new Message();
$message->address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";

$message->body = "This is a text string";
$messenger->put($message);
$messenger->send();
```

### <a name="receiving-messages-using-proton-php"></a>Empfangen von Nachrichten mithilfe von PHP-Proton

Im folgende Code veranschaulicht, wie eine Nachricht von einer Dienstbus erhalten messaging Entität.

```
$messenger = new Messenger();
$address = "amqps://[keyname]:[password]@[namespace].servicebus.windows.net/[entity]";
$messenger->subscribe($address);

$messenger->start();
$messenger->recv(1);

if($messenger->incoming())
{
   $message = new Message();
   $messenger->get($message);      
}

$messenger->stop();
```

## <a name="messaging-between-net-and-proton-php"></a>Zwischen .NET und Proton-PHP Messaging

### <a name="application-properties"></a>Anwendungseigenschaften

#### <a name="protonphp-to-service-bus-net-apis"></a>ProtonPHP zu Service Bus .NET-APIs

Nachrichten von PHP-Proton unterstützen Anwendungseigenschaften die folgenden Arten: **Integer**, **double**, **Boolean**, **Zeichenfolge**und **Objekt**. Im folgende Code von PHP veranschaulicht Festlegen von Eigenschaften für eine Nachricht mit jeder dieser Eigenschaftstypen.

```
$message->properties["TestInt"] = 1;    
$message->properties["TestDouble"] = 1.5;      
$message->properties["TestBoolean"] = False;
$message->properties["TestString"] = "Service Bus";    
$message->properties["TestObject"] = new UUID("1234123412341234");   
```

Eigenschaften der Nachricht Anwendung werden in den Dienst Bus .NET-APIs in der **Properties** -Auflistung des [BrokeredMessage][]übertragen. Im folgende Code veranschaulicht, wie die Anwendungseigenschaften von einer Nachricht von einem Client von PHP zu lesen.

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}if (message.Properties.Keys.Count > 0)
{
foreach (string name in message.Properties.Keys)
{
  Object value = message.Properties[name];
  Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
}
Console.WriteLine();
}
```

In der folgenden Tabelle wird die Typen von PHP die Typen von .NET zugeordnet.

| Typ von PHP-Eigenschaft | Eigenschaftentyp .NET |
|-------------------|--------------------|
| ganze Zahl           | Ganzzahl                |
| Doppelte            | Doppelte             |
| Boolesch           | bool               |
| Zeichenfolge            | Zeichenfolge             |
| Objekt            | Objekt             |

#### <a name="service-bus-net-apis-to-php"></a>Dienstbus .NET APIs zu von PHP

Den [BrokeredMessage][] unterstützt Anwendungseigenschaften der folgenden Typen: **Byte**, **Sbyte**, **Zeichen**, **kurze**, **Ushort**, **Int**, **Uint**, **long**, **Ulong**, **Pufferzeiten**, **double**, **decimal**, **Bool**, **Guid**, **Zeichenfolge**, **Uri**, **DateTime**, **DateTimeOffset**, und **TimeSpan**. Der folgende .NET Code veranschaulicht zum Festlegen von Eigenschaften für ein [BrokeredMessage][] Objekt mit jeder dieser Eigenschaft Typen.

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
```

Der folgende PHP-Code veranschaulicht, wie die Anwendungseigenschaften von einer Nachricht von einem Dienst Bus zu lesen.

```
if ($message->properties != null)
{
  foreach($message->properties as $key => $value)
  {
    printf("-- %s : %s (%s) \n", $key, $value, gettype($value));                       
  }         
}
```

In der folgenden Tabelle werden die .NET Eigenschaft Datentypen die Typen von PHP zugeordnet.

| Eigenschaftentyp .NET | Typ von PHP-Eigenschaft | Notizen                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Byte-Zeichen               | ganze Zahl           | -                                                                                                                                                                     |
| SByte              | ganze Zahl           | -                                                                                                                                                                     |
| Zeichen               | Zeichen              | Proton PHP Klasse                                                                                                                                                    |
| kurze              | ganze Zahl           | -                                                                                                                                                                     |
| ushort             | ganze Zahl           | -                                                                                                                                                                     |
| Ganzzahl                | ganze Zahl           | -                                                                                                                                                                     |
| uint               | Ganze Zahl           | -                                                                                                                                                                     |
| lange               | ganze Zahl           | -                                                                                                                                                                     |
| ulong              | ganze Zahl           | -                                                                                                                                                                     |
| frei verschieben              | Doppelte            | -                                                                                                                                                                     |
| Doppelte             | Doppelte            | -                                                                                                                                                                     |
| Dezimalzahl            | Zeichenfolge            | Dezimal wird mit Proton derzeit nicht unterstützt.                                                                                                                     |
| bool               | Boolesch           | -                                                                                                                                                                     |
| GUID               | UUID              | Proton PHP Klasse                                                                                                                                                    |
| Zeichenfolge             | Zeichenfolge            | -                                                                                                                                                                     |
| "DateTime"           | ganze Zahl           | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | DateTimeOffset.UtcTicks AMQP Typ zugeordnet:<type name="datetime-offset" class=restricted source="long"><descriptor name="com.microsoft:datetime-offset" /></type> |
| TimeSpan           | DescribedType     | Timespan.Ticks AMQP Typ zugeordnet:<type name="timespan" class=restricted source="long"><descriptor name="com.microsoft:timespan" /></type>                        |
| URI                | DescribedType     | Uri.AbsoluteUri AMQP Typ zugeordnet:<type name="uri" class=restricted source="string"><descriptor name="com.microsoft:uri" /></type>                               |

### <a name="standard-properties"></a>Standardeigenschaften

Die folgende Tabelle enthält die Zuordnung zwischen der Proton-PHP standard Nachricht und die [BrokeredMessage][] standard Nachrichteneigenschaften an.

| Proton-PHP           | Dienstbus .NET         | Notizen                                                    |
|----------------------|--------------------------|----------------------------------------------------------|
| Dauerhaftes              | n/v                      | Dienstbus unterstützt nur dauerhafte Nachrichten.          |
| Priorität             | n/v                      | Dienstbus unterstützt nur eine Kernaussage Priorität. |
| TTL                  | Message.TimeToLive       | Konvertierung von PHP-Proton TTL ist in Millisekunden definiert.   |
| erste\_Erwerber      | -                          | -                                                          |
| die Übermittlung\_zählen      | -                          | -                                                          |
| ID                   | Message.Id               | -                                                          |
| Benutzer\_Id             | -                          | -                                                          |
| Adresse              | Message.To               | -                                                          |
| Betreff              | Message.Label            | -                                                          |
| Antwort\_zu            | Message.ReplyTo          | -                                                          |
| Korrelationskoeffizienten\_Id      | Message.CorrelationId    | -                                                          |
| Inhalt\_Typ        | Message.ContentType      | -                                                          |
| Inhalt\_Codierung    | n/v                      | -                                                          |
| Ablauf\_Zeit         | Message.ExpiresAtUTC     | -                                                          |
| Erstellung\_Zeit       | n/v                      | -                                                          |
| Gruppe\_Id            | Message.SessionId        | -                                                          |
| Gruppe\_Sequenz      | -                          | -                                                          |
| Antwort\_auf\_Gruppe\_Id | Message.ReplyToSessionId | -                                                          |
| Format               | n/v                      | -

#### <a name="service-bus-net-apis-to-proton-php"></a>Dienstbus .NET APIs zur Proton-PHP

| Dienstbus .NET        | Proton-PHP                                             | Notizen                                                  |
|-------------------------|--------------------------------------------------------|--------------------------------------------------------|
| ContentType             | Nachricht -\>Inhalt\_Typ                                | -                                                        |
| CorrelationId           | Nachricht -\>Korrelationskoeffizienten\_Id                              | -                                                        |
| EnqueuedTimeUtc         | Nachricht -\>Anmerkungen [X-Suchbegriffen-Warteschlange-Time]             | -                                                        |
| Beschriftung                   | Nachricht -\>Betreff                                      | -                                                        |
| Nachrichten-ID               | Nachricht -\>Id                                           | -                                                        |
| ReplyTo                 | Nachricht -\>Antwort\_zu                                    | -                                                        |
| ReplyToSessionId        | Nachricht -\>Antwort\_auf\_Gruppe\_Id                         | -                                                        |
| ScheduledEnqueueTimeUtc | Nachricht -\>Anmerkungen ["X-Suchbegriffen-geplant-Warteschlange-Time"] | -                                                        |
| SessionId               | Nachricht -\>Gruppe\_Id                                    | -                                                        |
| TimeToLive              | Nachricht -\>Ttl                                          | Konvertierung von PHP-Proton TTL ist in Millisekunden definiert. |
| An                      | Nachricht -\>Adresse                                      | -                                                        |

## <a name="next-steps"></a>Nächste Schritte

Möchten Sie mehr erfahren? Besuchen Sie die folgenden Links:

- [Service Bus AMQP (Übersicht)]
- [AMQP in Dienstbus für WindowsServer]


[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP in Dienstbus für WindowsServer]: https://msdn.microsoft.com/library/dn574799.aspx
[Service Bus AMQP (Übersicht)]: service-bus-amqp-overview.md
