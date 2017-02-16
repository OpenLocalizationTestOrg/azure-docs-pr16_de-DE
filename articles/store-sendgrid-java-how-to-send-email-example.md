<properties 
    pageTitle="Store-sendgrid-Java-How-to-Send-Email-example" 
    description="Informationen zum Senden von e-Mail-Nachricht mithilfe von Java SendGrid in einer Azure-Bereitstellung" 
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
    ms.author="vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com"/>

# <a name="how-to-send-email-using-sendgrid-from-java-in-an-azure-deployment"></a>Informationen zum Senden von e-Mail-Nachricht mithilfe von Java SendGrid in Azure-Bereitstellung

Im folgenden Beispiel wird Sie, wie Sie SendGrid zum Senden von e-Mails aus einer Webseite in Azure gehostet verwenden können. Die resultierende Anwendung fordert den Benutzer für e-Mail-Werte aus, wie im folgenden Screenshot dargestellt.

![E-Mail-Formular][emailform]

Die resultierende e-Mail wird folgende Bildschirmdarstellung ähneln.

![E-Mail-Nachricht][emailsent]

Sie müssen gehen Sie wie folgt vor, um den Code in diesem Thema verwenden:

1. Beziehen Sie die Gläser zu "javax.Mail", beispielsweise von <http://www.oracle.com/technetwork/java/javamail/index.html>aus.
2. Fügen Sie die Gläser an den Pfad der Java-Generator an.
3. Wenn Sie diese Anwendung Java erstellen "Ellipse" verwenden, können Sie die SendGrid-Bibliotheken in Ihrer Anwendung Bereitstellung-Datei (WAR) mithilfe des Ellipse Feature zur Bereitstellung von Assembly einbeziehen. Wenn Sie diese Anwendung Java erstellen nicht "Ellipse" verwenden, stellen Sie sicher, die Bibliotheken innerhalb der gleichen Azure Rolle als Ihre Java-Anwendung enthalten sind, und den Pfad der Anwendung hinzugefügt.


Sie müssen auch eigene SendGrid-Benutzernamen und Ihr Kennwort ein, die e-Mails senden können. Um mit SendGrid anzufangen, finden Sie [Informationen zum Senden von e-Mail-Nachricht mithilfe von Java SendGrid](store-sendgrid-java-how-to-send-email.md)aus.

Vertrautheit mit den Informationen unter [Erstellen einer Hallo Welt-Anwendung für Azure in "Ellipse"](http://msdn.microsoft.com/library/windowsazure/hh690944)oder zusammen mit anderen Techniken für Java-Anwendungen in Azure Hostinganbieter, wenn Sie nicht, Ellipse arbeiten, wird außerdem dringend empfohlen.

## <a name="create-a-web-form-for-sending-email"></a>Erstellen eines Webformulars für das Senden von e-Mails

Mit dem folgende Code wird zum Erstellen eines Webformulars zum Abrufen von Benutzerdaten für das Senden einer e-Mail. Aus Gründen der dieses Inhaltstyps heißt JSP-Datei **emailform.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Send this email</b>.</p>
     <br/>
      <form action="sendemail.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="emailTo">
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="emailFrom">
           </td>
         </tr>
         <tr>
           <td>Subject:</td>
           <td><input type="text" size=100 name="emailSubject" value="My email subject">
           </td>
         </tr>
         <tr>
           <td>Text:</td>
           <td><input type="text" size=400 name="emailText" value="Hello,<p>This is my message.</p>Thank you." />
           </td>
         </tr>
         <tr>
           <td>SendGrid user name:</td>
           <td><input type="text" name="sendGridUser">
           </td>
         </tr>
         <tr>
           <td>SendGrid password:</td>
           <td><input type="password" name="sendGridPassword">
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Send this email">
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-send-the-email"></a>Erstellen Sie den Code, um die e-Mail zu senden

Der folgende Code ein, die beim Abschluss des Formulars auf emailform.jsp aufgerufen wird, wird die e-Mail-Nachricht erstellt und sendet sie. Aus Gründen der dieses Inhaltstyps heißt JSP-Datei **sendemail.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email processing happens here</title>
    </head>
    <body>
        <b>This is my send mail page.</b><p/>
     <%
     
     final String sendGridUser = request.getParameter("sendGridUser");
     final String sendGridPassword = request.getParameter("sendGridPassword");
     
     class SMTPAuthenticator extends Authenticator
     {
       public PasswordAuthentication getPasswordAuthentication()
       {
            String username = sendGridUser;
            String password = sendGridPassword;
          
            return new PasswordAuthentication(username, password);   
       }
     }
     try
     {
         
         // The SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";
    
         Properties properties;
        
         properties = new Properties();
         
         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");
         
         // Display the email fields entered by the user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");
    
         // Create the authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();
         
         // Create the mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);
         
         // Display debug information to stdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);
    
         // Create the message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 
         
         message = new MimeMessage(mailSession);
         
         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            
    
         // Specify the email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);
         
         // Uncomment the following if you want to add a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");
    
         // Uncomment the following if you want to enable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");
         
         Transport transport;
         transport = mailSession.getTransport();
         // Connect the transport object.
         transport.connect();
         // Send the message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close the connection.
         transport.close();
     
        out.println("<p>Email processing completed.</p>");
         
     }
     catch (Exception e)
     {
         out.println("<p>Exception encountered: " + 
                            e.getMessage()     +
                            "</p>");   
     }
    %>
    
    </body>
    </html>

Zusätzlich zum Senden der e-Mails, bietet emailform.jsp ein Ergebnis für den Benutzer. die folgenden Screenshot ist ein Beispiel:

![Senden von e-Mail-Ergebnis][emailresult]

## <a name="next-steps"></a>Nächste Schritte

Bereitstellen der Anwendung auf dem Serveremulator in einem Browser ausgeführt werden emailform.jsp, geben Sie Werte in das Formular, klicken Sie auf **diese e-Mail-Nachricht senden**und dann die Ergebnisse in sendemail.jsp angezeigt.

Dieser Code wurde um zu zeigen, wie Sie mithilfe von SendGrid in Java auf Azure bereitgestellt. Vor der Bereitstellung in Azure produziert, möchten Sie möglicherweise weitere Fehlerbehandlung oder andere Features hinzufügen. Beispiel: 

* Azure-Speicher Blobs oder SQL-Datenbank können Sie e-Mail-Adressen und e-Mail-Nachrichten, anstatt mithilfe eines Webformulars speichern. Informationen zur Verwendung von Azure-Speicher Blobs in Java finden Sie unter [der Blob-Speicherdienst von Java verwenden](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/). Informationen zur Verwendung von SQL-Datenbank in Java finden Sie unter [Verwenden der SQL-Datenbank in Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).
* Sie können `RoleEnvironment.getConfigurationSettings` zum Abrufen von SendGrid Benutzername und Kennwort in der Bereitstellung Konfiguration Einstellungen anstelle von Webformular diese Werte abgerufen. Informationen zu den `RoleEnvironment` Klasse, finden Sie unter [Verwenden der Azure Service Runtime-Bibliothek in JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) und der Azure-Dienstlaufzeit Paket-Dokumentation unter <http://dl.windowsazure.com/javadoc>.
* Weitere Informationen zur Verwendung von SendGrid in Java finden Sie unter [Informationen zum Senden von e-Mail-Nachricht mithilfe von Java SendGrid](store-sendgrid-java-how-to-send-email.md).

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
