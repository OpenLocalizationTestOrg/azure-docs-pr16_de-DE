<properties
   pageTitle="Installieren Sie Update 3 auf Ihrem Gerät StorSimple | Microsoft Azure"
   description="Erläutert, wie Sie StorSimple 8000 Reihe Update 3 auf dem mobilen Gerät StorSimple 8000-Serie zu installieren."
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
   ms.date="10/05/2016"
   ms.author="alkohli" />

# <a name="install-update-3-on-your-storsimple-device"></a>Installieren Sie auf Ihrem Gerät StorSimple Update 3

## <a name="overview"></a>(Übersicht)

In diesem Lernprogramm wird erläutert, wie Update 3 auf einem Gerät StorSimple einer früheren Version Software über die klassischen Azure-Portal und mithilfe der Methode Update zu installieren. Die Update-Methode wird verwendet, wenn ein Gateway an eine Schnittstelle als Daten 0 des Geräts StorSimple konfiguriert ist und Sie versuchen, eine Version vor dem Update 1 Software aktualisieren.

3-Update enthält Gerätesoftware, LSI-Treiber und Firmware, Storport und Spaceport aktualisiert. Wenn die Aktualisierung von Update 2 oder einer früheren Version durchführen, auch werden Sie iSCSI, WMI, anwenden und in bestimmten Fällen auf der Festplatte Firmwareupdates erforderlich. Gerätesoftware, WMI, iSCSI, LSI-Treiber, Spaceport und Storport Problembehebungen sind ohne Unterbrechung Updates und kann über das klassische Azure-Portal angewendet werden. Der Datenträger Firmwareupdates Unterbrechung Updates werden und können nur über die Windows PowerShell-Schnittstelle des Geräts angewendet werden. 

> [AZURE.IMPORTANT]

> - Eine Reihe von manuelle und automatische Pre Prüfungen fertig sind, vor der Installation, um die Integrität Gerät im Hinblick auf Hardware Zustand und die Netzwerkkonnektivität zu bestimmen. Diese Vorabversion geprüft nur, wenn Sie die Updates aus dem Azure klassischen Portal anwenden.
> - Es empfiehlt sich, dass Sie die Software- und Treiber-Updates über das klassische Azure-Portal installieren. Sie sollten nur der Windows PowerShell-Schnittstelle des Geräts (zum Installieren von Updates) wechseln, wenn das Kontrollkästchen vor der Aktualisierung Gateway im Portal fehlschlägt. Je nach der Version, der Sie aktualisieren aus, können die Updates installieren 1,5-2,5 Stunden dauern. Über die Windows PowerShell-Schnittstelle des Geräts müssen die Wartung Modus Updates installiert sein. Wie Wartung Modus Updates Unterbrechung Updates sind, führt dies zu nacheinander nach unten für Ihr Gerät.
> - Wenn das optionale StorSimple Snapshot-Manager ausgeführt wird, stellen Sie sicher, dass Sie Ihre Version Snapshot-Manager auf Update 2 aktualisiert haben, vor das Gerät zu aktualisieren.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-3-via-the-azure-classic-portal"></a>Installieren Sie Update 3 über das klassische Azure-portal

Führen Sie die folgenden Schritte aus, um das Gerät zu [Aktualisieren 3](storsimple-update3-release-notes.md)zu aktualisieren.


> [AZURE.NOTE]
Wenn Sie das Update 2 anwenden möchten oder höher (einschließlich Update 2.1), werden Microsoft zusätzliche Diagnoseinformationen vom Gerät abrufen können. Wenn unser Team Vorgänge Geräte, die Probleme auftreten bezeichnet, können wir daher besser ausgestattet, um Informationen vom Gerät zu sammeln und diagnostizieren Probleme. Indem Sie akzeptieren Update 2 oder höher, können Sie uns zur Bereitstellung dieser proaktive Unterstützung.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Stellen Sie sicher, dass Ihr Gerät **StorSimple 8000 Reihe Update 3 (6.3.9600.17759)**ausgeführt wird. Auch sollte das **Datum der letzten Aktualisierung** geändert werden. 

    Wenn Sie von einer Version vor Update 2 aktualisieren, werden Sie auch feststellen, dass die Wartung Modus Updates verfügbar sind (diese Meldung möglicherweise weiterhin angezeigt, bis zu 24 Stunden, nachdem Sie die Updates installiert haben).

    Wartung Modus Updates werden Unterbrechung Updates, die dazu führen, dass Gerät Ausfallzeiten und können nur über die Windows PowerShell-Benutzeroberfläche von Ihrem Gerät angewendet werden. In einigen Fällen beim Ausführen von Update 1.2, Ihrer Festplatte Firmware bereits auf dem neuesten Stand, in diesem Fall möglicherweise Sie keine Wartung Modus Updates installieren müssen.

    Wenn Sie die Aktualisierung von Update 2 oder höher sind, sollten jetzt Ihr Gerät auf dem neuesten Stand sein. Sie können die verbleibenden Schritte überspringen.

