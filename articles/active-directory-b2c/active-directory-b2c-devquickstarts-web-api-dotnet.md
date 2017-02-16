<properties
    pageTitle="Azure-Active Directory B2C | Microsoft Azure"
    description="So erstellen Sie eine Webanwendung, die eine Web-API ruft mithilfe von Azure Active Directory B2C."
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

# <a name="azure-ad-b2c-call-a-web-api-from-a-net-web-app"></a>Azure AD-B2C: Rufen Sie eine Web-API aus einer .NET Web app

Mithilfe von B2C Azure Active Directory (Azure AD), können Sie leistungsfähige Self-service-Identität Verwaltungsfunktionen Web apps und Web-APIs in wenigen einfachen Schritten hinzufügen. Dieser Artikel wird erläutert, wie eine .NET Model View Controller (MVC) "Aufgabenliste" Web app zu erstellen, die eine Web-API ruft mithilfe der Person Token

In diesem Artikel befasst sich nicht auf Anmeldung, Anmeldung Implementierung und Verwaltung von Profilen mit Azure AD B2C aus. Nachdem der Benutzer bereits authentifiziert ist befasst auf einen Web-APIs. Wenn Sie noch nicht geschehen ist, sollten Sie die Grundlagen des Azure AD B2C Lernen mit der [.NET Web app-erste Schritte-Lernprogramm](active-directory-b2c-devquickstarts-web-dotnet.md) beginnen.

## <a name="get-an-azure-ad-b2c-directory"></a>Abrufen einer Azure AD B2C-Verzeichnis

Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis erstellen, oder den Mandanten.  Ein Verzeichnis ist ein Container für alle Benutzer, apps, Gruppen und vieles mehr.  Wenn Sie eine bereits besitzen, [Erstellen Sie ein Verzeichnis B2C](active-directory-b2c-get-started.md) , bevor Sie Sie in diesem Handbuch fortsetzen.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Als Nächstes müssen Sie eine app in Ihrem Verzeichnis B2C erstellen. Dadurch Azure AD-Informationen, die sichere Kommunikation mit Ihrem app benötigt werden. In diesem Fall wird der Web app und die Web-API durch eine einzelne **ID der Anwendung**, dargestellt werden, weil eine logische app bestehen aus. [Wenn Sie eine app erstellen, gehen Sie wie folgt vor.](active-directory-b2c-app-registration.md) Achten Sie darauf, um:

- Fügen Sie einer **Web app/Web API** in die Anwendung.
- Geben Sie `https://localhost:44316/` als **Antwort-URL**. Es ist die Standard-URL für dieses Codebeispiel.
- Kopieren Sie die **ID der Anwendung** , die zu Ihrer Anwendung zugeordnet ist. Sie auch benötigen diese später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen Sie Ihre Richtlinien

