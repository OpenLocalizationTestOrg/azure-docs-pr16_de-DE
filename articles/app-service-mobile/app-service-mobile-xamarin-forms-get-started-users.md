<properties
    pageTitle="Erste Schritte mit der Authentifizierung für Mobile-Apps in Xamarin.Forms app | Microsoft Azure"
    description="Erfahren Sie, wie Mobile-Apps für die Benutzerauthentifizierung der app Xamarin Formulare mithilfe einer Vielzahl von Identitätsanbieter, einschließlich AAD, Google, Facebook, Twitter und Microsoft verwenden."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinforms-app"></a>Authentifizierung für Ihre app Xamarin.Forms hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

##<a name="overview"></a>(Übersicht)

In diesem Thema wird gezeigt, wie eine App Dienst Mobile-App von der Clientanwendung Benutzer authentifiziert wird. In diesem Lernprogramm fügen Sie Authentifizierung des Xamarin.Forms Schnellstart-Projekts mit einem Identitätsanbieter, der von der App-Dienst unterstützt wird. Der Benutzer-ID-Wert wird angezeigt, nachdem er erfolgreich authentifiziert und, indem Sie Ihre Mobile-App autorisiert, und Sie werden Tabellendaten eingeschränkter Zugriff auf.

##<a name="prerequisites"></a>Erforderliche Komponenten

Für das beste Ergebnis mit diesem Lernprogramm empfehlen wir, dass Abschluss des Lernprogramms [eine Xamarin.Forms app zu erstellen](app-service-mobile-xamarin-forms-get-started.md) . Nach Abschluss dieses Lernprogramms müssen Sie ein Projekt Xamarin.Forms, die eine Multi-Plattform Aufgabenliste app handelt.

Wenn Sie die heruntergeladene Schnellstart Server Project nicht verwenden, müssen Sie das Authentifizierung Erweiterungspaket zum Projekt hinzufügen. Weitere Informationen zu Server Erweiterung Paketen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Registrieren Sie Ihre app für die Authentifizierung und Konfigurieren von App-Diensten

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Einschränken der Berechtigungen für authentifizierte Benutzer

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]


##<a name="add-authentication-to-the-portable-class-library"></a>Hinzufügen von Authentifizierung zur Klassenbibliothek portable

Mobile-Apps verwendet die [LoginAsync] Erweiterungsmethode auf die [MobileServiceClient] , um ein Benutzer mit der Authentifizierung des App-Dienst anmelden. In diesem Beispiel wird einen Authentifizierung Server verwaltete Fluss, der der Anbieter Anmeldung Benutzeroberfläche in der app angezeigt werden. Weitere Informationen finden Sie unter [Authentifizierung Server verwaltet](app-service-mobile-dotnet-how-to-use-client-library.md#serverflow). Um Benutzerfunktionalität in der Herstellung app bereitstellen zu können, sollten Sie stattdessen mithilfe der [Authentifizierung Desktopclient verwaltet](app-service-mobile-dotnet-how-to-use-client-library.md#clientflow). 

Für die Authentifizierung mit einem Projekt Xamarin.Forms definieren Sie eine **IAuthenticate** -Benutzeroberfläche in der tragbaren Class-Bibliothek, für die app aus. Sie auch aktualisieren die Benutzeroberfläche in der tragbaren Klassenbibliothek eine **Anmelden** Schaltfläche hinzufügen, die der Benutzer klickt, um Authentifizierung starten definiert. Nach der erfolgreichen Authentifizierung werden die Daten aus der mobilen app Back-End-geladen.

Sie müssen für jede Plattform unterstützt, indem Sie Ihre app Schnittstelle **IAuthenticate** implementieren.


1. In Visual Studio oder Xamarin Studio, fügen Sie öffnen App.cs aus dem Projekt mit **tragbaren** in den Namen, das tragbare Class Library-Projekt befindet, klicken Sie dann den folgenden `using` Anweisung:

        using System.Threading.Tasks;

2. Fügen Sie folgende App.cs, `IAuthenticate` Definition unmittelbar vor Schnittstelle der `App` definierte Klasse.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }

3. Fügen Sie die folgenden statischen Mitglieder der **App** -Klasse Initialisierung die Schnittstelle mit einer bestimmten Plattform-Implementierung hinzu.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }

4. TodoList.xaml aus dem Projekt tragbares Class-Bibliothek zu öffnen, das folgende **Schaltfläche** Element in das Element *ButtonsPanel* Layout nach der vorhandenen Schaltfläche hinzufügen: 

        <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30" 
            Clicked="loginButton_Clicked"/>

    Die Schaltfläche löst Authentifizierung mit Ihrem mobilen app Back-End-Server verwaltet.

5. TodoList.xaml.cs aus dem Projekt tragbares Class-Bibliothek zu öffnen, und fügen Sie das folgende Feld in der `TodoList` Klasse:

        // Track whether the user has authenticated. 
        bool authenticated = false;


