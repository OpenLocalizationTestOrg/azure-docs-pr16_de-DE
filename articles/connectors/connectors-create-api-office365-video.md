<properties
pageTitle="Verwenden den Office 365-Video Verbinder in Ihrer apps Logik | Microsoft Azure"
description="Erste Schritte mit der Office 365-Video Verbinder in Ihrer App für Microsoft Azure Service Logik apps"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office365-video-connector"></a>Erste Schritte mit der Office 365-Video-Anschluss
Verbinden Sie mit Office 365 Video erhalten Informationen zu Office 365 video, erhalten eine Liste von Videos und vieles mehr. Der Verbinder Office 365-Video kann aus verwendet werden:

- Logik apps 

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus. Dieser Verbinder wird auf alle vorherigen Schemaversionen nicht unterstützt.

Mit Office 365-Video können Sie folgende Aktionen ausführen:

- Erstellen Sie Ihrer Business Fluss basierend auf den Daten, die Sie von Office 365-Video zu gelangen. 
- Verwenden Sie die Aktionen, die Überprüfen des Status des Videos Portals, erhalten eine Liste aller video in einem Kanal und vieles mehr. Diese Aktionen erhalten eine Antwort, und nehmen Sie die Ausgabe dann für andere Aktionen verfügbar. Sie können beispielsweise den Verbinder Bing-Suchfeld zum Suchen nach Office 365-Videos verwenden, und verwenden Sie dann den Office 365-video-Verbinder, um Informationen zu diesem Video erhalten. Wenn Sie das Video Ihren Anforderungen entspricht, können Sie dieses Video auf Facebook Posten. 

Um einen Vorgang in Logik apps hinzuzufügen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

Der Office 365-Video Verbinder weist die folgenden Aktionen verfügbar. Es gibt keine Trigger aus.

| Trigger | Aktionen|
| --- | --- |
| Keine | <ul><li>Video-Portals Status überprüft</li><li>Alle sichtbaren Kanäle abrufen</li><li>Abrufen von Wiedergabe-Url des Manifests Azure Media-Dienste für ein video</li><li>Abrufen des Person Tokens zum Zugreifen auf das Video entschlüsseln können.</li><li>Ruft Informationen zu einer bestimmten office365 video</li><li>Listen, die die Office 365-Videos in einem Kanal präsentieren</li></ul>

Alle Connectors unterstützen die Daten in JSON und XML-Formaten. 

## <a name="create-a-connection-to-office365-video-connector"></a>Herstellen einer Verbindung mit Office 365-Video-Anschluss
Wenn Sie Ihre apps Logik dieser Verbinder hinzugefügt haben, müssen mit Ihrem Office 365-Video-Konto anmelden und Logik apps für die Verbindung zu Ihrem Konto zulassen.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]

Nachdem Sie die Verbindung zu erstellen, die Office 365-video-Eigenschaften wie den Namen des Mandanten eingeben oder channel-ID an. Die **REST-API-Referenz** in diesem Thema werden diese Eigenschaften beschrieben.

>[AZURE.TIP] Sie können diese dieselbe Verbindung mit Office 365-Video in andere apps Logik verwenden.

## <a name="swagger-rest-api-reference"></a>Swagger REST-API-Referenz
Version gilt: 1.0.

### <a name="checks-video-portal-status"></a>Video-Portals Status überprüft 
Prüft den video Portal Status aus, um festzustellen, ob video Dienste aktiviert sind.  
```GET: /{tenant}/IsEnabled``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Mandanten|Zeichenfolge|Ja|Pfad|keine|Der Mandanten Namen für das Verzeichnis der Benutzer ist Teil|


#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang wurde erfolgreich abgeschlossen|
|400|BadRequest|
|401|Nicht autorisierte|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|



### <a name="get-all-viewable-channels"></a>Alle sichtbaren Kanäle abrufen 
Ruft alle Kanäle, die der Benutzer verfügt über Zugriff auf anzeigen.  
```GET: /{tenant}/Channels``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Mandanten|Zeichenfolge|Ja|Pfad|keine|Der Mandanten Namen für das Verzeichnis der Benutzer ist Teil|


