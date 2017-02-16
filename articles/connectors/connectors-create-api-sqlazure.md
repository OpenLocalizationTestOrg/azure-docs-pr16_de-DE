<properties
    pageTitle="Hinzufügen den Verbinder Azure SQL-Datenbank in Ihrer Apps Logik | Microsoft Azure"
    description="Übersicht über Azure SQL-Datenbank Verbinder mit den Parametern REST-API"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="anneta"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="10/18/2016"
   ms.author="mandia"/>


# <a name="get-started-with-the-azure-sql-database-connector"></a>Erste Schritte mit der Verbinder Azure SQL-Datenbank
Erstellen Sie mit der Verbinder Azure SQL-Datenbank Workflows für Ihre Organisation, die Daten in Tabellen zu verwalten. 

Mit SQL-Datenbank Sie:

- Erstellen Sie den Workflow nach dem Hinzufügen eines neuen Kunden auf eine Datenbank Kunden oder Aktualisieren eines Auftrags in einer Datenbank Bestellungen aus.
- Verwenden von Aktionen zum Abrufen einer Zeile mit Daten, eine neue Zeile einfügen, und sogar löschen. Beispielsweise, wenn ein Datensatz in Dynamics CRM Online (eines Triggers) erstellt wird, klicken Sie dann Einfügen einer Zeile in einer SQL Azure-Datenbank (eine Aktion). 

In diesem Thema wird gezeigt, wie den SQL-Datenbank Verbinder in einer app Logik verwenden, und Sie werden auch die Aktionen aufgelistet.

>[AZURE.NOTE] Diese Version des Artikels gilt Logik Apps allgemeine Verfügbarkeit (GA) aus. 

