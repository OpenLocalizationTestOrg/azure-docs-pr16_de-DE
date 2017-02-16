<properties 
    pageTitle="Dienstbus vermittelte messaging .NET Lernprogramm | Microsoft Azure"
    description="Vermittelten messaging .NET Lernprogramm."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-brokered-messaging-net-tutorial"></a>Vermittelte Dienstbus messaging .NET Lernprogramm

Azure Service dieses Bedienfeld enthält zwei umfassende messaging Lösungen – eine über einen zentralen "Relay" Dienst ausgeführt wird, in der Cloud, die eine Vielzahl von anderen Transportprotokolle und Web unterstützt services Standards, einschließlich SOAP, WS-*, und die restlichen. Der Client benötigt eine direkte Verbindung mit dem lokalen Dienst nicht noch muss er wissen, wo sich der Dienst befindet, und der lokalen Dienst benötigt keine eingehenden Ports in der Firewall geöffnet.

Die zweite messaging Lösung ermöglicht "vermittelten" messaging-Funktionen. Diese können betrachtet werden als asynchrone oder -Funktionen, die Unterstützung veröffentlichen-abonnieren, zeitliche Entkopplung und Szenarien mit der Dienstbus messaging-Infrastruktur für den Lastenausgleich abgekoppelt. Entkoppelter Kommunikation bietet viele Vorteile; Beispielsweise können Clients und Server Bedarf verbinden und deren Operationen in einer asynchronen Weise.

In diesem Lernprogramm soll Ihnen eine Übersicht und praktische Erfahrung mit Warteschlangen mitzuteilen, eine der die Hauptkomponenten von Dienstbus vermittelte messaging. Nachdem Sie die Reihenfolge der Themen in diesem Lernprogramm durcharbeiten, haben Sie eine Anwendung, die eine Liste der Nachrichten auffüllt, erstellt eine Warteschlange und sendet Nachrichten an diese Warteschlange. Schließlich die Anwendung empfängt zeigt die Nachrichten aus der Warteschlange an, und dann seine Ressourcen und Ausgänge bereinigt. Ein entsprechendes Lernprogramm, die beschreibt, wie Sie eine Anwendung zu erstellen, die die Relay Service Bus verwendet, finden Sie unter der [Dienstbus weitergeleitet messaging Lernprogramm](../service-bus-relay/service-bus-relay-tutorial.md).

## <a name="introduction-and-prerequisites"></a>Einführung und Komponenten

Warteschlangen bieten First In Nachrichtenübermittlung für einen oder mehrere verschiedenen Consumer ersten Out (FIFO). FIFO bedeutet, dass Nachrichten empfangen und von den Empfängern in der zeitliche Reihenfolge, in der Warteschlange e-Mails hatten und jede Nachricht wird empfangen und nur eine Nachricht Consumer verarbeiteten, verarbeitet werden in der Regel erwartet werden. Ein wesentlicher Vorteil der Verwendung von Warteschlangen ist der Anwendungskomponenten *zeitliche Entkopplung* erzielen: Zählung ausnehmen Herstellern und Nutzer müssen nicht senden und Empfangen von Nachrichten zur gleichen Zeit, da die Nachrichten in der Warteschlange als gespeichert werden. Ein verwandter Vorteil ist *Abgleich laden*, womit Hersteller und Verbraucher zu senden und Empfangen von Nachrichten mit unterschiedlichen Zinssätzen können.

Es folgen einige administrative und vorbereitende Schritte, denen, die Sie folgen sollten, bevor Sie das Lernprogramm beginnen. Die erste ist, erstellen einen Dienstnamespace und erhalten Sie einen freigegebene Access Signatur (SAS) Key. Ein Namespace stellt eine Begrenzungslinie Anwendung für jede Anwendung über Dienstbus zur Verfügung gestellt. Eine SAS-Taste wird automatisch vom System generiert, wenn ein Dienstnamespace erstellt wird. Die Kombination von Dienstnamespace und SAS-Taste bietet einen Eintrag mit dem Dienstbus Zugriff auf eine Anwendung authentifiziert.

### <a name="create-a-service-namespace-and-obtain-a-sas-key"></a>Erstellen Sie einen Dienstnamespace und erhalten Sie eine SAS-Taste

