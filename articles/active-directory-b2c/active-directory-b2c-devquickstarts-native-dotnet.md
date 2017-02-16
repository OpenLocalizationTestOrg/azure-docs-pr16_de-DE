<properties
    pageTitle="Azure-Active Directory B2C | Microsoft Azure"
    description="So erstellen Sie eine Windows-desktop-Anwendung, die Anmeldung, Anmeldung, enthält und Verwaltung von Profilen mithilfe von Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure AD-B2C: Erstellen einer Windows-desktop-app

Mithilfe von B2C Azure Active Directory (Azure AD), können Sie mit der Desktopanwendung in wenigen einfachen Schritten Verwaltungsfunktionen leistungsfähige Self-service-Identität hinzufügen. In diesem Artikel wird gezeigt, wie eine .NET Windows Presentation Foundation (WPF) "Aufgabenliste" app erstellen, die Benutzer Anmeldung, Anmeldung und Verwaltung von Profilen enthält. Die app wird Unterstützung für die Anmeldung und-Anmeldung mithilfe eines Benutzernamen oder e-Mail-enthalten. Es wird ebenfalls Unterstützung für die Anmeldung und-Anmeldung sind, mithilfe von Konten für soziale wie Facebook und Google.

## <a name="get-an-azure-ad-b2c-directory"></a>Abrufen einer Azure AD B2C-Verzeichnis

Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis erstellen, oder den Mandanten.  Ein Verzeichnis ist ein Container für alle Benutzer, apps, Gruppen und mehr. Wenn Sie eine bereits besitzen, [Erstellen Sie ein Verzeichnis B2C](active-directory-b2c-get-started.md) , bevor Sie Sie in diesem Handbuch fortsetzen.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Als Nächstes müssen Sie eine app in Ihrem Verzeichnis B2C erstellen. Dadurch Azure AD-Informationen, die sichere Kommunikation mit Ihrem app benötigt werden. [Wenn Sie eine app erstellen, gehen Sie wie folgt vor.](active-directory-b2c-app-registration.md)  Achten Sie darauf, um:

- Fügen Sie einen **native Client** in die Anwendung.
- Kopieren Sie die **URI umleiten** `urn:ietf:wg:oauth:2.0:oob`. Es ist die Standard-URL für dieses Codebeispiel.
- Kopieren Sie die **ID der Anwendung** , die zu Ihrer Anwendung zugeordnet ist. Sie werden diese später benötigen.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen Sie Ihre Richtlinien

In Azure AD B2C wird jede Erfahrungen von einer [Richtlinie](active-directory-b2c-reference-policies.md)definiert. In diesem Codebeispiel enthält drei Identität Erfahrung: registrieren, melden Sie sich und Profil bearbeiten. Sie müssen zum Erstellen einer Richtlinie für jedes Typs wie in der [Richtlinie Bezug Artikel](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)beschrieben. Wenn Sie die drei Richtlinien erstellen, müssen Sie unbedingt:

- Wählen Sie entweder **Benutzer-ID anmelden** oder **Anmelde-e-Mail** in das Identität Anbieter Blade aus.
- Wählen Sie in Ihrer Anmeldung Richtlinie **Anzeigenamen** und andere Attribute Anmeldung aus.
- Wählen Sie als Anwendung Ansprüche für jede Richtlinie **Anzeigenamen** und **Objekt-ID** Ansprüche aus. Sie können auch anderen Ansprüche auswählen.
- Kopieren Sie den **Namen** der einzelnen Richtlinie, nachdem Sie es erstellt haben. Es sollte das Präfix haben `b2c_1_`.  Sie benötigen diese Richtliniennamen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die drei Richtlinien erfolgreich erstellt haben, sind Sie bereit sind, Ihre app zu erstellen.

## <a name="download-the-code"></a>Laden Sie den code

Der Code für dieses Lernprogramm [auf GitHub verwaltet wird](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet). Wenn Sie um das Beispiel beim Durcharbeiten zu erstellen, können Sie [ein Gerüst Projekt als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). Sie können auch das Gerüst Klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

Die fertige app steht auch [als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) oder das `complete` Zweig des gleichen Repositorys.