#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang wurde erfolgreich abgeschlossen|
|400|BadRequest|
|401|Nicht autorisierte|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|




### <a name="lists-all-the-office365-videos-present-in-a-channel"></a>Listen, die die Office 365-Videos in einem Kanal präsentieren 
Listen, die die Office 365-Videos in einem Kanal präsentieren.  
```GET: /{tenant}/Channels/{channelId}/Videos``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Mandanten|Zeichenfolge|Ja|Pfad|keine|Der Mandanten Namen für das Verzeichnis der Benutzer ist Teil|
|channelId|Zeichenfolge|Ja|Pfad|keine|Die Kanal-Id aus der Videos abgerufen werden müssen|


#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang wurde erfolgreich abgeschlossen|
|400|BadRequest|
|401|Nicht autorisierte|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|




### <a name="gets-information-about-a-particular-office365-video"></a>Ruft Informationen zu einer bestimmten office365 video 
Ruft Informationen zu einer bestimmten office365 video ab.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Mandanten|Zeichenfolge|Ja|Pfad|keine|Der Mandanten Namen für das Verzeichnis der Benutzer ist Teil|
|channelId|Zeichenfolge|Ja|Pfad|keine|Die Kanal-id|
|videoId|Zeichenfolge|Ja|Pfad|keine|Die video-id|


#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang wurde erfolgreich abgeschlossen|
|400|BadRequest|
|401|Nicht autorisierte|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|




### <a name="get-playback-url-of-the-azure-media-services-manifest-for-a-video"></a>Abrufen von Wiedergabe-Url des Manifests Azure Media-Dienste für ein video 
Erste Wiedergabe-Url des Manifests Azure Media-Dienste für ein Video an.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/playbackurl``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Mandanten|Zeichenfolge|Ja|Pfad|keine|Der Mandanten Namen für das Verzeichnis der Benutzer ist Teil|
|channelId|Zeichenfolge|Ja|Pfad|keine|Die Kanal-id|
|videoId|Zeichenfolge|Ja|Pfad|keine|Die video-id|
|streamingFormatType|Zeichenfolge|Ja|Abfrage|keine|Streaming Formattyp. 1: Glätten Streaming oder MPEG-Strich. 0 - HLS Streaming|


#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang wurde erfolgreich abgeschlossen|
|400|BadRequest|
|401|Nicht autorisierte|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|




### <a name="get-the-bearer-token-to-get-access-to-decrypt-the-video"></a>Abrufen des Person Tokens zum Zugreifen auf das Video entschlüsseln können. 
Abrufen des Person Tokens zum Zugreifen auf das Video entschlüsseln können.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/token```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Mandanten|Zeichenfolge|Ja|Pfad|keine|Der Mandanten Namen für das Verzeichnis der Benutzer ist Teil|
|channelId|Zeichenfolge|Ja|Pfad|keine|Die Kanal-id|
|videoId|Zeichenfolge|Ja|Pfad|keine|Die video-id|


#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang wurde erfolgreich abgeschlossen|
|400|BadRequest|
|401|Nicht autorisierte|
|404|Nicht gefunden|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


## <a name="object-definitions"></a>Objektdefinitionen

#### <a name="channel-channel-class"></a>Kanal: Channel Klasse

| Namen | Datentyp | Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|
|Titel|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|


#### <a name="video"></a>Video 

| Namen | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|
|Titel|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|
|CreationDate|Zeichenfolge|Nein|
|Besitzer|Zeichenfolge|Nein|
|ThumbnailUrl|Zeichenfolge|Nein|
|VideoUrl|Zeichenfolge|Nein|
|VideoDuration|ganze Zahl|Nein|
|VideoProcessingStatus|ganze Zahl|Nein|
|ViewCount|ganze Zahl|Nein|


## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).
