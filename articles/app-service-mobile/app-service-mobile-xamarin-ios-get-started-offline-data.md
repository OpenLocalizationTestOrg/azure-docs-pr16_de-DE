<properties
    pageTitle="Aktivieren Sie offlinesynchronisierung für Ihre Azure Mobile-App (iOS Xamarin)"
    description="Informationen Sie zum Verwenden des App-Dienst Mobile-App mit Cache und synchronisieren offline Daten in Ihrer Xamarin iOS-Anwendung"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinios-mobile-app"></a>Aktivieren Sie die offlinesynchronisierung für Ihre Xamarin.iOS mobile-app

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>(Übersicht)

In diesem Lernprogramm wird das Feature offlinesynchronisierung Azure Mobile-Apps für Xamarin.iOS eingeführt. Offlinesynchronisierung ermöglicht es Endbenutzern zu interagieren mit einer mobile-app – anzeigen, hinzufügen oder Ändern von Daten – selbst wenn keine Verbindung zum Netzwerk vorhanden ist. Änderungen werden in eine lokale Datenbank gespeichert. Nachdem Sie das Gerät wieder online ist, werden diese Änderungen mit den Remotedienst synchronisiert.

Aktualisieren Sie in diesem Lernprogramm Xamarin.iOS-app-Projekt aus [Erstellen einer Xamarin iOS-app] , um die offline Features von Azure Mobile-Apps unterstützen. Wenn Sie die heruntergeladene Schnellstart Server Project nicht verwenden, müssen Sie die Daten Access Erweiterung Pakete zum Projekt hinzufügen. Weitere Informationen zu Server Erweiterung Paketen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Mobile-Apps Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Erfahren Sie mehr über das Feature zur Synchronisierung offline, finden Sie unter [Offline Synchronisieren von Daten in Azure Mobile-Apps].

## <a name="update-the-client-app-to-support-offline-features"></a>Aktualisieren der app Client offline Features unterstützt.

Azure offline Mobile-App-Features ermöglichen es Ihnen, mit einer lokalen Datenbank zu interagieren, wenn Sie in einem Szenario offline sind. Zum Verwenden dieser Features in Ihrer app, die Initialisierung einer [SyncContext] zu einem lokalen Speicher aus. Verweisen auf die Tabelle über die Benutzeroberfläche [IMobileServiceSyncTable]. SQLite wird als lokaler Speicher auf dem Gerät verwendet.

1. Öffnen Sie den NuGet-Paket-Manager im Projekt, das Sie in das [Erstellen einer Xamarin iOS-app] -Lernprogramm abgeschlossen und klicken Sie dann suchen Sie, und installieren Sie das Paket **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet.

2. Öffnen Sie die Datei QSTodoService.cs, und entfernen Sie die Kommentarzeichen der `#define OFFLINE_SYNC_ENABLED` Definition.

3. Neu zu erstellen Sie, und führen Sie die app Client. Die app funktioniert genauso wie vor dem Sie offline synchronisieren aktiviert. Die lokale Datenbank ist jedoch jetzt mit Daten aufgefüllt, die in einem Szenario offline verwendet werden können.

## <a name="a-nameupdate-syncaupdate-the-app-to-disconnect-from-the-backend"></a><a name="update-sync"></a>Aktualisieren Sie die app von der Back-End-trennen

In diesem Abschnitt heben Sie die Verbindung in Ihre Mobile-App Back-End-um eine offline Situation zu reproduzieren. Wenn Sie Datenelemente hinzufügen, wird Ihre Ereignishandler Ausnahme, dass die app im Offlinemodus befindet. In diesem Zustand neue Elemente im lokalen Speicher hinzugefügt und wenn Sie Pushbenachrichtigungen in einem verbundenen Zustand weiter ausgeführt wird in der mobilen app Back-End-synchronisiert.

1. Bearbeiten Sie QSToDoService.cs in das freigegebene Projekt ein. Ändern der **ApplicationURL** verweisen auf einen ungültigen URL an:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Sie können auch offline Verhalten vorführen, durch Deaktivieren von Wifi und Mobile Netzwerke auf dem Gerät oder Flugzeug-Modus verwenden.

