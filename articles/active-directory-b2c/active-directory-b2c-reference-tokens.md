<properties
    pageTitle="Azure-Active Directory B2C | Microsoft Azure"
    description="Die Typen von Token in der Azure-Active Directory B2C ausgestellt."
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


# <a name="azure-ad-b2c-token-reference"></a>Azure AD-B2C: Token Bezug

Azure Active Directory (Azure AD) B2C gibt verschiedene Arten von Sicherheitstokens Verarbeitung von jedem [Authentifizierung Fluss](active-directory-b2c-apps.md)aus. Dieses Dokument beschreibt das Format, Sicherheitseigenschaften und Inhalt der einzelnen Token.

## <a name="types-of-tokens"></a>Typen von Token

Azure AD B2C unterstützt das [OAuth 2.0 Autorisierungsprotokoll](active-directory-b2c-reference-protocols.md), welche sowohl Access Token und aktualisieren Token verwendet wird. Unterstützt auch Authentifizierung und Anmeldung über [OpenID verbinden](active-directory-b2c-reference-protocols.md), einen dritten Typ von Token vorgestellt: das ID-Token. Jede dieser Token wird als eine Person Token dargestellt.

Eine Person Token ist ein einfaches Sicherheitstoken, den "Person" den Zugriff auf eine geschützte Ressource gewährt. Die Person ist eine Partei, die das Token präsentieren kann. Azure AD muss zuerst eine Party authentifizieren, bevor sie eine Person Token empfangen kann. Wenn die erforderlichen Schritte zum Token im Übertragung und Speicherung gesichert nicht geöffnet werden, kann jedoch werden abgefangen und durch einen dritten unbeabsichtigte verwendet. Einige Sicherheitstokens verfügen über eine integrierte Funktionalität für verhindern, dass unbefugten deren Verwendung, aber Person Token nicht dieses Verfahren. Sie müssen in einem Kanal, wie z. B. Transport Layer Security (HTTPS) übertragen werden.

Bei der Übertragung einer Person Token außerhalb eines Kanal können bösartige dritte ein Mann in zweiter Angriffen das Token erfassen und verwenden, um unbefugten Zugriff auf eine geschützte Ressource. Diese Sicherheitsprinzipien angewendet werden, wenn die Person Token gespeichert oder zur späteren Verwendung zwischengespeichert werden. Immer Stellen Sie sicher, dass Ihre app übermittelt und Person Token auf sichere Weise speichert.

Zusätzliche Sicherheitsaspekte auf Person Token finden Sie unter [RFC 6750 Abschnitt 5](http://tools.ietf.org/html/rfc6750).

Viele der Token, die Probleme Azure AD B2C werden als JSON Web Token (JWTs) implementiert. Eine JWT ist eine komprimieren, URL-sichere Mittel Übertragung von Daten zwischen zwei Seiten. JWTs enthalten die Informationen, die als Ansprüche bekannt. Dies sind Assertionen von Informationen zu der Person und den gewünschten Betreff für die Token. Die Ansprüche im JWTs sind JSON-Objekte, die codierte und für die Übertragung serialisiert. Da die JWTs ausgestellt von Azure AD B2C angemeldet sind, aber nicht verschlüsselt, können Sie einfach den Inhalt einer JWT zum Debuggen überprüfen. Verschiedene Tools zur Verfügung, die dies ist möglich, einschließlich [calebb.net](http://calebb.net). Weitere Informationen zu JWTs finden Sie in [JWT Spezifikationen](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>ID-Token

Ein ID-Token ist ein Formular des Sicherheitstokens, die Ihre app von der Azure AD B2C empfängt `authorize` und `token` Endpunkte. ID-Token als [JWTs](#types-of-tokens)dargestellt werden, und sie enthalten ist, die Sie verwenden können, um Benutzer in Ihrer app zu identifizieren. Wenn ID Token aus erworben werden die `authorize` Endpunkt, häufig werden Sie sich Webanwendungen Benutzer anmelden. Wenn ID Token aus erworben werden die `token` Endpunkt, diese gesendet werden, um in HTTP-Anfragen während der Kommunikation zwischen zwei Komponenten den Anwendung oder den Dienst. Sie können die Ansprüche in ein ID-Token verwenden, je nach Bedarf. Sie werden häufig um Kontoinformationen anzeigen oder Erstellen von Access-Steuerelement Entscheidungen in einer app verwendet werden.  

ID Token angemeldet sind, aber sie nicht zu diesem Zeitpunkt verschlüsselt werden. Wenn Ihre app oder API ein ID-Token erhält, müssen sie [die Signatur überprüfen](#token-validation) , um nachzuweisen, dass das Token authentisch ist. Ihre app oder API muss ein paar Ansprüche im Token um nachzuweisen, dass er gültig ist auch überprüft werden. Je nach den Anforderungen Szenario Claims überprüft, indem Sie eine app können abweichen, aber Ihre app muss einige [Allgemeine anfordern Validierungen](#token-validation) in jeder Szenario durchführen.

#### <a name="sample-id-token"></a>Beispiel-ID-token
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Access-Token

Eine Access-Token ist ebenfalls ein Formular des Sicherheitstokens, die Ihre app von der Azure AD B2C empfängt `authorize` und `token` Endpunkte. Access Token werden auch als [JWTs](#types-of-tokens)dargestellt, und sie enthalten ist, die Sie verwenden können, um Benutzer in Ihrer Webdienste und APIs zu identifizieren.

Access Token angemeldet sind, aber sie sind nicht zu diesem Zeitpunkt - verschlüsselte und ähnelt dem Id-Token.  Access Token sollte bieten Zugriff auf Webdienste und APIs zu identifizieren und Authentifizieren des Benutzers in Dienste verwendet werden.  Allerdings bieten sie keine Assertion der Autorisierung auf Dienste.  Das heißt, die die `scp` anfordern in Access Token nicht beschränken oder andernfalls darstellen des Zugriffs, der der gewünschten Betreff für die Token gewährt wurde.

Wenn Ihre API einer Access-Token erhält, müssen sie [die Signatur überprüfen](#token-validation) , um nachzuweisen, dass das Token authentisch ist. Ihre API muss auch überprüft werden einige Ansprüche im Token um nachzuweisen, dass sie zulässig ist. Je nach den Anforderungen Szenario Claims überprüft, indem Sie eine app können abweichen, aber Ihre app muss einige [Allgemeine anfordern Validierungen](#token-validation) in jeder Szenario durchführen.

### <a name="claims-in-id--access-tokens"></a>Ansprüche ID & Access Token

Wenn Sie Azure AD B2C verwenden, können Sie abgestimmte Steuern des Inhalts Ihrer Token. Sie können konfigurieren, [Richtlinien](active-directory-b2c-reference-policies.md) , um bestimmte Sätze von Benutzerdaten in Ansprüche zu senden, die Ihre app für Vorgänge erforderlich sind. Diese Ansprüche können Standardeigenschaften wie des Benutzers einbeziehen `displayName` und `emailAddress`. Sie können auch [eigene benutzerdefinierte Attribute](active-directory-b2c-reference-custom-attr.md) umfassen, die Sie in Ihrem Verzeichnis B2C definieren können. Jeder-ID und Access Token, die Sie erhalten, wird eine bestimmte Gruppe von Ansprüchen Sicherheits-enthalten. Diese Ansprüche können Ihre Applikationen Sicheres Authentifizieren von Benutzern und Besprechungsanfragen.

Beachten Sie, dass die Ansprüche im Token ID nicht in keiner bestimmten Reihenfolge zurückgegeben werden. Darüber hinaus können neue Ansprüche in ID-Token zu einem beliebigen Zeitpunkt eingeführt werden. Ihre app sollte nicht getrennt werden als neue Ansprüche eingeführt werden. Hier sind die Ansprüche, die Sie nicht vorhaben, in-ID und Access Token ausgestellt von Azure AD B2C vorhanden sind. Alle zusätzlichen Ansprüchen werden durch Richtlinien bestimmt. Versuchen Sie zur wird empfohlen Prüfen der Ansprüche in der Stichprobe-ID-Token nach [calebb.net](http://calebb.net)eingefügt. Weitere Details finden Sie in der [Spezifikation OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html).

| Namen | Anfordern | Beispiel für einen Wert | Beschreibung |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Zielgruppe | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | Ein Publikum Anspruch identifiziert den Empfänger, der das Token. Für Azure AD B2C ist das Publikum Ihre app ID der Anwendung, wie Ihre app im app-Registrierung Portal zugewiesen. Ihre app sollte dieser Wert überprüfen und das Token ablehnen, wenn es nicht übereinstimmt. |
| Herausgeber | `iss` | `https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | Dieser Anspruch identifiziert den Security token Service (STS), der erstellt und das Token zurückgibt. Er gibt außerdem Azure AD-Verzeichnis, in dem der Benutzer authentifiziert wurde. Ihre app sollte der Herausgeber anfordern, um sicherzustellen, dass das Token den Endpunkt Version 2.0 stammen überprüfen. |
| Ausgestellt am | `iat` | `1438535543` | Dieses Argument ist die Uhrzeit, an der das Token Epoche Zeitpunkt dargestellt ausgestellt wurde. |
| Ablaufzeit | `exp` | `1438539443` | Das Ablaufdatum Mal anfordern die Zeit wird, das Token ungültig, wird, dargestellt in Epoche Zeit. Überprüfen Sie die Gültigkeit der token Lebensdauer sollte Ihre app dieser Anspruch verwenden.  |
| Nicht vor | `nbf` | `1438535543` | Dieses Argument ist die Zeit, das Token gültig, Epoche Zeitpunkt dargestellt wird. Dies ist normalerweise der gleiche wie die Zeit, die das Token ausgestellt wurde. Überprüfen Sie die Gültigkeit der token Lebensdauer sollte Ihre app dieser Anspruch verwenden.  |
| Version | `ver` | `1.0` | Dies ist die Version von der Token-ID durch Azure AD definiert. |
| Code hash | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Nur, wenn das Token zusammen mit einer OAuth 2.0 Autorisierungscode ausgegeben wird, ist in einer Token-ID Hashfunktion Code enthalten. Ein Hash Code kann verwendet werden, die Echtheit eines Autorisierungscodes überprüft. Finden Sie in der [Spezifikation OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html) für weitere Details zum Ausführen dieser Überprüfung. |
| Access token hash | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Ein token Access-Hash ist Bestandteil einer Token-ID nur, wenn das Token in Verbindung mit einer Access-Token OAuth 2.0 ausgegeben wird. Ein token Access-Hash kann verwendet werden, die Echtheit von einer Access-Token überprüft. Finden Sie in der [Spezifikation OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html) für weitere Details zum Ausführen dieser Überprüfung. |
| Nonce | `nonce` | `12345` | Eine Nonce ist eine Strategie verwendet, um die Wiedergabe token Angriffen zu verringern. Ihre app kann in einer Anforderung Autorisierung Nonce angeben, indem Sie mit der `nonce` Abfrage Parameter. Der Wert in der Besprechungsanfrage werden unverändert ausgegeben werden die `nonce` der ein ID-Token beanspruchen. Dadurch wird die app, überprüfen Sie, ob den Wert mit dem Wert der Anforderung angegebenen das des app Sitzung mit einem gegebenen ID Token verknüpft. Ihre app sollten diese Validierung während der ID token Validierung durchführen. |
| Betreff | `sub` | `Not supported currently. Use oid claim.` | Hierbei handelt es sich um eine Tilgungsanteile darüber, welche das Token Informationen, wie z. B. der Benutzer der app bestätigt. Dieser Wert ist unveränderlich und kann nicht erneut zugewiesen oder wiederverwendet werden. Hiermit können Sie Autorisierung Prüfungen ausführen sicheres, z. B., wenn das Token Zugriff auf eine Ressource verwendet wird. In der Azure AD B2C ist der Betreff Anspruch jedoch noch nicht implementiert. Sie sollten Ihre Richtlinien zum Einschließen der Objekt-ID konfigurieren `oid` beanspruchen und deren Wert verwenden, um Benutzer zu identifizieren, statt den Betreff Anspruch für die Autorisierung verwenden. |
| Referenz für die Authentifizierung Kontext | `acr` | `b2c_1_sign_in` | Dies ist der Name der Richtlinie, die verwendet wurde, um das ID-Token zu erfassen.  |
| Zeitpunkt der Authentifizierung | `auth_time` | `1438535543` | Dieses Argument ist die Zeit, eine letzten eingegebenen Benutzeranmeldeinformationen, dargestellt, Epoche Zeitpunkt. |


### <a name="refresh-tokens"></a>Aktualisieren von Token

Aktualisieren von Token sind Sicherheitstoken, mit denen Ihre app neue ID Token erfassen und Token in einen Fluss OAuth 2.0 zugreifen kann. Sie bieten Ihre app langfristiges Zugriff auf Ressourcen für Benutzer ohne Interaktion mit diesen Benutzer.

Erhalten eine Aktualisierung token Reaktion token Ihre app muss Anfordern der `offline_acesss` Bereich. Weitere Informationen zu den `offline_access` Bereich, schlagen Sie in den [Azure AD B2C Protokoll Bezug](active-directory-b2c-reference-protocols.md).

Aktualisieren von Token sind, und ist immer, zu Ihrer Anwendung vollständig undurchsichtig. Sie können sind ausgestellt von Azure AD und werden überprüft und nur von Azure AD interpretiert. Sie sind langer Lebensdauer, aber Ihre app nicht in der Annahme, die ein Token aktualisieren für einen bestimmten Zeitraum dauert geschrieben werden sollen. Aktualisieren Token können zu einem beliebigen Zeitpunkt einer Vielzahl von Gründen ungültig sein. Die einzige Möglichkeit für Ihre app wissen, ob eine Aktualisierung Token gültig ist, wird versucht, es einlösen, indem er eine token Anforderung an Azure AD.

Wenn Sie eine Aktualisierung Token für ein neues Token einlösen (und wenn Ihre app gewährt wurde die `offline_access` Umfang), erhalten Sie ein neues aktualisieren Token in der Antwort token. Speichern Sie das Token neu ausgestellt aktualisieren. Sie sollten das Aktualisierung Token ersetzen, die, das Sie zuvor in die Anfrage verwendet haben. Dies sorgt dafür, dass Ihre Token aktualisieren gültiger so lange bleiben.

## <a name="token-validation"></a>Token Überprüfung

Um ein Token zu überprüfen, sollten Ihre app sowohl die Signatur und allen Ansprüchen für das Token.

Viele open-Source-Bibliotheken, die zur Überprüfung JWTs, abhängig von Ihrer bevorzugten Sprache verfügbar sind. Es empfiehlt sich, sehen Sie sich diese Optionen statt zu Ihrer eigenen Überprüfungslogik implementieren. Die Informationen in diesem Handbuch helfen Sie erfahren, wie diese Bibliotheken ordnungsgemäß zu verwenden.

### <a name="validate-the-signature"></a>Überprüfen Sie die Signatur
Eine JWT enthält drei Segmente getrennt durch die `.` Zeichen. Der erste Abschnitt ist der **Kopfzeile**, die zweite ist der **Textkörper**und der dritte ist die **Signatur**. Die Echtheit der das Token überprüft, dass sie Ihre app vertrauenswürdig sein kann, kann das Segment Signatur verwendet werden.

Azure AD B2C Token sind mithilfe von Industriestandard Asymmetrische Verschlüsselungsalgorithmen, wie z. B. RSA 256 angemeldet. Die Kopfzeile der das Token enthält Informationen über die Taste und die Verschlüsselung-Methode verwendet, um das Token melden Sie sich an:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

Die `alg` anfordern zeigt an, den Algorithmus, der sich das Token verwendet wurde. Die `kid` anfordern zeigt den bestimmten öffentlichen Schlüssel, der sich das Token verwendet wurde.

Bei einem beliebigen Zeitpunkt möglicherweise Azure AD ein Token mithilfe eines eine bestimmte Gruppe von Paare aus öffentlichen und dem privaten Schlüsseln signieren. Azure AD dreht die Menge der Schlüssel in regelmäßigen Abständen, damit Ihre app geschrieben werden sollen, die wichtigsten Änderungen automatisch zu behandeln. Angemessene Häufigkeit zum nach Updates suchen auf die öffentlichen Schlüssel Azure AD untersuchten ist alle 24 Stunden.

Azure AD B2C weist einen Metadaten-Endpunkt OpenID verbinden. Dadurch wird die apps Informationen Azure AD B2C zur Laufzeit abgerufen werden sollen. Diese Informationen umfassen Endpunkte, token Inhalt und Token Schlüssel signieren. Ihr B2C-Verzeichnis enthält ein JSON-Metadaten-Dokument für jede Richtlinie. Angenommen, die Metadatendokument für die `b2c_1_sign_in` Richtlinie in der `fabrikamb2c.onmicrosoft.com` befindet sich unter:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com`wird verwendet, um die Benutzerauthentifizierung B2C Verzeichnis und `b2c_1_sign_in` ist die Richtlinie verwendet, um das Token zu erfassen. Um festzustellen, welche Richtlinie verwendet wurde, um ein Token melden (und wo die Metadaten abgerufen werden sollen), stehen Ihnen zwei Optionen zur Verfügung. Zunächst ist der Richtlinienname enthalten, der `acr` im Token beanspruchen. Sie können Ansprüche aus dem Hauptteil der JWT analysieren, indem Base 64 Textkörper decodieren und aus diesen JSON-Zeichenfolge, die sich ergibt. Die `acr` anfordern, werden die Namen der Richtlinie, die verwendet wurde, um die auszustellen.  Eine andere Möglichkeit besteht darin, die Richtlinie in den Wert von Codieren der `state` Parameter, wenn Sie die Anforderung und Entschlüsseln klicken Sie dann darauf, um zu bestimmen, welche Richtlinie verwendet wurde. Beide Methoden ist gültig.

Das Metadatendokument ist ein JSON-Objekt, das mehrere Textstellen hilfreiche Informationen enthält. Hierzu gehören die Position der Endpunkte zum Ausführen von OpenID verbinden Authentifizierung erforderlich ist. Sie auch umfassen `jwks_uri`, wodurch des Orts für die Menge der öffentlichen Schlüssel erhält werden verwendet, um Token zu melden. Die Position hier bereitgestellt, aber es empfiehlt sich, den Speicherort dynamisch abrufen, indem Sie mithilfe der Metadaten des Dokuments und Analysieren `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

Das JSON-Dokument am diese URL enthält alle öffentlichen wichtige Informationen zu einem bestimmten Zeitpunkt verwendet. Können Sie Ihre app der `kid` beanspruchen in der Kopfzeile JWT öffentlichen Schlüssel in das JSON-Dokument auswählen, die sich ein bestimmtes Token verwendet wird. Sie können die Überprüfung der Signatur klicken Sie dann mit den richtigen öffentlichen Schlüssel und die angegebene Algorithmus ausführen.

Eine Beschreibung, wie die Validierung der Signatur ist außerhalb des Gültigkeitsbereichs dieses Dokuments. Viele open-Source-Bibliotheken stehen dabei helfen, wenn Sie es benötigen.

### <a name="validate-the-claims"></a>Überprüfen der Ansprüche
Wenn Ihre app oder API ein ID-Token erhält, sollten sie auch mehrere Prüfungen gegen die Ansprüche im Token ID durchführen. Diese enthalten, aber es besteht keine Beschränkung auf:

- Die **Zielgruppe** anfordern: Dadurch wird sichergestellt, dass das ID-Token vorgesehen wurde, Ihre app gegeben werden soll.
- Die Ansprüche **nicht vor dem** und die **Uhrzeit Ablauf** : Diese Stellen Sie sicher, dass das ID-Token nicht abgelaufen ist.
- Der **Herausgeber** anfordern: Dadurch wird sichergestellt, dass das Token zu Ihrer Anwendung von Azure AD ausgestellt wurde.
- Die **Nonce**: Dies ist eine Strategie für die Wiedergabe token-Angriffen.

Eine vollständige Liste der Validierungen, die Ihre app durchgeführt werden soll, finden Sie die [Spezifikation OpenID verbinden](https://openid.net). Details zu den erwarteten Werten für diese Ansprüche sind im vorherigen [Abschnitt token](#types-of-tokens)enthalten.  

## <a name="token-lifetimes"></a>Token Lebensdauer

Die folgenden token Lebensdauer werden bereitgestellt, um Ihre Kenntnisse zu erweitern. Sie können Ihnen helfen, beim Entwickeln und Debuggen von apps. Beachten Sie, dass Ihre apps nicht zu erwarten eine der folgenden Lebensdauer konstant bleiben geschrieben werden sollen. Sie können und ändern.  Sie können weitere Informationen über die Anpassung der token Gültigkeitsdauer in Azure AD B2C [hier](active-directory-b2c-token-session-sso.md).

| Token | Lebensdauer | Beschreibung |
| ----------------------- | ------------------------------- | ------------ |
| ID-Token | Eine Stunde | ID Token sind für eine Stunde in der Regel gültig. Diese Lebensdauer können Web app verwalten einen eigenen Sitzungen-Benutzer (empfohlen). Sie können auch eine andere Sitzung Lebensdauer auswählen. Wenn Ihre app eine neue ID token abrufen muss, muss sie einfach eine neue Anforderung Anmeldung an Azure AD vornehmen. Wenn ein Benutzer eine gültige Browsersitzung mit Azure AD-verfügt, kann diesen Benutzer nicht erforderlich ist Anmeldeinformationen erneut eingeben. |
| Aktualisieren von Token | Bis zu 14 Tage | Ein einzelnes aktualisieren Token ist für maximal 14 Tage gültig. Jedoch möglicherweise ein Token aktualisieren, zu einem beliebigen Zeitpunkt eine Reihe von Gründen ungültig. Ihre app sollte weiterhin versuchen, eine Aktualisierung Token verwenden, bis die Anforderung fehlschlägt, oder die app das Aktualisieren Token durch ein neues ersetzt.  Ein Token aktualisieren kann auch ungültig, wenn der Benutzer letzten Anmeldeinformationen eingegeben ist 90 Tage verstrichen. |
| Autorisierungscodes | Fünf Minuten | Autorisierungscodes werden absichtlich kurzlebige. Sie sollten sofort eingelöst werden für Access Token, ID Token oder Token aktualisieren, wenn sie empfangen werden. |
