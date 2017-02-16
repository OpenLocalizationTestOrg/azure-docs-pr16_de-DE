<properties
   pageTitle="Azure Active Directory Version 2.0 Authentifizierung Bibliotheken | Microsoft Azure"
   description="Clientbibliotheken kompatibel und Server Middleware Bibliotheken und gehörende Bibliothek, Quelle und Beispiele Verknüpfungen, für den Azure-Active Directory-Version 2.0-Endpunkt."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="skwan;bryanla"/>


# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory Version 2.0 Authentifizierung Bibliotheken
Der Endpunkt Azure Active Directory (Azure AD) Version 2.0 unterstützt Industriestandard OAuth 2.0 und OpenID verbinden 1.0 Protokolle. Sie können verschiedene Bibliotheken von Microsoft und anderen Organisationen mit den Endpunkt Version 2.0 verwenden.

Wenn Sie eine Anwendung, die den Endpunkt Version 2.0 verwendet erstellen, es empfiehlt sich, dass Sie Bibliotheken verwenden, die von Experten beantwortet Protocol-Domäne geschrieben werden, die eine Methode Security Development Lifecycle (SDL), wie [die gefolgt von Microsoft]folgen[Microsoft-SDL]. Wenn Sie zur Unterstützung von Hand-Code in die Protokolle entscheiden, empfiehlt es SDL Methoden folgen, und achten Sie insbesondere auf die zur Sicherheit in den Spezifikationen Standards für jedes Protokoll.

## <a name="types-of-libraries"></a>Typen von Bibliotheken

Azure AD-Version 2.0 arbeitet mit zwei Typen von Bibliotheken auf:

- **Client-Bibliotheken**. Systemeigene-Clients und -Servern mithilfe von Clientbibliotheken um Access Token für das Anrufen von einer Ressource, wie etwa Microsoft Graph zu erhalten.
- **Server-Middleware Bibliotheken**. Verwenden Sie Web apps Server Middleware Bibliotheken für Benutzer anmelden. Web-APIs verwenden Server Middleware Bibliotheken Token zu bestätigen, die durch systemeigenen Clients oder von anderen Servern gesendet werden.

## <a name="library-support"></a>Unterstützung für Bibliothek
Da Sie alle Standard-Bibliothek auswählen können, wenn Sie den Endpunkt Version 2.0 verwenden, ist es wichtig zu wissen, wo Sie Unterstützung zu erhalten. Probleme und Feature Besprechungsanfragen in der Bibliothekscode wenden Sie sich an den Bibliotheksbesitzer. Probleme und Feature Besprechungsanfragen in der Dienst angeordneten Protokoll Implementierung wenden Sie sich an Microsoft.

Bibliotheken gibt zwei Kategorien von Support:

- **Microsoft unterstützt**. Microsoft stellt Updates für diese Bibliotheken und hat SDL fertig fristgerechte auf diese Bibliotheken.
- **Kompatibel**. Microsoft hat diese Bibliotheken in grundlegende Szenarios getestet und bestätigt, dass sie mit den Endpunkt Version 2.0 funktionieren. Microsoft bietet keine Problembehebungen für diese Bibliotheken und hat einen Durchgang anhand dieser Bibliotheken nicht erfolgt. Probleme und Featureanfragen sollten an der Bibliothek Open Source-Projekt weitergeleitet werden.

Eine Liste der Bibliotheken, die mit den Endpunkt Version 2.0 arbeiten, finden Sie in den nächsten Abschnitten in diesem Artikel.

## <a name="microsoft-supported-client-libraries"></a>Microsoft unterstützt Client-Bibliotheken
| Plattform| Name der Bibliothek| Herunterladen | Quellcode | Beispiel |
| :-: | :-: | :-: | :-: | :-: |
| .NET, Windows Store-Xamarin | Microsoft-Authentifizierung Bibliothek (MSAL) für .NET | [Microsoft.Identity.Client (NuGet)][ClientLib-NET-Lib] | [MSAL für .NET (GitHub)][ClientLib-NET-Repo] | [Beispiel für Windows-desktop-native client][ClientLib-NET-Sample] |
| Node.js | Microsoft Azure-Active Directory-Passport.js-Plug-in | [Passport Azure AD (Npm)][ClientLib-Node-Lib] | [Passport Azure AD (GitHub)][ClientLib-Node-Repo] | Demnächst |

<!--- COMMENTING OUT UNTIL THEY ARE READY
| iOS, Mac | Microsoft Authentication Library (MSAL) for ObjC | In development | In development | In development |
| Android | Microsoft Authentication Library (MSAL) for Android | In development | In development | In development |
| JavaScript | Microsoft Authentication Library (MSAL) for JavaScript | In development | In development | In development |
 -->