6. Ersetzen Sie die Methode **OnAppearing** mit den folgenden Code ein:

        protected override async void OnAppearing()
        {
            base.OnAppearing();
    
            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data 
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    Dadurch wird sichergestellt, dass die Daten aus dem Dienst nur aktualisiert werden, nachdem der Benutzer authentifiziert wurde.

7. Fügen Sie den folgenden Ereignishandler für das Ereignis **Clicked** zur Klasse **Aufgabenliste** ein:

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }

8. Die Änderungen zu speichern und Projekt tragbares Class Library Überprüfung keine Fehler neu erstellen.


##<a name="add-authentication-to-the-android-app"></a>Hinzufügen von Authentifizierung zur Android-app

In diesem Abschnitt veranschaulicht, wie die Benutzeroberfläche **IAuthenticate** im Projekt Android app implementieren. Überspringen Sie diesen Abschnitt, wenn Sie Android-Geräten nicht unterstützt werden.

1. In Visual Studio oder Xamarin Studio und mit der rechten Maustaste **Droid** Projekt, klicken Sie dann **als beim Start-Projekt festlegen**.

2. Drücken Sie F5, um das Projekt im Debugger starten, und klicken Sie dann stellen Sie sicher, dass Ausnahmefehler mit einem Statuscode von 401 (nicht autorisiert) ausgelöst wird, nachdem die app gestartet wird. In diesem Fall, da der Zugriff auf die Back-End-auf nur autorisierte Benutzer beschränkt ist.

3. Öffnen Sie MainActivity.cs im Android Projekt, und fügen Sie den folgenden `using` Anweisungen:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Aktualisieren Sie die **MainActivity** -Klasse, um die Schnittstelle **IAuthenticate** implementieren, wie folgt:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate


5. Aktualisieren Sie die **MainActivity** Klasse durch Hinzufügen eines Felds **MobileServiceUser** und **einer Authentifizierungsmethode, das über die **IAuthenticate** -Schnittstelle, wie folgt benötigt wird** aus:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this,
                    MobileServiceAuthenticationProvider.Facebook);
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }


    Wenn Sie mit einen Identitätsanbieter als Facebook arbeiten, wählen Sie einen anderen Wert für [MobileServiceAuthenticationProvider].

