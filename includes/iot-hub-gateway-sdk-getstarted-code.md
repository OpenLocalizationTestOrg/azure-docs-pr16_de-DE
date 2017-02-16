## <a name="typical-output"></a>Typische Ausgabe

Es folgt ein Beispiel für die Ausgabe in die Protokolldatei von der Stichprobe Hallo Welt geschrieben. Zeilenwechsel und Registerkarte Zeichen wurden hinzugefügt, um die Lesbarkeit:

```
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Codeausschnitte

In diesem Abschnitt werden einige wichtige Teile des Codes in der Stichprobe Hallo Welt.

### <a name="gateway-creation"></a>Erstellung des Gateways

Der Entwickler muss des *Gatewayprozesses*schreiben. Dieses Programm erstellt die interne Infrastruktur (der Makler), lädt die Module und richtet alles ordnungsgemäß funktioniert. Das SDK bietet die **Gateway_Create_From_JSON** -Funktion, um Sie zum Starten eines Gateways aus einer JSON-Datei aktivieren. Um die Funktion **Gateway_Create_From_JSON** verwenden, müssen Sie ihr den Pfad zu einer JSON-Datei übergeben, die angibt, die Module zu laden. 

Suchen Sie den Code für Gatewayprozesses in der Stichprobe Hallo Welt in der [main.c] [ lnk-main-c] Datei. Um die Lesbarkeit zeigt der folgenden Codeausschnitt eine gekürzte Version des Gateways Prozess Codes. Dieses Programm erstellt einen Gateway, und klicken Sie dann **die EINGABETASTE** drücken, bevor Sie es ab dem Gateway abbaut der Benutzer wartet. 

```
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

Die JSON-Einstellungsdatei enthält eine Liste der Module zu laden. Jedem Modul muss a: angeben.

- **Modulname**: einen eindeutigen Namen für das Modul.
- **Module_path**: der Pfad zu der Bibliothek, die das Modul enthält. Für Linux Dies ist eine .so-Datei, die auf Windows ist dies eine DLL-Datei.
- **Args**: alle Konfigurationsinformationen das Modul muss.

Die JSON-Datei enthält auch die Links zwischen Module ab, die an der Makler übergeben werden. Ein Link verfügt über zwei Eigenschaften:
- **Datenquelle**: einen Modulnamen aus der `modules` Abschnitt oder "\*".
- **Empfänger**: einen Modulnamen aus der `modules` Abschnitt

Jede Verknüpfung definiert eine Nachricht weiterleiten und Richtung. Nachrichten von Modul `source` an das Modul übermittelt werden sollen `sink`. Die `source` kann festgelegt werden, um "\*", gibt an, dass Nachrichten von jedem Modul eingehen werden `sink`.

Das folgende Beispiel zeigt die JSON-Einstellungsdatei zum Konfigurieren der Stichprobe Hallo Welt unter Linux verwendet. Jede Nachricht vom Modul gefertigt `hello_world` behandelt werden vom Modul `logger`. Gibt an, ob ein Modul erfordert, dass das Design des Moduls Argument abhängt. In diesem Beispiel das Protokollierung Modul ein Argument also den Pfad zu der Ausgabedatei und das Modul Hallo Welt keine Argumente:

```
{
    "modules" :
    [ 
        {
            "module name" : "logger",
            "module path" : "./modules/logger/liblogger_hl.so",
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "hello_world",
            "module path" : "./modules/hello_world/libhello_world_hl.so",
            "args" : null
        }
    ],
    "links" :
    [
        {
            "source" : "hello_world",
            "sink" : "logger"
        }
    ]
}
```

### <a name="hello-world-module-message-publishing"></a>Hallo Welt Modul Nachricht für die Veröffentlichung

Sie können den Code, der vom Modul "Hallo Welt" verwendet werden, um Nachrichten in der ['hello_world.c'] veröffentlichen finden[ lnk-helloworld-c] Datei. Der Codeausschnitt unten zeigt eine geänderte Version mit zusätzlichen Kommentare und einige Fehlerbehandlung Code entfernt, um die Lesbarkeit:

```
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);
    
    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;
    
    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="hello-world-module-message-processing"></a>Hallo Welt Modul Verarbeitung von Nachrichten

Das Modul Hallo Welt muss nie keine Nachrichten verarbeitet werden, die andere Module in der Makler veröffentlichen. Dadurch Implementierung des Rückrufs Nachricht im Modul Hallo Welt eine Funktion keine Aktion ausgeführt.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Protokollierung Modul Nachricht veröffentlichen und Verarbeitung

Das Modul für die Protokollierung empfängt Nachrichten von der Makler und schreibt sie in eine Datei. Veröffentlichen der nie alle Nachrichten. Daher ruft der Code des Moduls Protokollierung nie die Funktion **Broker_Publish** aus.

Die Funktion **Logger_Recieve** in der [logger.c] [ lnk-logger-c] Datei ist der Rückruf der Makler aufgerufen wird, um Nachrichten in das Modul für die Protokollierung zu übermitteln. Der Codeausschnitt unten zeigt eine geänderte Version mit zusätzlichen Kommentare und einige Fehlerbehandlung Code entfernt, um die Lesbarkeit:

```
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden von SDK Gateway IoT, probieren Sie Folgendes ein:

- [IoT Gateway SDK – senden Gerät-Cloud-Nachrichten mit einem simulierten Gerät mit Linux][lnk-gateway-simulated].
- [Azure IoT Gateway SDK] [ lnk-gateway-sdk] auf GitHub.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md