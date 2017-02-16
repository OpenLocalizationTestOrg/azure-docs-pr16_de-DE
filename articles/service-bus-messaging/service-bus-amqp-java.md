<properties 
    pageTitle="Service Bus und Java mit AMQP 1.0 | Microsoft Azure"
    description="Verwenden von Java Dienstbus mit AMQP"
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

# <a name="use-service-bus-from-java-with-amqp-10"></a>Verwenden von Java Dienstbus mit AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Java Message Service (JMS) ist eine standard-API für die Zusammenarbeit mit Nachricht Orientierung Middleware Java-Plattform. Microsoft, Azure Dienstbus getestet wurde, mit dem AMQP 1.0 basiert JMS-Client-Bibliothek, die Apache Qpid Projekt. Diese Bibliothek unterstützt die vollständige JMS 1.1-API und anderen AMQP 1.0-kompatible messaging-Diensten verwendet werden kann. Dieses Szenario wird auch in [Service Bus für Windows Server](https://msdn.microsoft.com/library/dn282144.aspx) unterstützt (lokale Dienstbus). Weitere Informationen finden Sie unter [AMQP in Service Bus für Windows Server][].

## <a name="download-the-apache-qpid-amqp-10-jms-client-library"></a>Herunterladen der Apache Qpid AMQP 1.0 JMS-Client-Bibliothek

Informationen zum Herunterladen der neuesten Version von der Apache Qpid JMS AMQP 1.0-Client-Bibliothek finden Sie auf [http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html](http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html).

Sie müssen die folgenden vier JAR-Dateien aus dem Apache Qpid JMS AMQP 1.0 Verteilung Archiv Java-KLASSENPFAD hinzufügen beim Erstellen und Ausführen von Applications JMS mit Dienstbus:

-   Geronimo Jms\_1.1\_.jar Spezifikation-[Version]

-   qpid-amqp-1-0-Client-[Version].jar

-   qpid-amqp-1-0-Client-JMS-[Version].jar

-   qpid-amqp-1-0-Common-[Version].jar

## <a name="work-with-service-bus-queues-topics-and-subscriptions-from-jms"></a>Arbeiten Sie mit Warteschlangen, Themen und Abonnements von JMS Dienstbus

### <a name="java-naming-and-directory-interface-jndi"></a>Java Naming and Directory Interface (JNDI)

JMS verwendet Java Naming and Directory Interface (JNDI), um eine Trennung zwischen logischen Namen und physischen Namen zu erstellen. Zwei Arten von JMS-Objekten werden mit JNDI aufgelöst: **ConnectionFactory** und **Ziel**. JNDI verwendet eine Anbietermodell, in dem anderen Verzeichnis-Services, um die Namen mit einer Auflösung von Aufgaben behandeln eingebunden werden kann. Die Apache Qpid JMS AMQP 1.0-Bibliothek im Lieferumfang von einer einfachen Eigenschaften Datei-basierten JNDI-Anbieter, die mit einer Textdatei konfiguriert ist.

Der Qpid Eigenschaften Datei JNDI-Anbieter ist so konfiguriert, dass mithilfe einer Eigenschaftsdatei mit der folgenden Syntax:

```
# servicebus.properties – sample JNDI configuration

# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCONNECTIONFACTORY = amqps://[username]:[password]@[namespace].servicebus.windows.net

# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
topic.TOPIC = topic1
queue.QUEUE = queue1
```

#### <a name="configure-the-connection-factory"></a>Konfigurieren der Verbindung factory

Der Eintrag verwendet, um eine **ConnectionFactory** im Qpid Eigenschaften Datei JNDI Anbieter definieren weist das folgende Format:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Wo `[jndi\_name]` und `[ConnectionURL]` haben folgende Bedeutung:

| Namen            | Bedeutung                                                                                                                                    |   |   |   |   |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------|---|---|---|---|
| `[jndi\_name]`    | Der logische Name der Verbindung Factory. Dieser Name wird in die Java-Anwendung mithilfe den JNDI aufgelöst `IntialContext.lookup()` Methode. |   |   |   |   |
| `[ConnectionURL]` | Ein URL, die JMS-Bibliothek mit den erforderlichen Informationen in der Makler AMQP enthält.                                                      |   |   |   |   |

Das Format der Verbindung ist URL wie folgt aus:

