
<properties
    pageTitle="Azure AD-Version 2.0 OAuth Client Anmeldeinformationen Fluss | Microsoft Azure"
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
    ms.date="09/26/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-client-credentials-flow"></a>Datenfluss Version 2.0 Protokolle - OAuth 2.0-Client-Anmeldeinformationen

Die [Anmeldeinformationen OAuth 2.0-Clients erteilen](http://tools.ietf.org/html/rfc6749#section-4.4), genannt "OAuth zwei Abschnitten" kann Zugriff auf Web gehosteten Ressourcen, die mit der Identität des Anwendung verwendet werden.  Es wird häufig für Server-zu-Server-Interaktionen verwendet, die im Hintergrund, ohne die sofortige Precense von einem Endbenutzer ausgeführt werden müssen.  Diese Typen von Applications werden häufig als **Daemons** oder **Dienstkonten**bezeichnet.

> [AZURE.NOTE]
    Nicht alle Azure Active Directory-Szenarien und Features werden von den Endpunkt Version 2.0 unterstützt.  Um festzustellen, ob den Version 2.0-Endpunkt verwendet werden sollen, erfahren Sie, [Version 2.0 Einschränkungen](active-directory-v2-limitations.md).

In der häufiger vor drei "drei Abschnitten OAuth," ist eine Clientanwendung Zugriffsberechtigung für eine Ressource für einen bestimmten Benutzer erteilt.  Die Berechtigung ist vom Benutzer mit der Anwendung, in der Regel während des Prozesses [Zustimmung](active-directory-v2-scopes.md) **delegiert** .  Berechtigungen werden jedoch im Client Anmeldeinformationen Fluss, **direkt** für die Anwendung selbst erteilt.  Wenn die app ein Token für eine Ressource bietet, erzwingt die Ressource an, dass die Anwendung selbst Autorisierung einige Aktion – nicht ausführen ist, dass einige Benutzer Autorisierung verfügt.

## <a name="protocol-diagram"></a>Protokoll-Diagramm
Der gesamte Client Anmeldeinformationen Fluss sieht ungefähr wie folgt aus: einzelnen Schritte sind im folgenden ausführlich beschrieben.

![Client Anmeldeinformationen Fluss](../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## <a name="get-direct-authorization"></a>Erhalten direkten Autorisierung 
Es gibt zwei Methoden, um eine app in der Regel direkte Autorisierung Zugriff auf eine Ressource empfängt: über eine Access Control List, an der Ressource oder Anwendung Berechtigung Zuordnung in Azure Active Directory.  Es gibt mehrere Methoden, die eine Ressource auswählen kann, deren Kunden zu autorisieren, und jeder Ressourcenserver möglicherweise wählen Sie die Methode, die für die Anwendung am sinnvollsten ist.  Diese beiden Methoden sind die am häufigsten verwendeten in Azure AD- und empfohlen für Kunden und Ressourcen, die den Client Anmeldeinformationen Fluss durchführen möchten.

### <a name="access-control-lists"></a>Listen der Access-Steuerelement
Ein bestimmte Ressourcenanbieter möglicherweise eine Autorisierung Kontrollkästchen basierend auf eine Liste der Anwendung IDs, dass er weiß und einige bestimmte Zugriffsebene gewährt erzwingen.  Wenn die Ressource ein Token aus den Endpunkt Version 2.0 erhält, können sie das Token entschlüsseln und Extrahieren der die Kunden-ID für aus der `appid` und `iss` Ansprüche.  Sie können dann vergleichen, dass die Anwendung gegen einige Access Steuerelement Zugriffssteuerungsliste er verwaltet.  Die Genauigkeit und die Methode der Liste Steuerelement zugreifen können erheblich von Ressourcen zu Ressourcen abweichen.

Eine allgemeine Anwendungsfall-solche ACLs ist für eine Webanwendung Läufer testen oder web-api.  Im Web möglicherweise api nur eine Teilmenge der zugehörigen Vollzugriff verschiedene Clients erteilen.  Aber zum Ausführen von durchgehende Tests zur-api ein Testclient erstellt, die aus der Version 2.0-Endpunkt Token erhält und an die api gesendet werden.  Die api können dann ACL-ID für des Test-Clients für Vollzugriff auf die api des gesamten Funktionalität.  Beachten Sie, dass, wenn Sie eine solche Liste auf dem Dienst verfügen, Sie unbedingt des Anrufers nicht nur die Gültigkeit sollten `appid`, aber auch zur Überprüfung, die die `iss` das Token ist vertrauenswürdigen ebenfalls.

Diese Art von Autorisierung ist üblich Daemons und Dienstkonten, die Zugriff auf Daten Besitz Consumer Benutzer mit persönlichen Microsoft-Konten müssen.  Daten von Organisationen empfiehlt es sich, dass Sie die erforderliche Autorisierung über Anwendung Perimssions erwerben.

### <a name="application-permissions"></a>Von Anwendungsberechtigungen
Statt ACLs verwenden, können APIs ein Satzes von **Anwendungsberechtigungen** verfügen, die eine Anwendung gewährt werden können.  Eine Anwendungsberechtigung zur Anwendung von einem Administrator einer Organisation gewährt wird, und kann nur Zugriff auf die Organisation und ihre Mitarbeiter im Besitz Daten verwendet werden.  Zum Beispiel stellt die Microsoft Graph mehrere Anwendungsberechtigungen zur Verfügung:

- Lesen von e-Mail in alle Postfächer
- Lesen und Schreiben von e-Mail in alle Postfächer
- Senden einer e-Mail als jeder Benutzer
- Lesen Sie Directory-Daten
- [+ Weitere](https://graph.microsoft.io)

Damit diese Berechtigungen in Ihrer app zu erfassen, können Sie die folgenden Schritte ausführen.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Anfordern der Berechtigungen in der Registrierung app-portal

- Navigieren Sie zu Ihrer Anwendung in [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), oder [Erstellen Sie eine app](active-directory-v2-app-registration.md) , wenn Sie noch nicht geschehen ist.  Sie müssen Sie sicherstellen, dass eine Anwendung mindestens eine Anwendung geheim erstellt hat.
- Suchen Sie im Abschnitt **Berechtigungen für direkte Anwendung** , und fügen Sie die Berechtigungen, die Ihre app erforderlich sind.
- Vergewissern Sie sich zum **Speichern** der app-Registrierung

#### <a name="recommended-sign-the-user-into-your-app"></a>Empfohlen: Melden Sie sich den Benutzer bei der app

Normalerweise beim Erstellen einer Anwendung, die Anwendungsberechtigungen verwendet, wird die app muss eine Seitenansicht sein, mit dem der Administrator des app Berechtigungen genehmigen können.  Auf dieser Seite kann Teil des app Anmeldung Fluss, Teil des app Einstellungen oder eine dedizierte "Verbinden" Fluss sein.  In vielen Fällen ist es sinnvoll, die für die app folgt anzeigen "anzeigen verbinden" erst ein Benutzers mit einem Arbeit oder Schule Microsoft-Konto angemeldet hat.

Bei der Anmeldung des Benutzers in die app, können Sie die Organziation zu identifizieren, zu der der Benutzer gehört, bevor Sie bitten, die Berechtigungen der Anwendung zu genehmigen.  Während nicht unbedingt erforderlich ist, können sie eine weitere intuitive Benutzeroberfläche für Ihre Organisation Benutzer erstellen helfen.  Wenn den Benutzer anmelden möchten, führen Sie unsere [Version 2.0-Protokoll Lernprogramme](active-directory-v2-protocols.md)aus.

#### <a name="request-the-permissions-from-a-directory-admin"></a>Beantragen Sie die Berechtigungen eines Administrators Verzeichnis

Wenn Sie bereit sind Anfrage-Berechtigungen aus dem Administrator des Unternehmens, können Sie den Benutzer in der Version 2.0 **Administrator Endpunkt Zustimmung**umleiten.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro Tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| Mandanten | Erforderlich | Das Directory-Mandanten, dem Sie anfordern der Genehmigung von möchten.  Kann in Guid oder Anzeigename Format bereitgestellt werden.  Wenn Sie nicht, welche Mandanten, die der Benutzer gehört wissen, und möchten, lassen sie die melden Sie sich mit einem beliebigen Mandanten, verwenden Sie `common`. |
| client_id | Erforderlich | Die Anwendung-Id, dass das Registrierung-Portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihre app zugewiesen. |
| redirect_uri | Erforderlich | Die Redirect_uri, werden die Antwort für Ihre app zu behandeln gesendet werden soll.  Es muss exakt eine der Redirect_uris übereinstimmen, die Sie im Portal registriert, außer es Url codiert werden und kann zusätzliche Pfadsegmente muss. |
| Bundesstaat | empfohlen | Einen Wert enthalten, in der Besprechungsanfrage, die auch in der token Antwort zurückgegeben wird.  Es kann eine Textzeichenfolge alle Inhalte, die Sie möchten.  Der Status wird verwendet, um die Informationen zu den Status des Benutzers in der app codieren, bevor die Authentifizierungsanfrage ist, beispielsweise die Seite oder die Ansicht, die sie aufgetreten auf waren. |

An diesem Punkt wird Azure AD erzwingen, dass nur mandantenadministrator sich anmelden kann, um die Anforderung abzuschließen.  Der Administrator werden aufgefordert, alle Anwendungsberechtigungen direkte genehmigen, die Sie für Ihre app im Portal Registrierung angefordert haben. 

##### <a name="successful-response"></a>Erfolgreiche Antwort
Wenn der Administrator die Berechtigungen für eine Anwendung genehmigt hat, wird die erfolgreiche Antwort werden:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| Mandanten | Das Directory-Mandanten, der die Berechtigungen für einer Anwendung gewünscht, im Guid-Format. |
| Bundesstaat | Einen Wert enthalten, in der Besprechungsanfrage, die auch in der token Antwort zurückgegeben wird.  Es kann eine Textzeichenfolge alle Inhalte, die Sie möchten.  Der Status wird verwendet, um die Informationen zu den Status des Benutzers in der app codieren, bevor die Authentifizierungsanfrage ist, beispielsweise die Seite oder die Ansicht, die sie aufgetreten auf waren. |
| admin_consent | Wird auf festgelegt `True`. |


##### <a name="error-response"></a>Antwort zurück
Der Administrator die Berechtigungen für eine Anwendung nicht genehmigt, werden die Fehler beim Antwort:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| Fehler | Eine Zeichenfolge des Fehlercodes, die zum Fehlertypen klassifizieren, die auftreten verwendet werden kann, und kann verwendet werden, um auf Fehler zu reagieren. |
| error_description | Eine bestimmte Fehlermeldung, die einen Entwickler die Ursache eines Fehlers ermitteln helfen können.  |

Nachdem Sie eine erfolgreiche Antwort von der app bereitgestellt Endpunkt erhalten haben, gewonnen Ihre app hat die direkte Anwendungsberechtigungen, die sie dazu aufgefordert.  Sie können jetzt verschieben auf ein Token für die gewünschte Ressource anfordert.

## <a name="get-a-token"></a>Ein Token abrufen
Nachdem Sie die erforderliche Autorisierung für eine Anwendung erworben haben, können Sie fortfahren mit dem Erwerb von Access Token für APIs.  Um ein Token über den Client Anmeldeinformationen erteilen erhalten, senden Sie eine Besprechungsanfrage Beitrag zu den `/token` Version 2.0-Endpunkt:

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| client_id | Erforderlich | Die Anwendung-Id, dass das Registrierung-Portal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihre app zugewiesen. |
| Bereich | Erforderlich | Der Wert für die `scope` Parameter in dieser Anforderung sollten den Resource Identifier (URI für App-ID) der gewünschten Ressource, angebracht, mit der `.default` Suffix.  Damit beispielsweise Microsoft Graph angegeben sein sollten `https://graph.microsoft.com/.default`.  Dieser Wert informiert dem Endpunkt Version 2.0, der alle direkten Anwendungsberechtigungen, die Sie für Ihre app konfiguriert haben, es ein die Schriftarten in Verbindung mit der gewünschten Ressource Gruppenrichtlinien auszustellen sollte. |
| client_secret | Erforderlich | Die Anwendung geheim, die Sie in der Registrierung Portal für Ihre app generiert. |
| grant_type | Erforderlich | Muss `client_credentials`. | 

#### <a name="successful-response"></a>Erfolgreiche Antwort
Eine erfolgreiche Antwort dauert das Formular an:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| access_token | Die angeforderten Zugriffstoken. Dieses Token können die app für die geschützte Ressourcen, z. B. ein Web-API authentifizieren. |
| token_type | Gibt den Wert des Sicherheitstokens an. Der einzige Typ, Azure AD-unterstützt ist `Bearer`.  |
| expires_in | Wie lange das Access-Token (in Sekunden) gültig ist. |

#### <a name="error-response"></a>Antwort zurück
Eine Fehlerantwort dauert das Formular an:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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

## <a name="use-a-token"></a>Verwenden Sie ein token
Jetzt, da Sie ein Token erworben haben, können Sie diese Token anzufordern, die der Ressource verwenden.  Wenn die Sicherheitstoken abläuft, wiederholen Sie einfach die Anforderung an die `/token` Endpunkt, ein Zugriffstoken frisch zu erhalten.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro Tip: Try the below command out! (but replace the token with your own)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## <a name="code-sample"></a>Beispiel für Code
Wenn Sie ein Beispiel für eine Anwendung anzeigen möchten, dass implementiert, die die Client_credentials erteilen mithilfe des Administrators Endpunkt Zustimmung, finden Sie in unseren [Version 2.0 Daemon Code Stichprobe](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
