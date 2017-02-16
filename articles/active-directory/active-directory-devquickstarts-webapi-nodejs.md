<properties
    pageTitle="Erste Schritte mit Azure AD-NodeJS | Microsoft Azure"
    description="So erstellen Sie eine Node.js REST Web-API, die in Azure AD für Authentifizierung integriert."
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
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="getting-started-with-web-api-for-node"></a>Erste Schritte mit WEB-API für Knoten

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

**Passport** ist Authentifizierung Middleware für Node.js. Extrem flexible und modulare, Passport abgelegt werden kann als zu beliebiger Express-basierten oder Webanwendung Resitify. Eine umfassende Reihe von Strategien unterstützt Authentifizierung über einen Benutzernamen und Kennwort, Facebook, Twitter und mehr. Wir haben für Microsoft Azure Active Directory eine Strategie entwickelt. Wir dieses Modul installieren und Hinzufügen klicken Sie dann auf Microsoft Azure Active Directory `passport-azure-ad` -Plug-in.

Um dies zu tun, müssen Sie:

1. Registrieren Sie sich eine Anwendung mit Azure AD
2. Richten Sie Ihre app aus, für die Passport Azure-Active Directory--Plug-in Verwendung.
3. Konfigurieren einer Clientanwendung aufrufen, die zum führen Liste Web-API

Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/Azure-Samples/active-directory-node-webapi)verwaltet.

> [AZURE.NOTE] In diesem Artikel befasst sich nicht auf Anmeldung, Anmeldung Implementierung und Verwaltung von Profilen mit Azure AD B2C aus.  Nachdem der Benutzer bereits authentifiziert wird Schwerpunkt einen Web APIs.  Wenn Sie noch nicht geschehen ist, sollten Sie mit der [Vorgehensweise zur Integration mit Azure Active Directory-Dokuments](./develop/active-directory-how-to-integrate.md) Informationen über die Grundlagen der Azure Active Directory beginnen.


Wir haben alle des Quellcodes für dieses laufenden Beispiel in GitHub unter MIT-Lizenz freigegeben, also datenbeschriftungsreihe (oder sogar Verzweigung!) gerne und Feedback und Anfragen extrahieren.

## <a name="about-nodejs-modules"></a>Informationen zu Modulen Node.js

Wir werden Node.js Module in diese exemplarische Vorgehensweise verwenden. Module werden geladen werden JavaScript-Paketen, die bestimmten Funktionen für die Anwendung bereitstellen. Installieren Sie normalerweise Module mithilfe der Node.js NPM im Installationsverzeichnis NPM, während einige Module, beispielsweise das HTTP-Modul enthalten sind die Core Node.js-Paket.
Installierte Module werden im Verzeichnis Node_modules im Stammverzeichnis der Node.js Installationsverzeichnis gespeichert. Jedem Modul im Verzeichnis Node_modules unterhält ein eigene Node_modules-Verzeichnis, das Module enthält, denen sie abhängig, und jedes erforderliches Modul hat ein Verzeichnis Node_modules. Diese rekursive Directory Struktur stellt die Abhängigkeitskette dar.

Diese Abhängigkeit Kette Struktur angegeben, ergibt sich eine größere Anwendung Platzbedarf, aber es wird sichergestellt, dass alle Abhängigkeiten erfüllt sind und die Version der bei der Entwicklung verwendeten Module auch bei Herstellung verwendet werden soll. Dadurch wird das Verhalten der Herstellung app mehr vorhersehbar und verhindert Versioning Problemen, die sich möglicherweise auf Benutzer auswirkt.

## <a name="1-register-a-azure-ad-tenant"></a>1. Registrieren einer Azure AD-Mandanten

Damit dieses Beispiel verwendet benötigen Sie einen Azure Active Directory-Mandanten. Wenn Sie unsicher sind welche ein Mandanten ist oder wie Sie erhalten möchten, finden Sie unter [wie abzurufenden einer Azure AD-Mandanten](active-directory-howto-tenant.md).

