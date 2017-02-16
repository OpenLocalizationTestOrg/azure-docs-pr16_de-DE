<properties
    pageTitle="Azure AD-.NET Protokoll Übersicht | Microsoft Azure"
    description="Dieser Artikel beschreibt, wie Sie HTTP-Nachrichten, um die Autorisierung des Zugriffs auf Webanwendungen und Web-APIs in Ihrem Mandanten mithilfe von Azure Active Directory und OAuth 2.0."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="authorize-access-to-web-applications-using-oauth-20-and-azure-active-directory"></a>Autorisierung des Zugriffs auf OAuth 2.0 und Azure Active Directory mithilfe von Webanwendungen

Azure Active Directory (Azure AD) verwendet OAuth 2.0, um die Autorisierung des Zugriffs auf Webanwendungen und Web-APIs in Ihrem Azure AD-Mandanten aktivieren. Dieses Handbuch Sprache unabhängig ist, und beschreibt, wie Sie senden und Empfangen von HTTP-Nachrichten ohne unsere öffnen Source-Bibliotheken.

Illustrieren OAuth 2.0 Autorisierung Code wird im [Abschnitt 4.1 der OAuth 2.0-Spezifikation](https://tools.ietf.org/html/rfc6749#section-4.1) beschrieben. Hiermit wird in den meisten app Dateitypen, einschließlich der Web apps und systembedingt installierten apps Authentifizierung und Autorisierung durchzuführen.

[AZURE.INCLUDE [active-directory-protocols-getting-started](../../includes/active-directory-protocols-getting-started.md)]


## <a name="oauth-20-authorization-flow"></a>OAuth 2.0 Autorisierung Fluss

Auf hoher Ebene sieht der gesamte Autorisierung Fluss für eine Anwendung etwas wie folgt aus:

![OAuth autorisierende Code Fluss](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)


## <a name="request-an-authorization-code"></a>Anfordern eines Genehmigung Codes

Autorisierung Code illustrieren beginnt mit dem Client, leiten den Benutzer, zu der `/authorize` Endpunkt. Der Client in dieser Anforderung zeigt an, dass die Berechtigungen, die es vom Benutzer abrufen muss. Sie können die Endpunkte OAuth 2.0 von Ihrer Anwendung klassischen Azure-Portal in die **Ansicht Endpunkte** Schaltfläche in der unteren Einzug erstellen.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| Mandanten | Erforderlich | Die `{tenant}` Wert in den Pfad der Anforderung zum Steuern, wer bei der Anwendung anmelden kann verwendet werden kann.  Die zulässigen Werte sind Mandanten Bezeichnern, z. B. `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` oder `contoso.onmicrosoft.com` oder `common` für den Mandanten unabhängig Token |
| client_id | Erforderlich | Die Anwendung-Id zu Ihrer Anwendung zugewiesen werden, wenn Sie ihn mit Azure AD registriert. Sie können dies im Verwaltungsportal Azure suchen. Klicken Sie auf **Active Directory**, klicken Sie auf das Verzeichnis, klicken Sie auf die Anwendung und klicken Sie dann auf **Konfigurieren** |
| response_type | Erforderlich | Darf enthalten `code` für die Autorisierung Code Fluss. |
| redirect_uri | empfohlen | Die Redirect_uri der app, wo Authentifizierungsantworten gesendet und Empfangen von Ihrer app werden können.  Es muss exakt eine der Redirect_uris übereinstimmen, die Sie im Portal registriert, außer es Url codiert werden muss.  Für systemeigene und mobile-apps, sollten Sie des Standardwerts für `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode | empfohlen | Gibt die Methode, die verwendet werden soll, das sich daraus ergebende token zurück zu Ihrer Anwendung zu senden.  Kann sein `query` oder `form_post`.  |
| Bundesstaat | empfohlen | Einen Wert enthalten, in der Besprechungsanfrage, die auch in der token Antwort zurückgegeben wird. Ein eindeutiger erzeugten Wert wird in der Regel für die [websiteübergreifende Anforderungsfälschungsangriffe verhindern](http://tools.ietf.org/html/rfc6749#section-10.12)verwendet.  Der Status wird auch Informationen zu den Status des Benutzers in der app codieren, bevor die Authentifizierungsanfrage ist, beispielsweise die Seite oder die Ansicht, die sie aufgetreten auf Waren, verwendet. |
| Ressource | Optional | Der App-ID-URI des Web-API (gesicherte Ressource). Um die App-ID-URI des Web-API im Verwaltungsportal Azure finden Sie klicken Sie auf **Active Directory**, klicken Sie auf das Verzeichnis, klicken Sie auf die Anwendung, und klicken Sie dann auf **Konfigurieren**. |
| Aufforderung | Optional |  Geben Sie den Typ der Interaktion mit dem Benutzer, die erforderlich ist.<p> Gültige Werte sind: <p> *Benutzername*: der Benutzer neu authentifiziert aufgefordert werden sollte. <p> *Zustimmung*: Zustimmung des Benutzers erteilt wurde, aber aktualisiert werden soll. Der Benutzer sollte aufgefordert, Zustimmung. <p> *Admin_consent*: ein Administrator sollte für alle Benutzer in ihrer Organisation Zustimmung aufgefordert werden |
| login_hint | Optional | Kann verwendet werden um vorab füllen Sie das Feld Benutzername/e-Mail-Adresse von der Anmeldeseite für den Benutzer, wenn Sie ihren Benutzernamen im Voraus kennen.  Häufig apps werden für diesen Parameter verwenden, während eine erneute Authentifizierung, haben Sie bereits den Benutzernamen aus einem vorherigen Anmeldung mit extrahiert die `preferred_username` beanspruchen. |
| domain_hint | Optional | Stellt einen Hinweis zu den Mandanten oder die Domäne, die der Benutzer zur Anmeldung verwendet werden sollen. Der Wert von der Domain_hint ist eine eingetragene Domäne für den Mandanten. Wenn Sie der Mandanten zu einem lokalen Verzeichnis verbunden ist, leitet AAD auf dem angegebenen Mandanten Föderation Server. |

> [AZURE.NOTE] Ist der Benutzer Mitglied einer Organisation, kann ein Administrator der Organisation Zustimmung oder ablehnen auf den Namen des Benutzers oder zulassen den Benutzer Zustimmung. Der Benutzer erhält die Möglichkeit, Zustimmung nur, wenn der Administrator dies zulässt.

Zu diesem Zeitpunkt der Benutzer aufgefordert werden, geben Sie ihre Anmeldeinformationen und Zustimmung zu den Berechtigungen, der der `scope` Abfrage Parameter. Nachdem der Benutzer authentifiziert und Zustimmung erteilt, sendet Azure AD eine Antwort auf Ihre app bei der `redirect_uri` in der Adresse.

### <a name="successful-response"></a>Erfolgreiche Antwort

Eine erfolgreiche Antwort könnte wie folgt aussehen:

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parameter | Beschreibung |
| -----------------------| --------------- |
| admin_consent | Der Wert ist wahr, wenn ein Administrator eine Zustimmung Anforderung Aufforderung zugestimmt.|
| Code | Die Autorisierungscode, den die Anwendung angefordert hat. Die Anwendung können den Autorisierungscode in einer Access-Token für die Zielressource anfordern. |
| session_state | Ein eindeutiger Wert, der die aktuelle Sitzung des Benutzers ausgeführt werden. Dieser Wert ist eine GUID, aber als einen undurchsichtig zurück, der ohne Prüfung weitergegeben wird behandelt werden muss. |
| Bundesstaat | Wenn der Parameter Status in der Besprechungsanfrage enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Es ist sinnvoll, dass die Anwendung, um sicherzustellen, dass die Statuswerte in die Anforderung und Antwort identisch sind, bevor Sie verwenden die Antwort. Auf diese Weise [websiteübergreifende anfordern Fälschung (CSRF) Angriffen](https://tools.ietf.org/html/rfc6749#section-10.12) gegen den Client erkennen können.  

### <a name="error-response"></a>Antwort zurück

Fehler beim Antworten möglicherweise auch gesendet werden, mit der `redirect_uri` , damit die Anwendung sie angemessen behandelt werden kann.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschreibung |
|-----------|-------------|
| Fehler | Im Abschnitt 5.2 von [OAuth 2.0 Autorisierung Framework](http://tools.ietf.org/html/rfc6749)definierten Wert einer Fehlercodes. Die nächste Tabelle beschreibt die Fehlercodes, die Azure AD zurückgibt. |
| error_description | Eine detaillierte Beschreibung des Fehlers. Diese Meldung nicht erhebt auf Endbenutzer freundlichen. |
| Bundesstaat | Der Statuswert ist ein erzeugten nicht wiederverwendet Wert, der in der Anforderung gesendet und in der Antwort zu verhindern, dass websiteübergreifende (CSRF) Anforderungsfälschungsangriffe zurückgegeben. |

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

## <a name="use-the-authorization-code-to-request-an-access-token"></a>Verwenden Sie den Autorisierungscode in einer Access-Token anfordern

Jetzt, da Sie haben einen Autorisierungscode erworben haben und Zugriff gewährt wurde durch den Benutzer, können Sie den Code für eine Access-Token auf die gewünschte Ressource einlösen, per POST-Anforderung an das `/token` Endpunkt:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%2Fmyapp%2F
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------------- |
| Mandanten | Erforderlich |  Die `{tenant}` Wert in den Pfad der Anforderung zum Steuern, wer bei der Anwendung anmelden kann verwendet werden kann.  Die zulässigen Werte sind Mandanten Bezeichnern, z. B. `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` oder `contoso.onmicrosoft.com` oder `common` für den Mandanten unabhängig Token |
| client_id | Erforderlich | Die Anwendung-Id zu Ihrer Anwendung zugewiesen werden, wenn Sie ihn mit Azure AD registriert. Sie können dies im klassischen Azure-Portal suchen. Klicken Sie auf **Active Directory**, klicken Sie auf das Verzeichnis, klicken Sie auf die Anwendung und klicken Sie dann auf **Konfigurieren** |
| grant_type | Erforderlich | Muss `authorization_code` für die Autorisierung Code Fluss. |
| Code | Erforderlich | Die `authorization_code` , die Sie im vorherigen Abschnitt erworben   |
| redirect_uri | Erforderlich | Die gleiche `redirect_uri` Wert, verwendet wurde, erhalten, die `authorization_code`. |
| client_secret | für den Web apps erforderlich ist | Die Anwendung geheim, die Sie in der Registrierung app-Portal für Ihre app erstellt haben.  Es sollte nicht in einer systemeigenen app verwendet werden, da Client_secrets zuverlässig auf Geräten gespeichert werden kann.  Es ist erforderlich für Web apps und Web-APIs, die die Möglichkeit zum Speichern der `client_secret` sicher auf dem Server. |
| Ressource | erforderlich, wenn in Autorisierung Anforderung des Codes, andere optional angegeben | Der App-ID-URI des Web-API (gesicherte Ressource).
Um den App-ID-URI im Verwaltungsportal Azure zu finden, klicken Sie auf **Active Directory**, klicken Sie auf das Verzeichnis, klicken Sie auf die Anwendung und klicken Sie dann auf **Konfigurieren**.

### <a name="successful-response"></a>Erfolgreiche Antwort

Azure AD gibt ein Zugriffstoken auf eine erfolgreiche Antwort an. Um Netzwerk Anrufe aus der Clientanwendung und deren zugeordneten Wartezeit zu minimieren, sollte die Clientanwendung Access Token für die Gültigkeitsdauer token cache, die in der Antwort OAuth 2.0 angegeben ist. Um die Gültigkeitsdauer token zu ermitteln, verwenden Sie entweder die `expires_in` oder `expires_on` Parameterwerte.

Wenn eine Web-API Ressource gibt eine `invalid_token` Fehlercode, dies kann darauf hinweisen, dass die Ressource festgestellt, dass das Token abgelaufen ist. Wenn der Client und Ressourcen für die Uhrzeit unterschiedliche (auch bekannt als "Schiefe Zeit") sind, sollten die Ressource das Token abgelaufen zu sein, bevor das Token aus dem Clientcache deaktiviert ist. In diesem Fall deaktivieren Sie das Token aus dem Cache, auch wenn es immer noch in der berechneten Lebensdauer ist.

Eine erfolgreiche Antwort könnte wie folgt aussehen:

```
{
  "access_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
"id_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.”
}

```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| access_token | Die angeforderten Zugriffstoken. Dieses Token können die app für die geschützte Ressourcen, z. B. ein Web-API authentifizieren. |
| token_type | Gibt den Wert des Sicherheitstokens an. Person ist nur der Typ, der Azure AD unterstützt. Weitere Informationen zu der Person Token, finden Sie unter [OAuth2.0 Autorisierung Framework: Person Token Verwendung (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt)  |
| expires_in | Wie lange das Access-Token (in Sekunden) gültig ist. |
| expires_on | Die Zeit, wenn das Access-Token abgelaufen ist. Das Datum wird als die Anzahl der Sekunden zwischen 1970 dargestellt-01-01T0:0:0Z UTC erst Ablauf. Dieser Wert wird verwendet, um die Gültigkeitsdauer des zwischengespeicherten Token zu bestimmen. |
| Ressource | Der App-ID-URI des Web-API (gesicherte Ressource).|
| Bereich | Identitätswechsel Berechtigungen an die Clientanwendung. Ist die Berechtigung standardmäßig `user_impersonation`. Der Besitzer der gesicherte Ressource kann zusätzliche Werte in Azure AD zu registrieren.|
| refresh_token |  Ein OAuth 2.0 aktualisieren Token. Dieses Token können die app Erfassen von zusätzlichen Zugriffstoken nach das aktuellen Access Token läuft ab.  Aktualisieren von Token langer Lebensdauer sind, und können verwendet werden, um den Zugriff auf Ressourcen für längere Zeit beibehalten. |
| id_token | Eine nicht signierte JSON Web Token (JWT). Die app kann base64Url entschlüsseln die Segmente des dieses Token das Anfordern von Informationen über den Benutzer, die sich angemeldet haben. Die app können Sie die Werte im cache und anzeigen, aber es dürfen nicht auf diese verlassen, Autorisierung oder Begrenzung Sicherheit. |

### <a name="jwt-token-claims"></a>JWT Token Ansprüche
Das Token JWT im Wert von der `id_token` Parameter in den folgenden Ansprüche decodiert werden kann:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

Die `id_token` Parameter umfasst die folgenden Arten von anfordern. Weitere Informationen zu JSON Web Token finden Sie unter der [JWT IETF Entwurfsspezifikation](http://go.microsoft.com/fwlink/?LinkId=392344). Weitere Informationen zu den Arten von token und Ansprüche lesen Sie [Token unterstützt sowie Ansprüche](active-directory-token-and-claims.md)

| Typ beanspruchen | Beschreibung |
|------------|-------------|
| also | Zielgruppe für das Token. Wenn das Token an eine Clientanwendung ausgegeben wird, ist das Publikum die `client_id` des Clients.
| EXP | Der Ablaufzeit. Die Zeit, wenn das Token abgelaufen ist. Für das Token gültig sein, die aktuelle Uhrzeit muss kleiner als oder gleich der `exp` Wert. Die Zeit wird als die Anzahl der Sekunden dargestellt, aus dem 1. Januar 1970 (1970-01-01T0:0:0Z) UTC bis zum Zeitpunkt das Token ausgestellt wurde. |
| family_name | Des Benutzers Nachnamen oder Nachnamen. Die Anwendung kann diesen Wert anzeigen. |
| angegebener_Name | Vorname des Benutzers. Die Anwendung kann diesen Wert anzeigen. |
| IAT | Zeitpunkt ausgestellt. Die Uhrzeit, wann die JWT ausgestellt wurde. Die Zeit wird als die Anzahl der Sekunden dargestellt, aus dem 1. Januar 1970 (1970-01-01T0:0:0Z) UTC bis zum Zeitpunkt das Token ausgestellt wurde. |
| ISS | Kennzeichnet den token Herausgeber |
| NBF | Frühestens Zeit. Die Zeit, wenn das Token wirksam wird. Für das Token gültig ist muss die aktuelle Uhrzeit größer als oder gleich dem Wert Nbf. Die Zeit wird als die Anzahl der Sekunden dargestellt, aus dem 1. Januar 1970 (1970-01-01T0:0:0Z) UTC bis zum Zeitpunkt das Token ausgestellt wurde. |
| OID | Objekt-ID (ID) des Benutzerobjekts in Azure AD-. |
| Sub | Bezeichner für Token Betreff. Dies ist ein beständiger und unveränderlich Bezeichner für den Benutzer, den das Token beschreibt. Verwenden Sie diesen Wert in Logik Zwischenspeichern. |
| TID | Mandanten Sie Bezeichner (ID), der den Azure AD-Mandanten, der das Token ausgestellt. |
| unique_name | Der Benutzer kann ein eindeutiger Bezeichner für das angezeigt werden. Dies ist in der Regel ein Benutzer Benutzerprinzipalnamen (UPN). |
| Benutzerprinzipalnamen | Hauptbenutzer Benutzername des Benutzers. |
| Version | Version. Die Version des JWT Token normalerweise 1.0. |

### <a name="error-response"></a>Antwort zurück

Token Emission Endpunkt Fehler sind HTTP-Fehlercodes, da der Client direkt den Endpunkt token Emission Anrufe. Zusätzlich zu den HTTP-Statuscode gibt der Azure AD-token Emission Endpunkt auch ein Dokument JSON-Objekte, die den Fehler zu beschreiben.

Eine Antwort der Stichprobe Fehler könnte wie folgt aussehen:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: The provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Zeichenfolge des Fehlercodes, die zum Fehlertypen klassifizieren, die auftreten verwendet werden kann, und kann verwendet werden, um auf Fehler zu reagieren. |
| error_description | Eine bestimmte Fehlermeldung, die einen Entwickler die Ursache eines Authentifizierungsfehlers ermitteln helfen können.  |
| error_codes | Eine Liste der STS bestimmte Fehlercodes, die bei der Diagnose helfen können. |
| Zeitstempel | Die Uhrzeit, an der der Fehler aufgetreten ist. |
| _id | Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose helfen können.  |
| correlation_id | Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose über Komponenten helfen können.|

#### <a name="http-status-codes"></a>HTTP-Status-codes

Die folgende Tabelle enthält die HTTP Statuscodes, die der Endpunkt token Emission zurückgibt. In einigen Fällen der Fehlercode reicht aus, um die Antwort zu beschreiben, aber im Fall eines Fehlers, müssen Sie Analysieren der zugehörigen JSON-Dokuments und den Fehlercode untersuchen.

| HTTP-Code | Beschreibung |
|-----------|-------------|
| 400       | Standardmäßige HTTP-Code. In den meisten Fällen verwendet und wird meist durch eine fehlerhafte Anforderung. Beheben, und senden Sie die Anfrage erneut. |
| 401       | Fehler bei der Authentifizierung. Die Anforderung, beispielsweise den Parameter Client_secret fehlen.|
| 403       | Fehler bei der Autorisierung. Beispielsweise verfügt der Benutzer nicht über die Berechtigung zum Zugriff auf die Ressource. |
| 500       | Interner Fehler bei der Dienst. Wiederholen Sie die Anforderung. |

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

## <a name="use-the-access-token-to-access-the-resource"></a>Verwenden Sie das Access-Token Zugriff auf die Ressource

Sie erfolgreich erworben haben, haben Sie nun eine `access_token`, können Sie das Token in Anfragen auf Web-APIs, indem Sie ihn in den `Authorization` Kopfzeile. Die Spezifikation [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) erläutert, wie Sie die Person Token Zugriff auf geschützte Ressourcen in HTTP-Anfragen zu verwenden.

### <a name="sample-request"></a>Beispiel für eine Anforderung

```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### <a name="error-response"></a>Antwort zurück

Gesicherte Ressourcen, die RFC 6750 Problem HTTP Statuscodes implementieren. Wenn die Anforderung keine Anmeldeinformationen für die Authentifizierung umfasst oder das Token fehlt, wird die Antwort enthält eine `WWW-Authenticate` Kopfzeile. Wenn eine Anforderung fehlschlägt, reagiert der Ressourcenserver mit den Code ein HTTP-Status und dem Fehlercode.

Folgendes ist ein Beispiel für eine Antwort nicht erfolgreich, wenn das Person Token nicht in die Clientanfrage enthalten ist:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.window.net/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### <a name="error-parameters"></a>Fehlerparameter

| Parameter | Beschreibung |
|-----------|-------------|
| authorization_uri | Der URI (physische Endpunkt) des Servers Autorisierung. Dieser Wert wird auch als ein Schlüssel zum Nachschlagen verwendet, um weitere Informationen zu den Server von einem Discovery-Endpunkt zu erhalten. <p><p> Der Client muss überprüfen Sie, ob der Server für die Autorisierung vertrauenswürdig ist. Wenn die Ressource durch Azure AD geschützt ist, ist es ausreichend, stellen Sie sicher, dass der URL beginnt mit https://login.windows.net oder einem anderen Hostname, die Azure AD unterstützt. Eine Ressource Mandanten-spezifische sollte immer eine Mandanten-spezifische Autorisierung URI zurückgeben. |
| Fehler | Im Abschnitt 5.2 von [OAuth 2.0 Autorisierung Framework](http://tools.ietf.org/html/rfc6749)definierten Wert einer Fehlercodes.|
| error_description | Eine detaillierte Beschreibung des Fehlers. Diese Meldung nicht erhebt auf Endbenutzer freundlichen.|
| resource_id | Gibt den eindeutigen Bezeichner für die Ressource an. Die Clientanwendung kann diese Kennung verwenden, als Wert des der `resource` Parameter, wenn es ein Token für die Ressource anfordert. <p><p> Es ist sehr wichtig für die Clientanwendung, um diesen Wert zu überprüfen, andernfalls wird ein bösartiger Dienst möglicherweise **Erhöhung von Berechtigungen** Angriffen auslösen <p><p> Die empfohlene Strategie für ein verhindern von Angriffen besteht darin, zu überprüfen, ob die `resource_id` entspricht die Basis des im Web-URL-API zugegriffen wird. Wenn https://service.contoso.com/data zugegriffen wird, beispielsweise die `resource_id` htttps://service.contoso.com/ sind möglich. Die Clientanwendung muss ablehnen einer `resource_id` , die nicht mit beginnt der Basis-URL, wenn es eine zuverlässige alternative Möglichkeit zur Überprüfung der Id. |

#### <a name="bearer-scheme-error-codes"></a>Person Farbschema Fehlercodes

Die RFC 6750-Spezifikation definiert folgender Fehler für Ressourcen, die mit der WWW-Authenticate-Header und das Schema der Person in der Antwort verwenden.

| HTTP-Status Code | Fehlercode | Beschreibung | Clientaktion |
|------------------|------------|-------------|---------------|
| 400 | invalid_request | Die Anforderung ist nicht ordnungsgemäß formatiert. Angenommen, sie möglicherweise werden fehlen ein Parameters oder verwenden den gleichen Parameter zweimal. | Beheben Sie des Fehlers, und wiederholen Sie die Anforderung. Diese Art von Fehler sollte nur bei der Entwicklung auftreten und in der ersten Tests erkannt. |
| 401 | invalid_token   | Das Access-Token fehlt, ungültige oder gesperrt wird. Der Wert des Parameters Error_description bietet zusätzlichen Details. |  Anfordern eines neuen Tokens vom Server Autorisierung an. Wenn das neue Token fehlschlägt, ist ein unerwarteter Fehler aufgetreten. Senden Sie eine Fehlermeldung an die Benutzer und "Wiederholen" nach zufällige verzögert. |
| 403 | insufficient_scope | Das Access-Token enthält keine Identitätswechsel welche Berechtigungen zum Zugriff auf die Ressource erforderlich sind. | Senden Sie eine neue Autorisierung Anforderung an den Endpunkt Autorisierung an. Wenn die Antwort im Bereichsparameter enthält, verwenden Sie den Bereichswert in der Besprechungsanfrage, die der Ressource |
| 403 | insufficient_access | Der Betreff der das Token verfügt nicht die Berechtigungen, die erforderlich sind, auf die Ressource zuzugreifen. | Der Benutzer ein anderes Konto verwenden oder Anfordern von Berechtigungen für die Ressource angegebenen aufgefordert werden. |

## <a name="refreshing-the-access-tokens"></a>Aktualisieren die Access-Token

Access Token sind kurzlebige und müssen aktualisiert werden, nachdem sie ablaufen, um den Vorgang fortzusetzen, den Zugriff auf Ressourcen. Können Sie aktualisieren die `access_token` durch das Senden von einer anderen `POST` zum Anfordern der `/token` Endpunkt, aber dieses Mal Bereitstellen der `refresh_token` statt der `code`.

Aktualisieren von Token verfügen nicht über die angegebene Gültigkeitsdauer. Üblicherweise sind die Lebensdauer der Aktualisierung Token relativ lange. Jedoch in einigen Fällen aktualisieren Token abläuft, werden widerrufen oder verfügen nicht über ausreichende Berechtigungen für die gewünschte Aktion. Eine Anwendung muss erwarten und den Endpunkt token Emission ordnungsgemäß zurückgegebene Fehler zu behandeln.

Wenn Sie eine Antwort mit einem token aktualisieren-Fehler erhalten, das aktuelle aktualisieren Token verwerfen und einen neuen Autorisierungscode anfordern oder Token zugreifen. Insbesondere wenn mithilfe einer Aktualisierung token im Fluss Autorisierung Code erteilen, wenn Sie eine Antwort mit erhalten die `interaction_required` oder `invalid_grant` Fehlercodes, das Aktualisieren Token verwerfen und einen neuen Autorisierungscode anfordern.

Eine Stichprobe anfordern an den **Mandanten-spezifische** Endpunkt (Sie können auch den Endpunkt **Allgemeine** ) um zu erhalten, ein neues Access token mit einer Aktualisierung Token wie folgt aus:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```
| Parameter | Beschreibung |
|-----------|-------------|
| access_token | Das neue Access-Token, das angefordert wurde.|
| expires_in   | Gültigkeitsdauer das Token in Sekunden. Ein häufig angegebener Wert ist 3600 (eine Stunde). |
| expires_on   | Datum und Uhrzeit, an dem das Token abläuft. Das Datum wird als die Anzahl der Sekunden zwischen 1970 dargestellt-01-01T0:0:0Z UTC erst Ablauf. |
| refresh_token | Eine neue OAuth 2.0-Refresh_token, die verwendet werden können, um neue Access Token anfordern, wenn Sie das Schema in dieser Antwort läuft ab. |
| Ressource     | Identifiziert die gesicherte Ressource, die das Access-Token sein kann in Access verwendet wird. |
| Bereich        | Identitätswechsel Berechtigungen für die native Client-Anwendung. Die Standardberechtigung ist **User_impersonation**. Der Besitzer der Zielressource kann alternative Werte in Azure AD zu registrieren. |
| token_type   | Der token Typ. Der einzige unterstützte Wert ist **Person**an. |

### <a name="successful-response"></a>Erfolgreiche Antwort

Wie wird eine erfolgreiche token Antwort aus:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```

### <a name="error-response"></a>Antwort zurück

Eine Antwort der Stichprobe Fehler könnte wie folgt aussehen:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant.  You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Zeichenfolge des Fehlercodes, die zum Fehlertypen klassifizieren, die auftreten verwendet werden kann, und kann verwendet werden, um auf Fehler zu reagieren. |
| error_description | Eine bestimmte Fehlermeldung, die einen Entwickler die Ursache eines Authentifizierungsfehlers ermitteln helfen können.  |
| error_codes | Eine Liste der STS bestimmte Fehlercodes, die bei der Diagnose helfen können. |
| Zeitstempel | Die Uhrzeit, an der der Fehler aufgetreten ist. |
| _id | Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose helfen können.  |
| correlation_id | Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose über Komponenten helfen können.|

Eine Beschreibung der Fehlercodes und die Aktion empfohlene Client finden Sie unter [Fehlercodes token Endpunkt Fehler](#error-codes-for-token-endpoint-errors).
