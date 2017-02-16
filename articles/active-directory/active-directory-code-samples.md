<properties
   pageTitle="Azure-Active Directory-Codebeispielen | Microsoft Azure"
   description="Ein Index des Azure Active Directory-Codebeispielen, Szenario geordnet."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="priyamohanram"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="azure-active-directory-code-samples"></a>Azure-Active Directory-Codebeispielen

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Microsoft Azure-Active Directory (Azure AD) können Ihre Webanwendungen und Web-APIs Authentifizierung und Autorisierung hinzufügen. In diesem Abschnitt finden Sie links zu Codebeispielen, die Ihnen zeigen, wie dies funktioniert und Codeausschnitte, die Sie in Ihrer Anwendung verwenden können. Auf der Seite Code Stichprobe finden Sie ausführliche gelesen-mich Hilfethemen, die mit Anforderungen, Installation und Einrichtung. Und der Code wird kommentiert, um die kritische Abschnitte helfen können.

Um das grundlegende Szenario für jedes Sample-Typ zu verstehen, finden Sie unter Authentifizierungsszenarien Azure AD.

Eigene Notizen hinzufügen können unsere Beispiele auf GitHub: [Microsoft Azure-Beispielen für Active Directory und Dokumentation](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-to-web-application"></a>Webbrowser zu Webanwendung

Diesen Beispielen wird so schreiben Sie eine Webanwendung, die Browser des Benutzers zu Azure AD anmelden weiterleitet.



| Sprache/Plattform | Beispiel | Beschreibung
| ----------------- | ------ | -----------
| C#-/ .NET | [WebApp-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Verwenden Sie zum Authentifizieren von Benutzern aus einer Azure AD-Mandanten OpenID verbinden (ASP.Net OpenID verbinden OWIN Middleware).
| C#-/ .NET | [WebApp-mehrere-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Einen Mandanten mit mehreren .NET MVC Web-Anwendung, die OpenID verbinden (ASP.Net OpenID verbinden OWIN Middleware) werden zum Authentifizieren von Benutzern aus mehreren Azure AD-Mandanten verwendet.
| C#-/ .NET | [WebApp-WSFederation-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | Verwenden Sie WS-Verbund (ASP.Net WS-Verbund OWIN Middleware) zum Authentifizieren von Benutzern von einem Azure AD-Mandanten.






## <a name="single-page-application-spa"></a>Einzelne Seite Anwendung (SP a)

Dieses Beispiel zeigt, wie Sie eine einzelne Seite Anwendung mit Azure AD gesicherte schreiben.  

| Sprache/Plattform | Beispiel | Beschreibung
| ----------------- | ------ | -----------
| JavaScript, c# / .NET | [SinglePageApp-DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) | Verwenden Sie ADAL für JavaScript und Azure AD-, um eine einzelne Seite AngularJS-basierten app implementiert mit einem ASP.NET Web API Back-End sichern.


## <a name="native-application-to-web-api"></a>Systemeigene Anwendung Web-API

In diesen Codebeispielen zeigen, wie systemeigene Clientanwendungen erstellen, die Web-APIs aufrufen, der durch Azure AD gesichert werden. Sie verwenden [Azure AD Authentifizierung Bibliothek (ADAL)](active-directory-authentication-libraries.md) und [OAuth 2.0 in Azure Active Directory](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Sprache/Plattform | Beispiel | Beschreibung
| ----------------- | ------ | -----------
| JavaScript | [NativeClient-MultiTarget-Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) | Verwenden Sie die ADAL-Plug-In für Apache Cordova Apache Cordova app erstellen, die eine Web-API ruft und Azure AD verwendet, für die Authentifizierung ein.
| C#-/ .NET | [NativeClient-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) | Mithilfe von Azure AD ist eine .NET WPF-Anwendung, die eine Web-API-Anrufe gesichert.
| C#-/ .NET | [NativeClient-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Eine Windows Store-Anwendung, die eine Web-API-Anrufe wird mit Azure AD gesichert.
| C#-/ .NET | [NativeClient-WebAPI-mehrere-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) | Eine Windows Store-Anwendung, die eine Web mit mehreren Mandanten mit Azure AD sitzt API aufrufen.
| C#-/ .NET | [WebAPI-OnBehalfOf-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) | Eine native Clientanwendung, die eine Web-API die Ruft ein Token Ruft für die im Auftrag des ursprünglichen Benutzers und dann das Token aufrufen, ein anderes Web API verwendet.
| C#-/ .NET | [NativeClient-WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) | Eine Windows Store-Anwendung für Windows Phone 8.1, die eine Web-API-Anrufe wird durch Azure AD gesichert.
| ObjC | [IOS-NativeClient](https://github.com/Azure-Samples/active-directory-ios) | Eine iOS-Anwendung, die eine Web-API-Anrufe erfordert Azure AD für die Authentifizierung ein.
| C#-/ .NET | [WebAPI-ManuallyValidateJwt-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) | Eine native Clientanwendung, die Logik zum Verarbeiten von ein JWT Token in einem Web-API, anstelle von OWIN Middleware enthält.
| C#-/ Xamarin | [NativeClient-Xamarin-Android](https://github.com/Azure-Samples/active-directory-xamarin-android) | Eine Xamarin Bindung in die native Azure AD Authentifizierung Bibliothek (ADAL) für die Bibliothek Android.
| C#-/ Xamarin | [NativeClient-Xamarin-iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) | Eine Xamarin Bindung in die native Azure AD Authentifizierung Bibliothek (ADAL) für iOS.
| C#-/ Xamarin | [NativeClient-MultiTarget-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) | Ein Xamarin-Projekt, das Ziel hat fünf Plattformen und eine Web-API-Anrufe, die wird durch Azure AD gesichert.
| C#-/ .NET | [NativeClient Monitor DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) | Systemeigene Anwendung, die nicht interaktiven Authentifizierung durchführt und eine Web-API-Anrufe, die wird durch Azure AD gesichert.



## <a name="web-application-to-web-api"></a>Webanwendung Web-API

Diese Codebeispielen anzeigen wie [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) verwenden, um Webanwendungen zu erstellen, die anrufen web-APIs, die durch Azure AD gesichert werden.

| Sprache/Plattform | Beispiel | Beschreibung
| ----------------- | ------ | -----------
| C#-/ .NET | [WebApp-WebAPI-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) | Rufen Sie eine Web-API mit Berechtigungen des angemeldeten Benutzers an.
|  C#-/ .NET | [WebApp-WebAPI-OAuth2-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) | Rufen Sie eine Web-API mit der Anwendung von Berechtigungen.
| C#-/ .NET | [WebApp-WebAPI-OAuth2-Benutzeridentität-Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | Hinzufügen von Autorisierung mit [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) zu einer vorhandenen Anwendung, damit eine Web-API aufgerufen werden können.
| JavaScript | [WebAPI-Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) | Richten Sie ein REST-API-Dienst, der in Azure AD für API Schutz integriert ist. Enthält einen Node.js-Server mit einem Web-API.
| C#-/ .NET | [WebApp-WebAPI-mehrere-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |  Ein mit mehreren Mandanten MVC web-Anwendung, die OpenID verbinden (ASP.Net OpenID verbinden OWIN Middleware) werden zum Authentifizieren von Benutzern aus einem Azure AD-Mandanten verwendet. Einen Autorisierungscode verwendet zum Aufrufen der Graph-API.

## <a name="server-or-daemon-application-to-web-api"></a>Server- oder Daemon Anwendung Web-API

So erstellen Sie eine Daemon oder Server-Anwendung, die die Ressourcen aus einem Web-API bezieht, mithilfe von [Azure AD Authentifizierung Bibliothek (ADAL)](active-directory-authentication-libraries.md) und [OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx)anzeigen diese Codebeispielen

| Sprache/Plattform | Beispiel | Beschreibung
| ----------------- | ------ | -----------
| C#-/ .NET | [Daemon-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) | Eine Console-Anwendung ruft eine Web-API. Die Client-Anmeldeinformationen ist ein Kennwort an.
| C#-/ .NET | [Daemon-CertificateCredential-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) | Eine Console-Anwendung, die eine Web-API-Anrufe. Die Client-Anmeldeinformationen ist ein Zertifikat.


## <a name="calling-azure-ad-graph-api"></a>Azure AD-Graph-API aufrufen

In diesen Beispielen Code zeigen, wie Applikationen zu erstellen, die die Azure AD Graph-API zum Lesen und Schreiben von Daten Directory aufrufen.

| Sprache/Plattform | Beispiel | Beschreibung
| ----------------- | ------ | -----------
| Java | [WebApp-GraphAPI-Java](https://github.com/Azure-Samples/active-directory-java-graphapi-web) | Eine Anwendung, die das Graph-API verwendet, um Azure AD-Directory Daten zugreifen.
| PHP | [WebApp-GraphAPI-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) | Eine Anwendung, die das Graph-API verwendet, um Azure AD-Directory Daten zugreifen.
| C#-/ .NET | [WebApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Eine Anwendung, die das Graph-API verwendet, um Azure AD-Directory Daten zugreifen.
| C#-/ .NET | [ConsoleApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-console) | Diese app Console allgemeine Lese- und Schreibberechtigungen Anrufe an die Graph-API veranschaulicht und gezeigt, wie Benutzer Lizenzen zugewiesen ausführen und Aktualisieren eines Benutzers Miniaturansicht des Fotos und Links.
| C#-/ .NET | [ConsoleApp-GraphAPI-DiffQuery-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) | Eine Console-Anwendung, die die Differenz Abfrage in der Graph-API verwendet werden, um die periodische Änderungen Benutzerobjekte in einem Azure AD-Mandanten zu erhalten.
| C#-/ .NET | [WebApp-GraphAPI-DirectoryExtensions-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) | Eine MVC-Anwendung verwendet Graph-API Abfragen, um eine einfache Unternehmen Organigramm generieren.
| PHP | [WebApp-GraphAPI-DirectoryExtensions-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) | Eine Anwendung von PHP, die das Diagramm API, um eine Erweiterung zu registrieren, und klicken Sie dann lesen, aktualisieren und Löschen von Werten in das Erweiterungsattribut.


## <a name="authorization"></a>Autorisierung

In diesen Codebeispielen zeigen, wie Azure AD für die Autorisierung verwenden.

| Sprache/Plattform | Beispiel | Beschreibung
| ----------------- | ------ | -----------
| C#-/ .NET | [WebApp-GroupClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Führen Sie rollenbasierte Access Steuerelement (RBAC) Verwenden von Azure Active Directory-Gruppenansprüche in eine Anwendung, die in Azure AD integriert ist.
| C#-/ .NET | [WebApp-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Führen Sie rollenbasierte Access Steuerelement (RBAC) Verwenden von Azure Active Directory-Anwendungsrollen in eine Anwendung, die mit Azure AD integriert ist.


## <a name="legacy-walkthroughs"></a>Legacy Exemplarische Vorgehensweisen

Diese Vorgehensweisen etwas älteren Technologie verwenden, aber weiterhin von Interesse sein könnten.

| Sprache/Plattform | Beispiel | Beschreibung
| ----------------- | ------ | -----------
| C#-/ .NET | [Rollenbasierte und ACL-basierten Autorisierung in einer Microsoft Azure AD-Anwendung](http://go.microsoft.com/fwlink/?LinkId=331694) | Führen Sie rollenbasierte Autorisierung (RBAC) und ACL-basierten Autorisierung in einer Anwendung, die mit Azure AD integriert ist.
| C#-/ .NET |  [AAL - Windows Store-app REST-Dienst - Authentifizierung](http://go.microsoft.com/fwlink/?LinkId=330605) |  Verwenden Sie [Azure AD Authentifizierung Bibliothek (ADAL)](active-directory-authentication-libraries.md) (vormals AAL) für die Betaversion von Windows Store, um Authentifizierung Benutzerfunktionen Windows Store-app hinzufügen.
| C#-/ .NET | [ADAL - REST-Dienst systemeigenen App - Authentifizierung mit AAD über Browser-Dialogfeld](http://go.microsoft.com/fwlink/?LinkId=259814) |  Verwenden Sie einen WPF-Client-Authentifizierung Benutzerfunktionen hinzuzufügende [Azure AD Authentifizierung Bibliothek (ADAL)](active-directory-authentication-libraries.md) ein.
| C#-/ .NET | [ADAL - REST-Dienst systemeigenen App - Authentifizierung mit ACS über Browser-Dialogfeld](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |  Verwenden von [Azure AD Authentifizierung Bibliothek (ADAL)](active-directory-authentication-libraries.md) und [Access Control Service 2.0 (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) so einen WPF-Client-Authentifizierung Benutzerfunktionen hinzu.
| C#-/ .NET | [ADAL - Server-zu-Server-Authentifizierung](http://go.microsoft.com/fwlink/?LinkId=259816) | Verwenden Sie [Azure AD Authentifizierung Bibliothek (ADAL)](active-directory-authentication-libraries.md) zum Dienst Anrufe aus einer serverseitige Prozess an einer MVC4 Web-API REST-Dienst zu sichern.
| C#-/ .NET | [Melden Sie sich auf der Web-Anwendung, die mit Azure AD hinzufügen](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Konfigurieren einer Anwendungs .NET Web einmaliges Anmelden anhand Ihrer Azure AD-Unternehmensverzeichnis ausführen.
| C#-/ .NET | [Entwickeln mit mehreren Mandanten Webanwendungen mit Azure AD-](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Verwenden Sie Azure AD, um die einmaliges Anmelden und Directory Access-Funktionen, die eine .NET Anwendung entwickelt in mehreren Organisationen hinzuzufügen.
JAVA | [Java-Beispiel-App für Azure AD-Diagramm-API](http://go.microsoft.com/fwlink/?LinkId=263969) | Verwenden Sie die Graph-API mit Access Directory Daten aus Azure Active Directory.
PHP | [Beispiel-App für Azure AD-Diagramm-API PHP](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) | Verwenden Sie die Graph-API mit Access Directory Daten aus Azure Active Directory.
| C#-/ .NET | [Beispiel-App für Azure AD-Diagramm-API](http://go.microsoft.com/fwlink/?LinkID=262648) | Verwenden Sie die Graph-API mit Access Directory Daten aus Azure Active Directory.
| C#-/ .NET | [Beispiel-App für Differenz Abfrage Azure AD-Diagramm](http://go.microsoft.com/fwlink/?LinkId=275398) | Verwenden Sie die Differenz Abfrage in der Graph-API abzurufenden periodische Änderungen an Benutzerobjekte in einem Azure AD-Mandanten.
| C#-/ .NET | [Beispiel-App für die Integration mit mehreren Mandanten Cloudanwendung für Azure AD-](http://go.microsoft.com/fwlink/?LinkId=275397) | Integrieren Sie eine Anwendung mit mehreren Mandanten in Azure AD.
| C#-/ .NET | [Schützen eines Windows Store-Anwendung und Azure AD-REST-Webdienst](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Erstellen einer einfachen API Webressource und eine Windows Store-Clientanwendung Azure AD- und der [Azure AD Authentifizierung Bibliothek (ADAL)](active-directory-authentication-libraries.md).
| C#-/ .NET| [Verwenden das Diagramm API Azure AD-Abfragen](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Konfigurieren einer Microsoft .NET-Anwendungs, für die Azure AD Graph-API verwenden, um Daten aus einem Azure AD-Mandanten Verzeichnis zugreifen.

## <a name="see-also"></a>Siehe auch

##### <a name="other-resources"></a>Weitere Ressourcen

[Azure-Active Directory Developer's Guide](active-directory-developers-guide.md)

[Azure AD-Graph-API konzeptionelle und verweisen](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD-Graph-API Helper-Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)