Weitere Informationen zu Logik Apps finden Sie unter [Was sind die Logik apps](../app-service-logic/app-service-logic-what-are-logic-apps.md) , und [Erstellen Sie eine app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-azure-sql-database"></a>Verbinden Sie mit SQL Azure-Datenbank

Bevor Sie Ihre app Logik Dienste zugreifen kann, erstellen Sie zuerst eine *Verbindung* mit dem Dienst an. Eine Verbindung stellt eine Verbindung zwischen einer app Logik und einem anderen Dienst. Beispielsweise erstellen zum Verbinden mit SQL-Datenbank Sie zuerst eine SQL-Datenbank- *Verbindung*. Um eine Verbindung herzustellen, geben Sie die Anmeldeinformationen, die Sie normalerweise verwenden, um den Zugriff auf den Dienst, den, dem Sie eine Verbindung herstellen. Ja, SQL-Datenbank, geben Sie Ihre Anmeldeinformationen SQL-Datenbank, um die Verbindung zu erstellen. 

#### <a name="create-the-connection"></a>Herstellen der Verbindungs

>[AZURE.INCLUDE [Create the connection to SQL Azure](../../includes/connectors-create-api-sqlazure.md)]

## <a name="use-a-trigger"></a>Verwenden eines Triggers

Alle Trigger keinen dieser Verbinder. Verwenden Sie andere Trigger So starten Sie die app Logik einer Serie auslösen, eine HTTP-Webhook auslösen, Trigger mit anderen Verbindern und vieles mehr zur Verfügung. [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md) enthält ein Beispiel.

## <a name="use-an-action"></a>Verwenden Sie eine Aktion
    
Eine Aktion ist ein Vorgang durchgeführten durch den Workflow in einer app Logik definiert. [Erfahren Sie mehr über Aktionen](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Wählen Sie das Pluszeichen (+) aus. Die Reihe von Optionen angezeigt: **Hinzufügen einer Aktion**, **Hinzufügen einer Bedingung**oder eine der **Weitere** Optionen.

    ![](./media/connectors-create-api-sqlazure/add-action.png)

2. Wählen Sie **eine Aktion hinzufügen**.

3. Geben Sie "Sql" in das Textfeld um eine Liste aller verfügbaren Aktionen zu erhalten.

    ![](./media/connectors-create-api-sqlazure/sql-1.png) 

4. Wählen Sie in diesem Beispiel **SQL Server - erste Zeile**ein. Wenn Sie bereits eine Verbindung besteht, dann wählen Sie den **Tabellennamen** in der Dropdown-Liste aus, und geben Sie die **Zeilen-ID** , die Sie zurückgeben möchten.

    ![](./media/connectors-create-api-sqlazure/sample-table.png)

    Wenn Sie für die Verbindungsinformationen aufgefordert werden, geben Sie dann die Details, um die Verbindung zu erstellen. [Erstellen Sie die Verbindung](connectors-create-api-sqlazure.md#create-the-connection) in diesem Thema werden diese Eigenschaften beschrieben. 

    > [AZURE.NOTE] In diesem Beispiel geben wir eine Zeile aus einer Tabelle zurück. Wenn die Daten in dieser Zeile sehen zu können, fügen Sie eine andere Aktion, die eine Datei unter Verwendung der Felder aus der Tabelle erstellt wird. Angenommen, fügen Sie eine OneDrive-Aktion hinzu, die die Felder FirstName und LastName wird verwendet, um das Erstellen einer neuen Datei in der Cloud-Speicher-Konto. 

5. **Speichern** der Änderungen (oberen linken Ecke der Symbolleiste). Ihre app Logik wird gespeichert und automatisch aktiviert werden kann.


## <a name="technical-details"></a>Technische Details

## <a name="sql-database-actions"></a>SQL-Datenbank-Aktionen
Eine Aktion ist ein Vorgang durchgeführten durch den Workflow in einer app Logik definiert. Der Verbinder SQL-Datenbank umfasst die folgenden Aktionen aus. 

|Aktion|Beschreibung|
|--- | ---|
|[ExecuteProcedure](connectors-create-api-sqlazure.md#execute-stored-procedure)|Führt eine gespeicherte Prozedur in SQL|
|[[GetRow](connectors-create-api-sqlazure.md#get-row)|Ruft eine einzelne Zeile aus einer SQL-Tabelle|
|[GetRows](connectors-create-api-sqlazure.md#get-rows)|Ruft Zeilen aus einer SQL-Tabelle|
|[InsertRow](connectors-create-api-sqlazure.md#insert-row)|Fügt eine neue Zeile in einer SQL-Tabelle|
|[DeleteRow](connectors-create-api-sqlazure.md#delete-row)|Löscht eine Zeile aus einer SQL-Tabelle|
|[GetTables](connectors-create-api-sqlazure.md#get-tables)|Ruft Tabellen aus SQL-Datenbank|
|[UpdateRow](connectors-create-api-sqlazure.md#update-row)|So aktualisiert, dass eine vorhandene Zeile in einer SQL-Tabelle|

### <a name="action-details"></a>Aktionsdetails

In diesem Abschnitt finden Sie unter der bestimmte Details zu jeder Aktion, einschließlich alle erforderlichen oder optionalen von Eigenschaften und eine entsprechende Ausgabe der Verbinder zugeordnet.


#### <a name="execute-stored-procedure"></a>Ausführen einer gespeicherten Prozedur
Führt eine gespeicherte Prozedur in SQL.  

| Eigenschaftsname| Anzeigename |Beschreibung|
| ---|---|---|
|Verfahren * | Name der Prozedur | Den Namen der gespeicherten Prozedur, die Sie ausführen möchten. |
|Parameter * | Eingabeparameter | Die Parameter sind dynamisch und basierend auf der gespeicherten Prozedur ausgewählt haben. <br/><br/> Wenn Sie die Adventure Works-Beispieldatenbank verwenden, wählen Sie beispielsweise die *UfnGetCustomerInformation* gespeicherte Prozedur. Der Eingabeparameter für **Kunden-ID** wird angezeigt. Geben Sie "6" oder eine der anderen Kunden-ID an. |

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Die Ausgabedetails
ProcedureResult: Führt Ergebnis der Ausführung einer gespeicherten Prozedur

| Eigenschaftsname | Datentyp | Beschreibung |
|---|---|---|
|OutputParameters|Objekt|Die Ausgabeparameterwerte |
|Rückgabecode|ganze Zahl|Code einer Prozedur zurück |
|ResultSets|Objekt| Ergebnismengen|


#### <a name="get-row"></a>Erste Zeile 
Ruft eine einzelne Zeile aus einer SQL-Tabelle.  

| Eigenschaftsname| Anzeigename |Beschreibung|
| ---|---|---|
|Tabelle * | Tabellenname |Namen der SQL-Tabelle|
|ID * | Zeilen-id |Eindeutiger Bezeichner der Zeile zum Abrufen von|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Die Ausgabedetails
Element

| Eigenschaftsname | Datentyp |
|---|---|
|ItemInternalId|Zeichenfolge|


#### <a name="get-rows"></a>Abrufen von Zeilen 
Ruft Zeilen aus einer SQL-Tabelle ab.  

|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Tabelle *|Tabellenname|Namen der SQL-Tabelle|
|$skip|Überspringen zählen|Anzahl von Einträgen, überspringen (Standard = 0)|
|$top|Abrufen der maximalen Anzahl|Maximale Anzahl von Einträgen zum Abrufen (Standard = 256)|
|$filter|Filtern der Abfrage|Die Abfrage ein ODATA Filter zum Einschränken der Anzahl von Einträgen|
|$orderby|Sortiert nach|Eine ODATA OrderBy Abfrage zum Festlegen der Reihenfolge von Einträgen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Die Ausgabedetails
ItemsList

| Eigenschaftsname | Datentyp |
|---|---|
|Wert|Matrix|


#### <a name="insert-row"></a>Zeile einfügen 
Fügt eine neue Zeile in einer SQL-Tabelle ein.  

|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Tabelle *|Tabellenname|Namen der SQL-Tabelle|
|Element *|Zeile|Zeile in der angegebenen Tabelle in SQL einfügen|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Die Ausgabedetails
Element

| Eigenschaftsname | Datentyp |
|---|---|
|ItemInternalId|Zeichenfolge|


#### <a name="delete-row"></a>Zeile löschen 
Löscht eine Zeile aus einer SQL-Tabelle.  

|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Tabelle *|Tabellenname|Namen der SQL-Tabelle|
|ID *|Zeilen-id|Eindeutiger Bezeichner des zu löschenden Zeile|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Die Ausgabedetails
Keine.

#### <a name="get-tables"></a>Abrufen von Tabellen 
Ruft Tabellen aus SQL-Datenbank.  

Es gibt keine Parameter für diesen Anruf. 

##### <a name="output-details"></a>Die Ausgabedetails 
TablesList

| Eigenschaftsname | Datentyp |
|---|---|
|Wert|Matrix|

#### <a name="update-row"></a>Zeile aktualisieren 
So aktualisiert, dass eine vorhandene Zeile in einer SQL-Tabelle.  

|Eigenschaftsname| Anzeigename|Beschreibung|
| ---|---|---|
|Tabelle *|Tabellenname|Namen der SQL-Tabelle|
|ID *|Zeilen-id|Eindeutiger Bezeichner für die zu aktualisierende Zeile|
|Element *|Zeile|Zeile mit aktualisierten Werte|

Ein Sternchen (*) bedeutet, dass die Eigenschaft erforderlich ist.

##### <a name="output-details"></a>Die Ausgabedetails  
Element

| Eigenschaftsname | Datentyp |
|---|---|
|ItemInternalId|Zeichenfolge|


### <a name="http-responses"></a>HTTP-Antworten

Wenn die anderen Aktionen aufrufen, möglicherweise bestimmte Antworten erhalten haben. In der folgenden Tabelle werden die Antworten und deren Beschreibung an:  

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


## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md). Untersuchen der verfügbaren Connectors in Logik Apps auf unserer [APIs Liste](apis-list.md)an.
