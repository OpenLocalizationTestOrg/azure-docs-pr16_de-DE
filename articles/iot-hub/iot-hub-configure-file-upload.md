<properties
     pageTitle="Verwenden das Azure-Portal so konfigurieren Sie die Datei hochladen | Microsoft Azure"
     description="Eine Übersicht zum Hochladen von Dateien mit dem Azure-Portal zu konfigurieren"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="configure-file-uploads-using-the-azure-portal"></a>Konfigurieren von Dateiuploads über das Azure-portal

## <a name="file-upload"></a>Hochladen von Dateien

Verwenden der [Datei hochladen Funktionalität IoT Hub][lnk-upload], Sie müssen zuerst ein Konto Azure-Speicher mit Ihrem Hub verknüpfen. Wählen Sie die Einstellungen **Datei hochladen** , um eine Liste der Upload Dateieigenschaften für den IoT-Hub anzuzeigen, die geändert werden.

![][13]

**Speichercontainer**: verwenden das Azure-Portal auswählen ein Containers Blob in einem Speicher Azure-Konto in Ihrem aktuellen Azure-Abonnement, mit Ihrer IoT Hub zugeordnet werden soll. Bei Bedarf können Sie ein Konto Azure-Speicher für den **Speicherkonten** Blade- und Blob Container auf das Blade **Container** erstellen. IoT Hub generiert automatisch SAS-URIs mit Schreibberechtigungen für diesen Container Blob für Geräte zu verwenden, wenn sie Dateien hochladen.

![][14]

**Empfangen von Benachrichtigungen für hochgeladene Dateien**: Aktivieren oder deaktivieren Sie die Datei hochladen Benachrichtigungen über die Umschaltfläche.

**SAS TTL**: Diese Einstellung ist die Time-to-live der SAS-URIs zurückgegebene IoT-Hub an das Gerät. Standardmäßig auf eine Stunde festgelegt, aber andere Werte mit dem Schieberegler angepasst werden können.

**Datei-Benachrichtigung, dass TTL Standardeinstellungen**: Benachrichtigung der Time-to-live eine Datei hochgeladen werden, bevor sie abgelaufen ist. Standardmäßig auf Tag festgelegt, aber andere Werte mit dem Schieberegler angepasst werden können.

**Übermittlung maximale Anzahl von Dateien Benachrichtigung**: die Anzahl der Wiederholungsversuche IoT-Hub vorführen eine Datei hochladen Benachrichtigung. Standardmäßig auf 10 festgelegt, aber andere Werte mit dem Schieberegler angepasst werden können.

![][15]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Funktionen, die Datei Hochladen der IoT Hub, finden Sie unter [Hochladen von Dateien auf einem Gerät] [ lnk-upload] im Developer Guide.

Führen Sie die folgenden Links, um weitere Informationen zur Verwaltung von Azure IoT Hub:

- [Verwalten von Massen IoT Geräte][lnk-bulk]
- [Verwendung von Kriterien][lnk-metrics]
- [Überwachung von Vorgängen][lnk-monitor]

Die Funktionen von IoT Hub weiteren kann, finden Sie unter:

- [Leitfaden für Entwickler][lnk-devguide]
- [Ein Gerät mit dem IoT Gateway SDK simulieren][lnk-gateway]
- [Sichern Sie Ihre IoT-Lösung von Grund nach oben][lnk-securing]


  [13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
  [14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
  [15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md