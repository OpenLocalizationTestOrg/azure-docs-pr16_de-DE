### <a name="create-a-console-application"></a>Erstellen Sie eine

- Starten Sie Visual Studio und erstellen Sie eine neue.

### <a name="add-the-relay-nuget-package"></a>Hinzufügen des Relay NuGet-Pakets

1. Mit der rechten Maustaste in des neue Projekts ein, und wählen Sie **NuGet-Pakete verwalten**.

2. Klicken Sie auf die Registerkarte **Durchsuchen** , und klicken Sie dann suchen Sie nach "Microsoft Azure Relay", und wählen Sie das Element **Microsoft Azure Relay** . Klicken Sie auf **Installieren** , um die Installation durchzuführen, und klicken Sie dann schließen Sie in diesem Dialogfeld zu.

### <a name="write-some-code-to-receive-messages"></a>Schreiben von entsprechendem Code, der zum Empfangen von Nachrichten

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
3. Fügen Sie eine neue Methode mit `ProcessMessagesOnConnection` in der `Program` Klasse wie folgt aus:

    ```cs
    private static async void ProcessMessagesOnConnection(HybridConnectionStream relayConnection, CancellationTokenSource cts)
    {
        Console.WriteLine("New session");

        // The connection is a fully bidrectional stream. 
        // We put a stream reader and a stream writer over it 
        // which allows us to read UTF-8 text that comes from 
        // the sender and to write text replies back.
        var reader = new StreamReader(relayConnection);
        var writer = new StreamWriter(relayConnection) { AutoFlush = true };
        while (!cts.IsCancellationRequested)
        {
            try
            {
                // Read a line of input until a newline is encountered
                var line = await reader.ReadLineAsync();

                if (string.IsNullOrEmpty(line))
                {
                    // If there's no input data, we will signal that 
                    // we will no longer send data on this connection
                    // and then break out of the processing loop.
                    await relayConnection.ShutdownAsync(cts.Token);
                    break;
                }

                // Output the line on the console
                Console.WriteLine(line);

                // Write the line back to the client, prepending "Echo:"
                await writer.WriteLineAsync($"Echo: {line}");
            }
            catch (IOException)
            {
                // Catch an IO exception that is likely caused because
                // the client disconnected.
                Console.WriteLine("Client closed connection");
                break;
            }
        }

        Console.WriteLine("End session");

        // Closing the connection
        await relayConnection.CloseAsync(cts.Token);
    }
    ```

4. Fügen Sie eine andere neue Methode, die mit `RunAsync` in der `Program` Klasse wie folgt aus:

    ```cs
    private static async Task RunAsync()
    {
        var cts = new CancellationTokenSource();

        var tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(KeyName, Key);
        var listener = new HybridConnectionListener(new Uri(string.Format("sb://{0}/{1}", RelayNamespace, ConnectionName)), tokenProvider);

        // Subscribe to the status events
        listener.Connecting += (o, e) => { Console.WriteLine("Connecting"); };
        listener.Offline += (o, e) => { Console.WriteLine("Offline"); };
        listener.Online += (o, e) => { Console.WriteLine("Online"); };

        // Opening the listener will establish the control channel to
        // the Azure Relay service. The control channel will be continuously 
        // maintained and reestablished when connectivity is disrupted.
        await listener.OpenAsync(cts.Token);
        Console.WriteLine("Server listening");

        // Providing callback for cancellation token that will close the listener.
        cts.Token.Register(() => listener.CloseAsync(CancellationToken.None));

        // Start a new thread that will continuously read the console.
        new Task(() => Console.In.ReadLineAsync().ContinueWith((s) => { cts.Cancel(); })).Start();

        // Accept the next available, pending connection request. 
        // Shutting down the listener will allow a clean exit with 
        // this method returning null
        while (true)
        {
            var relayConnection = await listener.AcceptConnectionAsync();
            if (relayConnection == null)
            {
                break;
            }

            ProcessMessagesOnConnection(relayConnection, cts);
        }

        // Close the listener after we exit the processing loop
        await listener.CloseAsync(cts.Token);
    }
    ```

5. Fügen Sie die folgende Zeile des Codes zu den `Main` Methode in der `Program` Class.

    ```cs
    RunAsync().GetAwaiter().GetResult();
    ```

    Hier wird der Program.cs aussehen sollte:

    ```cs
    namespace Server
    {
        using System;
        using System.IO;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Azure.Relay;

        public class Program
        {
            private const string RelayNamespace = "{RelayNamespace}";
            private const string ConnectionName = "{HybridConnectionName}";
            private const string KeyName = "{SASKeyName}";
            private const string Key = "{SASKey}";

            public static void Main(string[] args)
            {
                RunAsync().GetAwaiter().GetResult();
            }

            private static async Task RunAsync()
            {
                var cts = new CancellationTokenSource();

                var tokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(KeyName, Key);
                var listener = new HybridConnectionListener(new Uri(string.Format("sb://{0}/{1}", RelayNamespace, ConnectionName)), tokenProvider);

                // Subscribe to the status events
                listener.Connecting += (o, e) => { Console.WriteLine("Connecting"); };
                listener.Offline += (o, e) => { Console.WriteLine("Offline"); };
                listener.Online += (o, e) => { Console.WriteLine("Online"); };

                // Opening the listener will establish the control channel to
                // the Azure Relay service. The control channel will be continuously 
                // maintained and reestablished when connectivity is disrupted.
                await listener.OpenAsync(cts.Token);
                Console.WriteLine("Server listening");

                // Providing callback for cancellation token that will close the listener.
                cts.Token.Register(() => listener.CloseAsync(CancellationToken.None));

                // Start a new thread that will continuously read the console.
                new Task(() => Console.In.ReadLineAsync().ContinueWith((s) => { cts.Cancel(); })).Start();

                // Accept the next available, pending connection request. 
                // Shutting down the listener will allow a clean exit with 
                // this method returning null
                while (true)
                {
                    var relayConnection = await listener.AcceptConnectionAsync();
                    if (relayConnection == null)
                    {
                        break;
                    }

                    ProcessMessagesOnConnection(relayConnection, cts);
                }

                // Close the listener after we exit the processing loop
                await listener.CloseAsync(cts.Token);
            }

            private static async void ProcessMessagesOnConnection(HybridConnectionStream relayConnection, CancellationTokenSource cts)
            {
                Console.WriteLine("New session");

                // The connection is a fully bidrectional stream. 
                // We put a stream reader and a stream writer over it 
                // which allows us to read UTF-8 text that comes from 
                // the sender and to write text replies back.
                var reader = new StreamReader(relayConnection);
                var writer = new StreamWriter(relayConnection) { AutoFlush = true };
                while (!cts.IsCancellationRequested)
                {
                    try
                    {
                        // Read a line of input until a newline is encountered
                        var line = await reader.ReadLineAsync();

                        if (string.IsNullOrEmpty(line))
                        {
                            // If there's no input data, we will signal that 
                            // we will no longer send data on this connection
                            // and then break out of the processing loop.
                            await relayConnection.ShutdownAsync(cts.Token);
                            break;
                        }

                        // Output the line on the console
                        Console.WriteLine(line);

                        // Write the line back to the client, prepending "Echo:"
                        await writer.WriteLineAsync($"Echo: {line}");
                    }
                    catch (IOException)
                    {
                        // Catch an IO exception that is likely caused because
                        // the client disconnected.
                        Console.WriteLine("Client closed connection");
                        break;
                    }
                }

                Console.WriteLine("End session");

                // Closing the connection
                await relayConnection.CloseAsync(cts.Token);
            }
        }
    }
    ```