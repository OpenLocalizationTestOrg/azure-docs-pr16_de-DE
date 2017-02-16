<properties
   pageTitle="Sichern einer Back-End-Webs API in einer Anwendung mandantenfähigen | Microsoft Azure"
   description="Sichern eine Back-End-Web-API"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="securing-a-backend-web-api-in-a-multitenant-application"></a>Sichern einer Back-End-Webs API in einer mandantenfähigen Anwendung

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel ist [Teil einer Serie]. Es gibt auch eine vollständige [Beispiel-Anwendung] , die dieser Reihe begleitet.

Die Anwendung [Tailspin Umfragen] verwendet eine Back-End-Web-API Vorgänge auf Umfragen verwalten. Wenn ein Benutzer "Meine Umfragen" klickt, sendet der Web-Anwendung eine HTTP-Anforderung im Web-API an:

```
GET /users/{userId}/surveys
```

Das Web-API gibt ein JSON-Objekt:

```
{
  "Published":[],
  "Own":[
    {"Id":1,"Title":"Survey 1"},
    {"Id":3,"Title":"Survey 3"},
    ],
  "Contribute": [{"Id":8,"Title":"My survey"}]
}
```

Im Web API lässt nicht anonyme Anfragen, damit das Web app selbst mit OAuth 2 Person Token authentifizieren muss.

> [AZURE.NOTE] Dies ist eine Server-zu-Server-Szenario. Die Anwendung keine AJAX-API aus dem Webclient anrufen.

Es gibt zwei Hauptmethoden, die Sie ergreifen können:

- Delegierte Benutzeridentität. Die Webanwendung authentifiziert mit der Identität des Benutzers.
- Anwendungsidentität. Die Webanwendung authentifiziert mit seine Client-ID, die mit OAuth2 Client Anmeldeinformationen Fluss.

Die Anwendung Tailspin implementiert delegierten Benutzeridentität. Hier sind die wichtigsten Unterschiede:

**Delegierte Benutzeridentität**

- Im Web API gesendete Person Token enthält die Identität des Benutzers an.
- Das Web-API macht Autorisierung Entscheidungen ausgehend von der Identität des Benutzers an.
- Die Anwendung muss 403 (Verboten) Fehler aus dem Web API, wenn der Benutzer nicht autorisiert ist, eine Aktion zu behandeln.
- In der Regel ist die Webanwendung noch einige Autorisierung Entscheidungen, die Benutzeroberfläche, z. B. ein- oder Ausblenden von Benutzeroberflächenelementen beeinflussen).
- Die Web-API kann potenziell von nicht vertrauenswürdigen Clients, wie z. B. eine JavaScript-Anwendung oder einer systemeigenen Clientanwendung verwendet werden.

**Anwendungsidentität**

- Im Web API erhält keine Informationen über den Benutzer.
- Alle Autorisierung ausgehend von der Identität des Benutzers nicht im Web-API möglich. Alle Autorisierung Entscheidungen, die von der Webanwendung vorgenommen wurden.  
- Die Web-API kann nicht von einem nicht vertrauenswürdigen Client (JavaScript oder native Client-Anwendung) verwendet werden.
- Dieser Ansatz möglicherweise etwas einfacher zu implementieren, da es keine Autorisierungslogik in der Web-API gibt.

Bei beiden Vorgehensweisen muss die Webanwendung einer Access-Token abzurufen, die ist der Anmeldeinformationen erforderlich, um die Web-API aufzurufen.

- Für delegierte Benutzeridentität muss das Token im Namen des Benutzers auszustellen, können von der IDP stammen.

- Für Client-Anmeldeinformationen möglicherweise eine Anwendung Token aus der IDP abrufen oder einen eigene token-Server hosten. (Aber nicht schreiben token-Server völlig; ein gut getesteten Framework wie [IdentityServer3]verwenden.) Wenn Sie mit Azure AD authentifizieren, hat es dringend empfohlen, das Access-Token aus Azure AD, selbst bei Client Anmeldeinformationen Fluss abrufen.

Im weiteren Verlauf dieses Artikels wird davon ausgegangen, dass die Anwendung mit Azure AD authentifiziert wird.

![Abrufen des Access-Tokens](media/guidance-multitenant-identity/access-token.png)

## <a name="register-the-web-api-in-azure-ad"></a>Registrieren Sie sich im Web API in Azure AD

In der Reihenfolge für Azure AD zu einer Person für das Web API auszustellen müssen Sie einige Elemente Azure AD konfigurieren.

1. [Registrieren Sie sich im Web-API in Azure AD].

2. Fügen Sie die Client-ID des Web app auf das Web API Anwendungsmanifest, in der `knownClientApplications` Eigenschaft. Finden Sie unter [Aktualisieren der Anwendungsmanifeste].

3. [Erteilen der Erlaubnis der Web-Anwendung im Web-API Aufrufen].

  Sie können im Verwaltungsportal Azure, zwei Arten von Berechtigungen festlegen: "Anwendung von Berechtigungen" für Anwendungsidentität (Client Anmeldeinformationen Fluss) oder "Delegierte Berechtigungen" für delegierte Benutzeridentität.

  ![Delegieren von Berechtigungen](media/guidance-multitenant-identity/delegated-permissions.png)

## <a name="getting-an-access-token"></a>Abrufen einer Access-token

Vor dem Aufrufen des Web-API, erhält die Webanwendung eine Access aus Azure AD token. Verwenden Sie in einer Anwendung .NET der [Azure AD Authentifizierung Bibliothek (ADAL) für .NET][ADAL].

Im OAuth 2 Autorisierung Code Fluss austauscht die Anwendung einen Autorisierungscode für eine Access-Token. Im folgenden Code wird ADAL das Access-Token abrufen. Aufrufen des Codes ist während der `AuthorizationCodeReceived` Ereignis.

```csharp
// The OpenID Connect middleware sends this event when it gets the authorization code.   
public override async Task AuthorizationCodeReceived(AuthorizationCodeReceivedContext context)
{
    string authorizationCode = context.ProtocolMessage.Code;
    string authority = "https://login.microsoftonline.com/" + tenantID
    string resourceID = "https://tailspin.onmicrosoft.com/surveys.webapi" // App ID URI
    ClientCredential credential = new ClientCredential(clientId, clientSecret);

    AuthenticationContext authContext = new AuthenticationContext(authority, tokenCache);
    AuthenticationResult authResult = await authContext.AcquireTokenByAuthorizationCodeAsync(
        authorizationCode, new Uri(redirectUri), credential, resourceID);

    // If successful, the token is in authResult.AccessToken
}
```

Hier sind die verschiedenen Parameter, die erforderlich sind:

- `authority`. Abgeleitet von den Mandanten-ID des Benutzers Anmeldung bei. (Nicht die Mandanten-ID des Anbieters SaaS)  
- `authorizationCode`. der Code der Authentifizierung, den Sie wieder von der IDP erhalten haben.
- `clientId`. Die Webanwendung Client-ID an.
- `clientSecret`. Die Webanwendung Client geheim.
- `redirectUri`. Verbinden Sie die Umleitung URI, die Sie für OpenID festlegen. Dies ist die Stelle, an der die IDP wieder Anrufe mit dem Token.
- `resourceID`. Der App-ID URI des Web-API, die Sie erstellt, wenn Sie das Web-API in Azure AD registriert haben.
- `tokenCache`. Ein Objekt, das die Access-Token speichert. Finden Sie unter [Token Zwischenspeichern].

Wenn `AcquireTokenByAuthorizationCodeAsync` erfolgreich ist, ADAL speichert das Token. Später können Sie das Token aus dem Cache erhalten, indem Sie AcquireTokenSilentAsync:

```csharp
AuthenticationContext authContext = new AuthenticationContext(authority, tokenCache);
var result = await authContext.AcquireTokenSilentAsync(resourceID, credential, new UserIdentifier(userId, UserIdentifierType.UniqueId));
```

wo `userId` Objekt-ID des Benutzers, die sich im befindet ist die `http://schemas.microsoft.com/identity/claims/objectidentifier` beanspruchen.

## <a name="using-the-access-token-to-call-the-web-api"></a>Verwenden das Access-Token, um im Web API anzurufen

Nachdem Sie das Token haben, senden Sie es in der Kopfzeile Autorisierung HTTP-Anfragen im Web-API.

```
Authorization: Bearer xxxxxxxxxx
```

Die folgende Erweiterungsmethode aus der Anwendung Umfragen legt die Autorisierung Kopfzeile auf einer HTTP-Anforderung, die **HttpClient** -Klasse verwenden.

```csharp
public static async Task<HttpResponseMessage> SendRequestWithBearerTokenAsync(this HttpClient httpClient, HttpMethod method, string path, object requestBody, string accessToken, CancellationToken ct)
{
    var request = new HttpRequestMessage(method, path);
    if (requestBody != null)
    {
        var json = JsonConvert.SerializeObject(requestBody, Formatting.None);
        var content = new StringContent(json, Encoding.UTF8, "application/json");
        request.Content = content;
    }

    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
    request.Headers.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

    var response = await httpClient.SendAsync(request, ct);
    return response;
}
```

> [AZURE.NOTE] Finden Sie unter [HttpClientExtensions.cs].

## <a name="authenticating-in-the-web-api"></a>Im Web API authentifizieren

Das Web API hat das Token Person authentifizieren. In ASP.NET Core 1.0, können Sie die [Microsoft.AspNet.Authentication.JwtBearer] [ JwtBearer] Paket. Dieser Kurs bietet Middleware, die die Anwendung OpenID verbinden Person Token zu erhalten.

Registrieren die Middleware in Ihrem Web API `Startup` Class.

```csharp
app.UseJwtBearerAuthentication(options =>
{
    options.Audience = "[app ID URI]";
    options.Authority = "https://login.microsoftonline.com/common/";
    options.TokenValidationParameters = new TokenValidationParameters
    {
        //Instead of validating against a fixed set of known issuers, we perform custom multi-tenant validation logic
        ValidateIssuer = false,
    };
    options.Events = new SurveysJwtBearerEvents();
});
```

> [AZURE.NOTE] Finden Sie unter [Startup.cs].

- **Zielgruppe**. Legen Sie diese Option, um die App-ID-URL für das Web-API, die Sie erstellt haben, wenn Sie im Web-API in Azure Active Directory registriert.
- **Zertifizierungsstelle**. Legen Sie für eine Anwendung mandantenfähigen dies zu `https://login.microsoftonline.com/common/`.
- **TokenValidationParameters**. Legen Sie für eine Anwendung mandantenfähigen **ValidateIssuer** auf False ein. Dies bedeutet, dass die Anwendung der Herausgeber überprüft werden.
- **Ereignisse** ist eine Klasse, die von **JwtBearerEvents**abgeleitet wird.

### <a name="issuer-validation"></a>Überprüfung des Herausgebers

Überprüfen der Tokenausgeber in das Ereignis **JwtBearerEvents.ValidatedToken** . Der Herausgeber wird in den Anspruch "Iss" gesendet.

In der Anwendung Umfragen behandeln nicht im Web API [Mandanten Anmeldung]. Sie überprüft daher nur, wenn der Herausgeber bereits in der Anwendungsdatenbank ist. Wenn dies nicht der Fall ist, wird eine Ausnahme, wodurch Authentifizierung fehlschlägt.

```csharp
public override async Task ValidatedToken(ValidatedTokenContext context)
{
    var principal = context.AuthenticationTicket.Principal;
    var tenantManager = context.HttpContext.RequestServices.GetService<TenantManager>();
    var userManager = context.HttpContext.RequestServices.GetService<UserManager>();
    var issuerValue = principal.GetIssuerValue();
    var tenant = await tenantManager.FindByIssuerValueAsync(issuerValue);

    if (tenant == null)
    {
        // the caller was not from a trusted issuer - throw to block the authentication flow
        throw new SecurityTokenValidationException();
    }
}
```

> [AZURE.NOTE] Finden Sie unter [SurveysJwtBearerEvents.cs].

Sie können auch das Ereignis **ValidatedToken** verwenden, [Ansprüche Transformation]ausführen. Beachten Sie, dass die Ansprüche direkt aus Azure AD stammen, wenn die Webanwendung Transfor Ansprüche haben, die nicht im Token Person wiedergegeben werden, dass das Web-API empfängt.

## <a name="authorization"></a>Autorisierung

Eine allgemeine Beschreibung der Autorisierung, finden Sie unter [rollenbasierte und Ressourcen-basierte Autorisierung][Authorization]. 

Die Middleware JwtBearer übernimmt die Autorisierung Antworten. Angenommen, um eine Aktion Controller authentifizierte Benutzer einschränken möchten, verwenden Sie die Atrribute **[autorisieren]** und geben Sie **JwtBearerDefaults.AuthenticationScheme** als Authentifizierungsschema an:

```csharp
[Authorize(ActiveAuthenticationSchemes = JwtBearerDefaults.AuthenticationScheme)]
```

Einen Statuscode 401 zurückgegeben, wenn der Benutzer nicht authentifiziert wird.

Um eine Aktion Controller durch Authorizaton Richtlinie einschränken möchten, geben Sie den Namen der Richtlinie in das Attribut **[autorisieren]** aus:

```csharp
[Authorize(Policy = PolicyNames.RequireSurveyCreator)]
```

Dies zurückgegeben einen Statuscode 401, wenn der Benutzer nicht authentifiziert ist und 403, wenn der Benutzer authentifiziert, aber nicht die erforderlichen Berechtigungen. Registrieren Sie sich die Richtlinie beim Start:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthorization(options =>
    {
        options.AddPolicy(PolicyNames.RequireSurveyCreator,
            policy =>
            {
                policy.AddRequirements(new SurveyCreatorRequirement());
                policy.AddAuthenticationSchemes(JwtBearerDefaults.AuthenticationScheme);
            });
    });
}
```

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie den nächsten Artikel in dieser Reihe: [Zwischenspeichern Access Token in einer mandantenfähigen Anwendung][token cache]

<!-- links -->
[ADAL]: https://msdn.microsoft.com/library/azure/jj573266.aspx
[JwtBearer]: https://www.nuget.org/packages/Microsoft.AspNet.Authentication.JwtBearer
[Teil einer Serie]: guidance-multitenant-identity.md
[Tailspin Umfragen]: guidance-multitenant-identity-tailspin.md
[IdentityServer3]: https://github.com/IdentityServer/IdentityServer3
[Registrieren Sie sich im Web API in Azure AD]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md#register-the-surveys-web-api
[Aktualisieren Sie die Anwendungsmanifeste]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md#update-the-application-manifests
[Geben Sie die Web-Anwendung-Berechtigung zum Aufrufen des Web-API]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md#give-the-web-app-permissions-to-call-the-web-api
[Zwischenspeichern von Token]: guidance-multitenant-identity-token-cache.md
[HttpClientExtensions.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Common/HttpClientExtensions.cs
[Startup.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.WebAPI/Startup.cs
[Anmeldung Mandanten]: guidance-multitenant-identity-signup.md
[SurveysJwtBearerEvents.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.WebAPI/SurveyJwtBearerEvents.cs
[Ansprüche-Transformation zurück]: guidance-multitenant-identity-claims.md#claims-transformations
[Authorization]: guidance-multitenant-identity-authorize.md
[Beispiel-Anwendung]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[token cache]: guidance-multitenant-identity-token-cache.md
