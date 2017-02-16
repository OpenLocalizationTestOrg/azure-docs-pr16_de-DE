<properties
    pageTitle="Azure-Active Directory B2C | Microsoft Azure"
    description="So erstellen Sie einen Web-Anwendung, die Anmeldung, Anmeldung enthält, und klicken Sie auf Kennwort zurücksetzen Azure Active Directory B2C verwenden."
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

# <a name="azure-ad-b2c-sign-up--sign-in-in-a-aspnet-web-app"></a>Azure AD-B2C: Anmeldung und Anmeldung in einer ASP.NET Web-App

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Mithilfe von Azure Active Directory (Azure AD) B2C können Sie leistungsfähige Self-service-Identität Verwaltungsfunktionen Web app in wenigen einfachen Schritten hinzufügen. Dieser Artikel wird erläutert, wie eine ASP.NET Web app zu erstellen, die Benutzer Anmeldung, Anmeldung enthält, und Kennwort zurücksetzen. Die app wird Unterstützung für die Anmeldung und-Anmeldung mithilfe eines Benutzernamen oder e-Mail- und mithilfe von Konten für soziale wie Facebook und Google enthalten.

In diesem Lernprogramm unterscheidet sich von [unserem .NET Weblernprogramm](active-directory-b2c-devquickstarts-web-dotnet.md) verwendet einen [registrieren, oder melden Sie sich die Richtlinie](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy) zu bieten Registrierung Benutzer & Anmeldung mithilfe einer Schaltfläche statt zwei (eine für Anmeldung und eine für die Anmeldung).  In einer Nutshell, bei der Anmeldung von oder anmelden Richtlinie ermöglicht Benutzern, mit einem bestehenden Konto anmelden, wenn sie mindestens ein, oder einen neuen Erstellen ihrer ersten Mal mithilfe der app ist.

## <a name="get-an-azure-ad-b2c-directory"></a>Abrufen einer Azure AD B2C-Verzeichnis

Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis erstellen, oder den Mandanten. Ein Verzeichnis ist ein Container für alle Benutzer, apps, Gruppen und mehr.  Wenn Sie eine bereits besitzen, [Erstellen Sie ein Verzeichnis B2C](active-directory-b2c-get-started.md) , bevor Sie Sie in diesem Handbuch fortsetzen.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Als Nächstes müssen Sie eine app in Ihrem Verzeichnis B2C erstellen. Dadurch Azure AD-Informationen, die sichere Kommunikation mit Ihrem app benötigt werden. [Wenn Sie eine app erstellen, gehen Sie wie folgt vor.](active-directory-b2c-app-registration.md)  Achten Sie darauf, um:

- Fügen Sie einer **Web app/Web API** in die Anwendung.
- Geben Sie `https://localhost:44316/` als eine **URI umleiten**. Es ist die Standard-URL für dieses Codebeispiel.
- Kopieren Sie unten die **ID der Anwendung** , die zu Ihrer Anwendung zugeordnet ist.  Sie werden diese später benötigen.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen Sie Ihre Richtlinien

In Azure AD B2C wird jede Erfahrungen von einer [Richtlinie](active-directory-b2c-reference-policies.md)definiert. In diesem Codebeispiel enthält zwei Identität Erfahrung: **Anmeldung und Anmeldung**und **Kennwort zurücksetzen**.  Sie müssen zum Erstellen einer Richtlinie für jedes Typs wie in der [Richtlinie Bezug Artikel](active-directory-b2c-reference-policies.md)beschrieben. Wenn Sie die beiden Richtlinien erstellen, müssen Sie unbedingt:

- Wählen Sie in der Identität Anbieter Blade- **Benutzer-ID anmelden** oder die **e-Mail-Anmeldung** aus.
- Wählen Sie den **Anzeigenamen** und andere Attribute Anmeldung in Ihrer Richtlinie Anmeldung und Anmeldung an.
- Wählen Sie aus den **Anzeigenamen** Anspruch als Anwendung in jeder Richtlinie anfordern. Sie können auch anderen Ansprüche auswählen.
- Kopieren Sie den **Namen** der einzelnen Richtlinie, nachdem Sie es erstellt haben. Sie benötigen diese Richtliniennamen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie Ihre zwei Richtlinien erstellt haben, können Sie bereit sind, Ihre app zu erstellen.

## <a name="download-the-code-and-configure-authentication"></a>Laden Sie den Code und Konfigurieren der Authentifizierung

Der Code für dieses Beispiel [auf GitHub verwaltet wird](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI). Wenn Sie um das Beispiel beim Durcharbeiten zu erstellen, können Sie [das Gerüst Projekt als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/skeleton.zip). Sie können auch das Gerüst Klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

Das vollständigen Beispiel steht auch [als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip) oder das `complete` Zweig des gleichen Repositorys.

Nachdem Sie den Code Stichprobe heruntergeladen haben, öffnen Sie die Visual Studio sln-Datei, um anzufangen.

