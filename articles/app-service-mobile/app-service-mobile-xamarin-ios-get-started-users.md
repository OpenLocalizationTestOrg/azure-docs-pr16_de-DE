<properties
    pageTitle="Erste Schritte mit der Authentifizierung für Mobile-Apps in Xamarin für iOS"
    description="Erfahren Sie, wie Mobile-Apps für die Benutzerauthentifizierung der app iOS Xamarin mithilfe einer Vielzahl von Identitätsanbieter, einschließlich AAD, Google, Facebook, Twitter und Microsoft verwenden."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinios-app"></a>Authentifizierung für Ihre app Xamarin.iOS hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In diesem Thema wird gezeigt, wie eine App Dienst Mobile-App von der Clientanwendung Benutzer authentifiziert wird. In diesem Lernprogramm fügen Sie Authentifizierung des Xamarin.iOS Schnellstart-Projekts mit einem Identitätsanbieter, der von der App-Dienst unterstützt wird. Der Benutzer-ID-Wert wird angezeigt, nachdem er erfolgreich authentifiziert und, indem Sie Ihre Mobile-App autorisiert, und Sie auf eingeschränkten Tabellendaten zugreifen.

Sie müssen zuerst das Lernprogramm [Erstellen Sie eine app Xamarin.iOS]ausfüllen. Wenn Sie die heruntergeladene Schnellstart Server Project nicht verwenden, müssen Sie das Authentifizierung Erweiterungspaket zum Projekt hinzufügen. Weitere Informationen zu Server Erweiterung Paketen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Registrieren Sie Ihre app für die Authentifizierung und Konfigurieren von App-Diensten

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Einschränken der Berechtigungen für authentifizierte Benutzer

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4 Klicken Sie in Visual Studio oder Xamarin Studio führen Sie das Clientprojekt auf einem Gerät oder Emulator aus. Stellen Sie sicher, dass Ausnahmefehler mit einem Statuscode von 401 (nicht autorisiert) ausgelöst wird, nachdem die app gestartet wird. Der Fehler ist auf der Konsole des Debuggers protokolliert. Daher sollte in Visual Studio den Fehler im Ausgabefenster angezeigt werden.

&nbsp;&nbsp;Dieser Fehler nicht autorisierte liegt daran, dass die app versucht wird, auf Ihre Mobile-App Back-End-als nicht authentifizierte Benutzer zuzugreifen. Die Tabelle *TodoItem* benötigt nun Authentifizierung.

Sie werden als Nächstes Client-app zu Anforderung Ressourcen aus der Mobile-App Back-End-mit einem authentifizierten Benutzer aktualisieren.

##<a name="add-authentication-to-the-app"></a>Authentifizierung für die app hinzufügen

In diesem Abschnitt ändern Sie die app, um einen Anmeldebildschirm vor dem Anzeigen von Daten anzuzeigen. Wenn die app gestartet wird, wird nicht nicht Herstellen einer Verbindung mit Ihrem App-Dienst und alle Daten werden nicht angezeigt. Nach dem ersten ablaufen, die führt der Benutzer die Bewegung Aktualisieren der Anmeldebildschirm wird angezeigt. nach der erfolgreichen Anmeldung wird die Liste der Aufgaben angezeigt.

1. Im Clientprojekt, öffnen Sie die Datei **QSTodoService.cs** , und fügen Sie die folgende Anweisung verwenden und `MobileServiceUser` mit Accessor der Klasse QSTodoService:

    ```
        using UIKit;
    ```

        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

2. Fügen Sie die neue Methode, die mit dem Namen **authentifizieren** zu **QSTodoService** mit der folgenden Definition hinzu:


        public async Task Authenticate(UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Wenn Sie einen Identitätsanbieter als eine Facebook verwenden, ändern Sie den Wert, der an **LoginAsync** auf einen der folgenden übergeben: _MicrosoftAccount_, _Twitter_, _Google_oder _WindowsAzureActiveDirectory_.

3. Öffnen Sie **QSTodoListViewController.cs**. Ändern der Methodendefinition **ViewDidLoad** Entfernen des Anrufs an die **RefreshAsync()** am Ende:

        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            todoService = QSTodoService.DefaultService;
           await todoService.InitializeStoreAsync ();

           RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync ();
           }

            // Comment out the call to RefreshAsync
            // await RefreshAsync ();
        }


4. Ändern Sie die Methode **RefreshAsync** authentifizieren, wenn die **Benutzer** -Eigenschaft null ist. Fügen Sie den folgenden Code am oberen Rand der Methodendefinition hinzu:

        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method

5. In Visual Studio oder Xamarin Studio Xamarin Erstellen von Host auf dem Mac, führen Sie das Clientprojekt verwendet werden, einem Gerät oder Emulator verbunden. Stellen Sie sicher, dass der app keine Daten angezeigt werden.

    Führen Sie die Aktualisierung Bewegung durch Ziehen Sie in der Liste der Elemente, die unterbindet den Anmeldebildschirm angezeigt werden. Sobald Sie erfolgreich gültige Anmeldeinformationen eingegeben haben, die app wird die Liste der Aufgaben angezeigt, und können vorgenommenen Aktualisierungen zu den Daten.


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Erstellen Sie eine app Xamarin.iOS]: app-service-mobile-xamarin-ios-get-started.md
