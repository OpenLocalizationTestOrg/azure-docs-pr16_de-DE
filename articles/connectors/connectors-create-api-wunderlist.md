<properties
pageTitle="Wunderlist | Microsoft Azure"
description="Erstellen Sie Logik apps mit Azure-App-Dienst an. Wunderlist bieten einen erledigen Listen- und Task-Manager damit andere Personen, deren privates fertig abrufen können.  Ob Sie eine Einkaufsliste durch eine schenken freigegeben haben, erleichtert an einem Projekt arbeiten, oder einen Urlaub Planung, Wunderlist zu erfassen, freizugeben und zu Ihrer To¬dos durchführen. Wunderlist synchronisiert sofort zwischen Ihrem Telefon, Tablets und Computer, damit Sie alle Ihre Aufgaben von überall darauf zugreifen können."
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

# <a name="get-started-with-the-wunderlist-connector"></a>Erste Schritte mit der Wunderlist Verbinder

Wunderlist bieten einen erledigen Listen- und Task-Manager damit andere Personen, deren privates fertig abrufen können.  Ob Sie eine Einkaufsliste durch eine schenken freigegeben haben, erleichtert an einem Projekt arbeiten, oder einen Urlaub Planung, Wunderlist zu erfassen, freizugeben und zu Ihrer To¬dos durchführen. Wunderlist synchronisiert sofort zwischen Ihrem Telefon, Tablets und Computer, damit Sie alle Ihre Aufgaben von überall darauf zugreifen können.

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus. 

Sie können beginnen, indem Sie eine app Logik jetzt erstellen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

Der Verbinder Wunderlist kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen die Daten in JSON und XML-Formaten. 

 Der Verbinder Wunderlist weist die folgenden Aktion(en) und/oder Triggern zur Verfügung:

