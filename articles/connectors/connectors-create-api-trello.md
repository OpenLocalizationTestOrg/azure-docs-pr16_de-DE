<properties
pageTitle="Trello | Microsoft Azure"
description="Erstellen Sie Logik apps mit Azure-App-Dienst an. Trello bietet Ihnen mit Perspektive über alle Ihre Projekte, bei der Arbeit und zu Hause.  Es ist eine einfache, kostenlos, flexible und visuelle Möglichkeit zum Verwalten von Projekten und Organisieren von nichts.  Herstellen einer Verbindung Trello zum Verwalten Ihrer Bretter, Listen und Karten mit"
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

# <a name="get-started-with-the-trello-connector"></a>Erste Schritte mit der Trello Verbinder

Trello bietet Ihnen mit Perspektive über alle Ihre Projekte, bei der Arbeit und zu Hause.  Es ist eine einfache, kostenlos, flexible und visuelle Möglichkeit zum Verwalten von Projekten und Organisieren von nichts.  Verbinden Sie mit Trello Boards, Listen und Karten verwalten.

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus. 

Sie können beginnen, indem Sie eine app Logik jetzt erstellen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

Der Verbinder Trello kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen die Daten in JSON und XML-Formaten. 

 Der Verbinder Trello weist die folgenden Aktion(en) und/oder Triggern zur Verfügung:

### <a name="trello-actions"></a>Trello Aktionen
Sie können folgende Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|[ListCards](connectors-create-api-trello.md#listcards)|Liste Karten in Brett|
|[GetCard](connectors-create-api-trello.md#getcard)|Karte nach Id abrufen|
|[UpdateCard](connectors-create-api-trello.md#updatecard)|Aktualisieren der Karte|
|[DeleteCard](connectors-create-api-trello.md#deletecard)|Löschen der Karte|
|[CreateCard](connectors-create-api-trello.md#createcard)|Erstellt eine neue Karte in Ihr Konto trello|
|[ListBoards](connectors-create-api-trello.md#listboards)|Liste boards|
|[GetBoard](connectors-create-api-trello.md#getboard)|Ruft Brett nach Id|
|[ListLists](connectors-create-api-trello.md#listlists)|Listen der Karte in der Karte|
|[GetList](connectors-create-api-trello.md#getlist)|Ruft Liste nach Id|
### <a name="trello-triggers"></a>Trello Trigger
Sie können diese Ereignisse überwachen:

|Trigger | Beschreibung|
|--- | ---|
|Wenn eine neue Karte ein Brett hinzugefügt wird|Einen Fluss ausgelöst wird, wenn eine neue Karte ein Brett hinzugefügt wird|
|Wenn eine neue Karte zu einer Liste hinzugefügt wird|Einen Fluss ausgelöst wird, wenn eine neue Karte zu einer Liste hinzugefügt wird|


## <a name="create-a-connection-to-trello"></a>Herstellen einer Verbindung mit Trello
Um Logik apps mit Trello zu erstellen, müssen Sie zunächst eine **Verbindung** erstellen anschließend geben Sie die Details für die folgenden Eigenschaften: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Geben Sie Trello-Anmeldeinformationen|
Nachdem Sie die Verbindung zu erstellen, können Sie es verwenden, führen Sie die Aktionen aus, und achten Sie auf die in diesem Artikel beschriebenen Trigger. 

>[AZURE.INCLUDE [Steps to create a connection to Trello](../../includes/connectors-create-api-trello.md)]

>[AZURE.TIP] Sie können diese Verbindung in andere apps Logik verwenden.

## <a name="reference-for-trello"></a>Referenz für Trello
Version gilt: 1.0

## <a name="onnewcardinboard"></a>OnNewCardInBoard
Wenn eine neue Karte für ein Brett hinzugefügt wird: einen Fluss ausgelöst wird, wenn eine neue Karte ein Brett hinzugefügt wird 

```GET: /trigger/boards/{board_id}/cards``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Pfad|keine|Eindeutige Id der Karte Karten in abgerufen werden sollen.|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler beim Vorgang|


## <a name="onnewcardinlist"></a>OnNewCardInList
Wenn eine neue Karte zu einer Liste hinzugefügt wird: einen Fluss ausgelöst wird, wenn eine neue Karte zu einer Liste hinzugefügt wird 

```GET: /trigger/lists/{list_id}/cards``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Abfrage|keine|Eindeutige Id der Karte Karten in abgerufen werden sollen.|
|list_id|Zeichenfolge|Ja|Pfad|keine|Eindeutige Id der Liste Karten in abgerufen werden sollen.|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler beim Vorgang|


## <a name="listcards"></a>ListCards
Karten in Brett Liste: Karten in Brett Liste 

```GET: /boards/{board_id}/cards``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Pfad|keine|ID der Karte alle Karten abgerufen werden sollen.|
|Aktionen|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Aktionen aus, um zurückzukehren. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Anlagen|Boolesch|Nein|Abfrage|keine|Anzeigen von Anlagen|
|attachment_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Anlagenfelder zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Aufkleber|Boolesch|Nein|Abfrage|keine|Aufkleber anzeigen|
|Mitglieder|Boolesch|Nein|Abfrage|keine|Elemente anzeigen|
|memeber_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder Mitglied zurückzugebenden. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|CheckItemStates|Boolesch|Nein|Abfrage|keine|Die Karte Staaten zurück|
|Checklisten|Zeichenfolge|Nein|Abfrage|keine|Checklisten anzeigen|
|Grenzwert|ganze Zahl|Nein|Abfrage|keine|Die maximale Anzahl von Ergebnissen zurück, die zwischen 1 und 1000|
|seit|Zeichenfolge|Nein|Abfrage|keine|Alle Karten nach diesem Datum abrufen|
|Bevor Sie|Zeichenfolge|Nein|Abfrage|keine|Alle Karten vor diesem Datum abrufen|
|Filter|Zeichenfolge|Nein|Abfrage|keine|Filtern der Antwort|
|(Felder)|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Karte Felder zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler beim Vorgang|


## <a name="getcard"></a>GetCard
Erste Karte nach Id: Get-Karte nach Id 

```GET: /cards/{card_id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Abfrage|keine|ID der Karte Karten in abgerufen werden sollen.|
|card_id|Zeichenfolge|Ja|Pfad|keine|ID der Karte abgerufen werden sollen|
|Aktionen|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Aktionen aus, um zurückzukehren. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|actions_entities|Boolesch|Nein|Abfrage|keine|Aktion Einheiten zurück|
|actions_display|Boolesch|Nein|Abfrage|keine|Aktion zurückgegeben wird angezeigt.|
|actions_limit|ganze Zahl|Nein|Abfrage|keine|Die maximale Anzahl der zurückzugebenden Aktionen|
|action_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Felder der Aktion für jede Aktion zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|action_memberCreator_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Aktion Element Ersteller Felder zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Anlagen|Boolesch|Nein|Abfrage|keine|Zurückgeben von Anlagen|
|attachement_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Anlagenfelder für jede Anlage zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Mitglieder|Boolesch|Nein|Abfrage|keine|Mitglieder zurück|
|member_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Mitglied Felder, um für jedes Element zurückzukehren. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|membersVoted|Boolesch|Nein|Abfrage|keine|Mitglieder hat bewertet zurück|
|memberVoted_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Mitglied hat bewertet Felder, um für jedes Element voted zurückzukehren. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|checkItemStates|Boolesch|Nein|Abfrage|keine|Rückgabetyp Karte Staaten|
|checkItemState_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Statusfelder für jede der Karte Elementzustand zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Checklisten|Zeichenfolge|Nein|Abfrage|keine|Checklisten zurück|
|checklist_fields|Zeichenfolge|Nein|Abfrage|keine|Liste Checkliste für die Felder für jede Checkliste zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Karte|Boolesch|Nein|Abfrage|keine|Eingabe der Karte auf der Karte gehört|
|board_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder Brett zurückzugebenden. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Liste|Boolesch|Nein|Abfrage|keine|Zurückgeben der Liste der Karte gehört|
|list_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Liste der Felder, um zurückzukehren. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Aufkleber|Boolesch|Nein|Abfrage|keine|Aufkleber zurück|
|sticker_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Felder Aufkleber für jede Aufkleber zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|(Felder)|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Karte Felder zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler beim Vorgang|


## <a name="updatecard"></a>UpdateCard
Update Karte: Update-Karte 

```PUT: /cards/{card_id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Abfrage|keine|ID der Karte Karten aus abgerufen werden sollen.|
|card_id|Zeichenfolge|Ja|Pfad|keine|ID der Karte aktualisieren|
|updateCard| |Ja|Textkörper|keine|Aktualisierte Kartenwerte|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler beim Vorgang|


## <a name="deletecard"></a>DeleteCard
Löschen der Karte: Karte löschen 

```DELETE: /cards/{card_id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Abfrage|keine|ID der Karte Karten aus abgerufen werden sollen.|
|card_id|Zeichenfolge|Ja|Pfad|keine|ID der Karte zu löschen|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler beim Vorgang|


## <a name="createcard"></a>CreateCard
Erstellen der Karte: erstellt eine neue Karte in Ihr Konto Trello 

```POST: /cards``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Abfrage|keine|Eindeutige Id der zum Erstellen von Karten in-Karte|
|newCard| |Ja|Textkörper|keine|Glückwunschkarte an die Karte Trello hinzufügen|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler beim Vorgang|


## <a name="listboards"></a>ListBoards
Liste der Boards: Liste Boards 

```GET: /member/me/boards``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Filter|Zeichenfolge|Nein|Abfrage|keine|Listenfilter Brett Ergebnisse vor. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|(Felder)|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder Brett zurückzugebenden. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Aktionen|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Aktion Felder zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|actions_entities|Boolesch|Nein|Abfrage|keine|Aktion Einheiten zurück|
|actions_limit|ganze Zahl|Nein|Abfrage|keine|Die maximale Anzahl der zurückzugebenden Aktionen|
|actions_format|Zeichenfolge|Nein|Abfrage|keine|Geben Sie das Format der zurückzugebenden Aktionen|
|actions_since|Zeichenfolge|Nein|Abfrage|keine|Aktionen nach dem angegebenen Datum zurück|
|action_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder der Aktion zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Mitgliedschaften|Zeichenfolge|Nein|Abfrage|keine|Geben Sie Mitgliedschaftsinformationen zurückzugebenden an. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Organisation|Boolesch|Nein|Abfrage|keine|Gibt an, dass Sie Informationen über Ihre Organisation zurückzukehren.|
|organization_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Organisation Felder zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Listen|Zeichenfolge|Nein|Abfrage|keine|Geben Sie an, ob Listen zurückgegeben, die an der Karte gehören|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler beim Vorgang|


## <a name="getboard"></a>GetBoard
Ruft Brett nach Id: Ruft Brett nach Id 

```GET: /boards/{board_id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Pfad|keine|Eindeutige Id der abzurufenden-Karte|
|Aktionen|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Aktionen aus, um zurückzukehren. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|action_entities|Boolesch|Nein|Abfrage|keine|Geben Sie an, ob Personen Aktion zurück|
|actions_display|Boolesch|Nein|Abfrage|keine|Geben Sie an, ob zurückzugebenden Aktionen anzeigen|
|actions_format|Zeichenfolge|Nein|Abfrage|keine|Geben Sie das Format der zurückzugebenden Aktionen|
|actions_since|Zeichenfolge|Nein|Abfrage|keine|Nur die Aktionen nach diesem Datum zurück|
|actions_limit|ganze Zahl|Nein|Abfrage|keine|Die maximale Anzahl der zurückzugebenden Aktionen|
|action_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder für jedes Feld zurückgeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|action_memeber|Boolesch|Nein|Abfrage|keine|Geben Sie an, ob die Aktion Mitglieder zurückzukehren.|
|action_member_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Mitglied-Felder mit jeder Aktion Mitglied zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|action_memberCreator|Boolesch|Nein|Abfrage|keine|Geben Sie an, ob die Aktion Mitglied Ersteller zurückzukehren.|
|action_memberCreator_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Aktion Mitglied Ersteller Felder zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Karten|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Karten zurück|
|card_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder für jede Karte zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|card_attachments|Boolesch|Ja|Abfrage|keine|Geben Sie an, ob Anlagen auf Karten zurückzukehren.|
|card_attachment_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Anlagenfelder für jede Anlage zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|card_checklists|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Checklisten, um für jede Karte zurückzukehren.|
|card_stickers|Boolesch|Nein|Abfrage|keine|Geben Sie an, ob Karte Aufkleber zurückzukehren.|
|boardStarts|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Karte Sterne zurückzugebenden|
|Etiketten|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Daten zurück|
|label_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Bezeichnung-Felder für jedes Etikett zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|labels_limits|ganze Zahl|Nein|Abfrage|keine|Die maximale Anzahl der zurückzugebenden Etiketten|
|Listen|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Listen zurück|
|list_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Listenfelder für jede Liste zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Mitgliedschaften|Zeichenfolge|Nein|Abfrage|keine|Liste der Mitgliedschaft an zurückzukehren. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|memberships_member|Boolesch|Nein|Abfrage|keine|Geben Sie an, ob die Mitgliedschaft Mitglieder zurückzukehren.|
|memberships_member_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Mitglied-Felder, um für jedes Element Mitgliedschaft zurückzukehren. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Mitglieder|Zeichenfolge|Nein|Abfrage|keine|Liste der Mitgliedern zur Rückkehr|
|member_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Mitglied Felder, um für jedes Element zurückzukehren. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|membersInvited|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die eingeladenen Mitgliedern zur Rückkehr|
|membersInvited_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder, die Sie für jede zurückgeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Checklisten|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Checklisten zurück|
|checklist_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Felder Checkliste für jede Checkliste zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Organisation|Boolesch|Nein|Abfrage|keine|Geben Sie an, ob die Organisationsinformationen zurückgeben|
|organization_fields|Zeichenfolge|Nein|Abfrage|keine|Liste der Organisation Felder für jede Organisation zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|organization_memberships|Zeichenfolge|Nein|Abfrage|keine|Liste der Organisation Mitgliedschaften zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|myPerfs|Boolesch|Nein|Abfrage|keine|Geben Sie an, ob meine Perfs zurückgegeben.|
|(Felder)|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder aus, um zurückzukehren. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler beim Vorgang|


## <a name="listlists"></a>ListLists
Liste der Karte Listen in Brett: Listen Karte in der Karte 

```GET: /boards/{board_id}/lists``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Pfad|keine|Eindeutige Id der Karte zum Abrufen von Listen|
|Karten|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Karten zurück|
|card_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Karte Felder zurück. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Filter|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Filtereigenschaft für Listen|
|(Felder)|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder aus, um zurückzukehren. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler beim Vorgang|


## <a name="getlist"></a>GetList
Ruft Liste nach Id: Ruft Liste nach Id 

```GET: /lists/{list_id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|board_id|Zeichenfolge|Ja|Abfrage|keine|Eindeutige Id der Karte die Listen aus abgerufen werden sollen.|
|list_id|Zeichenfolge|Ja|Pfad|keine|Eindeutige Id der Liste abgerufen werden sollen|
|Karten|Zeichenfolge|Nein|Abfrage|keine|Geben Sie die Karten zurück|
|card_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Karte Felder für jede Karte zurückgegeben. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|Karte|Boolesch|Nein|Abfrage|keine|Geben Sie an, ob Brett Informationen zurück|
|board_fields|Zeichenfolge|Nein|Abfrage|keine|Listen Sie die Felder Brett zurückzugebenden. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|
|(Felder)|Zeichenfolge|Nein|Abfrage|keine|Liste der Liste der Felder, um zurückzukehren. Geben Sie 'alle' oder eine durch Kommas getrennte Liste mit gültigen Werten|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|
|400|Ungültige Anforderung|
|401|Nicht autorisierte|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler beim Vorgang|


## <a name="object-definitions"></a>Objektdefinitionen 

### <a name="card"></a>Karte


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|checkItemStates|Matrix|Nein |
|geschlossen|Boolesch|Nein |
|dateLastActivity|Zeichenfolge|Nein |
|DESC|Zeichenfolge|Nein |
|idBoard|Zeichenfolge|Nein |
|Zonen|Zeichenfolge|Nein |
|idMembersVoted|Matrix|Nein |
|idShort|ganze Zahl|Nein |
|idAttachmentCover|Zeichenfolge|Nein |
|manualCoverAttachment|Boolesch|Nein |
|idLabels|Matrix|Nein |
|Namen|Zeichenfolge|Nein |
|POS|ganze Zahl|Nein |
|shortLink|Zeichenfolge|Nein |
|Badges|nicht definiert|Nein |
|fällig|Zeichenfolge|Nein |
|E-Mail|Zeichenfolge|Nein |
|shortUrl|Zeichenfolge|Nein |
|abonniert|Boolesch|Nein |
|URL|Zeichenfolge|Nein |



### <a name="badges"></a>Badges


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Stimmen|ganze Zahl|Nein |
|ViewingMemberVoted|Boolesch|Nein |
|Abonniert|Boolesch|Nein |
|Fogbugz|Zeichenfolge|Nein |
|CheckItems|ganze Zahl|Nein |
|CheckItemsChecked|ganze Zahl|Nein |
|Kommentare|ganze Zahl|Nein |
|Anlagen|ganze Zahl|Nein |
|Beschreibung|Boolesch|Nein |
|Fällig|Zeichenfolge|Nein |



### <a name="object"></a>Objekt


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|



### <a name="createcard"></a>CreateCard


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Zonen|Zeichenfolge|Ja |
|Namen|Zeichenfolge|Ja |
|DESC|Zeichenfolge|Nein |
|POS|Zeichenfolge|Nein |
|idMembers|Zeichenfolge|Nein |
|idLabels|Zeichenfolge|Nein |
|urlSource|Zeichenfolge|Nein |
|fileSource|Zeichenfolge|Nein |
|idCardSource|Zeichenfolge|Nein |
|keepFromSource|Zeichenfolge|Nein |



### <a name="updatecard"></a>UpdateCard


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Namen|Zeichenfolge|Nein |
|DESC|Zeichenfolge|Nein |
|geschlossen|Boolesch|Nein |
|idMembers|Zeichenfolge|Nein |
|idAttachmentCover|Zeichenfolge|Nein |
|Zonen|Zeichenfolge|Nein |
|idBoard|Zeichenfolge|Nein |
|POS|Zeichenfolge|Nein |
|fällig|Zeichenfolge|Nein |
|abonniert|Boolesch|Nein |



### <a name="board"></a>Karte


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|geschlossen|Boolesch|Nein |
|dateLastActivity|Zeichenfolge|Nein |
|dateLastView|Zeichenfolge|Nein |
|DESC|Zeichenfolge|Nein |
|idOrganization|Zeichenfolge|Nein |
|Einladungen|Matrix|Nein |
|eingeladen|Boolesch|Nein |
|labelNames|nicht definiert|Nein |
|Mitgliedschaften|Matrix|Nein |
|Namen|Zeichenfolge|Nein |
|angehefteten|Boolesch|Nein |
|powerUps|Matrix|Nein |
|perfs|nicht definiert|Nein |
|shortLink|Zeichenfolge|Nein |
|shortUrl|Zeichenfolge|Nein |
|starred|Zeichenfolge|Nein |
|abonniert|Zeichenfolge|Nein |
|URL|Zeichenfolge|Nein |



### <a name="label"></a>Beschriftung


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Grün|Zeichenfolge|Nein |
|Gelb|Zeichenfolge|Nein |
|Orange|Zeichenfolge|Nein |
|Rot|Zeichenfolge|Nein |
|Lila|Zeichenfolge|Nein |
|Blau|Zeichenfolge|Nein |
|Sky|Zeichenfolge|Nein |
|limone|Zeichenfolge|Nein |
|rosa|Zeichenfolge|Nein |
|Schwarz|Zeichenfolge|Nein |



### <a name="membership"></a>Mitgliedschaft


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|idMember|Zeichenfolge|Nein |
|memberType|Zeichenfolge|Nein |
|nicht bestätigt|Boolesch|Nein |



### <a name="perfs"></a>Perfs


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|permissionLevel|Zeichenfolge|Nein |
|Abstimmung|Zeichenfolge|Nein |
|Kommentare|Zeichenfolge|Nein |
|Einladungen|Zeichenfolge|Nein |
|selfJoin|Boolesch|Nein |
|cardCovers|Boolesch|Nein |
|calendarFeedEnabled|Boolesch|Nein |
|Hintergrund|Zeichenfolge|Nein |
|Hintergrundfarbe|Zeichenfolge|Nein |
|backgroundImage|Zeichenfolge|Nein |
|backgroundImageScaled|Zeichenfolge|Nein |
|backgroundTile|Boolesch|Nein |
|backgroundBrightness|Zeichenfolge|Nein |
|canBePublic|Boolesch|Nein |
|canBeOrg|Boolesch|Nein |
|canBePrivate|Boolesch|Nein |
|canInvite|Boolesch|Nein |



### <a name="list"></a>Liste


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|Zeichenfolge|Nein |
|Namen|Zeichenfolge|Nein |
|geschlossen|Boolesch|Nein |
|idBoard|Zeichenfolge|Nein |
|POS|Zahl|Nein |
|abonniert|Boolesch|Nein |
|Karten|Matrix|Nein |
|Karte|nicht definiert|Nein |


## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)