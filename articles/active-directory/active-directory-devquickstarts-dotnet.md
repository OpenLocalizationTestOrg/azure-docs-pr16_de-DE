<properties
    pageTitle="Erste Schritte mit Azure AD-.NET | Microsoft Azure"
    description="So erstellen Sie eine .NET Windows-Desktop-Anwendung, die für die Anmeldung Azure AD integriert und Azure AD-Anrufe geschützt APIs OAuth verwenden."
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>In einer Desktopversion von WPF-App Windows Azure AD integriert werden soll

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Wenn Sie eine desktop-Anwendung entwickeln, macht Azure AD einfach und unkompliziert für Sie Ihre Benutzer mit ihren Active Directory-Konten authentifizieren.  Darüber hinaus können die Anwendung zu einem beliebigen Web-API von Azure AD, wie die Office 365-APIs oder der Azure-API geschützten sicher nutzen.

Für systemeigene .NET-Clients, die Zugriff auf geschützte Ressourcen benötigen, bietet Azure AD an den Active Directory-Authentifizierungsbibliothek oder ADAL.  ADAL des alleinige Zweck im Leben ist für Ihre app abzurufenden Zugriffstoken zu erleichtern.  Um veranschaulichen, wie einfach es ist, wird hier wir einer Aufgabenliste für .NET WPF-Anwendung, die erstellen:

-   Ruft Token für das Anrufen von der Azure AD Graph-API mit dem [OAuth 2.0-Authentifizierung-Protokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx)zugegriffen werden.
-   Sucht in einem Verzeichnis für Benutzer mit einer angegebenen Alias.
-   Vorzeichen Benutzer ab.

Wenn Sie um die gesamte Arbeit der Anwendung zu erstellen, müssen Sie:

2. Registrieren Sie Ihrer Anwendung Azure AD.
3. Installieren und Konfigurieren von ADAL.
5. Verwenden Sie ADAL Token aus Azure AD abgerufen.

