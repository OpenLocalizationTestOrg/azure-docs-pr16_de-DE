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

# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C: Web-Anmelden mit OpenID verbinden

OpenID verbinden ist ein Protokoll zur Authentifizierung basiert auf OAuth 2.0, die sichere sich Benutzer in Webanwendungen verwendet werden können.  Mithilfe der Azure-Active Directory (Azure AD) B2C-Implementierung von OpenID verbinden, Sie können externe Anmeldung, Anmeldung und andere Identitätsmanagement in Ihre Webanwendungen zum Azure AD-auftritt. Mit diesem Leitfaden wird gezeigt, wie in einer Weise Sprache unabhängig vergeblich werden. Es wird beschrieben, wie senden und Empfangen von HTTP-Nachrichten ohne unsere öffnen Source-Bibliotheken.

[Verbinden von OpenID](http://openid.net/specs/openid-connect-core-1_0.html) erweitert das OAuth 2.0 *Autorisierung* Protokoll für den Einsatz als *Authentication* -Protokoll. So können Sie einmaliges Anmelden mit OAuth durchführen. Es das Konzept von einer `id_token`, welche ist ein Sicherheitstoken, über die der Client zum Überprüfen der Identität des Benutzers und grundlegende Profilinformationen über den Benutzer zu erhalten.

Da sie OAuth 2.0 erweitert, ermöglicht es auch apps sicher **Access_tokens**erwerben. Sie können Access_tokens Zugriff auf die Ressourcen, die von einem [Server Autorisierung](active-directory-b2c-reference-protocols.md#the-basics)geschützt sind. Wir empfehlen OpenID verbinden, wenn Sie eine Webanwendung erstellen, die auf einem Server gehostet wird und Sie über einen Browser. Wenn Sie den mobilen oder desktop-Clientanwendungen mithilfe von Azure AD B2C Identitätsmanagement hinzufügen möchten, sollten Sie OpenID herstellen, anstatt [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) verwenden.

Azure AD B2C erweitert standard OpenID verbinden Protokoll um mehr als einfache Authentifizierung und Autorisierung auszuführen. Es gibt [**Richtlinienparameter**](active-directory-b2c-reference-policies.md), womit Sie OpenID verbinden verwenden, um Ihre app – wie Benutzerfunktionalität hinzufügen können Anmeldung, Anmeldung und Verwaltung von Profilen. Hier zeigen wir Ihnen so OpenID verbinden und Richtlinien verwenden, um jede der folgenden Erfahrung in Ihre Webanwendungen implementieren. Wir werden Sie auch so erhalten Sie Access_tokens für den Zugriff auf Web-APIs anzeigen.

Im folgenden Beispiel HTTP-Anfragen werden unser Beispiel B2C Verzeichnis, **fabrikamb2c.onmicrosoft.com**, ebenso wie unsere Stichprobe Anwendung **https://aadb2cplayground.azurewebsites.net** und Richtlinien verwendet. Sie können Sie die Anfragen selbst ausprobieren, indem Sie mithilfe der folgenden Werte oder Sie können sie durch ein eigenes ersetzen.
Erfahren Sie, wie Sie [Eigene B2C Mandanten, Anwendung, und Richtlinien](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Senden Sie Besprechungsanfragen Authentifizierung
Wenn die Web-app muss Authentifizierung des Benutzers, und führen Sie eine Richtlinie, können sie weisen Sie ihn an die `/authorize` Endpunkt. Dies ist die interaktive Teil des Ablaufs, wo wird der Benutzer tatsächlich, abhängig von der Richtlinie bezüglich.

In dieser Anforderung, der Client zeigt an, die Berechtigungen, die es abrufen aus der Benutzer muss die `scope` Parameter und die Richtlinie auszuführende in der `p` Parameter. Drei Beispiele für die unten (mit Zeilenumbrüche zur Verbesserung der Lesbarkeit), jede mit einer anderen Richtlinie wurde. Zu verschaffen Sie sich einen Eindruck für jede Anforderung Funktionsweise, die Anfrage in einem Browser einfügen, und starten ihn.

#### <a name="use-a-sign-in-policy"></a>Verwenden Sie eine Richtlinie Anmeldung

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Verwenden Sie eine Richtlinie Anmelde

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Verwenden Sie eine Richtlinie Profil bearbeiten

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parameter | Erforderlich? | Beschreibung |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Erforderlich | Die Anwendung-ID, die Ihre app der [Azure-Portal](https://portal.azure.com/) zugewiesen. |
| response_type | Erforderlich | Der Antworttyp, welche enthalten muss `id_token` für OpenID verbinden. Wenn Ihre Web app auch Token benötigen werden für das Anrufen von einer Webs-API, können Sie `code+id_token`, wie wir es getan haben. |
| redirect_uri | Empfohlen | Die Redirect_uri der app, wo Authentifizierungsantworten gesendet und Empfangen von Ihrer app werden können. Es muss exakt eine der Redirect_uris, die Sie im Portal registriert mit der Ausnahme, dass es URL codiert werden muss. |
| Bereich | Erforderlich | Ein Leerzeichen getrennte Liste mit Bereichen. Ein einzelnen Bereichswert zeigt an, um beide Azure AD über die Berechtigungen, die aufgefordert werden. Die `openid` Bereich eine Berechtigung Anmeldung des Benutzers und Abrufen von Daten über den Benutzer in Form von **Id_tokens** (Weitere Informationen zu diesem später in diesem Artikel folgen) angezeigt. Die `offline_access` Bereich ist optional für Web apps. Er gibt an, dass Ihre app ein **Refresh_token** für langer Lebensdauer Zugriff auf Ressourcen benötigt werden. |
| response_mode | Empfohlen | Die Methode, die verwendet werden soll, das sich daraus ergebende Authorization_code wieder zu Ihrer Anwendung zu senden. Sie können eine der 'Abfrage', 'Form_post' oder 'Fragment' sein.  'Form_post' wird aus Sicherheitsgründen sollten empfohlen. |
| Bundesstaat | Empfohlen | Einen Wert enthalten, in der Besprechungsanfrage, die auch in der token Antwort zurückgegeben wird. Es kann eine Textzeichenfolge alle Inhalte sein, soll. Ein eindeutiger erzeugten Wert wird in der Regel für die websiteübergreifende Anforderungsfälschungsangriffe verhindern verwendet. Der Status wird auch Informationen zu den Status des Benutzers in der app codieren, bevor die Authentifizierungsanfrage wie der Seite stattgefunden, denen sie sich auf verwendet. |
| Nonce | Erforderlich | Ein Wert in der Anforderung (generiert von der app), die Bestandteil der resultierende Id_token als Anspruch enthalten. Die app kann dann diesen Wert, um die Wiedergabe token Angriffen zu verringern überprüfen. Der Wert ist in der Regel eine zufällige, eindeutige Zeichenfolge, die verwendet werden kann, um den Ursprung der Anfrage zu identifizieren. |
| p | Erforderlich | Die Richtlinie, die ausgeführt wird. Es ist der Name einer Richtlinie, die in Ihrem Mandanten B2C erstellt wird. Der Wert für Name Richtlinie sollte beginnen mit "b2c\_1\_". Weitere Informationen zu den Richtlinien in [Extensible Policy Framework](active-directory-b2c-reference-policies.md). |
| Aufforderung | Optional | Die Art der Interaktion mit dem Benutzer, die erforderlich ist. Zu diesem Zeitpunkt der einzige gültige Wert ist 'Anmeldung', wodurch den Benutzer die Anforderung seine Anwendungsinformationen eingeben zu müssen. Einmaliges Anmelden wird nicht wirksam werden. |

An diesem Punkt werden der Benutzer aufgefordert, die Richtlinie für den Workflow abzuschließen. Dies kann den Benutzer seinen Benutzernamen und Kennwort eingeben Anmelden mit einem sozialen Identität, Anmelden für Verzeichnis oder einer beliebigen anderen Zahl Schritten, je nachdem, wie die Richtlinie definiert ist beinhalten.

Nach Abschluss der Benutzer die Richtlinie Azure AD zurückgegeben werden kann eine Antwort auf Ihre app auf die angegebene `redirect_uri`, mithilfe der Methode, die in angegeben ist die `response_mode` Parameter. Die Antwort wird genau für jeden der oben genannten Fälle, unabhängig von der Richtlinie identisch sein, die ausgeführt wurde.

Eine erfolgreiche Antwort mit `response_mode=fragment` würde so aussehen:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| id_token | Die Id_token, die die app angefordert. Sie können die Id_token zum Überprüfen der Identität des Benutzers, und beginnen eine Sitzung mit dem Benutzer verwenden. Informationen zur Id_tokens und deren Inhalt sind in den [Azure AD B2C token Verweis](active-directory-b2c-reference-tokens.md)enthalten. |
| Code | Die Authorization_code, die die app angefordert, wenn Sie verwendet haben, `response_type=code+id_token`. Den Autorisierungscode können die app ein Access_token für eine Ressource anfordern. Authorization_codes sind sehr kurzlebige. In der Regel, diese nach etwa 10 Minuten ablaufen. |
| Bundesstaat | Wenn der Parameter Status in der Besprechungsanfrage enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die app sollte überprüfen Sie, ob die Bundesstaat Werte in die Anforderung und Antwort identisch sind. |

Fehler beim Antworten können auch gesendet werden, mit der `redirect_uri` , dass die app diese angemessen behandelt werden kann:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Zeichenfolge des Fehlercodes, die zum Fehlertypen klassifizieren, die auftreten verwendet werden kann, und kann verwendet werden, um auf Fehler zu reagieren. |
| error_description | Eine bestimmte Fehlermeldung, die einen Entwickler die Ursache eines Authentifizierungsfehlers ermitteln helfen können. |
| Bundesstaat | Finden Sie die vollständige Beschreibung in der obigen Tabelle ein. Wenn der Parameter Status in der Besprechungsanfrage enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die app sollte überprüfen Sie, ob die Bundesstaat Werte in die Anforderung und Antwort identisch sind. |


## <a name="validate-the-idtoken"></a>Überprüfen der id_token
Nur empfangen eine Id_token reicht nicht zum Authentifizieren des Benutzers – müssen die Id_token der Signatur überprüft und überprüfen Sie die Angaben im Token gemäß Ihrer app Anforderungen. Azure AD B2C verwendet [JSON Web Token (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) und öffentlichem Schlüssel Token melden, und stellen Sie sicher, dass sie gültig sind.

Es gibt viele Open Source-Bibliotheken, die für die Überprüfung JWTs, je nachdem Ihre bevorzugte Sprache verfügbar sind. Es empfiehlt sich diese Optionen untersuchen, anstatt eigene Überprüfungslogik zu implementieren. Hier die Informationen werden hilfreich, um herauszufinden, wie diese Bibliotheken ordnungsgemäß zu verwenden.

Azure AD B2C verfügt über eine OpenID verbinden Metadaten-Endpunkt, wodurch eine app zum Abrufen von Informationen zu Azure AD B2C zur Laufzeit. Diese Informationen umfassen Endpunkte, token Inhalt und Token Schlüssel signieren. Es ist ein JSON-Metadaten-Dokument für jede Richtlinie in Ihrem Mandanten B2C aus. Beispielsweise die Metadatendokument für die `b2c_1_sign_in` Richtlinie in der `fabrikamb2c.onmicrosoft.com` befindet sich unter:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Ist eine der Eigenschaften dieses Dokuments Konfiguration der `jwks_uri`, dessen Wert für die gleiche Richtlinie wäre:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

In bestellen, um festzustellen, welche Richtlinie verwendet wurde, in einer Id_token signieren (und wo Sie die Metadaten aus abgerufen werden sollen), stehen Ihnen zwei Optionen zur Verfügung. Zunächst ist der Richtlinienname enthalten, der `acr` in der Id_token beanspruchen. Informationen zum Analysieren der Ansprüche aus einer Id_token finden Sie unter den [Azure AD B2C token Bezug](active-directory-b2c-reference-tokens.md). Eine andere Möglichkeit besteht darin, die Richtlinie in den Wert von Codieren der `state` Parameter, wenn Sie die Anforderung und Entschlüsseln klicken Sie dann darauf, um zu bestimmen, welche Richtlinie verwendet wurde. Beide Methoden sind uneingeschränkt.

Nachdem Sie das Metadatendokument aus den Metadaten-Endpunkt OpenID verbinden erworben haben, können Sie die öffentlichen Schlüssel von RSA 256 (die an diesem Endpunkt befinden) auf die Signatur der Id_token überprüfen. Möglicherweise gibt es mehrere Schlüssel an diesem Endpunkt zu einem bestimmten Zeitpunkt Zeitpunkt aufgelistet werden, jeweils identifiziert, indem Sie eine `kid`. Die Kopfzeile der das Id_token enthält auch eine `kid` beanspruchen, die angibt, welche der folgenden Tasten zum Melden der Id_token verwendet wurde. Finden Sie unter [Azure AD B2C token Bezug](active-directory-b2c-reference-tokens.md) für Weitere Informationen, einschließlich der Abschnitte [Token überprüfen](active-directory-b2c-reference-tokens.md#validating-tokens) und [wichtige Informationen zum Signieren von Key Rollover](active-directory-b2c-reference-tokens.md#validating-tokens).
<!--TODO: Improve the information on this-->

Nachdem Sie die Signatur der Id_token überprüft haben, stehen mehrere Ansprüche, die Sie benötigen, um zu überprüfen, z. B.:

- Sie sollten Validieren der `nonce` beanspruchen um token Wiederholungsangriffe zu verhindern. Der Wert sollte sein, in der Besprechungsanfrage Anmeldung angegeben haben.
- Sie sollten Validieren der `aud` beanspruchen, um sicherzustellen, dass die Id_token für Ihre app ausgestellt wurde. Der Wert sollte die Anwendung-ID Ihre app sein.
- Überprüfen Sie sollten die `iat` und `exp` tatsächlich um sicherzustellen, dass die Id_token nicht abgelaufen ist.

Es gibt auch mehrere weitere Validierungen, die Sie des [OpenID verbinden Core Spezifikation](http://openid.net/specs/openid-connect-core-1_0.html)ausführlich beschrieben ausführen sollten.  Sie möchten möglicherweise auch zusätzliche Ansprüche, abhängig von Ihrem Szenario überprüfen. Einige allgemeinen Validierungen umfassen:

- Sicherstellen, dass die Benutzer/Organisation für die app registriert hat.
- Sicherstellen, dass der Benutzer ordnungsgemäße Autorisierung-Berechtigungen verfügt.
- Um sicherzustellen, dass eine bestimmte Stärke von Authentifizierung, wie z. B. Azure kombinierte Authentifizierung aufgetreten ist.

Weitere Informationen über die Ansprüche in einer Id_token finden Sie unter den [Azure AD B2C token Bezug](active-directory-b2c-reference-tokens.md).

Nachdem Sie die Id_token vollständig überprüft haben, können Sie eine Sitzung mit dem Benutzer beginnen und Claims in der Id_token zum Abrufen von Informationen über den Benutzer in Ihrer app verwenden. Diese Informationen kann für die Anzeige, Datensätze, Autorisierung usw. verwendet werden.

## <a name="get-a-token"></a>Ein Token abrufen
Ist, alle Ihre Web app-Anforderungen zu tun ist Richtlinien ausführen, können Sie die nächsten Abschnitten überspringen. Diese Abschnitte gelten nur für Web apps, die vornehmen müssen authentifiziert Anrufe an eine Web-API und auch durch Azure AD B2C geschützt sind.

Können Sie die Authorization_code, die Sie erworben einlösen (mithilfe von `response_type=code+id_token`) für ein Token auf die gewünschte Ressource durch Senden eines `POST` zum Anfordern der `/token` Endpunkt. Derzeit ist die einzige Ressource, der Sie, ein Token für anfordern können Ihre app eigenen Back-End-Web-API. Die Benennungskonvention für ein Token an sich selbst anfordert besteht darin, Ihre app Client-ID als Umfang verwenden:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parameter | Erforderlich? | Beschreibung |
| ----------------------- | ------------------------------- | --------------------- |
| p | Erforderlich | Die Richtlinie, die verwendet wurde, um die Autorisierungscode erwerben. Sie können nicht in dieser Anforderung eine andere Richtlinie verwenden. Beachten Sie, dass Sie für diesen Parameter der **Abfragezeichenfolge**, nicht in den POST-Text hinzufügen. |
| client_id | Erforderlich | Die Anwendung-ID, die Ihre app der [Azure-Portal](https://portal.azure.com/) zugewiesen. |
| grant_type | Erforderlich | Den Typ des erteilen, die sein muss `authorization_code` für die Autorisierung Code Fluss. |
| Bereich | Empfohlen | Ein Leerzeichen getrennte Liste mit Bereichen. Ein einzelnen Bereichswert zeigt an, um beide Azure AD über die Berechtigungen, die aufgefordert werden. Die `openid` Bereich eine Berechtigung Anmeldung des Benutzers und Abrufen von Daten über den Benutzer in Form von **Id_tokens**angezeigt. Hiermit können Sie Ihre app eigenen Back-End-Web-API Token abzurufen, die durch dieselbe Anwendung ID wie der Client dargestellt wird. Die `offline_access` Bereich zeigt an, dass Ihre app ein **Refresh_token** für langer Lebensdauer Zugriff auf Ressourcen benötigt werden. |
| Code | Erforderlich | Die Authorization_code, die Sie in die Ausrichtung des ersten Abschnitts des Ablaufs erworben haben. |
| redirect_uri | Erforderlich | Die Redirect_uri der Anwendung, in dem Sie die Authorization_code erhalten haben. |
| client_secret | Erforderlich | Die Anwendung geheim, die Sie in der [Azure-Portal](https://portal.azure.com/)generiert. Diese Anwendung geheim ist ein Element wichtige Sicherheit. Sie sollten sie sicher auf dem Server gespeichert. Sie sollten auch darauf achten zu diesen Client Schlüssel in regelmäßigen Abständen drehen. |

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
| access_token | Das signierten Token, JWT, das Sie angefordert. |
| Bereich | Bereichen, die das Token gültig ist, ist die zum Zwischenspeichern von Token zur späteren Verwendung verwendet werden können. |
| expires_in | Die Länge der Zeit, die die Access_token (in Sekunden) gültig ist. |
| refresh_token | Eine Refresh_token OAuth 2.0. Dieses Token können die app Erfassen von zusätzlichen Token nach das aktuelle Token läuft ab.  Refresh_tokens lange halten werden, und zum Zugriff auf Ressourcen für längere Zeit beibehalten verwendet werden können. Weitere Details finden Sie unter [B2C token verweisen](active-directory-b2c-reference-tokens.md). Beachten Sie, dass Sie den Bereich verwendet haben, müssen `offline_access` in die Autorisierung und die token Besprechungsanfragen, um eine Refresh_token zu erhalten. |

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

## <a name="use-the-token"></a>Verwenden Sie das token
Jetzt, da Sie bereits erfolgreich gekauft haben ein `access_token`, verwenden Sie das Token in Besprechungsanfragen in Ihrem Back-End-web-APIs durch Einschließen in der `Authorization` Kopfzeile:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Aktualisieren Sie das token
Die Id_tokens sind kurzlebige. Sie müssen diese aktualisiert, nachdem sie ablaufen, um den Vorgang fortzusetzen, wird der Zugriff auf Ressourcen. Sie können dazu Übermitteln einer anderen `POST` zum Anfordern der `/token` Endpunkt. Bieten Sie diesmal die `refresh_token` statt der `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parameter | Erforderlich | Beschreibung |
| ----------------------- | ------------------------------- | -------- |
| p | Erforderlich | Die Richtlinie, die zum Erfassen von der ursprünglichen Refresh_token verwendet wurde. Sie können nicht in dieser Anforderung eine andere Richtlinie verwenden. Beachten Sie, dass Sie für diesen Parameter der **Abfragezeichenfolge**, nicht in den POST-Text hinzufügen. |
| client_id | Erforderlich | Die Anwendung-ID, die Ihre app der [Azure-Portal](https://portal.azure.com/) zugewiesen. |
| grant_type | Erforderlich | Den Typ des erteilen, die sein muss `refresh_token` für dieses Abschnitts eines Autorisierung Code illustrieren. |
| Bereich | Empfohlen | Ein Leerzeichen getrennte Liste mit Bereichen. Ein einzelnen Bereichswert zeigt an, um beide Azure AD über die Berechtigungen, die aufgefordert werden. Die `openid` Bereich eine Berechtigung Anmeldung des Benutzers und Abrufen von Daten über den Benutzer in Form von **Id_tokens**angezeigt. Hiermit können Sie Ihre app eigenen Back-End-Web-API Token abzurufen, die durch dieselbe Anwendung ID wie der Client dargestellt wird. Die `offline_access` Bereich zeigt an, dass Ihre app ein **Refresh_token** für langer Lebensdauer Zugriff auf Ressourcen benötigt werden. |
| redirect_uri | Empfohlen | Die Redirect_uri der Anwendung, in dem Sie die Authorization_code erhalten haben. |
| refresh_token | Erforderlich | Die ursprüngliche Refresh_token, die Sie in der zweiten Ausrichtung des Abschnitts des Ablaufs erworben haben. Beachten Sie, dass Sie den Bereich verwendet haben, müssen `offline_access` in die Autorisierung und die token Besprechungsanfragen, um eine Refresh_token zu erhalten. |
| client_secret | Erforderlich | Die Anwendung geheim, die Sie in der [Azure-Portal](https://portal.azure.com/)generiert. Diese Anwendung geheim ist ein Element wichtige Sicherheit. Sie sollten sie sicher auf dem Server gespeichert. Sie sollten auch darauf achten zu diesen Client Schlüssel in regelmäßigen Abständen drehen. |

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
| access_token | Das signierten Token, JWT, das Sie angefordert. |
| Bereich | Bereichen, die das Token gültig ist, ist die zum Zwischenspeichern von Token zur späteren Verwendung verwendet werden können. |
| expires_in | Die Länge der Zeit, die die Access_token (in Sekunden) gültig ist. |
| refresh_token | Eine Refresh_token OAuth 2.0. Dieses Token können die app Erfassen von zusätzlichen Token nach das aktuelle Token läuft ab.  Refresh_tokens lange halten werden, und zum Zugriff auf Ressourcen für längere Zeit beibehalten verwendet werden können. Weitere Details finden Sie unter [B2C token verweisen](active-directory-b2c-reference-tokens.md). |

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


## <a name="send-a-sign-out-request"></a>Senden Sie eine Besprechungsanfrage Abmelde

Wenn Sie sich für den Benutzer bei der app abmelden möchten, ist es nicht ausreichend, deaktivieren Sie die Sitzung mit dem Benutzer Cookies oder andernfalls Ende Ihrer app. Sie müssen den Benutzer auch auf Azure AD Abmelden umleiten. Wenn Sie dies nicht tun, möglicherweise der Benutzer zu Ihrer Anwendung eine erneute Authentifizierung, ohne ihre Anmeldeinformationen erneut eingeben können. Dies ist, da sie eine gültige einzelne anmelden Sitzung mit Azure AD zur Verfügung steht.

Sie können den Benutzer einfach Umleiten der `end_session_endpoint` aufgeführt ist im selben OpenID verbinden Metadatendokument bereits im Abschnitt "Überprüfen der Id_token" beschrieben:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Erforderlich? | Beschreibung |
| ----------------------- | ------------------------------- | ------------ |
| p | Erforderlich | Die Richtlinie, die Sie sich den Benutzer von Ihrer Anwendung verwenden möchten. |
| post_logout_redirect_uri | Empfohlen | Die URL, die nach der Benutzer umgeleitet werden sollte erfolgreich abmelden. Wenn es nicht enthalten ist, wird der Benutzer eine allgemeine Meldung durch Azure AD B2C angezeigt.  |

> [AZURE.NOTE]
    Während Sie den Benutzer die `end_session_endpoint` werden einige der den Status des Benutzers einzelnen Anmelden mit Azure AD B2C löschen, wird den Benutzer von der Sitzung des Benutzers sozialen Identität Anbieter (IDP) nicht signieren. Wenn der Benutzer den gleichen IDP während nachfolgende anmelden markiert hat, werden er erneut authentifiziert werden ohne Eingabe ihrer Anmeldeinformationen. Wenn ein Benutzer Ihrer Anwendung B2C abzumelden möchte, bedeutet dies nicht zwangsläufig, dass die Benutzer ihre Facebook-Konto vollständig abmelden möchten. Allerdings werden bei lokalen Konten, die Sitzung des Benutzers ordnungsgemäß beendet werden.

## <a name="use-your-own-b2c-tenant"></a>Verwenden Sie Ihrer eigenen B2C-Mandanten

Wenn Sie diese Anfragen selbst ausprobieren möchten, müssen Sie zunächst diese drei Schritte ausführen, und klicken Sie dann die Beispielwerten über durch ein eigenes ersetzen:

- [Erstellen Sie einen Mandanten B2C](active-directory-b2c-get-started.md), und verwenden Sie den Namen Ihrer Mandanten in die Anforderungen.
- [Erstellen einer Anwendung](active-directory-b2c-app-registration.md) zur Personalnummer Anwendung und einer Redirect_uri zu erhalten. Wird enthalten sein sollen einer **web app/Web api** in Ihrer app und optional eine **Anwendung geheim**erstellen.
- [Erstellen Ihrer Richtlinien](active-directory-b2c-reference-policies.md) , um den Richtliniennamen Ihrer zu erhalten.
