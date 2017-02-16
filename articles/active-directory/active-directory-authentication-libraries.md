<properties
   pageTitle="Azure-Active Directory-Authentifizierung Bibliotheken | Microsoft Azure"
   description="Der Azure AD Authentifizierung Bibliothek (ADAL) ermöglicht Client Entwicklern das problemlos in die cloud Benutzerauthentifizierung oder lokalen Active Directory (AD) und dann Access Token zum Sichern von API Anrufe zu erhalten."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-authentication-libraries"></a>Azure-Active Directory-Authentifizierung Bibliotheken

Die Azure AD-Authentifizierung Bibliothek (ADAL) ermöglicht Entwickler der Clientanwendung zur problemlos in die cloud Benutzerauthentifizierung oder lokalen Active Directory (AD) und dann Access Token zum Sichern des API-Aufrufe anfordern. ADAL wartet mit zahlreichen Funktionen vergewissern, dass die Authentifizierung einfacher für Entwickler, wie z. B. asynchrone Unterstützung, eine konfigurierbare token Cache, in dem Access-Token und aktualisieren Token, automatische token aktualisieren, wenn eine Access-Sicherheitstoken abläuft und ein Token aktualisieren steht, gespeichert und vieles mehr. Behandelt die meisten der Komplexität, kann ADAL Entwicklertools Schwerpunkt auf Geschäftslogik in ihrer Anwendung helfen und Sichern von Ressourcen einfach ohne Experte für die Sicherheit.

ADAL steht auf einer Vielzahl von Plattformen.

