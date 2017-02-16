<properties 
   pageTitle="StorSimple 8000 Reihe Update 2 Versionsinformationen | Microsoft Azure"
   description="Beschreibt die neuen Features, Probleme und problemumgehungen für StorSimple 8000 Reihe Update 2."
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
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-2-release-notes"></a>Versionsinformationen StorSimple 8000 Reihe Update 2  

## <a name="overview"></a>(Übersicht)

Die folgenden Versionsinformationen beschreiben der neuen Features und Identifizieren der geöffneten Probleme für StorSimple 8000 Reihe Update 2. Sie enthalten auch eine Liste der StorSimple Software, Treiber und Datenträger Firmwareupdates in dieser Version enthalten. 

Update 2 kann auf ein beliebiges StorSimple Gerät ausführen Release (GA) oder-Aktualisierung 0,1 bis Update 1.2 angewendet werden. Die Geräteversion Update 2 zugeordnet ist 6.3.9600.17673.

Überprüfen Sie die Informationen in den Versionsinformationen enthalten hat, bevor Sie das Update in Ihre Lösung StorSimple bereitstellen.

>[AZURE.IMPORTANT]
> 
- Dauert ungefähr 4 bis 7 Stunden zur Installation dieses Updates (einschließlich der Windows-Updates). 
- Update 2 hat die Software, LSI-Treiber und Firmwareupdates SSD.
- Neue Versionen Sie möglicherweise nicht finden Sie unter Updates sofort auf, da wir eine geplante Bereitstellung von Updates führen. Warten Sie einige Tage, und klicken Sie dann nach Updates erneut als diese werden erst verfügbar verfügbar.


## <a name="whats-new-in-update-2"></a>Was ist neu in Update 2

Update 2 werden die folgenden neuen Features vorgestellt.

- **Lokal angehefteten Datenmengen** – wurden In früheren Versionen von der Reihe StorSimple 8000 Datenblöcke gestuft in der Cloud, je nach Verwendung. Es wurde keine Möglichkeit, um sicherzustellen, dass Blöcke auf lokalen bleiben würden. Unter Update 2 beim Erstellen einer Volumen, können Sie ein Volume festlegen, lokal angeheftete und primäre Daten aus dem Volume nicht in der Cloud gestuft werden werden. Momentaufnahmen der lokal angeheftete Datenmengen werden immer noch in der Cloud für die Sicherung kopiert werden, damit die Cloud für die Daten Mobilität und Disaster Wiederherstellung verwendet werden kann. Darüber hinaus können Sie den Lautstärke Typ ändern (d. h., konvertieren gestuft Datenmengen zu lokal angeheftete Datenmengen und gestuft konvertieren lokal angehefteten Datenmengen zu). 

- **StorSimple virtuelles Gerät Verbesserungen** – positioniert zuvor, die Reihe StorSimple 8000 virtuelle Gerät als Disaster Wiederherstellung oder Entwicklung/Test-Lösung. Es ist nur ein Modell virtuelle Gerät (Modell 1100). Update 2 werden zwei virtuelle Gerätemodelle beschrieben: 

     - 8010 (früher der 1100 genannt) – keine Änderung; hat eine Kapazität von 30 TB und Azure standard-Speicher verwendet.
     - 8020 – hat eine Kapazität von 64 TB und Azure Premium Speicher zum Verbessern der Leistung verwendet.

    Es gibt eine einzelne virtuelle Festplatte für beide Modelle virtuelles Gerät (8010/8020) aus. Wenn Sie das virtuelle Gerät zum ersten Mal starten, erkennt die Parameter Plattform und gilt für die richtige Version.

- **Verbesserungen networking** – Update 2 enthält die folgenden Netzwerken Verbesserungen an:

     - Für die Cloud können mehrere NICs aktiviert sein, damit Failover auftreten kann, wenn Sie ein Netzwerkadapter schlägt fehl.
     - Weiterleitung Verbesserungen, mit festen Kennzahlen für Cloud aktiviert Blöcke.
     - Fehler beim Ressourcen vor einem Failover Wiederholung Online.
     - Neue Benachrichtigungen für fehlgeschlagene Dienste.

- **Verbesserte aktualisieren** – Update 1.2 und früher, die Reihe StorSimple 8000 über zwei Kanäle aktualisiert wurde: Windows Update für Cluster, iSCSI, und So weiter und Microsoft Update für Binärdateien und Firmware.
    Update 2 verwendet Microsoft Update für alle Pakete zu aktualisieren. Dies sollte zu weniger Zeit Patch oder Failovers Aktionen führen. 

