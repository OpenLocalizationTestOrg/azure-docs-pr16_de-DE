<properties
    pageTitle="Azure AD-Version 2.0 .NET Web App | Microsoft Azure"
    description="So erstellen Sie eine .NET MVC Web App, die Benutzer, die mit beide persönliche Microsoft Account anmeldet und geschäftlichen oder schulnotizbücher Konten."
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

# <a name="add-sign-in-to-an-net-mvc-web-app"></a>Anmelden bei einer .NET MVC Web app hinzufügen

Mit den Endpunkt Version 2.0 können Sie schnell Authentifizierung bei der Web apps mit Unterstützung für beide persönlichen Microsoft-Konten und geschäftlichen oder schulnotizbücher Konten hinzufügen.  ASP.NET Web Apps lässt sich dies realisieren mithilfe von Microsoft OWIN Middleware in .NET Framework 4.5 enthalten.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory-Szenarien und Features werden von den Endpunkt Version 2.0 unterstützt.  Um festzustellen, ob den Version 2.0-Endpunkt verwendet werden sollen, erfahren Sie, [Version 2.0 Einschränkungen](active-directory-v2-limitations.md).

 Hier werden wir eine Web app erstellen, mit denen OWIN den Benutzer anmelden, einige Informationen zu den Benutzer, und melden Sie den Benutzer bei der app abmelden.
 
 ## <a name="download"></a>Herunterladen
Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet)verwaltet.  Wenn Sie nachvollziehen möchten, können Sie Sie [Herunterladen des app Gerüst als eine ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) oder das Gerüst Klonen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

Die fertige app wird am Ende dieses Lernprogramms ebenfalls bereitgestellt.

## <a name="register-an-app"></a>Registrieren einer app
Erstellen Sie eine neue app bei [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), oder gehen Sie folgendermaßen [detaillierten Schritte](active-directory-v2-app-registration.md).  Vergewissern Sie sich an:

- Kopieren nach unten die **Id der Anwendung** , die Ihre app zugewiesen ist, Sie benötigen diese Informationen verfügbar.
- Fügen Sie die **Web** -Plattform für Ihre app hinzu.
- Geben Sie den entsprechenden **URI umleiten**aus. Der Redirect-Uri gibt zu Azure AD Stelle, an der Authentifizierungsantworten geleitet werden – das Standardformat für dieses Lernprogramms ist `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Installieren und Konfigurieren der OWIN Authentifizierung
Konfigurieren Sie hier die Middleware OWIN, um die OpenID verbinden Authentication-Protokoll verwenden.  OWIN wird zum Anmelden und Abmelde Anfragen Emission, die Sitzung des Benutzers zu verwalten und erhalten Informationen über den Benutzer, unter anderem verwendet werden.

-   Um zu beginnen, öffnen Sie die `web.config` Datei im Stammverzeichnis des Projekts, und geben Sie Ihre app Konfigurationswerte in der `<appSettings>` Abschnitt.
    -   Die `ida:ClientId` ist die **Id der Anwendung** , die Ihre app in der Registrierung Portal zugewiesen ist.
    -   Die `ida:RedirectUri` ist der **Uri umleiten** , die Sie im Portal eingegeben haben.

-   Fügen Sie anschließend die OWIN Middleware NuGet-Paketen des Projekts mithilfe der Verwaltungskonsole Paket-Manager.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Hinzufügen einer "OWIN Startklasse" des Projekts namens `Startup.cs` rechts auf das Projekt-- **Hinzufügen** --> **Neues Element** --> Suche nach "OWIN".  Die Middleware OWIN aufrufen wird die `Configuration(...)` Methode, wenn die app gestartet wird.
-   Ändern Sie die Deklaration Verzicht auf `public partial class Startup` – wir haben Teil dieser Klasse bereits für Sie in einer anderen Datei implementiert.  In der `Configuration(...)` Methode, einem Anruf an ConfigureAuth(...) zum Einrichten der Authentifizierung für Ihre Web app  

```C#
[assembly: OwinStartup(typeof(Startup))]

