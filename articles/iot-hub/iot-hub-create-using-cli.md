<properties
    pageTitle="Erstellen einer IoT Hub mit Azure CLI | Microsoft Azure"
    description="Führen Sie in diesem Artikel, um eine Schnittstelle der Azure Line IoT-Hub zu erstellen."
    services="iot-hub"
    documentationCenter=".net"
    authors="BeatriceOltean"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="multiple"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/21/2016"
     ms.author="boltean"/>

# <a name="create-an-iot-hub-using-azure-cli"></a>Erstellen einer mit Azure CLI IoT-Hub

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Einführung

Sie können Azure Line Schnittstelle verwenden, erstellen und Verwalten von Azure IoT Hubs programmgesteuert. In diesem Artikel wird gezeigt, wie die CLI Azure verwenden, um einen IoT-Hub zu erstellen.

Zum Abschluss des Lernprogramms benötigen Sie Folgendes:

- Ein aktives Azure-Konto. Wenn Sie kein Konto haben, können Sie ein [kostenloses Konto] erstellen[ lnk-free-trial] in nur ein paar Minuten.
- [Azure CLI 0.10.4] [ lnk-CLI-install] oder höher. Wenn Sie Azure CLI bereits können Sie die aktuelle Version über die Befehlszeile ausführen, mit dem folgenden Befehl überprüfen:
```
    azure --version
```

> [AZURE.NOTE] Azure weist zwei verschiedenen Bereitstellungsmodelle für das Erstellen und Arbeiten mit Ressourcen: [Azure Ressourcenmanager und Classic](../resource-manager-deployment-model.md). Die CLI Azure muss im Ressourcenmanager Azure-Modus ist:
```
    azure config mode arm
```

## <a name="set-your-azure-account-and-subscription"></a>Festlegen der Azure-Konto und Ihr Abonnement 

1. Bei der Anmeldung Eingabeaufforderungsfenster, indem Sie den folgenden Befehl eingeben
```
    azure login
```
Verwenden Sie den vorgeschlagenen Webbrowser und Code zum Authentifizieren ein.

2. Wenn Sie mehrere Azure-Abonnements verfügen, wird das Herstellen einer Verbindung mit Azure Zugriff auf alle Ihre Anmeldeinformationen zugeordnet Azure-Abonnements erteilen. Sie können Anzeigen der Azure-Abonnements sowie die standardmäßig wird mit dem Befehl
```
    azure account list 
```

Den Abonnement Kontext festlegen, unter dem Sie die restlichen Befehle verwenden ausführen möchten

```
    azure account set <subscription name>
```

3. Wenn Sie nicht über eine Ressourcengruppe verfügen, können Sie einen benannten **ExampleResourceGroup** erstellen 
```
    azure group create -n exampleResourceGroup -l westus
```

> [AZURE.TIP] [Verwenden Sie die Azure CLI zum Verwalten von Azure Ressourcen und Ressourcengruppe] Artikel[ lnk-CLI-arm] finden Sie weitere Informationen zur Verwendung von Azure CLI Azure Ressourcen verwalten. 


## <a name="create-an-iot-hub"></a>Erstellen einer IoT-Hub

Erforderliche Parameter:

```
 azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>  
    - <resourceGroup> The resource group name (case insensitive alphanumeric, underscore and hyphen, 1-64 length)
    - <name> (The name of the IoT hub to be created. The format is case insensitive alphanumeric, underscore and hyphen, 3-50 length )
    - <location> (The location (azure region/datacenter) where the IoT hub will be provisioned.
    - <sku-name> (The name of the sku, one of: [F1, S1, S2, S3] etc. For the latest full list refer to the pricing page for IoT Hub.
    - <units> (The number of provisioned units. Range : F1 [1-1] : S1, S2 [1-200] : S3 [1-10]. IoT Hub units are based on your total message count and the number of devices you want to connect.)
```
Wenn Sie alle Parameter finden Sie unter verfügbar für die Erstellung der Befehl ' Hilfe ' im Eingabeaufforderungsfenster können Sie
```
    azure iothub create -h 
```
Kurzes Beispiel:

 Zum Erstellen einer IoT Hub Objekt namens **ExampleIoTHubName** in der Ressource Gruppe **ExampleResourceGroup** einfach den folgenden Befehl ausführen
```
    azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [AZURE.NOTE] Dieser Befehl Azure CLI erstellt einen S1 Standard IoT Hub für die Rechnung gestellt werden. Sie können die IoT Hub **ExampleIoTHubName** mit folgendem Befehl löschen. 
```
    azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
```


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Entwickeln für IoT Hub, probieren Sie Folgendes ein:
- [IoT Hub SDKs][lnk-sdks]

Die Funktionen von IoT Hub weiteren kann, finden Sie unter:

- [Verwenden des Azure-Portals IoT Hub verwalten][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]: ../xplat-cli-install.md
[lnk-rest-api]: https://msdn.microsoft.com/library/mt589014.aspx
[lnk-CLI-arm]: ../xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
