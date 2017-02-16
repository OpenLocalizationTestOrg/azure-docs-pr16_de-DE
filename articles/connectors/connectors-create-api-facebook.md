<properties
    pageTitle="Hinzufügen den Facebook-Verbinder in Ihrer Apps Logik | Microsoft Azure"
    description="Übersicht über die Facebook-Connector mit den Parametern REST-API"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="08/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-facebook-connector"></a>Erste Schritte mit der Facebook-Verbinder
Verbinden mit Facebook und Bereitstellen einer Zeitachse, und erhalten einer Seite-Feeds und vieles mehr. 

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus.


Mit Facebook können Sie folgende Aktionen ausführen:

- Erstellen Sie Ihrer Business Fluss basierend auf den Daten, die Sie von Facebook zu gelangen. 
- Verwenden eines Triggers an, wenn Sie ein neuer Beitrag empfangen wird.
- Aktionen verwenden, die in der Zeitachse bereitstellen Abrufen einer Seite-Feeds und vieles mehr. Diese Aktionen erhalten eine Antwort, und nehmen Sie die Ausgabe dann für andere Aktionen verfügbar. Wenn auf der Zeitachse ein neuer Beitrag vorhanden ist, können Sie beispielsweise den Beitrag ausführen und schieben Sie ihn zu Ihrem Twitter-Feed. 



Um einen Vorgang in Logik apps hinzuzufügen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Der Verbinder Facebook umfasst die folgenden Trigger und Aktionen. 

| Trigger | Aktionen|
| --- | --- |
| <ul><li>Wenn Sie ein neuer Beitrag auf Meine Zeitachse vorhanden ist</li></ul> |<ul><li>Abrufen von meinem Zeitachse-Feeds</li><li>Veröffentlichen auf Meine Zeitachse</li><li>Wenn Sie ein neuer Beitrag auf Meine Zeitachse vorhanden ist</li><li>Erste Seite feed</li><li>Abrufen von Benutzer-Zeitachse</li><li>Veröffentlichen auf der Seite</li></ul>

Alle Connectors unterstützen die Daten in JSON und XML-Formaten.

## <a name="create-a-connection-to-facebook"></a>Herstellen einer Verbindung mit Facebook
Wenn Sie Ihre apps Logik dieser Verbinder hinzugefügt haben, müssen Sie Logik apps für die Verbindung mit Ihrer Facebook autorisieren.

1. Melden Sie sich bei Ihrem Konto Facebook
2. Aktivieren Sie **Autorisieren**, und lassen Sie Ihre apps Logik zuzugreifen und Ihre Facebook. 

>[AZURE.INCLUDE [Steps to create a connection to Facebook](../../includes/connectors-create-api-facebook.md)]

>[AZURE.TIP] Sie können diese dieselbe Facebook-Verbindung in andere apps Logik verwenden.

## <a name="swagger-rest-api-reference"></a>Swagger REST-API-Referenz
Version gilt: 1.0.

### <a name="get-feed-from-my-timeline"></a>Abrufen von meinem Zeitachse-Feeds
Ruft die Feeds aus der angemeldete Benutzer Zeitachse ab.  
```GET: /me/feed```

| Namen|Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|(Felder)|Zeichenfolge|Nein|Abfrage|keine |Geben Sie die Felder, die zurückgegeben werden soll. Beispiel (Id, Name und Bild).|
|Grenzwert|ganze Zahl|Nein|Abfrage| keine|Maximale Anzahl von Beiträgen abgerufen werden sollen|
|mit|Zeichenfolge|Nein|Abfrage| keine|Einschränken der Liste der Beiträge auf jene mit Speicherort angefügt.|
|Filter|Zeichenfolge|Nein|Abfrage| keine|Abrufen von nur Beiträge, die einen bestimmten Streamfilter entsprechen.|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


### <a name="post-to-my-timeline"></a>Veröffentlichen auf Meine Zeitachse
Senden einer Statusnachricht mit zur Zeitachse der angemeldete Benutzer.  
```POST: /me/feed```

| Namen|Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Bereitstellen|Zeichenfolge |Ja|Textkörper|keine |Neue Nachricht bereitgestellt|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


### <a name="when-there-is-a-new-post-on-my-timeline"></a>Wenn Sie ein neuer Beitrag auf Meine Zeitachse vorhanden ist
Löst keinen neuen Datenfluss, wenn ein neuer Beitrag in der angemeldete Benutzer Zeitachse vorhanden ist.  
```GET: /trigger/me/feed```

Es sind keine Parameter. 

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


### <a name="get-page-feed"></a>Erste Seite feed
Rufen Sie Beiträge in den Feed für eine angegebene Seite.  
```GET: /{pageId}/feed```

| Namen|Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Seiten-ID|Zeichenfolge|Ja|Pfad| keine|ID des Zeichenblatts aus der Beiträge, die abgerufen werden müssen.|
|Grenzwert|ganze Zahl|Nein|Abfrage| keine|Maximale Anzahl von Beiträgen abgerufen werden sollen|
|include_hidden|Boolesch|Nein|Abfrage|keine |Ob alle Beiträge enthalten, die von der Seite ausgeblendet wurden|
|(Felder)|Zeichenfolge|Nein|Abfrage|keine |Geben Sie die Felder, die zurückgegeben werden soll. Beispiel (Id, Name und Bild).|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


### <a name="get-user-timeline"></a>Abrufen von Benutzer-Zeitachse
Rufen Sie Beiträge in Zeitachse des Benutzers ein.  
```GET: /{userId}/feed```

| Namen|Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Benutzer-ID|Zeichenfolge|Ja|Pfad|keine |ID des Benutzers, dessen Zeitachse abgerufen werden müssen.|
|Grenzwert|ganze Zahl|Nein|Abfrage|keine |Maximale Anzahl von Beiträgen abgerufen werden sollen|
|mit|Zeichenfolge|Nein|Abfrage|keine |Einschränken der Liste der Beiträge auf jene mit Speicherort angefügt.|
|Filter|Zeichenfolge|Nein|Abfrage| keine|Abrufen von nur Beiträge, die einen bestimmten Streamfilter entsprechen.|
|(Felder)|Zeichenfolge|Nein|Abfrage| keine|Geben Sie die Felder, die zurückgegeben werden soll. Beispiel (Id, Name und Bild).|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


### <a name="post-to-page"></a>Veröffentlichen auf der Seite
Bereitstellen einer Nachricht in einer Facebook Page als der Benutzer.  
```POST: /{pageId}/feed```

| Namen|Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Seiten-ID|Zeichenfolge|Ja|Pfad|keine |ID des Zeichenblatts zu veröffentlichen.|
|Bereitstellen|viele |Ja|Textkörper|keine |Neue Nachricht bereitgestellt.|

#### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


## <a name="object-definitions"></a>Objektdefinitionen

#### <a name="getfeedresponse"></a>GetFeedResponse

|Eigenschaftsname | Datentyp | Erforderlich|
|---|---|---|
|Daten|Matrix|Nein|

#### <a name="triggerfeedresponse"></a>TriggerFeedResponse

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Daten|Matrix|Nein|

#### <a name="postitem-a-single-entry-in-a-profiles-feed"></a>PostItem: Ein einzelnen Eintrag im Profil der feed
Das Profil könnte ein Benutzer, einer Seite, app oder Gruppe. 

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|
|admin_creator|Matrix|Nein|
|Beschriftung|Zeichenfolge|Nein|
|created_time|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|
|feed_targeting|nicht definiert|Nein|
|Von|nicht definiert|Nein|
|Symbol|Zeichenfolge|Nein|
|is_hidden|Boolesch|Nein|
|is_published|Boolesch|Nein|
|Link|Zeichenfolge|Nein|
|Nachricht|Zeichenfolge|Nein|
|Namen|Zeichenfolge|Nein|
|object_id|Zeichenfolge|Nein|
|Bild|Zeichenfolge|Nein|
|Platzieren Sie|nicht definiert|Nein|
|Datenschutz|nicht definiert|Nein|
|Eigenschaften|Matrix|Nein|
|Datenquelle|Zeichenfolge|Nein|
|status_type|Zeichenfolge|Nein|
|Geschichte|Zeichenfolge|Nein|
|als Ziel|nicht definiert|Nein|
|An|Matrix|Nein|
|Typ|Zeichenfolge|Nein|
|updated_time|Zeichenfolge|Nein|
|with_tags|nicht definiert|Nein|

#### <a name="triggeritem-a-single-entry-in-a-profiles-feed"></a>TriggerItem: Ein einzelnen Eintrag im Profil der feed
Das Profil könnte ein Benutzer, einer Seite, app oder Gruppe.

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|
|created_time|Zeichenfolge|Nein|
|Von|nicht definiert|Nein|
|Nachricht|Zeichenfolge|Nein|
|Typ|Zeichenfolge|Nein|

#### <a name="adminitem"></a>AdminItem

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|
|Link|Zeichenfolge|Nein|

#### <a name="propertyitem"></a>PropertyItem

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Namen|Zeichenfolge|Nein|
|Text|Zeichenfolge|Nein|
|href|Zeichenfolge|Nein|

#### <a name="userpostfeedrequest"></a>UserPostFeedRequest

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Nachricht|Zeichenfolge|Ja|
|Link|Zeichenfolge|Nein|
|Bild|Zeichenfolge|Nein|
|Namen|Zeichenfolge|Nein|
|Beschriftung|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|
|Platzieren Sie|Zeichenfolge|Nein|
|Kategorien|Zeichenfolge|Nein|
|Datenschutz|nicht definiert|Nein|
|object_attachment|Zeichenfolge|Nein|

