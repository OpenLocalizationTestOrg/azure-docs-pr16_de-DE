<properties
    pageTitle="Erste Schritte mit Azure AD melden Sie an und melden Sie node.js verwenden ab"
    description="So erstellen Sie eine node.js Express MVC Web App, die mit Azure AD für die Anmeldung integriert."
    services="active-directory"
    documentationCenter="nodejs"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="brandwe"/>

# <a name="nodejs-web-app-sign-in--sign-out-with-azure-ad"></a>NodeJS Web App anmelden, und melden Sie sich mit Azure AD Out


Verwenden Sie hier Passport zu:

- Melden Sie den Benutzer in der app Azure AD-aus.
- Einige Informationen zu den Benutzer anzeigen.
- Melden Sie den Benutzer bei der app abmelden.

**Passport** ist Authentifizierung Middleware für Node.js. Extrem flexible und modulare, Passport abgelegt werden kann als zu beliebiger Express-basierten oder Webanwendung Resitify. Eine umfassende Reihe von Strategien unterstützt Authentifizierung über einen Benutzernamen und Kennwort, Facebook, Twitter und mehr. Wir haben für Microsoft Azure Active Directory eine Strategie entwickelt. Wir dieses Modul installieren und Hinzufügen klicken Sie dann auf Microsoft Azure Active Directory `passport-azure-ad` -Plug-in.

Um dies zu tun, müssen Sie:

1. Registrieren Sie sich eine app.
2. Verwenden die Passport-Azure-Active Directory-Strategie richten Sie Ihre app ein.
3. Verwenden Sie Passport anmelden und Abmelde Anfragen an Azure AD ausgeben.
4. Drucken Sie Daten über den Benutzer aus.

Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS)verwaltet.  Wenn Sie nachvollziehen möchten, können Sie Sie [Herunterladen des app Gerüst als eine ZIP](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) oder das Gerüst Klonen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

Die fertige Anwendung wird am Ende dieses Lernprogramms ebenfalls bereitgestellt.

## <a name="1-register-an-app"></a>1. Registrieren einer App
- Melden Sie sich bei der Azure-Verwaltungsportal.
- Klicken Sie im linken Navigationsbereich auf **Active Directory**.
- Wählen Sie den Mandanten, in dem die Anwendung registriert werden soll.
- Klicken Sie auf die Registerkarte **Applications** aus, und klicken Sie auf der unteren Einzug hinzufügen.
- Erstellen Sie einer neuen **Webanwendung und/oder WebAPI**, und folgen Sie den Anweisungen.
    - Den **Namen** der Anwendung wird eine Anwendung an Endbenutzer beschrieben.
    -   Die **Anmeldung URL** ist die Basis der app-URL.  Das Gerüst der Standardwert liegt ' Http://localhost:3000/autorisierende/Openid/Eingabe ''.
    - Der **App-ID-URI** ist ein eindeutiger Bezeichner für eine Anwendung.  Ist die Verwendung der Messe `https://<tenant-domain>/<app-name>`, z. B.`https://contoso.onmicrosoft.com/my-first-aad-app`
- Nachdem Sie die Registrierung abgeschlossen haben, zuweisen AAD Ihre app eine eindeutige Client-ID.  Sie benötigen diesen Wert in den nächsten Abschnitten, also kopieren Sie sie von der Registerkarte konfigurieren.

## <a name="2-add-pre-requisities-to-your-directory"></a>2. Pre-Requisities zu Ihrem Verzeichnis hinzufügen

Über die Befehlszeile wechseln Sie zu Ihrem Stammordner sofern nicht bereits vorhanden, und führen Sie die folgenden Befehle:

- `npm install express`
- `npm install ejs`
- `npm install ejs-locals`
- `npm install restify`
- `npm install mongoose`
- `npm install bunyan`
- `npm install assert-plus`
- `npm install passport`

- Müssen darüber hinaus unsere `passport-azure-ad` auch:

- `npm install passport-azure-ad`

