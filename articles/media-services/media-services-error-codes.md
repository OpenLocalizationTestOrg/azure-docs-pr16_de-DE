<properties
    pageTitle="Azure Media-Dienste Fehlercodes | Microsoft Azure"
    description="Das Thema bietet einen Überblick über Fehlercodes Azure Media-Dienste."
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

# <a name="azure-media-services-error-codes"></a>Fehlercodes für Azure Media-Dienste

Wenn Sie Microsoft Azure Media-Dienste verwenden, erhalten Sie HTTP-Fehlercodes aus dem Dienst je nach Problemen, wie z. B. Authentifizierungstoken Ablauf von Aktionen, die Media-Dienste nicht unterstützt werden. Im folgenden finden eine Liste der **http-Fehlercodes** , die von Media-Dienste und den möglichen Ursachen für diese zurückgegeben werden kann.  
  
## <a name="400-bad-request"></a>400 Ungültige Anforderung

Die Anforderung enthält ungültige Daten und zurückgewiesen aus einem der folgenden Gründe:

- Eine nicht unterstützte API Version angegeben ist. Die neuste Version finden Sie unter [Einrichten für die Entwicklung von Medien Services REST API](media-services-rest-how-to-use.md).
- Die API-Version von Media-Dienste ist nicht angegeben. Informationen zum Angeben der Version API finden Sie unter [Herstellen einer Verbindung mit Media Services API REST Media-Dienste](media-services-rest-connect-programmatically.md). 
   
    >[AZURE.NOTE] Wenn Sie die .NET oder Java SDKs für die Verbindung mit Media-Dienste verwenden, wird die API-Version für Sie angegeben, wenn Sie versuchen, und führen Sie einige Schritte gegen Media-Dienste.
