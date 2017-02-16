<properties
pageTitle="SendGrid | Microsoft Azure"
description="Erstellen Sie Logik apps mit Azure-App-Dienst an. SendGrid Verbindungsanbieter können Sie e-Mails senden und Empfängerlisten verwalten."
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

# <a name="get-started-with-the-sendgrid-connector"></a>Erste Schritte mit der SendGrid Verbinder

SendGrid Verbindungsanbieter können Sie e-Mails senden und Empfängerlisten verwalten.

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus. 

Sie können beginnen, indem Sie eine app Logik jetzt erstellen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

Der Verbinder SendGrid kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen die Daten in JSON und XML-Formaten. 

 Der Verbinder SendGrid weist die folgenden Aktionen verfügbar. Es gibt keine Trigger aus.

### <a name="sendgrid-actions"></a>SendGrid Aktionen
Sie können folgende Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|[SendEmail](connectors-create-api-sendgrid.md#sendemail)|Sendet eine e-Mail-Nachricht mit SendGrid-API (maximal 10.000 Empfänger)|
|[AddRecipientToList](connectors-create-api-sendgrid.md#addrecipienttolist)|Hinzufügen eines einzelnen Empfängers zu einer Empfängerliste|


## <a name="create-a-connection-to-sendgrid"></a>Herstellen einer Verbindung mit SendGrid
Um Logik apps mit SendGrid zu erstellen, müssen Sie zunächst eine **Verbindung** erstellen anschließend geben Sie die Details für die folgenden Eigenschaften: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|ApiKey|Ja|Geben Sie Ihre SendGrid Api Schlüssel|
 

>[AZURE.INCLUDE [Steps to create a connection to SendGrid](../../includes/connectors-create-api-sendgrid.md)]

>[AZURE.TIP] Sie können diese Verbindung in andere apps Logik verwenden.

Nachdem Sie die Verbindung zu erstellen, können Sie es verwenden, führen Sie die Aktionen aus, und achten Sie auf die in diesem Artikel beschriebenen Trigger.

## <a name="reference-for-sendgrid"></a>Referenz für SendGrid
Version gilt: 1.0

## <a name="sendemail"></a>SendEmail
Senden von Nachrichten: sendet eine e-Mail-Nachricht mit SendGrid-API (maximal 10.000 Empfänger) 

```POST: /api/mail.send.json``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Anforderung| |Ja|Textkörper|keine|E-Mail-Nachricht zu senden|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|429|Zu viele Anforderung|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="addrecipienttolist"></a>AddRecipientToList
Hinzufügen eines Empfängers zum Liste: einen einzelnen Empfänger einer Empfängerliste hinzufügen 

```POST: /v3/contactdb/lists/{listId}/recipients/{recipientId}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|listId|Zeichenfolge|Ja|Pfad|keine|Eindeutige Id der Empfängerliste|
|recipientId|Zeichenfolge|Ja|Pfad|keine|Eindeutige Id des Empfängers|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="object-definitions"></a>Objektdefinitionen 

### <a name="emailrequest"></a>EmailRequest


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Von|Zeichenfolge|Ja |
|FromName|Zeichenfolge|Nein |
|An|Zeichenfolge|Ja |
|ToName|Zeichenfolge|Nein |
|Betreff|Zeichenfolge|Ja |
|Textkörper|Zeichenfolge|Ja |
|ishtml|Boolesch|Nein |
|cc|Zeichenfolge|Nein |
|ccname|Zeichenfolge|Nein |
|"Bcc"|Zeichenfolge|Nein |
|bccname|Zeichenfolge|Nein |
|ReplyTo|Zeichenfolge|Nein |
|Datum|Zeichenfolge|Nein |
|Kopfzeilen|Zeichenfolge|Nein |
|Dateien|Matrix|Nein |
|Dateinamen|Matrix|Nein |



### <a name="emailresponse"></a>EmailResponse


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Nachricht|Zeichenfolge|Nein |



### <a name="recipientlists"></a>RecipientLists


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Listen|Matrix|Nein |



### <a name="recipientlist"></a>RecipientList


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|Namen|Zeichenfolge|Nein |
|recipient_count|ganze Zahl|Nein |



### <a name="recipients"></a>Empfänger


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Empfänger|Matrix|Nein |



### <a name="recipient"></a>Empfänger


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|E-Mail|Zeichenfolge|Nein |
|Nachname|Zeichenfolge|Nein |
|Vorname|Zeichenfolge|Nein |
|ID|Zeichenfolge|Nein |


## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)