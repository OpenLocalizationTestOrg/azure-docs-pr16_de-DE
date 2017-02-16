<properties
     pageTitle="Verwenden Sie zum Erstellen einer IoT Hub Azure-Portal | Microsoft Azure"
     description="Eine Übersicht zum Erstellen und Verwalten von Azure IoT Hubs über das Azure-portal"
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

# <a name="create-an-iot-hub-using-the-azure-portal"></a>Erstellen Sie einen IoT Hub über das Azure-portal

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]


## <a name="introduction"></a>Einführung

Dieser Artikel beschreibt, wie den Dienst IoT Hub Azure-Portal zu finden, und wie Sie erstellen und Verwalten von IoT Hubs.

## <a name="where-to-find-iot-hubs"></a>Wo befinden sich IoT Hubs?

Es gibt verschiedene Stellen, wo Sie IoT Hubs finden können.

1. **+ Neu**: **Azure IoT Hub** ist ein IoT-Dienst, und finden Sie in der Kategorie **Internet der Dinge**, unter **+ neu**, ähnlich wie andere Dienste.

2. IoT Hubs können auch über die Marketplace als Hero Dienst unter **Internet der Dinge**zugegriffen werden.

## <a name="create-an-iot-hub"></a>Erstellen Sie einen IoT-hub

Sie können einen IoT Hub mithilfe der folgenden Methoden erstellen:

- Erstellen einen IoT Hub über die Option **+ neu** leads an die Blade im nächsten Screenshot dargestellt. Die Schritte zum Erstellen von IoT-Hub über diese Methode sowie durch die Marketplace sind identisch.

- Erstellen einen IoT Hub über die Marketplace: durch Klicken auf **Erstellen** wird eine Blade, die mit dem vorherigen Blade für die Oberfläche **+ neue** identisch ist. In den nächsten Abschnitten Liste die unterschiedlichen Schritten zum Erstellen eines IoT-Hub an.

### <a name="choose-the-name-of-the-iot-hub"></a>Wählen Sie den Namen der IoT-hub

Um einen Hub IoT erstellen zu können, müssen Sie den Hub benennen. Dieser Name muss für den untergeordneten Servern eindeutig sein. Keine doppelten Hubs ist auf Back-End zulässig, sodass es wird empfohlen, diesen Hub möglichst eindeutig benannt wird.

### <a name="choose-the-pricing-tier"></a>Wählen Sie die Ebene Preisgestaltung

Sie können aus vier Ebenen auswählen: **frei**, **Standard 1** und **2 Standard**und **Standard S3**. Die kostenlose Ebene kann nur 500 Geräte an den Hub IoT und bis zu 8.000 Nachrichten pro Tag verbunden sein.

**Standard S1**: IoT Hubs S1 Edition ist für das IoT Lösungen, die eine große Anzahl von Geräten generieren relativ kleine Datenmengen pro Gerät aufweisen. Jede Einheit der S1 Edition ermöglicht bis zu 400.000 Nachrichten pro Tag auf allen verbundenen Geräten.

**Standard S2**: IoT Hub S2 Edition ist für das IoT Lösungen, in dem Geräte große Datenmengen generieren. Jede Einheit der S2 Edition kann bis zu 6 Millionen Nachrichten pro Tag zwischen allen verbundenen Geräten.

**Standard S3**: IoT Hub S3 Edition ist für das IoT Lösungen, die große Datenmengen generieren. Jede Einheit der S3 Edition kann bis zu 300 Millionen Nachrichten pro Tag zwischen allen verbundenen Geräten.

![][4]

> [AZURE.NOTE] IoT Hub lässt nur eine kostenlose Hub pro Azure-Abonnement.

### <a name="iot-hub-units"></a>IoT Hub Einheiten

Eine IoT Hub Einheit verfügt über eine bestimmte Anzahl von Nachrichten pro Tag. Die Gesamtzahl der Nachrichten, die für diesen Hub unterstützt, wird die Anzahl der Einheiten, die die Anzahl der Nachrichten, die pro Tag für die Leiste multipliziert. Wenn Sie den IoT Hub zur Unterstützung von eingehende Nachrichten 700.000 möchten, wählen Sie zwei S1 Ebene Einheiten.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Gerät Cloud Partitionen und Ressourcengruppe

Sie können die Anzahl der Partitionen einen IoT-Hub ändern. Standardpartitionen werden mit 4 festgelegt. Sie können jedoch unterschiedlich viele Partitionen aus einer Dropdownliste auswählen.

