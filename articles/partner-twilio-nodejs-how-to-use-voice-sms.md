<properties 
    pageTitle="Verwenden von Twilio für VoIP, VoIP und SMS in Azure Messaging" 
    description="Informationen Sie zum Tätigen eines Telefonanrufs, und senden eine SMS-Nachricht mit dem Dienst Twilio API auf Azure. Beispiele in Node.js." 
    services="" 
    documentationCenter="nodejs" 
    authors="devinrader" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="wpickett"/>


# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Verwenden von Twilio für VoIP, VoIP und SMS in Azure Messaging

Mit diesem Leitfaden wird veranschaulicht, wie apps erstellen, die mit Twilio und node.js auf Azure kommunizieren.

<a id="whatis"/>
## <a name="what-is-twilio"></a>Was ist Twilio?

Twilio ist eine API-Plattform, mit die Hilfe für Entwickler einfach zu führen und Empfangen von Anrufen, senden und Empfangen von Textnachrichten und Einbetten VoIP aufrufen browserbasierte und einer systemeigenen Windows-Dienste.  Lassen Sie uns kurz wie dies funktioniert vor dem Einstieg.

### <a name="receiving-calls-and-text-messages"></a>Anrufe und Textnachrichten empfangen

Twilio ermöglicht Entwicklern das [kaufen programmierbare Telefonnummern] [ purchase_phone] der zum Senden und Empfangen von Anrufen und Textnachrichten verwendet werden können.  Wenn eine Zahl Twilio einer eingehenden Anruf oder Text empfängt, sendet Twilio Ihrer Webanwendung einer HTTP POST oder GET-Anforderung fragt Sie Anweisungen zum Behandeln des Anrufs oder Text.  Twilios HTTP-Anforderung mit [TwiML]wird der Server Antworten[twiml], einen einfachen Satz von XML-Tags, die Anweisungen zum Behandeln von Anrufen oder Text enthalten.  Beispiele für TwiML sehen wir gleich.

### <a name="making-calls-and-sending-text-messages"></a>Anrufe und Textnachrichten senden

Entwickler können, indem Sie auf die Twilio Webdienst-API HTTP-Anfragen machen, Textnachrichten senden oder ausgehende Telefonanrufe einleiten.  Für ausgehende Anrufe muss der Entwickler auch eine URL angeben, die gibt TwiML Anleitungen zu den ausgehenden Anruf zu verarbeiten, nachdem es angeschlossen ist.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>Einbetten von VoIP-Funktionen in Benutzeroberflächen-Code (JavaScript, iOS und Android)

Twilio bietet eine clientseitige SDK die alle desktop-Webbrowser, iOS-app oder Android-app in VoIP-Telefon umgewandelt werden kann.  In diesem Artikel liegt der Schwerpunkt auf so VoIP aufrufen im Browser verwenden.  Zusätzlich zu den Twilio JavaScript-SDK im Browser ausgeführt muss eine serverseitige Anwendung (unsere node.js) zu einer "Funktion" an den JavaScript-Client auszustellen verwendet werden.  Sie können weitere Informationen zur Verwendung von VoIP mit node.js [Blog Entwickler Twilio][voipnode].

<a id="signup"/>
## <a name="sign-up-for-twilio-microsoft-discount"></a>Melden Sie sich für Twilio (Microsoft Rabatt)

Bevor Sie Twilio Services, müssen Sie erste [für ein Konto anmelden][signup].  Microsoft Azure-Kunden erhalten Sie einen besonderen Rabatt - [Achten Sie hier registrieren][signup]!

<a id="azuresite"/>
## <a name="create-and-deploy-a-nodejs-azure-website"></a>Erstellen und Bereitstellen einer node.js Azure-Website

Als Nächstes müssen Sie eine Azure ausgeführt node.js-Website erstellen.  [Die offizielle Dokumentation hierfür befindet sich hier][azure_new_site].  Auf hoher Ebene werden Sie die folgenden Aktionen werden:

* Wenn Sie eine bereits besitzen anmelden für ein Azure-Konto
* Verwenden der Azure-Verwaltungskonsole zum Erstellen einer neuen website
* Hinzufügen von Unterstützung des Datenquellen-Steuerelement (angenommen, dass Sie Git verwendet)
* Erstellen einer Datei `server.js` mit einer einfachen node.js Webanwendung
* Diese einfache Anwendung in Azure bereitstellen

<a id="twiliomodule"/>
## <a name="configure-the-twilio-module"></a>Konfigurieren des Moduls Twilio

Als Nächstes beginnt wir mit eine einfachen node.js-Anwendung zu schreiben, die die Twilio-API verwendet.  Bevor wir beginnen, müssen wir unsere Twilio Anmeldeinformationen für das Konto konfigurieren.  

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Konfigurieren von Twilio Anmeldeinformationen in System-Umgebungsvariablen

Authentifizierte Anforderungen für die Back-End Twilio festlegen möchten, benötigen wir unsere Konto SID und autorisierende Token, welche Funktion als Benutzername und Kennwort für unsere Twilio-Konto einrichten. Die sicherste Methode zum Konfigurieren Sie diese für die Verwendung mit dem Modul Knoten in Azure ist über System-Umgebungsvariablen, die Sie direkt in der Azure-Verwaltungskonsole festlegen können.

Wählen Sie Ihre Website node.js aus, und klicken Sie auf den Link "Konfigurieren".  Wenn Sie einen Bildlauf nach unten ein bisschen durchführen, sehen Sie einen Bereich, in dem Sie für eine Anwendung Konfigurationseigenschaften festlegen können.  Geben Sie Ihre Anmeldeinformationen ein Konto Twilio ([gefunden auf dem Dashboard Twilio][twilio_dashboard]) Siehe - stellen Sie sicher, "TWILIO_ACCOUNT_SID" und "TWILIO_AUTH_TOKEN", jeweils einen Namen eingeben:

![Azure-Verwaltungskonsole][azure-admin-console]

Nachdem Sie diese Variablen konfiguriert haben, starten Sie die Anwendung in der Azure-Konsole neu.

### <a name="declaring-the-twilio-module-in-packagejson"></a>Das Modul Twilio in package.json deklarieren

Als Nächstes müssen wir eine package.json zum Verwalten von unserem Knoten Modul Abhängigkeiten über [Npm]zu erstellen.  Erstellen Sie auf der gleichen Ebene wie die "server.js"-Datei, die Sie in der Azure/node.js Lernprogramm erstellt haben eine Datei namens "package.json".  Platzieren Sie innerhalb dieser Datei Folgendes ein:

  {"Name": "Anwendung Name", "Version": "0.0.1", "Privat": WAHR, "Skripts": {"start": "Knoten Server"}, "Abhängigkeiten": {"express": "3.1.0", "Ejs": "*", "Twilio": "*"}}

Hiermit wird das Modul Twilio als eine Abhängigkeit als auch das beliebte [express Web Framework] deklariert[ express] und das Modul für EJS-Vorlage.  Ich klicke jetzt wir sind schon – wir Schreiben von Code!

<a id="makecall"/>
## <a name="make-an-outbound-call"></a>Stellen Sie einen ausgehenden Anruf

