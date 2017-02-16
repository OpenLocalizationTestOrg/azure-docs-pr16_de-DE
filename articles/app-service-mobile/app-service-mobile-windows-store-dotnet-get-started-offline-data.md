<properties
    pageTitle="Aktivieren Sie die offlinesynchronisierung für Ihre app Universal Windows-Plattform (UWP) mit Mobile-Apps | Azure App-Verwaltungsdienst"
    description="Informationen Sie zum Verwenden einer Azure Mobile-App zu Cache und synchronisieren offline-Daten in Ihrer app Universal Windows-Plattform (UWP)."
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-windows-app"></a>Aktivieren Sie die offlinesynchronisierung für Ihre Windows-app

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>(Übersicht)

In diesem Lernprogramm erfahren Sie, wie eine Universal Windows-Plattform (UWP)-app mit einer Azure Mobile-App Back-End-offlinesupport hinzugefügt. Offlinesynchronisierung ermöglicht es Endbenutzern zu interagieren mit einer mobile-app – anzeigen, hinzufügen oder Ändern von Daten – selbst wenn keine Verbindung zum Netzwerk vorhanden ist. Änderungen werden in eine lokale Datenbank gespeichert. Nachdem Sie das Gerät wieder online ist, werden diese Änderungen mit den entfernten Back-End-synchronisiert.

In diesem Lernprogramm aktualisieren Sie das app-Projekt UWP Lernprogramm [Erstellen einer app für Windows] , um die offline Features von Azure Mobile-Apps unterstützen. Wenn Sie die heruntergeladene Schnellstart Server Project nicht verwenden, müssen Sie die Daten Access Erweiterung Pakete zum Projekt hinzufügen. Weitere Informationen zu Server Erweiterung Paketen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Mobile-Apps Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Erfahren Sie mehr über das Feature zur Synchronisierung offline, finden Sie unter [Offline Synchronisieren von Daten in Azure Mobile-Apps].

## <a name="requirements"></a>Anforderungen

In diesem Lernprogramm erfordert die folgenden erforderlichen Komponenten:

* Visual Studio 2013 unter Windows 8.1 oder höher ausgeführt.
* [Erstellen einer app für Windows],[erstellen eine Windows-app]Beendigung.
* [Azure Mobile Services SQLite Store][sqlite store nuget]
* [SQLite für die Entwicklung von Universal Windows-Plattform](http://www.sqlite.org/downloads)

## <a name="update-the-client-app-to-support-offline-features"></a>Aktualisieren der app Client offline Features unterstützt.

Azure offline Mobile-App-Features ermöglichen es Ihnen, mit einer lokalen Datenbank zu interagieren, wenn Sie in einem Szenario offline sind. Zum Verwenden dieser Features in Ihrer app Initialisierung Sie eine [SyncContext] [ synccontext] zu einem lokalen Speicher. Klicken Sie dann verwiesen Sie die Tabelle über die [IMobileServiceSyncTable][IMobileServiceSyncTable] Schnittstelle werden. SQLite wird als lokaler Speicher auf dem Gerät verwendet.

1. Installieren Sie die [SQLite Runtime für die Universal Windows-Plattform](http://sqlite.org/2016/sqlite-uwp-3120200.vsix).

2. Öffnen Sie in Visual Studio das NuGet-Paket-Manager für das UWP-app-Projekt, das Sie in das [Erstellen einer app für Windows] -Lernprogramm abgeschlossen.
    Suchen Sie und installieren Sie das **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-Paket.

4. Explorer-Lösung, mit der Maustaste **Verweise** > **Verweis hinzufügen...**  >  **Universal Windows** > **Erweiterungen**, aktivieren Sie sowohl **SQLite für Universal Windows-Plattform** und **Visual C++ 2015 Runtime für Windows-Plattform universeller apps**.

    ![Fügen Sie SQLite UWP Verweis][1]

5. Öffnen Sie die Datei MainPage.xaml.cs und entfernen Sie die Kommentarzeichen der `#define OFFLINE_SYNC_ENABLED` Definition.

6. Drücken Sie **F5** , um neu zu erstellen, und führen Sie die Client-app in Visual Studio. Die app funktioniert genauso wie vor dem Sie offline synchronisieren aktiviert. Die lokale Datenbank ist jedoch jetzt mit Daten aufgefüllt, die in einem Szenario offline verwendet werden können.

## <a name="a-nameupdate-syncaupdate-the-app-to-disconnect-from-the-backend"></a><a name="update-sync"></a>Aktualisieren Sie die app von der Back-End-trennen

In diesem Abschnitt heben Sie die Verbindung in Ihre Mobile-App Back-End-um eine offline Situation zu reproduzieren. Wenn Sie Datenelemente hinzufügen, wird Ihre Ereignishandler Ausnahme, dass die app im Offlinemodus befindet. In diesem Zustand neue Elemente im lokalen Speicher hinzugefügt und wenn Sie Pushbenachrichtigungen in einem verbundenen Zustand weiter ausgeführt wird in der mobilen app Back-End-synchronisiert.

1. Bearbeiten Sie App.xaml.cs in das freigegebene Projekt. Kommentieren der Initialisierung der **MobileServiceClient** , und fügen Sie die folgende Zeile, die eine ungültiges mobile-app-URL verwendet:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    Sie können auch offline Verhalten durch Deaktivieren der Wifi und Mobile Netzwerke auf dem Gerät veranschaulichen oder Flugzeug-Modus verwenden.

2. Drücken Sie **F5** , um zu erstellen, und führen Sie die app aus. Beachten Sie Ihre synchronisieren, klicken Sie auf Aktualisierung fehlgeschlagen ist, wenn die app gestartet wird.

3. Geben Sie die neue Elemente, und beachten Sie, dass Pushbenachrichtigungen mit dem Status [CancelledByNetworkError] jedes Mal schlägt Sie klicken Sie auf **Speichern**. Es gibt jedoch die neue erledigen Elemente im lokalen Speicher, bis sie zu der mobilen app Back-End-abgelegt werden können.  In einer app Herstellung Sie diese Ausnahmen unterdrücken verhält sich die app Client wie bei sie noch in der mobilen app Back-End-verbunden ist.

4. Schließen Sie die app, und starten Sie erneut darauf, um zu überprüfen, ob der neue Elemente, die Sie erstellt haben, auf den lokalen Speicher beibehalten werden.

5. (Optional) Öffnen Sie in Visual Studio **Server-Explorer**. Navigieren Sie zu Ihrer Datenbank in **Azure**->**SQL-Datenbanken**. Mit der rechten Maustaste in der Datenbank, und wählen Sie **in der SQL Server-Objekt-Explorer geöffnet**. Jetzt können Sie die SQL-Datenbank-Tabelle und deren Inhalt durchsuchen. Stellen Sie sicher, dass die Daten in die Back-End-Datenbank nicht geändert hat.

6. (Optional) Verwenden ein REST-Tools, wie etwa Fiddler oder Postman zum Abfragen von Ihrem mobilen Back-End-, mithilfe einer GET-Abfrage in das Formular `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="a-nameupdate-online-appaupdate-the-app-to-reconnect-your-mobile-app-backend"></a><a name="update-online-app"></a>Aktualisieren Sie die app, um Ihre Mobile-App Back-End-erneut verbinden

In diesem Abschnitt schließen Sie die app in der mobilen app Back-End-wieder. Diese Änderungen simulieren der App ein Netzwerk erneut eine Verbindung herzustellen.

Beim Ausführen der Anwendungs, die `OnNavigatedTo` Ereignishandler ruft `InitLocalStoreAsync`. Diese Methode wiederum ruft `SyncAsync` Ihrer lokalen Speicher mit der Back-End-Datenbank zu synchronisieren. Die app versucht, die beim Start synchronisieren.

1. Öffnen Sie App.xaml.cs in das freigegebene Projekt, und entfernen Sie die Kommentarzeichen Ihre vorherige Initialisierung der `MobileServiceClient` die richtige URL mobile-app verwendet.

2. Drücken Sie **F5** , um neu zu erstellen, und führen Sie die app aus. Die app synchronisiert die lokalen Änderungen mit Azure Mobile-App Back-End-mithilfe von Pushbenachrichtigungen fest, und Ziehen bei der `OnNavigatedTo` Ereignishandler ausgeführt wird.

3. (Optional) Anzeigen der aktualisierten Daten mithilfe eines Tools REST wie Fiddler oder SQL Server-Objekt-Explorer. Beachten Sie die Daten wurde zwischen der Azure Mobile-App Back-End-Datenbank und der lokalen Speicher synchronisiert.

4. Klicken Sie in der app auf das Kontrollkästchen neben dem einige Elemente können in den lokalen Speicher ausführen.

  `UpdateCheckedTodoItem`Anrufe `SyncAsync` zum Synchronisieren jedes abgeschlossen Element mit der Mobile-App Back-End. `SyncAsync`Ruft beiden Pushbenachrichtigungen, und ziehen. Jedoch **immer, wenn Sie einen Abruf für eine Tabelle ausführen, die der Client Änderungen vorgenommen hat eine Pushbenachrichtigungen wird immer automatisch ausgeführt**. Dieses Verhalten wird sichergestellt, dass alle Tabellen im lokalen Speicher zusammen mit Beziehungen konsistent bleiben. Dieses Verhalten kann dazu führen, dass ein unerwarteter Pushbenachrichtigungen.  Weitere Informationen zu diesem Verhalten finden Sie unter [Offline Daten synchronisieren in Azure Mobile-Apps].


##<a name="api-summary"></a>Zusammenfassung-API

Die mobile-Dienste offline Features unterstützt, wir verwendet die [IMobileServiceSyncTable] -Benutzeroberfläche und Initialisierung [MobileServiceClient.SyncContext] [ synccontext] mit einer lokalen SQLite Datenbank. Wenn Sie offline arbeiten der normalen Vorgänge für Mobile-Apps, als wäre die app noch verbunden ist, während die Vorgänge im lokalen Speicher auftreten. Die folgenden Methoden werden verwendet, um die im lokalen Speicher mit dem Server zu synchronisieren:

*  **[PushAsync]** Da diese Methode ein Mitglied der [IMobileServicesSyncContext]ist, werden die Änderungen über alle Tabellen in die Back-End-abgelegt. Nur Datensätze mit lokalen Änderungen werden auf dem Server gesendet.

* **[PullAsync] ** 
   ein Abruf aus einer [IMobileServiceSyncTable]gestartet wird. Überarbeitungen in der Tabelle vorhanden sind, wird ein implizit Pushbenachrichtigungen ausgeführt, um sicherzustellen, dass alle Tabellen im lokalen Speicher zusammen mit Beziehungen konsistent bleiben. Der Parameter *PushOtherTables* steuert, ob andere Tabellen im Kontext in einer implizit Pushbenachrichtigungen abgelegt werden. Der *Abfrage* Parameter hat eine [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] 
   oder OData-Abfragezeichenfolge, um die zurückgegebenen Daten zu filtern. Der *QueryId* -Parameter dient zum Definieren von inkrementell synchronisieren. Weitere Informationen finden Sie unter  [Offline Daten synchronisieren in Azure Mobile-Apps](app-service-mobile-offline-data-sync.md#how-sync-works).

* **[PurgeAsync]** Ihre app sollten regelmäßig rufen Sie diese Methode, um veraltete Daten aus dem lokalen Speicher zu löschen. Verwenden Sie den Parameter *erzwingen* , wenn Sie benötigen, um Änderungen zu bereinigen, die noch nicht synchronisiert wurden.

Weitere Informationen zu diesen Konzepten finden Sie unter [Offline Daten synchronisieren in Azure Mobile-Apps](app-service-mobile-offline-data-sync.md#how-sync-works).

## <a name="more-info"></a>Weitere Informationen

Die folgenden Themen enthalten weitere Hintergrundinformationen zum Feature offlinesynchronisierung Mobile-Apps:

* [Offline-Daten synchronisieren in Azure Mobile-Apps]
* [So wird's gemacht Azure mobilen Apps .NET SDK][8]

<!-- Anchors. -->
[Update the app to support offline features]: #enable-offline-app
[Update the sync behavior of the app]: #update-sync
[Update the app to reconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[Offline-Daten synchronisieren in Azure Mobile-Apps]: app-service-mobile-offline-data-sync.md
[Erstellen einer app für windows]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