Dieser erste Schritt besteht, um einen Dienstnamespace erstellen, und klicken Sie in einer [Access-Signatur freigegeben](service-bus-sas-overview.md) (SAS) Schlüssel beziehen. Ein Namespace stellt eine Begrenzungslinie Anwendung für jede Anwendung über Dienstbus zur Verfügung gestellt. Eine SAS-Taste wird automatisch vom System generiert, wenn ein Dienstnamespace erstellt wird. Die Kombination von Dienstnamespace und SAS-Taste stellt einen Satz von Anmeldeinformationen für Dienstbus zum Authentifizieren des Zugriffs auf eine Anwendung.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Im nächsten Schritt wird zum Erstellen eines Projekts Visual Studio und Schreiben zwei Helper-Funktionen, die eine kommagetrennte Liste der Nachrichten in eine Stimme eingegebene [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) .NET [Liste](https://msdn.microsoft.com/library/6sh2ey19.aspx) zu laden.

### <a name="create-a-visual-studio-project"></a>Erstellen eines Visual Studio-Projekts

1. Öffnen Sie Visual Studio als Administrator an, indem Sie mit der rechten Maustaste in des Programms im Menü Start, und klicken Sie auf **als Administrator ausführen**.

1. Erstellen eines neuen Projekts der Console-Anwendung. Klicken Sie im Menü **Datei** auf **neu**, und klicken Sie auf **Projekt**. Klicken Sie im Dialogfeld **Neues Projekt** auf **Visual c#** (falls **Visual c#** nicht suchen unter **Andere Sprachen**angezeigt wird), klicken Sie auf die Vorlage **Console-Anwendung** , und nennen Sie es **QueueSample**. Verwenden Sie den standardmäßigen **Speicherort**. Klicken Sie auf **OK** , um das Projekt zu erstellen.

1. Verwenden Sie den NuGet-Paket-Manager die Dienstbus Bibliotheken zum Projekt hinzufügen:
    1. In Lösung Explorer mit der rechten Maustaste in des Projekts **QueueSample** , und klicken Sie dann auf **NuGet-Pakete verwalten**.
    2. Klicken Sie im Dialogfeld **Nuget-Pakete verwalten** klicken Sie auf die Registerkarte **Durchsuchen** , suchen Sie nach **Azure-Dienstbus**, und klicken Sie auf **Installieren**.
<br />
1. Explorer-Lösung Doppelklicken Sie auf die Datei Program.cs, um es im Visual Studio-Editor zu öffnen. Ändern des Namespacenamens aus den Standardnamen der `QueueSample` auf `Microsoft.ServiceBus.Samples`.

    ```
    Microsoft.ServiceBus.Samples
    {
        ...
    ```

1. Ändern der `using` Anweisungen, wie im folgenden Code dargestellt.

    ```
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;
    ```

1. Erstellen Sie eine Textdatei namens Data.csv, und Kopieren in den folgenden Text mit Kommas als Trennzeichen.

    ```
    IssueID,IssueTitle,CustomerID,CategoryID,SupportPackage,Priority,Severity,Resolved
    1,Package lost,1,1,Basic,5,1,FALSE
    2,Package damaged,1,1,Basic,5,1,FALSE
    3,Product defective,1,2,Premium,5,2,FALSE
    4,Product damaged,2,2,Premium,5,2,FALSE
    5,Package lost,2,2,Basic,5,2,TRUE
    6,Package lost,3,2,Basic,5,2,FALSE
    7,Package damaged,3,7,Premium,5,3,FALSE
    8,Product defective,3,2,Premium,5,3,FALSE
    9,Product damaged,4,6,Premium,5,3,TRUE
    10,Package lost,4,8,Basic,5,3,FALSE
    11,Package damaged,5,4,Basic,5,4,FALSE
    12,Product defective,5,4,Basic,5,4,FALSE
    13,Package lost,6,8,Basic,5,4,FALSE
    14,Package damaged,6,7,Premium,5,5,FALSE
    15,Product defective,6,2,Premium,5,5,FALSE
    ```

    Speichern und schließen Sie die Datei Data.csv, und Sie sich den Speicherort, in dem Sie gespeichert haben.

1. Explorer-Lösung, mit der Maustaste des Namens Ihres Projekts (in diesem Beispiel **QueueSample**), klicken Sie auf **Hinzufügen**, und klicken Sie auf **Vorhandenes Element**.

1. Navigieren Sie zu der Data.csv-Datei, die Sie in Schritt 6 erstellt haben. Klicken Sie auf Datei, und klicken Sie auf **Hinzufügen**. Stellen Sie sicher, * *Alle Dateien (*.*) ** wird in der Liste der Dateitypen ausgewählt.

### <a name="create-a-method-that-parses-a-list-of-messages"></a>Erstellen Sie eine Methode, die analysiert wird eine Liste der Nachrichten

1. In der `Program` Klasse vor der `Main()` Methode deklarieren Sie zwei Variablen: eine vom Typ **Datentabelle**, um die Liste der Nachrichten in Data.csv enthalten. Das andere sollten vom Typ Listenobjekt, Stimme zu [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)eingegeben haben. Letztere folgt eine Liste der vermittelten Nachrichten, die nachfolgenden Schritte in diesem Lernprogramm verwendet werden.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList;
    ```

1. Außen `Main()`, Definieren einer `ParseCSV()` Methode, die die Liste der Nachrichten in Data.csv analysiert und lädt die Nachrichten in einer Tabelle [Datentabelle](https://msdn.microsoft.com/library/azure/system.data.datatable.aspx) , wie hier dargestellt. **Die Methode gibt ein.**

    ```
    static DataTable ParseCSVFile()
    {
        DataTable tableIssues = new DataTable("Issues");
        string path = @"..\..\data.csv";
        try
        {
            using (StreamReader readFile = new StreamReader(path))
            {
                string line;
                string[] row;
    
                // create the columns
                line = readFile.ReadLine();
                foreach (string columnTitle in line.Split(','))
                {
                    tableIssues.Columns.Add(columnTitle);
                }
    
                while ((line = readFile.ReadLine()) != null)
                {
                    row = line.Split(',');
                    tableIssues.Rows.Add(row);
                }
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Error:" + e.ToString());
        }
    
        return tableIssues;
    }
    ```

1. In der `Main()` Methode, fügen Sie eine Anweisung, die Ruft die `ParseCSVFile()` Methode:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
    
    }
    ```

### <a name="create-a-method-that-loads-the-list-of-messages"></a>Erstellen Sie eine Methode, die die Liste der Nachrichten zu laden

1. Außen `Main()`, Definieren einer `GenerateMessages()` Methode, die das **Datentabelle** Objekt zurückgegebene `ParseCSVFile()` und lädt die Tabelle in eine Stimme eingegebene Liste von vermittelten Nachrichten. Die Methode gibt dann das **Liste** Objekt, wie im folgenden Beispiel gezeigt. 

    ```
    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
        // Instantiate the brokered list object
        List<BrokeredMessage> result = new List<BrokeredMessage>();
    
        // Iterate through the table and create a brokered message for each row
        foreach (DataRow item in issues.Rows)
        {
            BrokeredMessage message = new BrokeredMessage();
            foreach (DataColumn property in issues.Columns)
            {
                message.Properties.Add(property.ColumnName, item[property]);
            }
            result.Add(message);
        }
        return result;
    }
    ```

1. In `Main()`, direkt hinter den Anruf an `ParseCSVFile()`, fügen Sie eine Anweisung, die ruft der `GenerateMessages()` Methode der Rückgabewert aus `ParseCSVFile()` als Argument:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
    }
    ```

### <a name="obtain-user-credentials"></a>Benutzeranmeldeinformationen abrufen

1. Erstellen Sie zuerst die drei globalen Zeichenfolgenvariablen, um diese Werte enthalten. Diese Variablen direkt hinter der vorherigen Deklarationen von Variablen deklariert. Beispiel:

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        public class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList; 

            // Add these variables
            private static string ServiceNamespace;
            private static string sasKeyName = "RootManageSharedAccessKey";
            private static string sasKeyValue;
            …
    ```

1. Erstellen Sie dann eine Funktion, die akzeptiert und speichert die Dienstnamespace und SAS-Taste. Fügen Sie diese Methode außerhalb `Main()`. Beispiel: 

    ```
    static void CollectUserInput()
    {
        // User service namespace
        Console.Write("Please enter the namespace to use: ");
        ServiceNamespace = Console.ReadLine();
    
        // Issuer key
        Console.Write("Enter the SAS key to use: ");
        sasKeyValue = Console.ReadLine();
    }
    ```

1. In `Main()`, direkt hinter den Anruf an `GenerateMessages()`, fügen Sie eine Anweisung, die Ruft die `CollectUserInput()` Methode:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
        
        // Collect user input
        CollectUserInput();
    }
    ```

### <a name="build-the-solution"></a>Erstellen Sie die Lösung

Sie können in Visual Studio im Menü **Erstellen** klicken Sie auf die **Lösung erstellen** oder drücken Sie **STRG + UMSCHALT + B** , um die Genauigkeit der Ihre Arbeit bisher zu bestätigen.

## <a name="create-management-credentials"></a>Erstellen der Verwaltung von Anmeldeinformationen

In diesem Schritt definieren Sie die Management-Vorgänge, die mit der gemeinsamen Zugriff Anmeldeinformationen Signatur (SAS) erstellen, mit denen Ihrer Anwendung autorisiert werden muss.

1. Aus Gründen der Übersichtlichkeit platziert dieses Lernprogramms alle Vorgänge der Warteschlange in einer separaten Methode aus. Erstellen einer asynchronen `Queue()` Methode in der `Program` Klasse, nach der `Main()` Methode. Beispiel:
 
    ```
    public static void Main(string[] args)
    {
    …
    }
    static async Task Queue()
    {
    }
    ```

1. Im nächste Schritt besteht im Erstellen einer SAS Anmeldeinformationen mithilfe eines Objekts [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) . Die Erstellungsmethode akzeptiert der Name des Schlüssels SAS und Wert abgerufen der `CollectUserInput()` Methode. Fügen Sie den folgenden Code ein, um die `Queue()` Methode:

    ```
    static async Task Queue()
    {
        // Create management credentials
        TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
    }
    ```

2. Erstellen eines neuen Namespace Management-Objekts mit einem URI, enthält den Namen des Namespaces und die Management-Anmeldeinformationen, die Sie im vorherigen Schritt als Argumente abgerufen. Fügen Sie diesen Code direkt hinter den Code im vorherigen Schritt hinzugefügt. Denken Sie daran `<yourNamespace>` mit dem Namen des Namespaces Dienst:
    
    ```
    NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

### <a name="example"></a>Beispiel

An diesem Punkt sollte der Code etwa wie folgt aussehen:

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
  public class Program
  {
    private static DataTable issues;
    private static List<BrokeredMessage> MessageList;
    private static string ServiceNamespace;
    private static string sasKeyName = "RootManageSharedAccessKey";
    private static string sasKeyValue;

    static void Main(string[] args)
    {
      // Populate test data
      issues = ParseCSVFile();
      MessageList = GenerateMessages(issues);

      // Collect user input
      CollectUserInput();

      // Add this call
      Task.WaitAll(Queue());
    }

    static async Task Queue()
    {
      // Create management credentials
      TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
      NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    }

    static DataTable ParseCSVFile()
    {
      DataTable tableIssues = new DataTable("Issues");
      string path = @"..\..\data.csv";
      try
      {
        using (StreamReader readFile = new StreamReader(path))
        {
          string line;
          string[] row;

          // create the columns
          line = readFile.ReadLine();
          foreach (string columnTitle in line.Split(','))
          {
            tableIssues.Columns.Add(columnTitle);
          }

          while ((line = readFile.ReadLine()) != null)
          {
            row = line.Split(',');
            tableIssues.Rows.Add(row);
          }
        }
      }
      catch (Exception e)
      {
        Console.WriteLine("Error:" + e.ToString());
      }

      return tableIssues;
    }

    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
      // Instantiate the brokered list object
      List<BrokeredMessage> result = new List<BrokeredMessage>();

      // Iterate through the table and create a brokered message for each row
      foreach (DataRow item in issues.Rows)
      {
        BrokeredMessage message = new BrokeredMessage();
        foreach (DataColumn property in issues.Columns)
        {
          message.Properties.Add(property.ColumnName, item[property]);
        }
        result.Add(message);
      }
      return result;
    }

    static void CollectUserInput()
    {
      // User service namespace
      Console.Write("Please enter the service namespace to use: ");
      ServiceNamespace = Console.ReadLine();

      // Issuer key
      Console.Write("Please enter the issuer key to use: ");
      sasKeyValue = Console.ReadLine();
    }
  }
}
```

## <a name="send-messages-to-the-queue"></a>Senden von Nachrichten an die Warteschlange

In diesem Schritt stellen Sie eine Warteschlange erstellen und dann die in der Liste der Nachrichten, die an diese Warteschlange vermittelten enthaltenen Nachrichten zu senden.

### <a name="create-queue-and-send-messages-to-the-queue"></a>Warteschlange erstellen und Senden von Nachrichten an die Warteschlange

1. Erstellen Sie zuerst die Warteschlange ein. Rufen Sie ihn beispielsweise `myQueue`, und deklarieren Sie sie direkt hinter der Management-Vorgänge, die Sie hinzugefügt, in haben der `Queue()` Methode im letzten Schritt:

    ```
    QueueDescription myQueue;

    if (namespaceClient.QueueExists("IssueTrackingQueue"))
    {
        namespaceClient.DeleteQueue("IssueTrackingQueue");
    }

    myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
    ```

1. In der `Queue()` Methode, erstellen Sie ein messaging Factoryobjekt mit einem neu erstellten Service Bus URI als Argument. Fügen Sie den folgenden Code direkt nach der Management-Vorgänge, die Sie im letzten Schritt hinzugefügt. Denken Sie daran `<yourNamespace>` mit dem Namen des Namespaces Dienst:

    ```
    MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