```
amqps://[username]:[password]@[namespace].servicebus.windows.net
```

Wo `[namespace]`, `[username]`, und `[password]` haben folgende Bedeutung:

| Namen          | Bedeutung                                                                        |   |   |   |   |
|---------------|--------------------------------------------------------------------------------|---|---|---|---|
| `[namespace]` | Dienstbus Namespace aus dem [Azure-Portal][]abgerufen.                      |   |   |   |   |
| `[username]`  | Der Dienst Bus SAS Key Name aus dem [Azure-Portal][]abgerufen.                    |   |   |   |   |
| `[password]`  | URL-codierte Formular, der die vom im [Portal Azure][]Service Bus SAS-Taste. |   |   |   |   |

> [AZURE.NOTE] Sie müssen URL codiert das Kennwort manuell. Ein hilfreiche Codierung URL-Programm ist unter [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp)verfügbar.

Wenn die Informationen von erhaltenen beträgt beispielsweise im Portal wie folgt:

| Namespace:   | Test.Servicebus.Windows.NET                  |
|--------------|----------------------------------------------|
| Name des Herausgebers: | RootManageSharedAccessKey                                        |
| Herausgeber Schlüssel:  | abcdefg |

Klicken Sie dann, um ein **ConnectionFactory** Objekt mit dem Namen definieren `SBCONNECTIONFACTORY`, die Konfigurationszeichenfolge wäre wie folgt:

```
connectionfactory.SBCONNECTIONFACTORY = amqps://RootManageSharedAccessKey:abcdefg@test.servicebus.windows.net
```

#### <a name="configure-destinations"></a>Konfigurieren der Ziele

Der Eintrag, der ein Ziel im Qpid Eigenschaften Datei JNDI Anbieter definiert weist das folgende Format:

```
queue.[jndi_name] = [physical_name]
topic.[jndi_name] = [physical_name]
```

Wo `[jndi\_name]` und `[physical\_name]` haben folgende Bedeutung:

| Namen              | Bedeutung                                                                                                                                  |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| `[jndi\_name]`    | Der logische Name des Ziels. Dies ist Name wird in die Java-Anwendung mithilfe den JNDI aufgelöst `IntialContext.lookup()` Methode. |
| `[physical\name]` | Der Name der Dienstbus Entität, die die Anwendung gesendet oder Empfangen von Nachrichten.                                                  |

Beachten Sie Folgendes:

- Die `[physical\name]` Wert kann eine Dienstbus Warteschlange oder Thema sein.
- Wenn von einer Dienstbus Thema Abonnement zu empfangen, sollten im JNDI angegebene physische Name den Namen des Themas. Wenn das permanente Abonnement im JMS-Anwendungscode erstellt wurde, wird der Namen des Abonnements bereitgestellt.
- Es ist auch möglich, ein Dienstbus Thema Abonnement als eine JMS-Warteschlange zu behandeln. Es gibt mehrere Vorteile dieser Vorgehensweise: derselbe Empfänger-Code für Warteschlangen und Thema Abonnements verwendet werden kann, und alle Adressinformationen (die Namen Thema und Abonnement) in der Eigenschaftsdatei externalisiert ist.
- Damit ein Dienstbus Thema Abonnement als eine JMS-Warteschlange behandelt werden, sollten der Eintrag in der Datei mit den Eigenschaften des Formulars: `queue.[jndi\_name] = [topic\_name]/Subscriptions/[subscription\_name]`. |

Definieren einer logischen JMS-Ziel mit dem Namen "Thema", die mit dem Namen "topic1" Dienstbus Themas zugeordnet ist, würde der Eintrag in der Eigenschaftsdatei wie folgt aussehen:

```
topic.TOPIC = topic1
```

### <a name="send-messages-using-jms"></a>Senden von Nachrichten mithilfe von JMS

