<properties
    pageTitle="Hinzufügen den Office 365-Benutzer Verbinder in Logik Apps | Microsoft Azure"
    description="Übersicht über Office 365-Benutzer Verbinder mit den Parametern REST-API"
    services=""    
    documentationCenter=""     
    authors="msftman"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office-365-users-connector"></a>Erste Schritte mit der Office 365-Benutzer-Verbinder

Verbinden Sie mit Office 365-Benutzer zum Abrufen von Benutzerprofilen und Suchbenutzer. 

>[AZURE.NOTE] Diese Version des Artikels gilt Logik apps 2015-08-01-Schema Vorschauversion aus.

Mit Office 365-Benutzer können Sie folgende Aktionen ausführen:

- Erstellen Sie Ihr Unternehmen Fluss basierend auf den Daten, die Sie von Office 365-Benutzer erhalten. 
- Verwenden Sie Aktionen, die direkt unterstellt, erhalten Abrufen eines Vorgesetzten Benutzerprofil und vieles mehr. Diese Aktionen erhalten eine Antwort, und nehmen Sie die Ausgabe dann für andere Aktionen verfügbar. Angenommen, erhalten Sie eine Person direkt unterstellt, und klicken Sie dann entnehmen diese Informationen und Aktualisieren einer SQL Azure-Datenbank. 

Um einen Vorgang in Logik apps hinzuzufügen, finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen

Der Benutzer von Office 365-Verbinder weist die folgenden Aktionen verfügbar. Es gibt keine Trigger aus.

| Trigger | Aktionen|
| --- | --- |
|Keine | <ul><li>Einbringen manager</li><li>Mein Profil abrufen</li><li>Abrufen von direkten Vorgesetzten</li><li>Abrufen von Benutzerprofilen</li><li>Suchen nach Benutzer</li></ul>|

Alle Connectors unterstützen die Daten in JSON und XML-Formaten. 


## <a name="create-a-connection-to-office-365-users"></a>Herstellen einer Verbindung mit Office 365-Benutzer

Wenn Sie Ihre apps Logik dieser Verbinder hinzugefügt haben, müssen mit Ihrem Office 365-Benutzer-Konto anmelden und Logik apps für die Verbindung zu Ihrem Konto zulassen.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]

Nachdem Sie die Verbindung zu erstellen, geben Sie die Eigenschaften von Office 365-Benutzer, wie die Benutzer-ID Die **REST-API-Referenz** in diesem Thema werden diese Eigenschaften beschrieben.

>[AZURE.TIP] Sie können diese dieselbe Verbindung mit Office 365-Benutzer in andere apps Logik verwenden.


## <a name="office-365-users-rest-api-reference"></a>Office 365-Benutzer-REST-API-Referenz
Version gilt: 1.0.

### <a name="get-my-profile"></a>Mein Profil abrufen 
Ruft das Profil für den aktuellen Benutzer ab.  
```GET: /users/me``` 

Es gibt keine Parameter für diesen Anruf.

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


### <a name="get-user-profile"></a>Abrufen von Benutzerprofilen 
Ruft ein bestimmtes Benutzerprofil an.  
```GET: /users/{userId}``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Benutzer-ID|Zeichenfolge|Ja|Pfad|keine|Benutzer Hauptbenutzer Namen oder e-Mail-id|

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


### <a name="get-manager"></a>Einbringen manager 
Ruft Benutzerprofil der-Manager für den angegebenen Benutzer ab.  
```GET: /users/{userId}/manager``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Benutzer-ID|Zeichenfolge|Ja|Pfad|keine|Benutzer Hauptbenutzer Namen oder e-Mail-id|

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



### <a name="get-direct-reports"></a>Abrufen von direkten Vorgesetzten 
Abrufen von direkten Vorgesetzten.  
```GET: /users/{userId}/directReports``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|Benutzer-ID|Zeichenfolge|Ja|Pfad|keine|Benutzer Hauptbenutzer Namen oder e-Mail-id|

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



### <a name="search-for-users"></a>Suchen nach Benutzer 
Ruft Suchergebnisse von Benutzerprofilen.  
```GET: /users``` 

| Namen| Datentyp|Erforderlich|Befindet sich im|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|searchTerm|Zeichenfolge|Nein|Abfrage|keine|Suchen Sie die Zeichenfolge (gilt für: Name, Vorname, Nachname, e-Mail, Mail Spitzname und Benutzer Benutzerprinzipalnamen anzeigen)|

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



## <a name="object-definitions"></a>Objektdefinitionen

#### <a name="user-user-model-class"></a>Benutzer: Benutzer Modellklasse

|Eigenschaftsname | Datentyp |Erforderlich
|---|---|---|
|DisplayName|Zeichenfolge|Nein|
|Vorname|Zeichenfolge|Nein|
|Nachname|Zeichenfolge|Nein|
|E-Mail-Nachrichten|Zeichenfolge|Nein|
|MailNickname|Zeichenfolge|Nein|
|TelephoneNumber|Zeichenfolge|Nein|
|AccountEnabled|Boolesch|Nein|
|ID|Zeichenfolge|Ja
|UserPrincipalName|Zeichenfolge|Nein|
|Abteilung|Zeichenfolge|Nein|
|Position|Zeichenfolge|Nein|
|Kontaktdetails|Zeichenfolge|Nein|


## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

Kehren Sie zu der [Liste der APIs](apis-list.md).

<!--References-->
[5]: https://portal.azure.com
[7]: ./media/connectors-create-api-office365-users/aad-tenant-applications.PNG
[8]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-appinfo.PNG
[9]: ./media/connectors-create-api-office365-users/aad-tenant-applications-add-app-properties.PNG
[10]: ./media/connectors-create-api-office365-users/contoso-aad-app.PNG
[11]: ./media/connectors-create-api-office365-users/contoso-aad-app-configure.PNG