2. Erstellen Sie und führen Sie die app. Beachten Sie Ihre synchronisieren, klicken Sie auf Aktualisierung fehlgeschlagen ist, wenn die app gestartet wird.

3. Geben Sie die neue Elemente, und beachten Sie, dass Pushbenachrichtigungen mit dem Status "[CancelledByNetworkError]" jedes Mal schlägt Sie klicken Sie auf **Speichern**. Es gibt jedoch die neue erledigen Elemente im lokalen Speicher, bis sie zu der mobilen app Back-End-abgelegt werden können.  In einer app Herstellung Sie diese Ausnahmen unterdrücken verhält sich die app Client wie bei sie noch in der mobilen app Back-End-verbunden ist.

4. Schließen Sie die app, und starten Sie erneut darauf, um zu überprüfen, ob der neue Elemente, die Sie erstellt haben, auf den lokalen Speicher beibehalten werden.

5. (Optional) Wenn Sie Visual Studio auf einem PC installiert haben, öffnen Sie **Server-Explorer**. Navigieren Sie zu Ihrer Datenbank in **Azure**-> **SQL-Datenbanken**. Mit der rechten Maustaste in der Datenbank, und wählen Sie **in der SQL Server-Objekt-Explorer geöffnet**. Jetzt können Sie die SQL-Datenbank-Tabelle und deren Inhalt durchsuchen. Stellen Sie sicher, dass die Daten in die Back-End-Datenbank nicht geändert hat.

