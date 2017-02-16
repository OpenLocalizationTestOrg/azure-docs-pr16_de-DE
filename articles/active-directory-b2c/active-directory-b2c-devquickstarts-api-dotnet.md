<properties
    pageTitle="Azure AD B2C | Microsoft Azure"
    description="So erstellen Sie eine Web-API für .NET mithilfe von Azure Active Directory B2C, für die Authentifizierung mithilfe von OAuth 2.0 Access Token gesichert."
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
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: Erstellen einer .NET Webs-API

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Mit B2C Azure Active Directory (Azure AD) können Sie eine Web-API mithilfe von OAuth 2.0 Access Token sichern. Diese Token ermöglichen Ihrer Client-apps, die an die API authentifizieren Azure AD B2C verwenden. In diesem Artikel wird gezeigt, wie Sie eine .NET Model View Controller (MVC)-API "Aufgabenliste" erstellen, die Benutzern CRUD Aufgaben ermöglicht. Das Web-API ist mit Azure AD B2C gesichert und lässt nur authentifizierten Benutzer zum Verwalten ihrer Vorgangsliste werden.

## <a name="create-an-azure-ad-b2c-directory"></a>Erstellen Sie eine Azure AD B2C-Verzeichnis

Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis erstellen, oder den Mandanten. Ein Verzeichnis ist ein Container für alle Benutzer, apps, Gruppen und mehr. Wenn Sie eine bereits besitzen, [Erstellen Sie ein Verzeichnis B2C](active-directory-b2c-get-started.md) , bevor Sie Sie in diesem Handbuch fortsetzen.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Als Nächstes müssen Sie eine app in Ihrem Verzeichnis B2C erstellen. Dadurch Azure AD-Informationen, die sichere Kommunikation mit Ihrem app benötigt werden. [Wenn Sie eine app erstellen, gehen Sie wie folgt vor.](active-directory-b2c-app-registration.md) Achten Sie darauf, um:

- Fügen Sie einer **Web app** oder **Web API** in die Anwendung.
- Verwenden Sie den **uniform Resource Identifier umleiten** `https://localhost:44316/` für das Web app. Dies ist der Standardspeicherort des Web app-Clients für dieses Codebeispiel.
- Kopieren Sie die **ID der Anwendung** , die zu Ihrer Anwendung zugeordnet ist. Sie benötigen es später.

 [AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen Sie Ihre Richtlinien

In Azure AD B2C wird jede Erfahrungen von einer [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Der Client in diesem Codebeispiel enthält drei Identität Erfahrung: registrieren, melden Sie sich und Profil bearbeiten. Sie müssen zum Erstellen einer Richtlinie für jedes Typs wie in der [Richtlinie Bezug Artikel](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)beschrieben. Wenn Sie Ihre drei Richtlinien erstellen, müssen Sie unbedingt:

- Wählen Sie entweder **Benutzer-ID anmelden** oder **Anmelde-e-Mail** in das Identität Anbieter Blade aus.
- Wählen Sie in Ihrer Anmeldung Richtlinie **Anzeigenamen** und andere Attribute Anmeldung aus.
- Wählen Sie als Anwendung Ansprüche für jede Richtlinie **Anzeigenamen** und **Objekt-ID** Ansprüche aus. Sie können auch anderen Ansprüche auswählen.
- Kopieren Sie den **Namen** der einzelnen Richtlinie, nachdem Sie es erstellt haben. Sie benötigen diese Richtliniennamen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die drei Richtlinien erfolgreich erstellt haben, sind Sie bereit sind, Ihre app zu erstellen.

## <a name="download-the-code"></a>Laden Sie den code

Der Code für dieses Lernprogramm [auf GitHub verwaltet wird](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet). Wenn Sie um das Beispiel beim Durcharbeiten zu erstellen, können Sie [ein Gerüst Projekt als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). Sie können auch das Gerüst Klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

Die fertige app steht auch [als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) oder das `complete` Zweig des gleichen Repositorys.

Nachdem Sie den Code Stichprobe heruntergeladen haben, öffnen Sie die Visual Studio sln-Datei, um anzufangen. Die Lösungsdatei enthält zwei Projekte: `TaskWebApp` und `TaskService`. `TaskWebApp`ist eine MVC Web-Anwendung, der mit der Benutzer interagiert. `TaskService`ist der app Back-End-Web-API, in der Aufgabenliste des Benutzers gespeichert.

## <a name="configure-the-task-web-app"></a>Konfigurieren der Aufgabe Web app

Wenn ein Benutzer mit interagiert `TaskWebApp`, Client sendet Anfragen an Azure AD und wieder Token, die verwendet werden können, aufrufen, wird die `TaskService` web-API. Wenn der Benutzer anmelden und erste Token, müssen Sie bereitstellen `TaskWebApp` mit einige Informationen zu Ihrer app. In der `TaskWebApp` Project Öffnen der `web.config` -Datei im Stammverzeichnis des Projekts, und Ersetzen Sie die Werte in den `<appSettings>` Abschnitt.  Lassen Sie die `AadInstance`, `RedirectUri`, und `TaskServiceUrl` als Werte-ist.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
  </appSettings>
```

In diesem Artikel befasst sich nicht auf die Erstellung der `TaskWebApp` Client.  So erstellen Sie eine Web-app mit Azure AD B2C finden Sie unter [unseren .NET Web app-Lernprogramm](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="secure-the-api"></a>Sichern der-API

Wenn Sie einen Client, die die API für Benutzer Anrufe haben, können Sie secure `TaskService` mithilfe von OAuth 2.0 Person Token. Ihre API kann annehmen und Token mithilfe von Microsoft Open Web-Oberfläche für .NET (OWIN) Bibliothek überprüfen.

### <a name="install-owin"></a>Installieren von OWIN
Installieren der Verkaufspipeline OWIN OAuth Authentifizierung, um zu beginnen:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### <a name="enter-your-b2c-details"></a>Geben Sie Ihre Daten B2C
Öffnen der `web.config` Datei im Stammverzeichnis der `TaskService` Projekt, und Ersetzen Sie die Werte in der `<appSettings>` Abschnitt. Diese Werte werden in der Bibliothek API und OWIN verwendet werden.  Lassen Sie die `AadInstance` Wert unverändert beizubehalten.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
```

### <a name="add-an-owin-startup-class"></a>Fügen Sie eine OWIN Start-Klasse
Hinzufügen eine OWIN Start-Klasse die `TaskService` Projekt mit dem Namen `Startup.cs`.  Mit der rechten Maustaste auf das Projekt, wählen Sie **Hinzufügen** und **Neues Element**aus, und suchen Sie nach OWIN.


```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>Konfigurieren der OAuth 2.0-Authentifizierung
Öffnen Sie die Datei `App_Start\Startup.Auth.cs`, und Implementieren der `ConfigureAuth(...)` Methode:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string signUpPolicy = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string signInPolicy = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string editProfilePolicy = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {   
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signUpPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signInPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(editProfilePolicy));
    }

    public OAuthBearerAuthenticationOptions CreateBearerOptionsFromPolicy(string policy)
    {
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
            AuthenticationType = policy,
        };

        return new OAuthBearerAuthenticationOptions
        {
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, policy))),
        };
    }
}
```

### <a name="secure-the-task-controller"></a>Den Vorgang Controller Secure
Nach die app für die Verwendung von OAuth 2.0-Authentifizierung konfiguriert ist, können Sie durch Hinzufügen das Web-API secure einer `[Authorize]` Tag, um den Vorgang Controller. Dies ist der Controller, an dem alle Aufgabe Liste Manipulation, erfolgt, damit Sie den gesamten Controller Ebene der Klasse secure sollte. Sie können auch hinzufügen der `[Authorize]` -Tag für einzelne Aktionen für eine genauere Kontrolle.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Abrufen von Benutzerinformationen aus dem token
`TasksController`Aufgaben in einer Datenbank, in dem jede Aufgabe einen zugeordneten Benutzer, die die Aufgabe hat "besitzt" gespeichert. Der Besitzer wird durch die Benutzer **Objekt­ID**identifiziert. (Dies ist, warum Sie die Objekt-ID als eine Anwendung hinzufügen erforderlich anfordern in allen Ihren Richtlinien.)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## <a name="run-the-sample-app"></a>Führen Sie die Beispielapp

Schließlich erstellen, und führen Sie beide `TaskWebApp` und `TaskService`. Registrieren Sie sich für die app mithilfe einer e-Mail-Adresse oder Benutzername an. Erstellen Sie einige Vorgänge auf des Benutzers Vorgangsliste, und beachten Sie, wie diese in der-API beibehalten werden auch nach dem Beenden und den Client neu starten.

## <a name="edit-your-policies"></a>Bearbeiten Sie Ihre Richtlinien

Nachdem Sie eine API mithilfe von Azure AD B2C gesichert haben, können experimentieren Sie mit den Richtlinien Ihrer app und die Effekte anzeigen (oder fehlen Absatz) zur-API. Können Sie die Anwendung Ansprüche aus der Liste Richtlinien bearbeiten und ändern die Benutzerinformationen, die im Web API zur Verfügung. Ansprüche, die Sie hinzufügen werden zur Verfügung, Ihre .NET MVC Web API in der `ClaimsPrincipal` -Objekts, wie weiter oben in diesem Artikel beschrieben.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->
