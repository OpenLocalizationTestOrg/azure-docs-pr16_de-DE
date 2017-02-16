<properties
    pageTitle="Azure-Active Directory-B2C: Verwenden Sie den Graph-API | Microsoft Azure"
    description="So rufen Sie die Graph-API für einen Mandanten B2C mithilfe einer Anwendungsidentität automatisieren."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-use-the-graph-api"></a>Azure AD-B2C: Verwenden Sie den Graph-API

Azure Active Directory (Azure AD) B2C Mandanten meist sehr groß sein. Dies bedeutet, dass viele häufige Mandanten Verwaltungsaufgaben programmgesteuert ausgeführt werden müssen. Ein Beispiel für die primäre ist Benutzermanagement. Möglicherweise müssen einen vorhandenen Benutzerspeicher auf einen Mandanten B2C migrieren. Sie möchten möglicherweise hosten Registrierung klicken Sie auf Ihrer eigenen Seite Benutzer und Erstellen von Benutzerkonten in Azure AD Hintergrundinformationen. Diese Arten von Aufgaben benötigen die Möglichkeit zum Erstellen, lesen, aktualisieren und Löschen von Benutzerkonten. Sie können diese Aufgaben mithilfe der Azure AD Graph-API ausführen.

Für B2C Mandanten gibt es zwei primäre Modi zum Kommunizieren mit der Graph-API.

- Für Vorgänge interaktive, einmalige ausführen sollten Sie dienen als einem Administratorkonto im B2C Mandanten, wenn Sie die Aufgaben ausführen. In diesem Modus müssen einen Administrator voraus, melden Sie sich mit den Anmeldeinformationen vor diesem Administrator alle Anrufe an die Graph-API ausführen kann.
- Für automatische, fortlaufende Vorgänge sollten Sie einige Typ des Dienstkontos verwenden, dass Sie die erforderlichen Berechtigungen zum Durchführen von Verwaltungsaufgaben zur Verfügung zu stellen. In Azure AD können Sie hierzu eine Anwendung registrieren und Azure AD authentifizieren. Dies geschieht mithilfe einer **Anwendung-ID** , mit der [Anmeldeinformationen OAuth 2.0-Clients erteilen](../active-directory/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api). In diesem Fall fungiert die Anwendung als sich selbst, nicht als ein Benutzer, der Graph-API aufrufen.

So führen Sie die automatische Anwendungsfall-wird in diesem Artikel erläutert. Um veranschaulichen, können wir eine .NET 4.5 erstellen `B2CGraphClient` , die Benutzer ausführt erstellen, lesen, aktualisieren und Löschen von Vorgänge. Der Client muss eine Windows line Interface (CLI), die Sie zum Aufrufen von verschiedenen Methoden ermöglicht. Der Code wird jedoch in einer nicht interaktiven, automatisierte Weise Verhalten geschrieben.

## <a name="get-an-azure-ad-b2c-tenant"></a>Abrufen von einem Mandanten Azure AD B2C

Bevor Sie Applikationen oder einen neuen Benutzer erstellen oder gar mit Azure AD interagieren können, benötigen Sie eine B2C von Azure AD-Mandanten und ein globaler Administrator-Konto in den Mandanten. Wenn Sie einen Mandanten bereits, [Erste Schritte mit Azure AD B2C](active-directory-b2c-get-started.md)besitzen.

## <a name="register-a-service-application-in-your-tenant"></a>Registrieren einer Service-Anwendungs in Ihrem Mandanten