Für die Ressourcengruppen müssen Sie nicht explizit eine leere Ressourcengruppe erstellen. Wenn Sie eine Ressource zu erstellen, können Sie zum Erstellen einer neuen Ressourcengruppe oder verwenden Sie eine vorhandene Ressourcengruppe auswählen.

![][5]

### <a name="choose-subscriptions"></a>Wählen Sie Abonnements

Azure IoT Hub zeigt automatisch die Liste der Azure Abonnements, mit denen das Benutzerkonto verknüpft ist. Sie können eine der Optionen hier IoT-Hub mit dieser Azure-Abonnement zu verbinden auswählen.

### <a name="choose-the-location"></a>Wählen Sie den Speicherort aus.

Die Option Speicherort enthält eine Liste der Regionen IoT Hub angeboten ist. IoT Hub steht an folgenden Speicherorten bereitstellen: Australien Osten, Australien oder, Asien – Osten, Asien oder, Europa Nord-, Europa – Westen, Japan Osten, Westen, uns Osten und uns "Westen" Japan.

### <a name="create-the-iot-hub"></a>Erstellen Sie den IoT-hub

Wenn Sie alle vorherigen Schritte abgeschlossen sind, ist der IoT-Hub bereit sind, erstellt werden. Klicken Sie auf **Erstellen** , die zum Starten des Back-End-Vorgang des Erstellens von diesen IoT-Hub mit bestimmten Optionen, und es an der angegebenen Position bereitstellen.

Es kann einige Minuten dauern für den Hub IoT erstellt werden, wie es dauert für die Back-End-Bereitstellung in den entsprechenden Speicherort Servern ausgeführt.

## <a name="change-the-settings-of-the-iot-hub"></a>Ändern der Einstellungen der IoT-hub

Sie können die Einstellungen eines vorhandenen IoT Hub ändern, nachdem sie aus dem Blade IoT Hub erstellt wurde.

![][8]

**Freigegebene Richtlinien**: Diese Richtlinien für die Berechtigungen für Geräte und Dienste zum Herstellen einer Verbindung IoT Hub mit festgelegt. Sie können diese Richtlinien **Freigegeben-Richtlinien** unter **Allgemein**auf zugreifen. In diesem Blade können Sie vorhandene Richtlinien ändern oder eine neue Richtlinie hinzufügen.

### <a name="create-a-policy"></a>Erstellen einer Richtlinie

- Klicken Sie auf **Hinzufügen** , um ein Blade zu öffnen. Hier können Sie den neuen Richtliniennamen und die Berechtigungen, die Sie bei Verwendung dieser Richtlinie zuordnen, wie in der folgenden Abbildung gezeigt möchten.

    Es gibt verschiedene Berechtigungen, die diese freigegebenen Richtlinien zugeordnet werden können. Die ersten beiden Richtlinien, **Registrierung lesen** und **Schreiben Registrierung**erteilen lesen und Schreiben Zugriffsrechte für das Gerät Identität Store oder der Registrierung Identität. Mit der Option schreiben automatisch wählt auch die finden Sie hier die Option.

    Die Richtlinie **Dienst verbinden** gewährt Zugriffsberechtigung für die Endpunkte Cloud-Seite wie der Gruppe Consumer für Services Herstellen einer Verbindung mit dem IoT-Hub an. Die Richtlinie **Gerät verbinden** erteilt für das Senden und Empfangen von Nachrichten auf den IoT-Hub an den Endpunkten Gerät angeordneten.

- Klicken Sie auf **Erstellen** , um diese neu erstellte Richtlinie der vorhandenen Liste hinzufügen.

![][10]

## <a name="messaging"></a>Messaging

Klicken Sie auf **Messaging** zum Anzeigen einer Liste von messaging Eigenschaften für den IoT-Hub, der geändert wird. Es gibt zwei Hauptarten von Eigenschaften, die Sie ändern oder kopieren können: **Cloud auf Gerät** und des **Geräts für die Cloud**.

- Einstellungen der **Cloud auf Gerät** : Diese Einstellung hat zwei Subsettings: **Cloud Gerät TTL** (Time-to-live) und **Aufbewahrungsrichtlinien Zeit** für die Nachrichten. Wenn der Hub IoT zuerst erstellt wurde, werden beide diese Einstellungen mit einem Standardwert von einer Stunde erstellt. Um diese Werte anzupassen, verwenden Sie die Schieberegler, oder geben Sie die Werte.

