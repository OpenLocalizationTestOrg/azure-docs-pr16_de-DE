<properties
    pageTitle="Benachrichtigung Hubs - Pushbenachrichtigungen Unternehmensarchitektur"
    description="Anleitung für den Einsatz von Azure Benachrichtigung Hubs in eine Enterprise-Umgebung"
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="enterprise-push-architectural-guidance"></a>Leitfaden für Unternehmen Pushbenachrichtigungen Architektur

Unternehmen sind heute allmählich verschieben in Richtung erstellen Windows-Dienste für eine Endbenutzer (extern) oder für die Mitarbeiter (intern). Sie verfügen über vorhandenen Back-End-Systeme direkte es werden Mainframecomputer oder einige LoB Applikationen die in der mobilen Anwendungsarchitektur integriert werden müssen. Mit diesem Leitfaden wird zur optimalen führen Sie diese möglichen Lösung auf allgemeine Szenarien empfehlen Integration sprechen.

Häufige erforderlich ist für das Senden von Pushbenachrichtigung an die Benutzer über ihre mobile Anwendung Eintreten ein Ereignisses relevante in die Back-End-Systeme. Z. B. eine Bank, die der Bank Bank app auf dem iPhone hat, möchte benachrichtigt werden, wenn eine EC über einen bestimmten Betrag in ihr Konto oder einem Intranetszenario, in denen ein Mitarbeiters aus der Finanzabteilung erfolgt, das eine Budget Genehmigung app auf Ihrem Windows Phone hat benachrichtigt werden möchte, wenn er eine Aufforderung zur Genehmigung erhält.

Die Bankkonto oder Genehmigung Verarbeitung ist wahrscheinlich einige Back-End-System ausgeführt werden, die eine Pushbenachrichtigungen für den Benutzer initiieren muss. Möglicherweise gibt es mehrere solche Back-End-Systeme die müssen alle dieselbe Art von Logik Pushbenachrichtigungen implementiert wird, wenn ein Ereignis eine Benachrichtigung auslöst erstellen. Hier die Komplexität liegt in Integration von mehreren Back-End-Systemen zusammen mit einem Pushbenachrichtigungen einzelnen System, wo die Endbenutzer möglicherweise zu anderen Benachrichtigungen abonniert haben und es auch möglicherweise mehrere mobile Anwendungen, z. B. bei mobilen apps Intranet eine mobile Anwendung möglicherweise aus mehreren solcher Back-End-Systemen Benachrichtigungen erhalten möchten. Die Back-End-Systeme nicht kennen oder noch Pushbenachrichtigungen Semantik/Technologie wissen, damit eine allgemeine Lösung hier traditionell eine Komponente vorstellen, die die Back-End-Systeme für alle Ereignisse relevante abfragt und wurde für das Senden von Nachrichten Pushbenachrichtigungen an den Kunden verantwortlich ist.
Hier werden wir über eine noch bessere Lösung mit Azure-Dienstbus - Thema-Abonnement-Modell, und wird die Komplexität gewahrt wird die Lösung skalierbare sprechen.

Hier die allgemeine Architektur der Lösung (mit mehreren mobilen apps GRG aber gleichmäßig verfügbar, wenn nur eine mobile-app vorhanden ist)

## <a name="architecture"></a>Architektur

![][1]

Die wichtigste in diesem Architektur Diagramm ist Azure Service Bus bietet eine Themen/Abonnements programming Modell (Weitere daran am [Service Bus Pub/Sub programming]). Der Empfänger, der in diesem Fall ist der mobilen Back-End-(normalerweise [Azure Mobile Service], der eine Pushbenachrichtigungen in der mobilen apps wird initiiert) empfangen Nachrichten nicht direkt in die Back-End-Systeme, aber stattdessen haben wir eine mittlere Abstraktionsschicht von [Azure-Dienstbus] wodurch mobilen Back-End-zum Empfangen von Nachrichten von einem oder mehreren Back-End-Systemen bereitgestellt. Ein Service Bus Thema muss für jede der Back-End-Systeme z. B. Konto HR, Finanzen, d. h. im Grunde "Themen" relevante, welche Nachrichten als der Pushbenachrichtigung gesendet wird initiiert erstellt werden. Die Back-End-Systeme werden zu folgenden Themen kontaktieren. Eine Mobile Back-End-können solche Themen durch Erstellen eines Abonnements Dienstbus abonnieren. Dadurch wird die mobile Back-End-erhalten eine Benachrichtigung aus dem entsprechenden Back-End-System berechtigen. Mobile Back-End-weiterhin auf ihre Abonnements Nachrichten empfangen und sobald eine eingeht, wird wieder und sendet sie als Benachrichtigung an seinem Hub Benachrichtigung. Benachrichtigung Hubs dann später übermittelt die Nachricht an die mobile-app. Um die Hauptkomponenten zusammenfassen, haben wir:

1. Back-End-Systemen (LoB und ältere Systeme)
    - Service Bus Thema erstellt
    - Sendet die Nachricht
2. Mobile Back-End-
    - Service-Abonnement erstellt
    - Empfängt Nachricht (über Back-End-System)
    - Sendet eine Benachrichtigung an Clients (über Azure Benachrichtigung Hub)
3. Mobile Anwendung
    - Empfängt und Benachrichtigung anzeigen

###<a name="benefits"></a>Vorteile:

1. Im Entkoppeln zwischen dem Empfänger (mobile-app/Dienst per Benachrichtigung Hub) und dem Absender (Back-End-Systeme) ermöglicht zusätzliche Back-End-Systeme Integration mit minimalen ändern.
2. Dadurch auch das Szenario mehrere mobile Apps Ereignisse von einem oder mehreren Back-End-Systemen empfangen werden.  

## <a name="sample"></a>Beispiel:

###<a name="prerequisites"></a>Erforderliche Komponenten
Führen Sie die folgenden Lernprogramme, mit der Konzepte als auch die gemeinsame Erstellung und Konfigurationsschritte vertraut zu machen:

1. [Programming Service Bus Pub/Sub] - Dies wird erläutert, die Details über das Arbeiten mit Service Bus Themen/Abonnements, so erstellen Sie einen Namespace Themen/Abonnements, enthalten kann zum Senden und Empfangen von Nachrichten daraus.
2. [Benachrichtigung Hubs - Lernprogramm Universal Windows] - Dies wird erläutert, wie Sie Einrichten von Windows Store-app und Benachrichtigung Hubs registrieren, und klicken Sie dann auf Benachrichtigungen.

###<a name="sample-code"></a>Beispiel-code

Der vollständige Code zeigt bei [Benachrichtigung Hub Beispiele]zur Verfügung. Es ist in drei Komponenten unterteilt:

1. **EnterprisePushBackendSystem**

    ein. Dieses Projekt verwendet das *WindowsAzure.ServiceBus* Nuget-Paket und basiert auf [Service Bus Pub/Sub programming].

    b. Hierbei handelt es sich um eine einfache C#-Console-app, ein Branchensystem zu simulieren, die die Nachricht an die mobile-app übermittelt werden konnte eingestellt.

        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the topic where we will send notifications
            CreateTopic(connectionString);

            // Send message
            SendMessage(connectionString);
        }

    c. `CreateTopic`wird verwendet, um das Thema Dienstbus erstellen, in dem wir werden Nachrichten senden.

        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already

            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }

    d. `SendMessage`Dient zum Senden der Nachrichten Service Bus Thema. Hier sind wir mit dem Thema regelmäßig zum Zweck der Stichprobe einfach eine Reihe von Zufallszahl Nachrichten senden. Es kann ein Back-End-System, das Nachrichten senden soll, wenn ein Ereignis eintritt.

        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);

            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
            };

            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);

                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);

                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);

                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }

2. **ReceiveAndSendNotification**

    ein. Dieses Projekt verwendet die *WindowsAzure.ServiceBus* und *Microsoft.Web.WebJobs.Publish* Nuget-Paketen und basiert auf [Service Bus Pub/Sub programming].

    b. Hierbei handelt es sich um eine andere C#-Konsole app der wir als eine [Azure WebJob] ausgeführt wird, da es zum Überwachen von Nachrichten aus den LoB/Back-End-Systemen fortlaufend ausgeführt hat. Dies wird ein Teil der mobilen Back-End-sein.

        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the subscription which will receive messages
            CreateSubscription(connectionString);

            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }

    c. `CreateSubscription`Dient zum Erstellen eines Abonnements Dienstbus nach dem Thema, wird das Back-End-System Nachrichten senden. Je nach dem Szenario Business wird diese Komponente ein oder mehrere Abonnements zu entsprechenden Themen erstellen (z. B. einige möglicherweise Nachrichten von HR-System, einige von Finanz- und So weiter empfangen werden)

        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);

            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }

    d. ReceiveMessageAndSendNotification wird verwendet, lesen die Nachricht aus dem Thema mit dem Abonnement und ist lesen erfolgreich gestalten eine Benachrichtigung (in der Stichprobe Szenario Windows systemeigenen Spruch Benachrichtigung) um die mobile Anwendung mit Azure Benachrichtigung Hubs gesendet werden.

        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");

            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);

            Client.Receive();

            // Continuously process messages received from the subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";

                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");

                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);

                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }

    e. Klicken Sie für die Veröffentlichung von dies als eine **WebJob**mit der rechten Maustaste auf die Lösung in Visual Studio, und wählen Sie **als WebJob veröffentlichen**

    ![][2]

    f. Wählen Sie Ihr Profil für die Veröffentlichung, und Erstellen einer neuen Azure-WebSite aus, wenn er noch nicht vorhanden ist dieser WebJob gehostet wird, und nachdem Sie die WebSite, und drücken Sie dann **Veröffentlichen**haben.

    ![][3]

    g. Konfigurieren Sie den Auftrag benutzerspezifisch "Kontinuierlich ausführen", damit bei der Anmeldung zum [Klassischen Azure-Portal] sollte die ungefähr wie folgt angezeigt:

    ![][4]


3. **EnterprisePushMobileApp**

    ein. Dies ist eine Windows Store-Anwendung, die Spruch Benachrichtigungen aus der WebJob als Teil der mobilen Back-End-ausgeführt wird, und blenden sie aus. [Benachrichtigung Hubs - Universal Windows Lernprogramm]basiert auf.  

    b. Stellen Sie sicher, dass die Anwendung Spruch Benachrichtigungen erhalten aktiviert ist.

    c. Sicherstellen, dass den folgenden Benachrichtigung untergeordneten Servern Registrierungscode bei der App aufgerufen wird starten (nach ersetzen die *HubName* und *DefaultListenSharedAccessSignature*:

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a>Beispiel ausführen:

1. Stellen Sie sicher, dass Ihre WebJob erfolgreich ausgeführt wird, und auf "Kontinuierlich ausführen" geplant.
2. Führen Sie die **EnterprisePushMobileApp** aus dem Windows Store-app gestartet werden kann.
3. Führen Sie die **EnterprisePushBackendSystem** Console-Anwendung wird die LoB Back-End-simulieren und Senden von Nachrichten beginnt, und es sollte Spruch Benachrichtigungen angezeigte wie die folgende angezeigt:

    ![][5]

4. Die Nachrichten wurden ursprünglich Dienstbus Themen, in denen Dienstbus Abonnements in Ihrem Auftrag Web überwacht wurde gesendet. Nachdem Sie eine Nachricht empfangen wurde, wurde zur mobile-app eine Benachrichtigung erstellt und gesendet. Sie können mit der WebJob Protokolle, die Verarbeitung zu bestätigen, wenn Sie den Link Protokolle [klassischen Azure] -Portal für den Job Web wechseln ansehen:

    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Benachrichtigung Hub Beispiele]: https://github.com/Azure/azure-notificationhubs-samples
[Azure Mobile Service]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure Dienstbus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Service Bus Pub/Sub-Programmierung.]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure WebJob]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/
[Benachrichtigung Hubs - Universal Windows-Lernprogramm]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Azure klassischen-Portal]: https://manage.windowsazure.com/
