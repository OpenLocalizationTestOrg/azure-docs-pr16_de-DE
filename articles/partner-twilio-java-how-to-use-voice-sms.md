<properties 
    pageTitle="Verwendung von Twilio für VoIP und SMS (Java) | Microsoft Azure" 
    description="Informationen Sie zum Tätigen eines Telefonanrufs, und senden eine SMS-Nachricht mit dem Dienst Twilio API auf Azure. In Java geschriebenen Codebeispielen." 
    services="" 
    documentationCenter="java" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>Verwendung von Twilio für VoIP und SMS-Funktionen in Java

Mit diesem Leitfaden wird veranschaulicht, wie häufig vorkommende Aufgaben mit dem Dienst Twilio API auf Azure ausführen. Die Szenarios dieser gehören Durchführung eines Telefonanrufs, und senden die Nachricht eine kurze Textnachricht (SMS). Weitere Informationen zu Twilio und Voicemail und SMS in Ihrer Anwendung verwenden finden Sie unter Abschnitt [Weitere Schritte](#NextSteps) .

## <a name="a-idwhatisawhat-is-twilio"></a><a id="WhatIs"></a>Was ist Twilio?
Twilio ist eine Telefonie Webdienst-API, die Sie Ihre Kenntnisse und vorhandenen Web Sprachen Voicemail und SMS Applications erstellen kann. Twilio ist eine Drittanbieter-Dienst (kein Azure Feature und nicht mit einem Microsoft-Produkt).

**VoIP Twilio** ist die Ihrer Anwendung tätigen und entgegennehmen Telefonanrufe. **Twilio SMS** ermöglicht Ihrer Anwendung tätigen und Entgegennehmen von SMS-Nachrichten an. **Twilio Client** ist die Ihrer Anwendung VoIP-Kommunikation mit vorhandenen internetverbindungen, einschließlich der mobile Verbindungen zu ermöglichen.

## <a name="a-idpricingatwilio-pricing-and-special-offers"></a><a id="Pricing"></a>Twilio Preise und spezielle Angebote
Informationen zur Preisgestaltung Twilio steht zu einem [Preis Twilio] [twilio_pricing]. Azure Kunden erhalten eine [spezielle Angebot][special_offer]: einer kostenlosen Gutschrift 1000 Texte oder Minuten eingehende 1000. Zum Anmelden für dieses Angebot oder Weitere Informationen zu erhalten, besuchen Sie [http://ahoy.twilio.com/azure][special_offer].  

## <a name="a-idconceptsaconcepts"></a><a id="Concepts"></a>Konzepte
Die Twilio-API ist ein Rest API, Sprach- und SMS-Funktionalität für Applikationen bereitstellt. Client-Bibliotheken sind in mehreren Sprachen verfügbar. eine Liste finden Sie unter [Twilio API-Bibliotheken] [twilio_libraries].

Wichtige Aspekte der Twilio APIs sind Twilio Verben und Twilio Markup Language (TwiML).

### <a name="a-idverbsatwilio-verbs"></a><a id="Verbs"></a>Twilio Verben
Die API nutzt Twilio Verben. Angenommen, die ** &lt;sagen&gt; ** Verb weist Twilio, eine Nachricht in einem Anruf hörbar zu übermitteln. 

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

### <a name="a-idtwimlatwiml"></a><a id="TwiML"></a>TwiML
TwiML ist eine Reihe von XML-basierten Anweisungen basierend auf den Twilio Verben, die Twilio zum Verarbeiten eines Anrufs oder SMS zu informieren.

Beispielsweise würde die folgenden TwiML den Text **Hallo Welt** Symbolleisten konvertieren.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Wenn eine Anwendung die Twilio-API aufgerufen wird, ist einer der Parameter API die URL, die die TwiML Antwort zurückgegeben wird. Bereitgestellte Twilio URLs können Sie hinsichtlich der Entwicklung die von Ihrer Anwendung verwendeten TwiML Antworten bereitgestellt. Sie könnten auch eigene URLs zu erzeugen, die Antworten TwiML hosten, und eine weitere Möglichkeit besteht darin, das Objekt **TwiMLResponse** verwenden.

Weitere Informationen zu Twilio Verben, deren Attribute und TwiML finden Sie unter [TwiML] [twiml]. Weitere Informationen über die Twilio-API, finden Sie unter [Twilio API] [twilio_api].

## <a name="a-idcreateaccountacreate-a-twilio-account"></a><a id="CreateAccount"></a>Erstellen Sie ein Konto Twilio
Wenn Sie bereit sind, ein Konto Twilio erhalten, melden Sie sich an [Twilio versuchen Sie es] [try_twilio]. Sie können ein kostenloses Konto verwenden, und später upgrade Ihres Kontos.

Wenn Sie sich für ein Konto Twilio registrieren, erhalten Sie eine Konto-ID und eine Authentifizierungstoken. Beide werden erforderlich sein, um Twilio API anzurufen. Um unbefugten Zugriff auf Ihr Konto zu verhindern, schützen Sie Ihrer Authentifizierungstoken. Ihr Konto-ID und Authentifizierung Token auf der [Kontoseite Twilio]sichtbar sind [twilio_account], in die Felder Beschriftung **SID zu berücksichtigen** und **Autorisierende TOKEN**,.

## <a name="a-idcreateappacreate-a-java-application"></a><a id="create_app"></a>Erstellen einer Java-Anwendungs
1. Die JAR-Twilio zu erhalten und Ihre Java-Generator Pfad und Ihre WAR Bereitstellung Assembly hinzugefügt werden. Bei [https://github.com/twilio/twilio-java][twilio_java], können Sie GitHub Quellen herunterladen und Erstellen eigener JAR oder einem vordefinierten Glas (mit oder ohne Abhängigkeiten) herunterladen.
2. Stellen Sie sicher, Ihre JDKs **Cacerts** Keystore enthält das Zertifikat Equifax Secure Zertifizierungsstelle mit MD5 Fingerabdruck 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (die fortlaufende Zahl ist 35:DE:F4:CF und SHA1 Fingerabdruck D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Dies ist das Zertifikat der Zertifizierungsstelle (CA) für die [https://api.twilio.com] [ twilio_api_service] -Dienst, der bei der Verwendung von Twilio APIs aufgerufen wird. Informationen zum Einrichten Ihrer JDKs **Cacerts** Keystore das richtige CA Zertifikat enthält, finden Sie unter [Hinzufügen eines Zertifikats im Speicher Java Zertifizierungsstelle Zertifikate][add_ca_cert].

Ausführliche Informationen zur Verwendung der Twilio-Client-Bibliothek für Java finden Sie unter [So stellen Sie einen Anruf mithilfe von Twilio in einer Java-Anwendung auf Azure][howto_phonecall_java].

## <a name="a-idconfigureappaconfigure-your-application-to-use-twilio-libraries"></a><a id="configure_app"></a>Konfigurieren der Anwendungs Twilio Bibliotheken verwendet
Innerhalb des Codes können Sie die Anweisungen **Importieren** hinzufügen, am oberen Rand der Quelldateien für die Twilio-Paketen Klassen, die Sie in Ihrer Anwendung verwenden möchten. 

Für Java-Quelldateien:

    import com.twilio.*;
    import com.twilio.sdk.*;
    import com.twilio.sdk.resource.factory.*;
    import com.twilio.sdk.resource.instance.*;

Für Java Server Page (JSP) Quelldateien:

    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
Je nachdem, welche Twilio Pakete oder Klassen verwenden möchten können Ihre Aussagen **Importieren** abweichen.

## <a name="a-idhowtomakecallahow-to-make-an-outgoing-call"></a><a id="howto_make_call"></a>So: tätigen eines Anrufs ausgehenden
Die nachstehende Abbildung zeigt, wie eine mithilfe der Klasse **CallFactory** für ausgehende Anrufe. Dieser Code verwendet eine bereitgestellte Twilio Website auch zum Zurückgeben der Antwort Twilio Markup Language (TwiML). Ersetzen Sie die Werte für die Telefonnummern **von** und **bis** , und stellen Sie sicher, dass Sie die Telefonnummer **aus** für Ihr Konto Twilio vor dem Ausführen des Codes überprüfen.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the CallFactory.
    Account account = client.getAccount();

    // Use the Twilio-provided site for the TwiML response.
    String Url="http://twimlets.com/message";
    Url = Url + "?Message%5B0%5D=Hello%20World";

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN"); // Use your own value for the second parameter.
    params.put("To", "NNNNNNNNNN");   // Use your own value for the second parameter.
    params.put("Url", Url);

    // Create an instance of the CallFactory class.
    CallFactory callFactory = account.getCallFactory();

    // Make the call.
    Call call = callFactory.create(params);

Weitere Informationen, die für die **CallFactory.create** Methode übergebene Parameter, finden Sie unter [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Wie erwähnt, wird mit dieser Code einer Website bereitgestellten Twilio die TwiML Antwort zurückgegeben. Sie können stattdessen anhand Ihrer eigenen Website die Antwort TwiML bereitzustellen. Weitere Informationen finden Sie unter [So bieten TwiML Antworten in einer Java-Anwendung auf Azure](#howto_provide_twiml_responses).

## <a name="a-idhowtosendsmsahow-to-send-an-sms-message"></a><a id="howto_send_sms"></a>So: Senden einer SMS-Nachricht
Die nachstehende Abbildung zeigt, wie eine SMS-Nachricht mithilfe der Klasse **SmsFactory** senden. Die **von** Zahl, **4155992671**, erfolgt über Twilio für Testkonten zum Senden von SMS-Nachrichten. Die Anzahl **zu** muss für Ihr Konto Twilio vor dem Ausführen des Codes überprüft werden.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the SmsFactory.
    Account account = client.getAccount();

    // Send an SMS message.
    MessageFactory messageFactory = account.getMessageFactory();
    
    List<NameValuePair> params = new ArrayList<NameValuePair>();
    params.add(new BasicNameValuePair("To", "+14159352345")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("From", "+14158141829")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("Body", "Where's Wallace?"));
    
    Message sms = messageFactory.create(params);
        
Weitere Informationen, die für die **SmsFactory.create** Methode übergebene Parameter, finden Sie unter [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].

## <a name="a-idhowtoprovidetwimlresponsesahow-to-provide-twiml-responses-from-your-own-website"></a><a id="howto_provide_twiml_responses"></a>Wie: Bereitstellen TwiML Antworten aus Ihrer eigenen Website
Bei die Anwendung ein Anrufs an die Twilio-API eingestellt, sendet beispielsweise über die Methode **CallFactory.create** Twilio Anforderung einer URL an, die möglicherweise eine Antwort TwiML zurückzukehren. Im oben genannten Beispiel verwendet die Twilio bereitgestellte URL [http://twimlets.com/message][twimlet_message_url]. (Während der TwiML für die Verwendung von Web Services ausgelegt ist, können Sie die TwiML in Ihrem Browser anzeigen. Klicken Sie beispielsweise auf [http://twimlets.com/message] [ twimlet_message_url] in ein leeres finden Sie unter ** &lt;Antwort&gt; ** Element. ein weiteres Beispiel, klicken Sie auf [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] Anzeigen einer ** &lt;Antwort&gt; ** Element, enthält eine ** &lt;sagen&gt; ** Element.)

Statt sich auf die URL bereitgestellten Twilio zu verlassen, können Sie Ihrer eigenen Website URL erstellen, die HTTP-Antworten gibt. Sie können die Website in einer anderen Sprache erstellen, die HTTP-Antworten gibt; In diesem Thema wird davon ausgegangen, dass Sie die URL in eine JSP-Seite gehostet werden, werden.

Die folgende JSP-Seite angegeben, ergibt sich eine TwiML Antwort, die besagt **Hallo Welt** auf den Anruf.

    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello World</Say>
    </Response>

Die folgende JSP-Seite angegeben, ergibt sich eine TwiML Antwort, die besagt Textabschnitt, mehrere Pausen, sowie Informationen über die Twilio API-Version und den Namen der Azure-Rolle besagt.


    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello from Azure</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>

Der Parameter **ApiVersion** ist in Twilio VoIP-Anfragen (keine SMS-Anfragen) verfügbar. Um die verfügbaren Abfrageparameter für Twilio Voicemail und SMS-Anfragen angezeigt wird, finden Sie unter <https://www.twilio.com/docs/api/twiml/twilio_request> und <https://www.twilio.com/docs/api/twiml/sms/twilio_request>. Die **RoleName** Umgebungsvariable steht als Teil einer Azure-Bereitstellung. (Wenn Sie benutzerdefinierte Umgebungsvariablen hinzufügen, sodass diese von **System.getenv**übernommen werden konnte möchten, finden Sie unter Abschnitt Variablen Umgebung am [Verschiedene Rolle Konfiguration Einstellungen][misc_role_config_settings].)

Nachdem Sie Ihre JSP-Seite eingerichtet TwiML Antworten bereitgestellt haben, verwenden Sie die URL der Seite JSP als die URL, die an die Methode **CallFactory.create** übergeben. Beispielsweise wenn Sie eine Webanwendung namens verfügen MyTwiML befinden, die auf einer Azure gehosteten Dienst und der Namen der Seite JSP ist mytwiml.jsp, die URL an **CallFactory.create** wie im folgenden Beispiel übergeben werden kann:

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN");
    params.put("To", "NNNNNNNNNN");
    params.put("Url", "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    CallFactory callFactory = account.getCallFactory();
    Call call = callFactory.create(params);

Eine weitere Möglichkeit zum reagieren mit TwiML wird über die **TwiMLResponse** -Klasse, die in das Paket **com.twilio.sdk.verbs** verfügbar ist.

Weitere Informationen zur Verwendung von Twilio in Azure mit Java, finden Sie unter [So stellen Sie einen Anruf mithilfe von Twilio in einer Java-Anwendung auf Azure][howto_phonecall_java].

## <a name="a-idadditionalservicesahow-to-use-additional-twilio-services"></a><a id="AdditionalServices"></a>How to: Verwenden Sie zusätzliche Twilio Services
Zusätzlich zu den hier gezeigten Beispielen bietet Twilio webbasierten APIs, mit denen Sie weitere Twilio Funktionen aus Azure-Anwendung nutzen können. Einzelheiten hierzu finden Sie in der [Dokumentation zur Twilio-API] [twilio_api_documentation].

## <a name="a-idnextstepsanext-steps"></a><a id="NextSteps"></a>Nächste Schritte
Die Grundlagen des Diensts Twilio bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen:

* [Richtlinien für Twilio-Sicherheit] [twilio_security_guidelines]
* [Twilio HowTos und Beispielcode] [twilio_howtos]
* [Twilio Schnellstart-Lernprogramme][twilio_quickstarts] 
* [Klicken Sie auf GitHub Twilio] [twilio_on_github]
* [Wenden Sie sich an den Support Twilio] [twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
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
