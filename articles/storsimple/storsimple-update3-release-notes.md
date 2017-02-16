<properties 
   pageTitle="StorSimple 8000 Reihe Update 3 – Versionsinformationen | Microsoft Azure"
   description="Beschreibt die neuen Features, Probleme und problemumgehungen für StorSimple 8000 Reihe Update 3."
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
   ms.date="10/14/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-3-release-notes"></a>StorSimple 8000 Reihe Update 3 – Versionsinformationen  

## <a name="overview"></a>(Übersicht)

Die folgenden Versionsinformationen beschreiben der neuen Features und Identifizieren der geöffneten Probleme für StorSimple 8000 Reihe Update 3. Sie enthalten auch eine Übersicht über die StorSimple Softwareupdates in dieser Version enthalten. 

Update 3 kann auf ein beliebiges StorSimple Gerät ausführen Release (GA) oder-Aktualisierung 0,1 bis Update 2.2 angewendet werden. Die Geräteversion Update 3 zugeordnet ist 6.3.9600.17759.

Überprüfen Sie die Informationen in den Versionsinformationen enthalten hat, bevor Sie das Update in Ihre Lösung StorSimple bereitstellen.

>[AZURE.IMPORTANT]
> 
> - Update 3 weist Gerätesoftware, LSI-Treiber und Firmware und Storport und Spaceport aktualisiert. Es dauert ungefähr 1,5 bis 2 Stunden, dieses Update zu installieren. 

> - Neue Versionen Sie möglicherweise nicht finden Sie unter Updates sofort auf, da wir eine geplante Bereitstellung von Updates führen. Warten Sie einige Tage, und klicken Sie dann nach Updates erneut als diese werden erst verfügbar verfügbar.


## <a name="whats-new-in-update-3"></a>Was ist neu in Update 3

