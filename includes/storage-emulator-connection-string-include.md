Speicheremulator unterstützt ein einzelnes feste Konto und einen bekannten Authentifizierungsschlüssel für die Authentifizierung Schlüssel freigegeben. Dieses Konto und einen Schlüssel sind die nur freigegebene Schlüssel Anmeldeinformationen für die Verwendung mit Speicheremulator zulässig. Sie sind:

    Account name: devstoreaccount1
    Account key: Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
    
> [AZURE.NOTE] Die Authentifizierung-Taste vom Speicheremulator unterstützt dient nur zum Testen der Funktionen des Codes Authentifizierung Client. Sie dienen keine Zwecke Sicherheit. Sie können keine der Herstellung Speicherkonto und einen Schlüssel Speicheremulator verwenden. Beachten Sie auch, dass das Konto Entwicklung nicht mit Daten verwendet werden sollen.
>
> Beachten Sie, dass der Speicheremulator nur Verbindung über HTTP unterstützt. HTTPS ist jedoch das empfohlene Protokoll für den Zugriff auf Ressourcen in einem Azure Herstellung Speicher-Konto an.
 
#### <a name="connect-to-the-emulator-account-using-a-shortcut"></a>Herstellen einer Verbindung Emulator Konto mithilfe einer Verknüpfung mit

Die einfachste Möglichkeit zum Verbinden mit Speicheremulator von Ihrer Anwendung ist eine Verbindungszeichenfolge aus innerhalb Ihrer Anwendung Konfigurationsdatei konfiguriert werden, die die Verknüpfung verweist auf `UseDevelopmentStorage=true`. Hier ist ein Beispiel für eine Verbindungszeichenfolge zu Speicheremulator in einer App aus: 

    <appSettings>
      <add key="StorageConnectionString" value="UseDevelopmentStorage=true" />
    </appSettings>

#### <a name="connect-to-the-emulator-account-using-the-well-known-account-name-and-key"></a>Verbinden mit der Emulator Konto mit den bekannten Kontonamen und Schlüssel

Um eine Verbindungszeichenfolge erstellen, die Emulator Kontonamen und Schlüssel verweist, beachten Sie, dass Sie die Endpunkte für jeden Dienst angeben müssen, die Sie verwenden, aus dem Emulator in der Verbindungszeichenfolge möchten. Dies ist erforderlich, damit die Verbindungszeichenfolge die Endpunkte Emulator verwiesen wird, die sich als die für ein Konto der Herstellung Speicher unterscheiden. Beispielsweise wird der Wert der Verbindungszeichenfolge wie folgt aussehen:

    DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;
    AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;
    BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;
    TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
    QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1; 

Dieser Wert entspricht der oben angezeigten Kontextmenü `UseDevelopmentStorage=true`.

#### <a name="specify-an-http-proxy"></a>Geben Sie einen HTTP-proxy

Sie können auch einen HTTP-Proxy zu verwenden, wenn Sie den Dienst gegen Speicheremulator testen, angeben. Dies kann für das HTTP-Anfragen und Antworten beobachten, während Sie Operationen, die auf die Speicherdienste testen nützlich sein. Wenn einen Proxy festlegen möchten, fügen Sie die `DevelopmentStorageProxyUri` option, um die Verbindungszeichenfolge aus, und legen Sie den Wert an den Proxy-URI. So sieht beispielsweise eine Verbindungszeichenfolge, die auf Speicheremulator zeigt und konfiguriert einen HTTP-Proxy aus:

    UseDevelopmentStorage=true;DevelopmentStorageProxyUri=http://myProxyUri
