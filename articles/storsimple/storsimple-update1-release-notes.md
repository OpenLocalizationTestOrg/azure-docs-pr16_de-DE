<properties 
   pageTitle="StorSimple 8000 Reihe Update 1.2 Versionsinformationen | Microsoft Azure"
   description="Beschreibt die neuen Features, Probleme und problemumgehungen für StorSimple 8000 Reihe Update 1.2."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-12-release-notes"></a>Versionsinformationen StorSimple 8000 Reihe Update 1.2  

## <a name="overview"></a>(Übersicht)

Die folgenden Versionsinformationen beschreiben der neuen Features und der geöffneten Probleme für StorSimple 8000 Reihe Update 1.2 zu identifizieren. Sie enthalten auch eine Liste der StorSimple Software, Treiber und Datenträger Firmwareupdates in dieser Version enthalten. 

Update 1.2 kann auf ein beliebiges StorSimple Gerät unter Release (GA), Update 0.1, Update 0,2 oder Update 0,3 Software angewendet werden. Update 1.2 ist nicht verfügbar, wenn Ihr Gerät Update 1 oder Update 1.1 ausgeführt wird. Wenn Ihr Gerät Release (GA) ausgeführt wird, nehmen Sie die [an den Microsoft-Support](storsimple-contact-microsoft-support.md) , um Unterstützung bei der Installation dieses Updates.

Die folgende Tabelle enthält die Gerät Software-Versionen, die Updates 1, 1.1 und 1.2 entspricht.

| Wenn Update ausführen... | Dies ist Ihr Gerät Software-Version. |
|---------------------|---------------------------------------|
| Update 1.2          | 6.3.9600.17584                        |
| Aktualisieren von 1.1          | 6.3.9600.17521                        |
| Aktualisieren von 1.0          | 6.3.9600.17491                        |

Überprüfen Sie die Informationen in den Versionsinformationen enthalten hat, bevor Sie das Update in Ihre Lösung StorSimple bereitstellen. Weitere Informationen finden Sie unter So [Installieren Sie Update 1.2 auf Ihrem Gerät StorSimple](storsimple-install-update-1.md). 

>[AZURE.IMPORTANT]
> 
- Es dauert etwa 5 bis 10 Stunden zur Installation dieses Updates (einschließlich der Windows-Updates). 
- Update 1.2 hat Software, LSI-Treiber und Firmwareupdates Datenträger. Folgen Sie den Anweisungen in [Update 1.2 auf Ihrem Gerät StorSimple installieren](storsimple-install-update-1.md), um zu installieren.
- Neue Versionen Sie möglicherweise nicht finden Sie unter Updates sofort auf, da wir eine geplante Bereitstellung von Updates führen. Suche nach Updates in wenigen Tagen erneut wird als diese bald verfügbar.


## <a name="whats-new-in-update-12"></a>Was ist neu in Update 1.2

Diese Features wurden zuerst mit Update 1 zur Verfügung gestellt, die eine begrenzte Anzahl von Benutzern zur Verfügung gestellt wurde. Mit der Version 1.2 aktualisieren würde die meisten Benutzer StorSimple die folgenden neuen und verbesserten Features finden Sie unter:

- **Migration von 5000 7000-er auf 8000 Reihe Geräte** – diese Version bietet eine neue Migrationsfunktion, die die StorSimple 5000-7000 Reihe Einheit Benutzern ihre Daten mit einer physischen Einheiten der Serie StorSimple 8000 oder eine virtuelle Einheit migrieren ermöglicht. Das Feature für die Migration besteht aus zwei wichtigste nutzen:                                                                  

    - **Geschäftskontinuität**, durch das Aktivieren der Migration von vorhandenen Daten auf Geräte der Serie 5000-7000 auf Einheiten, 8000-Serie.
    - **Verbesserte Features Angebote 8000 Reihe Einheiten**, wie effizient zentrale Verwaltung von mehreren Einheiten über StorSimple-Manager-Dienst, besser Klasse von Hardware und Firmware, virtuelle Einheiten, Datenmobilität und Features in der Wegweiser für die Zukunft aktualisiert.

    Finden Sie weitere Informationen zum Migrieren einer StorSimple 5000-7000 Reihe zu einem 8000 Reihe-Gerät der [Migrationshandbuch](http://www.microsoft.com/download/details.aspx?id=47322) . 

- **Verfügbarkeit im Portal Government Azure** – StorSimple steht jetzt im Portal Azure Government. Finden Sie unter Gewusst wie: [Bereitstellen ein Geräts StorSimple im Portal Government Azure](storsimple-deployment-walkthrough-gov.md).

- **Unterstützung für andere Cloud-Dienstanbieter** – die anderen Cloud-Dienstanbieter unterstützt werden Amazon a3, Amazon S3 mit RRS, HP und OpenStack (Beta).

- **Update auf die neueste Speicher-APIs** – wurde in der neuesten Azure Speicherdienst APIs mit dieser Version StorSimple aktualisiert. StorSimple 8000 Reihe Geräte, die Software-Versionen vor dem Update 1 ausgeführt werden (Version 0.1, 0,2 und 0,3) Versionen älter als 17 Juli 2009 Azure-Speicher Dienst APIs verwenden. Wie in der aktualisierten [Ankündigung zum Entfernen von Speicher-Service-Versionen](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx)von 1 August 2016 erwähnt wird diese APIs nicht mehr unterstützt. Es ist unbedingt erforderlich, dass Sie die StorSimple 8000 Reihe Update 1 vor 1 August 2016 anwenden. Wenn Sie dies nicht tun, werden StorSimple Geräte funktioniert nicht mehr ordnungsgemäß.

- **Unterstützung für Zone redundante Speicher (ZRS)** – wird mit dem Upgrade auf die neueste Version der Speicher-APIs, die Reihe StorSimple 8000 Zone redundante Speicher (ZRS) sowie lokal redundante Speicher (LRS) und Geo redundante Speicher (GRS) unterstützen. Finden Sie in diesem [Artikel auf Azure Redundanz Speicheroptionen](../storage/storage-redundancy.md) ZRS Details.

- **Erweiterte anfänglichen Bereitstellung und die Aktualisierung Erfahrung** – wurden In dieser Version, die Installation und Update Prozesse weiterentwickelt. Die Installation durch den Setup-Assistenten wurde verbessert, um Feedback für den Benutzer bereitgestellt werden, wenn die Netzwerkkonfiguration und Firewall-Einstellungen nicht korrekt sind. Zusätzliche Diagnose Cmdlets wurden, die Ihnen bei der Problembehandlung Netzwerke des Geräts bereitgestellt. Finden Sie unter der [Bereitstellung Artikel Problembehandlung](storsimple-troubleshoot-deployment.md) für Weitere Informationen zu den neuen diagnostic-Cmdlets für die Problembehandlung verwendet.

## <a name="issues-fixed-in-update-12"></a>Probleme in Update 1.2

Die folgende Tabelle enthält eine Zusammenfassung der Probleme, die Updates 1.2, 1.1 und 1 behoben wurden.    


| Nein. | Feature | Problem | Fixed in aktualisieren | Gilt für physische Gerät | Gilt für virtuelles Gerät |
|-----|---------|-------|-----------------|---------------------------------|--------------------------------|
| 1 | Windows PowerShell für StorSimple | Wenn ein Benutzer per Remotezugriff das Gerät StorSimple verfügbar mithilfe von Windows PowerShell für StorSimple, und klicken Sie dann den Setup-Assistenten gestartet, aufgetreten ein Absturz so früh wie Daten 0 IP eingegeben wurde. Dieser Fehler wird nun in Update 1 behoben. | Update 1 | Ja | Ja |
| 2 | Factory zurücksetzen | In einigen Fällen, wenn Sie eine Fabrik Zurücksetzen durchgeführt das Gerät StorSimple hängen geworden ist und diese Meldung angezeigt: **Zurücksetzen Factory wird ausgeführt (Phase 8)**. Dies passiert, wenn Sie STRG + C gedrückt, während das Cmdlet ausgeführt wurde. Dieser Fehler ist jetzt behoben.| Update 1 | Ja | Nein |
| 3 | Factory zurücksetzen | Nachdem eine fehlgeschlagene zwei Controllerfactory zurückgesetzt werden soll, darf Sie wurden Gerät Registrierung fortzusetzen. Dies ist ein nicht unterstütztes Systemkonfiguration. Update 1 wird eine Fehlermeldung angezeigt und Registrierung ist auf einem Gerät, dass eine fehlgeschlagene Factory zurückgesetzt wurde gesperrt. | Update 1 | Ja | Nein |
| 4 | Factory zurücksetzen | In einigen Fällen wurden falsch positiv Konflikt Warnungen ausgelöst. Falsche Konflikt Benachrichtigungen werden nicht mehr auf Update 1-Geräten generiert. | Update 1 | Ja | Nein |
| 5 | Factory zurücksetzen | Wenn eine Fabrik zurücksetzen vor der Fertigstellung unterbrochen wurde, wird das Gerät Wiederherstellungsmodus eingegeben und Sie Windows PowerShell für StorSimple zugreifen nicht zulassen. Dieser Fehler ist jetzt behoben. | Update 1 | Ja | Nein |
| 6 | Wiederherstellung | Ein Disaster Wiederherstellung (DR)-Fehler wurde behoben in dem DR während die Suche nach Sicherungskopien auf dem Zielgerät fehlschlägt. | Update 1 | Ja | Ja |
| 7 | Überwachen der LED-Anzeigen | In bestimmten Fällen die Überwachung LED auf der Rückseite Einheit nicht richtig Status angeben. Die blaue LED deaktiviert wurde. Daten 0 und 1-LED Daten wurden aufleuchtende, auch wenn diese Schnittstellen nicht konfiguriert wurden. Dieses Problem behoben wurde und Überwachung LED-Anzeigen jetzt den richtigen Status.  | Update 1 | Ja | Nein |
| 8 | Überwachen der LED-Anzeigen | In bestimmten Fällen deaktiviert nach dem Update 1 anwenden das blaue Licht, das auf dem aktiven Controller wodurch er schwer den aktiven Controller identifizieren. In dieser Patchversion wurde dieses Problem behoben.| Update 1.2 | Ja | Nein |
| 9 | Netzwerk-Schnittstellen | In früheren Versionen konnte ein StorSimple Gerät mit einer nicht geroutet Gateway konfiguriert den Offlinebetrieb wechseln. In dieser Version wurde die Weiterleitung Metrik für Daten 0 die niedrigste; vorgenommen daher, auch wenn andere Netzwerk-Schnittstellen Cloud aktiviert sind, alle Cloud Datenverkehr vom Gerät über Daten 0 geleitet. | Update 1 | Ja | Ja | 
| 10 | Sicherungskopien | Ein Fehler in Update 1 die Sicherungskopien nach 24 Tage treten verursacht hat in der Patchversion 1.1 Update behoben wurde. | Aktualisieren von 1.1 | Ja | Ja |
| 11 | Sicherungskopien | Ein Fehler in früheren Versionen geführt einer schlechten Leistung Cloud Momentaufnahmen mit Sätzen niedrig ändern. Dieser Fehler wurde in dieser Patchversion behoben.| Update 1.2 | Ja | Ja |
| 12 | Updates | Ein Fehler in Update 1, die einen Fehler beim Upgrade gemeldet und verursacht die Controller Wiederherstellungsmodus wechseln wurde in dieser Patchversion behoben.| Update 1.2 | Ja | Ja |


## <a name="known-issues-in-update-12"></a>Bekannte Probleme in Update 1.2

Die folgende Tabelle enthält eine Übersicht über bekannte Probleme in dieser Version.

| Nein. | Feature | Problem | Kommentare/dieses Problem zu umgehen | Gilt für physische Gerät | Gilt für virtuelles Gerät |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Quorum Datenträger | Gelegentlich Wenn die Mehrzahl der Datenträger in der Einheit EBOD einer 8600 Gerät getrennt werden keine Quorum Datenträger wodurch, werden Speicherpool offline. Es verbleibt offline, auch wenn die Datenträger erneut verbunden sind. | Sie müssen das Gerät neu starten. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support, für den nächsten Schritten fort. | Ja | Nein |
| 2 | Falsche Controller-ID | Wenn kein Ersatz Controller ausgeführt wird, kann als Controller 1 Controller 0 angezeigt. Während Controller Ersatz nachdem das Bild aus dem Peerknoten geladen wird kann die Controller-ID Anfangs als dem Peer-Controller ID angezeigt Dieses Verhalten möglicherweise auch gelegentlich nach einem Neustart des Systems angezeigt. | Es ist keine Benutzeraktion erforderlich. Dies wird sich selbst beheben nach Abschluss der Controller ersetzt. | Ja | Nein |
| 3 | Speicherkonten | Verwenden zum Löschen des Speicherkontos der Speicherdienst ist ein nicht unterstütztes Szenario. Dies wird zu einer Situation führen, in dem Benutzerdaten abgerufen werden können. | Ja | Ja |
| 4 | Geräte-failover | Mehrere Failovers eines Containers aus dem gleichen Quellgerät Lautstärke für verschiedene Geräte wird nicht unterstützt. Geräte-Failover auf einem einzelnen inaktive Gerät auf mehreren Geräten machen Lautstärke Container auf der ersten konnte nicht über Gerät Besitz Daten gehen verloren. Nach einem Failover werden diese Lautstärke Container angezeigt oder Verhalten sich anders, wenn Sie in der klassischen Azure-Portal angezeigt werden. | | Ja | Nein |
| 5 | Installation | Während StorSimple Netzwerkadapter für SharePoint-Installation müssen Sie eine Gerät IP in Reihenfolge für die Installation erfolgreich abgeschlossen eingeben.    | | Ja | Nein |
| 6 | Web-proxy | Wenn die Web Proxy-Konfiguration HTTPS als das angegebene Protokoll verfügt, Ihr Gerät-zu-Service-Kommunikation beeinflusst werden, und das Gerät wird im Offlinemodus wechselt. Support-Paketen werden auch im Prozess, Verarbeitung erhebliche Ressourcen auf Ihrem Gerät generiert werden. | Stellen Sie sicher, dass die Web-Proxy-URL HTTP als das angegebene Protokoll enthält. Weitere Informationen zum [Konfigurieren von Webproxy für Ihr Gerät](storsimple-configure-web-proxy.md)wechseln. | Ja | Nein |
| 7 | Web-proxy | Wenn Sie konfigurieren und Webproxy auf einem Gerät registrierten aktivieren, müssen Sie den aktiven Controller auf Ihrem Gerät neu starten. | | Ja | Nein |
| 8 | Hohe Cloud Wartezeit und hoher e/a-Arbeitsbelastung | Wenn Ihr Gerät StorSimple eine Kombination von sehr hoch Cloud Wartezeiten (Reihenfolge der Sekunden) und hoher e/a-Arbeitsbelastung trifft, die Gerät Datenmengen in einem beeinträchtigt Zustand wechseln und e / kann ein Fehler auftreten, mit einem Fehler "Gerät nicht bereit". | Sie müssen manuell neu starten, die Gerätecontroller oder einen Gerät Failover zum Beheben des Problems führen. | Ja | Nein |
| 9 | Azure PowerShell | Wenn Sie das Cmdlet StorSimple **Get-AzureStorSimpleStorageAccountCredential & #124 verwenden. Select-Object zuerst 1 - warten** das erste Objekt aktivieren, damit Sie ein neues **VolumeContainer** Objekt erstellen können, das Cmdlet gibt alle Objekte. | Umbrechen des Cmdlets in Klammern wie folgt: **(Get-Azure-StorSimpleStorageAccountCredential) & #124; Select-Object zuerst 1 - warten** | Ja | Ja |
| 10| Migration | Bei mehreren Lautstärke Container für die Migration weitergegeben werden, wird der ETA für die neueste Sicherung nur für den ersten Lautstärke Container genau. Darüber hinaus kann parallele Migration starten, nachdem die ersten 4 Sicherungskopien in den ersten Lautstärke Container migriert werden. | Es empfiehlt sich, dass Sie jeweils eine Lautstärke Container migriert werden. | Ja | Nein |
| 11| Migration | Nach der Wiederherstellung werden Datenmengen nicht die Sicherung Richtlinie oder der virtuelle Laufwerkgruppe hinzugefügt. | Sie benötigen diese Datenträger Sicherungskopien zum Erstellen einer Sicherungskopie Richtlinie hinzufügen. | Ja | Ja |
| 12| Migration | Nach Abschluss die Migration muss das 5000/7000-er Gerät nicht den Datencontainer migrierte zugreifen. | Es empfiehlt sich, dass Sie die Datencontainer migrierte löschen, nachdem die Migration abgeschlossen ist. | Ja | Nein |
| 13| Klonen und DR | Ein Update 1 ausführen StorSimple-Gerät nicht Klonen oder Wiederherstellung zu einem vor dem Update 1 Software-Gerät ausführen. | Sie müssen das Gerät 1 aktualisieren, um diese Vorgänge zu erlauben aktualisieren | Ja | Ja |
| 14 | Migration | Sicherungskopie der Konfiguration für die Migration kann auf einem 5000-7000 Reihe Gerät möglicher Fehler beim Lautstärke Gruppen mit keine zugeordneten Datenträger vorhanden sind. | Löschen Sie alle leeren Lautstärke Gruppen mit keine zugeordneten Datenmengen, und wiederholen Sie dann die Sicherung Konfiguration.| Ja | Nein |

## <a name="physical-device-updates-in-update-12"></a>Physische Gerät Aktualisierungen in Update 1.2

Wenn Patch aktualisieren 1.2 zu einem physischen Gerät (Versionen vor Update 1 ausführen) angewendet wird, wird die Version der Software zu 6.3.9600.17584 ändern.

## <a name="controller-and-firmware-updates-in-update-12"></a>Controller und Firmware-Updates in Update 1.2

Diese Version aktualisiert der Treiber und die Festplatten-Software auf Ihrem Gerät.
 
- Weitere Informationen zu den SAS Controller aktualisieren finden Sie unter [Update 1 für LSI SAS Controller in Microsoft Azure StorSimple Einheit](https://support.microsoft.com/kb/3043005). 

- Weitere Informationen über die Festplatten-Software aktualisieren Sie, finden Sie unter [Datenträger Firmware Update 1 für Microsoft Azure StorSimple Einheit](https://support.microsoft.com/kb/3063416).
 
## <a name="virtual-device-updates-in-update-12"></a>Virtuelles Gerät Aktualisierungen in Update 1.2

Dieses Update kann nicht auf das virtuelle Gerät angewendet werden. Neue virtuelle Geräte müssen erstellt werden. 

## <a name="next-steps"></a>Nächste Schritte

- [Installieren von Update 1.2 auf Ihrem Gerät](storsimple-install-update-1.md).
 
