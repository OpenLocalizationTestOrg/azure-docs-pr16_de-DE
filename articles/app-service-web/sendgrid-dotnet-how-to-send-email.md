<properties 
    pageTitle="So verwenden Sie den SendGrid e-Mail-Dienst (.NET) | Microsoft Azure" 
    description="Erfahren Sie, wie Senden von e-Mails mit der e-Mail-Dienst SendGrid auf Azure. Beispiele in C#-Code ein, und verwenden Sie die .NET-API." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="thinkingserious" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="01/14/2016" 
    ms.author="team-pi@sendgrid.com"/>





# <a name="how-to-send-email-using-sendgrid-with-azure"></a>Informationen zum Senden von e-Mail-Nachricht mithilfe von SendGrid mit Azure


## <a name="overview"></a>(Übersicht)

Mit diesem Leitfaden wird veranschaulicht, wie häufig vorkommende Aufgaben mit der e-Mail-Dienst SendGrid auf Azure ausführen. Die Beispiele in C geschrieben werden\#
und die .NET-API. Die Szenarios dieser gehören **bauen-e-Mail**, **Senden einer e-Mail**, **Anlagen hinzufügen**und **Verwenden von Filtern**. Weitere Informationen zu SendGrid und Senden von e-Mails finden Sie unter Abschnitt [Weitere Schritte][] .

## <a name="what-is-the-sendgrid-email-service"></a>Was ist der SendGrid e-Mail-Dienst?

SendGrid ist ein [Cloud-basierten e-Mail-Dienst] , der stellt zuverlässigen [Transaktionen e-Mail-Übermittlung], Skalierbarkeit und in Echtzeit Analytics sowie flexible APIs, die benutzerdefinierte Integration zu erleichtern. Allgemeine Szenarien, SendGrid Verwendung gehören:

-   Senden von übermittlungsbestätigungen automatisch an Kunden.
-   Verwalten der Verteilung von Listen zum Senden von Kunden monatliche e-Handzettel und spezielle Angebote.
-   Sammeln in Echtzeit Kennzahlen zur Aspekten wie blockierte e-Mail und Kunden Reaktionszeiten ein.
-   Erstellen von Berichten zum Identifizieren von Trends aus.
-   Weiterleitung Kunden Abfragen.
-   Verarbeitung eingehende e-Mails an.

