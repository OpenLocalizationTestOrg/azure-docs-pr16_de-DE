<properties
    pageTitle="Erste Schritte mit der IoT Hub Gateway SDK | Microsoft Azure"
    description="Azure IoT Gateway SDK Anleitung für die Verwendung von Windows um zu veranschaulichen Konzepte, dass Sie bei Verwendung der Azure IoT Gateway SDK vertraut sein sollten."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-windows"></a>Azure IoT Gateway SDK (Beta) - erste Schritte mit Windows

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>So erstellen Sie das Beispiel

Bevor Sie beginnen, müssen Sie [Einrichten Ihrer Entwicklungsumgebung] [ lnk-setupdevbox] für die Arbeit mit dem SDK unter Windows.

1. Öffnen Sie ein Eingabeaufforderungsfenster **Entwicklertools Eingabeaufforderungsfenster für VS2015** .
2. Navigieren Sie zu dem Stammordner in Ihrer lokalen Kopie des Repositorys **Azure-Iot-Gateway-Sdk** .
3. Führen Sie die **Tools\\build.cmd** Skript. Dieses Skript erstellt eine Visual Studio-Lösung-Datei, die Lösung erstellt und führt die Tests. Sie können die Visual Studio-Lösung in den Ordner **Erstellen** in Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** Repository suchen.

## <a name="how-to-run-the-sample"></a>So führen Sie das Beispiel

1. Das **build.cmd** -Skript erstellt einen Ordner in der lokalen Kopie des Repositorys namens **Erstellen** . Diesen Ordner enthält, die in diesem Beispiel verwendete beiden Module.

    Das Skript platziert **logger_hl.dll** in der **Erstellen\\Module\\Protokollierung\\Debuggen** Ordner und **hello_world_hl.dll** in der **Erstellen\\Module\\Hello_world\\Debuggen** Ordner. Verwenden Sie diese Pfade für den **Pfad zum Modul** Wert ein, wie in der nachstehenden JSON-Einstellungsdatei dargestellt.

2. Die Datei **hello_world_win.json** in der **Beispiele\\Hello_world\\Src** Ordner ist ein Beispiel für JSON-Einstellungsdatei für Windows, die Sie verwenden können, um das Beispiel auszuführen. Im unten gezeigten Beispiel JSON-Einstellungen wird davon ausgegangen, dass Sie das Repository IoT Gateway SDK werden im Stammordner Ihrer Laufwerk **C:** geklont. Wenn Sie es an eine andere Position heruntergeladen haben, müssen Sie den **Pfad zum Modul** Werte in Anpassen der **Beispiele\\Hello_world\\Src\\hello_world_win.json** entsprechend ablegen.

3. Legen Sie den **Filename** -Wert für das Modul **Logger_hl** im Abschnitt **Args** auf den Namen und den Pfad der Datei, die die Daten enthalten sollen.

    Dies ist ein Beispiel für eine JSON-Einstellungsdatei für Windows, die in die Datei **log.txt** im Stammverzeichnis von Laufwerk **C:** schreibt.

    ```
    {
      "modules" :
      [
        {
          "module name" : "logger_hl",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
          "args" : 
          {
            "filename":"C:\\log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\\\modules\\hello_world\\Debug\\hello_world_hl.dll",
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

3. Navigieren Sie über die Befehlszeile zu dem Stammordner Ihrer lokalen Kopie der **Azure-Iot-Gateway-Sdk** Repository.
4. Führen Sie den folgenden Befehl ein:
  
  ```
  build\samples\hello_world\Debug\hello_world_sample.exe samples\hello_world\src\hello_world_win.json
  ```

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md