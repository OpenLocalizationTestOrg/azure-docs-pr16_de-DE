<properties
    pageTitle="Überwachen Sie Ihre APIs mit Azure-API Management, Ereignis Hubs und Runscope"
    description="Beispiel-Anwendung, die die Log-Eventhub-Richtlinie durch Verbindungslinien Azure-API Management, Azure Ereignis Hubs und Runscope für HTTP-Protokollierung und Überwachung"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Überwachen Sie Ihre APIs mit Azure-API Management, Ereignis Hubs und Runscope

Der [API-Verwaltungsdienst](api-management-key-concepts.md) bietet viele Funktionen verbessern die Verarbeitung von HTTP-Anforderungen an Ihre HTTP-API gesendet. Jedoch das Vorhandensein der Besprechungsanfragen und Antworten sind vorübergehend. Angefordert wird, und es durch den Verwaltungsdienst-API zu Ihrem Back-End-API fließt. Ihre API verarbeitet die Anforderung und eine Antwort fließt rückwärts an den Consumer API. Der API-Verwaltungsdienst bleibt einige wichtigen Statistiken über die APIs für die Anzeige im Portal Publisher-Dashboard, aber darüber hinaus, dass die Details nicht mehr vorhanden sind.

Mithilfe der [Log-Eventhub-](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [Richtlinie](api-management-howto-policies.md) im API-Verwaltungsdienst können Sie alle Details aus der Anfrage und Antwort auf einen [Azure Ereignis Hub](../event-hubs/event-hubs-what-is-event-hubs.md)senden. Es gibt verschiedene Gründe geben Sie zum Generieren von Ereignissen von HTTP-Nachrichten an Ihre APIs gesendet werden. Einige Beispiele für Prüfliste Updates, Nutzungsanalysen, Ausnahme warnen und 3rd Party Integration.   

Dieser Artikel beschreibt, wie erfassen die gesamte HTTP-Anforderung und Antwort Nachricht, senden Sie diese an ein Ereignis Hub und leitet dann die Nachricht an einen Dienst dritten, die HTTP-Protokollierung und Überwachen von Diensten bereitstellt.

## <a name="why-send-from-api-management-service"></a>Warum vom Verwaltungsdienst API senden?
Es ist möglich, HTTP-Middleware zu schreiben, die in HTTP-API Framework zum Erfassen von HTTP-Anfragen und Antworten, und legen diese in Protokollierung und Überwachung Systeme eingebunden werden kann. Der Nachteil dabei ist die HTTP-Middleware in die Back-End-API integriert werden muss und die Plattform der API muss übereinstimmen. Wenn es mehrere APIs gibt muss jeweils die Middleware bereitstellen. Es gibt häufig Gründe, warum die Back-End-APIs nicht aktualisiert werden kann.

Verwenden den Azure-API Verwaltungsdienst Integration mit Infrastruktur für die Protokollierung bietet eine zentrale und Plattform-unabhängige Lösung. Es ist darüber hinaus skalierbar, in einem Teil aufgrund der [Geo-Replikation](api-management-howto-deploy-multi-region.md) -Funktionen der Azure-API Management.

## <a name="why-send-to-an-azure-event-hub"></a>Warum an einen Azure Ereignis Hub senden?
Es ist eine angemessen, Fragen, warum Erstellen einer Richtlinie, die speziell für Azure Ereignis Hubs ist? Es gibt viele unterschiedliche Stellen, in denen ich meine Anfragen melden möchten. Nur senden die Anfragen direkt an das endgültige Ziel Warum nicht?  Dies ist eine Option aus. Sind jedoch bei der Durchführung einer API-Verwaltungsdienst-Protokollierung Anfragen notwendigen zu berücksichtigen ist, wie die Protokollierungsnachrichten die Leistung der API auswirkt. Schrittweise Erhöhung laden können durch verfügbaren Instanzen von Systemkomponenten erhöhen oder indem nutzen Geo-Replikation behandelt werden. Kurze Spitzen im Datenverkehr können jedoch Anfragen zu erheblich verzögert werden, wenn Anfragen zu Infrastruktur für die Protokollierung starten Auslastung verlangsamt führen.

Den untergeordneten Servern Azure-Ereignis soll eingehende großer Datenmengen, für den Umgang mit einer weit höhere Anzahl von Ereignissen als die Anzahl der HTTP-die meisten APIs Prozess Anfragen mit Kapazität. Das Ereignis Hub fungiert als eine Art anspruchsvolle Puffer zwischen Ihrem API-Verwaltungsdienst und der Infrastruktur, die gespeichert und die Nachrichten verarbeitet. Dadurch wird sichergestellt, dass Ihre API die Leistung aufgrund der Infrastruktur für die Protokollierung nicht beeinträchtigt wird.  

Nachdem die Daten an ein Ereignis Verteiler übergeben wurde erkannt wird beibehalten und wartet auf Ereignis Hub Nutzer verarbeitet. Wie diese verarbeitet werden nicht das Ereignis Hub interessiert, interessiert sie einfach sicherzustellen, dass die Nachricht erfolgreich übermittelt werden.     

Ereignis Hubs haben die Möglichkeit, Stream Ereignisse zu mehreren Consumer Gruppen. Dadurch können Ereignisse von vollständig unterschiedliche Systeme verarbeitet werden sollen. Dies ermöglicht die Unterstützung von vielen Integrationsszenarios ohne Erweiterung verzögert auf die Verarbeitung von Anforderungen API innerhalb der API Verwaltungsdienst mit, wie nur ein Ereignis generiert werden muss.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Einer Richtlinie für die Anwendung/HTTP-Nachrichten senden.
Ein Ereignis Hub akzeptiert Ereignisdaten als eine einfache Zeichenfolge an. Die Inhalte dieser Zeichenfolge sind vollständig auf Sie. Zum Packen von einer HTTP-Anforderung und senden es zur Ereignis Hubs möglich müssen wir die Zeichenfolge mit den Informationen Anforderung oder eine Antwort zu formatieren. In diesen Situationen befindet sich ein vorhandenes Format wir wiederverwenden können, und wir eventuell nicht auf eine eigene Analysecode schreiben. Anfangs eingestuft ich die Verwendung der [HAR](http://www.softwareishard.com/blog/har-12-spec/) zum Senden von HTTP-Anfragen und Antworten. Dieses Format ist jedoch optimiert für eine Abfolge von HTTP-Anfragen in einem JSON-basierten Format gespeichert. Sie enthalten eine Anzahl obligatorisch Elemente, die nicht benötigten Komplexität Szenario, die HTTP-Meldung über das Netzwerk übergeben hinzugefügt.  

Eine alternative Option wurde mit der `application/http` Medien Geben Sie in der HTTP-Spezifikation [RFC 7230](http://tools.ietf.org/html/rfc7230)beschriebenen. Diese Medientypen verwendet genaue dem Format, mit dem HTTP-Nachrichten tatsächlich über das Netzwerk senden, aber die gesamte Nachricht in den Textkörper der andere HTTP-Anforderung. In diesem Fall werden nur wir mithilfe des Textkörpers als unsere Nachricht um an Ereignis Hubs zu senden. Einfache, ein, die in [Microsoft ASP.NET-API 2.2 Webclient](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) Bibliotheken vorhanden, die diesem Format zu analysieren und konvertieren es in das systemeigene können Parser vorhanden ist `HttpRequestMessage` und `HttpResponseMessage` Objekte.

Damit diese Nachricht zu erstellen, die wir nutzen c# müssen die Grundlage [Richtlinienausdrücke](https://msdn.microsoft.com/library/azure/dn910913.aspx) in Azure-API Management aus. Hier ist die Richtlinie sendet eine HTTP-Anforderung Meldung an Azure Ereignis Hubs aus.

       <log-to-eventhub logger-id="conferencelogger" partition-id="0">
       @{
           var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                       context.Request.Method,
                                                       context.Request.Url.Path + context.Request.Url.QueryString);

           var body = context.Request.Body?.As<string>(true);
           if (body != null && body.Length > 1024)
           {
               body = body.Substring(0, 1024);
           }

           var headers = context.Request.Headers
                                  .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                  .ToArray<string>();

           var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

           return "request:"   + context.Variables["message-id"] + "\n"
                               + requestLine + headerString + "\r\n" + body;
       }
       </log-to-eventhub>

### <a name="policy-declaration"></a>Richtlinie Deklaration
Es ein paar bestimmte Punkte erwähnt zu dieser Richtlinienausdruck. Die Log-Eventhub-Richtlinie enthält ein Attribut namens Protokollierung-Id bezieht sich auf den Namen der Protokollierung, die innerhalb der API Verwaltungsdienst erstellt wurde. Die Details zum Einrichten eines Ereignisses Hub Protokollierung im API-Verwaltungsdienst finden Sie im Dokument [so zu Azure Ereignis Hubs in Azure-API Management Ereignisse zu melden](api-management-howto-log-event-hubs.md). Das zweite Attribut ist ein optionaler Parameter, der Ereignis Hubs die partitionieren weist, um die Nachricht zu speichern. Ereignis Hubs verwenden Partitionen Scalabilty aktivieren und mindestens zwei erforderlich. Die geordnete Übermittlung von Nachrichten ist nur innerhalb einer Partition garantiert. Wenn Ereignis-Hub in welcher Partition platzieren Sie die Nachricht nicht angewiesen, wird einen Round-Robert Algorithmus Verteilung die Last verwendet. Jedoch möglicherweise, die Teil unsere Nachrichten außerhalb der Reihenfolge verarbeitet werden.  

### <a name="partitions"></a>Partitionen
Um sicherzustellen, unsere Nachrichten an Nutzer in Reihenfolge geleitet, und nutzen Sie die Möglichkeit, beim Laden Verteilung Partitionen, habe ich eine Partition und HTTP-Antwort-Nachrichten an eine zweite Partition HTTP-Anforderungsnachrichten zu senden. Dadurch wird eine gerade Verteilung sichergestellt und wir können sicherstellen, dass alle Besprechungsanfragen in Reihenfolge behandelt werden und alle Antworten in der Reihenfolge behandelt werden. Es ist möglich, dass eine Antwort an, bevor Sie die entsprechende Anforderung verbraucht werden, aber wie das kein Problem aufgetreten ist, wir haben eine andere Verfahren für das Abgleichen anfordert, Antworten und wir wissen, dass Anfragen immer vor dem Antworten kommen.

### <a name="http-payloads"></a>HTTP-Fracht
Nach dem Erstellen der `requestLine` wir überprüfen, um festzustellen, ob die Anforderungstexts abgeschnitten werden soll. Hauptteil der Anforderung wird auf nur 1024 gekürzt. Dies kann erhöht werden, jedoch einzelne Ereignis Hub Nachrichten auf 256 KB begrenzt werden, sodass es wahrscheinlich ist, dass einige HTTP-Nachricht passt, die nicht in einer einzelnen Nachricht stellen. Bei der Protokollierung und Analytics kann sehr viel Informationen einfach die HTTP-Anforderungszeile und die Kopfzeilen abgeleitet werden. Darüber hinaus viele API Anfragen nur kleine Stellen zurück und somit der Verlust der Wert abgeschnitten große stellen Informationen ist ziemlich minimalen im Vergleich zu der Rückgang durchstellen, Verarbeitung und Kosten für die Speicherung alle Textinhalt zu belassen. Ein letzter Hinweis zum Verarbeiten von Text ist, dass wir übergeben müssen `true` zum As<string>Methode (), da wir gerade den Inhalt der Textkörper lesen aber wurde auch die Back-End-API zum Lesen des Textkörpers können sollen. Durch die Übergabe True, wenn diese Methode dazu führen, dass wir Textkörper gepuffert wird, damit sie ein zweites Mal gelesen werden kann. Dies ist wichtig, stets bewusst sein, wenn Sie über eine API verfügen, das sehr großer Dateien hochladen oder lange Umfragen verwendet. In diesen Fällen wäre es sinnvoll, zu vermeiden, lesen den Textkörper überhaupt.   

### <a name="http-headers"></a>HTTP-Header
HTTP-Header können einfach über in das Nachrichtenformat in einem einfachen Schlüsselwert Paar Format übertragen. Wir haben Angaben bestimmte Sicherheit vertrauliche, um zu vermeiden, unnötigerweise Verluste im Anmeldeinformationen zu entfernen. Es ist wahrscheinlich nicht, dass API Schlüsseln und anderen Anmeldeinformationen Analytics Zwecken verwendet werden sollte. Wunsch Analyse auf bestimmtes Produkt und der Benutzer kann diese verwenden und dann konnte abgerufen werden, die aus der `context` -Objekt, und fügen Sie, die die Nachricht.     
### <a name="message-metadata"></a>Nachrichtenmetadaten
Wenn Sie die vollständige Nachricht senden an den Hub Ereignis zu erstellen, ist nicht die erste Zeile tatsächlich Teil der `application/http` Nachricht. Die erste Zeile ist zusätzliche Metadaten aus, ob die Nachricht ist, eine Anforderung oder Antwortnachricht und eine Nachricht-Id, die verwendet wird, um die Anfragen zu Antworten zu koordinieren. Die Nachrichten-Id wird erstellt, mit einer anderen Richtlinie, die sieht wie folgt aus:

    <set-variable name="message-id" value="@(Guid.NewGuid())" />

Wir konnte die Anfragenachricht erstellt, gespeichert, die in einer Variablen, bis die Antwort zurückgegeben, und klicken Sie dann einfach gesendet wurde die Anforderung und Antwort einer einzelnen Nachricht haben. Jedoch erhalten senden die Anforderung und Antwort unabhängig voneinander und verwenden eine Nachricht-Id in den beiden zu koordinieren, wir ein wenig mehr Flexibilität in der Größe der Nachricht, die Möglichkeit, nutzen Sie die Vorteile von mehreren Partitionen bei weitgehender Reihenfolge der Nachrichten und die Anfrage in unseren Protokollierung Dashboard früher erscheint. Es gibt auch möglicherweise einige Szenarien, in dem eine gültige Antwort an den Hub Ereignis, möglicherweise aufgrund einer schwerwiegenden Anforderungsfehler in der API-Verwaltungsdienst nicht gesendeten ist jedoch weiterhin haben wir einen Eintrag der Anfrage.

Die Richtlinie zum Senden der Antwort HTTP-Nachricht sehr ähnlich der Anfrage und daher Konfiguration der Fertigstellung sieht wie folgt aus:

      <policies>
        <inbound>
            <set-variable name="message-id" value="@(Guid.NewGuid())" />
            <log-to-eventhub logger-id="conferencelogger" partition-id="0">
              @{
                  var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                              context.Request.Method,
                                                              context.Request.Url.Path + context.Request.Url.QueryString);

                  var body = context.Request.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Request.Headers
                                       .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                       .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                       .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "request:"   + context.Variables["message-id"] + "\n"
                                      + requestLine + headerString + "\r\n" + body;
              }
          </log-to-eventhub>
        </inbound>
        <backend>
            <forward-request follow-redirects="true" />
        </backend>
        <outbound>
            <log-to-eventhub logger-id="conferencelogger" partition-id="1">
              @{
                  var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                                      context.Response.StatusCode,
                                                      context.Response.StatusReason);

                  var body = context.Response.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Response.Headers
                                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                                  .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "response:"  + context.Variables["message-id"] + "\n"
                                      + statusLine + headerString + "\r\n" + body;
             }
          </log-to-eventhub>
        </outbound>
      </policies>