namespace TodoList_WebApp
{
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
}
```

-   Öffnen Sie die Datei `App_Start\Startup.Auth.cs` und Implementieren der `ConfigureAuth(...)` Methode.  Die Parameter in werden `OpenIdConnectAuthenticationOptions` dient als Koordinaten für Ihre app zur Kommunikation mit Azure AD.  Sie müssen außerdem Cookie-Authentifizierung einrichten – die Middleware OpenID verbinden verwendet Cookies hinter den.

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
                                     RedirectUri = redirectUri,
                                     Scope = "openid email profile",
                                     ResponseType = "id_token",
                                     PostLogoutRedirectUri = redirectUri,
                                     TokenValidationParameters = new TokenValidationParameters
                                     {
                                             ValidateIssuer = false,
                                     },
                                     Notifications = new OpenIdConnectAuthenticationNotifications
                                     {
                                             AuthenticationFailed = OnAuthenticationFailed,
                                     }
                             });
             }
```

## <a name="send-authentication-requests"></a>Senden Sie Besprechungsanfragen Authentifizierung
Ihre app ist für die Kommunikation mit dem Version 2.0-Endpunkt über das Verbinden OpenID Authentifizierungsprotokoll jetzt ordnungsgemäß konfiguriert.  OWIN enthält alle hässlich Details der Authentifizierungsnachrichten zu erstellen, das Token aus Azure AD überprüfen und Verwalten von Benutzer-Sitzung durchgeführt.  Alle bleibt besteht darin, Ihre Benutzer stellen eine Möglichkeit zum Anmelden und Abmelden.

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

// BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
    Response.Redirect("/");
}
```

-   Öffnen Sie jetzt `Views\Shared\_LoginPartial.cshtml`.  Dies ist die Stelle, an der Sie dem Benutzer Ihrer app anmelden und Abmelden Links anzeigen möchten, und ausdrucken auf den Namen des Benutzers in einer Ansicht.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">

                @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@

                Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
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

## <a name="display-user-information"></a>Benutzerinformationen anzeigen
Bei der Benutzerauthentifizierung mit OpenID verbinden, gibt der Endpunkt Version 2.0 einer Id_token bei der app, die [Ansprüche](active-directory-v2-tokens.md#id_tokens)oder Assertionen über den Benutzer enthält.  Diese Ansprüche können Sie Ihre app personalisieren:

- Öffnen der `Controllers\HomeController.cs` Datei.  Sie können Ansprüche des Benutzers zugreifen, in Ihrem Controller über die `ClaimsPrincipal.Current` Sicherheit Hauptbenutzer Objekt.

```C#
[Authorize]
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;

    // The object ID claim will only be emitted for work or school accounts at this time.
    Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
    ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;

    // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
    ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;

    // The subject or nameidentifier claim can be used to uniquely identify the user
    ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;

    return View();
}
```

## <a name="run"></a>Ausführen

Schließlich erstellen Sie, und führen Sie die app!   Melden Sie sich mit entweder eine persönliche Microsoft Account oder einem geschäftlichen oder schulnotizbücher-Konto, und beachten Sie, wie die Identität des Benutzers in der oberen Navigationsleiste wiedergegeben wird.  Sie verfügen jetzt über eine gesicherte Protokolle nach Industriestandard, die Benutzer mit ihren persönlichen und Arbeit/Schule Konten authentifizieren können Web-app.

Als Referenz können die vollständigen Beispiel (ohne die Konfigurationswerte) [als eine ZIP hier bereitgestellt wird](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), oder Sie es GitHub zu duplizieren:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Nächste Schritte

Sie können nun auf Erweiterte Themen verschieben.  Möglicherweise möchten versuchen:

[Secure eine Web-API mit der Version 2.0-Endpunkt >>](active-directory-devquickstarts-webapi-dotnet.md)

Zusätzliche Ressourcen Auschecken:
- [Die Version 2.0 Entwicklertools Leitfaden >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "Azure-Active Directory" Kategorie >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unseren Produkten

Wir empfehlen Ihnen erhalten von Benachrichtigungen Sicherheitsvorfälle auftreten, besuchen [Diese Seite](https://technet.microsoft.com/security/dd252948) und von Ihren Sicherheitshinweisen abonnieren.