13. Laden Sie die Wartung Modus Updates mit den Schritten [zum Herunterladen von Updates](#to-download-hotfixes) aufgeführt suchen und Herunterladen von KB3121899, welche Installationen Datenträger Firmwareupdates (die anderen Updates sollte nun bereits installiert sein).

13. Folgen Sie den Schritten in [Installieren und Wartung Modus Updates überprüfen](#to-install-and-verify-maintenance-mode-hotfixes) die Wartung Modus Updates installieren. 

  

## <a name="install-update-3-as-a-hotfix"></a>Update 3 als ein Update zu installieren.

Verwenden Sie dieses Verfahren, wenn Sie das Kontrollkästchen Gateway ein Fehler auftreten, bei dem Versuch, die Updates über das klassische Azure-Portal zu installieren. Das Kontrollkästchen schlägt fehl, während Sie einen Gateway zu einer nicht-Daten 0 Netzwerkschnittstelle zugewiesen haben und auf Ihrem Gerät eine Version vor Update 1 ausgeführt wird.

Die Softwareversionen, die mithilfe der Methode Update aktualisiert werden können, sind:

- Aktualisieren von 0,1 ab, 0,2 und 0,3
- Aktualisieren von 1, 1.1, 1.2
- Aktualisieren von 2, 2.1, 2.2 

> [AZURE.IMPORTANT]
>
> - Wenn Ihr Gerät Release (GA) Version ausgeführt wird, wenden Sie sich an den [Microsoft-Support](storsimple-contact-microsoft-support.md) , um Unterstützung bei der Aktualisierung können.

Die Update-Methode umfasst die folgenden drei Schritte:

1.  Laden Sie die Updates aus dem Microsoft Update-Katalog aus.

2.  Installieren und normalen Modus Updates überprüfen.

3.  Installieren Sie und überprüfen Sie das Wartung Modus Update (nur, wenn vor dem Update 2 zu aktualisieren).


#### <a name="download-updates-for-your-device"></a>Updates für Ihr Gerät herunterladen

**Wenn Ihr Gerät Update 2.1 oder 2.2 ausgeführt wird**, müssen Sie herunterladen und installieren Sie die folgenden Updates in der vorgegebenen Reihenfolge:

| Reihenfolge  | KB        | Beschreibung                    | Aktualisieren Sie den Typ  | Installieren der Uhrzeit |
|--------|-----------|-------------------------|------------- |-------------|
| 1.      | KB3186843 | Softwareupdates & #42;  |  Normale <br></br>Keine Unterbrechung     | ~ 45 Minuten |
| 2.      | KB3186859 | LSI-Treiber und firmware             |  Normale <br></br>Keine Unterbrechung      | ~ 20 Min. |
| 3.      | KB3121261 | Fix Storport und Spaceport </br> Windows Server 2012 R2 |  Normale <br></br>Keine Unterbrechung      | ~ 20 Min. |

& #42;  *Notiz, Software Update besteht aus zwei binäre Dateien: Gerät Softwareupdates mit vorangestellten `all-hcsmdssoftwareupdate` und der Cis und Mds-Agent mit vorangestellten `all-cismdsagentupdatebundle`. Softwareupdates Gerät muss vor der Cis und Mds-Agent installiert sein. Sie müssen auch über den aktiven Controller neu starten der `Restart-HcsController` Cmdlet nach der Installation des Cis und Mds-Agents aktualisieren (und vor dem Anwenden der verbleibenden aktualisiert).* 


**Wenn Ihr Gerät Update 0.1, 0,2, 0,3, 1.0, 1.1, 1.2, oder 2.0 ausgeführt wird**, müssen Sie herunterladen und installieren die folgenden Updates zusätzlich zu den Software, LSI-Treiber und Firmwareupdates (siehe in der obigen Tabelle), in der vorgegebenen Reihenfolge:

| Reihenfolge  | KB        | Beschreibung                    | Aktualisieren Sie den Typ  | Installieren der Uhrzeit |
|--------|-----------|-------------------------|------------- |-------------|
| 4.      | KB3146621 | iSCSI-Paket | Normale <br></br>Keine Unterbrechung  | ~ 20 Min. |
| 5.      | KB3103616 | WMI-Paket |  Normale <br></br>Keine Unterbrechung      | ~ 12 Min. |


<br></br>

**Wenn Ihr Gerät Versionen 0,2, 0,3, 1.0, 1.1 und 1.2 ausgeführt wird**, müssen Sie eine Installation von Datenträger Firmwareupdates auf alle Updates, die in den vorherigen Tabellen angezeigt. Sie können überprüfen, ob Sie durch Ausführen der Datenträger Firmwareupdates benötigen die `Get-HcsFirmwareVersion` Cmdlet. Wenn Sie diese Firmwareversionen arbeiten: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, und klicken Sie dann Sie nicht benötigen, um diese Updates installieren.


| Reihenfolge  | KB        | Beschreibung                    | Aktualisieren Sie den Typ  | Installieren der Uhrzeit |
|--------|-----------|-------------------------|------------- |-------------|
| 6.      | KB3121899 | Festplatten-firmware              |  Wartung <br></br>Unterbrechung      | ~ 30 Minuten |
 
<br></br>

> [AZURE.IMPORTANT]
>
> - Dieses Verfahren muss nur einmal durchgeführt werden, um Update 3 anzuwenden. Im klassische Azure-Portal können Sie die nachfolgenden Updates beziehen.
> - Aktualisieren von 2,2 aktualisieren, ist die Zeit für insgesamt installieren möglichst ähnlich 1.1 Stunden.
> - Vor dem verwenden dieses Verfahren, um das Update anzuwenden, stellen Sie sicher, dass sowohl die Gerätecontroller online sind und alle Hardware-Komponenten fehlerfrei sind.

Führen Sie die folgenden Schritte aus, zum Herunterladen und Installieren von Updates.

[AZURE.INCLUDE [storsimple-install-update3-hotfix](../../includes/storsimple-install-update3-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den [Update 3 enthalten](storsimple-update3-release-notes.md).
