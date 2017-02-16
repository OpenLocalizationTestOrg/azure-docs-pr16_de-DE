<properties
    pageTitle="Azure Benachrichtigung Hubs Secure Pushbenachrichtigungen"
    description="Informationen Sie zum Senden von sicheren Pushbenachrichtigungen in Azure. In c# mithilfe der .NET API geschriebenen Codebeispielen."
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs" 
    ms.workload="mobile"
    ms.tgt_pltfrm="windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Azure Benachrichtigung Hubs Secure Pushbenachrichtigungen

> [AZURE.SELECTOR]
- [Windows-Dienst](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>(Übersicht)

Pushbenachrichtigungen Benachrichtigung Unterstützung in Microsoft Azure ermöglicht Ihnen eine Infrastruktur einfach zu verwendendes, für mehrere Plattformen, skalierten Pushbenachrichtigungen Zugriff auf, die um die Implementierung von Pushbenachrichtigungen für Consumer und Enterprise Applications für mobile Plattformen erheblich zu vereinfachen.

Aufgrund von behördliche oder Einschränkungen Sicherheit, manchmal Anwendung möglicherweise etwas in der Benachrichtigung einschließen möchten, die über die Standardansicht Pushbenachrichtigungen Benachrichtigungsinfrastruktur übermittelt werden können. In diesem Lernprogramm beschrieben, wie die gleiche Oberfläche zu erzielen, indem Sie vertraulichen Informationen über eine sichere, authentifizierte Verbindung zwischen dem Client-Gerät und der app Back-End-senden.

Ist der Fluss auf hoher Ebene wie folgt:

1. Die app-Back-End:
    - Stores secure Nutzlast in die Back-End-Datenbank.
    - Sendet die ID dieser Benachrichtigung am Gerät (keine sichere Informationen gesendet).
2. Die app auf dem Gerät, wenn Sie die Benachrichtigung empfangen:
    - Das Gerät Kontakte die Back-End-secure Nutzlast anfordern.
    - Die app kann die Nutzlast als eine Benachrichtigung auf dem Gerät anzeigen.

Es ist wichtig, beachten Sie, dass im vorhergehenden Fluss (und in diesem Lernprogramm) davon ausgegangen, dass das Gerät eine Authentifizierungstoken im lokalen Speicher, speichert nach der Anmeldung des Benutzers. Dadurch wird eine vollständig nahtlose zur Verfügung steht, sichergestellt, da das Gerät die Benachrichtigung des secure Payload mithilfe dieses Token abrufen kann. Wenn eine Anwendung auf dem Gerät nicht Authentifizierungstoken speichert, oder diese Token abgelaufen sein können, sollte die app Gerät nach Erhalt der Benachrichtigung eine generische Benachrichtigung Bestätigung durch den Benutzer die app gestartet angezeigt werden. Die app, klicken Sie dann authentifiziert den Benutzer und zeigt die Benachrichtigung Nutzlast.

In diesem Lernprogramm Secure Pushbenachrichtigungen veranschaulicht, wie eine Pushbenachrichtigung sicher zu senden. Das Lernprogramm erstellt auf das Lernprogramm [Benutzer benachrichtigen](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) , damit Sie die Schritte in diesem Lernprogramm zuerst ausführen soll.

> [AZURE.NOTE] In diesem Lernprogramm wird davon ausgegangen, dass Sie erstellt und Ihre Benachrichtigung Hub wie in [Erste Schritte mit Benachrichtigung Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)beschrieben konfiguriert haben.
Beachten Sie, dass Windows Phone 8.1 Windows (nicht Windows Phone)-Anmeldeinformationen erforderlich sind und nicht funktionieren auf Windows Phone 8.0 oder Silverlight 8.1 Hintergrundaufgaben. Für Windows Store-Apps können Sie Benachrichtigungen über einen Hintergrund Vorgang erhalten, nur, wenn die app Sperrbildschirm aktiviert ist (klicken Sie auf das Kontrollkästchen in der Appmanifest).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>Ändern Sie das Windows Phone-Projekt

1. Fügen Sie den folgenden Code in das Projekt **NotifyUserWindowsPhone** in App.xaml.cs, um die Aufgabe Pushbenachrichtigungen Hintergrund zu registrieren. Fügen Sie die folgende Zeile des Codes am Ende der `OnLaunched()` Methode:

        RegisterBackgroundTask();

2. Immer noch in App.xaml.cs, fügen Sie den folgenden code unmittelbar nach der `OnLaunched()` Methode:

        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();

                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }

3. Fügen Sie den folgenden `using` Anweisungen am oberen Rand der App.xaml.cs-Datei:

        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;

4. Klicken Sie in Visual Studio im Menü **Datei** auf **Alles speichern**.

## <a name="create-the-push-background-component"></a>Erstellen Sie die Komponente Pushbenachrichtigungen Hintergrund

