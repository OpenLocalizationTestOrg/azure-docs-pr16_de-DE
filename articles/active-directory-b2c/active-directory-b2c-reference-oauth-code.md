<properties
    pageTitle="Azure-Active Directory B2C | Microsoft Azure"
    description="Erstellen von Webanwendungen mithilfe der Azure-Active Directory-Implementierung von der herstellen OpenID Authentication-Protokoll."
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
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Azure Active Directory B2C: OAuth 2.0 Autorisierung Code Fluss

In apps, die auf einem Gerät für den Zugriff auf geschützte Ressourcen, z. B. Web APIs installiert werden, kann die OAuth 2.0 Autorisierung Code erteilen verwendet werden. Mithilfe der Azure-Active Directory (Azure AD) B2C Implementierung von OAuth 2.0, können Sie Verwaltungsaufgaben für die Anmeldung, Anmeldung und andere Identität für Ihre mobile und desktop-apps hinzufügen. Dieses Handbuch ist unabhängig Sprache. Es wird beschrieben, wie senden und Empfangen von HTTP-Nachrichten ohne unsere öffnen Source-Bibliotheken.

<!-- TODO: Need link to libraries -->

Illustrieren OAuth 2.0 Autorisierung Code wird im [Abschnitt 4.1 der OAuth 2.0-Spezifikation](http://tools.ietf.org/html/rfc6749)beschrieben. Sie können Sie in den meisten app Dateitypen, einschließlich [Web apps](active-directory-b2c-apps.md#web-apps) und [systembedingt installierten apps](active-directory-b2c-apps.md#mobile-and-native-apps)Authentifizierung und Autorisierung durchzuführen. Sie können apps zum Erfassen von sicher **Access_tokens**, die verwendet werden können, den Zugriff auf Ressourcen, die von einem [Server Autorisierung](active-directory-b2c-reference-protocols.md#the-basics)gesichert werden.

In diesem Handbuch liegt der Schwerpunkt auf einer bestimmten Art der OAuth 2.0 Autorisierung Code Fluss –**Öffentliche Clients**. Ein öffentlicher Client ist eine Clientanwendung, die die Integrität des geheimen Kennwort sicher verwalten nicht vertrauenswürdig. Dies umfasst mobilen apps, desktop-apps und ziemlich viel jede Anwendung, die auf einem Gerät ausgeführt wird und muss Access_tokens zu erhalten. Wenn Sie mithilfe von Azure AD B2C Identität Verwaltung einer Web app hinzufügen möchten, sollten Sie [OpenID verbinden](active-directory-b2c-reference-oidc.md) statt OAuth 2.0 verwenden.

Azure AD B2C erweitert den Standard-OAuth 2.0 fließt, um mehr als einfache Authentifizierung und Autorisierung auszuführen. Es gibt [**Richtlinienparameter**](active-directory-b2c-reference-policies.md), womit Sie Sie OAuth 2.0 Benutzerfunktionalität zu Ihrer Anwendung, z. B. hinzufügen können Anmeldung, Anmeldung und Verwaltung von Profilen. Hier zeigen wir wie OAuth 2.0 und Richtlinien verwenden, um jede der folgenden Erfahrung in einer systemeigenen Anwendung implementieren. Wir zeigen auch so Access_tokens für den Zugriff auf Web-APIs zu erhalten.

Im folgenden Beispiel HTTP-Anfragen werden unser Beispiel B2C Verzeichnis, **fabrikamb2c.onmicrosoft.com**, ebenso wie unsere Beispiel-Anwendung und Richtlinien verwendet. Sie sind kostenlos, die Anfragen sich selbst zu testen mithilfe der folgenden Werte, oder Sie können sie durch ein eigenes ersetzen.
Erfahren Sie, wie Sie [Eigene B2C Directory-Anwendung, und Richtlinien](#use-your-own-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1 holen Sie 1 sich einen Autorisierungscode
Autorisierung Code illustrieren beginnt mit dem Client, leiten den Benutzer, zu der `/authorize` Endpunkt. Dies ist die interaktive Teil des Ablaufs, Stelle, an der der Benutzer tatsächlich Aktion dauert. In dieser Anforderung, der Client zeigt an, die Berechtigungen, die es abrufen aus der Benutzer muss die `scope` Parameter und die Richtlinie auszuführende in der `p` Parameter. Drei Beispiele für die unten (mit Zeilenumbrüche zur Verbesserung der Lesbarkeit), jede mit einer anderen Richtlinie wurde.

#### <a name="use-a-sign-in-policy"></a>Verwenden Sie eine Richtlinie Anmeldung

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Verwenden Sie eine Richtlinie Anmelde

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Verwenden Sie eine Richtlinie Profil bearbeiten

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Parameter | Erforderlich? | Beschreibung |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Erforderlich | Die Anwendung-ID, die Ihre app der [Azure-Portal](https://portal.azure.com) zugewiesen. |
| response_type | Erforderlich | Der Antworttyp, welche enthalten muss `code` für die Autorisierung Code Fluss. |
| redirect_uri | Erforderlich | Die Redirect_uri der app, wo Authentifizierungsantworten gesendet und Empfangen von Ihrer app werden können. Es muss exakt eine der Redirect_uris, die Sie im Portal registriert mit der Ausnahme, dass es URL codiert werden muss. |
| Bereich | Erforderlich | Ein Leerzeichen getrennte Liste mit Bereichen. Ein einzelnen Bereichswert zeigt an, um beide Azure AD über die Berechtigungen, die aufgefordert werden. Verwenden die Client-ID aus, wie der Bereich zeigt an, dass Ihre app ein **Token Zugriff** benötigt, die mit Ihrer eigenen Dienst oder Web API verwendet werden kann, dargestellt durch die gleiche Client-ID  Die `offline_access` Bereich zeigt an, dass Ihre app ein **Refresh_token** für langer Lebensdauer Zugriff auf Ressourcen benötigt werden.  Sie können auch die `openid` Bereich eines **Id_token** von Azure AD B2C anfordern. |
| response_mode | Empfohlen | Die Methode, die verwendet werden soll, das sich daraus ergebende Authorization_code wieder zu Ihrer Anwendung zu senden. Sie können eine der 'Abfrage', 'Form_post' oder 'Fragment' sein.
| Bundesstaat | Empfohlen | Einen Wert enthalten, in der Besprechungsanfrage, die auch in der token Antwort zurückgegeben wird. Sie können eine Textzeichenfolge alle Inhalte sein, soll. Ein eindeutiger erzeugten Wert wird in der Regel für die websiteübergreifende Anforderungsfälschungsangriffe verhindern verwendet. Der Status wird auch Informationen zu den Status des Benutzers in der app codieren, bevor die Authentifizierungsanfrage aufgetreten, wie die Seite, die, der Sie ist auf Waren, oder die Richtlinie ausgeführt wird, verwendet. |
| p | Erforderlich | Die Richtlinie, die ausgeführt wird. Es ist der Name einer Richtlinie, die in Ihrem Verzeichnis B2C erstellt wird. Der Wert für die Richtlinie Name sollte beginnen mit "b2c\_1\_". Weitere Informationen zu den Richtlinien in [Extensible Policy Framework](active-directory-b2c-reference-policies.md). |
| Aufforderung | Optional | Die Art der Interaktion mit dem Benutzer, die erforderlich ist. Zu diesem Zeitpunkt der einzige gültige Wert ist 'Anmeldung', wodurch den Benutzer die Anforderung seine Anwendungsinformationen eingeben zu müssen. Einmaliges Anmelden wird nicht wirksam werden. |

An diesem Punkt werden der Benutzer aufgefordert, die Richtlinie für den Workflow abzuschließen. Dies kann den Benutzer seinen Benutzernamen und Kennwort eingeben Anmelden mit einem sozialen Identität, Anmelden für Verzeichnis oder einer beliebigen anderen Zahl Schritten, je nachdem, wie die Richtlinie definiert ist beinhalten.

Nach Abschluss der Benutzer die Richtlinie Azure AD zurückgegeben werden kann eine Antwort auf Ihre app auf die angegebene `redirect_uri`, mit der angegebenen Methode der `response_mode` Parameter. Die Antwort wird genau für jeden der oben genannten Fälle, unabhängig von der Richtlinie identisch sein, die ausgeführt wurde.

Eine erfolgreiche Antwort, die verwendet `response_mode=query` sieht wie folgt aus:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Code | Die Authorization_code, die die app angefordert. Den Autorisierungscode können die app ein Access_token für eine Ressource anfordern. Authorization_codes sind sehr kurzlebige. In der Regel, diese nach etwa 10 Minuten ablaufen. |
| Bundesstaat | Finden Sie die vollständige Beschreibung in der obigen Tabelle ein. Wenn der Parameter Status in der Besprechungsanfrage enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die app sollte überprüfen, ob die Statuswerte in die Anfrage und die Antwort identisch sind. |

Fehler beim Antworten möglicherweise auch gesendet werden, mit der `redirect_uri` , dass die app diese angemessen behandelt werden kann:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Ein Fehlercodezeichenfolge, die kann verwendet werden, um die Fehlertypen klassifizieren, die auftreten, und kann verwendet werden, um auf Fehler zu reagieren. |
| error_description | Eine bestimmte Fehlermeldung, die einen Entwickler zum Identifizieren der Ursache eines Authentifizierungsfehlers helfen können. |
| Bundesstaat | Finden Sie in der ersten Tabelle in diesem Abschnitt die vollständige Beschreibung ein. Wenn der Parameter Status in der Besprechungsanfrage enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die app sollte überprüfen, ob die Statuswerte in die Anfrage und die Antwort identisch sind. |


## <a name="2-get-a-token"></a>2. ein Token abrufen
Jetzt, da Sie eine Authorization_code erworben haben, können Sie einlösen die `code` für ein Token auf die gewünschte Ressource durch Senden eines `POST` zum Anfordern der `/token` Endpunkt. In Azure AD B2C ist die einzige Ressource, der Sie, ein Token für anfordern können Ihre app eigenen Back-End-Web-API. Der Messe, die verwendet wird, für die ein Token an sich selbst anfordert besteht darin, Ihre app Client-ID als Umfang verwenden:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parameter | Erforderlich? | Beschreibung |
| ----------------------- | ------------------------------- | --------------------- |
| p | Erforderlich | Die Richtlinie, die verwendet wurde, um die Autorisierungscode erwerben. Sie können nicht in dieser Anforderung eine andere Richtlinie verwenden. Beachten Sie, dass Sie für diesen Parameter der *Abfragezeichenfolge*, nicht in den POST-Text hinzufügen. |
| client_id | Erforderlich | Die Anwendung-ID, die Ihre app der [Azure-Portal](https://portal.azure.com) zugewiesen. |
| grant_type | Erforderlich | Den Typ des erteilen, die sein muss `authorization_code` für die Autorisierung Code Fluss. |
| Bereich | Empfohlen | Ein Leerzeichen getrennte Liste mit Bereichen. Ein einzelnen Bereichswert zeigt an, um beide Azure AD über die Berechtigungen, die aufgefordert werden. Verwenden die Client-ID aus, wie der Bereich zeigt an, dass Ihre app ein **Token Zugriff** benötigt, die mit Ihrer eigenen Dienst oder Web API verwendet werden kann, dargestellt durch die gleiche Client-ID  Die `offline_access` Bereich zeigt an, dass Ihre app ein **Refresh_token** für langer Lebensdauer Zugriff auf Ressourcen benötigt werden.  Sie können auch die `openid` Bereich eines **Id_token** von Azure AD B2C anfordern. |
| Code | Erforderlich | Die Authorization_code, die Sie in die Ausrichtung des ersten Abschnitts des Ablaufs erworben haben. |
| redirect_uri | Erforderlich | Die Redirect_uri der Anwendung, in dem Sie die Authorization_code erhalten haben. |

Wie wird eine erfolgreiche token Antwort aus:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| not_before | Die Uhrzeit, an der das Token, Epoche Zeitpunkt gültig ist. |
| token_type | Der token Typwert. Person ist nur der Typ, der Azure AD unterstützt. |
| access_token | Das signiertes JSON Web Token (JWT) Token, das Sie angefordert. |
| Bereich | Bereichen, die das Token gültig ist, ist die zum Zwischenspeichern von Token zur späteren Verwendung verwendet werden können. |
| expires_in | Die Länge der Zeit, die das Token (in Sekunden) gültig ist. |
| refresh_token | Eine Refresh_token OAuth 2.0. Dieses Token können die app Erfassen von zusätzlichen Token nach das aktuelle Token läuft ab. Refresh_tokens lange halten werden, und zum Zugriff auf Ressourcen für längere Zeit beibehalten verwendet werden können. Weitere Details finden Sie unter [B2C token verweisen](active-directory-b2c-reference-tokens.md). |

Fehler beim Antworten sieht wie aus:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Ein Fehlercodezeichenfolge, die kann verwendet werden, um die Fehlertypen klassifizieren, die auftreten, und kann verwendet werden, um auf Fehler zu reagieren. |
| error_description | Eine bestimmte Fehlermeldung, die einen Entwickler zum Identifizieren der Ursache eines Authentifizierungsfehlers helfen können. |

## <a name="3-use-the-token"></a>3 verwenden Sie 3 das token
Jetzt, da Sie bereits erfolgreich gekauft haben ein `access_token`, verwenden Sie das Token in Besprechungsanfragen in Ihrem Back-End-web-APIs durch Einschließen in der `Authorization` Kopfzeile:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4 aktualisieren Sie 4 das token
Access & Id Token sind kurzlebige. Sie müssen diese aktualisiert, nachdem sie ablaufen, um den Vorgang fortzusetzen, wird der Zugriff auf Ressourcen. Sie können dazu Übermitteln einer anderen `POST` zum Anfordern der `/token` Endpunkt. Geben Sie diesmal die `refresh_token` statt der `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parameter | Erforderlich? | Beschreibung |
| ----------------------- | ------------------------------- | -------- |
| p | Erforderlich | Die Richtlinie, die zum Erfassen von der ursprünglichen Refresh_token verwendet wurde. Sie können nicht in dieser Anforderung eine andere Richtlinie verwenden. Beachten Sie, dass Sie für diesen Parameter der *Abfragezeichenfolge*, nicht in den POST-Text hinzufügen. |
| client_id | Empfohlen | Die Anwendung-ID, die Ihre app der [Azure-Portal](https://portal.azure.com) zugewiesen. |
| grant_type | Erforderlich | Den Typ des erteilen, die sein muss `refresh_token` für dieses Abschnitts eines Autorisierung Code illustrieren. |
| Bereich | Empfohlen | Ein Leerzeichen getrennte Liste mit Bereichen. Ein einzelnen Bereichswert zeigt an, um beide Azure AD über die Berechtigungen, die aufgefordert werden. Verwenden die Client-ID aus, wie der Bereich zeigt an, dass Ihre app ein **Token Zugriff** benötigt, die mit Ihrer eigenen Dienst oder Web API verwendet werden kann, dargestellt durch die gleiche Client-ID  Die `offline_access` Bereich zeigt an, dass Ihre app ein **Refresh_token** für langer Lebensdauer Zugriff auf Ressourcen benötigt werden.  Sie können auch die `openid` Bereich eines **Id_token** von Azure AD B2C anfordern. |
| redirect_uri | Optional | Die Redirect_uri der Anwendung, in dem Sie die Authorization_code erhalten haben. |
| refresh_token | Erforderlich | Die ursprüngliche Refresh_token, die Sie in der zweiten Ausrichtung des Abschnitts des Ablaufs erworben haben. |

Wie wird eine erfolgreiche token Antwort aus:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| not_before | Die Uhrzeit, an der das Token, Epoche Zeitpunkt gültig ist. |
| token_type | Der token Typwert. Person ist nur der Typ, der Azure AD unterstützt. |
| access_token | Das signiertes JSON Web Token (JWT) Token, das Sie angefordert. |
| Bereich | Bereichen, die das Token gültig ist, ist die zum Zwischenspeichern von Token zur späteren Verwendung verwendet werden können. |
| expires_in | Die Länge der Zeit, die das Token (in Sekunden) gültig ist. |
| refresh_token | Eine Refresh_token OAuth 2.0. Dieses Token können die app Erfassen von zusätzlichen Token nach das aktuelle Token läuft ab. Refresh_tokens lange halten werden, und zum Zugriff auf Ressourcen für längere Zeit beibehalten verwendet werden können. Weitere Details finden Sie unter [B2C token verweisen](active-directory-b2c-reference-tokens.md). |

Fehler beim Antworten sieht wie aus:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Zeichenfolge des Fehlercodes, die zum Fehlertypen klassifizieren, die auftreten verwendet werden kann, und kann verwendet werden, um auf Fehler zu reagieren. |
| error_description | Eine bestimmte Fehlermeldung, die einen Entwickler die Ursache eines Authentifizierungsfehlers ermitteln helfen können. |

## <a name="use-your-own-b2c-directory"></a>Verwenden Sie Ihrer eigenen B2C-Verzeichnis

Wenn Sie diese Anfragen selbst ausprobieren möchten, müssen Sie zunächst diese drei Schritte ausführen, und klicken Sie dann die Beispielwerten über durch ein eigenes ersetzen:

- [Erstellen Sie ein Verzeichnis B2C](active-directory-b2c-get-started.md) und verwenden Sie den Namen Ihres Verzeichnisses in die Anforderungen.
- [Erstellen einer Anwendung](active-directory-b2c-app-registration.md) zur Personalnummer Anwendung und einer Redirect_uri zu erhalten. Sie möchten einen **native Client** in Ihre app aufnehmen möchten.
- [Erstellen Ihrer Richtlinien](active-directory-b2c-reference-policies.md) , um den Richtliniennamen Ihrer zu erhalten.
