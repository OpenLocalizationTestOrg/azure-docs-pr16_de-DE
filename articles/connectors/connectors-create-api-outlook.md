<properties
pageTitle="Outlook.com | Microsoft Azure"
description="Erstellen Sie Logik apps mit Azure-App-Dienst an. Outlook.com-Verbinder können Sie Ihre e-Mail, Kalender und Kontakte zu verwalten. Sie können verschiedene Aktionen wie z. B. e-Mail senden, Besprechungen planen, Kontakte usw.."
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

# <a name="get-started-with-the-outlookcom-connector"></a>Erste Schritte mit der Outlook.com-Verbinder

Outlook.com-Verbinder können Sie Ihre e-Mail, Kalender und Kontakte zu verwalten. Sie können verschiedene Aktionen wie z. B. e-Mail senden, Besprechungen planen, Kontakte usw..

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus. 

Sie können beginnen, indem Sie eine app Logik jetzt erstellen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

Der Verbinder Outlook.com kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen die Daten in JSON und XML-Formaten. 

 Der Verbinder Outlook.com weist die folgenden Aktion(en) und/oder Triggern zur Verfügung:

### <a name="outlookcom-actions"></a>Outlook.com-Aktionen
Sie können folgende Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|[GetEmails](connectors-create-api-outlook.md#GetEmails)|Ruft e-Mails von einem Ordner|
|[SendEmail](connectors-create-api-outlook.md#SendEmail)|Sendet eine e-Mail-Nachricht|
|[DeleteEmail](connectors-create-api-outlook.md#DeleteEmail)|Löscht eine e-Mail-Nachricht nach id|
|[MarkAsRead](connectors-create-api-outlook.md#MarkAsRead)|Eine e-Mail-Nachricht als gelesen markiert|
|[ReplyTo](connectors-create-api-outlook.md#ReplyTo)|Antworten auf eine e-Mail-Nachricht|
|[GetAttachment](connectors-create-api-outlook.md#GetAttachment)|Ruft e-Mail-Anlage nach id|
|[SendMailWithOptions](connectors-create-api-outlook.md#SendMailWithOptions)|Senden einer e-Mail-Nachricht mit mehreren Optionen und warten, bis des Empfängers eine der Optionen wieder mit Antworten|
|[SendApprovalMail](connectors-create-api-outlook.md#SendApprovalMail)|Senden einer e-Mail Genehmigung und Warten auf eine Antwort von der Empfänger|
|[CalendarGetTables](connectors-create-api-outlook.md#CalendarGetTables)|Ruft Kalender|
|[CalendarGetItems](connectors-create-api-outlook.md#CalendarGetItems)|Ruft Elemente aus einem Kalender|
|[CalendarPostItem](connectors-create-api-outlook.md#CalendarPostItem)|Erstellt ein neues Ereignis|
|[CalendarGetItem](connectors-create-api-outlook.md#CalendarGetItem)|Ruft ein bestimmtes Element in einem Kalender|
|[CalendarDeleteItem](connectors-create-api-outlook.md#CalendarDeleteItem)|Löscht ein Kalenderelement|
|[CalendarPatchItem](connectors-create-api-outlook.md#CalendarPatchItem)|Teilweise aktualisiert ein Kalenderelements|
|[ContactGetTables](connectors-create-api-outlook.md#ContactGetTables)|Ruft Kontakteordner|
|[ContactGetItems](connectors-create-api-outlook.md#ContactGetItems)|Kontakte aus einem Kontakteordner abgerufen.|
|[ContactPostItem](connectors-create-api-outlook.md#ContactPostItem)|Erstellt einen neuen Kontakt|
|[ContactGetItem](connectors-create-api-outlook.md#ContactGetItem)|Ruft ab einem bestimmten Kontakt aus einem Kontakteordner|
|[ContactDeleteItem](connectors-create-api-outlook.md#ContactDeleteItem)|Löschen ein Kontakts|
|[ContactPatchItem](connectors-create-api-outlook.md#ContactPatchItem)|Teilweise aktualisiert ein Kontakts|
### <a name="outlookcom-triggers"></a>Outlook.com-Trigger
Sie können diese Ereignisse überwachen:

|Auslösen | Beschreibung|
|--- | ---|
|Klicken Sie auf Ereignis bald beginnen.|Einen Fluss ausgelöst wird, wenn eine anstehende Kalenderereignis gestartet werden kann|
|Klicken Sie auf neue e-Mail-Nachricht|Einen Fluss ausgelöst wird beim Eintreffen einer neuen e-Mail-Nachricht|
|Klicken Sie auf neue Elemente|Ausgelöst, wenn Sie ein neues Kalenderelement erstellt wurde|
|Klicken Sie auf aktualisierte Elemente|Ausgelöst, wenn ein Kalenderelement geändert wird|


## <a name="create-a-connection-to-outlookcom"></a>Herstellen einer Verbindung mit Outlook.com
Um Logik apps mit Outlook.com zu erstellen, müssen Sie zunächst eine **Verbindung** erstellen anschließend geben Sie die Details für die folgenden Eigenschaften: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Geben Sie Outlook.com-Anmeldeinformationen|
Nachdem Sie die Verbindung zu erstellen, können Sie es verwenden, führen Sie die Aktionen aus, und achten Sie auf die in diesem Artikel beschriebenen Trigger.

>[AZURE.INCLUDE [Steps to create a connection to Outlook.com](../../includes/connectors-create-api-outlook.md)] 

>[AZURE.TIP] Sie können diese Verbindung in andere apps Logik verwenden.  

## <a name="reference-for-outlookcom"></a>Referenz für Outlook.com
Version gilt: 1.0

## <a name="onupcomingevents"></a>OnUpcomingEvents
Zum Starten von Ereignis schnell: einen Fluss ausgelöst wird, wenn eine anstehende Kalenderereignis gestartet werden kann 

```GET: /Events/OnUpcomingEvents``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Abfrage|keine|Eindeutiger Bezeichner des Kalenders|
|lookAheadTimeInMinutes|ganze Zahl|Nein|Abfrage|15|Zeit (in Minuten) zum Suchen nach anstehen bevorstehende Ereignisse|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang wurde erfolgreich abgeschlossen|
|202|Vorgang wurde erfolgreich abgeschlossen|
|400|BadRequest|
|401|Nicht autorisierte|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


## <a name="getemails"></a>GetEmails
Abrufen von e-Mails: Ruft Sie e-Mails von einem Ordner 

```GET: /Mail``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Ordnerpfad|Zeichenfolge|Nein|Abfrage|Posteingang|Pfad zum Ordner zum Abrufen von e-Mails (Standard: 'Posteingang')|
|Nach oben|ganze Zahl|Nein|Abfrage|10|Anzahl der e-Mails abrufen (Standard: 10)|
|fetchOnlyUnread|Boolesch|Nein|Abfrage|WAHR|Abrufen von nur ungelesenen e-Mails?|
|includeAttachments|Boolesch|Nein|Abfrage|falsch|Wenn wahr, Anlagen festlegen auch zusammen mit der e-Mail einbezogen werden sollen|
|searchQuery|Zeichenfolge|Nein|Abfrage|keine|Suchabfrage zum Filtern von e-Mails|
|Überspringen|ganze Zahl|Nein|Abfrage|0|Anzahl der e-Mails zu überspringen (Standard: 0)|
|skipToken|Zeichenfolge|Nein|Abfrage|keine|Überspringen von Token auf Abruf neue Seite|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang wurde erfolgreich abgeschlossen|
|400|BadRequest|
|401|Nicht autorisierte|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


## <a name="sendemail"></a>SendEmail
Senden von Nachrichten: sendet eine e-Mail-Nachricht 

```POST: /Mail``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|' EmailMessage '| |Ja|Textkörper|keine|E-Mail|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang wurde erfolgreich abgeschlossen|
|400|BadRequest|
|401|Nicht autorisierte|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


## <a name="deleteemail"></a>DeleteEmail
E-Mail löschen: Löscht eine e-Mail-Nachricht nach Id 

```DELETE: /Mail/{messageId}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Nachrichten-ID|Zeichenfolge|Ja|Pfad|keine|Die e-Mail zu löschenden-ID|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang wurde erfolgreich abgeschlossen|
|400|BadRequest|
|401|Nicht autorisierte|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


## <a name="markasread"></a>MarkAsRead
Als gelesen markieren: eine e-Mail-Nachricht als gelesen markiert 

```POST: /Mail/MarkAsRead/{messageId}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Nachrichten-ID|Zeichenfolge|Ja|Pfad|keine|Lesen Sie-ID die e-Mail als markiert sein soll.|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang wurde erfolgreich abgeschlossen|
|400|BadRequest|
|401|Nicht autorisierte|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


## <a name="replyto"></a>ReplyTo
Antworten auf e-Mail: Antworten auf eine e-Mail-Nachricht 

```POST: /Mail/ReplyTo/{messageId}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Nachrichten-ID|Zeichenfolge|Ja|Pfad|keine|ID der e-Mail, um zu Antworten|
|Kommentar|Zeichenfolge|Ja|Abfrage|keine|Kommentar antworten|
|replyAll|Boolesch|Nein|Abfrage|falsch|Senden einer Antwort an alle Empfänger|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang wurde erfolgreich abgeschlossen|
|400|BadRequest|
|401|Nicht autorisierte|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


## <a name="getattachment"></a>GetAttachment
Abrufen der Anlage: Ruft e-Mail-Anlage nach Id 

```GET: /Mail/{messageId}/Attachments/{attachmentId}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Nachrichten-ID|Zeichenfolge|Ja|Pfad|keine|Die e-Mail-ID|
|attachmentId|Zeichenfolge|Ja|Pfad|keine|ID der Anlage herunterladen|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang wurde erfolgreich abgeschlossen|
|400|BadRequest|
|401|Nicht autorisierte|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


## <a name="onnewemail"></a>OnNewEmail
Klicken Sie auf neue e-Mail-Nachricht: einen Fluss ausgelöst wird beim Eintreffen einer neuen e-Mail-Nachricht 

```GET: /Mail/OnNewEmail``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Ordnerpfad|Zeichenfolge|Nein|Abfrage|Posteingang|Abrufen von e-Mail-Ordner (Standard: Posteingang)|
|An|Zeichenfolge|Nein|Abfrage|keine|Empfänger e-Mail-Adressen|
|Von|Zeichenfolge|Nein|Abfrage|keine|Von Adresse|
|Wichtigkeit: hoch|Zeichenfolge|Nein|Abfrage|Normal|Bedeutung der e-Mail (hoch, Normal, Niedrig) (Standard: Normal)|
|fetchOnlyWithAttachment|Boolesch|Nein|Abfrage|falsch|Abrufen von nur e-Mail-Nachrichten mit einer Anlage|
|includeAttachments|Boolesch|Nein|Abfrage|falsch|Einschließen von Anlagen|
|subjectFilter|Zeichenfolge|Nein|Abfrage|keine|Die Zeichenfolge, die in der Betreffzeile suchen|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang wurde erfolgreich abgeschlossen|
|202|Akzeptiert|
|400|BadRequest|
|401|Nicht autorisierte|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


## <a name="sendmailwithoptions"></a>SendMailWithOptions
Senden von e-Mails mit Optionen: senden eine e-Mail-Nachricht mit mehreren Optionen und warten, bis der Empfänger eine der Optionen wieder mit Antworten 

```POST: /mailwithoptions/$subscriptions``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|optionsEmailSubscription| |Ja|Textkörper|keine|Abonnementsanfrage für Optionen e-Mail|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|201|Abonnement erstellt|
|400|BadRequest|
|401|Nicht autorisierte|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


## <a name="sendapprovalmail"></a>SendApprovalMail
Senden von e-Mail mit der Genehmigung: Senden einer e-Mail Genehmigung und Warten auf eine Antwort von der Empfänger 

```POST: /approvalmail/$subscriptions``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|approvalEmailSubscription| |Ja|Textkörper|keine|Zur Genehmigung e-Mail-Abonnement stehende Frage|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|201|Abonnement erstellt|
|400|BadRequest|
|401|Nicht autorisierte|
|403|Verboten|
|500|Interner Serverfehler|
|Standard|Fehler bei Vorgang.|


## <a name="calendargettables"></a>CalendarGetTables
Abrufen von Kalendern: Ruft Kalender 

```GET: /datasets/calendars/tables``` 

Es sind keine Parameter für diesen Anruf
#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="calendargetitems"></a>CalendarGetItems
Abrufen von Ereignissen: Ruft Elemente aus einem Kalender 

```GET: /datasets/calendars/tables/{table}/items``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner des Kalenders zum Abrufen von|
|$filter|Zeichenfolge|Nein|Abfrage|keine|Die Abfrage ein ODATA Filter zum Einschränken der Anzahl von Einträgen|
|$orderby|Zeichenfolge|Nein|Abfrage|keine|Eine ODATA OrderBy Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|ganze Zahl|Nein|Abfrage|keine|Anzahl von Einträgen, überspringen (Standard = 0)|
|$top|ganze Zahl|Nein|Abfrage|keine|Maximale Anzahl von Einträgen zum Abrufen (Standard = 256)|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="calendarpostitem"></a>CalendarPostItem
Erstellen von Ereignis: erstellt ein neues Ereignis 

```POST: /datasets/calendars/tables/{table}/items``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutige ID eines Kalenders|
|Element| |Ja|Textkörper|keine|Kalenderelement erstellen|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="calendargetitem"></a>CalendarGetItem
Rufen Sie Ereignis: Ruft ein bestimmtes Elements in einem Kalender 

```GET: /datasets/calendars/tables/{table}/items/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutige ID eines Kalenders|
|ID|Zeichenfolge|Ja|Pfad|keine|Eindeutige ID zum Abrufen eines Kalenderelements|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="calendardeleteitem"></a>CalendarDeleteItem
Ereignis löschen: Löscht ein Kalenderelement 

```DELETE: /datasets/calendars/tables/{table}/items/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutige ID eines Kalenders|
|ID|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner des Kalenderelements löschen|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="calendarpatchitem"></a>CalendarPatchItem
Update Ereignis: teilweise aktualisiert ein Kalenderelements 

```PATCH: /datasets/calendars/tables/{table}/items/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutige ID eines Kalenders|
|ID|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner des Kalenderelements aktualisieren|
|Element| |Ja|Textkörper|keine|Kalenderelement aktualisieren|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="calendargetonnewitems"></a>CalendarGetOnNewItems
Klicken Sie auf neue Elemente: ausgelöst, wenn Sie ein neues Kalenderelement erstellt wurde 

```GET: /datasets/calendars/tables/{table}/onnewitems``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutige ID eines Kalenders|
|$filter|Zeichenfolge|Nein|Abfrage|keine|Die Abfrage ein ODATA Filter zum Einschränken der Anzahl von Einträgen|
|$orderby|Zeichenfolge|Nein|Abfrage|keine|Eine ODATA OrderBy Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|ganze Zahl|Nein|Abfrage|keine|Anzahl von Einträgen, überspringen (Standard = 0)|
|$top|ganze Zahl|Nein|Abfrage|keine|Maximale Anzahl von Einträgen zum Abrufen (Standard = 256)|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="calendargetonupdateditems"></a>CalendarGetOnUpdatedItems
Klicken Sie auf Artikel aktualisiert: ausgelöst, wenn ein Kalenderelement geändert wird 

```GET: /datasets/calendars/tables/{table}/onupdateditems``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutige ID eines Kalenders|
|$filter|Zeichenfolge|Nein|Abfrage|keine|Die Abfrage ein ODATA Filter zum Einschränken der Anzahl von Einträgen|
|$orderby|Zeichenfolge|Nein|Abfrage|keine|Eine ODATA OrderBy Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|ganze Zahl|Nein|Abfrage|keine|Anzahl von Einträgen, überspringen (Standard = 0)|
|$top|ganze Zahl|Nein|Abfrage|keine|Maximale Anzahl von Einträgen zum Abrufen (Standard = 256)|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="contactgettables"></a>ContactGetTables
Abrufen von Kontaktordnern: Ruft Kontakte Ordner 

```GET: /datasets/contacts/tables``` 

Es sind keine Parameter für diesen Anruf
#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="contactgetitems"></a>ContactGetItems
Abrufen von Kontakten: Ruft Sie Kontakte aus einem Kontakteordner 

```GET: /datasets/contacts/tables/{table}/items``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner für den Kontakteordner zum Abrufen von|
|$filter|Zeichenfolge|Nein|Abfrage|keine|Die Abfrage ein ODATA Filter zum Einschränken der Anzahl von Einträgen|
|$orderby|Zeichenfolge|Nein|Abfrage|keine|Eine ODATA OrderBy Abfrage zum Festlegen der Reihenfolge von Einträgen|
|$skip|ganze Zahl|Nein|Abfrage|keine|Anzahl von Einträgen, überspringen (Standard = 0)|
|$top|ganze Zahl|Nein|Abfrage|keine|Maximale Anzahl von Einträgen zum Abrufen (Standard = 256)|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="contactpostitem"></a>ContactPostItem
Kontakt erstellen: erstellt einen neuen Kontakt 

```POST: /datasets/contacts/tables/{table}/items``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner des eines Kontaktordners|
|Element| |Ja|Textkörper|keine|Kontakt erstellen|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="contactgetitem"></a>ContactGetItem
Erste Kontakt: Ruft ab einem bestimmten Kontakt aus einem Kontakteordner 

```GET: /datasets/contacts/tables/{table}/items/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner des eines Kontaktordners|
|ID|Zeichenfolge|Ja|Pfad|keine|Eindeutige ID eines Kontakts zum Abrufen von|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="contactdeleteitem"></a>ContactDeleteItem
Kontakt löschen: Löscht einen Kontakt 

```DELETE: /datasets/contacts/tables/{table}/items/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner des eines Kontaktordners|
|ID|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner des Kontakts zu löschen|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="contactpatchitem"></a>ContactPatchItem
Kontakt aktualisieren: teilweise aktualisiert ein Kontakts 

```PATCH: /datasets/contacts/tables/{table}/items/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Tabelle|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner des eines Kontaktordners|
|ID|Zeichenfolge|Ja|Pfad|keine|Eindeutiger Bezeichner des Kontakts aktualisieren|
|Element| |Ja|Textkörper|keine|Kontakt zu aktualisierenden Element|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|Standard|Fehler bei Vorgang.|


## <a name="object-definitions"></a>Objektdefinitionen 

### <a name="triggerbatchresponseidictionarystringobject"></a>TriggerBatchResponse [IDictionary [String, Object]]


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Matrix|Nein |



### <a name="object"></a>Objekt


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|



### <a name="sendmessage"></a>SendMessage


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Anlagen|Matrix|Nein |
|Von|Zeichenfolge|Nein |
|Cc|Zeichenfolge|Nein |
|"Bcc"|Zeichenfolge|Nein |
|Betreff|Zeichenfolge|Ja |
|Textkörper|Zeichenfolge|Ja |
|Wichtigkeit: hoch|Zeichenfolge|Nein |
|IsHtml|Boolesch|Nein |
|An|Zeichenfolge|Ja |



### <a name="sendattachment"></a>SendAttachment


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|@odata.type|Zeichenfolge|Nein |
|Namen|Zeichenfolge|Ja |
|ContentBytes|Zeichenfolge|Ja |



### <a name="receivemessage"></a>ReceiveMessage


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|IsRead|Boolesch|Nein |
|HasAttachment|Boolesch|Nein |
|DateTimeReceived|Zeichenfolge|Nein |
|Anlagen|Matrix|Nein |
|Von|Zeichenfolge|Nein |
|Cc|Zeichenfolge|Nein |
|"Bcc"|Zeichenfolge|Nein |
|Betreff|Zeichenfolge|Ja |
|Textkörper|Zeichenfolge|Ja |
|Wichtigkeit: hoch|Zeichenfolge|Nein |
|IsHtml|Boolesch|Nein |
|An|Zeichenfolge|Ja |



### <a name="receiveattachment"></a>ReceiveAttachment


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Ja |
|ContentType|Zeichenfolge|Ja |
|@odata.type|Zeichenfolge|Nein |
|Namen|Zeichenfolge|Ja |
|ContentBytes|Zeichenfolge|Ja |



### <a name="digestmessage"></a>DigestMessage


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Betreff|Zeichenfolge|Ja |
|Textkörper|Zeichenfolge|Nein |
|Wichtigkeit: hoch|Zeichenfolge|Nein |
|Übersicht|Matrix|Ja |
|Anlagen|Matrix|Nein |
|An|Zeichenfolge|Ja |



### <a name="triggerbatchresponsereceivemessage"></a>TriggerBatchResponse [ReceiveMessage]


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Matrix|Nein |



### <a name="datasetsmetadata"></a>DataSetsMetadata


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|tabellarische|nicht definiert|Nein |
|BLOB|nicht definiert|Nein |



### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Datenquelle|Zeichenfolge|Nein |
|displayName|Zeichenfolge|Nein |
|urlEncoding|Zeichenfolge|Nein |
|tableDisplayName|Zeichenfolge|Nein |
|tablePluralName|Zeichenfolge|Nein |



### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Datenquelle|Zeichenfolge|Nein |
|displayName|Zeichenfolge|Nein |
|urlEncoding|Zeichenfolge|Nein |



### <a name="tablemetadata"></a>TableMetadata


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Namen|Zeichenfolge|Nein |
|Titel|Zeichenfolge|Nein |
|X-ms-Berechtigung|Zeichenfolge|Nein |
|X-ms-Funktionen|nicht definiert|Nein |
|Schema|nicht definiert|Nein |



### <a name="tablecapabilitiesmetadata"></a>TableCapabilitiesMetadata


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|sortRestrictions|nicht definiert|Nein |
|filterRestrictions|nicht definiert|Nein |
|filterFunctions|Matrix|Nein |



### <a name="tablesortrestrictionsmetadata"></a>TableSortRestrictionsMetadata


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|sortierbar|Boolesch|Nein |
|unsortableProperties|Matrix|Nein |
|ascendingOnlyProperties|Matrix|Nein |



### <a name="tablefilterrestrictionsmetadata"></a>TableFilterRestrictionsMetadata


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|gefiltert|Boolesch|Nein |
|nonFilterableProperties|Matrix|Nein |
|requiredProperties|Matrix|Nein |



### <a name="optionsemailsubscription"></a>OptionsEmailSubscription


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|NotificationUrl|Zeichenfolge|Nein |
|Nachricht|nicht definiert|Nein |



### <a name="messagewithoptions"></a>MessageWithOptions


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Betreff|Zeichenfolge|Ja |
|Optionen|Zeichenfolge|Ja |
|Textkörper|Zeichenfolge|Nein |
|Wichtigkeit: hoch|Zeichenfolge|Nein |
|Anlagen|Matrix|Nein |
|An|Zeichenfolge|Ja |



### <a name="subscriptionresponse"></a>SubscriptionResponse


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|Ressource|Zeichenfolge|Nein |
|notificationType|Zeichenfolge|Nein |
|notificationUrl|Zeichenfolge|Nein |



### <a name="approvalemailsubscription"></a>ApprovalEmailSubscription


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|NotificationUrl|Zeichenfolge|Nein |
|Nachricht|nicht definiert|Nein |



### <a name="approvalmessage"></a>ApprovalMessage


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Betreff|Zeichenfolge|Ja |
|Optionen|Zeichenfolge|Ja |
|Textkörper|Zeichenfolge|Nein |
|Wichtigkeit: hoch|Zeichenfolge|Nein |
|Anlagen|Matrix|Nein |
|An|Zeichenfolge|Ja |



### <a name="approvalemailresponse"></a>ApprovalEmailResponse


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Winkelseite|Zeichenfolge|Nein |



### <a name="tableslist"></a>TablesList


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Matrix|Nein |



### <a name="table"></a>Tabelle


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Namen|Zeichenfolge|Nein |
|DisplayName|Zeichenfolge|Nein |



### <a name="item"></a>Element


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ItemInternalId|Zeichenfolge|Nein |



### <a name="calendaritemslist"></a>CalendarItemsList


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Matrix|Nein |



### <a name="calendaritem"></a>CalendarItem


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ItemInternalId|Zeichenfolge|Nein |



### <a name="contactitemslist"></a>ContactItemsList


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Matrix|Nein |



### <a name="contactitem"></a>ContactItem


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ItemInternalId|Zeichenfolge|Nein |



### <a name="datasetslist"></a>DataSetsList


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Matrix|Nein |



### <a name="dataset"></a>DataSet


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Namen|Zeichenfolge|Nein |
|DisplayName|Zeichenfolge|Nein |


## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)