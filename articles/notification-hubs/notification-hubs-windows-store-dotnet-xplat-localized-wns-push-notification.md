<properties
    pageTitle="Benachrichtigung Hubs lokalisiert abgeschnitten werden News Lernprogramm"
    description="Informationen Sie zum Azure Benachrichtigung Hubs verwenden, um lokalisierte abgeschnitten werden News Benachrichtigungen zu senden."
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

# <a name="use-notification-hubs-to-send-localized-breaking-news"></a>Verwenden Sie Benachrichtigung Hubs, um lokalisierte dem neusten Stand zu senden

> [AZURE.SELECTOR]
- [Windows Store c#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)

##<a name="overview"></a>(Übersicht)

In diesem Thema wird gezeigt, wie Sie mithilfe des Features " **Vorlage** " der Azure-Benachrichtigung Hubs abgeschnitten werden News Benachrichtigungen übertragen, die nach Sprache und Gerät lokalisiert wurden. In diesem Lernprogramm starten Sie mit der Windows Store-app erstellt in [Verwenden Benachrichtigung Hubs auf dem neusten Stand zu senden]. Wenn Sie fertig sind, werden Sie möglicherweise den registrieren Sie sich für Kategorien, die, denen Sie interessiert sind, geben Sie eine Sprache aus, in dem Sie die Benachrichtigungen erhalten, und erhalten nur Pushbenachrichtigungen für den ausgewählten Kategorien in dieser Sprache.


Es gibt zwei Komponenten dieses Szenario:

- Windows Store-app kann Client Geräte eine Sprache angeben, und Abonnieren von anderen abgeschnitten werden Newskategorien;

- die Back-End sendet die Benachrichtigungen, verwenden die **Kategorie** und **Vorlage** Feautres der Azure-Benachrichtigung Hubs.



##<a name="prerequisites"></a>Erforderliche Komponenten

Sie müssen bereits abgeschlossen haben das Lernprogramm [Verwenden Benachrichtigung Hubs auf dem neusten Stand zu senden] und den Code verfügbar, da dieses Lernprogramms direkt auf die Code basiert.

Sie benötigen ferner Visual Studio 2012 oder höher.


##<a name="template-concepts"></a>Vorlage Konzepte

In [Verwenden Benachrichtigung Hubs auf dem neusten Stand senden] erstellt Sie eine app, die **Kategorien** zum Abonnieren von Benachrichtigungen für verschiedene News-Kategorien verwendet.
Viele apps, allerdings Adressieren von mehreren Märkten und lokalisiert werden müssen. Dies bedeutet, die der Inhalt der Benachrichtigungen selbst lokalisiert und an den richtigen Satz von Geräten übermittelt werden müssen.
In diesem Thema wird wir die Verwendung der **Vorlage** Features der Benachrichtigung Hubs einfach lokalisierten abgeschnitten werden News Benachrichtigungen vorführen angezeigt.

Hinweis: eine Methode, um lokalisierte Benachrichtigungen senden ist die Erstellung mehrerer Versionen jedes Tag. Beispielsweise zur Unterstützung von Englisch, Französisch und Mandarin müssten drei verschiedene Kategorien für Welt News: "World_en", "World_fr," und "World_ch". Klicken Sie dann hätten wir eine lokalisierte Version von der internationalen Nachrichten an jedes dieser Tags zu senden. In diesem Thema verwenden wir Vorlagen zur Vermeidung von der zu viele Kategorien und die Anforderung mehrere Nachrichten zu senden.

Vorlagen wurden auf hoher Ebene eine Möglichkeit zum angeben, wie ein bestimmtes Gerät eine Benachrichtigung gesendet werden soll. Die Vorlage gibt das Format für die genauen Nutzlast verweisen auf Eigenschaften, die die Nachricht gesendet, indem Sie Ihre app Back-End gehören. In diesem Fall schicken wir eine Gebietsschema-unabhängige Nachricht, die alle unterstützte Sprachen enthält:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Klicken Sie dann stellen wir sicher, dass Geräte mit einer Vorlage erfassen, die auf die richtige Eigenschaft verweist. Beispielsweise wird eine Windows Store-app, die eine einfache Spruch Nachricht empfangen möchte für die folgenden Vorlage mit alle entsprechenden Kategorien registrieren:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Vorlagen handelt es sich um eine sehr leistungsfähige Funktion, die Sie weitere Informationen zu unseren Artikel [Vorlagen](notification-hubs-templates-cross-platform-push-messages.md) finden können. 


##<a name="the-app-user-interface"></a>Der app-Benutzeroberfläche

Wir werden nun die Bruchfestigkeit News-app im Thema [Verwenden Benachrichtigung Hubs auf dem neusten Stand zu senden] , senden lokalisierten mithilfe von Vorlagen auf dem neusten erstellte ändern.

Klicken Sie in der Windows Store-app:

Ändern Sie Ihrer MainPage.xaml um ein Gebietsschema Kombinationsfeld einbeziehen:

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

##<a name="building-the-windows-store-client-app"></a>Erstellen der Windows Store-app-client

1. Fügen Sie in der Klasse Benachrichtigungen einen Gebietsschemaparameter an Ihre Methoden *StoreCategoriesAndSubscribe* und *SubscribeToCateories* ein.

        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }

        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            if (categories == null)
            {
                categories = RetrieveCategories();
            }

            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);

            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }

    Beachten Sie, dass statt der *RegisterNativeAsync* Methode wir *RegisterTemplateAsync*anrufen: Wir sind eine bestimmte Benachrichtigung-Format in der hängt von die Vorlage das Gebietsschema registrieren. Wir geben Sie einen Namen für die Vorlage ("LocalizedWNSTemplateExample"), da wir möglicherweise mehr als eine Vorlage (beispielsweise eine Spruch Benachrichtigungen) und eine für die Kacheln erfassen möchten, und wir müssen akzeptieren, um in der Lage, aktualisieren oder löschen sie einen Namen eingeben.

    Beachten Sie, dass, wenn ein Gerät mit dem gleichen Tag mehrere Vorlagen registriert, eine eingehende Nachricht verwendet, die mehrere Benachrichtigungen Kategorie führt am Gerät (eine für jede Vorlage) übermittelt. Dieses Verhalten ist nützlich, wenn muss dieselbe logische Meldung mehrere visuelle Benachrichtigungen, z. B. Seitennavigationsbereich mit einen Badge und eine Spruch in einer Windows Store-Anwendung zur Folge haben.

2. Fügen Sie die folgende Methode zum Abrufen des gespeicherten Gebietsschemas hinzu:

        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }

3. In Ihrem MainPage.xaml.cs, aktualisieren Sie die Schaltfläche Ereignishandler wie dargestellt durch den aktuellen Wert des Kombinationsfelds Gebietsschema abrufen und Bereitstellen von es um den Anruf an die Benachrichtigungen-Klasse, klicken Sie auf:

        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;

            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");

            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);

            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }


4. Schließlich in der Datei App.xaml.cs stellen Sie sicher, aktualisieren Sie Ihre `InitNotificationsAsync` Methode, um das Gebietsschema abrufen und diese zu verwenden, wenn Sie Ihr Abonnement:

        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }


##<a name="send-localized-notifications-from-your-back-end"></a>Senden von lokalisierten Benachrichtigungen aus der Back-end

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]






<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Verwenden Sie die Benachrichtigung Hubs auf dem neusten Stand zu senden]: /manage/services/notification-hubs/breaking-news-dotnet

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
