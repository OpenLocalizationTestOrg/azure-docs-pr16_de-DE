<properties
    pageTitle="So stellen Sie einen Anruf aus Twilio (PHP) | Microsoft Azure"
    description="Informationen Sie zum Tätigen eines Telefonanrufs, und senden eine SMS-Nachricht mit dem Dienst Twilio API auf Azure. Beispiele dienen zur Anwendung von PHP."
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

# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>So stellen Sie einen Anruf mit Twilio in einer PHP-Anwendung auf Azure

Im folgenden Beispiel wird Sie, wie Sie Twilio verwenden können, um bei einem Anruf von PHP Webseite in Azure gehostet wird. Die resultierende Anwendung fordert den Benutzer für den Anruf Werte aus, wie im folgenden Screenshot dargestellt.

![Azure Anruf Formular mit Twilio und PHP][twilio_php]

Sie müssen gehen Sie wie folgt vor, um den Code in diesem Thema verwenden:

1. Erwerben eines Twilio-Konto und Authentifizierung token. Um mit Twilio anzufangen, auswerten Preise bei [http://www.twilio.com/pricing][twilio_pricing]. Sie können für ein Testkonto am [https://www.twilio.com/try-twilio]registrieren[try_twilio]. Informationen über die API von Twilio finden Sie unter [http://www.twilio.com/api][twilio_api].
2. Abrufen Sie der [Twilio Bibliothek für PHP](https://github.com/twilio/twilio-php) oder als Paket BIRNBÄUME zu installieren. Weitere Informationen finden Sie in der [Infodatei](https://github.com/twilio/twilio-php/blob/master/README.md).
3. Installieren Sie das Azure SDK für PHP. Eine Übersicht über die SDK und Anweisungen zum Installieren dieser, finden Sie unter [Einrichten von PHP Azure SDK][setup_php_sdk].

## <a name="create-a-web-form-for-making-a-call"></a>Erstellen eines Webformulars für das Tätigen eines Anrufs

Der folgende HTML-Code zeigt, wie auf einer Webseite (**callform.html**) erstellen, die einen Anruf zu tätigen Benutzerdaten abruft:

    <html>
    <head>
        <title>Automated call form</title>
    </head>
    <body>
    <h1>Automated Call Form</h1>
    <p>Fill in all fields and click <b>Make this call</b>.</p>
    <form action="makecall.php" method="post">
    <table>
        <tr>
            <td>To:</td>
            <td><input type="text" size=50 name="callTo" value=""></td>
        </tr>
        <tr>
            <td>From:</td>
            <td><input type="text" size=50 name="callFrom" value=""></td>
        </tr>
        <tr>
            <td>Call message:</td>
            <td><input type="text" size=100 name="callText" value="Hello. This is the call text. Good bye." /></td>
        </tr>
        <tr>
            <td colspan=2><input type="submit" value="Make this call"></td>
        </tr>
    </table>
    </form>
    <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Erstellen Sie den Code, um den Anruf zu tätigen
Der folgende Code wird gezeigt, wie auf einer Webseite (**makecall.php**) erstellen, die aufgerufen wird, wenn der Benutzer durch **callform.html**angezeigte Formular übermittelt wird. Der folgenden Code wird die Anruf Nachricht erstellt und den Anruf generiert. (Verwenden Sie Ihr Twilio-Konto und Authentifizierung anstelle der Platzhalterwerte **$sid** und **$token** in den folgenden Code zugewiesen token.)

    <html>
    <head><title>Making call...</title></head>
    <body>
    <p>Your call is being made.</p>

    <?php
    require_once 'Services/Twilio.php';

    $sid = "your_account_sid";
    $token = "your_authentication_token";

    $from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
    $to_number = $_POST['callTo'];
    $message = $_POST['callText'];

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    $call = $client->account->calls->create(
        $from_number,
        $to_number,
        'http://twimlets.com/message?Message='.urlencode($message)
    );

    echo "Call status: ".$call->status."<br />";
    echo "URI resource: ".$call->uri."<br />";
    ?>
    </body>
    </html>

Zusätzlich zu den Anruf zeigt **makecall.php** einige Anruf Metadaten (siehe in folgenden Screenshot Beispiel). Weitere Informationen zu Anruf Metadaten finden Sie unter [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Azure Anruf Antwort mit Twilio und PHP][twilio_php_response]

## <a name="run-the-application"></a>Führen Sie die Anwendung
Im nächsten Schritt wird zur Bereitstellung Ihrer Anwendungs auf Azure-Websites. Die Informationen für eine Website erstellen und Bereitstellen von Code mit Git, FTP oder WebMatrix (zwar nicht alle Informationen in jedem Artikel relevant sind) Sie in den folgenden Artikeln:

* [Erstellen einer Website von PHP-MySQL Azure und Bereitstellen von Git verwenden][website-git]
* [Azure PHP-MySQL-Website erstellen und Bereitstellen von mithilfe von FTP][website-ftp]

## <a name="next-steps"></a>Nächste Schritte
Zum Anzeigen von Grundfunktionen Twilio in PHP auf Azure verwenden, wurde dieser Code bereitgestellt. Vor der Bereitstellung in Azure produziert, möchten Sie möglicherweise weitere Fehlerbehandlung oder andere Features hinzufügen. Beispiel:

* Verwenden eines Webformulars, sondern können Sie Azure-Speicher Blobs oder SQL-Datenbank Speichern von Telefonnummern und rufen Text. Informationen zur Verwendung von Azure-Speicher Blobs in PHP, finden Sie unter [Verwenden von Azure Storage bei Programmen von PHP][howto_blob_storage_php]. Informationen zur Verwendung von SQL-Datenbank in PHP, finden Sie unter [Verwenden der SQL-Datenbank mit Applikationen von PHP][howto_sql_azure_php].
* Der Code **makecall.php** verwendet Twilio bereitgestellte URL ([http://twimlets.com/message][twimlet_message_url]) eine Antwort Twilio Markup Language (TwiML) bereitstellen, die Twilio Vorgehensweise mit den Anruf informiert. Beispielsweise kann der TwiML, der zurückgegeben wird enthalten eine `<Say>` Verb, das Text wird dem Empfänger fremdsprachigen ergibt. Anstatt die URL Twilio vorausgesetzt, könnten Sie Ihre eigenen ab, um zu Twilios Besprechungsanfrage Antworten erstellen; Weitere Informationen finden Sie unter [So verwenden Sie Twilio für VoIP und SMS-Funktionen in PHP][howto_twilio_voice_sms_php]. Weitere Informationen zu TwiML finden Sie unter [http://www.twilio.com/docs/api/twiml][twiml], sowie weitere Informationen zu `<Say>` und anderen Twilio Verben finden Sie unter [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Lesen die Twilio Sicherheitsrichtlinien bei [https://www.twilio.com/docs/security][twilio_docs_security].

Weitere Informationen zu Twilio, finden Sie unter [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Siehe auch
* [Wie Twilio für VoIP und SMS-Funktionen in PHP verwendet werden soll.](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[setup_php_sdk]: http://azurephp.interoperabilitybridges.com/articles/setup-the-windows-azure-sdk-for-php
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[website-git]: ./web-sites/web-sites-php-mysql-deploy-use-git.md
[website-ftp]: ./web-sites/web-sites-php-mysql-deploy-use-ftp.md
[twilio_php_github]: https://github.com/twilio/twilio-php
