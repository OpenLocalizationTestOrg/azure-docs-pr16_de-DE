<properties 
    pageTitle="So verwenden Sie den SendGrid e-Mail-Dienst (Node.js) | Microsoft Azure" 
    description="Erfahren Sie, wie Senden von e-Mails mit der e-Mail-Dienst SendGrid auf Azure. Fehlercode Beispiele mithilfe der Node.js-API geschrieben." 
    services="" 
    documentationCenter="nodejs" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="01/05/2016" 
    ms.author="erikre"/>
# <a name="how-to-send-email-using-sendgrid-from-nodejs"></a>Informationen zum Senden von e-Mail-Nachricht mithilfe von Node.js SendGrid

Mit diesem Leitfaden wird veranschaulicht, wie häufig vorkommende Aufgaben mit der e-Mail-Dienst SendGrid auf Azure ausführen. Die Beispiele werden mithilfe der Node.js-API geschrieben. Die Szenarios dieser einschließen **bauen-e-Mail**, **Senden von e-Mails**, **Anlagen hinzufügen**, **Verwenden von Filtern**und **Eigenschaften aktualisieren**. Weitere Informationen zu SendGrid und Senden von e-Mails finden Sie unter Abschnitt [Weitere Schritte](#next-steps) .

## <a name="what-is-the-sendgrid-email-service"></a>Was ist der SendGrid e-Mail-Dienst?

SendGrid ist ein [Cloud-basierten e-Mail-Dienst], zuverlässigen bereitstellt [Transaktionen e-Mail-Übermittlung], Skalierbarkeit und in Echtzeit Analytics sowie flexible APIs, die benutzerdefinierte Integration zu erleichtern. Allgemeine Szenarien, SendGrid Verwendung gehören:

-   Senden von übermittlungsbestätigungen automatisch an Kunden
-   Zum Senden von Kunden monatliche e-Handzettel und spezielle Angebote Listen verwalten der Verteilung
-   Sammeln in Echtzeit Kennzahlen zur beispielsweise blockierte e-Mail- und Kunden Reaktionszeiten
-   Erstellen von Berichten zum Identifizieren von trends
-   Weiterleiten von Abfragen
-   E-Mail-Benachrichtigungen aus Ihrer Anwendung

Weitere Informationen finden Sie unter [https://sendgrid.com](https://sendgrid.com).

## <a name="create-a-sendgrid-account"></a>Erstellen Sie ein Konto SendGrid

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-nodejs-module"></a>Bezug auf die SendGrid Node.js-Modul

Das SendGrid-Modul für Node.js kann über den Knoten Paket-Manager (Npm) installiert werden, mit den folgenden Befehl aus:

    npm install sendgrid

Nach der Installation können Sie das Modul in Ihrer Anwendung benötigen, mit dem folgenden Code:

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

Das Modul SendGrid exportiert die **SendGrid** und **e-Mail-** Funktionen.
**SendGrid** ist für das Senden von e-Mails über Web-API, während **E-Mail** mit eine e-Mail-Nachricht kapselt verantwortlich.

## <a name="how-to-create-an-email"></a>How to: erstellen eine E-Mail

Erstellen einer e-Mail-Nachricht mithilfe des Moduls SendGrid umfasst das zuerst Erstellen einer e-Mail-Nachricht mithilfe der Funktion e-Mail-, und klicken Sie dann mit der Funktion SendGrid senden. Im folgenden finden ein Beispiel für eine neue Nachricht mit der e-Mail-Funktion erstellen:

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

Sie können auch eine HTML-Nachricht für Clients angeben, die sie mithilfe der HTML-Eigenschaft unterstützen. Beispiel:

    html: This is a sample <b>HTML<b> email message.

Festlegen von Eigenschaften für den Text und die HTML-bietet sicheres Alternative zu Textinhalten für Clients, die HTML-Nachrichten nicht unterstützt.

Weitere Informationen über alle Eigenschaften, die von der e-Mail-Funktion unterstützt finden Sie unter [Sendgrid-Nodejs] [].

## <a name="how-to-send-an-email"></a>So: Senden einer E-Mail

Nach dem Erstellen einer e-Mail-Nachricht, die e-Mail-Funktion verwenden, können Sie es mithilfe der Web-API von SendGrid bereitgestellten senden. 

### <a name="web-api"></a>Web-API

    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [AZURE.NOTE] Während die oben genannten Beispiele in einer e-Mail-Objekt und Rückruf-Funktion Übergabe anzeigen, können Sie die senden-Funktion durch direkten Angabe von e-Mail-Eigenschaften auch direkt aufrufen. Beispiel:  
>
>`````
sendgrid.send({
    to: 'john@contoso.com',
    from: 'anna@contoso.com',
    subject: 'test mail',
    text: 'This is a sample email message.'
});
`````

## How to: Add an Attachment

Attachments can be added to a message by specifying the file name(s) and
path(s) in the **files** property. The following example demonstrates
sending an attachment:

    sendgrid.send({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.',
        files: [
            {
                filename:     '',           // required only if file.content is used.
                contentType:  '',           // optional
                cid:          '',           // optional, used to specify cid for inline content
                path:         '',           //
                url:          '',           // == One of these three options is required
                content:      ('' | Buffer) //
            }
        ],
    });

> [AZURE.NOTE] When using the **files** property, the file must be accessible
through [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile). If the file you wish to attach is hosted in Azure Storage, such as in a Blob container, you must first copy the file to local storage or to an Azure drive before it can be sent as an attachment using the **files** property.

## How to: Use Filters to Enable Footers and Tracking

SendGrid provides additional email functionality through the use of
filters. These are settings that can be added to an email message to
enable specific functionality such as enabling click tracking, Google
analytics, subscription tracking, and so on. For a full list of filters,
see [Filter Settings][].

Filters can be applied to a message by using the **filters** property.
Each filter is specified by a hash containing filter-specific settings.
The following examples demonstrate the footer and click tracking filters:

### Footer

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });
    
    email.setFilters({
        'footer': {
            'settings': {
                'enable': 1,
                'text/plain': 'This is a text footer.'
            }
        }
    });

    sendgrid.send(email);

### Click Tracking

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });
    
    email.setFilters({
        'clicktrack': {
            'settings': {
                'enable': 1
            }
        }
    });
    
    sendgrid.send(email);

## How to: Update Email Properties

Some email properties can be overwritten using **set*Property*** or
appended using **add*Property***. For example, you can add additional
recipients by using

    email.addTo('jeff@contoso.com');

or set a filter by using

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

For more information, see [sendgrid-nodejs][].

## How to: Use Additional SendGrid Services

SendGrid offers web-based APIs that you can use to leverage additional
SendGrid functionality from your Azure application. For full
details, see the [SendGrid API documentation][].

## Next Steps

Now that you've learned the basics of the SendGrid Email service, follow
these links to learn more.

-   SendGrid Node.js module repository: [sendgrid-nodejs][]
-   SendGrid API documentation:
    <https://sendgrid.com/docs>
-   SendGrid special offer for Azure customers:
    [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)
  [special offer]: https://sendgrid.com/windowsazure.html
  [sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
  [Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [SendGrid API documentation]: https://sendgrid.com/docs
  [cloud-based email service]: https://sendgrid.com/email-solutions
  [transactional email delivery]: https://sendgrid.com/transactional-email
