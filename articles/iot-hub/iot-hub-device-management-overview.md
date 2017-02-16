<properties
 pageTitle="IoT Hub Gerät Management Übersicht | Microsoft Azure"
 description="Dieser Artikel bietet einen Überblick über die Verwaltung Azure IoT Hub Geräte: Enterprise Gerät Lebenszyklus Neustart Factory zurücksetzen Firmwareupdate Konfiguration Gerät im Vergleich Abfragen, Aufträge"
 services="iot-hub"
 documentationCenter=""
 authors="bzurcher"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/03/2016"
 ms.author="bzurcher"/>

# <a name="overview-of-azure-iot-hub-device-management-preview"></a>Übersicht über Azure IoT Hub Gerätemanagement (Preview)

## <a name="introduction"></a>Einführung

Azure IoT Hub stellt die Features und ein Erweiterungsmodell, mit denen Gerät und Back-End-Entwickler zuverlässigen IoT Gerät Management Lösung erstellen können. IoT Geräte Bereich von eingeschränkten Sensoren und einzelnen Zweck Mikrocontroller, um leistungsstarke Gateways, die Kommunikation für Gruppen von Geräten weiterleiten.  Darüber hinaus variieren Fällen verwenden und Anforderungen für IoT Operatoren erheblich in Branchen.  Trotz dieser Variante bietet Azure IoT Hub Gerätemanagement-Funktionen, Muster und Code-Bibliotheken, um eine Reihe von Geräten und Endbenutzer Funktionstypen zu behandeln.

Wichtiger Bestandteil der Erstellen eines erfolgreichen Unternehmens IoT Lösung, um eine Strategie zum wie Operatoren fortlaufende Verwaltung von deren Auflistung von Geräten behandeln bereitzustellen. IoT Operatoren erfordern einfach und zuverlässig Tools und Anwendungen, mit denen sie den Fokus auf die strategische Aspekte ihrer Arbeit an. Dieser Artikel enthält:

- Eine kurze Übersicht der Azure IoT Hub Gerätemanagement.
- Eine Beschreibung der gemeinsamen Gerät Management Grundsätze.
- Eine Beschreibung des Lebenszyklus Gerät.
- Übersicht über gängige Gerät Management Muster.

## <a name="iot-device-management-principles"></a>IoT Gerät-Prinzipien

IoT bringt mit eines eindeutigen Satzes von Verfahren zum Verwalten und jeder-Lösung muss die folgenden Prinzipien zu beheben:

![Azure IoT Hub Gerät Management Prinzipien Grafik][img-dm_principles]

- **Skalieren und Automatisierung**: IoT Lösungen erforderlich, einfache Tools, die eine routinemäßige Aufgaben automatisieren und Aktivieren eines relativ kleinen IT-Mitarbeiter Millionen von Geräten verwalten können. Täglichen, erwarten Operatoren beziehen sich auf Gerätevorgänge Remote, in Massen, und nur bei auftretenden Problemen, die benachrichtigt werden müssen, deren direkte Aufmerksamkeit.

- **Kompatibilität und Zugänglichkeit**: das IoT Gerät Netz ist extrem unterschiedlich. Tools zum Projektmanagement müssen angepasst werden, um eine Vielzahl von Geräteklassen, Plattformen und Protokolle aufnehmen zu können. Operatoren müssen viele Arten von Geräten, aus der am häufigsten eingeschränkten eingebettete Single-Prozess Chips leistungsfähige und voll funktionsfähige Computern unterstützen können.

- **Kontext Präsenz**: IoT Umgebungen sind dynamische und ändern. Dienst Zuverlässigkeit hat höchste Priorität. Gerät Management Vorgänge müssen berücksichtigt werden in Vereinbarung zum SERVICELEVEL Wartung Windows, Netzwerk- und Power Staaten, Bedingungen in verwenden und Gerät geografischen Standort, um sicherzustellen, dass die Wartung Ausfallzeiten nicht kritischen Geschäftsabläufe beeinflussen oder gefährliche Situationen erstellen.

- **Viele Rollen Service**: Unterstützung für die eindeutigen Workflows und Prozessen IoT Vorgänge Rollen ist entscheidend. Die Mitarbeiter müssen kuenftige mit der angegebenen Einschränkungen der internen IT-Abteilung arbeiten.  Sie müssen auch nachhaltige Wege einbinden Echtzeit Gerät Vorgänge Informationen Abteilungsleiter und anderen Business leitenden Rollen finden.

## <a name="iot-device-lifecycle"></a>IoT Gerät Lebenszyklus

Es gibt eine Reihe von allgemeinen Gerät Management Phasen, die allen Enterprise-Projekten IoT feldbezogene aus. In Azure IoT gibt es fünf Phasen im Lebenszyklus IoT Gerät:

![Die fünf Azure IoT Gerät Lebenszyklus Phasen: Planen, bereitstellen, konfigurieren, überwachen, zurückziehen][img-device_lifecycle]

Innerhalb jeder dieser fünf Phasen gibt es mehrere Gerät Operator Anforderungen, die erfüllt sein müssen, damit eine umfassende Lösung:

- **Planen**: Operatoren ein Gerät Metadatenaustausch-Schema zu erstellen, können sie einfach und genau für Abfragen und Adressieren einer Gruppe von Geräten Massen Management Vorgänge, aktivieren. Das Gerät und können Sie um diese Gerätemetadaten in Form von Tags und Eigenschaften zu speichern.

    *Weitere Lektüre*: [Erste Schritte mit dem Gerät im Vergleich][lnk-twins-getstarted], [arbeiten Gerät im Vergleich][lnk-twins-devguide], [wie zwei Eigenschaften verwenden][lnk-twin-properties]

- **Bereitstellen von**: sicher Bereitstellung von neuen Geräten IoT Hub und Operatoren, um sofort entdecken Sie die Funktionen aktivieren.  Verwenden der Registrierungs IoT Hub Geräts flexible Gerät Identitäten und Anmeldeinformationen zu erstellen, und führen Sie diesen Vorgang in Massen mithilfe eines Auftrags. Erstellen von Geräten, um über deren Funktionen und Bedingungen Eigenschaften in das Gerät und Gerät.

    *Weitere Lektüre*: [Verwalten Gerät Identitäten][lnk-identity-registry], [Massen Verwaltung von Gerät Identitäten][lnk-bulk-identity], [wie zwei Eigenschaften verwenden][lnk-twin-properties]

- **Konfigurieren**: Massen Konfiguration Änderungen und Firmwareupdates auf Geräte Beibehaltung sowohl Gesundheit und Sicherheit zu ermöglichen. Führen Sie diese Gerät Management Vorgänge in Massen mithilfe der gewünschte Eigenschaften oder mit direkten Methoden und übertragenen Aufträge.

    *Weitere Lektüre*: [direkte Methoden verwenden][lnk-c2d-methods], [Aufrufen eine direkte Methode auf einem Gerät][lnk-methods-devguide], [wie zwei Eigenschaften verwendet][lnk-twin-properties], [Zeitplan und die übertragenen Aufträge][lnk-jobs], [Planen von Projekten auf mehreren Geräten][lnk-jobs-devguide]

- **Monitor**: allgemeinen Gerät Websitesammlung Zustand, den Status des laufenden Betrieb überwachen und Operatoren auf Probleme, die ihre Aufmerksamkeit erfordern möglicherweise benachrichtigen.  Wenden Sie das Gerät zwei Geräten Bericht Echtzeit Geschäftsbedingungen und den Status der Update-Vorgänge zulässig. Erstellen Sie leistungsfähige Dashboardberichte, die die aktuellste Probleme mit Gerät und Abfragen bereitstellen.

    *Weitere Lektüre*: [So verwenden Sie die Eigenschaften und][lnk-twin-properties], [Abfragesprache für Projekte und im Vergleich][lnk-query-language]

- **Außerkraftsetzungsrichtlinie**: Ersetzen oder außer Betrieb setzen Geräte nach einem Fehler, upgrade Kreis, oder am Ende der Lebensdauer von Diensten.  Verwenden Sie das Gerät und Geräteinformationen zu verwalten, ob das physische Gerät wird ersetzt oder archiviert werden, wenn er Rente. Verwenden Sie die Registrierung des Geräts IoT Hub für sicher widerrufen Gerät Identitäten und die Anmeldeinformationen ein.

    *Weitere Lektüre*: [So verwenden Sie die Eigenschaften und][lnk-twin-properties], [Verwalten Gerät Identitäten][lnk-identity-registry]

## <a name="iot-hub-device-management-patterns"></a>IoT Hub Gerät Management Mustern

IoT Hub ermöglicht den folgenden Satz Gerät Management Muster.  Das [Gerät Management Lernprogramme] [ lnk-get-started] Sie ausführlicher anzeigen, wie Sie diese Muster zum Anpassen Ihrer genauen Szenarios erweitern und zum neue Mustern basierend auf diesen Vorlagen Core entwerfen.

- Informiert dem Gerät **neu gestartet** - die Back-End-Anwendung über eine direkte Methode, dass es einen Neustart initiiert hat.  Das Gerät verwendet das Gerät zwei gemeldet Eigenschaften, um den Status Neustart des Geräts aktualisieren.

    ![Azure IoT Hub Gerätemanagement Neustart Muster Grafik][img-reboot_pattern]

- Informiert dem Gerät **Zurücksetzen Factory** - die Back-End-Anwendung über eine direkte Methode, dass sie eine Fabrik zurücksetzen initiiert hat.  Das Gerät verwendet das Gerät zwei gemeldete Eigenschaften aktualisieren die Factory Status des Geräts zurücksetzen.

    ![Azure IoT Hub Gerät Management Factory zurücksetzen Muster Grafik][img-facreset_pattern]

- **Konfigurations** - die Back-End-Anwendung verwendet die Gerät gewünscht und Eigenschaften so konfigurieren Sie die Software auf dem Gerät ausgeführt.  Das Gerät verwendet das Gerät zwei gemeldet Eigenschaften Konfigurationsstatus des Geräts zu aktualisieren.

    ![Azure IoT Hub Gerät Management Konfiguration Muster Grafik][img-config_pattern]

- Informiert dem Gerät **Firmwareupdates** - die Back-End-Anwendung über eine direkte Methode, dass es ein Firmwareupdate initiiert hat.  Um das Firmwarepaket herunterladen, das Firmwarepaket anwenden und schließen Sie schließlich wieder zum Dienst IoT Hub aus mehreren Schritten initiiert das Gerät.  Im Verlauf des Prozesses Mult-Schritt das Gerät verwendet das Gerät zwei gemeldet Eigenschaften des Fortschritts und Status des Geräts zu aktualisieren.

    ![Azure IoT Hub Management Gerätefirmware aktualisieren Muster Grafik][img-fwupdate_pattern]

- **Status und Status melden** - Anwendung Back-End ausgeführt Gerät und Abfragen über eine Reihe von Geräten, Bericht über den Status und den Fortschritt der Aktionen, die auf den Geräten ausgeführt wird.

    ![Azure IoT Hub Gerätemanagement reporting des Fortschritts und Status Muster Grafik][img-report_progress_pattern]

## <a name="next-steps"></a>Nächste Schritte

Sie können die Funktionen, Muster und Code-Bibliotheken, die Verwaltung von Azure IoT Hub Geräte bereitstellt, IoT Applications erstellen, die die Enterprise IoT Operator Anforderungen innerhalb der in den einzelnen Phasen des Geräts Lebenszyklus erfüllen, verwenden.

Um Informationen zu den Funktionen für die Azure IoT Hub Gerät weiterhin, finden Sie in die [Erste Schritte mit Azure IoT Hub Gerätemanagement] [ lnk-get-started] Lernprogramm.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md