- Eine nicht definierte Eigenschaft wurde angegeben. Der Name der Eigenschaft ist in der Fehlermeldung. Nur diejenigen Eigenschaften, die eine bestimmte Entität gehören können angegeben werden muss. Eine Liste von Personen und deren Eigenschaften finden Sie unter [Azure Media Services REST-API-Referenz](http://msdn.microsoft.com/library/azure/hh973617.aspx) .
- Ein ungültiger Eigenschaftswert wurde angegeben. Der Name der Eigenschaft ist in der Fehlermeldung. Wird der vorherigen Link für gültige Eigenschaftentypen und deren Werte angezeigt.
- Ein Eigenschaftswert ist nicht vorhanden und ist erforderlich.
- Teil der angegebenen URL enthält einen ungültigen Wert an.
- Es wurde versucht, eine WriteOnce Eigenschaft zu aktualisieren.
- Es wurde versucht, ein Projekt zu erstellen, die eine Eingabe Anlage mit einer primären AssetFile hat, die nicht angegeben wurde oder konnte nicht ermittelt werden.
- Es wurde versucht, ein SAS Locator zu aktualisieren. SAS Locator können nur erstellt oder gelöscht werden soll. Streaming Locator aktualisiert werden kann. Weitere Informationen finden Sie unter [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).
- Ein nicht unterstützter Vorgang oder eine Abfrage wurde übermittelt. 

## <a name="401-unauthorized"></a>401 nicht autorisiert

Die Anfrage konnte nicht authentifiziert werden (bevor diese autorisiert werden können) aus einem der folgenden Gründe:

- Fehlende Authentifizierungskopfzeile.
- Fehlerhafte Authentifizierung Header-Wert.
    - Das Token ist abgelaufen. Wenn die REST-APIs direkt verwenden zu können, finden Sie unter [Herstellen einer Verbindung mit Media-Dienste, mit der Media Services REST-API](media-services-rest-connect_programmatically.md) erfahren, wie ein neues Authentifizierungstoken generieren. Wenn Sie die .NET oder Java SDKs verwenden, erstellen Sie ein Objekt CloudMediaContext oder MediaContract, um das Token zu generieren. Weitere Informationen hierzu finden Sie unter [Herstellen einer Verbindung mit Media-Dienste mit dem Media Services SDK für .NET](media-services-dotnet-connect-programmatically.md).
    - Das Token enthält eine ungültige Signatur.</li></ul></li></ul>

## <a name="403-forbidden"></a>403 Verboten

Die Anforderung darf nicht aus einem der folgenden Gründe:

- Das Media-Dienste-Konto kann nicht gefunden werden oder wurde gelöscht.
- Das Konto Media-Dienste ist deaktiviert und der Anforderungstyp ist nicht HTTP GET. Dienstvorgänge werden auch eine 403 Antwort zurückgegeben.
- Das Authentifizierungstoken enthält keine Anmeldeinformationen des Benutzers: Kontoname und/oder SubscriptionId. Sie finden diese Informationen in der Benutzeroberfläche von Medien Services-Erweiterung für Ihr Konto Media-Dienste im Verwaltungsportal Azure.
- Ressource kann nicht zugegriffen werden.
    - Es wurde versucht, ein MediaProcessor zu verwenden, die für Ihr Konto Media-Dienste nicht verfügbar ist.
    - Es wurde versucht, eine Auftragsvorlage von Medien Diensten definierte aktualisieren.
    - Es wurde versucht, einige andere Media-Dienste des Kontos Locator zu überschreiben.
    - Es wurde versucht, einige andere Media-Dienste des Kontos ContentKey zu überschreiben.

- Die Ressource konnte nicht aufgrund einer Dienst Kontingent erstellt werden, die für das Konto Media-Dienste erreicht wurde. Weitere Informationen zu den Dienst Kontingenten finden Sie unter [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md).

## <a name="404-not-found"></a>404 nicht gefunden

Die Anforderung ist für eine Ressource aus einem der folgenden Gründe nicht zulässig:

- Es wurde versucht, eine Entität aktualisieren, die nicht vorhanden ist.
- Es wurde versucht, ein Element zu löschen, die nicht vorhanden ist.
- Es wurde versucht, ein Element mit einem Link zu einer Entität erstellen, die nicht vorhanden ist.
- Es wurde versucht, um eine Entität abzurufen, die nicht vorhanden ist.
- Es wurde versucht, ein Speicherkonto angeben, die nicht mit dem Media-Dienste-Konto verknüpft ist.  

## <a name="409-conflict"></a>409 Konflikt

Die Anforderung darf nicht aus einem der folgenden Gründe:

- Mehrere AssetFile hat den angegebenen Namen in der Anlage.
- Es wurde versucht, einen zweiten primären AssetFile innerhalb der Anlage zu erstellen.
- Es wurde versucht, erstellen Sie eine ContentKey mit der angegebenen Id wird bereits verwendet.
- Es wurde versucht, erstellen Sie einen Locator mit der angegebenen Id wird bereits verwendet.
- Mehrere IngestManifestFile hat den angegebenen Namen in der IngestManifest.
- Es wurde versucht, zum Verknüpfen von einer zweiten Speicher Verschlüsselung ContentKey auf die Anlage Speicher verschlüsselt.
- Es wurde versucht, die gleichen ContentKey auf die Anlage zu verknüpfen.
- Es wurde versucht, einen Locator auf eine Anlage erstellen, deren Speichercontainer fehlt oder ist nicht mehr der Anlage zugeordnet.
- Es wurde versucht, einen Locator auf eine Anlage erstellen, die 5 Locator bereits verwendet wurde. (Azure-Speicher erzwingt der maximal fünf Richtlinien für den freigegebenen Zugriff auf eine Speichercontainer.)
- Verknüpfen eines Wirtschaftsguts Speicher-Konto zu einer IngestManifestAsset ist nicht das von der übergeordneten IngestManifest verwendete Speicherkonto entspricht.  

## <a name="500-internal-server-error"></a>500 Interner Serverfehler

Während der Verarbeitung der Anfrage stößt Media-Dienste einige Fehler, der verhindert, die Verarbeitung nicht fortgesetzt werden kann dass. Dies kann einen der folgenden Gründe haben:

- Erstellen einer Anlage oder Auftrag fehlschlägt, weil das Media-Dienste-Konto Dienst Kontingentinformationen vorübergehend nicht verfügbar ist.
- Erstellen eines Anlage oder IngestManifest BLOB-Speicher Containers fehlschlägt, weil der Firma Speicher Kontoinformationen vorübergehend nicht verfügbar ist.
- Andere unerwarteter Fehler. 

## <a name="503-service-unavailable"></a>503 Dienst nicht verfügbar

Der Server ist derzeit keine Besprechungsanfragen empfangen. Dieser Fehler kann durch übermäßige Anfragen zum Dienst verursacht werden. Media-Dienste begrenzungsebene Verfahren schränkt die Ressource: Einsatz für Applikationen, die übermäßige Anforderung an den Dienst vornehmen.

>[AZURE.NOTE]Überprüfen Sie die Fehlermeldung und die Codezeichenfolge zurück, um erhalten ausführliche Informationen zu den Grund, dass Sie den Fehler 503 verfügen. Dieser Fehler bedeutet nicht unbedingt begrenzungsebene.

Beschreibungen der möglichen Status sind:

- "Server ist ausgelastet. Vorherige Strecken von diese Art der Anforderung benötigte mehr als {0} Sekunden."
- "Server ist ausgelastet. Mehr als {0} Anforderungen pro Sekunde können gedrosselt werden."
- "Server ist ausgelastet. Mehr als {0} Anfragen innerhalb von {1} Sekunden gedrosselt werden können."

Zur Behandlung dieses Fehlers, empfehlen wir die Verwendung von Logik exponentiellen Back-off Wiederholungsversuche. Dies bedeutet zunehmend länger wartet zwischen Wiederholungsversuche für aufeinander folgende Fehlerantworten verwenden.  Weitere Informationen finden Sie unter [Vorübergehende Fehlerstrukturanalyse Handling Application Block](https://msdn.microsoft.com/library/hh680905.aspx). 

>[AZURE.NOTE]Wenn Sie [Azure Media Services SDK für .net](https://github.com/Azure/azure-sdk-for-media-services/tree/master)verwenden, wurde die Logik "Wiederholen" für den Fehler 503 vom SDK implementiert.  
  
## <a name="see-also"></a>Siehe auch  

[Fehlercodes Management Media-Dienste](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Nächste Schritte

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Angeben von feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
