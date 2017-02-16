<properties 
   pageTitle="StorSimple 8000 Reihe Update 2.2 Versionsinformationen | Microsoft Azure"
   description="Beschreibt die neuen Features, Probleme und problemumgehungen für StorSimple 8000 Reihe Update 2.2."
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
   ms.date="07/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-22-release-notes"></a>StorSimple 8000 Reihe Update 2.2 – Versionsinformationen  

## <a name="overview"></a>(Übersicht)

Die folgenden Versionsinformationen beschreiben der neuen Features und der geöffneten Probleme für StorSimple 8000 Reihe Update 2.2 identifizieren. Sie enthalten auch eine Übersicht über die StorSimple Softwareupdates in dieser Version enthalten. 

Update 2.2 kann auf einem Gerät mit StorSimple Version (GA) oder-Aktualisierung 0,1 bis Update 2.1 ausgeführt angewendet werden. Die Geräteversion 2.2 aktualisieren zugeordnet ist 6.3.9600.17708.

Überprüfen Sie die Informationen in den Versionsinformationen enthalten hat, bevor Sie das Update in Ihre Lösung StorSimple bereitstellen.

>[AZURE.IMPORTANT]
> 
> - Update 2.2 enthält nur Softwareupdates. Es dauert ungefähr 1,5 bis 2 Stunden, dieses Update zu installieren. 

> - Wenn Sie Update 2.1 ausgeführt werden, wird empfohlen, dass Sie so früh wie möglich Update 2.2 anwenden.

> - Neue Versionen Sie möglicherweise nicht finden Sie unter Updates sofort auf, da wir eine geplante Bereitstellung von Updates führen. Warten Sie einige Tage, und klicken Sie dann nach Updates erneut als diese werden erst verfügbar verfügbar.


## <a name="whats-new-in-update-22"></a>Was ist neu in 2.2 aktualisieren

Die folgenden wichtigen Verbesserungen wurden in Update 2.2 vorgenommen.

 
- **Optimierung der Reservierung automatisierte** – Wenn Daten auf Speicherdefizite Datenmengen gelöscht werden, müssen die Blöcke nicht verwendeter Speicher freigegeben werden. Diese Version hat den Speicherplatz freigeben Prozess aus der Cloud in der nicht verwendete Speicherplatz verfügbar schneller im Vergleich zu älteren Versionen resultierender verbessert.


- **Verbesserte-Momentaufnahme Performance** – Update 2.2 hat die Verarbeitungszeit für eine Cloud snapshot in bestimmten Szenarien, wobei große Datenmengen verwendet werden und minimale zu keine Änderung Daten verbessert. Ein Szenario, die von dieser Effekt profitieren würde wäre die Datenmengen archivieren.


- **Sichern von der Support-Paket tragen** – es wurden Verbesserungen in der Methode das Support-Paket gesammelt und in dieser Version hochgeladen wird. 


- **Verbesserte Zuverlässigkeit aktualisieren** – dieser Version sind in alle Updates, die eine verbesserte Zuverlässigkeit der Update führen.

  
 

## <a name="issues-fixed-in-update-22"></a>Probleme in Update 2.2

In den folgenden Tabellen enthält eine Zusammenfassung der Probleme, die Updates 2.2 und 2.1 behoben wurden.    

| Nein | Feature                                    | Problem                                                                                                                                                                                                                                                                                        | Gilt für physische Gerät | Gilt für virtuelles Gerät |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | Host Leistung                      | In der früheren Version wurden Host angeordneten Leistungsprobleme während der Erstellung eines lokal angeheftete Datenträgers und während der Konvertierung eines gestufte Datenträgers auf einen Datenträger lokal angeheftete beobachtet. In dieser Version, wodurch eine Verbesserung in die Leistung Host, während die Lautstärke Erstellung und Konvertierung Verfahren werden diese Probleme behoben.                                                                        | Ja                        | Nein                        |
| 2  | Lokal angeheftete Datenmengen                     | Gelegentlich würde das System stürzt ab beim Erstellen einer lokal angeheftete Lautstärke. In dieser Version wurde dieser Fehler behoben.                                                                                                                                                               | Ja                        | Nein                        |
| 3  | Stufen                                    | Wenn die Metadaten für die StorSimple Cloud Einheiten (8010 und 8020) in der Cloud gestuft wurden gelegentliche stürzt ab. In dieser Version wird dieses Problem behoben.                                                                                                                              | Nein                         | Ja                       |
| 4  | Snapshot-Erstellung                          | Es wurden Probleme zur Entstehung inkrementell Momentaufnahmen in Szenarien mit große Datenmengen Verwandte und minimale zu keine Änderung von Daten. In dieser Version werden diese Probleme behoben.                                                                                                                 | Ja                        | Ja                       |
| 5  | Openstack-Authentifizierung                   | Bei Verwendung von Openstack als Cloud-Dienstanbieter würde der Benutzer in einem seltene Fehler im Zusammenhang mit der Authentifizierung ausführen geführt haben, in dem der JSON-Parser in einem Absturz. Dieser Fehler wird in dieser Version behoben.                                                                                                                              | Ja                        | Nein                        |
| 6  | Host angeordneten kopieren                             | Bei einem seltene Fehler im Zusammenhang mit der Terminierung ODX wurde in früheren Versionen der Software angezeigt, beim Kopieren von Daten von einem Volume auf ein anderes Volume. Dies ergibt ein Controller Failover und das System konnte potenziell in Wiederherstellungsmodus wechseln. Dieser Fehler wird in dieser Version behoben. | Ja                        | Nein       |
| 7  | Windows-Verwaltungsinstrumentation (WMI) | In den vorherigen Versionen der Software, es wurden mehrere Instanzen von Web Proxy-Fehler mit der Ausnahme "<ManagementException> Anbieter Fehler beim Laden des". Dieser Fehler WMI-Speicher freigegeben zugeordnet wurde, und ist nun fest.                                                               | Ja                        | Nein                        |
| 8  | Aktualisieren                                     | In bestimmten Fällen seltenen in den vorherigen Versionen der Software erhalten der Benutzer eine "CisPowershellHcsscripterror" aus, bei dem Versuch, Scannen oder Updates installieren. In dieser Version wird dieses Problem behoben.                                                                                        | Ja                        | Ja                       |
| 9  | Support-Paket                            | In dieser Version wurden Verbesserungen die Möglichkeit, die das Support-Paket zusammengestellt und hochgeladen werden.                                                                                                                                                                                                      | Ja                        | Ja                                    |


## <a name="known-issues-in-update-22"></a>Bekannte Probleme in Update 2.2

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
| 20 |Lokal angeheftete Datenmengen | Wenn Sie versuchen, eine gestufte Volumen (und duplizierten mit Update 1.2 oder früher erstellt) in ein lokales angeheftete Volume umwandeln und Ihrem Gerät nicht mehr Speicherplatz genügend oder Cloud derzeit nicht zur Verfügung, können die Clone(s) beschädigt.| Dieses Problem tritt nur bei Datenmengen, die erstellt wurden und mit duplizierten vor dem Update 2.1 Software wurden. Dies sollte eine seltene Szenario sein.|
| 21 | Volumen-Konvertierung | Die ACRs auf einen Datenträger angefügt werden, während eine Konvertierung Lautstärke abgehalten wird nicht aktualisiert (Abstufung auf lokal angehefteten oder umgekehrt). Aktualisieren der ACRs kann Beschädigung der Daten führen. | Bei Bedarf aktualisieren Sie der ACRs vor der Konvertierung Lautstärke, und treffen Sie keine weiteren ACR Updates während die Konvertierung ausgeführt wird. |

## <a name="controller-and-firmware-updates-in-update-22"></a>Controller und Firmware-Updates in 2.2 aktualisieren

Diese Version enthält nur Software-Updates. Wenn Sie von einer Version vor Update 2 aktualisieren, Sie müssen allerdings um Treiber Storport, Spaceport, installieren und (in einigen Fällen) Datenträger Firmwareupdates auf Ihrem Gerät.
 
Weitere Informationen zum Installieren der Treiber, Storport, Spaceport und Firmwareupdates Datenträger finden Sie unter [Installieren von Updates 2.2](storsimple-install-update-21.md) auf Ihrem Gerät StorSimple.

 
## <a name="virtual-device-updates-in-update-22"></a>Virtuelles Gerät Aktualisierungen in 2.2 aktualisieren

Dieses Update kann nicht auf das virtuelle Gerät angewendet werden. Neue virtuelle Geräte müssen erstellt werden. 

## <a name="next-step"></a>Als Nächstes

Erfahren Sie, wie Sie auf Ihrem Gerät StorSimple [2.2 Update zu installieren](storsimple-install-update-21.md) .
