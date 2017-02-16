<properties
    pageTitle="Aktivieren Sie für Ihre Azure Mobile-App (Cordova) offlinesynchronisierung | Microsoft Azure"
    description="Informationen Sie zum Verwenden des App-Dienst Mobile-App mit Cache und synchronisieren offline Daten in Ihrer Anwendung Cordova"
    documentationCenter="cordova"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-cordova-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Aktivieren Sie die offlinesynchronisierung für Ihre Cordova mobile-app

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

In diesem Lernprogramm wird das Feature offlinesynchronisierung Azure Mobile-Apps für Cordova eingeführt. Offlinesynchronisierung ermöglicht es den Endbenutzern Interaktion mit einem mobilen app&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;auch, wenn keine Verbindung zum Netzwerk vorhanden ist. Änderungen werden in eine lokale Datenbank gespeichert. Nachdem Sie das Gerät wieder online ist, werden diese Änderungen mit den Remotedienst synchronisiert.

In diesem Lernprogramm basiert auf der Cordova Schnellstart-Lösung für Mobile-Apps, die Sie erstellen, wenn Sie das Lernprogramm [Starten Apache Cordova Symbolleiste]abschließen. In diesem Lernprogramm aktualisieren Sie die Lösung Schnellstart offline Features von Azure Mobile-Apps hinzufügen. Wir werden auch den offline-spezifischen Code in der app hervorheben.

Erfahren Sie mehr über das Feature zur Synchronisierung offline, finden Sie unter [Offline Synchronisieren von Daten in Azure Mobile-Apps]. Details der API Verwendung finden Sie unter [der Infodatei in das Plug-in] .

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Hinzufügen von offlinesynchronisierung zur Lösung Schnellstart

Der offlinesynchronisierung Code muss bei der app hinzugefügt werden. Offlinesynchronisierung erfordert die Cordova-Sqlite-Speicher-Plug-Ins, die zu Ihrer Anwendung automatisch hinzugefügt wird, wenn das Plug-in Azure Mobile-Apps im Projekt enthalten ist. Schnellstart Projekt umfasst beide der folgenden-Plug-Ins.

1. Klicken Sie im Visual Studio-Lösung Explorer öffnen Sie index.js, und Ersetzen Sie den folgenden code

        var client,            // Connection to the Azure Mobile App backend
          todoItemTable;      // Reference to a table endpoint on backend

    mit diesem Code:

        var client,             // Connection to the Azure Mobile App backend
          todoItemTable, syncContext; // Reference to table and sync context

2. Als Nächstes ersetzen Sie den folgenden Code ein:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    mit diesem Code:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

        // Note: Requires at least version 2.0.0-beta6 of the Azure Mobile Apps plugin
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              deleted: 'boolean',
              complete: 'boolean'
          }
        });

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    Die vorstehende Code Ergänzungen Initialisierung den lokalen Speicher und definieren eine lokale Tabelle, die die Spaltenwerte verwendet entspricht in Ihrer Azure back-End. (Sie müssen nicht alle Spaltenwerte in dieser Code aufnehmen möchten.)

    Sie erhalten einen Verweis auf den Kontext synchronisieren, indem Sie **GetSyncContext**aus. Im Kontext synchronisieren hilft beibehalten tabellenbeziehungen durch das verfolgen, und drücken Sie die Änderungen in allen Tabellen, die eine app Client geändert hat, wenn Sie **Pushbenachrichtigungen** aufgerufen wird.

3. Aktualisieren Sie die URL Ihrer Anwendung Mobile-App-URL ein.

