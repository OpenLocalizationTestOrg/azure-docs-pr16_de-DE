<properties
    pageTitle="Wiederholen Sie die Logik im Media Services-SDK für .NET | Microsoft Azure"
    description="Das Thema bietet einen Überblick über die Logik Wiederholungsversuche im Media Services-SDK für .NET."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>


# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>Wiederholen Sie die Logik im Media Services-SDK für .NET

Bei der Arbeit mit Microsoft Azure Services können vorübergehenden Fehler auftreten. Bei einer vorübergehenden, in den meisten Fällen nach ein paar Wiederholungsversuche, die der Vorgang erfolgreich ist Fehlfunktion. Das Media Services SDK für .NET implementiert die Logik Wiederholungsversuche zum vorübergehenden zugeordnet Ausnahmen und Fehler, die durch Webanfragen verursacht werden Fehler behandeln Ausführen von Abfragen, Speichern von Änderungen und Speichervorgänge.  Standardmäßig wird der Media Services SDK für .NET vier Wiederholungsversuche vor dem erneuten Auslösen der Ausnahme an Ihrer Anwendung ausgeführt. Der Code in Ihrer Anwendung muss diese Ausnahme dann ordnungsgemäß behandeln.  
  
 Im folgenden finden eine kurze Richtlinie Web anfordern, Speicher, Abfrage- oder SaveChanges Richtlinien:  
  
-   Die Richtlinie Speicher wird für BLOB-Speichervorgänge (Uploads oder Downloads von Dateien Anlage) verwendet.  
  
-   Die Richtlinie Web Request wird für generische Webanfragen (zum Beispiel für ein Authentifizierungstoken erste und das Beheben von des Benutzern Cluster Endpunkts) verwendet.  
  
-   Die Abfrage wird verwendet für die Abfrage Elemente aus REST (z. B. mediaContext.Assets.Where(...)).  
  
-   Die Richtlinie SaveChanges dient zum Ausführen nichts, die Daten innerhalb des Diensts (z. B. Erstellen einer Entität Aktualisieren einer Entität, eine Service-Funktion für einen Vorgang aufrufen) geändert.  
  
 In diesem Thema werden die Arten von Ausnahme, und Fehlercodes, die vom Medien Services SDK für .NET verarbeitet werden Wiederholen Logik.  
  
## <a name="exception-types"></a>Ausnahmetypen  

Die folgende Tabelle beschreibt die Ausnahmen, die die Media Services SDK für .NET verarbeitet oder behandelt nicht für einige Vorgänge, die möglicherweise vorübergehenden Fehler verursachen.  
  
Ausnahme|Web-Anforderung|Speicher|Abfrage|SaveChanges
----|------|----|---|---
WebException<br/>Weitere Informationen finden Sie im Abschnitt [WebException Statuscodes](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) .|Ja|Ja|Ja|Ja  
DataServiceClientException<br/> Weitere Informationen finden Sie unter [http-Fehlercodes Status](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Nein|Ja|Ja|Ja
DataServiceQueryException<br/> Weitere Informationen finden Sie unter [http-Fehlercodes Status](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Nein|Ja|Ja|Ja  
DataServiceRequestException<br/> Weitere Informationen finden Sie unter [http-Fehlercodes Status](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Nein|Ja|Ja|Ja  
DataServiceTransportException|Nein|Nein|Ja|Ja
TimeoutException|Ja|Ja|Ja|Nein
SocketException|Ja|Ja|Ja|Ja  
StorageException|Nein|Ja|Nein|Nein 
IOException|Nein|Ja|Nein|Nein
  
###  <a name="a-namewebexceptionstatusa-webexception-status-codes"></a><a name="WebExceptionStatus"></a>WebException Statuscodes  

In der folgenden Tabelle zeigt an, für welche WebException Fehlercodes die Logik Wiederholungsversuche implementiert wird. Die Enumeration [WebExceptionStatus](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) definiert die Statuscodes.  
  
Status|Web-Anforderung|Speicher|Abfrage|SaveChanges  
-----|-----------------|-------------|-----------|----------  
Verbindungsfehler|Ja|Ja|Ja|Ja
NameResolutionFailure|Ja|Ja|Ja|Ja  
ProxyNameResolutionFailure|Ja|Ja|Ja|Ja  
SendFailure|Ja|Ja|Ja|Ja
PipelineFailure|Ja|Ja|Ja|Nein  
ConnectionClosed|Ja|Ja|Ja|Nein  
KeepAliveFailure|Ja|Ja|Ja|Nein  
UnknownError ab|Ja|Ja|Ja|Nein 
ReceiveFailure|Ja|Ja|Ja|Nein  
RequestCanceled|Ja|Ja|Ja|Nein  
Timeout|Ja|Ja|Ja|Nein
Protokollfehler <br/>Die Wiederholung auf Protokollfehler wird durch den Umgang mit HTTP-Status Code gesteuert. Weitere Informationen finden Sie unter [http-Fehlercodes Status](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Ja|Ja|Ja|Ja|  
  
###  <a name="a-namehttpstatuscodea-http-error-status-codes"></a><a name="HTTPStatusCode"></a>HTTP-Status Fehlercodes  

Wenn Abfrage- und SaveChanges Vorgänge DataServiceClientException, DataServiceQueryException oder DataServiceQueryException lösen, wird der HTTP-Fehlercode Status in der Eigenschaft StatusCode zurückgegeben.  In der folgenden Tabelle zeigt an, für welche Fehlercodes die Logik Wiederholungsversuche implementiert wird.  
  
 
Status|Web-Anforderung|Speicher|Abfrage|SaveChanges 
---|----|----|----|----
401|Nein|Ja|Nein|Nein
403|Nein|Ja<br/>Behandeln von Wiederholungsversuche mit mehr wartet.|Nein|Nein  
408|Ja|Ja|Ja|Ja
429|Ja|Ja|Ja|Ja  
500|Ja|Ja|Ja|Nein  
502|Ja|Ja|Ja|Nein  
503|Ja|Ja|Ja|Ja  
504|Ja|Ja|Ja|Nein  
  
Wenn Sie nutzen möchten kann ein Blick auf die eigentliche Implementierung von Medien Services SDK für .NET Logik wiederholen, finden Sie unter [Azure Sdk für Medien Services](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
