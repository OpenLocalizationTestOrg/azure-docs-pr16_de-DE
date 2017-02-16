<properties
    pageTitle="Azure AD Version 2.0 token Verweis | Microsoft Azure"
    description="Die Arten von Token und Ansprüche ausgegeben vom Endpunkt Version 2.0"
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

# <a name="v20-token-reference"></a>Version 2.0 token Bezug

Der Endpunkt Version 2.0 gibt verschiedene Arten von Sicherheitstokens in die Verarbeitung von jedem [Authentifizierung Fluss](active-directory-v2-flows.md)aus. Dieses Dokument beschreibt das Format, Sicherheitseigenschaften und Inhalt der einzelnen Token.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory-Szenarien und Features werden von den Endpunkt Version 2.0 unterstützt.  Um festzustellen, ob den Version 2.0-Endpunkt verwendet werden sollen, erfahren Sie, [Version 2.0 Einschränkungen](active-directory-v2-limitations.md).

## <a name="types-of-tokens"></a>Typen von Token

Der Endpunkt Version 2.0 unterstützt das [OAuth 2.0 Autorisierungsprotokoll](active-directory-v2-protocols.md), welche sowohl Access_tokens und Refresh_tokens verwendet wird.  Außerdem unterstützt Authentifizierung und Anmeldung über [OpenID verbinden](active-directory-v2-protocols.md#openid-connect-sign-in-flow), einen dritten Typ von Token, die Id_token vorgestellt.  Jede dieser Token wird als "Person"Token dargestellt.

Eine Person Token ist ein einfaches Sicherheitstoken, den "Person" den Zugriff auf eine geschützte Ressource gewährt. In diesem Sinne ist die "Person" einer Partei, die das Token präsentieren kann. Obwohl eine Party zuerst mit Azure AD erhalten das Token Person authentifizieren muss, wenn die erforderlichen Schritte zum Token im Übertragung und Speicherung gesichert nicht geöffnet werden, können Sie abgefangen und durch einen dritten unbeabsichtigte verwendet werden. Während einige Sicherheitstokens eine integrierte Funktionalität für verhindern, dass unbefugten sie verwendet haben, werden Person Token verfügen nicht über dieses Verfahren und in einem sicheren Kanal wie z. B. Transport Layer Security (HTTPS) übertragen werden müssen. Bei der Übertragung einer Person Token als Klartext kann ein Mann-in der mittleren Angriffen verwendet werden durch bösartige dritte das Token erfassen und diese für einen nicht autorisierten Zugriff auf eine geschützte Ressource verwenden. Diese Sicherheitsprinzipien gelten beim Speichern oder Person Token zur späteren Verwendung zwischenspeichern. Immer Stellen Sie sicher, dass Ihre app übermittelt und Person Token auf sichere Weise speichert. Weitere Sicherheitsaspekte auf Person Token finden Sie unter [RFC 6750 Abschnitt 5](http://tools.ietf.org/html/rfc6750).

Viele der Token ausgestellt von den Endpunkt Version 2.0 werden als Json Web Token oder JWTs implementiert.  Eine JWT ist eine komprimieren, URL-sichere Mittel Übertragung von Daten zwischen zwei Seiten.  In JWTs enthaltene Informationen werden als "Ansprüche" oder Assertionen von Informationen über die Person und den gewünschten Betreff für die Token bezeichnet.  Die Ansprüche im JWTs sind JSON-Objekte codierte und für die Übertragung serialisiert.  Da der JWTs ausgestellt von den Endpunkt Version 2.0 angemeldet sind, aber nicht verschlüsselt, können Sie problemlos des Inhalts einer JWT für das Debuggen überprüfen. Weitere Informationen zum JWTs können Sie auf der [JWT Spezifikation](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)verweisen.

## <a name="idtokens"></a>Id_tokens

Id_tokens sind eine Form der Anmeldung Sicherheitstoken, die Ihre app erhält, bei der Durchführung der Authentifizierung mit [OpenID verbinden](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  Sie werden als [JWTs](#types-of-tokens)dargestellt und enthalten ist, die Sie verwenden können, für den Benutzer in Ihrer app signieren.  Sie können die Ansprüche in einer Id_token verwenden, wie gewünscht – häufig zum werden Anzeigen von Kontoinformationen oder Entscheidung Access Steuerelement in einer app.  Der Endpunkt Version 2.0 Probleme mit nur einem Typ von Id_token, die einen konsistenten Satz von Ansprüchen unabhängig vom Typ des Benutzers hat, die angemeldet hat.  Dies ist das Format und den Inhalt der Id_tokens dasselbe gilt für beide persönliche Microsoft Account Benutzer und geschäftlichen oder schulnotizbücher Konten werden können, die besagen.

Id_tokens sind angemeldet, aber nicht zu diesem Zeitpunkt verschlüsselt.  Wenn Ihre app ein Id_token empfängt, müssen sie [die Signatur überprüfen](#validating-tokens) , um das Token des Echtheit beweisen und ein paar Ansprüche im Token um nachzuweisen seine Gültigkeit überprüfen.  Überprüft, indem Sie eine app Claims variieren je nach Szenario Anforderungen, aber es gibt einige [Allgemeine anfordern Validierungen](#validating-tokens) , die Ihre app in jeder Szenario ausgeführt werden muss.

Vollständige Details auf die Ansprüche im Id_tokens sind, sowie eine Stichprobe Id_token unten.  Beachten Sie, dass die Ansprüche im Id_tokens nicht in keiner bestimmten Reihenfolge zurückgegeben werden.  Darüber hinaus neue Ansprüche können in Id_tokens an einer beliebigen Stelle in der Zeit eingeführt werden – Ihre app sollte nicht unterbrechen, als neue Ansprüche eingeführt werden.  Die folgende Liste enthält die Ansprüche, die Ihre app zuverlässig zum Zeitpunkt der Erstellung dieses Dokuments interpretiert werden kann.  Gegebenenfalls kann sogar noch mehr Details in der [Spezifikation OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html)gefunden werden.

#### <a name="sample-idtoken"></a>Beispiel für id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [AZURE.TIP] Versuchen Sie zur wird empfohlen prüfen die Ansprüche in der Stichprobe Id_token durch [calebb.net](https://calebb.net)eingefügt.

#### <a name="claims-in-idtokens"></a>Ansprüche in id_tokens
| Namen | Anfordern | Beispiel für einen Wert | Beschreibung |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Zielgruppe | `aud` | `6731de76-14a6-49ae-97bc-6eba6914391e` | Gibt den Empfänger, der das Token.  In Id_tokens ist das Publikum Ihre app Id der Anwendung, wie Ihre app im app-Registrierung Portal zugewiesen.  Ihre app sollte dieser Wert überprüfen und das Token ablehnen, wenn es nicht übereinstimmt. |
| Herausgeber | `iss` | `https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` | Gibt den Security token Service (STS), der erstellt, und gibt das Token sowie den Azure AD-Mandanten, in dem der Benutzer authentifiziert wurde.  Ihre app sollte der Herausgeber anfordern, um sicherzustellen, dass das Token den Endpunkt Version 2.0 stammen überprüfen.  Sie sollten auch Guid-Anteil der Anspruch zum Festlegen des Mandanten einschränken, die berechtigt sind, melden Sie sich bei der app verwenden.  Die Guid, die den Benutzer angibt ist ein Consumer Benutzer von Microsoft-Konto ist `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| Ausgestellt am | `iat` | `1452285331` | Die Uhrzeit, an der das Token ausgestellt wurde, dargestellt im Epoche Zeit. |
| Ablaufzeit | `exp` | `1452289231` | Die Uhrzeit, zu der das Token ungültig ist, wird, in Epoche Zeit dargestellt ist.  Überprüfen Sie die Gültigkeit der token Lebensdauer sollte Ihre app dieser Anspruch verwenden.  |
| Nicht vor | `nbf` | `1452285331` |  Die Uhrzeit, zu der das Token gültig ist, wird, in Epoche Zeit dargestellt ist. Es ist normalerweise die Zeit Emission entspricht.  Überprüfen Sie die Gültigkeit der token Lebensdauer sollte Ihre app dieser Anspruch verwenden.  |
| Version | `ver` | `2.0` | Die Version von der Id_token, wie von Azure AD definiert.  Für den Endpunkt Version 2.0, kann ich der Wert `2.0`. |
| Mandanten-Id | `tid` | `b9419818-09af-49c2-b0c3-653adc1f376e` | Eine Guid, darstellt Azure AD-Mandanten also der Benutzer aus.  Für die geschäftlichen und schulnotizbücher-Konten werden die Guid der unveränderlich Mandanten-ID des Unternehmens, zu der der Benutzer gehört.  Für Persönliche Konten kann ich der Wert `9188040d-6c67-4c5b-b112-36a304b66dad`.  Die `profile` Bereich ist erforderlich, um diesen Anspruch zu erhalten. |
| Code Hash | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Der Code Hash ist im Id_tokens enthalten, nur, wenn die Id_token entlang einer OAuth 2.0 Autorisierungscode ausgegeben wird.  Hiermit können Sie die Echtheit eines Autorisierungscodes überprüfen.  Finden Sie in der [Spezifikation OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html) Weitere Details zur Durchführung dieser Überprüfung. |
| Access Token Hash | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Access token Hash ist im Id_tokens enthalten, nur, wenn die Id_token entlang einer OAuth 2.0 Access Token ausgestellt wurde.  Hiermit können Sie die Echtheit von einer Access-Token zu überprüfen.  Finden Sie in der [Spezifikation OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html) Weitere Details zur Durchführung dieser Überprüfung. |
| Nonce | `nonce` | `12345` | Die Nonce ist eine Strategie für das token Wiedergabe Angriffen zu minimieren.  Ihre app kann in einer Anforderung Autorisierung Nonce angeben, indem Sie mit der `nonce` Abfrage Parameter.  Der Wert, der Sie in der Besprechungsanfrage bereitstellen wird in der Id_token des ausgegeben werden `nonce` anfordern, unverändert.  Dadurch wird Ihre app überprüfen Sie, ob den Wert mit dem Wert der Anforderung angegebenen das des app Sitzung mit einer bestimmten Id_token verknüpft.  Ihre app sollte diese Validierung während der Validierung Id_token ausführen. |
| Namen | `name` | `Babe Ruth` | Der Anspruch bietet einen Menschen lesbaren Wert, der den gewünschten Betreff für die Token bezeichnet. Dieser Wert nicht unbedingt eindeutig sein, veränderbare ist, und nur zu Anzeigezwecken verwendet werden soll.  Die `profile` Bereich ist erforderlich, um diesen Anspruch zu erhalten. |
| E-Mail | `email` | `thegreatbambino@nyy.onmicrosoft.com` | Die primäre e-Mail-Adresse das Benutzerkonto zugeordnet ist, sofern eines vorhanden ist.  Der Wert ist veränderbare und möglicherweise über einen Zeitraum für einen bestimmten Benutzer ändern.  Die `email` Bereich ist erforderlich, um diesen Anspruch zu erhalten. |
| Bevorzugte Benutzername | `preferred_username` | `thegreatbambino@nyy.onmicrosoft.com` | Der primäre Benutzername, der zur Darstellung des Benutzers in der Version 2.0-Endpunkt verwendet wird.  Es könnte eine e-Mail-Adresse, Telefonnummer oder eine generische Benutzername ohne zu einem angegebenen Format.  Der Wert ist veränderbare und möglicherweise über einen Zeitraum für einen bestimmten Benutzer ändern.  Die `profile` Bereich ist erforderlich, um diesen Anspruch zu erhalten. |
| Betreff | `sub` | `MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | Die Tilgungsanteile darüber, die welche das Token Informationen, wie z. B. der Benutzer der app bestätigt. Dieser Wert ist unveränderlich und kann nicht zugewiesen werden oder wiederverwendet werden, damit sie verwendet werden kann Autorisierung Prüfungen ausführen sicheres, z. B., wenn das Token Zugriff auf eine Ressource verwendet wird. Da der Betreff immer in der Token die Probleme Azure AD-vorhanden ist, wird empfohlen, verwenden diesen Wert in einem allgemeinen Zweck Autorisierungssystem. |
| ObjectId | `oid` | `a1dbdde8-e4f9-4571-ad93-3059e3750d23` | Das Objekt-Id des Kontos geschäftlichen oder schulnotizbücher Azure AD-System.  Dieser Anspruch wird nur für persönlichen Microsoft-Konten erteilt werden.  Die `profile` Bereich ist erforderlich, um diesen Anspruch zu erhalten. |


## <a name="access-tokens"></a>Access-Token

Access-Token ausgestellt von den Endpunkt Version 2.0 sind nur zu diesem Zeitpunkt von Microsoft Services.  Führen Sie eine Überprüfung oder Prüfung von Access Token nach jedem der derzeit unterstützten Szenarien sollten nicht Ihrer apps müssen.  Access-Token kann als transparent behandelt: sie sind nur Zeichenfolgen, da diese Ihre app in HTTP-Anfragen an Microsoft übergeben kann.

In Kürze wird die Möglichkeit für Ihre app Access Token von anderen Clients erhalten der Endpunkt Version 2.0 vorstellen können.  Zu diesem Zeitpunkt werden diese Informationen mit den Informationen aktualisiert werden, Ihre app Access token Überprüfung und andere ähnlichen Aufgaben ausführen muss.

Wenn Sie eine Access-Token aus den Endpunkt Version 2.0 anfordern, gibt der Endpunkt Version 2.0 auch einige Metadaten über die Access-Token für Ihre app Ernährung.  Diese Informationen umfassen die Anzeigedauer Ablauf der Access-Token und die Bereiche, für die es gültig ist.  Dadurch wird Ihre app intelligente Zwischenspeichern von Access Token ohne Öffnen Analysieren ausführen das Access-Token selbst.

## <a name="refresh-tokens"></a>Aktualisieren von Token

Aktualisieren von Token Sicherheitstokens dem Ihre app neu erfassen können Zugriff auf Token in einen Fluss OAuth 2.0 sind.  Es kann Ihre app langfristiges Zugriff auf Ressourcen im Auftrag eines Benutzers ohne Interaktion vom Benutzer zu erzielen.

Aktualisieren Token werden mehrere Ressource an.  Das heißt werden, dass eine Aktualisierung Token empfangen während einer token Anforderung für eine Ressource kann für Access-Token zu einer vollständig anderen Ressource eingelöst.

Um eine Aktualisierung in einer token Antwort zu erhalten, Ihre app muss anfordern und gewährt werden die `offline_acesss` Bereich.   Weitere Informationen zu den `offline_access` Bereich, finden Sie [hier Zustimmung und Bereiche Artikel](active-directory-v2-scopes.md).

Aktualisieren von Token sind, und ist immer, zu Ihrer Anwendung vollständig undurchsichtig.  Sie können werden von den Azure AD-Version 2.0-Endpunkt ausgestellt und nur überprüft und von den Endpunkt Version 2.0 interpretiert werden.  Sie sind langer Lebensdauer, aber Ihre app nicht zu erwarten, dass ein Token aktualisieren für einen bestimmten Zeitraum dauert geschrieben werden sollen.  Aktualisieren Token können zu einem beliebigen Zeitpunkt einer Vielzahl von Gründen ungültig gemacht werden.  Die einzige Möglichkeit für Ihre app wissen, ob eine Aktualisierung Token gültig ist ist für den Versuch es einlösen, indem er eine token Anforderung an den Endpunkt Version 2.0.

Wenn Sie ein Token aktualisieren für ein neues Access Token einlösen (und wenn Ihre app gewährt wurde die `offline_access` Umfang), erhalten Sie ein neues aktualisieren Token in der Antwort token.  Speichern Sie das Token neu ausgestellt aktualisieren, ersetzen das Element, das Sie in der Besprechungsanfrage verwendet haben.  Dadurch wird sichergestellt, dass Ihre Token aktualisieren gültiger so lange bleiben.

## <a name="validating-tokens"></a>Überprüfen von Token

Zu diesem Zeitpunkt ist die nur token Überprüfung Ihrer apps ausführen müssen sollten Id_tokens überprüfen.  Um eine Id_token zu überprüfen, sollte Ihre app sowohl die Id_token der Signatur und die Ansprüche in der Id_token überprüfen.

<!-- TODO: Link -->
Bibliotheken und Codebeispielen, die veranschaulichen, token Prüfung – einfach behandeln wir erläutern die notwendigen die Informationen ist einfach weiter unten für diejenigen, die den zugrunde liegenden Prozess verstehen möchten.  Es stehen auch mehrere 3rd Party open-Source-Bibliotheken für die JWT Validierung – mindestens eine Option für fast jede Plattform & draußen Sprache vorhanden ist.

#### <a name="validating-the-signature"></a>Überprüfen die Signatur
Eine JWT enthält drei Segmente durch getrennt werden der `.` Zeichen.  Der erste Abschnitt wird als der **Kopfzeile**, der zweite als **Textkörper**, und die dritte als **Signatur**bezeichnet.  Die Echtheit der Id_token überprüft, dass sie Ihre app vertrauenswürdig sein kann, kann das Segment Signatur verwendet werden.

Id_Tokens angemeldet sind, Industry standard Asymmetrische Verschlüsselungsalgorithmen, wie z. B. RSA 256 verwenden. Die Kopfzeile der die Id_token enthält Informationen über die Taste und die Verschlüsselung-Methode verwendet, um das Token melden Sie sich an:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Die `alg` anfordern zeigt den Algorithmus, die verwendet wurde, um die Weile token, melden Sie sich an die `kid` anfordern zeigt den bestimmten öffentlichen Schlüssel, der sich das Token verwendet wurde.

Zu einem beliebigen angegebenen Zeitpunkt melden Sie sich der Endpunkt Version 2.0 möglicherweise eine Id_token mit einem von einer bestimmten Gruppe von Paare aus öffentlichen und dem privaten Schlüsseln.  Der Endpunkt Version 2.0 dreht die Menge der Schlüssel in regelmäßigen Abständen, damit Ihre app geschrieben werden sollen, die wichtigsten Änderungen automatisch zu behandeln.  Angemessene Häufigkeit zum nach Updates suchen auf die öffentlichen Schlüssel verwendet, die für den Endpunkt Version 2.0 ist ungefähr 24 Stunden.

Sie können die signierenden wichtigen Daten, die erforderlich sind, um die Signatur zu überprüfen, indem Sie mithilfe der Herstellen einer OpenID mit Metadaten des Dokuments am erwerben:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [AZURE.TIP] Probieren Sie diese URL in einem Browser!

Dieses Metadatendokument ist ein JSON-Objekt, enthält mehrere Textstellen hilfreiche Informationen, wie z. B. den Speicherort der verschiedenen Endpunkte zur Durchführung OpenID verbinden Authentifizierung erforderlich.  

Darüber hinaus enthält eine `jwks_uri`, wodurch erhält des Orts für die Menge der öffentlichen Schlüssel zum Signieren Token verwendet.  Das JSON-Dokument gespeichert, bei der `jwks_uri` enthält alle öffentlichen wichtige Informationen zu dieser bestimmten Zeitpunkt verwendet.  Können Sie Ihre app der `kid` beanspruchen in der Kopfzeile JWT auswählen, welche öffentlicher Schlüssel in diesem Dokument verwendet wurde, um ein bestimmtes Token melden.  Sie können Sie die Überprüfung der Signatur mit den richtigen öffentlichen Schlüssel und die angegebene Algorithmus durchführen.

Ausführen der Signatur Überprüfung außerhalb des Gültigkeitsbereichs des dieses Dokument ist – es stehen viele open-Source-Bibliotheken für eine dauerhafte hierzu bei Bedarf.

#### <a name="validating-the-claims"></a>Überprüfen der Ansprüche
Wenn Ihre app ein Id_token bei der Anmeldung für den Benutzer erhält, sollten sie auch ein paar Prüfungen gegen die Ansprüche in der Id_token durchführen.  Diese umfassen, aber nicht auf beschränkt sind:

- Der **Zielgruppe** Anspruch – um sicherzustellen, dass die Id_token vorgesehen war, Ihre app gegeben werden soll.
- **Frühestens** und **Ablaufzeit** Claims - to stellen Sie sicher, dass die Id_token nicht abgelaufen ist.
- Der **Herausgeber** Anspruch – um sicherzustellen, dass das Token tatsächlich um zu Ihrer Anwendung von den Endpunkt Version 2.0 ausgestellt wurde.
- Die **Nonce** - als-Angriffen ein token Wiedergabe.
- und vieles mehr...

Eine vollständige Liste der anfordern Validierungen, die Ihre app durchführen sollten, finden Sie in der [Spezifikation OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Details zu den erwarteten Werten für diese Ansprüche sind oben im [Abschnitt Id_token](#id_tokens)enthalten.


## <a name="token-lifetimes"></a>Token Lebensdauer

Die folgenden token Lebensdauer werden rein für Ihr Verständnis, bereitgestellt, wie sie zum Entwickeln und Debuggen von apps beitragen können.  Ihrer apps nicht zu erwarten eine der folgenden Lebensdauer konstant bleiben geschrieben werden sollen: sie können und Zeitpunkt zu einem bestimmten Zeitpunkt ändern.

| Token | Lebensdauer | Beschreibung |
| ----------------------- | ------------------------------- | ------------ |
| Id_Tokens (geschäftlichen oder schulnotizbücher Konten) | 1 Stunde | Id_Tokens sind für eine Stunde in der Regel gültig.  Web app kann diese dieselbe Lebensdauer bei der Verwaltung einer eigenen Sitzung mit dem Benutzer (empfohlen), oder klicken Sie eine vollständig anderen Sitzung Lebensdauer.  Wenn Ihre app ein neues Id_token abrufen muss, muss sie einfach eine neue Anmeldung Anforderung der Version 2.0 Endpunkt autorisieren erfolgen soll.  Wenn der Benutzer eine gültige Browsersitzung mit dem Version 2.0-Endpunkt aufweist, können sie ihre Anmeldeinformationen erneut eingeben nicht erforderlich. |
| Id_Tokens (Persönliche Konten) | 24 Stunden | Id_Tokens für Persönliche Konten sind in der Regel gültig 24 Stunden.  Web app kann diese dieselbe Lebensdauer bei der Verwaltung einer eigenen Sitzung mit dem Benutzer (empfohlen), oder klicken Sie eine vollständig anderen Sitzung Lebensdauer.  Wenn Ihre app ein neues Id_token abrufen muss, muss sie einfach eine neue Anmeldung Anforderung der Version 2.0 Endpunkt autorisieren erfolgen soll.  Wenn der Benutzer eine gültige Browsersitzung mit dem Version 2.0-Endpunkt aufweist, können sie ihre Anmeldeinformationen erneut eingeben nicht erforderlich. |
| Access-Token (geschäftlichen oder schulnotizbücher Konten) | 1 Stunde | Angegeben in token Antworten als Teil der token Metadaten. |
| Access-Token (Persönliche Konten) | 1 Stunde | Angegeben in token Antworten als Teil der token Metadaten.  Ausgestellt für Persönliche Konten Access_tokens möglicherweise für eine andere Lebensdauer konfiguriert werden, aber eine Stunde ist normalerweise der Fall. |
| Aktualisieren von Token (geschäftlichen oder schulnotizbücher-Konto) | Bis zu 14 Tage | Ein einzelnes aktualisieren Token ist für maximal 14 Tage gültig.  Jedoch möglicherweise das Token Aktualisierung zu einem beliebigen Zeitpunkt für verschiedene Gründe, möglicherweise ungültig, damit Ihre app zum Testen und verwenden ein Token aktualisieren, bis sie fehlschlägt oder bis Ihre app mit einem neuen aktualisieren Token ersetzt einen fortgesetzt werden soll.  Ein Token aktualisieren wird auch ungültig, wenn er 90 Tage wurde, da der Benutzer ihre Anmeldeinformationen eingegeben hat. |
| Aktualisieren von Token (Persönliche Konten) | Bis zu 1 Jahr | Ein einzelnes aktualisieren Token ist für maximal 1 Jahr gültig.  Jedoch möglicherweise das Token Aktualisierung zu einem beliebigen Zeitpunkt für verschiedene Gründe, möglicherweise ungültig, damit Ihre app zum Testen und verwenden ein Token aktualisieren, bis es nicht mehr fortgesetzt werden soll. |
| Autorisierungscodes (geschäftlichen oder schulnotizbücher Konten) | 10 Minuten | Autorisierungscodes sind absichtlich kurzlebige und sollte sofort für Access_tokens und Refresh_tokens eingelöst werden, wenn sie empfangen werden. |
| Autorisierungscodes (Persönliche Konten) | 5 Minuten | Autorisierungscodes sind absichtlich kurzlebige und sollte sofort für Access_tokens und Refresh_tokens eingelöst werden, wenn sie empfangen werden.  Autorisierungscodes für Persönliche Konten ausgestellt sind auch einmalige verwenden. |