4. Als Nächstes ersetzen Sie diesen Code ein:

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    mit diesem Code:

        // todoItemTable = client.getTable('todoitem');

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (serverRecord, clientRecord, pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    Der vorherige Code initialisiert im Kontext synchronisieren und ruft dann GetSyncTable (statt getTable), um einen Verweis auf die lokale Tabelle erhalten.

    In diesem Code wird verwendet, die die lokale Datenbank für alle erstellen, lesen, aktualisieren und Löschen von Tabellenvorgänge.

    In diesem Beispiel führt einfachen Fehlerbehandlung auf Synchronisierungskonflikte. Eine reale Anwendung würde der verschiedenen Fehler wie Netzwerkprobleme und Server Konflikte behandeln. Codebeispielen finden Sie unter der [Stichprobe offline synchronisieren].

5. Fügen Sie diese Funktion, um die eigentliche Synchronisierung ausführen.

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    Sie entscheiden, wann Änderungen auf die Mobile-App Back-End-Pushbenachrichtigungen durch Aufrufen von **Pushbenachrichtigungen** für das **SyncContext** Objekt vom Client verwendet wird. Beispielsweise könnten Sie einen Anruf an **SyncBackend** einer Schaltflächenereignishandler in der app wie eine neue Schaltfläche "Synchronisieren" hinzufügen, oder Sie könnten hinzufügen den Anruf an die Funktion **AddItemHandler** synchronisieren, sobald ein neues Element hinzugefügt wird.

##<a name="offline-sync-considerations"></a>Offlinesynchronisierung Aspekte

Im Beispiel wird die **Pushbenachrichtigungen** Berechnungsmethode **SyncContext** nur app beim Start in der Rückruffunktion für Login aufgerufen.  In einer realen Anwendung können Sie auch diese ausgelöst wurde manuell synchronisieren-Funktion oder wenn sich der Status des Netzwerks ändert vornehmen.

Wenn Sie ein Abruf für eine Tabelle, die ausstehende lokale Updates erfasst hat nach dem Kontext ausgeführt wird, wird dieser Vorgang Abruf automatisch eine vorherige Kontext Pushbenachrichtigungen auslösen. Beim Aktualisieren, können hinzufügen und Durchführen von Elementen in diesem Beispiel Sie den Anruf explizite **Pushbenachrichtigungen** weggelassen werden, da es redundante (ersten Überprüfung Features des aktuellen Status der [Infos] ) sein kann.

In den bereitgestellten Code werden alle Datensätze in der Tabelle remote TodoItem abgefragt, es kann jedoch auch zum Filtern von Datensätzen, indem Sie eine Abfrage-Id und die Abfrage an **Pushbenachrichtigungen**übergeben. Weitere Informationen finden Sie im Abschnitt *Inkrementell synchronisieren* in [Offline Daten synchronisieren in Azure Mobile-Apps].

## <a name="optional-disable-authentication"></a>(Optional) Deaktivieren Sie die Authentifizierung

Wenn Sie nicht bereits Authentifizierung eingerichtet und möchten nicht Authentifizierung vor dem Testen offlinesynchronisierung eingerichtet haben, die Rückruffunktion für Login kommentieren, aber behalten Sie den Code in nicht kommentiert die Rückruffunktion.

Der Code sollte wie folgt aussehen, nach dem die Anmeldung Linien kommentieren.

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Nun wird die app mit der Azure Back-End-synchronisiert werden beim Ausführen der app statt, wenn Sie sich anmelden.

## <a name="run-the-client-app"></a>Führen Sie die Client-app

Mit offlinesynchronisierung jetzt aktiviert können Sie jetzt die Clientanwendung mindestens einmal auf jeder Plattform die lokale Datenbank gefüllt ausführen. Sie werden später eine offline Szenario simulieren und ändern Sie die Daten im lokalen Speicher, während die app offline ist.

## <a name="optional-test-the-sync-behavior"></a>(Optional) Das Synchronisierungsverhalten testen

In diesem Abschnitt ändern Sie das Clientprojekt für eine offline-Szenario simulieren, indem Sie über einen ungültigen Anwendung-URL für Ihre Back-End. Beim Hinzufügen oder Ändern von Datenelementen, werden diese Änderungen frei, die im lokalen Speicher, aber nicht auf die Back-End-Datenspeicher synchronisiert, bis die Verbindung erneut hergestellt wird.

1. Explorer-Lösung öffnen Sie die index.js Projektdatei, und ändern Sie die URL der Anwendung verweisen auf einen ungültigen URL, wie die folgende:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. In index.html, aktualisieren Sie den Anbieter, den `<meta>` Element mit der gleichen ungültige URL.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Erstellen und Ausführen der Client-app und beachten Sie, dass eine Ausnahme in der Konsole angemeldet ist, wenn die app versucht, nach der Anmeldung mit die Back-End-synchronisieren. Alle neuen Elemente, die Sie hinzufügen werden nur in den lokalen Speicher vorhanden, bis sie zu der mobilen Back-End-abgelegt werden können. Die app Client verhält sich, wie bei der Back-End-Unterstützung, die alle erstellen, lesen, aktualisieren, Löschvorgänge (CRUD) verbunden ist.

4. Schließen Sie die app, und starten Sie erneut darauf, um zu überprüfen, ob der neue Elemente, die Sie erstellt haben, auf den lokalen Speicher beibehalten werden.

5. (Optional) Verwenden Sie Visual Studio zum Anzeigen der Tabelle Azure SQL-Datenbank, um anzuzeigen, dass die Daten in die Back-End-Datenbank nicht geändert hat.

    Öffnen Sie in Visual Studio **Server-Explorer**. Navigieren Sie zu Ihrer Datenbank in **Azure**->**SQL-Datenbanken**. Mit der rechten Maustaste in der Datenbank, und wählen Sie **in der SQL Server-Objekt-Explorer geöffnet**. Jetzt können Sie die SQL-Datenbank-Tabelle und deren Inhalt durchsuchen.


## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(Optional) Testen der eine neue Verbindung mit Ihrem mobilen Back-End-

In diesem Abschnitt schließen Sie die app in der mobilen Back-End-wieder die die app in Kürze wieder Onlinestatus simuliert. Wenn Sie sich anmelden, werden Daten auf Ihrem mobilen Back-End-synchronisiert werden.

1. Öffnen Sie index.js erneut, und korrigieren Sie den URL der Anwendung an die richtige URL verweisen.

2. Öffnen Sie erneut index.html und korrigieren Sie den URL der Anwendung in den Anbieter, den `<meta>` Element.

3. Neu zu erstellen Sie, und führen Sie die app Client. Die app versucht, nach der Anmeldung mit der mobilen app Back-End-synchronisieren. Stellen Sie sicher, dass keine Ausnahmen in der Verwaltungskonsole Debuggen angemeldet sind.

4. (Optional) Anzeigen der aktualisierten Daten mithilfe eines Tools REST wie Fiddler oder SQL Server-Objekt-Explorer. Beachten Sie, dass die Daten zwischen der Back-End-Datenbank und den lokalen Speicher synchronisiert wurde.

    Beachten Sie die Daten zwischen der Datenbank und den lokalen Speicher wurde synchronisiert und enthält die Elemente, die Sie hinzugefügt haben, während die app getrennt wurde.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Offline-Daten synchronisieren in Azure Mobile-Apps]

* [Cloud Deckblatt: Offlinesynchronisierung in Azure Mobile Dienste] \(Hinweis: befindet sich das Video auf Mobile Dienste, aber offlinesynchronisierung funktioniert in ähnlicher Weise in Azure Mobile-Apps\)

* [Visual Studio-Tools für Apache Cordova]

## <a name="next-steps"></a>Nächste Schritte

* Prüfen Sie erweiterte offlinesynchronisierung Features wie Konflikt mit einer Auflösung von in der [Stichprobe offlinesynchronisierung]
* Schauen Sie sich die offlinesynchronisierung-API-Referenz in die [Infos zu]

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Schnellstart Apache Cordova]: app-service-mobile-cordova-get-started.md
[INFODATEI ZU]: https://github.com/Azure/azure-mobile-apps-js-client#offline-data-sync-preview
[Beispiel für offlinesynchronisierung]: https://github.com/shrishrirang/azure-mobile-apps-quickstarts/tree/samples/client/cordova/ZUMOAPPNAME
[Offline-Daten synchronisieren in Azure Mobile-Apps]: app-service-mobile-offline-data-sync.md
[Cloud Deckblatt: Offlinesynchronisierung Azure Mobile-Dienste]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio-Tools für Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