#### <a name="pagepostfeedrequest"></a>PagePostFeedRequest

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Nachricht|Zeichenfolge|Ja|
|Link|Zeichenfolge|Nein|
|Bild|Zeichenfolge|Nein|
|Namen|Zeichenfolge|Nein|
|Beschriftung|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|
|Aktionen|Matrix|Nein|
|Platzieren Sie|Zeichenfolge|Nein|
|Kategorien|Zeichenfolge|Nein|
|object_attachment|Zeichenfolge|Nein|
|als Ziel|nicht definiert|Nein|
|feed_targeting|nicht definiert|Nein|
|veröffentlicht|Boolesch|Nein|
|scheduled_publish_time|Zeichenfolge|Nein|
|backdated_time|Zeichenfolge|Nein|
|backdated_time_granularity|Zeichenfolge|Nein|
|child_attachments|Matrix|Nein|
|multi_share_end_card|Boolesch|Nein|

#### <a name="postfeedresponse"></a>PostFeedResponse

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|

#### <a name="profilecollection"></a>ProfileCollection

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Daten|Matrix|Nein|

#### <a name="useritem"></a>UserItem

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|
|Vorname|Zeichenfolge|Nein|
|Nachname|Zeichenfolge|Nein|
|Namen|Zeichenfolge|Nein|
|Geschlecht|Zeichenfolge|Nein|
|Informationen zu|Zeichenfolge|Nein|

#### <a name="actionitem"></a>ActionItem

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Namen|Zeichenfolge|Nein|
|Link|Zeichenfolge|Nein|

#### <a name="targetitem"></a>TargetItem

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Länder|Matrix|Nein|
|Gebietsschemas|Matrix|Nein|
|Regionen|Matrix|Nein|
|Orte|Matrix|Nein|

#### <a name="feedtargetitem-object-that-controls-news-feed-targeting-for-this-post"></a>FeedTargetItem: Objekt, das steuert News feed verwendet, die für diesen Beitrag
Jede Person in diesen Gruppen wird eher finden in diesem Beitrag, andere sind weniger wahrscheinlich. Gilt nur für Seiten.

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Länder|Matrix|Nein|
|Regionen|Matrix|Nein|
|Orte|Matrix|Nein|
|age_min|ganze Zahl|Nein|
|age_max|ganze Zahl|Nein|
|genders|Matrix|Nein|
|relationship_statuses|Matrix|Nein|
|interested_in|Matrix|Nein|
|college_years|Matrix|Nein|
|Interessen|Matrix|Nein|
|relevant_until|ganze Zahl|Nein|
|education_statuses|Matrix|Nein|
|Gebietsschemas|Matrix|Nein|

#### <a name="placeitem"></a>PlaceItem

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Nein|
|Namen|Zeichenfolge|Nein|
|overall_rating|Zahl|Nein|
|Speicherort|nicht definiert|Nein|

#### <a name="locationitem"></a>LocationItem

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Ort|Zeichenfolge|Nein|
|Land|Zeichenfolge|Nein|
|Breite|Zahl|Nein|
|located_in|Zeichenfolge|Nein|
|Länge|Zahl|Nein|
|Namen|Zeichenfolge|Nein|
|Region|Zeichenfolge|Nein|
|Bundesstaat|Zeichenfolge|Nein|
|Straße|Zeichenfolge|Nein|
|ZIP|Zeichenfolge|Nein|

#### <a name="privacyitem"></a>PrivacyItem

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Beschreibung|Zeichenfolge|Nein|
|Wert|Zeichenfolge|Ja|
|zulassen|Zeichenfolge|Nein|
|Verweigern|Zeichenfolge|Nein|
|Freunde|Zeichenfolge|Nein|

#### <a name="childattachmentsitem"></a>ChildAttachmentsItem

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Link|Zeichenfolge|Nein|
|Bild|Zeichenfolge|Nein|
|image_hash|Zeichenfolge|Nein|
|Namen|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|

#### <a name="postphotorequest"></a>PostPhotoRequest

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|URL|Zeichenfolge|Ja|
|Beschriftung|Zeichenfolge|Nein|

#### <a name="postphotoresponse"></a>PostPhotoResponse

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Ja|
|post_id|Zeichenfolge|Ja|

#### <a name="postvideorequest"></a>PostVideoRequest

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|videoData|Zeichenfolge|Ja|
|Beschreibung|Zeichenfolge|Ja|
|Titel|Zeichenfolge|Ja|
|uploadedVideoName|Zeichenfolge|Nein|

#### <a name="getphotoresponse"></a>GetPhotoResponse

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Daten|nicht definiert|Ja|

#### <a name="getphotoresponseitem"></a>GetPhotoResponseItem

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|URL|Zeichenfolge|Ja|
|is_silhouette|Boolesch|Ja|
|Höhe|Zeichenfolge|Nein|
|Breite|Zeichenfolge|Nein|

#### <a name="geteventresponse"></a>GetEventResponse

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Daten|Matrix|Ja|

#### <a name="geteventresponseitem"></a>GetEventResponseItem

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|ID|Zeichenfolge|Ja|
|Namen|Zeichenfolge|Ja|
|start_time|Zeichenfolge|Nein|
|end_time|Zeichenfolge|Nein|
|Zeitzone|Zeichenfolge|Nein|
|Speicherort|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|
|ticket_uri|Zeichenfolge|Nein|
|rsvp_status|Zeichenfolge|Ja|


## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).
