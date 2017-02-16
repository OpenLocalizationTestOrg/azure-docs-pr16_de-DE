<properties
    pageTitle="Typen von den Endpunkt Version 2.0 | Microsoft Azure"
    description="Die Typen von apps und Szenarien, die von den Azure AD-Version 2.0-Endpunkt unterstützt werden."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="types-of-apps-for-the-v20-endpoint"></a>Arten von apps für den Endpunkt Version 2.0
Der Endpunkt Version 2.0 unterstützt Authentifizierung für eine Vielzahl von moderne app Architekturen, die auf die Protokolle nach Industriestandard [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) und/oder [OpenID verbinden](active-directory-v2-protocols.md#openid-connect-sign-in-flow)basieren.  Dieses Dokument kurz die Arten von apps beschrieben, die Sie erstellen können, empfiehlt sich die unabhängig von der Sprache oder Plattform.  Es hilft Ihnen die Szenarien auf hoher Ebenen, bevor Sie Sie [direkt in den Code](active-directory-appmodel-v2-overview.md#getting-started)zu verstehen.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory-Szenarien und Features werden von den Endpunkt Version 2.0 unterstützt.  Um festzustellen, ob den Version 2.0-Endpunkt verwendet werden sollen, erfahren Sie, [Version 2.0 Einschränkungen](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Die Grundlagen
Jeder app, die den Endpunkt Version 2.0 verwendet wird in [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)registriert werden müssen.  Die app Registrierung zu wird sammeln und Ihre app ein paar Werte zuzuweisen:

- Eine **Id der Anwendung** , die Ihre app eindeutig
- Ein **URI umleiten** , die verwendet werden kann, um Antworten zu Ihrer Anwendung wieder zu leiten.
- Ein paar andere Szenario-spezifische Werte.  Erfahren Sie mehr Details zum [Registrieren einer app](active-directory-v2-app-registration.md).

Nach der Registrierung kommuniziert Senden von Anfragen an den Azure-Active Directory-Version 2.0-Endpunkt die app mit Azure AD.  Wir erläutern die notwendigen open-Source-Framework und Bibliotheken, die die Details der diese Anfragen erledigen, oder Sie können Implementieren der Authentifizierungslogik selbst durch Anfragen an diese Endpunkte erstellen:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a>Web apps
Für Web apps (.NET, PHP, Java, Ruby, Python, Knoten usw.), die über einen Browser zugegriffen werden, können Sie Benutzer ausführen Anmelden mit [OpenID verbinden](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  In OpenID Verbinden des Web app empfängt eine `id_token`, ein Sicherheitstoken, das die Identität des Benutzers überprüft und stellt Informationen über den Benutzer in Form von Ansprüchen:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Sie können alle Typen von Token und Ansprüche verfügbar mit einer [Version 2.0 token Bezug](active-directory-v2-tokens.md)kennen.

Im Web apps Server nimmt Anmeldung Authentifizierung illustrieren Schritte auf hoher Ebene:

![Web App Verantwortlichkeitsbereiche Bild](../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

Die Überprüfung der Id_token mit einem öffentlichen signierenden Schlüssel erhalten von den Endpunkt Version 2.0 ist ausreichend, vergewissern Sie sich die Identität des Benutzers, und legen Sie eine Sitzungscookie, die zum Identifizieren des Benutzers bei nachfolgenden Seitenanfragen verwendet werden kann.

Um dieses Szenario in Aktion sehen zu können, testen Sie eine Web app anmelden Codebeispielen unseren Bereich [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started) .

Zusätzlich zu einfachen anmelden müssen einen Server Online auch einige andere Webdienst z. B. ein REST-API zugreifen.  In diesem Fall kann die Server online eine kombinierte OpenID verbinden und OAuth 2.0 Fluss, mit [OAuth 2.0 Autorisierungscode Fluss](active-directory-v2-protocols.md#oauth2-authorization-code-flow)populärer. Dieses Szenario wird nachfolgend in unseren [Thema WebApp-WebAPI erste Schritte](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)erläutert.

## <a name="web-apis"></a>Web-APIs
Den Endpunkt Version 2.0 können Webdienste auch, z. B. Ihrer app Rest Web-API gesichert.  Statt Id_tokens und Sitzung Cookies verwenden Web-APIs OAuth 2.0 Access_tokens sichern Sie ihre Daten und authentifizieren eingehende Anfragen an.  Anrufer einer Web-API stellt eine Access_token in der Kopfzeile Autorisierung einer HTTP-Anforderung an:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Die Web-API können Sie die Access_token zum Überprüfen der Identität des Anrufers API und Extrahieren von Informationen zum Anrufer aus Ansprüche, die in der Access_token codiert werden.  Sie können alle Typen von Token und Ansprüche verfügbar mit einer [Version 2.0 token Bezug](active-directory-v2-tokens.md)kennen.

Eine Web-API können die Benutzer die Möglichkeit, Suchbegriffen-in/Suchbegriffen ausgehend von bestimmter Funktionen oder Daten verleihen, indem Sie Berechtigungen bekannt als [Bereiche](active-directory-v2-scopes.md)verfügbar zu machen.  Für eine einen app, über die Berechtigung zum Bereich zu erhalten muss der Benutzer auf den Bereich während einer Fluss einverstanden.  Der Endpunkt Version 2.0 wird den Benutzer um Erlaubnis zu Fragen, und übernehmen Aufzeichnung der jeweiligen Berechtigungen in alle Access_tokens, die die Web-API empfängt.  Sämtliche kümmern muss die Web-API ist überprüfen die Access_tokens, die sie auf jede empfängt und entsprechende Autorisierung Prüfungen ausführen.

Eine Web-API können Access_tokens aus allen Arten von apps, einschließlich Web Server apps, Desktop- und mobile-apps, Einzelseite apps, Seite Dämonprozesse und sogar anderen Web-APIs erhalten.  Für die Web-api Authentifizierung auf hoher Ebene illustrieren sieht wie folgt aus:

![Webbild API Verantwortlichkeitsbereichen](../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

Weitere Informationen zu Authorization_codes, Refresh_tokens und die detaillierten Schritte der erste Access_tokens zu finden, erfahren Sie das [OAuth 2.0-Protokoll](active-directory-v2-protocols-oauth-code.md)aus.

Erfahren, wie eine Web gesichert-api mit OAuth2 Access_tokens, schauen Sie sich die Web-api Codebeispielen unseren [Bereich Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started).


## <a name="mobile-and-native-apps"></a>Mobile und systemeigenen apps
Apps, die auf einem Gerät, z. B. mobile als auch desktop-apps installiert werden müssen häufig Back-End-Services oder Web-APIs, die Daten zu speichern, und führen Sie verschiedene Funktionen im Auftrag eines Benutzers zugreifen.  Diese apps können Anmeldung und Autorisierung Back-End-Dienste verwenden [OAuth 2.0 Autorisierungscode Fluss](active-directory-v2-protocols-oauth-code.md)hinzufügen.  

In diesem Fluss eine die app erhält eine Authorization_code von den Endpunkt Version 2.0 bei der Anmeldung Benutzern, die des app Berechtigung zum Aufrufen von Back-End-Dienste im Namen des Benutzers momentan angemeldeten darstellt.  Die app kann die Authoriztion_code in den Hintergrund für eine OAuth 2.0 Access_token und eine Refresh_token austauschen.  Die app können Sie die Access_token zur Web-APIs in HTTP-Anfragen Authentifizierung, und kann die Refresh_token verwenden, können Sie um neue Access_tokens zu gelangen, wenn ältere abläuft.

![Systemeigene App Verantwortlichkeitsbereiche Bild](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Einzelseite apps (Javascript)
Viele moderne apps verfügen über eine (gesicherte KENNWORTAUTHENTIFIZIERUNG einzelne Seite App) Front-End-geschrieben hauptsächlich in Javascript und häufig verwenden Framework wie AngularJS, Ember.js, Durandal.  Der Azure AD-Version 2.0-Endpunkt unterstützt diese mithilfe der [OAuth 2.0 implizit Datenfluss](active-directory-v2-protocols-implicit.md)apps.

In diesem Fluss, erhält die app Token aus der Version 2.0 Endpunkt direkt, ohne die Durchführung einer beliebigen Back-End-Server zu Server Austausch autorisieren.  Dadurch, dass alle Authentifizierungslogik und Sitzung Behandlung ausführen vollständig im Javascript-Client, ohne zusätzliche Seite leitet zur Verfügung.

![Implizite Fluss Verantwortlichkeitsbereiche Bild](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

Um dieses Szenario in Aktion sehen, probieren Sie eine einzelne Seite app Codebeispielen unseren Bereich [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started) .

### <a name="daemonsserver-side-apps"></a>Die Seite apps Daemons/server
Apps, die lange laufenden Prozesse enthalten oder bearbeitet werden ohne die Anwesenheitsinformationen eines Benutzers, benötigen ferner eine Möglichkeit zum gesicherte Ressourcen, wie z. B. Web-APIs zugreifen.  Diese apps können authentifizieren und Token mithilfe des app Identität (anstelle der delegierten Benutzeridentität) mithilfe des OAuth 2.0-Clients-Anmeldeinformationen Fluss abrufen.

In diesem Fluss, erhält die app Token durch direktes interagieren mit den `/token` Endpunkt:

![Daemon App Verantwortlichkeitsbereiche Bild](../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

Zum Erstellen einer app Daemon, finden Sie unter der Client Anmeldeinformationen Documeenation unsere [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started) im Abschnitt oder finden Sie in [diesem Beispiel .NET-app](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).

## <a name="current-limitations"></a>Aktuelle Einschränkungen
Diese Arten von apps werden derzeit nicht durch die Version 2.0-Endpunkt unterstützt, aber auf die Roadmap sind.  Zusätzliche Einschränkungen und Einschränkungen für den Endpunkt Version 2.0 werden in der [Version 2.0 Einschränkungen Artikel](active-directory-v2-limitations.md)beschrieben.

### <a name="chained-web-apis-on-behalf-of"></a>Verkettete Web APIs (im Auftrag von)
Viele Architekturen enthalten eine Web-API, die eine andere untergeordneten Web-API, beide durch den Endpunkt Version 2.0 gesicherte anrufen muss.  Dieses Szenario wird häufig in einer systemeigenen-Clients, die ein Web-API Back-End-aufweisen, wodurch wiederum einen Microsoft Online-Dienst, wie Office 365 oder in der Graph-API aufgerufen.

Dieses Szenario der verketteten Web-API kann mithilfe der OAuth 2.0 Jwt Person Anmeldeinformationen erteilen, bekannt als den [Datenfluss On-Behalf-Of](active-directory-v2-protocols.md#oauth2-on-behalf-of-flow)unterstützt werden.  In den Endpunkt Version 2.0 ist auf Auftrag von illustrieren jedoch zurzeit nicht implementiert.  Zu sehen, wie diese Fluss in allgemein verfügbar Azure AD funktioniert service, schauen Sie sich das [Codebeispiel im Auftrag von, klicken Sie auf GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).
