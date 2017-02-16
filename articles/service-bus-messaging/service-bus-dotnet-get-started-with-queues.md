<properties
    pageTitle="Erste Schritte mit Servicebuswarteschlangen | Microsoft Azure"
    description="So schreiben Sie eine C#-Console-Anwendung für messaging Dienstbus"
    services="service-bus"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="hero-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/23/2016"
    ms.author="jotaub;sethm"/>

# <a name="get-started-with-service-bus-queues"></a>Erste Schritte mit Servicebuswarteschlangen

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a>Was realisiert werden sollen

In diesem Lernprogramm werden wir die folgenden Schritte ausführen:

1. Erstellen Sie einen Dienstbus Namespace, mit dem Azure-Portal an.

2. Erstellen einer Warteschlange Service Bus Messaging, über das Azure-Portal an.

3. Schreiben Sie eine Console-Anwendung zum Senden einer Nachricht an.

4. Schreiben Sie eine Console-Anwendung zum Empfangen von Nachrichten an.

## <a name="prerequisites"></a>Erforderliche Komponenten

1. [Visual Studio-2013 oder Visual Studio 2015](http://www.visualstudio.com). In den Beispielen in diesem Lernprogramm verwenden Sie Visual Studio 2015.

2. Ein Azure-Abonnement.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. erstellen Sie 1. einen Namespace mit dem Azure-portal

Wenn Sie bereits einen Dienstbus Namespace erstellt haben, wechseln Sie zu im Abschnitt [Erstellen einer Warteschlange mit dem Azure-Portal](#2-create-a-queue-using-the-azure-portal) .

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-the-azure-portal"></a>2. Erstellen einer Warteschlange, indem die Azure-portal

Wenn Sie bereits eine Dienstbus Warteschlange erstellt haben, wechseln Sie zum [Senden von Nachrichten an die Warteschlange](#3-send-messages-to-the-queue) Abschnitt.

[AZURE.INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-to-the-queue"></a>3 Senden von Nachrichten in der Warteschlange

Zum Senden von Nachrichten in der Warteschlange werden wir eine c# Console-Anwendung mit Visual Studio schreiben.

### <a name="create-a-console-application"></a>Erstellen Sie eine

1. Starten Sie Visual Studio und erstellen Sie eine neue.

### <a name="add-the-service-bus-nuget-package"></a>Hinzufügen des Dienst Bus NuGet-Pakets

1. Mit der rechten Maustaste in des neue Projekts ein, und wählen Sie **NuGet-Pakete verwalten**.

2. Klicken Sie auf die Registerkarte **Durchsuchen** , und suchen Sie nach "Microsoft Azure-Dienstbus" und wählen Sie den Artikel **Microsoft Azure-Dienstbus** . Klicken Sie auf **Installieren** , um die Installation durchzuführen, und klicken Sie dann schließen Sie in diesem Dialogfeld zu.

    ![Wählen Sie ein NuGet-Paket aus.][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-queue"></a>Schreiben Sie Teil des Codes zum Senden einer Nachricht an die Warteschlange

1. Fügen Sie die folgende Anweisung an den Anfang der Datei Program.cs verwenden.

    ```
    using Microsoft.ServiceBus.Messaging;
    ```
    
2. Fügen Sie den folgenden Code ein, um die `Main` Methode, die **ConnectionString** Variable als der Verbindungszeichenfolge zurück, das beim Erstellen des Namespaces abgerufen und zum Festlegen von **QueueName** als den Namen der Warteschlange, die beim Erstellen der Warteschlange verwendet.

    ```
    var connectionString = "<Your connection string>";
    var queueName = "<Your queue name>";
  
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");
    client.Send(message);
    ```

    Hier wird der Program.cs aussehen sollte.

    ```
    using System;
    using Microsoft.ServiceBus.Messaging;

    namespace GettingStartedWithQueues
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "<Your connection string>";
                var queueName = "<Your queue name>";

                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");

                client.Send(message);
            }
        }
    }
    ```
  
3. Führen Sie das Programm, und aktivieren Sie im Azure-Portal. Klicken Sie auf den Namen der Warteschlange in das Namespace **Übersicht** Blade. Beachten Sie, dass der **aktive Nachricht zählen** Wert 1 jetzt werden soll.
    
      ![Anzahl der Nachrichten][queue-message]
    
## <a name="4-receive-messages-from-the-queue"></a>4 empfangen von Nachrichten aus der Warteschlange

1. Erstellen einer neuen Console-Anwendung, und fügen Sie einen Verweis auf das Paket Service Bus NuGet, ähnlich wie der vorherige sendenden Anwendung.

2. Fügen Sie den folgenden `using` -Anweisung an den Anfang der Datei Program.cs.
  
    ```
    using Microsoft.ServiceBus.Messaging;
    ```
  
3. Fügen Sie den folgenden Code ein, um die `Main` Methode, die **ConnectionString** Variable als der Verbindungszeichenfolge zurück, das beim Erstellen des Namespaces abgerufen und zum Festlegen von **QueueName** als den Namen der Warteschlange, die Sie beim Erstellen der Warteschlange verwendet.

    ```
    var connectionString = "";
    var queueName = "samplequeue";
  
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
  
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
  
    Console.ReadLine();
    ```

    Hier wird die Datei Program.cs aussehen sollte:

    ```
    using System;
    using Microsoft.ServiceBus.Messaging;
  
    namespace GettingStartedWithQueues
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "";
          var queueName = "samplequeue";
  
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
  
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });
  
          Console.ReadLine();
        }
      }
    }
    ```
  
4. Führen Sie das Programm, und aktivieren Sie im Portal. Beachten Sie, dass der **Länge der Warteschlange** -Wert 0 jetzt werden soll.

    ![Länge der Warteschlange][queue-message-receive]
  
Herzlichen Glückwunsch! Sie haben nun eine Warteschlange, die eine Nachricht erstellt und eine Nachricht erhalten.

## <a name="next-steps"></a>Nächste Schritte

Schauen Sie sich unsere [GitHub Repository mit Beispielen](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) , die veranschaulichen einige der erweiterten Features von Azure-Dienstbus messaging.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png


<!--Reference style links - using these makes the source content way more readable than using inline links-->

[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples