<properties 
    pageTitle="StorSimple 8000 Update 0.1 Versionsinformationen | Microsoft Azure"
    description="Beschreibt die neuen Features und Updates, Tagesordnungspunkte und verfügbaren problemumgehungen für die Oktober 2014 Microsoft Azure StorSimple Release (Update 0,1)."
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
    ms.date="09/21/2016"
    ms.author="alkohli" />

# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>StorSimple 8000 Reihe Update 0.1 Versionsinformationen – Oktober 2014  

## <a name="overview"></a>(Übersicht)

Die folgenden Versionsinformationen stellen die kritische Tagesordnungspunkte für StorSimple 8000 Reihe Update 0,1 in Oktober 2014 freigegeben. Sie enthalten auch eine Liste der StorSimple Software- und Firmwareversionen Aktualisierungen in dieser Version enthalten. Dies ist die erste Version, nachdem die StorSimple 8000 Reihe aktualisierte Version in der Regel in Juli 2014 zur Verfügung gestellt wurde und sich auf Softwareversion 6.3.9600.17312 bezieht.  

Es empfiehlt sich, für Scannen und alle verfügbaren Updates sofort nach der Installation auf des Geräts zu installieren. Sie können auch aktivieren automatische Updates zum Herunterladen und Installieren von Updates mit hoher Priorität von Microsoft, sobald sie freigegeben werden. Weitere Informationen finden Sie unter So [Aktualisieren Sie Ihr Gerät StorSimple](storsimple-update-device.md).  

Überprüfen Sie die Informationen in den Versionsinformationen enthalten hat, bevor Sie die Updates in Ihre Lösung StorSimple bereitstellen.  

>[AZURE.IMPORTANT]
> 
-   Verwenden der StorSimple-Manager-Dienst und nicht Windows PowerShell für StorSimple Oktober Updates zu installieren.  
-   Die Updates werden in der Regel ungefähr 3 Stunden abgeschlossen.  
-   Die Oktober-Version von StorSimple enthält alle Aktualisierungen der StorSimple virtuelles Gerät nicht. Sie können weiterhin anwenden verfügbaren Windows-Updates, einschließlich der aktuelle Sicherheitsupdates, aber eine Änderung in der Version für das virtuelle Gerät wird nicht angezeigt.  

Stellen Sie sicher, dass die folgenden Komponenten vor der Aktualisierung von Ihrem Geräts StorSimple erfüllt sind.  