Der folgende Code wird gezeigt, wie zu einem Thema Dienstbus eine Nachricht gesendet wird. Es wird davon ausgegangen, dass `SBCONNECTIONFACTORY` und `TOPIC` in einer Konfigurationsdatei **servicebus.properties** definiert werden, wie im vorherigen Abschnitt beschrieben.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env); 
 
ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
MessageProducer producer = session.createProducer(topic);
TextMessage message = session.createTextMessage("This is a text string"); 
producer.send(message);
```

### <a name="receive-messages-using-jms"></a>Empfangen von Nachrichten mithilfe von JMS

Der folgende code zeigt `how` eine Nachricht aus einem Dienstbus Thema Abonnement zu erhalten. Es wird davon ausgegangen, dass `SBCONNECTIONFACTORY` und Thema in einer Konfigurationsdatei **servicebus.properties** definiert werden, wie im vorherigen Abschnitt beschrieben. Außerdem wird vorausgesetzt, dass der Namen des Abonnements ist `subscription1`.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env);

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
TopicSubscriber subscriber = session.createDurableSubscriber(topic, "subscription1");
connection.start();
Message message = messageConsumer.receive();
```

### <a name="guidelines-for-building-robust-applications"></a>Richtlinien für das Erstellen von robuster Anwendungen

Die JMS-Spezifikation definiert wie der Vertrag Ausnahme API Methoden und Anwendungscode solche Ausnahmen behandelt geschrieben werden sollen. Hier sind einige andere Punkte zur Fehlerbehandlung berücksichtigen:

-   Registrieren einer **ExceptionListener** der JMS-Verbindungs **connection.setExceptionListener**verwenden. Dies ermöglicht einen Client ein Problem asynchrone benachrichtigt werden sollen. Diese Benachrichtigung ist besonders wichtig für Verbindungen, bei die nur Nachrichten nutzen, wie sie müssten keine andere Möglichkeit, wenn Sie wissen, dass ihre Verbindung fehlgeschlagen ist. Der **ExceptionListener** wird aufgerufen, wenn es ein Problem mit der zugrunde liegenden AMQP Verbindung, Sitzung oder Link liegt. In diesem Fall sollte die Anwendung der **JMS-Verbindung**, **Sitzung**, **MessageProducer** und **MessageConsumer** Objekte völlig neu erstellen.

-   Um zu überprüfen, dass eine Nachricht für eine Person Dienstbus erfolgreich aus einer **MessageProducer** gesendet wurde, stellen Sie sicher, dass die Anwendung mit konfiguriert wurde der **qpid.sync\_veröffentlichen** System Eigenschaft festlegen. Hierzu können Sie Starten des Programms, mit dem **-Dqpid.sync\_veröffentlichen = WAHR** Javavm Option in der Befehlszeile legen Sie beim Starten der Anwendung. Mit dieser Option wird die Bibliothek, um den Anruf senden zurückgibt, bis zur Bestätigung empfangen wurde, die die Nachricht von Dienstbus akzeptiert wurde konfiguriert. Wenn der Sendevorgang ein Problem auftritt, wird eine **JMSException** ausgelöst. Es gibt zwei mögliche Ursachen: 
    1. Wenn das Problem aufgrund Dienstbus Ablehnen der bestimmten Nachricht gesendet wird, wird eine Ausnahme **MessageRejectedException** ausgelöst werden. Dieser Fehler wird vorübergehendes, oder da einige Probleme mit der Nachricht. Die empfohlene Vorgehensweise ist an mehreren Stellen versucht, um den Vorgang mit einer Back-off Logik zu wiederholen. Wenn das Problem weiterhin besteht, sollte die Nachricht mit einem Fehler lokal angemeldet verworfen werden. Es gibt keine müssen die **JMS-Verbindung**, **Sitzung**oder **MessageProducer** Objekte in dieser Situation neu zu erstellen. 
    2. Wenn das Problem aufgrund Dienstbus, schließen den Link AMQP ist, wird eine Ausnahme **InvalidDestinationException** ausgelöst werden. Dies kann durch ein vorübergehendes Problem, oder die Nachrichtenentität, der gelöscht werden. In beiden Fällen sollte der **JMS-Verbindung**, **Sitzung**und **MessageProducer** Objekte neu erstellt werden. Wenn die Bedingung Fehler vorübergehende wurde, wird dieser Vorgang später erfolgreich durchgeführt werden. Wenn die Entität gelöscht wurde, werden der Fehler permanent.

## <a name="messaging-between-net-and-jms"></a>Zwischen .NET und JMS-Messaging

### <a name="message-bodies"></a>Nachrichtentext

JMS definiert fünf verschiedene Message Dateitypen: **BytesMessage**, **MapMessage**, **ObjectMessage**, **TextMessage**und **StreamMessage**. Der Bus .NET API verfügt über eine Kernaussage Typ [BrokeredMessage][].

