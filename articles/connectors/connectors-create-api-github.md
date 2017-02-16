<properties
pageTitle="GitHub | Microsoft Azure"
description="Erstellen Sie Logik apps mit Azure-App-Dienst an. GitHub ist ein Repository Git webbasierten Hostingdienst. Es bietet alle verteilten Überarbeitung Steuerelement und Datenquelle Code Management (SCM) Funktionen von Git sowie eine eigene Features hinzufügen."
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

# <a name="get-started-with-the-github-connector"></a>Erste Schritte mit der GitHub Verbinder

GitHub ist ein Repository Git webbasierten Hostingdienst. Es bietet alle verteilten Überarbeitung Steuerelement und Datenquelle Code Management (SCM) Funktionen von Git sowie eine eigene Features hinzufügen.

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus. 

Sie können beginnen, indem Sie eine app Logik jetzt erstellen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

Der Verbinder GitHub kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen die Daten in JSON und XML-Formaten. 

 Der Verbinder GitHub weist die folgenden Aktion(en) und/oder Triggern zur Verfügung:

### <a name="github-actions"></a>GitHub Aktionen
Sie können folgende Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|[CreateIssue](connectors-create-api-github.md#createissue)|Erstellt ein Problem|
### <a name="github-triggers"></a>GitHub Trigger
Sie können diese Ereignisse überwachen:

|Auslösen | Beschreibung|
|--- | ---|
|Wenn ein Problem geöffnet wird|Ein Problem wird geöffnet.|
|Wenn ein Problem geschlossen wird|Ein Problem wird geschlossen.|
|Wenn ein Problem zugewiesen wird|Ein Problem zugewiesen ist|


## <a name="create-a-connection-to-github"></a>Herstellen einer Verbindung mit GitHub
Um Logik apps mit GitHub zu erstellen, müssen Sie zunächst eine **Verbindung** erstellen anschließend geben Sie die Details für die folgenden Eigenschaften: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Geben Sie GitHub-Anmeldeinformationen|
Nachdem Sie die Verbindung zu erstellen, können Sie es verwenden, führen Sie die Aktionen aus, und achten Sie auf die in diesem Artikel beschriebenen Trigger. 

>[AZURE.INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]

>[AZURE.TIP] Sie können diese Verbindung in andere apps Logik verwenden.

## <a name="reference-for-github"></a>Referenz für GitHub
Version gilt: 1.0

## <a name="createissue"></a>CreateIssue
Erstellen Sie ein Problem: erstellt ein Problem 

```POST: /repos/{repositoryOwner}/{repositoryName}/issues``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|repositoryOwner|Zeichenfolge|Ja|Pfad|keine|Repository-Eigentümer|
|: repositoryName|Zeichenfolge|Ja|Pfad|keine|Repository-name|
|issueBasicDetails| |Ja|Textkörper|keine|Problemdetails|

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


## <a name="issueopened"></a>IssueOpened
Wenn ein Problem geöffnet ist: ein Problem wird geöffnet. 

```GET: /trigger/issueOpened``` 

Es sind keine Parameter für diesen Anruf
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


## <a name="issueclosed"></a>IssueClosed
Wenn ein Problem geschlossen ist: ein Problem wird geschlossen. 

```GET: /trigger/issueClosed``` 

Es sind keine Parameter für diesen Anruf
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


## <a name="issueassigned"></a>IssueAssigned
Wenn ein Problem zugewiesen ist: ein Problem zugewiesen ist 

```GET: /trigger/issueAssigned``` 

Es sind keine Parameter für diesen Anruf
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

### <a name="issuebasicdetailsmodel"></a>IssueBasicDetailsModel


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Titel|Zeichenfolge|Ja |
|Textkörper|Zeichenfolge|Ja |
|zugewiesenen|Zeichenfolge|Ja |



### <a name="issuedetailsmodel"></a>IssueDetailsModel


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Titel|Zeichenfolge|Ja |
|Textkörper|Zeichenfolge|Ja |
|zugewiesenen|Zeichenfolge|Ja |
|Zahl|Zeichenfolge|Nein |
|Bundesstaat|Zeichenfolge|Nein |
|created_at|Zeichenfolge|Nein |
|repository_url|Zeichenfolge|Nein |


## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)