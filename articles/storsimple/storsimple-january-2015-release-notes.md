<properties 
   pageTitle="0,2 Versionsinformationen StorSimple 8000 aktualisieren | Microsoft Azure"
   description="Beschreibt die neuen Features und Updates, Tagesordnungspunkte und verfügbaren problemumgehungen für den Januar 2015 Microsoft Azure StorSimple Release (Update 0,2)."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/16/2016"
   ms.author="v-sharos" />


# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>0,2 Versionsinformationen StorSimple 8000 Reihe Update - Januar 2015

## <a name="overview"></a>(Übersicht)

Die folgenden Versionsinformationen stellen die kritische Tagesordnungspunkte für den Januar 2015-Version von Microsoft Azure StorSimple. Sie enthalten auch eine Liste der StorSimple Software- und Firmwareversionen Aktualisierungen in dieser Version enthalten. Dies ist das zweite Release, nachdem die StorSimple 8000 Reihe aktualisierte Version Juli 2014 im Allgemeinen zur Verfügung gestellt wurde.
  
Dieses Update ändert sich die Version der Software physische Gerät nicht aus der Aktualisierung Oktober. Es wird weiterhin Version 6.3.9600.17312 sein. In dieser Version wurde das Bild im Bild virtuelles Gerät verwendeten geändert. Daher werden alle neuen virtuelle Geräte nach 1/20/2015 erstellt die Version als 6.3.9600.17361 angezeigt.  

Überprüfen Sie die folgende Informationen in den Release Notes für die Aktualisierung Januar 2015 enthalten sind.

> [AZURE.IMPORTANT]  
>
>- Dieses Update ist über Windows Update nicht verfügbar und kann nicht wie andere Updates installiert werden. Ihr Gerät erhalten keine dieses Update, auch wenn Sie die Updates mithilfe des Azure klassischen Portals angewendet haben. Dieses Update gilt nur für virtuelle Geräte nach 20 Januar 2015 erstellt. 
> 
>- Die Januar-Version von StorSimple enthält alle Updates für das StorSimple physische Gerät nicht. Sie können weiterhin alle verfügbaren Updates für Windows auf das virtuelle Gerät, aktuelle Sicherheitsupdates, einschließlich anwenden, aber eine Änderung in der Version für das StorSimple physische Gerät wird nicht angezeigt.

## <a name="whats-new-in-the-january-release"></a>Was ist neu in der Version Januar

Dieses Update enthält ein Update für den Wechsel in den Offlinemodus auf dem Gerät virtuelle Datenmengen. (Siehe [Probleme in dieser Version behoben](#issues-fixed-in-the-january-release).)  

Das Update enthält keine neuen Features und Funktionen.  

## <a name="issues-fixed-in-the-january-release"></a>Probleme in der Januar-Version

Die folgende Tabelle beschreibt das Problem, das in diesem Update behoben wurde.

|Nein.|Feature|Problem|Gilt für physische Gerät|Gilt für virtuelles Gerät 
|---|-------|-----|--------------------------|-------------------------
|1|Wechsel in den Offlinemodus Datenmengen|Wenn hohe Cloud Wartezeiten Minuten nicht beibehalten werden, gehen die StorSimple virtuelles Gerät Datenmengen auf den Hosts offline. Dieses Fix erhöht den Schwellenwert für Cloud Wartezeiten, damit auch die Situationen, die bewirken die Datenmengen, klicken Sie auf ' Hosts ' offline zu minimieren.|Nein|Ja  

## <a name="known-issues-in-the-january-release"></a>Bekannte Probleme in der Januar-Version

Die folgende Tabelle enthält eine Übersicht über bekannte Probleme in dieser Version.
 
|Nein.|Feature|Problem|Kommentare/dieses Problem zu umgehen|Gilt für physische Gerät|Gilt für virtuelles Gerät 
|---|-------|-----|-------------------|--------------------------|-------------------------
|1| Factory zurücksetzen|  In einigen Fällen beim Ausführen einer Factory zurücksetzen das StorSimple Gerät ist hängen geblieben und diese Meldung angezeigt: **Zurücksetzen Factory wird ausgeführt (Phase 8).** Dies passiert, wenn Sie STRG + C drücken, während das Cmdlet ausgeführt wird.| Drücken Sie STRG + C nicht nach dem Initiieren einer Factory zurücksetzen. Wenn Sie bereits in diesem Zustand sind, wenden Sie sich an den Microsoft-Support, für den nächsten Schritten fort.|Ja|Nein|
|2|Quorum Datenträger| Gelegentlich Wenn die Mehrzahl der Datenträger in der Einheit EBOD einer 8600 Gerät getrennt werden keine Quorum Datenträger wodurch, werden Speicherpool offline. Es verbleibt offline, auch wenn die Datenträger erneut verbunden sind.|Sie müssen das Gerät neu starten. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support, für den nächsten Schritten fort.|Ja |Nein
|3|Cloud Momentaufnahme Fehlern|Gelegentlich kann eine Momentaufnahme der Cloud tritt der Fehler **Maximum Sicherung Grenzwert erreicht**. Dies geschieht, wenn Sie 255 online Klonen auf dem gleichen Gerät, aus der gleichen ursprünglichen Lautstärke überschreiten, die gelöscht wurden.||Ja|Ja
|4|Falsche Controller-ID|Wenn kein Ersatz Controller ausgeführt wird, kann als Controller 1 Controller 0 angezeigt. Während Controller Ersatz nachdem das Bild aus dem Peerknoten geladen wird kann die Controller-ID Anfangs als dem Peer-Controller ID angezeigt  Dieses Verhalten möglicherweise auch gelegentlich nach einem Neustart des Systems angezeigt.|Es ist keine Benutzeraktion erforderlich. Dies wird sich selbst beheben nach Abschluss der Controller ersetzt.|Ja|Nein 
|5| Überwachen von Diagrammen Gerät|In dem Dienst StorSimple Manager die Überwachung Gerät Diagramme funktionieren nicht, wenn grundlegende oder in der Konfiguration des Proxyservers für das Gerät NTLM-Authentifizierung aktiviert ist.|Ändern Sie die Web-Proxy-Konfiguration für das Gerät mit Ihrem Dienst StorSimple Manager registriert ist, damit die Authentifizierung auf keine festgelegt ist. Führen Sie hierzu die der Windows PowerShell für StorSimple Set-HcsWebProxy-Cmdlet.|Ja|Ja
|6| Speicherkonten|Verwenden zum Löschen des Speicherkontos der Speicherdienst ist ein nicht unterstütztes Szenario. Dies wird zu einer Situation führen, in dem Benutzerdaten abgerufen werden können.|| Ja |  Ja
|7|Geräte-failover| Mehrere Failovers eines Containers aus dem gleichen Quellgerät Lautstärke für verschiedene Geräte wird nicht unterstützt.| Failover auf einem einzelnen inaktive Gerät auf mehreren Geräten machen Lautstärke Container auf der ersten konnte nicht über Gerät Besitz Daten gehen verloren. Nach einem Failover werden diese Lautstärke Container angezeigt oder Verhalten sich anders, wenn Sie in der klassischen Azure-Portal angezeigt werden.|Ja|Nein
|8| Installation|Während StorSimple Netzwerkadapter für SharePoint-Installation müssen Sie eine Gerät IP in Reihenfolge für die Installation erfolgreich abgeschlossen eingeben.||Ja|Nein
|9| Web-proxy|Wenn die Web Proxy-Konfiguration HTTPS als das angegebene Protokoll verfügt, Ihr Gerät-zu-Service-Kommunikation beeinflusst werden, und das Gerät wird im Offlinemodus wechselt. Support-Paketen werden auch im Prozess, Verarbeitung erhebliche Ressourcen auf Ihrem Gerät generiert werden.|Stellen Sie sicher, dass die Web-Proxy-URL HTTP als das angegebene Protokoll enthält. Weitere Informationen zum [Konfigurieren von Webproxy für Ihr Gerät](storsimple-configure-web-proxy.md)anzeigen.|Ja |Nein
|10|Web-proxy|  Wenn Sie konfigurieren und Webproxy auf einem Gerät registrierten aktivieren, müssen Sie den aktiven Controller auf Ihrem Gerät neu starten.|| Ja |Nein
|11|Hohe Cloud Wartezeit und hoher e/a-Arbeitsbelastung|Wenn Ihr Gerät StorSimple eine Kombination von sehr hoch Cloud Wartezeiten (Reihenfolge der Sekunden) und hoher e/a-Arbeitsbelastung trifft, die Gerät Datenmengen in einem beeinträchtigt Zustand wechseln und e / kann ein Fehler auftreten, mit einem Fehler "Gerät nicht bereit".|Sie müssen manuell neu starten, die Gerätecontroller oder einen Gerät Failover zum Beheben des Problems führen.|Ja|Nein

## <a name="physical-device-updates-in-the-january-release"></a>Lassen Sie wieder los physische Gerät Aktualisierungen in der Januar

Dieses Update enthält keine anderen Änderungen am Gerät StorSimple.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-january-release"></a>Lassen Sie wieder los seriell angeschlossenen SCSI (SAS) Controller und Firmwareupdates in der Januar

Diese Version enthält alle Updates an den Seriell angeschlossenen SCSI (SAS) Controller oder die Firmware nicht. Die Treiber-Aktualisierung wurde in der Oktober, 2014 Release. 

## <a name="virtual-device-updates-in-the-january-release"></a>Lassen Sie wieder los virtuelles Gerät Aktualisierungen in der Januar

Diese Version enthält ein aktualisierte Bild für das virtuelle Gerät. Alle virtuellen Geräte nach 20 Januar 2015 erstellt wurden, werden die Version der Software als 6.3.9600.17361 angezeigt.

 