#### <a name="jms-to-service-bus-net-api"></a>JMS zu Dienstbus .NET-API

In den folgenden Abschnitten zeigen, wie Nachrichten aller JMS-Nachrichtentypen von .NET nutzen. Beispiel für **ObjectMessage** wurde als Textkörper einer **ObjectMessage** ein serialisierbares Objekt in der Programmiersprache enthält, nicht durch eine Anwendung .NET Ingenieure ist nicht eingeschlossen.

##### <a name="bytesmessage"></a>BytesMessage

Im folgende Code veranschaulicht, wie Text eines Objekts **BytesMessage** mithilfe der Dienst Bus .NET APIs nutzen.

```
Stream stream = message.GetBody<Stream>();
int streamLength = (int)stream.Length;

byte[] byteArray = new byte[streamLength];
stream.Read(byteArray, 0, streamLength);

Console.WriteLine("Length = " + streamLength);
for (int i = 0; i < stream.Length; i++)
{
  Console.Write("[" + (sbyte) byteArray[i] + "]");
}
```

##### <a name="mapmessage"></a>MapMessage

Im folgende Code veranschaulicht, wie Text eines Objekts **MapMessage** mithilfe der Dienst Bus .NET APIs nutzen. In diesem Code durchläuft die Elemente der Karte, den Namen und den Wert der einzelnen Elemente anzeigen aus.

```
Dictionary<String, Object> dictionary = message.GetBody<Dictionary<String, Object>>();

foreach (String mapItemName in dictionary.Keys)
{
  Object mapItemValue = null;
  if (dictionary.TryGetValue(mapItemName, out mapItemValue))
  {
    Console.WriteLine(mapItemName + ":" + mapItemValue);
  }
}
```

##### <a name="streammessage"></a>StreamMessage

Im folgende Code veranschaulicht, wie Text eines Objekts **StreamMessage** mithilfe der Dienst Bus .NET APIs nutzen. Dieser Code sind alle Elemente aus dem Stream, zusammen mit ihren Typen aufgeführt.

```
List<Object> list = message.GetBody<List<Object>>();

foreach (Object item in list)
{
  Console.WriteLine(item + " (" + item.GetType() + ")");
}
```

##### <a name="textmessage"></a>TextMessage

Im folgende Code veranschaulicht, wie Text eines Objekts **TextMessage** mithilfe der Dienst Bus .NET APIs nutzen. Dieser Code zeigt die Textzeichenfolge, die in den Textbereich der Nachricht enthalten sind.

```
Console.WriteLine("Text: " + message.GetBody<String>());
```

#### <a name="service-bus-net-apis-to-jms"></a>Dienstbus .NET APIs zu JMS

In den folgenden Abschnitten zeigen, wie eine Anwendung .NET eine Nachricht erstellen kann, die in JMS in den verschiedenen Arten von JMS-Nachricht empfangen werden. Beispiel für **ObjectMessage** wurde als Textkörper einer **ObjectMessage** ein serialisierbares Objekt in der Programmiersprache enthält, nicht durch eine Anwendung .NET Ingenieure ist nicht eingeschlossen.

##### <a name="bytesmessage"></a>BytesMessage

Im folgende Code veranschaulicht, wie ein [BrokeredMessage][] Objekt in .NET zu erstellen, die von einem JMS-Client als eine **BytesMessage**empfangen wird.

```
byte[] bytes = { 33, 12, 45, 33, 12, 45, 33, 12, 45, 33, 12, 45 };
message = new BrokeredMessage(bytes);
```

##### <a name="streammessage"></a>StreamMessage

Mit dem folgende Code veranschaulicht, wie ein [BrokeredMessage][] Objekt in .NET erstellen, die von einem JMS-Client als eine **StreamMessage**erhält.

```
List<Object> list = new List<Object>();
list.Add("String 1");
list.Add("String 2");
list.Add("String 3");
list.Add((double)3.14159);
message = new BrokeredMessage(list);
```

##### <a name="textmessage"></a>TextMessage

Der folgende Code wird das Textkörper einer **TextMessage** nutzen mithilfe der Bus .NET API veranschaulicht. Dieser Code zeigt die Textzeichenfolge, die in den Textbereich der Nachricht enthalten sind.

```
message = new BrokeredMessage("this is a text string");
```

### <a name="application-properties"></a>Anwendungseigenschaften