Hiermit wird die Bibliotheken installiert die Passport Azure Ad abhängig.

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3. richten Sie Ihre app der Passport-Knoten-Js Strategie
Konfigurieren Sie hier die Middleware Express, um die OpenID verbinden Authentication-Protokoll verwenden.  Passport wird zum Anmelden und Abmelde Anfragen Emission, die Sitzung des Benutzers zu verwalten und erhalten Informationen über den Benutzer, unter anderem verwendet werden.

-   Um zu beginnen, öffnen Sie die `config.js` Datei im Stammverzeichnis des Projekts, und geben Sie Ihre app Konfigurationswerte in der `exports.creds` Abschnitt.
    -   Die `clientID:` ist die **Id der Anwendung** , die Ihre app in der Registrierung Portal zugewiesen ist.
    -   Die `returnURL` ist der **Uri umleiten** , die Sie im Portal eingegeben haben.
    - Die `clientSecret` ist das Geheimnis Sie im Portal generiert

- Öffnen Sie als Nächstes `app.js` Datei im Stammverzeichnis der das Projekt, und fügen Sie den folgenden sein Anruf zum Aufrufen der `OIDCStrategy` Strategie, die im Lieferumfang`passport-azure-ad`


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// add a logger

var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

- Verwenden Sie danach die Strategie wir nur verwiesen wird, um unser Login Anfragen zu behandeln.

```JavaScript
// Use the OIDCStrategy within Passport. (Section 2) 
// 
//   Strategies in passport require a `validate` function, which accept
//   credentials (in this case, an OpenID identifier), and invoke a callback
//   with a user object.
passport.use(new OIDCStrategy({
    callbackURL: config.creds.returnURL,
    realm: config.creds.realm,
    clientID: config.creds.clientID,
    clientSecret: config.creds.clientSecret,
    oidcIssuer: config.creds.issuer,
    identityMetadata: config.creds.identityMetadata,
    skipUserProfile: config.creds.skipUserProfile,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    if (!profile.email) {
      return done(new Error("No email found"), null);
    }
    // asynchronous verification, for effect...
    process.nextTick(function () {
      findByEmail(profile.email, function(err, user) {
        if (err) {
          return done(err);
        }
        if (!user) {
          // "Auto-registration"
          users.push(profile);
          return done(null, profile);
        }
        return done(null, user);
      });
    });
  }
));
```
Passport verwendet ein ähnliches Muster für alle, Strategien (Twitter, Facebook usw.), die alle Strategie Autoren entsprechen. Die Strategie betrachtet wird, dass wir es eine möchten übergeben, die ein Token und als die Parameter fertig sind. Die Strategie verlässliche immer wieder an uns nachdem alle bedeutet es handelt sich um Arbeit. Nachdem bedeutet möchten wir Speichern des Benutzers, und speichern das Token, damit wir nicht erneut dafür Fragen benötigen.


> [AZURE.IMPORTANT] 
Der obige Code hat jeder Benutzer, die zum Authentifizieren an unseren Server geschieht. Dies wird als automatische Registrierung bezeichnet. Herstellung Servern möchten Sie würde nicht zulassen, dass jeder ohne zuvor diese mithilfe eines Registrierungsprozesses wechseln, die Sie sich entscheiden. Dies ist normalerweise der Muster, die, das Sie in Consumer apps, wer ermöglichen es Ihnen sehen, mit Facebook registrieren, aber dann aufgefordert werden, um zusätzliche Informationen auszufüllen. Wenn dies eine Stichprobe Anwendung nicht war, konnte wir nur die e-Mail aus dem token Objekt extrahiert haben, die zurückgegeben wird, und dann aufgefordert werden, um zusätzliche Informationen auszufüllen. Da dies ein Test-Server wir einfach die Datenbank im Speicher hinzufügen ist.

- Als Nächstes wir fügen Sie die Methoden, mit der uns die protokollierten in Benutzer je nach Bedarf von Passport verfolgen können. Dies umfasst serialisieren und Deserialisieren Informationen des Benutzers:

