<properties 
    pageTitle="Verwendung von Twilio für VoIP und SMS (PHP) | Microsoft Azure" 
    description="Informationen Sie zum Tätigen eines Telefonanrufs, und senden eine SMS-Nachricht mit dem Dienst Twilio API auf Azure. Codebeispielen in PHP geschrieben." 
    documentationCenter="php" 
    services="" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>Wie Twilio für VoIP und SMS-Funktionen in PHP verwendet werden soll.
Mit diesem Leitfaden wird veranschaulicht, wie häufig vorkommende Aufgaben mit dem Dienst Twilio API auf Azure ausführen. Die Szenarios dieser gehören Durchführung eines Telefonanrufs, und senden die Nachricht eine kurze Textnachricht (SMS). Weitere Informationen zu Twilio und Voicemail und SMS in Ihrer Anwendung verwenden finden Sie im Abschnitt für die [Nächste Schritte](#NextSteps) .

## <a name="a-idwhatisawhat-is-twilio"></a><a id="WhatIs"></a>Was ist Twilio?
Twilio wird die Zukunft der Business-Kommunikationsgeräten, damit Entwicklern das Einbetten von Voicemail, VoIP und messaging in Applications einschalten. Diese Virtualisierung alle Infrastruktur in einer Umgebung cloudbasierten, globale, über die Twilio Kommunikation API Plattform Verfügbarmachen erforderlich. Applikationen sind einfach zu erstellen und skalierbare. Einfacheres Flexibilität mit Bezahlung-als-you wechseln Preise und nutzbringend Cloud Zuverlässigkeit.

**VoIP Twilio** ist die Ihrer Anwendung tätigen und entgegennehmen Telefonanrufe. **Twilio SMS** ermöglicht Ihrer Anwendung zu senden und Empfangen von Textnachrichten. **Twilio Client** können Sie von einem beliebigen Telefon, Ihren Tablet oder Browser VoIP-Anrufe tätigen und WebRTC unterstützt.

## <a name="a-idpricingatwilio-pricing-and-special-offers"></a><a id="Pricing"></a>Twilio Preise und spezielle Angebote

Azure Kunden erhalten eine [spezielle Angebot](http://www.twilio.com/azure): Grußformeln $10 des Kredits Twilio, wenn Sie Ihr Konto Twilio aktualisieren. Diese Kreditkarte Twilio können auf eine beliebige Twilio Verwendung (bis zu 1.000 SMS-Nachrichten senden oder Empfangen von bis zu 1000 Eingehende Voicemail Minuten, je nachdem, wo Ihres Telefons Zahl und Nachricht oder Anruf Ziels entspricht $10 haben) angewendet werden. Diese Kreditkarte Twilio einlösen und erste Schritte mit: [http://ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).

Twilio ist ein je nach Bedarf berechnet Dienst. Es gibt keine Gebühren ansetzen, und Sie können Ihr Konto jederzeit schließen. Weitere Informationen hierzu finden Sie am [Twilio Preise][twilio_pricing].

## <a name="a-idconceptsaconcepts"></a><a id="Concepts"></a>Konzepte
Die Twilio-API ist ein Rest API, Sprach- und SMS-Funktionalität für Applikationen bereitstellt. Client-Bibliotheken sind in mehreren Sprachen verfügbar. eine Liste finden Sie unter [Twilio API-Bibliotheken][twilio_libraries].

Wichtige Aspekte der Twilio APIs sind Twilio Verben und Twilio Markup Language (TwiML) an.

### <a name="a-idverbsatwilio-verbs"></a><a id="Verbs"></a>Twilio Verben
Die API nutzt Twilio Verben. Angenommen, die ** &lt;sagen&gt; ** Verb weist Twilio, eine Nachricht in einem Anruf hörbar zu übermitteln.

Im folgenden finden eine Liste der Twilio Verben. Informationen Sie zu anderen Verben und Funktionen, die über [Twilio Markup Language Dokumentation](http://www.twilio.com/docs/api/twiml).

* ** &lt;Einwahlnummern&gt;**: den Anrufer an ein anderes Telefon verbindet.
* ** &lt;Sammeln&gt;**: sammelt Ziffern auf der Zehnertastatur Telefon eingegeben.
* ** &lt;Auflegen&gt;**: ein Anrufs endet.
* ** &lt;Wiedergeben&gt;**: eine Audiodatei.
* ** &lt;Anhalten&gt;**: im Hintergrund für eine angegebene Anzahl von Sekunden wartet.
* ** &lt;Datensatz&gt;**: Einträge des Anrufers VoIP und gibt eine URL einer Datei, die die Aufzeichnung enthält.
* ** &lt;Umleiten&gt;**: Steuerung eines Anrufs oder SMS an die TwiML unter einem anderen URL.
* ** &lt;Ablehnen&gt;**: lehnt einen eingehenden Anruf Ihre Twilio Zahl ohne Abrechnung Sie ab
* ** &lt;Sagen&gt;**: konvertiert Text in Sprache, die in einem Anruf vorgenommen werden.
* ** &lt;Sms&gt;**: sendet eine SMS-Nachricht.

### <a name="a-idtwimlatwiml"></a><a id="TwiML"></a>TwiML
TwiML ist eine Reihe von XML-basierten Anweisungen basierend auf den Twilio Verben, die Twilio zum Verarbeiten eines Anrufs oder SMS zu informieren.

Beispielsweise würde die folgenden TwiML den Text **Hallo Welt** Symbolleisten konvertieren.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Wenn eine Anwendung die Twilio-API aufgerufen wird, ist einer der Parameter API die URL, die die TwiML Antwort zurückgegeben wird. Bereitgestellte Twilio URLs können Sie hinsichtlich der Entwicklung die von Ihrer Anwendung verwendeten TwiML Antworten bereitgestellt. Sie könnten auch eigene URLs zu erzeugen, die Antworten TwiML hosten, und eine weitere Möglichkeit besteht darin, das Objekt **TwiMLResponse** verwenden.

Weitere Informationen zu Twilio Verben, deren Attribute und TwiML finden Sie unter [TwiML][twiml]. Weitere Informationen über die Twilio-API, finden Sie unter [Twilio API][twilio_api].

## <a name="a-idcreateaccountacreate-a-twilio-account"></a><a id="CreateAccount"></a>Erstellen Sie ein Konto Twilio
Wenn Sie bereit sind, ein Konto Twilio erhalten, melden Sie sich an [Twilio versuchen Sie es][try_twilio]. Sie können ein kostenloses Konto verwenden, und später upgrade Ihres Kontos.

Wenn Sie für ein Twilio-Konto anmelden, erhalten Sie eine Konto-ID und eine Authentifizierungstoken. Beide werden erforderlich sein, um Twilio API anzurufen. Um unbefugten Zugriff auf Ihr Konto zu verhindern, schützen Sie Ihrer Authentifizierungstoken. Ihr Konto-ID und Authentifizierung Token auf der [Kontoseite Twilio]sichtbar sind[twilio_account], in die Felder Beschriftung **SID zu berücksichtigen** und **Autorisierende TOKEN**,.


## <a name="a-idcreateappacreate-a-php-application"></a><a id="create_app"></a>Erstellen Sie eine Anwendung von PHP
Eine PHP-Anwendung, verwendet den Twilio-Dienst in Azure läuft unterscheidet sich nicht als einer anderen Anwendung von PHP, die den Twilio-Dienst verwendet. Während Twilio Services basieren auf REST und von PHP auf verschiedene Weise aufgerufen werden können, wird in diesem Artikel erfahren Sie, wie Sie Twilio Services mit [Twilio Bibliothek für PHP aus GitHub]verwenden konzentrieren[twilio_php]. Weitere Informationen zur Verwendung der Bibliothek Twilio für PHP finden Sie unter [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Ausführliche Anweisungen für das Erstellen und Bereitstellen einer Twilio/PHP-Anwendung in Azure finden Sie unter [So stellen Sie einen Anruf mithilfe von Twilio in einer PHP-Anwendung auf Azure][howto_phonecall_php].

## <a name="a-idconfigureappaconfigure-your-application-to-use-twilio-libraries"></a><a id="configure_app"></a>Konfigurieren der Anwendungs Twilio Bibliotheken verwendet
Sie können eine Anwendung verwenden Sie die Bibliothek Twilio für PHP auf zwei Arten konfigurieren:

1. Laden Sie die Bibliothek Twilio PHP aus GitHub ([https://github.com/twilio/twilio-php][twilio_php]), und fügen Sie das **Dienste** Verzeichnis an Ihrer Anwendung.

    – ODER –

2. Installieren Sie die Bibliothek Twilio für PHP als Paket BIRNBÄUME. Sie können mit den folgenden Befehlen installiert werden:

        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

Nachdem Sie die Bibliothek Twilio für PHP installiert haben, können Sie eine **Require_once** Anweisung dann am oberen Rand von PHP-Dateien in der Bibliothek verwiesen hinzufügen:

        require_once 'Services/Twilio.php';

Weitere Informationen finden Sie unter [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a name="a-idhowtomakecallahow-to-make-an-outgoing-call"></a><a id="howto_make_call"></a>So: tätigen eines Anrufs ausgehenden
Die nachstehende Abbildung zeigt, wie eine mithilfe der Klasse **Services_Twilio** für ausgehende Anrufe. Dieser Code verwendet eine bereitgestellte Twilio Website auch zum Zurückgeben der Antwort Twilio Markup Language (TwiML). Ersetzen Sie die Werte für die Telefonnummern **von** und **bis** , und stellen Sie sicher, dass Sie die Telefonnummer **aus** für Ihr Konto Twilio vor dem Ausführen des Codes überprüfen.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";
    
    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

Wie erwähnt, wird mit dieser Code einer Website bereitgestellten Twilio die TwiML Antwort zurückgegeben. Sie können stattdessen anhand Ihrer eigenen Website die Antwort TwiML bereitzustellen. Weitere Informationen finden Sie unter [So bieten TwiML Antworten aus Ihrer eigenen Website](#howto_provide_twiml_responses).


- **Hinweis**: Sie SSL Zertifikat Überprüfungsfehler zur Problembehandlung finden Sie unter [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation] 


## <a name="a-idhowtosendsmsahow-to-send-an-sms-message"></a><a id="howto_send_sms"></a>So: Senden einer SMS-Nachricht
Die nachstehende Abbildung zeigt, wie eine SMS-Nachricht mithilfe der Klasse **Services_Twilio** senden. Die Anzahl **von** wird von Twilio bei Testkonten zum Senden von SMS-Nachrichten bereitgestellt. Die Anzahl **zu** muss für Ihr Konto Twilio vor dem Ausführen des Codes überprüft werden.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->$client->account->messages->sendMessage($from_number, $to_number, $message);
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a name="a-idhowtoprovidetwimlresponsesahow-to-provide-twiml-responses-from-your-own-website"></a><a id="howto_provide_twiml_responses"></a>Wie: Bereitstellen von TwiML Antworten aus Ihrer eigenen Website
Bei die Anwendung ein Anrufs an die Twilio-API eingestellt, sendet Twilio Anforderung an einer URL, die möglicherweise eine Antwort TwiML zurückzukehren. Im oben genannten Beispiel verwendet die Twilio bereitgestellte URL [http://twimlets.com/message][twimlet_message_url]. (Während der TwiML für die Verwendung von Twilio ausgelegt ist, können Sie die It in Ihrem Browser anzeigen. Klicken Sie beispielsweise auf [http://twimlets.com/message] [ twimlet_message_url] in ein leeres finden Sie unter `<Response>` Element. ein weiteres Beispiel, klicken Sie auf [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] Anzeigen einer `<Response>` Element, enthält eine `<Say>` Element.)

Auf angewiesen die URL Twilio vorausgesetzt, können Sie Ihrer eigenen Website erstellen, die HTTP-Antworten gibt. Sie können die Website in einer anderen Sprache erstellen, die XML-Antworten gibt; In diesem Thema wird davon ausgegangen, dass Sie PHP verwenden werden die TwiML erstellen.

Die folgende Seite von PHP angegeben, ergibt sich eine TwiML Antwort, die besagt **Hallo Welt** auf den Anruf.

    <?php    
        header("content-type: text/xml");    
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>    
        <Say>Hello world.</Say>
    </Response>

Wie Sie aus dem oben genannten Beispiel sehen können, wird die Antwort TwiML einfach ein XML-Dokument. Die Bibliothek Twilio für PHP enthält Klassen, die für Sie TwiML generiert werden. Im folgenden Beispiel wird die entsprechende Antwort erzeugt, wie oben gezeigt, verwendet jedoch die **Services\_Twilio\_Twiml** Klasse in der Bibliothek Twilio für PHP:

    require_once('Services/Twilio.php');
    
    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

Weitere Informationen zu TwiML finden Sie unter [https://www.twilio.com/docs/api/twiml][twiml_reference]. 

Nachdem Sie Ihre PHP-Seite eingerichtet TwiML Antworten bereitgestellt haben, verwenden Sie die URL der Seite PHP übergeben die URL der `Services_Twilio->account->calls->create` Methode. Beispielsweise wenn Sie eine Webanwendung mit dem Namen **MyTwiML** auf bereitgestellt haben ein Azure gehosteten Dienst, und der Name der Seite von PHP ist **mytwiml.php**, die URL zu übergeben werden kann **Services_Twilio-Konto > -> Anrufe -> Erstellen** wie im folgenden Beispiel gezeigt:

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number, 
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e) 
    {
        echo 'Error: ' . $e->getMessage();
    }

Weitere Informationen zur Verwendung von Twilio in Azure mithilfe von PHP, finden Sie unter [So stellen Sie einen Anruf mithilfe von Twilio in einer PHP-Anwendung auf Azure][howto_phonecall_php].

## <a name="a-idadditionalservicesahow-to-use-additional-twilio-services"></a><a id="AdditionalServices"></a>How to: Verwenden Sie zusätzliche Twilio Services
Zusätzlich zu den hier gezeigten Beispielen bietet Twilio webbasierten APIs, mit denen Sie weitere Twilio Funktionen aus Azure-Anwendung nutzen können. Einzelheiten hierzu finden Sie in der [Dokumentation zur Twilio-API][twilio_api_documentation].

## <a name="a-idnextstepsanext-steps"></a><a id="NextSteps"></a>Nächste Schritte
Die Grundlagen des Diensts Twilio bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen:

* [Richtlinien für Twilio-Sicherheit][twilio_security_guidelines]
* [Twilio HowTos und Beispielcode][twilio_howtos]
* [Twilio Schnellstart-Lernprogramme][twilio_quickstarts] 
* [Twilio auf GitHub][twilio_on_github]
* [Wenden Sie sich an den Support Twilio][twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_api_service]: https://api.twilio.com
[howto_phonecall_php]: partner-twilio-php-make-phone-call.md
[twilio_voice_request]: https://www.twilio.com/docs/api/twiml/twilio_request
[twilio_sms_request]: https://www.twilio.com/docs/api/twiml/sms/twilio_request
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
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
