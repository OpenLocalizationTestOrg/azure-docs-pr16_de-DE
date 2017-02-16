<properties
    pageTitle="Azure AD-B2C: Secure eine Web-API mithilfe von Node.js | Microsoft Azure"
    description="So erstellen Sie eine Node.js Web API, die von einem Mandanten B2C Token akzeptiert"
    services="active-directory-b2c"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="08/30/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure AD-B2C: Secure eine Web-API mithilfe von Node.js

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Mit B2C Azure Active Directory (Azure AD) können Sie eine Web-API mithilfe von OAuth 2.0 Access Token sichern. Diese Token ermöglichen Ihrer Client-apps, die an die API authentifizieren Azure AD B2C verwenden. In diesem Artikel wird gezeigt, wie eine API "Aufgabenliste" zu erstellen, können Benutzer hinzufügen und Liste Aufgaben, werden kann. Das Web-API ist mit Azure AD B2C gesichert und lässt nur authentifizierten Benutzer zum Verwalten ihrer Vorgangsliste werden.

> [AZURE.NOTE]  In diesem Beispiel wurde geschrieben mit mithilfe unserer [iOS B2C Beispiel Anwendung](active-directory-b2c-devquickstarts-ios.md)verbunden sein. Führen Sie zuerst den aktuellen – Exemplarische Vorgehensweise aus, und klicken Sie dann zusammen mit diesem Beispiel folgen.

**Passport** ist Authentifizierung Middleware für Node.js. Flexible und am, Passport kann als installiert werden, in einem Express-basierten oder Webanwendung Restify. Eine umfassende Reihe von Strategien unterstützt Authentifizierung mithilfe eines Benutzernamens und Kennworts, Facebook, Twitter und mehr. Wir haben eine Strategie für Azure Active Directory (Azure AD) entwickelt. Sie dieses Modul installieren und hinzufügen das Azure AD `passport-azure-ad` -Plug-in.

Um dieses Beispiel auszuführen, müssen Sie:

1. Registrieren Sie sich eine Anwendung mit Azure AD.
2. Richten Sie die Anwendung für die Verwendung eines `azure-ad-passport` -Plug-in.
3. Konfigurieren einer Clientanwendung "Aufgabenliste" Web API aufrufen.


## <a name="get-an-azure-ad-b2c-directory"></a>Abrufen einer Azure AD B2C-Verzeichnis

Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis erstellen, oder den Mandanten.  Ein Verzeichnis ist ein Container für alle Benutzer, apps, Gruppen und vieles mehr.  Wenn Sie eine besitzen bereits, fortsetzen [Erstellen Sie ein Verzeichnis B2C](active-directory-b2c-get-started.md) , bevor Sie.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Als Nächstes müssen Sie eine app in Ihrem Verzeichnis B2C zu erstellen, die Azure AD einige Informationen zu erhalten, die sichere Kommunikation mit Ihrem app benötigt. In diesem Fall sind die Client-app und die Web-API durch eine einzelne **Anwendung ID**dargestellt, da eine logische app bestehen aus. [Wenn Sie eine app erstellen, gehen Sie wie folgt vor.](active-directory-b2c-app-registration.md) Achten Sie darauf, um:

- Einschließen einer **web app/Web api** in der Anwendung
- Geben Sie `http://localhost/TodoListService` als **Antwort-URL**. Es ist die Standard-URL für dieses Codebeispiel.
- Erstellen Sie einer **Anwendung geheim** für eine Anwendung, und kopieren Sie es zu. Diese Daten benötigen später. Beachten Sie, dass dieser Wert muss [XML mit Escapezeichen versehen](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) werden, bevor Sie es verwenden.
- Kopieren Sie die **ID der Anwendung** , die zu Ihrer Anwendung zugeordnet ist. Diese Daten benötigen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen Sie Ihre Richtlinien

