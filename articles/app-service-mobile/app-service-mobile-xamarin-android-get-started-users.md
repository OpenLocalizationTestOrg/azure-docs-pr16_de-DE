<properties
    pageTitle="Erste Schritte mit der Authentifizierung für Mobile-Apps in Xamarin Android"
    description="Erfahren Sie, wie Mobile-Apps für die Benutzerauthentifizierung der app Xamarin Android mithilfe einer Vielzahl von Identitätsanbieter, einschließlich AAD, Google, Facebook, Twitter und Microsoft verwenden."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinandroid-app"></a>Authentifizierung für Ihre app Xamarin.Android hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In diesem Thema wird gezeigt, wie Benutzer einer Mobile-App von der Clientanwendung authentifizieren. In diesem Lernprogramm fügen Sie Authentifizierung zum Schnellstart Projekt mit einem Identitätsanbieter, der von Azure Mobile-Apps unterstützt wird. Nach dem wird erfolgreich authentifiziert und in der Mobile-App berechtigt, wird der Benutzer-ID-Wert angezeigt.

In diesem Lernprogramm basiert auf den Schnellstart Mobile-App. Sie müssen zuerst auch das Lernprogramm [Erstellen Sie eine app Xamarin.Android]abschließen. Wenn Sie die heruntergeladene Schnellstart Server Project nicht verwenden, müssen Sie das Authentifizierung Erweiterungspaket zum Projekt hinzufügen. Weitere Informationen zu Server Erweiterung Paketen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Mobile-Apps Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="a-nameregisteraregister-your-app-for-authentication-and-configure-app-services"></a><a name="register"></a>Registrieren Sie Ihre app für die Authentifizierung und Konfigurieren von App-Diensten

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="a-namepermissionsarestrict-permissions-to-authenticated-users"></a><a name="permissions"></a>Einschränken der Berechtigungen für authentifizierte Benutzer

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Führen Sie in Visual Studio oder Xamarin Studio das Clientprojekt auf einem Gerät oder Emulator. Stellen Sie sicher, dass Ausnahmefehler mit einem Statuscode von 401 (nicht autorisiert) ausgelöst wird, nachdem die app gestartet wird. Dies geschieht, da die app versucht wird, auf Ihre Mobile-App Back-End-als nicht authentifizierte Benutzer zuzugreifen. Die Tabelle *TodoItem* benötigt nun Authentifizierung.

Sie werden als Nächstes Client-app zu Anforderung Ressourcen aus der Mobile-App Back-End-mit einem authentifizierten Benutzer aktualisieren.

##<a name="a-nameadd-authenticationaadd-authentication-to-the-app"></a><a name="add-authentication"></a>Authentifizierung für die app hinzufügen

Die app wird aktualisiert, um Benutzern verlangen, tippen Sie auf die Schaltfläche **Anmelden** und authentifizieren, bevor Daten angezeigt werden.

1. Fügen Sie den folgenden Code der Klasse **TodoActivity** an:

        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook);
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");

                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }

        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;

                // Load the data.
                OnRefreshItemsSelected();
            }
        }

    Dadurch wird eine neue Methode zum Authentifizieren eines Benutzers und eine Methode Ereignishandler für eine **Anmelden** -Schaltfläche erstellt. Der Benutzer im obigen Beispielcode authentifiziert über eine Facebook-Login. Einmal authentifizierte Benutzer-ID angezeigt werden, wird ein Dialogfeld verwendet.

    > [AZURE.NOTE] Wenn Sie mit einen Identitätsanbieter als Facebook arbeiten, ändern Sie den Wert, der an **LoginAsync** auf einen der folgenden übergeben: _MicrosoftAccount_, _Twitter_, _Google_oder _WindowsAzureActiveDirectory_.

3. Die Methode **OnCreate** löschen Sie oder Kommentar Auschecken des Codes der folgenden Zeile:

        OnRefreshItemsSelected ();

4. Fügen Sie in der Datei Activity_To_Do.axml die folgende *LoginUser* Schaltfläche Definition, bevor Sie die vorhandenen *AddItem* Schaltfläche hinzu:

        <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />

5. Fügen Sie das folgende Element in die Ressourcendatei Strings.xml aus:

        <string name="login_button_text">Sign in</string>

6. Führen Sie in Visual Studio oder Xamarin Studio das Clientprojekt auf einem Gerät oder Emulator, und melden Sie sich mit Ihrer ausgewählten Identitätsanbieter.

    Wenn Sie erfolgreich angemeldeten sind, die app werden Ihre Anmelde-ID und die Liste der Aufgaben angezeigt, und können vorgenommenen Aktualisierungen zu den Daten.


<!-- URLs. -->
[Erstellen Sie eine app Xamarin.Android]: app-service-mobile-xamarin-android-get-started.md
