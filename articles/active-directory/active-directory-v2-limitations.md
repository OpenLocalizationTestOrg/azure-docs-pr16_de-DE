<properties
    pageTitle="Version 2.0-Endpunkt Einschränkungen und Einschränkungen | Microsoft Azure"
    description="Eine Liste der Einschränkungen und Einschränkungen mit dem Azure AD-Version 2.0-Endpunkt."
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

# <a name="should-i-use-the-v20-endpoint"></a>Sollte ich den Version 2.0-Endpunkt werden verwendet?

Beim Erstellen von Applications, die in Azure Active Directory zu integrieren, müssen Sie entscheiden, ob die Version 2.0-Endpunkt und Authentifizierung Protokolle Ihren Anforderungen entsprechen.  Der ursprüngliche Azure AD-Endpunkt ist immer noch vollständig unterstützt und in Hinsicht Weitere Funktionsumfang als Version 2.0.  Jedoch die Version 2.0 Endpunkt [werden signifikante Vorteile](active-directory-v2-compare.md) für Entwickler, die Ihnen die Verwendung das neue programming Modell verleiten kann.

Zu diesem Zeitpunkt sieht wie folgt aus unserem Empfehlungen bei der Verwendung des Endpunkts Version 2.0:

- Wenn Sie persönliche Microsoft-Konten in der Anwendung unterstützen möchten, sollten Sie den Endpunkt Version 2.0 verwenden.  Aber Vergewissern Sie sich zu verstehen die Einschränkungen in diesem Artikel, insbesondere diejenigen, die speziell zum Arbeiten und Schule Konten betreffen aufgeführt.
- Wenn die Anwendung nur die Arbeit unterstützen & Schule Konten erfordert, sollten Sie [die ursprüngliche Azure AD-Endpunkte](active-directory-developers-guide.md)verwenden.

Im Laufe der Zeit wächst der Version 2.0-Endpunkt um die Einschränkungen, die hier aufgeführten zu entfernen, sodass Sie immer nur den Endpunkt Version 2.0 verwenden müssen.  In der Zwischenzeit in diesem Artikel soll Ihnen helfen festzustellen, der Endpunkt Version 2.0 ist für Sie.  Wir werden weiterhin in diesem Artikel über einen Zeitraum auf dem aktuellen Status des Endpunkts Version 2.0, daher sollten Sie wieder, um Ihren Anforderungen anhand der Version 2.0-Funktionen neu auswerten aktualisieren.

Wenn Sie eine vorhandene app mit Azure AD, die nicht den Endpunkt Version 2.0 verwendet verfügen, gibt es keine völlig neu beginnen müssen.  In der Zukunft werden wir eine Möglichkeit, damit Sie Ihre vorhandenen Azure AD-Applikationen für die Verwendung mit der Version 2.0-Endpunkt aktivieren bereit sein.

## <a name="restrictions-on-apps"></a>Einschränkungen apps
Die folgenden Arten von apps werden durch die Version 2.0-Endpunkt derzeit nicht unterstützt.  Eine Beschreibung der unterstützten Typen von apps finden Sie in [diesem Artikel](active-directory-v2-flows.md).

##### <a name="standalone-web-apis"></a>Eigenständige Web-APIs
Mit den Endpunkt Version 2.0 haben Sie die Möglichkeit, [eine Web-API erstellen, die mithilfe von OAuth 2.0 gesichert wird](active-directory-v2-flows.md#web-apis).  Die Web-API nur werden jedoch Token von einer Anwendung empfangen, das die gleiche Anwendung-ID teilt  Erstellen eine Web-API, die von einem Client mit einer anderen Anwendung Id zugegriffen werden kann, wird nicht unterstützt.  Dieser Client ist nicht möglich anfordern oder Berechtigungen zur Verwaltung der Web-API zu erhalten.

So erstellen Sie eine Web-API, die von einem Client mit der gleichen App-Id Token akzeptiert, finden Sie unter der Version 2.0-Endpunkt Web-API Beispiele in [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started).

##### <a name="web-api-on-behalf-of-flow"></a>Web-API im Auftrag von Fluss
Viele Architekturen enthalten eine Web-API, die eine andere untergeordneten Web-API, beide durch den Endpunkt Version 2.0 gesicherte anrufen muss.  Dieses Szenario wird häufig in einer systemeigenen-Clients, die ein Web-API Back-End-aufweisen, wodurch wiederum aufgerufen eines Microsoft online Service oder eine andere benutzerdefinierte Programme Web-API, die Azure AD unterstützt.

Dieses Szenario kann mithilfe der OAuth 2.0 Jwt Person Anmeldeinformationen erteilen, bekannt als die On-Behalf-Of Datenfluss unterstützt werden.  Für die Version 2.0-Endpunkt wird auf Auftrag von illustrieren jedoch derzeit nicht unterstützt.  Zu sehen, wie diese Fluss in allgemein verfügbar Azure AD funktioniert service, schauen Sie sich das [Codebeispiel im Auftrag von, klicken Sie auf GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).

## <a name="restrictions-on-app-registrations"></a>Einschränkungen app Registrierungen
Zu diesem Zeitpunkt müssen alle apps, die aus den Endpunkt Version 2.0 integriert werden soll, eine neue app-Registrierung am [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)erstellen.  Alle vorhandenen Azure AD oder Microsoft Account apps werden nicht mit der Version 2.0-Endpunkt kompatibel noch auf apps, die in einem beliebigen Portal neben dem das neue App Registrierung Portal registriert wird.  Wir möchten bieten eine Möglichkeit, vorhandene Applikationen für die Verwendung als Version 2.0 app aktivieren. Zu diesem Zeitpunkt durch, ist es keine Migrationspfad für eine app an den Endpunkt Version 2.0.

Ebenso funktioniert in das neue App Registrierung Portal registriert apps nicht gegen den ursprünglichen Azure AD-Authentifizierung Endpunkt.  Sie können jedoch apps im Portal Registrierung App erstellt verwenden, mit dem Microsoft-Konto-Authentifizierung Endpunkt erfolgreich integriert werden soll `https://login.live.com`.

Darüber hinaus stehen die app Registrierungen erstellt am [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) folgenden zu beachten:

- Die Eigenschaft **Homepage** , auch bekannt als die **Anmelden-URL** wird nicht unterstützt.  Ohne eine Homepage werden diese Anwendungen nicht in der Systemsteuerung Office MyApps angezeigt werden.
- Nur zwei app geheimen Informationen, die pro Anwendung Id zu diesem Zeitpunkt zulässig sind.
- Eine app-Registrierung kann nur angezeigt und einem einzelnen Entwicklertools-Konto verwaltet werden.  Es kann nicht zwischen mehreren Entwickler freigegeben werden.
- Es gibt verschiedene Einschränkungen auf das Format der Redirect_uri zulässig.  Im folgenden Abschnitt Weitere Informationen hierzu finden Sie unter.

## <a name="restrictions-on-redirect-uris"></a>Einschränkungen umleiten URIs
Apps, die im neuen Anwendung Registrierung Portal registriert sind, sind derzeit auf eine begrenzte Anzahl von Redirect_uri Werte beschränkt.  Die Redirect_uri für Web apps und Dienste muss mit dem Schema beginnen `https`, und alle Redirect_uri Werte müssen freigeben eine einzelne DNS-Domäne.  Beispielsweise ist es nicht möglich, eine Web app zu registrieren, die Redirect_uris enthält:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

Das Registrierungssystem vergleicht den gesamten DNS-Namen von der vorhandenen Redirect_uri mit der DNS-Name der Redirect_uri, die Sie hinzufügen möchten. Die Anfrage hinzufügen den Namen der DNS-schlägt fehl, wenn einer der folgenden Bedingungen erfüllt sind:  

- Wenn der gesamte DNS-Name der neuen Redirect_uri nicht den DNS-Namen für die vorhandene Redirect_uri übereinstimmt
- Wenn Sie der gesamte DNS-Namen für die neue Redirect_uri nicht die vorhandenen Redirect_uri Unterdomäne

Wenn beispielsweise die Anwendung derzeit Redirect_uri ist:

`https://login.contoso.com`

Klicken Sie dann ist es möglich, hinzuzufügen:

`https://login.contoso.com/new`

die stimmt exakt mit des Namens des DNS- oder:

`https://new.login.contoso.com`

welche DNS-Unterdomäne des login.contoso.com ist.  Wenn Sie eine app haben möchten, bei dem Login-east.contoso.com und Login-west.contoso.com als Redirect_uris, und Sie müssen die folgenden Redirect_uris in Reihenfolge hinzufügen:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Die letzten beiden hinzugefügt werden können, da sie von der ersten Redirect_uri Unterdomänen sind "contoso.com". Diese Einschränkung wird eine bevorstehende Version entfernt.

Um weitere Informationen zum Registrieren einer app im neuen Anwendung Registrierung Portal finden Sie in [diesem Artikel](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services--apis"></a>Klicken Sie auf Dienste und APIs Einschränkungen
Version 2.0-Endpunkt liegt derzeit unterstützt Anmeldung für eine beliebige app registriert die neue Anwendung Registrierung-Portal zur Verfügung gestellt, es in die Liste der [unterstützten Authentifizierung Zahlungen](active-directory-v2-flows.md).  Allerdings werden diese apps nur OAuth 2.0 Access Token für eine sehr begrenzte Anzahl von Ressourcen zu erwerben sein.  Der Endpunkt Version 2.0 wird nur für Access_tokens auszustellen:

- Die app, die das Token angefordert hat.  Eine app kann eine Access_token für sich selbst, erwerben, wenn die logische app aus mehreren unterschiedlichen Komponenten oder Ebenen besteht.  Um dieses Szenario in Aktion sehen zu können, schauen Sie sich unsere [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started) -Lernprogramme.
- Das Outlook E-Mail, Kalender und Kontakte REST-APIs, von denen finden Sie unter https://outlook.office.com werden.  Erfahren Sie, wie Sie eine app schreiben, die diese APIs greift auf, finden Sie in diesen Lernprogrammen [Office erste Schritte](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) .
- Die Microsoft Graph-APIs.  Um Informationen zu den Microsoft Graph und alle Daten, die verfügbar ist, finden Sie auf [https://graph.microsoft.io](https://graph.microsoft.io).

Es werden keine anderen Diensten zu diesem Zeitpunkt unterstützt.  Weitere Microsoft Online Services werden in der Zukunft sowie Support für Ihre eigenen benutzerdefinierten erstellten Web-APIs und Dienste hinzugefügt werden.

## <a name="restrictions-on-libraries--sdks"></a>Einschränkungen Bibliotheken und SDKs
Bibliothek Unterstützung für den Endpunkt Version 2.0 ist ziemlich beschränkt zu diesem Zeitpunkt zu ändern.  Wenn Sie den Version 2.0-Endpunkt in einer Anwendung Herstellung verwenden möchten, müssen Sie die folgenden Optionen aus:

- Wenn Sie eine Webanwendung erstellen, können Sie unsere allgemein verfügbar serverseitigen Middleware sicheres Anmelden ausführen und token Überprüfung verwenden.  Hierzu gehören die OWIN Open-ID verbinden Middleware für ASP.NET und unsere NodeJS Passport-Plug-in.  Mit unseren Middleware Codebeispielen stehen unsere [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started) im Abschnitt ebenfalls zur Verfügung.
- Für andere Plattformen und für systemeigene und mobile Applikationen können Sie auch mit der Version 2.0-Endpunkt integrieren, indem direkt senden und Empfangen von Protokollnachrichten in Ihrer Anwendungscode.  Die Version 2.0 OpenID verbinden und OAuth Protokolle so [explizit dokumentierten wurde](active-directory-v2-protocols.md) , damit Sie solche eine Integration ausführen können.
- Open-Source-ID-verbinden öffnen und OAuth Bibliotheken können Sie schließlich mit den Endpunkt Version 2.0 integriert werden soll.  Das Protokoll Version 2.0 sollte mit vielen open-Source-Protokollbibliotheken ohne die wichtigsten Änderungen kompatibel sein.  Die Verfügbarkeit solcher Bibliotheken variiert pro Sprache und Plattform und die Websites [Öffnen ID verbinden](http://openid.net/connect/) und [OAuth 2.0](http://oauth.net/2/) eine Liste mit beliebten Implementierungen verwalten. Weitere Details und die Liste der open-Source-Client-Bibliotheken und Beispielen, die mit der Version 2.0-Endpunkt getestet wurden, finden Sie unter [Azure Active Directory (AD) Version 2.0 und Authentifizierung Bibliotheken](active-directory-v2-libraries.md) .

Wir haben auch nur Initiale eine Vorschau der [Microsoft-Authentifizierung-Bibliothek (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) für .NET veröffentlicht.  Sie sind Willkommen bei dieser Bibliothek in .NET Client- und Server-Clientanwendungen testen, aber als Vorschau Bibliothek, die es nicht GA auflösendes beigefügt werden unterstützt.

## <a name="restrictions-on-protocols"></a>Einschränkungen Protokolle
Der Endpunkt Version 2.0 unterstützt nur öffnen ID verbinden und OAuth 2.0.  Jedoch nicht alle Features und Funktionen für die verschiedenen Protokolle wurde in den Endpunkt Version 2.0 integriert einschließlich:

- Verbinden der OpenID `end_session_endpoint`, wodurch eine app, um die Sitzung des Benutzers mit dem Version 2.0-Endpunkt zu beenden.
- ausgestellt von den Endpunkt Version 2.0 Id_tokens enthalten nur einen paarweiser Bezeichner für den Benutzer.  Dies bedeutet, dass zwei verschiedene Komponenten unterschiedlichen IDs für den gleichen Benutzer erhalten.  Beachten Sie, dass durch die Microsoft Graph Abfragen `/me` Endpunkt, Sie werden möglicherweise eine correlatable-ID für den Benutzer erhalten, die für Webanwendungen verwendet werden können.
- Id_tokens ausgestellt von den Endpunkt Version 2.0 enthalten keiner `email` beanspruchen für den Benutzer zu diesem Zeitpunkt, auch wenn Erfassen von Erlaubnis des Benutzers, um ihre e-Mail anzuzeigen.
- Der Endpunkt OpenID verbinden Benutzerinformationen. Der Endpunkt Benutzerinformationen ist nicht zu diesem Zeitpunkt für den Endpunkt Version 2.0 implementiert.  Alle Benutzerprofildaten, die Sie potenziell an diesem Endpunkt erhalten möchten ist jedoch verfügbar aus dem Microsoft Graph `/me` Endpunkt.
- Rolle und Gruppenansprüche.  Zu diesem Zeitpunkt unterstützt der Endpunkt Version 2.0 ausstellenden Rolle oder Gruppe Ansprüche in Id_tokens.

Lesen Sie zum besseren Verständnis des Gültigkeitsbereichs von Protokoll-Funktionalität in der Version 2.0-Endpunkt unterstützt durch unsere [OpenID verbinden und OAuth 2.0 Protokoll verweisen](active-directory-v2-protocols.md).

## <a name="restrictions-for-work--school-accounts"></a>Einschränkungen für Schule & arbeiten Konten
Es gibt einige Funktionen speziell für Microsoft-Organisation/Business-Benutzer, die von den Endpunkt Version 2.0 noch nicht unterstützt werden.

##### <a name="device-based-conditional-access-native-and-mobile-apps-and-the-microsoft-graph"></a>Bedingte Access-basierten Gerät, systemeigenen und mobile-apps und der Microsoft Graph
Der Endpunkt Version 2.0 unterstützt noch keine Geräteauthentifizierung für mobile und systemeigenen Anwendungen wie systemeigene apps für iOS oder Android ausgeführt.  Dies könnte einer systemeigenen Anwendung aufrufen, die Microsoft Graph für bestimmte Organisationen blockieren.  Geräteauthentifizierung ist erforderlich, wenn ein Administrator Gerät-basierten bedingte Zugriffsrichtlinie für eine Anwendung fest.  Für den Endpunkt Version 2.0 ist das wahrscheinlichste Szenario für bedingten Zugriff Gerät-basierten Administrator Festlegen einer Richtlinie für eine Ressource in der Microsoft Graph, z. B. der Outlook-API.  Wenn ein Administrator dieser Richtlinie legt und Ihre systemeigene Anwendung, ein Token für die Microsoft Graph anfordert, tritt die Anforderung schließlich, da Geräteauthentifizierung noch nicht unterstützt wird.  Webanwendungen, die Token zu den Microsoft Graph, anfordern werden jedoch unterstützt, wenn Gerät basierende Richtlinien konfiguriert sind.  Im Web erfolgt die app Szenario Geräteauthentifizierung über Webbrowser des Benutzers.

Als Entwickler haben Sie wahrscheinlich keine Kontrolle über, wenn Richtlinien festgelegt sind, klicken Sie auf Microsoft Graph-Ressourcen oder sogar bewusst, wenn dies geschieht.  Wenn Sie eine Anwendung für Benutzer von geschäftlichen und Schule erstellen, sollten Sie [den ursprünglichen Azure AD-Endpunkt](active-directory-developers-guide.md) verwenden, bis der Version 2.0-Endpunkt Geräteauthentifizierung unterstützt.  Weitere Informationen zu bedingter Access-basierten Gerät lesen Sie [in diesem Artikel](active-directory-conditional-access.md#device-based-conditional-access)aus.

##### <a name="windows-integrated-authentication-for-federated-tenants"></a>Windows-Authentifizierung für partnerverbundkontakte Mandanten
Wenn Sie in Windows-Clientanwendungen ADAL (und daher den ursprünglichen Azure AD-Endpunkt) verwendet haben, haben Sie geschaltet Vorteile des sogenannte der SAML-Assertion erteilen.  Diese erteilen kann Benutzer von verbundenen Azure AD Mandanten Authentifizierung im Hintergrund mit ihren mit lokalen Active Directory-Instanz ohne ihre Anmeldeinformationen eingeben zu müssen.  Erteilen der SAML-Assertion ist für den Endpunkt Version 2.0 derzeit nicht unterstützt.
