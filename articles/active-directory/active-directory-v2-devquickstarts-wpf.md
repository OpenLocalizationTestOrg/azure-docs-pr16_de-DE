<properties
pageTitle="Azure-Active Directory Version 2.0 .NET systemeigenen App | Microsoft Azure"
description="So erstellen Sie eine systemeigene .NET-app, die Benutzer, die mit beide persönliche Microsoft Account anmeldet und geschäftlichen oder schulnotizbücher Konten."
services="active-directory"
documentationCenter=""
authors="dstrockis"
manager="mbaldwin"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="dotnet"
ms.topic="article"
ms.date="07/30/2016"
ms.author="dastrock; vittorib"/>

# <a name="add-sign-in-to-a-windows-desktop-app"></a>Anmeldung bei einem Windows-Desktop app hinzufügen

Mit der Version 2.0-Endpunkt, können Sie schnell Authentifizierung auf Ihrem desktop-apps mit Unterstützung für beide persönlichen Microsoft-Konten und geschäftlichen oder schulnotizbücher Konten hinzufügen.  Sie können auch Ihre app sicher mit einer Back-End-Web-api als auch [Die Microsoft Graph](https://graph.microsoft.io) kommunizieren und einige der [Office 365 Unified APIs](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).

> [AZURE.NOTE] Nicht alle Szenarien Azure Active Directory (AD) und Funktionen werden von den Endpunkt Version 2.0 unterstützt.  Um festzustellen, ob den Version 2.0-Endpunkt verwendet werden sollen, erfahren Sie, [Version 2.0 Einschränkungen](active-directory-v2-limitations.md).

Für [.NET systemeigenen apps, die auf einem Gerät ausführen](active-directory-v2-flows.md#mobile-and-native-apps)bietet Azure AD an den Microsoft Identität Authentifizierungsbibliothek oder MSAL.  MSAL der einzige Zweck im Leben ist für Ihre app abzurufenden Token-Webdienste aufrufen zu erleichtern.  Um veranschaulichen, wie einfach es ist, wird hier wir einer Aufgabenliste für .NET WPF-app, die erstellen:

- Den Benutzer anmeldet und ruft zugreifen Token mit dem [OAuth 2.0 Authentication-Protokoll](active-directory-v2-protocols.md#oauth2-authorization-code-flow).
- Sichere Anrufe Back-End-Vorgangsliste-Webdienst, der auch durch OAuth 2.0 gesichert wird.
- Bei der Benutzer abgemeldet.

## <a name="download-sample-code"></a>Beispiel-Code herunterladen

Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet)verwaltet.  Wenn Sie nachvollziehen möchten, können Sie Sie [Herunterladen des app Gerüst als eine ZIP](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) oder das Gerüst Klonen:

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

Die fertige app wird am Ende dieses Lernprogramms ebenfalls bereitgestellt.

## <a name="register-an-app"></a>Registrieren einer app
Erstellen Sie eine neue app bei [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), oder gehen Sie folgendermaßen [detaillierten Schritte](active-directory-v2-app-registration.md).  Vergewissern Sie sich an:

- Kopieren nach unten die **Id der Anwendung** , die Ihre app zugewiesen ist, Sie benötigen diese Informationen verfügbar.
- Fügen Sie die **Mobile** -Plattform für Ihre app hinzu.

## <a name="install--configure-msal"></a>Installieren und Konfigurieren von MSAL
Jetzt, da Sie eine app für Microsoft registriert haben, können Sie MSAL installieren und Schreiben von Code Identität Zusammenhang.  In der Reihenfolge für MSAL den Endpunkt Version 2.0 kommunizieren können müssen Sie es einige Informationen über Ihre app-Registrierung zur Verfügung zu stellen.

-   Durch Hinzufügen von MSAL zu TodoListClient Projekt mithilfe der Verwaltungskonsole Paket-Manager zu beginnen.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

-   Öffnen Sie das Projekt TodoListClient `app.config`.  Ersetzen Sie die Werte der Elemente in der `<appSettings>` Abschnitt zu den Werten, die Sie in der Registrierung app-Portal eingeben.  Code werden diese Werte verwiesen werden, wenn es MSAL verwendet.
    -   Die `ida:ClientId` ist die **Id der Anwendung** , der Sie kopiert, auf dem Portal haben app.

- Öffnen Sie in der Aufgabenliste-Service-Projekt `web.config` im Stammverzeichnis des Projekts.  
    - Ersetzen Sie die `ida:Audience` Wert mit der gleichen **Id der Anwendung** auf dem Portal.

## <a name="use-msal-to-get-tokens"></a>Verwenden von MSAL Token abrufen
Das grundlegende Prinzip hinter MSAL ist, wenn Ihre app ein Access-Token benötigt, Sie einfach rufen `app.AcquireToken(...)`, und MSAL erledigt den Rest.  

-   In der `TodoListClient` geöffneten Projekt `MainWindow.xaml.cs` , und suchen Sie die `OnInitialized(...)` Methode.  Dieser erste Schritt besteht Ihre app Initialisierung `PublicClientApplication` -MSALs primären Klasse systemeigene Applikationen darstellt.  Dies ist die Stelle, an der Sie MSAL die Koordinaten übergeben, die Kommunikation mit Azure AD- und feststellen, wie Token zwischengespeichert werden muss.

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

- Wenn die app gestartet wird, möchten wir überprüfen, ob der Benutzer bereits bei der app angemeldet ist.  Jedoch möchten wir nicht nur noch eine Anmeldung Benutzeroberfläche aufrufen - Wir stellen "Anmelden" dazu klicken.  Auch in der `OnInitialized(...)` Methode:

```C#
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.
    
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

- Wenn der Benutzer nicht angemeldet ist, und sie klicken Sie auf die Schaltfläche "Anmelden", möchten wir eine Anmeldung Benutzeroberfläche aufzurufen, und lassen Sie die Benutzer ihre Anmeldeinformationen eingeben.  Implementieren Sie den Ereignishandler anmelden Schaltfläche an:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }

        MessageBox.Show(message);
    }

    return;
}


}
```

- Wenn der Benutzer erfolgreich Vorzeichen-in, MSAL empfängt und ein Token für Sie zwischenzuspeichern und Nummer fortgesetzt werden können die `GetTodoList()` Methode ohne Sicherheitsrisiko.  Zum Abrufen eines Benutzers Aufgaben offen steht lediglich zum Implementieren der `GetTodoList()` Methode.

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If the user clicked the 'clear cache' button,
        // clear the MSAL token cache and show the user as signed out.
        // It's also necessary to clear the cookies from the browser
        // control so the next user has a chance to sign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a>Ausführen

Herzlichen Glückwunsch! Sie verfügen jetzt über eine arbeiten .NET WPF-app, die die Möglichkeit zum Authentifizieren von Benutzern und sicheres Aufrufen von Web-APIs mit OAuth 2.0 enthält.  Führen Sie beide Projekte, und melden Sie sich mit entweder ein persönliches Microsoft-Konto oder einem geschäftlichen oder schulnotizbücher-Konto.  Hinzufügen von Aufgaben zur Aufgabenliste des Benutzers.  Melden Sie sich ab, und melden Sie sich wieder einzuchecken als ein anderer Benutzer zu ihrer Vorgangsliste anzeigen.  Schließen Sie die app, und führen Sie erneut aus.  Beachten Sie, wie die Sitzung des Benutzers bleibt erhalten – das ist, da die app Token in einer lokalen Datei speichert.

MSAL erleichtert das allgemeine Identität Features in Ihrer app integrieren mit persönlichen und geschäftlichen Konten.  Es übernimmt alle Prozesse laufen für Sie - Cache-Verwaltung, OAuth Protokoll Support Vorführung des Benutzers mit einer Login UI, Aktualisieren von abgelaufenen Token und vieles mehr.  Sie wirklich wissen müssen lediglich einen einzelnen API Anruf `app.AcquireTokenAsync(...)`.

Als Referenz können die vollständigen Beispiel (ohne die Konfigurationswerte) [als eine ZIP hier bereitgestellt wird](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), oder Sie es GitHub zu duplizieren:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>Nächste Schritte

Sie können nun auf Erweiterte Themen verschieben.  Möglicherweise möchten versuchen:

- [Schutz der TodoListService Web-API mit dem Endpunkt Version 2.0](active-directory-v2-devquickstarts-dotnet-api.md)

Zusätzliche Ressourcen Auschecken:  

- [Die Version 2.0 Entwicklertools Leitfaden >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "Msal" Kategorie >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unseren Produkten

Wir empfehlen Ihnen erhalten von Benachrichtigungen Sicherheitsvorfälle auftreten, besuchen [Diese Seite](https://technet.microsoft.com/security/dd252948) und von Ihren Sicherheitshinweisen abonnieren.