Lassen Sie uns Erstellen eines einfachen Formulars, das Anrufen wird in eine Zahl, dass wir auswählen.  Öffnen Sie server.js, und geben Sie den folgenden Code ein.  Beachten Sie, wo sie "E-Mail" - setzen den Namen Ihrer Website Azure es besagt:

    // Module dependencies
    var express = require('express'), 
      path = require('path'), 
      http = require('http'), 
      twilio = require('twilio');

    // Create Express web application
    var app = express();

    // Express configuration
    app.configure(function(){
      app.set('port', process.env.PORT || 3000);
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.favicon());
      app.use(express.logger('dev'));
      app.use(express.bodyParser());
      app.use(express.methodOverride());
      app.use(app.router);
      app.use(express.static(path.join(__dirname, 'public')));
    });
    app.configure('development', function(){
      app.use(express.errorHandler());
    });

    // Render an HTML user interface for the application's home page
    app.get('/', function(request, response) {
      response.render('index');
    });

    // Handle the form POST to place a call
    app.post('/call', function(request, response) {
      var client = twilio();
      client.makeCall({
          // make a call to this number
          to:request.body.number,

          // Change to a Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // A URL in our app which generates TwiML
          // Change "CHANGE_ME" to your app's name
          url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

    // Generate TwiML to handle an outbound call
    app.post('/outbound_call', function(request, response) {
      var twiml = new twilio.TwimlResponse();

      // Say a message to the call's receiver 
      twiml.say('hello - thanks for checking out Twilio and Azure', {
          voice:'woman'
      });

      response.set('Content-Type', 'text/xml');
      response.send(twiml.toString());
    });

    // Start server
    http.createServer(app).listen(app.get('port'), function(){
      console.log("Express server listening on port " + app.get('port'));
    });

Als Nächstes erstellen Sie ein Verzeichnis als "Ansichten" - innerhalb dieses Verzeichnis bezeichnet, erstellen Sie eine Datei mit dem Namen "index.ejs" mit dem folgenden Inhalt:

    <!DOCTYPE html>
    <html>
    <head>
      <title>Twilio Test</title>
      <style>
        input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
      </style>
    </head>
    <body>
      <h1>Twilio Test</h1>
      <form action="/call" method="POST">
          <input placeholder="Enter a phone number" name="number"/>
          <br/>
          <input type="submit" value="Call the number above"/>
      </form>
    </body>
    </html>

Nun bereitstellen Sie Ihrer Website zu Azure, und öffnen Sie Ihre Start.  Sie sollten nicht mehr geben Sie Ihre Telefonnummer in das Textfeld ein, und annehmen eines Anrufs von Ihrem Twilio Zahl!

<a id="sendmessage"/>
## <a name="send-an-sms-message"></a>Senden Sie eine SMS-Nachricht

Jetzt, richten Sie eine Benutzeroberfläche und Code für die Fehlerbehandlung Formular Textnachricht senden.  Öffnen von "server.js", und fügen Sie den folgenden Code nach dem letzten Aufrufen "app.post":

    app.post('/sms', function(request, response) {
      var client = twilio();
      client.sendSms({
          // send a text to this number
          to:request.body.number,

          // A Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // The body of the text message
          body: request.body.message
          
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

Fügen Sie in "views/index.ejs" einem anderen Formular unter der ersten Phase, und übermitteln Sie eine Zahl und eine Textnachricht hinzu:

    <form action="/sms" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input placeholder="Enter a message to send" name="message"/>
      <br/>
      <input type="submit" value="Send text to the number above"/>
    </form>

Ihrer Anwendung in Azure erneut bereitstellen, und Sie sollten jetzt übermitteln, das Formular, und senden selbst (oder eine gute Freunde) Textnachricht!

<a id="nextsteps"/>
## <a name="next-steps"></a>Nächste Schritte

Sie haben nun die Grundlagen der Verwendung von node.js und Twilio apps erstellen, die Kommunikation gelernt.  Aber in diesen Beispielen Kratzer kaum die Oberfläche, was mit Twilio und node.js möglich ist.  Weitere Informationen zum Verwenden von Twilio mit node.js zum Auschecken der folgenden Ressourcen:

* [Offizielle Modul Dokumente][docs]
* [VoIP-Lernprogramm mit node.js Applikationen][voipnode]
* [Votr - eine in Echtzeit SMS Abstimmung Anwendung mit node.js und CouchDB (drei Teile)][votr]
* [Paar Programmierung im Browser mit node.js][pair]

Wir hoffen, dass Sie gerne node.js und Twilio auf Azure Hacker!

[purchase_phone]: https://www.twilio.com/user/account/phone-numbers/available/local
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: /app-service-web/web-sites-nodejs-develop-deploy-mac.md
[twilio_dashboard]: https://www.twilio.com/user/account
[npm]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png



