<properties
    pageTitle="So verwenden Sie BLOB-Speicher von Xamarin | Microsoft Azure"
    description="Der Azure-Speicher Client-Bibliothek für Xamarin ermöglicht Entwicklern iOS, Android und Windows Store-apps mit ihren systemeigenen Benutzeroberflächen zu erstellen. In diesem Lernprogramm erfahren, wie Sie Xamarin verwenden, um eine Anwendung zu erstellen, die Azure Blob-Speicher verwendet."
    services="storage"
    documentationCenter="xamarin"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/08/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-xamarin"></a>So verwenden Sie BLOB-Speicher von Xamarin

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## <a name="overview"></a>(Übersicht)

Xamarin ermöglicht Entwicklern das Verwenden eines freigegebenen C#-codebase um iOS, Android und Windows Store-apps mit ihren systemeigenen Benutzeroberflächen zu erstellen. In diesem Lernprogramm erfahren Sie, wie Azure Blob-Speicher mit einer Xamarin Anwendung verwendet. Wenn Sie weitere Informationen zur Azure-Speicher vor dem Einstieg in den Code möchten, finden Sie unter [Einführung in Microsoft Azure-Speicher](storage-introduction.md).

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Erstellen einer neuen Xamarin-Anwendung

Bei dieser erste Schritte, werden wir eine app erstellen, die auf Windows, iOS und Android verweisen. Diese app wird einfach einen Container erstellen und Hochladen einen Blob in diesem Container. Wir wird eine Version von Visual Studio unter Windows für die ersten Schritte, aber die gleichen Befunde können beim Erstellen einer app Xamarin Studio unter Mac OS angewendet werden.

Wie folgt vor, um eine Anwendung zu erstellen:

1. Wenn Sie nicht bereits geschehen, herunterladen und Installieren von [Xamarin für Visual Studio](https://www.xamarin.com/download).
2. Öffnen Sie Visual Studio, und erstellen Sie eine leere App (Native freigegeben): **Datei > Neu > Projekt > Plattformen > leere App(Native Shared)**.
3. Mit der rechten Maustaste in der Lösung im Explorer-Lösung, und wählen Sie **NuGet-Pakete verwalten, für die Lösung**. Suchen Sie nach **WindowsAzure.Storage** , und installieren Sie die neueste Version des unveränderliche für alle Projekte in Ihrer Lösung.
4. Erstellen Sie, und führen Sie Ihr Projekt.

Sie sollten jetzt eine Anwendung verfügen, mit dem Sie auf eine Schaltfläche klicken, die einen Indikator erhöht.

> [AZURE.NOTE] Der Azure-Speicher Client-Bibliothek für Xamarin unterstützt derzeit die folgenden Projekttypen: Native freigegeben, Xamarin.Forms freigegeben, Xamarin.Android und Xamarin.iOS.

## <a name="create-container-and-upload-blob"></a>Container erstellen und Blob hochladen

Als Nächstes werden Sie die freigegebene Klasse Teil des Codes hinzufügen `MyClass.cs` , die erstellt einen Container und wird einen Blob in diesem Container hochgeladen. `MyClass.cs`sollte wie folgt aussehen:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    using System.Threading.Tasks;

    namespace XamarinApp
    {
        public class MyClass
        {
            public MyClass ()
            {
            }

            public static async Task createContainerAndUpload()
            {
                // Retrieve storage account from connection string.
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

                // Create the blob client.
                CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

                // Retrieve reference to a previously created container.
                CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

                // Create the container if it doesn't already exist.
                await container.CreateIfNotExistsAsync();

                // Retrieve reference to a blob named "myblob".
                CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

                // Create the "myblob" blob with the text "Hello, world!"
                await blockBlob.UploadTextAsync("Hello, world!");
            }
        }
    }

Vergewissern Sie sich mit Ihrem tatsächlichen Kontonamen und kontoschlüssel "Your_account_name_here" und "Your_account_key_here" ersetzen. Anschließend können Sie diese freigegebenen Klasse in Ihrem iOS, Android und Windows Phone-Anwendung. Sie können einfach hinzufügen `MyClass.createContainerAndUpload()` auf jedes Projekt. Beispiel:

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

    using Android.App;
    using Android.Widget;
    using Android.OS;

    namespace XamarinApp.Droid
    {
        [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
        public class MainActivity : Activity
        {
            int count = 1;

            protected override async void OnCreate (Bundle bundle)
            {
                base.OnCreate (bundle);

                // Set our view from the "main" layout resource
                SetContentView (Resource.Layout.Main);

                // Get our button from the layout resource,
                // and attach an event to it
                Button button = FindViewById<Button> (Resource.Id.myButton);

                button.Click += delegate {
                    button.Text = string.Format ("{0} clicks!", count++);
                };

              await MyClass.createContainerAndUpload();
            }
        }
    }

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

    using System;
    using UIKit;

    namespace XamarinApp.iOS
    {
        public partial class ViewController : UIViewController
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading the view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.createContainerAndUpload();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc that aren't in use.
            }
        }
    }

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Navigation;

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=391641

    namespace XamarinApp.WinPhone
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about to be displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used to configure the page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling the hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using the NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.createContainerAndUpload();
            }
        }
    }


## <a name="run-the-application"></a>Führen Sie die Anwendung

Sie können diese Anwendung nun in einen Android oder Windows Phone-Emulator ausführen. Sie können auch diese Anwendung in einem iOS-Emulator ausführen, aber dies erfordert einen Mac Weitere Informationen zur Vorgehensweise lesen Sie der Dokumentation zum [Herstellen einer Verbindung zu einem Mac Visual Studio](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/)

Nachdem Sie Ihre app ausführen, erstellen Sie den Container `mycontainer` in Ihr Konto Speicher. Sollte das Blob enthalten `myblob`, die den Text enthält `Hello, world!`. Sie können dies überprüfen, indem Sie mit dem [Microsoft Azure-Speicher-Explorer](http://storageexplorer.com/).

## <a name="next-steps"></a>Nächste Schritte

Dieser erste Schritte, gelernt Sie eine Anwendung in Xamarin zu erstellen, Azure-Speicher verwendet. Dieser erste Schritte speziell Schwerpunkt auf ein Szenario im BLOB-Speicher. Sie können jedoch viel mehr mit, nicht nur BLOB-Speicher, sondern auch mit Tabelle, Datei- und Warteschlangenspeicher ausführen. Bitte checken Sie weitere den folgenden Artikeln:
- [Erste Schritte mit Azure BLOB-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md)
- [Erste Schritte mit Azure Table Storage mit .NET](storage-dotnet-how-to-use-tables.md)
- [Erste Schritte mit Azure Warteschlangenspeicher mit .NET](storage-dotnet-how-to-use-queues.md)
- [Erste Schritte mit Dateispeicher auf Windows Azure](storage-dotnet-how-to-use-files.md)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]
