<properties
    pageTitle="Erste Schritte mit der IoT Hub Gateway SDK | Microsoft Azure"
    description="Diese Anleitung Azure IoT Gateway SDK verwendet Linux Konzepte erläutern, die Sie kennen sollten, wenn Sie das Azure IoT Gateway SDK verwenden."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-linux"></a>Azure IoT Gateway SDK (Beta) - erste Schritte mit Linux

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>So erstellen Sie das Beispiel

Bevor Sie beginnen, müssen Sie [Einrichten Ihrer Entwicklungsumgebung] [ lnk-setupdevbox] für die Arbeit mit dem SDK unter Linux.

1. Öffnen Sie eine Shell.
2. Navigieren Sie zu dem Stammordner in Ihrer lokalen Kopie des Repositorys **Azure-Iot-Gateway-Sdk** .
3. Führen Sie das Skript **tools/build.sh** . Dieses Skript verwendet das Programm **Cmake** , erstellen einen Ordner im Stammordner Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** Repository namens **Erstellen** und Generieren eines Makefiles. Das Skript erstellt die Lösung und führt die Tests.

> [AZURE.NOTE]  Jedes Mal, wenn Sie das Skript **build.sh** ausführen, wird gelöscht und erstellt dann den Ordner **Erstellen** im Stammordner Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** Repository neu.

## <a name="how-to-run-the-sample"></a>So führen Sie das Beispiel

1. Das Skript **build.sh** generiert deren Ausgabe in den Ordner **Erstellen** in der lokalen Kopie des Repositorys **Azure-Iot-Gateway-Sdk** . Dies umfasst zwei Module in diesem Beispiel verwendete.

    Das Skript platziert **liblogger_hl.so** in der **Erstellen/Module/Protokollierung/** Ordner und **libhello_world_hl.so** in der **Erstellen/Module/Hello_world/** Ordner. Verwenden Sie diese Pfade für den **Pfad zum Modul** Wert ein, wie in der nachstehenden JSON-Einstellungsdatei dargestellt.

2. Die Datei **hello_world_lin.json** im Ordner **Beispiele/Hello_world/Src** ist ein Beispiel für JSON-Einstellungsdatei für Linux, die Sie verwenden können, um das Beispiel auszuführen. Im unten gezeigten Beispiel JSON-Einstellungen wird davon ausgegangen, dass Sie die Stichprobe der Stammebene Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** Repository ausgeführt werden.

3. Legen Sie den **Filename** -Wert für das Modul **Logger_hl** im Abschnitt **Args** auf den Namen und den Pfad der Datei, die die Daten enthalten sollen.

    Dies ist ein Beispiel für eine JSON-Einstellungsdatei für Linux, die in der **log.txt** in den Ordner schreibt, in dem Sie das Beispiel ausführen.

    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "module path" : "./build/modules/logger/liblogger_hl.so",
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "./build/modules/hello_world/libhello_world_hl.so",
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```

3. Navigieren Sie in Ihre Shell zur **Azure-Iot-Gateway-Sdk** -Ordner.
4. Führen Sie den folgenden Befehl ein:
  
  ```
  ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
  ``` 

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
