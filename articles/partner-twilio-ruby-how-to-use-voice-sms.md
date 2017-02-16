<properties 
    pageTitle="Verwendung von Twilio für VoIP und SMS (Ruby) | Microsoft Azure" 
    description="Informationen Sie zum Tätigen eines Telefonanrufs, und senden eine SMS-Nachricht mit dem Dienst Twilio API auf Azure. Codebeispielen in Ruby geschrieben wurde." 
    services="" 
    documentationCenter="ruby" 
    authors="devinrader" 
    manager="twilio" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Verwendung von Twilio für VoIP und SMS-Funktionen in Ruby
Mit diesem Leitfaden wird veranschaulicht, wie häufig vorkommende Aufgaben mit dem Dienst Twilio API auf Azure ausführen. Die Szenarios dieser gehören Durchführung eines Telefonanrufs, und senden die Nachricht eine kurze Textnachricht (SMS). Weitere Informationen zu Twilio und Voicemail und SMS in Ihrer Anwendung verwenden finden Sie unter Abschnitt [Weitere Schritte](#NextSteps) .

## <a name="a-idwhatisawhat-is-twilio"></a><a id="WhatIs"></a>Was ist Twilio?
Twilio ist eine Telefonie Webdienst-API, die Sie Ihre Kenntnisse und vorhandenen Web Sprachen Voicemail und SMS Applications erstellen kann. Twilio ist eine Drittanbieter-Dienst (kein Azure Feature und nicht mit einem Microsoft-Produkt).

**VoIP Twilio** ist die Ihrer Anwendung tätigen und entgegennehmen Telefonanrufe. **Twilio SMS** ermöglicht Ihrer Anwendung tätigen und Entgegennehmen von SMS-Nachrichten an. **Twilio Client** ist die Ihrer Anwendung VoIP-Kommunikation mit vorhandenen internetverbindungen, einschließlich der mobile Verbindungen zu ermöglichen.

## <a name="a-idpricingatwilio-pricing-and-special-offers"></a><a id="Pricing"></a>Twilio Preise und spezielle Angebote
Informationen zur Preisgestaltung Twilio steht zu einem [Preis Twilio] [twilio_pricing]. Azure Kunden erhalten eine [spezielle Angebot][special_offer]: einer kostenlosen Gutschrift 1000 Texte oder Minuten eingehende 1000. Zum Anmelden für dieses Angebot oder Weitere Informationen zu erhalten, besuchen Sie [http://ahoy.twilio.com/azure][special_offer].  

## <a name="a-idconceptsaconcepts"></a><a id="Concepts"></a>Konzepte
Die Twilio-API ist ein Rest API, Sprach- und SMS-Funktionalität für Applikationen bereitstellt. Client-Bibliotheken sind in mehreren Sprachen verfügbar. eine Liste finden Sie unter [Twilio API-Bibliotheken] [twilio_libraries].

### <a name="a-idtwimlatwiml"></a><a id="TwiML"></a>TwiML
TwiML ist eine Reihe von XML-basierten Anweisungen, die Twilio zum Verarbeiten eines Anrufs oder SMS zu informieren.

Beispielsweise würde die folgenden TwiML den Text **Hallo Welt** Symbolleisten konvertieren.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Alle TwiML Dokumente `<Response>` als deren Stammelement. Hier verwenden Sie Twilio Verben, um das Verhalten der Anwendung zu definieren.

### <a name="a-idverbsatwiml-verbs"></a><a id="Verbs"></a>TwiML Verben
Twilio Verben sind XML-Tags, die was zu **tun ist**die Twilio verwendet. Angenommen, die ** &lt;sagen&gt; ** Verb weist Twilio, eine Nachricht in einem Anruf hörbar zu übermitteln. 

Im folgenden finden eine Liste der Twilio Verben.

* ** &lt;Einwahlnummern&gt;**: den Anrufer an ein anderes Telefon verbindet.
* ** &lt;Sammeln&gt;**: sammelt Ziffern auf der Zehnertastatur Telefon eingegeben.
* ** &lt;Auflegen&gt;**: ein Anrufs endet.
* ** &lt;Wiedergeben&gt;**: eine Audiodatei.
* ** &lt;Anhalten&gt;**: im Hintergrund für eine angegebene Anzahl von Sekunden wartet.
* ** &lt;Datensatz&gt;**: Einträge des Anrufers VoIP und gibt eine URL einer Datei, die die Aufzeichnung enthält.
* ** &lt;Umleiten&gt;**: Steuerung eines Anrufs oder SMS an die TwiML unter einem anderen URL.
* ** &lt;Ablehnen&gt;**: einen eingehenden Anruf an Ihre Nummer Twilio ablehnt, ohne Sie Abrechnung
* ** &lt;Sagen&gt;**: konvertiert Text in Sprache, die in einem Anruf vorgenommen werden.
* ** &lt;Sms&gt;**: sendet eine SMS-Nachricht.

Weitere Informationen zu Twilio Verben, deren Attribute und TwiML finden Sie unter [TwiML] [twiml]. Weitere Informationen über die Twilio-API, finden Sie unter [Twilio API] [twilio_api].

## <a name="a-idcreateaccountacreate-a-twilio-account"></a><a id="CreateAccount"></a>Erstellen Sie ein Konto Twilio
Wenn Sie bereit sind, ein Konto Twilio erhalten, melden Sie sich an [Twilio versuchen Sie es] [try_twilio]. Sie können ein kostenloses Konto verwenden, und später upgrade Ihres Kontos.

Wenn Sie sich für ein Konto Twilio registrieren, erhalten Sie eine kostenlose Telefonnummer für eine Anwendung. Sie erhalten auch ein Konto SID und eine autorisierende Token. Beide werden erforderlich sein, um Twilio API anzurufen. Um unbefugten Zugriff auf Ihr Konto zu verhindern, schützen Sie Ihrer Authentifizierungstoken. Ihr Konto SID und autorisierende Token sind auf der [Kontoseite Twilio]machen[twilio_account], in die Felder Beschriftung **SID zu berücksichtigen** und **Autorisierende TOKEN**,.

### <a name="a-idverifyphonenumbersaverify-phone-numbers"></a><a id="VerifyPhoneNumbers"></a>Vergewissern Sie sich Telefonnummern
Neben der Anzahl erhalten Sie von Twilio, Sie können auch feststellen, Zahlen Steuerelement (d. h. Ihr Mobiltelefon oder privat Telefonnummer) zur Verwendung in Ihrer Anwendung. 

Informationen zum Überprüfen, ob eine Telefonnummer finden Sie unter [Verwalten von Zahlen] [verify_phone].

## <a name="a-idcreateappacreate-a-ruby-application"></a><a id="create_app"></a>Erstellen Sie eine Anwendung Ruby
Eine Ruby Anwendung, verwendet den Twilio-Dienst in Azure läuft unterscheidet sich nicht als andere Ruby Anwendung, die den Twilio-Dienst verwendet. Während Twilio Services RESTful sind und von Ruby auf verschiedene Weise aufgerufen werden können, wird in diesem Artikel erfahren Sie, wie Sie Twilio Services mit [Twilio Hilfebibliothek für die Ruby]verwenden konzentrieren[twilio_ruby].

Zuerst [ansetzen eines neuen Azure Linux virtuellen Computers] [ azure_vm_setup] als Host für Ihre neue Ruby Webanwendung fungieren. Ignorieren Sie die Schritte im Zusammenhang mit der Erstellung einer app Schienen soeben angelegt den virtuellen Computer an. Stellen Sie sicher, dass Sie einen Endpunkt mit einer externen Port 80 und dem internen Anschluss von 5000 erstellen.

In den folgenden Beispielen wir verwenden [Sinatra][sinatra], eine sehr einfache Web Framework für Ruby. Sie können jedoch sicher über Twilio Helper Bibliothek für Ruby mit einem beliebigen anderen Web Framework, einschließlich Ruby Schienenfahrzeugen.

SSH in Ihrem neuen virtuellen Computer, und erstellen Sie ein Verzeichnis für Ihre neue app. Innerhalb dieses Verzeichnis erstellen Sie eine Datei mit der Bezeichnung Gemfile, und kopieren Sie den folgenden Code hinein:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

Klicken Sie auf die Befehlszeile ausführen `bundle install`. Hiermit wird die oben genannten Abhängigkeiten installiert. Als Nächstes erstellen Sie eine Datei namens `web.rb`. Dadurch werden, in dem der Code für Ihre Web app verfügbar ist. Fügen Sie den folgenden Code ein:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

An diesem Punkt sollte es möglich sein Ausführen den Befehl `ruby web.rb -p 5000`. Dies wird Drehfeld einen kleinen Webserver auf Port 5000 auszurichten. Sie sollten diese App in Ihrem Browser durchsuchen, indem Sie die URL besuchen Sie ansetzen für Ihre Azure-virtuellen Computer. Nachdem Sie die Web-app im Browser erreichen können, können Sie beginnen, erstellen eine app Twilio.

## <a name="a-idconfigureappaconfigure-your-application-to-use-twilio"></a><a id="configure_app"></a>Konfigurieren der Anwendungs Twilio verwendet.
Sie können Ihre Web-app, um die Bibliothek Twilio durch Aktualisieren der verwenden Konfigurieren Ihrer `Gemfile` diese Zeile hinzuzufügen:

    gem 'twilio-ruby'

Führen Sie in der Befehlszeile `bundle install`. Öffnen Sie jetzt `web.rb` und diese Zeile oben, einschließlich:

    require 'twilio-ruby'

Sie sind jetzt schon Twilio Helper Bibliothek für Ruby im Web app verwenden.

## <a name="a-idhowtomakecallahow-to-make-an-outgoing-call"></a><a id="howto_make_call"></a>So: tätigen eines Anrufs ausgehenden
Die nachstehende Abbildung zeigt, wie Sie einen ausgehenden Anruf zu tätigen. Grundlegende Konzepte enthalten, mit der Twilio Hilfebibliothek für die Ruby REST-API Anrufe tätigen und TwiML darstellen. Ersetzen Sie die Werte für die Telefonnummern **von** und **bis** , und stellen Sie sicher, dass Sie die Telefonnummer **aus** für Ihr Konto Twilio vor dem Ausführen des Codes überprüfen.

Fügen Sie diese Funktion zum `web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";
      
    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);
      
      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end
    
Wenn Sie öffnen oben `http://yourdomain.cloudapp.net/make_call` in einem Browser, die den Anruf an die Twilio-API, um den Anruf tätigen möchten auslösen wird. Die ersten beiden Parameter in `client.account.calls.create` verständlich sind: die Nummer der Anruf ist `from` und die Nummer der Anruf ist `to`. 

Der dritte Parameter (`url`) ist die URL, die Twilio anfordert, um Informationen zu erhalten, was zu tun ist, nachdem die Verbindung hergestellt wird. In diesem Fall wir ansetzen eine URL (`http://yourdomain.cloudapp.net`), gibt ein einfaches TwiML Dokument und verwendet die `<Say>` Verb einige Sprachausgabe und sagen "Hallo Affe", um den Anruf gegenüber der Person.

## <a name="a-idhowtorecievesmsahow-to-recieve-an-sms-message"></a><a id="howto_recieve_sms"></a>So: empfangen eine SMS-Nachricht
Im vorherigen Beispiel initiiert wir einen **ausgehenden** Anruf. Diese Uhrzeit, verwenden Sie die Telefonnummer, die Twilio bei der Anmeldung angegeben haben wir eine **eingehende** SMS-Nachricht zu verarbeiten.

Erste, melden Sie sich bei Ihrem [Twilio Dashboard][twilio_account]. Klicken Sie auf "Zahlen" im oberen Navigationsbereich, und klicken Sie dann auf die Twilio-Nummer, die Sie zur Verfügung gestellt wurden. Sie sehen zwei URLs, die Sie konfigurieren können. Die URL einer Voicemail Anforderung und eine SMS anfordern URL. Dies sind die URLs, die Twilio Anrufe bei jedem aus ein Anruf besteht oder einer SMS, um Ihre Nummer gesendet wird aus. Die URLs werden auch als "Web Haken" bezeichnet.

Wir möchten eingehende SMS-Nachrichten zu verarbeiten, aktualisieren die URL, also `http://yourdomain.cloudapp.net/sms_url`. Fahren Sie fort, und klicken Sie auf Save Changes am unteren Rand der Seite. Jetzt wieder einzuchecken `web.rb` Programm wir unsere Anwendung folgt verarbeitet:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Sicherzustellen Sie nach der Änderung Web app erneut zu starten. Nun nehmen auf Ihrem Smartphone und eine SMS an Ihre Nummer Twilio senden. Sie sollten umgehend SMS-Antwort erhalten, die besagt "Hallo, vielen Dank für das Pingsignal! Twilio und Azure Felsen! ".

## <a name="a-idadditionalservicesahow-to-use-additional-twilio-services"></a><a id="additional_services"></a>How to: Verwenden Sie zusätzliche Twilio Services
Zusätzlich zu den hier gezeigten Beispielen bietet Twilio webbasierten APIs, mit denen Sie weitere Twilio Funktionen aus Azure-Anwendung nutzen können. Einzelheiten hierzu finden Sie in der [Dokumentation zur Twilio-API] [twilio_api_documentation].

### <a name="a-idnextstepsanext-steps"></a><a id="NextSteps"></a>Nächste Schritte
Die Grundlagen des Diensts Twilio bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen:

* [Richtlinien für Twilio-Sicherheit] [twilio_security_guidelines]
* [Twilio HowTos und Beispielcode] [twilio_howtos]
* [Twilio Schnellstart-Lernprogramme][twilio_quickstarts] 
* [Klicken Sie auf GitHub Twilio] [twilio_on_github]
* [Wenden Sie sich an den Support Twilio] [twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/