In Azure AD B2C wird jede Erfahrungen von einer [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Diese Web app enthält drei Identität Erfahrung: registrieren, melden Sie sich und Profil bearbeiten. Sie müssen zum Erstellen einer Richtlinie für jedes Typs wie in der [Richtlinie Bezug Artikel](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)beschrieben. Wenn Sie die drei Richtlinien erstellen, müssen Sie unbedingt:

- Wählen Sie den **Anzeigenamen** und andere Attribute Anmeldung in Ihrer Anmeldung Richtlinie aus.
- Wählen Sie die Anwendung Ansprüche **Anzeigenamen** und **Objekt-ID** in jeder Richtlinie aus. Sie können auch anderen Ansprüche auswählen.
- Kopieren Sie den **Namen** der einzelnen Richtlinie, nachdem Sie es erstellt haben. Es sollte das Präfix haben `b2c_1_`. Sie benötigen diese Richtliniennamen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie Ihre drei Richtlinien erstellt haben, sind Sie bereit sind, Ihre app zu erstellen.

Beachten Sie, dass in diesem Artikel nicht so verwenden Sie die Richtlinien, die Sie soeben erstellte behandelt werden. Weitere Informationen zur Funktionsweise von Richtlinien in Azure AD B2C, beginnen Sie mit dem [Lernprogramm .NET Web app-erste Schritte](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Laden Sie den code

Der Code für dieses Lernprogramm [auf GitHub verwaltet wird](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet). Wenn Sie um das Beispiel beim Durcharbeiten zu erstellen, können Sie [das Gerüst Projekt als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/skeleton.zip). Sie können auch das Gerüst Klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git
```

Die fertige app steht auch [als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip) oder das `complete` Zweig des gleichen Repositorys.

Nachdem Sie den Code Stichprobe heruntergeladen haben, öffnen Sie die Visual Studio sln-Datei, um anzufangen.

## <a name="configure-the-task-web-app"></a>Konfigurieren der Aufgabe Web app

Zum Abrufen von `TaskWebApp` mit Azure AD B2C kommunizieren zu können, müssen Sie einige allgemeine Parameter bereitstellen. In der `TaskWebApp` Project Öffnen der `web.config` -Datei im Stammverzeichnis des Projekts, und Ersetzen Sie die Werte in den `<appSettings>` Abschnitt. Lassen Sie die `AadInstance`, `RedirectUri`, und `TaskServiceUrl` Werte, wie sie sind.

```
  <appSettings>
    
    ...
    
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:ClientSecret" value="E:i~5GHYRF$Y7BcM" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>

## <a name="get-access-tokens-and-call-the-task-api"></a>Erhalten Sie Access Token, und rufen Sie die Aufgabe API

In diesem Abschnitt wird erläutert, wie Sie bei der Anmeldung mit Azure AD B2C erhaltene Token verwenden, den Zugriff auf ein Web-API, die auch gesichert wird mit Azure AD B2C.

In diesem Artikel befasst sich nicht auf die Details zum Sichern der-API aus. Schauen Sie sich das [Web API überfordert Artikel](active-directory-b2c-devquickstarts-api-dotnet.md), um zu erfahren, wie eine Web-API sicher Anfragen authentifiziert mithilfe von Azure AD B2C.

### <a name="save-the-sign-in-token"></a>Speichern Sie die melden Sie sich im token

Zunächst authentifizieren Sie des Benutzers (mit einem von Ihrer Richtlinien) und erhalten Sie ein Token Anmeldung von Azure AD B2C.  Wenn Sie nicht sicher, wie Richtlinien ausgeführt wird, wechseln Sie zurück, und versuchen Sie es der [.NET Web app-erste Schritte-Lernprogramm](active-directory-b2c-devquickstarts-web-dotnet.md) lernen Sie die Grundlagen des Azure AD B2C kennen.

Öffnen Sie die Datei `App_Start\Startup.Auth.cs`.  Es gibt eine wichtige ändern müssen, um die `OpenIdConnectAuthenticationOptions` -müssen Sie festlegen `SaveSignInToken = true`.

```C#
// App_Start\Startup.Auth.cs

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
        AuthenticationFailed = OnAuthenticationFailed,
    },
    Scope = "openid",
    ResponseType = "id_token",

    TokenValidationParameters = new TokenValidationParameters
    {
        NameClaimType = "name",
        
        // Add this line to reserve the sign in token for later use
        SaveSigninToken = true,
    },
};
```

### <a name="get-a-token-in-the-controllers"></a>Abrufen einer Token in die Controller

Die `TasksController` für die Kommunikation mit dem Web-API, senden HTTP-Anfragen an die API zu lesen, erstellen und Löschen von Aufgaben verantwortlich ist.  Becuase, die die API durch Azure AD B2C gesichert wird, müssen Sie zuerst das Token abzurufen, die, das Sie in den vorstehenden Schritt gespeichert haben.

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        
    ...
}
```

Die `BootstrapContext` enthält der Anmeldung Token, die Sie erworben haben, durch eine von der B2C Richtlinien ausführen.

### <a name="read-tasks-from-the-web-api"></a>Weitere Aufgaben aus dem Web-API

Wenn Sie ein Token haben, können Sie es an den HTTP-Anfügen `GET` Anfordern der `Authorization` Kopfzeile sicher aufrufen `TaskService`:

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        ...

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, serviceUrl + "/api/tasks");

        // Add the token acquired from ADAL to the request headers
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", bootstrapContext.Token);
        HttpResponseMessage response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            String responseString = await response.Content.ReadAsStringAsync();
            JArray tasks = JArray.Parse(responseString);
            ViewBag.Tasks = tasks;
            return View();
        }
        else
        {
            // If the call failed with access denied, show the user an error indicating they might need to sign-in again.
            if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
            {
                return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
            }
        }

        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + response.StatusCode);
    }
    catch (Exception ex)
    {
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ex.Message);
    }
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a>Erstellen und Löschen von Aufgaben im Web-API

Folgen Sie dem gleichen Muster aus, wenn Sie senden `POST` und `DELETE` Anfragen im Web-API, mit der `BootstrapContext` zum Abrufen der Anmeldung Token. Wir implementiert die Erstellaktion für Sie. Versuchen Sie die Löschaktion in abschließen `TasksController.cs`.

## <a name="run-the-sample-app"></a>Führen Sie die Beispielapp

Schließlich erstellen Sie, und führen Sie die app. Registrieren und melden Sie sich an, und Erstellen von Aufgaben für den Benutzer angemeldet. Melden Sie sich ab, und melden Sie sich als ein anderer Benutzer. Erstellen von Aufgaben für diesen Benutzer. Beachten Sie, wie die Aufgaben gespeicherten pro Benutzer zur-API sind, da die API die Identität des Benutzers aus dem Token extrahiert empfängt.

Als Referenz der vollständigen Beispiel [als ZIP-Datei bereitgestellt wird](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip). Sie können auch aus GitHub Klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
