<properties
    pageTitle="Erste Schritte mit Azure AD-.NET | Microsoft Azure"
    description="So erstellen Sie eine .NET MVC Web App, die mit Azure AD für die Anmeldung integriert."
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

# <a name="aspnet-web-app-sign-in--sign-out-with-azure-ad"></a>ASP.NET Web App anmelden und Abmelden mit Azure AD

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD erleichtert problemlos und einfach Ihre Web-app Identitätsmanagement Auslagern Bereitstellen von einzelnen anmelden und Abmelden mit nur wenigen Codezeilen.  Asp.NET Web Apps lässt sich dies realisieren mithilfe von Microsoft Implementierung von der Community leistungsgesteuert OWIN Middleware in .NET Framework 4.5 enthalten.  Verwenden Sie hier OWIN an:
-   Melden Sie den Benutzer in der app Azure AD-als Identitätsanbieter aus.
-   Einige Informationen zu den Benutzer anzeigen.
-   Melden Sie den Benutzer bei der app abmelden.

Um dies zu tun, müssen Sie:

1. Registrieren Sie sich eine Anwendung mit Azure AD
2. Richten Sie Ihre app der Verkaufspipeline OWIN Authentifizierung verwendet.
3. Verwenden Sie OWIN anmelden und Abmelde Anfragen an Azure AD ausgeben.
4. Drucken Sie Daten über den Benutzer aus.

