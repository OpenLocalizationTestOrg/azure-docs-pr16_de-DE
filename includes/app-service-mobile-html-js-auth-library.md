###<a name="a-nameserver-authahow-to-authenticate-with-a-provider-server-flow"></a><a name="server-auth"></a>So: Authentifizieren bei einem Anbieter (Server Fluss)

Zum Verwalten des Authentifizierungsprozess in Ihrer app Mobile-Apps zu haben, müssen Sie Ihre app mit Ihrer Identitätsanbieter registrieren. In Ihrem Dienst Azure App müssen Sie dann die Anwendung-ID und die von Ihrem Anbieter bereitgestellten geheim konfigurieren.
Weitere Informationen finden Sie im Lernprogramm [hinzufügen Authentifizierung zu Ihrer Anwendung].

Nachdem Sie Ihre Identitätsanbieter registriert haben, rufen Sie einfach die .login() Methode mit dem Namen von Ihrem Anbieter. Um beispielsweise den folgenden Code Anmeldung mit Facebook verwenden.

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

Wenn Sie einen Identitätsanbieter als Facebook verwenden, ändern Sie den Wert, der an die Methode Login über auf einen der folgenden übergeben: `microsoftaccount`, `facebook`, `twitter`, `google`, oder `aad`.

In diesem Fall verwaltet Azure-App-Verwaltungsdienst OAuth 2.0 Authentifizierung illustrieren durch Anzeige der Anmeldeseite des ausgewählten Anbieters und ein App-Dienst Authentifizierungstoken nach dem erfolgreiche Anmeldung mit dem Identitätsanbieter generieren. Die Funktion Login gibt nach Abschluss des Vorgangs ein JSON-Objekt (Benutzer), die die Benutzer-ID und die App-Dienst macht Authentifizierungstoken in den Feldern Benutzer-ID und AuthenticationToken Hilfethemas. Dieses Token kann zwischengespeichert werden und wieder verwendet werden, bis sie abgelaufen ist.

###<a name="a-nameclient-authahow-to-authenticate-with-a-provider-client-flow"></a><a name="client-auth"></a>So: Authentifizieren bei einem Anbieter (Client Fluss)

Ihre app können Sie auch unabhängig voneinander wenden Sie sich an der Identitätsanbieter und geben Sie dann das zurückgegebene Token an den App-Dienst für die Authentifizierung. Diese Client-Fluss können Sie eine einzelne Anmeldeverhalten für Benutzer bereitstellen oder zum Abrufen von weiteren Benutzerdaten aus der Identitätsanbieter.

#### <a name="social-authentication-basic-example"></a>Für soziale Netzwerke Beispiel einer einfachen Authentifizierung

In diesem Beispiel wird die Facebook-Client SDK für die Authentifizierung:

```
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```
In diesem Beispiel wird davon ausgegangen, dass das vom jeweiligen Anbieter SDK bereitgestellte Token in der token Variablen gespeichert ist.

#### <a name="microsoft-account-example"></a>Microsoft Account Beispiel

Im folgenden Beispiel wird das Live SDK, die mithilfe von Microsoft Account-einmaliges Anmelden für Windows Store-apps zu unterstützt:

```
WL.login({ scope: "wl.basic"}).then(function (result) {
      client.login(
            "microsoftaccount",
            {"authenticationToken": result.session.authentication_token})
      .done(function(results){
            alert("You are now logged in as: " + results.userId);
      },
      function(error){
            alert("Error: " + err);
      });
});
```

In diesem Beispiel wird ein Token aus Live verbinden, das mit Ihrem App-Dienst bereitgestellt wird, indem Sie die Funktion Login.

###<a name="a-nameauth-getinfoahow-to-obtain-information-about-the-authenticated-user"></a><a name="auth-getinfo"></a>So: Abrufen von Informationen zu den authentifizierten Benutzer

Die Authentifizierungsinformationen für den aktuellen Benutzer abgerufen werden kann, aus der `/.auth/me` mit einer beliebigen Methode AJAX-Endpunkt.  Stellen Sie sicher, legen Sie die `X-ZUMO-AUTH` Kopfzeile auf der Authentifizierungstoken.  Das Authentifizierungstoken befindet sich an `client.currentUser.mobileServiceAuthenticationToken`.  Wenn Sie beispielsweise den Abruf API verwenden:

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

Abruf steht als ein Paket Npm oder Browser Downloads von CDNJS. JQuery oder einer anderen AJAX-API können auch die Informationen abgerufen werden sollen.  Daten werden als JSON-Objekt empfangen.
