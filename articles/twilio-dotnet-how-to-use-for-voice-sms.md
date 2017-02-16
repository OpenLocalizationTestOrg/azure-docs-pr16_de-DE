<properties
    pageTitle="Verwendung von Twilio für VoIP und SMS (.NET) | Microsoft Azure"
    description="Informationen Sie zum Tätigen eines Telefonanrufs, und senden eine SMS-Nachricht mit dem Dienst Twilio API auf Azure. Codebeispielen in .NET geschrieben wurde."
    services=""
    documentationCenter=".net"
    authors="devinrader"
    manager="twilio"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="04/24/2015"
    ms.author="devinrader"/>

# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-from-azure"></a>Verwendung von Twilio für VoIP und SMS-Funktionen aus Azure

Mit diesem Leitfaden wird veranschaulicht, wie häufig vorkommende Aufgaben mit dem Dienst Twilio API auf Azure ausführen. Die Szenarios dieser gehören Durchführung eines Telefonanrufs, und senden die Nachricht eine kurze Textnachricht (SMS). Weitere Informationen zu Twilio und Voicemail und SMS in Ihrer Anwendung verwenden finden Sie unter Abschnitt [Weitere Schritte](#NextSteps) .

## <a name="a-idwhatisawhat-is-twilio"></a><a id="WhatIs"></a>Was ist Twilio?
Twilio wird die Zukunft der Business-Kommunikationsgeräten, damit Entwicklern das Einbetten von Voicemail, VoIP und messaging in Applications einschalten. Diese Virtualisierung alle Infrastruktur in einer Umgebung cloudbasierten, globale, über die Twilio Kommunikation API Plattform Verfügbarmachen erforderlich. Applikationen sind einfach zu erstellen und skalierbare. Einfacheres Flexibilität mit Bezahlung-als-you wechseln Preise und nutzbringend Cloud Zuverlässigkeit.

**VoIP Twilio** ist die Ihrer Anwendung tätigen und entgegennehmen Telefonanrufe. **Twilio SMS** ermöglicht Ihrer Anwendung zu senden und Empfangen von SMS-Nachrichten an. **Twilio Client** können Sie von einem beliebigen Telefon, Ihren Tablet oder Browser VoIP-Anrufe tätigen und WebRTC unterstützt.

## <a name="a-idpricingatwilio-pricing-and-special-offers"></a><a id="Pricing"></a>Twilio Preise und spezielle Angebote
Azure Kunden erhalten eine [spezielle Angebot](http://www.twilio.com/azure): Grußformeln $10 des Kredits Twilio, wenn Sie Ihr Konto Twilio aktualisieren. Diese Kreditkarte Twilio können auf eine beliebige Twilio Verwendung (bis zu 1.000 SMS-Nachrichten senden oder Empfangen von bis zu 1000 Eingehende Voicemail Minuten, je nachdem, wo Ihres Telefons Zahl und Nachricht oder Anruf Ziels entspricht $10 haben) angewendet werden. Diese Kreditkarte Twilio einlösen und erste Schritte mit [ahoy.twilio.com/azure](http://ahoy.twilio.com/azure).

Twilio ist ein je nach Bedarf berechnet Dienst. Es gibt keine Gebühren ansetzen, und Sie können Ihr Konto jederzeit schließen. Weitere Informationen hierzu finden Sie am [Twilio Preise](http://www.twilio.com/voice/pricing).  

## <a name="a-idconceptsaconcepts"></a><a id="Concepts"></a>Konzepte
Die Twilio-API ist ein Rest API, Sprach- und SMS-Funktionalität für Applikationen bereitstellt. Client-Bibliotheken sind in mehreren Sprachen verfügbar. eine Liste finden Sie unter [Twilio API-Bibliotheken] [twilio_libraries].

Wichtige Aspekte der Twilio APIs sind Twilio Verben und Twilio Markup Language (TwiML) an.

### <a name="a-idverbsatwilio-verbs"></a><a id="Verbs"></a>Twilio Verben
Die API nutzt Twilio Verben. Angenommen, die ** &lt;sagen&gt; ** Verb weist Twilio, eine Nachricht in einem Anruf hörbar zu übermitteln.

Im folgenden finden eine Liste der Twilio Verben.  Informationen Sie zu anderen Verben und Funktionen, die über [Twilio Markup Language Dokumentation](http://www.twilio.com/docs/api/twiml).

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

Weitere Informationen zu Twilio Verben, deren Attribute und TwiML finden Sie unter [TwiML] [twiml]. Weitere Informationen über die Twilio-API, finden Sie unter [Twilio API] [twilio_api].

## <a name="a-idcreateaccountacreate-a-twilio-account"></a><a id="CreateAccount"></a>Erstellen Sie ein Konto Twilio
Wenn Sie bereit sind, ein Konto Twilio erhalten, melden Sie sich an [Twilio versuchen Sie es] [try_twilio]. Sie können ein kostenloses Konto verwenden, und später upgrade Ihres Kontos.

Wenn Sie für ein Twilio-Konto anmelden, erhalten Sie eine Konto-ID und eine Authentifizierungstoken. Beide werden erforderlich sein, um Twilio API anzurufen. Um unbefugten Zugriff auf Ihr Konto zu verhindern, schützen Sie Ihrer Authentifizierungstoken. Ihr Konto-ID und Authentifizierung Token auf der [Kontoseite Twilio]sichtbar sind [twilio_account], in die Felder Beschriftung **SID zu berücksichtigen** und **Autorisierende TOKEN**,.

## <a name="a-idcreateappacreate-an-azure-application"></a><a id="create_app"></a>Erstellen einer Azure-Anwendung
Eine Azure-Anwendung, die eine Anwendung aktiviert Twilio hostet unterscheidet sich nicht von einer anderen Azure-Anwendung. Fügen Sie die Bibliothek Twilio .NET und konfigurieren die Rolle aus, um die Bibliotheken Twilio .NET verwenden.
Informationen zum Erstellen einer anfänglichen Azure-Projekts finden Sie unter [Erstellen eines Azure-Projekts mit Visual Studio][vs_project].

## <a name="a-idconfigureappaconfigure-your-application-to-use-twilio-libraries"></a><a id="configure_app"></a>Konfigurieren Sie Ihrer Anwendung zur Verwendung von Bibliotheken Twilio
Twilio bietet eine Reihe von .NET Helper Bibliotheken, die verschiedene Aspekte des Twilio Bereitstellen von einfachen und einfache Methoden für die Interaktion mit der Twilio REST-API und der Twilio Client TwiML Antworten generieren umbrechen.

Twilio bietet fünf Bibliotheken für .NET Entwickler an:
Bibliothek|Beschreibung
---|---
Twilio.API|Die Core Twilio-Bibliothek, die die Twilio REST-API in einer angezeigten .NET Bibliothek umschließt. Diese Bibliothek steht für .NET, Silverlight und Windows Phone 7.
Twilio.TwiML|Ermöglicht das .NET geeignet TwiML Markup zu generieren.
Twilio.MVC|Für Entwickler mit ASP.NET-MVC enthält diese Bibliothek an einen TwilioController, TwiML ActionResult und Anforderung Überprüfung Attribut.
Twilio.WebMatrix|Für Entwickler, die mit Microsoft kostenloses WebMatrix Entwicklungstool enthält diese Bibliothek Razor Syntax Helfern für verschiedene Twilio Aktionen.
Twilio.Client.Capability|Enthält den Funktion token Generator für die Verwendung mit Twilio Client-JavaScript-SDK.

Beachten Sie, dass alle Bibliotheken .NET 3.5, Silverlight 4 oder Windows Phone 7 oder höher erforderlich.

In diesem Handbuch bereitgestellten Beispiele verwenden Twilio.API-Bibliothek.

Die Bibliotheken können [installiert ist, verwenden die NuGet-Paket-Manager-Erweiterung](http://www.twilio.com/docs/csharp/install) für Visual Studio 2010 und 2012 verfügbar sein.  Quellcode gehostet wird, klicken Sie auf [GitHub][twilio_github_repo], einschließlich ein Wikis, die vollständige Dokumentation zur Verwendung von Bibliotheken enthält.

Standardmäßig werden in Microsoft Visual Studio 2010 Version 1.2 NuGet installiert. Installieren die Bibliotheken Twilio ist Version 1.6 NuGet oder höher erforderlich. Informationen zum Installieren von oder Aktualisieren von NuGet finden Sie unter [http://nuget.org/][nuget].

> [AZURE.NOTE] Um die neueste Version der NuGet installiert haben, müssen Sie zuerst die geladene Version mithilfe der Visual Studio-Erweiterung-Manager deinstallieren. Dazu müssen Sie Visual Studio als Administrator ausführen. Andernfalls wird die Schaltfläche Deinstallieren deaktiviert.

### <a name="a-idusenugetato-add-the-twilio-libraries-to-your-visual-studio-project"></a><a id="use_nuget"></a>Die Twilio-Bibliotheken zum Projekt Visual Studio hinzufügen:

1.  Ihre Lösung in Visual Studio zu öffnen.
2.  Mit der rechten Maustaste **verweisen**.
3.  Klicken Sie auf **... NuGet-Pakete verwalten**
4.  Klicken Sie auf **Online**.
5.  Geben Sie im Suchfeld online *Twilio*ein.
6.  Klicken Sie auf das Paket Twilio auf **Installieren** .


## <a name="a-idhowtomakecallahow-to-make-an-outgoing-call"></a><a id="howto_make_call"></a>So: tätigen eines Anrufs ausgehenden
Die nachstehende Abbildung zeigt, wie eine mithilfe der Klasse **TwilioRestClient** für ausgehende Anrufe. Dieser Code verwendet eine bereitgestellte Twilio Website auch zum Zurückgeben der Antwort Twilio Markup Language (TwiML). Ersetzen Sie die Werte für die Telefonnummern **von** und **bis** , und stellen Sie sicher, dass Sie die Telefonnummer **aus** für Ihr Konto Twilio vor dem Ausführen des Codes überprüfen.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    string accountSID = "your_twilio_account";
    string authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Use the Twilio-provided site for the TwiML response.
    String Url="http://twimlets.com/message";
    Url = Url + "?Message%5B0%5D=Hello%20World";

    // Instantiate the call options that are passed
    // to the outbound call
    CallOptions options = new CallOptions();

    // Set the call From, To, and URL values to use for the call.
    // This sample uses the sandbox number provided by
    // Twilio to make the call.
    options.From = "+NNNNNNNNNN";
    options.To = "NNNNNNNNNN";
    options.Url = Url;

    // Make the call.
    var call = client.InitiateOutboundCall(options);

Weitere Informationen, die an den Client **übergebene Parameter. InitiateOutboundCall** Methode, finden Sie unter [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Wie erwähnt, wird mit dieser Code einer Website bereitgestellten Twilio die TwiML Antwort zurückgegeben. Sie könnten stattdessen Ihrer eigenen Website verwenden, um die Antwort TwiML bereitzustellen. Weitere Informationen finden Sie unter [wie: Bereitstellen TwiML Antworten in Ihrer eigenen Website](#howto_provide_twiml_responses).

## <a name="a-idhowtosendsmsahow-to-send-an-sms-message"></a><a id="howto_send_sms"></a>So: Senden einer SMS-Nachricht
Der folgende Screenshot zeigt, wie eine SMS-Nachricht mithilfe der Klasse **TwilioRestClient** gesendet. Die Anzahl **von** wird von Twilio bei Testkonten zum Senden von SMS-Nachrichten bereitgestellt. Bevor Sie den Code ausführen, muss die Anzahl **zu** für Ihr Konto Twilio überprüft werden.

        // Use your account SID and authentication token instead
        // of the placeholders shown here.
        string accountSID = "your_twilio_account";
        string authToken = "your_twilio_authentication_token";

        // Create an instance of the Twilio client.
        TwilioRestClient client;
        client = new TwilioRestClient(accountSID, authToken);

        // Send an SMS message.
        Message result = client.SendMessage(
            "+14155992671", "+12069419717", "This is my SMS message.");

        if (result.RestException != null)
        {
            //an exception occurred making the REST call
            string message = result.RestException.Message;
        }

## <a name="a-idhowtoprovidetwimlresponsesahow-to-provide-twiml-responses-from-your-own-website"></a><a id="howto_provide_twiml_responses"></a>Wie: Bereitstellen von TwiML Antworten aus Ihrer eigenen Website
Wenn eine Anwendung ein Anrufs an die Twilio-API – beispielsweise, über den Client **eingestellt. InitiateOutboundCall** Methode - Twilio sendet Anforderung an eine URL, die möglicherweise eine Antwort TwiML zurückzukehren. Das Beispiel in [wie: tätigen eines Anrufs ausgehenden](#howto_make_call) verwendet die Twilio bereitgestellte URL [http://twimlets.com/message] [ twimlet_message_url] die Antwort zurückgegeben.

> [AZURE.NOTE] Während der TwiML für die Verwendung von Web Services ausgelegt ist, können Sie die TwiML in Ihrem Browser anzeigen. Klicken Sie beispielsweise auf [http://twimlets.com/message] [ twimlet_message_url] in ein leeres finden Sie unter &lt;Antwort&gt; Element. ein weiteres Beispiel, klicken Sie auf [http://twimlets.com/message?Message%5B0%5D=Hello%20World](http://twimlets.com/message?Message%5B0%5D=Hello%20World) Anzeigen einer &lt;Antwort&gt; Element, enthält eine &lt;sagen&gt; Element.

Statt sich auf die URL bereitgestellten Twilio zu verlassen, können Sie Ihrer eigenen Website URL erstellen, die HTTP-Antworten gibt. Sie können die Website in einer anderen Sprache erstellen, die HTTP-Antworten zurückgibt. In diesem Thema wird davon ausgegangen, dass Sie die URL einer ASP.NET generische Ereignishandler gehostet werden werden.

Die folgenden ASP.NET Ereignishandler bietet eine TwiML Antwort, die besagt **Hallo Welt** auf den Anruf an.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;

    namespace WebRole1
    {
        /// <summary>
        /// Summary description for Handler1
        /// </summary>
        public class Handler1 : IHttpHandler
        {

            public void ProcessRequest(HttpContext context)
            {
                context.Response.Clear();
                context.Response.ContentType = "text/xml";
                context.Response.ContentEncoding = System.Text.Encoding.UTF8;
                string twiMLResponse = "<Response><Say>Hello World.</Say></Response>";
                context.Response.Write(twiMLResponse);
                context.Response.End();
            }

            public bool IsReusable
            {
                get
                {
                    return false;
                }
            }
        }
    }

Wie Sie aus dem oben genannten Beispiel sehen können, wird die Antwort TwiML einfach ein XML-Dokument. Die Bibliothek Twilio.TwiML enthält Klassen, die für Sie TwiML generiert werden. Im folgenden Beispiel wird die entsprechende Antwort erzeugt, wie oben gezeigt, verwendet jedoch die Klasse TwilioResponse.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;

    namespace WebRole1
    {
        /// <summary>
        /// Summary description for Handler1
        /// </summary>
        public class Handler1 : IHttpHandler
        {

            public void ProcessRequest(HttpContext context)
            {
                var twiml = new Twilio.TwiML.TwilioResponse();
                twiml.Say("Hello World.");

                context.Response.Clear();
                context.Response.ContentType = "text/xml";
                context.Response.Write(twiml.ToString());
                context.Response.End();
            }

            public bool IsReusable
            {
                get
                {
                    return false;
                }
            }
        }
    }

Weitere Informationen zu TwiML finden Sie unter [https://www.twilio.com/docs/api/twiml](https://www.twilio.com/docs/api/twiml).

Nachdem Sie eine Möglichkeit zum Bereitstellen von TwiML Antworten eingerichtet haben, können Sie diese URL in den **Client übergeben. InitiateOutboundCall** Methode. Beispielsweise kann, wenn Sie eine Webanwendung, die mit dem Namen MyTwiML an einen Azure-Cloud-Dienst bereitgestellt haben, und der Namen Ihrer ASP.NET Ereignishandler mytwiml.ashx ist, die URL an **Client übergeben werden. InitiateOutboundCall** wie im folgenden Beispiel gezeigt:

    // Place the call From, To, and URL values into a hash map.
    // This sample uses the sandbox number provided by Twilio to make the call.
    options.From = "NNNNNNNNNN";
    options.To = "NNNNNNNNNN";
    options.Url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.ashx";

    // Place the call.
    var call = client.InitiateOutboundCall(options);


Weitere Informationen zur Verwendung von Twilio auf Azure mit ASP.NET, finden Sie unter [So stellen Sie einen Anruf mit Twilio in einer Webrolle auf Azure][howto_phonecall_dotnet].

[AZURE.INCLUDE [twilio-additional-services-and-next-steps](../includes/twilio-additional-services-and-next-steps.md)]





[howto_phonecall_dotnet]: partner-twilio-cloud-services-dotnet-phone-call-web-role.md



[twimlet_message_url]: http://twimlets.com/message

[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls

[vs_project]:http://msdn.microsoft.com/library/windowsazure/ee405487.aspx
[nuget]:http://nuget.org/
[twilio_github_repo]:https://github.com/twilio/twilio-csharp



[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