|Plattform|Name des Pakets|Client-/Server|Herunterladen|Quellcode|Dokumentation und Beispiele|
|---|---|---|---|---|---|
|.NET Client, Windows Store, UWP, Xamarin iOS und Android|Active Directory-Authentifizierungsbibliothek (ADAL) für .NET v3 |Client|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory)|[ADAL für .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet)|[Dokumentation](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory)|
|.NET-Windows Store-Client für Windows Phone 8.1 |Active Directory-Authentifizierungsbibliothek (ADAL) für .NET Version 2 |Client|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.2)|[ADAL für .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.2)|[Dokumentation](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory)|
|JavaScript|Active Directory-Authentifizierungsbibliothek (ADAL) für JavaScript|Client|[ADAL für JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|[ADAL für JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|Beispiel: [SinglePageApp-DotNet (Github)](https://github.com/AzureADSamples/SinglePageApp-DotNet)|
|IOS OS X|Active Directory-Authentifizierungsbibliothek (ADAL) zum Ziel-C|Client|[ADAL für die Ziel-C (CocoaPods)](http://cocoadocs.org/docsets/ADAL/)|[ADAL für die Ziel-C (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-objc)|Beispiel: [NativeClient-iOS (Github)](https://github.com/AzureADSamples/NativeClient-iOS)|
|Android|Active Directory-Authentifizierungsbibliothek (ADAL) für Android|Client|[ADAL für Android (zentralen Repository)](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/)|[ADAL für Android (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-android)|Beispiel: [NativeClient kein Android (Github)](https://github.com/AzureADSamples/NativeClient-Android)|
|Node.js|Active Directory-Authentifizierungsbibliothek (ADAL) für Node.js|Client|[ADAL für Node.js (Npm)](https://www.npmjs.com/package/adal-node)|[ADAL für Node.js (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)|Beispiel: [WebAPI-Nodejs (Github)](https://github.com/AzureADSamples/WebAPI-Nodejs)|
|Node.js|Microsoft Azure Active Directory Passport Authentifizierung Middleware für Knoten|Client|[Azure Active Directory Passport für Node.js (Npm)](https://www.npmjs.com/package/passport-azure-ad)|[Azure-Active Directory für Node.js (Github)](https://github.com/AzureAD/passport-azure-ad)||
|Java|Active Directory-Authentifizierungsbibliothek (ADAL) für Java|Client|[ADAL für Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)|[ADAL für Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)||
|.NET|Identität Protocol Extensions für Microsoft .NET Framework 4.5|Server|[Microsoft.IdentityModel.Protocol.Extensions (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions)|[Azure AD-Identität Modell Erweiterungen für .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|JSON Web Token Ereignishandler für Microsoft .net Framework 4.5|Server|[System.IdentityModel.Tokens.Jwt (NuGet)](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt)|[Azure AD-Identität Modell Erweiterungen für .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|OWIN Middleware, die eine Anwendung Microsoft Technologie zur Authentifizierung verwendet werden kann.|Server|[Microsoft.Owin.Security.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)||
|.NET|OWIN Middleware, die eine Anwendung OpenIDConnect zur Authentifizierung verwendet werden kann.|Server|[Microsoft.Owin.Security.OpenIdConnect (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Beispiel: [WebApp OpenIDConnecty DotNet (Github)](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet)|
|.NET|OWIN Middleware, die eine Anwendung WS-Verbund zur Authentifizierung verwendet werden kann.|Server|[Microsoft.Owin.Security.WsFederation (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Beispiel: [WebApp WSFederation DotNet (Github)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet)|

## <a name="scenarios"></a>Szenarien

Hier sind drei häufige Szenarien, in denen ADAL für die Authentifizierung verwendet werden kann.  

### <a name="authenticating-users-of-a-client-application-to-a-remote-resource"></a>Authentifizieren von Benutzern von einer Clientanwendung Remote einer Ressourcenansicht hinzu

In diesem Szenario ist ein Entwickler einen Client, wie etwa einer WPF-Anwendung, die Zugriff auf eine remote Ressource durch Azure AD, beispielsweise ein Web-API gesicherte muss. Er enthält ein Azure-Abonnement, weiß, wie die untergeordneten Web-API aufzurufen und kennt den Azure AD-Mandanten, den im Web-API verwendet. ADAL können er daher um Authentifizierung mit Azure AD, indem vollständig Delegieren der Authentifizierung-Oberfläche auf ADAL oder explizit Behandlung Benutzeranmeldeinformationen zu erleichtern. ADAL macht fordert sie einfach zum Authentifizieren des Benutzers, erhalten eine Access-Token und aktualisieren Token aus Azure Active Directory und verwenden Sie dann das Zugriffstoken vornehmen im Web-API.

Ein Codebeispiel, dieses Szenario mithilfe der Azure AD-Authentifizierung veranschaulicht, finden Sie unter [Native Client WPF-Anwendung zu Web-API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-server-application-to-a-remote-resource"></a>Eine Server-Anwendung, die eine Ressource Remote Authentifizierung

In diesem Szenario weist ein Entwickler eine Anwendung auf einem Server, der Zugriff auf eine remote Ressource durch Azure AD, beispielsweise ein Web-API gesicherte muss ausgeführt. Er enthält ein Azure-Abonnement, weiß, wie den untergeordneten Dienst aufrufen und weiß, dass der Azure AD-Mandanten die Web-API verwendet wird. Daher kann er ADAL verwenden, um Authentifizierung mit Azure AD zu vereinfachen, indem Sie explizit Behandeln der Anwendung Anmeldeinformationen. ADAL macht fordert sie einfach ein Token aus Azure Active Directory mithilfe der Anwendung Client-Anmeldeinformationen abrufen und verwenden Sie dann die Token vornehmen im Web-API. ADAL übernimmt auch Verwalten der Lebensdauer der Access-Token durch zwischenzuspeichern, um ihn und erneuern sie nach Bedarf. Ein Codebeispiel, dieses Szenario veranschaulicht, finden Sie unter [Console-Anwendung Web-API](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-server-application-on-behalf-of-a-user-to-access-a-remote-resource"></a>Eine Server-Anwendung im Auftrag eines Benutzers Zugriff auf eine Ressource Remote Authentifizierung

In diesem Szenario weist ein Entwickler eine Anwendung auf einem Server, der Zugriff auf eine remote Ressource durch Azure AD, beispielsweise ein Web-API gesicherte muss ausgeführt. Die Anforderung auch im Namen eines Benutzers in Azure Active Directory erstellt werden soll. Er ein Azure-Abonnement, weiß, wie das untergeordnete Web API aufzurufen und kennt das Azure AD tenant der Dienst verwendet. Sobald der Benutzer mit der Webanwendung authentifiziert wurde, kann die Anwendung einen Autorisierungscode für den Benutzer aus Azure AD abrufen. Die Webanwendung können Sie ADAL einer Access-Token abrufen und Token im Namen eines Benutzers mit den Autorisierung Code und Client Anmeldeinformationen der Anwendung von Azure AD zugeordneten aktualisieren. Nachdem die Anwendung im Besitz eines Access-Token befindet, kann es im Web-API aufrufen, das Token abgelaufen ist. Wenn die Sicherheitstoken abläuft, können die Webanwendung ADAL, um ein neues Access Token abrufen, indem die Aktualisierung token zuvor empfangen wurde.


## <a name="see-also"></a>Siehe auch

[Azure Active Directory developer's guide](active-directory-developers-guide.md)

[Authentifizierungsszenarien Azure-Active directory](active-directory-authentication-scenarios.md)

[Azure Active Directory-Codebeispielen](active-directory-code-samples.md)
