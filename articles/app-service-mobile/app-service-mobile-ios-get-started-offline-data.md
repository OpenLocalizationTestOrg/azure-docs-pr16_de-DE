<properties
    pageTitle="Aktivieren von offlinesynchronisierung für Ihre Azure Mobile-App (iOS)"
    description="Informationen Sie zum Verwenden des App-Dienst Mobile-Apps mit Cache und synchronisieren offline Daten in Ihrer Anwendung iOS"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-ios-mobile-app"></a>Aktivieren Sie die offlinesynchronisierung für Ihre mobile iOS-app

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>(Übersicht)

In diesem Lernprogramm wird das Feature offlinesynchronisierung Azure Mobile-Apps für iOS behandelt. Offlinesynchronisierung ermöglicht es den Endbenutzern Interaktion mit einem mobilen app&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;auch, wenn keine Verbindung zum Netzwerk vorhanden ist. Änderungen werden in eine lokale Datenbank gespeichert. Nachdem Sie das Gerät wieder online ist, werden diese Änderungen mit den entfernten Back-End-synchronisiert.

Ist dies Ihr erster Eindruck mit Azure Mobile-Apps, sollten Sie zuerst das [Erstellen einer App für iOS]Lernprogramm abschließen. Wenn Sie die heruntergeladene Schnellstart Server Project nicht verwenden, müssen Sie die Daten Access Erweiterung Pakete zum Projekt hinzufügen. Weitere Informationen zu Server Erweiterung Paketen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Erfahren Sie mehr über das Feature zur Synchronisierung offline, finden Sie unter [Offline Synchronisieren von Daten in Azure Mobile-Apps].

## <a name="a-namereview-syncareview-the-client-sync-code"></a><a name="review-sync"></a>Überprüfen Sie den Code der Client synchronisieren

Das Clientprojekt, das Sie bereits für das [Erstellen einer App für iOS] -Lernprogramm heruntergeladen enthält Code mit einer lokalen Core Daten basierenden Datenbank offlinesynchronisierung unterstützen. In diesem Abschnitt finden Sie eine Zusammenfassung der was bereits in der Code des Lernprogramms enthalten ist. Eine konzeptionelle Übersicht über das Feature finden Sie unter [Offline Daten synchronisieren in Azure Mobile-Apps].

Das Feature zur Synchronisierung offline-Daten synchronisieren von Azure Mobile-Apps ermöglicht es Endbenutzern zu interagieren mit einer lokalen Datenbank, wenn das Netzwerk nicht zugegriffen werden. Zum Verwenden dieser Features in Ihrer app Initialisierung Sie synchronisieren Kontext `MSClient` und einen lokalen Speicher verweisen. Verweisen Sie dann auf die Tabelle über die `MSSyncTable` Schnittstelle.

1. Beachten Sie in **QSTodoService.m** (Ziel-C) oder **ToDoTableViewController.swift** (Swift), die den Typ des Elements `syncTable` ist `MSSyncTable`. Offlinesynchronisierung verwendet diese synchronisieren Tabellenschnittstelle anstelle von `MSTable`. Beim Synchronisierungstabelle verwendet wird, wechseln Sie auf den lokalen Speicher alle Vorgänge, und nur mit den remote Back-End-mit explizite Pushbenachrichtigungen fest, und ziehen Vorgänge synchronisiert werden.

    Wenn einen Bezug auf eine Synchronisierungstabelle erhalten möchten, verwenden Sie die Methode `syncTableWithName` auf `MSClient`. Verwenden Sie zum Entfernen offlinesynchronisierung Funktionalität `tableWithName` stattdessen.

2. Damit alle Tabellenvorgänge ausgeführt werden können, muss der lokale Speicher Initialisierung. Dies ist der relevante Code ein. 
    
    **Ziel-C**:
    
    In der `QSTodoService.init` Methode:
    
    
            MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
            self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
    
    
    **Swift**:
    
    In der `ToDoTableViewController.viewDidLoad` Methode:
    
    
            let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
            let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
            self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
            client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
    

    Dies erstellt einen lokalen Speicher die Schnittstelle `MSCoreDataStore`, die im Mobile Apps SDK enthaltenen ist. Sie können stattdessen eine Bereitstellen von einem anderen lokalen Speicher durch Implementieren der `MSSyncContextDataSource` Protokoll. 
    
    Darüber hinaus erster Parameter der `MSSyncContext` wird verwendet, um einen Konflikt Ereignishandler angeben. Da wir vergangenen `nil`, wir auf Konflikte fehlschlägt Standard-Ereignishandler Konflikt erhalten.
    
