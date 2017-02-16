<properties
   pageTitle="Installieren Sie Update 2 auf Ihrem Gerät StorSimple | Microsoft Azure"
   description="Erläutert, wie Sie StorSimple 8000 Reihe Update 2 auf Ihrem StorSimple 8000 Reihe Gerät installiert haben."
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

# <a name="install-update-2-on-your-storsimple-device"></a>Installieren Sie auf Ihrem Gerät StorSimple Update 2

## <a name="overview"></a>(Übersicht)

In diesem Lernprogramm wird erläutert, wie auf einer früheren Version Software über die klassischen Azure-Portal StorSimple Geräten Update 2 installiert haben. Das Lernprogramm behandelt auch die Schritte für die Aktualisierung erforderlich, wenn ein Gateway an eine Schnittstelle als Daten 0 des Geräts StorSimple konfiguriert ist und Sie versuchen, eine Version vor dem Update 1 Software aktualisieren.

Update 2 enthält Softwareupdates Gerät, LSI-Treiberupdates und Firmwareupdates Datenträger. Das Gerätesoftware LSI Updates ohne Unterbrechung Updates und sind über das klassische Azure-Portal angewendet werden können. Der Datenträger Firmwareupdates Unterbrechung Updates werden und können nur über die Windows PowerShell-Schnittstelle des Geräts angewendet werden.

> [AZURE.IMPORTANT]

> -  Möglicherweise Update 2 nicht angezeigt wird sofort auf, da wir eine geplante Bereitstellung von Updates führen. Suche nach Updates in wenigen Tagen erneut wird als dieses Update bald verfügbar.
> - Eine Reihe von manuelle und automatische Pre Prüfungen fertig sind, vor der Installation, um die Integrität Gerät im Hinblick auf Hardware Zustand und die Netzwerkkonnektivität zu bestimmen. Diese Vorabversion geprüft nur, wenn Sie die Updates aus dem Azure klassischen Portal anwenden.
> - Es empfiehlt sich, dass Sie die Software- und Treiber-Updates über das klassische Azure-Portal installieren. Sie sollten nur der Windows PowerShell-Schnittstelle des Geräts (zum Installieren von Updates) wechseln, wenn das Kontrollkästchen vor der Aktualisierung Gateway im Portal fehlschlägt. Die Updates können 4 bis 7 (einschließlich der Windows-Updates) installieren Stunden dauern. Über die Windows PowerShell-Schnittstelle des Geräts müssen die Wartung Modus Updates installiert sein. Wie Wartung Modus Updates Unterbrechung Updates sind, führt dies zu nacheinander nach unten für Ihr Gerät.
> - Wenn das optionale StorSimple Snapshot-Manager ausgeführt wird, stellen Sie sicher, dass Sie Ihre Version Snapshot-Manager auf Update 2 aktualisiert haben, vor das Gerät zu aktualisieren.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-2-via-the-azure-classic-portal"></a>Installieren Sie Update 2 über das klassische Azure-portal

Führen Sie die folgenden Schritte aus, um Ihr Gerät zu [Update 2](storsimple-update2-release-notes.md)zu aktualisieren.


> [AZURE.NOTE]
Update 2 mit der Microsoft zusätzliche Diagnoseinformationen vom Gerät zu extrahieren. Wenn unser Team Vorgänge Geräte, die Probleme auftreten bezeichnet, können wir daher besser ausgestattet, um Informationen vom Gerät zu sammeln und diagnostizieren Probleme. Indem Sie akzeptieren Update 2, können Sie uns zur Bereitstellung dieser proaktive Unterstützung.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Stellen Sie sicher, dass Ihr Gerät **StorSimple 8000 Reihe Update 2 (6.3.9600.17673)**ausgeführt wird. Auch sollte das **Datum der letzten Aktualisierung** geändert werden. Außerdem sehen Sie, dass Wartung Modus Updates verfügbar sind (diese Meldung möglicherweise weiterhin angezeigt, bis zu 24 Stunden, nachdem Sie die Updates installiert haben).

    Wartung Modus Updates werden Unterbrechung Updates, die dazu führen, dass Gerät Ausfallzeiten und können nur über die Windows PowerShell-Benutzeroberfläche von Ihrem Gerät angewendet werden. In einigen Fällen beim Ausführen von Update 1.2, Ihrer Festplatte Firmware bereits auf dem neuesten Stand, in diesem Fall möglicherweise Sie keine Wartung Modus Updates installieren müssen.

13. Laden Sie die Wartung-Modus-Updates mit den Schritten [zum Herunterladen von Updates](#to-download-hotfixes) aufgeführt zu suchen und diese herunterladen KB3121899, welche Installationen Datenträger Firmwareupdates (die anderen Updates sollte nun bereits installiert sein).

13. Folgen Sie den Schritten in [Installieren und Wartung Modus Updates überprüfen](#to-install-and-verify-maintenance-mode-hotfixes) die Wartung Modus Updates installieren.


## <a name="install-update-2-as-a-hotfix"></a>Update 2 als ein Update zu installieren.

Verwenden Sie dieses Verfahren, wenn Sie das Kontrollkästchen Gateway ein Fehler auftreten, bei dem Versuch, die Updates über das klassische Azure-Portal zu installieren. Das Kontrollkästchen schlägt fehl, während Sie einen Gateway zu einer nicht-Daten 0 Netzwerkschnittstelle zugewiesen haben und auf Ihrem Gerät eine Version vor Update 1 ausgeführt wird.

Die Softwareversionen, die mithilfe der Methode Update aktualisiert werden können sind Update 0.1, Update 0,2, und aktualisieren 0,3, Update 1, Update 1.1 und 1.2 aktualisieren. Die Update-Methode umfasst die folgenden drei Schritte:

- Laden Sie die Updates aus dem Microsoft Update-Katalog aus.
- Installieren und normalen Modus Updates überprüfen.
- Installieren und das Wartung Modus Update überprüfen.

Um Update 2 als ein Update installiert haben, müssen Sie herunterladen und installieren die folgenden Updates:

| Reihenfolge  | KB        | Beschreibung                    | Aktualisieren Sie den Typ  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3121901 | Softwareupdates         |  Normale     |
| 2      | KB3121900 | LSI-Treiber              |  Normale     |
| 3      | KB3080728 | Storport beheben </br> Windows Server 2012 R2 |  Normale     |
| 4      | KB3090322 | Spaceport beheben </br> Windows Server 2012 R2 |  Normale     |
| 5      | KB3121899 | Festplatten-firmware           | Wartung  |


> [AZURE.IMPORTANT]
>
> - Wenn Ihr Gerät Release (GA) Version ausgeführt wird, wenden Sie sich an den [Microsoft-Support](storsimple-contact-microsoft-support.md) , um Unterstützung bei der Aktualisierung können.
> - Dieses Verfahren muss nur einmal durchgeführt werden, um Update 2 anzuwenden. Im klassische Azure-Portal können Sie die nachfolgenden Updates beziehen.
> - Jeder Update-Installation kann ungefähr 20 Minuten dauern. Total installieren ist beinahe 2 Stunden.
> - Verwenden dieses Verfahren, um das Update anzuwenden, bevor sicherzustellen Sie, dass beide Gerätecontroller online sind und alle Hardware-Komponenten fehlerfrei sind.

Führen Sie die folgenden Schritte aus, um dieses Update als ein Update anzuwenden.

[AZURE.INCLUDE [storsimple-install-update2-hotfix](../../includes/storsimple-install-update2-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]



## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den [Update 2-Version](storsimple-update2-release-notes.md).
