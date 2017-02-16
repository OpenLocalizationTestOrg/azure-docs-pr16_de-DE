## <a name="create-the-webapi-project"></a>Erstellen Sie das Projekt WebAPI

In den folgenden Abschnitten wird eine neue ASP.NET WebAPI Back-End-erstellt werden, und er muss drei Hauptfunktionen:

1. **Clients authentifizieren**: ein Nachricht Ereignishandler hinzukommen authentifiziert Client-Anfragen und ordnen Sie den Benutzer die Anfrage.
2. **Client Benachrichtigung Registrierungen**: später fügen Sie einen Controller um neue Registrierungen für ein Clientgerät-Benachrichtigungen erhalten zu behandeln. Der Namen der authentifizierten Benutzer wird automatisch der Registrierung eine [Kategorie](https://msdn.microsoft.com/library/azure/dn530749.aspx)hinzugefügt werden.
3. **Senden von Benachrichtigungen für Clients**: später fügen Sie auch einen Controller, um die Möglichkeit für einen Benutzer zum Auslösen eines sicheren Pushbenachrichtigungen Geräte und Clients, die mit dem Tag verknüpft ist. 

Die folgenden Schritte zeigen, wie der neuen ASP.NET WebAPI Back-End-erstellen: 


> [AZURE.NOTE] **Wichtig**: vor dem Starten dieses Lernprogramms, stellen Sie sicher, dass Sie die neueste Version des NuGet Package Manager installiert haben. Um zu überprüfen, starten Sie Visual Studio. Klicken Sie im Menü **Extras** auf **Extensions und Updates**. Suchen Sie nach **NuGet-Paket-Manager für Visual Studio 2013**, und vergewissern Sie sich, dass Version 2.8.50313.46 oder höher. Wenn nicht, deinstallieren und dann die NuGet-Paket-Manager erneut zu installieren.
> 
> ![][B4]

> [AZURE.NOTE] Stellen Sie sicher, dass Sie die Visual Studio [Azure SDK](https://azure.microsoft.com/downloads/) für die Bereitstellung der Website installiert haben.

1. Starten Sie Visual Studio oder Visual Studio Express. Klicken Sie auf **Server-Explorer** , und melden Sie sich bei Ihrem Konto Azure. Visual Studio benötigen Sie erstellen die Website-Ressourcen für Ihr Konto angemeldet sind.
2. Klicken Sie in Visual Studio auf **Datei**, dann klicken Sie auf **neu**und dann auf **Projekt** **Vorlagen**, **Visual c#**, erweitern Sie und dann auf **Web-** und **ASP.NET Web-Anwendung**, geben Sie den Namen **AppBackend**, und klicken Sie dann auf **OK**. 
    
    ![][B1]

3. Klicken Sie im Dialogfeld **Neues Projekt von ASP.NET** klicken Sie auf **Web-API**und dann auf **OK**.

    ![][B2]

4. Wählen Sie ein Abonnement und ein **App-Serviceplan** bereits erstellt haben, klicken Sie im Dialogfeld **Microsoft Azure online zu konfigurieren** . Sie können auch wählen Sie **Erstellen einer neuen app-Serviceplan** und im Dialogfeld erstellen. Eine Datenbank benötigen nicht in diesem Lernprogramm. Wenn Sie Ihre app-Serviceplan ausgewählt haben, klicken Sie auf **OK** , um das Projekt zu erstellen.

    ![][B5]



## <a name="authenticating-clients-to-the-webapi-backend"></a>Authentifizierung in die Back-End-WebAPI-Clients

In diesem Abschnitt erstellen Sie eine neue Nachricht Ereignishandler-Klasse mit dem Namen **AuthenticationTestHandler** für die neue Back-End. Diese Klasse ist abgeleitet von [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) und als eine Nachricht Ereignishandler hinzugefügt werden, damit alle Anfragen in die Back-End-verarbeitet werden können. 



1. Im Lösung-Explorer mit der rechten Maustaste in des Projekts **AppBackend** , klicken Sie auf **Hinzufügen**, und klicken Sie dann klicken Sie auf **Klasse**. Nennen Sie die neue Klasse **AuthenticationTestHandler.cs**, und klicken Sie auf **Hinzufügen** , um die Klasse zu generieren. Diese Klasse werden zum Authentifizieren von Benutzern, die mithilfe der *Standardauthentifizierung* zur Vereinfachung verwendet werden. Beachten Sie, dass Ihre app ein Authentifizierungsschema verwenden kann.

2. Fügen Sie folgende AuthenticationTestHandler.cs, `using` Anweisungen:

        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Web;

3. In AuthenticationTestHandler.cs ersetzen die `AuthenticationTestHandler` Klasse Definition mit den folgenden Code. 

    Dieser Ereignishandler wird die Anfrage autorisieren, wenn die folgenden drei alle zutrifft:
    * Die Anforderung enthielt eine *Autorisierung* Kopfzeile. 
    * Die Anfrage verwendet *Standardauthentifizierung* . 
    * Sind dieselbe Zeichenfolge als Zeichenfolge der Benutzer sowie das Kennwort.

    Andernfalls wird die Anforderung abgelehnt. Dies ist nicht wahr Authentifizierung und Autorisierung und systematisch. Es ist nur ein einfaches Beispiel für dieses Lernprogramm aus.

    Wenn die Anfragenachricht authentifiziert und autorisiert, indem Sie die `AuthenticationTestHandler`, und klicken Sie dann auf die aktuelle Anforderung auf [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx)der Standardauthentifizierung Benutzer zugeordnet werden. Benutzerinformationen im HttpContext wird später von einem anderen Controller (RegisterController) verwendet werden, eine [Kategorie](https://msdn.microsoft.com/library/azure/dn530749.aspx) auf die Benachrichtigung Registrierung Anforderung hinzufügen.

        public class AuthenticationTestHandler : DelegatingHandler
        {
            protected override Task<HttpResponseMessage> SendAsync(
            HttpRequestMessage request, CancellationToken cancellationToken)
            {
                var authorizationHeader = request.Headers.GetValues("Authorization").First();
    
                if (authorizationHeader != null && authorizationHeader
                    .StartsWith("Basic ", StringComparison.InvariantCultureIgnoreCase))
                {
                    string authorizationUserAndPwdBase64 =
                        authorizationHeader.Substring("Basic ".Length);
                    string authorizationUserAndPwd = Encoding.Default
                        .GetString(Convert.FromBase64String(authorizationUserAndPwdBase64));
                    string user = authorizationUserAndPwd.Split(':')[0];
                    string password = authorizationUserAndPwd.Split(':')[1];
    
                    if (verifyUserAndPwd(user, password))
                    {
                        // Attach the new principal object to the current HttpContext object
                        HttpContext.Current.User =
                            new GenericPrincipal(new GenericIdentity(user), new string[0]);
                        System.Threading.Thread.CurrentPrincipal =
                            System.Web.HttpContext.Current.User;
                    }
                    else return Unauthorized();
                }
                else return Unauthorized();
    
                return base.SendAsync(request, cancellationToken);
            }
    
            private bool verifyUserAndPwd(string user, string password)
            {
                // This is not a real authentication scheme.
                return user == password;
            }
    
            private Task<HttpResponseMessage> Unauthorized()
            {
                var response = new HttpResponseMessage(HttpStatusCode.Forbidden);
                var tsc = new TaskCompletionSource<HttpResponseMessage>();
                tsc.SetResult(response);
                return tsc.Task;
            }
        }

    > [AZURE.NOTE] **Sicherheitshinweis**: die `AuthenticationTestHandler` Klasse bietet keine WAHR Authentifizierung. Es wird nur für Standardauthentifizierung Nachbilden verwendet und ist nicht sicher. Sie müssen eine sichere Authentifizierungsmethode in der Herstellung-Anwendungen und Dienste implementieren.               

4. Fügen Sie den folgenden Code am Ende der `Register` Methode in der Klasse **App_Start/WebApiConfig.cs** in der Nachricht Protokollhandler registriert werden:

        config.MessageHandlers.Add(new AuthenticationTestHandler());

5. Die Änderungen zu speichern.

## <a name="registering-for-notifications-using-the-webapi-backend"></a>Registrieren für Benachrichtigungen über WebAPI Backend

In diesem Abschnitt werden wir die WebAPI Back-End-Anfragen zum Registrieren eines Benutzers und das Gerät für Benachrichtigungen über den Clientbibliothek für Benachrichtigung Hubs verarbeitet einen neuen Controller hinzugefügt. Der Controller wird ein Benutzertag für den Benutzer, die authentifiziert und HttpContext durch angefügter wurde Hinzufügen der `AuthenticationTestHandler`. Die Kategorie haben das Zeichenfolgenformat `"username:<actual username>"`.


 

1. In Lösung Explorer mit der rechten Maustaste in des Projekts **AppBackend** , und klicken Sie dann auf **NuGet-Pakete verwalten**.

2. Klicken Sie auf der linken Bildschirmseite klicken Sie auf **Online**, und suchen Sie nach **Microsoft.Azure.NotificationHubs** in **das Suchfeld** .

3. Klicken Sie in der Ergebnisliste klicken Sie auf **Microsoft Azure Benachrichtigung Hubs**, und klicken Sie dann auf **Installieren**. Schließen Sie die Installation, und dann schließen Sie das Fenster NuGet-Paket-Manager.

    Dadurch wird einen Verweis auf die mit dem <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-Paket</a>Azure Benachrichtigung Hubs SDK hinzugefügt.

4. Wir werden nun eine neue Klassendatei erstellen, die die Verbindung mit Benachrichtigung Hub verwendet, um Benachrichtigungen senden darstellt. Explorer-Lösung mit der rechten Maustaste in des Ordners **Modelle** ", klicken Sie auf **Hinzufügen**" und dann auf **Class**". Namen der neuen Klasse **Notifications.cs**, klicken Sie auf **Hinzufügen** , die Klasse zu generieren. 

    ![][B6]

5. Fügen Sie folgende Notifications.cs, `using` -Anweisung am Anfang der Datei ein:

        using Microsoft.Azure.NotificationHubs;

6. Ersetzen der `Notifications` mit den folgenden definierte Klasse, und stellen Sie sicher, ersetzen Sie die zwei Platzhaltern mit der Verbindungszeichenfolge (mit Vollzugriff) für Ihre Benachrichtigung Hub und den Hubnamen ( [Klassische Azure-Portal](http://manage.windowsazure.com)verfügbar):

        public class Notifications
        {
            public static Notifications Instance = new Notifications();
        
            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }



7. Als Nächstes werden wir einen neuen Controller mit dem Namen **RegisterController**erstellen. Klicken Sie im Explorer-Lösung mit der rechten Maustaste in des Ordners **Controller** dann klicken Sie auf **Hinzufügen**, dann klicken Sie auf **Controller**. Klicken Sie auf der **Web-API 2-Controller – leeren** Element, und klicken Sie dann auf **Hinzufügen**. Nennen Sie die neue Klasse **RegisterController**, und klicken Sie dann auf **Hinzufügen** , um den Controller generieren.

    ![][B7]

    ![][B8]

8. Fügen Sie folgende RegisterController.cs, `using` Anweisungen:

        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;

9. Fügen Sie den folgenden Code innerhalb der `RegisterController` definierte Klasse. Beachten Sie, dass in diesem Code wir eine Kategorie Benutzer für den Benutzer hinzufügen, die diese HttpContext zugeordnet ist. Der Benutzer authentifiziert wurde, und die Nachricht filtern, die wir hinzugefügt haben, auf den HttpContext angefügt `AuthenticationTestHandler`. Sie können auch hinzufügen optional überprüft, um sicherzustellen, dass der Benutzer Zugriffsrechte für die angeforderten Tags registrieren verfügt.

        private NotificationHubClient hub;

        public RegisterController()
        {
            hub = Notifications.Instance.Hub;
        }

        public class DeviceRegistration
        {
            public string Platform { get; set; }
            public string Handle { get; set; }
            public string[] Tags { get; set; }
        }

        // POST api/register
        // This creates a registration id
        public async Task<string> Post(string handle = null)
        {
            string newRegistrationId = null;
            
            // make sure there are no existing registrations for this push handle (used for iOS and Android)
            if (handle != null)
            {
                var registrations = await hub.GetRegistrationsByChannelAsync(handle, 100);

                foreach (RegistrationDescription registration in registrations)
                {
                    if (newRegistrationId == null)
                    {
                        newRegistrationId = registration.RegistrationId;
                    }
                    else
                    {
                        await hub.DeleteRegistrationAsync(registration);
                    }
                }
            }

            if (newRegistrationId == null) 
                newRegistrationId = await hub.CreateRegistrationIdAsync();

            return newRegistrationId;
        }

        // PUT api/register/5
        // This creates or updates a registration (with provided channelURI) at the specified id
        public async Task<HttpResponseMessage> Put(string id, DeviceRegistration deviceUpdate)
        {
            RegistrationDescription registration = null;
            switch (deviceUpdate.Platform)
            {
                case "mpns":
                    registration = new MpnsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "wns":
                    registration = new WindowsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "apns":
                    registration = new AppleRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "gcm":
                    registration = new GcmRegistrationDescription(deviceUpdate.Handle);
                    break;
                default:
                    throw new HttpResponseException(HttpStatusCode.BadRequest);
            }

            registration.RegistrationId = id;
            var username = HttpContext.Current.User.Identity.Name;

            // add check if user is allowed to add these tags
            registration.Tags = new HashSet<string>(deviceUpdate.Tags);
            registration.Tags.Add("username:" + username);

            try
            {
                await hub.CreateOrUpdateRegistrationAsync(registration);
            }
            catch (MessagingException e)
            {
                ReturnGoneIfHubResponseIsGone(e);
            }

            return Request.CreateResponse(HttpStatusCode.OK);
        }

        // DELETE api/register/5
        public async Task<HttpResponseMessage> Delete(string id)
        {
            await hub.DeleteRegistrationAsync(id);
            return Request.CreateResponse(HttpStatusCode.OK);
        }

        private static void ReturnGoneIfHubResponseIsGone(MessagingException e)
        {
            var webex = e.InnerException as WebException;
            if (webex.Status == WebExceptionStatus.ProtocolError)
            {
                var response = (HttpWebResponse)webex.Response;
                if (response.StatusCode == HttpStatusCode.Gone)
                    throw new HttpRequestException(HttpStatusCode.Gone.ToString());
            }
        }

10. Die Änderungen zu speichern.

## <a name="sending-notifications-from-the-webapi-backend"></a>Das Senden von Benachrichtigungen über die WebAPI Back-End-

In diesem Abschnitt fügen Sie einen neuen Controller, der eine Möglichkeit zum Senden einer Benachrichtigung, basierend auf der Username-Kategorie mit Azure Benachrichtigung Hubs Service Management Bibliothek in der ASP.NET WebAPI Back-End-Client-Geräte verfügbar macht.


1. Erstellen Sie einen anderen neuen Controller mit dem Namen **NotificationsController**. Erstellen sie die gleiche Weise, wie Sie die **RegisterController** im vorherigen Abschnitt erstellt haben.

2. Fügen Sie folgende NotificationsController.cs, `using` Anweisungen:

        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;

3. Fügen Sie die folgende Methode zur Klasse **NotificationsController** .

    In diesem Code senden einen Benachrichtigungstyp basierend auf der Plattform Benachrichtigung Service (PNS) `pns` Parameter. Der Wert der `to_tag` wird verwendet, um die Nachricht die Kategorie *Benutzernamen* festlegen. Dieses Tag muss eine Kategorie Benutzernamen einer aktiven Benachrichtigung Hub Registrierung übereinstimmen. Die Benachrichtigung ist aus dem Textkörper der Anfrage Beitrag herausgezogen und für das Ziel PNS formatiert. 

    Je nach der Plattform Benachrichtigung Service (PNS), die Ihren unterstützten Geräten verwenden, um Benachrichtigungen zu erhalten, werden andere Benachrichtigungen unterstützt mit unterschiedlichen Formaten. Beispielsweise können Sie auf Windows-Geräten [Spruch Benachrichtigung mit WNS](https://msdn.microsoft.com/library/windows/apps/br230849.aspx) verwenden, die direkt von einer anderen PNS nicht unterstützt wird. Ihre Back-End-müssten so formatieren die Benachrichtigung in eine unterstützte Benachrichtigung für die PNS Geräte, dass die unterstützt werden soll. Verwenden Sie dann die entsprechende senden-API der [NotificationHubClient-Klasse](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx)

        public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag)
        {
            var user = HttpContext.Current.User.Identity.Name;
            string[] userTag = new string[2];
            userTag[0] = "username:" + to_tag;
            userTag[1] = "from:" + user;

            Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;
            HttpStatusCode ret = HttpStatusCode.InternalServerError;

            switch (pns.ToLower())
            {
                case "wns":
                    // Windows 8.1 / Windows Phone 8.1
                    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" + 
                                "From " + user + ": " + message + "</text></binding></visual></toast>";
                    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
                    break;
                case "apns":
                    // iOS
                    var alert = "{\"aps\":{\"alert\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(alert, userTag);
                    break;
                case "gcm":
                    // Android
                    var notif = "{ \"data\" : {\"message\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendGcmNativeNotificationAsync(notif, userTag);
                    break;
            }

            if (outcome != null)
            {
                if (!((outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Abandoned) ||
                    (outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Unknown)))
                {
                    ret = HttpStatusCode.OK;
                }
            }

            return Request.CreateResponse(ret);
        }


4. Drücken Sie **F5** , um die Anwendung ausführen und die Genauigkeit der Ihre Arbeit bisher sicherzustellen. Die app sollte einen Webbrowser starten und Anzeigen der Startseite ASP.NET. 

##<a name="publish-the-new-webapi-backend"></a>Neue WebAPI Backend veröffentlichen

1. Wir werden nun diese app zu einer Website Azure bereitzustellen, um ein von sämtlichen Geräten zugänglich machen. Mit der rechten Maustaste auf das Projekt **AppBackend** , und wählen Sie **Veröffentlichen**aus.

2. Wählen Sie **Microsoft Azure Web Apps** als Ziel veröffentlichen aus.

    ![][B15]

3. Melden Sie sich mit Ihrem Azure-Konto, und wählen Sie aus einer vorhandenen oder neuen Web App.

    ![][B16]

4. Notieren Sie die **Ziel-URL** -Eigenschaft auf der Registerkarte **Verbindung** . Wir werden auf diese URL wie Ihre *Back-End-Endpunkt* später in diesem Lernprogramm verweisen. Klicken Sie auf **Veröffentlichen**.

    ![][B18]


[B1]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push1.png
[B2]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push2.png
[B3]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push3.png
[B4]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push4.png
[B5]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push5.png
[B6]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push6.png
[B7]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push7.png
[B8]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push8.png
[B14]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push14.png
[B15]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users15.PNG
[B16]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users16.PNG
[B18]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users18.PNG
