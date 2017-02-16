<properties
   pageTitle="Authentifizierung in Clientanwendungen mandantenfähigen | Microsoft Azure"
   description="Wie kann eine Anwendung mandantenfähigen aus Azure AD Benutzerauthentifizierung"
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
   ms.date="05/23/2016"
   ms.author="mwasson"/>

# <a name="authentication-in-multitenant-apps-using-azure-ad-and-openid-connect"></a>Authentifizierung in mandantenfähigen apps, Azure AD- und OpenID verbinden

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Artikel ist [Teil einer Serie](guidance-multitenant-identity.md). Es gibt auch eine vollständige [Beispiel-Anwendung] , die dieser Reihe begleitet.

Dieser Artikel beschreibt, wie eine Anwendung mandantenfähigen Benutzer Azure Active Directory (Azure AD), verwenden OpenID verbinden (OIDC) zum Authentifizieren authentifiziert werden kann.

## <a name="overview"></a>(Übersicht)

Unsere [Implementierung Bezug](guidance-multitenant-identity-tailspin.md) ist eine ASP.NET Core 1.0-Anwendung. Die Anwendung wird mit den integrierten OpenID verbinden Middleware OIDC Authentifizierung illustrieren ausgeführt. Das folgende Diagramm veranschaulicht, was passiert, wenn der Benutzer auf hoher Ebene, signiert.

![Authentifizierung Fluss](media/guidance-multitenant-identity/auth-flow.png)

1.  Der Benutzer klickt auf die Schaltfläche "Anmelden" in der app. Diese Aktion wird von MVC-Controller behandelt.
2.  MVC-Controller gibt eine Aktion **ChallengeResult** .
3.  Die Middleware hört die **ChallengeResult** ab und erstellt eine 302 Antwort, die den Benutzer zur Azure AD-Anmeldeseite umgeleitet.
4.  Die Benutzerauthentifizierung mit Azure AD.
5.  Azure AD sendet eine Token-ID an die Anwendung.
6.  Die Middleware überprüft das Token-ID an. Der Benutzer wird zu diesem Zeitpunkt jetzt in der Anwendung authentifiziert.
7.  Die Middleware leitet den Benutzer zur Anwendung wieder an.

## <a name="register-the-app-with-azure-ad"></a>Registrieren Sie die app mit Azure AD

Klicken Sie zum Aktivieren der OpenID verbinden registriert SaaS-Anbieter die Anwendung innerhalb ihrer eigenen Azure AD-Mandanten.

