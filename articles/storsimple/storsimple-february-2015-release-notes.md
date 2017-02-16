<properties 
   pageTitle="StorSimple 8000 Update 0,3 Versionsinformationen | Microsoft Azure"
   description="Beschreibt die neuen Features und Updates, Tagesordnungspunkte und problemumgehungen zur Verfügung, für die Februar 2015 Microsoft Azure StorSimple Release (Update 0,3)."
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
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>0,3 Versionsinformationen StorSimple 8000 Reihe Update - Februar 2015

## <a name="overview"></a>(Übersicht)

Die folgenden Versionsinformationen stellen die kritische Tagesordnungspunkte für StorSimple 8000 Reihe Update 0,3 in Februar 2015 freigegeben. Sie enthalten auch eine Liste der StorSimple Software- und Firmwareversionen Aktualisierungen in dieser Version enthalten. Dies ist das dritte Release, nachdem die StorSimple 8000 Reihe aktualisierte Version Juli 2014 im Allgemeinen zur Verfügung gestellt wurde.
  
Dieses Update ändert sich die Version der Software nicht aus der Januar aktualisieren. Es wird weiterhin Version 6.3.9600.17312 sein. Sie können bestätigen, dass das Update installiert wurde, indem Sie das Datum der **Letzten Aktualisierung** aktivieren. Wenn das Datum 2/10/2015 oder höher ist, wurde das Update erfolgreich installiert.  

Es empfiehlt sich, die Sie für Scannen und Anwenden von verfügbaren Updates sofort nach der Installation von Ihrem Geräts StorSimple. Sie können auch aktivieren automatische Updates zum Herunterladen und Installieren von Updates mit hoher Priorität von Microsoft, sobald sie freigegeben werden. Weitere Informationen finden Sie unter [Aktualisieren von Ihrem Gerät StorSimple](storsimple-update-device.md).  

Überprüfen Sie die Informationen in den Versionsinformationen enthalten hat, bevor Sie das Update in Ihre Lösung StorSimple bereitstellen.  

>[AZURE.IMPORTANT]   
>
> - Verwenden Sie den StorSimple-Manager-Dienst und nicht Windows PowerShell für StorSimple Sie das Update vom Februar an.   
> - Es dauert ungefähr eine Stunde, dieses Update zu installieren. Jedoch, wenn Sie über die kumulativen Updates installieren, kann der Prozess ungefähr 3 Stunden dauern.  
> - Februar-Release von StorSimple enthält alle Aktualisierungen der StorSimple virtuelles Gerät nicht. Sie können weiterhin alle verfügbaren Updates für Windows auf das virtuelle Gerät, aktuelle Sicherheitsupdates, einschließlich anwenden, aber eine Änderung in der Version für das virtuelle Gerät wird nicht angezeigt.  

Stellen Sie sicher, dass die folgenden Komponenten vor der Aktualisierung von Ihrem Geräts StorSimple erfüllt sind.  

