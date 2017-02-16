<properties
    pageTitle="Den Azure AD-Version 2.0-Endpunkt | Microsoft Azure"
    description="Ein Vergleich zwischen den ursprünglichen Azure AD- und die Endpunkte des Version 2.0."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="whats-different-about-the-v20-endpoint"></a>Was ist anders bei den Endpunkt Version 2.0?

Wenn Sie mit Azure Active Directory vertraut sind oder in der Vergangenheit Azure AD apps integriert haben, werden möglicherweise einige Unterschiede in der Version 2.0-Endpunkt, den Sie nicht erwartet.  In diesem Dokument wird sich diese Unterschiede für Ihr Verständnis.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory-Szenarien und Features werden von den Endpunkt Version 2.0 unterstützt.  Um festzustellen, ob den Version 2.0-Endpunkt verwendet werden sollen, erfahren Sie, [Version 2.0 Einschränkungen](active-directory-v2-limitations.md).


## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Microsoft-Konten und Azure AD-Konten
der Version 2.0-Endpunkt können Entwickler apps zu schreiben, die sowohl Microsoft Accounts und Azure AD-Konten, mithilfe eines einzelnen autorisierende Endpunkts anmelden akzeptieren.  Dies bietet die Möglichkeit, Ihre app vollständig Konto-unabhängige schreiben. Es kann nichts von den Typ des Kontos sein, die der Benutzer in signiert.  Natürlich *können* Sie Ihre app stellen den Typ des Kontos zusammen in einer bestimmten Sitzung beachten, aber nicht müssen.