- **Firmwareupdates** – die folgenden Firmwareupdates enthalten sind:
    - LSI: lsi_sas2.sys 2.00.72.10 Version des Produkts
    - Nur SSD (keine Festplatte Updates): XMGG, XGEG, KZ50, F6C2 und VR08

- Mit der **Proaktiven Support** – Update 2 für Microsoft zusätzliche Diagnoseinformationen vom Gerät zu extrahieren. Wenn unser Team Vorgänge Geräte, die Probleme auftreten bezeichnet, können wir besser ausgestattet, um Informationen vom Gerät zu sammeln und diagnostizieren Probleme. **Indem Sie akzeptieren Update 2, Sie können uns zur Bereitstellung dieser proaktive Unterstützung**.    
 

## <a name="issues-fixed-in-update-2"></a>Probleme in Update 2

In den folgenden Tabellen enthält eine Zusammenfassung der Probleme, die Updates 2 behoben wurden.    

| Nein. | Feature | Problem | Gilt für physische Gerät | Gilt für virtuelles Gerät |
|-----|---------|-------|--------------------------------|--------------------------------|
| 1 | Netzwerk-Schnittstellen | Nach einem Upgrade auf Update 1 gemeldet der StorSimple Manager-Dienst an, dass die Ports Data2 und Data3 auf einen Controller fehlgeschlagen ist. Dieses Problem hat behoben wurde. | Ja | Nein |
| 2 | Updates | Nach einem Upgrade auf Update 1 ist aufgetreten Erinnerung akustischen Benachrichtigungen im klassischen Azure-Portal auf mehreren Geräten ein. Dieses Problem hat behoben wurde. | Ja | Nein |
| 3 | Openstack-Authentifizierung | Bei Verwendung von Openstack als Cloud Service Provider könnten Sie eine Fehlermeldung, dass Ihre Cloud-Authentifizierungszeichenfolge zu lang war. Dies wurde behoben. | Ja | Nein |


## <a name="known-issues-in-update-2"></a>Bekannte Probleme in Update 2

Die folgende Tabelle enthält eine Übersicht über bekannte Probleme in dieser Version.

| Nein. | Feature | Problem | Kommentare / dieses Problem zu umgehen | Gilt für physische Gerät | Gilt für virtuelles Gerät |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Quorum Datenträger | Gelegentlich Wenn die Mehrzahl der Datenträger in der Einheit EBOD einer 8600 Gerät getrennt werden keine Quorum Datenträger wodurch, werden dann Speicherpool offline geleitet. Es verbleibt offline, auch wenn die Datenträger erneut verbunden sind. | Sie müssen das Gerät neu starten. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft-Support, für den nächsten Schritten fort. | Ja | Nein |
| 2 | Falsche Controller-ID | Wenn kein Ersatz Controller ausgeführt wird, kann als Controller 1 Controller 0 angezeigt. Während Controller Ersatz nachdem das Bild aus dem Peerknoten geladen wird kann die Controller-ID Anfangs als dem Peer-Controller ID angezeigt Dieses Verhalten möglicherweise auch gelegentlich nach einem Neustart des Systems angezeigt. | Es ist keine Benutzeraktion erforderlich. Dies wird sich selbst beheben nach Abschluss der Controller ersetzt. | Ja | Nein |
| 3 | Speicherkonten | Verwenden zum Löschen des Speicherkontos der Speicherdienst ist ein nicht unterstütztes Szenario. Dies wird zu einer Situation führen, in dem Benutzerdaten abgerufen werden können.|  | Ja | Ja |
| 4 | Geräte-failover | Mehrere Failovers eines Containers aus dem gleichen Quellgerät Lautstärke für verschiedene Geräte wird nicht unterstützt. Failover auf einem einzelnen inaktive Gerät auf mehreren Geräten machen Lautstärke Container auf der ersten konnte nicht über Gerät Besitz Daten gehen verloren. Nach einem Failover werden diese Lautstärke Container angezeigt oder Verhalten sich anders, wenn Sie in der klassischen Azure-Portal angezeigt werden. | | Ja | Nein |
| 5 | Installation | Während StorSimple Netzwerkadapter für SharePoint-Installation müssen Sie eine Gerät IP in Reihenfolge für die Installation erfolgreich abgeschlossen eingeben.    | | Ja | Nein |
| 6 | Web-proxy | Wenn die Web Proxy-Konfiguration HTTPS als das angegebene Protokoll verfügt, Ihr Gerät-zu-Service-Kommunikation beeinflusst werden, und das Gerät wird im Offlinemodus wechselt. Support-Paketen werden auch im Prozess, Verarbeitung erhebliche Ressourcen auf Ihrem Gerät generiert werden. | Stellen Sie sicher, dass die Web-Proxy-URL HTTP als das angegebene Protokoll enthält. Weitere Informationen zum [Konfigurieren von Webproxy für Ihr Gerät](storsimple-configure-web-proxy.md)wechseln. | Ja | Nein |
| 7 | Web-proxy | Wenn Sie konfigurieren und Webproxy auf einem Gerät registrierten aktivieren, müssen Sie den aktiven Controller auf Ihrem Gerät neu starten. | | Ja | Nein |
| 8 | Hohe Cloud Wartezeit und hoher e/a-Arbeitsbelastung | Wenn Ihr Gerät StorSimple eine Kombination von sehr hoch Cloud Wartezeiten (Reihenfolge der Sekunden) und hoher e/a-Arbeitsbelastung trifft, die Gerät Datenmengen in einem beeinträchtigt Zustand wechseln und e / mit einem Fehler "Gerät nicht bereit" fehlschlagen. | Sie müssen manuell neu zu starten die Gerätecontroller oder einen Gerät Failover zum Beheben des Problems führen. | Ja | Nein |
| 9 | Azure PowerShell | Wenn Sie das Cmdlet StorSimple **Get-AzureStorSimpleStorageAccountCredential & #124 verwenden. Select-Object zuerst 1 - warten** das erste Objekt aktivieren, damit Sie ein neues **VolumeContainer** Objekt erstellen können, das Cmdlet gibt alle Objekte. | Umbrechen des Cmdlets in Klammern wie folgt: **(Get-Azure-StorSimpleStorageAccountCredential) & #124; Select-Object zuerst 1 - warten** | Ja | Ja |
| 10| Migration | Bei mehreren Lautstärke Container für die Migration weitergegeben werden, wird der ETA für die neueste Sicherung nur für den ersten Lautstärke Container genau. Darüber hinaus kann parallele Migration starten, nachdem die ersten 4 Sicherungskopien in den ersten Lautstärke Container migriert werden. | Es empfiehlt sich, dass Sie jeweils eine Lautstärke Container migriert werden. | Ja | Nein |
| 11| Migration | Nach der Wiederherstellung werden Datenmengen nicht die Sicherung Richtlinie oder der virtuelle Laufwerkgruppe hinzugefügt. | Sie benötigen diese Datenträger Sicherungskopien zum Erstellen einer Sicherungskopie Richtlinie hinzufügen. | Ja | Ja |
| 12| Migration | Nach Abschluss die Migration muss das 5000/7000-er Gerät nicht den Datencontainer migrierte zugreifen. | Es empfiehlt sich, dass Sie die Datencontainer migrierte löschen, nachdem die Migration abgeschlossen ist. | Ja | Nein |
| 13| Klonen und DR | Ein Update 1 ausführen StorSimple-Gerät nicht Klonen oder Wiederherstellung zu einem vor dem Update 1 Software-Gerät ausführen. | Sie müssen das Gerät 1 aktualisieren, um diese Vorgänge zu erlauben aktualisieren | Ja | Ja |
| 14 | Migration | Sicherungskopie der Konfiguration für die Migration kann auf einem 5000-7000 Reihe Gerät möglicher Fehler beim Lautstärke Gruppen mit keine zugeordneten Datenträger vorhanden sind. | Löschen Sie alle leeren Lautstärke Gruppen mit keine zugeordneten Datenmengen, und wiederholen Sie dann die Sicherung Konfiguration.| Ja | Nein |
| 15 | Azure PowerShell-Cmdlets und lokal angeheftete Datenmengen | Sie können kein lokales angeheftete Volume über Azure PowerShell-Cmdlets erstellen. (Sie wird jedes Volume über Azure PowerShell erstellten gestuft.) |Verwenden des StorSimple Manager-Diensts immer lokal angeheftete Datenmengen konfigurieren.| Ja | Nein |
| 16 |Für lokal angeheftete Datenmengen verfügbarer Speicherplatz | Wenn Sie ein lokales angeheftete Volume löschen, der verfügbaren Speicherplatz für neue Datenmengen möglicherweise nicht sofort aktualisiert. Der Dienst StorSimple-Manager aktualisiert den lokalen verfügbaren Speicherplatz ungefähr pro Stunde an.| Warten Sie eine Stunde, bevor Sie versuchen, das neue Volume erstellt. | Ja | Nein |
| 17 | Lokal angeheftete Datenmengen | Ihre Arbeit wiederherstellen macht die Snapshotsicherung temporäre im Katalog Sicherung, aber nur für die Dauer des Projekts wiederherstellen. Darüber hinaus zur Verfügung stellt einer virtuelle Laufwerkgruppe mit Präfix **TmpCollection** auf der Seite **Sicherung Richtlinien** , aber nur für die Dauer des Projekts wiederherstellen. | Dieses Verhalten kann auftreten, wenn Ihre Arbeit Wiederherstellen nur lokal Datenmengen oder eine Mischung lokal angeheftete und gestufte Datenmengen angehefteten hat. Wenn Sie der Wiederherstellungsauftrag nur gestufte Datenmengen enthält, wird dieses Verhalten nicht auftreten. Es ist kein Eingriff erforderlich. | Ja | Nein |
| 18 | Lokal angeheftete Datenmengen | Tritt auf, wenn Sie ein Projekt wiederherstellen und ein Controller Failover Abbrechen sofort, danach der Wiederherstellungsauftrag **fehlgeschlagen** statt **storniert**angezeigt wird. Wenn ein Wiederherstellungsauftrag fehlschlägt und ein Controller ausgeführt wird sofort danach statt **Fehler beim** **abgebrochen** der Wiederherstellungsauftrag wird angezeigt. | Dieses Verhalten kann auftreten, wenn Ihre Arbeit Wiederherstellen nur lokal Datenmengen oder eine Mischung lokal angeheftete und gestufte Datenmengen angehefteten hat. Wenn Sie der Wiederherstellungsauftrag nur gestufte Datenmengen enthält, wird dieses Verhalten nicht auftreten. Es ist kein Eingriff erforderlich. | Ja | Nein |
| 19 |Lokal angeheftete Datenmengen | Wenn Sie einen Wiederherstellungsauftrag abbrechen oder eine Wiederherstellung schlägt fehl, und klicken Sie dann ein Controller Failover auftritt, wird eine zusätzliche wiederherstellen Position auf der Seite **Projekte** angezeigt. | Dieses Verhalten kann auftreten, wenn Ihre Arbeit Wiederherstellen nur lokal Datenmengen oder eine Mischung lokal angeheftete und gestufte Datenmengen angehefteten hat. Wenn Sie der Wiederherstellungsauftrag nur gestufte Datenmengen enthält, wird dieses Verhalten nicht auftreten. Es ist kein Eingriff erforderlich. | Ja | Nein |
| 20 |Lokal angeheftete Datenmengen | Wenn Sie versuchen, eine gestufte Volumen (und duplizierten mit Update 1.2 oder früher erstellt) in ein lokales angeheftete Volume umwandeln und Ihr Gerät nicht mehr Speicherplatz genügend oder Cloud derzeit nicht zur Verfügung, können die Clone(s) beschädigt.| Dieses Problem tritt nur bei Datenmengen, die Software erstellt wurden und mit duplizierten vor dem Update 2 wurden. Dies sollte eine seltene Szenario sein.|
| 21 | Volumen-Konvertierung | Die ACRs auf einen Datenträger angefügt werden, während eine Konvertierung Lautstärke abgehalten wird nicht aktualisiert (Abstufung auf lokal angehefteten oder umgekehrt). Aktualisieren der ACRs kann Beschädigung der Daten führen. | Bei Bedarf aktualisieren Sie der ACRs vor der Konvertierung Lautstärke, und treffen Sie keine weiteren ACR Updates während die Konvertierung ausgeführt wird. |

## <a name="controller-and-firmware-updates-in-update-2"></a>Controller und Firmware-Updates in Update 2

Diese Version aktualisiert der Treiber und die Festplatten-Software auf Ihrem Gerät.
 
- Weitere Informationen zu den LSI Firmware aktualisieren Sie, finden Sie im Microsoft Knowledge Base-Artikel 3121900. 
- Weitere Informationen über die Festplatten-Software aktualisieren Sie, finden Sie im Microsoft Knowledge Base-Artikel 3121899.
 
## <a name="virtual-device-updates-in-update-2"></a>Virtuelles Gerät Aktualisierungen in Update 2

Dieses Update kann nicht auf das virtuelle Gerät angewendet werden. Neue virtuelle Geräte müssen erstellt werden. 

## <a name="next-step"></a>Als Nächstes

Erfahren Sie, wie Sie auf Ihrem Gerät StorSimple [Update 2 installieren](storsimple-install-update-2.md) .
