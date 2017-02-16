<properties
    pageTitle="Verwenden Sie die Benachrichtigung Hubs auf dem neusten Stand (Windows Phone) senden"
    description="Verwenden von Azure Benachrichtigung Hubs Kategorie in Registrierungen verwenden, um dem neusten Stand zu einer app für Windows Phone zu senden."
    services="notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Verwenden Sie die Benachrichtigung Hubs auf dem neusten Stand zu senden

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>(Übersicht)

In diesem Thema wird gezeigt, wie mit Azure Benachrichtigung Hubs abgeschnitten werden News Benachrichtigungen zu einer app für Windows Phone 8.0/8.1 Silverlight übertragen werden können. Wenn Sie Windows Store oder 8.1 für Windows Phone-app verwenden möchten, Lizenzinformationen finden Sie auf die Version [Windows Universal](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) . Wenn Sie fertig sind, werden Sie Lage, melden Sie sich für die Bruchfestigkeit News-Kategorien, die, denen Sie interessiert sind, und erhalten nur Pushbenachrichtigungen für diese Kategorien. Dieses Szenario ist ein gemeinsames Muster für viele apps haben, in dem Benachrichtigungen des Planungsprozesses gesendet werden, die zuvor Interesse können, z. B. RSS-Reader, apps für Musik Lüfter usw. deklariert haben.

Übertragenen Szenarien aktiviert wird, indem Sie ein oder mehrere _Kategorien_ einschließlich, wenn eine Registrierung in der Benachrichtigung Hub zu erstellen. Beim Senden von Benachrichtigungen zu einer Kategorie, alle Geräte, die registriert haben, für die Kategorie die Benachrichtigung erhalten. Da Kategorien einfach Zeichenfolgen werden, müssen sie keinen im Voraus bereitgestellt werden. Weitere Informationen zu Kategorien finden Sie in der [Benachrichtigung Hubs Routing und Kategorie Ausdrücke](notification-hubs-tags-segment-push-message.md).

##<a name="prerequisites"></a>Erforderliche Komponenten

In diesem Thema basiert auf die app, die Sie in die [Erste Schritte mit Benachrichtigung Hubs]erstellt haben. Bevor Sie beginnen in diesem Lernprogramm, müssen bereits [Erste Schritte mit Benachrichtigung Hubs]abgeschlossen sein.

##<a name="add-category-selection-to-the-app"></a>Hinzufügen von Kategorieauswahl zur app

Dieser erste Schritt besteht hinzufügen die Elemente der Benutzeroberfläche Ihrer vorhandenen Hauptseite, mit denen den Benutzer auswählen von Kategorien zu registrieren. Die Kategorien, die von einem Benutzer ausgewählt werden auf dem Gerät gespeichert. Wenn die app gestartet wird, wird eine Registrierung Gerät in der Benachrichtigung Hub mit den ausgewählten Kategorien als Kategorien erstellt.

1. Die MainPage.xaml Projektdatei öffnen, und klicken Sie dann im **Raster** Elemente mit dem Namen ersetzen `TitlePanel` und `ContentPanel` mit den folgenden Code:

        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>

        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>

2. Erstellen Sie im Projekt eine neue Klasse namens **Benachrichtigungen**, fügen Sie den **öffentlichen** Modifizierer für die Definition der Klasse, und dann fügen Sie die folgenden Aussagen zur **Verwendung** in die neue Codedatei hinzu:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;

3. Kopieren Sie den folgenden Code in die neue **Benachrichtigungen** Klasse ein:

        private NotificationHub hub;

        // Registration task to complete registration in the ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;

        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }

        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }

        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();

            return await SubscribeToCategories();
        }

        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();

            var channel = HttpNotificationChannel.Find("MyPushChannel");

            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;

                // This is optional, used to receive notifications while the app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }

            // If channel.ChannelUri is not null, we will complete the registrationTask here.  
            // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
            
            return await registrationTask.Task;
        }

        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }

        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.

            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";

            // The stored categories tags are passed with the template registration.

            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));

            return await registrationTask.Task;
        }

        // This is optional. It is used to receive notifications while the app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;

            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());

            // Parse out the information that was part of the message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);

                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }

            // Display a dialog of all the fields in the toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }


    Diese Klasse verwendet die isoliert Datenspeicher zum Speichern Sie die Kategorien der News verwendet, dieses Gerät erhalten. Darüber hinaus enthält Methoden für diese Kategorien mithilfe einer [Vorlage](notification-hubs-templates-cross-platform-push-messages.md) Benachrichtigung Registrierung registrieren.


4. Fügen Sie die folgende Eigenschaft in der Projektdatei App.xaml.cs um die **App** -Klasse. Ersetzen der `<hub name>` und `<connection string with listen access>` Platzhalter mit Ihren Benachrichtigung Hubnamen und die Verbindungszeichenfolge für *DefaultListenSharedAccessSignature* , die Sie zuvor für Ihren Kunden.

        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");

    > [AZURE.NOTE] Da Anmeldeinformationen, die mit einer Clientanwendung verteilt werden nicht in der Regel sicher sind, sollten Sie nur die Taste für Listen Access Client-App verteilen. Abhören von Access ermöglicht, die Ihre app registrieren für Benachrichtigungen, aber vorhandene Registrierungen geändert werden kann, und nicht die Benachrichtigungen gesendet werden. Die vollständige Zugriffstaste wird in einem gesicherten Back-End-Dienst für Benachrichtigungen senden, und ändern die vorhandene Registrierungen verwendet.

5. Fügen Sie in Ihrem MainPage.xaml.cs die folgende Zeile hinzu:

        using Windows.UI.Popups;

6. Fügen Sie in der Projektdatei MainPage.xaml.cs die folgende Methode:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
    
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
    
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }

    Diese Methode erstellt eine Liste der Kategorien und verwendet die **Benachrichtigungen** -Klasse zum Speichern der Liste im lokalen Speicher und die entsprechenden Tags mit Ihrem Hub Benachrichtigung registrieren. Wenn Kategorien geändert werden, wird die Registrierung mit den neuen Kategorien neu erstellt werden.

Ihre app kann nun eine Reihe von Kategorien im lokalen Speicher auf dem Gerät gespeichert und mit dem Hub Benachrichtigung registrieren, wenn der Benutzer die Auswahl der Kategorien ändert.

##<a name="register-for-notifications"></a>Registrieren für Benachrichtigungen

Diese Schritte registrieren Sie sich mit dem Hub Benachrichtigung beim Start mit Hilfe der Kategorien, die im lokalen Speicher gespeichert wurden.

> [AZURE.NOTE] Da der Channel-URI zugewiesen von Microsoft Pushbenachrichtigungen Benachrichtigung Service (MPNS) zu einem beliebigen Zeitpunkt ändern kann, sollten Sie für Benachrichtigung Fehler vermeiden, häufig die Benachrichtigungen registrieren. In diesem Beispiel registriert für Benachrichtigungen jedes Mal, die die app gestartet wird. Für apps, die häufig ausgeführt werden, können mehr als einmal pro Tag, Sie wahrscheinlich überspringen Registrierung um Bandbreite zu sparen, wenn Sie weniger als ein Tag nach der vorherigen Registrierung verstrichen ist.


1. Öffnen Sie die Datei App.xaml.cs, fügen Sie den **asynchronen** Modifizierer **Application_Launching** Methode, und Ersetzen Sie die Benachrichtigung Hubs Registrierungscode, die Sie mit den folgenden Code in die [Erste Schritte mit Benachrichtigung Hubs] hinzugefügt:

        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();

            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }

    Dadurch wird sichergestellt, dass jedes Mal, wenn die app gestartet Kategorien aus dem lokalen Speicher abgerufen und eine Registrierung für diese Kategorien anfordert.

2. Fügen Sie in der Projektdatei MainPage.xaml.cs den folgenden Code, der die **OnNavigatedTo** -Methode implementiert hinzu:

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();

            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
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

    ![][2]

3. Führen Sie nach Erhalt einer Bestätigungs, dass Ihre Kategorien Abonnement durchgeführt wurden, die Console-app zum Senden von Benachrichtigungen für jede Kategorien aus. Stellen Sie sicher, dass Sie nur eine Benachrichtigung für die Kategorien erhalten, die Sie abonniert haben.

    ![][3]

In diesem Thema abgeschlossen haben.

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Erste Schritte mit Benachrichtigung Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