6. Fügen Sie den folgenden Code in der Klasse **MainActivity** vor den Anruf an die **OnCreate** -Methode `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    Dadurch wird sichergestellt, dass der Authentifizierung vor dem Laden des app Initialisierung.

7. Die app neu zu erstellen, auszuführen und dann Anmeldung mit der Authentifizierungsanbieter ausgewählt haben, und stellen Sie sicher, dass Sie die Daten als authentifizierter Benutzer zugreifen können.

##<a name="add-authentication-to-the-ios-app"></a>Hinzufügen von Authentifizierung zur iOS-app

In diesem Abschnitt wird gezeigt, wie Schnittstelle **IAuthenticate** in iOS-app-Projekt implementiert werden. Überspringen Sie diesen Abschnitt, wenn iOS-Geräte nicht unterstützt werden.

1. In Visual Studio oder Xamarin Studio und mit der rechten Maustaste **iOS** Projekt, klicken Sie dann **als beim Start-Projekt festlegen**.

2. Drücken Sie F5, um das Projekt im Debugger starten, und klicken Sie dann stellen Sie sicher, dass Ausnahmefehler mit einem Statuscode von 401 (nicht autorisiert) ausgelöst wird, nachdem die app gestartet wird. In diesem Fall, da der Zugriff auf die Back-End-auf nur autorisierte Benutzer beschränkt ist.

4. Öffnen Sie im Projekt iOS AppDelegate.cs, und fügen Sie den folgenden `using` Anweisungen:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Aktualisieren Sie die **AppDelegate** -Klasse, um die Schnittstelle **IAuthenticate** implementieren, wie folgt:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate

5. Aktualisieren Sie die **AppDelegate** Klasse durch Hinzufügen eines Felds **MobileServiceUser** und **einer Authentifizierungsmethode, das über die **IAuthenticate** -Schnittstelle, wie folgt benötigt wird** aus:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;                        
                    }
                }        
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();         

            return success;
        }

    Wenn Sie mit einen Identitätsanbieter als Facebook arbeiten, wählen Sie einen anderen Wert für [MobileServiceAuthenticationProvider].

6. Fügen Sie die folgende Zeile des Codes an die Methode **FinishedLaunching** , bevor Sie den Anruf an `LoadApplication()`: 

        App.Init(this);

    Dadurch wird sichergestellt, dass die Authentifizierung Initialisierung vor dem Laden der app.

7. Die app neu zu erstellen, auszuführen und dann Anmeldung mit der Authentifizierungsanbieter ausgewählt haben, und stellen Sie sicher, dass Sie die Daten als authentifizierter Benutzer zugreifen können.


##<a name="add-authentication-to-windows-app-projects"></a>Authentifizierung für Windows-app-Projekten hinzufügen

Dieser Abschnitt listet wie Windows 8.1 und Windows Phone 8.1 app Projekte Schnittstelle **IAuthenticate** implementiert wird. Die gleichen Schritte gelten für Projekte, die Universal Windows-Plattform (UWP). Überspringen Sie diesen Abschnitt, wenn Sie Windows-Geräten nicht unterstützt werden.

1. In Visual Studio mit der rechten Maustaste entweder die **WinApp** oder **WinPhone81** Projekt, klicken Sie dann **als beim Start-Projekt festlegen**.

2. Drücken Sie F5, um das Projekt im Debugger starten, und klicken Sie dann stellen Sie sicher, dass Ausnahmefehler mit einem Statuscode von 401 (nicht autorisiert) ausgelöst wird, nachdem die app gestartet wird. In diesem Fall, da der Zugriff auf die Back-End-auf nur autorisierte Benutzer beschränkt ist.

3. Öffnen Sie MainPage.xaml.cs für das Windows-app-Projekt, und fügen Sie den folgenden `using` Anweisungen:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Ersetzen Sie `<your_Portable_Class_Library_namespace>` mit dem Namespace für Ihre Bibliothek tragbare Class.

4. Aktualisieren der **MainPage** -Klasse zum Implementieren der Schnittstelle **IAuthenticate** wie folgt:

        public sealed partial class MainPage : IAuthenticate


5. Aktualisieren Sie die **MainPage** -Klasse durch Hinzufügen eines Felds **MobileServiceUser** und **einer Authentifizierungsmethode, das über die **IAuthenticate** -Schnittstelle, wie folgt benötigt wird** aus:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }
                
            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }


    Wenn Sie mit einen Identitätsanbieter als Facebook arbeiten, wählen Sie einen anderen Wert für [MobileServiceAuthenticationProvider].

6. Fügen Sie die folgende Zeile des Codes im Konstruktor für die Klasse **MainPage** , bevor Sie den Anruf an `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);
 
    Ersetzen Sie `<your_Portable_Class_Library_namespace>` mit dem Namespace für Ihre Bibliothek tragbare Class.  
    Wenn sich das Projekt WinApp handelt, können Sie nach unten bis zum Schritt 8 überspringen. Im nächste Schritt gilt nur für WinPhone81 Projekt, wo Sie den Login Rückruf ausführen müssen.

7. (Optional) Im Projekt **WinPhone81** app, öffnen Sie App.xaml.cs, und fügen Sie Folgendes `using` Anweisungen:

        using Microsoft.WindowsAzure.MobileServices;
        using <your_Portable_Class_Library_namespace>;

    Ersetzen Sie `<your_Portable_Class_Library_namespace>` mit dem Namespace für Ihre Bibliothek tragbare Class.

8.  Die folgende **OnActivated** Außerkraftsetzung der **App** -Klasse hinzufügen:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            // We just need to handle activation that occurs after web authentication. 
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Get the client and call the LoginComplete method to complete authentication.
                var client = TodoItemManager.DefaultManager.CurrentClient as MobileServiceClient;
                client.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
        }

    Wenn die Außerkraftsetzung bereits vorhanden ist, fügen Sie bedingten Code nur aus den oben stehenden Ausschnitt.

7. Die app neu zu erstellen, auszuführen und dann Anmeldung mit der Authentifizierungsanbieter ausgewählt haben, und stellen Sie sicher, dass Sie die Daten als authentifizierter Benutzer zugreifen können.

##<a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie in diesem Lernprogramm Standardauthentifizierung abgeschlossen haben, sollten Sie Sie mit einer der folgenden Lernprogramme fortfahren:

+ [Hinzufügen von Pushbenachrichtigungen zu Ihrer Anwendung](app-service-mobile-xamarin-forms-get-started-push.md)  
  Informationen Sie zum Hinzufügen von Pushbenachrichtigungen Benachrichtigungen zu Ihrer Anwendung zu unterstützen und konfigurieren Ihre Mobile-App Back-End-um Azure Benachrichtigung Hubs verwenden, um Pushbenachrichtigungen zu senden.

+ [Offlinesynchronisierung für Ihre app zu aktivieren](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Erfahren Sie, wie offlinesupport Ihre app ein Mobile-App Back-End hinzufügen. Offlinesynchronisierung ermöglicht es den Endbenutzern Interaktion mit einem mobilen app&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;auch, wenn keine Verbindung zum Netzwerk vorhanden ist.

<!-- Images. -->

<!-- URLs. -->
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
[LoginAsync]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[MobileServiceClient]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx

