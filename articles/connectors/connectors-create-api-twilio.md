<properties
pageTitle="Hinzufügen der Twilio Verbinder in Ihrer apps Logik | Microsoft Azure"
description="Übersicht über den Twilio Verbinder mit den Parametern REST-API"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="logic-apps"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="09/19/2016"
ms.author="mandia"/>

# <a name="get-started-with-the-twilio-connector"></a>Erste Schritte mit der Twilio Verbinder

Verbinden Sie mit Twilio zum globale SMS, MMS und IP-Nachrichten senden und empfangen.

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus.

Mit Twilio können Sie folgende Aktionen ausführen:

- Erstellen Sie Ihrer Business Fluss basierend auf den Daten, die Sie von Twilio zu gelangen. 
- Verwenden Sie die Aktionen, die einer Nachricht, Listennachrichten und mehr erhalten. Diese Aktionen erhalten eine Antwort, und nehmen Sie die Ausgabe dann für andere Aktionen verfügbar. Wenn Sie eine neue Twilio Nachricht erhalten, können Sie beispielsweise machen Sie diese Meldung und verwenden sie einen Workflow Dienstbus. 

Um einen Vorgang in Logik apps hinzuzufügen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Der Verbinder Twilio umfasst die folgenden Aktionen aus. Es gibt keine Trigger aus. 

| Trigger | Aktionen|
| --- | --- |
|Keine| <ul><li>Abrufen der Nachricht</li><li>Listennachrichten</li><li>Senden der Nachricht</li></ul>|

Alle Connectors unterstützen die Daten in JSON und XML-Formaten. 

## <a name="create-a-connection-to-twilio"></a>Herstellen einer Verbindung mit Twilio
Wenn Sie diesen Connector Ihrer apps Logik hinzufügen, geben Sie die folgenden Twilio Werte:

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Konto-ID|Ja|Geben Sie Ihre Konto-ID Twilio|
|Access-Token|Ja|Geben Sie Ihre Access-Token Twilio|

>[AZURE.INCLUDE [Steps to create a connection to Twilio](../../includes/connectors-create-api-twilio.md)] 

