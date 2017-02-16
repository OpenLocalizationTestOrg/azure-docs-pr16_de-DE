<properties
    pageTitle="Azure AD-Version 2.0 .NET Web API | Microsoft Azure"
    description="So erstellen Sie eine .NET MVC Web-Api, die aus beiden persönliche Microsoft Account Token akzeptiert und geschäftlichen oder schulnotizbücher Konten."
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
    ms.date="10/10/2016"
    ms.author="dastrock"/>

# <a name="secure-an-mvc-web-api"></a>Sichern einer MVC Web-API

Mit Azure Active Directory den Endpunkt Version 2.0 können Sie eine Web-API mit Access Token [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) , aktivieren Benutzer mit persönlichen Microsoft-Konto sowohl Arbeit schützen oder Schule Konten für den sicheren Zugriff auf Ihre Web-API.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory-Szenarien und Features werden von den Endpunkt Version 2.0 unterstützt.  Um festzustellen, ob den Version 2.0-Endpunkt verwendet werden sollen, erfahren Sie, [Version 2.0 Einschränkungen](active-directory-v2-limitations.md).

In ASP.NET-Web APIs lässt sich dies realisieren mithilfe von Microsoft OWIN Middleware in .NET Framework 4.5 enthalten.  Hier wird OWIN verwendet, um eine "Aufgabenliste" MVC Web-API zu erstellen, durch die Clients zu erstellen und Lesen von Aufgaben aus der Aufgabenliste eines Benutzers.  Im Web-API wird überprüft, eingehende Anfragen ein gültiger Access Token enthalten und alle Besprechungsanfragen, die keine Überprüfung auf einem geschützten Routing übergeben abzulehnen.  In diesem Beispiel wurde unter Verwendung von Visual Studio 2015 erstellt.

## <a name="download"></a>Herunterladen
Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet)verwaltet.  Wenn Sie nachvollziehen möchten, können Sie Sie [Herunterladen des app Gerüst als eine ZIP](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) oder das Gerüst Klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

Die Gerüst app enthält alle häufig verwendeten Code für eine einfache API, doch fehlt die Identität-bezogene Teile. Wenn Sie nicht folgen möchten, können Sie stattdessen Klonen oder [die fertige Stichprobe herunterladen](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip).

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>Registrieren einer app
Erstellen Sie eine neue app bei [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), oder gehen Sie folgendermaßen [detaillierten Schritte](active-directory-v2-app-registration.md).  Vergewissern Sie sich an:

- Kopieren nach unten die **Id der Anwendung** , die Ihre app zugewiesen ist, Sie benötigen diese Informationen verfügbar.

Diese visual Studio-Lösung enthält auch eine einfache WPF-app "TodoListClient".  Die TodoListClient wird verwendet, um zu veranschaulichen, wie ein Benutzer Vorzeichen-in, und wie ein Client Anfragen Ihrer Web-API senden kann.  In diesem Fall werden sowohl die TodoListClient als auch die TodoListService durch die gleichen app dargestellt.  Um die TodoListClient zu konfigurieren, sollten Sie auch an:

- Fügen Sie die **Mobile** -Plattform für Ihre app hinzu.


## <a name="install-owin"></a>Installieren von OWIN

Jetzt, da Sie eine app registriert haben, müssen Sie Ihre app einrichten, dass Sie mit der Version 2.0-Endpunkt kommunizieren, um eingehenden Anfragen und Token zu überprüfen.

- Um zu beginnen, öffnen Sie die Lösung und Hinzufügen der OWIN Middleware NuGet-Paketen des TodoListService Projekts mithilfe der Verwaltungskonsole Paket-Manager.

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>Konfigurieren der OAuth-Authentifizierung

- Fügen Sie eine OWIN Start-Klasse zum Projekt TodoListService aufgerufen `Startup.cs`.  Rechts auf das Projekt-- **Hinzufügen** --> **Neues Element** --> Suche nach "OWIN".  Die Middleware OWIN aufrufen wird die `Configuration(…)` Methode, wenn die app gestartet wird.
- Ändern Sie die Deklaration Verzicht auf `public partial class Startup` – wir haben Teil dieser Klasse bereits für Sie in einer anderen Datei implementiert.  In der `Configuration(…)` Methode, einem Anruf an ConfgureAuth(...) Authentifizierung für Ihre Web-app einrichten.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

- Öffnen Sie die Datei `App_Start\Startup.Auth.cs` und Implementieren der `ConfigureAuth(…)` -Methode, die die Web-API eingerichtet wird aus den Endpunkt Version 2.0 Token annehmen.

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

- Sie können jetzt `[Authorize]` Attribute zum Schutz von Controller und Aktionen für OAuth 2.0 Person Authentifizierung.  Passen Sie die `Controllers\TodoListController.cs` Klasse mit einem Tag autorisieren.  Dies zwingt den Benutzer melden Sie sich vor dem Zugriff auf dieser Seite.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Wenn ein Anrufer autorisierte erfolgreich auf eine der Ruft die `TodoListController` APIs, die Aktion möglicherweise Zugriff auf Informationen zum Anrufer benötigen.  OWIN bietet Zugriff auf die Ansprüche innerhalb der Person Token über die `ClaimsPrincpal` Objekt.  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

-   Öffnen Sie schließlich die `web.config` im Stammverzeichnis des Projekts TodoListService Datei, und geben Sie Ihre Konfigurationswerte in der `<appSettings>` Abschnitt.
  - Ihre `ida:Audience` ist die **Id der Anwendung** der app, die Sie im Portal eingegeben haben.

## <a name="configure-the-client-app"></a>Konfigurieren der Client-app
Bevor Sie den Dienst erledigen Liste in Aktion sehen können, müssen Sie den erledigen Liste Client konfigurieren, damit es Token aus den Endpunkt Version 2.0 erhalten und kann Anrufe an den Dienst.

- Öffnen Sie das Projekt TodoListClient `App.config` , und geben Sie die Konfigurationswerte in der `<appSettings>` Abschnitt.
  - Ihre `ida:ClientId` Id der Anwendung, die Sie aus dem Portal kopiert haben.

Schließlich bereinigen, erstellen und Ausführen von jedem Projekt!  Sie verfügen jetzt über eine .NET MVC-Web-API, die beide persönlichen Microsoft-Konten Token und geschäftlichen oder schulnotizbücher Konten annimmt.  Melden Sie sich bei der TodoListClient, und rufen Sie Ihre Web api Aufgaben zu des Benutzers Aufgabenliste hinzufügen.

Als Referenz können die vollständigen Beispiel (ohne die Konfigurationswerte) [als eine ZIP hier bereitgestellt wird](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), oder Sie es GitHub zu duplizieren:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>Nächste Schritte
Sie können nun auf Weitere Themen verschieben.  Möglicherweise möchten versuchen:

[Aufrufen einer Webs-API aus einer Web App >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

Zusätzliche Ressourcen Auschecken:
- [Die Version 2.0 Entwicklertools Leitfaden >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "Azure-Active Directory" Kategorie >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unseren Produkten

Wir empfehlen Ihnen erhalten von Benachrichtigungen Sicherheitsvorfälle auftreten, besuchen [Diese Seite](https://technet.microsoft.com/security/dd252948) und von Ihren Sicherheitshinweisen abonnieren.
