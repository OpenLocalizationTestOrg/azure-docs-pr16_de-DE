<properties
    pageTitle="Authentifizierung für Ihre app Universal Windows-Plattform (UWP) hinzufügen | Azure Mobile-Apps"
    description="Informationen zum Azure App Dienst Mobile-Apps zu verwenden, um Benutzer der app Universal Windows-Plattform (UWP) mithilfe einer Vielzahl von Identitätsanbieter, einschließlich authentifizieren: AAD, Google, Facebook, Twitter und Microsoft."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-windows-app"></a>Hinzufügen von Authentifizierung zu Ihrem Windows-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In diesem Thema wird gezeigt, wie Ihre mobile-app cloudbasierten Authentifizierung hinzugefügt. In diesem Lernprogramm fügen Sie Authentifizierung zum Projekt Schnellstart Universal Windows-Plattform (UWP) für Mobile-Apps mit einem Identitätsanbieter, der von der App-Verwaltungsdienst Azure unterstützt wird. Nach dem wird erfolgreich authentifiziert und durch Ihre Mobile-App Back-End-autorisiert, wird der Benutzer-ID-Wert angezeigt.

In diesem Lernprogramm basiert auf den Schnellstart Mobile-Apps. Sie müssen zuerst das [Erste Schritte mit Mobile-Apps](app-service-mobile-windows-store-dotnet-get-started.md)Lernprogramm ausfüllen.

##<a name="a-nameregisteraregister-your-app-for-authentication-and-configure-the-app-service"></a><a name="register"></a>Registrieren Sie Ihre app für die Authentifizierung und konfigurieren Sie der App-Verwaltungsdienst

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="a-namepermissionsarestrict-permissions-to-authenticated-users"></a><a name="permissions"></a>Einschränken der Berechtigungen für authentifizierte Benutzer

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nun können Sie überprüfen, dass anonymer Zugriff auf Ihre Back-End-deaktiviert wurde. Bereitstellen von UWP app Projekt festgelegt, wie das Startprojekt und führen Sie die app; Stellen Sie sicher, dass Ausnahmefehler mit einem Statuscode von 401 (nicht autorisiert) ausgelöst wird, nachdem die app gestartet wird. Dies geschieht, da die app Zugriff auf Ihre Mobile-App-Code als nicht authentifizierte Benutzer versucht, aber die *TodoItem* Tabelle jetzt erfordert Authentifizierung eine.

Aktualisieren Sie als Nächstes die app, um Benutzer authentifizieren, bevor Sie Ressourcen aus dem App-Dienst anfordern.

##<a name="a-nameadd-authenticationaadd-authentication-to-the-app"></a><a name="add-authentication"></a>Authentifizierung für die app hinzufügen

1. In der UWP app project-Datei MainPage.cs und der MainPage-Klasse den folgenden Codeausschnitt hinzufügen:
    
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

3. Kommentar Auschecken oder löschen Sie den Anruf an die Methode **ButtonRefresh_Click** (oder die **InitLocalStoreAsync** -Methode) in die vorhandene **OnNavigatedTo** Außerkraftsetzung. Dadurch wird verhindert, dass die Daten geladen werden, bevor der Benutzer authentifiziert wird. Als Nächstes fügen Sie eine Schaltfläche **Melden Sie sich** bei der app, die Authentifizierung auslöst.

4. Im folgenden Codeausschnitt der MainPage-Klasse hinzufügen:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Öffnen Sie die Projektdatei MainPage.xaml, suchen Sie das Element, das die Schaltfläche " **Speichern** " definiert, und Ersetzen Sie ihn durch den folgenden Code:

        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>

9. Drücken Sie die Taste F5, führen Sie die app, klicken Sie auf die Schaltfläche **Anmelden** , und melden Sie sich mit Ihrer ausgewählten Identitätsanbieter bei der app aus. Nach der Anmeldung erfolgreich ist, die app, die ohne Fehler ausgeführt wird und Sie können Ihre Back-End-Abfragen und Updates Daten.


##<a name="a-nametokensastore-the-authentication-token-on-the-client"></a><a name="tokens"></a>Speichern Sie das Authentifizierungstoken auf dem client

Im vorherige Beispiel gezeigt einen Standard anmelden, die erfordert, dass des Clients sowohl der Identitätsanbieter und der App-Verwaltungsdienst jedes Mal wenden Sie sich an, die die app gestartet wird. Diese Methode ist nicht nur nicht effizient ausgeführt werden kann in Verwendung-bezieht sich Probleme sollten viele Kunden versuchen Sie die app zur gleichen Zeit gestartet. Ein besserer Ansatz ist zum Zwischenspeichern von des Autorisierung Tokens zurückgegeben, indem Sie Ihre App-Verwaltungsdienst, und versuchen Sie, diese ersten verwenden, bevor eine Anmeldung Anbieter-basierten.

>[AZURE.NOTE]Sie können das Token ausgestellt von App-Diensten, unabhängig davon, ob Sie Client verwaltet oder Dienst verwaltet Authentifizierung arbeiten Zwischenspeichern. In diesem Lernprogramm verwendet Authentifizierung Dienst verwaltet.

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie in diesem Lernprogramm Standardauthentifizierung abgeschlossen haben, sollten Sie Sie mit einer der folgenden Lernprogramme fortfahren:

+ [Hinzufügen von Pushbenachrichtigungen zu Ihrer Anwendung](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Informationen Sie zum Hinzufügen von Pushbenachrichtigungen Benachrichtigungen zu Ihrer Anwendung zu unterstützen und konfigurieren Ihre Mobile-App Back-End-um Azure Benachrichtigung Hubs verwenden, um Pushbenachrichtigungen zu senden.

+ [Offlinesynchronisierung für Ihre app zu aktivieren](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Erfahren Sie, wie offlinesupport Ihre app ein Mobile-App Back-End hinzufügen. Offlinesynchronisierung ermöglicht es den Endbenutzern Interaktion mit einem mobilen app&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;auch, wenn keine Verbindung zum Netzwerk vorhanden ist.


<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md

