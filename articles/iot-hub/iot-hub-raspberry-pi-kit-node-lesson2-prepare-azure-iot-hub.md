<properties
 pageTitle="Erstellen Sie Ihre IoT Hub und Registrieren Ihrer Himbeeren Pi 3 | Microsoft Azure"
 description="Erstellen einer Ressourcengruppe, einen Hub Azure IoT erstellen und Ihrer Pi im Azure IoT Hub mithilfe der CLI Azure registrieren."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="22-create-your-iot-hub-and-register-your-raspberry-pi-3"></a>2.2 Erstellen Sie Ihrer IoT Hub und registrieren Sie der Brombeere Pi 3

## <a name="221-what-you-will-do"></a>2.2.1 mögliche Aktionen werden

- Erstellen Sie eine Ressourcengruppe aus.
- Erstellen Sie Ihre Azure IoT-Hub in der Ressourcengruppe ein.
- Fügen Sie Ihrer Brombeere Pi 3 an den mit der CLI Azure IoT Azure-Hub an hinzu.

Wenn Sie die CLI Azure verwenden, um Ihre Pi an Ihre IoT Verteiler hinzufügen, erstellt der Dienst einen Schlüssel für Ihre Pi authentifizieren mit dem Dienst an. Wenn Sie Probleme mit dem entsprechen, Zielwertsuche Lösungen in die [Seite zu behandeln](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="222-what-you-will-learn"></a>2.2.2 Gelernte wird

- Wie die CLI Azure verwenden, um einen Azure IoT Hub zu erstellen.
- So erstellen Sie eine Gerät Identität für Ihre Pi Ihrer Azure IoT Hub.

## <a name="223-what-you-need"></a>2.2.3, benötigen Sie

- Ein Azure-Konto
- Einem Mac oder einem Windows-Computer mit der Azure CLI installiert

## <a name="224-create-your-azure-iot-hub"></a>2.2.4 Erstellen Sie Ihrer Azure IoT-hub

Azure IoT Hub hilft Ihnen der herstellen, überwachen und Verwalten von Millionen von IoT Posten. Gehen Sie folgendermaßen vor, um Ihre Azure IoT-Hub zu erstellen:

1. Melden Sie sich bei Ihrem Konto Azure durch den folgenden Befehl ausführen:

    ```bash
    az login
    ```

    Ihre verfügbar Azure-Abonnements werden nach einer erfolgreichen Anmeldung aufgeführt.

2. Festlegen der Azure-Abonnement, das Sie verwenden, indem Sie den folgenden Befehl ausführen möchten:

    ```bash
    az account set -n {subscription id or name}
    ```

    Das Abonnement-ID oder den Namen finden Sie in der Ausgabe der `az login`.

3. Registrieren Sie den Anbieter, indem Sie den folgenden Befehl ausführen:

    ```bash
    az resource provider register -n "Microsoft.Devices"
    ```

    Sie müssen den Anbieter registrieren, bevor Sie Azure Ressource bereitstellen können, die der Anbieter bereitstellt.

    > [AZURE.NOTE] Die meisten Anbieter werden automatisch nach dem Azure-Portal oder der Azure-Komponenten, die Sie mit Hilfe sind registriert, aber nicht alle. Weitere Informationen zu den Anbieter finden Sie unter [Behandeln von Azure-Bereitstellung häufigen Azure Ressourcenmanager](../resource-manager-common-deployment-errors.md)

4. Erstellen Sie eine Ressourcengruppe mit dem Namen Iot Stichproben in der Region Westen US, indem Sie den folgenden Befehl ausführen:

    ```bash
    az resource group create --name iot-sample --location westus
    ```

5. Erstellen Sie einen IoT-Hub in der Gruppe Iot Stichproben Ressource, indem Sie den folgenden Befehl ausführen:

    ```bash
    az iot hub create --name {my hub name} --resource-group iot-sample
    ```

    Die Standard-Edition IoT-Hub erstellten **F1**, einer Zahlenmenge ist **kostenlos**. Weitere Informationen finden Sie unter [Azure IoT Hub Preise](https://azure.microsoft.com/pricing/details/iot-hub/).

> [AZURE.NOTE] Der Name des Ihrer IoT Hub muss global eindeutig sein.
>
> Sie können nur eine **F1** -Edition von Azure IoT Hub unter Ihrem Abonnement Azure erstellen.

## <a name="225-register-your-pi-in-your-iot-hub"></a>2.2.5 registrieren Sie Ihrer Pi in Ihrem IoT-hub

Jedes Gerät, das in Ihrer Azure IoT Hub Nachrichten sendet/empfängt muss mit einer eindeutige ID registriert sein

Registrieren Sie Ihre Pi Ihrer Azure IoT-Hub an, indem Sie laufenden folgenden Befehl:

```bash
az iot device create --device-id myraspberrypi --hub {my hub name} --resource-group iot-sample
```

## <a name="226-summary"></a>2.2.6 Zusammenfassung

Erstellt einen Azure IoT Hub und Ihre Pi mit einem Gerät Identität Ihrer Azure IoT Hub registriert haben. Sie sind bereit, mit der nächsten Lektion verschieben, erfahren, wie Sie zum Senden von Nachrichten von Ihrem Pi an Ihre IoT Verteiler.

## <a name="next-steps"></a>Nächste Schritte

Sie nun können zum Starten der Lektion 3, die beginnt mit [einer app Azure-Funktion und ein Konto Azure-Speicher zum Verarbeiten und Speichern von IoT Hub Nachrichten zu erstellen](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).