3. Jetzt, lassen Sie uns die eigentliche Synchronisierungsoperation ausführen und Abrufen von Daten aus dem remote Back-End.

    **Ziel-C**:
    
    `syncData`zunächst legt die neue Änderungen, dann ruft `pullData` zum Abrufen von Daten aus dem remote Back-End. Die Methode wiederum `pullData` ruft Sie neue Daten, die eine Abfrage entspricht:
    
    
            -(void)syncData:(QSCompletionBlock)completion
            {
                // push all changes in the sync context, then pull new data
                [self.client.syncContext pushWithCompletion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
                    [self pullData:completion];
                }];
            }
    
            -(void)pullData:(QSCompletionBlock)completion
            {
                MSQuery *query = [self.syncTable query];
    
                // Pulls data from the remote server into the local table.
                // We're pulling all items and filtering in the view
                // query ID is used for incremental sync
                [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
    
                    // Let the caller know that we have finished
                    if (completion != nil) {
                        dispatch_async(dispatch_get_main_queue(), completion);
                    }
                }];
            }
        
        
      **Swift**:
        
        
        func onRefresh(sender: UIRefreshControl!) {
            UIApplication.sharedApplication().networkActivityIndicatorVisible = true
            
            self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
                (error) -> Void in
                
                UIApplication.sharedApplication().networkActivityIndicatorVisible = false
                
                if error != nil {
                    // A real application would handle various errors like network conditions,
                    // server conflicts, etc via the MSSyncContextDelegate
                    print("Error: \(error!.description)")
                    
                    // We will just discard our changes and keep the servers copy for simplicity
                    if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                        for opError in opErrors {
                            print("Attempted operation to item \(opError.itemId)")
                            if (opError.operation == .Insert || opError.operation == .Delete) {
                                print("Insert/Delete, failed discarding changes")
                                opError.cancelOperationAndDiscardItemWithCompletion(nil)
                            } else {
                                print("Update failed, reverting to server's copy")
                                opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                            }
                        }
                    }
                }
                self.refreshControl?.endRefreshing()
            }
        } 
    
    
    In der Ziel-C-Version in `syncData`, wir zuerst anrufen `pushWithCompletion` auf dem Kontext synchronisieren. Diese Methode ist ein Mitglied der `MSSyncContext` (und nicht synchronisieren Tabelle selbst), da es Änderungen über alle Tabellen schieben wird. Nur die Datensätze, die in irgendeiner Weise lokal (über CRUD-Vorgänge) geändert wurden, werden auf dem Server gesendet. Klicken Sie dann auf die Helper `pullData` heißt, welche Anrufe `MSSyncTable.pullWithQuery` remote Daten abgerufen und in der lokalen Datenbank gespeichert.
    
    In der Version Swift vorhanden ist keine eines Anrufs an die `pushWithCompletion`. Dies ist, da der Vorgang Pushbenachrichtigungen nicht unbedingt erforderlich ist. Wenn in den Kontext synchronisieren, für die Tabelle, die einen Vorgang Pushbenachrichtigungen durchführt Änderungen ausstehend vorhanden sind, ziehen Sie immer Probleme einer Pushbenachrichtigungen zuerst. Jedoch, wenn Sie mehr als eine Synchronisierungstabelle verfügen, ist es am besten explizit aufrufen Pushbenachrichtigungen, um sicherzustellen, dass alles auf verknüpften Tabellen konsistent ist.
    
    In der Ziel-C und die Swift-Versionen, die Methode `pullWithQuery` ermöglicht es Ihnen, geben Sie eine Abfrage zum Filtern der Datensätze, die Sie abrufen möchten. In diesem Beispiel die Abfrage nur Ruft alle Datensätze in der entfernten `TodoItem` Tabelle.
    
    Der zweite Parameter für `pullWithQuery` wird eine Abfrage-ID, die für *inkrementell synchronisieren*verwendet wird. Inkrementell synchronisieren ruft nur die Datensätze, die seit der letzten Synchronisierung, mithilfe des Datensatzes geändert `UpdatedAt` Zeitstempel (aufgerufen `updatedAt` im lokalen Speicher.) Die Abfrage-ID sollten eine beschreibende Zeichenfolge, die für jeden logischen Abfrage in Ihrer app eindeutig ist. Um der inkrementell synchronisieren Teilnahme, übergeben `nil` als die Abfrage-ID. Beachten Sie, dass dies möglicherweise nicht effizient sein kann da es werden alle Datensätze bei jedem Abruf Vorgang abrufen.

5. Die Ziel-C-app synchronisiert, wenn wir ändern oder Hinzufügen von Daten, ein Benutzer führt die Bewegung aktualisieren, und klicken Sie auf Launch. Die Swift-app synchronisiert, wenn ein Benutzer die Aktualisierung Bewegung ausführt, und klicken Sie auf Launch. 

