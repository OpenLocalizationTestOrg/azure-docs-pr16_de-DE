<properties
    pageTitle="Anpassen von Lösungen vorkonfiguriert | Microsoft Azure"
    description="Enthält Anleitungen zum Anpassen der Azure IoT Suite vorkonfiguriert Lösungen."
    services=""
    suite="iot-suite"
    documentationCenter=".net"
    authors="aguilaaj"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/11/2016"
     ms.author="aguilaaj"/>

# <a name="customize-a-preconfigured-solution"></a>Anpassen einer vorkonfigurierten Lösung

Die vorkonfigurierten Solutions zur Verfügung gestellt, mit der Azure IoT Suite führen Sie die Dienste innerhalb der Suite zusammenarbeiten, um eine End-to-End-Lösung vorführen vor. Aus diesem Ausgangspunkt gibt es eine Vielzahl von Stellen, in denen Sie erweitern und die Lösung für bestimmte Szenarien anpassen können. Den folgenden Abschnitten werden diese allgemeine Anpassungspunkte.

## <a name="finding-the-source-code"></a>Suchen den Quellcode

Der Quellcode für den vorkonfigurierten Lösungen ist auf GitHub in den folgenden Repositorys zur Verfügung:

- Remote-Überwachung: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
- Vorhersagen Wartung: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)

Der Quellcode für den vorkonfigurierten Lösungen werden bereitgestellt, um zu veranschaulichen, das gewünschte Muster und Methoden verwendet, um die End-to-End-Funktionalität einer IoT-Lösung mit Azure IoT Suite implementieren. Sie können weitere Informationen zum Erstellen und Bereitstellen der Lösungen in den Repositorys GitHub suchen.

## <a name="changing-the-preconfigured-rules"></a>Ändern die vorkonfigurierten Regeln

Die Überwachung remote-Lösung umfasst drei [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) Einzelvorgänge zum Implementieren der Gerät Informationen, werden und Regeln Logik für die Lösung angezeigt.

Die drei Stream Analytics Einzelvorgänge und deren Syntax wird in [Remote Überwachung vorkonfigurierten Lösung Exemplarische Vorgehensweise](iot-suite-remote-monitoring-sample-walkthrough.md)detailliert beschrieben. 

Sie können diese Aufträge direkt, um die Logik alter bearbeiten oder Hinzufügen von Logik bestimmte zu Ihrem Szenario. Sie können die Einzelvorgänge Stream Analytics wie folgt finden:
 