- Stellen Sie sicher, dass beide Gerätecontroller ausgeführt werden, bevor Sie nach Updates suchen. Wenn entweder Controller nicht ausgeführt wird, schlägt die Überprüfung fehl. Um zu überprüfen, dass die Controller in einem ordnungsgemäßen Zustand sind, navigieren Sie zu **Hardware Status** aus, klicken Sie unter der Seite **zum Warten** . Falls Komponenten die **Aufmerksamkeit Bedarf sind**, wenden Sie sich an den Microsoft-Support Vorgang fortsetzen.
- Sicherstellen Sie, dass die feste IP-Adressen für sowohl 0 und Controller 1 geroutet werden und mit dem Internet verbinden können, da diese für die Wartung der Updates auf dem Gerät verwendet werden. Sie können das [Cmdlet "Verbindung testen"](https://technet.microsoft.com/library/hh849808.aspx) verwenden, Signal außerhalb des Netzwerks, z. B. outlook.com, um sicherzustellen, dass der Controller mit dem externen Netzwerk verbunden ist eine bekannte Adresse an.
- Stellen Sie sicher, dass die Ports 80 und 443 auf Ihrem Gerät StorSimple für ausgehende Kommunikation zur Verfügung stehen. Weitere Informationen finden Sie unter die [Anforderungen für Ihr Gerät StorSimple Networking](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
- Ist die Version der Software 6.3.9600.17312 (Oktober 2014 Update) überschreiten, deaktivieren Sie die Ports Daten 2 und 3 von Daten, wenn vor dem Starten der Aktualisierung aktiviert. Verlassen der Daten 2 oder Daten 3 Ports aktiviert, wenn Sie das Update anwenden möglicherweise Ihr Gerätecontroller Wiederherstellungsmodus wechseln. Bitte beachten Sie, wenn Sie die Netzwerk-Schnittstellen deaktivieren, alle zugehörigen Datenträger offline geschaltet und die Ausgaben für die Dauer der Aktualisierung unterbrochen.  
  
## <a name="whats-new-in-the-february-release"></a>Was ist neu in der Version Februar

Dieses Update enthält eine Lösung für die Factory Problem, die zurücksetzen auf aufgetreten Geräten, die von der GA-Bereich aktualisiert wurde in der Version Oktober 2014 lassen. Weitere Informationen finden Sie unter [Probleme behoben in dieser Version](#issues-fixed-in-the-february-release).   

Dieses Update enthält keine neuen Features und Funktionen.  

## <a name="issues-fixed-in-the-february-release"></a>Probleme in der Februar-Version

Die folgende Tabelle beschreibt das Problem, das in diesem Update behoben wurde.  
 
| Nein. | Feature | Problem | Gilt für physische Gerät | Gilt für virtuelles Gerät |
|-----|---------|-------|---------------------------------|-------------------------------|
| 1 | Factory zurücksetzen | Sie versuchen, eine Fabrik zurücksetzen auf einem Gerät, die ursprünglich die GA-Version (Version 6.3.9600.17215) installiert haben, aber die Oktober aktualisiert wurde durchzuführen Release (Version 6.3.9600.17312). Die Factory zurücksetzen fehlschlägt, und das Gerät instabil wird. | Ja | Nein |


## <a name="known-issues-in-the-february-release"></a>Bekannte Probleme in der Februar-Version

Die folgende Tabelle enthält eine Übersicht über bekannte Probleme in dieser Version.
 
| Nein. | Feature | Problem | Kommentare/dieses Problem zu umgehen | Gilt für physische Gerät  | Gilt für virtuelles Gerät |
|-----|---------|-------|----------------------------|-----------------------------|--------------------------|
| 1 | Factory zurücksetzen | In einigen Fällen beim Ausführen einer Factory zurücksetzen das StorSimple Gerät ist hängen geblieben und diese Meldung angezeigt: **Zurücksetzen Factory wird ausgeführt (Phase 8)**. Dies passiert, wenn Sie STRG + C drücken, während das Cmdlet ausgeführt wird. | Drücken Sie STRG + C nicht nach dem Initiieren einer Factory zurücksetzen. Wenn Sie bereits in diesem Zustand sind, wenden Sie sich an den Microsoft-Support, für den nächsten Schritten fort. | Ja | Nein |
| 2 | Datenträger quorum | Gelegentlich Wenn die Mehrzahl der Datenträger in der Einheit EBOD, der eine 8600device getrennt werden keine Quorum Datenträger wodurch, werden Speicherpool offline. Es verbleibt offline, auch wenn die Datenträger erneut verbunden sind. | Sie müssen das Gerät neu starten. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support, für den nächsten Schritten fort. | Ja | Nein |
| 3 | Cloud Momentaufnahme Fehlern | Gelegentlich kann eine Momentaufnahme der Cloud tritt der Fehler **Maximum Sicherung Grenzwert erreicht**. Dies geschieht, wenn Sie 255 online Klonen auf dem gleichen Gerät, aus der gleichen ursprünglichen Lautstärke überschreiten, die gelöscht wurden. |  | Ja | Ja |
| 4 | Falsche Controller-ID | Wenn kein Ersatz Controller ausgeführt wird, kann als Controller 1 Controller 0 angezeigt. Während Controller Ersatz nachdem das Bild aus dem Peerknoten geladen wird kann die Controller-ID Anfangs als dem Peer-Controller ID angezeigt Dieses Verhalten möglicherweise auch gelegentlich nach einem Neustart des Systems angezeigt. | Es ist keine Benutzeraktion erforderlich. Dies wird sich selbst beheben nach Abschluss der Controller ersetzt. | Ja | Nein |
| 5 | Überwachen von Diagrammen Gerät | In dem Dienst StorSimple Manager die Überwachung Gerät Diagramme funktionieren nicht, wenn grundlegende oder in der Konfiguration des Proxyservers für das Gerät NTLM-Authentifizierung aktiviert ist. | Ändern Sie die Web-Proxy-Konfiguration für das Gerät, das mit Ihrem Dienst StorSimple Manager registriert ist, damit die Authentifizierung auf keine festgelegt ist. Führen Sie hierzu die der Windows PowerShell für StorSimple Set-HcsWebProxy-Cmdlet. | Ja | Ja |
| 6 | Speicherkonten | Verwenden zum Löschen des Speicherkontos der Speicherdienst ist ein nicht unterstütztes Szenario. Dies wird zu einer Situation führen, in dem Benutzerdaten abgerufen werden können. |  | Ja | Ja |
| 7 | Geräte-failover | Mehrere Failovers eines Containers aus dem gleichen Quellgerät Lautstärke für verschiedene Geräte wird nicht unterstützt.  Failover auf einem einzelnen inaktive Gerät auf mehreren Geräten machen Lautstärke Container auf der ersten konnte nicht über Gerät Besitz Daten gehen verloren. Nach einem Failover werden diese Lautstärke Container angezeigt oder Verhalten sich anders, wenn Sie in der klassischen Azure-Portal angezeigt werden. |   | Ja | Nein |
| 8 | Installation | Während StorSimple Netzwerkadapter für SharePoint-Installation müssen Sie eine Gerät IP in Reihenfolge für die Installation erfolgreich abgeschlossen eingeben. |  | Ja | Nein |
| 9 | Web-proxy | Wenn die Web Proxy-Konfiguration HTTPS als das angegebene Protokoll verfügt, Ihr Gerät-zu-Service-Kommunikation beeinflusst werden, und das Gerät wird im Offlinemodus wechselt. Support-Paketen werden auch im Prozess, Verarbeitung erhebliche Ressourcen auf Ihrem Gerät generiert werden. | Stellen Sie sicher, dass die Web-Proxy-URL HTTP als das angegebene Protokoll enthält. Weitere Informationen zum [Konfigurieren von Webproxy für Ihr Gerät](storsimple-configure-web-proxy.md). | Ja | Nein |
| 10 | Web-proxy | Wenn Sie konfigurieren und Webproxy auf einem Gerät registrierten aktivieren, müssen Sie den aktiven Controller auf Ihrem Gerät neu starten. |  | Ja | Nein |
| 11 | Hohe Cloud Wartezeit und hoher e/a-Arbeitsbelastung | Wenn Ihr Gerät StorSimple eine Kombination von sehr hoch Cloud Wartezeiten (Reihenfolge der Sekunden) und hoher e/a-Arbeitsbelastung trifft, die Gerät Datenmengen in einem beeinträchtigt Zustand wechseln und e / mit einem Fehler "Gerät nicht bereit" fehlschlagen. | Sie müssen manuell neu starten, die Gerätecontroller oder einen Gerät Failover zum Beheben des Problems führen. | Ja | Nein |

## <a name="physical-device-updates-in-the-february-release"></a>Lassen Sie wieder los physische Gerät Aktualisierungen in der Februar

Dieses Update behebt das Problem der Factory-zurücksetzen, die auf aufgetreten Geräten, die von der GA-Bereich aktualisiert wurde in der Version Oktober 2014 lassen. Er enthält keine anderen Updates auf dem Gerät StorSimple keine.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-february-release"></a>Lassen Sie wieder los seriell angeschlossenen SCSI (SAS) Controller und Firmwareupdates für Februar

Diese Version enthält alle Updates an den Seriell angeschlossenen SCSI (SAS) Controller oder die Firmware nicht. Die Treiber-Aktualisierung wurde in der Oktober, 2014 Release.  

## <a name="virtual-device-updates-in-the-february-release"></a>Lassen Sie wieder los virtuelles Gerät Aktualisierungen in der Februar

Diese Version enthält alle verfügbaren Updates für das virtuelle Gerät nicht. Anwenden dieser Aktualisierung ändert sich nicht auf die Softwareversion von einem virtuellen Gerät aus.
 
