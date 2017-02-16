### <a name="create-a-console-application"></a>Erstellen Sie eine

- Starten Sie Visual Studio und erstellen Sie eine neue.

### <a name="add-the-relay-nuget-package"></a>Hinzufügen des Relay NuGet-Pakets

1. Mit der rechten Maustaste in des neue Projekts ein, und wählen Sie **NuGet-Pakete verwalten**.

2. Klicken Sie auf die Registerkarte **Durchsuchen** , und klicken Sie dann suchen Sie nach "Microsoft Azure Relay", und wählen Sie das Element **Microsoft Azure Relay** . Klicken Sie auf **Installieren** , um die Installation durchzuführen, und klicken Sie dann schließen Sie in diesem Dialogfeld zu.

### <a name="write-some-code-to-send-messages"></a>Schreiben von entsprechendem Code, um Nachrichten zu senden.

1. Fügen Sie den folgenden `using` -Anweisung an den Anfang der Datei Program.cs.

    ```cs
    using Microsoft.Azure.Relay;
    ```

2. Hinzufügen von Konstanten in die `Program` Class Details Verbindung in Verbindung. Ersetzen Sie die Platzhalter in Klammern mit geeigneten Werten, die beim Erstellen der Verbindung Hybrid abgerufen wurden.

    ```cs
    private const string RelayNamespace = "{RelayNamespace}";
    private const string ConnectionName = "{HybridConnectionName}";
    private const string KeyName = "{SASKeyName}";
    private const string Key = "{SASKey}";
    ```

3. Hinzufügen einer neue Methode zur der `Program` Klasse wie folgt aus:

    ```cs
    private static async Task RunAsync()
    {
        Console.WriteLine("Enter lines of text to send to the server with ENTER");

        // Create a new hybrid connection client
        var tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(KeyName, Key);
        var client = new HybridConnectionClient(new Uri(String.Format("sb://{0}/{1}", RelayNamespace, ConnectionName)), tokenProvider);

        // Initiate the connection
        var relayConnection = await client.CreateConnectionAsync();

        // We run two conucrrent loops on the connection. One 
        // reads input from the console and writes it to the connection 
        // with a stream writer. The other reads lines of input from the 
        // connection with a stream reader and writes them to the console. 
        // Entering a blank line will shut down the write task after 
        // sending it to the server. The server will then cleanly shut down
        // the connection which will terminate the read task.

        var reads = Task.Run(async () => {
            // initialize the stream reader over the connection
            var reader = new StreamReader(relayConnection);
            var writer = Console.Out;
            do
            {
                // read a full line of UTF-8 text up to newline
                string line = await reader.ReadLineAsync();
                // if the string is empty or null, we are done.
                if (String.IsNullOrEmpty(line))
                    break;
                // write to the console
                await writer.WriteLineAsync(line);
            }
            while (true);
        });

        // read from the console and write to the hybrid connection
        var writes = Task.Run(async () => {
            var reader = Console.In;
            var writer = new StreamWriter(relayConnection) { AutoFlush = true };
            do
            {
                // read a line form the console
                string line = await reader.ReadLineAsync();
                // write the line out, also when it's empty
                await writer.WriteLineAsync(line);
                // quit when the line was empty
                if (String.IsNullOrEmpty(line))
                    break;
            }
            while (true);
        });

        // wait for both tasks to complete
        await Task.WhenAll(reads, writes);
        await relayConnection.CloseAsync(CancellationToken.None);
    }
    ```

4. Fügen Sie die folgende Zeile des Codes zu den `Main` Methode in der `Program` Class.

    ```cs
    RunAsync().GetAwaiter().GetResult();
    ```

    Hier wird der Program.cs aussehen sollte.

    ```cs
    using System;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Relay;

    namespace Client
    {
        class Program
        {
            private const string RelayNamespace = "{RelayNamespace}";
            private const string ConnectionName = "{HybridConnectionName}";
            private const string KeyName = "{SASKeyName}";
            private const string Key = "{SASKey}";

            static void Main(string[] args)
            {
                RunAsync().GetAwaiter().GetResult();
            }

            private static async Task RunAsync()
            {
                Console.WriteLine("Enter lines of text to send to the server with ENTER");

                // Create a new hybrid connection client
                var tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(KeyName, Key);
                var client = new HybridConnectionClient(new Uri(String.Format("sb://{0}/{1}", RelayNamespace, ConnectionName)), tokenProvider);

                // Initiate the connection
                var relayConnection = await client.CreateConnectionAsync();

                // We run two conucrrent loops on the connection. One 
                // reads input from the console and writes it to the connection 
                // with a stream writer. The other reads lines of input from the 
                // connection with a stream reader and writes them to the console. 
                // Entering a blank line will shut down the write task after 
                // sending it to the server. The server will then cleanly shut down
                // the connection which will terminate the read task.

                var reads = Task.Run(async () => {
                    // initialize the stream reader over the connection
                    var reader = new StreamReader(relayConnection);
                    var writer = Console.Out;
                    do
                    {
                        // read a full line of UTF-8 text up to newline
                        string line = await reader.ReadLineAsync();
                        // if the string is empty or null, we are done.
                        if (String.IsNullOrEmpty(line))
                            break;
                        // write to the console
                        await writer.WriteLineAsync(line);
                    }
                    while (true);
                });

                // read from the console and write to the hybrid connection
                var writes = Task.Run(async () => {
                    var reader = Console.In;
                    var writer = new StreamWriter(relayConnection) { AutoFlush = true };
                    do
                    {
                        // read a line form the console
                        string line = await reader.ReadLineAsync();
                        // write the line out, also when it's empty
                        await writer.WriteLineAsync(line);
                        // quit when the line was empty
                        if (String.IsNullOrEmpty(line))
                            break;
                    }
                    while (true);
                });

                // wait for both tasks to complete
                await Task.WhenAll(reads, writes);
                await relayConnection.CloseAsync(CancellationToken.None);
            }
        }
    }
    ```