Weitere Informationen finden Sie unter [https://sendgrid.com](https://sendgrid.com) oder unsere [C#-Bibliothek][Sendgrid-csharp]

## <a name="create-a-sendgrid-account"></a>Erstellen Sie ein Konto SendGrid

[AZURE.INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a>Bezug auf die SendGrid .NET Class-Bibliothek

Das [SendGrid NuGet-Paket](https://www.nuget.org/packages/Sendgrid) ist die einfachste Möglichkeit, die SendGrid-API abzurufenden und so konfigurieren Sie die Anwendung mit allen Abhängigkeiten an. NuGet ist eine Visual Studio-Erweiterung Lieferumfang von Microsoft Visual Studio 2015, die zum Installieren und Aktualisieren von Bibliotheken und Tools erleichtert. 

> [AZURE.NOTE] Um NuGet installieren, wenn Sie eine Version von Visual Studio vor Visual Studio 2015 ausgeführt werden, finden Sie auf [http://www.nuget.org](http://www.nuget.org), und klicken Sie auf die Schaltfläche **NuGet installieren** .

Gehen Sie wie folgt vor, um das Paket SendGrid NuGet in Ihrer Anwendung zu installieren:

1.  Erstellt ein neues Projekt.

    ![Erstellen eines neuen Projekts][create-new-project]

2.  Wählen Sie eine Vorlage aus.

    ![Wählen Sie eine Vorlage aus.][select-a-template]

3.  Klicken Sie im **Explorer Lösung**mit der rechten Maustaste **Verweise**und dann auf **NuGet-Pakete verwalten**.

4.  Suchen Sie nach **SendGrid** , und wählen Sie das **SendGrid** -Element in der Ergebnisliste.

    ![SendGrid NuGet-Paket][SendGrid-NuGet-package]

5.  Klicken Sie auf **Installieren** , um die Installation durchzuführen, und klicken Sie dann schließen Sie in diesem Dialogfeld zu.

SendGrid des .NET Class Library heißt **SendGridMail**. Dieser Abschnitt enthält die folgenden Namespaces:

-   **SendGridMail** für das Erstellen und Arbeiten mit e-Mail-Elemente.
-   **SendGridMail.Transport** zum Senden von e-Mail verwenden entweder das **SMTP-** Protokoll oder das Protokoll HTTP 1.1 mit **Web/REST**.

Fügen Sie die folgenden Code Namespace-Deklarationen am oberen Rand einer beliebigen C\# Datei in der e-Mail-Dienst SendGrid programmgesteuert Zugriff werden soll.
**System.Net** und **System.Net.Mail** sind .NET Framework-Namespaces enthalten sind, da sie Datentypen enthalten, die häufig mit den SendGrid-APIs soll verwendet werden.

    using System;
    using System.Net;
    using System.Net.Mail;
    using SendGrid;

## <a name="how-to-create-an-email"></a>So: Erstellen Sie eine e-Mail-Nachricht

Verwenden Sie das Objekt **SendGridMessage** , um eine e-Mail-Nachricht zu erstellen. Nachdem das Nachrichtenobjekt erstellt wurde, können Sie die Eigenschaften und Methoden, einschließlich e-Mail-Absenders, der e-Mail-Empfänger und den Betreff und Textkörper der e-Mail festlegen.

Im folgenden Beispiel wird veranschaulicht, wie ein Objekt vollständig eingetragenen-e-Mail zu erstellen:

    // Create the email object first, then add the properties.
    var myMessage = new SendGridMessage();

    // Add the message properties.
    myMessage.From = new MailAddress("john@example.com");

    // Add multiple addresses to the To field.
    List<String> recipients = new List<String>
    {
        @"Jeff Smith <jeff@example.com>",
        @"Anna Lidman <anna@example.com>",
        @"Peter Saddow <peter@example.com>"
    };

    myMessage.AddTo(recipients);

    myMessage.Subject = "Testing the SendGrid Library";

    //Add the HTML and Text bodies
    myMessage.Html = "<p>Hello World!</p>";
    myMessage.Text = "Hello World plain text!";

Weitere Informationen auf alle Eigenschaften und Methoden vom Typ **SendGrid** unterstützt finden Sie unter [Sendgrid-Csharp][] auf GitHub.

## <a name="how-to-send-an-email"></a>So: Senden einer e-Mail

Nach dem Erstellen einer e-Mail-Nachricht, können Sie es mithilfe der Web-API von SendGrid bereitgestellten senden. Alternativ können Sie [verwenden. Netz des integrierten Bibliothek](https://sendgrid.com/docs/Code_Examples/csharp.html).

Senden einer e-Mail erfordert, dass Sie Ihre SendGrid Anmeldeinformationen (Benutzername und Kennwort) oder SendGrid API-Schlüssel angeben. API-Schlüssel ist die bevorzugte Methode. Wenn Sie ausführliche Informationen zum Konfigurieren von Tasten API benötigen, besuchen Sie unseren [Dokumentation](https://sendgrid.com/docs/Classroom/Send/api_keys.html)

Sie möglicherweise über Ihre Azure-Portal diese Anmeldeinformationen speichern, indem Sie auf konfigurieren, und der Schlüssel/Wert-Paare unter "app-Einstellungen" hinzufügen.

 ![Azure app-Einstellungen][azure_app_settings]

 Dann können Sie wie folgt zugreifen: 
    
    var username = System.Environment.GetEnvironmentVariable("SENDGRID_USERNAME"); 
    var pswd = System.Environment.GetEnvironmentVariable("SENDGRID_PASSWORD");
    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");

Verwenden von Anmeldeinformationen:
    
    // Create network credentials to access your SendGrid account
    var username = "your_sendgrid_username";
    var pswd = "your_sendgrid_password";

    var credentials = new NetworkCredential(username, pswd);
    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

Verwenden von API-Schlüssel:

    var apiKey = "your_sendgrid_api_key";  
    // create a Web transport, using API Key
    var transportWeb = new Web(apiKey);


Den folgenden Beispielen wird die zum Senden einer Nachricht mithilfe der Web-API.

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Create credentials, specifying your user name and password.
    var credentials = new NetworkCredential("username", "password");

    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

    // Send the email, which returns an awaitable task.
    transportWeb.DeliverAsync(myMessage);

    // If developing a Console Application, use the following
    // transportWeb.DeliverAsync(mail).Wait();

## <a name="how-to-add-an-attachment"></a>So: Hinzufügen einer Anlage

Anlagen können zu einer Nachricht hinzugefügt werden, indem die Methode **AddAttachment** und den Namen und den Pfad der Datei, die Sie anfügen möchten.
Sie können mehrere Anlagen einschließen, indem Sie diese Methode aufrufen, nachdem Sie für jede Datei anfügen möchten. Im folgende Beispiel wird veranschaulicht, Hinzufügen einer Anlage zu einer Nachricht:

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    myMessage.AddAttachment(@"C:\file1.txt");
    
Sie können auch aus den Daten des **Streams**Anlagen hinzufügen. Es kann ausgeführt werden, indem Sie die gleichen Weg über **AddAttachment**, aber durch den Stream Daten übergeben, und der Dateiname soll wie in der Nachricht angezeigt. In diesem Fall müssen Sie die Bibliothek System.IO hinzufügen.

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    using (var attachmentFileStream = new FileStream(@"C:\file.txt", FileMode.Open))
    {
        myMessage.AddAttachment(attachmentFileStream, "My Cool File.txt");
    }


## <a name="how-to-use-apps-to-enable-footers-tracking-and-analytics"></a>So: Verwenden von apps Fußzeilen, nachverfolgen und Analytics aktivieren

SendGrid bietet zusätzliche e-Mail-Funktionen, durch die Verwendung von apps. Hierbei handelt es sich um Einstellungen, die eine e-Mail-Nachricht aktivieren bestimmten Funktionen wie klicken Sie auf Überwachung, Google Analytics, Überwachung, Abonnement hinzugefügt werden können und so weiter. Eine vollständige Liste der apps finden Sie unter [Einstellungen für die App][].

Apps können auf **SendGrid** e-Mail-Nachrichten mithilfe von als Teil der Klasse **SendGrid** implementiert Methoden angewendet werden.

In den folgenden Beispielen veranschaulichen die Fußzeile, und klicken Sie auf Überwachung Filter:

### <a name="footer"></a>Fußzeile

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Add a footer to the message.
    myMessage.EnableFooter("PLAIN TEXT FOOTER", "<p><em>HTML FOOTER</em></p>");

### <a name="click-tracking"></a>Klicken Sie auf Verlauf

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Html = "<p><a href=\"http://www.example.com\">Hello World Link!</a></p>";
    myMessage.Text = "Hello World!";
    
    // true indicates that links in plain text portions of the email 
    // should also be overwritten for link tracking purposes. 
    myMessage.EnableClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>So: Verwenden Sie zusätzliche SendGrid Dienste

SendGrid bietet Web-basierte APIs und Webhooks, mit denen Sie weitere SendGrid Funktionen aus Azure-Anwendung nutzen können. Vollständige Details finden Sie in der [Dokumentation zur SendGrid-API][].

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des SendGrid e-Mail-Diensts bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen.

*   SendGrid C\# Bibliothek Repo: [Sendgrid-Csharp][]
*   Dokumentation zur SendGrid-API: <https://sendgrid.com/docs>
*   SendGrid besonderes Angebot Azure Kunden: [https://sendgrid.com](https://sendgrid.com)

  [Nächste Schritte]: #next-steps
  [What is the SendGrid Email Service?]: #whatis
  [Create a SendGrid Account]: #createaccount
  [Reference the SendGrid .NET Class Library]: #reference
  [How to: Create an Email]: #createemail
  [How to: Send an Email]: #sendemail
  [How to: Add an Attachment]: #addattachment
  [How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
  [How to: Use Additional SendGrid Services]: #useservices
  
  [special offer]: https://www.sendgrid.com/windowsazure.html
  
  [create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/create_new_project.png
  [select-a-template]: ./media/sendgrid-dotnet-how-to-send-email/select_a_template.png
  [SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid_nuget.png
  [azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/app_settings.png
  [Sendgrid-csharp]: https://github.com/sendgrid/sendgrid-csharp
  [SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
  [App-Einstellungen]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [Dokumentation zur SendGrid-API]: https://sendgrid.com/docs
  
  [Cloud-basierten e-Mail-Dienst]: https://sendgrid.com/email-solutions
  [Transaktionen e-Mail-Übermittlung]: https://sendgrid.com/transactional-email
 