Nachdem Sie einen Mandanten B2C haben, müssen Sie Ihre Service-Anwendung mithilfe von Azure AD-PowerShell-Cmdlets erstellen.
Zuerst herunterladen Sie und installieren Sie der- [Microsoft Online Services-Anmeldeassistent](http://go.microsoft.com/fwlink/?LinkID=286152). Klicken Sie dann herunterladen Sie und installieren Sie der [64-Bit-Azure-Active Directory-Modul für Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).

> [AZURE.IMPORTANT]
Um die Graph-API mit Ihrem Mandanten B2C verwenden zu können, müssen Sie eine spezielle Anwendung mithilfe der PowerShell registrieren. Führen Sie die Anweisung in diesem Artikel erledigen. Sie können keine die bereits vorhandenen B2C Applikationen wiederverwenden, die Sie in der Azure-Portal registriert.

Nach der Installation des PowerShell-Moduls PowerShell zu öffnen, und Verbinden mit Ihrem Mandanten B2C. Nach dem Ausführen `Get-Credential`, Sie werden aufgefordert, einen Benutzernamen und Ihr Kennwort ein, geben Sie den Benutzernamen und das Kennwort für Ihr Konto B2C Mandanten Administrator.

```
> $msolcred = Get-Credential
> Connect-MsolService -credential $msolcred
```

Bevor Sie eine Anwendung erstellen, müssen Sie einen neuen **Client geheim**generieren.  Die Anwendung wird im Client geheim Azure AD authentifizieren und zum Erfassen von Access Token verwenden. Sie können einen gültigen Schlüssel in PowerShell generieren:

```
> $bytes = New-Object Byte[] 32
> $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
> $rand.GetBytes($bytes)
> $rand.Dispose()
> $newClientSecret = [System.Convert]::ToBase64String($bytes)
> $newClientSecret
```

Der letzte Befehl sollte Ihre neue Client geheim drucken. Kopieren sie sicherer Stelle. Sie benötigen es später. Jetzt können Sie Ihrer Anwendung erstellen, indem Sie den neuen Client-Schlüssel als Referenz für die app bereitstellen:

```
> New-MsolServicePrincipal -DisplayName "My New B2C Graph API App" -Type password -Value $newClientSecret

DisplayName           : My New B2C Graph API App
ServicePrincipalNames : {dd02c40f-1325-46c2-a118-4659db8a55d5}
ObjectId              : e2bde258-6691-410b-879c-b1f88d9ef664
AppPrincipalId        : dd02c40f-1325-46c2-a118-4659db8a55d5
TrustedForDelegation  : False
AccountEnabled        : True
Addresses             : {}
KeyType               : Password
KeyId                 : a261e39d-953e-4d6a-8d70-1f915e054ef9
StartDate             : 9/2/2015 1:33:09 AM
EndDate               : 9/2/2016 1:33:09 AM
Usage                 : Verify
```

Wenn Sie die Anwendung erfolgreich erstellt haben, sollten sie Eigenschaften der Anwendung wie die oben drucken. Sie benötigen beide `ObjectId` und `AppPrincipalId`, damit diese Werte, zu kopieren.

Nachdem Sie eine Anwendung in Ihrem Mandanten B2C erstellt haben, müssen Sie es die Berechtigungen zuweisen, die es Benutzervorgänge ausführen muss. Weisen Sie die Anwendung drei Rollen: Directory Leser (Benutzer lesen), Directory Autoren (zum Erstellen und Aktualisieren von Benutzern), und ein Benutzer Konto-Administrator (So löschen Sie Benutzer). Ausführenden haben bekannte Bezeichner ersetzen können Sie die `-RoleMemberObjectId` Parameter mit `ObjectId` von oben, und führen Sie die folgenden Befehle. In der Liste aller Rollen Verzeichnis, wiederholen Sie die Ausführung finden Sie unter `Get-MsolRole`.

```
> Add-MsolRoleMember -RoleObjectId 88d8e3e3-8f55-4a1e-953a-9b9898b8876b -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId 9360feb5-f418-4baa-8175-e2a00bac4301 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
```

Jetzt haben eine Anwendung, die verfügt über die Berechtigung zum Erstellen, lesen, aktualisieren und Löschen von Benutzern aus Ihrem Mandanten B2C.

## <a name="download-configure-and-build-the-sample-code"></a>Herunterladen und konfigurieren den Stichprobe Code erstellen

Zunächst herunterladen Sie den Code Stichprobe und wieder ausführen. Klicken Sie dann dauert wir näher betrachten.  Sie können [den Beispielcode als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). Sie können auch in ein Verzeichnis Ihrer Wahl Klonen:

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Öffnen der `B2CGraphClient\B2CGraphClient.sln` Visual Studio-Lösung in Visual Studio. In der `B2CGraphClient` Projekt, öffnen Sie die Datei `App.config`. Ersetzen Sie die Einstellungen für die drei app durch eigene Werte ein:

```
<appSettings>
    <add key="b2c:Tenant" value="contosob2c.onmicrosoft.com" />
    <add key="b2c:ClientId" value="{The AppPrincipalId from above}" />
    <add key="b2c:ClientSecret" value="{The client secret you generated above}" />
</appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Als Nächstes mit der rechten Maustaste auf die `B2CGraphClient` Lösung und Wiederherstellen der Stichprobe. Wenn Sie erfolgreich sind, sollten Sie jetzt haben eine `B2C.exe` ausführbare Datei befindet sich im `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>Erstellen Sie mithilfe der Graph-API Benutzervorgänge

Öffnen, um die B2CGraphClient verwenden eine `cmd` Windows Befehl auffordern, und ändern im Verzeichnis nach der `Debug` Directory. Führen Sie dann die `B2C Help` Befehl.

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

Dadurch wird eine kurze Beschreibung der einzelnen Befehle angezeigt. Wenn Sie einen der folgenden Befehle, aufrufen, `B2CGraphClient` führt eine Anforderung an die Azure AD Graph-API.

### <a name="get-an-access-token"></a>Abrufen einer Access-token

Eine Anforderung an das Graph-API erfordert eine Access-Token für die Authentifizierung ein. `B2CGraphClient`Mithilfe der Open Source Active Directory Authentifizierung Bibliothek (ADAL) zu erfassen von Access Token. ADAL erleichtert die token Übernahme durch eine einfache API bereitstellen und kümmern uns einige wichtige Details, wie z. B. Access Token Zwischenspeichern. Sie müssen nicht mit ADAL Token, durch abrufen. Sie können auch Token abrufen, indem HTTP-Anfragen erstellen.

> [AZURE.NOTE]
    In diesem Codebeispiel wird für die Kommunikation mit dem Diagramm-API ADAL Version 2 verwendet.  Sie müssen ADAL Version 2 oder v3 verwenden, um Access Token abzurufen, die API Azure AD-Diagramm verwendet werden können.

Wenn `B2CGraphClient` ausgeführt wird, es erstellt eine Instanz der `B2CGraphClient` Klasse. Der Konstruktor für diese Klasse richtet eine ADAL Authentifizierung Gerüstbau aus:

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

Wir verwenden die `B2C Get-User` -Befehl als Beispiel. Wenn `B2C Get-User` wird aufgerufen, ohne zusätzlichen Eingaben, CLI Anrufe die `B2CGraphClient.GetAllUsers(...)` Methode. Diese Methode ruft `B2CGraphClient.SendGraphGetRequest(...)`, sendet eine HTTP GET-Anforderung an das Graph-API. Bevor Sie `B2CGraphClient.SendGraphGetRequest(...)` sendet GET anfordern, es zuerst erhält eine Access token mithilfe von ADAL:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

Sie können Zugriff ein token für die Graph-API, indem Sie die ADAL `AuthenticationContext.AcquireToken(...)` Methode. Klicken Sie dann ADAL gibt eine `access_token` , der Anwendung Identität darstellt.

### <a name="read-users"></a>Lesen von Benutzern

Wenn Sie eine Liste der Benutzer erhalten oder ein bestimmtes Benutzers aus der Graph-API erhalten möchten, können Sie eine HTTP senden `GET` zum Anfordern der `/users` Endpunkt. Eine Anforderung für alle Benutzer in einen Mandanten sieht wie folgt aus:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Wenn Sie diese Anforderung anzeigen möchten, führen Sie Folgendes aus:

 ```
 > B2C Get-User
 ```

Es gibt zwei wichtige Dinge zu beachten:

- Das Zugriffstoken über ADAL erworben hinzugefügt wird die `Authorization` Kopfzeile mithilfe der `Bearer` Farbschema.
- Für B2C Mandanten, müssen Sie den Abfrageparameter verwenden `api-version=1.6`.

Beide der folgenden Details in gehandhabt werden die `B2CGraphClient.SendGraphGetRequest(...)` Methode:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Erstellen Sie Benutzerkonten consumer

Wenn Sie in Ihrem Mandanten B2C Benutzerkonten erstellen, können Sie eine HTTP senden `POST` zum Anfordern der `/users` Endpunkt:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",              // a value that can be used for displaying to the end user
    "mailNickname": "joec",                     // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

Die meisten dieser Eigenschaften in dieser Anforderung müssen Consumer Benutzer erstellen. Wenn Sie mehr erfahren möchten, klicken Sie auf [hier](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Beachten Sie, dass die `//` Kommentare Abbildung aufgenommen wurden. Nehmen Sie diese nicht in einer tatsächlichen Anforderung.

Um die Anfrage angezeigt wird, führen Sie einen der folgenden Befehle:

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

Die `Create-User` Befehl wird eine .json-Datei als Eingabeparameter. Diese Datei enthält eine JSON-Darstellung eines Benutzerobjekts. Es gibt zwei Beispieldateien .json im Code: `usertemplate-email.json` und `usertemplate-username.json`. Sie können diese Dateien Ihren Anforderungen entsprechend ändern. Zusätzlich zu den erforderlichen Feldern beinhaltet diese Dateien unterschiedliche optionale Felder, die Sie verwenden können. Optionaler Felder Details finden Sie im [Azure AD Graph-API Entität verweisen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).

Sie können sehen, wie die POST-Anforderung erstellt wird, in `B2CGraphClient.SendGraphPostRequest(...)`.

- Es wird ein Access-Token zu angefügt der `Authorization` Kopfzeile der Anfrage.
- Er legt `api-version=1.6`.
- Sie enthält das JSON-Benutzer-Objekt in den Textkörper der Anfrage ein.

> [AZURE.NOTE]
Weist die Konten, die Sie einen vorhandenen Benutzerspeicher migrieren möchten unteren Kennwortstärke als die [sicheres Kennwortstärke Azure AD B2C seitens](https://msdn.microsoft.com/library/azure/jj943764.aspx), können Sie die Anforderung sicheres Kennwort verwenden deaktivieren der `DisableStrongPassword` Wert in der `passwordPolicies` Eigenschaft. Sie können beispielsweise die oben bereitgestellte erstellen Benutzer Anforderung ändern: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.

### <a name="update-consumer-user-accounts"></a>Aktualisieren der Consumer von Benutzerkonten

Wenn Sie Benutzerobjekte aktualisieren, ähnelt der Vorgang für das Element, das Sie verwenden, um Benutzerobjekte zu erstellen. Aber dieses Verfahren verwendet die HTTP `PATCH` Methode:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",              // this request updates only the user's displayName
}
```

Versuchen Sie, einen Benutzer zu aktualisieren, indem Sie Ihre JSON-Dateien mit neuen Daten aktualisieren. Anschließend können Sie `B2CGraphClient` , führen Sie einen der folgenden Befehle:

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Prüfen der `B2CGraphClient.SendGraphPatchRequest(...)` Methode Details zum Senden Sie diese Anforderung.

### <a name="search-users"></a>Suchbenutzer

Sie können für Benutzer in Ihrem Mandanten B2C auf verschiedene Weise suchen. Eine, mithilfe des Benutzers Objekt-ID oder beiden, mithilfe des Benutzers Anmeldung Bezeichner (d. h., die `signInNames` Eigenschaft).

Führen Sie einen der folgenden Befehle zum Suchen nach einer bestimmten Benutzer:

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

Hier finden Sie einige Beispiele für:

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Löschen von Benutzern

Der Vorgang zum Löschen eines Benutzers ist einfach. Verwenden Sie den HTTP `DELETE` Methode und erstellen die URL mit den richtigen Objekt ID:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Um ein Beispiel zu sehen, geben Sie diesen Befehl und Anzeigen der Löschanfrage, die an der Konsole ausgegeben wird:

```
> B2C Delete-User <object-id-of-user>
```

Prüfen der `B2CGraphClient.SendGraphDeleteRequest(...)` Methode Details zum Senden Sie diese Anforderung.

Sie können viele andere Aktionen mit der Azure AD Graph-API zusätzlich zur Verwaltung des Benutzers ausführen. [Azure AD Graph-API-Referenz](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) enthält Details über die einzelnen, zusammen mit der Stichprobe Anfragen.

## <a name="use-custom-attributes"></a>Verwenden von benutzerdefinierten Attributen

Die meisten Consumer Applikationen müssen einige der benutzerdefinierten Benutzerprofildaten speichern. Eine Möglichkeit besteht darin, Sie dies tun können ist benutzerdefinierte Attribute in Ihrem Mandanten B2C definiert. Sie können dann behandeln dieses Attribut die gleiche Weise wie jede andere Eigenschaft für ein Objekt des Benutzers zu behandeln. Sie können aktualisieren Sie das Attribut, löschen Sie das Attribut, Abfragen, indem Sie das Attribut klicken, senden das Attribut als Anspruch in Anmeldung Token und vieles mehr.

Ein benutzerdefiniertes Attribut in Ihrem Mandanten B2C definieren, finden Sie unter [B2C benutzerdefiniertes Attribut verweisen](active-directory-b2c-reference-custom-attr.md).

Sie können die benutzerdefinierten Attributen in Ihrem Mandanten B2C mit definiert anzeigen `B2CGraphClient`:

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

Die Ausgabe dieser Funktionen werden die Details der einzelnen benutzerdefinierte Attribute, z. B.:

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

Sie verwenden den vollständigen Namen, z. B. `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, als eine Eigenschaft für Ihre Benutzerobjekte.  Die .json-Datei mit der neuen Eigenschaft und einen Wert für die Eigenschaft zu aktualisieren, und klicken Sie dann auf ausführen:

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

Mithilfe von `B2CGraphClient`, Sie verfügen über eine Service-Anwendung, die Ihre B2C Mandanten Benutzer programmgesteuert verwaltet werden kann. `B2CGraphClient`verwendet eine eigenen Anwendungsidentität für die Azure AD Graph-API Authentifizierung. Es erhält auch Token mithilfe eines Client geheim. Denken Sie dieses Feature in Ihrer Anwendung zu integrieren, einige wichtigsten Punkte für B2C-apps:

- Sie müssen der Anwendung die entsprechenden Berechtigungen in den Mandanten erteilen.
- Jetzt müssen Sie ADAL Version 2 führen, um Access Token zu gelangen. (Sie können auch Protokollnachrichten direkt senden, ohne mit einer Bibliothek.)
- Die Graph-API aufzurufen, verwenden Sie `api-version=1.6`.
- Wenn Sie erstellen und Aktualisieren von Consumer Benutzer, sind einige Eigenschaften erforderlich, wie zuvor beschrieben.

Wenn Sie alle Fragen oder Anforderung von Aktionen, dass Sie mit der Graph-API auf Ihrem Mandanten B2C ausführen möchten, lassen Sie einen Kommentar in diesem Artikel oder legen Sie ein Problems im GitHub Code Stichprobe Repository ab.
