<properties
pageTitle="RSS | Microsoft Azure"
description="Erstellen Sie Logik apps mit Azure-App-Dienst an. RSS-Verbinder kann die Benutzer veröffentlichen und Stare Elemente abzurufen. Sie können auch Benutzer Vorgänge ausgelöst, wenn ein neues Element in den Feed veröffentlicht wird."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-rss-connector"></a>Erste Schritte mit der RSS-Verbinder
RSS ist ein beliebter Syndication Format verwendet, um häufig aktualisierte Inhalte – wie Blog-Einträge und Schlagzeilen zu veröffentlichen.  Viele Personen Inhalte bereitstellen ein RSS-Feeds, damit Benutzer abonnieren können.  Verwenden Sie den Verbinder RSS-feed Informationen und der Trigger Zahlungen abgerufen werden, wenn neue Elemente in einen RSS-feed veröffentlicht werden.

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus. 

Sie können beginnen, indem Sie eine app Logik jetzt erstellen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

Der RSS-Verbinder kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen die Daten in JSON und XML-Formaten. 

 Der RSS-Verbinder weist die folgenden Aktion(en) und/oder Triggern zur Verfügung:

### <a name="rss-actions"></a>RSS-Aktionen
Sie können folgende Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|[ListFeedItems](connectors-create-api-rss.md#listfeeditems)|Erhalten Sie alle RSS-feed Elemente.|
### <a name="rss-triggers"></a>RSS-Triggern
Sie können diese Ereignisse überwachen:

|Auslösen | Beschreibung|
|--- | ---|
|Wenn ein neues Element des feed veröffentlicht|Einen Workflow ausgelöst wird, wenn Sie ein neuer Feed veröffentlicht wird|


## <a name="create-a-connection-to-rss"></a>Herstellen einer Verbindung mit RSS

>[AZURE.INCLUDE [Steps to create a connection to an RSS feed](../../includes/connectors-create-api-rss.md)]

>[AZURE.TIP] Sie können diese Verbindung in andere apps Logik verwenden.

## <a name="reference-for-rss"></a>Für RSS-Referenz
Version gilt: 1.0

## <a name="onnewfeed"></a>OnNewFeed
Wenn ein neues Element des feed veröffentlicht: einen Workflow ausgelöst wird, wenn Sie ein neuer Feed veröffentlicht wird 

```GET: /OnNewFeed``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|feedUrl|Zeichenfolge|Ja|Abfrage|keine|Feed-url|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|202|Akzeptiert|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="listfeeditems"></a>ListFeedItems
Liste aller RSS-feed Elemente.: Alle RSS-feed Elemente zu erhalten. 

```GET: /ListFeedItems``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|feedUrl|Zeichenfolge|Ja|Abfrage|keine|Feed-url|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|202|Akzeptiert|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="object-definitions"></a>Objektdefinitionen 

### <a name="triggerbatchresponsefeeditem"></a>TriggerBatchResponse [FeedItem]


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Matrix|Nein |



### <a name="feeditem"></a>FeedItem


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Ja |
|Titel|Zeichenfolge|Ja |
|Inhalt|Zeichenfolge|Ja |
|Links|Matrix|Nein |
|updatedOn|Zeichenfolge|Nein |


## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)