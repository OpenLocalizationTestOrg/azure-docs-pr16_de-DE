<properties 
    pageTitle="So stellen Sie einen Anruf aus Twilio (.NET) | Microsoft Azure" 
    description="Informationen Sie zum Tätigen eines Telefonanrufs, und senden eine SMS-Nachricht mit dem Dienst Twilio API auf Azure. Codebeispielen in .NET geschrieben wurde." 
    services="" 
    documentationCenter=".net" 
    authors="devinrader" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="05/04/2016" 
    ms.author="microsofthelp@twilio.com"/>




# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>So stellen Sie einen Anruf mit Twilio in einer Webrolle auf Azure

Mit diesem Leitfaden wird veranschaulicht, wie Twilio zu verwenden, um bei einem Anruf aus einer Webseite in Azure gehostet wird. Die resultierende Anwendung fordert den Benutzer zur Anruf Werte, wie im folgenden Screenshot dargestellt.

![Azure Anruf Formular mit Twilio und ASP.NET][twilio_dotnet_basic_form]

## <a name="a-nametwilio-prereqsaprerequisites"></a><a name="twilio-prereqs"></a>Erforderliche Komponenten

Sie müssen gehen Sie wie folgt vor, um den Code in diesem Thema verwenden:

1. Erwerben eines Twilio-Konto und Authentifizierung token. Um mit Twilio anzufangen, melden Sie sich an [https://www.twilio.com/try-twilio][try_twilio]. Sie können die Preise bei [http://www.twilio.com/pricing]auswerten[twilio_pricing]. Informationen über die API von Twilio finden Sie unter [http://www.twilio.com/voice/api][twilio_api].
2. Fügen Sie der Bibliothek Twilio .NET für Ihre Webrolle hinzu. Finden Sie unter "So fügen die Twilio-Bibliotheken zum Projekt Web Rolle" weiter unten in diesem Thema.

Sie sollten bei der Erstellung einer einfachen Webrolle auf Azure vertraut sein.

## <a name="a-namehowtocreateformahow-to-create-a-web-form-for-making-a-call"></a><a name="howtocreateform"></a>So: Erstellen eines Webformulars für das Tätigen eines Anrufs

<a id="use_nuget"></a>So fügen Sie die Twilio-Bibliotheken zum Projekt Rolle Web hinzu:

1.  Öffnen Sie Ihre Lösung in Visual Studio.
2.  Mit der rechten Maustaste **verweisen**.
3.  Klicken Sie auf **NuGet-Pakete verwalten**.
4.  Klicken Sie auf **Online**.
5.  Geben Sie im Suchfeld online *Twilio*ein.
6.  Klicken Sie auf das Paket Twilio auf **Installieren** .

Mit dem folgende Code wird zum Erstellen eines Webformulars zum Abrufen von Benutzerdaten einen Anruf zu tätigen. In diesem Beispiel wird eine ASP.NET Webrolle mit dem Namen **TwilioCloud** erstellt.

    <%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
        AutoEventWireup="true" CodeBehind="Default.aspx.cs"
        Inherits="WebRole1._Default" %>

    <asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
    </asp:Content>
    <asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
        <div>
            <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
            </asp:BulletedList>
        </div>
        <div>
            <p>Fill in all fields and click <b>Make this call</b>.</p>
            <div>
                To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
                Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
                <asp:Button ID="callpage" runat="server" Text="Make this call"
                    onclick="callpage_Click" />
            </div>
        </div>
    </asp:Content>

## <a name="a-idhowtocreatecodeahow-to-create-the-code-to-make-the-call"></a><a id="howtocreatecode"></a>So: Erstellen Sie den Code, um den Anruf zu tätigen
Der folgende Code ein, die aufgerufen wird, wenn der Benutzer das Formular abschließt, wird die Anruf Nachricht erstellt und den Anruf generiert. In diesem Beispiel wird der Code in den Onclick-Ereignishandler der Schaltfläche auf dem Formular ausgeführt werden. (Verwenden Sie Ihr Twilio-Konto und Authentifizierung anstelle der Platzhalterwerte zugewiesen **AccountSID** und **AuthToken** im folgenden Code token.)

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;
    using Twilio;

    namespace WebRole1
    {
        public partial class _Default : System.Web.UI.Page
        {
            protected void Page_Load(object sender, EventArgs e)
            {

            }

            protected void callpage_Click(object sender, EventArgs e)
            {
                // Call porcessing happens here.

                // Use your account SID and authentication token instead of
                // the placeholders shown here.
                string accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
                string authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

                // Instantiate an instance of the Twilio client.
                TwilioRestClient client;
                client = new TwilioRestClient(accountSID, authToken);

                // Retrieve the account, used later to retrieve the
                Twilio.Account account = client.GetAccount();
                string APIversuion = client.ApiVersion;
                string TwilioBaseURL = client.BaseUrl;

                this.varDisplay.Items.Clear();
                if (this.toNumber.Text == "" || this.message.Text == "")
                {
                    this.varDisplay.Items.Add(
                            "You must enter a phone number and a message.");
                }
                else
                {
                    // Retrieve the values entered by the user.
                    string to = this.toNumber.Text;
                    string myMessage = this.message.Text;

                    // Create a URL using the Twilio message and the user-entered
                    // text. You must replace spaces in the user's text with '%20'
                    // to make the text suitable for a URL.
                    String Url = "http://twimlets.com/message?Message%5B0%5D="
                            + myMessage.Replace(" ", "%20");

                    // Display the endpoint, API version, and the URL for the message.
                    this.varDisplay.Items.Add("Using Twilio endpoint "
                        + TwilioBaseURL);
                    this.varDisplay.Items.Add("Twilioclient API Version is "
                        + APIversuion);
                    this.varDisplay.Items.Add("The URL is " + Url);

                    // Instantiate the call options that are passed
                    // to the outbound call.
                    CallOptions options = new CallOptions();

                    // Set the call From, To, and URL values.                    
                    options.From = "+14155992671";
                    options.To = to;
                    options.Url = Url;

                    // Place the call.
                    var call = client.InitiateOutboundCall(options);
                    this.varDisplay.Items.Add("Call status: " + call.Status);
                }
            }
        }
    }

Besteht aus der Anruf und den Endpunkt Twilio, API-Version und der Status des Anrufs angezeigt werden. Das folgende Bildschirmabbild zeigt die Ausgabe einer Stichprobe ausführen.

![Azure Anruf Antwort mit Twilio und ASP.NET][twilio_dotnet_basic_form_output]

Weitere Informationen zu TwiML finden Sie unter [http://www.twilio.com/docs/api/twiml][twiml]. Weitere Informationen zu &lt;sagen&gt; und anderen Twilio Verben finden Sie unter [http://www.twilio.com/docs/api/twiml/say][twilio_say].

## <a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Nächste Schritte
Zum Anzeigen von Grundfunktionen Twilio in einer ASP.NET Webrolle auf Azure verwenden, wurde dieser Code bereitgestellt. Vor der Bereitstellung in Azure produziert, möchten Sie möglicherweise weitere Fehlerbehandlung oder andere Features hinzufügen. Beispiel:

* Verwenden eines Webformulars, sondern können Sie Azure Blob-Speicher oder eine Instanz von SQL Azure-Datenbank Speichern von Telefonnummern und rufen Text. Informationen zur Verwendung von Blobs in Azure finden Sie unter [So verwenden Sie den Dienst in .NET Azure Blob-Speicher][howto_blob_storage_dotnet]. Informationen zur Verwendung von SQL-Datenbank, finden Sie unter [SQL Azure-Datenbank in .NET Applications verwenden][howto_sql_azure_dotnet].
* Sie könnten RoleEnvironment.getConfigurationSettings zum Abrufen der Twilio Konto-ID und Authentifizierungstoken in der Bereitstellung Konfiguration Einstellungen anstelle von Programmieren die Werte in einem Formular verwenden. Informationen zu RoleEnvironment-Klasse finden Sie unter [Microsoft.WindowsAzure.ServiceRuntime Namespace][azure_runtime_ref_dotnet].
* Lesen die Twilio Sicherheitsrichtlinien bei [https://www.twilio.com/docs/security][twilio_docs_security].
* Weitere Informationen zu Twilio am [https://www.twilio.com/docs][twilio_docs].

##<a name="a-nameseealsoasee-also"></a><a name="seealso"></a>Siehe auch
* [Verwendung von Twilio für VoIP und SMS-Funktionen aus Azure](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx
