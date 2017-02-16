<properties
   pageTitle="Schließen Sie ein Gerät mit C auf Linux | Microsoft Azure"
   description="Beschreibt, wie ein Gerät mit der Azure IoT Suite vorkonfiguriert remote Überwachung-Lösung mit einer Anwendung geschrieben in C Linux ausgeführt verbinden."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-linux"></a>Schließen Sie Ihr Gerät in der remote Überwachung vorkonfigurierten Lösung (Linux)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a>Erstellen und Ausführen eines Stichprobe C Clients Linux

Die folgenden Verfahren zeigen Sie zum Erstellen einer Clientanwendung in C geschrieben und erstellt, und führen Sie unter Ubuntu Linux kommuniziert, auf denen die remote Überwachung vorkonfigurierten Lösung. Wenn Sie diese Schritte ausführen zu können, benötigen Sie ein Gerät unter Ubuntu Version 15.04 oder 15.10. Installieren Sie bevor Sie fortfahren erforderlichen Pakete auf Ihrem Ubuntu-Gerät mit dem folgenden Befehl aus:

```
sudo apt-get install cmake gcc g++
```

## <a name="install-the-client-libraries-on-your-device"></a>Installieren Sie die Clientbibliotheken auf Ihrem Gerät

Die Azure IoT Hub Client-Bibliotheken stehen als ein Paket, die Sie auf Ihrem Ubuntu-Gerät mit dem **apt-Get** -Befehl installieren können. Führen Sie die folgenden Schritte aus, um das Paket zu installieren, das die IoT Hub Client-Bibliothek und Header-Dateien auf Ihrem Computer Ubuntu enthält:

1. Fügen Sie auf dem Computer AzureIoT Repository hinzu:

    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```

2. Installieren Sie das Paket Azure-Iot-Sdk-c-Entwickler

    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="add-code-to-specify-the-behavior-of-the-device"></a>Hinzufügen von Code, um das Verhalten des Geräts anzugeben.

Erstellen Sie einen Ordner auf Ihrem Computer Ubuntu **remote\_Überwachung**. In der **remote\_Überwachung** Ordner erstellen, die vier Dateien **main.c** **remote\_monitoring.c**, **remote\_monitoring.h**, und **CMakeLists.txt**.

Die IoT Hub Serialisierungsprogramm Client-Bibliotheken verwenden ein Modell zum Angeben des Formats von Nachrichten, die das Gerät sendet IoT Hub und die Befehle, die sie von IoT Hub erhält.

1. Öffnen Sie in einem Text-Editor, die **remote\_monitoring.c** Datei. Fügen Sie den folgenden `#include` Anweisungen:

    ```
    #include "iothubtransportamqp.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    ```

2. Fügen Sie die folgenden Variablen Deklarationen nach der `#include` Anweisungen. Ersetzen Sie die Platzhalterwerte [Geräte-Id] und [Gerät Key] mit Werten für Ihr Gerät aus dem remote Überwachung Lösung Dashboard. Verwenden Sie die IoT Hub Hostname aus dem Dashboard, um [Name der IoTHub] ersetzen. Beispielsweise ist Ihre IoT Hub Hostname **contoso.azure-devices.net**, ersetzt [Name der IoTHub] mit **Contoso**:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "IoTHub Name]";
    static const char* hubSuffix = "azure-devices.net";
    ```

3. Fügen Sie den folgenden Code ein, um das Modell zu definieren, das können das Gerät zur Kommunikation mit IoT Hub, hinzu. Dieses Modell gibt an, dass das Gerät Temperatur, externe Temperaturen, Feuchtigkeit und Geräte-Id als werden sendet. Das Gerät sendet auch Metadaten über das Gerät an IoT Verteiler, einschließlich einer Liste der Befehle, die das Gerät unterstützt. Dieses Gerät reagiert auf die Befehle **SetTemperature** und **SetHumidity**:

    ```
    // Define the Model
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

### <a name="add-code-to-implement-the-behavior-of-the-device"></a>Hinzufügen von Code, um das Verhalten des Geräts implementieren

Fügen Sie die Funktionen nicht ausführen, wenn das Gerät einen Befehl aus dem Hub und der Code simulierten werden an den Hub senden erhält.

1. Fügen Sie die folgenden Funktionen, die ausgeführt werden, wenn das Gerät die **SetTemperature** und **SetHumidity** Befehle, die im Modell definierten empfängt hinzu:

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

2. Fügen Sie die folgende Funktion, die eine Nachricht an IoT Hub sendet:

    ```
    static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
    {
      IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
      if (messageHandle == NULL)
      {
        printf("unable to create a new IoTHubMessage\r\n");
      }
      else
      {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, NULL, NULL) != IOTHUB_CLIENT_OK)
        {
          printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
          printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
      }
    free((void*)buffer);
    }
    ```

3. Fügen Sie die folgende Funktion, die die Bibliothek Serialisierung im SDK verknüpft:

    ```
    static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
    {
      IOTHUBMESSAGE_DISPOSITION_RESULT result;
      const unsigned char* buffer;
      size_t size;
      if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
      {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
      }
      else
      {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
          printf("failed to malloc\r\n");
          result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
          memcpy(temp, buffer, size);
          temp[size] = '\0';
          EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
          result =
            (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
            (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
            IOTHUBMESSAGE_REJECTED;
          free(temp);
        }
      }
      return result;
    }
    ```