- **Cloud** Audiogeräts: Diese Einstellung hat mehrere Subsettings, von denen einige mit dem Namen/zugewiesen werden, wenn der Hub IoT erstellt wird und kann nur auf andere Subsettings, die angepasst werden kopiert werden. Diese Einstellungen werden im nächsten Abschnitt aufgeführt.

**Partitionen**: Dieser Wert wird festgelegt, wenn der Hub IoT erstellt und durch diese Einstellung geändert werden kann.

**Ereignis Hub-kompatible Namen und Endpunkt**: Wenn das IoT Hub erstellt wird, wird ein Ereignis Hub ist, dass Sie Zugriff auf unter bestimmten Umständen möglicherweise intern erstellt. Dieses Ereignis Hub-kompatible Namen und den Endpunkt kann nicht angepasst werden, aber steht für die Verwendung über die Schaltfläche **Kopieren** .

**Aufbewahrungsrichtlinien Zeit**: Legen Sie auf einen Tag standardmäßig aber andere Werte mithilfe der Dropdown-Liste angepasst werden können. Dieser Wert ist in Tagen für Cloud-Gerät und nicht in Stunden, wie ähnlichen Cloud auf Gerät festgelegt ist.

**Consumer Gruppen**: Consumer Gruppen sind eine Einstellung ähnelt anderen messaging-Systemen, die zum Extrahieren von Daten auf bestimmte Weise zu anderen Applikationen oder Services Verbindung IoT Hub verwendet werden können. Jeder IoT Hub wird mit einer Consumer Standardgruppe erstellt. Sie können jedoch hinzufügen oder löschen Consumer Gruppen an Ihre IoT Hubs.

> [AZURE.NOTE] Die Consumer Standardgruppe kann nicht bearbeitet oder gelöscht werden.

![][11]

## <a name="pricing-and-scale"></a>Preise und Dezimalstellen

Die Preise von einem vorhandenen IoT-Hub kann über die **Preise** Einstellungen mit den folgenden Ausnahmen geändert werden:

- In der aktuellen Implementierung kein Hub IoT mit einer kostenlosen SKU Ebenen auf einen der kostenpflichtiges SKU ändern oder umgekehrt.
- In der Azure-Abonnement kann nur eine kostenlose Ebene IoT Hub sein.

![][12]

Verschieben von einer höheren Ebene (S2 oder S3) nach unten Ebene (S1 oder S2) darf nur, wenn die Anzahl der für den betreffenden Tag gesendeten Nachrichten nicht in Konflikt stehen. Angenommen, wenn die Anzahl der Nachrichten, die pro Tag 400.000 überschreitet, kann Klicken Sie dann die Leiste für den Hub IoT geändert werden. Jedoch, wenn Sie auf der Ebene S1 ändern klicken Sie dann im Hub für diesen Tag beschränkt.

## <a name="delete-the-iot-hub"></a>Löschen Sie den IoT-hub

Sie können an den Hub IoT navigieren, die Sie löschen, indem Sie auf **Durchsuchen**klicken, und wählen Sie dann auf den entsprechenden Hub löschen möchten. Klicken Sie auf die Schaltfläche **Löschen** unter dem Hubnamen im Hub zu löschen.

## <a name="next-steps"></a>Nächste Schritte

Führen Sie die folgenden Links, um weitere Informationen zur Verwaltung von Azure IoT Hub:

- [Verwalten von Massen IoT Geräte][lnk-bulk]
- [Verwendung von Kriterien][lnk-metrics]
- [Überwachung von Vorgängen][lnk-monitor]

Die Funktionen von IoT Hub weiteren kann, finden Sie unter:

- [Leitfaden für Entwickler][lnk-devguide]
- [Ein Gerät mit dem IoT Gateway SDK simulieren][lnk-gateway]
- [Sichern Sie Ihre IoT-Lösung von Grund nach oben][lnk-securing]


  [4]: ./media/iot-hub-create-through-portal/create-iothub.png
  [5]: ./media/iot-hub-create-through-portal/location1.png
  [8]: ./media/iot-hub-create-through-portal/portal-settings.png
  [10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
  [11]: ./media/iot-hub-create-through-portal/messaging-settings.png
  [12]: ./media/iot-hub-create-through-portal/pricing-error.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md