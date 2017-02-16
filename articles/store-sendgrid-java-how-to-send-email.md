<properties 
    pageTitle="So verwenden Sie den SendGrid-e-Mail-Dienst (Java) | Microsoft Azure" 
    description="Erfahren Sie, wie Senden von e-Mails mit der e-Mail-Dienst SendGrid auf Azure. In Java geschriebenen Codebeispielen." 
    services="" 
    documentationCenter="java" 
    authors="thinkingserious" 
    manager="sendgrid" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork"/>
# <a name="how-to-send-email-using-sendgrid-from-java"></a>Informationen zum Senden von e-Mail-Nachricht mithilfe von Java SendGrid

Mit diesem Leitfaden wird veranschaulicht, wie häufig vorkommende Aufgaben mit der e-Mail-Dienst SendGrid auf Azure ausführen. Die Beispiele sind in Java geschrieben. Die Szenarios dieser einschließen **bauen-e-Mail**, **Senden von e-Mails**, **Anlagen hinzufügen**, **Verwenden von Filtern**und **Eigenschaften aktualisieren**. Weitere Informationen zu SendGrid und e-Mail senden finden Sie unter Abschnitt [Weitere Schritte](#next-steps) .

## <a name="what-is-the-sendgrid-email-service"></a>Was ist der SendGrid e-Mail-Dienst?

SendGrid ist ein [Cloud-basierten e-Mail-Dienst] , der stellt zuverlässigen [Transaktionen e-Mail-Übermittlung], Skalierbarkeit und in Echtzeit Analytics sowie flexible APIs, die benutzerdefinierte Integration zu erleichtern. Häufige SendGrid Verwendungsszenarien enthalten:

-   Automatisches Senden von übermittlungsbestätigungen für Kunden
-   Zum Senden von Kunden monatliche e-Handzettel und spezielle Angebote Listen verwalten der Verteilung
-   Sammeln in Echtzeit Kennzahlen zur beispielsweise blockierte e-Mail- und Kunden Reaktionszeiten
-   Erstellen von Berichten zum Identifizieren von trends
-   Weiterleiten von Abfragen
- E-Mail-Benachrichtigungen aus Ihrer Anwendung

Weitere Informationen finden Sie unter <http://sendgrid.com>.

## <a name="create-a-sendgrid-account"></a>Erstellen Sie ein Konto SendGrid

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="how-to-use-the-javaxmail-libraries"></a>So: Verwenden Sie die Bibliotheken zu "javax.Mail"

Rufen Sie die Bibliotheken zu "javax.Mail" beispielsweise aus <http://www.oracle.com/technetwork/java/javamail> und in Ihren Code importieren. Auf hoher umfasst die für die Verwendung der Bibliothek zu "javax.Mail" zum Senden von e-Mail unter Verwendung von SMTP auf eine der folgenden Aktionen ausführen:

1.  Geben Sie die Werte von SMTP, einschließlich des SMTP-Servers, also für SendGrid smtp.sendgrid.net an.
    
```
        import java.util.Properties;
        import javax.activation.*;
        import javax.mail.*;
        import javax.mail.internet.*;

        public class MyEmailer {
           private static final String SMTP_HOST_NAME = "smtp.sendgrid.net";
           private static final String SMTP_AUTH_USER = "your_sendgrid_username";
           private static final String SMTP_AUTH_PWD = "your_sendgrid_password";
        
           public static void main(String[] args) throws Exception{
              new MyEmailer().SendMail();
           }
        
           public void SendMail() throws Exception
           {
              Properties properties = new Properties();
              properties.put("mail.transport.protocol", "smtp");
              properties.put("mail.smtp.host", SMTP_HOST_NAME);
              properties.put("mail.smtp.port", 587);
              properties.put("mail.smtp.auth", "true");
              // …
```

2.  Erweitern Sie die Klasse *"javax.Mail.Authenticator"* , und in der Implementierung der Methode *GetPasswordAuthentication* zurückzukehren Sie, Ihren SendGrid-Benutzernamen und Ihr Kennwort ein.  

        private class SMTPAuthenticator extends javax.mail.Authenticator {
        public PasswordAuthentication getPasswordAuthentication() {
           String username = SMTP_AUTH_USER;
           String password = SMTP_AUTH_PWD;
           return new PasswordAuthentication(username, password);
        }

3.  Erstellen einer authentifizierten-e-Mail-Sitzung über ein Objekt *"javax.Mail.Session"* .  

        Authenticator auth = new SMTPAuthenticator();
        Session mailSession = Session.getDefaultInstance(properties, auth);

4.  Erstellen Sie Ihre Nachricht ein, und weisen Sie **an**, **von**, **Betreff** und Inhalt Werte. Dies wird angezeigt, der [How To: erstellen eine E-Mail](#bkmk_HowToCreateEmail) Abschnitt.
5.  Senden Sie die Nachricht über ein *javax.mail.Transport* -Objekt. Dies wird angezeigt, der [How To: Senden einer E-Mail] [so: Senden einer E-Mail] Abschnitt.

## <a name="how-to-create-an-email"></a>So: Erstellen Sie eine e-Mail-Nachricht

Die nachstehende Abbildung zeigt, wie Sie Werte für eine e-Mail-Nachricht angeben.

    MimeMessage message = new MimeMessage(mailSession);
    Multipart multipart = new MimeMultipart("alternative");
    BodyPart part1 = new MimeBodyPart();
    part1.setText("Hello, Your Contoso order has shipped. Thank you, John");
    BodyPart part2 = new MimeBodyPart();
    part2.setContent(
        "<p>Hello,</p>
        <p>Your Contoso order has <b>shipped</b>.</p>
        <p>Thank you,<br>John</br></p>",
        "text/html");
    multipart.addBodyPart(part1);
    multipart.addBodyPart(part2);
    message.setFrom(new InternetAddress("john@contoso.com"));
    message.addRecipient(Message.RecipientType.TO,
       new InternetAddress("someone@example.com"));
    message.setSubject("Your recent order");
    message.setContent(multipart);

## <a name="how-to-send-an-email"></a>So: Senden einer e-Mail

Die nachstehende Abbildung zeigt, wie Sie eine e-Mail zu senden.

    Transport transport = mailSession.getTransport();
    // Connect the transport object.
    transport.connect();
    // Send the message.
    transport.sendMessage(message, message.getAllRecipients());
    // Close the connection.
    transport.close();

## <a name="how-to-add-an-attachment"></a>So: Hinzufügen einer Anlage

Mit dem folgende Code wird gezeigt, wie Sie eine Anlage hinzufügen möchten.

    // Local file name and path.
    String attachmentName = "myfile.zip";
    String attachmentPath = "c:\\myfiles\\"; 
    MimeBodyPart attachmentPart = new MimeBodyPart();
    // Specify the local file to attach.
    DataSource source = new FileDataSource(attachmentPath + attachmentName);
    attachmentPart.setDataHandler(new DataHandler(source));
    // This example uses the local file name as the attachment name.
    // They could be different if you prefer.
    attachmentPart.setFileName(attachmentName);
    multipart.addBodyPart(attachmentPart);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>So: Fußzeilen, nachverfolgen und Analytics aktivieren mithilfe von Filtern

SendGrid bietet zusätzliche e-Mail-Funktionen, durch die Verwendung von *Filtern*. Hierbei handelt es sich um Einstellungen, die eine e-Mail-Nachricht aktivieren bestimmten Funktionen wie aktivieren, klicken Sie auf Überwachung, Google Analytics, Abonnement nachverfolgen, usw. hinzugefügt werden können. Eine vollständige Liste der Filter finden Sie unter [Filter Settings][].

-   Die nachstehende Abbildung zeigt, wie ein Filters Fußzeile einfügen, das HTML-Text am unteren Rand der gesendeten e-Mail angezeigte ergibt.

        message.addHeader("X-SMTPAPI", 
            "{\"filters\": 
            {\"footer\": 
            {\"settings\": 
            {\"enable\":1,\"text/html\": 
            \"<html><b>Thank you</b> for your business.</html>\"}}}}");

-   Ein weiteres Beispiel für einen Filter ist Klicken Sie auf Verlauf. Angenommen, dass Ihre e-Mail-Text enthält einen Link, z. B. Folgendes ein, und klicken Sie auf verfolgen möchten:

        messagePart.setContent(
            "Hello,
            <p>This is the body of the message. Visit 
            <a href='http://www.contoso.com'>http://www.contoso.com</a>.</p>
            Thank you.", 
            "text/html");

-   Wenn die klicken Sie auf Überwachung aktivieren möchten, verwenden Sie den folgenden Code ein:

        message.addHeader("X-SMTPAPI", 
            "{\"filters\": 
            {\"clicktrack\": 
            {\"settings\": 
            {\"enable\":1}}}}");

## <a name="how-to-update-email-properties"></a>So: Update-e-Mail-Eigenschaften

Einige Eigenschaften können mit *Eigenschaft *festlegen** ** oder angefügt mit **überschrieben werden e-Mails hinzufügen*Eigenschaft***.

Um **ReplyTo** Adressen angeben möchten, verwenden Sie beispielsweise Folgendes ein:

    InternetAddress addresses[] = 
        { new InternetAddress("john@contoso.com"),
          new InternetAddress("wendy@contoso.com") };
    
    message.setReplyTo(addresses);

Um einen **Cc** -Empfänger hinzuzufügen, verwenden Sie die folgenden Schritte aus:

    message.addRecipient(Message.RecipientType.CC, new 
    InternetAddress("john@contoso.com"));

## <a name="how-to-use-additional-sendgrid-services"></a>So: Verwenden Sie zusätzliche SendGrid Dienste

SendGrid bietet Web-basierte APIs, mit denen Sie weitere SendGrid Funktionen aus Azure-Anwendung nutzen können. Vollständige Details finden Sie in der [Dokumentation zur SendGrid-API][].

## <a name="next-steps"></a>Nächste Schritte

Die Grundlagen des SendGrid e-Mail-Diensts bearbeitet haben, führen Sie die folgenden Links, um weitere Informationen.

* Beispiel, das veranschaulicht, wie mit SendGrid in einer Azure-Bereitstellung: [Informationen zum Senden von e-Mail-Nachricht mithilfe der SendGrid von Java in Azure-Bereitstellung](store-sendgrid-java-how-to-send-email-example.md)
* SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html>
* Dokumentation zur SendGrid-API: <https://sendgrid.com/docs/API_Reference/index.html>
* SendGrid besonderes Angebot Azure Kunden: <https://sendgrid.com/windowsazure.html>

  [http://sendgrid.com]: https://sendgrid.com
  [http://sendgrid.com/pricing.html]: http://sendgrid.com/pricing.html
  [http://www.sendgrid.com/azure.html]: https://www.sendgrid.com/windowsazure.html
  [http://sendgrid.com/features]: https://sendgrid.com/features
  [http://www.oracle.com/technetwork/java/javamail]: http://www.oracle.com/technetwork/java/javamail/index.html
  [Filtereinstellungen]: https://sendgrid.com/docs/API_Reference/Web_API/filter_settings.html
  [Dokumentation zur SendGrid-API]: https://sendgrid.com/docs/API_Reference/index.html
  [http://sendgrid.com/azure.html]: https://sendgrid.com/windowsazure.html
  [Cloud-basierten e-Mail-Dienst]: https://sendgrid.com/email-solutions
  [Transaktionen e-Mail-Übermittlung]: https://sendgrid.com/transactional-email