```JavaScript

// Passport session setup. (Section 2)

//   To support persistent login sessions, Passport needs to be able to
//   serialize users into and deserialize users out of the session.  Typically,
//   this will be as simple as storing the user ID when serializing, and finding
//   the user by ID when deserializing.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// array to hold logged in users
var users = [];

var findByEmail = function(email, fn) {
  for (var i = 0, len = users.length; i < len; i++) {
    var user = users[i];
   log.info('we are using user: ', user);
    if (user.email === email) {
      return fn(null, user);
    }
  }
  return fn(null, null);
};
```

- Als Nächstes fügen Sie den Code, um die express-Engine laden. Hier sehen Sie wir verwenden den standardmäßigen /views und /routes Muster, die Express bietet.

```JavaScript

// configure Express (Section 2)

var app = express();


app.configure(function() {
  app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.logger());
  app.use(express.methodOverride());
  app.use(cookieParser());
  app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
  app.use(bodyParser.urlencoded({ extended : true }));
  // Initialize Passport!  Also use passport.session() middleware, to support
  // persistent login sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

- Schließlich fügen Sie die Arbeitspläne, die die eigentliche Login Anfragen zu übergeben werden die `passport-azure-ad` Engine:

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  The first step in OpenID authentication will involve redirecting
//   the user to their OpenID provider.  After authenticating, the OpenID
//   provider will redirect the user back to this application at
//   /auth/openid/return
app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authentication was called in the Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('We received a return from AzureAD.');
    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('We received a return from AzureAD.');
    res.redirect('/');
  });
  ```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4 verwenden Sie 4 Passport anmelden und Abmelde Anfragen an Azure AD ausgeben

Ihre app ist für die Kommunikation mit dem Version 2.0-Endpunkt über das Verbinden OpenID Authentifizierungsprotokoll jetzt ordnungsgemäß konfiguriert.  `passport-azure-ad`enthält alle hässlich Details der Authentifizierungsnachrichten zu erstellen, das Token aus Azure AD überprüfen und Verwalten von Benutzer-Sitzung durchgeführt.  Alle bleibt dann an die Benutzer stellen eine Möglichkeit zum Melden Sie sich abmelden und zusätzliche Informationen auf einem Benutzer sammeln.

- Zunächst können Sie die standardmäßigen, Login, Konto und Abmeldung Methoden zum Hinzufügen unsere `app.js` Datei:

```JavaScript

//Routes (Section 4)

app.get('/', function(req, res){
  res.render('index', { user: req.user });
});

app.get('/account', ensureAuthenticated, function(req, res){
  res.render('account', { user: req.user });
});

app.get('/login',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Login was called in the Sample');
    res.redirect('/');
});

app.get('/logout', function(req, res){
  req.logout();
  res.redirect('/');
});

```

-   Betrachten Sie diese im Detail aus:
    -   Die `/` Routing wird leiten Sie an der index.ejs Ansicht übergeben, also Benutzername in der Besprechungsanfrage (falls vorhanden)
    - Die `/account` Routing wird die erste ***sicherstellen, dass wir authentifiziert werden*** (wir implementieren, die unterhalb) und dann den Benutzer in der Anforderung übergeben, damit wir zusätzliche Informationen zu der Benutzer gelangen.
    - Die `/login` Routing ruft unsere Azuread-Openidconnect Authentifizierung aus `passport-azuread` und die wird nicht erfolgreich leiten Sie den Benutzer wieder zur /login
    - Die `/logout` werden einfach die logout.ejs aufrufen (und Weiterleiten) die löscht Cookies, und klicken Sie dann den Benutzer zum index.ejs zurückzukehren


- Für den letzten Teil des `app.js`, lassen Sie uns die EnsureAuthenticated Methode hinzu, die in verwendet wird `/account` über.

