<properties
   pageTitle="Schließen Sie ein Gerät mit C unter Windows | Microsoft Azure"
   description="Beschreibt, wie ein Gerät mit der Azure IoT Suite vorkonfiguriert remote Überwachung-Lösung mit einer Anwendung in C, auf dem Windows geschrieben verbinden."
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


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Schließen Sie Ihr Gerät in der remote Überwachung vorkonfigurierten Lösung (Windows)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Erstellen einer Stichprobe-Lösung C unter Windows

Die folgenden Schritte gezeigt, wie in Visual Studio verwenden, um eine Clientanwendung in C, mit denen die Lösung Remote-Überwachung vorkonfiguriert kommuniziert geschriebene zu erstellen.

Erstellen eines Startprojekts in Visual Studio 2015 und Hinzufügen der IoT Hub Gerät Client NuGet-Paketen:

1. Erstellen Sie in Visual Studio 2015 eine C mithilfe der Vorlage Visual C++ **Win32-Console-Anwendung** . Nennen Sie das Projekt **RMDevice**.

2. Auf der Einstellungsseite **Applications** im **Win32-Anwendung-Assistenten**stellen Sie sicher, dass **Console-Anwendung** ausgewählt ist, und deaktivieren Sie **Vorkompilierte Header** und **Security Development Lifecycle (SDL) überprüft**.

3. Löschen Sie im **Explorer Lösung**die Dateien stdafx.h, targetver.h und stdafx.cpp aus.

4. Benennen Sie im **Explorer Lösung**die Datei RMDevice.cpp in RMDevice.c aus.

5. **Lösung-Explorer**mit der rechten Maustaste auf das Projekt **RMDevice** und dann auf **Verwalten NuGet-Pakete**. Klicken Sie auf **Durchsuchen**, und klicken Sie dann suchen Sie, und installieren Sie die folgenden NuGet Pakete in das Projekt:

    - Microsoft.Azure.IoTHub.Serializer
    - Microsoft.Azure.IoTHub.IoTHubClient
    - Microsoft.Azure.IoTHub.HttpTransport

6. Im- **Lösung-Explorer**mit der rechten Maustaste auf das Projekt **RMDevice** , und klicken Sie dann auf **Eigenschaften** , um des Projekts, die im Dialogfeld **Eigenschaften** zu öffnen. Details finden Sie unter [Festlegen von Visual C++-Projekteigenschaften][lnk-c-project-properties]. 

7. Klicken Sie auf den Ordner **Linker** , und klicken Sie auf die Eigenschaftenseite **Eingabe** .

8. Die Eigenschaft **Zusätzliche Abhängigkeiten** **crypt32.lib** hinzugefügt. Klicken Sie auf **OK** und dann auf **OK** erneut zum Speichern des Projekts Immobilienwerte.

## <a name="specify-the-behavior-of-the-iot-hub-device"></a>Geben Sie das Verhalten des Geräts IoT-Hub

Die IoT Hub Client-Bibliotheken verwenden ein Modell zum Angeben des Formats der Nachrichten, die das Gerät sendet IoT Hub und die Befehle, die sie von IoT Hub erhält.

1. Öffnen Sie die Datei RMDevice.c in Visual Studio. Ersetzen Sie das vorhandene `#include` Anweisungen in Kombination mit den folgenden Code:

    ```
    #include "iothubtransporthttp.h"
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
    static const char* hubName = "[IoTHub Name]";
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

## <a name="implement-the-behavior-of-the-device"></a>Implementieren das Verhalten des Geräts

Fügen Sie nun Code, das im Modell definierten Verhalten implementiert.

1. Fügen Sie die folgenden Funktionen, die ausgeführt werden, wenn das Gerät die Befehle **SetTemperature** und **SetHumidity** von IoT Hub empfängt hinzu:

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
        config.iotHubName = hubName;
        config.iotHubSuffix = hubSuffix;
        config.protocol = HTTP_Protocol;
        config.deviceSasToken = NULL;
        config.protocolGatewayHostName = NULL;
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

5. Ersetzen der **main** -Funktion mit den folgenden Code zum Aufrufen der Funktion **Remote_monitoring_run** an:

    ```
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

6. Klicken Sie auf **Erstellen** und dann **Lösung erstellen** , wenn Sie die Anwendung Gerät zu erstellen.

7. Im **Explorer Lösung**mit der rechten Maustaste in des Projekts **RMDevice** , klicken Sie auf **Debuggen**, und klicken Sie dann auf **neue Instanz starten** , um das Beispiel auszuführen. Die Konsole zeigt Nachrichten die Anwendung sendet Beispiel werden an IoT Verteiler und Befehle empfängt.

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx