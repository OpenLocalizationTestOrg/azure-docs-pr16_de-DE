<properties 
    pageTitle="Service Bus mit .NET und AMQP 1.0 | Microsoft Azure"
    description="Verwenden von .NET Dienstbus mit AMQP"
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
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-net-with-amqp-10"></a>Verwenden von .NET Dienstbus mit AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

## <a name="downloading-the-service-bus-sdk"></a>Herunterladen der Dienstbus SDK

Unterstützung für AMQP 1.0 steht im Dienst Bus SDK Version 2.1 oder höher. Sie können sicherstellen, dass Sie die neueste Version verfügen, indem Sie die Dienstbus Bits von [NuGet][]herunterladen.

## <a name="configuring-net-applications-to-use-amqp-10"></a>Konfigurieren von .NET Clientanwendungen AMQP 1.0 verwenden

Standardmäßig kommuniziert Service Bus .NET Clientbibliothek Dienstbus Dienst mit einer dedizierten SOAP-Protokoll ein. Um statt der standardmäßigen AMQP 1.0 verwendet erfordert Protokoll explizite Konfiguration auf die Verbindungszeichenfolge Dienstbus aus, wie im nächsten Abschnitt beschrieben. Als diese Änderung bleibt die Anwendungscode unverändert bei Verwendung von AMQP 1.0.

In der aktuellen Version gibt es ein paar API-Features, die nicht unterstützt werden, wenn Sie AMQP verwenden. Diese nicht unterstützte Features werden später im Abschnitt [nicht unterstützte Features, Einschränkungen, und unterschieden im Verhalten](#unsupported-features-restrictions-and-behavioral-differences)aufgeführt. Einige erweiterte Konfiguration Einstellungen können beim Sie auch eine andere Bedeutung AMQP verwenden.

### <a name="configuration-using-appconfig"></a>Verwenden von App.config Konfiguration

Es empfiehlt für Applikationen mit der Konfiguration App um Einstellungen zu speichern. Für Applikationen Dienstbus können Sie App.config zum Speichern der Einstellungen für den Dienstbus **ConnectionString** -Wert. Eine Beispiel-App ist wie folgt aus:

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="Microsoft.ServiceBus.ConnectionString"
                 value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
        </appSettings>
    </configuration>

Der Wert, der die `Microsoft.ServiceBus.ConnectionString` Einstellung ist die Verbindungszeichenfolge Dienstbus, mit dem die Verbindung zu Dienstbus konfigurieren. Das Format ist wie folgt aus:

    Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp

Wo `[namespace]` und `SharedAccessKey` vom [Azure-Portal][]abgerufen werden. Weitere Informationen finden Sie unter [Erste Schritte mit Servicebuswarteschlangen][].

Bei der Verwendung von AMQP anfügen Verbindungszeichenfolge zurück, wobei `;TransportType=Amqp`. Diese Notation informiert Clientbibliothek um seine Verbindung unter Verwendung AMQP 1.0 mit Service zu machen.

## <a name="message-serialization"></a>Meldungsserialisierung

Wenn das Standardprotokoll verwenden, ist das Standardserialisierungsverhalten der .NET Client-Bibliothek mit den [DataContractSerializer][] Typ eine Instanz [BrokeredMessage][] für Transport zwischen der Clientbibliothek und den Dienst Dienstbus serialisiert. Wenn Sie den AMQP Transportregel-Modus verwenden zu können, verwendet die Clientbibliothek AMQP Typsystem für die Serialisierung der [vermittelten Nachricht][BrokeredMessage] in eine Nachricht AMQP an. Diese Serialisierung ermöglicht die Nachricht empfangen und von einer empfangen-Anwendung, die potenziell auf einer anderen Plattform, beispielsweise eine Java-Anwendung, die die JMS-API verwendet ausgeführt wird, um Zugriff Dienstbus interpretiert werden.

Wenn Sie eine Instanz des [BrokeredMessage][] erstellen, können Sie ein Objekt .NET als Parameter an den Konstruktor um dienen als Textkörper der Nachricht bereitstellen. Objekte, die AMQP Grundtypen zugeordnet werden kann, wird der Textkörper in AMQP Datentypen serialisiert. Wenn das Objekt direkt in AMQP auf einen einfachen Typ zugeordnet werden kann; d. h., ein benutzerdefinierter Typ von der Anwendung, definiert werden, und klicken Sie dann das Objekt serialisiert wird mit der [DataContractSerializer][]und die serialisierten Bytes in einer AMQP Datennachricht gesendet werden.

Um Interoperabilität mit nicht .NET Clients zu erleichtern, verwenden Sie nur .NET Dateitypen, die direkt in AMQP Typen für den Textkörper der Nachricht serialisiert werden können. Die folgende Tabelle enthält diese Dateitypen und die entsprechende Zuordnung zu AMQP Typsystem.

| .NET Textkörper Objekttyp          | Zugeordnete AMQP Typ                          | AMQP Abschnitt Texttyp                                                                                                                                    |
|--------------------------------|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| bool                           | Boolesch                                   | AMQP Wert                                                                                                                                                |
| Byte-Zeichen                           | ubyte                                     | AMQP Wert                                                                                                                                                |
| ushort                         | ushort                                    | AMQP Wert                                                                                                                                                |
| uint                           | uint                                      | AMQP Wert                                                                                                                                                |
| ulong                          | ulong                                     | AMQP Wert                                                                                                                                                |
| SByte                          | Byte-Zeichen                                      | AMQP Wert                                                                                                                                                |
| kurze                          | kurze                                     | AMQP Wert                                                                                                                                                |
| Ganzzahl                            | Ganzzahl                                       | AMQP Wert                                                                                                                                                |
| lange                           | lange                                      | AMQP Wert                                                                                                                                                |
| frei verschieben                          | frei verschieben                                     | AMQP Wert                                                                                                                                                |
| Doppelte                         | Doppelte                                    | AMQP Wert                                                                                                                                                |
| Dezimalzahl                        | decimal128                                | AMQP Wert                                                                                                                                                |
| Zeichen                           | Zeichen                                      | AMQP Wert                                                                                                                                                |
| "DateTime"                       | Zeitstempel                                 | AMQP Wert                                                                                                                                                |
| GUID                           | UUID                                      | AMQP Wert                                                                                                                                                |
| Byte]                         | Binärzahl                                    | AMQP Wert                                                                                                                                                |
| Zeichenfolge                         | Zeichenfolge                                    | AMQP Wert                                                                                                                                                |
| System.Collections.IList       | Liste                                      | AMQP Wert: in der Auflistung enthaltenen Elemente können nur diejenigen sein, die in dieser Tabelle definiert sind.                                                             |
| System.Array                   | Matrix                                     | AMQP Wert: in der Auflistung enthaltenen Elemente können nur diejenigen sein, die in dieser Tabelle definiert sind.                                                             |
| System.Collections.IDictionary | Karte                                       | AMQP Wert: in der Auflistung enthaltenen Elemente können nur diejenigen sein, die in dieser Tabelle definiert sind. Hinweis: nur Zeichenfolge Tasten werden unterstützt.                        |
| URI                            | Zeichenfolge beschrieben (siehe die folgende Tabelle) | AMQP Wert                                                                                                                                                |
| DateTimeOffset                 | Lange beschrieben (siehe die folgende Tabelle)   | AMQP Wert                                                                                                                                                |
| TimeSpan                       | Lange beschrieben wird (siehe folgendes)         | AMQP Wert                                                                                                                                                |
| Stream                         | Binärzahl                                    | AMQP Daten (möglicherweise mehrere sein). In den Abschnitten Daten enthalten die unformatierten Bytes, die aus dem Streamobjekt lesen.                                                           |
| Anderes Objekt                   | Binärzahl                                    | AMQP Daten (möglicherweise mehrere sein). Enthält die serialisierte Binärdatei des Objekts, das die DataContractSerializer oder ein Serialisierungsprogramm von der Anwendung bereitgestellte verwendet. |

