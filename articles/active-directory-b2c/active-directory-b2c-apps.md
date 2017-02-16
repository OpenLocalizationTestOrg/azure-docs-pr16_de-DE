<properties
    pageTitle="Azure AD B2C | Microsoft Azure"
    description="Die Typen von Applications können Sie in der Azure-Active Directory B2C erstellen."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-types-of-applications"></a>Azure Active Directory B2C: Typen von applications

Azure Active Directory (Azure AD) B2C unterstützt Authentifizierung für eine Vielzahl von moderne app Architekturen. Alle basieren auf die Protokolle nach Industriestandard [OAuth 2.0](active-directory-b2c-reference-protocols.md) oder [OpenID verbinden](active-directory-b2c-reference-protocols.md). Dieses Dokument kurz die Arten von apps beschrieben, die Sie erstellen können, empfiehlt sich die unabhängig von der Sprache oder Plattform. Außerdem können Sie die Szenarien auf hoher Ebene, bevor Sie [beginnen, Erstellen von Applications](active-directory-b2c-overview.md#getting-started)zu verstehen.

## <a name="the-basics"></a>Die Grundlagen
Jeder app, die Azure AD B2C verwendet wird, muss in Ihrem [Verzeichnis B2C](active-directory-b2c-get-started.md) über das [Azure-Portal](https://portal.azure.com/)registriert sein. Die app Registrierung gesammelt, und Ihre app ein paar Werte zugewiesen:

- Eine **ID der Anwendung** , die Ihre app identifiziert.
- Ein **URI umgeleitet** , die verwendet werden können, um wieder zu Ihrer Anwendung Antworten zu leiten.
- Alle anderen Szenario-spezifischen Werten. Weitere Einzelheiten hierzu erfahren Sie, wie Sie [eine app registrieren](active-directory-b2c-app-registration.md).

Nach die app registriert ist, wird mit Azure AD kommuniziert, per Anfragen an den Endpunkt des Azure AD-Version 2.0:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Jede Anforderung, die in Azure AD B2C gesendet wurde, gibt einer **Richtlinie**an. Eine Richtlinie steuert das Verhalten der Azure AD. Diese Grenzwerte können Sie auch um eine anpassbare Reihe von Benutzerfunktionalität zu erstellen. Allgemeine Richtlinien gehören Anmeldung, Anmeldung und Richtlinien Profil bearbeiten. Wenn Sie nicht mit Richtlinien vertraut sind, sollten Sie über Azure AD B2C [extensible Policy Framework](active-directory-b2c-reference-policies.md) lesen, bevor Sie fortfahren.

Die Interaktion mit einem Version 2.0-Endpunkt von jeder app folgt ein ähnliches auf hoher Ebene Muster:

1. Die app weist den Benutzer an den Endpunkt Version 2.0, einer [Richtlinie](active-directory-b2c-reference-policies.md)auszuführen.
2. Der Benutzer schließt die Richtlinie entsprechend der Richtliniendefinition ab.
4. Die app erhält eine Art des Sicherheitstokens von den Endpunkt Version 2.0.
5. Die app verwendet das Sicherheitstoken auf geschützte Informationen oder eine geschützte Ressource zugreifen.
6. Der Ressourcenserver überprüft, ob das Sicherheitstoken, um sicherzustellen, dass der Zugriff gewährt werden kann.
7. Die app wird regelmäßig das Sicherheitstoken aktualisiert.

<!-- TODO: Need a page for libraries to link to -->
Diese Schritte können variieren etwas basierend auf den Typ der app, die Sie erstellen. Open-Source-Bibliotheken können die Details für Sie zu beheben.

## <a name="web-apps"></a>Web apps
Für Web apps (einschließlich .NET, PHP, Java, Ruby, Python und Node.js), die auf einem Server gehostet werden und Sie über einen Browser, unterstützt Azure AD B2C für alle Benutzerfunktionalität [OpenID verbinden](active-directory-b2c-reference-protocols.md) . Dies umfasst Anmeldung, Anmeldung, und Verwaltung von Profilen. In der Implementierung Azure AD B2C OpenID verbinden initiiert Web app diese Benutzerfunktionalität durch Authentifizierungsanfragen an Azure AD an. Das Ergebnis der Anfrage ist ein `id_token`. Dieses Sicherheitstoken stellt die Identität des Benutzers an. Darüber hinaus Informationen über den Benutzer in Form von Ansprüchen:

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

Weitere Informationen zu den vorstehend beschriebenen Token und Ansprüche verfügbar mit einer der [B2C Token zuzugreifen](active-directory-b2c-reference-tokens.md).

In einer Web app hat jede Ausführung einer [Richtlinie](active-directory-b2c-reference-policies.md) diese allgemeinen Schritte:

![Web App Verantwortlichkeitsbereiche Bild](./media/active-directory-b2c-apps/webapp.png)

Überprüfung von der `id_token` mithilfe ein öffentlicher signierenden Schlüssel, die aus dem Azure Active Directory erhält reicht zum Überprüfen der Identität des Benutzers. Dadurch wird außerdem eine Sitzungscookie, die zum Identifizieren des Benutzers bei nachfolgenden Seitenanfragen verwendet werden kann.

Um dieses Szenario in Aktion sehen zu können, versuchen Sie eine Web app anmelden Codebeispielen unsere [erste Abschnitt Schritte](active-directory-b2c-overview.md#getting-started)aus.

Zusätzlich zu einfachen Anmeldung zu erleichtern, müssen einen Server Online auch einen Back-End-Webdienst zuzugreifen. In diesem Fall kann einen weicht [Fluss OpenID verbinden](active-directory-b2c-reference-oidc.md) die Web app und Token mithilfe von Autorisierungscodes erfassen und Ausführen Token aktualisieren. Dieses Szenario wird im folgenden [Abschnitt Web-APIs](#web-apis)dargestellt.

<!--, and in our [WebApp-WebAPI Getting started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## <a name="web-apis"></a>Web-APIs
Azure AD B2C können Webdienste wie Ihrer app Rest Web API gesichert. Web-APIs können OAuth 2.0 so sichern Sie ihre Daten, Authentifizieren eingehender HTTP-Anfragen Token verwenden. Anrufer mit einem Web-API Fügt ein Token in der Kopfzeile Autorisierung einer HTTP-Anforderung an:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Das Web-API können Sie das Token des Anrufers API Identität zu überprüfen und Informationen zum Anrufer aus Ansprüche extrahieren, die im Token codiert werden. Weitere Informationen zu den vorstehend beschriebenen Token und Ansprüche verfügbar mit einer der [Azure AD B2C Token zuzugreifen](active-directory-b2c-reference-tokens.md).

> [AZURE.NOTE]
    Azure AD B2C unterstützt derzeit nur Web APIs, die durch ihre eigenen bekannten Clients zugegriffen werden. Beispielsweise kann Ihre vollständige app einer app für iOS und Android app einer Back-End-Web-API enthalten. Diese Architektur wird vollständig unterstützt. Partner Client kann, wie etwa einer anderen app für iOS, dasselbe Web zugreifen, die, das API derzeit nicht unterstützt wird. Alle Komponenten der vollständigen app muss eine Anwendung für einmaliges ID. freigeben.

Eine Web-API kann Token aus vielen Arten von Clients, einschließlich Web apps, Desktop- und mobile-apps, Einzelseite apps, serverseitigen Daemons, und andere APIs empfangen werden. Hier ist ein Beispiel für den gesamten Ablauf eines Web-app, die eine Web-API ruft ein:

![Web App API Verantwortlichkeitsbereiche Webbild](./media/active-directory-b2c-apps/webapi.png)

Erfahren Sie mehr Informationen zu Autorisierungscodes, Token aktualisieren und die Schritte zum Abrufen der Token das [OAuth 2.0-Protokoll](active-directory-b2c-reference-oauth-code.md)aus.

So sichern Sie eine Web-API mit Azure AD B2C zu finden, schauen Sie sich im Web API Lernprogrammen unsere [Erste Schritte im Abschnitt](active-directory-b2c-overview.md#getting-started).

## <a name="mobile-and-native-apps"></a>Mobile und systemeigenen apps
Apps, die auf Geräten, wie z. B. mobilen und desktop-apps installiert werden müssen häufig Back-End-Dienste oder Web APIs im Auftrag von Benutzern zugreifen. Sie können Ihre systemeigenen apps und sicheren Anruf Back-End-Dienste mithilfe von Azure AD B2C und [OAuth 2.0 Autorisierung Code Fluss](active-directory-b2c-reference-oauth-code.md)angepasste Identität Management Erfahrung hinzufügen.  

In diesem Fluss, die app [Richtlinien](active-directory-b2c-reference-policies.md) führt und empfängt eine `authorization_code` aus Azure AD-der Benutzer die Richtlinie abgeschlossen hat. Die `authorization_code` steht für die app-Berechtigung zum Aufrufen von Back-End-Dienste im Namen des Benutzers, die aktuell angemeldet ist. Kann die app Austauschen der `authorization_code` im Hintergrund für eine `id_token` und eine `refresh_token`.  Die app können Sie die `id_token` mit einer Back-End-Web-API in HTTP-Anfragen authentifiziert. Können Sie auch die `refresh_token` ein neues abzurufenden `id_token` eine ältere Wann läuft ab.

> [AZURE.NOTE]
    Azure AD B2C unterstützt derzeit nur Token, die verwendet werden, um einen des app-Back-End-Webdienst zugreifen. Beispielsweise kann Ihre vollständige app einer app für iOS und Android app einer Back-End-Web-API enthalten. Diese Architektur wird vollständig unterstützt. Gleicht der iOS-app auf einem Partner Web API mit OAuth 2.0 Access Token wird derzeit nicht unterstützt. Alle Komponenten der vollständigen app muss eine Anwendung für einmaliges ID. freigeben.

![Systemeigene App Verantwortlichkeitsbereiche Bild](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>Aktuelle Einschränkungen
Azure AD B2C unterstützt derzeit nicht die folgenden Arten von apps, aber sie sind auf der roadmapy. Zusätzliche Einschränkungen und Einschränkungen im Zusammenhang mit Azure AD B2C werden in [Einschränkungen und Einschränkungen](active-directory-b2c-limitations.md)beschrieben.

### <a name="single-page-apps-javascript"></a>Einzelseite apps (JavaScript)
Viele moderne apps verfügen über eine einzelne Seite app front-End hauptsächlich in JavaScript geschrieben. Einen Rahmen wie AngularJS, Ember.js oder Durandal für diese häufig verwendet. In der Regel verfügbar Azure AD-Dienst unterstützt diese apps mithilfe von OAuth 2.0 implizit illustrieren. Dieser Fluss ist jedoch nicht noch in Azure AD B2C verfügbar.

### <a name="daemonsserver-side-apps"></a>Daemons/serverseitigen apps
Apps, die lange Prozesse enthalten oder bearbeitet werden ohne die Anwesenheitsinformationen eines Benutzers, benötigen ferner eine Möglichkeit, z. B. Web APIs gesicherte Ressourcen zugreifen. Diese apps können authentifizieren und erste Token mithilfe des app Identität (anstelle der delegierten Benutzeridentität) und unter Verwendung des OAuth 2.0-Clients von Anmeldeinformationen Fluss.

Dieser Verkehr wird durch Azure AD B2C derzeit nicht unterstützt. Diese apps können Token finden Sie erst nach einer interaktiven Benutzer Fluss aufgetreten ist.

### <a name="web-api-chains-on-behalf-of-flow"></a>Web API verkettet (im Auftrag von Fluss)
Viele Architekturen enthalten eine Web-API, die von einem anderen untergeordneten Web-API aufrufen, in dem beide durch Azure AD B2C gesichert muss. Dieses Szenario wird häufig in einer systemeigenen-Clients, die ein Web-API Back-End aufweisen. Dies ruft dann ein Microsoft-Onlinedienst wie die Azure AD Graph-API.

Dieses Webszenario verketteten API kann mithilfe der OAuth 2.0 JWT Person Anmeldeinformationen erteilen, auch bekannt als im Auftrag von illustrieren unterstützt werden.  Im Auftrag von illustrieren ist jedoch nicht derzeit in der Azure AD B2C implementiert.