Ihre app kommuniziert mit Azure AD B2C per HTTP-Authentifizierungsanfragen, die die Richtlinie angeben, die als Teil der Anforderung ausgeführt werden sollen. Für .NET Webanwendungen können Sie Microsoft OWIN Bibliothek OpenID verbinden Authentifizierungsanfragen zu senden, Richtlinien ausführen, Verwalten Benutzer Sitzungen und vieles mehr.

Um zu beginnen, fügen Sie der OWIN Middleware NuGet-Paketen des Projekts, mithilfe der Verwaltungskonsole für Visual Studio-Paket-Manager.

```
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
Install-Package System.IdentityModel.Tokens.Jwt
```

Öffnen Sie die `web.config` -Datei im Stammverzeichnis des Projekts, und geben Sie Ihre app Konfigurationswerte in der `<appSettings>` Abschnitt, die Werte unter durch ein eigenes ersetzen.  Lassen Sie möglicherweise die `ida:RedirectUri` und die `ida:AadInstance` Werte ungeändert, unverändert bei.

```
<configuration>
  <appSettings>

    ...

    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}{1}{2}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SusiPolicyId" value="b2c_1_susi" />
    <add key="ida:PasswordResetPolicyId" value="b2c_1_reset" />
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Fügen Sie eine OWIN Start-Klasse weiter, um das Projekt mit dem Namen `Startup.cs`. Mit der rechten Maustaste auf das Projekt, wählen Sie **Hinzufügen** und **Neues Element**aus, und suchen Sie nach "OWIN". Ändern Sie die Deklaration Verzicht auf `public partial class Startup`. Wir implementiert Teil dieser Klasse für Sie in einer anderen Datei. Die Middleware OWIN aufrufen wird die `Configuration(...)` Methode, wenn die app gestartet wird. Bei dieser Methode anrufen zu `ConfigureAuth(...)`, in dem Sie Authentifizierung für Ihre app festlegen.

```C#
// Startup.cs

public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

Öffnen Sie die Datei `App_Start\Startup.Auth.cs` und Implementieren der `ConfigureAuth(...)` Methode.  Die Parameter in werden `OpenIdConnectAuthenticationOptions` dienen als Koordinaten für Ihre app zur Kommunikation mit Azure AD. Sie müssen auch Cookie-Authentifizierung einrichten. Die Middleware OpenID verbinden verwendet Cookies, um Benutzer Sitzungen, unter anderem verwalten.

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SusiPolicyId = ConfigurationManager.AppSettings["ida:SusiPolicyId"];
    public static string PasswordResetPolicyId = ConfigurationManager.AppSettings["ida:PasswordResetPolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        // Configure OpenID Connect middleware for each policy
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(PasswordResetPolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SusiPolicyId));

    }

    private Task OnSecurityTokenValidated(SecurityTokenValidatedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
    {
        // If you wanted to keep some local state in the app (like a db of signed up users),
        // you could use this notification to create the user record if it does not already
        // exist.

        return Task.FromResult(0);
    }

    private OpenIdConnectAuthenticationOptions CreateOptionsFromPolicy(string policy)
    {
        return new OpenIdConnectAuthenticationOptions
        {
            // For each policy, give OWIN the policy-specific metadata address, and
            // set the authentication type to the id of the policy
            MetadataAddress = String.Format(aadInstance, tenant, policy),
            AuthenticationType = policy,

            // These are standard OpenID Connect parameters, with values pulled from web.config
            ClientId = clientId,
            RedirectUri = redirectUri,
            PostLogoutRedirectUri = redirectUri,
            Notifications = new OpenIdConnectAuthenticationNotifications
            {
                AuthenticationFailed = AuthenticationFailed,
                SecurityTokenValidated = OnSecurityTokenValidated,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {
                NameClaimType = "name",
            },
        };
    }
}
...
```

## <a name="send-authentication-requests-to-azure-ad"></a>Senden Sie Besprechungsanfragen Authentifizierung an Azure AD
Ihre app ist für die Kommunikation mit Azure AD B2C mithilfe des herstellen OpenID Authentication-Protokolls jetzt ordnungsgemäß konfiguriert.  OWIN enthält alle Details der Authentifizierungsnachrichten zu erstellen, das Token aus Azure AD überprüfen und Verwalten von Benutzer-Sitzung durchgeführt.  Übrig bleibt des Benutzers Fluss einleiten.

Wenn ein Benutzer **Login** auswählt oder **Kennwort vergessen?** in der Web-app, die zugeordnete Aktion aufgerufen wird, `Controllers\AccountController.cs`. Integrierte OWIN Methoden können Sie in jedem Fall die richtige Richtlinie auslösen:

```C#
// Controllers\AccountController.cs

public void Login()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by specifying the policy id as the AuthenticationType
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.SusiPolicyId);
    }
}

public void ResetPassword()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
        new AuthenticationProperties() { RedirectUri = "/" }, Startup.PasswordResetPolicyId);
    }
}
```

Während der Ausführung des Anmeldefensters nach oben oder Richtlinie anmelden, der Benutzer hat die Möglichkeit, klicken Sie auf eine **Kennwort vergessen?** Link.  In diesem Fall wird Azure AD B2C senden, dass die app, die eine bestimmte Fehlermeldung zeigt an, dass ein Kennwort ausführen sollen Richtlinie zurücksetzen.  Sie können diesen Fehler in erfassen `Startup.Auth.cs` mithilfe der `AuthenticationFailed` Benachrichtigung:

```C#
// Used for avoiding yellow-screen-of-death TODO
private Task AuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If the user clicked the reset password link, redirect to the reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        // If the user canceled the sign in, redirect back to the home page
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```


Zusätzlich zur explizit aufrufen einer Richtlinie, können Sie mithilfe einer `[Authorize]` Kategorie in Ihrem Controller, die eine Richtlinie ausgeführt wird, wenn der Benutzer nicht angemeldet ist. Open `Controllers\HomeController.cs` und Hinzufügen der `[Authorize]` Kategorie an den Controller Ansprüche.  OWIN wählt die letzte Richtlinie konfiguriert, wenn die `[Authorize]` Kategorie erreicht wird.

```C#
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a certain policy if the user is not already signed into the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

OWIN können Sie auch den Benutzer von der app anmelden. In `Controllers\AccountController.cs`:  

```C#
// Controllers\AccountController.cs

public void Logout()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

## <a name="display-user-information"></a>Benutzerinformationen anzeigen
Wenn Sie Benutzer mithilfe der herstellen OpenID authentifizieren, gibt Azure AD eine Token-ID bei der app, die **Ansprüche**enthält. Hierbei handelt es sich um Assertionen über den Benutzer. Ansprüche können Sie Ihre app personalisieren.  

Öffnen der `Controllers\HomeController.cs` Datei. Sie können die Benutzeransprüche zugreifen, in Ihrem Controller über die `ClaimsPrincipal.Current` Sicherheit Hauptbenutzer Objekt.

```C#
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

Sie können Ansprüche zugreifen, die auf die gleiche Weise wie die Anwendung empfängt.  Eine Liste aller Ansprüche, die die app empfangen werden, wird auf der Seite **Ansprüche** für Sie verfügbar.

## <a name="run-the-sample-app"></a>Führen Sie die Beispielapp

Schließlich können Sie erstellen, und führen die app. Registrieren Sie sich mithilfe eines e-Mail-Adresse oder die Benutzer die Namens für die app an. Melden Sie sich ab und melden Sie sich wieder unter demselben Benutzernamen. Bearbeiten Sie das Profil der Benutzer ein. Melden Sie sich ab, und melden Sie sich als ein anderer Benutzer. Beachten Sie, dass die Informationen auf der Registerkarte **Ansprüche** angezeigte Informationen entspricht, die Sie auf Ihre Richtlinien konfiguriert.

## <a name="add-social-idps"></a>Hinzufügen von IDPs für soziale Netzwerke

Die app unterstützt derzeit nur Benutzer Anmeldung und-Anmeldung mittels **lokaler Konten**. Hierbei handelt es sich um Konten in Ihrem Verzeichnis B2C gespeichert, die einen Benutzernamen und ein Kennwort verwenden. Mithilfe von Azure AD B2C, können Sie Unterstützung für andere **Identitätsanbieter** (IDPs) hinzufügen, ohne ein Teil des Codes ändern.

Beginnen Sie anhand der detaillierten Anweisungen in folgenden Artikeln, um Ihre app für soziale Netzwerke IDPs hinzuzufügen. Für jede IDP unterstützt werden soll, müssen Sie eine Anwendung registrieren, in dem jeweiligen System und erhalten eine Client-ID an.

- [Einrichten von Facebook als eine IDP](active-directory-b2c-setup-fb-app.md)
- [Einrichten von Google als eine IDP](active-directory-b2c-setup-goog-app.md)
- [Einrichten von Amazon als eine IDP](active-directory-b2c-setup-amzn-app.md)
- [Einrichten von LinkedIn als eine IDP](active-directory-b2c-setup-li-app.md)

Nachdem Sie die Identitätsanbieter in Ihrem Verzeichnis B2C hinzugefügt haben, müssen Sie jeder der drei Richtlinien zum Einschließen der neuen IDPs bearbeiten, wie in der [Richtlinie Bezug Artikel](active-directory-b2c-reference-policies.md)beschrieben. Nachdem Sie Ihre Richtlinien gespeichert haben, führen Sie die app erneut aus.  Folgendes sollte angezeigt werden die neuen IDPs als Anmeldung hinzugefügt und Anmeldeoptionen in jedem der Ihre Identität auftritt.

Können Sie Ihren Richtlinien experimentieren und beobachten Sie die Auswirkung auf die app Stichprobe. Fügen Sie hinzu oder entfernen Sie IDPs, manipulieren Sie Anwendung Ansprüche oder ändern Sie Anmeldung Attribute. Experimentieren Sie, bis Sie sehen können, wie Richtlinien, Authentifizierung und OWIN gemeinsam einbinden.

Als Referenz der vollständigen Beispiel (ohne die Konfigurationswerte) [als eine ZIP-Datei bereitgestellt wird](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip). Sie können auch aus GitHub Klonen:

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