### <a name="wunderlist-actions"></a>Wunderlist Aktionen
Sie können folgende Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|[RetrieveLists](connectors-create-api-wunderlist.md#retrievelists)|Rufen Sie die Listen, die mit Ihrem Konto verbunden sind.|
|[CreateList](connectors-create-api-wunderlist.md#createlist)|Erstellen einer Liste an.|
|[ListTasks](connectors-create-api-wunderlist.md#listtasks)|Abrufen von Aufgaben aus einer bestimmten Liste.|
|[CreateTask](connectors-create-api-wunderlist.md#createtask)|Erstellen einer Aufgabe|
|[ListSubTasks](connectors-create-api-wunderlist.md#listsubtasks)|Abrufen von Teilvorgängen aus einer bestimmten Liste oder aus einer bestimmten Aufgabe.|
|[CreateSubTask](connectors-create-api-wunderlist.md#createsubtask)|Erstellen eines Teilvorgangs innerhalb einer bestimmten Aufgabe|
|[ListNotes](connectors-create-api-wunderlist.md#listnotes)|Abrufen von Notizen für eine bestimmte Liste oder einen bestimmten Vorgang.|
|[CreateNote](connectors-create-api-wunderlist.md#createnote)|Hinzufügen einer Notiz zu einer bestimmten Aufgabe|
|[ListComments](connectors-create-api-wunderlist.md#listcomments)|Abrufen von Vorgang-Kommentare zu einer bestimmten Liste oder einen bestimmten Vorgang.|
|[CreateComment](connectors-create-api-wunderlist.md#createcomment)|Hinzufügen eines Kommentars zu einer bestimmten Aufgabe|
|[RetrieveReminders](connectors-create-api-wunderlist.md#retrievereminders)|Abrufen von Erinnerungen für einer bestimmten Liste oder einen bestimmten Vorgang an.|
|[CreateReminder](connectors-create-api-wunderlist.md#createreminder)|Festlegen einer Erinnerung.|
|[RetrieveFiles](connectors-create-api-wunderlist.md#retrievefiles)|Abrufen von Dateien, die einer bestimmten Liste oder einen bestimmten Vorgang.|
|[GetList](connectors-create-api-wunderlist.md#getlist)|Ruft ab einer bestimmten Liste|
|[DeleteList](connectors-create-api-wunderlist.md#deletelist)|Löscht eine Liste|
|[UpdateList](connectors-create-api-wunderlist.md#updatelist)|Aktualisieren einer bestimmten Liste|
|[GetTask](connectors-create-api-wunderlist.md#gettask)|Ruft einen bestimmten Vorgang|
|[UpdateTask](connectors-create-api-wunderlist.md#updatetask)|Einen bestimmten Vorgang aktualisiert|
|[DeleteTask](connectors-create-api-wunderlist.md#deletetask)|Löscht einen bestimmten Vorgang|
|[GetSubTask](connectors-create-api-wunderlist.md#getsubtask)|Ruft eine bestimmte Teilvorgangs|
|[UpdateSubTask](connectors-create-api-wunderlist.md#updatesubtask)|Eine bestimmte Teilvorgangs aktualisiert|
|[DeleteSubTask](connectors-create-api-wunderlist.md#deletesubtask)|Löscht eine bestimmte Teilvorgangs|
|[GetNote](connectors-create-api-wunderlist.md#getnote)|Abrufen einer bestimmten Notiz|
|[UpdateNote](connectors-create-api-wunderlist.md#updatenote)|Aktualisieren einer bestimmten Notiz|
|[DeleteNote](connectors-create-api-wunderlist.md#deletenote)|Löschen einer bestimmten Notiz|
|[GetComment](connectors-create-api-wunderlist.md#getcomment)|Abrufen eines bestimmten Aufgabe Kommentars|
|[UpdateReminder](connectors-create-api-wunderlist.md#updatereminder)|Aktualisieren einer bestimmten Erinnerung|
|[DeleteReminder](connectors-create-api-wunderlist.md#deletereminder)|Löschen einer bestimmten Erinnerung|
### <a name="wunderlist-triggers"></a>Wunderlist Trigger
Sie können diese Ereignisse überwachen:

|Auslösen | Beschreibung|
|--- | ---|
|Wenn eine Aufgabe fällig ist|Einen neuen Fluss ausgelöst wird, wenn eine Aufgabe in der Liste fällig ist|
|Wenn eine neue Aufgabe erstellt wird|Einen neuen Fluss ausgelöst wird, wenn Sie in der Liste eine neue Aufgabe erstellt wurde|
|Tritt auf, wenn eine Erinnerung|Einen neuen Fluss ausgelöst wird, wenn eine Erinnerung erfolgt|


## <a name="create-a-connection-to-wunderlist"></a>Herstellen einer Verbindung mit Wunderlist
Um Logik apps mit Wunderlist zu erstellen, müssen Sie zunächst eine **Verbindung** erstellen anschließend geben Sie die Details für die folgenden Eigenschaften: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Geben Sie Wunderlist-Anmeldeinformationen|
Nachdem Sie die Verbindung zu erstellen, können Sie es verwenden, führen Sie die Aktionen aus, und achten Sie auf die in diesem Artikel beschriebenen Trigger. 


>[AZURE.INCLUDE [Steps to create a connection to Wunderlist](../../includes/connectors-create-api-wunderlist.md)] 


>[AZURE.TIP] Sie können diese Verbindung in andere apps Logik verwenden.

## <a name="reference-for-wunderlist"></a>Referenz für Wunderlist
Version gilt: 1.0

## <a name="triggertaskdue"></a>TriggerTaskDue
Wenn eine Aufgabe fällig ist: keinen neuen Datenfluss ausgelöst wird, wenn eine Aufgabe in der Liste fällig ist 

```GET: /trigger/tasksdue``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|


## <a name="triggertasknew"></a>TriggerTaskNew
Beim Erstellen eines neuen Vorgangs: keinen neuen Datenfluss ausgelöst wird, wenn Sie in der Liste eine neue Aufgabe erstellt wurde 

```GET: /trigger/tasksnew``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|


## <a name="triggerreminder"></a>TriggerReminder
Tritt auf, wenn eine Erinnerung: keinen neuen Datenfluss ausgelöst wird, wenn eine Erinnerung erfolgt 

```GET: /trigger/reminders``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|TASK_ID|ganze Zahl|Nein|Abfrage|keine|Vorgangsnummer|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|


## <a name="retrievelists"></a>RetrieveLists
Abrufen von Listen: Abrufen die Listen, die mit Ihrem Konto verbunden sind. 

```GET: /lists``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="createlist"></a>CreateList
Erstellen einer Liste: Erstellen einer Liste. 

```POST: /lists``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Bereitstellen| |Ja|Textkörper|keine|Neue Liste erstellt werden|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|Standard|Fehler bei Vorgang.|


## <a name="listtasks"></a>ListTasks
Abrufen von Aufgaben: Aufgaben aus einer bestimmten Liste abrufen. 

```GET: /tasks``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|abgeschlossen|Boolesch|Nein|Abfrage|keine|Abgeschlossen|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="createtask"></a>CreateTask
Erstellen einer Aufgabe: Erstellen einer Aufgabe 

```POST: /tasks``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Bereitstellen| |Ja|Textkörper|keine|Neue Aufgabe erstellt werden|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|201|Erstellt|


## <a name="listsubtasks"></a>ListSubTasks
Abrufen von Teilvorgängen: Teilvorgänge aus einer bestimmten Liste oder aus einer bestimmten Aufgabe abrufen. 

```GET: /subtasks``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|TASK_ID|ganze Zahl|Nein|Abfrage|keine|Vorgangsnummer|
|abgeschlossen|Boolesch|Nein|Abfrage|keine|Abgeschlossen|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="createsubtask"></a>CreateSubTask
Erstellen ein Teilvorgangs: erstellen ein Teilvorgangs innerhalb einer bestimmten Aufgabe 

```POST: /subtasks``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Bereitstellen| |Ja|Textkörper|keine|Neue Teilvorgangs erstellt werden|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|201|Erstellt|


## <a name="listnotes"></a>ListNotes
Abrufen von Notizen: Abrufen von Notizen für eine bestimmte Liste oder einen bestimmten Vorgang. 

```GET: /notes``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|TASK_ID|ganze Zahl|Nein|Abfrage|keine|Vorgangsnummer|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="createnote"></a>CreateNote
Erstellen einer Notiz: Hinzufügen einer Notiz zu einer bestimmten Aufgabe 

```POST: /notes``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Bereitstellen| |Ja|Textkörper|keine|Neue Notiz erstellt werden|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|201|Erstellt|


## <a name="listcomments"></a>ListComments
Erste Aufgabenkommentare: Vorgang-Kommentare zu einer bestimmten Liste oder einen bestimmten Vorgang abzurufen. 

```GET: /task_comments``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|TASK_ID|ganze Zahl|Nein|Abfrage|keine|Vorgangsnummer|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="createcomment"></a>CreateComment
Hinzufügen eines Kommentars zu einer Aufgabe: Hinzufügen eines Kommentars zu einer bestimmten Aufgabe 

```POST: /task_comments``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Bereitstellen| |Ja|Textkörper|keine|Neue Aufgabenkommentar erstellt werden|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|201|Erstellt|


## <a name="retrievereminders"></a>RetrieveReminders
Abrufen von Erinnerungen: Abrufen von Erinnerungen für einer bestimmten Liste oder einen bestimmten Vorgang. 

```GET: /reminders``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|TASK_ID|ganze Zahl|Nein|Abfrage|keine|Vorgangsnummer|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="createreminder"></a>CreateReminder
Festlegen einer Erinnerung: eine Erinnerung festlegen. 

```POST: /reminders``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Bereitstellen| |Ja|Textkörper|keine|Neue Erinnerung erstellt werden|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|Standard|Fehler bei Vorgang.|


## <a name="retrievefiles"></a>RetrieveFiles
Abrufen von Dateien: Abrufen von Dateien, die einer bestimmten Liste oder einen bestimmten Vorgang. 

```GET: /files``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|TASK_ID|ganze Zahl|Nein|Abfrage|keine|Vorgangsnummer|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Vorgang erfolgreich|
|400|Ungültige Anforderung|
|500|Interner Serverfehler. Unbekannter Fehler aufgetreten ist.|
|Standard|Fehler bei Vorgang.|


## <a name="getlist"></a>GetList
Abrufen Liste: Ruft eine bestimmte Liste 

```GET: /lists/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine|Listen-ID|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|


## <a name="deletelist"></a>DeleteList
Löschen: Löscht eine Liste 

```DELETE: /lists/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|ganze Zahl|Ja|Pfad|keine|Listen-ID|
|Überarbeitung|ganze Zahl|Ja|Abfrage|keine|Überarbeitung|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|204|Keine Inhalte|


## <a name="updatelist"></a>UpdateList
Aktualisieren einer Liste: Aktualisieren eine bestimmte Liste 

```PATCH: /lists/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|ganze Zahl|Ja|Pfad|keine|Listen-ID|
|Bereitstellen| |Ja|Textkörper|keine|Listendetails|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|


## <a name="gettask"></a>GetTask
Aufgabe abrufen: Ruft einen bestimmten Vorgang 

```GET: /tasks/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|ID|ganze Zahl|Ja|Pfad|keine|Vorgangsnummer|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|


## <a name="updatetask"></a>UpdateTask
Aktualisieren einer Aufgabe: einen bestimmten Vorgang aktualisiert 

```PATCH: /tasks/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|ID|ganze Zahl|Ja|Pfad|keine|Vorgangsnummer|
|Bereitstellen| |Ja|Textkörper|keine|Aufgabendetails|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|


## <a name="deletetask"></a>DeleteTask
Aufgabe löschen: Löscht einen bestimmten Vorgang 

```DELETE: /tasks/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|list_id|ganze Zahl|Ja|Abfrage|keine|Listen-ID|
|ID|ganze Zahl|Ja|Pfad|keine|Vorgangsnummer|
|Überarbeitung|ganze Zahl|Ja|Abfrage|keine|Überarbeitung|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|204|Keine Inhalte|


## <a name="getsubtask"></a>GetSubTask
Abrufen von Teilvorgangs: Ruft eine bestimmte Teilvorgangs 

```GET: /subtasks/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine|Teilvorgangs-ID|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|


## <a name="updatesubtask"></a>UpdateSubTask
Aktualisieren ein Teilvorgangs: ein bestimmtes Teilvorgangs aktualisiert 

```PATCH: /subtasks/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|ganze Zahl|Ja|Pfad|keine|Teilvorgangs-ID|
|Bereitstellen| |Ja|Textkörper|keine|Teilvorgangs details|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|


## <a name="deletesubtask"></a>DeleteSubTask
Löschen ein Teilvorgangs: Löscht eine bestimmte Teilvorgangs 

```DELETE: /subtasks/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|ganze Zahl|Ja|Pfad|keine|Teilvorgangs-ID|
|Überarbeitung|ganze Zahl|Ja|Abfrage|keine|Überarbeitung|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|204|Keine Inhalte|


## <a name="getnote"></a>GetNote
Abrufen eine Notiz: Abrufen eine bestimmte Notiz 

```GET: /notes/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine|Hinweis-ID|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|


## <a name="updatenote"></a>UpdateNote
Aktualisieren eine Notiz: Aktualisieren eine bestimmte Notiz 

```PATCH: /notes/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|ganze Zahl|Ja|Pfad|keine|Hinweis-ID|
|Bereitstellen| |Ja|Textkörper|keine|Hinweis-details|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|


## <a name="deletenote"></a>DeleteNote
Löschen einer Notiz: eine bestimmte Notiz löschen 

```DELETE: /notes/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|ganze Zahl|Ja|Pfad|keine|Hinweis-ID|
|Überarbeitung|ganze Zahl|Ja|Abfrage|keine|Überarbeitung|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|204|Keine Inhalte|


## <a name="getcomment"></a>GetComment
Erste Aufgabenkommentar: einen bestimmten Vorgang Kommentar abrufen 

```GET: /task_comments/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|Zeichenfolge|Ja|Pfad|keine|Kommentar-ID|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|


## <a name="updatereminder"></a>UpdateReminder
Aktualisieren einer Erinnerung: Aktualisieren einer bestimmte Erinnerung 

```PATCH: /reminders/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|ganze Zahl|Ja|Pfad|keine|Erinnerung-ID|
|Bereitstellen| |Ja|Textkörper|keine|Details der Erinnerung|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|200|Okay|


## <a name="deletereminder"></a>DeleteReminder
Löschen eine Erinnerung: löschen eine bestimmte Erinnerung 

```DELETE: /reminders/{id}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|ID|ganze Zahl|Ja|Pfad|keine|ID des die Erinnerung.|
|Überarbeitung|ganze Zahl|Ja|Abfrage|keine|Überarbeitung|

#### <a name="response"></a>Antwort

|Namen|Beschreibung|
|---|---|
|204|Keine Inhalte|


## <a name="object-definitions"></a>Objektdefinitionen 

### <a name="list"></a>Liste


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|created_at|Zeichenfolge|Nein |
|Titel|Zeichenfolge|Nein |
|list_type|Zeichenfolge|Nein |
|Typ|Zeichenfolge|Nein |
|Überarbeitung|ganze Zahl|Nein |



### <a name="createdlist"></a>CreatedList


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|created_at|Zeichenfolge|Nein |
|Titel|Zeichenfolge|Nein |
|Überarbeitung|ganze Zahl|Nein |
|Typ|Zeichenfolge|Nein |



### <a name="task"></a>Aufgabe


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|assignee_id|ganze Zahl|Nein |
|assigner_id|ganze Zahl|Nein |
|created_at|Zeichenfolge|Nein |
|created_by_id|ganze Zahl|Nein |
|Fälligkeitsdatum|Zeichenfolge|Nein |
|list_id|ganze Zahl|Nein |
|Überarbeitung|ganze Zahl|Nein |
|starred|Boolesch|Nein |
|Titel|Zeichenfolge|Nein |



### <a name="subtask"></a>Teilvorgangs


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|TASK_ID|ganze Zahl|Nein |
|created_at|Zeichenfolge|Nein |
|created_by_id|ganze Zahl|Nein |
|Überarbeitung|Zeichenfolge|Nein |
|Titel|Zeichenfolge|Nein |



### <a name="note"></a>Notiz


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|TASK_ID|ganze Zahl|Nein |
|Inhalt|Zeichenfolge|Nein |
|created_at|Zeichenfolge|Nein |
|updated_at|Zeichenfolge|Nein |
|Überarbeitung|ganze Zahl|Nein |



### <a name="comment"></a>Kommentar


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|TASK_ID|ganze Zahl|Nein |
|Überarbeitung|ganze Zahl|Nein |
|Text|Zeichenfolge|Nein |
|Typ|Zeichenfolge|Nein |
|created_at|Zeichenfolge|Nein |



### <a name="reminder"></a>Erinnerung


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|Datum|Zeichenfolge|Nein |
|TASK_ID|ganze Zahl|Nein |
|Überarbeitung|ganze Zahl|Nein |
|Typ|Zeichenfolge|Nein |
|created_at|Zeichenfolge|Nein |
|updated_at|Zeichenfolge|Nein |



### <a name="createdreminder"></a>CreatedReminder


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|Datum|Zeichenfolge|Nein |
|TASK_ID|ganze Zahl|Nein |
|Überarbeitung|ganze Zahl|Nein |
|created_at|Zeichenfolge|Nein |
|updated_at|Zeichenfolge|Nein |



### <a name="file"></a>Datei


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ID|ganze Zahl|Nein |
|URL|Zeichenfolge|Nein |
|TASK_ID|ganze Zahl|Nein |
|list_id|ganze Zahl|Nein |
|USER_ID|ganze Zahl|Nein |
|Dateiname|Zeichenfolge|Nein |
|content_type|Zeichenfolge|Nein |
|file_size|ganze Zahl|Nein |
|local_created_at|Zeichenfolge|Nein |
|created_at|Zeichenfolge|Nein |
|updated_at|Zeichenfolge|Nein |
|Typ|Zeichenfolge|Nein |
|Überarbeitung|ganze Zahl|Nein |



### <a name="newtask"></a>Neue Aufgabe


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|list_id|ganze Zahl|Ja |
|Titel|Zeichenfolge|Ja |
|assignee_id|ganze Zahl|Nein |
|abgeschlossen|Boolesch|Nein |
|recurrence_type|Zeichenfolge|Nein |
|recurrence_count|ganze Zahl|Nein |
|Fälligkeitsdatum|Zeichenfolge|Nein |
|starred|Boolesch|Nein |



### <a name="newlist"></a>NewList


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Titel|Zeichenfolge|Ja |



### <a name="newsubtask"></a>NewSubtask


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|list_id|ganze Zahl|Ja |
|TASK_ID|ganze Zahl|Ja |
|Titel|Zeichenfolge|Ja |
|abgeschlossen|Boolesch|Nein |



### <a name="newnote"></a>NewNote


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|list_id|ganze Zahl|Ja |
|TASK_ID|ganze Zahl|Ja |
|Inhalt|Zeichenfolge|Ja |



### <a name="newcomment"></a>NeuerKommentar


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|list_id|ganze Zahl|Ja |
|TASK_ID|ganze Zahl|Ja |
|Text|Zeichenfolge|Ja |



### <a name="newreminder"></a>NewReminder


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|list_id|ganze Zahl|Ja |
|TASK_ID|ganze Zahl|Ja |
|Datum|Zeichenfolge|Ja |



### <a name="updatetask"></a>UpdateTask


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Überarbeitung|ganze Zahl|Nein |
|Titel|Zeichenfolge|Nein |
|assignee_id|ganze Zahl|Nein |
|abgeschlossen|Boolesch|Nein |
|recurrence_type|Zeichenfolge|Nein |
|recurrence_count|ganze Zahl|Nein |
|Fälligkeitsdatum|Zeichenfolge|Nein |
|starred|Boolesch|Nein |



### <a name="updatelist"></a>UpdateList


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Überarbeitung|ganze Zahl|Nein |
|Titel|Zeichenfolge|Nein |



### <a name="updatesubtask"></a>UpdateSubtask


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Überarbeitung|ganze Zahl|Nein |
|Titel|Zeichenfolge|Nein |
|abgeschlossen|Boolesch|Nein |



### <a name="updatenote"></a>UpdateNote


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Überarbeitung|ganze Zahl|Nein |
|Inhalt|Zeichenfolge|Nein |



### <a name="updatereminder"></a>UpdateReminder


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Datum|Zeichenfolge|Nein |
|Überarbeitung|ganze Zahl|Nein |


## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)