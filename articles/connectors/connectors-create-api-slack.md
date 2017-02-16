<properties
pageTitle=" Verwenden Sie den Pufferzeit Verbinder in Ihrer apps Logik | Microsoft Azure"
description="Erste Schritte mit der Pufferzeit Verbinder in Ihrer Microsoft Azure Logik des App-apps"
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

# <a name="get-started-with-the-slack-connector"></a>Erste Schritte mit der Pufferzeit Verbinder

Pufferzeit ist regelmäßige kurze, mit der vereint alle Kommunikation in Ihrem Team in einem platzieren sofort durchsucht und Sie überall verfügbar.

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus.

Mit der Pufferzeit Verbinder können Sie folgende Aktionen ausführen:

* Mithilfe dieser Logik apps erstellen

Um einen Vorgang in Logik apps hinzuzufügen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="lets-talk-about-triggers-and-actions"></a>Wissenswertes zu Trigger und Aktionen

Der Verbinder Pufferzeit kann als eine Aktion verwendet werden. Es gibt keine Trigger aus. Alle Connectors unterstützen die Daten in JSON und XML-Formaten. 

 Der Verbinder Pufferzeit weist die folgenden Aktion(en) und/oder Triggern zur Verfügung:

### <a name="slack-actions"></a>Pufferzeit Aktionen
Sie können folgende Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|PostMessage|Posten einer Nachricht auf einen bestimmten Kanal an.|
## <a name="create-a-connection-to-slack"></a>Herstellen einer Verbindung mit Pufferzeit
Wenn den Verbinder Pufferzeit verwenden möchten, erstellen Sie zunächst eine **Verbindung** Sie die Details für diese Eigenschaften bereit: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Pufferzeit Anmeldeinformationen|

Wie folgt vor, um melden Sie sich bei Pufferzeit und schließen Sie die Konfiguration der Pufferzeit **Verbindung** in Ihrer app Logik:

1. Wählen Sie **Wiederholung**
2. Wählen Sie eine **Häufigkeit** aus, und geben Sie ein **Intervall**
3. Wählen Sie **Hinzufügen einer Aktion**  
![Konfigurieren von Pufferzeit][1]  
4. Geben Sie in das Suchfeld Pufferzeit und warten Sie, bis die Suche auf alle Einträge mit Pufferzeit im Namen zurückzukehren.
5. Wählen Sie die **Pufferzeit - Nachricht bereitstellen**
6. Wählen Sie aus, **Melden Sie sich bei Pufferzeit Ende**:  
![Konfigurieren von Pufferzeit][2]
7. Geben Sie Ihre Anmeldeinformationen Pufferzeit melden Sie sich bei die Anwendung autorisieren    
![Konfigurieren von Pufferzeit][3]  
8. Sie können in Ihrer Organisation Protokoll Seite umgeleitet werden. **Autorisieren** Pufferzeit Logik App interagieren:      
![Konfigurieren von Pufferzeit][5] 
9. Nach Abschluss die Autorisierung werden Sie zu Ihrer Anwendung Logik, um sie durchführen, indem Sie im Abschnitt **Pufferzeit Ende – erhalten Sie alle Nachrichten** umgeleitet werden. Fügen Sie andere Trigger und Aktionen, die Sie benötigen.  
![Konfigurieren von Pufferzeit][6]
10. Speichern Sie Ihre Arbeit, indem Sie auf der Menüleiste oben **Speichern** auswählen.


>[AZURE.TIP] Sie können diese Verbindung in andere apps Logik verwenden.

## <a name="slack-rest-api-reference"></a>Pufferzeit REST-API-Referenz
#### <a name="this-documentation-is-for-version-10"></a>Diese Dokumentation ist für Version: 1.0


### <a name="post-a-message-to-a-specified-channel"></a>Posten einer Nachricht auf einen bestimmten Kanal an.
**```POST: /chat.postMessage```** 



| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Kanal|Zeichenfolge|Ja|Abfrage|keine|Kanal, private Gruppe oder Chat-Kanal Nachricht senden. Ein Name (ex: #general) oder eine codierte-ID.|
|Text|Zeichenfolge|Ja|Abfrage|keine|Text der Nachricht zu senden. Formatierungsoptionen, finden Sie unter https://api.slack.com/docs/formatting.|
|Benutzername|Zeichenfolge|Nein|Abfrage|keine|Name des bot|
|as_user|Boolesch|Nein|Abfrage|keine|Übergeben Sie true, wenn die Nachricht als authentifizierter Benutzer, statt als ein Bot Posten|
|Analysieren|Zeichenfolge|Nein|Abfrage|keine|Ändern Sie, wie Nachrichten behandelt werden. Weitere Informationen finden Sie unter https://api.slack.com/docs/formatting.|
|link_names|ganze Zahl|Nein|Abfrage|keine|Suchen und Link Channelnamen und Benutzernamen.|
|unfurl_links|Boolesch|Nein|Abfrage|keine|True, wenn das Aktivieren von Inhalten vorrangig textbasierten unfurling übergeben.|
|unfurl_media|Boolesch|Nein|Abfrage|keine|Zum Deaktivieren von Medieninhalten unfurling falsch zu übergeben.|
|icon_url|Zeichenfolge|Nein|Abfrage|keine|Die URL zu einem Bild, das als Symbol für diese Nachricht verwendet|
|icon_emoji|Zeichenfolge|Nein|Abfrage|keine|Emoji als Symbol für diese Nachricht verwenden|


### <a name="here-are-the-possible-responses"></a>Hier sind die möglichen Antworten aus:

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|408|Timeout für die Anforderung|
|429|Zu viele Anfragen|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|503|Pufferzeit Anfang Dienst nicht verfügbar|
|504|Gateway-Timeout|
|Standard|Fehler bei Vorgang.|
------



## <a name="object-definitions"></a>Objekt Definitionen: 

 **Meldung**: Yammer-Nachricht

Erforderlichen Eigenschaften für Nachricht:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|ID|ganze Zahl|
|content_excerpt|Zeichenfolge|
|sender_id|ganze Zahl|
|replied_to_id|ganze Zahl|
|created_at|Zeichenfolge|
|network_id|ganze Zahl|
|message_type|Zeichenfolge|
|sender_type|Zeichenfolge|
|URL|Zeichenfolge|
|web_url|Zeichenfolge|
|group_id|ganze Zahl|
|Textkörper|nicht definiert|
|thread_id|ganze Zahl|
|direct_message|Boolesch|
|client_type|Zeichenfolge|
|client_url|Zeichenfolge|
|Sprache|Zeichenfolge|
|notified_user_ids|Matrix|
|Datenschutz|Zeichenfolge|
|liked_by|nicht definiert|
|system_message|Boolesch|



 **PostOperationRequest**: eine Anforderung Beitrag für Yammer-Verbinder zum Veröffentlichen von Beiträgen in yammer darstellt

Erforderlichen Eigenschaften für PostOperationRequest:

Textkörper

**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Textkörper|Zeichenfolge|
|group_id|ganze Zahl|
|replied_to_id|ganze Zahl|
|direct_to_id|ganze Zahl|
|übertragen|Boolesch|
|topic1|Zeichenfolge|
|topic2|Zeichenfolge|
|topic3|Zeichenfolge|
|topic4|Zeichenfolge|
|topic5|Zeichenfolge|
|topic6|Zeichenfolge|
|topic7|Zeichenfolge|
|topic8|Zeichenfolge|
|topic9|Zeichenfolge|
|topic10|Zeichenfolge|
|topic11|Zeichenfolge|
|topic12|Zeichenfolge|
|topic13|Zeichenfolge|
|topic14|Zeichenfolge|
|topic15|Zeichenfolge|
|topic16|Zeichenfolge|
|topic17|Zeichenfolge|
|topic18|Zeichenfolge|
|topic19|Zeichenfolge|
|topic20|Zeichenfolge|



 **MessageList**: Liste der Nachrichten

Erforderlichen Eigenschaften für MessageList:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Nachrichten|Matrix|



 **MessageBody**: Nachrichtentext

Erforderlichen Eigenschaften für MessageBody:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|analysiert|Zeichenfolge|
|nur|Zeichenfolge|
|Rich-|Zeichenfolge|



 **LikedBy**: durch befunden

Erforderlichen Eigenschaften für LikedBy:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|zählen|ganze Zahl|
|Namen|Matrix|



 **YammmerEntity**: durch befunden

Erforderlichen Eigenschaften für YammmerEntity:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Typ|Zeichenfolge|
|ID|ganze Zahl|
|full_name|Zeichenfolge|


## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)
## <a name="object-definitions"></a>Objekt Definitionen: 

 **WebResultModel**: Bing-Web-Suchergebnissen

Erforderlichen Eigenschaften für WebResultModel:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Titel|Zeichenfolge|
|Beschreibung|Zeichenfolge|
|DisplayUrl|Zeichenfolge|
|ID|Zeichenfolge|
|FullUrl|Zeichenfolge|



 **VideoResultModel**: video Bing-Suchergebnisse

Erforderlichen Eigenschaften für VideoResultModel:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Titel|Zeichenfolge|
|DisplayUrl|Zeichenfolge|
|ID|Zeichenfolge|
|MediaUrl|Zeichenfolge|
|Laufzeit|ganze Zahl|
|Miniaturansicht|nicht definiert|



 **ThumbnailModel**: Miniaturansicht Eigenschaften des Elements multimedia

Erforderlichen Eigenschaften für ThumbnailModel:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|MediaUrl|Zeichenfolge|
|ContentType|Zeichenfolge|
|Breite|ganze Zahl|
|Höhe|ganze Zahl|
|FileSize|ganze Zahl|



 **ImageResultModel**: Bing Bild Suchergebnisse

Erforderlichen Eigenschaften für ImageResultModel:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Titel|Zeichenfolge|
|DisplayUrl|Zeichenfolge|
|ID|Zeichenfolge|
|MediaUrl|Zeichenfolge|
|SourceUrl|Zeichenfolge|
|Miniaturansicht|nicht definiert|



 **NewsResultModel**: Bing News Suchergebnisse

Erforderlichen Eigenschaften für NewsResultModel:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Titel|Zeichenfolge|
|Beschreibung|Zeichenfolge|
|DisplayUrl|Zeichenfolge|
|ID|Zeichenfolge|
|Datenquelle|Zeichenfolge|
|Datum|Zeichenfolge|



 **SpellResultModel**: Bing Rechtschreibung Vorschläge Ergebnisse

Erforderlichen Eigenschaften für SpellResultModel:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|ID|Zeichenfolge|
|Wert|Zeichenfolge|



 **RelatedSearchResultModel**: Bing Zusammenhang Suchergebnisse

Erforderlichen Eigenschaften für RelatedSearchResultModel:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Titel|Zeichenfolge|
|ID|Zeichenfolge|
|BingUrl|Zeichenfolge|



 **CompositeSearchResultModel**: zusammengesetzte Bing-Suchergebnisse

Erforderlichen Eigenschaften für CompositeSearchResultModel:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|WebResultsTotal|ganze Zahl|
|ImageResultsTotal|ganze Zahl|
|VideoResultsTotal|ganze Zahl|
|NewsResultsTotal|ganze Zahl|
|SpellSuggestionsTotal|ganze Zahl|
|WebResults|Matrix|
|ImageResults|Matrix|
|VideoResults|Matrix|
|NewsResults|Matrix|
|SpellSuggestionResults|Matrix|
|RelatedSearchResults|Matrix|


## <a name="object-definitions"></a>Objekt Definitionen: 

 **PostOperationResponse**: Antwort des Beitrags Vorgangs Pufferzeit Verbinders darstellt, für die Bereitstellung Pufferzeit

Erforderlichen Eigenschaften für PostOperationResponse:


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Okay|Boolesch|
|Kanal|Zeichenfolge|
|Terminaldienste|Zeichenfolge|
|Nachricht|nicht definiert|
|Fehler|Zeichenfolge|



 **"MessageItem"**: eine Nachricht erstellen.

Erforderlichen Eigenschaften für "MessageItem":


Keiner der Eigenschaften ist erforderlich. 


**Alle Eigenschaften**: 


| Namen | Datentyp |
|---|---|
|Text|Zeichenfolge|
|ID|Zeichenfolge|
|Benutzer|Zeichenfolge|
|erstellt|ganze Zahl|
|Is_user gelöscht|Boolesch|


## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
