### <a name="create-a-nodejs-application"></a>Erstellen Sie eine Anwendung Node.js

- Erstellen Sie eine neue JavaScript-Datei mit `listener.js`.

### <a name="add-the-relay-npm-package"></a>Fügen Sie die Paketdatei Relay NPM

- Führen Sie `npm install hyco-ws` Knoten Befehlszeile im Ordner "Projekt".

### <a name="write-some-code-to-receive-messages"></a>Schreiben von entsprechendem Code, der zum Empfangen von Nachrichten

1. Fügen Sie den folgenden `constant` am oberen Rand der `listener.js` Datei.

    ```js
    const WebSocket = require('hyco-ws');
    ```

2. Fügen Sie das folgende Relay `constants` zu den `listener.js` Details Verbindung in Verbindung. Ersetzen Sie die Platzhalter in Klammern mit geeigneten Werten, die beim Erstellen der Verbindung Hybrid abgerufen wurden.

    1. `const ns`-Relay-namespace

    2. `const path`– Der Name der Verbindung Hybrid

    3. `const keyrule`– Der Name des SAS-Taste

    4. `const key`-Den Schlüsselwert SAS

3. Fügen Sie den folgenden Code ein, um die `listener.js` Datei:

    ```js
    var wss = WebSocket.createRelayedServer(
    {
        server : WebSocket.createRelayListenUri(ns, path),
        token: WebSocket.createRelayToken('http://' + ns, keyrule, key)
    }, 
    function (ws) {
        console.log('connection accepted');
        ws.onmessage = function (event) {
            console.log(event.data);
        };
        ws.on('close', function () {
            console.log('connection closed');
        });       
    });

    console.log('listening');

    wss.on('error', function(err) {
        console.log('error' + err);
    });
    ```
    So sieht wie Ihre listener.js aussehen sollte:

    ```js
    const WebSocket = require('hyco-ws');

    const ns = "{RelayNamespace}";
    const path = "{HybridConnectionName}";
    const keyrule = "{SASKeyName}";
    const key = "{SASKeyValue}";

    var wss = WebSocket.createRelayedServer(
        {
            server : WebSocket.createRelayListenUri(ns, path),
            token: WebSocket.createRelayToken('http://' + ns, keyrule, key)
        }, 
        function (ws) {
            console.log('connection accepted');
            ws.onmessage = function (event) {
                console.log(event.data);
            };
            ws.on('close', function () {
                console.log('connection closed');
            });       
    });

    console.log('listening');

    wss.on('error', function(err) {
        console.log('error' + err);
    });
    ```