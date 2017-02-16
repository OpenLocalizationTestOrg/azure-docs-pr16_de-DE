<properties
    pageTitle="Herstellen einer Verbindung Azure-Speicher in Ihrer app Xamarin.Forms mit"
    description="Hinzufügen von Bildern zu erledigen Liste Xamarin.Forms mobile-app durch Herstellen einer Verbindung mit Azure Blob-Speicher"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
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

#<a name="connect-to-azure-storage-in-your-xamarinforms-app"></a>Herstellen einer Verbindung Azure-Speicher in Ihrer app Xamarin.Forms mit

## <a name="overview"></a>(Übersicht)

Mobile-Apps Azure-Client und Server SDK unterstützt offlinesynchronisierung von strukturierten Daten mit gegen den Endpunkt /tables Vorgänge. Im Allgemeinen werden diese Daten in einer Datenbank oder eine ähnliche Store gespeichert und in der Regel diese Datenspeicher können große binäre Daten speichern effizient. Darüber hinaus haben einige Applikationen verknüpften Daten, die an anderer Stelle (z. B. BLOB-Speicher, Dateifreigaben), gespeichert, und es ist sinnvoll, Zuordnungen zwischen Datensätzen in den Endpunkt /tables und anderen Daten erstellen können.

In diesem Thema wird gezeigt, wie die Mobile-Apps erledigen Liste Schnellstart Unterstützung für Bilder hinzugefügt. Sie müssen zuerst das Lernprogramm [Erstellen Sie eine app Xamarin.Forms]ausfüllen.

In diesem Lernprogramm werden Sie erstellen Sie ein Speicherkonto und Ihre Mobile-App Back-End-eine Verbindungszeichenfolge hinzugefügt. Anschließend fügen Sie eine neue von den neuen Mobile-Apps Typ erben `StorageController<T>` zum Serverprojekt.