####<a name="jms-to-service-bus-net-apis"></a>JMS zu Service Bus .NET-APIs

JMS-Nachrichten unterstützen Anwendungseigenschaften die folgenden Arten: **boolean**, **Byte**, **short**, **Int**, **lange**, **Pufferzeiten**, **doppelte**und **Zeichenfolge**. Der folgende Java-Code veranschaulicht zum Festlegen von Eigenschaften für eine Nachricht mit jeder dieser Eigenschaftstypen.

```
message.setBooleanProperty("TestBoolean", true); 
message.setByteProperty("TestByte", (byte) 33); 
message.setDoubleProperty("TestDouble", 3.14159D); 
message.setFloatProperty("TestFloat", 3.13159F); 
message.setIntProperty("TestInt", 100); 
message.setStringProperty("TestString", "Service Bus");
```

Eigenschaften der Nachricht Anwendung werden in den Dienst Bus .NET-APIs in der **Properties** -Auflistung des [BrokeredMessage][]übertragen. Im folgende Code veranschaulicht, wie die Anwendungseigenschaften von einer Nachricht von einem JMS-Client zu lesen.

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

Die folgende Tabelle zeigt die Typen von .NET wie die JMS-Eigenschaftentypen zuordnen.

| Typ des JMS-Eigenschaft | Eigenschaftentyp .NET |
|-------------------|--------------------|
| Byte-Zeichen              | SByte              |
| Ganze Zahl           | Ganzzahl                |
| Frei verschieben             | frei verschieben              |
| Double            | Doppelte             |
| Boolesch           | bool               |
| Zeichenfolge            | Zeichenfolge             |

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

Der folgende Java-Code veranschaulicht, wie die Anwendungseigenschaften von einer Nachricht von einem Dienst Bus zu lesen.

```
Enumeration propertyNames = message.getPropertyNames(); 
while (propertyNames.hasMoreElements()) 
{ 
  String name = (String) propertyNames.nextElement(); 
  Object value = message.getObjectProperty(name); 
  System.out.println(name + ": " + value + " (" + value.getClass() + ")"); 
}
```

Die folgende Tabelle zeigt, wie die Typen von .NET den Typen der JMS-Eigenschaft zuordnen.

| Eigenschaftentyp .NET | Typ des JMS-Eigenschaft | Notizen                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Byte-Zeichen               | UnsignedByte      | -                                                                                                                                                                      |
| SByte              | Byte-Zeichen              | -                                                                                                                                                                     |
| Zeichen               | Zeichen         | -                                                                                                                                                                     |
| kurze              | Kurze             | -                                                                                                                                                                     |
| ushort             | UnsignedShort     | -                                                                                                                                                                     |
| Ganzzahl                | Ganze Zahl           | -                                                                                                                                                                     |
| uint               | UnsignedInteger   | -                                                                                                                                                                     |
| lange               | Lange              | -                                                                                                                                                                     |
| ulong              | UnsignedLong      | -                                                                                                                                                                     |
| frei verschieben              | Frei verschieben             | -                                                                                                                                                                     |
| Doppelte             | Double            | -                                                                                                                                                                     |
| Dezimalzahl            | BigDecimal        | -                                                                                                                                                                     |
| bool               | Boolesch           | -                                                                                                                                                                     |
| GUID               | UUID              | -                                                                                                                                                                     |
| Zeichenfolge             | Zeichenfolge            | -                                                                                                                                                                     |
| "DateTime"           | Datum              | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | DateTimeOffset.UtcTicks AMQP Typ zugeordnet:<type name=”datetime-offset” class=restricted source=”long”><descriptor name=”com.microsoft:datetime-offset” /></type> |
| TimeSpan           | DescribedType     | Timespan.Ticks AMQP Typ zugeordnet:<type name=”timespan” class=restricted source=”long”><descriptor name=”com.microsoft:timespan” /></type>                        |
| URI                | DescribedType     | Uri.AbsoluteUri AMQP Typ zugeordnet:<type name=”uri” class=restricted source=”string”><descriptor name=”com.microsoft:uri” /></type>                               |

### <a name="standard-headers"></a>Standard-Kopfzeilen

Die folgende Tabelle enthält die JMS-Standard Überschriften und die Standardeigenschaften [BrokeredMessage][] wie werden mithilfe der AMQP 1.0 zugeordnet.