Beispielsweise wenn Ihre app der [Microsoft Graph](https://graph.microsoft.io)-Anrufe, werden einige zusätzliche Funktionen und Daten Enterprise-Benutzern, wie ihre SharePoint-Websites oder Directory-Daten zur Verfügung.  Für viele Aktionen, wie z. B. [Lesen von e-Mail-Nachrichten eines Benutzers](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), der Code kann jedoch geschrieben werden genau für sowohl Microsoft Accounts und Azure AD-Konten.  

Integrieren von Ihrer app in Microsoft Accounts und Azure AD-Konten ist jetzt ein einfacher Vorgang aus.  Eine einzelne Reihe von Endpunkten, einer einzelnen Bibliothek und die Erfassung eines einzelnen app können Sie um Zugriff auf die Consumer und der Enterprise Umgebung zu erhalten.  Weitere Informationen zu den Endpunkt Version 2.0, lesen Sie [Übersicht über die](active-directory-appmodel-v2-overview.md)aus.


## <a name="new-app-registration-portal"></a>Neue app Registrierung-portal
der Version 2.0-Endpunkt kann nur in eine neue Position registriert werden: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Dies ist das Portal wo Personalnummer Anwendung bekomme Anpassen des Aussehens Ihrer app-Anmeldeseite und vieles mehr.  Sie müssen den Zugriff auf das Portal lediglich einem außerdem Microsoft-Konto - personal oder Arbeit/Schule Konto.  

Wir weiterhin diese App Registrierung Portal über einen Zeitraum mehr Funktionen hinzu.  Die Absicht ist dieses Portal am neuen Speicherort werden kann, wo Sie zum Verwalten von überall mit Ihrem Microsoft-apps zu tun haben, die an.


## <a name="one-app-id-for-all-platforms"></a>Eine app-Id für alle Plattformen
In den ursprünglichen Azure Active Directory-Dienst möglicherweise mehrere andere apps für ein einzelnes Projekt registriert haben.  Sie wurden separate app Registrierungen für Ihre systemeigene-Clients und Web apps verwendet wird:

![Alten Anwendung Registrierung UI](../media/active-directory-v2-flows/old_app_registration.PNG)

Beispielsweise, wenn Sie eine Website, und eine app für iOS erstellt haben, haben Sie separat, Registrieren mit zwei verschiedenen Anwendungs-Ids.  Wenn Sie über eine Website und einer Back-End-web-api verfügen Sie möglicherweise jede als separate app in Azure AD registriert haben.  Wenn Sie eine app für iOS und Android app hatten, Sie auch möglicherweise zwei verschiedenen apps konfiguriert wurden.  

<!-- You may have even registered different apps for each of your build environments - one for dev, one for test, and one for production. -->

Nun, die Sie benötigen lediglich eine einzelne app Registrierung und eine einzelne Anwendung Id für jedes Ihrer Projekte.  Sie können mehrere "Plattformen" zu einem einzelnen Projekt hinzufügen, und Sie können die entsprechenden Angaben für jede Plattform, die Sie hinzufügen.  Natürlich können Sie beliebig viele apps, die Ihnen gefällt, je nach Ihren Anforderungen, doch für den meisten Fällen nur eine Anwendung Id erforderlich sein sollte, erstellen.

<!-- You can also label a particular platform as "production-ready" when it is ready to be published to the outside world, and use that same Application Id safely in your development environments. -->

Unser Ziel ist, dass dies dazu, dass eine weitere vereinfachte app-Verwaltung und Erfahrung in der Entwicklung führen, und erstellen eine weitere konsolidierte Übersicht über ein einzelnes Projekt, an dem Sie arbeiten werden möglicherweise auf.


## <a name="scopes-not-resources"></a>Bereiche, die nicht von Ressourcen
In den ursprünglichen Azure AD-Dienst kann eine app als eine **Ressource**oder einen Empfänger der Token anders.  Definieren einer Ressource eine Anzahl der **Bereiche** oder **oAuth2Permissions** , die es versteht, gleicht Client apps Token auf diese Ressource für eine bestimmte Gruppe von Bereichen anfordern.  Beachten Sie die Azure AD Graph-API als Beispiel für eine Ressource aus:

- Resource Identifier, oder `AppID URI`:`https://graph.windows.net/`
- Bereiche, oder `OAuth2Permissions`: `Directory.Read`, `Directory.Write`usw..  

All dies gilt für die Version 2.0-Endpunkt.  Eine app kann weiterhin anders als Ressource, Definieren von Bereichen und mit einem URI gekennzeichnet werden.  Client-apps können weiterhin Zugriff auf diese Bereiche anfordern.  Die Möglichkeit, in der ein Client diese Berechtigungen anfordert, wurde jedoch geändert.  In der Vergangenheit eine OAuth 2.0 autorisieren Anfrage Azure AD möglicherweise wie gesucht haben:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

Stelle, an der der **Ressource** Parameter angegeben welche Ressource Autorisierung für die app Client anfordert.  Azure AD berechnet die Berechtigungen von der app basierend auf statische Konfiguration der Azure-Portal und Token entsprechend ausgestellt erforderlich.  Nun autorisieren die gleiche OAuth 2.0 Anforderung sieht wie folgt aus:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Stelle, an der der **Bereich** Parameter zeigt an, welche Ressource, und Berechtigungen die app Autorisierung für fordert. Die gewünschte Ressource dennoch sehr in der Anforderung vorhanden ist – es ist einfach in jedem der Werte in den Bereichsparameter einschließt.  Verwenden den Bereichsparameter auf diese Weise ermöglicht den Endpunkt Version 2.0 mehr mit der OAuth 2.0-Spezifikation kompatibel sein und genauer mit allgemeine Methoden für die Branche ausgerichtet.  Darüber hinaus können apps [inkrementell Zustimmung](#incremental-and-dynamic-consent), führen Sie die im nächsten Abschnitt beschrieben wird.

## <a name="incremental-and-dynamic-consent"></a>Inkrementell und dynamische Zustimmung
Apps registriert in allgemein verfügbar Azure AD Dienst erforderlich, um anzugeben, dass zum Zeitpunkt der Erstellung app im Portal Azure jeweils erforderlichen OAuth 2.0-Berechtigungen:

![Berechtigungen Registrierung UI](../media/active-directory-v2-flows/app_reg_permissions.PNG)

Die app erforderlich wurden Berechtigungen konfiguriert **statisch**.  Während dies zulässig Konfiguration von Azure-Portal vorhanden sein, dass die app und den Code übersichtliche und einfache gespeichert, bietet es ein paar Probleme für Entwickler:

- Eine app hat alle Berechtigungen kennen, die sie zum Zeitpunkt der Erstellung app jemals muss.  Hinzufügen von Berechtigungen über einen Zeitraum wurde ein schwieriger Prozess.
- Eine app hat aller Ressourcen kennen, die es jemals im Voraus zugreifen würden.  Es wurde schwierig, apps zu erstellen, die eine beliebige Anzahl von Ressourcen zugreifen können.
- Eine app hat, alle Berechtigungen anzufordern, die sie beim ersten Anmelden für des Benutzers jemals muss.  In einigen Fällen geführt hat dies auf eine sehr lange Liste von Berechtigungen, die Endbenutzer die Genehmigung des app Zugriff auf die ursprüngliche Anmeldung überfordert.

Mit den Endpunkt Version 2.0 können Sie angeben, die für Ihre app **dynamisch**zur Laufzeit, während der regelmäßigen Verwendung der app erforderlichen Berechtigungen.  Sie können dazu die Bereiche Ihre app zu einem bestimmten Zeitpunkt Zeitpunkt durch Einschließen in muss angeben der `scope` Parameter einer Anforderung Autorisierung:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Die oben angegebenen fordert die Berechtigung für die app zu einer Azure AD-Benutzers Directory-Daten, sowie das Schreiben von Daten in ihrem Verzeichnis lesen.  Wenn der Benutzer diese Berechtigungen in der Vergangenheit für diese spezielle app zugestimmt hat, sie werden einfach Geben Sie ihre Anmeldeinformationen und in der app angemeldet sein.  Wenn der Benutzer eine der folgenden Berechtigungen nicht zugestimmt hat, wird der Version 2.0-Endpunkt vom Benutzer Zustimmung zur jeweiligen Berechtigungen abzufragen.  Wenn Sie mehr erfahren möchten, können Sie auf [Berechtigungen, Zustimmung, und Bereichen](active-directory-v2-scopes.md)bis lesen.

Gleicht einer app zu Berechtigungen anzufordern dynamisch über die `scope` Parameter haben Sie die vollständige Kontrolle über Erfahrung des Benutzers.  Wenn Sie möchten, können Sie auswählen, um Frontload Ihre Zustimmung für alle Berechtigungen in einer anfänglichen Autorisierung Anforderung Fragen und Funktionalität.  Oder wenn Ihre app eine große Anzahl von Berechtigungen sind erforderlich, Sie können auch die Berechtigungen des Benutzers inkrementell, erfassen sie versuchen, über einen Zeitraum bestimmter Features der app zu verwenden.

## <a name="well-known-scopes"></a>Bekannte Bereiche

#### <a name="offline-access"></a>Offline-Zugriff
der Endpunkt Version 2.0 erfordern möglicherweise die Verwendung von einer bekannten neuen Berechtigungsstufe für apps - die `offline_access` Bereich.  Alle apps müssen diese Berechtigung anfordern, wenn sie den Zugriff auf Ressourcen auf den Auftrag eines Benutzers für einen längeren Zeitraum, selbst wenn der Benutzer nicht aktiv die app möglicherweise verwenden müssen.  Die `offline_access` Bereich erscheinen für den Benutzer in Zustimmung Dialogfeldern als "Zugriff auf Ihre Daten offline", die der Benutzer zustimmen muss.  Anfordern der `offline_access` Berechtigung wird OAuth 2.0 Refresh_tokens aus den Endpunkt Version 2.0 erhalten Web app zu aktivieren.  Refresh_tokens langer Lebensdauer sind und für längere Zeiträume von Access für neue OAuth 2.0 Access_tokens ausgetauscht werden können.  

Wenn Ihre app keine anfordert der `offline_access` Bereich, erhalten keine Refresh_tokens.  Dies bedeutet, wenn Sie eine Authorization_code im [OAuth 2.0 Autorisierung Code Fluss](active-directory-v2-protocols.md#oauth2-authorization-code-flow)einlösen, nur wieder ein Access_token von Sie erhalten die `/token` Endpunkt.  Die Access_token für einen kurzen Zeitraum (in der Regel eine Stunde) gültig bleiben, aber schließlich ablaufen.  AT, die die Zeitpunkt, Ihre app benötigen, können Sie den Benutzer umleiten zurück zu den `/authorize` Endpunkt zum Abrufen eines neuen Authorization_code.  Während dieser umleiten, der Benutzer kann oder möglicherweise nicht müssen Sie ihre Anmeldeinformationen erneut eingeben oder Zustimmung erneut Berechtigungen, je nach den den Typ der app.

Weitere Informationen zu OAuth 2.0, Refresh_tokens und Access_tokens zu erhalten, schauen Sie sich den [Bezug der Version 2.0-Protokoll](active-directory-v2-protocols.md).

#### <a name="openid-profile--email"></a>OpenID, Profil und e-Mails

In den ursprünglichen Azure Active Directory-Dienst würde der grundlegendste OpenID verbinden Anmeldung Fluss bereitstellen eine Vielzahl von Informationen über den Benutzer in die resultierende Id_token.  Die Ansprüche in einer Id_token können des Benutzers Name, bevorzugte Benutzernamen, e-Mail-Adresse, Objekt-ID und weitere einbeziehen.

Wir werden nun die Informationen einschränken, die die `openid` Umfang entspricht Ihren app-Zugriff auf.  Der Bereich 'Openid' wird nur Ihre app sich den Benutzer zulassen, und erhalten einen app-spezifischen Bezeichner für den Benutzer.  Wenn Sie personenbezogene Informationen (PII) über den Benutzer in Ihrer app abrufen möchten, müssen Ihre app zusätzliche Berechtigungen vom Benutzer anzufordern.  Wir werden zwei neue Bereiche – Einführung in die der `email` und `profile` Bereiche – mit denen Sie dies tun können.

Die `email` Bereich ist sehr einfach – dies zulässt Ihren app-Zugriff auf primäre e-Mail-Adresse des Benutzers über die `email` in der Id_token beanspruchen.  Die `profile` Umfang entspricht Ihren app-Zugriff auf andere grundlegende Informationen über den Benutzer – deren Namen, die bevorzugte Benutzernamen, Objekt-ID und vieles mehr.

So können Sie Ihre app in einer Weise minimale Offenlegung Fehlercode – können nur bitten Sie den Benutzer für die Menge der Informationen, dass Ihre app, die für ihre Arbeit erforderlich ist.  Weitere Informationen über diese Bereiche finden Sie in [der Version 2.0 Umfang Bezug](active-directory-v2-scopes.md). 

## <a name="token-claims"></a>Token Ansprüche

Die Ansprüche im Token ausgestellt von den Endpunkt Version 2.0 werden nicht identisch mit Token ausgestellt von allgemein verfügbar Azure AD-Endpunkte - apps Migrieren zu den neuen Dienst sollte nicht voraussetzen einer bestimmten anfordern wird in Id_tokens oder Access_tokens vorhanden sind.   Token ausgestellt von den Endpunkt Version 2.0 mit den Spezifikationen OAuth 2.0, und verbinden Sie OpenID kompatibel sind, aber möglicherweise andere Semantik als allgemein verfügbar folgen Azure AD-Dienst.

Weitere Informationen zu den bestimmten Ansprüche im Token Version 2.0 ausgegeben finden Sie unter [Version 2.0 token Bezug](active-directory-v2-tokens.md).

## <a name="limitations"></a>Einschränkungen
Es gibt einige Einschränkungen bei Verwendung des Punktes Version 2.0 berücksichtigt werden.  Lizenzinformationen finden Sie die [Version 2.0 Einschränkungen Dokument](active-directory-v2-limitations.md) anzeigen, wenn eine der folgenden Einschränkungen für das spezifische Szenario anwenden.
