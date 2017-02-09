
1. Fügen Sie die folgenden Aussagen zur **Verwendung** in der Projektdatei MainPage.xaml.cs hinzu:

        using System.Linq;      
        using Windows.Security.Credentials;

2. Ersetzen Sie die Methode **AuthenticateAsync** mit den folgenden Code ein:

        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;

            // This sample uses the Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;

            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;

            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }

            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;

                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;

                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.

                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);

                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);

                    success = true;
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
            
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();

            return success;
        }

    In dieser Version von **AuthenticateAsync**versucht die app in der **PasswordVault** gespeicherte Anmeldeinformationen verwenden, den Zugriff auf Dienste aus. Eine normale Anmeldung wird ebenfalls ausgeführt, wenn keine gespeicherten Anmeldeinformationen vorhanden ist.

    >[AZURE.NOTE]Eine Cache Token ist abgelaufen, und token Ablauf kann auch nach der Authentifizierung auftreten, wenn die app verwendet wird. Erfahren Sie, wie Sie ermitteln, ob ein Token abgelaufen ist, finden Sie unter [abgelaufenen Authentifizierungstoken prüfen](http://aka.ms/jww5vp). Eine Lösung für die Verarbeitung von Autorisierung Fehler im Zusammenhang mit ablaufenden Token finden Sie unter dem Beitrag [Zwischenspeichern und abgelaufene Token in Azure Mobile Services Behandlung von verwalteten SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx). 

3. Starten Sie zweimal die app erneut.

    Beachten Sie, dass beim Start ersten Anmeldung mit den Anbieter erneut erforderlich ist. Jedoch auf der zweiten Neustart werden die zwischengespeicherten Anmeldeinformationen verwendet und Anmeldung umgangen. 