Um die Anwendung zu registrieren, folgen Sie den Schritten [Integration von Applications mit Azure Active Directory](../active-directory/active-directory-integrating-applications.md)im Abschnitt [Hinzufügen einer Anwendung](../active-directory/active-directory-integrating-applications.md#adding-an-application).

Auf der Seite **Konfigurieren** :

-   Beachten Sie die Client-ID
-   Klicken Sie unter die **Anwendung wird mit mehreren Mandanten**wählen Sie **Ja**aus.
-   Befehlssatz **Antworten URL** zu einer URL Azure AD wird, in dem die Authentifizierungsantwort senden. Sie können die base der app-URL ein.
  - Hinweis: Der URL-Pfad kann nichts, sein, solange die Hostname Ihre app bereitgestellte entspricht.
  - Sie können mehrere Antworten URLs festlegen. Während der Entwicklung, können Sie eine `localhost` Adresse, für die app lokal ausgeführt.
-   Generieren einer Client geheim: unter **Schlüssel**, klicken Sie auf den Dropdownpfeil, die besagt, **Wählen Sie Dauer** , und wählen Sie entweder 1 oder 2 Jahre. Die Taste werden angezeigt, wenn Sie auf **Speichern**klicken. Achten Sie darauf, um den Wert, kopieren, da es nicht erneut angezeigt wird, wenn Sie auf die Konfigurationsseite erneut laden.

## <a name="configure-the-auth-middleware"></a>Konfigurieren der Authentifizierung middleware

In diesem Abschnitt beschreibt das Konfigurieren der Authentifizierung Middleware ASP.NET Core 1.0 für die Authentifizierung mandantenfähigen OpenID verbinden.

Fügen Sie in der Startklasse der herstellen OpenID Middleware hinzu:

```csharp
app.UseOpenIdConnectAuthentication(options =>
{
    options.AutomaticAuthenticate = true;
    options.AutomaticChallenge = true;
    options.ClientId = [client ID];
    options.Authority = "https://login.microsoftonline.com/common/";
    options.CallbackPath = [callback path];
    options.PostLogoutRedirectUri = [application URI];
    options.SignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.TokenValidationParameters = new TokenValidationParameters
    {
        ValidateIssuer = false
    };
    options.Events = [event callbacks];
});
```

> [AZURE.NOTE] Finden Sie unter [Startup.cs](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Web/Startup.cs).

Weitere Informationen über die Startklasse finden Sie unter [Start der Anwendung](https://docs.asp.net/en/latest/fundamentals/startup.html) in der Dokumentation ASP.NET Core 1.0.

Legen Sie die folgenden Middleware Optionen:

- **ClientId**. Der Anwendung Client-ID, die Sie erhalten haben, wenn Sie die Anwendung in Azure AD registriert.
- **Zertifizierungsstelle**. Legen Sie für eine Anwendung mandantenfähigen dies zu `https://login.microsoftonline.com/common/`. Dies ist die URL für die allgemeine Azure AD-Endpunkt, der womit Benutzer von einem beliebigen Azure AD-Mandanten anmelden kann. Weitere Informationen zu den allgemeinen Endpunkt finden Sie unter [diesen Blogbeitrag](http://www.cloudidentity.com/blog/2014/08/26/the-common-endpoint-walks-like-a-tenant-talks-like-a-tenant-but-is-not-a-tenant/).
- Legen Sie im **TokenValidationParameters** **ValidateIssuer** auf falsch ein. Dies bedeutet, dass die app für den Herausgeber Wert in das ID-Token validieren zuständig sind. (Die Middleware überprüft weiterhin das Token selbst.) Weitere Informationen zum Überprüfen der Herausgeber von finden Sie unter [Überprüfung des Herausgebers](guidance-multitenant-identity-claims.md#issuer-validation).
- **CallbackPath**. Legen Sie diese auf den Pfad in der Antwort-URL, die Sie in Azure AD registriert. Beispielsweise, wenn die Antwort-URL ist `http://contoso.com/aadsignin`, sollten **CallbackPath** `aadsignin`. Wenn Sie diese Option nicht festgelegt, ist der Standardwert `signin-oidc`.
- **PostLogoutRedirectUri**. Geben Sie eine URL umgeleitet Benutzer nach dem Abmelden. Diese sollten sich auf einer Seite, die anonyme Anfragen ermöglicht &mdash; in der Regel auf der Startseite.
- **SignInScheme**. Legen Sie den Wert `CookieAuthenticationDefaults.AuthenticationScheme`. Diese Einstellung bedeutet, dass nach der Benutzer authentifiziert wird, die Benutzeransprüche lokal in einem Cookie gespeichert sind. Dieses Cookie ist wie der Benutzer während der Browsersitzung angemeldeten bleibt.
- **Ereignisse.** Ereignisrückrufen; finden Sie unter [Authentifizierungsereignissen](#authentication-events).

Auch der Verkaufspipeline die Cookie-Authentifizierung Middleware hinzufügen. Diese Middleware ist verantwortlich für die Benutzeransprüche auf ein Cookie schreiben, und klicken Sie dann das Cookie lesen, während die Seite anschließend geladen.

```csharp
app.UseCookieAuthentication(options =>
{
    options.AutomaticAuthenticate = true;
    options.AutomaticChallenge = true;
    options.AccessDeniedPath = "/Home/Forbidden";
});
```

## <a name="initiate-the-authentication-flow"></a>Einleiten des Ablaufs Authentifizierung

Zum illustrieren Authentifizierung in ASP.NET-MVC beginnen möchten, geben Sie eine **ChallengeResult** aus der Controller zurück:

```csharp
[AllowAnonymous]
public IActionResult SignIn()
{
    return new ChallengeResult(
        OpenIdConnectDefaults.AuthenticationScheme,
        new AuthenticationProperties
        {
            IsPersistent = true,
            RedirectUri = Url.Action("SignInCallback", "Account")
        });
}
```

Dadurch wird die Middleware Reaktionen 302 (gefunden) zurück, die an den Endpunkt Authentifizierung leitet.

## <a name="user-login-sessions"></a>Benutzer Login Sitzungen

Wie erwähnt, wenn der Benutzer zuerst anmeldet, schreibt die Cookie-Authentifizierung Middleware Benutzer Claims to ein Cookie aus. Anschließend werden HTTP-Anfragen authentifiziert, durch das Cookie lesen.

Standardmäßig schreibt die Cookie Middleware eine [Sitzungscookie][session-cookie], den Browser schließt die Ruft einmal der Benutzer gelöscht. Das nächste Mal, das der Benutzer neben die Website besucht, haben sie sich erneut anzumelden. Wenn Sie in der **ChallengeResult**true **IsPersistent** festlegen, schreibt jedoch die Middleware ein dauerhafte Cookie, sodass der Benutzer angemeldet bleibt nach dem Schließen des Browsers. Sie können Cookies konfigurieren. [Steuern des Cookieoptionen]finden Sie unter[cookie-options]. Dauerhafte Cookies eignen sich besser für den Benutzer, aber möglicherweise unangemessene für einige Applikationen (z. B. eine Anwendung Bank), der Sie den Benutzer in jedem anmelden möchten.

## <a name="about-the-openid-connect-middleware"></a>Informationen zu den Middleware OpenID verbinden

Die Middleware OpenID verbinden in ASP.NET Blendet die meisten Protokolldetails. Dieser Abschnitt enthält einige Hinweise zur Implementierung, die für das Verständnis des Ablaufs Protokoll nützlich sein können.

Zunächst sehen wir uns Authentifizierung illustrieren im Hinblick auf ASP.NET (ignorieren die Details des OIDC Protokoll Flusses zwischen der app und Azure AD-). Das folgende Diagramm veranschaulicht den Prozess.

![Anmeldung Fluss](media/guidance-multitenant-identity/sign-in-flow.png)

In diesem Diagramm gibt es zwei MVC Controller. Der Kontocontroller verarbeitet anmeldeanforderungen und der Home-Controller von der Homepage der fungiert.

Hier umfasst die Authentifizierung:

1. Der Benutzer klickt auf die Schaltfläche "Anmelden", und im Browser sendet eine GET-Anforderung. Beispiel: `GET /Account/SignIn/`.
2. Das Konto Controller gibt eine `ChallengeResult`.
3. Die Middleware OIDC gibt eine HTTP-302 Antwort, zu Azure AD umzuleiten.
4. Im Browser sendet die Anforderung Authentifizierung an Azure AD
5. Der Benutzer meldet sich bei Azure AD und Azure AD sendet eine Authentifizierungsantwort zurück.
6. Die Middleware OIDC erstellt eine Ansprüche Tilgungsanteile, und übergibt sie an die Middleware Cookie-Authentifizierung.
7. Die Middleware Cookie serialisiert die Ansprüche Tilgungsanteile und ein Cookie festlegt.
8. Die Middleware OIDC leitet an die URL der Anwendung Rückruf.
10. Im Browser folgt die Umleitung, das Cookie in der Besprechungsanfrage senden.
11. Die Middleware Cookie deserialisiert Cookies, ein Hauptbenutzer Ansprüche und legt `HttpContext.User` die Ansprüche Tilgungsanteile gleich. Die Anfrage wird an einen MVC-Controller weitergeleitet.

### <a name="authentication-ticket"></a>Authentifizierungsticket

Wenn Authentifizierung erfolgreich ist, wird die OIDC Middleware ein Authentifizierungsticket, die eine Ansprüche Tilgungsanteile enthält, der Ansprüche des Benutzers erstellt. Sie können das Ticket innerhalb des Ereignisses **AuthenticationValidated** oder **TicketReceived** zugreifen.

> [AZURE.NOTE] Bis zum Abschluss des gesamten Authentifizierung Ablaufs `HttpContext.User` immer noch eine anonyme Hauptbenutzer, _nicht_ den authentifizierten Benutzer enthält. Die anonyme Tilgungsanteile verfügt über eine leere Claims-Sammlung. Nach abgeschlossener Authentifizierung und leitet der app, die Middleware Cookie deserialisiert die Authentifizierungscookie und Sätze `HttpContext.User` zu einer Ansprüche der Tilgungsanteile, die den authentifizierten Benutzer darstellt.

### <a name="authentication-events"></a>Authentifizierungsereignisse

Während der Authentifizierung löst die verbinden OpenID Middleware eine Reihe von Ereignissen aus:

- **RedirectToAuthenticationEndpoint**. Vor die Middleware an den Endpunkt Authentifizierung leitet rechts aufgerufen. Sie können dieses Ereignis verwenden, so ändern Sie die URL für die Umleitung; Wenn Sie beispielsweise Anforderungsparameter hinzuzufügen. Ein Beispiel finden Sie unter [Hinzufügen der Administrator Zustimmung auffordern](guidance-multitenant-identity-signup.md#adding-the-admin-consent-prompt) .

- **AuthorizationResponseReceived**. Aufgerufen, nachdem die Middleware aus der Identitätsanbieter (IDP) die Authentifizierungsantwort empfangen, aber bevor die Middleware die Antwort überprüft.  

- **AuthorizationCodeReceived**. Mit den Autorisierungscode aufgerufen.

- **TokenResponseReceived**. Aufgerufen, nachdem die Middleware eine Access aus der IDP token bezieht. Gilt nur für die Autorisierung Code Fluss.

- **AuthenticationValidated**. Aufgerufen, nachdem die Middleware das ID-Token überprüft. An diesem Punkt muss die Anwendung eine Reihe von überprüften Ansprüche über den Benutzer. Sie können dieses Ereignis zum Ausführen einer zusätzlichen Überprüfung auf die Ansprüche oder Ansprüche transformieren. Finden Sie unter [Arbeiten mit Ansprüche](guidance-multitenant-identity-claims.md).

- **UserInformationReceived**. Aufgerufen, wenn die Middleware das Benutzerprofil aus den Endpunkt des Benutzers Informationen bezieht. Gilt nur für die Autorisierung Code Fluss und nur, wenn `GetClaimsFromUserInfoEndpoint = true` in den Middleware-Optionen.

- **TicketReceived**. Aufgerufen, wenn die Authentifizierung abgeschlossen ist. Dies ist das letzte Ereignis, vorausgesetzt, dass die Authentifizierung erfolgreich ist. Nachdem das Ereignis behandelt wird, wird der Benutzer in der app signiert.

- **AuthenticationFailed**. Aufgerufen, wenn die Authentifizierung fehlschlägt. Mit diesem Ereignis können Fehler bei der Authentifizierung verarbeitet &mdash; z. B., indem Sie auf eine Fehlerseite umleiten.

Um für diese Ereignisse bereitzustellen, legen Sie die Option **Ereignisse** die Middleware. Es gibt zwei verschiedene Verfahren zum deklarieren die Ereignishandler: Inline mit Lambda-Ausdrücken oder in einer Klasse, die von **OpenIdConnectEvents**abgeleitet wird.

Inline mit Lambda-Ausdrücken:

```csharp
app.UseOpenIdConnectAuthentication(options =>
{
    // Other options not shown.

    options.Events = new OpenIdConnectEvents
    {
        OnTicketReceived = (context) =>
        {
             // Handle event
             return Task.FromResult(0);
        },
        // other events
    }
});
```

Abgeleitet von **OpenIdConnectEvents**:

```csharp
public class SurveyAuthenticationEvents : OpenIdConnectEvents
{
    public override Task TicketReceived(TicketReceivedContext context)
    {
        // Handle event
        return base.TicketReceived(context);
    }
    // other events
}

// In Startup.cs:
app.UseOpenIdConnectAuthentication(options =>
{
    // Other options not shown.

    options.Events = new SurveyAuthenticationEvents();
});
```

Der zweite Ansatz empfiehlt sich bei Ihrer Ereignisrückrufen wesentlichen Logik, damit sie Ihre Startklasse Datenmüll nicht. Implementierung des Verweis verwendet dieser Ansatz; finden Sie unter [SurveyAuthenticationEvents.cs](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Web/Security/SurveyAuthenticationEvents.cs).

### <a name="openid-connect-endpoints"></a>OpenID verbinden Endpunkte

Azure AD unterstützt [OpenID verbinden Discovery](https://openid.net/specs/openid-connect-discovery-1_0.html), in dem der Identitätsanbieter (IDP) ein JSON-Metadaten-Dokument von einem [bekannten Endpunkt](https://openid.net/specs/openid-connect-discovery-1_0.html#ProviderConfig)gibt. Das Metadatendokument sind Informationen enthalten:

-   Die URL des Endpunkts Autorisierung. Dies ist die Stelle, an der die app leitet zum Authentifizieren des Benutzers.
-   Die URL der Stelle, an der die app zum Abmelden des Benutzers wechselt, den "Sitzung beenden"-Endpunkt.
-   Die URL für den signierenden Schlüssel abzurufen, die der Client verwendet, um die Token OIDC überprüfen, die sie aus der IDP bezieht.

Standardmäßig weiß die OIDC Middleware, wie diese Metadaten abgerufen werden sollen. Legen die Option **Zertifizierungsstelle** in die Middleware, und die Middleware erstellt die URL für die Metadaten. (Sie können die Metadaten-URL überschreiben, indem Sie die Option **MetadataAddress** .)

### <a name="openid-connect-flows"></a>OpenID verbinden Zahlungen

Standardmäßig verwendet die OIDC Middleware Hybrid Fluss mit Formular Beitrag Antwort Modus.

-   _Hybrid Fluss_ bedeutet, dass der Client eine Token-ID und eine Autorisierungscode in der gleichen Schleife auf dem Server Autorisierung zugreifen kann.
-   _Formular Posten Antwort Modus_ bedeutet, dass der Server für die Autorisierung eine Anforderung HTTP POST verwendet, um die ID-Token und Autorisierung Code bei der app zu senden. Die Werte sind Formular-Urlencoded (Inhaltstyp = "Application/X-www-form-urlencoded").

Wenn die Middleware OIDC an den Endpunkt Autorisierung umgeleitet, enthält die URL umleiten alle die Zeichenfolge Abfrageparameter von OIDC benötigt. Für den Hybrid Verkehr:

-   Client_id. Dieser Wert wird in die Option **ClientId** festgelegt.
-   Umfang = "Openid Profil", d. h., es ist eine Anforderung OIDC, und wir wollen das Profil des Benutzers.
-   Response_type = "Id_token code". Dies gibt Hybrid Fluss an.
-   Response_mode = "Form_post". Dies gibt Formular Beitrag Antwort an.

Um einen anderen Fluss angeben möchten, legen Sie die Eigenschaft **ResponseType** auf Optionen. Beispiel:

```csharp
app.UseOpenIdConnectAuthentication(options =>
{
    options.ResponseType = "code"; // Authorization code flow

    // Other options
}
```

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie den nächsten Artikel in dieser Reihe: [Arbeiten mit anspruchsbasierte Identitäten in mandantenfähigen Clientanwendungen][claims]


[claims]: guidance-multitenant-identity-claims.md
[cookie-options]: https://docs.asp.net/en/latest/security/authentication/cookie.html#controlling-cookie-options
[session-cookie]: https://en.wikipedia.org/wiki/HTTP_cookie#Session_cookie
[Beispiel-Anwendung]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
