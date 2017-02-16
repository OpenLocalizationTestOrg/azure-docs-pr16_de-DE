<properties
   pageTitle="Schließen Sie ein Gerät mit C auf Mbed | Microsoft Azure"
   description="Beschreibt, wie ein Gerät mit der Azure IoT Suite vorkonfiguriert remote Überwachung-Lösung mit einer Anwendung in C ausführen auf einem Gerät Mbed geschriebene verbinden."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a>Schließen Sie Ihr Gerät in der remote Überwachung vorkonfigurierten Lösung (Mbed)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-c-sample-solution"></a>Erstellen und Ausführen der Lösung C Stichprobe

Die folgenden Anweisungen beschreiben Sie die Schritte zum Herstellen einer Verbindung eine [Mbed aktiviert Freescale FRDM-K64F] [ lnk-mbed-home] Gerät in die Überwachung remote-Lösung.

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a>Herstellen einer Verbindung Ihrer Netzwerk- und Desktop-Computer mit dem Gerät mbed

1. Schließen Sie das Gerät Mbed, über ein Ethernet-Kabel mit Ihrem Netzwerk. Dieser Schritt ist erforderlich, da die Anwendung Stichprobe Internetzugang erforderlich ist.

2. Finden Sie unter [Erste Schritte mit Mbed] [ lnk-mbed-getstarted] auf Ihrem Gerät Mbed Verbindung mit Ihrem desktop-PC.

3. Wenn Sie Ihren desktop-PC mit Windows ausgeführt wird, finden Sie unter [PC-Konfiguration] [ lnk-mbed-pcconnect] seriellen Anschluss Zugriff auf Ihr Gerät Mbed konfigurieren.

### <a name="create-an-mbed-project-and-import-the-sample-code"></a>Erstellen Sie ein Projekt Mbed und importieren Sie den Code Stichprobe