- Stellen Sie sicher, dass beide Gerätecontroller ausgeführt werden, bevor Sie nach Updates suchen. Wenn entweder Controller nicht ausgeführt wird, schlägt die Überprüfung fehl. Um zu überprüfen, dass die Controller in einem ordnungsgemäßen Zustand sind, navigieren Sie zu **Hardware Status** aus, klicken Sie unter der Seite **zum Warten** . Falls Komponenten die **Aufmerksamkeit Bedarf sind**, wenden Sie sich an den Microsoft-Support Vorgang fortsetzen.  
- Sicherstellen Sie, dass die feste IP-Adressen für beide Controller 0 und 1 Controller weitergeleitet werden und mit dem Internet verbinden können, da diese für die Wartung der Updates auf dem Gerät verwendet werden. Sie können das [Cmdlet "Verbindung testen"](https://technet.microsoft.com/library/hh849808.aspx) verwenden, Signal außerhalb des Netzwerks wie outlook.com, um sicherzustellen, dass der Controller mit dem externen Netzwerk verbunden ist eine bekannte Adresse an.  
- Stellen Sie sicher, dass die erforderlichen ausgehende Ports auf Ihrem Gerät StorSimple für ausgehende Kommunikation verfügbar sind. Weitere Informationen finden Sie unter die [Anforderungen für Ihr Gerät StorSimple Networking](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).  
- Ist die Version der Software 6.3.9600.17312 (Oktober 2014 Update) überschreiten, deaktivieren Sie die Ports Daten 2 und 3 von Daten, wenn vor dem Starten der Aktualisierung aktiviert. Wenn Sie die Daten 2 oder Daten 3 Ports aktiviert, wenn Sie das Update anwenden lassen, führt dies möglicherweise auf Ihrem Gerätecontroller Wiederherstellungsmodus wechseln. Bitte beachten Sie, wenn Sie die Netzwerk-Schnittstellen deaktivieren, alle zugehörigen Datenträger offline geschaltet und die e/a für die Dauer der Aktualisierung unterbrochen.  

## <a name="whats-new-in-the-october-release"></a>Was ist neu in der Oktober-Version

Dieses Update umfasst die folgenden Verbesserungen an:

- Den Dienst StorSimple Manager Benutzeroberfläche können jetzt Ihr Gerätecontroller verwalten. Die Verwaltung, die Aktionen umfassen Neustart, ausschalten, oder auf einem Controller deaktivieren. Weitere Informationen zum [Verwalten von StorSimple Gerätecontroller](storsimple-manage-device-controller.md)wechseln.  
- Sie können die WAN-Bandbreite Zuteilung entsprechend der Wochentag und die Uhrzeit des Tages Kombinationen planen. So können Sie besseren Nutzen WAN-Bandbreite in Zeiten vornehmen. Vorlagen für verschiedene Bandbreite dürfen für verschiedene Lautstärke Container. Weitere Informationen zum [Verwalten Ihrer StorSimple Bandbreite Vorlagen](storsimple-manage-bandwidth-templates.md)wechseln.  
- Sie können e-Mail-Benachrichtigungen zu benachrichtigen, die Administratoren und andere vorhandenen oder oftmals anstehende Probleme vorausschauende konfigurieren. Weitere Informationen zum [Konfigurieren von Benachrichtigungseinstellungen](storsimple-manage-alerts.md#configure-alert-settings)wechseln.  

## <a name="issues-fixed-in-the-october-release"></a>Probleme in der Oktober-Version


Die folgende Tabelle enthält eine Zusammenfassung der Probleme, die in diesem Update behoben wurden.  

| Nein. | Feature | Problem | Gilt für physische Gerät | Gilt für virtuelles Gerät |
|-----|---------|-------|---------------------------------|--------------------------------|
| 1 | Netzwerk-Schnittstellen | In der vorherigen Version wurden die Netzwerkschnittstellen Daten 2 und 3 von Daten in der Software ausgetauscht. Dies wurde in diesem Update behoben. Deaktivieren Sie die Einstellungen, und deaktivieren Sie diese Netzwerkschnittstellen, bevor Sie das Update zu installieren. Nach der Installation der Updates müssen Sie diese Schnittstellen neu zu konfigurieren. | Ja | Nein |
| 2 | Support-Paket | In der vorherigen Version, wenn Sie das Cmdlet Windows PowerShell **HcsSupportPackage exportieren** , um die Protokolle Baseboard Management Controller (BMC) abzurufen ausgeführt haben das fehlgeschlagen mit der folgenden Warnung: "der Vorgang wurde erfolgreich abgeschlossen auf diesem Controller, aber konnte nicht auf dem Peer-Controller aufgrund der folgenden nicht. Überprüfen Sie, ob der Peer fehlerfrei ist, und gibt an, ob der aktuelle Knoten mit dem Peer herstellen kann." Dieses Problem ist jetzt behoben. | Ja | Nein |
| 3 | Geräte-failover | In der vorherigen Version wurde eine Wahrscheinlichkeit von inkonsistente Daten Wenn bei einem Gerät Failover ein Auftrags **ermitteln Sicherung** fehlgeschlagen ist. Dieses Problem ist jetzt behoben. | Ja | Nein |
| 4 | Geräte-failover | In der vorherigen Version, nach einem Failover Gerät Sicherungskopien angezeigt wurden, aber der Container zugeordneten Lautstärke war nicht vorhanden ist, klicken Sie auf das Gerät. Dieses Problem ist jetzt behoben. | Ja | Nein |
| 5 | Geräte-failover | In der vorherigen Version bestand ein Problem in der Cloud Sicherungskopien-Enumeration während des Registrierungs-Wiederherstellungsvorgangs potenziell zu inkonsistente Daten führen kann, wenn es wurden Cloud Netzwerkkonnektivitätsprobleme vor. | Ja | Nein |
| 6 | Firmwareupdate | In der vorherigen Version der Auftrag Gerät Firmware Update fehlgeschlagen ist und einen Fehler die angegeben wird, dass die Cmdlets nicht erkennbar ist waren und daher Fehler bei der Aktualisierung angezeigt. Der Controller Problems klicken Sie dann in der Wiederherstellungsmodus. Dieses Problem ist jetzt behoben. | Ja | Nein |
| 7 | Installation | Durch das Gerät nicht ordnungsgemäß während der Installation abgebildet verursacht Fehler wurden behoben. | Ja | Nein |
| 8 | Factory zurücksetzen | Sie können jetzt das Kontrollkästchen Firmware Factory zurücksetzen optional überspringen. Dies ist eine Änderung aus der vorherigen Version. | Ja | Nein |
| 9 | Factory zurücksetzen | In der vorherigen Version Wenn eine Fabrik zurücksetzen Cmdlet ausgeführt wurde, wurden Firmware Version nur für einige Hardware-Komponenten überprüft. Zusätzliche Firmware überprüft wurde nach dem ersten Neustart im Prozess, die wodurch zurücksetzen zum Fehlschlagen könnte. Dieses Fix wird sichergestellt, dass alle Prüfungen Firmware vorgenommen werden, wenn die Factory Cmdlet Zurücksetzen wird ausgeführt und vor dem ersten System neu zu starten. | Ja | Nein |
| 10 | Rotation des Speicher-Konto | Das **Aufrufen-HcsmServiceDataEncryptionKeyChange** -Cmdlet verwenden, um die Tasten Speicher Konto jetzt Drehen der Benutzer aufgefordert, den Dienst Daten Verschlüsselung Key eingeben. Dies ist eine Änderung aus der vorherigen Version in der der Dienst Datenschlüssel als Inline-Parameter übergeben wurde. | Ja | Nein |
| 11 | Failback innerhalb von 24 Stunden | Während der Wiederherstellung das Aufräumen auf dem Quellgerät nicht ordnungsgemäß, sodass Failback zum Fehlschlagen erfolgen. Dies wurde in dieser Version behoben. | Ja | Nein |

## <a name="known-issues-in-the-october-release"></a>Bekannte Probleme in der Oktober-Version

Die folgende Tabelle enthält eine Übersicht über bekannte Probleme in dieser Version.

| Nein. | Feature | Problem | Kommentare/dieses Problem zu umgehen | Gilt für physische Gerät | Gilt für virtuelles Gerät |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Factory zurücksetzen | In einigen Fällen beim Ausführen einer Factory zurücksetzen das StorSimple Gerät ist hängen geblieben und diese Meldung angezeigt: **Zurücksetzen Factory wird ausgeführt (Phase 8)**. Dies passiert, wenn Sie STRG + C drücken, während das Cmdlet ausgeführt wird. | Drücken Sie STRG + C nicht nach dem Initiieren einer Factory zurücksetzen. Wenn Sie bereits in diesem Zustand sind, wenden Sie sich an den Microsoft-Support, für den nächsten Schritten fort. | Ja | Nein |
| 2 | Factory zurücksetzen | Führen Sie ein StorSimple Gerät, das von GA auf Oktober 2014 aktualisiert wird nicht Factory zurücksetzen lassen Sie wieder los. | Dieser Vorgang funktioniert nur, wenn ein Patch installiert ist. Wenden Sie sich an Microsoft Support, um diese erforderliche Patch zu erhalten. | Ja | Nein | 
| 3 | Quorum Datenträger | Gelegentlich Wenn die Mehrzahl der Datenträger in der Einheit EBOD einer 8600 Gerät getrennt werden keine Quorum Datenträger wodurch, werden Speicherpool offline. Es verbleibt offline, auch wenn die Datenträger erneut verbunden sind. | Sie müssen das Gerät neu starten. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support, für den nächsten Schritten fort. | Ja | Nein |
| 4 | Cloud Momentaufnahme Fehlern | Gelegentlich kann eine Momentaufnahme der Cloud tritt der Fehler **Maximum Sicherung Grenzwert erreicht**. Dies geschieht, wenn Sie 255 online Klonen auf dem gleichen Gerät, aus der gleichen ursprünglichen Lautstärke überschreiten, die gelöscht wurden. | | Ja | Ja |
| 5 | Falsche Controller-ID | Wenn kein Ersatz Controller ausgeführt wird, kann als Controller 1 Controller 0 angezeigt. Während Controller Ersatz nachdem das Bild aus dem Peerknoten geladen wird kann die Controller-ID Anfangs als dem Peer-Controller ID angezeigt Dieses Verhalten möglicherweise auch gelegentlich nach einem Neustart des Systems angezeigt. |Es ist keine Benutzeraktion erforderlich. Dies wird sich selbst beheben nach Abschluss der Controller ersetzt. | Ja | Nein |
| 6 | Überwachen von Diagrammen Gerät | In dem Dienst StorSimple Manager die Überwachung Gerät Diagramme funktionieren nicht, wenn grundlegende oder in der Konfiguration des Proxyservers für das Gerät NTLM-Authentifizierung aktiviert ist. | Ändern Sie die Web-Proxy-Konfiguration für das Gerät, das mit Ihrem Dienst StorSimple Manager registriert ist, damit die Authentifizierung auf keine festgelegt ist. Führen Sie hierzu die der Windows PowerShell für StorSimple Set-HcsWebProxy-Cmdlet. | Ja | Ja |
| 7 | Speicherkonten | Verwenden zum Löschen des Speicherkontos der Speicherdienst ist ein nicht unterstütztes Szenario. Dies wird zu einer Situation führen, in dem Benutzerdaten abgerufen werden können. | | Ja | Ja |
| 8 | Geräte-failover | Mehrere Failovers eines Containers aus dem gleichen Quellgerät Lautstärke für verschiedene Geräte wird nicht unterstützt. | Failover auf einem einzelnen inaktive Gerät auf mehreren Geräten machen Lautstärke Container auf der ersten konnte nicht über Gerät Besitz Daten gehen verloren. Nach einem Failover werden diese Lautstärke Container angezeigt oder Verhalten sich anders, wenn Sie in der klassischen Azure-Portal angezeigt werden. | Ja | Nein |
| 9 | Installation | Während StorSimple Netzwerkadapter für SharePoint-Installation müssen Sie eine Gerät IP in Reihenfolge für die Installation erfolgreich abgeschlossen eingeben.    | | Ja | Nein |
| 10 | Web-proxy | Wenn die Web Proxy-Konfiguration HTTPS als das angegebene Protokoll verfügt, Ihr Gerät-zu-Service-Kommunikation beeinflusst werden, und das Gerät wird im Offlinemodus wechselt. Support-Paketen werden auch im Prozess, Verarbeitung erhebliche Ressourcen auf Ihrem Gerät generiert werden. | Stellen Sie sicher, dass die Web-Proxy-URL HTTP als das angegebene Protokoll enthält. Weitere Informationen zum [Konfigurieren von Webproxy für Ihr Gerät](storsimple-configure-web-proxy.md). | Ja | Nein |
| 11 | Web-proxy | Wenn Sie konfigurieren und Webproxy auf einem Gerät registrierten aktivieren, müssen Sie den aktiven Controller auf Ihrem Gerät neu starten. | | Ja | Nein |
| 12 | Hohe Cloud Wartezeit und hoher e/a-Arbeitsbelastung | Wenn Ihr Gerät StorSimple eine Kombination von sehr hoch Cloud Wartezeiten (Reihenfolge der Sekunden) und hoher e/a-Arbeitsbelastung trifft, die Gerät Datenmengen in einem beeinträchtigt Zustand wechseln und e / mit einem Fehler "Gerät nicht bereit" fehlschlagen. | Sie müssen manuell neu zu starten die Gerätecontroller oder einen Gerät Failover zum Beheben des Problems führen. | Ja | Nein |

## <a name="physical-device-updates-in-the-october-release"></a>Lassen Sie wieder los physische Gerät Aktualisierungen in der Oktober

Wenn diese Updates zu einem physischen Gerät angewendet werden, wird die Version der Software in 6.3.9600.17312 ändern. Sofern nicht anders angegeben, werden diese Versionsinformationen gelten für alle Modelle des Geräts StorSimple. Weitere Informationen zu diesen Updates finden Sie unter [Oktober 2014 physische Einheit Software für Microsoft Azure StorSimple Einheit aktualisieren](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-october-release"></a>Lassen Sie wieder los seriell angeschlossenen SCSI (SAS) Controller und Firmwareupdates in der Oktober

Diese Version aktualisiert der Treiber und die Firmware auf dem SAS-Controller von Ihrem Gerät physisch. Weitere Informationen zu den SAS-Controller aktualisieren Sie, finden Sie unter [Oktober 2014 für LSI SAS Controller in Microsoft Azure StorSimple Einheit aktualisieren](http://support.microsoft.com/kb/2987020).   

Diese Version gilt auch ein kumulierte Firmwareupdate, die Adressen Zuverlässigkeitsprobleme mit dem Gerät Hardware-Komponenten. Weitere Informationen zu den Firmwareupdate finden Sie unter [Oktober 2014 Firmware für Microsoft Azure StorSimple Einheit aktualisieren](http://support.microsoft.com/kb/2987015).  

## <a name="virtual-device-updates-in-the-october-release"></a>Lassen Sie wieder los virtuelles Gerät Aktualisierungen in der Oktober

Diese Version enthält alle verfügbaren Updates für das virtuelle Gerät nicht. Anwenden dieser Aktualisierung ändert sich nicht auf die Softwareversion von einem virtuellen Gerät aus.
 
