<properties 
    pageTitle="Wie Sie einen Anruf tätigen, aus Twilio (Java) | Microsoft Azure" 
    description="Erfahren Sie, wie Sie einen Anruf aus einer Webseite mit Twilio in einer Java-Anwendung auf Azure vornehmen." 
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

# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>So stellen Sie einen Anruf mit Twilio in einer Java-Anwendung auf Azure 

Im folgenden Beispiel wird Sie, wie Sie Twilio verwenden können, um bei einem Anruf aus einer Webseite in Azure gehostet wird. Die resultierende Anwendung fordert den Benutzer für den Anruf Werte aus, wie im folgenden Screenshot dargestellt.

![Azure Anruf Formular mit Twilio und Java][twilio_java]

Sie müssen gehen Sie wie folgt vor, um den Code in diesem Thema verwenden:

1. Erwerben eines Twilio-Konto und Authentifizierung token. Um mit Twilio anzufangen, auswerten Preise bei [http://www.twilio.com/pricing][twilio_pricing]. Sie können bei [https://www.twilio.com/try-twilio]registrieren[try_twilio]. Informationen über die API von Twilio finden Sie unter [http://www.twilio.com/api][twilio_api].
2. Die Twilio JAR zu erhalten. Bei [https://github.com/twilio/twilio-java][twilio_java_github], können Sie GitHub Quellen herunterladen und Erstellen eigener JAR oder einem vordefinierten Glas (mit oder ohne Abhängigkeiten) herunterladen.
Der Code in diesem Thema wurde unter Verwendung der vordefinierten TwilioJava 3.3.8 mit Abhängigkeiten JAR geschrieben.
3. Hinzufügen der JAR an den Pfad der Java-Generator an.
4. Wenn Sie diese Anwendung Java erstellen "Ellipse" verwenden, in die Anwendung Bereitstellung-Datei (WAR) mithilfe des Ellipse Feature zur Bereitstellung von Assembly aufnehmen der JAR-Twilio. Wenn Sie diese Anwendung Java erstellen nicht "Ellipse" verwenden, vergewissern Sie sich innerhalb der gleichen Azure Rolle als Ihre Java-Anwendung enthalten und den Pfad der Anwendung hinzugefügt wird der JAR-Twilio.
5. Stellen Sie sicher, Ihre Cacerts Keystore enthält das Zertifikat Equifax Secure Zertifizierungsstelle mit MD5 Fingerabdruck 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (die fortlaufende Zahl ist 35:DE:F4:CF und SHA1 Fingerabdruck D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Dies ist das Zertifikat der Zertifizierungsstelle (CA) für die [https://api.twilio.com] [ twilio_api_service] -Dienst, der bei der Verwendung von Twilio APIs aufgerufen wird. Informationen zum Hinzufügen von dieses Zertifikats zu Ihrem JDKs Zertifizierungsstelle Store finden Sie unter [Hinzufügen eines Zertifikats in Java CA Zertifikat gespeichert][add_ca_cert].

Darüber hinaus Vertrautheit mit den Informationen im [Erstellen von einem Hallo Welt Anwendung mithilfe der Azure-Toolkit für "Ellipse"][azure_java_eclipse_hello_world], oder mit anderen Techniken für das Java-Anwendungen in Azure hosten, wenn Sie nicht "Ellipse" verwenden, wird dringend empfohlen.

## <a name="create-a-web-form-for-making-a-call"></a>Erstellen eines Webformulars für das Tätigen eines Anrufs

Mit dem folgende Code wird zum Erstellen eines Webformulars zum Abrufen von Benutzerdaten einen Anruf zu tätigen. Bei der in diesem Beispiel wird ein neues Webprojekt zu dynamischen, mit dem Namen **TwilioCloud**, erstellt wurde und **callform.jsp** als JSP-Datei hinzugefügt wurde.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Erstellen Sie den Code, um den Anruf zu tätigen
Der folgende Code ein, die aufgerufen wird, wenn der Benutzer das Formular angezeigt, indem Sie callform.jsp abschließt, wird die Anruf Nachricht erstellt und den Anruf generiert. Für dieses Beispiel JSP-Datei heißt **makecall.jsp** und dem **TwilioCloud** Projekt hinzugefügt wurde. (Verwenden Sie Ihr Twilio-Konto und Authentifizierung anstelle der Platzhalterwerte zugewiesen **AccountSID** und **AuthToken** im folgenden Code token.)

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";
     
         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");
     
         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");
    
         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");
     
         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");
     
         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;
     
         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");
    
         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);
     
         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

Zusätzlich zu den Anruf zeigt makecall.jsp an den Endpunkt Twilio, API-Version und der Status des Anrufs. Die folgenden Screenshot ist ein Beispiel:

![Azure Anruf Antwort mit Twilio und Java][twilio_java_response]

## <a name="run-the-application"></a>Führen Sie die Anwendung
Es folgen die allgemeinen Schritte zum Ausführen Ihrer Anwendungs; Details für diese Schritte finden Sie unter [Erstellen eines Hallo Welt Anwendung mithilfe der Azure-Toolkit für "Ellipse"][azure_java_eclipse_hello_world].

1. Exportieren Sie Ihrer TwilioCloud WAR in den Ordner Azure **Approot** an. 
2. Ändern Sie **startup.cmd** , um Ihre TwilioCloud WAR Entzippen Sie ihn an.
3. Kompilieren Sie die Anwendung für die Serveremulator.
4. Starten Sie eine Bereitstellung in der Serveremulator.
5. Öffnen Sie einen Browser, und führen Sie **Http://localhost:8080/TwilioCloud/callform.jsp**.
6. Geben Sie Werte in das Formular, klicken Sie auf **diese Anruf machen**und Lesen Sie die Ergebnisse in makecall.jsp.

Wenn Sie für die Bereitstellung auf Azure, erneute Kompilierung für die Bereitstellung in der Cloud, bereit sind in Azure bereitstellen und Ausführen http://*Your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp im Browser (einen Wert für *Your_hosted_name*ersetzen).

## <a name="next-steps"></a>Nächste Schritte
Zum Anzeigen von Grundfunktionen Twilio in Java auf Azure verwenden, wurde dieser Code bereitgestellt. Vor der Bereitstellung in Azure produziert, möchten Sie möglicherweise weitere Fehlerbehandlung oder andere Features hinzufügen. Beispiel:

* Verwenden eines Webformulars, sondern können Sie Azure-Speicher Blobs oder SQL-Datenbank Speichern von Telefonnummern und rufen Text. Informationen zur Verwendung von Azure-Speicher Blobs in Java finden Sie unter [So verwenden Sie den Dienst BLOB-Speicher von Java][howto_blob_storage_java]. Informationen zur Verwendung von SQL-Datenbank in Java, finden Sie unter [Verwenden der SQL-Datenbank in Java][howto_sql_azure_java].
* Sie könnten **RoleEnvironment.getConfigurationSettings** zum Abrufen der Twilio Konto-ID und Authentifizierungstoken aus der Bereitstellung Konfiguration Einstellungen, statt die Werte in makecall.jsp fest zu codieren verwenden. Informationen zu **RoleEnvironment** -Klasse, finden Sie unter [Verwenden der Azure Service Runtime-Bibliothek in JSP] [ azure_runtime_jsp] und der Azure-Dienstlaufzeit Paket-Dokumentation unter [http://dl.windowsazure.com/javadoc][azure_javadoc].
* Der makecall.jsp-Code weist einen Twilio bereitgestellten URL, [http://twimlets.com/message][twimlet_message_url], um die **Url** -Variable. Diese URL ermöglicht Reaktionen Twilio Markup Language (TwiML), die Twilio Vorgehensweise mit den Anruf informiert. Beispielsweise kann der TwiML, der zurückgegeben wird enthalten eine ** &lt;sagen&gt; ** Verb, das Text wird dem Empfänger fremdsprachigen ergibt. Anstatt die URL Twilio vorausgesetzt, könnten Sie Ihre eigenen ab, um zu Twilios Besprechungsanfrage Antworten erstellen; Weitere Informationen finden Sie unter [So verwenden Sie Twilio für VoIP und SMS-Funktionen in Java][howto_twilio_voice_sms_java]. Weitere Informationen zu TwiML finden Sie unter [http://www.twilio.com/docs/api/twiml][twiml], sowie weitere Informationen zu ** &lt;sagen&gt; ** und anderen Twilio Verben finden Sie unter [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Lesen die Twilio Sicherheitsrichtlinien bei [https://www.twilio.com/docs/security][twilio_docs_security].

Weitere Informationen zu Twilio, finden Sie unter [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Siehe auch
* [Verwendung von Twilio für VoIP und SMS-Funktionen in Java][howto_twilio_voice_sms_java]
* [Hinzufügen eines Zertifikats im Speicher Java Zertifizierungsstelle Zertifikate][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