1. Wechseln Sie in Ihrem Webbrowser zu der mbed.org [Entwicklerwebsite](https://developer.mbed.org/). Wenn Sie sich angemeldet haben, wird eine Option zum Erstellen eines Kontos (es ist kostenlos). Andernfalls wird, melden Sie sich mit Ihrem Kontoanmeldeinformationen an. Klicken Sie dann auf **Compiler** in der oberen rechten Ecke der Seite. Diese Aktion gelangen Sie zur der *Arbeitsbereich* -Benutzeroberfläche an.

2. Stellen Sie sicher, dass die Hardware-Plattform, die Sie verwenden in der oberen rechten Ecke des Fensters angezeigt wird, oder klicken Sie auf das Symbol in der rechten Ecke, um Ihre Hardwareplattform auszuwählen.

3. Klicken Sie im Hauptfenster Menü auf **Importieren** . Klicken Sie dann auf **hier klicken** , zu importierender URL links neben der Mbed Globus Logo.

    ![][6]

4. Klicken Sie im Popupfenster geben Sie den Link für die Stichprobe Code https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/, und klicken Sie auf **Importieren**.

    ![][7]

5. Sie können im Fenster Compiler Mbed anzeigen, dass dieses Projekt importieren auch in mehreren Bibliotheken importieren. Einige bereitgestellt und verwaltet durch das Team Azure IoT ([Azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [Iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [Iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [Azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), während andere Drittanbieter-Bibliotheken im Katalog Bibliotheken Mbed verfügbar sind.

    ![][8]

6. Öffnen Sie die Datei remote_monitoring\remote_monitoring.c, und suchen Sie den folgenden Code in der Datei:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "[IoTHub Suffix, i.e. azure-devices.net]";
    ```

7. Ersetzen Sie [Geräte-Id] und [Gerät Key], mit den Gerätedaten das Programm Stichprobe für die Verbindung zu Ihrem IoT-Hub zu aktivieren. Verwenden der IoT Hub Hostname ersetzen [Name der IoTHub] und [IoTHub Suffix, d. h. Azure devices.net]. Ist Ihre IoT Hub Hostname **contoso.azure-devices.net**, beträgt beispielsweise **"Contoso"** die **HubName** und alles nach der **HubSuffix**ist:

    ```
    static const char* deviceId = "mydevice";
    static const char* deviceKey = "mykey";
    static const char* hubName = "contoso";
    static const char* hubSuffix = "azure-devices.net";
    ```

    ![][9]

### <a name="walk-through-the-code"></a>Durchlaufen Sie den code

Des Programms Funktionsweise interessiert sind, werden in diesem Abschnitt einige wichtige Teile des Codes Stichprobe beschrieben. Wenn Sie den Code ausführen möchten, fahren Sie mit [das Programm erstellen und Ausführen](#buildandrun).

#### <a name="defining-the-model"></a>Definieren des Modells

In diesem Beispiel wird das [Serialisierungsprogramm] [ lnk-serializer] -Bibliothek auf ein Modell definiert haben, die angibt, die Nachrichten, die das Gerät an IoT Verteiler senden und Empfangen von IoT Hub kann. In diesem Beispiel definiert den **Contoso** -Namespace ein **Thermostat** Modell, die angibt, die **Temperatur**, **ExternalTemperature**und **Feuchtigkeit** werden Daten zusammen mit Metadaten wie die Geräte-Id, Geräte-Eigenschaften und die Befehle, denen auf das Gerät reagiert:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
);

DECLARE_STRUCT(DeviceProperties,
ascii_char_ptr, DeviceID,
_Bool, HubEnabledState
);

DECLARE_MODEL(Thermostat,

    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),

    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),

    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
);

END_NAMESPACE(Contoso);
```

Im Zusammenhang mit der Modelldefinition werden die Definitionen für die **SetTemperature** und **SetHumidity** Befehle, denen auf das Gerät reagiert:

```
EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
{
    (void)printf("Received temperature %d\r\n", temperature);
    thermostat->Temperature = temperature;
    return EXECUTE_COMMAND_SUCCESS;
}

EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
{
    (void)printf("Received humidity %d\r\n", humidity);
    thermostat->Humidity = humidity;
    return EXECUTE_COMMAND_SUCCESS;
}
```

#### <a name="connecting-the-model-to-the-library"></a>Verbinden das Modell zu der Bibliothek

Die Funktionen **SendMessage** und **IoTHubMessage** werden häufig verwendeter Code für das Senden von werden vom Gerät, und verbinden Nachrichten von IoT Hub mit den Befehl Ereignishandler.

#### <a name="the-remotemonitoringrun-function"></a>Die Remote_monitoring_run-Funktion

**Hauptfenster** des Programms-Funktion ruft die Funktion **Remote_monitoring_run** beim Start der Anwendung des Geräts Verhalten als IoT Hub-Client-Gerät ausführen. Diese Funktion **Remote_monitoring_run** enthält hauptsächlich geschachtelte Paare von Funktionen:

- **Plattform\_Initialisierung** und **Plattform\_Deinit** Plattform-spezifische Initialisierung und war(en) Vorgänge ausführen.
- **Serialisierungsprogramm\_Initialisierung** und **Serialisierungsprogramm\_Deinit** Initialisierung und Initialisierung aufheben die Bibliothek Serialisierungsprogramm..
- **IoTHubClient\_erstellen** und **IoTHubClient\_löschen** erstellen Sie einen Ziehpunkt Client, **IotHubClientHandle**, die Gerät Anmeldeinformationen für die Verbindung mit Ihrem IoT-Hub verwenden.

Im Hauptteil der Funktion **Remote_monitoring_run** führt das Programm die folgenden Vorgänge mithilfe der **IotHubClientHandle** :

- Erstellt eine Instanz des Modells Thermostat Contoso und die Nachrichten-Callbacks für die beiden Befehle richtet.
- Sendet Informationen über das Gerät selbst, einschließlich der Befehle, die es unterstützt, die können Sie an Ihrem IoT Verteiler mithilfe der Bibliothek Serialisierungsprogramm. Wenn der Hub diese Nachricht empfängt, wird den Gerätestatus im Dashboard von **Ausstehend** in den **ausgeführt**.
- Startet **während** endlos wiedergegeben wird, die Temperatur, externe Temperatur und Luftfeuchtigkeit Werte IoT Hub pro Sekunde sendet an.

Als Referenz hier ist eine Stichprobe **DeviceInfo** Nachricht gesendet an IoT Verteiler beim Start:

```
{
  "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties":
  {
    "DeviceID":"mydevice01", "HubEnabledState":true
  }, 
  "Commands":
  [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

Als Referenz hier ist eine Stichprobe **werden** Nachricht an IoT Hub gesendet werden:

```
{"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
```

Als Referenz so sieht ein Beispiel- **Befehl** von IoT Hub empfangen:

```
{
  "Name":"SetHumidity",
  "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
  "CreatedTime":"2016-03-11T15:09:44.2231295Z",
  "Parameters":{"humidity":23}
}
```

<a id="buildandrun"/>
### <a name="build-and-run-the-program"></a>Erstellen und Ausführen des Programms

1. Klicken Sie auf **Kompilieren** , um das Programm zu erstellen. Sicheres können Sie alle Warnungen ignorieren, aber wenn das Build Fehler erstellt beheben, bevor Sie fortfahren.

2. Wenn die Erstellung erfolgreich ist, wird die Mbed Compiler Website generiert eine bin-Datei mit dem Namen des Projekts und lädt diese auf Ihrem lokalen Computer. Kopieren Sie die bin-Datei auf das Gerät aus. Speichern die bin-Datei in das Gerät bewirkt, dass das Gerät neu starten, und führen Sie das Programm in die bin-Datei enthalten sind. Sie können das Programm manuell zu einem beliebigen Zeitpunkt neu, durch Drücken der Schaltfläche "Zurücksetzen" auf dem Gerät Mbed starten.

3. Verbinden Sie mit dem Gerät SSH-Client, z. B. über kitten. Sie können den seriellen Anschluss bestimmen, die, den Ihrem Gerät verwendet, indem Sie die Windows-Geräte-Manager.

    ![][11]

4. Klicken Sie in kitten auf den **serielle** Verbindungstyp. Das Gerät wird in der Regel am 9600Baud verbindet, geben Sie im Feld **Geschwindigkeit** daher 9600. Klicken Sie dann auf **Öffnen**.

5. Starten des Programms ausführen. Möglicherweise müssen Sie die Karte zurücksetzen (drücken Sie Strg + Pause oder drücken Sie die Karte auf die Schaltfläche "Zurücksetzen"), wenn das Programm für die Verbindung nicht automatisch gestartet wird.

    ![][10]

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[9]: ./media/iot-suite-connecting-devices-mbed/suite6.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
[lnk-serializer]: https://azure.microsoft.com/documentation/articles/iot-hub-device-sdk-c-intro/#serializer
