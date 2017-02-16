<properties 
    pageTitle="Azure mobilen Engagement - Back-End-integration" 
    description="Verbinden von Azure Mobile Engagement mit einer SharePoint-Back-End-massensendungen zu ermitteln von SharePoint erstellen" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement---api-integration"></a>Azure mobilen Engagement - API-integration

Ein automatisierte marketing-System auftreten, erstellen und Aktivieren der Marketingkampagnen auch automatisch. Für diesen Zweck - ermöglicht Azure Mobile Engagement solche automatisierten Marketingkampagnen auch mithilfe von APIs erstellen. 

In der Regel Formular mit Kunden die Mobile Engagement front-End-Benutzeroberfläche um Ankündigungen/Umfragen usw. als Teil ihrer Marketingkampagnen zu erstellen. Sobald die Marketingkampagnen Reifen sind, besteht jedoch nutzen Sie die Daten in die Back-End-Systeme (wie alle CRM-System oder CMS System wie SharePoint) gesperrt, sodass eine vollständig automatisierte Verkaufspipeline erstellt werden kann, die in Mobile Engagement dynamisch basierend auf den Daten, die von den Back-End-Systemen in parallelen massensendungen zu ermitteln erstellt werden muss. 

![][5]

In diesem Lernprogramm durchläuft ein Szenario, wo ein SharePoint-Benutzer Business füllt einer SharePoint-Liste mit marketing-Daten und ein automatisiertes Prozesses Elemente aus der Liste aufnimmt und stellt eine Verbindung mit Mobile Engagement System verfügbaren REST APIs So erstellen Sie eine Marketingkampagne aus der SharePoint-Daten verwenden. 
 
> [AZURE.IMPORTANT] Im Allgemeinen können Sie in diesem Beispiel als Ausgangspunkt verwenden, um zu verstehen, wie jeder Mobile Engagement REST-API aufgerufen, wie sie die beiden wichtigsten Aspekte des Aufrufen der APIs - Authentifizierung sowie die Übergabe Parameter enthält details zu. 

## <a name="sharepoint-integration"></a>SharePoint-integration
1. Hier sind die SharePoint-Beispielliste aussieht. **Titel**, **Kategorie**, **NotificationTitle**, **Nachricht** und **URL** werden verwendet, für die Ankündigung erstellen. Es gibt eine Spalte mit der Bezeichnung **IsProcessed** , die von der Stichprobe Automatisierung Prozess in Form von einem Programm Console verwendet wird. Können Sie manuell ausführen dieser Konsole Programm als eine Azure WebJob, geplant werden können, oder Sie können direkt zu den SharePoint-Workflow zum Programm erstellen und aktivieren die Ankündigung, wenn ein Element in der SharePoint-Liste eingefügt wird. In diesem Beispiel verwenden wir die Console-Anwendung durchläuft die Elemente in der SharePoint-Liste und Ankündigung Azure Mobile Engagement für jede hiervon erstellen und dann schließlich kennzeichnet die Kennzeichnung **IsProcessed** zum erfolgreichen Ankündigung Creation erfüllt werden.

    ![][1]

2. Wir verwenden den Code aus der Stichprobe *Remote Authentication in SharePoint Online mithilfe der Clientobjektmodell* [hier](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) Authentifizierung mit der SharePoint-Liste.
 
3. Nach der Authentifizierung wir durchlaufen Sie die Einträge, um herauszufinden, alle neu erstellten Elemente (die haben **IsProcessed** = False). 

        static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);

                // Load the SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();

                // Loop through the list to go through all the items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;

                        // Send an HTTP request to create AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;

                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this to true
                            item["IsProcessed"] = "Yes";

                            // Now try to activate the campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a>Mobile Engagement-integration
1.  Wenn wir ein Element gefunden Verarbeitung - einzugeben extrahieren wir die erforderlichen Informationen für das Erstellen einer Ankündigung aus der Listenelement und rufen `CreateAzMECampaign` zu erstellen und anschließend `ActivateAzMECampaign` um es zu aktivieren. Dies sind im Wesentlichen die Mobile Engagement Back-End-anrufende REST-API Anrufe. 

2.  Die Mobile Engagement REST-APIs erfordern eine **Standardauthentifizierung Farbschema Autorisierung HTTP-Header** der umfasst die `ApplicationId` und die `ApiKey` die Sie von der Azure-Portal zu gelangen. Stellen Sie sicher, dass Sie die Taste aus dem Abschnitt **api Tasten** verwenden und *nicht* aus dem Abschnitt **Sdk Tasten** . 

    ![][2]

        static string CreateAuthZHeader()
        {
            string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
            return BasicAuthzHeaderString;
        }

        static public string EncodeTo64(string toEncode)
        {
            byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
            string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
            return returnValue;
        }  

3. Zum Erstellen der Ankündigung Geben Sie für eine Marketingkampagne – finden Sie in der [Dokumentation](https://msdn.microsoft.com/library/azure/mt683750.aspx). Sie benötigen, um sicherzustellen, dass Sie die für eine Marketingkampagne angeben `kind` als *Ankündigung* und die [Nutzlast](https://msdn.microsoft.com/library/azure/mt683751.aspx) und die Übergabe als FormUrlEncodedContent. 

        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";

            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
                
                // Create the payload to send the content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
                
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });

                // Send the POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is the campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }

4. Nachdem Sie die Ankündigung erstellt haben, sehen Sie etwa Folgendes im Portal Mobile Engagement (Beachten Sie, dass der Status = Text- und aktiviert = n/v)

    ![][3]

5. `CreateAzMECampaign`erstellt eine Ankündigung für eine Marketingkampagne und seine Id gibt, um den Anrufer. `ActivateAzMECampaign`erfordert diese Id als Parameter, die für eine Marketingkampagne zu aktivieren. 

        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());

                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });

                // Send the POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }

6. Nachdem Sie die Ankündigung aktiviert haben, sehen Sie etwa Folgendes im Portal Mobile Engagement:

    ![][4]

7. Sobald die für eine Marketingkampagne aktiviert wird, beginnt alle Geräte, die das Kriterium auf die für eine Marketingkampagne erfüllen Benachrichtigungen angezeigt. 

8. Beachten Sie auch, dass das Listenelement mit IsProcessed gekennzeichnet = False weist true festgelegt wurde, nachdem die Ankündigung für eine Marketingkampagne erstellt wurde.  

In diesem Beispiel erstellt einen einfachen Ankündigung für eine Marketingkampagne hauptsächlich die erforderlichen Eigenschaften angeben. Sie können diese anpassen, wie Sie mithilfe der Informationen aus dem Portal können [hier](https://msdn.microsoft.com/library/azure/mt683751.aspx). 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png


 