Um zu beginnen, [Laden Sie das app-Gerüst](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) oder [Laden Sie das Beispiel fertige](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  Sie benötigen auch einem Azure AD-Mandanten in der Anwendung registriert.  Wenn Sie eine bereits, besitzen [erfahren, wie Sie eins zu erhalten](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>*1. Anwendung mit Azure AD registrieren*
Wenn Ihre app für die Benutzerauthentifizierung aktivieren möchten, müssen Sie zuerst eine neue Anwendung in Ihrem Mandanten zu registrieren.

- Melden Sie sich bei der Azure-Verwaltungsportal.
- Klicken Sie im linken Navigationsbereich auf **Active Directory**.
- Wählen Sie den Mandanten, in dem die Anwendung registriert werden soll.
- Klicken Sie auf die Registerkarte **Applications** aus, und klicken Sie auf der unteren Einzug hinzufügen.
- Erstellen Sie einer neuen **Webanwendung und/oder WebAPI**, und folgen Sie den Anweisungen.
    - Den **Namen** der Anwendung wird eine Anwendung an Endbenutzer beschrieben.
    -   Die **Anmeldung URL** ist die Basis der app-URL.  Das Gerüst der Standardwert liegt `https://localhost:44320/`.
    - Der **App-ID-URI** ist ein eindeutiger Bezeichner für eine Anwendung.  Ist die Verwendung der Messe `https://<tenant-domain>/<app-name>`, z. B.`https://contoso.onmicrosoft.com/my-first-aad-app`
- Nachdem Sie die Registrierung abgeschlossen haben, zuweisen AAD Ihre app eine eindeutige Client-ID.  Sie benötigen diesen Wert in den nächsten Abschnitten, also kopieren Sie sie von der Registerkarte konfigurieren.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. so eingerichtet, dass Ihre app der Verkaufspipeline OWIN Authentifizierung verwenden*
Konfigurieren Sie hier die Middleware OWIN, um die OpenID verbinden Authentication-Protokoll verwenden.  OWIN wird zum Anmelden und Abmelde Anfragen Emission, die Sitzung des Benutzers zu verwalten und erhalten Informationen über den Benutzer, unter anderem verwendet werden.

-   Um zu beginnen, fügen Sie der OWIN Middleware NuGet-Paketen des Projekts mithilfe der Verwaltungskonsole für das Paket ein.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Hinzufügen eine Klasse OWIN Start des Projekts aufgerufen `Startup.cs` rechts auf das Projekt-- **Hinzufügen** --> **Neues Element** --> Suche nach "OWIN".  Die Middleware OWIN aufrufen wird die `Configuration(...)` Methode, wenn die app gestartet wird.
-   Ändern Sie die Deklaration Verzicht auf `public partial class Startup` – wir haben Teil dieser Klasse bereits für Sie in einer anderen Datei implementiert.  In der `Configuration(...)` Methode, einem Anruf an ConfgureAuth(...) zum Einrichten der Authentifizierung für Ihre Web app  

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Öffnen Sie die Datei `App_Start\Startup.Auth.cs` und Implementieren der `ConfigureAuth(...)` Methode.  Die Parameter in werden `OpenIDConnectAuthenticationOptions` dient als Koordinaten für Ihre app zur Kommunikation mit Azure AD.  Sie müssen außerdem Cookie-Authentifizierung einrichten – die Middleware OpenID verbinden verwendet Cookies hinter den.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ClientId = clientId,
            Authority = authority,
            PostLogoutRedirectUri = postLogoutRedirectUri,
        });
}
```

-   Öffnen Sie schließlich die `web.config` Datei im Stammverzeichnis des Projekts, und geben Sie Ihre Konfigurationswerte in der `<appSettings>` Abschnitt.
    -   Ihre app `ida:ClientId` ist die Guid, die Sie vom Azure-Portal in Schritt 1 kopiert haben.
    -   Die `ida:Tenant` ist der Name Ihrer Azure AD-Mandanten, z. B. "contoso.onmicrosoft.com".
    -   Ihre `ida:PostLogoutRedirectUri` zeigt an, um Azure AD, wo ein Benutzer umgeleitet werden sollten nach dem erfolgreichen der Anforderung einer Abmeldung Abschluss.

## <a name="3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>*3. Verwenden von OWIN anmelden und Abmelde Anfragen an Azure AD ausgeben*
Ihre app ist jetzt ordnungsgemäß konfiguriert mit Azure Active Directory mithilfe des herstellen OpenID Authentication-Protokolls kommunizieren.  OWIN enthält alle hässlich Details der Authentifizierungsnachrichten zu erstellen, das Token aus Azure AD überprüfen und Verwalten von Benutzer-Sitzung durchgeführt.  Alle bleibt besteht darin, Ihre Benutzer stellen eine Möglichkeit zum Anmelden und Abmelden.

- Sie können autorisieren Kategorien in Ihrem Controller, um diesen Benutzer anmeldet vor dem Zugriff auf eine bestimmte Seite erforderlich.  Öffnen `Controllers\HomeController.cs`, und fügen Sie die `[Authorize]` Kategorie an den Controller Info.

```C#
[Authorize]
public ActionResult About()
{
  ...
```

-   Sie können auch OWIN verwenden, Authentifizierungsanfragen direkt innerhalb des Codes ausgeben.  Open `Controllers\AccountController.cs`.  Stellen Sie in der Liste der Vorgänge SignIn() und SignOut() Herausforderung OpenID verbinden und Abmeldung Besprechungsanfragen.

```C#
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(
        OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
}
```

-   Öffnen Sie jetzt `Views\Shared\_LoginPartial.cshtml`.  Dies ist, wo Sie dem Benutzer Ihrer app anmelden und Abmelden Links anzeigen und drucken Sie den Namen des Benutzers in einer Ansicht werden.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">
                Hello, @User.Identity.Name!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

## <a name="4--display-user-information"></a>*4 Benutzerinformationen anzeigen*
Bei der Benutzerauthentifizierung mit OpenID verbinden, gibt Azure AD einer Id_token mit der Anwendung, die "Ansprüche" oder Assertionen über den Benutzer enthält.  Diese Ansprüche können Sie Ihre app personalisieren:

- Öffnen der `Controllers\HomeController.cs` Datei.  Sie können Ansprüche des Benutzers zugreifen, in Ihrem Controller über die `ClaimsPrincipal.Current` Sicherheit Hauptbenutzer Objekt.

```C#
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
    ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
    ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
    ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

    return View();
}
```

Schließlich erstellen Sie, und führen Sie die app!  Wenn Sie noch nicht geschehen ist, ist nun die Zeit zum Erstellen eines neuen Benutzers in Ihrem Mandanten mit einer *. onmicrosoft.com-Domäne.  Melden Sie sich mit diesen Benutzer aus, und beachten Sie, wie die Identität des Benutzers in der oberen Navigationsleiste wiedergegeben wird.  Melden Sie sich ab, und melden Sie sich als ein anderer Benutzer in Ihrem Mandanten wieder.  Wenn Sie besonders anspruchsvoll erscheinen fühlen, registrieren Sie, und führen Sie eine andere Instanz dieser Anwendung (mit einem eigenen ClientId), und klicken Sie auf Überwachung finden Sie unter einmalige Anmelden auf in Aktion.

Als Referenz der vollständigen Beispiel (ohne die Konfigurationswerte) [hier bereitgestellt wird](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  

Sie können nun auf Erweiterte Themen verschieben.  Möglicherweise möchten versuchen:

[Sichern eine Web-API mit Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
