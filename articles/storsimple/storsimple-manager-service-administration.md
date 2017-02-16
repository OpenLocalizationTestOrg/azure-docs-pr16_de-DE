<properties 
   pageTitle="StorSimple Manager-Dienstadministration | Microsoft Azure"
   description="Informationen Sie zum Verwalten von Ihrem Geräts StorSimple mithilfe des StorSimple Manager-Diensts in der klassischen Azure-Portal."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-device"></a>Verwenden des StorSimple Manager-Diensts auf Ihrem Gerät StorSimple verwalten

## <a name="overview"></a>(Übersicht)

In diesem Artikel werden die StorSimple Manager Service Interface, einschließlich der Herstellung der Verbindung, die verschiedenen Optionen zur Verfügung und Links, die bestimmte Workflows, die über diese Benutzeroberfläche ausgeführt werden können. Dieser Leitfaden ist beides anwendbar; die physische StorSimple und virtuelles Gerät.

Nach dem Lesen dieses Artikels lernen Sie:

- Verbinden mit StorSimple-Manager-Dienst
- Navigieren der StorSimple Manager-Benutzeroberfläche
- Verwalten von Ihrem Gerät StorSimple über den StorSimple-Manager-Dienst


## <a name="connect-to-storsimple-manager-service"></a>Verbinden mit StorSimple-Manager-Dienst

Der Dienst StorSimple-Manager in Microsoft Azure ausgeführt wird und auf mehreren Geräten ein StorSimple verbindet. Sie verwenden eine zentrale klassischen Portal von Microsoft Azure in einem Browser ausgeführt, um diese Geräte verwalten. Informationen zum Verbinden mit dem Dienst StorSimple Manager Folgendes ein.

#### <a name="to-connect-to-the-service"></a>Die Verbindung zum Dienst

1. Navigieren Sie zu [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

1. Melden Sie sich mit Ihrem Microsoft-Konto-Anmeldeinformationen, klassische Microsoft Azure-Portal (befindet sich in der oberen rechten Rand des Bereichs).

1. Führen Sie einen Bildlauf nach unten im linken Navigationsbereich auf den Dienst StorSimple Manager zuzugreifen.


## <a name="navigate-storsimple-manager-service-ui"></a>Navigieren Sie StorSimple Verwaltungsdienst-Benutzeroberfläche

Der Navigationshierarchie für den Dienst StorSimple Manager ist Benutzeroberfläche in der folgenden Tabelle aufgeführt.

- Startseite **StorSimple Manager** gelangen Sie zum Servicelevel Benutzeroberfläche Seiten anwendbar alle Geräte in einem Dienst.

- **Geräte** -Seite gelangen Sie zum Gerät – Ebene Benutzeroberfläche Seiten zu einem bestimmten Gerät verfügbar.

- **Lautstärke Container** Seite gelangen Sie zu der Seite Datenträger, die alle Datenmengen, die einem Gerät zugeordnet werden.


#### <a name="storsimple-manager-service-navigational-hierarchy"></a>StorSimple Manager Service Navigationshierarchie

|Startseite|Servicelevel Seiten|Gerät Ebene Seiten|Gerät Ebene Seiten|
|---|---|---|---|
|StorSimple-Manager-Dienst|Service-dashboard|Gerät dashboard||
||Geräte →|Monitor|
||Zusätzliche Katalog|Volumen-containers→|Datenmengen|
||Konfigurieren (Service)|Zusätzliche Richtlinien||
||Aufträge|Konfigurieren von (Gerät)|
||Benachrichtigungen|Wartung|

![Video verfügbar](./media/storsimple-manager-service-administration/Video_icon.png) **Video verfügbar**

Wenn Sie ein Video zur Verfügung, die Sie über die Benutzeroberfläche von StorSimple Manager Service geführt, klicken Sie auf [hier](https://azure.microsoft.com/documentation/videos/storsimple-manager-service-overview/).

## <a name="administer-storsimple-device-using-storsimple-manager-service"></a>Verwalten von StorSimple Gerät mit StorSimple-Manager-Dienst

Die folgende Tabelle enthält eine Zusammenfassung aller häufige Verwaltungsaufgaben und komplexe Workflows, die innerhalb der StorSimple Verwaltungsdienst für die Benutzeroberfläche ausgeführt werden können. Diese Aufgaben werden basierend auf der Benutzeroberfläche Seiten organisiert Grundlage ihrer Ausführung beschrieben.

Weitere Informationen zu den einzelnen Workflows klicken Sie auf die entsprechende Vorgehensweise in der Tabelle.

#### <a name="storsimple-manager-workflows"></a>StorSimple Manager workflows

|Wenn Sie dies tun möchten...|Wechseln Sie zu dieser Seite Benutzeroberfläche...|Gehen Sie folgendermaßen vor.|
|---|---|---|
|Erstellen Sie einen Dienst</br>Löschen Sie einen Dienst</br>Abrufen von Service-Registrierungsschlüssel</br>Erstellen, jeweils Dienst Registrierungsschlüssel|StorSimple-Manager-Dienst|[Bereitstellen eines StorSimple-Manager-Diensts](storsimple-manage-service.md)
|Ändern Sie den Dienst Daten Verschlüsselungsschlüssel</br>Die Protokolle der Vorgang anzeigen|StorSimple Manager Service → Dashboard|[Verwenden Sie das StorSimple Manager Service-dashboard](storsimple-service-dashboard.md)|
|Deaktivieren Sie ein Gerät</br>Löschen Sie ein Gerät|StorSimple Manager Service →-Geräte|[Deaktivieren Sie oder löschen Sie ein Gerät](storsimple-deactivate-and-delete-device.md)|
|Erfahren Sie mehr über Disaster Wiederherstellung und Gerät failover</br>Failover zu einem physischen Gerät</br>Failover zu einem virtuellen Gerät</br>Business Continuity Wiederherstellung (BCDR)|StorSimple Manager Service →-Geräte|[Failover und Disaster Wiederherstellung für Ihr Gerät StorSimple](storsimple-device-failover-disaster-recovery.md)|
|Liste Sicherungskopien für einen Datenträger</br>Wählen Sie eine Sicherungskopie</br>Löschen einer Sicherung Menge|StorSimple Manager Service → Sicherungskatalog|[Verwalten von Sicherungskopien](storsimple-manage-backup-catalog.md)|
|Klonen Sie einen Datenträger|StorSimple Manager Service → Sicherungskatalog|[Klonen Sie einen Datenträger](storsimple-clone-volume.md)|
|Wiederherstellen einer Sicherung festlegen|StorSimple Manager Service → Sicherungskatalog|[Wiederherstellen einer Sicherung festlegen](storsimple-restore-from-backup-set.md)|
|Informationen zu Speicherkonten</br>Hinzufügen eines Kontos Speicher</br>Bearbeiten einer Speicher-Kontos</br>Löschen eines Kontos Speicher</br>Key Drehung Speicherkonten|StorSimple Manager Service → konfigurieren|[Verwalten von Speicherkonten](storsimple-manage-storage-accounts.md)|
|Informationen zu Vorlagen Bandbreite</br>Fügen Sie eine Vorlage Bandbreite</br>Bearbeiten einer Vorlage Bandbreite</br>Löschen einer Vorlage für die Bandbreite</br>Verwenden Sie eine Standardvorlage für die Bandbreite</br>Erstellen einer Vorlage für ganztägige Bandbreite, die einem bestimmten Zeitpunkt beginnt|StorSimple Manager Service → konfigurieren|[Verwalten von Vorlagen für die Bandbreite](storsimple-manage-bandwidth-templates.md)|
|Informationen zu Access-Steuerelement Datensätze</br>Erstellen eines Access-Steuerelement-Datensatzes</br>Bearbeiten eines Datensatzes der Access-Steuerelement</br>Löschen eines Datensatzes der Access-Steuerelement|StorSimple Manager Service → konfigurieren|[Verwalten von Datensätzen für Access-Steuerelement](storsimple-manage-acrs.md)|
|Anzeigen von Details zu Position</br>Abbrechen eines Auftrags|StorSimple Manager-Dienst → Aufträgen|[Verwalten von Projekten](storsimple-manage-jobs.md)
|Lassen Sie sich benachrichtigen</br>Benachrichtigungen verwalten</br>Überprüfen von Benachrichtigungen|StorSimple Manager Service → Benachrichtigungen|[Anzeigen und Verwalten von Benachrichtigungen StorSimple](storsimple-manage-alerts.md)
|Anzeigen von verbundenen Initiatoren</br>Suchen nach Seriennummer des Geräts</br>Suchen Sie das Ziel IQN|StorSimple Manager Service → Geräte → Dashboard|[Verwenden Sie das StorSimple Gerät dashboard](storsimple-device-dashboard.md)|
|Erstellen von Diagrammen Überwachung|StorSimple Manager Service → Geräte → überwachen|[Überwachen von Ihrem Gerät StorSimple](storsimple-monitor-device.md)|
|Hinzufügen eines Containers Lautstärke</br>Ändern eines Containers Lautstärke</br>Löschen eines Containers Lautstärke|StorSimple Manager Service → Geräte → Lautstärke Container|[Volumen-Container verwalten](storsimple-manage-volume-containers.md)|
|Hinzufügen eines Datenträgers</br>Ein Volume bearbeiten</br>Offlineschalten einer Lautstärke</br>Löschen eines Datenträgers</br>Überwachen von einem volume|StorSimple Manager Service → Geräte → Lautstärke Container → Datenmengen|[Verwalten von Datenmengen](storsimple-manage-volumes.md)|
|Ändern des Audiogeräts</br>Ändern von Einstellungen für Zeit</br>Ändern von Einstellungen für DNS.md</br>Konfigurieren von Netzwerk-Schnittstellen|StorSimple Manager Service → Geräte → konfigurieren|[Ändern Sie die Konfiguration des Geräts für Ihr Gerät StorSimple](storsimple-modify-device-config.md)|
|Web Proxy-Einstellungen anzeigen|StorSimple Manager Service → Geräte → konfigurieren|[Konfigurieren von Webproxy für Ihr Gerät](storsimple-configure-web-proxy.md)|
|Gerät Administratorkennwort ändern</br>StorSimple Snapshot-Manager Kennwort ändern|StorSimple Manager Service → Geräte → konfigurieren|[StorSimple Kennwörter ändern](storsimple-change-passwords.md)|
|Konfigurieren der remote-Verwaltung|StorSimple Manager Service → Geräte → konfigurieren|[Verbinden Sie per Remotezugriff auf Ihr Gerät StorSimple](storsimple-remote-connect.md)|
|Konfigurieren von Benachrichtigungseinstellungen|StorSimple Manager Service → Geräte → konfigurieren|[Anzeigen und Verwalten von Benachrichtigungen StorSimple](storsimple-manage-alerts.md)|
|Konfigurieren von CHAP für Ihr Gerät StorSimple|StorSimple Manager Service → Geräte → konfigurieren|[Konfigurieren von CHAP für Ihr Gerät StorSimple](storsimple-configure-chap.md)|
|Fügen Sie eine zusätzliche Richtlinie hinzu</br>Hinzufügen oder Ändern eines Projektplans</br>Löschen einer Sicherung Richtlinie</br>Nehmen Sie eine manuelle Sicherung</br>Erstellen Sie eine benutzerdefinierte Richtlinien mit mehreren Datenmengen und Zeitpläne|StorSimple-Manager-Dienst → Geräte → Sicherung Richtlinien|[Zusätzliche Richtlinien verwalten](storsimple-manage-backup-policies.md)|
|Beenden der Controller device</br>Starten Sie Gerätecontroller neu</br>Fahren Sie Controller device</br>Ihr Gerät auf Standardwerte für das Zurücksetzen</br>(Höher ist für lokale Gerät nur)|StorSimple Manager Service → Geräte → Wartung|[Verwalten von StorSimple Gerätecontroller](storsimple-manage-device-controller.md)|
|Erfahren Sie mehr über StorSimple Hardware-Komponenten</br>Monitor Hardwarestatus</br>(Höher ist für lokale Gerät nur)|StorSimple Manager Service → Geräte → Wartung|[Monitor Hardware-Komponenten](storsimple-monitor-hardware-status.md)|
|Erstellen Sie ein Supportpaket|StorSimple Manager Service → Geräte → Wartung|[Erstellen und Verwalten eines Support-Pakets](storsimple-create-manage-support-package.md)|
|Installieren von Software-updates|StorSimple Manager Service → Geräte → Wartung|[Aktualisieren von Ihrem Gerät](storsimple-update-device.md)|


##<a name="next-steps"></a>Nächste Schritte
Wenn Sie Probleme mit der tägliche Einsatz von Ihrem Gerät StorSimple oder anderer Bestandteilen Hardware auftreten, lesen Sie:

- [Behandeln von Problemen mit einem Betrieb-Gerät](storsimple-troubleshoot-operational-device.md)
- [Verwenden von StorSimple Überwachung LED-Indikator anzeigen](storsimple-monitoring-indicators.md)

Wenn Sie können nicht die Probleme beheben, und Sie eine Serviceanfrage erstellen müssen, lesen Sie [Microsoft-Support wenden](storsimple-contact-microsoft-support.md).