>[AZURE.TIP] In diesem Lernprogramm weist einer [Stichprobe Companion](https://azure.microsoft.com/documentation/samples/app-service-mobile-dotnet-todo-list-files/) verfügbar, die in Ihrem eigenen Azure-Konto bereitgestellt werden können. 

## <a name="prerequisites"></a>Erforderliche Komponenten

* Führen Sie das Lernprogramm [Erstellen einer app Xamarin.Forms] , das andere erforderliche Komponenten aufgeführt sind. In diesem Artikel wird die fertige app aus diesem Lernprogramm verwendet.

>[AZURE.NOTE] Wenn Sie mit Azure-App-Verwaltungsdienst anzufangen, bevor Sie für ein Azure-Konto anmelden möchten, wechseln Sie zu [App-Verwaltungsdienst versuchen](https://tryappservice.azure.com/?appServiceName=mobile). Vorhanden, können Sie sofort eine kurzlebige Starter mobile-app erstellen, im App-Dienst – keine Kreditkarte erforderlich, und keine Zusagen.

## <a name="create-a-storage-account"></a>Erstellen Sie ein Speicherkonto

1. Erstellen Sie ein Speicherkonto anhand des Lernprogramms [ein Azure-Speicher-Konto zu erstellen]. 

2. Im Portal Azure navigieren Sie zu Ihrem Speicherkonto neu erstellten, und klicken Sie auf das Symbol **Tasten** . Kopieren Sie die **primäre Verbindungszeichenfolge**.

3. Navigieren Sie zu Ihrem mobilen app Back-End. Klicken Sie unter **Alle Einstellungen** -> **Anwendungseinstellungen** -> **Verbindungszeichenfolgen**, erstellen Sie einen neuen Product Key, mit dem Namen `MS_AzureStorageAccountConnectionString` und verwenden Sie den Wert aus Ihrem Speicherkonto kopiert haben. Verwenden Sie **benutzerdefinierte** als Key Typ aus.

## <a name="add-a-storage-controller-to-the-server"></a>Fügen Sie einen Speichercontroller auf dem server

Sie müssen einen neuen Controller zu einem Serverprojekt hinzufügen, die wird Beantworten von Besprechungsanfragen für eine SAS Token für Azure-Speicher sowie eine Liste mit Dateien, die übereinstimmen mit einem Datensatz zurück:

- [Fügen Sie einen Speichercontroller zum Serverprojekt](#add-controller-code)
- [Vom Speichercontroller registriert ist](#routes-registered)
- [Client- und Server-Kommunikation](#client-communication)

###<a name="a-nameadd-controller-codeaadd-a-storage-controller-to-your-server-project"></a><a name="add-controller-code"></a>Fügen Sie einen Speichercontroller zum Serverprojekt

1. Öffnen Sie in Visual Studio .NET Serverprojekt aus. Fügen Sie die Nuget-Paket [Microsoft.Azure.Mobile.Server.Files]hinzu. Achten Sie darauf, wählen Sie die **Vorabversion enthalten**.

2. Öffnen Sie in Visual Studio .NET Serverprojekt aus. Mit der rechten Maustaste in des Ordners **Controller** , und wählen Sie **Hinzufügen** -> **Controller** -> **Web API 2 Controller - leeren**. Nennen Sie den Controller `TodoItemStorageController`.

3. Fügen Sie den folgenden Anweisungen verwenden:

        using Microsoft.Azure.Mobile.Server.Files;
        using Microsoft.Azure.Mobile.Server.Files.Controllers;

4. Ändern Sie die base Klasse zu `StorageController`:
    
        public class TodoItemStorageController : StorageController<TodoItem>

5. Fügen Sie der Klasse die folgenden Methoden:

        [HttpPost]
        [Route("tables/TodoItem/{id}/StorageToken")]
        public async Task<HttpResponseMessage> PostStorageTokenRequest(string id, StorageTokenRequest value)
        {
            StorageToken token = await GetStorageTokenAsync(id, value);

            return Request.CreateResponse(token);
        }

        // Get the files associated with this record
        [HttpGet]
        [Route("tables/TodoItem/{id}/MobileServiceFiles")]
        public async Task<HttpResponseMessage> GetFiles(string id)
        {
            IEnumerable<MobileServiceFile> files = await GetRecordFilesAsync(id);

            return Request.CreateResponse(files);
        }

        [HttpDelete]
        [Route("tables/TodoItem/{id}/MobileServiceFiles/{name}")]
        public Task Delete(string id, string name)
        {
            return base.DeleteFileAsync(id, name);
        }

6. Aktualisieren Sie die Web-API Konfiguration Attribut routing einrichten. In **Startup.MobileApp.cs**, fügen Sie die folgende Zeile in der `ConfigureMobileApp()` Methode, nach der Definition der `config` Variable:

        config.MapHttpAttributeRoutes();

7. Ihre mobile-app Back-End-veröffentlichen Sie Serverprojekt.

###<a name="a-nameroutes-registeredaroutes-registered-by-the-storage-controller"></a><a name="routes-registered"></a>Vom Speichercontroller registriert ist

Das neue `TodoItemStorageController` macht zwei untergeordnete Ressourcen unter den Eintrag es verwaltet:

- StorageToken

    + HTTP-POST: Erstellt ein Token mit Speicher
    
        `/tables/TodoItem/{id}/MobileServiceFiles`
    
- MobileServiceFiles

    + HTTP zu erhalten: Ruft eine Liste mit den Eintrag zugeordneten Dateien
    
        `/tables/TodoItem/{id}/MobileServiceFiles`

    + HTTP-löschen: Löscht die Datei, die in der Datei Resource Identifier angegeben
    
        `/tables/TodoItem/{id}/MobileServiceFiles/{fileid}`

###<a name="a-nameclient-communicationaclient-and-server-communication"></a><a name="client-communication"></a>Client- und Server-Kommunikation

Beachten Sie, dass `TodoItemStorageController` unterstützt ** keiner Routing für das Hochladen oder Herunterladen eines BLOBs haben. Dies liegt daran bei ein mobiler Client BLOB-Speicher *direkt* interagiert, um diese Vorgänge, nach dem ersten erste ein SAS Token (freigegebene Access Signatur) zu einem bestimmten Blob oder Container sicheren Zugriff auf ausführen. Dies ist eine wichtige Architektur Entwurf, da andernfalls Zugriff auf Speicher die Skalierbarkeit und die Verfügbarkeit von der mobilen Back-End-begrenzt werden möchten. Stattdessen kann direkt an Azure-Speicher anschließen, des mobile Clients seine Features wie automatische Partitionierung und Geo-Verteilung nutzen.

Eine freigegebene Access-Signatur bietet delegierten Zugriff auf Ihr Speicherkonto Ressourcen. Dies bedeutet, dass Sie, dass ein Client für einen angegebenen Zeitraum Zeit und mit einem zuvor festgelegten Berechtigungen, Berechtigungen für Objekte in Ihr Speicherkonto eingeschränkt gewähren können, ohne zu Ihrem Konto Zugriffstasten freigeben. Weitere Informationen finden Sie unter [Grundlegendes zu freigegebenen Access Signaturen].

Im nachstehenden Diagramm zeigt die Client- und Server-Interaktionen an. Vor dem Hochladen einer Datei, fordert der Client ein SAS Token vom Dienst an. Der Dienst verwendet die Verbindungszeichenfolge Speicher zum Generieren der eine neue SAS, das dann an den Kunden zurückgegeben. Die SAS zeitlich beschränkt ist, und den Berechtigungen nur eine bestimmte Datei oder Container beschränkt. Des mobile Clients werden dann diese-SAS- und den Azure-Speicher Client SDK zum Hochladen der Datei auf Blob-Speicher verwendet.

![Ein SAS Token anfordert](./media/app-service-mobile-xamarin-forms-blob-storage/storage-token-diagram.png)

## <a name="update-your-client-app-to-add-image-support"></a>Aktualisieren Sie Ihre app Client, um Unterstützung Bild hinzuzufügen

Öffnen Sie das Xamarin.Forms Schnellstart-Projekt in Visual Studio oder Xamarin Studio ein. Nuget-Pakete installieren und aktualisieren Sie das Bibliotheksprojekt tragbares und den iOS, Android und Windows Client-Projekten:

- [Hinzufügen von Nuget-Paketen](#add-nuget)
- [Hinzufügen von IPlatform-Benutzeroberfläche](#add-iplatform)
- [Fügen Sie FileHelper-Klasse](#add-filehelper)
- [Hinzufügen einer synchronisieren Dateihandler](#file-sync-handler)
- [Aktualisieren von TodoItemManager](#update-todoitemmanager)
- [Fügen Sie eine Detailansicht hinzu.](#add-details-view)
- [Aktualisieren Sie im Hauptfenster anzeigen](#update-main-view)
- [Aktualisieren Sie das Projekt Android](#update-android), [iOS Projekt](#update-ios), [Windows-Projekt](#update-windows)

>[AZURE.NOTE] In diesem Lernprogramm enthält nur Anweisungen für Android, iOS und Windows Store-Plattformen nicht Windows Phone.

###<a name="a-nameadd-nugetaadd-nuget-packages"></a><a name="add-nuget"></a>Hinzufügen von Nuget-Paketen

Mit der rechten Maustaste in der Lösung, und wählen Sie **Verwalten Nuget-Pakete Lösung**. Fügen Sie die folgenden Nuget-Paketen für **Alle** Projekte in der Lösung. Achten Sie darauf, dass **Vorabversion einschließen**aktivieren.

  - [Microsoft.Azure.Mobile.Client.Files]

  - [Microsoft.Azure.Mobile.Client.SQLiteStore]

  - [PCLStorage]

Zur Vereinfachung in diesem Beispiel wird die Bibliothek [PCLStorage] , aber es nicht vom Azure Mobile-Apps Client SDK erforderlich.

[PCLStorage]: https://www.nuget.org/packages/PCLStorage/

###<a name="a-nameadd-iplatformaadd-iplatform-interface"></a><a name="add-iplatform"></a>Hinzufügen von IPlatform-Benutzeroberfläche

Erstellen Sie eine neue Schnittstelle `IPlatform` im Hauptfenster portable Library-Projekt. Entspricht dem Muster [Xamarin.Forms DependencyService] , um die rechts-Plattform-spezifische Klasse zur Laufzeit zu laden. Sie werden später Plattform-spezifische Implementierungen mit jeder der Clientprojekte hinzufügen.

1. Fügen Sie den folgenden Anweisungen verwenden:

        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Sync;

2. Ersetzen Sie die Implementierung durch Folgendes ein:

        public interface IPlatform
        {
            Task <string> GetTodoFilesPathAsync();

            Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata);

            Task<string> TakePhotoAsync(object context);

            Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename);
        }

###<a name="a-nameadd-filehelperaadd-filehelper-class"></a><a name="add-filehelper"></a>Fügen Sie FileHelper-Klasse

1. Erstellen Sie eine neue Klasse `FileHelper` im Hauptfenster portable Library-Projekt. Fügen Sie den folgenden Anweisungen verwenden:

        using System.IO;
        using PCLStorage;
        using System.Threading.Tasks;
        using Xamarin.Forms;

2. Fügen Sie die Definition der Klasse hinzu:

        public class FileHelper
        {
            public static async Task<string> CopyTodoItemFileAsync(string itemId, string filePath)
            {
                IFolder localStorage = FileSystem.Current.LocalStorage;

                string fileName = Path.GetFileName(filePath);
                string targetPath = await GetLocalFilePathAsync(itemId, fileName);

                var sourceFile = await localStorage.GetFileAsync(filePath);
                var sourceStream = await sourceFile.OpenAsync(FileAccess.Read);

                var targetFile = await localStorage.CreateFileAsync(targetPath, CreationCollisionOption.ReplaceExisting);

                using (var targetStream = await targetFile.OpenAsync(FileAccess.ReadAndWrite)) {
                    await sourceStream.CopyToAsync(targetStream);
                }

                return targetPath;
            }

            public static async Task<string> GetLocalFilePathAsync(string itemId, string fileName)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();

                string recordFilesPath = Path.Combine(await platform.GetTodoFilesPathAsync(), itemId);

                    var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(recordFilesPath);
                    if (checkExists == ExistenceCheckResult.NotFound) {
                        await FileSystem.Current.LocalStorage.CreateFolderAsync(recordFilesPath, CreationCollisionOption.ReplaceExisting);
                    }

                return Path.Combine(recordFilesPath, fileName);
            }

            public static async Task DeleteLocalFileAsync(Microsoft.WindowsAzure.MobileServices.Files.MobileServiceFile fileName)
            {
                string localPath = await GetLocalFilePathAsync(fileName.ParentId, fileName.Name);
                var checkExists = await FileSystem.Current.LocalStorage.CheckExistsAsync(localPath);

                if (checkExists == ExistenceCheckResult.FileExists) {
                    var file = await FileSystem.Current.LocalStorage.GetFileAsync(localPath);
                    await file.DeleteAsync();
                }
            }
        }

###<a name="a-namefile-sync-handlera-add-a-file-sync-handler"></a><a name="file-sync-handler"></a>Hinzufügen einer synchronisieren Dateihandler

Erstellen Sie eine neue Klasse `TodoItemFileSyncHandler` im Hauptfenster portable Library-Projekt. Diese Klasse enthält Rückrufe aus dem Azure SDK von Code zu benachrichtigen, wenn eine Datei hinzugefügt oder entfernt wird.

Azure Mobile-Client-SDK keine gespeichert beliebige Daten: der Client SDK ruft Ihre Implementierung von `IFileSyncHandler` der wiederum bestimmt, ob und wie Dateien auf dem lokalen Gerät gespeichert sind.

1. Fügen Sie den folgenden Anweisungen verwenden:

        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Xamarin.Forms;

2. Ersetzen Sie die Definition der Klasse durch Folgendes ein: 

        public class TodoItemFileSyncHandler : IFileSyncHandler
        {
            private readonly TodoItemManager todoItemManager;

            public TodoItemFileSyncHandler(TodoItemManager itemManager)
            {
                this.todoItemManager = itemManager;
            }

            public Task<IMobileServiceFileDataSource> GetDataSource(MobileServiceFileMetadata metadata)
            {
                IPlatform platform = DependencyService.Get<IPlatform>();
                return platform.GetFileDataSource(metadata);
            }

            public async Task ProcessFileSynchronizationAction(MobileServiceFile file, FileSynchronizationAction action)
            {
                if (action == FileSynchronizationAction.Delete) {
                    await FileHelper.DeleteLocalFileAsync(file);
                }
                else { // Create or update. We're aggressively downloading all files.
                    await this.todoItemManager.DownloadFileAsync(file);
                }
            }
        }

###<a name="a-nameupdate-todoitemmanageraupdate-todoitemmanager"></a><a name="update-todoitemmanager"></a>Aktualisieren von TodoItemManager

1. Kommentieren Sie die Zeile in **TodoItemManager.cs**, `#define OFFLINE_SYNC_ENABLED`.

2. In **TodoItemManager.cs**, fügen Sie den folgenden Anweisungen verwenden:

        using System.IO;
        using Xamarin.Forms;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Eventing;

3. Im Konstruktor der `TodoItemManager`, fügen Sie Folgendes nach den Anruf an `DefineTable()`:

        // Initialize file sync
        this.client.InitializeFileSyncContext(new TodoItemFileSyncHandler(this), store);

4. Ersetzen Sie den Anruf an die im Konstruktor `InitializeAsync` mit den folgenden. Dadurch wird sichergestellt, dass Rückrufe vorhanden sind, wenn Datensätze in den lokalen Speicher geändert werden. Das Feature zur Synchronisierung verwendet diese Rückrufe, um Ihre synchronisieren Dateihandler auslösen.

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

5. In `SyncAsync()`, fügen Sie Folgendes nach den Anruf an `PushAsync()`:

        await this.todoTable.PushFileChangesAsync();

6. Fügen Sie die folgenden Methoden zu `TodoItemManager`:

        internal async Task DownloadFileAsync(MobileServiceFile file)
        {
            var todoItem = await todoTable.LookupAsync(file.ParentId);
            IPlatform platform = DependencyService.Get<IPlatform>();

            string filePath = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name); 
            await platform.DownloadFileAsync(this.todoTable, file, filePath);
        }

        internal async Task<MobileServiceFile> AddImage(TodoItem todoItem, string imagePath)
        {
            string targetPath = await FileHelper.CopyTodoItemFileAsync(todoItem.Id, imagePath);
            return await this.todoTable.AddFileAsync(todoItem, Path.GetFileName(targetPath));
        }

        internal async Task DeleteImage(TodoItem todoItem, MobileServiceFile file)
        {
            await this.todoTable.DeleteFileAsync(file);
        }

        internal async Task<IEnumerable<MobileServiceFile>> GetImageFilesAsync(TodoItem todoItem)
        {
            return await this.todoTable.GetFilesAsync(todoItem);
        }

###<a name="a-nameadd-details-viewaadd-a-details-view"></a><a name="add-details-view"></a>Fügen Sie eine Detailansicht hinzu.

In diesem Abschnitt fügen Sie eine neue Detailansicht für ein Element erledigen. Die Ansicht wird erstellt, wenn der Benutzer ein Element erledigen wählt und dies zulässt neue Bilder zu einem Element hinzugefügt werden.

1. Fügen Sie eine neue Klasse **TodoItemImage** zum Bibliotheksprojekt tragbares mit der folgenden Implementierung ein:

        public class TodoItemImage : INotifyPropertyChanged
        {
            private string name;
            private string uri;

            public MobileServiceFile File { get; private set; }

            public string Name
            {
                get { return name; }
                set
                {
                    name = value;
                    OnPropertyChanged(nameof(Name));
                }
            }

            public string Uri
            {
                get { return uri; }      
                set
                {
                    uri = value;
                    OnPropertyChanged(nameof(Uri));
                }
            }

            public TodoItemImage(MobileServiceFile file, TodoItem todoItem)
            {
                Name = file.Name;
                File = file;

                FileHelper.GetLocalFilePathAsync(todoItem.Id, file.Name).ContinueWith(x => this.Uri = x.Result);
            }

            public event PropertyChangedEventHandler PropertyChanged;

            private void OnPropertyChanged(string propertyName)
            {
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
            }
        }

2. Bearbeiten Sie **App.cs**. Ersetzen Sie die Fehler bei der Initialisierung `MainPage` mit den folgenden:
    
        MainPage = new NavigationPage(new TodoList());

3. Fügen Sie in **App.cs**die folgende Eigenschaft hinzu:

        public static object UIContext { get; set; }

4. Mit der rechten Maustaste in des Projekts tragbares Bibliothek, und wählen Sie **Hinzufügen** -> **Neues Element** -> **Plattformen** -> **Formulare XAML-Seite**. Benennen Sie die Ansicht `TodoItemDetailsView`.

5. Öffnen Sie **TodoItemDetailsView.xaml** und Ersetzen Sie den Hauptteil der ContentPage verwendet wird mit folgenden:

          <Grid>
            <Grid.RowDefinitions>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="Auto"/>
              <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            <Button Clicked="OnAdd" Text="Add image"></Button>
            <ListView x:Name="imagesList"
                      ItemsSource="{Binding Images}"
                      IsPullToRefreshEnabled="false"
                      Grid.Row="2">
              <ListView.ItemTemplate>
                <DataTemplate>
                  <ImageCell ImageSource="{Binding Uri}"
                             Text="{Binding Name}">
                  </ImageCell>
                </DataTemplate>
              </ListView.ItemTemplate>
            </ListView>
          </Grid>

6. Bearbeiten von **TodoItemDetailsView.xaml.cs** , und fügen Sie den folgenden Anweisungen verwenden:

        using System.Collections.ObjectModel;
        using Microsoft.WindowsAzure.MobileServices.Files;

7. Ersetzen Sie die Implementierung von `TodoItemDetailsView` mit den folgenden:

        public partial class TodoItemDetailsView : ContentPage
        {
            private TodoItemManager manager;

            public TodoItem TodoItem { get; set; }        
            public ObservableCollection<TodoItemImage> Images { get; set; }

            public TodoItemDetailsView(TodoItem todoItem, TodoItemManager manager)
            {
                InitializeComponent();
                this.Title = todoItem.Name;

                this.TodoItem = todoItem;
                this.manager = manager;

                this.Images = new ObservableCollection<TodoItemImage>();
                this.BindingContext = this;
            }

            public async Task LoadImagesAsync()
            {
                IEnumerable<MobileServiceFile> files = await this.manager.GetImageFilesAsync(TodoItem);
                this.Images.Clear();

                foreach (var f in files) {
                    var todoImage = new TodoItemImage(f, this.TodoItem);
                    this.Images.Add(todoImage);
                }
            }

            public async void OnAdd(object sender, EventArgs e)
            {
                IPlatform mediaProvider = DependencyService.Get<IPlatform>();
                string sourceImagePath = await mediaProvider.TakePhotoAsync(App.UIContext);

                if (sourceImagePath != null) {
                    MobileServiceFile file = await this.manager.AddImage(this.TodoItem, sourceImagePath);

                    var image = new TodoItemImage(file, this.TodoItem);
                    this.Images.Add(image);
                }
            }
        }

###<a name="a-nameupdate-main-viewaupdate-the-main-view"></a><a name="update-main-view"></a>Aktualisieren Sie im Hauptfenster anzeigen 

Aktualisieren Sie die Hauptfenster Ansicht aus, um die Detailansicht zu öffnen, wenn ein Element erledigen ausgewählt ist.

Ersetzen Sie die Implementierung von **TodoList.xaml.cs**, `OnSelected` mit den folgenden:

    public async void OnSelected(object sender, SelectedItemChangedEventArgs e)
    {
        var todo = e.SelectedItem as TodoItem;

        if (todo != null) {
            var detailsView = new TodoItemDetailsView(todo, manager);
            await detailsView.LoadImagesAsync();
            await Navigation.PushAsync(detailsView);
        }

        todoList.SelectedItem = null;
    }

###<a name="a-nameupdate-androidaupdate-the-android-project"></a><a name="update-android"></a>Aktualisieren Sie das Projekt Android

Hinzufügen von Plattform-spezifischen Code zum Android Projekt, einschließlich Code für Herunterladen einer Datei, und verwenden die Kamera, um ein neues Bild zu erfassen. 

Dieser Code verwendet die Xamarin.Forms [DependencyService](https://developer.xamarin.com/guides/xamarin-forms/dependency-service/) , die rechts-Plattform-spezifische Klasse zur Laufzeit laden.

1. Fügen Sie dem Projekt Android Komponente **Xamarin.Mobile** ein.

2. Fügen Sie eine neue Klasse `DroidPlatform` mit den folgenden Implementierung. Ersetzen Sie "YourNamespace" mit dem Hauptfenster Namespace des Projekts.

        using System;
        using System.IO;
        using System.Threading.Tasks;
        using Android.Content;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.Droid.DroidPlatform))]
        namespace YourNamespace.Droid
        {
            public class DroidPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var uiContext = context as Context;
                        if (uiContext != null) {
                            var mediaPicker = new MediaPicker(uiContext);
                            var photo = await mediaPicker.TakePhotoAsync(new StoreCameraMediaOptions());

                            return photo.Path;
                        }
                    }
                    catch (TaskCanceledException) {
                    }

                    return null;
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string appData = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
                    string filesPath = Path.Combine(appData, "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. Bearbeiten von **MainActivity.cs**. In `OnCreate`, fügen Sie die folgenden Punkte, bevor der Anruf an `LoadApplication()`:

        App.UIContext = this;

###<a name="a-nameupdate-iosaupdate-the-ios-project"></a><a name="update-ios"></a>Aktualisieren Sie das Projekt iOS

Hinzufügen von Plattform-spezifischen Code zum Projekt iOS.

1. Fügen Sie dem Projekt iOS Komponente **Xamarin.Mobile** ein.

2. Fügen Sie eine neue Klasse `TouchPlatform` mit den folgenden Implementierung. Ersetzen Sie "YourNamespace" mit dem Hauptfenster Namespace des Projekts.

        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Xamarin.Media;

        [assembly: Xamarin.Forms.Dependency(typeof(YourNamespace.iOS.TouchPlatform))]
        namespace YourNamespace.iOS
        {
            class TouchPlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        var mediaPicker = new MediaPicker();
                        var mediaFile = await mediaPicker.PickPhotoAsync();
                        return mediaFile.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }

                public Task<string> GetTodoFilesPathAsync()
                {
                    string filesPath = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments), "TodoItemFiles");

                    if (!Directory.Exists(filesPath)) {
                        Directory.CreateDirectory(filesPath);
                    }

                    return Task.FromResult(filesPath);
                }
            }
        }

3. Bearbeiten von **AppDelegate.cs** und entfernen Sie die Kommentarzeichen des Anrufs an die `SQLitePCL.CurrentPlatform.Init()`.

###<a name="a-nameupdate-windowsaupdate-the-windows-project"></a><a name="update-windows"></a>Aktualisieren des Windows-Projekts

1. Installieren der Visual Studio-Erweiterung [SQLite für Windows 8.1](http://go.microsoft.com/fwlink/?LinkID=716919). Weitere Informationen finden Sie unter des Lernprogramms [offlinesynchronisierung für Ihre Windows-app zu aktivieren](app-service-mobile-windows-store-dotnet-get-started-offline-data.md). 

2. Bearbeiten Sie **Package.appxmanifest** , und aktivieren Sie die **Webcam** -Funktionalität.

3. Fügen Sie eine neue Klasse `WindowsStorePlatform` mit den folgenden Implementierung. Ersetzen Sie "YourNamespace" mit dem Hauptfenster Namespace des Projekts.

        using System;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MobileServices.Files;
        using Microsoft.WindowsAzure.MobileServices.Files.Metadata;
        using Microsoft.WindowsAzure.MobileServices.Files.Sync;
        using Microsoft.WindowsAzure.MobileServices.Sync;
        using Windows.Foundation;
        using Windows.Media.Capture;
        using Windows.Storage;
        using YourNamespace;

        [assembly: Xamarin.Forms.Dependency(typeof(WinApp.WindowsStorePlatform))]
        namespace WinApp
        {
            public class WindowsStorePlatform : IPlatform
            {
                public async Task DownloadFileAsync<T>(IMobileServiceSyncTable<T> table, MobileServiceFile file, string filename)
                {
                    var path = await FileHelper.GetLocalFilePathAsync(file.ParentId, file.Name);
                    await table.DownloadFileAsync(file, path);
                }

                public async Task<IMobileServiceFileDataSource> GetFileDataSource(MobileServiceFileMetadata metadata)
                {
                    var filePath = await FileHelper.GetLocalFilePathAsync(metadata.ParentDataItemId, metadata.FileName);
                    return new PathMobileServiceFileDataSource(filePath);
                }

                public async Task<string> GetTodoFilesPathAsync()
                {
                    var storageFolder = ApplicationData.Current.LocalFolder;
                    var filePath = "TodoItemFiles";

                    var result = await storageFolder.TryGetItemAsync(filePath);

                    if (result == null) {
                        result = await storageFolder.CreateFolderAsync(filePath);
                    }

                    return result.Name; // later operations will use relative paths
                }

                public async Task<string> TakePhotoAsync(object context)
                {
                    try {
                        CameraCaptureUI dialog = new CameraCaptureUI();
                        Size aspectRatio = new Size(16, 9);
                        dialog.PhotoSettings.CroppedAspectRatio = aspectRatio;

                        StorageFile file = await dialog.CaptureFileAsync(CameraCaptureUIMode.Photo);
                        return file.Path;
                    }
                    catch (TaskCanceledException) {
                        return null;
                    }
                }
            }
        }

##<a name="summary"></a>Zusammenfassung

In diesem Artikel beschrieben, wie Sie die neue Datei Unterstützung in Azure Mobile Client und Server SDK für die Arbeit mit Azure-Speicher verwenden. 

- Erstellen Sie ein Speicherkonto und fügen Sie die Verbindungszeichenfolge hinzu, um Ihre mobile-app Back-End. Nur die Back-End-die EINGABETASTE, um Azure-Speicher hat: des mobile Clients anfordert ein SAS Token (Access-Signatur freigegeben) aus, wenn es auf Azure-Speicher zugreifen muss. Weitere Informationen zu SAS Token in Azure-Speicher finden Sie unter [Grundlegendes zu freigegebenen Access Signaturen].

- Erstellen Sie einen Controller die untergeordneten Klassen `StorageController` akzeptieren, um die SAS token Anfragen zu behandeln sowie die Dateien, die einem Datensatz zugeordnet sind. Standardmäßig sind Dateien mithilfe der Datensatz-ID als Teil des Namens Container einem Datensatz zugeordnet. das Verhalten kann angepasst werden, indem Sie eine Implementierung von `IContainerNameResolver`. Die token SAS-Richtlinie kann auch angepasst werden.

- Speichern des Azure Mobile Client SDK keine Dateidaten gespeichert. Ruft SDK eher den Kunden Ihre `IFileSyncHandler`, die dann beschließt wie (und ob) Dateien auf dem lokalen Gerät gespeichert sind. Synchronisieren ist wie folgt registriert:

        client.InitializeFileSync(new MyFileSyncHandler(), store);

      + `IFileSyncHandler.GetDataSource`wird aufgerufen, wenn das Azure Mobile Client SDK die Dateidaten (z. B. als Teil des Hochladens) benötigt. Das ermöglicht Ihnen die Möglichkeit zu verwalten, wie (und ob) Dateien werden auf dem lokalen Gerät gespeichert und diese Informationen bei Bedarf zurückzukehren.

      + `IFileSyncHandler.ProcessFileSynchronizationAction`wird als Teil der Datei Synchronisierung Fluss aufgerufen. Einen Dateiverweis und ein Wert der Enumeration FileSynchronizationAction werden bereitgestellt, damit Sie entscheiden können, wie eine Anwendung behandelt werden sollen, Ereignis (z. B. automatisch herunterladen einer Datei, wenn es erstellt oder aktualisiert wird, eine Datei aus dem lokalen Gerät löschen, wenn die Datei auf dem Server gelöscht wird).

- A `MobileServiceFile` können werden verwendet, entweder im Online- / Offlinemodus im Modus mithilfe einer `IMobileServiceTable` oder `IMobileServiceSyncTable`Hilfethemas. Im offline-Szenario der Upload tritt auf, wenn die app ruft `PushFileChangesAsync`. Dadurch wird die Offlinebetrieb Warteschlange verarbeitet werden; für jeden Dateivorgang der Azure-Mobilclient SDK Ruft die `GetDataSource` Methode für die `IFileSyncHandler` Instanz zum Abrufen des Inhalts der Datei zum Hochladen.

- Um ein Element den Dateien abzurufen, rufen Sie die '' GetFilesAsync` method on the  `IMobileServiceTable<T> ` or IMobileServiceSyncTable<T>` Instanz. Diese Methode gibt eine Liste der Dateien, die mit dem Datenelement verknüpft ist. (Notiz: Dies ist eine *lokale* Operation, und die Dateien basierend auf dem Status des Objekts, wenn es der letzten Synchronisierung zurückgegeben werden kann. Um eine aktualisierte Liste der Dateien vom Server zu erhalten, sollten Sie synchronisieren zuerst das Initiieren eines Vorgangs.)

        IEnumerable<MobileServiceFile> files = await myTable.GetFilesAsync(myItem);

- Das Feature zur Synchronisierung verwendet Datensatz ändern Benachrichtigungen auf den lokalen Speicher akzeptieren, um die Datensätze abzurufen, die der Client als Teil eines Vorgangs Pushbenachrichtigungen oder Abruf empfangen. Dies ist durch Umwandlung lokale und Benachrichtigungen für die Synchronisierung Kontext mithilfe der `StoreTrackingOptions` Parameter. 

        this.client.SyncContext.InitializeAsync(store, StoreTrackingOptions.NotifyLocalAndServerOperations);

      + Andere Optionen zur Speicherung Verlauf sind verfügbar, z. B. nur lokale oder nur für Benachrichtigungen. Können Sie hinzufügen oder Besitzer der benutzerdefinierten Rückruf mit der `EventManager` Eigenschaft `IMobileServiceClient`:

            jobService.MobileService.EventManager.Subscribe<StoreOperationCompletedEvent>(StoreOperationEventHandler);

- Es ist möglich, hinzufügen oder Entfernen von Dateien aus einem Datensatz durch Blob-Speicher direkt ändern, die Zuordnung über eine Benennungskonvention erzielt wird. Allerdings müssen in diesem Fall Sie immer **aktualisieren den Eintrag Zeitstempel, wenn die zugehörigen Blobs geändert werden**. Azure-Mobilclient SDK aktualisiert immer einen Eintrag beim Hinzufügen oder Entfernen einer Datei aus. 

    Der Grund für diese Anforderung ist, dass einige mobilen Clients im lokalen Speicher bereits den Eintrag verfügt. Wenn diese Clients eine inkrementell Abruf ausführen, diesen Eintrag nicht zurückgegeben wird, und der Client wird keine Abfrage für die neuen zugehörigen Dateien. Um dieses Problem zu vermeiden, empfiehlt es sich, dass Sie den Datensatz Zeitstempel Aktualisieren beim Durchführen von BLOB-Speicher geändert, die nicht der Azure-Mobilclient SDK verwendet.

- Das Clientprojekt mithilfe des Musters [Xamarin.Forms DependencyService] um die rechts-Plattform-spezifische Klasse zur Laufzeit zu laden. In diesem Beispiel wird eine Schnittstelle definiert `IPlatform` mit Implementierungen in jedes der Plattform-spezifische Projekte.

<!-- URLs. -->

[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
[Erstellen Sie eine app Xamarin.Forms]: app-service-mobile-xamarin-forms-get-started.md
[Xamarin.Forms DependencyService]: https://developer.xamarin.com/guides/xamarin-forms/dependency-service/
[Microsoft.Azure.Mobile.Client.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.Files/
[Microsoft.Azure.Mobile.Client.SQLiteStore]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[Microsoft.Azure.Mobile.Server.Files]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Files/
[Grundlegendes zu freigegebenen Access Signaturen]: ../storage/storage-dotnet-shared-access-signature-part-1.md
[Erstellen Sie ein Konto Azure-Speicher]:  ../storage/storage-create-storage-account.md#create-a-storage-account
