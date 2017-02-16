<properties
    pageTitle="Azure AD-Version 2.0 .NET Web App | Microsoft Azure"
    description="So erstellen Sie eine .NET MVC Web App die Anrufe Web services mit persönlichen Microsoft-Konten und der Arbeit oder Schule Konten für die Anmeldung."
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
    ms.date="10/27/2016"
    ms.author="dastrock"/>

# <a name="calling-a-web-api-from-a-net-web-app"></a>Aufrufen einer Webs-API aus einer .NET Web app

Mit den Endpunkt Version 2.0 können Sie schnell Authentifizierung für Ihre Web apps hinzufügen und web-APIs mit Unterstützung für beide persönlichen Microsoft-Konten und geschäftlichen oder schulnotizbücher Konten.  Hier werden wir eine MVC Web app erstellen, die Benutzer in die Verwendung von OpenID verbinden, mit einigen Hilfe von Microsoft OWIN Middleware signiert.  Das Web app wird erhalten OAuth 2.0 Access Token für ein Web-api durch OAuth 2.0, die ermöglicht gesicherte erstellen, lesen und löschen auf einen bestimmten Benutzers "Aufgabenliste".

In diesem Lernprogramm wird Schwerpunkt MSAL abrufen und Verwenden von Access Token in einer Web-app, vollständige [hier](active-directory-v2-flows.md#web-apps)beschrieben verwenden.  Als Voraussetzung, möchten Sie möglicherweise zuerst erfahren Sie, wie Sie [grundlegende Anmelden bei einer Web app hinzufügen](active-directory-v2-devquickstarts-dotnet-web.md) oder wie [ordnungsgemäß gesichert eine Web-API](active-directory-v2-devquickstarts-dotnet-api.md).

> [AZURE.NOTE]
    Nicht alle Azure Active Directory-Szenarien und Features werden von den Endpunkt Version 2.0 unterstützt.  Um festzustellen, ob den Version 2.0-Endpunkt verwendet werden sollen, erfahren Sie, [Version 2.0 Einschränkungen](active-directory-v2-limitations.md).

## <a name="download-sample-code"></a>Beispiel-Code herunterladen

Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet)verwaltet.  Wenn Sie nachvollziehen möchten, können Sie Sie [Herunterladen des app Gerüst als eine ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) oder das Gerüst Klonen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

Alternativ können Sie [Herunterladen der fertige app als ein ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) oder Klonen die fertige app:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Registrieren einer app
Erstellen Sie eine neue app bei [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), oder gehen Sie folgendermaßen [detaillierten Schritte](active-directory-v2-app-registration.md).  Vergewissern Sie sich an:

- Kopieren nach unten die **Id der Anwendung** , die Ihre app zugewiesen ist, Sie benötigen diese Informationen verfügbar.
- Erstellen Sie einer **App geheim** vom Typ **Kennwort** und ab dessen Wert für später kopieren
- Fügen Sie die **Web** -Plattform für Ihre app hinzu.
- Geben Sie den entsprechenden **URI umleiten**aus. Der Redirect-Uri gibt zu Azure AD Stelle, an der Authentifizierungsantworten geleitet werden – das Standardformat für dieses Lernprogramms ist `https://localhost:44326/`.