Wenn Sie eine besitzen, finden Sie unter [Twilio](https://www.twilio.com/docs/api/ip-messaging/guides/identity) zum Erstellen einer Access-Token.


>[AZURE.TIP] Sie können diese dieselbe Twilio Verbindung in andere apps Logik verwenden.

## <a name="swagger-rest-api-reference"></a>Swagger REST-API-Referenz
#### <a name="this-documentation-is-for-version-10"></a>Diese Dokumentation ist für Version: 1.0

### <a name="get-message"></a>Abrufen der Nachricht
Gibt eine einzelne Nachricht, die von der angegebenen Nachricht-ID angegeben  
```GET: /Messages/{MessageId}.json```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Nachrichten-ID|Zeichenfolge|Ja|Pfad|keine|Nachrichten-ID|

### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|404|Nachricht nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


### <a name="list-messages"></a>Listennachrichten
Gibt eine Liste der Nachrichten, die mit Ihrem Konto verbunden sind.  
```GET: /Messages.json```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|An|Zeichenfolge|Nein|Abfrage|keine|Rufnummer|
|Von|Zeichenfolge|Nein|Abfrage|keine|Telefonnummer|
|DateSent|Zeichenfolge|Nein|Abfrage|keine|Nur Anzeigen von Nachrichten, die an diesem Datum (im Format GMT), gesendet werden als JJJJ-MM-TT. Beispiel: DateSent = 2009-07-06. Sie können auch angeben, Ungleichheit, wie etwa DateSent < = JJJJ / MM / TT für Nachrichten, die am oder vor Mitternacht auf ein Datum und eine DateSent gesendet wurden > = JJJJ / MM / TT für Nachrichten, die am oder nach Mitternacht an einem Datum gesendet.|
|PageSize|ganze Zahl|Nein|Abfrage|50|Wie viele Ressourcen, die in jeder Listenseite zurückgegeben. Standardwert ist 50.|
|Seite|ganze Zahl|Nein|Abfrage|0|Seitenzahl. Standardwert ist 0.|

### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|



### <a name="send-message"></a>Senden der Nachricht
Senden Sie eine neue Nachricht an eine Mobiltelefonnummer ein.  
```POST: /Messages.json```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|sendMessageRequest| |Ja|Textkörper|keine|Nachricht zu senden|

### <a name="response"></a>Antwort
|Namen|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="object-definitions"></a>Objektdefinitionen

#### <a name="sendmessagerequest-request-model-for-send-message-operation"></a>SendMessageRequest: Fordern Sie Modell für die Nachricht senden an

|Eigenschaftsname | Datentyp | Erforderlich|
|---|---|---|
|Von|Zeichenfolge|Ja|
|An|Zeichenfolge|Ja|
|Textkörper|Zeichenfolge|Ja|
|media_url|Matrix|Nein|
|status_callback|Zeichenfolge|Nein|
|messaging_service_sid|Zeichenfolge|Nein|
|application_sid|Zeichenfolge|Nein|
|Max. Preis|Zeichenfolge|Nein|


#### <a name="message-model-for-message"></a>Meldung: Modell für die Nachricht

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Textkörper|Zeichenfolge|Nein|
|Von|Zeichenfolge|Nein|
|An|Zeichenfolge|Nein|
|Status|Zeichenfolge|Nein|
|SID|Zeichenfolge|Nein|
|account_sid|Zeichenfolge|Nein|
|api_version|Zeichenfolge|Nein|
|num_segments|Zeichenfolge|Nein|
|num_media|Zeichenfolge|Nein|
|date_created|Zeichenfolge|Nein|
|date_sent|Zeichenfolge|Nein|
|date_updated|Zeichenfolge|Nein|
|Richtung|Zeichenfolge|Nein|
|Fehlercode|Zeichenfolge|Nein|
|diesem|Zeichenfolge|Nein|
|Kurs|Zeichenfolge|Nein|
|price_unit|Zeichenfolge|Nein|
|URI|Zeichenfolge|Nein|
|subresource_uris|Matrix|Nein|
|messaging_service_sid|Zeichenfolge|Nein|

#### <a name="messagelist-response-model-for-list-messages-operation"></a>MessageList: Antwort Modell für die Listennachrichten

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Nachrichten|Matrix|Nein|
|Seite|ganze Zahl|Nein|
|page_size|ganze Zahl|Nein|
|num_pages|ganze Zahl|Nein|
|URI|Zeichenfolge|Nein|
|first_page_uri|Zeichenfolge|Nein|
|next_page_uri|Zeichenfolge|Nein|
|Summe|ganze Zahl|Nein|
|previous_page_uri|Zeichenfolge|Nein|

#### <a name="incomingphonenumberlist-response-model-for-list-messages-operation"></a>IncomingPhoneNumberList: Antwort Modell für die Listennachrichten

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|incoming_phone_numbers|Matrix|Nein|
|Seite|ganze Zahl|Nein|
|page_size|ganze Zahl|Nein|
|num_pages|ganze Zahl|Nein|
|URI|Zeichenfolge|Nein|
|first_page_uri|Zeichenfolge|Nein|
|next_page_uri|Zeichenfolge|Nein|


#### <a name="addincomingphonenumberrequest-request-model-for-add-incoming-number-operation"></a>AddIncomingPhoneNumberRequest: Anfordern des Datenmodells für eingehende Zahl hinzufügen Vorgang

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Telefonnummer|Zeichenfolge|Ja|
|AreaCode|Zeichenfolge|Nein|
|FriendlyName|Zeichenfolge|Nein|


#### <a name="incomingphonenumber-incoming-phone-number"></a>IncomingPhoneNumber: Eingehende Telefonnummer

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|phone_number|Zeichenfolge|Nein|
|Freundlicher_Name|Zeichenfolge|Nein|
|SID|Zeichenfolge|Nein|
|account_sid|Zeichenfolge|Nein|
|date_created|Zeichenfolge|Nein|
|date_updated|Zeichenfolge|Nein|
|Funktionen|nicht definiert|Nein|
|status_callback|Zeichenfolge|Nein|
|status_callback_method|Zeichenfolge|Nein|
|api_version|Zeichenfolge|Nein|


#### <a name="capabilities-phone-number-capabilities"></a>Funktionen: Phone Number-Funktionen

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|MMS|Boolesch|Nein|
|SMS|Boolesch|Nein|
|Voicemail|Boolesch|Nein|

#### <a name="availablephonenumbers-available-phone-numbers"></a>AvailablePhoneNumbers: Verfügbare Telefonnummern

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|phone_number|Zeichenfolge|Nein|
|Freundlicher_Name|Zeichenfolge|Nein|
|lata|Zeichenfolge|Nein|
|Breite|Zeichenfolge|Nein|
|Länge|Zeichenfolge|Nein|
|postal_code|Zeichenfolge|Nein|
|rate_center|Zeichenfolge|Nein|
|Region|Zeichenfolge|Nein|
|MMS|Boolesch|Nein|
|SMS|Boolesch|Nein|
|Voicemail|Boolesch|Nein|


#### <a name="usagerecords-usage-records-class"></a>UsageRecords: Die Verwendung von Datensätzen class

|Eigenschaftsname | Datentyp |Erforderlich|
|---|---|---|
|Kategorie|Zeichenfolge|Nein|
|Verwendung|Zeichenfolge|Nein|
|usage_unit|Zeichenfolge|Nein|
|Beschreibung|Zeichenfolge|Nein|
|Kurs|Zahl|Nein|
|price_unit|Zeichenfolge|Nein|
|zählen|Zeichenfolge|Nein|
|count_unit|Zeichenfolge|Nein|
|Ausgangsdatum|Zeichenfolge|Nein|
|Enddatum|Zeichenfolge|Nein|


## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)