## <a name="2-create-an-application"></a>2. erstellen Sie eine Anwendung

Jetzt müssen Sie eine app in Ihrem Verzeichnis, beträchtlich erhöhen Azure AD einige Informationen zu erhalten, die sichere Kommunikation mit Ihrem app benötigt.  Die Client-app und die Web-API werden durch eine einzelne **Anwendung-ID** in diesem Fall dargestellt werden, da ein Wahrheitswert app bestehen aus.  [Wenn Sie eine app erstellen, gehen Sie wie folgt vor.](active-directory-how-applications-are-added.md) Wenn Sie eine Line of Business-app [können diese weiteren Anweisungen nützlich sein](active-directory-applications-guiding-developers-for-lob-applications.md)erstellen.

Achten Sie darauf, um:

- Melden Sie sich bei der Azure-Verwaltungsportal.
- Klicken Sie im linken Navigationsbereich auf **Active Directory**.
- Wählen Sie den Mandanten, in dem die Anwendung registriert werden soll.
- Klicken Sie auf die Registerkarte **Applications** aus, und klicken Sie auf der unteren Einzug hinzufügen.
- Erstellen Sie einer neuen **Webanwendung und/oder WebAPI**, und folgen Sie den Anweisungen.
    - Den **Namen** der Anwendung wird eine Anwendung an Endbenutzer beschrieben.
    - Die **Anmeldung URL** ist die Basis der app-URL.  Beispiel-Code Standardwert ist `https://localhost:8080`.
    - Der **App-ID-URI** ist ein eindeutiger Bezeichner für eine Anwendung.  Ist die Verwendung der Messe `https://<tenant-domain>/<app-name>`, z. B.`https://contoso.onmicrosoft.com/my-first-aad-app`
- Nachdem Sie die Registrierung abgeschlossen haben, zuweisen AAD Ihre app eine eindeutige Client-ID.  Sie benötigen diesen Wert in den nächsten Abschnitten, also kopieren Sie sie von der Registerkarte konfigurieren.

- Zusammenfassung: Erstellen einer **Anwendung geheim** für eine Anwendung, und kopieren Sie ihn nach unten.  Sie benötigen diese in Kürze.
- Erinnerung: Kopieren nach unten die **ID der Anwendung** , die zu Ihrer Anwendung zugeordnet ist.  Sie auch benötigen diese in Kürze.


## <a name="3-download-nodejs-for-your-platform"></a>3. node.js für Ihre Plattform herunterladen
Damit dieses Beispiel erfolgreich verwendet werden soll, müssen Sie eine Installation funktioniert der Node.js verfügen.

