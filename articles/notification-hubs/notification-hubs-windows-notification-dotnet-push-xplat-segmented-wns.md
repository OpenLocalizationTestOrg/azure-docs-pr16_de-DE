<properties
    pageTitle="Verwenden Sie Hubs Benachrichtigung zum Senden von dem neusten Stand (Universal Windows)"
    description="Verwenden von Azure Benachrichtigung Hubs mit Kategorien in der Registrierung auf um dem neusten Stand zu einer universal Windows-app zu senden."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>


<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Verwenden Sie die Benachrichtigung Hubs auf dem neusten Stand zu senden


[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>(Übersicht)

In diesem Thema wird gezeigt, wie mit Azure Benachrichtigung Hubs abgeschnitten werden News Benachrichtigungen zu einem Windows Store oder die app für Windows Phone 8.1 (nicht-Silverlight) übertragen werden kann. Wenn Sie Silverlight für Windows Phone 8.1 verwenden möchten, finden Sie in der [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) -Version. Wenn Sie fertig sind, werden Sie Lage, melden Sie sich für die Bruchfestigkeit News-Kategorien, die, denen Sie interessiert sind, und erhalten nur Pushbenachrichtigungen für diese Kategorien. Dieses Szenario ist ein gemeinsames Muster für viele apps haben, in dem Gruppen Benutzer gesendet werden, die zuvor deklariert Interesse können, z. B. RSS-Reader, apps für Musik Lüfter, usw. Benachrichtigungen. 

Übertragenen Szenarien aktiviert wird, indem Sie ein oder mehrere _Kategorien_ einschließlich, wenn eine Registrierung in der Benachrichtigung Hub zu erstellen. Beim Senden von Benachrichtigungen zu einer Kategorie, alle Geräte, die registriert haben, für die Kategorie die Benachrichtigung erhalten. Da Kategorien einfach Zeichenfolgen werden, müssen sie keinen im Voraus bereitgestellt werden. Weitere Informationen zu Kategorien finden Sie in der [Benachrichtigung Hubs Routing und Kategorie Ausdrücke](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Erforderliche Komponenten

In diesem Thema erstellt wird, klicken Sie auf die app, die Sie erstellt, in die [Erste Schritte mit Benachrichtigung Hubs haben][get-started]. Bevor Sie beginnen in diesem Lernprogramm, muss bereits abgeschlossen haben [Erste Schritte mit Benachrichtigung Hubs][get-started].

##<a name="add-category-selection-to-the-app"></a>Hinzufügen von Kategorieauswahl zur app

Dieser erste Schritt besteht hinzufügen die Elemente der Benutzeroberfläche Ihrer vorhandenen Hauptseite, mit denen den Benutzer auswählen von Kategorien zu registrieren. Die Kategorien, die von einem Benutzer ausgewählt werden auf dem Gerät gespeichert. Wenn die app gestartet wird, wird eine Registrierung Gerät in der Benachrichtigung Hub mit den ausgewählten Kategorien als Kategorien erstellt.

1. Die MainPage.xaml Projektdatei öffnen, und klicken Sie dann den folgenden Code in das **Raster** Element kopieren:

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>


2. Klicken Sie mit der rechten Maustaste im Projekt **freigegeben** und fügen Sie eine neue Klasse mit dem Namen **Benachrichtigungen**, den **public** -Modifizierer für die Definition der Klasse und dann fügen Sie die folgenden Aussagen zur **Verwendung** in die neue Codedatei hinzu:

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. Kopieren Sie den folgenden Code in die neue **Benachrichtigungen** Klasse ein:

        private NotificationHub hub;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }

        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }

    Diese Klasse verwendet den lokalen Speicher, um die Kategorien der Informationen zu speichern, dieses Gerät hat, erhalten. Beachten Sie, dass statt der *RegisterNativeAsync* Methode wir *RegisterTemplateAsync* für die Kategorien mithilfe einer Vorlage Registrierung registrieren anrufen. 
    
    Wir geben Sie einen Namen für die Vorlage ("SimpleWNSTemplateExample"), da wir möglicherweise mehr als eine Vorlage (beispielsweise eine Spruch Benachrichtigungen) und eine für die Kacheln erfassen möchten, und wir müssen akzeptieren, um in der Lage, aktualisieren oder löschen sie einen Namen eingeben.

    Beachten Sie, dass, wenn ein Gerät mit dem gleichen Tag mehrere Vorlagen registriert, eine eingehende Nachricht verwendet, die mehrere Benachrichtigungen Kategorie führt am Gerät (eine für jede Vorlage) übermittelt. Dieses Verhalten ist nützlich, wenn muss dieselbe logische Meldung mehrere visuelle Benachrichtigungen, z. B. Seitennavigationsbereich mit einen Badge und eine Spruch in einer Windows Store-Anwendung zur Folge haben.

    Weitere Informationen zu Vorlagen finden Sie unter [Vorlagen](notification-hubs-templates-cross-platform-push-messages.md).