Die folgenden wichtigen Verbesserungen und Updates wurden in Update 3 vorgenommen.

 
- Führen Sie **automatisierte Speicherplatz freigeben Änderungen** – Update 3 beginnen, die Speicherplatz freigeben Algorithmen auf dem standby Controller des Systems resultierender ermöglicht die schnellere Ausführung. Weitere Informationen zu den Ports, die für die Arbeit mit Leerzeichen Reservierung erforderlich sind, finden Sie in der [StorSimple Netzwerke Anforderungen](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

- **Verbesserte Performance** – Update 3 hat Lese-und Schreibzugriff Leistung in der Cloud verbessert.

- **Verbesserte Migrations-bezogene** – wurden In dieser Version, mehrere Updates und verbesserte fertig für das Feature für die Migration von 5000/7000 Reihe Geräten auf 8000 Reihe Geräte. Weitere Informationen zum Verwenden des Features für die Migration, wechseln Sie zu [der Migration von 5000/7000 Reihe Gerät auf 8000 Reihe Gerät](https://www.microsoft.com/en-us/download/details.aspx?id=47322). 

- **Verwandte Updates für die Überwachung** – wurden In dieser Version, die für die Überwachung von Diagrammen und Dienst Dashboard Gerät Dashboard Fehler behoben.


## <a name="issues-fixed-in-update-3"></a>Probleme in Update 3

In den folgenden Tabellen enthält eine Zusammenfassung der Probleme, die in Update 3 behoben wurden.    

| Nein | Feature                                    | Problem                                                                                                                                                                                                                                                                                        | Gilt für physische Gerät | Gilt für virtuelles Gerät |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | Migration von Host angeordneten Daten                      | In der früheren Version wurde die StorSimple Cloud Einheit während der Datenmigration Host angeordneten offline gezeigt. In dieser Version wird dieses Problem behoben.                                                | Nein                        | Ja                        |
| 2  | Lokal angeheftete Datenmengen                     | In der vorherigen Version es wurden Problemen im Zusammenhang mit e/a-Fehlern, Lautstärke Konvertierungsfehlern und Datapath Fehlern für lokales angeheftete Datenmengen. Diese Probleme waren Stamm verursacht und in dieser Version behoben.                | Ja                        | Nein                       |
| 3  | Für die Überwachung                                    | Es wurden mehrere Probleme bei der Berichterstattung Einheiten und für die Überwachung sowie Gerät Dashboard Diagramme, wo falsche Informationen für lokales angeheftete Datenmengen angezeigt wurde. In dieser Version werden diese Probleme behoben. | Ja                         | Nein                       |
| 4  | Beanspruchen schreibt e/a                          | StorSimple für Auslastung betreffen beanspruchen schreibt verwenden, würde der Benutzer in einem seltene Fehler ausgeführt, wurde die Menge der Arbeit in der Cloud gestuft wird. Dieser Fehler wird in dieser Version behoben. | Ja                        | Ja                       |
| 5  | Sicherung                                     | In bestimmten Fällen seltenen in den vorherigen Versionen der Software Wenn der Benutzer eine Sicherungskopie der einer remote Klonen ausgeführt hat, diese in die Cloud Fehler ausführen möchten und der Vorgang würde Fehler ausgegeben. In dieser Version dieses Problem behoben wird, und der Vorgang erfolgreich abgeschlossen.             | Ja                        | Ja                       |
| 6  | Zusätzliche Richtlinie                                     | In bestimmten Fällen seltenen in der früheren Versionen von Software, enthielt einen Fehler im Zusammenhang mit den Löschvorgang für die Sicherungsdatei Richtlinie. In dieser Version wird dieses Problem behoben.       | Ja                        | Ja                       |



## <a name="known-issues-in-update-3"></a>Bekannte Probleme in Update 3

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
| 8 | Hohe Cloud Wartezeit und hoher e/a-Arbeitsbelastung | Wenn Ihr Gerät StorSimple eine Kombination von sehr hoch Cloud Wartezeiten (Reihenfolge der Sekunden) und hoher e/a-Arbeitsbelastung trifft, die Gerät Datenmengen in einem beeinträchtigt Zustand wechseln und e / mit einem Fehler "Gerät nicht bereit" fehlschlagen. | Sie müssen manuell neu starten, die Gerätecontroller oder einen Gerät Failover zum Beheben des Problems führen. | Ja | Nein |
| 9 | Azure PowerShell | Wenn Sie das Cmdlet StorSimple **Get-AzureStorSimpleStorageAccountCredential & #124 verwenden. Select-Object zuerst 1 - warten** das erste Objekt aktivieren, damit Sie ein neues **VolumeContainer** Objekt erstellen können, das Cmdlet gibt alle Objekte. | Umbrechen des Cmdlets in Klammern wie folgt: **(Get-Azure-StorSimpleStorageAccountCredential) & #124; Select-Object zuerst 1 - warten** | Ja | Ja |
| 10| Migration | Bei mehreren Lautstärke Container für die Migration weitergegeben werden, wird der ETA für die neueste Sicherung nur für den ersten Lautstärke Container genau. Darüber hinaus kann parallele Migration starten, nachdem die ersten 4 Sicherungskopien in den ersten Lautstärke Container migriert werden. | Es empfiehlt sich, dass Sie jeweils eine Lautstärke Container migriert werden. | Ja | Nein |
| 11| Migration | Nach der Wiederherstellung werden Datenmengen nicht die Sicherung Richtlinie oder der virtuelle Laufwerkgruppe hinzugefügt. | Sie benötigen diese Datenträger Sicherungskopien zum Erstellen einer Sicherungskopie Richtlinie hinzufügen. | Ja | Ja |
| 12| Migration | Nach Abschluss die Migration muss das 5000/7000-er Gerät nicht den Datencontainer migrierte zugreifen. | Es empfiehlt sich, dass Sie die Datencontainer migrierte löschen, nachdem die Migration abgeschlossen ist. | Ja | Nein |
| 13| Klonen und DR | Ein Update 1 ausführen StorSimple-Gerät nicht Klonen oder Wiederherstellung zu einem vor dem Update 1 Software-Gerät ausführen. | Sie müssen das Gerät 1 aktualisieren, um diese Vorgänge zu erlauben aktualisieren | Ja | Ja |
| 14 | Migration | Sicherungskopie der Konfiguration für die Migration kann auf einem 5000-7000 Reihe Gerät möglicher Fehler beim Lautstärke Gruppen mit keine zugeordneten Datenträger vorhanden sind. | Löschen Sie alle leeren Lautstärke Gruppen mit keine zugeordneten Datenmengen, und wiederholen Sie dann die Sicherung Konfiguration.| Ja | Nein |
| 15 | Azure PowerShell-Cmdlets und lokal angeheftete Datenmengen | Sie können kein lokales angeheftete Volume über Azure PowerShell-Cmdlets erstellen. (Sie wird jedes Volume über Azure PowerShell erstellten gestuft.) |Verwenden des StorSimple Manager-Diensts immer lokal angeheftete Datenmengen konfigurieren.| Ja | Nein |
| 16 |Für lokal angeheftete Datenmengen verfügbarer Speicherplatz | Wenn Sie ein lokales angeheftete Volume löschen, der verfügbaren Speicherplatz für neue Datenmengen möglicherweise nicht sofort aktualisiert. Der StorSimple Manager-Dienst wird im lokalen Speicherplatz ungefähr stündlich aktualisiert.| Warten Sie eine Stunde, bevor Sie versuchen, das neue Volume erstellt. | Ja | Nein |
| 17 | Lokal angeheftete Datenmengen | Ihre Arbeit wiederherstellen macht die Snapshotsicherung temporäre im Katalog Sicherung, aber nur für die Dauer des Projekts wiederherstellen. Darüber hinaus zur Verfügung stellt einer virtuelle Laufwerkgruppe mit Präfix **TmpCollection** auf der Seite **Sicherung Richtlinien** , aber nur für die Dauer des Projekts wiederherstellen. | Dieses Verhalten kann auftreten, wenn Ihre Arbeit Wiederherstellen nur lokal Datenmengen oder eine Mischung lokal angeheftete und gestufte Datenmengen angehefteten hat. Wenn Sie der Wiederherstellungsauftrag nur gestufte Datenmengen enthält, wird dieses Verhalten nicht auftreten. Es ist kein Eingriff erforderlich. | Ja | Nein |
| 18 | Lokal angeheftete Datenmengen | Tritt auf, wenn Sie ein Projekt wiederherstellen und ein Controller Failover Abbrechen sofort, danach der Wiederherstellungsauftrag **fehlgeschlagen** statt **storniert**angezeigt wird. Wenn ein Wiederherstellungsauftrag fehlschlägt und ein Controller ausgeführt wird sofort danach statt **Fehler beim** **abgebrochen** der Wiederherstellungsauftrag wird angezeigt. | Dieses Verhalten kann auftreten, wenn Ihre Arbeit Wiederherstellen nur lokal Datenmengen oder eine Mischung lokal angeheftete und gestufte Datenmengen angehefteten hat. Wenn Sie der Wiederherstellungsauftrag nur gestufte Datenmengen enthält, wird dieses Verhalten nicht auftreten. Es ist kein Eingriff erforderlich. | Ja | Nein |
| 19 |Lokal angeheftete Datenmengen | Wenn Sie einen Wiederherstellungsauftrag abbrechen oder eine Wiederherstellung schlägt fehl, und klicken Sie dann ein Controller Failover auftritt, wird eine zusätzliche wiederherstellen Position auf der Seite **Projekte** angezeigt. | Dieses Verhalten kann auftreten, wenn Ihre Arbeit Wiederherstellen nur lokal Datenmengen oder eine Mischung lokal angeheftete und gestufte Datenmengen angehefteten hat. Wenn Sie der Wiederherstellungsauftrag nur gestufte Datenmengen enthält, wird dieses Verhalten nicht auftreten. Es ist kein Eingriff erforderlich. | Ja | Nein |
| 20 |Lokal angeheftete Datenmengen | Wenn Sie versuchen, eine gestufte Volumen (und duplizierten mit Update 1.2 oder früher erstellt) in ein lokales angeheftete Volume umwandeln und Ihr Gerät nicht mehr Speicherplatz genügend oder Cloud derzeit nicht zur Verfügung, können die Clone(s) beschädigt.| Dieses Problem tritt nur bei Datenmengen, die erstellt wurden und mit duplizierten vor dem Update 2.1 Software wurden. Dies sollte eine seltene Szenario sein.|
| 21 | Volumen-Konvertierung | Die ACRs auf einen Datenträger angefügt werden, während eine Konvertierung Lautstärke abgehalten wird nicht aktualisiert (Abstufung auf lokal angehefteten oder umgekehrt). Aktualisieren der ACRs kann Beschädigung der Daten führen. | Bei Bedarf aktualisieren Sie der ACRs vor der Konvertierung Lautstärke, und treffen Sie keine weiteren ACR Updates während die Konvertierung ausgeführt wird. |
| 22 | Updates | Beim Anwenden von 3 aktualisieren die Seite **zum Warten** von in der klassischen Azure-Portal werden im Zusammenhang mit Update 2 folgende Meldung angezeigt: "StorSimple Update 2 8000-Serie bietet die Möglichkeit für Microsoft das Sammeln vorausschauende Protokollinformationen auf Ihrem Gerät, wenn wir potenzielle Probleme erkennen". Dies ist ein falsche Informationen bereitstellen, wie er gibt an, dass das Gerät zu Update 2 erneuert wird. Nachdem das Gerät Succeesfully nach Update 3 aktualisiert wurde, wird diese Meldung nicht mehr angezeigt. | Dieses Verhalten wird in zukünftigen Versionen behoben sein. | Ja | Nein |


## <a name="controller-and-firmware-updates-in-update-3"></a>Controller und Firmware-Updates in Update 3

Diese Version bietet LSI-Treiber und Firmware-Updates. Weitere Informationen zum Installieren der LSI-Treiber und Firmwareupdates finden Sie unter [Installieren von Update 3](storsimple-install-update-3.md) auf Ihrem Gerät StorSimple.

 
## <a name="virtual-device-updates-in-update-3"></a>Virtuelles Gerät Aktualisierungen in Update 3

Dieses Update kann nicht auf die StorSimple Cloud Einheit (auch bekannt als virtuelles Gerät) angewendet werden. Neue virtuelle Geräte müssen erstellt werden. 


## <a name="next-step"></a>Als Nächstes

Erfahren Sie, wie Sie auf Ihrem Gerät StorSimple [3 Update zu installieren](storsimple-install-update-3.md) .
