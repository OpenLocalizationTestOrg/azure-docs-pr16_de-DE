<properties 
    pageTitle="Verwenden die REST-API zum Azure Mobile Engagement Service-APIs zugreifen" 
    description="Beschreibt, wie Sie mithilfe der Mobile Engagement REST-APIs Azure Mobile Engagement Service-APIs zugreifen"       
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="wesmc7777" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="wesmc;ricksal" />

#<a name="using-rest-to-access-azure-mobile-engagement-service-apis"></a>Verwendung von REST Azure Mobile Engagement Service-APIs Zugriff auf

Azure Mobile-Angebot umfasst die [Azure Mobile Engagement REST-API](https://msdn.microsoft.com/library/azure/mt683754.aspx) für Sie zum Verwalten von Geräten Reichweite/Pushbenachrichtigungen massensendungen zu ermitteln usw.. In diesem Beispiel wird die REST-APIs direkt zum Erstellen einer Ankündigung für eine Marketingkampagne, und klicken Sie dann aktivieren, und entscheiden Sie sich für eine Reihe von Geräten verwenden. 

Wenn Sie nicht die REST-APIs direkt verwenden möchten, bieten wir auch eine [Swagger Datei](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) , die mit den Tools zum Generieren von SDKs für Ihre bevorzugte Sprache verwendet werden können. Es empfiehlt sich, mithilfe des Tools für die [AutoRest](https://github.com/Azure/AutoRest) zum Generieren der SDK aus unserem Swagger-Datei. Wir haben ein .NET SDK ähnlich dem Sie interagieren mit diesen APIs mithilfe eines C#-Wrappers können erstellt, und nicht token Authentifizierungsverhandlungen und selbst aktualisieren müssen. Wenn Sie ein ähnliches Beispiel mit dieser Wrapper durchzuführen möchten, finden Sie unter [Service API .NET SDK-Beispiel](mobile-engagement-dotnet-sdk-service-api.md)

In diesem Beispiel wird die REST-APIs direkt zu erstellen, und aktivieren eine Ankündigung für eine Marketingkampagne verwendet. 

## <a name="adding-a-username-appinfo-to-the-mobile-engagement-app"></a>Hinzufügen einer AppInfo Benutzername zur Engagement Mobile-app

In diesem Beispiel benötigen Sie ein app-Info-Tag, der Engagement Mobile-app hinzugefügt. Sie können die Kategorie im Portal Engagement für Ihre app hinzufügen, indem Sie auf **Einstellungen** > **Kategorie (app-Info)** > **neue Kategorie (app-Info)**. Fügen Sie der neuen Kategorie mit dem Namen **Benutzername** als **Zeichenfolgentyp** hinzu.

![](./media/mobile-engagement-dotnet-rest-service-api/user-name-app-info.png)



Wenn Sie [Erste Schritte mit Azure Mobile Engagement für Windows universeller Apps](mobile-engagement-windows-store-dotnet-get-started.md)befolgt haben, können Sie die Kategorie **Benutzername** senden, indem Sie einfach überschreiben testen `OnNavigatedTo()` in Ihrer `MainPage` Class, die app-Info Kategorie ähnelt dem folgenden Code zu senden:

    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        base.OnNavigatedTo(e);

        String name = "Wesley"; // Prompt the user to provide this in your client app.

        Dictionary<object, object> info = new Dictionary<object, object>();
        info.Add("user_name", name);
        EngagementAgent.Instance.SendAppInfo(info);
    }
 


## <a name="creating-the-service-api-app"></a>Erstellen die app API-Diensts

1. Zunächst einmal benötigen Sie vier Authentifizierungsparameter mit diesem Beispiel verwenden. Diese Parameter sind **SubscriptionId**, **TenantId**, **p** und **geheim**. Um diese Authentifizierungsparameter zu erhalten, empfiehlt es sich, dass Sie den PowerShell Skript Ansatz erwähnt unter dem Abschnitt *einmalige Setup (mithilfe von Skripts)* in der [Authentifizierung](mobile-engagement-api-authentication.md#authentication) Lernprogramm verwenden. 

2. Arbeiten mit den restlichen Service-APIs zu erstellen, und aktivieren eine neue Ankündigung für eine Marketingkampagne veranschaulichen, verwenden Sie eine einfache Windows Console-app. So öffnen Sie Visual Studio aus, und erstellen Sie eine neue **Console-Anwendung**.   

3. Fügen Sie des **Newtonsoft.Json** NuGet-Pakets zu einem Projekt.

4. In der `Program.cs` ablegen, fügen Sie den folgenden `using` Anweisungen für die folgenden Namespaces:

        using System.IO;
        using System.Net;
        using Newtonsoft.Json.Linq;
        using Newtonsoft.Json;

5. Als Nächstes müssen Sie die folgenden Konstanten in definieren die `Program` Class. Diese werden für die Authentifizierung verwendeten und interagieren mit der Mobile-App, Engagement in der Sie die Ankündigung für eine Marketingkampagne erstellen:


        // Parameters needed for authentication of API calls.
        // These are returned from the PowerShell script in the authentication tutorial. 
        // https://azure.microsoft.com/documentation/articles/mobile-engagement-api-authentication/
        static String SubscriptionId = "<Your Subscription Id>";
        static String TenantId = "<Your TenantId>";
        static String ApplicationId = "<Your Application Id>";
        static String Secret = "<Your Secret>";

        // The token for authenticating with your Mobile Engagement app.
        static String Token = null;

        // This is the Azure Resource group concept for grouping together resources 
        // See: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        static String ResourceGroup = "MobileEngagement";

        // For Mobile Engagement operations
        // App Collection Name from the Azure portal 
        static String Collection = "<Your App Collection Name>";

        // Application Resource Name - make sure you are using the one as specified in the dashboard of the
        // Azure portal. (This is NOT the App Name)
        static String AppName = "WesmcRestTest-windows";

        // New campaign id returned from creating the new campaign and used to activate and push the campaign
        // a set of devices.
        static String NewCampaignID = null;

        // This list will hold the device Ids we choose to push the campaign to.
        static List<String> deviceList = new List<String>();


6. Die folgenden beiden Methoden zum Hinzufügen der `Program` Class. Diese werden REST APIs ausführen und Anzeigen von Antworten auf die Verwaltungskonsole verwendet werden.

        private static async Task<HttpWebResponse> ExecuteREST(string httpMethod, string uri, string authToken, WebHeaderCollection headers = null, string body = null, string contentType = "application/json")
        {
            //=== Execute the request 
            HttpWebRequest request = (HttpWebRequest)HttpWebRequest.Create(uri);
            HttpWebResponse response = null;

            request.Method = httpMethod;
            request.ContentType = contentType;
            request.ContentLength = 0;

            if (authToken != null)
                request.Headers.Add("Authorization", authToken);

            if (headers != null)
            {
                request.Headers.Add(headers);
            }

            if (body != null)
            {
                byte[] bytes = Encoding.UTF8.GetBytes(body);

                try
                {
                    request.ContentLength = bytes.Length;
                    Stream requestStream = request.GetRequestStream();
                    requestStream.Write(bytes, 0, bytes.Length);
                    requestStream.Close();
                }
                catch (Exception e)
                {
                    Console.WriteLine(e.Message);
                }
            }

            try
            {
                response = (HttpWebResponse)await request.GetResponseAsync();
            }
            catch (WebException we)
            {
                if (we.Response != null)
                {
                    response = (HttpWebResponse)we.Response;
                }
                else
                    Console.WriteLine(we.Message);
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
            }

            return response;
        }


        private static void DisplayResponse(dynamic data, HttpWebResponse response)
        {
            Console.WriteLine("HTTP Status " + (int)response.StatusCode + " : " + response.StatusDescription);
            Console.WriteLine(data + "\n");
        }

    }

7. Fügen Sie den folgenden Code zu Ihrem `Main` Methode, um eine Authentifizierungstoken mit den Authentifizierungsparametern generieren Sie gesammelt:

        //***************************************************************************
        //*** Get a valid authorization token with your authentication parameters ***
        //***************************************************************************

        String methodUrl = "https://login.microsoftonline.com/" + TenantId + "/oauth2/token";
        String requestBody = "grant_type=client_credentials&client_id=" + ApplicationId +
                            "&client_secret=" + Secret +
                            "&resource=https://management.core.windows.net/";
        Console.WriteLine("Getting authorization token...\n");
        HttpWebResponse response = ExecuteREST("POST", methodUrl, null, null, requestBody, null).Result;
        Stream receiveStream = response.GetResponseStream();
        StreamReader readStream = new StreamReader(receiveStream, Encoding.UTF8);
        dynamic data = JObject.Parse(readStream.ReadToEnd());
        readStream.Close();
        receiveStream.Close();
        DisplayResponse(data, response);

        if (response.StatusCode == HttpStatusCode.OK)
        {
            Token = data.token_type + " " + data.access_token;
            Console.WriteLine("Got authorization token...");
            Console.WriteLine("Authorization : " + Token + "\n");
        }
        else
        {
            Console.WriteLine("*** Failed to get authorization token. Check your parameters for API calls.\n");
            return;
        }

8. Nun verfügen wir über eine gültige Authentifizierungstoken können wir eine neue Zeit für eine Marketingkampagne mithilfe der [Erstellen für eine Marketingkampagne](https://msdn.microsoft.com/library/azure/mt683742.aspx) REST-API erstellen. Die neue Campaign werden eine einfache **jederzeit** & **Benachrichtigung nur** Ankündigung für eine Marketingkampagne mit einem Titel und einer Nachricht. Dies ist eine manuelle Pushbenachrichtigungen für eine Marketingkampagne wird, wie im folgenden Screenshot dargestellt. Dies bedeutet, dass er nur abgelegt werden werden mithilfe der APIs.


    ![](./media/mobile-engagement-dotnet-rest-service-api/manual-push.png)

    Fügen Sie den folgenden Code zu Ihrem `Main` Methode, um die Ankündigung für eine Marketingkampagne zu erstellen: 

        //*****************************************************************************
        //*** Create a campaign to send a notification using the user-name app-info ***
        //*****************************************************************************

        String newCampaignMethodUrl = "https://management.azure.com/subscriptions/" + SubscriptionId + "/" +
               "resourcegroups/" + ResourceGroup + "/providers/Microsoft.MobileEngagement/appcollections/" +
               Collection + "/apps/" + AppName + "/campaigns/announcements?api-version=2014-12-01";

        String campaignRequestBody = "{ \"name\": \"BirthdayCoupon\", " +
                                        "\"type\": \"only_notif\", " +
                                        "\"deliveryTime\": \"any\", " +
                                        "\"notificationType\": \"popup\", " +
                                        "\"pushMode\":\"manual\", " +
                                        "\"notificationTitle\": \"Happy Birthday ${user_name}\", " +
                                        "\"notificationMessage\": \"Take extra 10% off on your orders today!\"}";

        Console.WriteLine("Creating new campaign...\n");
        HttpWebResponse newCampaignResponse = ExecuteREST("POST", newCampaignMethodUrl, Token, null, campaignRequestBody).Result;
        Stream receiveCampaignStream = newCampaignResponse.GetResponseStream();
        StreamReader readCampaignStream = new StreamReader(receiveCampaignStream, Encoding.UTF8);
        dynamic newCampaignData = JObject.Parse(readCampaignStream.ReadToEnd());
        readCampaignStream.Close();
        receiveCampaignStream.Close();
        DisplayResponse(newCampaignData, newCampaignResponse);

        if (newCampaignResponse.StatusCode == HttpStatusCode.Created)
        {
            NewCampaignID = newCampaignData.id;
            Console.WriteLine("Created new campaign...");
            Console.WriteLine("New Campaign Id    : " + NewCampaignID + "\n");
        }
        else
        {
            Console.WriteLine("*** Failed to create birthday campaign.\n");
            return;
        }


9. Die für eine Marketingkampagne muss aktiviert werden, bevor sie auf eine beliebige Geräte abgelegt werden kann. Wir einsparen die ID für die neue für eine Marketingkampagne in der `NewCampaignID` Variable. Wir verwenden dies als Parameter Pfad URI, um die für eine Marketingkampagne mithilfe der [Aktivieren für eine Marketingkampagne](https://msdn.microsoft.com/library/azure/mt683745.aspx) REST-API aktivieren. Dadurch wird den Zustand des die für eine Marketingkampagne in **geplanten** geändert, obwohl sie nur mit den APIs manuell abgelegt werden.

    Fügen Sie den folgenden Code zu Ihrem `Main` Methode, um die Ankündigung für eine Marketingkampagne zu aktivieren: 

        //******************************************
        //*** Activate the new birthday campaign ***
        //******************************************

        String activateCampaignUrl = "https://management.azure.com/subscriptions/" + SubscriptionId + "/" +
                  "resourcegroups/" + ResourceGroup + "/providers/Microsoft.MobileEngagement/appcollections/" +
                   Collection + "/apps/" + AppName + "/campaigns/announcements/" + NewCampaignID +
                   "/activate?api-version=2014-12-01";

        Console.WriteLine("Activating new campaign (ID : " + NewCampaignID + ")...\n");
        HttpWebResponse activateCampaignResponse = ExecuteREST("POST", activateCampaignUrl, Token).Result;
        Stream activateCampaignStream = activateCampaignResponse.GetResponseStream();
        StreamReader activateCampaignReader = new StreamReader(activateCampaignStream, Encoding.UTF8);
        dynamic activateCampaignData = JObject.Parse(activateCampaignReader.ReadToEnd());
        activateCampaignReader.Close();
        activateCampaignStream.Close();
        DisplayResponse(activateCampaignData, activateCampaignResponse);

        if (activateCampaignResponse.StatusCode == HttpStatusCode.OK)
        {
            Console.WriteLine("Activated new campaign...");
            Console.WriteLine("New Campaign State : " + activateCampaignData.state + "\n");
        }
        else
        {
            Console.WriteLine("*** Failed to activate birthday campaign.\n");
            return;
        }

10. Um die für eine Marketingkampagne Pushbenachrichtigungen müssen Geräte-Ids für die Benutzer bereitstellen, die wir die Benachrichtigung erhalten möchten. Wir verwenden die [Abfrage Geräte](https://msdn.microsoft.com/library/azure/mt683826.aspx) REST-API, um alle Geräte-Ids zu erhalten. Wir werden jedem Gerät-Id in der Liste hinzufügen, wenn es **Benutzername** AppInfo zugeordnet sind.

    Fügen Sie den folgenden Code zu Ihrem `Main` Methode, um alle Geräte-Ids erhalten, und füllen die Geräteliste:

        //************************************************************************
        //*** Now that the manualPush campaign is activated, get the deviceIds ***
        //*** that you want to trigger the push campaign on.                   ***
        //************************************************************************

        String getDevicesUrl = "https://management.azure.com/subscriptions/" + SubscriptionId + "/" +
                  "resourcegroups/" + ResourceGroup + "/providers/Microsoft.MobileEngagement/appcollections/" +
                   Collection + "/apps/" + AppName + "/devices?api-version=2014-12-01";

        Console.WriteLine("Getting device IDs...\n");
        HttpWebResponse devicesResponse = ExecuteREST("GET", getDevicesUrl, Token).Result;
        Stream devicesStream = devicesResponse.GetResponseStream();
        StreamReader devicesReader = new StreamReader(devicesStream, Encoding.UTF8);
        dynamic devicesData = JObject.Parse(devicesReader.ReadToEnd());
        devicesReader.Close();
        devicesStream.Close();
        DisplayResponse(devicesData, devicesResponse);

        if (devicesResponse.StatusCode == HttpStatusCode.OK)
        {
            Console.WriteLine("Got devices.");
            foreach (dynamic device in devicesData.value)
            {
                // Just adding all in this example
                if (device.appInfo.user_name != null)
                {
                    Console.WriteLine("Adding device for push campaign...");
                    Console.WriteLine("Device ID    : " + device.deviceId);
                    Console.WriteLine("user_name    : " + device.appInfo.user_name);
                    deviceList.Add((String)device.deviceId);
                }
            }
            Console.WriteLine();
        }
        else
        {
            Console.WriteLine("*** Failed to get devices.\n");
            return;
        }


11. Schließlich werden wir die für eine Marketingkampagne alle Geräte-Ids in die Liste mithilfe der [Pushbenachrichtigungen für eine Marketingkampagne](https://msdn.microsoft.com/library/azure/mt683734.aspx) REST-API drücken. Dies ist eine Benachrichtigung **in der app** . Die app, also auf dem Gerät dafür vom Benutzer empfangen werden nacheinander ausgeführt werden müssen.

    Fügen Sie den folgenden Code zu Ihrem `Main` Methode, um die Campign an die Geräte in der Geräteliste zu übertragen:

        //**************************************************************
        //*** Trigger the manualPush campaign on the desired devices ***
        //**************************************************************

        String pushCampaignUrl = "https://management.azure.com/subscriptions/" + SubscriptionId + "/" +
                  "resourcegroups/" + ResourceGroup + "/providers/Microsoft.MobileEngagement/appcollections/" +
                   Collection + "/apps/" + AppName + "/campaigns/announcements/" + NewCampaignID + 
                   "/push?api-version=2014-12-01";

        Console.WriteLine("Triggering push for new campaign (ID : " + NewCampaignID + ")...\n");
        String deviceIds = "{\"deviceIds\":" + JsonConvert.SerializeObject(deviceList) + "}";
        Console.WriteLine("\n" + deviceIds + "\n");
        HttpWebResponse pushDevicesResponse = ExecuteREST("POST", pushCampaignUrl, Token, null, deviceIds).Result;
        Stream pushDevicesStream = pushDevicesResponse.GetResponseStream();
        StreamReader pushDevicesReader = new StreamReader(pushDevicesStream, Encoding.UTF8);
        dynamic pushDevicesData = JObject.Parse(pushDevicesReader.ReadToEnd());
        pushDevicesReader.Close();
        pushDevicesStream.Close();
        DisplayResponse(pushDevicesData, pushDevicesResponse);

        if (pushDevicesResponse.StatusCode == HttpStatusCode.OK)
        {
            Console.WriteLine("Triggered push on new campaign.\n");
        }
        else
        {
            Console.WriteLine("*** Failed to push campaign to the device list.\n");
        }


12. Erstellen und Ausführen der Console-app. Ihre Ausgabe sollte etwa wie folgt aussehen:


        C:\Users\Wesley\AzME_Service_API_Rest\test.exe

        Getting authorization token...
        
        HTTP Status 200 : OK
        {
          "token_type": "Bearer",
          "expires_in": "3599",
          "expires_on": "1458085162",
          "not_before": "1458081262",
          "resource": "https://management.core.windows.net/",
          "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPW
        b3dzLm5ldC8iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFh
        NzE4LTQ0YzQtOGVjMS0xM2IwODExMTRmM2UiLCJhcHBpZGFjciI6IjEiLCJpZHAiOiJodHRwczovL3N0cy53
        MTdhNGJkIiwic3ViIjoiOWIzZGQ2MDctNmYwOC00Y2Y5LTk2N2YtZmUyOGU3MTdhNGJkIiwidGlkIjoiNzJm
        F5x9gj8JJ4CjtLaH3mm1_U22Qc_AjB9mtbM97dgu3kCiUm19ISatRBoAmVz3JGW6LSv2TyCeg9TGYVrE3OAE
        hl_pY9COXicc7I501_X67GsmUgs9-EedF3STrEoY5cONTiMvwCLfeBgScgXA0ikAu62KpzMu0VFDYu-HASI8
        }
        
        Got authorization token...
        Authorization : Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNN
        aW5kb3dzLm5ldC8iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYt
        Zi1jNzE4LTQ0YzQtOGVjMS0xM2IwODExMTRmM2UiLCJhcHBpZGFjciI6IjEiLCJpZHAiOiJodHRwczovL3N0
        OGU3MTdhNGJkIiwic3ViIjoiOWIzZGQ2MDctNmYwOC00Y2Y5LTk2N2YtZmUyOGU3MTdhNGJkIiwidGlkIjoi
        iI-oF5x9gj8JJ4CjtLaH3mm1_U22Qc_AjB9mtbM97dgu3kCiUm19ISatRBoAmVz3JGW6LSv2TyCeg9TGYVrE
        vsf3hl_pY9COXicc7I501_X67GsmUgs9-EedF3STrEoY5cONTiMvwCLfeBgScgXA0ikAu62KpzMu0VFDYu-H
        
        Creating new campaign...
        
        HTTP Status 201 : Created
        {
          "id": 24,
          "state": "draft"
        }
        
        Created new campaign...
        New Campaign Id    : 24
        
        Activating new campaign (ID : 24)...
        
        HTTP Status 200 : OK
        {
          "id": 24,
          "state": "scheduled"
        }
        
        Activated new campaign...
        New Campaign State : scheduled
        
        Getting device IDs...
        
        HTTP Status 200 : OK
        {
          "value": [
            {
              "meta": {
                "lastSeen": 1458080534371,
                "firstSeen": 1458080534371,
                "lastLocation": 1458081389617,
                "lastInfo": 1458080571603
              },
              "appInfo": {
                "user_name": "Wesley"
              },
              "deviceId": "1d6208b8f281203ecb49431e2e5ce6b3"
            },
            {
              "meta": {
                "lastSeen": 1457990584698,
                "firstSeen": 1457949384025,
                "lastLocation": 1457990914895,
                "lastInfo": 1457990584597
              },
              "appInfo": {
                "user_name": "Wesley"
              },
              "deviceId": "302486644890e26045884ee5aa0619ec"
            }
          ]
        }
        
        Got devices.
        Adding device for push campaign...
        Device ID    : 1d6208b8f281203ecb49431e2e5ce6b3
        user_name    : Wesley
        Adding device for push campaign...
        Device ID    : 302486644890e26045884ee5aa0619ec
        user_name    : Wesley
        
        Triggering push for new campaign (ID : 24)...
        
        
        {"deviceIds":["1d6208b8f281203ecb49431e2e5ce6b3","302486644890e26045884ee5aa0619ec"]}
        
        HTTP Status 200 : OK
        {
          "invalidDeviceIds": []
        }
        
        Triggered push on new campaign.
        


<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
