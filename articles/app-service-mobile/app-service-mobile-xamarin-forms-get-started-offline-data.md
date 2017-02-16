<properties
    pageTitle="Aktivieren Sie für Ihre Azure Mobile-App (Xamarin.Forms) offlinesynchronisierung | Microsoft Azure"
    description="Informationen Sie zum Verwenden des App-Dienst Mobile-App mit Cache und synchronisieren offline Daten in Ihrer Anwendung Xamarin.Forms"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Aktivieren Sie die offlinesynchronisierung für Ihre Xamarin.Forms mobile-app

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>(Übersicht)
In diesem Lernprogramm wird das Feature offlinesynchronisierung Azure Mobile-Apps für Xamarin.Forms eingeführt. Offlinesynchronisierung ermöglicht es Endbenutzern zu interagieren mit einer mobile-app – anzeigen, hinzufügen oder Ändern von Daten – selbst wenn keine Verbindung zum Netzwerk vorhanden ist. Änderungen werden in eine lokale Datenbank gespeichert. Nachdem Sie das Gerät wieder online ist, werden diese Änderungen mit den Remotedienst synchronisiert.

In diesem Lernprogramm basiert auf der Xamarin.Forms Schnellstart-Lösung für Mobile-Apps, die Sie erstellen, wenn Sie das Lernprogramm [Erstellen einer Xamarin iOS-app] abschließen. Die Lösung Schnellstart für Xamarin.Forms enthält den Code zur Unterstützung von offlinesynchronisierung, die nur muss aktiviert sein. In diesem Lernprogramm aktualisieren Sie die Schnellstart-Lösung für die offline Features von Azure Mobile-Apps zu aktivieren. Wir markieren Sie auch den offline-spezifische-Code in der app. Wenn Sie nicht die heruntergeladene Schnellstart-Lösung verwenden, müssen Sie die Daten Access Erweiterung Pakete zum Projekt hinzufügen. Weitere Informationen zu Server Erweiterung Paketen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Mobile-Apps Azure][1].

Erfahren Sie mehr über das Feature zur Synchronisierung offline, finden Sie im Thema [Offline Synchronisieren von Daten in Azure Mobile-Apps][2].

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a>Aktivieren der offlinesynchronisierung den Funktionsumfang Schnellstart

Der offlinesynchronisierung Code ist im Projekt mithilfe von Präprozessordirektiven c# enthalten. Wenn der **OFFLINE\_SYNCHRONISIEREN\_aktiviert** Symbol definiert ist, wird diese Codepfade im Build enthalten sind. Für Windows-apps müssen Sie auch die SQLite Plattform installieren.

1. In Visual Studio, mit der Maustaste der Lösung > **NuGet-Pakete verwalten, für die Lösung...**, und klicken Sie dann suchen und Installieren der **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-Paket für alle Projekte in der Lösung.

2. Kommentieren Sie Öffnen der Datei TodoItemManager.cs aus dem Projekt mit **tragbaren** im Namen, tragbaren Class Library-Projekt befindet, klicken Sie dann die folgenden Präprozessordirektive im Explorer-Lösung:

        #define OFFLINE_SYNC_ENABLED

4. (Optional) Installieren Sie Windows-Geräten unterstützt, eine der folgenden SQLite Laufzeit Pakete aus:

    * **Windows 8.1 Runtime:** Installieren von [SQLite für Windows 8.1][3].
    * **Windows Phone 8.1:** Installieren von [SQLite für Windows Phone 8.1][4].
    * **Universal Windows-Plattform** Installieren [SQLite für die Universal Windows Universal][5].

    Obwohl Schnellstart ein Projekts Universal Windows nicht enthält, wird die Universal Windows-Plattform mit Xamarin Formulare unterstützt.

5. (Optional) Jeder Windows-app-Projekt mit der Maustaste **Verweise** > **Verweis hinzufügen**, erweitern Sie den **Windows** -Ordner > **Erweiterungen**.
    Aktivieren Sie das entsprechende **SQLite für Windows** SDK zusammen mit **Visual C++ 2013 Runtime für Windows** SDK.
    Die Namen SQLite SDK sind ein wenig anders mit jeder Windows-Plattform.

## <a name="review-the-client-sync-code"></a>Überprüfen Sie den Code der Client synchronisieren

Es folgt eine kurze Übersicht darüber, wie bereits in der Code innerhalb des Lernprogramms enthalten ist die `#if OFFLINE_SYNC_ENABLED` Richtlinien. Die offlinesynchronisierung-Funktionalität ist in der Projektdatei TodoItemManager.cs im Projekt tragbares Class-Bibliothek. Eine konzeptionelle Übersicht über das Feature finden Sie unter [Offline Daten synchronisieren in Azure Mobile-Apps][2].

* Damit alle Tabellenvorgänge ausgeführt werden können, muss der lokale Speicher Initialisierung. Die lokale Datenbank ist in der Initialisierung **TodoItemManager** Initialisierung, mit dem folgenden Code:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Dieser Code erstellt eine neue lokale SQLite Datenbank mithilfe der **MobileServiceSQLiteStore** -Klasse an.

    Die Methode **DefineTable** erstellt eine Tabelle in den lokalen Speicher, der die Felder in den bereitgestellten Typ entspricht.  Der Typ verfügt nicht über alle Spalten aufnehmen möchten, die in der Remotedatenbank sind. Es ist möglich, eine Teilmenge von Spalten zu speichern.

* Das Feld **TodoTable** **TodoItemManager** ist eine Art **IMobileServiceSyncTable** anstelle von **IMobileServiceTable**. Diese Klasse verwendet die lokale Datenbank für alle erstellen, lesen, aktualisieren und Löschen von Tabellenvorgänge. Sie entscheiden, wann diese Änderungen auf die Mobile-App Back-End-abgelegt werden, indem Sie **PushAsync** für die **IMobileServiceSyncContext**aufrufen. Im Kontext synchronisieren hilft beibehalten tabellenbeziehungen durch das verfolgen, und drücken Sie die Änderungen in allen Tabellen, die eine app Client geändert hat, wenn **PushAsync** aufgerufen wird.

    Die folgenden **SyncAsync** wird aufgerufen, für die Synchronisierung mit der Back-End Mobile-App:

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting to server's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    In diesem Beispiel wird die einfachen Fehlerbehandlung mit dem standardmäßigen synchronisieren Ereignishandler verwendet. Eine reale Anwendung würde der verschiedenen Fehler wie Netzwerkprobleme und Server Konflikte mithilfe einer benutzerdefinierten **IMobileServiceSyncHandler** Implementierung behandeln.

## <a name="offline-sync-considerations"></a>Offlinesynchronisierung Aspekte

In der Stichprobe wird nur die Methode **SyncAsync** aufgerufen, klicken Sie auf Start, und wenn eine Synchronisierung angefordert wird.  Ziehen Sie in der Elementliste nach unten, um das Initiieren der Synchronisierung in einer app für Android oder iOS; Verwenden Sie für Windows die Schaltfläche " **Synchronisieren** ". In einer realen Anwendung könnten Sie den Trigger synchronisieren vornehmen, wenn sich der Status des Netzwerks ändert.

Wenn ein Abruf für eine Tabelle ausgeführt wird, die ausstehende weist erfasst lokale Updates nach dem Kontext, der Vorgang automatisch Trigger ziehen Sie eine vorherige Kontext Pushbenachrichtigungen. Beim Aktualisieren, hinzufügen und Elemente in diesem Beispiel durchführen, können Sie den expliziten **PushAsync** Anruf auslassen.

In den bereitgestellten Code werden alle Datensätze in der Tabelle der remote TodoItem abgefragt, es kann jedoch auch zum Filtern von Datensätzen, indem Sie eine Abfrage-Id und die Abfrage an **PushAsync**übergeben. Weitere Informationen finden Sie im Abschnitt *Inkrementell synchronisieren* in [Offline Daten synchronisieren in Azure Mobile-Apps][2].

## <a name="run-the-client-app"></a>Führen Sie die Client-app

Die Clientanwendung mit offlinesynchronisierung jetzt aktiviert klicken Sie auf jede Plattform die lokale Datenbank gefüllt wird mindestens einmal ausgeführt. Später reproduzieren Sie eine offline Szenario zu, und ändern Sie die Daten im lokalen Speicher, während die app offline ist.

## <a name="update-the-sync-behavior-of-the-client-app"></a>Aktualisieren Sie das Synchronisierungsverhalten der Client-app

In diesem Abschnitt ändern Sie das Clientprojekt, um eine offline-Szenario simulieren, indem Sie über einen ungültigen Anwendung-URL für Ihre Back-End-ein. Alternativ können Sie Netzwerk-Verbindungen deaktivieren durch Verschieben von Ihrem Gerät auf "Flugzeug Modus".  Beim Hinzufügen oder Ändern von Daten, werden diese Änderungen frei, die im lokalen Speicher, aber nicht auf die Back-End-Datenspeicher synchronisiert, bis die Verbindung erneut hergestellt wird.

1. Explorer-Lösung, die Projektdatei Constants.cs aus dem Projekt **tragbares** öffnen, und ändern Sie den Wert der `ApplicationURL` auf einen ungültigen URL verweisen:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";

2. Öffnen Sie die Datei TodoItemManager.cs aus dem Projekt **tragbares** , und klicken Sie dann an hinzuzufügen Sie, **abzufangen** für die Basis Klasse mit **Ausnahme** des Blocks **try...catch** in **SyncAsync**. **Diese Aktion** gibt die Ausnahme Nachricht auf der Konsole, wie folgt ein:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }

3. Erstellen Sie und führen Sie die app Client.  Fügen Sie einige neue Elemente hinzu. Beachten Sie, dass eine Ausnahme in der Verwaltungskonsole für jeden Versuch, mit der Back-End-synchronisieren angemeldet ist. Diese neue Elemente vorhanden nur in den lokalen Speicher, bis sie zu der mobilen Back-End-abgelegt werden können. Die app Client verhält sich, als ob es angeschlossen ist, die Back-End-Unterstützung, die alle erstellen, lesen, aktualisieren, Löschvorgänge (CRUD).

4. Schließen Sie die app, und starten Sie erneut darauf, um zu überprüfen, ob der neue Elemente, die Sie erstellt haben, auf den lokalen Speicher beibehalten werden.

5. (Optional) Verwenden Sie Visual Studio zum Anzeigen der Tabelle Azure SQL-Datenbank, um anzuzeigen, dass die Daten in die Back-End-Datenbank nicht geändert hat.

    Öffnen Sie in Visual Studio **Server-Explorer**. Navigieren Sie zu Ihrer Datenbank in **Azure**->**SQL-Datenbanken**. Mit der rechten Maustaste in der Datenbank, und wählen Sie **in der SQL Server-Objekt-Explorer geöffnet**. Jetzt können Sie die SQL-Datenbank-Tabelle und deren Inhalt durchsuchen.

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a>Aktualisieren der app Client Ihre mobile Back-End-Verbindung

Schließen Sie in diesem Abschnitt die app in der mobilen Back-End, das die app in Kürze wieder Onlinestatus simuliert wieder. Wenn Sie die Aktualisierung Bewegung durchführen, ist Daten auf Ihrem mobilen Back-End-synchronisiert.

1. Öffnen Sie die Constants.cs erneut. Korrigieren der `applicationURL` auf die richtige URL verweisen.

2. Neu zu erstellen Sie, und führen Sie die app Client. Die app versucht, nach dem Start mit der mobilen app Back-End-synchronisieren. Stellen Sie sicher, dass keine Ausnahmen in der Verwaltungskonsole Debuggen angemeldet sind.

3. (Optional) Zeigen Sie die aktualisierten Daten mit SQL Server-Objekt-Explorer oder ein REST-Tool wie Fiddler oder [Postman][6]. Beachten Sie, dass die Daten zwischen der Back-End-Datenbank und den lokalen Speicher synchronisiert wurde.

    Beachten Sie die Daten zwischen der Datenbank und den lokalen Speicher wurde synchronisiert und enthält die Elemente, die Sie hinzugefügt haben, während die app getrennt wurde.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Offline-Daten synchronisieren in Azure Mobile-Apps][2]
* [So wird's gemacht Azure mobilen Apps .NET SDK][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md