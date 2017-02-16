<properties 
    pageTitle="Dienstbus vermittelte messaging REST Lernprogramm | Microsoft Azure"
    description="Vermittelten messaging REST Lernprogramm."
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

# <a name="service-bus-brokered-messaging-rest-tutorial"></a>Vermittelte Dienstbus messaging REST Lernprogramm

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

In diesem Lernprogramm erfahren, wie eine einfache REST-basierten Azure-Dienstbus Warteschlange und Thema/Abonnement zu erstellen.

## <a name="create-a-namespace"></a>Erstellen Sie einen namespace

Dieser erste Schritt besteht, um einen Dienstnamespace erstellen, und klicken Sie in einer [Access-Signatur freigegeben](service-bus-sas-overview.md) (SAS) Schlüssel beziehen. Ein Namespace stellt eine Begrenzungslinie Anwendung für jede Anwendung über Dienstbus zur Verfügung gestellt. Eine SAS-Taste wird automatisch vom System generiert, wenn ein Dienstnamespace erstellt wird. Die Kombination von Dienstnamespace und SAS-Taste stellt einen Satz von Anmeldeinformationen für Dienstbus zum Authentifizieren des Zugriffs auf eine Anwendung.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-console-client"></a>Erstellen eines Console-Clients

Servicebuswarteschlangen können Sie Nachrichten in einer Warteschlange First in, First Out zu speichern. Themen und Abonnements implementieren ein Muster veröffentlichen/abonnieren. Erstellen ein Thema und erstellen Sie dann eine oder mehrere Abonnements, die mit diesem Thema verbundenes. Wenn Nachrichten mit dem Thema gesendet werden, werden sie sofort auf die Abonnenten dieses Themas gesendet.

Der Code in diesem Lernprogramm führt Folgendes aus.

- Verwendet einen Namespace sowie [Freigegebene Access Signatur](service-bus-sas-overview.md) (SAS) Schlüssel für den Zugriff auf Ihre Dienstbus Namespace Ressourcen.

- Erstellt eine Warteschlange, sendet eine Nachricht an die Warteschlange und liest die Meldung in der Warteschlange.

- Erstellt ein Thema, ein Abonnement für das Thema, und sendet die Nachricht aus dem Abonnement liest.

- Ruft alle Warteschlange, Thema und Abonnementinformationen, einschließlich Abonnementregeln für von Dienstbus ab.

- Löscht die Warteschlange, Thema und Abonnement Ressourcen.

Da der Dienst einen REST-Stil-Webdienst, sind es keine speziellen Typen verbindet, während die gesamte Exchange Zeichenfolgen umfasst. Dies bedeutet, dass Visual Studio-Projekt nicht keine Bibliotheken Dienstbus verwiesen werden muss.

Nach dem Erhalt den Namespace und die Anmeldeinformationen im ersten Schritt können erstellen Sie als Nächstes eine grundlegende Visual Studio Console-Anwendung.

### <a name="create-a-console-application"></a>Erstellen Sie eine

1. Starten Sie Visual Studio als Administrator, indem Sie mit der rechten Maustaste in des Programms im Menü **Start** , und klicken Sie auf **als Administrator ausführen**.

1. Erstellen eines neuen Projekts der Console-Anwendung. Klicken Sie im Menü **Datei** auf, klicken Sie auf **neu**, und klicken Sie auf **Projekt**. Klicken Sie im Dialogfeld **Neues Projekt** auf **Visual c#** (falls **Visual c#** nicht suchen unter **Andere Sprachen**angezeigt wird), wählen Sie die Vorlage **Console-Anwendung** , und nennen Sie es **Microsoft.ServiceBus.Samples**. Verwenden Sie den standardmäßigen Speicherort. Klicken Sie auf **OK** , um das Projekt zu erstellen.

1. Vergewissern Sie sich in Program.cs, Ihre `using` Anweisungen wie folgt aussehen:

    ```
    using System;
    using System.Globalization;
    using System.IO;
    using System.Net;
    using System.Security.Cryptography;
    using System.Text;
    using System.Xml;
    ```

1. Falls erforderlich, benennen Sie den Namespace für das Programm aus der Visual Studio wird standardmäßig `Microsoft.ServiceBus.Samples`.

1. Innerhalb der `Program` Klasse, fügen Sie die folgenden globalen Variablen hinzu:
    
    ```
    static string serviceNamespace;
    static string baseAddress;
    static string token;
    const string sbHostName = "servicebus.windows.net";
    ```

1. Innerhalb `Main()`, fügen Sie den folgenden Code:

    ```
    Console.Write("Enter your service namespace: ");
    serviceNamespace = Console.ReadLine();
    
    Console.Write("Enter your SAS key: ");
    string SASKey = Console.ReadLine();
    
    baseAddress = "https://" + serviceNamespace + "." + sbHostName + "/";
    try
    {
        token = GetSASToken("RootManageSharedAccessKey", SASKey);
    
        string queueName = "Queue" + Guid.NewGuid().ToString();
    
        // Create and put a message in the queue
        CreateQueue(queueName, token);
        SendMessage(queueName, "msg1");
        string msg = ReceiveAndDeleteMessage(queueName);
    
        string topicName = "Topic" + Guid.NewGuid().ToString();
        string subscriptionName = "Subscription" + Guid.NewGuid().ToString();
        CreateTopic(topicName);
        CreateSubscription(topicName, subscriptionName);
        SendMessage(topicName, "msg2");
    
        Console.WriteLine(ReceiveAndDeleteMessage(topicName + "/Subscriptions/" + subscriptionName));
    
        // Get an Atom feed with all the queues in the namespace
        Console.WriteLine(GetResources("$Resources/Queues"));
    
        // Get an Atom feed with all the topics in the namespace
        Console.WriteLine(GetResources("$Resources/Topics"));
    
        // Get an Atom feed with all the subscriptions for the topic we just created
        Console.WriteLine(GetResources(topicName + "/Subscriptions"));
    
        // Get an Atom feed with all the rules for the topic and subscription we just created
        Console.WriteLine(GetResources(topicName + "/Subscriptions/" + subscriptionName + "/Rules"));
    
        // Delete the queue we created
        DeleteResource(queueName);
    
        // Delete the topic we created
        DeleteResource(topicName);
    
        // Get an Atom feed with all the topics in the namespace, it shouldn't have the one we created now
        Console.WriteLine(GetResources("$Resources/Topics"));
    
        // Get an Atom feed with all the queues in the namespace, it shouldn't have the one we created now
        Console.WriteLine(GetResources("$Resources/Queues"));
    }
    catch (WebException we)
    {
        using (HttpWebResponse response = we.Response as HttpWebResponse)
        {
            if (response != null)
            {
                Console.WriteLine(new StreamReader(response.GetResponseStream()).ReadToEnd());
            }
            else
            {
                Console.WriteLine(we.ToString());
            }
        }
    }
    
    Console.WriteLine("\nPress ENTER to exit.");
    Console.ReadLine();
    ```

## <a name="create-management-credentials"></a>Erstellen der Verwaltung von Anmeldeinformationen

Im nächsten Schritt wird, Schreiben einer Methode, die den Namespace und SAS-Taste verarbeitet, die Sie im vorherigen Schritt eingegeben haben, und ein SAS Token gibt. Dieses Beispiel erstellt ein SAS Token, die für eine Stunde gültig ist.

### <a name="create-a-getsastoken-method"></a>Erstellen Sie eine GetSASToken()-Methode

Fügen Sie den folgenden Code nach der `Main()` Methode, in der `Program` Klasse:

```
private static string GetSASToken(string SASKeyName, string SASKeyValue)
{
  TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
  string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + 3600);
  string stringToSign = WebUtility.UrlEncode(baseAddress) + "\n" + expiry;
  HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(SASKeyValue));

  string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
  string sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}",
      WebUtility.UrlEncode(baseAddress), WebUtility.UrlEncode(signature), expiry, SASKeyName);
  return sasToken;
}
```
## <a name="create-the-queue"></a>Erstellen der Warteschlange

