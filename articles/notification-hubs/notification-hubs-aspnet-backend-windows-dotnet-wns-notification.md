<properties
    pageTitle="Azure Benachrichtigung Hubs Benutzer benachrichtigen mit .NET Back-End-"
    description="Informationen Sie zum Senden von sicheren Pushbenachrichtigungen in Azure. Codebeispielen in c# mithilfe der .NET API geschrieben wurde."
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    services="notification-hubs"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-notify-users-with-net-backend"></a>Azure Benachrichtigung Hubs Benutzer benachrichtigen mit .NET Back-End-

[AZURE.INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]


##<a name="overview"></a>(Übersicht)

Pushbenachrichtigungen Benachrichtigung Unterstützung in Azure ermöglicht es Ihnen zu einer einfach zu verwendendes, mehrere Plattformen und skalierten Pushbenachrichtigungen Infrastruktur die Implementierung von Pushbenachrichtigungen für Consumer und Enterprise Applications für mobile Plattformen vereinfacht zugegriffen werden. In diesem Lernprogramm erfahren Sie, wie Azure Benachrichtigung Hubs um Pushbenachrichtigungen an einen bestimmten app-Benutzer auf ein bestimmtes Gerät zu senden. Eine ASP.NET WebAPI Back-End-wird verwendet, um die Clients authentifiziert. Mit den authentifizierten Client-Benutzer, und klicken Sie auf Kategorie automatisch werden durch die Back-End-Benachrichtigung Registrierung hinzugefügt wird. Dieses Tag wird verwendet, durch die Back-End-Generieren von Benachrichtigungen für einen bestimmten Benutzer zu senden. Weitere Informationen zur Registrierung für Benachrichtigungen über eine app Back-End-finden Sie unter Anleitungen Thema [registrieren aus Ihrer app Back-End](http://msdn.microsoft.com/library/dn743807.aspx). In diesem Lernprogramm basiert auf die Benachrichtigung Hub und Projekt, das Sie in das [Erste Schritte mit Benachrichtigung Hubs] Lernprogramm erstellt haben.

In diesem Lernprogramm ist auch der Voraussetzung zum Lernprogramm [Secure Pushbenachrichtigungen] . Nachdem Sie die Schritte in diesem Lernprogramm abgeschlossen haben, können Sie in das Lernprogramm [Pushbenachrichtigungen Secure] nacheinander wird gezeigt, wie Sie den Code in diesem Lernprogramm zu einer Pushbenachrichtigung sicher senden zu ändern.





## <a name="before-you-begin"></a>Vorbemerkung

Wir führen Sie Ihr Feedback ernst nehmen. Wenn Sie dieses Thema oder Vorschläge zur Verbesserung dieses Inhaltstyps durchführen Probleme haben, möchten wir Ihr Feedback am unteren Rand der Seite schätzen.

Der vollständige Code für dieses Lernprogramms finden Sie auf GitHub [hier](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). 



##<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm starten, müssen Sie bereits diesen Lernprogrammen Mobile Dienste absolviert haben:

+ [Erste Schritte mit Benachrichtigung Hubs]<br/>Erstellen den Benachrichtigung Hub und reservieren den Namen der Anwendung und zum Empfangen von Benachrichtigungen in diesem Lernprogramm registrieren. In diesem Lernprogramm wird davon ausgegangen, dass Sie diese Schritte bereits durchgeführt haben. Wenn dies nicht der Fall ist, führen Sie die Schritte in [Erste Schritte mit Benachrichtigung Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md); insbesondere in den Abschnitten [Registrieren der app für Windows Store](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#register-your-app-for-the-windows-store) und [Konfigurieren der Benachrichtigung Hub](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub). Insbesondere, stellen Sie sicher, dass Sie das **Paket SID** und **Client geheim** Werte im Portal auf der Registerkarte " **Konfigurieren** " für Ihre Benachrichtigung Hub eingegeben haben. Dieses Verfahren Konfiguration wird im Abschnitt [Konfigurieren der Benachrichtigung Hub](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub)beschrieben. Dies ist ein wichtiger Schritt: Wenn die Anmeldeinformationen auf das Portal nicht für den Namen der Anwendung angegebenen entsprechen Sie auswählen, der Pushbenachrichtigung kann nicht durchgeführt werden.


> [AZURE.NOTE] Wenn Sie Mobile-Apps in Azure-App-Verwaltungsdienst als Ihren Back-End-Dienst verwenden, finden Sie unter der [Mobile-Apps Version](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) dieses Lernprogramms.

&nbsp;

[AZURE.INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="update-the-code-for-the-client-project"></a>Aktualisieren Sie den Code für das Clientprojekt

In diesem Abschnitt Aktualisieren Sie den Code in das Projekt, das Sie für das [Erste Schritte mit Benachrichtigung Hubs] Lernprogramm abgeschlossen. Die bereits im Store zugeordnet und für Ihre Benachrichtigung Hub konfiguriert werden soll. In diesem Abschnitt fügen Sie Code zum Aufrufen der neuen WebAPI Back-End- und verwenden sie für die Registrierung und Senden von Benachrichtigungen.

1. Öffnen Sie in Visual Studio, die die Lösung, die Sie für das [Erste Schritte mit Benachrichtigung Hubs] Lernprogramm erstellt haben.

2. In Lösung Explorer mit der rechten Maustaste in des Projekts **(Windows 8.1)** , und klicken Sie dann auf **NuGet-Pakete verwalten**.

3. Klicken Sie auf der linken Seite auf **Online**.

4. Geben Sie in **das Suchfeld** ein **HTTP-Client**.

5. Klicken Sie in der Liste der Suchergebnisse auf **Microsoft HTTP-Client-Bibliotheken**, und klicken Sie dann auf **Installieren**. Führen Sie die Installation.

6. Geben Sie wieder in das Feld NuGet **Suche** **Json.net**ein. Installieren Sie das Paket **Json.NET** , und schließen Sie das Fenster NuGet-Paket-Manager.

7. Wiederholen Sie die obigen Schritte für das Projekt **(Windows Phone 8.1)** , das **JSON.NET** NuGet-Paket für das Windows Phone-Projekt zu installieren.


8. Lösung-Explorer **(Windows 8.1)** im Projekt doppelklicken Sie auf **"MainPage.xaml"** , um es im Visual Studio-Editor zu öffnen.

9. Ersetzen Sie den **MainPage.xaml** XML-Code, der `<Grid>` Abschnitt mit den folgenden Code. Dieser Code Fügt einen Benutzernamen und Ihr Kennwort Textfeld, die mit der Benutzer authentifiziert wird. Darüber hinaus werden Textfelder für die Benachrichtigung und der Username-Kategorie, die die Benachrichtigung erhalten sollen hinzugefügt:

        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>

            <TextBlock Grid.Row="0" Text="Notify Users" HorizontalAlignment="Center" FontSize="48"/>

            <StackPanel Grid.Row="1" VerticalAlignment="Center">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                    </Grid.ColumnDefinitions>
                    <TextBlock Grid.Row="0" Grid.ColumnSpan="3" Text="Username" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="UsernameTextBox" Grid.Row="1" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
                    <TextBlock Grid.Row="2" Grid.ColumnSpan="3" Text="Password" FontSize="24" Margin="20,0,20,0" />
                    <PasswordBox Name="PasswordTextBox" Grid.Row="3" Grid.ColumnSpan="3" Margin="20,0,20,0"/>

                    <Button Grid.Row="4" Grid.ColumnSpan="3" HorizontalAlignment="Center" VerticalAlignment="Center"
                                Content="1. Login and register" Click="LoginAndRegisterClick" Margin="0,0,0,20"/>

                    <ToggleButton Name="toggleWNS" Grid.Row="5" Grid.Column="0" HorizontalAlignment="Right" Content="WNS" IsChecked="True" />
                    <ToggleButton Name="toggleGCM" Grid.Row="5" Grid.Column="1" HorizontalAlignment="Center" Content="GCM" />
                    <ToggleButton Name="toggleAPNS" Grid.Row="5" Grid.Column="2" HorizontalAlignment="Left" Content="APNS" />

                    <TextBlock Grid.Row="6" Grid.ColumnSpan="3" Text="Username Tag To Send To" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="ToUserTagTextBox" Grid.Row="7" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <TextBlock Grid.Row="8" Grid.ColumnSpan="3" Text="Enter Notification Message" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="NotificationMessageTextBox" Grid.Row="9" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <Button Grid.Row="10" Grid.ColumnSpan="3" HorizontalAlignment="Center" Content="2. Send push" Click="PushClick" Name="SendPushButton" />
                </Grid>
            </StackPanel>
        </Grid>


10. Klicken Sie in der Lösung-Explorer **(Windows Phone 8.1)** im Projekt **MainPage.xaml** öffnen, und Ersetzen Sie den Windows Phone 8.1 `<Grid>` Abschnitts durch die gleichen Code oben. Die Benutzeroberfläche sollte Neuigkeiten abgebildet ähneln.

    ![][13]

11. Öffnen Sie im Explorer-Lösung **MainPage.xaml.cs** Datei für die Projekte **(Windows 8.1)** und **(Windows Phone 8.1)** ein. Fügen Sie den folgenden `using` Anweisungen am oberen Rand der beiden Dateien:

        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Windows.Networking.PushNotifications;
        using Windows.UI.Popups;
        using System.Threading.Tasks;

12. In **MainPage.xaml.cs** für die Projekte **(Windows 8.1)** und **(Windows Phone 8.1)** , das folgende Mitglied hinzufügen der `MainPage` Class. Ersetzen Sie unbedingt `<Enter Your Backend Endpoint>` mit der der tatsächlichen Back-End-Endpunkt zuvor abgerufen. Beispielsweise `http://mybackend.azurewebsites.net`.

        private static string BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";



13. Fügen Sie den Code unterhalb der MainPage-Klasse in **MainPage.xaml.cs** für die Projekte **(Windows 8.1)** und **(Windows Phone 8.1)** .

    Die `PushClick` Methode ist der Click-Ereignishandler für die Schaltfläche **Pushbenachrichtigungen zu senden** . Es ruft die Back-End-, damit eine Benachrichtigung an alle Geräte mit einer Kategorie Benutzernamen ausgelöst wird, die entspricht der `to_tag` Parameter. Die Nachricht wird als JSON-Inhalt in den Hauptteil der Anforderung gesendet.

    Die `LoginAndRegisterClick` Methode ist der Click Ereignishandler für die **anmelden und registrieren** Schaltfläche. Speichert die Basic Authentifizierungstoken im lokalen Speicher (Beachten Sie, dass dies alle Ihre Authentifizierung des Farbschemas verwendet Token darstellt) und dann verwendet `RegisterClient` für Benachrichtigungen über die Back-End-registrieren.


        private async void PushClick(object sender, RoutedEventArgs e)
        {
            if (toggleWNS.IsChecked.Value)
            {
                await sendPush("wns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleGCM.IsChecked.Value)
            {
                await sendPush("gcm", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleAPNS.IsChecked.Value)
            {
                await sendPush("apns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);

            }
        }

        private async Task sendPush(string pns, string userTag, string message)
        {
            var POST_URL = BACKEND_ENDPOINT + "/api/notifications?pns=" +
                pns + "&to_tag=" + userTag;

            using (var httpClient = new HttpClient())
            {
                var settings = ApplicationData.Current.LocalSettings.Values;
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                try
                {
                    await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                        System.Text.Encoding.UTF8, "application/json"));
                }
                catch (Exception ex)
                {
                    MessageDialog alert = new MessageDialog(ex.Message, "Failed to send " + pns + " message");
                    alert.ShowAsync();
                }
            }
        }

        private async void LoginAndRegisterClick(object sender, RoutedEventArgs e)
        {
            SetAuthenticationTokenInLocalStorage();

            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            // The "username:<user name>" tag gets automatically added by the message handler in the backend.
            // The tag passed here can be whatever other tags you may want to use.
            try
            {
                // The device handle used will be different depending on the device and PNS. 
                // Windows devices use the channel uri as the PNS handle.
                await new RegisterClient(BACKEND_ENDPOINT).RegisterAsync(channel.Uri, new string[] { "myTag" });

                var dialog = new MessageDialog("Registered as: " + UsernameTextBox.Text);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
                SendPushButton.IsEnabled = true;
            }
            catch (Exception ex)
            {
                MessageDialog alert = new MessageDialog(ex.Message, "Failed to register with RegisterClient");
                alert.ShowAsync();
            }
        }

        private void SetAuthenticationTokenInLocalStorage()
        {
            string username = UsernameTextBox.Text;
            string password = PasswordTextBox.Password;

            var token = Convert.ToBase64String(System.Text.Encoding.UTF8.GetBytes(username + ":" + password));
            ApplicationData.Current.LocalSettings.Values["AuthenticationToken"] = token;
        }



14. Öffnen Sie im Explorer-Lösung unter dem Projekt **freigegeben** **App.xaml.cs** Datei ein. Suchen Sie den Anruf an die `InitNotificationsAsync()` in der `OnLaunched()` Ereignishandler. Kommentieren oder löschen Sie den Anruf an die `InitNotificationsAsync()`. Der Schaltfläche Ereignishandler über hinzugefügt werden Benachrichtigung Registrierungen Initialisierung.


        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
            //InitNotificationsAsync();


15. In Solution Explorer mit der rechten Maustaste in des Projekts **freigegeben** , dann klicken Sie auf **Hinzufügen**und dann auf **Klasse**. Name der Klasse **RegisterClient.cs**, klicken Sie dann auf **OK** , um die Klasse zu generieren.

    Diese Klasse wird die restlichen Anrufe erforderlich, um die app Back-End-Kontakt auf, um registrieren für Pushbenachrichtigungen umbrochen wird. Die Benachrichtigung wie ausführlich [registrieren aus Ihrer app Back-End-](http://msdn.microsoft.com/library/dn743807.aspx)Hub erstellte *RegistrationIds* auch lokal gespeichert. Beachten Sie, dass es verwendet eine Autorisierungstoken im lokalen Speicher gespeichert, beim Klicken auf die **anmelden und registrieren** Schaltfläche.


16. Fügen Sie den folgenden `using` Anweisungen am oberen Rand der RegisterClient.cs-Datei:

        using Windows.Storage;
        using System.Net;
        using System.Net.Http;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using System.Threading.Tasks;
        using System.Linq;

17. Fügen Sie den folgenden Code innerhalb der `RegisterClient` definierte Klasse.

        private string POST_URL;

        private class DeviceRegistration
        {
            public string Platform { get; set; }
            public string Handle { get; set; }
            public string[] Tags { get; set; }
        }

        public RegisterClient(string backendEndpoint)
        {
            POST_URL = backendEndpoint + "/api/register";
        }

        public async Task RegisterAsync(string handle, IEnumerable<string> tags)
        {
            var regId = await RetrieveRegistrationIdOrRequestNewOneAsync();

            var deviceRegistration = new DeviceRegistration
            {
                Platform = "wns",
                Handle = handle,
                Tags = tags.ToArray<string>()
            };

            var statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);

            if (statusCode == HttpStatusCode.Gone)
            {
                // regId is expired, deleting from local storage & recreating
                var settings = ApplicationData.Current.LocalSettings.Values;
                settings.Remove("__NHRegistrationId");
                regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
                statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
            }

            if (statusCode != HttpStatusCode.Accepted)
            {
                // log or throw
                throw new System.Net.WebException(statusCode.ToString());
            }
        }

        private async Task<HttpStatusCode> UpdateRegistrationAsync(string regId, DeviceRegistration deviceRegistration)
        {
            using (var httpClient = new HttpClient())
            {
                var settings = ApplicationData.Current.LocalSettings.Values;
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string) settings["AuthenticationToken"]);

                var putUri = POST_URL + "/" + regId;

                string json = JsonConvert.SerializeObject(deviceRegistration);
                                var response = await httpClient.PutAsync(putUri, new StringContent(json, Encoding.UTF8, "application/json"));
                return response.StatusCode;
            }
        }

        private async Task<string> RetrieveRegistrationIdOrRequestNewOneAsync()
        {
            var settings = ApplicationData.Current.LocalSettings.Values;
            if (!settings.ContainsKey("__NHRegistrationId"))
            {
                using (var httpClient = new HttpClient())
                {
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                    var response = await httpClient.PostAsync(POST_URL, new StringContent(""));
                    if (response.IsSuccessStatusCode)
                    {
                        string regId = await response.Content.ReadAsStringAsync();
                        regId = regId.Substring(1, regId.Length - 2);
                        settings.Add("__NHRegistrationId", regId);
                    }
                    else
                    {
                        throw new System.Net.WebException(response.StatusCode.ToString());
                    }
                }
            }
            return (string)settings["__NHRegistrationId"];

        }

18. Alle Ihre Änderungen zu speichern.


## <a name="testing-the-application"></a>Testen der Anwendung

1. Starten Sie die Anwendung auf Windows 8.1 und Windows Phone 8.1. Für Windows Phone 8.1 können Sie die Instanz in den Emulator oder einem tatsächlichen Gerät ausführen.

2. Geben Sie in der Windows 8.1-Instanz der app einen **Benutzernamen** und **ein Kennwort** wie in der folgenden Abbildung gezeigt. Sie sollten abweichen, aus den Benutzernamen und das Kennwort, die Sie auf Windows Phone eingeben.


3. Klicken Sie auf **anmelden und registrieren** und überprüfen Sie, ob ein Dialogfeld zeigt an, dass Sie sich angemeldet haben. Dadurch wird die Schaltfläche **Senden Pushbenachrichtigungen** auch aktiviert.

    ![][14]

4. Die Instanz 8.1 für Windows Phone Geben Sie als Zeichenfolge einen Benutzer in die Felder für den **Benutzernamen** und das **Kennwort ein** und dann klicken Sie auf **Login und Register**.
5. Klicken Sie dann im Feld **Empfänger Username Kategorie** Geben Sie den Benutzernamen, die auf Windows 8.1 registriert. Geben Sie eine Benachrichtigung, und klicken Sie auf **Senden Pushbenachrichtigungen**.

    ![][16]

6. Nur die Geräte, die mit dem entsprechenden Benutzernamen Tag registriert haben, erhalten die Benachrichtigung.

    ![][15]

## <a name="next-steps"></a>Nächste Schritte

* Wenn Sie Ihre Benutzer Zinsen gruppenweise segmentieren möchten, finden Sie unter [Verwenden von Benachrichtigung Hubs auf dem neusten Stand zu senden].
* Weitere Informationen zum Verwenden der Benachrichtigung Hubs finden Sie in der [Benachrichtigung Hubs Anleitung].



[9]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push9.png
[10]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push10.png
[11]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push11.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-ui.png
[14]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-windows-instance-username.png
[15]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-notification-received.png
[16]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-send-message.png



<!-- URLs. -->
[Erste Schritte mit Benachrichtigung Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Sichern von Pushbenachrichtigungen]: notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md
[Verwenden Sie die Benachrichtigung Hubs auf dem neusten Stand zu senden]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Benachrichtigung Hubs Anleitungen]: http://msdn.microsoft.com/library/jj927170.aspx
