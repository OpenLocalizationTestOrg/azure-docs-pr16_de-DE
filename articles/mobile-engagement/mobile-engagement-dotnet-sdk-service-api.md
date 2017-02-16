<properties 
    pageTitle="Verwenden von .NET SDK Azure Mobile Engagement Service-APIs Zugriff auf" 
    description="Beschreibt, wie Sie Mobile Engagement .NET SDK zu verwenden, um Azure Mobile Engagement Service-APIs zugreifen"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="using-net-sdk-to-access-azure-mobile-engagement-service-apis"></a>Verwenden von .NET SDK Azure Mobile Engagement Service-APIs Zugriff auf

Azure Mobile Engagement stellt eine Reihe von APIs für Sie zum Verwalten von Geräten Reichweite/Pushbenachrichtigungen massensendungen zu ermitteln usw.. Für die Interaktion mit dieser APIs stellen wir auch eine [Swagger-Datei](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) , die mit den Tools zum Generieren von SDKs für Ihre bevorzugte Sprache verwendet werden können. Es empfiehlt sich, mithilfe des Tools für die [AutoRest](https://github.com/Azure/AutoRest) zum Generieren der SDK aus unserem Swagger-Datei. 

Wir haben ein .NET SDK ähnlich dem Sie interagieren mit diesen APIs mithilfe eines C#-Wrappers können erstellt, und nicht token Authentifizierungsverhandlungen und selbst aktualisieren müssen.  

In diesem Beispiel durchläuft den Satz von die folgenden Schritte zum Verwenden von .NET SDK aus:

1. Zunächst müssen Sie für die Einrichtung der Authentifizierung für Ihre APIs Azure Active Directory als beschrieben [hier](mobile-engagement-api-authentication.md#authentication)verwenden. Am Ende der folgenden Schritte aus sollten Sie einen gültigen **SubscriptionId**, **TenantId**, **p** und **geheim**verfügen. 

2. Arbeiten mit .NET SDK mit dem Szenario erstellen Sie eine Ankündigung für eine Marketingkampagne veranschaulichen, verwenden Sie eine einfache Windows Console-app. So öffnen Sie Visual Studio aus, und erstellen Sie eine **Console-Anwendung**.   

3. Als Nächstes müssen Sie das .NET SDK herunterladen, die als **Microsoft Azure Engagement Management Bibliothek** im Katalog Nuget zur Verfügung steht [hier](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).
Wenn Sie die Nuget von Visual Studio installieren, müssen Sie sicherstellen, dass Sie überprüfen die Option **Vorabversion einschließen** , bei der Suche nach dem Paket markiert haben:

    ![][1]

4. In der `Program.cs` ablegen, fügen Sie die folgenden Namespaces hinzu:

        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;

5. Als Nächstes müssen Sie die folgenden Konstanten zu definieren, die wir für die Authentifizierung und interagieren mit der Mobile-App, Engagement in der Sie die Ankündigung für eine Marketingkampagne erstellen verwendet wird:

        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";

        // This is the Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";

        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using the one as specified in the Azure portal (NOT the App Name)
        const string APP_RESOURCE_NAME = "";

6. Definieren der `EngagementManagementClient` Variable, die wir zum Aufrufen der Methoden Mobile Engagement SDK verwendet wird:

        static EngagementManagementClient engagementClient; 

7. Fügen Sie die folgenden Optionen, um Ihre `Main` Methode:

        try
            {
                // Initialize the Engagement SDK to call out APIs. 
                InitEngagementClient().Wait();

                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }

8. Definieren Sie die folgende Methode die übernimmt der Initialisierung der `EngagementManagementClient` durch zuerst Authentifizierung, und verbinden selbst dann mit der Mobile-App, Engagement an dem Sie die Ankündigung für eine Marketingkampagne erstellen möchten:

        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
            
            // This is the Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;

            // This is your Mobile Engagement App Collection & App Resource Name in which you create the campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }

    > [AZURE.IMPORTANT] Beachten Sie, dass die **App Ressourcenname** verwenden, wie in der Azure-Verwaltungsportal für den Parameter AppName definiert werden müssen. 

9. Definieren Sie schließlich die CreateCampaign-Methode die übernehmen, wird die zuvor initialisierten EngagementClient zum Erstellen einer einfachen **jederzeit**mit & **Benachrichtigung nur** für eine Marketingkampagne mit einem Titel und einer Nachricht: 

        private async static Task CreateCampaign()
        {
            //  Refer to the Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all the non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing the app!",
                type:"only_notif",
                deliveryTime:"any"
                );

            // Refer to the Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }

10. Führen Sie die app Console und werden auf dem erfolgreiche Erstellung der für eine Marketingkampagne Folgendes angezeigt:

    **Für eine Marketingkampagne Id '159' wurde erfolgreich erstellt und befindet sich im Status 'Entwurf'**

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