4. Fügen Sie die folgende Funktion zum Herstellen einer Verbindung IoT Hub mit, senden und Empfangen von Nachrichten und Trennen vom Hub an. Beachten Sie, wie das Gerät Metadaten über sich selbst, einschließlich der Befehle, die zu beim Herstellen einer Verbindung IoT-Hub unterstützt wird, sendet. Diese Metadaten ermöglicht die Lösung, um den Status des Geräts **ausgeführt** , unter dem Dashboard zu aktualisieren:

    ```
    void remote_monitoring_run(void)
    {
      if (platform_init() != 0)
      {
        printf("Failed to initialize the platform.\r\n");
      }
      else
      {
        if (serializer_init(NULL) != SERIALIZER_OK)
        {
          printf("Failed on serializer_init\r\n");
        }
        else
        {
          IOTHUB_CLIENT_CONFIG config;
          IOTHUB_CLIENT_HANDLE iotHubClientHandle;

          config.deviceId = deviceId;
          config.deviceKey = deviceKey;
          config.deviceSasToken = NULL;
          config.iotHubName = hubName;
          config.iotHubSuffix = hubSuffix;
          config.protocol = AMQP_Protocol;
          iotHubClientHandle = IoTHubClient_Create(&config);
          if (iotHubClientHandle == NULL)
          {
            (void)printf("Failed on IoTHubClient_CreateFromConnectionString\r\n");
          }
          else
          {
            Thermostat* thermostat = CREATE_MODEL_INSTANCE(Contoso, Thermostat);
            if (thermostat == NULL)
            {
              (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
            }
            else
            {
              STRING_HANDLE commandsMetadata;

              if (IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, thermostat) != IOTHUB_CLIENT_OK)
              {
                printf("unable to IoTHubClient_SetMessageCallback\r\n");
              }
              else
              {

                /* send the device info upon startup so that the cloud app knows
                   what commands are available and the fact that the device is up */
                thermostat->ObjectType = "DeviceInfo";
                thermostat->IsSimulatedDevice = false;
                thermostat->Version = "1.0";
                thermostat->DeviceProperties.HubEnabledState = true;
                thermostat->DeviceProperties.DeviceID = (char*)deviceId;

                commandsMetadata = STRING_new();
                if (commandsMetadata == NULL)
                {
                  (void)printf("Failed on creating string for commands metadata\r\n");
                }
                else
                {
                  /* Serialize the commands metadata as a JSON string before sending */
                  if (SchemaSerializer_SerializeCommandMetadata(GET_MODEL_HANDLE(Contoso, Thermostat), commandsMetadata) != SCHEMA_SERIALIZER_OK)
                  {
                    (void)printf("Failed serializing commands metadata\r\n");
                  }
                  else
                  {
                    unsigned char* buffer;
                    size_t bufferSize;
                    thermostat->Commands = (char*)STRING_c_str(commandsMetadata);

                    /* Here is the actual send of the Device Info */
                    if (SERIALIZE(&buffer, &bufferSize, thermostat->ObjectType, thermostat->Version, thermostat->IsSimulatedDevice, thermostat->DeviceProperties, thermostat->Commands) != IOT_AGENT_OK)
                    {
                      (void)printf("Failed serializing\r\n");
                    }
                    else
                    {
                      sendMessage(iotHubClientHandle, buffer, bufferSize);
                    }

                  }

                  STRING_delete(commandsMetadata);
                }

                thermostat->Temperature = 50;
                thermostat->ExternalTemperature = 55;
                thermostat->Humidity = 50;
                thermostat->DeviceId = (char*)deviceId;

                while (1)
                {
                  unsigned char*buffer;
                  size_t bufferSize;

                  (void)printf("Sending sensor value Temperature = %d, Humidity = %d\r\n", thermostat->Temperature, thermostat->Humidity);

                  if (SERIALIZE(&buffer, &bufferSize, thermostat->DeviceId, thermostat->Temperature, thermostat->Humidity, thermostat->ExternalTemperature) != IOT_AGENT_OK)
                  {
                    (void)printf("Failed sending sensor value\r\n");
                  }
                  else
                  {
                    sendMessage(iotHubClientHandle, buffer, bufferSize);
                  }

                  ThreadAPI_Sleep(1000);
                }
              }

              DESTROY_MODEL_INSTANCE(thermostat);
            }
            IoTHubClient_Destroy(iotHubClientHandle);
          }
          serializer_deinit();
        }
        platform_deinit();
      }
    }
    ```
    
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

### <a name="add-code-to-invoke-the-remotemonitoringrun-function"></a>Hinzufügen von Code zum Aufrufen der Remote_monitoring_run-Funktion

Öffnen Sie in einem Text-Editor die **remote_monitoring.h** -Datei ein. Fügen Sie den folgenden Code ein:

```
void remote_monitoring_run(void);
```

Öffnen Sie in einem Texteditor **main.c** Datei ein. Fügen Sie den folgenden Code ein:

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="use-cmake-to-build-the-client-application"></a>Verwenden Sie zum Erstellen der Clientanwendung CMake

Die folgenden Schritte beschreiben, wie Sie *CMake* zum Erstellen der Clientanwendung.

1. Öffnen Sie in einem Text-Editor die Datei **CMakeLists.txt** im Ordner " **Remote_monitoring** " ein.

2. Fügen Sie die folgenden Anweisungen zum Erstellen der Clientanwendung definieren hinzu:

    ```
    cmake_minimum_required(VERSION 2.8.11)

    set(AZUREIOT_INC_FOLDER ".." "/usr/include/azureiot")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_amqp_transport
        aziotsharedutil
        uamqp
        pthread
        curl
        ssl
        crypto
    )
    ```

3. Erstellen Sie im Ordner **Remote_monitoring** einen Ordner zum Speichern von Dateien *vornehmen* , die CMake generiert, und führen Sie die Befehle **Cmake** , und **nehmen Sie** dann wie folgt aus:

    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

4. Führen Sie die Clientanwendung und senden Sie werden an IoT Hub zu:

    ```
    ./sample_app
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