Da der app synchronisiert Daten besteht (Ziel-C) geändert oder immer, wenn die app (Ziel-C und Swift) beginnt, bei die app wird vorausgesetzt, dass der Benutzer online ist. In einen anderen Abschnitt aktualisieren wir die app, damit Benutzer bearbeiten können, auch wenn sie offline sind.

## <a name="a-namereview-core-dataareview-the-core-data-model"></a><a name="review-core-data"></a>Überprüfen der Core-Datenmodell

Wenn Sie den offline Core Datenspeicher verwenden zu können, müssen Sie zum Definieren von bestimmter Tabellen und Feldern in Ihr Datenmodell. Beispiel-app enthält bereits ein Datenmodell für das gewünschte Format aus. In diesem Abschnitt werden wir durch diese Tabellen und deren Verwendung geführt.

- Öffnen Sie **QSDataModel.xcdatamodeld**. Es gibt vier Tabellen definiert – drei, die vom SDK, verwendet werden, und eine Tabelle für das erledigen Elemente selbst:     *MS_TableOperations: zum Nachverfolgen der Elemente, die mit dem Server synchronisiert werden müssen    * MS_TableOperationErrors: zum Nachverfolgen von Fehlern, die während offlinesynchronisierung erfolgen     *MS_TableConfig: zum Nachverfolgen des letzten Aktualisierung Zeit für den letzten Synchronisierungsvorgang für alle Vorgänge der Abruf    * TodoItem : Für das Speichern von erledigen Elemente aus. Das System Spalten **CreatedAt**, **UpdatedAt**und **Version** sind optionale Systemeigenschaften auf.

>[AZURE.NOTE] Das Azure Mobile-Apps SDK reserviert Spaltennamen dieser wird mit "**``**". Verwenden Sie dieses Präfix nicht auf einen anderen Wert als Systemspalten, andernfalls Ihrer Spaltennamen werden geändert werden, wenn der remote Back-End-verwenden.

- Wenn Sie das Feature zur Synchronisierung offline verwenden zu können, müssen Sie Systemtabellen definieren, wie unten dargestellt.

    ### <a name="system-tables"></a>Systemtabellen

    **MS_TableOperations**

    ![][defining-core-data-tableoperations-entity]

  	| Attribut  |    Typ     |
  	|----------- |   ------    |
  	| ID         | Ganze Zahl 64  |
  	| itemId     | Zeichenfolge      |
  	| Eigenschaften | Binäre Daten |
  	| Tabelle      | Zeichenfolge      |
  	| tableKind  | Ganze Zahl 16  |

    <br>**MS_TableOperationErrors**

    ![][defining-core-data-tableoperationerrors-entity]

  	| Attribut  |    Typ     |
  	|----------- |   ------    |
  	| ID         | Zeichenfolge      |
  	| operationId | Ganze Zahl 64 |
  	| Eigenschaften | Binäre Daten |
  	| tableKind  | Ganze Zahl 16  |

    <br>**MS_TableConfig**

    ![][defining-core-data-tableconfig-entity]

  	| Attribut  |    Typ     |
  	|----------- |   ------    |
  	| ID         | Zeichenfolge      |
  	| Schlüssel        | Zeichenfolge      |
  	| keyType    | Ganze Zahl 64  |
  	| Tabelle      | Zeichenfolge      |
  	| Wert      | Zeichenfolge      |

    ### <a name="data-table"></a>Datentabelle

    **TodoItem**

  	| Attribut    |  Typ   | Notiz                                                   |
  	|-----------   |  ------ | -------------------------------------------------------|
  	| ID           | Zeichenfolge, die erforderlichen markiert  | Primärschlüssel remote Store                            |
  	| Führen Sie die     | Boolesch | erledigen Elementfeld                                        |
  	| Text         | Zeichenfolge  | erledigen Elementfeld                                        |
  	| createdAt | Datum    | (optional) Maps CreatedAt System-Eigenschaft         |
  	| updatedAt | Datum    | (optional) Maps UpdatedAt System-Eigenschaft         |
  	| Version   | Zeichenfolge  | (optional) verwendet, um Konflikte, Karten, Version zu erkennen |


## <a name="a-namesetup-syncachange-the-sync-behavior-of-the-app"></a><a name="setup-sync"></a>Ändern Sie das Synchronisierungsverhalten der app

In diesem Abschnitt ändern Sie die app, damit es nicht app starten, oder wenn einfügen und Aktualisieren von Elementen synchronisieren, aber nur, wenn die Schaltfläche Aktualisieren Bewegung durchgeführt wird.

**Ziel-C**:

1. Ändern Sie die **ViewDidLoad** -Methode, um den Anruf zu entfernen **QSTodoListViewController.m**, `[self refresh]` am Ende der Methode. Jetzt, die Daten werden nicht mit dem Start des app-Server synchronisiert werden, aber stattdessen den Inhalt der lokalen Speicher werden.

2. Ändern Sie im **QSTodoService.m**, die Definition von `addItem` , damit es nicht synchronisieren, nachdem Sie das Element eingefügt wird. Entfernen der `self syncData` blockieren und Ersetzen durch Folgendes:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

3. Ändern die Definition `completeItem` wie oben; Entfernen des Zeitraums für `self syncData` und Ersetzen durch Folgendes:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

**Swift**:

1. In `viewDidLoad` in **ToDoTableViewController.swift**, kommentieren diese beiden Zeilen und beim Start der app Synchronisierung beenden. Zum Zeitpunkt der dieses Artikels schreiben aktualisiert die app Swift erledigen nicht den Dienst, wenn hinzufügt oder Abschluss ein Elements, nur auf die app starten.

        self.refreshControl?.beginRefreshing()
        self.onRefresh(self.refreshControl)


## <a name="a-nametest-appatest-the-app"></a><a name="test-app"></a>Testen der app

In diesem Abschnitt werden Sie auf einen ungültigen URL zu einem offline-Szenario simulieren verbunden. Wenn Sie Datenelemente hinzufügen, werden diese frei, die in der lokalen Core Datenspeicher, aber nicht auf dem mobilen Back-End synchronisiert.

1. Ändern Sie die Mobile-App-URL in **QSTodoService.m** auf einen ungültigen URL, und führen Sie die app erneut aus:

    **Ziel-C** in QSTodoService.m:
    
            self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
    
    **Swift** in ToDoTableViewController.swift:

        let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")

2. Fügen Sie einige Aufgaben hinzu. Beenden Sie den Simulator (oder schließen Sie die app zwangsweise) und neu starten. Stellen Sie sicher, dass die Änderungen gespeichert wurden.

3. Anzeigen des Inhalts der remote TodoItem Tabelle an:

    + Für eine Node.js Back-End-wechseln Sie in Ihre Mobile-App und [Azure-Portal](https://portal.azure.com/)Back-End-klicken Sie auf **Einfache Tabellen** > **TodoItem** zum Anzeigen des Inhalts der `TodoItem` Tabelle.
    + Zeigen Sie für eine .NET Back-End-den Tabelleninhalt entweder mit einem SQL-Tool wie SQL Server Management Studio oder ein REST-Client, wie beispielsweise Fiddler oder Postman an

    Überprüfen Sie, ob der neue Elemente wurden *nicht* auf dem Server synchronisiert wurde:

4. Ändern Sie die URL in der richtigen auf in **QSTodoService.m** , und führen Sie die app erneut aus. Führen Sie die Aktualisierung Bewegung durch Ziehen Sie in der Liste der Elemente aus. Ein Drehfeld Fortschritt wird angezeigt.

5. Zeigen Sie die Daten TodoItem wieder. Die neuen und geänderten TodoItems sollte nun angezeigt werden.

## <a name="summary"></a>Zusammenfassung

Um das Feature offlinesynchronisierung unterstützen, wir verwendet die `MSSyncTable` Schnittstelle und Initialisierung `MSClient.syncContext` mit einem lokalen Speicher. In diesem Fall wurde der lokale Speicher einer Core Daten basierenden Datenbank.

Wenn Sie einen lokalen Core Datenspeicher verwenden zu können, müssen Sie mehrere Tabellen mit den [richtigen Systemeigenschaften](#review-core-data)definieren.

Die normalen Vorgänge für Mobile-Apps Azure arbeiten, als ob die app ist immer noch verbunden, aber alle Vorgänge im lokalen Speicher auftreten.

Wenn den lokalen Speicher mit dem Server synchronisiert wollten, wir verwendet die `MSSyncTable.pullWithQuery`Methode.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Offline-Daten synchronisieren in Azure Mobile-Apps]

* [Cloud Deckblatt: Offlinesynchronisierung in Azure Mobile Dienste] \(Hinweis: befindet sich das Video auf Mobile Dienste, aber offlinesynchronisierung funktioniert in ähnlicher Weise in Azure Mobile-Apps\)

<!-- URLs. -->


[Erstellen einer App für iOS]: app-service-mobile-ios-get-started.md
[Offline-Daten synchronisieren in Azure Mobile-Apps]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Cloud Deckblatt: Offlinesynchronisierung Azure Mobile-Dienste]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
