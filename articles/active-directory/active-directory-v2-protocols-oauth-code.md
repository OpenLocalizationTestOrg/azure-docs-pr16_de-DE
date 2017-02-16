

<properties
    pageTitle="Azure AD-Version 2.0 OAuth Autorisierung Fehlercode Fluss | Microsoft Azure"
    description="Gebäude Webanwendungen Azure AD-Implementierung von OAuth 2.0-Authentication-Protokoll verwenden."
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
    ms.date="08/08/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-authorization-code-flow"></a>Version 2.0 Protokolle - OAuth 2.0 Autorisierung Code Fluss

In apps, die auf einem Gerät für den Zugriff auf geschützte Ressourcen, z. B. Web APIs installiert werden, kann die OAuth 2.0 Autorisierung Code erteilen verwendet werden.  Verwenden die app Modell Version 2.0 Implementierung von OAuth 2.0, können Sie die an- und API den Zugriff auf Ihre mobile als auch desktop-apps hinzufügen.  Dieses Handbuch Sprache unabhängig ist, und beschreibt, wie Sie senden und Empfangen von HTTP-Nachrichten ohne unsere öffnen Source-Bibliotheken.

<!-- TODO: Need link to libraries -->

> [AZURE.NOTE]
    Nicht alle Azure Active Directory-Szenarien und Features werden von den Endpunkt Version 2.0 unterstützt.  Um festzustellen, ob den Version 2.0-Endpunkt verwendet werden sollen, erfahren Sie, [Version 2.0 Einschränkungen](active-directory-v2-limitations.md).