| .NET Typ      | Zugeordnete AMQP beschriebenen Typ                                                                                              | Notizen                   |
|----------------|-------------------------------------------------------------------------------------------------------------------------|-------------------------|
| URI            | `<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>`                       | Uri.AbsoluteUri         |
| DateTimeOffset | `<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` | DateTimeOffset.UtcTicks |
| TimeSpan       | `<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> `              | TimeSpan.Ticks          |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Nicht unterstützte Features und Einschränkungen Unterschieden im Verhalten

Die folgenden Features der Dienst Bus .NET API werden derzeit nicht unterstützt, wenn Sie AMQP verwenden:

-   Transaktionen.

-   Senden Sie per durchstellen Ziel

Es gibt einige kleine Unterschiede im Verhalten der Dienst Bus .NET API auch bei Verwendung von AMQP, im Vergleich zu den Standard-Protokoll:

-   Die [OperationTimeout][] -Eigenschaft wird ignoriert.

-   `MessageReceiver.Receive(TimeSpan.Zero)`wird als implementiert `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.

-   Sperren Token Nachrichten zukommen kann nur von den Empfängern der Nachricht ausgeführt werden, die ursprünglich die Nachrichten empfangen.

## <a name="controlling-amqp-protocol-settings"></a>Steuern der AMQP Protokoll Einstellungen

Die APIs .NET verfügen über mehrere Einstellungen, um das Verhalten der das Protokoll AMQP steuern:

-   **MessageReceiver.PrefetchCount**: steuert die ursprüngliche Kreditkarte auf einen Link angewendet. Die Standardeinstellung ist 0 an.

-   **MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize**: Steuerelemente, die die maximale Größe von AMQP Rahmen während der Aushandlung bei Verbindung angeboten Zeit zu öffnen. Die Standardeinstellung ist 65.536 Byte.

-   **MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval**: Übertragung batchable sind, wird dieser Wert gibt die maximale Verzögerung für Abschneidedispositionen senden. Vererbt vom Absender/Empfänger standardmäßig ein. Einzelne Absender/Empfänger können standardmäßig überschreiben, also 20 Millisekunden.

-   **MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity**: steuert, ob AMQP Verbindungen über eine SSL-Verbindung eingerichtet werden. Die Standardeinstellung ist **Wahr**.

## <a name="next-steps"></a>Nächste Schritte

Möchten Sie mehr erfahren? Besuchen Sie die folgenden Links:

- [Service Bus AMQP (Übersicht)]
- [AMQP 1.0-Unterstützung für aufgeteilt Servicebuswarteschlangen und Themen]
- [AMQP in Dienstbus für WindowsServer]

  [Erste Schritte mit Servicebuswarteschlangen]: service-bus-dotnet-get-started-with-queues.md
  [DataContractSerializer]: https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: https://msdn.microsoft.com/library/azure/jj657638.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.CreateMessageSender(System.String,System.String)]: https://msdn.microsoft.com/library/azure/jj657703.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure-portal]: https://portal.azure.com
[Service Bus AMQP (Übersicht)]: service-bus-amqp-overview.md
[AMQP 1.0-Unterstützung für aufgeteilt Servicebuswarteschlangen und Themen]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP in Dienstbus für WindowsServer]: https://msdn.microsoft.com/library/dn574799.aspx
