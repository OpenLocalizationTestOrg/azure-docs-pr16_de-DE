## <a name="send-messages-to-event-hubs"></a>Senden von Nachrichten an Ereignis Hubs

In diesem Abschnitt werden Sie eine Windows-Console-app schreiben, die Ereignisse an Ihre Ereignis Hub sendet.

1. Erstellen Sie ein neues Visual C#-Desktop-App-Projekt mit der **Anwendung Console** -Projektvorlage in Visual Studio. Geben Sie dem Projekt **Absender**.

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp1.png)

2. Im Explorer-Lösung mit der rechten Maustaste in der Lösung, und klicken Sie dann auf **NuGet-Pakete verwalten, für die Lösung**. 

3. Klicken Sie auf die Registerkarte **Durchsuchen** , und suchen Sie nach `Microsoft Azure Service Bus`. Stellen Sie sicher, dass im Feld **Versionen** des Projektnamens (**Absender**) angegeben ist. Klicken Sie auf **Installieren**, und akzeptieren Sie der Vereinbarung. 

    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp2.png)

    Visual Studio-downloads, Installationen und fügt einen Verweis zum [Azure-Dienstbus Bibliothek NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus)hinzu.

4. Fügen Sie den folgenden `using` Anweisungen am oberen Rand der Datei **Program.cs** :

    ```
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Fügen Sie die folgenden Felder in der Klasse **Program** , ersetzen den Platzhalterwerte mit dem Namen der Ereignis-Hub, die Sie im vorherigen Abschnitt erstellt haben, und die Verbindungszeichenfolge mit Namespace Ebene, die Sie zuvor gespeichert haben.

    ```
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```

6. Fügen Sie die folgende Methode zur Klasse **Programm** aus:

    ```
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }

            Thread.Sleep(200);
        }
    }
    ```

    Diese Methode sendet Ereignisse kontinuierlich an Ihre Ereignis Verteiler mit einer 200-ms Verzögerung.

7. Fügen Sie schließlich die folgenden Zeilen für die **Main** -Methode:

    ```
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```
