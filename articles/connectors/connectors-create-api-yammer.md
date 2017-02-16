<properties
pageTitle="Hinzufügen den Yammer-Verbinder in Ihrer Apps Logik | Microsoft Azure"
description="Übersicht über den Yammer Verbinder mit den Parametern REST-API"
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

# <a name="get-started-with-the-yammer-connector"></a>Erste Schritte mit der Yammer-Verbinder

Verbinden Sie in Yammer mit Access Unterhaltungen im Netzwerk Ihres Unternehmens.

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus.

Mit Yammer können Sie folgende Aktionen ausführen:

- Erstellen Sie Ihrer Business Fluss basierend auf den Daten, die Sie von Yammer zu gelangen. 
- Verwenden Sie löst für, wenn es eine neue Nachricht in einer Gruppe oder ein Feed der folgenden wird.
- Verwenden von Aktionen zu Posten einer Nachricht, und erhalten Sie alle Nachrichten und vieles mehr. Diese Aktionen erhalten eine Antwort, und nehmen Sie die Ausgabe dann für andere Aktionen verfügbar. Wenn eine neue Nachricht angezeigt wird, können Sie beispielsweise eine e-Mail-Nachricht mit Office 365 senden.

Um einen Vorgang in Logik apps hinzuzufügen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Yammer umfasst die folgenden Trigger und Aktionen. 

Auslösen | Aktionen
--- | ---
<ul><li>Wenn eine neue Nachricht in eine Gruppe vorhanden ist</li><li>Wenn vorhanden ist-Feeds in meinem vor eine neue Nachricht</li></ul>| <ul><li>Alle Nachrichten erhalten</li><li>Ruft Nachrichten in einer Gruppe</li><li>Ruft feed die Nachrichten von meinem folgenden</li><li>Nachricht bereitstellen</li><li>Wenn eine neue Nachricht in eine Gruppe vorhanden ist</li><li>Wenn vorhanden ist-Feeds in meinem vor eine neue Nachricht</li></ul>

Alle Connectors unterstützen die Daten in JSON und XML-Formaten. 

## <a name="create-a-connection-to-yammer"></a>Herstellen einer Verbindung mit Yammer
Wenn den Verbinder Yammer verwenden möchten, erstellen Sie zunächst eine **Verbindung** Sie die Details für diese Eigenschaften bereit: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Geben Sie Yammer-Anmeldeinformationen|

>[AZURE.INCLUDE [Steps to create a connection to Yammer](../../includes/connectors-create-api-yammer.md)]


>[AZURE.TIP] Sie können diese Verbindung in andere apps Logik verwenden.

## <a name="yammer-rest-api-reference"></a>Yammer-REST-API-Referenz
Diese Dokumentation ist für Version: 1.0


### <a name="get-all-public-messages-in-the-logged-in-users-yammer-network"></a>Abrufen von allen öffentlichen Nachrichten in der angemeldete Benutzer Yammer-Netzwerk
"Alle" Unterhaltungen in Yammer-Web-Oberfläche entspricht.  
```GET: /messages.json```

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|older_then|ganze Zahl|Nein|Abfrage|keine|Gibt die Nachrichten, die älter als die Nachrichten-ID, die als numerischen Zeichenfolge angegeben. Dies ist nützlich zum Paginieren Nachrichten. Wenn aktuell angezeigten 20 Nachrichten sind und das älteste Zahl ist beispielsweise 2912, könnten Sie anfügen "? Older_than = 2912″ auf Ihre Anforderung, erhalten die 20 Nachrichten vor den immer.|
|newer_then|ganze Zahl|Nein|Abfrage|keine|Gibt die Nachrichten neuer als die Nachrichten-ID, die als numerischen Zeichenfolge angegeben. Dies sollte verwendet werden, beim Abruf für neue Nachrichten. Wenn Sie Nachrichten suchen, und die neueste Nachricht zurückgegeben 3516 ist, können Sie eine Anforderung mit dem Parameter vornehmen "? Newer_than = 3516″, um sicherzustellen, dass Sie keine doppelte Kopien von Nachrichten bereits auf Ihrer Seite erhalten.|
|Grenzwert|ganze Zahl|Nein|Abfrage|keine|Geben Sie nur die angegebene Anzahl von Nachrichten zurück.|
|Seite|ganze Zahl|Nein|Abfrage|keine|Abrufen der Seite angegeben. Wenn zurückgegeben, dass Daten größer als der Grenzwert ist, können Sie dieses Feld verwenden, um nachfolgende Seiten zuzugreifen|


### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|408|Timeout für die Anforderung|
|429|Zu viele Anfragen|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|503|Yammer-Dienst nicht verfügbar|
|504|Gateway-Timeout|
|Standard|Fehler bei Vorgang.|


### <a name="post-a-message-to-a-group-or-all-company-feed"></a>Posten einer Nachricht an einer Gruppe oder alle Unternehmen-Feeds
Wenn die Gruppen-ID bereitgestellt wird, wird Nachricht zur angegebenen Gruppe sonst bereitgestellt werden, die in allen Unternehmen Feed bereitgestellt werden.    
```POST: /messages.json``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Eingabe| |Ja|Textkörper|keine|Die Anforderung zum Beitrag Nachricht|


### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|201|Erstellt|



## <a name="object-definitions"></a>Objektdefinitionen

#### <a name="message-yammer-message"></a>Meldung: Yammer-Nachricht

| Namen | Datentyp | Erforderlich |
|---|---| --- | 
|ID|ganze Zahl|Nein|
|content_excerpt|Zeichenfolge|Nein|
|sender_id|ganze Zahl|Nein|
|replied_to_id|ganze Zahl|Nein|
|created_at|Zeichenfolge|Nein|
|network_id|ganze Zahl|Nein|
|message_type|Zeichenfolge|Nein|
|sender_type|Zeichenfolge|Nein|
|URL|Zeichenfolge|Nein|
|web_url|Zeichenfolge|Nein|
|group_id|ganze Zahl|Nein|
|Textkörper|nicht definiert|Nein|
|thread_id|ganze Zahl|Nein|
|direct_message|Boolesch|Nein|
|client_type|Zeichenfolge|Nein|
|client_url|Zeichenfolge|Nein|
|Sprache|Zeichenfolge|Nein|
|notified_user_ids|Matrix|Nein|
|Datenschutz|Zeichenfolge|Nein|
|liked_by|nicht definiert|Nein|
|system_message|Boolesch|Nein|

#### <a name="postoperationrequest-represents-a-post-request-for-yammer-connector-to-post-to-yammer"></a>PostOperationRequest: Eine Anforderung Beitrag für Yammer Verbinder zu Posten yammer darstellt.

| Namen | Datentyp | Erforderlich |
|---|---| --- | 
|Textkörper|Zeichenfolge|Ja|
|group_id|ganze Zahl|Nein|
|replied_to_id|ganze Zahl|Nein|
|direct_to_id|ganze Zahl|Nein|
|übertragen|Boolesch|Nein|
|topic1|Zeichenfolge|Nein|
|topic2|Zeichenfolge|Nein|
|topic3|Zeichenfolge|Nein|
|topic4|Zeichenfolge|Nein|
|topic5|Zeichenfolge|Nein|
|topic6|Zeichenfolge|Nein|
|topic7|Zeichenfolge|Nein|
|topic8|Zeichenfolge|Nein|
|topic9|Zeichenfolge|Nein|
|topic10|Zeichenfolge|Nein|
|topic11|Zeichenfolge|Nein|
|topic12|Zeichenfolge|Nein|
|topic13|Zeichenfolge|Nein|
|topic14|Zeichenfolge|Nein|
|topic15|Zeichenfolge|Nein|
|topic16|Zeichenfolge|Nein|
|topic17|Zeichenfolge|Nein|
|topic18|Zeichenfolge|Nein|
|topic19|Zeichenfolge|Nein|
|topic20|Zeichenfolge|Nein|

#### <a name="messagelist-list-of-messages"></a>MessageList: Liste der Nachrichten

| Namen | Datentyp | Erforderlich |
|---|---| --- | 
|Nachrichten|Matrix|Nein|


#### <a name="messagebody-message-body"></a>MessageBody: Nachrichtentext

| Namen | Datentyp | Erforderlich |
|---|---| --- | 
|analysiert|Zeichenfolge|Nein|
|nur|Zeichenfolge|Nein|
|Rich-|Zeichenfolge|Nein|

#### <a name="likedby-liked-by"></a>LikedBy: Befunden durch

| Namen | Datentyp | Erforderlich |
|---|---| --- | 
|zählen|ganze Zahl|Nein|
|Namen|Matrix|Nein|

#### <a name="yammmerentity-liked-by"></a>YammmerEntity: Befunden durch

| Namen | Datentyp | Erforderlich |
|---|---| --- | 
|Typ|Zeichenfolge|Nein|
|ID|ganze Zahl|Nein|
|full_name|Zeichenfolge|Nein|


## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

[1]: ./media/connectors-create-api-yammer/connectionconfig1.png
[2]: ./media/connectors-create-api-yammer/connectionconfig2.png 
[3]: ./media/connectors-create-api-yammer/connectionconfig3.png
[4]: ./media/connectors-create-api-yammer/connectionconfig4.png
[5]: ./media/connectors-create-api-yammer/connectionconfig5.png