Im nächsten Schritt wird eine Methode schreiben, die den REST-Schreibweise HTTP setzen Befehl zum Erstellen einer Warteschlange verwendet.

Fügen Sie die folgenden direkt hinter Code der `GetSASToken()` Code, die Sie im vorherigen Schritt hinzugefügt:

```
// Uses HTTP PUT to create the queue
private static string CreateQueue(string queueName, string token)
{
    // Create the URI of the new queue, note that this uses the HTTPS scheme
    string queueAddress = baseAddress + queueName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating queue {0}", queueAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                          <title type=""text"">" + queueName + @"</title>
                          <content type=""application/xml"">
                            <QueueDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                          </content>
                        </entry>";

    byte[] response = webClient.UploadData(queueAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

## <a name="send-a-message-to-the-queue"></a>Senden einer Nachricht an die Warteschlange

In diesem Schritt fügen Sie eine Methode, die den REST-Schreibweise HTTP POST-Befehl zum Senden einer Nachricht an die Warteschlange, die Sie im vorherigen Schritt erstellt haben.

1. Fügen Sie die folgenden direkt hinter Code der `CreateQueue()` Code, die Sie im vorherigen Schritt hinzugefügt haben:

    ```
    // Sends a message to the "queueName" queue, given the name and the value to enqueue
    // Uses an HTTP POST request.
    private static void SendMessage(string queueName, string body)
    {
        string fullAddress = baseAddress + queueName + "/messages" + "?timeout=60&api-version=2013-08 ";
        Console.WriteLine("\nSending message {0} - to address {1}", body, fullAddress);
        WebClient webClient = new WebClient();
        webClient.Headers[HttpRequestHeader.Authorization] = token;
    
        webClient.UploadData(fullAddress, "POST", Encoding.UTF8.GetBytes(body));
    }
    ```

1. Eigenschaften der Standard vermittelten Nachricht befinden sich einer `BrokerProperties` HTTP-Header. Die Eigenschaften Bank müssen im JSON-Format serialisiert werden. **TimeToLive** Wert von 30 Sekunden angeben und eine Nachricht Bezeichnung "M1" der Nachricht hinzufügen möchten, fügen Sie den folgenden Code unmittelbar vor der `webClient.UploadData()` anrufen im vorherigen Beispiel gezeigt:

    ```
    // Add brokered message properties "TimeToLive" and "Label"
    webClient.Headers.Add("BrokerProperties", "{ \"TimeToLive\":30, \"Label\":\"M1\"}");
    ```

    Beachten Sie, dass die vermittelten Nachrichteneigenschaften waren und hinzugefügt werden sollen. Anforderung zum Senden der muss deshalb eine API Version angeben, die alle Eigenschaften der Nachricht vermittelten unterstützt, die die Anforderung gehören. Wenn die angegebene API-Version eine Nachrichteneigenschaft vermittelten nicht unterstützt, wird die Eigenschaft ignoriert.

1. Benutzerdefinierte Eigenschaften werden als eine Reihe von Schlüssel-Wert-Paare definiert. Jede benutzerdefinierte Eigenschaft wird in einem eigenen Kopfzeile TPPT gespeichert. Wenn Sie die benutzerdefinierten Eigenschaften "Priorität" und "Kunden" hinzufügen möchten, fügen Sie den folgenden Code unmittelbar vor der `webClient.UploadData()` anrufen im vorherigen Beispiel gezeigt:

    ```
    // Add custom properties "Priority" and "Customer".
    webClient.Headers.Add("Priority", "High");
    webClient.Headers.Add("Customer", "12345");
    ```

## <a name="receive-and-delete-a-message-from-the-queue"></a>Erhalten und Löschen einer Nachricht aus der Warteschlange

Im nächsten Schritt wird eine Methode hinzufügen, die den Befehl REST-Schreibweise HTTP löschen, zu empfangen und Löschen einer Nachricht aus der Warteschlange verwendet.

Fügen Sie die folgenden direkt hinter Code der `SendMessage()` Code, die Sie im vorherigen Schritt hinzugefügt haben:

```
// Receives and deletes the next message from the given resource (queue, topic, or subscription)
// using the resourceName and an HTTP DELETE request
private static string ReceiveAndDeleteMessage(string resourceName)
{
    string fullAddress = baseAddress + resourceName + "/messages/head" + "?timeout=60";
    Console.WriteLine("\nRetrieving message from {0}", fullAddress);
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
    string responseStr = Encoding.UTF8.GetString(response);

    Console.WriteLine(responseStr);
    return responseStr;
}
```

## <a name="create-a-topic-and-subscription"></a>Erstellen Sie ein Thema und ein Abonnement

Im nächsten Schritt wird eine Methode schreiben, die den REST-Schreibweise HTTP setzen Befehl verwendet, um ein Thema zu erstellen. Klicken Sie dann schreiben Sie eine Methode, die ein Abonnement zu diesem Thema erstellt wird.

### <a name="create-a-topic"></a>Erstellen Sie ein Thema

Fügen Sie die folgenden direkt hinter Code der `ReceiveAndDeleteMessage()` Code, die Sie im vorherigen Schritt hinzugefügt haben:

```
// Using an HTTP PUT request.
private static string CreateTopic(string topicName)
{
    var topicAddress = baseAddress + topicName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating topic {0}", topicAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + topicName + @"</title>
                                  <content type=""application/xml"">
                                    <TopicDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

    byte[] response = webClient.UploadData(topicAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

### <a name="create-a-subscription"></a>Erstellen Sie ein Abonnement

Der folgende Code erstellt ein Abonnement für das Thema, das Sie im vorherigen Schritt erstellt haben. Fügen Sie den folgenden Code direkt nach der `CreateTopic()` Definition:

```
private static string CreateSubscription(string topicName, string subscriptionName)
{
    var subscriptionAddress = baseAddress + topicName + "/Subscriptions/" + subscriptionName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating subscription {0}", subscriptionAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + subscriptionName + @"</title>
                                  <content type=""application/xml"">
                                    <SubscriptionDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

    byte[] response = webClient.UploadData(subscriptionAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

## <a name="retrieve-message-resources"></a>Abrufen der Nachricht Ressourcen

In diesem Schritt fügen Sie Code, der ruft der Eigenschaften der Nachricht und löscht die messaging Ressourcen, die Sie in den vorherigen Schritten erstellt haben.

### <a name="retrieve-an-atom-feed-with-the-specified-resources"></a>Abrufen einer Atom-Feeds mit der angegebenen Ressourcen

Fügen Sie den folgenden Code direkt nach der `CreateSubscription()` Methode, die Sie im vorherigen Schritt hinzugefügt haben:

```
private static string GetResources(string resourceAddress)
{
    string fullAddress = baseAddress + resourceAddress;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;
    Console.WriteLine("\nGetting resources from {0}", fullAddress);
    return FormatXml(webClient.DownloadString(fullAddress));
}
```

### <a name="delete-messaging-entities"></a>Messaging Elemente löschen

Fügen Sie den folgenden Code direkt nach den Code, die, den Sie im vorherigen Schritt hinzugefügt:

```
private static string DeleteResource(string resourceName)
{
    string fullAddress = baseAddress + resourceName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nDeleting resource at {0}", fullAddress);
    byte[] response = webClient.UploadData(fullAddress, "DELETE", newbyte[0]);
    return Encoding.UTF8.GetString(response);
}
```

### <a name="format-the-atom-feed"></a>Formatieren des Atom-Feeds

Die `GetResources()` Methode enthält einen Anruf an eine `FormatXml()` Methode, die das abgerufene Atom formatiert feed benutzerspezifisch besser lesbar. Im folgenden finden Sie die Definition der `FormatXml()`; Fügen Sie diesen Code direkt hinter der `DeleteResource()` Code, die Sie im vorherigen Abschnitt hinzugefügt:

```
// Formats the XML string to be more human-readable; intended for display purposes
private static string FormatXml(string inputXml)
{
    XmlDocument document = new XmlDocument();
    document.Load(new StringReader(inputXml));

    StringBuilder builder = new StringBuilder();
    using (XmlTextWriter writer = new XmlTextWriter(new StringWriter(builder)))
    {
        writer.Formatting = Formatting.Indented;
        document.Save(writer);
    }

    return builder.ToString();
}
```

## <a name="build-and-run-the-application"></a>Erstellen Sie, und führen Sie die Anwendung

Sie können jetzt erstellen, und führen Sie die Anwendung. Klicken Sie auf die **Lösung erstellen**, oder drücken Sie **STRG + UMSCHALT + B**, in Visual Studio im Menü **Erstellen** .

### <a name="run-the-application"></a>Führen Sie die Anwendung

Wenn keine Fehler vorliegen, drücken Sie F5, um die Anwendung ausführen. Wenn Sie dazu aufgefordert werden, geben Sie Ihren Namespace, SAS Key Namen, SAS Schlüsselwert, die Sie für Ihren Kunden im ersten Schritt.

### <a name="example"></a>Beispiel

Im folgende Beispiel wird der vollständige Code, wie es angezeigt werden soll, nachdem Sie alle Schritte in diesem Lernprogramm folgen.

```
using System;
using System.Globalization;
using System.IO;
using System.Net;
using System.Security.Cryptography;
using System.Text;
using System.Xml;

namespace Microsoft.ServiceBus.Samples
{
    class Program
    {
        static string serviceNamespace;
        static string baseAddress;
        static string token;
        const string sbHostName = "servicebus.windows.net";

        static void Main(string[] args)
        {
            Console.Write("Enter your service namespace: ");
            serviceNamespace = Console.ReadLine();

            Console.Write("Enter your SAS key: ");
            string SASKey = Console.ReadLine();

            baseAddress = "https://" + serviceNamespace + "." + sbHostName + "/";
            try
            {
                token = GetSASToken("RootManageSharedAccessKey", SASKey);

                string queueName = "Queue" + Guid.NewGuid().ToString();

                // Create and put a message in the queue
                CreateQueue(queueName, token);
                SendMessage(queueName, "msg1");
                string msg = ReceiveAndDeleteMessage(queueName);

                string topicName = "Topic" + Guid.NewGuid().ToString();
                string subscriptionName = "Subscription" + Guid.NewGuid().ToString();
                CreateTopic(topicName);
                CreateSubscription(topicName, subscriptionName);
                SendMessage(topicName, "msg2");

                Console.WriteLine(ReceiveAndDeleteMessage(topicName + "/Subscriptions/" + subscriptionName));

                // Get an Atom feed with all the queues in the namespace
                Console.WriteLine(GetResources("$Resources/Queues"));

                // Get an Atom feed with all the topics in the namespace
                Console.WriteLine(GetResources("$Resources/Topics"));

                // Get an Atom feed with all the subscriptions for the topic we just created
                Console.WriteLine(GetResources(topicName + "/Subscriptions"));

                // Get an Atom feed with all the rules for the topic and subscription we just created
                Console.WriteLine(GetResources(topicName + "/Subscriptions/" + subscriptionName + "/Rules"));

                // Delete the queue we created
                DeleteResource(queueName);

                // Delete the topic we created
                DeleteResource(topicName);

                // Get an Atom feed with all the topics in the namespace, it shouldn't have the one we created now
                Console.WriteLine(GetResources("$Resources/Topics"));

                // Get an Atom feed with all the queues in the namespace, it shouldn't have the one we created now
                Console.WriteLine(GetResources("$Resources/Queues"));
            }
            catch (WebException we)
            {
                using (HttpWebResponse response = we.Response as HttpWebResponse)
                {
                    if (response != null)
                    {
                        Console.WriteLine(new StreamReader(response.GetResponseStream()).ReadToEnd());
                    }
                    else
                    {
                        Console.WriteLine(we.ToString());
                    }
                }
            }

            Console.WriteLine("\nPress ENTER to exit.");
            Console.ReadLine();
        }

        private static string GetSASToken(string SASKeyName, string SASKeyValue)
        {
            TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
            string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + 3600);
            string stringToSign = WebUtility.UrlEncode(baseAddress) + "\n" + expiry;
            HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(SASKeyValue));

            string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
            string sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}",
                WebUtility.UrlEncode(baseAddress), WebUtility.UrlEncode(signature), expiry, SASKeyName);
            return sasToken;
        }

        // Uses HTTP PUT to create the queue
        private static string CreateQueue(string queueName, string token)
        {
            // Create the URI of the new queue, note that this uses the HTTPS scheme
            string queueAddress = baseAddress + queueName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating queue {0}", queueAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + queueName + @"</title>
                                  <content type=""application/xml"">
                                    <QueueDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(queueAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        // Sends a message to the "queueName" queue, given the name and the value to enqueue
        // Uses an HTTP POST request.
        private static void SendMessage(string queueName, string body)
        {
            string fullAddress = baseAddress + queueName + "/messages" + "?timeout=60&api-version=2013-08 ";
            Console.WriteLine("\nSending message {0} - to address {1}", body, fullAddress);
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;
            // Add brokered message properties “TimeToLive” and “Label”.
            webClient.Headers.Add("BrokerProperties", "{ \"TimeToLive\":30, \"Label\":\"M1\"}");
            // Add custom properties “Priority” and “Customer”.
            webClient.Headers.Add("Priority", "High");
            webClient.Headers.Add("Customer", "12345");
            webClient.UploadData(fullAddress, "POST", Encoding.UTF8.GetBytes(body));

        }

        // Receives and deletes the next message from the given resource (queue, topic, or subscription)
        // using the resourceName and an HTTP DELETE request.
        private static string ReceiveAndDeleteMessage(string resourceName)
        {
            string fullAddress = baseAddress + resourceName + "/messages/head" + "?timeout=60";
            Console.WriteLine("\nRetrieving message from {0}", fullAddress);
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
            string responseStr = Encoding.UTF8.GetString(response);

            Console.WriteLine(responseStr);
            return responseStr;
        }

        // Using an HTTP PUT request.
        private static string CreateTopic(string topicName)
        {
            var topicAddress = baseAddress + topicName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating topic {0}", topicAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + topicName + @"</title>
                                  <content type=""application/xml"">
                                    <TopicDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(topicAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        private static string CreateSubscription(string topicName, string subscriptionName)
        {
            var subscriptionAddress = baseAddress + topicName + "/Subscriptions/" + subscriptionName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating subscription {0}", subscriptionAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + subscriptionName + @"</title>
                                  <content type=""application/xml"">
                                    <SubscriptionDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(subscriptionAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        private static string GetResources(string resourceAddress)
        {
            string fullAddress = baseAddress + resourceAddress;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;
            Console.WriteLine("\nGetting resources from {0}", fullAddress);
            return FormatXml(webClient.DownloadString(fullAddress));
        }

        private static string DeleteResource(string resourceName)
        {
            string fullAddress = baseAddress + resourceName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nDeleting resource at {0}", fullAddress);
            byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
            return Encoding.UTF8.GetString(response);
        }

        // Formats the XML string to be more human-readable; intended for display purposes
        private static string FormatXml(string inputXml)
        {
            XmlDocument document = new XmlDocument();
            document.Load(new StringReader(inputXml));

            StringBuilder builder = new StringBuilder();
            using (XmlTextWriter writer = new XmlTextWriter(new StringWriter(builder)))
            {
                writer.Formatting = Formatting.Indented;
                document.Save(writer);
            }

            return builder.ToString();
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Finden Sie weitere die folgenden Artikeln:

- [Übersicht über Dienstbus messaging](service-bus-messaging-overview.md)
- [Azure Dienstbus-Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
- [Dienstbus Relay REST Lernprogramm](../service-bus-relay/service-bus-relay-rest-tutorial.md)