Die `set-variable` Richtlinie erstellt einen Wert, der durch die beiden zugänglich ist die `log-to-eventhub` Richtlinie in der `<inbound>` Abschnitt und die `<outbound>` Abschnitt.  

## <a name="receiving-events-from-event-hubs"></a>Empfang von Ereignissen Ereignis Hubs
Empfangen von Ereignissen aus Azure Ereignis Hub mit dem [AMQP Protokoll](http://www.amqp.org/). Das Microsoft-Dienstbus Team haben Client Bibliotheken zur Verfügung, die in Anspruch nehmen Ereignisse zu erleichtern. Es gibt zwei verschiedene Ansätze unterstützt, wird eine *Direkte Consumer* und anderen wird mithilfe der `EventProcessorHost` Class. Beispiele für diese beiden Ansätze finden Sie in das [Ereignis Hubs Programming Guide](../event-hubs/event-hubs-programming-guide.md). Ist die Kurzfassung der Unterschiede, `Direct Consumer` vollständig gesteuert, und die `EventProcessorHost` Teil der Arbeit Sanitär-für Sie aber von bestimmten Annahmen zu, wie Sie diese Ereignisse verarbeitet werden.  

### <a name="eventprocessorhost"></a>EventProcessorHost
In diesem Beispiel verwenden wir die `EventProcessorHost` zur Vereinfachung, es kann jedoch nicht die beste Wahl für dieses bestimmte Szenario. `EventProcessorHost`unterstützt die harte Arbeit der sicherzustellen, dass Ihnen keine Gedanken über den Threadingproblemen innerhalb einer bestimmten Ereignis Prozessorklasse zu. In diesem Szenario sind wir allerdings einfach die Nachricht in ein anderes Format konvertieren und an einen anderen Dienst mithilfe einer asynchronen Methode weiterleitet. Es ist nicht erforderlich zum Aktualisieren von freigegebenen Zustand und daher keine Threadingproblemen Risiko ein. In den meisten Fällen `EventProcessorHost` wahrscheinlich am besten geeignet ist, und es ist sicher die Option einfacher.     

### <a name="ieventprocessor"></a>IEventProcessor
Zentraler Bestandteil der bei Verwendung von `EventProcessorHost` besteht im Erstellen einer Implementierung von der `IEventProcessor` Schnittstelle der die Methode enthält `ProcessEventAsync`. Das wesentliche dieser Methode wird hier dargestellt:

  asynchrone Aufgabe IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages) {}

           foreach (EventData eventData in messages)
           {
               _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

               try
               {
                   var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
                   await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
               }
               catch (Exception ex)
               {
                   _Logger.LogError(ex.Message);
               }
           }
            ... checkpointing code snipped ...
        }

Eine Liste von Objekten EventData werden an die Methode übergeben, und wir durchlaufen Sie die in dieser Liste enthalten. Die Bytes der einzelnen Methoden werden in ein Objekt HttpMessage analysiert, und das Objekt wird eine Instanz des IHttpMessageProcessor übergeben.

### <a name="httpmessage"></a>HttpMessage
Die `HttpMessage` Instanz beinhaltet drei Datenelemente:

      public class HttpMessage
       {
           public Guid MessageId { get; set; }
           public bool IsRequest { get; set; }
           public HttpRequestMessage HttpRequestMessage { get; set; }
           public HttpResponseMessage HttpResponseMessage { get; set; }

        ... parsing code snipped ...

      }

Die `HttpMessage` Instanz enthält eine `MessageId` GUID, mit dem wir verbinden Sie die HTTP-Anforderung der entsprechenden HTTP-Antwort und der boolesche Wert, der angibt, wenn das Objekt eine Instanz von einer HttpRequestMessage und HttpResponseMessage enthält. Mithilfe der integrierten HTTP-Klassen aus `System.Net.Http`, ich konnte Nutzen der `application/http` Analysecode ein, die zu bieten hat `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
Die `HttpMessage` Instanz wird dann an die Implementierung von weitergeleitet `IHttpMessageProcessor` eine Oberfläche um die empfangen und Interpretation des Ereignisses aus Azure Ereignis Hub und die eigentliche Verarbeitung davon entkoppeln erstellt.


## <a name="forwarding-the-http-message"></a>HTTP-Nachricht weiterleiten
In diesem Beispiel entschieden ich habe, dass es interessant, drücken Sie die HTTP-Anforderung über zu [Runscope](http://www.runscope.com)wäre. Runscope ist ein cloudbasierte-Dienst, der sich auf HTTP für das Debuggen, Protokollierung und Überwachung. Sie haben eine kostenlose Ebene, damit es einfach ist, versuchen Sie es, und sie wir die HTTP-Anfragen in Echtzeit parallelen durch unsere API Verwaltungsdienst finden Sie unter können.

Die `IHttpMessageProcessor` Implementierung sieht wie folgt, aus.

      public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
       {
           private HttpClient _HttpClient;
           private ILogger _Logger;
           private string _BucketKey;
           public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
           {
               _HttpClient = httpClient;
               var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
               _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
               _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
               _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
               _Logger = logger;
           }

           public async Task ProcessHttpMessage(HttpMessage message)
           {
               var runscopeMessage = new RunscopeMessage()
               {
                   UniqueIdentifier = message.MessageId
               };

               if (message.IsRequest)
               {
                   _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
                   runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
               }
               else
               {
                   _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
                   runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
               }

               var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
               messagesLink.BucketKey = _BucketKey;
               messagesLink.RunscopeMessage = runscopeMessage;
               var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
               _Logger.LogDebug("Request sent to Runscope");
           }
       }

Ich habe eine [vorhandene Clientbibliothek für Runscope](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) nutzen, die Pushbenachrichtigungen erleichtert `HttpRequestMessage` und `HttpResponseMessage` Instanzen von deren Inbetriebnahme. Um die Runscope-API zugreifen benötigen Sie ein Konto und einen API-Schlüssel. Anweisungen für den Abruf von eines API-Schlüssels finden Sie in dem [Erstellen von Applications in Access Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) Screencast.

## <a name="complete-sample"></a>Vollständige Beispiel
Der [Quellcode](https://github.com/darrelmiller/ApimEventProcessor) und Tests für die Stichprobe sind auf Github. Sie benötigen einen [API-Verwaltungsdienst](api-management-get-started.md), [Einen verbundenen Ereignis Hub](api-management-howto-log-event-hubs.md)und ein [Speicher-Konto](../storage/storage-create-storage-account.md) zum Ausführen des Beispiels für sich selbst.   

Das Beispiel ist nur eine einfache Console-Anwendung, die überwacht für Ereignisse in Kürze von Ereignis-Hub wandelt sie in einer `HttpRequestMessage` und `HttpResponseMessage` Objekte und anschließend auf die Runscope-API weiterleitet.

In der folgenden Abbildung animierte sehen Sie eine Anforderung an eine API im Portal Entwicklertools Console-Anwendung, die mit der Nachricht empfangen, verarbeitet und weitergeleitet und dann die Anfrage und Antwort im Runscope Datenverkehr Inspektor angezeigt.

![Demo der Anforderung an Runscope weitergeleitet werden](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Zusammenfassung
Verwaltungsdienst Azure-API bietet ideale Ort zum Erfassen des HTTP-Datenverkehrs an und von Ihrem APIs unterwegs. Azure Ereignis Hubs ist eine Lösung hoch skalierbare, niedrige Kosten für die Datenverkehr erfassen und einzuziehen es in sekundäre Verarbeitungssysteme für die Protokollierung, Überwachung und andere ausgefeilte Analytics. Verbinden mit 3rd Party Datenverkehr Überwachen von Systemen wie Runscope eine einfache als ein paar Dutzend Codezeilen ist.

## <a name="next-steps"></a>Nächste Schritte
-   Weitere Informationen zu Azure Ereignis Hubs
    -   [Erste Schritte mit Azure Ereignis Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Empfangen von Nachrichten mit EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Ereignis Hubs programming guide](../event-hubs/event-hubs-programming-guide.md)
-   Erfahren Sie mehr über die Integration von Management-API und Ereignis Hubs
    -   [Wie melde Azure Ereignis Hubs in Azure-API Management Ereignisse](api-management-howto-log-event-hubs.md)
    -   [Ein Verweis auf Protokollierung Entität](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [Log-Eventhub-Richtlinie Bezug](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    