Installieren Sie Node.js aus [http://nodejs.org](http://nodejs.org).

## <a name="4-install-mongodb-on-to-your-platform"></a>4: Installieren Sie MongoDB an Ihre Plattform

Damit dieses Beispiel erfolgreich verwendet werden soll, müssen Sie eine Installation funktioniert der MongoDB verfügen. Wir verwenden MongoDB unsere Autorisierungstypen REST-API über Serverinstanzen vornehmen.

Installieren Sie MongoDB aus [http://mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] Diese exemplarische Vorgehensweise wird vorausgesetzt, dass die Standard-Installation und Server Endpunkte MongoDB, verwenden Sie also zum Zeitpunkt der Erstellung dieses Dokuments ist: Mongodb://localhost


## <a name="5-install-the-restify-modules-in-to-your-web-api"></a>5. installieren Sie die Restify-Module in Ihrer Web-API

Wir werden Resitfy verwenden, um unser REST-API zu erstellen. Restify ist eine minimale und flexible Node.js Anwendungsframework abgeleitet von Express, eine umfangreiche Sammlung von Features zum Erstellen von REST-APIs auf Verbinden ist.

### <a name="install-restify"></a>Installieren Restify

Über die Befehlszeile wechseln Sie zum Verzeichnis Azuread. Wenn das **Azuread** Verzeichnis nicht vorhanden ist, erstellen Sie ihn aus.

`cd azuread - or- mkdir azuread; cd azuread`

Geben Sie den folgenden Befehl ein:

`npm install restify`

Dieser Befehl installiert Restify.

#### <a name="did-you-get-an-error"></a>ANGEZEIGT SIE EINE FEHLERMELDUNG?

Wenn Npm einige unter den Betriebssystemen verwenden, erhalten Sie möglicherweise einen Fehler des Fehlers: EPERM, Chmod "/ Usr/lokale/Bin /..." und eine Besprechungsanfrage zu versuchen, das Konto als Administrator ausführen. In diesem Fall mithilfe des Befehls Sudo Npm mit einer höheren Berechtigungsstufe ausführen.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>HABEN SIE EINEN FEHLER IM HINBLICK AUF DTRACE ERHALTEN?

Sie wird möglicherweise ungefähr wie folgt angezeigt, bei der Installation Restify von:

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


Restify bietet ein leistungsfähiges Verfahren zum REST Anrufe mit DTrace zu verfolgen. Viele Betriebssysteme haben jedoch keine DTrace zur Verfügung. Sie können diesen Fehler ignorieren.


Die Ausgabe dieses Befehls sollte ähnlich wie die folgende angezeigt:


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


## <a name="6-install-passportjs-in-to-your-web-api"></a>6. installieren Sie Passport.js in der Web-API

[Passport](http://passportjs.org/) ist Authentifizierung Middleware für Node.js. Extrem flexible und modulare, Passport abgelegt werden kann als zu beliebiger Express-basierten oder Webanwendung Resitify. Eine umfassende Reihe von Strategien unterstützt Authentifizierung über einen Benutzernamen und Kennwort, Facebook, Twitter und mehr. Wir haben für Azure Active Directory eine Strategie entwickelt. Wir dieses Modul installieren und dann die Strategie Azure Active Directory-Plug-In hinzufügen.

Über die Befehlszeile wechseln Sie zum Verzeichnis Azuread.

Geben Sie den folgenden Befehl aus, um passport.js zu installieren.

`npm install passport`

Die Ausgabe des Befehls sollte ähnlich wie die folgende angezeigt:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="7-add-passport-azure-ad-to-your-web-api"></a>7 fügen Sie 7 Passport-Azure-Active Directory zu Ihrem Web-API

Wir werden fügen Sie die Strategie OAuth mit Passport-Azuread, eine Reihe von Strategien, die mit Passport Azure Active Directory zu verbinden. Wir verwenden diese Strategie für Person Token in diesem Beispiel Rest-API.

> [AZURE.NOTE] Obwohl OAuth2 ein Framework bereitstellt, in dem alle bekannter token Typ ausgestellt werden kann, müssen nur bestimmte token Typen umfassende verwenden gewonnen. Zum Schutz von Endpunkten hat, die ergebende Person Token sein. Person Token sind die am häufigsten ausgestellt stark Typ von Token in OAuth2 und viele Implementierungen wird davon ausgegangen, dass die Person Token der einzige Typ von Token ausgestellt werden.

Über die Befehlszeile wechseln Sie in der Azuread Verzeichnis

Geben Sie den folgenden Befehl zum Passport.js Passport-Azure-Active Directory-Modul installieren:

`npm install passport-azure-ad`

Die Ausgabe des Befehls sollte ähnlich wie die folgende angezeigt:

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



## <a name="8-add-mongodb-modules-to-your-web-api"></a>8 fügen Sie 8 MongoDB Module zu Ihrer Web-API

Wir verwenden MongoDB als unsere Datenspeicher aus diesem Grund, wir müssen beide am häufigsten verwendeten installieren-Plug-in zum Verwalten von Modelle und Schemas bezeichnet Mongoose sowie den Datenbanktreiber für MongoDB, die Abkürzung MongoDB.


* `npm install mongoose`

## <a name="9--install-additional-modules"></a>9: Installieren Sie zusätzliche Module

Installieren Sie dann die verbleibenden erforderlichen Module.


Ändern Sie Verzeichnisse zu dem Ordner **Azuread** sofern nicht bereits vorhanden, über die Befehlszeile:

`cd azuread`


Geben Sie die folgenden Befehle, die folgenden Module in Ihrem Verzeichnis Node_modules zu installieren:

* `npm install assert-plus`
* `npm install bunyan`
* `npm update`


## <a name="10-create-a-serverjs-with-your-dependencies"></a>10 erstellen Sie 10 eine server.js mit Ihrem Abhängigkeiten

Die Datei server.js wird die meisten unserer Funktionalität für unsere Web-API Server bietet. Wir werden die meisten unser Code in dieser Datei hinzufügen. Gestalten Sie für die Herstellung auf die Funktionen in, um kleinere Dateien, wie z. B. getrennten Pfade und Controller werden soll. In dieser Demo werden wir server.js für dieses Feature verwenden.

Ändern Sie Verzeichnisse zu dem Ordner **Azuread** sofern nicht bereits vorhanden, über die Befehlszeile:

`cd azuread`

Erstellen einer `server.js` unserer bevorzugten-Editor-Datei, und fügen Sie die folgende Informationen:

```Javascript
    'use strict';

    /**
    * Module dependencies.
    */

    var fs = require('fs');
    var path = require('path');
    var util = require('util');
    var assert = require('assert-plus');
    var bunyan = require('bunyan');
    var getopt = require('posix-getopt');
    var mongoose = require('mongoose/');
    var restify = require('restify');
    var passport = require('passport');
  var BearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Speichern Sie die Datei ein. Wir werden in Kürze zu dieser zurückkehren.

## <a name="11-create-a-config-file-to-store-your-azure-ad-settings"></a>11:. Erstellen einer Config-Datei zum Speichern Ihrer Azure AD-Einstellungen

Diese Codedatei übergibt die Konfigurationsparameter aus Ihrer Azure Active Directory-Portal Passport.js. Diese Konfigurationswerte wird erstellt, wenn Sie die Web-API auf das Portal im ersten Teil der Anleitung hinzugefügt. Wir wird erläutert, wie die Werte dieser Parameter setzen, nachdem Sie den Code kopiert haben.


Ändern Sie Verzeichnisse zu dem Ordner **Azuread** sofern nicht bereits vorhanden, über die Befehlszeile:

`cd azuread`

Erstellen einer `config.js` unserer bevorzugten-Editor-Datei, und fügen Sie die folgende Informationen:

```Javascript
 exports.creds = {
     mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
     clientID: 'your client ID',
     audience: 'your application URL',
    // you cannot have users from multiple tenants sign in to your server unless you use the common endpoint
  // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
     identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
     validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in to your server
     passReqToCallback: false,
     loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes to stderr in Unix.

 };


```
Speichern Sie die Datei ein.

## <a name="12-add-configuration-to-your-serverjs-file"></a>12. Konfiguration zu Ihrer Datei server.js hinzufügen

Wir müssen, lesen Sie diese Werte aus der Konfigurationsdatei soeben erstellten über unsere Anwendung. Hierzu wir einfach die config-Datei als Ressource in unserer Anwendung hinzufügen, und legen Sie anschließend die globalen Variablen auf, die im Dokument config.js

Ändern Sie Verzeichnisse zu dem Ordner **Azuread** sofern nicht bereits vorhanden, über die Befehlszeile:

`cd azuread`

Öffnen Ihrer `server.js` unserer bevorzugten-Editor-Datei, und fügen Sie die folgende Informationen:

```Javascript
var config = require('./config');
```
Klicken Sie dann auf einen neuen Abschnitt hinzufügen `server.js` mit den folgenden Code:

```Javascript
var options = {
    // The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback,
    loggingLevel: config.creds.loggingLevel

};

// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;

// Our logger
var log = bunyan.createLogger({
    name: 'Azure Active Directory Bearer Sample',
         streams: [
        {
            stream: process.stderr,
            level: "error",
            name: "error"
        },
        {
            stream: process.stdout,
            level: "warn",
            name: "console"
        }, ]
});

  // if logging level specified, switch to it.
  if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
```

Speichern Sie die Datei ein.



## <a name="13-add-the-mongodb-model-and-schema-information-using-moongoose"></a>13. Fügen Sie MongoDB Modell und Schemainformationen mithilfe von Moongoose hinzu

Jetzt geht alle diese Vorbereitung bezahlt, wie wir diese drei Dateien zusammen in einem Dienst REST-API wind starten.

Für diese exemplarische Vorgehensweise werden wir MongoDB verwenden, um unser Vorgänge zu speichern, wie in ***Schritt 4***erläutert.

Wenn diese nicht zurückgerufen aus der `config.js` Datei, die wir erstellt, klicken Sie in ***Schritt 11 haben*** wir unserer Datenbank aufgerufen `tasklist` wie das war, was wir am Ende unserer Mogoose_auth_local Verbindungs-URL setzen. Sie brauchen diese Datenbank im Voraus in MongoDB erstellen erstellen, sondern wird dies für uns beim ersten Ausführen der unsere Server-Anwendung (vorausgesetzt, dass er nicht bereits vorhanden ist).

Jetzt, da wir dem Server welche MongoDB Datenbank eingerichtet haben, wir verwenden möchten, benötigen wir einige zusätzlichen Code zum Erstellen von die Modell und das Schema für Vorgänge des Servers schreiben.

#### <a name="discussion-of-the-model"></a>Diskussion des Modells

Unser Modell Schema ist sehr einfach, und erweitern Sie es nach Bedarf.

NAME - der Name des, die dem Vorgang zugeordnet ist. Eine ***Zeichenfolge***

Aufgabe - der Vorgang selbst. Eine ***Zeichenfolge***

Datum – das Datum, das die Aufgabe fällig ist. Ein ***DATETIME***

ABGESCHLOSSEN – Wenn der Vorgang oder nicht abgeschlossen ist. ***BOOLESCHE***

#### <a name="creating-the-schema-in-the-code"></a>Erstellen das Schema in den code


Ändern Sie Verzeichnisse zu dem Ordner **Azuread** sofern nicht bereits vorhanden, über die Befehlszeile:

`cd azuread`

Öffnen Ihrer `server.js` unserer bevorzugten-Editor-Datei, und fügen Sie die folgenden Informationen unter dem Konfigurationseintrag hinzu:

```Javascript
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    task: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Wie Sie in den Code erkennen können, wir unseres Schemas erstellen und dann erstellen Sie ein Modellobjekt, mit denen wir unsere Daten im gesamten Code speichern, wenn wir unsere ***leitet***definieren.

## <a name="14-add-our-routes-for-our-task-rest-api-server"></a>14. Hinzufügen von unsere leitet für unsere Server Vorgang REST-API

Fügen Sie nun verfügen wir über ein Datenbankmodell für die Arbeit mit wir die weitergeleitet, die wir für unsere Server REST-API verwendet wird.

### <a name="about-routes-in-restify"></a>Informationen zu leitet in Restify

Leitet funktionieren Restify auf genau dieselbe Weise, wie den Stapel Express verwenden. Sie definieren die Arbeitspläne mithilfe des URIS, der die Clientanwendungen Nummer erwartet. In der Regel definieren Sie Ihre leitet in einer separaten Datei. Für unsere Zwecke versetzt wir unsere leitet in der Datei server.js. Es empfiehlt sich, dass Sie diese in die eigene Datei für die Verwendung der Herstellung Faktor, in.

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


Dies ist das Muster auf der grundlegendsten Ebene. Bieten viel tieferen Functionaltiy wie Anwendungstypen definieren und Ausführen komplexer routing über verschiedene Endpunkte Resitfy (und Express). Für unsere Zwecke werden wir diese ist sehr einfach beibehalten.

### <a name="1-add-default-routes-to-our-server"></a>1. Standard leitet an unseren Server hinzufügen

Wir werden nun hinzufügen die grundlegende CRUD leitet der erstellen, abrufen, aktualisieren und löschen.

Ändern Sie Verzeichnisse zu dem Ordner **Azuread** sofern nicht bereits vorhanden, über die Befehlszeile:

`cd azuread`

Öffnen Ihrer `server.js` unserer bevorzugten-Editor-Datei, und fügen Sie die folgende Informationen unter der Datenbankeinträge über vorgenommenen:

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

    if (!req.params.task) {
        req.log.warn('createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.task = req.params.task;
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


// Delete a task by name

function removeTask(req, res, next) {

    Task.remove({
        task: req.params.task,
        owner: owner
    }, function(err) {
        if (err) {
            req.log.warn(err,
                'removeTask: unable to delete %s',
                req.params.task);
            next(err);
        } else {
            log.info('Deleted task:', req.params.task);
            res.send(204);
            next();
        }
    });
}

// Delete all tasks

function removeAll(req, res, next) {
    Task.remove();
    res.send(204);
    return next();
}


// Get a specific task based on name

function getTask(req, res, next) {

    log.info('getTask was called for: ', owner);
    Task.find({
        owner: owner
    }, function(err, data) {
        if (err) {
            req.log.warn(err, 'get: unable to read %s', owner);
            next(err);
            return;
        }

        res.json(data);
    });

    return next();
}

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

        if (err) {
            return next(err);
        }

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Did you initalize the database as stated in the README?");
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

### <a name="2-next-lets-add-some-error-handling-in-our-apis"></a>2. weiter, lassen Sie uns einige Fehlerbehandlung in unsere APIs hinzufügen:

```

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


## <a name="15-create-your-server"></a>15 erstellen Sie 15 Ihre Server!

Wir unserer Datenbank definiert haben, wir unsere leitet angeordnet haben, und das letzte, was zu tun ist ist unsere Server-Instanz hinzufügen, die unsere Anrufe verwalten soll.

Restify (und Express) haben Sie zahlreiche umfassender Anpassung können Sie für einen Server REST-API ausführen, aber erneut wir die am häufigsten grundlegende Einrichtung für unsere Zwecke verwenden.

```Javascript
/**
 * Our Server
 */


var server = restify.createServer({
    name: "Azure Active Directroy TODO Server",
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
```

## <a name="16-adding-the-routes-to-the-server-without-authentication-for-now"></a>16. hinzufügen die Arbeitspläne auf dem Server (ohne Authentifizierung jetzt)

```Javascript
/// Now the real handlers. Here we just CRUD
/**
/*
/* Each of these handlers are protected by our OIDCBearerStrategy by invoking 'oidc-bearer'
/* in the pasport.authenticate() method. We set 'session: false' as REST is stateless and
/* we don't need to maintain session state. You can experiement removing API protection
/* by removing the passport.authenticate() method like so:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```

## <a name="17-before-we-add-oauth-support-lets-run-the-server"></a>17. bevor wir OAuth Support hinzufügen möchten, führen wir auf den Server.

Testen Sie Ihrer Server vor dem Hinzufügen von Authentifizierung

Die einfachste Möglichkeit hierfür ist mithilfe von Curl in einer Befehlszeile. Bevor wir dies tun, benötigen wir ein einfaches Programm, uns Ausgabe als JSON zu analysieren. Installieren Sie dazu das Json-Tool wie die folgenden Beispielen, verwenden.

`$npm install -g jsontool`

Dadurch wird das JSON-Tool Global installiert. Jetzt, da wir die – erreicht haben los geht's mit dem Server:

Vergewissern Sie sich, dass Ihre MonogoDB Instanz ausgeführt wird.

`$sudo mongod`

Klicken Sie dann wechseln Sie zum Verzeichnis, und starten Sie gewelltem..

`$ cd azuread`
`$ node server.js`

`$ curl -isS http://127.0.0.1:8080 | json`

```Shell
HTTP/1.1 200 OK
Connection: close
Content-Type: application/json
Content-Length: 171
Date: Tue, 14 Jul 2015 05:43:38 GMT
[
"GET /",
"POST /tasks/:owner/:task",
"POST /tasks (for JSON body)",
"GET /tasks",
"PUT /tasks/:owner",
"GET /tasks/:owner",
"DELETE /tasks/:owner/:task"
]
```

Wir können eine Aufgabe klicken Sie dann auf diese Weise hinzufügen:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

Die Antwort sollten:

```Shell
HTTP/1.1 201 Created
Connection: close
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: X-Requested-With
Content-Type: application/x-www-form-urlencoded
Content-Length: 5
Date: Tue, 04 Feb 2014 01:02:26 GMT
Hello
```
Und wir können die Liste Aufgaben für Brandon auf diese Weise:

`$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Wenn alle dies funktioniert ab, können wir die REST-API Server OAuth hinzu.

**Sie haben einen REST-API-Server mit MongoDB!**


## <a name="18-add-authentication-to-our-rest-api-server"></a>18. Authentifizierung für unsere REST-API Server hinzufügen

Nun wir einer laufenden REST-API verfügen (Herzlichen btw!) erfahren Sie, wodurch es sinnvoll gegen Azure AD.

Ändern Sie Verzeichnisse zu dem Ordner **Azuread** sofern nicht bereits vorhanden, über die Befehlszeile:

`cd azuread`

### <a name="1-use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>1: Verwenden Sie die OIDCBearerStrategy, die mit Passport-Azure-Active Directory enthalten ist

Wir haben bisher einen typischen REST erledigen Server ohne jede Art von Autorisierung aufgebaut. Dies ist die Stelle, an der zusammengestellt, die zunächst.

Zuerst müssen wir darauf hinzuweisen, dass wir die Verwendung eines möchten. Setzen Sie dieses Recht nach Ihrer Server-Konfiguration aus:

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
Beim Erstellen von APIs sollten Sie immer die Daten verknüpfen mit einen eindeutigen Namen aus dem Token, die der Benutzer Spoofing nicht möglich. Wenn dieser Server erledigen Elemente gespeichert werden, gespeichert wird, basierend auf die Objekt-ID des Benutzers im (bis token.oid genannt) Token wir setzen Sie im Feld "Besitzer". Dies stellt sicher, dass nur für diesen Benutzer kann seine TODOs und niemand haben Zugriff auf, die TODOs eingegeben. Es gibt keine anzeigen in den APIs der "Besitzer", damit ein externer Benutzer gegenseitig TODOs anfordern kann, selbst wenn sie authentifiziert werden.

Als Nächstes verwenden wir Strategie der Person, die mit Passport-Azure-Active Directory stammen. Nur sehen Sie sich den Code jetzt, es wird in Kürze erläutert. Setzen Sie dies nach dem, was Sie über pated:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users
/*
/* Passport pattern provides the need to manage users and info tokens
/* with a FindorCreate() method that must be provided by the implementor.
/* Here we just autoregister any user and implement a FindById().
/* You'll want to do something smarter.
**/

var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.sub === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var bearerStrategy = new BearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.sub);
                users.push(token);
                owner = token.sub;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(bearerStrategy);
```

Passport verwendet ein ähnliches Muster für alle, Strategien (Twitter, Facebook usw.), die alle Strategie Autoren entsprechen. Die Strategie betrachtet wird, dass wir es eine möchten übergeben, die ein Token und als die Parameter fertig sind. Die Strategie verlässliche immer wieder an uns nachdem alle bedeutet es handelt sich um Arbeit. Nachdem bedeutet möchten wir Speichern des Benutzers, und speichern das Token, damit wir nicht erneut dafür Fragen benötigen.

> [AZURE.IMPORTANT]
Der obige Code hat jeder Benutzer, die zum Authentifizieren an unseren Server geschieht. Dies wird als automatische Registrierung bezeichnet. Herstellung Servern möchten Sie würde nicht zulassen, dass jeder ohne zuvor diese mithilfe eines Registrierungsprozesses wechseln, die Sie sich entscheiden. Dies ist normalerweise der Muster, die, das Sie in Consumer apps, wer ermöglichen es Ihnen sehen, mit Facebook registrieren, aber bitten Sie Sie, um zusätzliche Informationen auszufüllen. Wenn das Programm Befehlszeile war nicht, konnte wir nur die e-Mail aus dem token Objekt extrahiert haben, die zurückgegeben wird, und dann aufgefordert werden, um zusätzliche Informationen auszufüllen. Da dies ein Test-Server wir einfach die Datenbank im Speicher hinzufügen ist.

### <a name="2-finally-protect-some-endpoints"></a>2. einige Endpunkte schließlich zu schützen

Schützen Sie Endpunkte, durch Angabe der `passport.authenticate()` aufrufen, mit dem Protokoll, das Sie verwenden möchten.

Lassen Sie uns bearbeiten unsere Routing in unserem Servercode etwas interessanter ausführen:

```Javascript
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="19-run-your-server-application-again-and-ensure-it-rejects-you"></a>19 führen Sie die Server-Anwendung erneut, und stellen Sie sicher, dass Sie lehnt ab

Wir verwenden `curl` erneut aus, um festzustellen, ob wir nun OAuth2 Schutz gegen unsere Endpunkte haben. Wir werden dies vor ausgeführt unsere Kunden SDKs für diesen Endpunkt Aktionen. Die Kopfzeilen zurückgegeben sollten genug an uns mitteilen, dass wir ab dem richtigen Weg sind.

Vergewissern Sie sich, dass Ihre MonogoDB Instanz ausgeführt wird:

  $sudo mongod

Klicken Sie dann in das Verzeichnis ändern Sie und starten Sie gewelltem..

  $ cd Azuread $ Knoten server.js

Führen Sie einen einfachen Beitrag aus:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Ein 401 ist die Antwort, die hier von Ihnen gesuchte aus, wie das angibt, dass die Ebene Passport leiten Sie an den Endpunkt autorisieren, also genau das gewünschte will.

## <a name="congratulations-you-have-a-rest-api-service-using-oauth2"></a>Herzlichen Glückwunsch! Sie haben eine REST-API-Dienst verwenden OAuth2!

Sie haben Problems soweit mit diesem Server können Sie ohne einen kompatiblen OAuth2-Client. Sie müssen über eine zusätzliche exemplarische Vorgehensweise zu wechseln.

Wenn Sie nur Informationen zum Implementieren eines REST-API mit Restify und OAuth2 gesucht haben, müssen Sie mehr als genug Code Entwickeln des Diensts beibehalten, und lernen, wie Sie in diesem Beispiel zu erstellen.

Wenn Sie in Ihrer ADAL Reise in den nächsten Schritten interessiert sind, können hier einige unterstützten ADAL Clients, sollten Sie behalten Arbeit:

Einfach auf Ihrem Computer Entwicklers Klonen Sie, und konfigurieren Sie, wie in der Anleitung angegeben.

[ADAL für iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[ADAL für Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
