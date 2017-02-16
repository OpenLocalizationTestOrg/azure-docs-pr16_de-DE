<properties 
    pageTitle="So verwenden Sie den SendGrid e-Mail-Dienst (PHP) | Microsoft Azure" 
    description="Erfahren Sie, wie Senden von e-Mails mit der e-Mail-Dienst SendGrid auf Azure. Codebeispielen in PHP geschrieben." 
    documentationCenter="php" 
    services="" 
    manager="sendgrid" 
    editor="mollybos" 
    authors="thinkingserious"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com"/>
# <a name="how-to-use-the-sendgrid-email-service-from-php"></a>So verwenden Sie den SendGrid e-Mail-Dienst von PHP

Mit diesem Leitfaden wird veranschaulicht, wie häufig vorkommende Aufgaben mit der e-Mail-Dienst SendGrid auf Azure ausführen. Die Beispiele sind in PHP geschrieben.
Die Szenarios dieser gehören **bauen-e-Mail**, **Senden von e-Mails**und **Anlagen hinzufügen**. Weitere Informationen zu SendGrid und Senden von e-Mails finden Sie unter Abschnitt [Weitere Schritte](#next-steps) .

## <a name="what-is-the-sendgrid-email-service"></a>Was ist der SendGrid e-Mail-Dienst?

SendGrid ist ein [Cloud-basierten e-Mail-Dienst] , der stellt zuverlässigen [Transaktionen e-Mail-Übermittlung], Skalierbarkeit und in Echtzeit Analytics sowie flexible APIs, die benutzerdefinierte Integration zu erleichtern. Allgemeine Szenarien, SendGrid Verwendung gehören:

-   Senden von übermittlungsbestätigungen automatisch an Kunden
-   Zum Senden von Kunden monatliche e-Handzettel und spezielle Angebote Listen verwalten der Verteilung
-   Sammeln in Echtzeit Kennzahlen zur beispielsweise blockierte e-Mail- und Kunden Reaktionszeiten
-   Erstellen von Berichten zum Identifizieren von trends
-   Weiterleiten von Abfragen
- E-Mail-Benachrichtigungen aus Ihrer Anwendung

Weitere Informationen finden Sie unter [https://sendgrid.com][].

## <a name="create-a-sendgrid-account"></a>Erstellen Sie ein Konto SendGrid

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>Verwenden von SendGrid aus Ihrer Anwendung von PHP

Mithilfe von SendGrid in einer Anwendung von PHP Azure erfordert keine speziellen Konfiguration oder codieren. Da SendGrid ein Dienst ist, kann auf genau die gleiche Weise aus einer Cloudanwendung zugegriffen werden wie möglich aus einer lokalen Anwendung.

## <a name="how-to-send-an-email"></a>So: Senden einer E-Mail

Sie können mit SMTP oder die Web API von SendGrid e-Mail senden.

### <a name="smtp-api"></a>SMTP-API

Verwenden Sie zum Senden der e-Mail-Nachricht mithilfe der SendGrid SMTP-API *Swift Mailer*, einer Komponente-basierten Bibliothek zum Senden von e-Mails von PHP Applications aus. Sie können die Bibliothek *Swift Mailer* aus [http://swiftmailer.org/download][] v5.3.0 (verwenden Sie [Composer] Swift Mailer installieren) herunterladen. Senden einer e-Mail mit der Bibliothek umfasst das Erstellen von Instanzen von der <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_Mailer</span>, und <span class="auto-style2">Swift\_Nachricht</span> Klassen, die entsprechenden Eigenschaften und rufen die <span class="auto-style2">Swift\_Mailer::send</span> Methode.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */ 
     $text = "Hi!\nHow are you?\n";
     $html = "<html>
           <head></head>
           <body>
               <p>Hi!<br>
                   How are you?<br>
               </p>
           </body>
           </html>";
     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     // Email recipients
     $to = array(
           'john@contoso.com'=>'Destination 1 Name',
           'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
         // This will let us know how many users received this message
         echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
         echo "Something went wrong - ";
         print_r($failures);
     }

### <a name="web-api"></a>Web-API

Verwenden Sie zum Senden der e-Mail-Nachricht mithilfe der SendGrid Web-API von PHPs [Aufrollen (Funktion)][] .

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD'; 

     $params = array(
          'api_user' => $user,
          'api_key' => $pass,
          'to' => 'john@contoso.com',
          'subject' => 'testing from curl',
          'html' => 'testing body',
          'text' => 'testing body',
          'from' => 'anna@contoso.com',
       );
       
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

SendGrid des Web-API ist ein REST-API sehr ähnlich, obwohl er nicht wirklich eine REST-API ist, da die meisten Anrufe beide abrufen und Beitrag Verben synonym verwendet werden können.

## <a name="how-to-add-an-attachment"></a>How to: Hinzufügen einer Anlage

### <a name="smtp-api"></a>SMTP-API

Senden einer Anlage mithilfe der SMTP-API umfasst eine weitere Zeile des Codes zum Beispielskript zum Senden einer e-Mails mit Swift Mailer.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
      $html = "<html>
          <head></head>
          <body>
             <p>Hi!<br>
                How are you?<br>
             </p>
          </body>
          </html>";

     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     
     // Email recipients
     $to = array(
          'john@contoso.com'=>'Destination 1 Name',
          'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';
     
     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);
     
     // Create a message (subject)
     $message = new Swift_Message($subject);
     
     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
          // This will let us know how many users received this message
          echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
          echo "Something went wrong - ";
          print_r($failures);
     }

Die zusätzliche Zeile des Codes sieht wie folgt aus:

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

Diese Zeile des Codes Ruft die anfügen-Methode für die <span class="auto-style2">Swift\_Nachricht</span> Objekt und statische Methode <span class="auto-style2">FromPath</span> verwendet die <span class="auto-style2">Swift\_Anlage</span> Klasse zum Abrufen und Anfügen einer Datei an eine Nachricht.

### <a name="web-api"></a>Web-API

Senden einer Anlage mithilfe der Web-API ähnelt dem Senden einer e-Mails mit der Web-API. Beachten Sie jedoch, dass im folgenden Beispiel wird die Matrix Parameter dieses Element enthalten muss:

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

Beispiel:

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';
     
     $fileName = 'myfile';
     $filePath = dirname(__FILE__);

     $params = array(
         'api_user' => $user,
         'api_key' => $pass,
         'to' =>'john@contoso.com',
         'subject' => 'test of file sends',
         'html' => '<p> the HTML </p>',
         'text' => 'the plain text',
         'from' => 'anna@contoso.com',
         'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
     );
     
     print_r($params);
     
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>How to: verwenden Filter, um Fußzeilen, nachverfolgen und Analytics aktivieren

SendGrid bietet zusätzliche e-Mail-Funktionen, durch die Verwendung von 'Filter'. Hierbei handelt es sich um Einstellungen, die eine e-Mail-Nachricht aktivieren bestimmten Funktionen wie aktivieren, klicken Sie auf Überwachung, Google Analytics, Abonnement nachverfolgen, usw. hinzugefügt werden können.

Filter können zu einer Nachricht mithilfe der Filters-Eigenschaft angewendet werden. Die einzelnen Filter mit einer Raute, enthält spezielle Filter-Einstellungen angegeben. Im folgenden Beispiel wird den Fußzeile Filter ermöglicht und gibt eine Textnachricht an, die an das Ende der e-Mail-Nachricht angefügt wird.
In diesem Beispiel verwenden wir [Sendgrid Php Bibliothek].
Verwenden Sie [Composer] , um die Bibliothek zu installieren:
    
    php composer.phar require sendgrid/sendgrid 2.1.1

Beispiel:    

    <?php
     /*
      * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
      */
     include "vendor/autoload.php";

     $email = new SendGrid\Email();
     // The list of addresses this message will be sent to
     // [This list is used for sending multiple emails using just ONE request to SendGrid]
     $toList = array('john@contoso.com', 'anna@contoso.com');

     // Specify the names of the recipients
     $nameList = array('Name 1', 'Name 2');

     // Used as an example of variable substitution
     $timeList = array('4 PM', '5 PM');

     // Set all of the above variables
     $email->setTos($toList);
     $email->addSubstitution('-name-', $nameList);
     $email->addSubstitution('-time-', $timeList);

     // Specify that this is an initial contact message
     $email->addCategory("initial");

     // You can optionally setup individual filters here, in this example, we have 
     // enabled the footer filter
     $email->addFilter('footer', 'enable', 1);
     $email->addFilter('footer', "text/plain", "Thank you for your business");
     $email->addFilter('footer', "text/html", "Thank you for your business");

     // The subject of your email
     $subject = 'Example SendGrid Email';

     // Where is this message coming from. For example, this message can be from 
     // support@yourcompany.com, info@yourcompany.com
     $from = 'someone@example.com';

     // If you do not specify a sender list above, you can specifiy the user here. If 
     // a sender list IS specified above, this email address becomes irrelevant.
     $to = 'john@contoso.com';

     # Create the body of the message (a plain-text and an HTML version). 
     # text is your plain-text email 
     # html is your html version of the email
     # if the receiver is able to view html emails then only the html
     # email will be displayed

     /*
      * Note the variable substitution here =)
      */
     $text = "
     Hello -name-,
     Thank you for your interest in our products. We have set up an appointment to call you at -time- EST to discuss your needs in more detail.
     Regards,
     Fred";

     $html = "
     <html> 
     <head></head>
     <body>
     <p>Hello -name-,<br>
     Thank you for your interest in our products. We have set up an appointment
     to call you at -time- EST to discuss your needs in more detail.

     Regards,

     Fred<br>
     </p>
     </body>
     </html>";

     // set subject
     $email->setSubject($subject);

     // attach the body of the email
     $email->setFrom($from);
     $email->setHtml($html);
     $email->addTo($to);
     $email->setText($text);

     // Your SendGrid account credentials
     $username = 'sendgridusername@yourdomain.com';
     $password = 'example';

     // Create SendGrid object
     $sendgrid = new SendGrid($username, $password);

     // send message
     $response = $sendgrid->send($email);

     print_r($response);

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des SendGrid e-Mail-Diensts bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen.

-   SendGrid Dokumentation: <https://sendgrid.com/docs>
-   SendGrid von PHP Bibliothek: <https://github.com/sendgrid/sendgrid-php>
-   SendGrid besonderes Angebot Azure Kunden: <https://sendgrid.com/windowsazure.html>

Weitere Informationen finden Sie auch im [Developer Center von PHP](/develop/php/).


  [https://sendgrid.com]: https://sendgrid.com
  [https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
  [special offer]: https://www.sendgrid.com/windowsazure.html
  [Packaging and Deploying PHP Applications for Azure]: http://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
  [http://swiftmailer.org/Download]: http://swiftmailer.org/download
  [Aufrollen (Funktion)]: http://php.net/curl
  [Cloud-basierten e-Mail-Dienst]: https://sendgrid.com/email-solutions
  [Transaktionen e-Mail-Übermittlung]: https://sendgrid.com/transactional-email
  [Sendgrid Php Bibliothek]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
  [Composer]: https://getcomposer.org/download/