Im nächsten Schritt wird zum Erstellen der Pushbenachrichtigungen Hintergrund Komponente.

1. Im Lösung-Explorer mit der rechten Maustaste in den Knotens des obersten Ebene der Lösung (in diesem Fall**Lösung SecurePush** ), klicken Sie dann klicken Sie auf **Hinzufügen**, und klicken Sie auf **Neues Projekt**.

2. Erweitern Sie **Store-Apps**, dann klicken Sie auf **Windows Phone Apps**, dann klicken Sie auf **Windows-Runtime-Komponente (Windows Phone)**. Nennen Sie das Projekt **PushBackgroundComponent**, und klicken Sie dann auf **OK** , um das Projekt zu erstellen.

    ![][12]

3. Im Lösung-Explorer mit der rechten Maustaste in des Projekts **PushBackgroundComponent (Windows Phone 8.1)** , dann klicken Sie auf **Hinzufügen**, dann klicken Sie auf **Klasse**. Benennen Sie die neue Klasse **PushBackgroundTask.cs**. Klicken Sie auf **Hinzufügen** , um die Klasse zu generieren.

4. Ersetzen Sie den gesamten Inhalt der Definition **PushBackgroundComponent** Namespace mit den folgenden Code, und ersetzen den Platzhalter `{back-end endpoint}` mit dem Back-End-Endpunkt beim Bereitstellen von Ihrem Back-End abgerufen:

        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }

            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";

                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store the content received from the notification so it can be retrieved from the UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;

                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);

                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);

                    ShowToast(notification);

                    deferral.Complete();
                }

                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }

5. In Lösung Explorer mit der rechten Maustaste in des Projekts **PushBackgroundComponent (Windows Phone 8.1)** , und klicken Sie dann auf **NuGet-Pakete verwalten**.

6. Klicken Sie auf der linken Seite auf **Online**.

7. Geben Sie in **das Suchfeld** ein **HTTP-Client**.

8. Klicken Sie in der Ergebnisliste klicken Sie auf **Microsoft HTTP-Client-Bibliotheken**, und klicken Sie dann auf **Installieren**. Führen Sie die Installation.

9. Geben Sie **Json.net**zurück in das Feld NuGet **Suchen** . Installieren Sie das Paket **Json.NET** und dann schließen Sie das Fenster NuGet-Paket-Manager.

10. Fügen Sie den folgenden `using` Anweisungen am oberen Rand der **PushBackgroundTask.cs** -Datei:

        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;

11. Klicken Sie im Explorer-Lösung im Projekt **NotifyUserWindowsPhone (Windows Phone 8.1)** mit der rechten Maustaste **Verweise**und dann auf **Verweis hinzufügen**. Klicken Sie im Dialogfeld Verweis-Manager aktivieren Sie das Kontrollkästchen neben **PushBackgroundComponent**, und klicken Sie dann auf **OK**.

12. Explorer-Lösung Doppelklicken Sie auf **Package.appxmanifest** im Projekt **NotifyUserWindowsPhone (Windows Phone 8.1)** . Klicken Sie unter **Benachrichtigungen**legen Sie **Spruch in** auf **Ja**.

    ![][3]

13. **Package.appxmanifest**klicken Sie im Menü **Deklarationen** im oberen Bereich. Klicken Sie in der Dropdownliste **Verfügbare Deklarationen** **Hintergrundaufgaben**klicken Sie auf, und klicken Sie dann auf **Hinzufügen**.

14. Aktivieren Sie **Package.appxmanifest**unter **Eigenschaften** **Drücken Sie die Benachrichtigung**.

15. Geben Sie **PushBackgroundComponent.PushBackgroundTask** im Feld **Einstiegspunkt** **Package.appxmanifest**, klicken Sie unter **Einstellungen für die App**.

    ![][13]

16. Klicken Sie im Menü **Datei** auf **Alles speichern**.

## <a name="run-the-application"></a>Führen Sie die Anwendung

Wenn Sie die Anwendung ausführen zu können, führen Sie folgende Schritte aus:

1. Führen Sie in Visual Studio **AppBackend** Web-API Anwendung. Eine ASP.NET-Webseite wird angezeigt.

2. Führen Sie die **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone-app in Visual Studio. Windows Phone-Emulator ausgeführt wird und die app automatisch geladen.

3. Geben Sie in der app **NotifyUserWindowsPhone** Benutzeroberfläche einen Benutzernamen und ein Kennwort ein. Dabei kann es sich um eine beliebige Zeichenfolge, aber sie müssen den gleichen Wert sein.

4. Klicken Sie in der app **NotifyUserWindowsPhone** Benutzeroberfläche auf **anmelden und registriert**. Klicken Sie dann auf **Pushbenachrichtigungen zu senden**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