Illustrieren OAuth 2.0 Autorisierung Code wird im Abschnitt [4.1 der OAuth 2.0-Spezifikation](http://tools.ietf.org/html/rfc6749)beschrieben.  Hiermit wird in den meisten app Dateitypen, einschließlich [Web apps](active-directory-v2-flows.md#web-apps) und [systembedingt installierten apps](active-directory-v2-flows.md#mobile-and-native-apps)Authentifizierung und Autorisierung durchzuführen.  Sie können apps sicher Access_tokens Erfassen der Zugriff auf Ressourcen, die geschützt sind, verwenden den Endpunkt Version 2.0 verwendet werden kann.  

## <a name="protocol-diagram"></a>Protokoll-Diagramm
Auf hoher Ebene sieht illustrieren gesamte Authentifizierung für eine Anwendung einheitlichen/Mobile etwas wie folgt aus:

![OAuth autorisierende Code Fluss](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="request-an-authorization-code"></a>Anfordern eines Genehmigung Codes
Autorisierung Code illustrieren beginnt mit dem Client, leiten den Benutzer, zu der `/authorize` Endpunkt.  Der Client in dieser Anforderung zeigt an, dass die Berechtigungen, die es vom Benutzer abrufen muss:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [AZURE.TIP] Klicken Sie auf den Link unten, um diese Anforderung ausführen! Nach der Anmeldung, sollte in Ihrem Browser umgeleitet werden `https://localhost/myapp/` mit einem `code` in der Adressleiste.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://Login.microsoftonline.com/Common/oauth2/v2.0/Authorize...</a>

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| Mandanten | Erforderlich | Die `{tenant}` Wert in den Pfad der Anforderung zum Steuern, wer bei der Anwendung anmelden kann verwendet werden kann.  Die zulässigen Werte sind `common`, `organizations`, `consumers`, und Mandanten Bezeichnern.  Weitere Details finden Sie unter [Grundlagen Protokoll](active-directory-v2-protocols.md#endpoints). |
| client_id | Erforderlich | Die Anwendung-Id, dass das Registrierung-Portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihre app zugewiesen. |
| response_type | Erforderlich | Darf enthalten `code` für die Autorisierung Code Fluss. |
| redirect_uri | empfohlen | Die Redirect_uri der app, wo Authentifizierungsantworten gesendet und Empfangen von Ihrer app werden können.  Es muss exakt eine der Redirect_uris übereinstimmen, die Sie im Portal registriert, außer es Url codiert werden muss.  Für systemeigene und mobile-apps, sollten Sie des Standardwerts für `https://login.microsoftonline.com/common/oauth2/nativeclient`. |
| Bereich | Erforderlich | Ein Leerzeichen getrennte Liste von [Bereichen](active-directory-v2-scopes.md) , die Sie den Benutzer auf Zustimmung möchten.  |
| response_mode | empfohlen | Gibt die Methode, die verwendet werden soll, das sich daraus ergebende token zurück zu Ihrer Anwendung zu senden.  Kann sein `query` oder `form_post`.  |
| Bundesstaat | empfohlen | Einen Wert enthalten, in der Besprechungsanfrage, die auch in der token Antwort zurückgegeben wird.  Es kann eine Textzeichenfolge alle Inhalte, die Sie möchten.  Ein eindeutiger erzeugten Wert wird in der Regel für die [websiteübergreifende Anforderungsfälschungsangriffe verhindern](http://tools.ietf.org/html/rfc6749#section-10.12)verwendet.  Der Status wird auch Informationen zu den Status des Benutzers in der app codieren, bevor die Authentifizierungsanfrage ist, beispielsweise die Seite oder die Ansicht, die sie aufgetreten auf Waren, verwendet. |
| Aufforderung | Optional | Gibt den Typ der Interaktion mit dem Benutzer, die erforderlich ist.  Die einzige gültige Werte zu diesem Zeitpunkt sind 'Anmeldung', 'keine' und 'Zustimmung'.  `prompt=login`Erzwingt den Benutzer zur Eingabe ihrer Anmeldeinformationen auf die Anfrage Inverser_Operator einmaligen Anmeldung.  `prompt=none`ist die Umkehrung – es stellt sicher, dass der Benutzer jede interaktive Aufforderung überhaupt nicht angezeigt werden.  Wenn die Anfrage über einmaligen Anmeldung im Hintergrund ausgeführt werden kann, wird der Version 2.0-Endpunkt einen Fehler zurück.  `prompt=consent`Nachdem sich der Benutzer den Benutzer auffordert signiert, erteilen, um die app, wird das Dialogfeld OAuth Zustimmung ausgelöst. |
| login_hint | Optional | Kann verwendet werden um vorab füllen Sie das Feld Benutzername/e-Mail-Adresse von der Anmeldeseite für den Benutzer, wenn Sie ihren Benutzernamen im Voraus kennen.  Häufig apps werden für diesen Parameter verwenden, während eine erneute Authentifizierung, haben Sie bereits den Benutzernamen aus einem vorherigen Anmeldung mit extrahiert die `preferred_username` beanspruchen. |
| domain_hint | Optional | Kann eine der `consumers` oder `organizations`.  Wenn enthalten, überspringen Sie den e-Mail-basierte Erkennungsvorgang diesen Benutzer durchläuft, klicken Sie auf der Seite Anmelden Version 2.0 führenden zu einer etwas mehr optimierten Benutzeroberfläche.  Häufig apps werden während der erneuten Authentifizierung für diesen Parameter verwenden, indem Sie Extrahieren der `tid` aus einem vorherigen anmelden.  Wenn die `tid` beanspruchen Wert ist `9188040d-6c67-4c5b-b112-36a304b66dad`, sollten Sie `domain_hint=consumers`.  Verwenden Sie andernfalls `domain_hint=organizations`. |

An diesem Punkt werden der Benutzer aufgefordert, geben Sie ihre Anmeldeinformationen ein, und führen Sie die Authentifizierung.  Der Version 2.0-Endpunkt wird außerdem sichergestellt, dass der Benutzer die Berechtigungen, die im angegebenen zugestimmt hat die `scope` Abfrage Parameter.  Wenn der Benutzer eine dieser Berechtigungen nicht zugestimmt hat, wird der Benutzer zu den erforderlichen Berechtigungen Zustimmung gefragt werden.  Details zu [Berechtigungen, Zustimmung, und mit mehreren Mandanten apps dienen hier](active-directory-v2-scopes.md).

Nachdem der Benutzer authentifiziert und Zustimmung gewährt, Version 2.0-Endpunkt zurückgegeben werden kann eine Antwort auf Ihre app auf die angegebene `redirect_uri`, mithilfe der angegebenen Methode der `response_mode` Parameter.

#### <a name="successful-response"></a>Erfolgreiche Antwort
Eine erfolgreiche Antwort mit `response_mode=query` sieht wie folgt aus:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Code | Die Authorization_code, die die app angefordert. Den Autorisierungscode können die app in einer Access-Token für die Zielressource anfordern.  Authorization_codes sind sehr kurzlebige, in der Regel sie ablaufen nach etwa 10 Minuten. |
| Bundesstaat | Wenn der Parameter Status in der Besprechungsanfrage enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die app sollte überprüfen, ob die Statuswerte in die Anforderung und Antwort identisch sind. |

#### <a name="error-response"></a>Antwort zurück
Fehler beim Antworten möglicherweise auch gesendet werden, mit der `redirect_uri` , damit die app diese angemessen behandelt werden kann:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Zeichenfolge des Fehlercodes, die zum Fehlertypen klassifizieren, die auftreten verwendet werden kann, und kann verwendet werden, um auf Fehler zu reagieren. |
| error_description | Eine bestimmte Fehlermeldung, die einen Entwickler die Ursache eines Authentifizierungsfehlers ermitteln helfen können.  |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Fehlercodes Autorisierung Endpunkt Fehler

Die folgende Tabelle beschreibt die verschiedenen Fehlercodes, der in zurückgegeben werden, können die `error` Parameter der Antwort zurück.

| Fehlercode | Beschreibung | Clientaktion |
|------------|-------------|---------------|
| invalid_request | Protokoll Fehler aufgetreten, beispielsweise ein fehlender erforderlicher Parameter. | Beheben, und senden Sie die Anfrage erneut. Dies ist eine Entwicklung Fehler in der Regel während der anfänglichen Testen abgefangen wird.|
| unauthorized_client | Die Clientanwendung ist nicht zulässig, um einen Autorisierungscode anzufordern. | Das Problem tritt gewöhnlich auf, wenn die Clientanwendung in Azure AD nicht registriert ist oder nicht auf des Benutzers Azure AD-Mandanten hinzugefügt wird. Die Anwendung kann den Benutzer mit für das Installieren der Anwendung und das Hinzufügen zur Azure AD auffordern. |
| ACCESS_DENIED | Ressourcenbesitzer Zustimmung verweigert | Die Clientanwendung kann den Benutzer benachrichtigen, den fortgesetzt werden kann, wenn der Benutzer zulässt. |
| unsupported_response_type | Der Autorisierung Server unterstützt nicht den Antworttyp in der Besprechungsanfrage. | Beheben, und senden Sie die Anfrage erneut. Dies ist eine Entwicklung Fehler in der Regel während der anfänglichen Testen abgefangen wird.|
|server_error | Der Server ist einen unerwarteten Fehler aufgetreten. | Wiederholen Sie die Anforderung. Dieser Fehler können temporäre Bedingungen führen. Die Clientanwendung möglicherweise für den Benutzer erläutern, dass seine Antwort Fälligkeitsdatum ein temporärer Fehler verzögert ist. |
| temporarily_unavailable | Der Server ist vorübergehend ausgelastet und kann die Anfrage zu behandeln. | Wiederholen Sie die Anforderung. Die Clientanwendung möglicherweise für den Benutzer erläutern, dass seine Antwort Fälligkeitsdatum eine temporäre Bedingung verzögert ist. |
| invalid_resource |Die Zielressource ist ungültig, da es ist nicht vorhanden, Azure AD nicht werden gefunden kann oder nicht richtig konfiguriert.| Dies zeigt an, dass die Ressource, sofern vorhanden, nicht in den Mandanten konfiguriert wurde. Die Anwendung kann den Benutzer mit für das Installieren der Anwendung und das Hinzufügen zur Azure AD auffordern. |

## <a name="request-an-access-token"></a>Anfordern einer Access-token
Jetzt, Sie haben eine Authorization_code erworben haben und Zugriff durch den Benutzer gewährt wurde, können Sie einlösen die `code` für eine `access_token` auf die gewünschte Ressource, durch Senden einer `POST` zum Anfordern der `/token` Endpunkt:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Probieren Sie diese Anforderung in Postman ausführt! (Vergessen Sie nicht, ersetzen die `code`)     [ ![In Postman ausführen](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------------- |
| Mandanten | Erforderlich | Die `{tenant}` Wert in den Pfad der Anforderung zum Steuern, wer bei der Anwendung anmelden kann verwendet werden kann.  Die zulässigen Werte sind `common`, `organizations`, `consumers`, und Mandanten Bezeichnern.  Weitere Details finden Sie unter [Grundlagen Protokoll](active-directory-v2-protocols.md#endpoints). |
| client_id | Erforderlich | Die Anwendung-Id, dass das Registrierung-Portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihre app zugewiesen. |
| grant_type | Erforderlich | Muss `authorization_code` für die Autorisierung Code Fluss. |
| Bereich | Erforderlich | Ein Leerzeichen getrennte Liste mit Bereichen.  Die Bereiche in diesem Abschnitt angefordert muss entspricht dem oder eine Teilmenge der Bereiche in der Ausrichtung des ersten Abschnitts angefordert.  Wenn in dieser Anforderung angegebenen Bereiche mehrere Ressourcenserver umfassen, gibt der Endpunkt Version 2.0 ein Token für die Ressource angegebenen in den ersten Bereich zurück.  Eine ausführlichere Erklärung der Bereiche finden Sie in den [Berechtigungen, Zustimmung, und Bereichen](active-directory-v2-scopes.md).  |
| Code | Erforderlich | Die Authorization_code, die Sie in die Ausrichtung des ersten Abschnitts des Ablaufs erworben haben.   |
| redirect_uri | Erforderlich | Die gleichen Redirect_uri-Wert, der zum Erfassen der Authorization_code verwendet wurde. |
| client_secret | für den Web apps erforderlich ist | Die Anwendung geheim, die Sie in der Registrierung app-Portal für Ihre app erstellt haben.  Es sollte nicht in einer systemeigenen app verwendet werden, da Client_secrets zuverlässig auf Geräten gespeichert werden kann.  Es ist erforderlich, damit Web apps und Web-APIs, die die Möglichkeit, die Client_secret sicher auf dem Server zu speichern. |

#### <a name="successful-response"></a>Erfolgreiche Antwort
Wie wird eine erfolgreiche token Antwort aus:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| access_token | Die angeforderten Zugriffstoken. Dieses Token können die app für die geschützte Ressourcen, z. B. ein Web-API authentifizieren. |
| token_type | Gibt den Wert des Sicherheitstokens an. Der einzige Typ, der Azure AD unterstützt ist Person  |
| expires_in | Wie lange das Access-Token (in Sekunden) gültig ist. |
| Bereich | Die Bereiche, denen die Access_token für gültig ist. |
| refresh_token |  Ein OAuth 2.0 aktualisieren Token. Die app können Sie dieses Token Erfassen von zusätzlichen Zugriffstoken nach das aktuellen Access Token läuft ab.  Refresh_tokens langer Lebensdauer sind, und können verwendet werden, um den Zugriff auf Ressourcen für längere Zeit beibehalten.  Weitere Details finden Sie unter [Version 2.0 token Bezug](active-directory-v2-tokens.md).  |
| id_token | Eine nicht signierte JSON Web Token (JWT). Die app kann base64Url entschlüsseln die Segmente des dieses Token das Anfordern von Informationen über den Benutzer, die sich angemeldet haben. Die app können Sie die Werte im cache und anzeigen, aber es dürfen nicht auf diese verlassen, Autorisierung oder Begrenzung Sicherheit.  Weitere Informationen zu Id_tokens finden Sie unter [Version 2.0 Endpunkt token verweisen](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Antwort zurück
Fehler beim Antworten sieht wie aus:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Zeichenfolge des Fehlercodes, die zum Fehlertypen klassifizieren, die auftreten verwendet werden kann, und kann verwendet werden, um auf Fehler zu reagieren. |
| error_description | Eine bestimmte Fehlermeldung, die einen Entwickler die Ursache eines Authentifizierungsfehlers ermitteln helfen können.  |
| error_codes | Eine Liste der STS bestimmte Fehlercodes, die bei der Diagnose helfen können.  |
| Zeitstempel | Die Uhrzeit, an der der Fehler aufgetreten ist. |
| _id | Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose helfen können.  |
| correlation_id | Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose über Komponenten helfen können. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Fehlercodes token Endpunkt Fehler

| Fehlercode | Beschreibung | Clientaktion |
|------------|-------------|---------------|
| invalid_request | Protokoll Fehler aufgetreten, beispielsweise ein fehlender erforderlicher Parameter. | Beheben, und übermitteln Sie die Anforderung erneut. |
| invalid_grant | Der Autorisierungscode ist ungültig oder ist abgelaufen. | Versuchen Sie eine neue Anforderung an das `/authorize` Endpunkt |
| unauthorized_client | Der authentifizierte Client ist nicht autorisiert, um diesen Autorisierung erteilen verwenden. | Das Problem tritt gewöhnlich auf, wenn die Clientanwendung in Azure AD nicht registriert ist oder nicht auf des Benutzers Azure AD-Mandanten hinzugefügt wird. Die Anwendung kann den Benutzer mit für das Installieren der Anwendung und das Hinzufügen zur Azure AD auffordern. |
| invalid_client | Fehler bei Clientauthentifizierung. | Die Clientanmeldeinformationen sind ungültig. Zum beheben, aktualisiert der Anwendungsadministrator die Anmeldeinformationen. |
| unsupported_grant_type | Der Autorisierung Server unterstützt nicht den Typ der Autorisierung erteilen. | Ändern Sie den erteilen Typ in der Besprechungsanfrage. Diese Art von Fehler sollte nur bei der Entwicklung auftreten und während der anfänglichen Testen erkannt. |
| invalid_resource | Die Zielressource ist ungültig, da es ist nicht vorhanden, Azure AD nicht werden gefunden kann oder nicht richtig konfiguriert. | Dies zeigt an, dass die Ressource, sofern vorhanden, nicht in den Mandanten konfiguriert wurde. Die Anwendung kann den Benutzer mit für das Installieren der Anwendung und das Hinzufügen zur Azure AD auffordern. |
| interaction_required | Die Anforderung erfordert eine Benutzeraktion. Beispielsweise ist ein Schritt zusätzliche Authentifizierung erforderlich. | Wiederholen Sie die Anforderung mit dem gleichen Ressourcen. |
| temporarily_unavailable | Der Server ist vorübergehend ausgelastet und kann die Anfrage zu behandeln. | Wiederholen Sie die Anforderung. Die Clientanwendung möglicherweise für den Benutzer erläutern, dass seine Antwort Fälligkeitsdatum eine temporäre Bedingung verzögert ist.|

## <a name="use-the-access-token"></a>Verwenden Sie das Access-token
Sie erfolgreich erworben haben, haben Sie nun eine `access_token`, können Sie das Token in Anfragen auf Web-APIs durch Einschließen in der `Authorization` Kopfzeile:

> [AZURE.TIP] Führen Sie diese Anforderung in Postman! (Ersetzen die `Authorization` Kopfzeile erste)     [ ![In Postman ausführen](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Aktualisieren Sie das Access-token
Access_tokens sind kurze halten, und Sie müssen diese aktualisiert, nachdem sie ablaufen, um den Vorgang fortzusetzen, den Zugriff auf Ressourcen.  Dazu Übermitteln einer anderen `POST` zum Anfordern der `/token` Endpunkt, bietet dieses Mal der `refresh_token` statt der `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Probieren Sie diese Anforderung in Postman ausführt! (Vergessen Sie nicht, ersetzen die `refresh_token`)     [ ![In Postman ausführen](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | -------- |
| Mandanten | Erforderlich | Die `{tenant}` Wert in den Pfad der Anforderung zum Steuern, wer bei der Anwendung anmelden kann verwendet werden kann.  Die zulässigen Werte sind `common`, `organizations`, `consumers`, und Mandanten Bezeichnern.  Weitere Details finden Sie unter [Grundlagen Protokoll](active-directory-v2-protocols.md#endpoints). |
| client_id | Erforderlich | Die Anwendung-Id, dass das Registrierung-Portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihre app zugewiesen. |
| grant_type | Erforderlich | Muss `refresh_token` für dieses Abschnitts eines Autorisierung Code illustrieren. |
| Bereich | Erforderlich | Ein Leerzeichen getrennte Liste mit Bereichen.  Die Bereiche in diesem Abschnitt angefordert muss entspricht dem oder eine Teilmenge der Bereiche in die Ausrichtung des ursprünglichen Authorization_code Anforderung Abschnitts angefordert.  Wenn in dieser Anforderung angegebenen Bereiche mehrere Ressourcenserver umfassen, gibt der Endpunkt Version 2.0 ein Token für die Ressource angegebenen in den ersten Bereich zurück.  Eine ausführlichere Erklärung der Bereiche finden Sie in den [Berechtigungen, Zustimmung, und Bereichen](active-directory-v2-scopes.md).  |
| refresh_token | Erforderlich | Die Refresh_token, die Sie in der zweiten Ausrichtung des Abschnitts des Ablaufs erworben haben.   |
| redirect_uri | Erforderlich | Die gleichen Redirect_uri-Wert, der zum Erfassen der Authorization_code verwendet wurde. |
| client_secret | für den Web apps erforderlich ist | Die Anwendung geheim, die Sie in der Registrierung app-Portal für Ihre app erstellt haben.  Es sollte nicht in einer systemeigenen app verwendet werden, da Client_secrets zuverlässig auf Geräten gespeichert werden kann.  Es ist erforderlich, damit Web apps und Web-APIs, die die Möglichkeit, die Client_secret sicher auf dem Server zu speichern. |

#### <a name="successful-response"></a>Erfolgreiche Antwort
Wie wird eine erfolgreiche token Antwort aus:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| access_token | Die angeforderten Zugriffstoken. Dieses Token können die app für die geschützte Ressourcen, z. B. ein Web-API authentifizieren. |
| token_type | Gibt den Wert des Sicherheitstokens an. Der einzige Typ, der Azure AD unterstützt ist Person  |
| expires_in | Wie lange das Access-Token (in Sekunden) gültig ist. |
| Bereich | Die Bereiche, denen die Access_token für gültig ist. |
| refresh_token |  Ein neues OAuth 2.0 aktualisieren Token. Ersetzen Sie das alte aktualisieren Token mit diesem Token neu erworbenen aktualisieren, um sicherzustellen, dass Ihre Token aktualisieren gültiger so lange bleiben.  |
| id_token | Eine nicht signierte JSON Web Token (JWT). Die app kann base64Url entschlüsseln die Segmente des dieses Token das Anfordern von Informationen über den Benutzer, die sich angemeldet haben. Die app können Sie die Werte im cache und anzeigen, aber es dürfen nicht auf diese verlassen, Autorisierung oder Begrenzung Sicherheit.  Weitere Informationen zu Id_tokens finden Sie unter [Version 2.0 Endpunkt token verweisen](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Antwort zurück
```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Zeichenfolge des Fehlercodes, die zum Fehlertypen klassifizieren, die auftreten verwendet werden kann, und kann verwendet werden, um auf Fehler zu reagieren. |
| error_description | Eine bestimmte Fehlermeldung, die einen Entwickler die Ursache eines Authentifizierungsfehlers ermitteln helfen können.  |
| error_codes | Eine Liste der STS bestimmte Fehlercodes, die bei der Diagnose helfen können.  |
| Zeitstempel | Die Uhrzeit, an der der Fehler aufgetreten ist. |
| _id | Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose helfen können.  |
| correlation_id | Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose über Komponenten helfen können. |

Eine Beschreibung der Fehlercodes und die Aktion empfohlene Client finden Sie unter [Fehlercodes token Endpunkt Fehler](#error-codes-for-token-endpoint-errors).