#### <a name="jms-to-service-bus-net-apis"></a>JMS zu Service Bus .NET-APIs

| JMS              | Dienstbus .NET               | Notizen                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|------------------|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| JMSCorrelationID | Message.CorrelationID          | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSDeliveryMode  | Zurzeit nicht verfügbar        | Dienstbus unterstützt nur dauerhafte Nachrichten. beispielsweise DeliveryMode.PERSISTENT, unabhängig davon, was angegeben ist.                                                                                                                                                                                                                                                                                                                                                         |
| JMSDestination   | Message.To                     | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSExpiration    | Nachricht. TimeToLive            | Konvertierung                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSMessageID     | Message.MessageID              | Standardmäßig wird JMSMessageID im Binärformat in der Nachricht AMQP codiert. Wandelt die nach Erhalt des binäre Nachrichten-Id die .NET Client-Bibliothek in einer Zeichenfolge Darstellung basierend auf der Unicode-Werte der Bytes. Zum Wechseln von der JMS-Bibliothek zum Verwenden der Zeichenfolge Nachrichten-Ids Anfügen der "Binärzahl-Nachrichten-ID = False" Zeichenfolge der Abfrageparameter der JNDI ConnectionURL. Beispiel: “amqps://[username]:[password]@[namespace].servicebus.windows.net? Binärzahl-Nachrichten-ID = False ". |
| JMSPriority      | Zurzeit nicht verfügbar        | Nachrichtenpriorität unterstützt Dienstbus nicht.                                                                                                                                                                                                                                                                                                                                                                                                                             |
| JMSRedelivered   | Zurzeit nicht verfügbar        | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSReplyTo       | Nachricht. ReplyTo               | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| JMSTimestamp     | Message.EnqueuedTimeUtc        | Konvertierung                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSType          | Message.Properties ["Jms-Typ"] | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

#### <a name="service-bus-net-apis-to-jms"></a>Dienstbus .NET APIs zu JMS

| Dienstbus .NET        | JMS              | Notizen                   |
|-------------------------|------------------|-------------------------|
| ContentType             | -                  | Zurzeit nicht verfügbar |
| CorrelationId           | JMSCorrelationID | -                         |
| EnqueuedTimeUtc         | JMSTimestamp     | Konvertierung              |
| Beschriftung                   | n/v              | Zurzeit nicht verfügbar |
| Nachrichten-ID               | JMSMessageID     | -                         |
| ReplyTo                 | JMSReplyTo       | -                         |
| ReplyToSessionId        | n/v              | Zurzeit nicht verfügbar |
| ScheduledEnqueueTimeUtc | n/v              | Zurzeit nicht verfügbar |
| SessionId               | n/v              | Zurzeit nicht verfügbar |
| TimeToLive              | JMSExpiration    | Konvertierung              |
| An                      | JMSDestination   | -                         |

## <a name="unsupported-features-and-restrictions"></a>Nicht unterstützte Features und Einschränkungen

Die folgenden Einschränkungen bei der Verwendung von JMS über AMQP 1.0 mit Dienstbus vorhanden ist:

-   Pro Sitzung ist nur eine **MessageProducer** oder **MessageConsumer** zulässig. Wenn Sie mehrere **MessageProducer** oder **MessageConsumer** Objekte in einer Anwendung erstellen möchten, erstellen Sie für jede von ihnen dedizierte Sitzungen.

-   Veränderliche Thema Abonnements werden derzeit nicht unterstützt.

-   **MessageSelector** Objekte werden nicht unterstützt.

-   Temporäre Ziele; Beispielsweise werden **TemporaryQueue** oder **TemporaryTopic**, nicht unterstützt, zusammen mit den **QueueRequestor** und **TopicRequestor** APIs, die sie verwenden.

-   Durchgeführte Sitzungen werden nicht unterstützt.

-   Verteilte Transaktionen werden nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

Möchten Sie mehr erfahren? Besuchen Sie die folgenden Links:

- [Service Bus AMQP (Übersicht)]
- [AMQP in Dienstbus für WindowsServer]

[AMQP in Dienstbus für WindowsServer]: https://msdn.microsoft.com/library/dn574799.aspx
[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

[Service Bus AMQP (Übersicht)]: service-bus-amqp-overview.md
[Azure-portal]: https://portal.azure.com