Um zu beginnen, [Laden Sie das app-Gerüst](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) oder [Laden Sie das Beispiel fertige](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Sie benötigen auch einem Azure AD-Mandanten, in dem Sie Benutzer erstellen und Registrieren einer Anwendung.  Wenn Sie noch keinen Mandanten, [erfahren Sie, wie Sie eins zu erhalten](active-directory-howto-tenant.md)haben.

## <a name="1-register-the-directorysearcher-application"></a>*1. die Anwendung DirectorySearcher zu registrieren*
Wenn Ihre app Token abrufen aktivieren möchten, müssen Sie zuerst in Ihrem Azure AD-Mandanten zu registrieren, und gewähren sie über die Berechtigung zum Zugreifen auf der Azure AD Graph-API:

-   Melden Sie sich bei der Azure-Verwaltungsportal
-   Klicken Sie im linken Navigationsbereich auf **Active Directory**
-   Wählen Sie einen Mandanten, in dem die Anwendung registrieren aus.
-   Klicken Sie auf die Registerkarte **Applications** aus, und klicken Sie in der unteren Einzug auf **Hinzufügen** .
-   Folgen Sie den Anweisungen, und erstellen Sie eine neue **Native Client-Anwendung**.
    -   Den **Namen** der Anwendung wird eine Anwendung an Endbenutzer beschrieben.
    -   **Umleiten Uri** ist eine Zeichenfolge und Farbschema Kombination, mit denen Azure AD token Antworten zurückgeben.  Geben Sie einen bestimmten Wert an Ihrer Anwendung, z. b `http://DirectorySearcher`.
-   Nachdem Sie die Registrierung abgeschlossen haben, zuweisen AAD Ihre app eine eindeutige Client-ID.  Sie benötigen diesen Wert in den nächsten Abschnitten, also kopieren Sie sie von der Registerkarte **Konfigurieren** .
- Registerkarte **Konfigurieren** , suchen Sie auch im Abschnitt "Berechtigungen zu anderen Applications".  Fügen Sie die Berechtigung **Directory Access Ihrer Organisation** , klicken Sie unter **Delegierte Berechtigungen**für die Anwendung "Azure Active Directory" hinzu.  Dadurch wird eine Anwendung Abfragen die Graph-API für Benutzer aktiviert werden.

## <a name="2-install--configure-adal"></a>*2. installieren und Konfigurieren von ADAL*
Jetzt, da Sie eine Anwendung in Azure AD haben, können Sie ADAL installieren und Schreiben von Code Identität Zusammenhang.  In der Reihenfolge für ADAL mit Azure AD kommunizieren können müssen Sie es einige Informationen über Ihre app-Registrierung zur Verfügung zu stellen.
-   Durch Hinzufügen von ADAL zum Verwenden der Paket-Manager-Konsole DirectorySearcher Projekt beginnen.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Öffnen Sie im Projekt DirectorySearcher `app.config`.  Ersetzen Sie die Werte der Elemente in der `<appSettings>` Abschnitt zu den Werten, die Sie in einer Azure-Portal eingeben.  Code werden diese Werte verwiesen werden, wenn es ADAL verwendet.
    -   Die `ida:Tenant` ist die Domäne Ihrer Azure AD-Mandanten, z. B. contoso.onmicrosoft.com
    -   Die `ida:ClientId` ClientId Ihrer Anwendung, die Sie kopiert, auf dem Portal haben ist.
    -   Die `ida:RedirectUri` ist die Umleitung Url, die Sie im Portal registriert.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. ADAL abzurufenden Token aus AAD verwenden*
Das grundlegende Prinzip hinter ADAL ist, wenn Ihre app ein Access-Token benötigt, sie einfach ruft `authContext.AcquireTokenAsync(...)`, und ADAL erledigt den Rest.  

-   In der `DirectorySearcher` geöffneten Projekt `MainWindow.xaml.cs` , und suchen Sie die `MainWindow()` Methode.  Dieser erste Schritt besteht Ihre app Initialisierung `AuthenticationContext` -ADAL primäre Klasse ist.  Dies ist, wo Sie ADAL übergeben die Koordinaten Kommunikation mit Azure AD- und feststellen, wie Token zwischengespeichert werden muss.

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

- Suchen Sie jetzt die `Search(...)` Methode, die aufgerufen wird, wenn der Benutzer die "Suche" Cliks des app Benutzeroberfläche Schaltfläche.  Diese Methode stellt eine GET-Anforderung der Azure AD Graph-API Abfrage für Benutzer, dessen UPN mit der angegebenen Suchbegriff beginnt.  Um die Graph-API Abfragen, Sie müssen jedoch eine Access_token in können die `Authorization` Kopfzeile der Anfrage – hier kommt ADAL in.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
- Wenn Ihre app ein Token fordert, indem er `AcquireTokenAsync(...)`, ADAL versucht, ein Token zurück, ohne Bestätigung für Anmeldeinformationen.  ADAL fest, dass es sich bei der Benutzer muss sich anmelden, ein Token abzurufen, wird es ein Anmeldedialogfeld angezeigt, die Anmeldeinformationen des Benutzers sammeln und Token nach der erfolgreichen Authentifizierung zurückgibt.  Wenn ADAL aus irgendeinem Grund ein Token zurück kann, löst eine `AdalException`.
- Beachten Sie, dass die `AuthenticationResult` -Objekt enthält eine `UserInfo` Objekt, das zum Sammeln von Informationen möglicherweise Sie Ihre app müssen verwendet werden kann.  In der DirectorySearcher `UserInfo` zum Anpassen des app Benutzeroberfläche mit-Id des Benutzers verwendet wird.

- Wenn der Benutzer auf die Schaltfläche "Abmelden" klickt, möchten wir sicherstellen, dass auf den nächsten Anruf an `AcquireTokenAsync(...)` wird bitten Sie den Benutzer anmelden.  Mit ADAL ist dies so einfach wie das Löschen des Caches token:

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

- Wenn die Schaltfläche "Abmelden" auf der Benutzer nicht klicken, sollten Sie die Sitzung des Benutzers für das nächste Mal beibehalten möchten, die sie der DirectorySearcher ausgeführt werden.  Wenn die app gestartet wird, können Sie ADAL des Sicherheitstokens Cache für einen vorhandenen Token überprüfen und die Benutzeroberfläche entsprechend aktualisiert.  In der `CheckForCachedToken()` Methode, rufen Sie `AcquireTokenAsync(...)`, diesmal übergeben, der `PromptBehavior.Never` Parameter.  `PromptBehavior.Never`wird die ADAL angezeigt, dass der Benutzer nicht für die Anmeldung aufgefordert werden sollten, und ADAL stattdessen eine Ausnahme auslösen sollte, ist es kein Token zurückgegeben.

```C#
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

Herzlichen Glückwunsch! Sie jetzt ein .NET WPF-Anwendung, die die Möglichkeit zum Authentifizieren von Benutzern, sicheres Aufrufen von Web-APIs mit OAuth 2.0 enthält nicht funktioniert haben und Abrufen grundlegender Informationen über den Benutzer.  Wenn Sie noch nicht geschehen ist, ist jetzt die Zeit in Ihrem Mandanten für einige Benutzer gefüllt wird.  Führen Sie die DirectorySearcher-app, und melden Sie sich mit einem dieser Benutzer.  Suchen Sie nach anderen Benutzern basierend auf ihrem Benutzerprinzipalnamen.  Schließen Sie die app, und führen Sie erneut aus.  Beachten Sie, wie die Sitzung des Benutzers intakt bleibt.  Melden Sie sich ab, und melden Sie sich als ein anderer Benutzer wieder.

ADAL erleichtert die all diese allgemeine Identität Features in Ihrer Anwendung zu integrieren.  Es übernimmt alle Prozesse laufen für Sie - Cache-Verwaltung, OAuth Protokoll Support Vorführung des Benutzers mit einer Login UI, Aktualisieren von abgelaufenen Token und vieles mehr.  Sie wirklich wissen müssen lediglich einen einzelnen API Anruf `authContext.AcquireTokenAsync(...)`.

Als Referenz die fertige Stichprobe (ohne die Konfigurationswerte) zur Verfügung gestellt wird [hier](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Sie können jetzt in zusätzliche Szenarien verschieben, klicken Sie auf.  Möglicherweise möchten versuchen:

[Sichern eine .NET Web-API mit Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
