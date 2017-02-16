<properties
pageTitle="ProjectOnline | Microsoft Azure"
description="Erstellen Sie Logik apps mit Azure-App-Dienst an. Project Online ist eine flexible Lösung für Project Portfoliomanagement (PPM) und die tägliche Arbeit von Microsoft online. Über Office 365 übermittelt, Project Online es Organisationen ermöglicht, schnell behilflich leistungsfähige Projektmanagement zu planen, priorisieren und Verwalten von Projekten und projektportfolioinvestitionen – von beinahe jedem Standort auf nahezu jedem Gerät."
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

# <a name="get-started-with-the-projectonline-connector"></a>Erste Schritte mit der ProjectOnline-Verbinder

Project Online ist eine flexible Lösung für Project Portfoliomanagement (PPM) und die tägliche Arbeit von Microsoft online. Über Office 365 übermittelt, Project Online es Organisationen ermöglicht, schnell behilflich leistungsfähige Projektmanagement zu planen, priorisieren und Verwalten von Projekten und projektportfolioinvestitionen – von beinahe jedem Standort auf nahezu jedem Gerät.

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus. 

Sie können beginnen, indem Sie eine app Logik jetzt erstellen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

Der Verbinder ProjectOnline kann als eine Aktion verwendet werden. Es hat Triggern. Alle Connectors unterstützen die Daten in JSON und XML-Formaten. 

 Der Verbinder ProjectOnline weist die folgenden Aktion(en) und/oder Triggern zur Verfügung:

### <a name="projectonline-actions"></a>ProjectOnline-Aktionen
Sie können folgende Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|[ListProjects](connectors-create-api-projectonline.md#listprojects)|Listet die Projekte in die Project online-Website|
|[CreateProject](connectors-create-api-projectonline.md#createproject)|Erstellt ein neues Projekt in die Project online-Website|
|[CreateTask](connectors-create-api-projectonline.md#createtask)|Erstellt eine neue Aufgabe im Projekt|
|[CreateResource](connectors-create-api-projectonline.md#createresource)|Erstellt eine Enterprise-Ressourcen in die Project online-Website|
|[ListTasks](connectors-create-api-projectonline.md#listtasks)|Listet die veröffentlichten Vorgängen in einem Projekt|
|[CheckoutProject](connectors-create-api-projectonline.md#checkoutproject)|Auschecken von Projekten auf Ihrer Website|
|[PublishProject](connectors-create-api-projectonline.md#publishproject)|Checken Sie ein, veröffentlichen Sie und vorhandenen Sie Projekt auf Ihrer Website|
### <a name="projectonline-triggers"></a>ProjectOnline Trigger
Sie können diese Ereignisse überwachen:

|Auslösen | Beschreibung|
|--- | ---|
|Wenn ein neues Projekt erstellt wird|Einen Fluss ausgelöst, wenn ein neues Projekt erstellt wird|
|Wenn eine neue Ressource erstellt wird|Einen neuen Fluss ausgelöst wird, wenn eine neue Ressource erstellt wurde|
|Wenn eine neue Aufgabe erstellt wird|Einen Fluss ausgelöst wird, wenn eine neue Aufgabe erstellt wurde|


## <a name="create-a-connection-to-projectonline"></a>Herstellen einer Verbindung mit ProjectOnline
Um Logik apps mit ProjectOnline zu erstellen, müssen Sie zunächst eine **Verbindung** erstellen anschließend geben Sie die Details für die folgenden Eigenschaften: 

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|Token|Ja|Geben Sie ProjectOnline-Anmeldeinformationen|

>[AZURE.INCLUDE [Steps to create a connection to ProjectOnline](../../includes/connectors-create-api-projectonline.md)]

>[AZURE.TIP] Sie können diese Verbindung in andere apps Logik verwenden.

## <a name="reference-for-projectonline"></a>Referenz für ProjectOnline
Version gilt: 1.0

## <a name="onnewproject"></a>OnNewProject
Beim Erstellen eines neuen Projekts: einen Fluss ausgelöst, wenn ein neues Projekt erstellt wird 

```GET: /trigger/_api/ProjectData/Projects``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Stamm Website-Url für Ihre Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="onnewresource"></a>OnNewResource
Beim Erstellen einer neuen Ressource: keinen neuen Datenfluss ausgelöst wird, wenn eine neue Ressource erstellt wurde 

```GET: /trigger/_api/ProjectData/Resources``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Stamm Website-Url für Ihre Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="onnewtask"></a>OnNewTask
Beim Erstellen einer neuen Aufgabe: einen Fluss ausgelöst wird, wenn eine neue Aufgabe erstellt wurde 

```GET: /trigger/_api/ProjectData/Tasks``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Stamm Website-Url für Ihre Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="listprojects"></a>ListProjects
Liste der Projekte: Listet die Projekte in die Project online-Website 

```GET: /_api/ProjectServer/Projects``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Stamm Website-Url für Ihre Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|

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


## <a name="createproject"></a>CreateProject
Neues Projekt erstellt: erstellt ein neues Projekt in die Project online-Website 

```POST: /_api/ProjectServer/Projects``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Stamm Website-Url für Ihre Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|
|proj| |Ja|Textkörper|keine|Neues Projekt erstellen|

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


## <a name="createtask"></a>CreateTask
Neue Aufgabe erstellt: erstellt eine neue Aufgabe im Projekt 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Tasks/Add``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Stamm Website-Url für Ihre Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|Zeichenfolge|Ja|Pfad|keine|Eindeutige ID des Projekts, um den Vorgang hinzufügen|
|Aufgabe| |Ja|Textkörper|keine|Neue Aufgabe zum Projekt hinzufügen|

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


## <a name="createresource"></a>CreateResource
Erstellen Sie neue Ressource: erstellt eine Enterprise-Ressourcen in die Project online-Website 

```POST: /_api/ProjectServer/EnterpriseResources``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Stamm Website-Url für Ihre Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|
|Ressource| |Ja|Textkörper|keine|Neue Enterprise-Ressource zum Projekt hinzufügen|

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


## <a name="listtasks"></a>ListTasks
Führt Aufgaben: die veröffentlichten Aufgaben in einem Projekt 

```GET: /_api/ProjectServer/Projects('{project_id}')/Tasks``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Stamm Website-Url für Ihre Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|Zeichenfolge|Ja|Pfad|keine|Eindeutige ID des Projekts zum Abrufen von Aufgaben|

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


## <a name="checkoutproject"></a>CheckoutProject
Auschecken eines Projekts: Checkt ein Projekt in Ihrer Website 

```POST: /_api/ProjectServer/Projects('{project_id}')/checkOut``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Stamm Website-Url für Ihre Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|Zeichenfolge|Ja|Pfad|keine|Eindeutige ID des Projekts, um den Vorgang hinzufügen|

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


## <a name="publishproject"></a>PublishProject
Einchecken und Veröffentlichen von Project: Einchecken veröffentlichen und vorhandenen Projekt auf Ihrer Website 

```POST: /_api/ProjectServer/Projects('{project_id}')/Draft/Publish(true)``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|siteUrl|Zeichenfolge|Ja|Abfrage|keine|Stamm Website-Url für Ihre Projektwebsite (Beispiel: https://sampletenant.sharepoint.com/teams/sampleteam)|
|PROJECT_ID|Zeichenfolge|Ja|Pfad|keine|Eindeutige ID des Projekts zum Einchecken|

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

### <a name="triggerprojectswrapper"></a>TriggerProjectsWrapper


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Matrix|Nein |



### <a name="triggerproject"></a>TriggerProject


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ProjectStartDate|Zeichenfolge|Nein |
|ProjectFinishDate|Zeichenfolge|Nein |
|ProjectCreatedDate|Zeichenfolge|Nein |
|ProjectId|Zeichenfolge|Nein |
|ProjectModifiedDate|Zeichenfolge|Nein |
|ProjectType|ganze Zahl|Nein |
|Projektname|Zeichenfolge|Nein |



### <a name="triggerresourceswrapper"></a>TriggerResourcesWrapper


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Matrix|Nein |



### <a name="triggerresource"></a>TriggerResource


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ResourceId|Zeichenfolge|Nein |
|ResourceBaseCalendar|Zeichenfolge|Nein |
|ResourceBookingType|ganze Zahl|Nein |
|ResourceCanLevel|Boolesch|Nein |
|ResourceCostPerUse|Zahl|Nein |
|ResourceCreatedDate|Zeichenfolge|Nein |
|ResourceEarliestAvailableFrom|Zeichenfolge|Nein |
|ResourceEmail|Zeichenfolge|Nein |
|ResourceInitials|Zeichenfolge|Nein |
|ResourceIsActive|Boolesch|Nein |
|ResourceIsGeneric|Boolesch|Nein |
|ResourceLatestAvailableTo|Zeichenfolge|Nein |
|ResourceModifiedDate|Zeichenfolge|Nein |
|Ressourcenname|Zeichenfolge|Nein |
|ResourceStatsuName|Zeichenfolge|Nein |
|ResourceType|ganze Zahl|Nein |
|TypeDescription|Zeichenfolge|Nein |
|TypeName|Zeichenfolge|Nein |



### <a name="triggertaskswrapper"></a>TriggerTasksWrapper


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Matrix|Nein |



### <a name="triggertask"></a>TriggerTask


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ProjectId|Zeichenfolge|Nein |
|TaskId|Zeichenfolge|Nein |
|Projektname|Zeichenfolge|Nein |
|TaskName|Zeichenfolge|Nein |
|TaskCreatedDate|Zeichenfolge|Nein |
|TaskModifieddate|Zeichenfolge|Nein |
|TaskStartDate|Zeichenfolge|Nein |
|TaskFinishDate|Zeichenfolge|Nein |
|TaskPriority|ganze Zahl|Nein |
|TaskIsActive|Boolesch|Nein |



### <a name="newproject"></a>Neues Projekt


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Namen|Zeichenfolge|Ja |
|Beschreibung|Zeichenfolge|Nein |
|Starten|Zeichenfolge|Nein |



### <a name="newreource"></a>NewReource


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Namen|Zeichenfolge|Ja |
|IsBudget|Boolesch|Nein |
|IsGeneric|Boolesch|Nein |
|IsInactive|Boolesch|Nein |



### <a name="project"></a>Project


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|ApprovedStart|Zeichenfolge|Nein |
|ApprovedEnd|Zeichenfolge|Nein |
|CheckedOutDate|Zeichenfolge|Nein |
|CheckOutDescription|Zeichenfolge|Nein |
|CheckOutId|Zeichenfolge|Nein |
|"CreatedDate"|Zeichenfolge|Nein |
|ID|Zeichenfolge|Nein |
|IsCheckedOut|Boolesch|Nein |
|LastPublishedDate|Zeichenfolge|Nein |
|LastSavedDate|Zeichenfolge|Nein |
|OptimizerDecision|ganze Zahl|Nein |
|PlannerDecision|ganze Zahl|Nein |
|ProjectType|ganze Zahl|Nein |
|Namen|Zeichenfolge|Nein |
|WinprojVersion|Zeichenfolge|Nein |



### <a name="projectswrapper"></a>ProjectsWrapper


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Matrix|Nein |



### <a name="newtask"></a>Neue Aufgabe


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Parameter|nicht definiert|Ja |



### <a name="taskparameters"></a>TaskParameters


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Namen|Zeichenfolge|Ja |
|Notizen|Zeichenfolge|Nein |
|Starten|Zeichenfolge|Nein |
|Dauer|Zeichenfolge|Nein |



### <a name="enterpriseresource"></a>EnterpriseResource


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|CanLevel|Boolesch|Nein |
|Code|Zeichenfolge|Nein |
|CostAccrual|ganze Zahl|Nein |
|CostCenter|Zeichenfolge|Nein |
|Erstellt|Zeichenfolge|Nein |
|DefaultBookingType|ganze Zahl|Nein |
|E-Mail|Zeichenfolge|Nein |
|ExternalId|Zeichenfolge|Nein |
|Gruppe|Zeichenfolge|Nein |
|HireDate|Zeichenfolge|Nein |
|ID|Zeichenfolge|Nein |
|Initialen|Zeichenfolge|Nein |
|IsActive|Boolesch|Nein |
|IsBudget|Boolesch|Nein |
|IsCheckedOut|Boolesch|Nein |
|IsGeneric|Boolesch|Nein |
|IsTeam|Boolesch|Nein |
|MaterialLabel|Zeichenfolge|Nein |
|Geändert|Zeichenfolge|Nein |
|Namen|Zeichenfolge|Nein |
|Lautschrift|Zeichenfolge|Nein |
|ResourceType|ganze Zahl|Nein |
|TerminationDate|Zeichenfolge|Nein |



### <a name="taskswrapper"></a>TasksWrapper


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Wert|Matrix|Nein |



### <a name="task"></a>Aufgabe


| Eigenschaftsname | Datentyp | Erforderlich |
|---|---|---|
|Erstellt|Zeichenfolge|Nein |
|Geändert|Zeichenfolge|Nein |
|Starten|Zeichenfolge|Nein |
|Fertig stellen|Zeichenfolge|Nein |
|Namen|Zeichenfolge|Nein |
|ID|Zeichenfolge|Nein |
|Priorität|ganze Zahl|Nein |
|PercentComplete|ganze Zahl|Nein |
|Notizen|Zeichenfolge|Nein |
|Kontakt|Zeichenfolge|Nein |


## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)