6. (Optional) Verwenden ein REST-Tools, wie etwa Fiddler oder Postman zum Abfragen von Ihrem mobilen Back-End-, mithilfe einer GET-Abfrage in das Formular `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="a-nameupdate-online-appaupdate-the-app-to-reconnect-your-mobile-app-backend"></a><a name="update-online-app"></a>Aktualisieren Sie die app, um Ihre Mobile-App Back-End-erneut verbinden

Schließen Sie in diesem Abschnitt die app in der mobilen app Back-End-wieder. Dadurch wird die app Umstieg von Offlinemodus Onlinestatus mit der mobilen app Back-End-simuliert.   Wenn Sie zum Netzwerk Bruch Netzwerkkonnektivität Deaktivieren eines simulierten, werden keine Änderungen Code erforderlich.
Aktivieren Sie das Netzwerk erneut ein.  Beim Ausführen der Anwendungs, die `RefreshDataAsync` wird aufgerufen. Dies wiederum ruft `SyncAsync` Ihrer lokalen Speicher mit der Back-End-Datenbank zu synchronisieren.

1. Öffnen Sie QSToDoService.cs in das freigegebene Projekt, und Wiederherstellen Sie Ihrer Änderung der Eigenschaft **ApplicationURL** .

2. Neu zu erstellen Sie, und führen Sie die app. Die app synchronisiert die lokalen Änderungen mit Azure Mobile-App Back-End-mithilfe von Pushbenachrichtigungen fest, und Ziehen bei der `OnRefreshItemsSelected` Methode ausgeführt wird.

3. (Optional) Anzeigen der aktualisierten Daten mithilfe eines Tools REST wie Fiddler oder SQL Server-Objekt-Explorer. Beachten Sie die Daten wurde zwischen der Azure Mobile-App Back-End-Datenbank und der lokalen Speicher synchronisiert.

4. Klicken Sie in der app auf das Kontrollkästchen neben dem einige Elemente können in den lokalen Speicher ausführen.

  `CompleteItemAsync`Anrufe `SyncAsync` zum Synchronisieren jedes abgeschlossen Element mit der Mobile-App Back-End. `SyncAsync`Ruft beiden Pushbenachrichtigungen, und ziehen.
  **Immer, wenn Sie einen Abruf für eine Tabelle ausführen, die der Client Änderungen vorgenommen hat eine Pushbenachrichtigungen auf dem Client synchronisieren Kontext wird immer zuerst automatisch ausgeführt**. Die implizit Pushbenachrichtigungen wird sichergestellt, dass alle Tabellen im lokalen Speicher zusammen mit Beziehungen konsistent bleiben. Weitere Informationen zu diesem Verhalten finden Sie unter [Offline Daten synchronisieren in Azure Mobile-Apps].

## <a name="review-the-client-sync-code"></a>Überprüfen Sie den Code der Client synchronisieren

Das Xamarin Client-Projekt, das Sie heruntergeladen, wenn Sie das Lernprogramm [Erstellen einer Xamarin iOS-app] bereits abgeschlossen enthält Code mit einer lokalen SQLite Datenbank offlinesynchronisierung unterstützen. Hier ist eine kurze Übersicht darüber, wie bereits in der Code des Lernprogramms enthalten ist. Eine konzeptionelle Übersicht über das Feature finden Sie unter [Offline Daten synchronisieren in Azure Mobile-Apps].

* Damit alle Tabellenvorgänge ausgeführt werden können, muss der lokale Speicher Initialisierung. Ist der Initialisierung der lokalen Speicher Datenbank beim `QSTodoListViewController.ViewDidLoad()` führt `QSTodoService.InitializeStoreAsync()`. Diese Methode erstellt eine neue lokale SQLite Datenbank mit der `MobileServiceSQLiteStore` vom Azure Mobile-App Client SDK bereitgestellten Class.

    Die `DefineTable` Methode erstellt eine Tabelle in den lokalen Speicher, die die Felder in den bereitgestellten Typ, entspricht `ToDoItem` in diesem Fall. Der Typ verfügt nicht über alle Spalten aufnehmen möchten, die in der Remotedatenbank sind. Es ist möglich, nur eine Teilmenge von Spalten zu speichern.

        // QSTodoService.cs

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }


* Die `todoTable` Mitglied `QSTodoService` ist von der `IMobileServiceSyncTable` anstelle des `IMobileServiceTable`. Die IMobileServiceSyncTable weist alle erstellen, lesen, aktualisieren und löschen die Tabellenvorgänge in die lokale Datenbank.

    Sie entscheiden, wann diese Änderungen von Aufrufen auf der Azure Mobile-App Back-End-gelegt werden `IMobileServiceSyncContext.PushAsync()`. Im Kontext synchronisieren hilft beibehalten tabellenbeziehungen durch das verfolgen, und drücken Sie die Änderungen in allen Tabellen, die eine app Client, wann geändert wurde `PushAsync` aufgerufen wird.

    Anrufe bereitgestellten Code `QSTodoService.SyncAsync()` zu synchronisieren, wenn die Liste Todoitem aktualisiert wird oder einer Todoitem hinzugefügt oder abgeschlossen ist. Die app synchronisiert nach jeder lokalen ändern. Wenn Sie ein Abruf für eine Tabelle, die ausstehende lokale Updates erfasst hat nach dem Kontext ausgeführt wird, wird dieser Vorgang Abruf einer Kontext Pushbenachrichtigungen automatisch zuerst auslösen.

    Alle Einträge in der Fernbedienung in den bereitgestellten Code, `TodoItem` Tabelle abgefragt werden, aber es kann auch zum Filtern von Datensätzen durch Senden einer Abfrage-Id und Abfragen zu `PushAsync`. Weitere Informationen finden Sie im Abschnitt *Inkrementell synchronisieren* in [Offline Daten synchronisieren in Azure Mobile-Apps].

        // QSTodoService.cs
        public async Task SyncAsync()
        {
            try
            {
                await client.SyncContext.PushAsync();
                await todoTable.PullAsync("allTodoItems", todoTable.CreateQuery()); // query ID is used for incremental sync
            }

            catch (MobileServiceInvalidOperationException e)
            {
                Console.Error.WriteLine(@"Sync Failed: {0}", e.Message);
            }
        }


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Offline-Daten synchronisieren in Azure Mobile-Apps]
* [So wird's gemacht Azure mobilen Apps .NET SDK][8]

<!-- Images -->

<!-- URLs. -->
[Erstellen Sie eine Xamarin iOS-app]: app-service-mobile-xamarin-ios-get-started.md
[Offline-Daten synchronisieren in Azure Mobile-Apps]: app-service-mobile-offline-data-sync.md
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md