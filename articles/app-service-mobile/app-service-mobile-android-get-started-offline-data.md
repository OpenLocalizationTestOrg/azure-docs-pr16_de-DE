<properties
    pageTitle="Aktivieren Sie offlinesynchronisierung für Ihre Azure Mobile-App (Android)"
    description="Informationen Sie zum Verwenden des App-Dienst Mobile-Apps mit Cache und synchronisieren offline Daten in Ihrer Anwendung Android"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Aktivieren Sie die offlinesynchronisierung für Ihre Android mobile-app

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>(Übersicht)

In diesem Lernprogramm behandelt das Feature offlinesynchronisierung Azure Mobile-Apps für Android. Offlinesynchronisierung ermöglicht es Endbenutzern zu interagieren mit einer mobile-app&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;auch, wenn keine Verbindung zum Netzwerk vorhanden ist. Änderungen werden in eine lokale Datenbank gespeichert. Nachdem Sie das Gerät wieder online ist, werden diese Änderungen mit den entfernten Back-End-synchronisiert.

Ist dies Ihr erster Eindruck mit Azure Mobile-Apps, sollten Sie zuerst das [Erstellen einer App Android]Lernprogramm abschließen. Wenn Sie die heruntergeladene Schnellstart Server Project nicht verwenden, müssen Sie die Daten Access Erweiterung Pakete zum Projekt hinzufügen. Weitere Informationen zu Server Erweiterung Paketen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Mobile-Apps Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Erfahren Sie mehr über das Feature zur Synchronisierung offline, finden Sie unter [Offline Synchronisieren von Daten in Azure Mobile-Apps].

## <a name="update-the-app-to-support-offline-sync"></a>Aktualisieren Sie die app offlinesynchronisierung unterstützen

Mit offlinesynchronisierung zum Lesen und Schreiben aus einer *Synchronisierungstabelle* (Schnittstelle der *IMobileServiceSyncTable* ), die Teil einer **SQLite** Datenbank auf Ihrem Gerät ist.

Wenn Sie Pushbenachrichtigungen, und ziehen Sie die Änderungen zwischen dem Gerät und Azure Mobile Dienste, verwenden Sie eine *Synchronisierungskontext* (*MobileServiceClient.SyncContext*), die Sie mit der lokalen Datenbank speichert Daten lokal Initialisierung.

1. In `TodoActivity.java`, kommentieren Sie die vorhandene Definition der `mToDoTable` und entfernen Sie die Kommentarzeichen Version Tabelle synchronisieren:

        private MobileServiceSyncTable<ToDoItem> mToDoTable;

2. In der `onCreate` Methode, kommentieren Sie die vorhandenen Initialisierung `mToDoTable` und kommentieren dieser Definition:

        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);

3. In `refreshItemsFromTable` kommentieren Sie die Definition der `results` und kommentieren Sie diese Definition:

        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();

4. Kommentieren Sie die Definition von `refreshItemsFromMobileServiceTable`.

5. Kommentieren Sie die Definition von `refreshItemsFromMobileServiceTableSyncTable`:

        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }

6. Kommentieren Sie die Definition von `sync`:

        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a>Testen der app

In diesem Abschnitt das Verhalten mit WiFi auf testen, und klicken Sie dann zum Erstellen einer offline Szenario WiFi deaktivieren.

Wenn Sie Datenelemente hinzufügen, werden diese frei, die im lokalen SQLite Speicher, aber nicht auf den mobilen Dienst synchronisiert, bis Sie die Schaltfläche **Aktualisieren** . Andere apps möglicherweise andere Vorschriften hinsichtlich der Daten synchronisiert werden, wann müssen haben, aber Demo Zwecken hat in diesem Lernprogramm des Benutzers explizit anfordern.

Wenn Sie die Taste drücken, wird eine neue Hintergrundaufgabe gestartet. Er legt zuerst alle Änderungen vorgenommen werden, auf den lokalen Speicher mit der Synchronisierungskontext, und klicken Sie dann auf alle geändert zieht Daten aus Azure der lokalen Tabelle.

### <a name="offline-testing"></a>Offline-Tests

1. Platzieren Sie das Gerät oder Simulator im *Flugzeug Modus*aus. Dadurch wird eine offline Szenario erstellt.

2. Fügen Sie *einige Aufgaben* oder markieren Sie einige Elemente als erledigt. Beenden Sie das Gerät oder Simulator (oder schließen Sie die app zwangsweise) und neu starten. Stellen Sie sicher, dass die Änderungen auf dem Gerät gespeichert wurden, da diese gehalten werden im lokalen SQLite Store.

3. Anzeigen des Inhalts der Tabelle Azure *TodoItem* entweder mit einem SQL-, wie etwa *SQL Server Management Studio*oder ein REST-Client, wie beispielsweise *Fiddler* oder *Postman Tool*. Überprüfen Sie, ob der neue Elemente wurden _nicht_ auf dem Server synchronisiert wurde

    + Für eine Node.js Back-End-wechseln Sie in Ihre Mobile-App und [Azure-Portal](https://portal.azure.com/)Back-End-klicken Sie auf **Einfache Tabellen** > **TodoItem** zum Anzeigen des Inhalts der `TodoItem` Tabelle.
    + Zeigen Sie für eine .NET Back-End-den Tabelleninhalt entweder mit einem SQL-Tool wie *SQL Server Management Studio*oder ein REST-Client, wie beispielsweise *Fiddler* oder *Postman an*

4. Aktivieren Sie in das Gerät oder Simulator WiFi. Drücken Sie dann die Schaltfläche **Aktualisieren** .

5. Anzeigen von TodoItem Daten im Azure-Portal erneut. Die neuen und geänderten TodoItems sollte nun angezeigt werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Offline-Daten synchronisieren in Azure Mobile-Apps]

* [Cloud Deckblatt: Offlinesynchronisierung in Azure Mobile Dienste] \(Hinweis: befindet sich das Video auf Mobile Dienste, aber offlinesynchronisierung funktioniert in ähnlicher Weise in Azure Mobile-Apps\)


<!-- URLs. -->

[Offline-Daten synchronisieren in Azure Mobile-Apps]: app-service-mobile-offline-data-sync.md

[Erstellen einer Android-App]: app-service-mobile-android-get-started.md

[Cloud Deckblatt: Offlinesynchronisierung Azure Mobile-Dienste]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