1. Erstellen Sie anschließend das Warteschlangenobjekt mithilfe der [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) -Klasse. Fügen Sie den folgenden Code direkt nach den Code, die, den Sie im letzten Schritt hinzugefügt:

    ```
    QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");
    ```

1. Fügen Sie anschließend Code, die durch die Liste der vermittelten Nachrichten Schleifen zuvor erstellten jede an die Warteschlange senden. Fügen Sie den folgenden Code direkt nach der `CreateQueueClient()` Anweisung im vorherigen Schritt:
    
    ```
    // Send messages
    Console.WriteLine("Now sending messages to the queue.");
    for (int count = 0; count < 6; count++)
    {
        var issue = MessageList[count];
        issue.Label = issue.Properties["IssueTitle"].ToString();
        await myQueueClient.SendAsync(issue);
        Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
    }
    ```

## <a name="receive-messages-from-the-queue"></a>Empfangen Sie Nachrichten in der Warteschlange.

In diesem Schritt rufen Sie die Liste der Nachrichten aus der Warteschlange, die Sie im vorherigen Schritt erstellt haben.

### <a name="create-a-receiver-and-receive-messages-from-the-queue"></a>Erstellen eines Empfängers und Empfangen von Nachrichten aus der Warteschlange

In der `Queue()` Methode der Warteschlange durchlaufen und mithilfe der [QueueClient.ReceiveAsync](https://msdn.microsoft.com/library/azure/dn130423.aspx) -Methode, jede Nachricht an die Konsole ausgeben empfangen. Fügen Sie den folgenden Code direkt nach der Code, den Sie im vorherigen Schritt hinzugefügt haben:

```
Console.WriteLine("Now receiving messages from Queue.");
BrokeredMessage message;
while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();
    
        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Beachten Sie, dass die `Thread.Sleep` wird nur verwendet, um die Nachricht zu reproduzieren Verarbeitung und Sie wahrscheinlich würde nicht benötigt in einer realen e-Mail-Programm.

### <a name="end-the-queue-method-and-clean-up-resources"></a>Beenden der Warteschlange-Methode und Ressourcen bereinigen

Direkt nach dem vorherigen Code fügen Sie den folgenden Code, um die Nachricht Factory und Warteschlange Ressourcen zu bereinigen hinzu:

```
factory.Close();
myQueueClient.Close();
namespaceClient.DeleteQueue("IssueTrackingQueue");
```

### <a name="call-the-queue-method"></a>Rufen Sie die Warteschlange-Methode

Der letzte Schritt darin ist eine Anweisung hinzufügen, die Ruft die `Queue()` Methode aus `Main()`. Fügen Sie die folgende hervorgehobene Zeile des Codes am Ende des Main() hinzu:
    
```
public static void Main(string[] args)
{
    // Collect user input
    CollectUserInput();
    
    // Populate test data
    issues = ParseCSVFile();
    MessageList = GenerateMessages(issues);
    
    // Add this call
    Queue();
}
```

### <a name="example"></a>Beispiel

Im folgende Code enthält die vollständige **QueueSample** -Anwendung.

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
    public class Program
    {
        private static DataTable issues;
        private static List<BrokeredMessage> MessageList;

        // Add these variables
        private static string ServiceNamespace;
        private static string sasKeyName = "RootManageSharedAccessKey";
        private static string sasKeyValue;

        static void Main(string[] args)
        {

            // Populate test data
            issues = ParseCSVFile();
            MessageList = GenerateMessages(issues);

            // Collect user input
            CollectUserInput();

            Queue();

        }

        static async Task Queue()
        {
            // Create management credentials
            TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
            NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueDescription myQueue;

            if (namespaceClient.QueueExists("IssueTrackingQueue"))
            {
                namespaceClient.DeleteQueue("IssueTrackingQueue");
            }
            
            myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
            
            MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");

            // Send messages
            Console.WriteLine("Now sending messages to the queue.");
            for (int count = 0; count < 6; count++)
            {
                var issue = MessageList[count];
                issue.Label = issue.Properties["IssueTitle"].ToString();
                await myQueueClient.SendAsync(issue);
                Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
            }

            Console.WriteLine("Now receiving messages from Queue.");
            BrokeredMessage message;
            while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
            {
                Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
                message.Complete();

                Console.WriteLine("Processing message (sleeping...)");
                Thread.Sleep(1000);
            }

            factory.Close();
            myQueueClient.Close();
            namespaceClient.DeleteQueue("IssueTrackingQueue");


        }

        static void CollectUserInput()
        {
            // User service namespace
            Console.Write("Please enter the namespace to use: ");
            ServiceNamespace = Console.ReadLine();

            // Issuer key
            Console.Write("Enter the SAS key to use: ");
            sasKeyValue = Console.ReadLine();
        }

        static List<BrokeredMessage> GenerateMessages(DataTable issues)
        {
            // Instantiate the brokered list object
            List<BrokeredMessage> result = new List<BrokeredMessage>();

            // Iterate through the table and create a brokered message for each row
            foreach (DataRow item in issues.Rows)
            {
                BrokeredMessage message = new BrokeredMessage();
                foreach (DataColumn property in issues.Columns)
                {
                    message.Properties.Add(property.ColumnName, item[property]);
                }
                result.Add(message);
            }
            return result;
        }

        static DataTable ParseCSVFile()
        {
            DataTable tableIssues = new DataTable("Issues");
            string path = @"..\..\data.csv";
            try
            {
                using (StreamReader readFile = new StreamReader(path))
                {
                    string line;
                    string[] row;

                    // create the columns
                    line = readFile.ReadLine();
                    foreach (string columnTitle in line.Split(','))
                    {
                        tableIssues.Columns.Add(columnTitle);
                    }

                    while ((line = readFile.ReadLine()) != null)
                    {
                        row = line.Split(',');
                        tableIssues.Rows.Add(row);
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error:" + e.ToString());
            }

            return tableIssues;
        }
    }
}
```

## <a name="build-and-run-the-queuesample-application"></a>Erstellen Sie, und führen Sie die Anwendung QueueSample

Jetzt, da Sie die vorherigen Schritte abgeschlossen haben, können Sie erstellen, und führen Sie die Anwendung **QueueSample** .

### <a name="build-the-queuesample-application"></a>Erstellen Sie die Anwendung QueueSample

Klicken Sie in Visual Studio über das Menü **Erstellen** auf die **Lösung erstellen**, oder drücken Sie **STRG + UMSCHALT + B**. Wenn Fehler auftreten, stellen Sie sicher, dass der Code korrekt ist basierend auf das vollständige Beispiel am Ende der im vorhergehenden Schritt.

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm gezeigt, wie eine Dienstbus Clientbuild Anwendung und -Diensts mithilfe der Dienstbus vermittelte messaging-Funktionen. Eine ähnliche Lernprogramm, in dem Dienstbus [Relay](service-bus-messaging-overview.md#Relayed-messaging)verwendet wird, finden Sie unter der [Dienstbus weitergeleitet messaging Lernprogramm](../service-bus-relay/service-bus-relay-tutorial.md).

Weitere Informationen zu [Dienstbus](https://azure.microsoft.com/services/service-bus/)finden Sie unter den folgenden Themen.

- [Übersicht über Dienstbus messaging](service-bus-messaging-overview.md)
- [Dienstbus-Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
- [Dienst Busarchitektur](service-bus-architecture.md)