## <a name="microsoft-supported-server-middleware-libraries"></a>Microsoft unterstützt Server Middleware Bibliotheken
| Plattform| Name der Bibliothek| Herunterladen | Quellcode | Beispiel |
| :-: | :-: | :-: | :-: | :-: |
| .NET 4.x | OWIN OpenID verbinden Middleware für ASP.NET | [Microsoft.Owin.Security.OpenIdConnect (NuGet)][ServerLib-Net4-Owin-Oidc-Lib] | [Katana Project (CodePlex)][ServerLib-Net4-Owin-Oidc-Repo] | [Beispiel für Web app][ServerLib-Net4-Owin-Oidc-Sample] |
| .NET 4.x | OWIN OAuth Person Middleware für ASP.NET | [Microsoft.Owin.Security.OAuth (NuGet)][ServerLib-Net4-Owin-Oauth-Lib] | [Katana Project (CodePlex)][ServerLib-Net4-Owin-Oauth-Repo] | [Beispiel für die Web-API][ServerLib-Net4-Owin-Oauth-Sample] |
| .NET core | OWIN OpenID Middleware für .NET Core verbinden | [Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] | [ASP.NET-Sicherheit (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] | [Beispiel für Web app][ServerLib-NetCore-Owin-Oidc-Sample] |
| .NET core | OWIN OAuth Person Middleware für .NET Core | [Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] | [ASP.NET-Sicherheit (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] | Demnächst |
| Node.js | Microsoft Azure Active Directory Passport.js-Plug-in | [Passport Azure AD (Npm)][ServerLib-Node-Lib] | [Passport Azure AD (GitHub)][ServerLib-Node-Repo] | [Beispiel für Web app][ServerLib-Node-Sample] |
<!--- COMMENTING UNTIL SAMPLE IS AVAILABLE
| .NET 4.x, .NET Core | JSON Web Token Handler for .NET | [System.IdentityModel.Tokens.Jwt (NuGet)][ServerLib-Net-Jwt-Lib] | [Azure AD identity model extensions for .NET (GitHub)][ServerLib-Net-Jwt-Repo] | Coming soon |
--->
## <a name="compatible-client-libraries"></a>Kompatible Client-Bibliotheken
| Plattform| Name der Bibliothek | Getesteten version | Quellcode | Beispiel |
| :-: | :-: | :-: | :-: | :-: |
| Android | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/wiki) | 0.2.1 | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) | [Beispiel für systemeigene app](active-directory-v2-devquickstarts-android.md) |
| iOS | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | 1.2.8 | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | [Beispiel für systemeigene app](active-directory-v2-devquickstarts-ios.md)|
| JavaScript | [Hello.js](https://adodson.com/hello.js/) | 1.13.5 | [Hello.js](https://github.com/MrSwitch/hello.js) | Demnächst |
| Python-wird | [Man-OAuthlib](https://github.com/lepture/flask-oauthlib) | 0.9.3 | [Man-OAuthlib](https://github.com/lepture/flask-oauthlib) | Demnächst |
| Ruby | [OmniAuth](https://github.com/omniauth/omniauth/wiki) | omniauth:1.3.1</br>Omniauth-Oauth2:1.4.0 | [OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) | Demnächst |
<!--- REMOVING BRANDON'S FOR NOW
|  |  |  |  |  |
| Android | [OAuth2 Client](https://github.com/wuman/android-oauth-client) |   | [OAuth2 Client](https://github.com/wuman/android-oauth-client)  | Coming soon  |
| Java | [WSO2 Identity Server](https://docs.wso2.com/display/IS500/Introducing+the+Identity+Server) | [Version 5.2.0](http://wso2.com/products/identity-server/) | [Source](https://docs.wso2.com/display/IS500/Building+from+Source) | [Samples index](https://docs.wso2.com/display/IS500/Samples)  |
| Java | [Java Gluu Server](https://gluu.org/docs/) |   | [oxAuth](https://github.com/GluuFederation/oxAuth)  | Coming soon |
| Node.js | [NPM passport-openidconnect](https://www.npmjs.com/package/passport-openidconnect) | 0.0.1  | [Passport-OpenID Connect](https://github.com/jaredhanson/passport-openidconnect) | Coming soon  |
| PHP | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP) |   | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP)  | Coming soon  |
-->

## <a name="compatible-server-middleware-libraries"></a>Kompatible Server Middleware-Bibliotheken
Demnächst

## <a name="related-content"></a>Siehe auch
Weitere Informationen zu den Azure AD-Endpunkt Version 2.0, finden Sie unter der [Azure AD-app Modell Version 2.0 Übersicht][AAD-App-Model-V2-Overview].

Helfen Sie uns verfeinern und modellieren unsere Inhalte, verwenden Sie die Kommentarfunktion Disqus am Ende dieses Artikels, Feedback zu geben.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:
[ClientLib-Iosmac-Lib]:
[ClientLib-Iosmac-Repo]:
[ClientLib-Iosmac-Sample]:
[ClientLib-Android-Lib]:
[ClientLib-Android-Repo]:
[ClientLib-Android-Sample]:
[ClientLib-Js-Lib]:
[ClientLib-Js-Repo]:
[ClientLib-Js-Sample]:
[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-node-web/
