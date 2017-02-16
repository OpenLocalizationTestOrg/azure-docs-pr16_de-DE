<properties
    pageTitle="Ändert sich in den Azure AD-Version 2.0-Endpunkt | Microsoft Azure"
    description="Eine Beschreibung der Änderungen, die an die app Modell Version 2.0 public Preview-Version Protokolle erfolgen."
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

# <a name="important-updates-to-the-v20-authentication-protocols"></a>Wichtige Updates die Protokolle der Version 2.0-Authentifizierung
Aufmerksamkeit Entwickler! In den nächsten zwei Wochen werden wir einige Updates für unsere Version 2.0 Authentifizierungsprotokolle Magazins werden, die bedeuten kann unterbrechen Änderungen für alle apps, die Sie während unserer Preview-Zeitraum geschrieben haben.  

## <a name="who-does-this-affect"></a>Wer wirkt sich dies?
Alle apps, die mit der Version 2.0 geschrieben wurden zusammengeführt Authentifizierung Endpunkt,

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

Weitere Informationen zu den Endpunkt Version 2.0 finden Sie [hier](active-directory-appmodel-v2-overview.md).

Wenn Sie eine app, die mit der Version 2.0-Endpunkt durch das Codieren von direkt an das Protokoll Version 2.0 erstellt, haben sollten unsere Web-Middlewares OpenID verbinden oder OAuth wird verhindert, oder verwenden andere 3rd Party Bibliotheken zur Durchführung der Authentifizierung, Sie vorbereiten Testen Ihre Projekte und bei Bedarf ändern.

## <a name="who-doesnt-this-affect"></a>Wer hat keine Auswirkung auf diese?
Alle apps, die für die Herstellung Azure AD-Authentifizierung Endpunkt geschrieben wurden,

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Dieses Protokoll unveränderlich, und es werden keine Änderungen auftreten.

Wenn Ihre app **nur** unserer ADAL Bibliothek zur Durchführung der Authentifizierung verwendet, müssen Sie darüber hinaus nichts ändern.  ADAL weist die Änderungen der app geschützter.  

## <a name="what-are-the-changes"></a>Was sind die Änderungen?
### <a name="removing-the-x5t-value-from-jwt-headers"></a>Entfernen den Wert x5t aus JWT Kopfzeilen
Der Version 2.0-Endpunkt verwendet JWT Token arbeiten, die Parameter Kopfzeilenbereich mit relevanten Metadaten über das Token enthalten.  Wenn Sie die Kopfzeile eines unserer aktuellen JWTs entschlüsseln, finden Sie ungefähr wie folgt:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Stelle, an der die "x5t" und "für Kinder"-Eigenschaft öffentlichen Schlüssel identifizieren möchten, die mit der Signatur des Token, überprüfen, wie aus den Metadaten-Endpunkt OpenID verbinden abgerufen werden sollen.

Die Änderung angibt, die wir hier herstellen ist so entfernen Sie die Eigenschaft "x5t".  Sie können weiterhin die gleichen Methoden verwenden Token zu bestätigen, aber sollten sich nur auf die Eigenschaft "für Kinder" zum Abrufen von des richtigen öffentlichen Schlüssels gemäß Angabe in das Protokoll OpenID verbinden verlassen. 

> [AZURE.IMPORTANT] **Ihre Aufgabe: sicherstellen, dass die app nicht das Vorhandensein des Werts x5t abhängig.**

