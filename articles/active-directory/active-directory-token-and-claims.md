 <properties
   pageTitle="Azure AD-Token Verweis | Microsoft Azure"
   description="Leitfaden für verstehen und Bewerten von der Ansprüche in der SAML 2.0 und JSON Web Token (JWT) Token ausgestellt von Azure Active Directory (AAD)"
   documentationCenter="na"
   authors="bryanla"
   services="active-directory"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/06/2016"
   ms.author="mbaldwin"/>

# <a name="azure-ad-token-reference"></a>Azure AD token Bezug

Azure Active Directory (Azure AD) gibt verschiedene Arten von Sicherheitstokens in die Verarbeitung von jedem Flow Authentifizierung aus. Dieses Dokument beschreibt das Format, Sicherheitseigenschaften und Inhalt der einzelnen Token.

## <a name="types-of-tokens"></a>Typen von Token

Azure AD unterstützt das [OAuth 2.0 Autorisierungsprotokoll](active-directory-protocols-oauth-code.md), welche sowohl Access_tokens und Refresh_tokens verwendet wird.  Außerdem unterstützt Authentifizierung und Anmeldung über [OpenID verbinden](active-directory-protocols-openid-connect-code.md), einen dritten Typ von Token, die Id_token vorgestellt.  Jede dieser Token wird als "Person"Token dargestellt.

Eine Person Token ist ein einfaches Sicherheitstoken, den "Person" den Zugriff auf eine geschützte Ressource gewährt. In diesem Sinne ist die "Person" einer Partei, die das Token präsentieren kann. Obwohl mit Azure AD Authentifizierung erforderlich ist, um eine Person Token zu erhalten, müssen die Schritte durchgeführt werden, das Token, um zu verhindern, dass durch einen unbeabsichtigte Dritten abgefangen gesichert. Da Token Person nicht über eine integrierte Methode, um zu verhindern, dass unbefugten deren Verwendung verfügen, müssen sie in einem sicheren Kanal wie z. B. Transport Layer Security (HTTPS) übertragen werden. Bei der Übertragung einer Person Token als Klartext kann ein Mann-in der mittleren Angriffen auf das Token erhalten und nicht autorisierten Zugriff auf eine geschützte Ressource verwendet werden. Diese Sicherheitsprinzipien gelten beim Speichern oder Person Token zur späteren Verwendung zwischenspeichern. Immer Stellen Sie sicher, dass Ihre app übermittelt und Person Token auf sichere Weise speichert. Weitere Sicherheitsaspekte auf Person Token finden Sie unter [RFC 6750 Abschnitt 5](http://tools.ietf.org/html/rfc6750).

Viele der Token ausgestellt von Azure AD werden als JSON Web Token oder JWTs implementiert.  Eine JWT ist eine komprimieren, URL-sichere Mittel Übertragung von Daten zwischen zwei Seiten.  In JWTs enthaltene Informationen werden als "Ansprüche" oder Assertionen von Informationen über die Person und den gewünschten Betreff für die Token bezeichnet.  Die Ansprüche im JWTs sind JSON-Objekte codierte und für die Übertragung serialisiert.  Da der JWTs ausgestellt von Azure AD angemeldet sind, aber nicht verschlüsselt, können Sie problemlos des Inhalts einer JWT für das Debuggen überprüfen.  Es gibt verschiedene Tools dafür zur Verfügung stehen, wie z. B. [jwt.calebb.net](http://jwt.calebb.net). Weitere Informationen zum JWTs können Sie auf der [JWT Spezifikation](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html)verweisen.

## <a name="idtokens"></a>Id_tokens

Id_tokens sind eine Form der Anmeldung Sicherheitstoken, die Ihre app erhält, bei der Durchführung der Authentifizierung mit [OpenID verbinden](active-directory-protocols-openid-connect-code.md).  Sie werden als [JWTs](#types-of-tokens)dargestellt und enthalten ist, die Sie verwenden können, für den Benutzer in Ihrer app signieren.  Sie können die Ansprüche in einer Id_token verwenden, wie gewünscht – häufig zum werden Anzeigen von Kontoinformationen oder Entscheidung Access Steuerelement in einer app.

Id_tokens sind angemeldet, aber nicht zu diesem Zeitpunkt verschlüsselt.  Wenn Ihre app ein Id_token empfängt, müssen sie [die Signatur überprüfen](#validating-tokens) , um das Token des Echtheit beweisen und ein paar Ansprüche im Token um nachzuweisen seine Gültigkeit überprüfen.  Überprüft, indem Sie eine app Claims variieren je nach Szenario Anforderungen, aber es gibt einige [Allgemeine anfordern Validierungen](#validating-tokens) , die Ihre app in jeder Szenario ausgeführt werden muss.

Finden Sie im folgenden Abschnitt Informationen auf Id_tokens Ansprüche sowie eine Stichprobe Id_token aus.  Beachten Sie, dass die Ansprüche im Id_tokens nicht in keiner bestimmten Reihenfolge zurückgegeben werden.  Darüber hinaus neue Ansprüche können in Id_tokens an einer beliebigen Stelle in der Zeit eingeführt werden – Ihre app sollte nicht unterbrechen, als neue Ansprüche eingeführt werden.  Die folgende Liste enthält die Ansprüche, die Ihre app zuverlässig zum Zeitpunkt der Erstellung dieses Dokuments interpretiert werden kann.  Gegebenenfalls kann sogar noch mehr Details in der [Spezifikation OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html)gefunden werden.

#### <a name="sample-idtoken"></a>Beispiel für id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [AZURE.TIP] Versuchen Sie zur wird empfohlen prüfen die Ansprüche in der Stichprobe Id_token durch [calebb.net](http://jwt.calebb.net)eingefügt.

#### <a name="claims-in-idtokens"></a>Ansprüche in id_tokens

| JWT anfordern | Namen | Beschreibung |
|-----------|------|-------------|
| `appid`| ID der Anwendung | Identifiziert die Anwendung, die das Token Zugriff auf eine Ressource verwendet wird. Die Anwendung kann als sich selbst oder im Namen eines Benutzers fungieren. Die Anwendung ID steht normalerweise für ein Application-Objekt, jedoch können auch Hauptbenutzer Objekt in einem Dienst in Azure AD dar. <br><br> **Beispiel für einen JWT Wert**: <br> `"appid":"15CB020F-3984-482A-864D-1D92265E8268"` |
| `aud`| Zielgruppe | Der Empfänger das Token. Die Anwendung, die das Token erhalten muss überprüfen, ob es sich bei der Zielgruppe Wert ist richtig und Ablehnen von einem beliebigen anderen Zielgruppe vorgesehen Token. <br><br> **Beispiel für SAML-Wert**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **Beispiel für einen JWT Wert**: <br> `"aud":"https://contoso.com"` |
| `appidacr`| Referenz für Kontext die Anwendung Authentifizierung | Gibt an, wie der Client authentifiziert wurde. Für einen öffentlichen Client ist der Wert 0 an. Wenn Client-ID und Client geheim verwendet werden, ist der Wert 1 an. <br><br> **Beispiel für einen JWT Wert**: <br> `"appidacr": "0"`|
| `acr`| Referenz für die Authentifizierung Kontext | Gibt an, wie der Betreff authentifiziert wurde, im Gegensatz zu den Client in der Anwendung Referenz für die Authentifizierung Kontext anfordern. Wert "0" gibt an, dass die Endbenutzer-Authentifizierung die Anforderungen der ISO/IEC 29115 nicht gerecht. <br><br> **Beispiel für einen JWT Wert**: <br> `"acr": "0"`|
| | Chatnachrichten Authentifizierung | Einträge Datum und Uhrzeit, wann Authentifizierung stattgefunden hat. <br><br> **Beispiel für SAML-Wert**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` |
| `amr`| Authentifizierungsmethode | Gibt an, wie der gewünschten Betreff für die Token authentifiziert wurde. <br><br> **Beispiel für SAML-Wert**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **Beispiel für einen JWT Wert**:`“amr”: ["pwd"]` |
| `given_name`| Vorname | Stellt die erste oder Namen des Benutzers, als auf das Objekt Azure AD-festlegen "angegebenen". <br><br> **Beispiel für SAML-Wert**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **Beispiel für einen JWT Wert**: <br> `"given_name": "Frank"` |
| `groups`| Gruppen | Stellt die Objekt-IDs, die den Betreff der Gruppenmitgliedschaft darstellen. Diese Werte sind eindeutige (Siehe Objekt-ID) und für die Verwaltung von Access, z. B. Autorisierung Zugriff auf eine Ressource erzwingen sicher verwendet werden können. Die Gruppen, die in den Gruppen Anspruch enthalten sind auf Basis pro Anwendung, durch die Eigenschaft "GroupMembershipClaims" des Anwendungsmanifests konfiguriert. Ein Wert von Null werden alle Gruppen ausschließen, ein Wert von "SecurityGroup" wird nur Active Directory-Sicherheitsgruppe Mitgliedschaften und ein Wert von "Alle" wird sowohl Sicherheitsgruppen und Verteilerlisten in Office 365 gehören einbeziehen. <br><br> **Beispiel für SAML-Wert**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **Beispiel für einen JWT Wert**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
| `idp` | Identitätsanbieter | Einträge der Identitätsanbieter, die den gewünschten Betreff für die Token authentifiziert. Dieser Wert ist identisch mit dem Wert über die Inanspruchnahme des Herausgebers, es sei denn, das Benutzerkonto in einem anderen Mandanten als den Herausgeber ist. <br><br> **Beispiel für SAML-Wert**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **Beispiel für einen JWT Wert**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` | IssuedAt | Speichert den Zeitpunkt an, an dem das Token ausgestellt wurde. Es wird häufig token Aktualität messen. <br><br> **Beispiel für SAML-Wert**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **Beispiel für einen JWT Wert**: <br> `"iat": 1390234181` |
| `iss` | Herausgeber | Gibt den Security token Service (STS), der erstellt und das Token zurückgibt. In der Token, der Azure AD zurückgibt, ist der Herausgeber sts.windows.net. Die GUID der Herausgeber anfordern Wert ist die Mandanten-ID des Azure AD-Verzeichnis. Die Mandanten-ID ist ein Bezeichner unveränderlich und zuverlässigen des Verzeichnisses. <br><br> **Beispiel für SAML-Wert**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **Beispiel für einen JWT Wert**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` | Nachname | Enthält den letzten Namen, Nachnamen oder Familienname des Benutzers an, wie in der Azure AD-Benutzer-Objekt definiert. <br><br> **Beispiel für SAML-Wert**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **Beispiel für einen JWT Wert**: <br> `"family_name": "Miller"` |
| `unique_name`| Namen | Stellt einen Menschen lesbaren Wert, der den gewünschten Betreff für die Token bezeichnet. Dieser Wert wird nicht in einen Mandanten eindeutig unbedingt und nur zu Anzeigezwecken verwendet werden soll. <br><br> **Beispiel für SAML-Wert**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **Beispiel für einen JWT Wert**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` | Objekt-ID | Enthält einen eindeutigen Bezeichner eines Objekts in Azure Active Directory. Dieser Wert ist unveränderlich und kann nicht erneut zugewiesen oder wiederverwendet werden. Verwenden Sie die Objekt-ID, um ein Objekt in Azure AD-Abfragen zu identifizieren. <br><br> **Beispiel für SAML-Wert**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **Beispiel für einen JWT Wert**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` | Rollen | Stellt alle Funktionen der Anwendung, die der Betreff gewährt wurde direkt oder indirekt über eine Gruppenmitgliedschaft und kann verwendet werden, um die Steuerung des Benutzerzugriffs rollenbasierte erzwingen. Anwendungsrollen auf Basis pro Anwendung, definiert werden, bis die `appRoles` Eigenschaft des Anwendungsmanifests. Die `value` Eigenschaft jeder Anwendung Rolle ist der Wert, der in der Rollen Anspruch angezeigt wird. <br><br> **Beispiel für SAML-Wert**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **Beispiel für einen JWT Wert**: <br> `“roles”: ["Admin", … ]` |
| `scp` | Bereich | Zeigt die Berechtigungen Identitätswechsel-Clientanwendung an. Ist die Standardberechtigung `user_impersonation`. Der Besitzer der gesicherte Ressource kann zusätzliche Werte in Azure AD zu registrieren. <br><br> **Beispiel für einen JWT Wert**: <br> `"scp": "user_impersonation"`|
| `sub` |Betreff| Gibt die Tilgungsanteile darüber, die welche das Token Informationen, wie z. B. der Benutzer einer Anwendung bestätigt. Dieser Wert ist unveränderlich und kann nicht zugewiesen werden oder wiederverwendet werden, damit es verwendet werden kann sicheres Autorisierung Prüfungen ausführen. Da der Betreff immer in der Token die Probleme Azure AD-vorhanden ist, wird empfohlen, verwenden diesen Wert in einem allgemeinen Zweck Autorisierungssystem. <br> `SubjectConfirmation`ist keiner anfordern. Es wird beschrieben, wie der gewünschten Betreff für die Token überprüft wird. `Bearer`Gibt an, dass der Betreff nach deren Besitz eines das Token bestätigt wird. <br><br> **Beispiel für SAML-Wert**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **Beispiel für einen JWT Wert**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"`|
| `tid` | Mandanten-ID | Ein unveränderlich, nicht wieder verwendbare Bezeichner, der den Mandanten Verzeichnis identifiziert, der das Token ausgestellt. Diesen Wert können Zugriff auf Mandanten-spezifische Directory Ressourcen in einer Anwendung mit mehreren Mandanten. Diesen Wert können Sie beispielsweise den Mandanten eines Anrufs an die Graph-API identifizieren. <br><br> **Beispiel für SAML-Wert**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **Beispiel für einen JWT Wert**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"`|
| `nbf`, `exp`|Token Lebensdauer | Definiert das Zeitintervall, in dem ein Token gültig ist. Der Dienst, der das Token überprüft sollte überprüfen, ob das aktuelle Datum innerhalb der token Lebensdauer, sonst ist es sollte das Token abzulehnen. Der Dienst möglicherweise alle Unterschiede zwischen in Systemzeit ("Time Schiefe") zwischen Azure AD bis zu fünf Minuten außerhalb des Bereichs token Lebensdauer zulassen und dem Dienst. <br><br> **Beispiel für SAML-Wert**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **Beispiel für einen JWT Wert**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn`| Benutzerprinzipalnamen | Der Benutzername für den Benutzerprinzipalnamen gespeichert.<br><br> **Beispiel für einen JWT Wert**: <br> `"upn": frankm@contoso.com`|
| `ver`| Version | Speichert die Versionsnummer der das Token. <br><br> **Beispiel für einen JWT Wert**: <br> `"ver": "1.0"`|

## <a name="access-tokens"></a>Access-Token

Access Token sind nur zu diesem Zeitpunkt von Microsoft Services.  Führen Sie eine Überprüfung oder Prüfung von Access Token nach jedem der derzeit unterstützten Szenarien sollten nicht Ihrer apps müssen.  Access-Token kann als transparent behandelt: sie sind nur Zeichenfolgen, da diese Ihre app in HTTP-Anfragen an Microsoft übergeben kann.

Wenn Sie eine Access-Token anfordern, gibt Azure AD auch einige Metadaten über die Access-Token für Ihre app Ernährung.  Diese Informationen umfassen die Anzeigedauer Ablauf der Access-Token und die Bereiche, für die es gültig ist.  Dadurch wird Ihre app intelligente Zwischenspeichern von Access Token ohne Öffnen Analysieren ausführen das Access-Token selbst.

## <a name="refresh-tokens"></a>Aktualisieren von Token

Aktualisieren von Token sind Sicherheitstoken, die Ihre app verwenden kann, um neue Access Token in einer OAuth 2.0-Verkehr zu erfassen.  Sie können Ihre app langfristiges Zugriff auf Ressourcen im Namen eines Benutzers zu erreichen, ohne Interaktion vom Benutzer.

Aktualisieren Token sind mehrere Ressourcen, d. h., sie können werden während einer token Anforderung für eine Ressource empfangen, aber für Access-Token zu einer vollständig anderen Ressource eingelöst. Um mehrere Ressource anzugeben, legen Sie die `resource` Parameter in der Besprechungsanfrage auf die gezielte Ressource.

Aktualisieren von Token sind vollständig transparent zu Ihrer Anwendung. Sie sind langer Lebensdauer, aber Ihre app nicht zu erwarten, dass ein Token aktualisieren für einen bestimmten Zeitraum dauert geschrieben werden sollen.  Aktualisieren Token können zu einem beliebigen Zeitpunkt einer Vielzahl von Gründen ungültig gemacht werden.  Die einzige Möglichkeit für Ihre app wissen, ob eine Aktualisierung Token gültig ist ist für den Versuch, die sie durch eine token Anforderung an token Azure AD-Endpunkt einlösen.

Wenn Sie ein Token aktualisieren für ein neues Access Token einlösen, erhalten Sie ein neues aktualisieren Token in der Antwort token.  Speichern Sie das Token neu ausgestellt aktualisieren, ersetzen das Element, das Sie in der Besprechungsanfrage verwendet haben.  Dadurch wird sichergestellt, dass Ihre Token aktualisieren gültiger so lange bleiben.

## <a name="validating-tokens"></a>Überprüfen von Token

Zu diesem Zeitpunkt ist die nur token Überprüfung, die Ihre app Client ausführen muss sollten Id_tokens überprüfen.  Um eine Id_token zu überprüfen, sollte Ihre app sowohl die Id_token der Signatur und die Ansprüche in der Id_token überprüfen.

Wir erläutern die notwendigen Bibliotheken und Codebeispielen, die so einfach token Überprüfung, verarbeitet anzeigen, wenn Sie den zugrunde liegenden Prozess verstehen möchten.  Es gibt auch mehrere offene Quelle Drittanbieter-Bibliotheken für JWT Überprüfung, die mindestens eine Option für fast jede Plattform und Sprache verfügbar. Weitere Informationen zu Azure AD-Authentifizierung Bibliotheken und Codebeispielen finden Sie unter [Azure AD-Authentifizierung Bibliotheken](active-directory-authentication-libraries.md).

#### <a name="validating-the-signature"></a>Überprüfen die Signatur

Eine JWT enthält drei Segmente durch getrennt werden der `.` Zeichen.  Der erste Abschnitt wird als der **Kopfzeile**, der zweite als **Textkörper**, und die dritte als **Signatur**bezeichnet.  Die Echtheit der Id_token überprüft, dass sie Ihre app vertrauenswürdig sein kann, kann das Segment Signatur verwendet werden.

Id_Tokens angemeldet sind, Industry standard Asymmetrische Verschlüsselungsalgorithmen, wie z. B. RSA 256 verwenden. Die Kopfzeile der die Id_token enthält Informationen über die Taste und die Verschlüsselung-Methode verwendet, um das Token melden Sie sich an:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "kriMPdmBvx68skT8-mPAB3BseeA"
}
```

Die `alg` anfordern zeigt den Algorithmus, die verwendet wurde, um die Weile token, melden Sie sich an die `x5t` anfordern zeigt den bestimmten öffentlichen Schlüssel, der sich das Token verwendet wurde.

Zu einem beliebigen angegebenen Zeitpunkt möglicherweise Azure AD einer Id_token mit einem von einer bestimmten Gruppe von Paare aus öffentlichen und dem privaten Schlüsseln signieren. Azure AD dreht die Menge der Schlüssel in regelmäßigen Abständen, damit Ihre app geschrieben werden sollen, die wichtigsten Änderungen automatisch zu behandeln.  Angemessene Häufigkeit zum nach Updates suchen auf die öffentlichen Schlüssel Azure AD untersuchten ist alle 24 Stunden.

Sie können die signierenden wichtigen Daten, die erforderlich sind, um die Signatur zu überprüfen, indem Sie mithilfe der Herstellen einer OpenID mit Metadaten des Dokuments am erwerben:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [AZURE.TIP] Probieren Sie diese URL in einem Browser!

Dieses Metadatendokument ist ein JSON-Objekt, enthält mehrere Textstellen hilfreiche Informationen, wie z. B. den Speicherort der verschiedenen Endpunkte zur Durchführung OpenID verbinden Authentifizierung erforderlich.  

Darüber hinaus enthält eine `jwks_uri`, wodurch erhält des Orts für die Menge der öffentlichen Schlüssel zum Signieren Token verwendet.  Das JSON-Dokument gespeichert, bei der `jwks_uri` enthält alle öffentlichen wichtige Informationen zu dieser bestimmten Zeitpunkt verwendet.  Können Sie Ihre app der `kid` beanspruchen in der Kopfzeile JWT auswählen, welche öffentlicher Schlüssel in diesem Dokument verwendet wurde, um ein bestimmtes Token melden.  Sie können Sie die Überprüfung der Signatur mit den richtigen öffentlichen Schlüssel und die angegebene Algorithmus durchführen.

Ausführen der Signatur Überprüfung außerhalb des Gültigkeitsbereichs des dieses Dokument ist – es stehen viele open-Source-Bibliotheken für eine dauerhafte hierzu bei Bedarf.

#### <a name="validating-the-claims"></a>Überprüfen der Ansprüche

Wenn Ihre app ein Id_token bei der Anmeldung für den Benutzer erhält, sollten sie auch ein paar Prüfungen gegen die Ansprüche in der Id_token durchführen.  Diese umfassen, aber nicht auf beschränkt sind:

  - Der **Zielgruppe** Anspruch – um sicherzustellen, dass die Id_token vorgesehen war, Ihre app gegeben werden soll.
  - **Frühestens** und **Ablaufzeit** Claims - to stellen Sie sicher, dass die Id_token nicht abgelaufen ist.
  - Der **Herausgeber** Anspruch – um sicherzustellen, dass das Token tatsächlich um zu Ihrer Anwendung von Azure AD ausgestellt wurde.
  - Die **Nonce** - um einen token Schutz vor zu verringern.
  - und vieles mehr...

Eine vollständige Liste der anfordern Validierungen, die Ihre app durchführen sollten, finden Sie in der [Spezifikation OpenID verbinden](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Details zu den erwarteten Werten für diese Ansprüche sind in der vorhergehenden Abschnitt [Id_token](#id-tokens) enthalten.

## <a name="sample-tokens"></a>Beispiel für Token

In diesem Abschnitt zeigt Beispiele von SAML und JWT Token, der Azure AD zurückgibt. In diesen Beispielen können Sie die Ansprüche im Kontext zu sehen.
SAML-Token

Dies ist ein Beispiel für eine typische SAML-Token.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>JWT Token - Benutzeridentitätswechsel

Dies ist ein Beispiel für eine typische JSON Web Token (JWT) in eine Autorisierung Code erteilen Fluss verwendet werden.
Zusätzlich zu Ansprüche umfasst das Token eine Versionsnummer **Version** und **Appidacr**, Authentifizierung Kontext Klasse Bezug, der festlegt, wie der Client authentifiziert wurde. Für einen öffentlichen Client ist der Wert 0 an. Wenn eine Client-ID oder Client geheim verwendet wurde, ist der Wert 1 an.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>Siehe auch
- Weitere Informationen zum Verwalten von Richtlinien für die Gültigkeitsdauer token über die Azure AD Graph-API finden Sie unter die Azure AD Graph- [Richtlinie Vorgänge](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) und die [Richtlinie Entität](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity).
- Weitere Informationen und Beispiele zum Verwalten von Richtlinien über PowerShell-Cmdlets, einschließlich Beispiele finden Sie unter [konfigurierbare token Lebensdauer in Azure Active Directory](active-directory-configurable-token-lifetimes.md). 