4. Fügen Sie die folgende Eigenschaft in der Projektdatei App.xaml.cs um die **App** -Klasse:

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    Diese Eigenschaft wird verwendet, zu erstellen und eine Instanz von **Benachrichtigungen** zugreifen.

    Ersetzen Sie im obigen Code, der `<hub name>` und `<connection string with listen access>` Platzhalter mit Ihren Benachrichtigung Hubnamen und die Verbindungszeichenfolge für *DefaultListenSharedAccessSignature* , die Sie zuvor für Ihren Kunden.

    > [AZURE.NOTE] Da Anmeldeinformationen, die mit einer Clientanwendung verteilt werden nicht in der Regel sicher sind, sollten Sie nur die Taste für Listen Access Client-App verteilen. Abhören von Access ermöglicht, die Ihre app registrieren für Benachrichtigungen, aber vorhandene Registrierungen geändert werden kann, und nicht die Benachrichtigungen gesendet werden. Die vollständige Zugriffstaste wird in einem gesicherten Back-End-Dienst für Benachrichtigungen senden, und ändern die vorhandene Registrierungen verwendet.

5. Fügen Sie in Ihrem MainPage.xaml.cs die folgende Zeile hinzu:

        using Windows.UI.Popups;

6. Fügen Sie in der Projektdatei MainPage.xaml.cs die folgende Methode:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);

            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }

    Diese Methode erstellt eine Liste der Kategorien und verwendet die **Benachrichtigungen** -Klasse zum Speichern der Liste im lokalen Speicher und die entsprechenden Tags mit Ihrem Hub Benachrichtigung registrieren. Wenn Kategorien geändert werden, wird die Registrierung mit den neuen Kategorien neu erstellt werden.

Ihre app kann nun eine Reihe von Kategorien im lokalen Speicher auf dem Gerät gespeichert und mit dem Hub Benachrichtigung registrieren, wenn der Benutzer die Auswahl der Kategorien ändert.

##<a name="register-for-notifications"></a>Registrieren für Benachrichtigungen

Diese Schritte registrieren Sie sich mit dem Hub Benachrichtigung beim Start mit Hilfe der Kategorien, die im lokalen Speicher gespeichert wurden.

> [AZURE.NOTE] Da der Channel-URI zugewiesen von Windows Benachrichtigung Service (WNS) zu einem beliebigen Zeitpunkt ändern kann, sollten Sie für Benachrichtigung Fehler vermeiden, häufig die Benachrichtigungen registrieren. In diesem Beispiel registriert für Benachrichtigung jedes Mal, die die app gestartet wird. Für apps, die häufig ausgeführt werden, können mehr als einmal pro Tag, Sie wahrscheinlich überspringen Registrierung um Bandbreite zu sparen, wenn Sie weniger als ein Tag nach der vorherigen Registrierung verstrichen ist.

1. Öffnen Sie die Datei App.xaml.cs und aktualisieren Sie die zu verwendende Methode **InitNotificationsAsync** der `notifications` Klasse abonnieren basierend auf Kategorien.

        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
    
        var result = await notifications.SubscribeToCategories();

    Dadurch wird sichergestellt, dass jedes Mal, wenn die app gestartet Kategorien aus dem lokalen Speicher abgerufen und eine Registeration für diese Kategorien anfordert. Die Methode **InitNotificationsAsync** erstellt wurde, als Teil der [Erste Schritte mit Benachrichtigung Hubs] [ get-started] Lernprogramm.

3. Fügen Sie in der Projektdatei MainPage.xaml.cs den folgenden Code an die *OnNavigatedTo* -Methode hinzu:

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
        }

    Dadurch wird die Hauptseite basierend auf den Status des zuvor gespeicherte Kategorien aktualisiert.

Die app ist jetzt vollständig, und speichern eine Reihe von Kategorien kann in den lokalen Speicher des Geräts verwendet, um mit dem Hub Benachrichtigung registrieren, wenn der Benutzer die Auswahl der Kategorien ändert. Als Nächstes werden wir eine Back-End-definieren, die diese app Kategorie Benachrichtigungen senden können.

##<a name="sending-tagged-notifications"></a>Markierte Benachrichtigungen senden

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

##<a name="run-the-app-and-generate-notifications"></a>Führen Sie die app und Generieren von Benachrichtigungen

1. Drücken Sie F5 kompilieren und starten Sie die app in Visual Studio.

    ![][1]

    Beachten Sie, dass die app-Benutzeroberfläche eine Reihe von schaltet enthält, mit dem Sie wählen Sie die Kategorien zum Abonnieren aus.

2. Aktivieren eine oder mehrere Kategorien schaltet, klicken Sie dann auf **Abonnieren**.

    Die app wandelt die ausgewählten Kategorien in Kategorien und eine neue Gerät Registrierung für den ausgewählten Tags vom Hub Benachrichtigung anfordert. Die registrierten Kategorien werden zurückgegeben und in einem Dialogfeld angezeigt.

    ![][19]

4. Senden einer neuen Benachrichtigung über die Back-End-in eine der folgenden Methoden:

    + **Console-app:** starten Console-app.

    + **Java/PHP:** führen Sie Ihre app-Skript.

    Benachrichtigungen für den ausgewählten Kategorien werden als Spruch Benachrichtigungen angezeigt.

    ![][14]

##<a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben wir wie dem neusten Stand nach Kategorie zu übertragen. Erwägen Sie eine der folgenden Lernprogramme, die andere erweiterten Benachrichtigung Hubs Szenarien hervorheben durchführen:

+ [Verwenden Sie die Benachrichtigung Hubs auf dem neusten Stand lokalisierten übertragen]

    Informationen Sie zum Erweitern der abgeschnitten werden News app zum Senden lokalisierte Benachrichtigungen aktivieren.



<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /manage/services/notification-hubs/getting-started-windows-dotnet/
[Verwenden Sie die Benachrichtigung Hubs auf dem neusten Stand lokalisierten übertragen]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