In Azure AD B2C wird jede Erfahrungen von einer [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Diese app enthält zwei Identität Erfahrung: registrieren, und melden Sie sich an. Sie müssen zum Erstellen einer Richtlinie für jedes Typs wie in der [Richtlinie Bezug Artikel](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)beschrieben.  Wenn Sie Ihre drei Richtlinien erstellen, müssen Sie unbedingt:

- Wählen Sie den **Anzeigenamen** und andere Attribute Anmeldung in Ihrer Anmeldung Richtlinie aus.
- Wählen Sie die Anwendung Ansprüche **Anzeigenamen** und **Objekt-ID** in jeder Richtlinie aus.  Sie können auch anderen Ansprüche auswählen.
- Kopieren Sie unten den **Namen** der einzelnen Richtlinie, nachdem Sie es erstellt haben. Es sollte das Präfix haben `b2c_1_`.  Diese Richtliniennamen benötigen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie Ihre drei Richtlinien erstellt haben, sind Sie bereit sind, Ihre app zu erstellen.

Weitere Informationen zur Funktionsweise von Richtlinien in Azure AD B2C, beginnen Sie mit dem [Lernprogramm .NET Web app-erste Schritte](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Laden Sie den code

Der Code für dieses Lernprogramm [auf GitHub verwaltet wird](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS). Wenn Sie um das Beispiel beim Durcharbeiten zu erstellen, können Sie [ein Gerüst Projekt als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip). Sie können auch das Gerüst Klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

Die fertige app steht auch [als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) oder das `complete` Zweig des gleichen Repositorys.

## <a name="download-nodejs-for-your-platform"></a>Herunterladen von Node.js für Ihre Plattform

Damit dieses Beispiel erfolgreich verwendet, benötigen Sie eine Installation funktioniert der Node.js. 

Installieren Sie Node.js aus [nodejs.org](http://nodejs.org).

## <a name="install-mongodb-for-your-platform"></a>Installieren von MongoDB für Ihre Plattform

Damit dieses Beispiel erfolgreich verwendet, benötigen Sie eine Installation funktioniert der MongoDB. Wir verwenden MongoDB, um Ihre REST-API über Serverinstanzen dauerhaft zu machen.

Installieren Sie MongoDB aus [mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] Diese exemplarische Vorgehensweise wird vorausgesetzt, dass die Standard-Installation und Server Endpunkte MongoDB, verwenden Sie also zum Zeitpunkt der Erstellung dieses Dokuments ist `mongodb://localhost`.

## <a name="install-the-restify-modules-in-your-web-api"></a>Installieren Sie die Restify-Module in Ihrem Web-API

Wir verwenden Restify, um Ihre REST-API zu erstellen. Restify ist eine minimale und flexible Node.js Anwendungsframework abgeleitet von Express. Es verfügt über eine umfangreiche Sammlung von Features zum Erstellen von REST-APIs auf Verbinden.

### <a name="install-restify"></a>Installieren Restify

Ändern Sie über die Befehlszeile im Verzeichnis nach `azuread`. Wenn die `azuread` Verzeichnis nicht vorhanden ist, erstellen Sie ihn.

`cd azuread`oder`mkdir azuread;`

Geben Sie den folgenden Befehl aus:

`npm install restify`

Dieser Befehl installiert Restify.

#### <a name="did-you-get-an-error"></a>Angezeigt Sie eine Fehlermeldung?

In einigen Betriebssystemen, bei der Verwendung von `npm`, möglicherweise die Fehlermeldung `Error: EPERM, chmod '/usr/local/bin/..'` und eine Anforderung, dass Sie das Konto als Administrator ausführen. Wenn dieses Problem auftritt, verwenden Sie die `sudo` Befehl ausführen `npm` mit einer höheren Berechtigungsstufe.

#### <a name="did-you-get-a-dtrace-error"></a>Haben Sie einen Fehler DTrace erhalten?

Sie wird möglicherweise bei der Installation von Restify ungefähr wie dieser Text angezeigt:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```

Restify bietet leistungsfähige dafür, REST tracing mithilfe von DTrace Anrufe. Viele Betriebssysteme haben jedoch keine DTrace zur Verfügung. Sie können diesen Fehler ignorieren.

Die Ausgabe des Befehls sollte ähnlich wie dieser Text angezeigt werden:

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)

## <a name="install-passport-in-your-web-api"></a>Installieren von Passport in Ihrem Web-API

Ändern Sie über die Befehlszeile im Verzeichnis nach `azuread`, sofern es noch nicht angezeigt wird.

Installieren Sie Passport mit dem folgenden Befehl ein:

`npm install passport`

Die Ausgabe des Befehls sollte ähnlich wie dieser Text aussehen:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a>Hinzufügen von Passport-Azuread der Web-API

Fügen Sie die Strategie OAuth mithilfe von `passport-azuread`, eine Reihe von Strategien, die mit Passport Azure AD zu verbinden. Verwenden Sie diese Strategie für Person Token in der Stichprobe REST-API ein.

> [AZURE.NOTE] Obwohl OAuth2 ein Framework bereitstellt, in dem alle bekannter token Typ ausgestellt werden kann, entwickelt nur bestimmte token Typen universell verwendet haben. Die Token für den Schutz von Endpunkten sind Person Token. Diese Typen von Token sind die am häufigsten in OAuth2 ausgestellt. Viele Implementierungen wird davon ausgegangen, dass die Person Token der einzige Typ von Token ausgestellt werden.

Ändern Sie über die Befehlszeile im Verzeichnis nach `azuread`, sofern es noch nicht angezeigt wird.

Installieren der Pass `passport-azure-ad` Modul mit den folgenden Befehl aus:

`npm install passport-azure-ad`

Die Ausgabe des Befehls sollte ähnlich wie dieser Text aussehen:

``
passport-azure-ad@1.0.0 node_modules/passport-azure-ad
├── xtend@4.0.0
├── xmldom@0.1.19
├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
├── underscore@1.8.3
├── async@1.3.0
├── jsonwebtoken@5.0.2
├── xml-crypto@0.5.27 (xpath.js@1.0.6)
├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
└── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)
``

## <a name="add-mongodb-modules-to-your-web-api"></a>Hinzufügen von MongoDB Module der Web-API

In diesem Beispiel wird als Ihren Datenspeicher MongoDB verwendet. Für die Mongoose, häufig verwendeten installieren-Plug-In für die Verwaltung von Modelle und Schemas.

* `npm install mongoose`

## <a name="install-additional-modules"></a>Installieren Sie zusätzliche Module

Installieren Sie dann die verbleibenden erforderlichen Module.

Ändern Sie über die Befehlszeile im Verzeichnis nach `azuread`, sofern es noch nicht angezeigt wird:

`cd azuread`

Installieren Sie die Module in Ihre `node_modules` Verzeichnis:

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`


## <a name="create-a-serverjs-file-with-your-dependencies"></a>Erstellen Sie eine Datei server.js mit Ihrer Abhängigkeiten

Die `server.js` Datei enthält die Mehrzahl der Funktionalität für Ihren Web-API-Server. 

Ändern Sie über die Befehlszeile im Verzeichnis nach `azuread`, sofern es noch nicht angezeigt wird:

`cd azuread`

Erstellen einer `server.js` Datei in einem Editor. Fügen Sie die folgende Informationen ein:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Speichern Sie die Datei ein. Sie zurückkehren später zu ihm.

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a>Erstellen Sie eine Datei config.js zum Speichern Ihrer Azure AD-Einstellungen

Diese Codedatei übergibt die Konfigurationsparameter aus Ihrer Azure AD-Portal für die `Passport.js` Datei. Diese Konfigurationswerte wird erstellt, wenn Sie das Web-API auf das Portal im ersten Teil der – exemplarische Vorgehensweise hinzugefügt. Erläutert, wie die Werte dieser Parameter setzen, nachdem Sie den Code zu kopieren.

Ändern Sie über die Befehlszeile im Verzeichnis nach `azuread`, sofern es noch nicht angezeigt wird:

`cd azuread`

Erstellen einer `config.js` Datei in einem Editor. Fügen Sie die folgende Informationen ein:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>', 
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>Erforderliche Werte

`clientID`: Die Client-ID der Anwendung Web-API.

`IdentityMetadata`: Dies ist Where `passport-azure-ad` für Ihre Konfigurationsdaten der Identitätsanbieter gesucht. Es sieht so aus auch für die Tasten Token das JSON-Web zu bestätigen. 

`audience`: Den uniform Resource Identifier (URI) aus dem Portal, das eine Anwendung einen bezeichnet. 

`tenantName`: Der Name der Mandanten (beispielsweise **contoso.onmicrosoft.com**).

`policyName`: Die Richtlinie, die die Ihrem Server in Kürze Token überprüft werden sollen. Dieser Richtlinie sollten die gleiche Richtlinie, die Sie für die Anmeldung auf der Clientanwendung verwenden.

> [AZURE.NOTE] Verwenden Sie dieselben Richtlinien für unsere Preview B2C über Client- und einrichten. Wenn Sie bereits eine exemplarische Vorgehensweise abgeschlossen und diese Richtlinien erstellt haben, müssen Sie nicht wiederholt werden. Da Sie die exemplarische Vorgehensweise abgeschlossen haben, müssen Sie dürfen nicht neue Richtlinien für Client Exemplarische Vorgehensweisen auf der Website einrichten.

## <a name="add-configuration-to-your-serverjs-file"></a>Hinzufügen von Konfiguration zu einer server.js-Datei

Lesen Sie die Werte aus der `config.js` Datei, die Sie erstellt haben, fügen Sie die `.config` Datei als Ressource in der Anwendung, und legen Sie dann die globalen Variablen, mit denen in der `config.js` Dokument.

Ändern Sie über die Befehlszeile im Verzeichnis nach `azuread`, sofern es noch nicht angezeigt wird:

`cd azuread`

Öffnen der `server.js` Datei in einem Editor. Fügen Sie die folgende Informationen ein:

```Javascript
var config = require('./config');
```
Hinzufügen eines neuen Abschnitts auf `server.js` , die den folgenden Code enthält:

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

Als Nächstes fügen wir einige Platzhalter für die Benutzer, die wir von unserer Aufrufen von Applications erhalten.

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

Lassen Sie uns also und unsere Protokollierung zu erstellen.

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>Fügen Sie die MongoDB-Modell und Schemadateien Informationen mithilfe von Mongoose

Die frühere Vorbereitung auszahlt deaktivieren, wie Sie diese drei Dateien zusammen in einem REST-API-Dienst wieder abrufen.

Verwenden Sie für diese exemplarische Vorgehensweise MongoDB zum Speichern von Aufgaben, wie bereits zuvor erwähnt.

In der `config.js` Datei Sie Ihre Datenbank **Aufgabenliste**bezeichnet. Dieser Name wurde auch, was Sie am Ende des setzen die `mongoose_auth_local` Verbindungs-URL. Sie müssen nicht im Voraus in MongoDB dieser Datenbank zu erstellen. Es wird die Datenbank für Sie bei der ersten Ausführung der Anwendung Server erstellt.

Nachdem Sie die zu verwendende Datenbank MongoDB dem Server mitteilen, müssen Sie einige zusätzlichen Code zum Erstellen von die Modell und das Schema für Ihre Vorgänge Server schreiben.

### <a name="expand-the-model"></a>Erweitern des Modells

Dieses Schemamodell ist einfach. Sie können sie nach Bedarf erweitern.

`owner`:, Die dem Vorgang zugeordnet ist. Dieses Objekt ist eine **Zeichenfolge**.  

`Text`: Der Vorgang selbst. Dieses Objekt ist eine **Zeichenfolge**.

`date`: Der Termin, an dem die Aufgabe fällig ist. Dieses Objekt ist eine **Datetime**.

`completed`: Wenn der Vorgang abgeschlossen ist. Dieses Objekt ist **Boolean**.

### <a name="create-the-schema-in-the-code"></a>Erstellen Sie das Schema in den code

Ändern Sie über die Befehlszeile im Verzeichnis nach `azuread`, sofern es noch nicht angezeigt wird:

`cd azuread`

Öffnen der `server.js` Datei in einem Editor. Fügen Sie die folgenden Informationen unter dem Konfigurationseintrag hinzu:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Erstellen Sie zuerst das Schema aus, und klicken Sie dann erstellen Sie ein Modellobjekt, das Sie verwenden, um Ihre Daten in den Code speichern, wenn Sie Ihre **leitet**definieren.

## <a name="add-routes-for-your-rest-api-task-server"></a>Leitet für den REST-API Vorgang Server hinzufügen

Jetzt, da Sie eine Datenbankmodell für die Arbeit mit haben, fügen Sie die weitergeleitet, die Sie für den REST-API Server verwenden.

### <a name="about-routes-in-restify"></a>Informationen zu leitet in Restify

Leitet funktionieren in Restify auf die gleiche Weise, die sie bei der Verwendung von des Stapels Express funktionieren. Definieren Sie leitet mithilfe des URIS, der die Clientanwendungen Nummer erwartet. 

Eine typische Muster für eine Restify Routing lautet:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Restify und Express viel tieferen Funktionen, wie z. B. Anwendungstypen definieren und Ausführen komplexer routing über verschiedene Endpunkte angeben kann. Für die Zwecke dieses Lernprogramms halten wir diese leitet einfach.

#### <a name="add-default-routes-to-your-server"></a>Standard-leitet zu Ihrem Server hinzufügen

Fügen Sie jetzt die grundlegende CRUD leitet **Erstellen** und die **Liste** , für unsere REST-API. Anderen Ort leitet finden Sie der `complete` Zweig der Stichprobe.

Ändern Sie über die Befehlszeile im Verzeichnis nach `azuread`, sofern es noch nicht angezeigt wird:

`cd azuread`

Öffnen der `server.js` Datei in einem Editor. Im folgenden fügen Sie der Datenbankeinträge vorgenommenen über die folgenden Informationen hinzu:

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}
```

```Javascript
/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err)
            return next(err);

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Add one!");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}
```


#### <a name="add-error-handling-for-the-routes"></a>Fügen Sie Fehlerbehandlung für die Arbeitspläne hinzu

Fügen Sie einige Fehlerbehandlung, damit Sie Probleme, die auftreten wieder an den Kunden auf eine Weise kommunizieren können, die verstanden werden kann.

Fügen Sie den folgenden Code ein:

```Javascript
///--- Errors for communicating something interesting back to the client
function MissingTaskError() {
restify.RestError.call(this, {
statusCode: 409,
restCode: 'MissingTask',
message: '"task" is a required parameter',
constructorOpt: MissingTaskError
});
this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);
function TaskExistsError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 409,
restCode: 'TaskExists',
message: owner + ' already exists',
constructorOpt: TaskExistsError
});
this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);
function TaskNotFoundError(owner) {
assert.string(owner, 'owner');
restify.RestError.call(this, {
statusCode: 404,
restCode: 'TaskNotFound',
message: owner + ' was not found',
constructorOpt: TaskNotFoundError
});
this.name = 'TaskNotFoundError';
}
util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="create-your-server"></a>Erstellen Sie Ihre server

