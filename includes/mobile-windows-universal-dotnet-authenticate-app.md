
1. Öffnen Sie die freigegebene Projektdatei MainPage.cs, und fügen Sie den folgenden Codeausschnitt zur MainPage Klasse:
    
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;

        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now signed in - {0}", user.UserId);

                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }

    In diesem Code authentifiziert den Benutzer mit einem Facebook-Konto an. Wenn Sie mit einen Identitätsanbieter als Facebook arbeiten, ändern Sie den Wert von **MobileServiceAuthenticationProvider** über den Wert für Ihren Anbieter.

3. Kommentar Auschecken oder löschen Sie den Anruf an die **RefreshTodoItems** -Methode in die vorhandene **OnNavigatedTo** Außerkraftsetzung.

    Dadurch wird verhindert, dass die Daten geladen werden, bevor der Benutzer authentifiziert wird. Als Nächstes fügen Sie eine Schaltfläche **Melden Sie sich** bei der app, die Authentifizierung auslöst.

4. Im folgenden Codeausschnitt der MainPage-Klasse hinzufügen:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Hide the login button and load items from the mobile app.
                ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Öffnen Sie in der Windows Store-app-Projekt die MainPage.xaml Projektdatei, und fügen Sie das folgende **Schaltfläche** Element unmittelbar vor dem Element, das die Schaltfläche " **Speichern** " definiert:

        <Button Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible">Sign in</Button>

6. Fügen Sie in der Windows Phone-Store-app-Projekt das folgende **Schaltfläche** Element in der **ContentPanel**, nach dem **Textfeld** Element hinzu:

        <Button Grid.Row ="1" Grid.Column="1" Name="ButtonLogin" Click="ButtonLogin_Click" 
            Margin="10, 0, 0, 0" Visibility="Visible">Sign in</Button>

8. Öffnen Sie die freigegebene App.xaml.cs Projektdatei, und fügen Sie den folgenden Code hinzu:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            // Windows Phone 8.1 requires you to handle the respose from the WebAuthenticationBroker.
            #if WINDOWS_PHONE_APP
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Completes the sign-in process started by LoginAsync.
                // Change 'MobileService' to the name of your MobileServiceClient instance. 
                App.MobileService.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
            #endif

            base.OnActivated(args);
        }

    Wenn die **OnActivated** -Methode bereits vorhanden ist, fügen Sie einfach die `#if...#endif` Codeblock.

9. Drücken Sie F5, um das Ausführen von Windows Store-app, klicken Sie auf die Schaltfläche **Anmelden** , und melden Sie sich mit Ihrer ausgewählten Identitätsanbieter bei der app aus. 

    Wenn Sie erfolgreich angemeldeten sind, sollte die app ohne Fehler ausgeführt, und Sie sollten Ihre Back-End-Abfragen und Updates Daten möglicherweise.

10. Mit der rechten Maustaste in des Windows Phone-Store-app-Projekts, klicken Sie auf **als beim Startprojekt festlegen**, und wiederholen Sie im vorhergehenden Schritt zu überprüfen, ob das Windows Phone Store-app auch korrekt ausgeführt wird.  

 