```JavaScript

// Simple route middleware to ensure user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs to be protected.  If
//   the request is authenticated (typically via a persistent login session),
//   the request will proceed.  Otherwise, the user will be redirected to the
//   login page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}
```

- Lassen Sie uns tatsächlich erstellen Sie schließlich den Server selbst in `app.js`:

```JavaScript

app.listen(3000);

```


## <a name="5-create-the-views-and-routes-in-express-to-display-our-user-in-the-website"></a>5 erstellen Sie 5 die Ansichten und leitet im Express unsere Benutzer in der Website angezeigt werden

Wir haben unsere `app.js` abgeschlossen. Jetzt müssen wir einfach hinzufügen die Arbeitspläne und Ansichten, die die Informationen angezeigt werden wir Benutzer als auch Ziehpunkt zum Aufrufen der `/logout` und `/login` weitergeleitet, die wir erstellt haben.

- Erstellen der `/routes/index.js` Routing unter dem Stammverzeichnis.

```JavaScript
/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

- Erstellen der `/routes/user.js` Routing unter dem Stammverzeichnis

```JavaScript
/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

Diese einfache leitet übergibt nur entlang der Anfrage zu unseren Ansichten, einschließlich des Benutzers aus, falls vorhanden.

- Erstellen der `/views/index.ejs` Ansicht unterhalb des Stammverzeichnisses. Dies ist eine einfache Seite, die anrufen unserer Methoden anmelden und Abmelden und können wir die Kontoinformationen zu extrahieren. Beachten Sie, dass wir, die bedingte verwenden können `if (!user)` als Benutzer in der Besprechungsanfrage weitergegeben wird Beweisen wir in Benutzer angemeldet haben.

```JavaScript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
    <h2>Hello, <%= user.displayName %>.</h2>
    <a href="/account">Account Info</a></br>
    <a href="/logout">Log Out</a>
<% } %>
```

- Erstellen der `/views/account.ejs` unter dem Stammverzeichnis anzeigen, damit wir weitere Informationen angezeigt werden, die `passport-azuread` hat in der Besprechungsanfrage Benutzer setzen.

```Javascript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
<p>displayName: <%= user.displayName %></p>
<p>givenName: <%= user.name.givenName %></p>
<p>familyName: <%= user.name.familyName %></p>
<p>UPN: <%= user._json.upn %></p>
<p>Profile ID: <%= user.id %></p>
<p>Full Claimes</p>
<%- JSON.stringify(user) %>
<p></p>
<a href="/logout">Log Out</a>
<% } %>
```

- Wir stellen diese aussehen ziemlich durch Hinzufügen eines Layouts. Erstellen der ' / views/layout.ejs Ansicht unterhalb des Stammverzeichnisses

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>Passport-OpenID Example</title>
    </head>
    <body>
        <% if (!user) { %>
            <p>
            <a href="/">Home</a> | 
            <a href="/login">Log In</a>
            </p>
        <% } else { %>
            <p>
            <a href="/">Home</a> | 
            <a href="/account">Account</a> | 
            <a href="/logout">Log Out</a>
            </p>
        <% } %>
        <%- body %>
    </body>
</html>
```

Schließlich erstellen Sie, und führen Sie die app! 

Führen Sie `node app.js` und navigieren Sie zu`http://localhost:3000`


Melden Sie sich mit entweder eine persönliche Microsoft Account oder einem geschäftlichen oder schulnotizbücher-Konto, und beachten Sie, wie die Identität des Benutzers in der Liste AccountType wiedergegeben wird.  Sie verfügen jetzt über eine gesicherte Protokolle nach Industriestandard, die Benutzer mit ihren persönlichen und Arbeit/Schule Konten authentifizieren können Web-app.

Als Referenz können die vollständigen Beispiel (ohne die Konfigurationswerte) [als eine ZIP hier bereitgestellt wird](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip), oder Sie es GitHub zu duplizieren:

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```


Sie können nun auf Erweiterte Themen verschieben.  Möglicherweise möchten versuchen:

[Sichern eine Web-API mit Azure AD >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