### <a name="removing-profileinfo"></a>Entfernen von profile_info
In früheren Versionen der Version 2.0-Endpunkt weist wurde Zurückgeben eines base64-codierte JSON-Objekts in token Antworten aufgerufen `profile_info`.  Beim Anfordern von einer Access-Token aus den Endpunkt Version 2.0 durch eine Besprechungsanfrage zu senden:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Die Antwort sieht wie den folgenden JSON-Objekt aus:
```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Die `profile_info` enthaltenen Wertinformationen den Benutzer, der bei der app - deren Anzeigenamen, Vornamen, Nachnamen, e-Mail-Adresse, Bezeichner usw. angemeldet.  Hauptsächlich, die `profile_info` verwendet wurde, zum Zwischenspeichern von token und Zwecke anzeigen.

Wir werden nun Entfernen der `profile_info` Wert –, aber keine Sorge, wir sind weiterhin diese Informationen für Entwickler an einem Ort weicht bereitstellt.  Anstelle von `profile_info`, der Endpunkt Version 2.0 zurückgegeben werden jetzt kann eine `id_token` in jeder token Antwort:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Wählen Sie entschlüsseln und Analysieren der Id_token, um die gleichen Informationen abzurufen, die Sie vom Profile_info erhalten haben.  Die Id_token ist eine JSON Web Token (JWT), mit Inhalt gemäß OpenID verbinden.  Der Code für Dadurch sollten daher sehr ähnlich sein – müssen Sie einfach im mittlere Segment (Text) der Id_token extrahieren und base64 entschlüsseln sie Zugriff auf das JSON-Objekt in.

In den nächsten zwei Wochen, sollten Sie Ihre app, um die Benutzerinformationen abzurufen, klicken Sie entweder code der `id_token` oder `profile_info`; Je nachdem, was vorhanden ist.  Auf diese Weise, wenn die Änderung vorgenommen wird, Ihre app kann den Übergang von nahtlos verarbeitet `profile_info` auf `id_token` ohne Unterbrechung.

> [AZURE.IMPORTANT] **Ihre Aufgabe: sicherstellen, dass Ihre app abhängig nicht auf das Vorhandensein der `profile_info` Wert.**

### <a name="removing-idtokenexpiresin"></a>Entfernen von id_token_expires_in
Ähnlich wie `profile_info`, wir sind auch Entfernen der `id_token_expires_in` Parameter aus Antworten.  Der Version 2.0-Endpunkt liefern zuvor, einen Wert für `id_token_expires_in` zusammen mit jeder Antwort Id_token, beispielsweise in einer Antwort autorisieren:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Oder in einer token Antwort:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Die `id_token_expires_in` bewertet die Anzahl der Sekunden der Id_token würde für gültig bleiben.  Wir werden nun Entfernen der `id_token_expires_in` vollständig Wert.  Sie können stattdessen den standardmäßigen OpenID verbinden `nbf` und `exp` tatsächlich um die Gültigkeit einer Id_token untersuchen.  Finden Sie die [Version 2.0 token Bezug](active-directory-v2-tokens.md) für Weitere Informationen auf diese Ansprüche aus.

> [AZURE.IMPORTANT] **Ihre Aufgabe: sicherstellen, dass Ihre app abhängig nicht auf das Vorhandensein der `id_token_expires_in` Wert.**


### <a name="changing-the-claims-returned-by-scopeopenid"></a>Ändern der Ansprüche zurückgegebene Umfang = Openid
Diese Änderung wird der wichtigste – wirklich, er wirkt sich auf fast jede app, die den Endpunkt Version 2.0 verwendet.  Viele Clientanwendungen senden Anfragen der Version 2.0-Endpunkt mithilfe der `openid` Bereich, wie:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Heute, wenn der Benutzer Zustimmung für gewährt die `openid` Bereich, der app empfängt eine Vielzahl von Informationen über den Benutzer in die resultierende Id_token.  Diese Ansprüche können seinen Namen, bevorzugte Benutzernamen, e-Mail-Adresse, Objekt-ID und mehr enthalten.

In diesem Update, wir die Informationen ändern, die die `openid` Umfang entspricht Ihren app-Zugriff, um eine bessere Comform mit der Spezifikation OpenID verbinden.  Die `openid` Umfang wird nur Ihre app sich den Benutzer zulassen, und erhalten Sie einen app-spezifischen Bezeichner für den Benutzer in der `sub` von der Id_token beanspruchen.  Die Ansprüche in einer Id_token, wobei nur die `openid` Umfang erteilt werden frei von persönlichen Informationen.  Beispiel für Id_token Ansprüche sind:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Wenn Sie personenbezogene Informationen (PII) über den Benutzer in Ihrer app abrufen möchten, müssen Ihre app zusätzliche Berechtigungen vom Benutzer anzufordern.  Wir Unterstützung für zwei neue Bereiche einführen, aus der Spezifikation OpenID verbinden – die `email` und `profile` Bereiche – mit denen Sie dies tun können.

- Die `email` Bereich ist sehr einfach – dies zulässt Ihren app-Zugriff auf primäre e-Mail-Adresse des Benutzers über die `email` in der Id_token beanspruchen.  Beachten Sie, dass die `email` anfordern wird nicht immer in vorhanden sein Id_tokens – es werden nur enthalten sein, falls vorhanden, in dem Profil des Benutzers.
- Die `profile` Umfang entspricht Ihren app-Zugriff auf andere grundlegende Informationen über den Benutzer – deren Namen, die bevorzugte Benutzernamen, Objekt-ID und vieles mehr.

So können Sie Ihre app in einer Weise minimale Offenlegung Fehlercode – bitten Sie den Benutzer für nur den Satz von Informationen, dass Ihre app, die für ihre Arbeit erforderlich ist.  Wenn Sie erste sämtlicher Benutzerinformationen, die aktuell Ihre app erhält fortsetzen möchten, sollten Sie alle drei Bereiche in Ihre Autorisierungsanfragen einbeziehen:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Die app beginnen kann, senden die `email` und `profile` Bereiche sofort und den Endpunkt des Version 2.0 akzeptiert diese beiden Bereiche und Anfordern von Berechtigungen von Benutzern nach Bedarf zu beginnen.  Jedoch die Änderung bei der Interpretation von der `openid` Umfang wird erst wirksam ein paar Wochen.

> [AZURE.IMPORTANT] **Ihre Aufgabe: Hinzufügen der `profile` und `email` Bereiche, wenn Ihre app Informationen über den Benutzer erforderlich sind.**  Beachten Sie, dass ADAL beide Berechtigungen in Besprechungsanfragen standardmäßig enthalten sein sollen. 

### <a name="removing-the-issuer-trailing-slash"></a>Entfernen der Herausgeber nachgestellten Schrägstrich.
In früheren Versionen Erstellen der Herausgeber-Wert, der in Token aus dem Endpunkt Version 2.0 wird das Formular

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

Stelle, an der die Guid der TenantId von den Azure AD-Mandanten wurde die das Token ausgestellt.  Mit diesen Änderungen wird der Wert des Herausgebers

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

in den beiden Token und im Discovery-Dokument OpenID verbinden.

> [AZURE.IMPORTANT] **Ihre Aufgabe: sicherstellen, dass Ihre app akzeptiert den Wert Herausgeber mit und ohne einen nachstehenden Schrägstrich während der Validierung des Herausgebers.**

## <a name="why-change"></a>Warum ändern?
Der primäre Hintergrund für die Einführung in die betreffenden Änderungen ist mit der herstellen OpenID standard Specification kompatibel sein.  OpenID verbinden kompatibel wird, wir hoffen, um die Unterschiede zwischen der Integration mit Microsoft Identität-Diensten und mit anderen Identitätsdiensten in der Branche zu minimieren.  Wir möchten Entwickler zu ihrer bevorzugten open-Source-Authentifizierung Bibliotheken verwenden Microsoft Unterschiede gerecht werden kann, ohne dass so ändern Sie die Bibliotheken aktivieren.

## <a name="what-can-you-do"></a>Was können Sie tun?
Aufweist können Sie beginnen, wodurch alle über die oben beschriebenen Änderungen.  Sie sollten sofort:

1.  **Aufheben von Abhängigkeiten an die `x5t` Kopfzeilenparameter.**
2.  **Ordnungsgemäß behandelt werden, den Übergang von `profile_info` zu `id_token` in token Antworten.**
3.  **Aufheben von Abhängigkeiten an die `id_token_expires_in` Antwort Parameter.**
3.  **Hinzufügen der `profile` und `email` Bereiche zu Ihrer Anwendung, wenn Ihre app grundlegende Benutzerinformationen benötigt.**
4.  **Herausgeber Werte in Token mit und ohne einen nachstehenden Schrägstrich zu übernehmen.**

Unsere [Version 2.0-Protokoll Dokumentation](active-directory-v2-protocols.md) wurde bereits aktualisiert, um diese Änderungen widerspiegeln, sodass Sie es als Bezug helfen, aktualisieren den Code verwenden können.

Wenn Sie auf den Bereich der Änderungen Fragen haben, senden Sie uns erreichen uns auf Twitter unter @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Wie häufig werden Protokoll Änderungen ausgeführt?
Wir nicht erwarten, dass alle mit der Bruchfestigkeit in die Authentifizierungsprotokolle verwandelt hat.  Wir werden diese Änderungen in einem Release absichtlich bündeln, damit Sie nicht diese Art von Aktualisierungsvorgang erneut aus einem beliebigen Zeitpunkt bald durchgehen müssen.  Natürlich weiterhin wir die Features der Authentifizierungsdienst zusammengeführt Version 2.0 hinzufügen, die Sie nutzen können, aber diese Änderungen sollten addiert, und nicht die vorhandenen Code Seitenumbruch.

Schließlich möchten wir Dank für Dinge ausprobieren, während des Preview-Zeitraums angenommen.  Die Einsichten und die Verwendung des unsere frühen Anwender wurden besonderem bisher, und wir hoffen, dass Sie Ihre Meinung und Ideen weiterhin werden.

Herzlichen Glückwunsch zum Codieren!

Die Division von Microsoft-Identität