Nachdem Sie den Code Stichprobe heruntergeladen haben, öffnen Sie die Visual Studio sln-Datei, um anzufangen. Die `TaskClient` Projekt ist die WPF-desktop-Anwendung, die mit der Benutzer interagiert. Für die Zwecke dieses Lernprogramms wird eine Vorgang Back-End-Web-API in Azure, die Aufgabenliste des Benutzers speichert gehostet wird.  Sie müssen nicht im Web-API erstellen, die wir bereits für Sie ausführen.

Schauen Sie sich das [Web API überfordert Artikel](active-directory-b2c-devquickstarts-api-dotnet.md), um zu erfahren, wie eine Web-API sicher Anfragen authentifiziert mithilfe von Azure AD B2C.

## <a name="execute-policies"></a>Ausführen von Richtlinien
Ihre app kommuniziert mit Azure AD B2C per Authentifizierungsnachrichten, die die Richtlinie angeben, die sie als Teil der HTTP-Anforderung ausführen möchten. Für .NET desktopanwendungen können Sie mit der Vorschau Microsoft Authentifizierung Bibliothek (MSAL) können OAuth 2.0 Authentifizierungsnachrichten zu senden, Richtlinien ausführen und erste Token, die Web-APIs aufrufen.

### <a name="install-msal"></a>Installieren von MSAL
Hinzufügen von MSAL zu den `TaskClient` Projekt mithilfe der Verwaltungskonsole für Visual Studio-Paket-Manager.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Geben Sie Ihre Daten B2C
Öffnen Sie die Datei `Globals.cs` und jeder der Eigenschaftswerte durch ein eigenes ersetzen. Diese Klasse wird verwendet, in der gesamten `TaskClient` Bezug gängige Werte.

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]


### <a name="create-the-publicclientapplication"></a>Erstellen der PublicClientApplication
Ist die primäre Klasse der MSAL `PublicClientApplication`. Diese Klasse stellt eine Anwendung im Azure AD B2C-System. Beim Erstellen der app initialisiert einer Instanz der `PublicClientApplication` in `MainWindow.xaml.cs`. Dies kann in der gesamten im Fenster verwendet werden.

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app, 
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };
    
    ...
```

### <a name="initiate-a-sign-up-flow"></a>Einleiten eines Anmeldung Fluss
Wenn ein Benutzer auf Vorzeichen von-Optionen, einen Anmeldung Fluss einleiten, der die Anmelde Richtlinie, die Sie erstellt haben, verwendet werden soll. Mithilfe von MSAL, rufen Sie einfach `pca.AcquireTokenAsync(...)`. Die Parameter, die Sie zu übergeben `AcquireTokenAsync(...)` bestimmen, welche Token Sie erhalten, die Richtlinie in die Authentifizierungsanfrage und vieles mehr verwendet werden.

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user 
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
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

### <a name="initiate-a-sign-in-flow"></a>Einleiten eines Anmeldung Fluss
Sie können die gleiche Weise wie einen Fluss Anmeldung initiieren, dass Sie einen Anmeldung Fluss einleiten. Wenn sich der Benutzer anmeldet, stellen Sie den gleichen Anruf an MSAL, diesmal mithilfe Ihrer Richtlinie Anmeldung:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>Einleiten einer Fluss Profil bearbeiten
In diesem Fall können Sie eine Richtlinie Profil bearbeiten auf die gleiche Weise ausführen:

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

In allen diesen Fällen gibt MSAL entweder ein Token im `AuthenticationResult` oder eine Ausnahme auslösen. Jedes Mal, wenn Sie ein Token aus MSAL, abrufen, die Sie verwenden die `AuthenticationResult.User` Objekt die Benutzerdaten in der app, wie etwa die Benutzeroberfläche aktualisieren. ADAL werden auch das Token für die Verwendung in anderen Teile der Anwendung zwischengespeichert.


### <a name="check-for-tokens-on-app-start"></a>Überprüfen Sie unter Start eine app Token
Sie können auch MSAL verwenden, um den Status des Benutzers Anmeldung verfolgen.  Bei dieser app möchten wir den Benutzer angemeldet zu bleiben, auch nachdem sie die app schließen und öffnen Sie es erneut.  Wieder innerhalb der `OnInitialized` außer Kraft setzen, verwenden Sie die MSAL `AcquireTokenSilent` Methode, um zu prüfen, ob Cache Token:

```C#
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
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
```

## <a name="call-the-task-api"></a>Rufen Sie die Aufgabe API
Sie haben nun MSAL zum Ausführen von Richtlinien sowie zum Abrufen von Token verwendet.  Wenn Sie eine dieser Token aufrufen, die Aufgabe API verwenden möchten, können Sie erneut verwenden des MSAL `AcquireTokenSilent` Methode, um zu prüfen, ob Cache Token:

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
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
    ...
```

Wenn des Anrufs an die `AcquireTokenSilentAsync(...)` erfolgreich ist und ein Token im Cache gefunden wird, können Sie das Token zum Hinzufügen der `Authorization` Kopfzeile der HTTP-Anforderung. Das Vorgang Web API wird diese Kopfzeile zum Authentifizieren des Benutzers Vorgangsliste Lesen der Anforderung verwendet:

```C#
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a>Den Benutzer abmelden
Schließlich können Sie MSAL zum Beenden der Sitzung eines Benutzers mit der app aus, wenn der Benutzer **Abmelden auswählt**aus.  Bei der Verwendung von MSAL geschieht dies, indem Sie alle der Token aus dem Cache token deaktivieren:

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a>Führen Sie die Beispielapp

Schließlich erstellen Sie, und führen Sie das Beispiel.  Registrieren Sie sich mithilfe eines e-Mail-Adresse oder die Benutzer die Namens für die app an. Melden Sie sich ab und melden Sie sich wieder unter demselben Benutzernamen. Bearbeiten Sie das Profil der Benutzer ein. Melden Sie sich ab, und melden Sie sich mit einem anderen Benutzer.

## <a name="add-social-idps"></a>Hinzufügen von IDPs für soziale Netzwerke

Die app unterstützt derzeit nur Benutzer Anmeldung und-Anmeldung, die **lokale Konten**verwenden. Hierbei handelt es sich um Konten in Ihrem Verzeichnis B2C gespeichert, die einen Benutzernamen und ein Kennwort verwenden. Mithilfe von Azure AD B2C, können Sie Unterstützung für andere Identitätsanbieter (IDPs) hinzufügen, ohne ein Teil des Codes ändern.

Beginnen Sie anhand der detaillierten Anweisungen in folgenden Artikeln, um Ihre app für soziale Netzwerke IDPs hinzuzufügen. Für jede IDP unterstützt werden soll, müssen Sie eine Anwendung registrieren, in dem jeweiligen System und erhalten eine Client-ID an.

- [Einrichten von Facebook als eine IDP](active-directory-b2c-setup-fb-app.md)
- [Einrichten von Google als eine IDP](active-directory-b2c-setup-goog-app.md)
- [Einrichten von Amazon als eine IDP](active-directory-b2c-setup-amzn-app.md)
- [Einrichten von LinkedIn als eine IDP](active-directory-b2c-setup-li-app.md)

Nachdem Sie die Identitätsanbieter in Ihrem Verzeichnis B2C hinzugefügt haben, müssen Sie jeder der drei Richtlinien zum Einschließen der neuen IDPs bearbeiten, wie in der [Richtlinie Bezug Artikel](active-directory-b2c-reference-policies.md)beschrieben. Nachdem Sie Ihre Richtlinien gespeichert haben, führen Sie die app erneut aus. Folgendes sollte angezeigt werden die neuen IDPs als Anmeldung hinzugefügt und Anmeldeoptionen in jedem der Ihre Identität auftritt.

Können Sie experimentieren Sie mit Ihren Richtlinien und Effekte auf Ihre app Stichprobe beobachten. Fügen Sie hinzu oder entfernen Sie IDPs, manipulieren Sie Anwendung Ansprüche oder ändern Sie Anmeldung Attribute. Experimentieren Sie, bis Sie sehen können, wie Richtlinien, Authentifizierung und MSAL gemeinsam einbinden.

Als Referenz der vollständigen Beispiel [als ZIP-Datei bereitgestellt wird](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). Sie können auch aus GitHub Klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