Sie haben nun definiert Ihrer Datenbank und Ihrer weitergeleitet werden sollen. Das letzte, was für Sie zu tun ist besteht darin, die Server-Instanz hinzuzufügen, die Ihre Anrufe verwaltet.

Restify und Express bieten umfassender Anpassung für einen Server REST-API, wobei wir die am häufigsten grundlegende Einrichtung hier verwenden. 

```Javascript

**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a>Fügen Sie die Arbeitspläne auf dem Server (ohne Authentifizierung)

```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

``` 

## <a name="add-authentication-to-your-rest-api-server"></a>Hinzufügen von Authentifizierung zu Ihrem Server REST-API

Jetzt, da Sie einen laufenden REST-API-Server verfügen, können Sie diese nützlichen gegen Azure AD machen.

Ändern Sie über die Befehlszeile im Verzeichnis nach `azuread`, sofern es noch nicht angezeigt wird:

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Verwenden Sie die OIDCBearerStrategy, die mit Passport-Azure-Active Directory enthalten ist


> [AZURE.TIP]
Wenn Sie APIs schreiben, sollten Sie immer die Daten verknüpfen mit einen eindeutigen Namen aus dem Token, die der Benutzer Spoofing kann nicht. Wenn der Server erledigen Elemente speichert, geschieht dies also basierend auf den **Oid** des Benutzers in das Token (bis token.oid genannt), der im Feld "Besitzer" wechselt. Dieser Wert wird sichergestellt, dass nur die Benutzer eigene Aufgaben zugreifen können. Es gibt keine anzeigen in den APIs der "Besitzer", damit ein externer Benutzer anderer Benutzer erledigen Elemente anfordern kann, selbst wenn sie authentifiziert werden.

Als Nächstes verwenden Sie die Person Strategie, die im Lieferumfang `passport-azure-ad`.

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

Passport verwendet das gleiche Muster für alle zugehörigen Strategien an. Sie übergeben es ein `function()` , bei dem `token` und `done` als Parameter. Die Strategie kommt zurück Sie, nachdem sie alle seine Arbeit abgeschlossen hat. Klicken Sie dann sollten Sie den Benutzer zu speichern und das Token speichern, damit Sie ihn erneut abgefragte müssen nicht.

> [AZURE.IMPORTANT]
Der obige Code hat jeder Benutzer, der zum Authentifizieren zu Ihrem Server geschieht. Dieses Verfahren wird als Autoregistration bezeichnet. Lassen Sie in der Herstellung-Servern sich nicht in einem beliebigen Benutzern den Zugriff die-API ohne zuvor, wechseln Sie durch einen Registrierungsprozess. Dieser Vorgang ist in der Regel das Muster aus, das die in Consumer apps, mit denen Sie angezeigt mithilfe von Facebook registrieren, aber bitten Sie Sie, um zusätzliche Informationen auszufüllen. Wenn dieses Programm Befehlszeile Programm wurde nicht, konnten wir die e-Mail aus dem token Objekt extrahiert haben, die zurückgegeben wird, und klicken Sie dann gestellte Benutzer zusätzliche Informationen ausfüllen. Da dies einer Stichprobe ist, müssen wir eine in-Memory-Datenbank hinzufügen.



## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a>Führen Sie die Server-Anwendung, um sicherzustellen, dass Sie lehnt ab

Sie können `curl` um festzustellen, ob Sie OAuth2-Schutz jetzt anhand Ihrer Endpunkte verfügen. Die Kopfzeilen zurückgegeben sollten genug, um Sie feststellen, dass Sie auf dem richtigen Weg sind.

Stellen Sie sicher, dass Ihre MongoDB Instanz ausgeführt wird:

    $sudo mongodb

Wechseln Sie zum Verzeichnis, und führen Sie den Server:

    $ cd azuread
    $ node server.js

Führen Sie in einem neuen Fenster terminal`curl`

Führen Sie einen einfachen Beitrag aus:

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Ein 401-Fehler ist die gewünschte Antwort an. Er gibt an, dass die Ebene Passport versucht, an den Endpunkt autorisieren umleiten.


## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>Sie verfügen nun über ein REST-API-Dienst, der OAuth2 verwendet.

Sie haben eine REST-API mithilfe von Restify und OAuth implementiert! Sie verfügen jetzt ausreichend Code, sodass des Diensts zu entwickeln und erstellen in diesem Beispiel fortgesetzt werden kann. Sie müssen nicht mehr angezeigt soweit Sie mit diesem Server, ohne einen OAuth2-kompatiblen Client können. Verwenden Sie eine zusätzliche – exemplarische Vorgehensweise wie unsere [Verbinden mit einem Web-API mithilfe von iOS mit B2C](active-directory-b2c-devquickstarts-ios.md) Exemplarische Vorgehensweise für die nächsten Schritt.


## <a name="next-steps"></a>Nächste Schritte

Sie können jetzt wie in kompliziertere Themen verschieben:

[Verbinden Sie mit einer Web-API mithilfe von iOS mit B2C](active-directory-b2c-devquickstarts-ios.md)