1. Wechseln Sie zur [Azure-Portal](https://portal.azure.com)an.
2. Navigieren Sie zu der Ressourcengruppe mit demselben Namen wie Ihre IoT-Lösung. 
3. Wählen Sie den Auftrag Azure Stream Analytics aus, die, den Sie ändern möchten. 
4. Beenden Sie den Auftrag, indem Sie in der Gruppe von Befehlen **Beenden**auswählen. 
5. Bearbeiten Sie die Eingaben, Abfrage, und Ausgaben.

    Eine einfache Änderung besteht darin, ändern Sie die Abfrage für das Projekt **Regeln** Verwenden eines **"<"** statt einer **">"**. Das Lösung Portal wird weiterhin angezeigt **">"** Wenn Sie eine Regel bearbeiten, aber Sie sehen, wird das Verhalten aufgrund der Änderung in der zugrunde liegenden Auftrag gekippt.

6. Starten Sie den Auftrag

> [AZURE.NOTE] Das remote Überwachung Dashboard hängt von bestimmter Daten so ändern die Einzelvorgänge das Dashboard zum Fehlschlagen verursachen kann.

## <a name="adding-your-own-rules"></a>Hinzufügen Ihrer eigenen Regeln

Zusätzlich zum Ändern der vorkonfigurierten Azure Stream Analytics Aufträge, können Sie das Azure-Portal Hinzufügen neuer Aufträge oder neue Abfragen von bestehenden Projekte.

## <a name="customizing-devices"></a>Anpassen von Geräten

Einer der am häufigsten verwendeten Erweiterungsaktivitäten arbeitet mit Ihrem Szenario-spezifischen Geräte. Es gibt mehrere Methoden zum Arbeiten mit Geräten aus. Diese Methoden gehören Ändern eines simulierten Gerät aus, um Ihrem Szenario entsprechen, oder Verwenden des [IoT Gerät SDK][] zum Verbinden von Ihrem Geräts physischen zur Lösung.

Eine schrittweise Anleitung zum Hinzufügen von Geräten zur Überwachung remote vorkonfigurierten Lösung finden Sie unter [Herstellen einer Verbindung Iot Suite-Geräte](iot-suite-connecting-devices.md) und der [Remote Überwachung C SDK Stichprobe](https://github.com/Azure/azure-iot-sdks/tree/master/c/serializer/samples/remote_monitoring) , die für die Arbeit mit den entfernten Überwachung vorkonfigurierten Lösung ausgelegt ist.

### <a name="creating-your-own-simulated-device"></a>Erstellen eines eigenen eines simulierten Gerät

Im Lieferumfang des remote Überwachung Lösung Quellcode (über verwiesen), wird eine .NET Simulator. Diese Simulator ist eine nach der Bereitstellung als Teil der Lösung und zum Senden von unterschiedlichen Metadaten, werden oder Beantworten von anderen Befehle geändert werden kann.

Der vorkonfigurierte Simulator in die remote Überwachung vorkonfigurierten Lösung ist ein besser Gerät, die Temperatur und Feuchtigkeit werden, gibt Sie können den Simulator im Projekt [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) ändern, wenn Sie das GitHub Repository gespalten haben.

### <a name="available-locations-for-simulated-devices"></a>Verfügbare Speicherorte für simulierten Geräten

Die Standardgruppe von Speicherorte ist in Seattle Redmond, Washington, USA. Sie können diese Orte in [SampleDeviceFactory.cs]ändern[lnk-sample-device-factory].


### <a name="building-and-using-your-own-physical-device"></a>Erstellen und Verwenden von Ihrem eigenen (physischen) Gerät

Die [Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks) bieten Bibliotheken zum Herstellen einer Verbindung zahlreiche Gerät Diagrammtypen (Sprachen und Betriebssysteme) in IoT Lösungen.

## <a name="modifying-dashboard-limits"></a>Ändern von Speichergrenzwerten für dashboard

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Anzahl der in der Dropdownliste "Dashboard" angezeigten Geräte

Die Standardeinstellung ist 200. Sie können diese Nummer in [DashboardController.cs]ändern[lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Anzahl der Stifte Bing-Karte Steuerelement anzuzeigende

Die Standardeinstellung ist 200. Sie können diese Nummer in [TelemetryApiController.cs]ändern[lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Zeitraum des Diagramms werden

Die Standardeinstellung ist 10 Minuten. Sie können dies ändern, in der [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="manually-setting-up-application-roles"></a>Manuelles Einrichten Anwendungsrollen

Im folgenden wird beschrieben, wie eine Lösung vorkonfigurierten **Administrator** und **schreibgeschützt** Anwendungsrollen hinzugefügt. Beachten Sie, dass vorkonfigurierte Lösungen, die nach der Bereitstellung bereits von der Website azureiotsuite.com die Rollen **Administrator** und **schreibgeschützt** sind.

Mitglieder der Rolle **schreibgeschützt** können finden Sie unter dem Dashboard und der Geräteliste, aber Sie sind nicht berechtigt, Geräte hinzufügen, ändern die Geräteattribute oder Befehle senden.  Die Rolle des **Administrators** Mitglieder haben Vollzugriff auf alle Funktionen, in der Lösung.

1. Wechseln Sie zu der [Azure klassischen Portal][lnk-classic-portal].

2. Wählen Sie aus **Active Directory**.

3. Klicken Sie auf den Namen der AAD Mandanten, die Sie verwendet werden, wenn Sie nach der Bereitstellung der Lösung.

4. Klicken Sie auf **Anwendungen**.

5. Klicken Sie auf den Namen der Anwendung, die Ihren Namen vorkonfigurierten Lösung entspricht. Wenn Sie eine Anwendung in der Liste angezeigt werden, wählen **Applikationen wurde von meiner Firma zugegriffen** in der **Anzeigen** Dropdown-Liste, und klicken Sie auf das Häkchen.

6.  Klicken Sie am unteren Rand der Seite auf **Manifest verwalten** , und klicken Sie dann auf **Manifest herunterladen**.

7. Dies wird eine .json-Datei auf den lokalen Computer heruntergeladen.  Öffnen Sie diese Datei zur Bearbeitung in einem Text-Editor Ihrer Wahl.

8. Klicken Sie auf der dritten Zeile der Datei .json finden Sie folgende Angaben:

  ```
  "appRoles" : [],
  ```
  Ersetzen Sie dies durch den folgenden Code ein:

  ```
  "appRoles": [
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Administrator access to the application",
  "displayName": "Admin",
  "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
  "isEnabled": true,
  "value": "Admin"
  },
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Read only access to device information",
  "displayName": "Read Only",
  "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
  "isEnabled": true,
  "value": "ReadOnly"
  } ],
  ```

9. Speichern Sie die aktualisierte .json-Datei (Sie können die vorhandene Datei überschreiben).

10.  Wählen Sie in der Azure-Verwaltungsportal am unteren Rand der Seite **Manifest verwalten** und **Manifest hochladen** die .json Datei hochladen, die Sie im vorherigen Schritt gespeichert haben.

11. Sie haben nun die Rollen **Administrator** und **schreibgeschützt** an Ihrer Anwendung hinzugefügt.

12. Wenn Sie eine der folgenden Rollen eines Benutzers in Ihrem Verzeichnis zuweisen, finden Sie unter [Berechtigungen auf der Website azureiotsuite.com][lnk-permissions].

## <a name="feedback"></a>Feedback

Sie haben eine Anpassung möchten Sie finden in diesem Dokument verdeckte? Hinzufügen von Bitte Feature Vorschläge zu [Benutzer Sprach-](https://feedback.azure.com/forums/321918-azure-iot)oder Kommentar in diesem Artikel aus. 

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über die Optionen zum Anpassen der vorkonfigurierten Lösungen, finden Sie unter:

- [Verbinden von Logik App zu Ihrer Lösung Azure IoT Suite Remote-Überwachung vorkonfiguriert][lnk-logicapp]
- [Verwenden von dynamischen werden mit der remote-Überwachung vorkonfiguriert Lösung][lnk-dynamic]
- [Gerät Informations-Metadaten in die Überwachung vorkonfiguriert Lösung][lnk-devinfo]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[IoT Gerät SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