## <a name="install-owin"></a>Installieren von OWIN
Hinzufügen der OWIN Middleware NuGet Pakete in der `TodoList-WebApp` project mithilfe der Verwaltungskonsole Paket-Manager.  Anmelden und Abmelde Anfragen Emission, verwalten die Sitzung des Benutzers, und erhalten Informationen über den Benutzer, unter anderem, wird die Middleware OWIN verwendet werden.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a>Den Benutzer anmelden
Konfigurieren Sie jetzt die Middleware OWIN zur Nutzung des [OpenID verbinden Authentication-Protokoll](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  

-   Öffnen der `web.config` Datei im Stammverzeichnis der `TodoList-WebApp` project, und geben Sie Ihre app Konfigurationswerte in der `<appSettings>` Abschnitt.
    -   Die `ida:ClientId` ist die **Id der Anwendung** , die Ihre app in der Registrierung Portal zugewiesen ist.
    - Die `ida:ClientSecret` der **App geheim** , die Sie im Portal Registrierung erstellt wird.
    -   Die `ida:RedirectUri` ist der **Uri umleiten** , die Sie im Portal eingegeben haben.
- Öffnen der `web.config` Datei im Stammverzeichnis der der `TodoList-Service` Projekt, und Ersetzen Sie die `ida:Audience` mit der gleichen **Id der Anwendung** wie oben.


- Öffnen Sie die Datei `App_Start\Startup.Auth.cs` und Hinzufügen von `using` Anweisungen für die Bibliotheken von oben.
- Klicken Sie in der gleichen Datei Implementieren der `ConfigureAuth(...)` Methode.  Die Parameter in werden `OpenIDConnectAuthenticationOptions` dient als Koordinaten für Ihre app zur Kommunikation mit Azure AD.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {

                    // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0 
                    // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                    // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                    ClientId = clientId,
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a>Verwenden Sie zum Abrufen von Access Token MSAL
In der `AuthorizationCodeReceived` Benachrichtigung, die wir [OAuth 2.0 gemeinsam mit OpenID verbinden](active-directory-v2-protocols.md#openid-connect-with-oauth-code-flow) der Authorization_code für eine Access-Token in der Aufgabenleiste Liste Service einlösen verwenden möchten.  MSAL kann dieses Verfahren für Sie erleichtern:

- Installieren Sie zuerst die Vorabversion von MSAL:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```
- Und Hinzufügen einer anderen `using` -Anweisung, um die `App_Start\Startup.Auth.cs` Datei für MSAL.
- Fügen Sie nun eine neue Methode, die `OnAuthorizationCodeReceived` Ereignishandler.  Dieser Ereignishandler wird MSAL verwenden, um ein Access-Token der Aufgabe Liste-API zu erwerben, und das Token wird im MSALs Sicherheitstokens Cache für später speichern:

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

- In der Web apps hat MSAL einen extensible token Cache, der zum Speichern von Token verwendet werden kann.  In diesem Beispiel implementiert die `NaiveSessionCache` die HTTP-Sitzungsspeicher zu Cache Token verwendet.

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a>Rufen Sie das Web-API
Nun ist es an der Zeit, die Access_token tatsächlich zu verwenden, die Sie in Schritt 3 erworben haben.  Öffnen Sie die Web-app `Controllers\TodoListController.cs` Datei, wodurch die CRUD-Anfragen für die Aufgabe Liste-API sind.

- MSAL können erneut hier Sie Access_tokens aus dem Cache MSAL abgerufen werden sollen.  Fügen Sie zunächst ein `using` -Anweisung für MSAL auf diese Datei.

    `using Microsoft.Identity.Client;`

- In der `Index` Aktion verwenden MSAL des `AcquireTokenSilentAsync` Methode, um eine Access_token abzurufen, die zum Lesen von Daten aus der Vorgangsliste Dienst verwendet werden können:

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

- Im Beispiel wird dann das resultierende Token hinzugefügt, auf die HTTP GET-Anforderung als die `Authorization` Kopfzeile, die der Aufgabenliste Dienst zum Authentifizieren der Anforderung verwendet.
- Ist der Vorgangsliste Dienst gibt eine `401 Unauthorized` Antwort, die Access_tokens in MSAL ungültig wurden aus irgendeinem Grund.  In diesem Fall sollten Sie alle Access_tokens aus dem Cache MSAL ablegen und anzeigen dem Benutzer eine Nachricht, die sie möglicherweise müssen, melden Sie sich erneut, die den Datenfluss token Acquisition neu gestartet wird.

```C#
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

- Wenn MSAL einer Access_token aus irgendeinem Grund zurückgeben kann, sollten Sie den Benutzer erneut anmelden anweisen.  Dies ist so einfach wie das Aufspüren von jedem `MSALException`:

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

- Die gleiche eine exakte `AcquireTokenSilentAsync` Anruf ist Implementd in der `Create` und `Delete` Aktionen.  Diese Methode MSAL können Sie im Web apps Access_tokens erhalten, wenn in Ihrer app zu machen.  MSAL übernimmt beim Abrufen und Zwischenspeichern Token für Sie aktualisieren.

Schließlich erstellen Sie, und führen Sie die app!  Melden Sie sich mit entweder ein Microsoft-Account oder ein Azure-Active Directory-Konto, und beachten Sie, wie die Identität des Benutzers in der oberen Navigationsleiste wiedergegeben wird.  Fügen Sie hinzu und löschen Sie einige Elemente aus der Aufgabenliste des Benutzers, um anzuzeigen, dass die OAuth 2.0-API-Aufrufe in Aktion gesichert.  Sie verfügen jetzt über eine Web app und die Web-API beide gesicherte Protokolle nach Industriestandard, die Benutzer mit ihren persönlichen und Arbeit/Schule Konten authentifizieren können.

Als Referenz der vollständigen Beispiel (ohne die Konfigurationswerte) [hier bereitgestellt wird](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).  

## <a name="next-steps"></a>Nächste Schritte

Zusätzliche Ressourcen Auschecken:
- [Die Version 2.0 Entwicklertools Leitfaden >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "Azure-Active Directory" Kategorie >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unseren Produkten

Wir empfehlen Ihnen erhalten von Benachrichtigungen Sicherheitsvorfälle auftreten, besuchen [Diese Seite](https://technet.microsoft.com/security/dd252948) und von Ihren Sicherheitshinweisen abonnieren.
