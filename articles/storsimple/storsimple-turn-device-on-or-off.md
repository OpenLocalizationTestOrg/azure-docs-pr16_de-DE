<properties 
   pageTitle="Aktivieren oder Deaktivieren von Ihrem Gerät StorSimple | Microsoft Azure"
   description="Erläutert, wie Sie ein neues StorSimple Gerät aktivieren, ein Gerät, das wurde beendet wird oder verloren gegangen sind Power aktivieren und Deaktivieren einer laufenden Gerät."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/23/2016"
   ms.author="alkohli" />

# <a name="turn-your-storsimple-device-on-or-off"></a>Aktivieren Sie oder Deaktivieren von Ihrem Gerät StorSimple 

## <a name="overview"></a>(Übersicht)

Beenden eines Microsoft Azure StorSimple Gerät ist nicht als Teil des normalen System Vorgangs erforderlich. Jedoch müssen Sie möglicherweise aktivieren auf ein neues Gerät oder ein Gerät, die aufwiesen beendet werden soll. Im Allgemeinen ist eine war(en) in Fällen erforderlich, in dem müssen fehlerhafte Hardware ersetzen, physisch eine Einheit, übertragen oder ein Gerät nicht verfügbar. In diesem Lernprogramm beschreibt das erforderliche Verfahren einschalten und Ihrem Gerät StorSimple in anderen Szenarien beendet.

In der folgenden Tabelle listet die verschiedenen Szenarios für einschalten und Ihrem Gerät StorSimple beendet und enthält Links zu den entsprechenden Verfahren.

|Szenario|Verweis Themen|
|:-------|:---------------|
|Aktivieren Sie ein neues Gerät|[Aktivieren Sie ein neues Gerät](#turn-on-a-new-device)<ul><li>[Neues Gerät mit nur der primäre Einheit](#new-device-with-primary-enclosure-only)</li><li>[Neues Gerät mit EBOD Einheit](#new-device-with-ebod-enclosure)</li></ul>|
|Aktivieren Sie ein Gerät nach war(en)|[Aktivieren Sie ein Gerät nach war(en)](#turn-on-a-device-after-shutdown)<ul><li>[Mit nur der primäre Einheit Gerät](#device-with-primary-enclosure-only)</li><li>[Gerät mit EBOD Einheit](#device-with-ebod-enclosure)</li></ul>|
|Aktivieren Sie ein Gerät nach einem Verlust power|[Aktivieren Sie ein Gerät nach einem Verlust power](#turn-on-a-device-after-a-power-loss)<ul><li>[Mit nur der primäre Einheit Gerät](#8100)</li><li>[Gerät mit EBOD Einheit](#8600)</li></ul>|
|Aktivieren Sie ein Gerät nach der primären Einheit und EBOD Verbindung geht verloren|[Aktivieren Sie ein Gerät nach der primären und EBOD Einheit Verbindung geht verloren](#turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost)|
|Fahren Sie einen laufenden Gerät|[Deaktivieren einer laufenden Gerät](#turn-off-a-running-device)<ul><li>[Mit nur der primäre Einheit Gerät](#8100a)</li><li>[Gerät mit EBOD Einheit](#8600a)</li></ul>|

## <a name="turn-on-a-new-device"></a>Aktivieren Sie ein neues Gerät

Die Schritte für ein StorSimple Gerät zum ersten Mal einschalten variieren je nachdem, ob das Gerät eine 8100 oder ein Modell 8600. 8100 weist eine einzelne primäre Einheit, während das 8600 ein zwei-Einheit Gerät mit einer primären Einheit und eine Anlage EBOD ist. In den folgenden Abschnitten werden die detaillierten Schritte für beide Modelle behandelt.

- [Neues Gerät mit nur der primäre Einheit](#new-device-with-primary-enclosure-only)

- [Neues Gerät mit EBOD Einheit](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Neues Gerät mit nur der primäre Einheit

Das Modell StorSimple 8100 ist eine einzelne Einheit Gerät. Ihr Gerät enthält redundant Power und Kühlmodule (PCMs). Sowohl PCMs installiert und bei einer Verbindung zu anderen Power Quellen hohen Verfügbarkeit sichergestellt werden muss.

Führen Sie die folgenden Schritte aus, um Ihr Gerät für Power Kabel an.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

>[AZURE.NOTE]Vollständige Einrichtung einrichten und Anweisungen Kabel finden Sie unter [Installieren von Ihrem Gerät StorSimple 8100](storsimple-8100-hardware-installation.md). Stellen Sie sicher, dass Sie die Anweisungen genau folgen.

### <a name="new-device-with-ebod-enclosure"></a>Neues Gerät mit EBOD Einheit

Das Modell StorSimple 8600 hat sowohl eine primäre Einheit und eine Anlage EBOD. Setzt die Einheiten, die für serielle angefügt SCSI (SAS) Konnektivität und Power miteinander verbunden werden.

Wenn dieses Gerät zum ersten Mal einrichten, führen Sie die Schritte für die SAS-Kabel zuerst, und führen Sie die Schritte für das Kabel Power.

[AZURE.INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

>[AZURE.NOTE]Vollständige Einrichtung einrichten und Anweisungen Kabel finden Sie unter [Installieren von Ihrem Gerät StorSimple 8600](storsimple-8600-hardware-installation.md). Stellen Sie sicher, dass Sie die Anweisungen genau folgen.

## <a name="turn-on-a-device-after-shutdown"></a>Aktivieren Sie ein Gerät nach war(en)

Die Schritte für ein Gerät StorSimple einschalten, nachdem es beendet unterscheiden sich je nachdem, ob das Gerät eine 8100 oder ein Modell 8600. 8100 weist eine einzelne primäre Einheit, während das 8600 ein zwei-Einheit Gerät mit einer primären Einheit und eine Anlage EBOD ist.

- [Mit nur der primäre Einheit Gerät](#device-with-primary-enclosure-only)

- [Gerät mit EBOD Einheit](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Mit nur der primäre Einheit Gerät

Gehen Sie nach Beendigung folgendermaßen vor, ein StorSimple Gerät mit einer primären Einheit und keine EBOD Einheit aktivieren.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>So aktivieren Sie ein Gerät mit nur einem primären Einheit

1. Stellen Sie sicher, dass die Potenz auf beide Power wechselt und Kühlmodule (PCMs) werden in der Position aus. Liegen die Schalter nicht in die Position aus, an die Position aus zu kippen Sie, und warten Sie, bis die Leuchten zu wechseln, deaktivieren.

2. Aktivieren Sie auf dem Gerät, indem Sie die Schalter Power kippen, klicken Sie auf beide PCMs in die Stellung. Das Gerät sollte aktivieren.

3. Überprüfen Sie Folgendes ein, um zu überprüfen, dass das Gerät vollständig aktiviert ist:

    1. Die LED OK auf beide PCM Module werden grün angezeigt.

    2. Der Status LED auf beide Controller sind grün.

    3. Klicken Sie auf einen der Controller die blaue LED blinkt, was bedeutet, dass der Controller aktiv ist.

    Wenn Sie eine dieser Bedingungen nicht erfüllt ist, ist Ihr Gerät nicht fehlerfrei. Melden Sie [sich an den Microsoft-Support](storsimple-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Gerät mit EBOD Einheit

Gehen Sie nach Beendigung folgendermaßen vor, ein StorSimple Gerät mit einer primären Einheit und eine Anlage EBOD aktivieren. Führen Sie jeden Schritt nacheinander genau wie beschrieben. Wenn dies nicht der Fall kann Datenverlust führen.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>So aktivieren Sie ein Gerät mit einem primären und eine Anlage EBOD

1. Stellen Sie sicher, dass die Einheit EBOD mit der primären Einheit verbunden ist. Weitere Informationen finden Sie unter [Installieren von Ihrem Gerät StorSimple 8600](storsimple-8600-hardware-installation.md).

2. Stellen Sie sicher, dass die Power und Kühlmodule (PCMs) auf der EBOD und die primäre Anlagen in die Position aus. Liegen die Schalter nicht in die Position aus, an die Position aus zu kippen Sie, und warten Sie, bis die Leuchten zu wechseln, deaktivieren.

3. Aktivieren der EBOD Einheit wechselt zuerst durch die Potenz kippen auf beide PCMs an die Position auf. Die LED PCM sollten grünen. Ein grüner EBOD Controller LED auf dieser Unit zeigt an, dass die Einheit EBOD aktiviert ist.

4. Aktivieren Sie die primäre Einheit durch den Schalter Power kippen, klicken Sie auf beide PCMs in die Stellung. Im gesamte System sollte jetzt auf befinden.

5. Stellen Sie sicher, dass die SAS LED grün, die wird sichergestellt sind, dass die Verbindung zwischen der EBOD Einheit und die primäre Einheit in Ordnung ist.

## <a name="turn-on-a-device-after-a-power-loss"></a>Aktivieren Sie ein Gerät nach einem Verlust power

Ein Gerät StorSimple kann eine Power Ausfall oder Unterbrechung ausschalten. Der Ausfall Power kann auf eine Power Materialien oder beide Power benötigtes Material erfolgen. Die Schritte zur Wiederherstellung unterscheiden sich je nachdem, ob das Gerät ein 8100 oder ein 8600-Modell. 8100 weist eine einzelne primäre Einheit, während das 8600 ein zwei-Einheit Gerät mit einer primären Einheit und eine Anlage EBOD ist. In diesem Abschnitt werden des Wiederherstellungsvorgangs für jedes Szenario.

- [Mit nur der primäre Einheit Gerät](#8100)

- [Gerät mit EBOD Einheit](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Mit nur der primäre Einheit Gerät<a name="8100">

Das System kann den normalen Betrieb fortsetzen, wenn Power Verlust auf einen der zugehörigen Power benötigtes Material vorhanden ist. Jedoch zur Sicherstellung der hohen Verfügbarkeit des Geräts wiederherstellen Sie Power an die an der Power so früh wie möglich.

Wenn eine Power Ausfall oder Power Unterbrechung auf beide Power benötigtes Material vorhanden ist, wird das System auf eine Weise klar strukturiertes und gesteuert fahren. Wenn die Potenz wiederhergestellt wird, wird das System automatisch aktivieren.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Gerät mit EBOD Einheit<a name="8600">

#### <a name="power-loss-on-one-power-supply"></a>Angeben von Power Verlust auf einem power

Das System kann seine normalen Betrieb fortsetzen, wenn Power Verlust auf einen der zugehörigen Power benötigtes Material auf die primäre Einheit oder EBOD Einheit vorhanden ist. Jedoch zur Sicherstellung der hohen Verfügbarkeit des Geräts stellen Sie wieder her Power an die an der Power so früh wie möglich.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Power-Verlust auf beide Power benötigtes Material auf Primär und EBOD Anlagen

Wenn Power Ausfall oder ein Power Unterbrechung auf beide Power benötigtes Material vorliegt, EBOD Einheit wird sofort beendet, und die primäre Einheit wird ein klar strukturiertes und gesteuert Weise beendet. Wenn Power wiederhergestellt wird, wird das Gerät automatisch gestartet.

Wenn die Potenz manuell abgeschaltet ist, klicken Sie dann wie folgt vor um Power auf das System wiederherzustellen.

1. Aktivieren Sie die Anlage EBOD.

2. Nachdem die EBOD-Anlage auf befindet, aktivieren Sie die primäre Einheit.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>Power-Verlust auf beide Power benötigtes Material zu EBOD Einheit

Wenn Sie Ihre Kabel eingerichtet haben, müssen Sie sicherstellen, dass der EBOD nie allein in einer separaten PDU verbunden ist. Wenn die EBOD und die primäre Einheit zur gleichen Zeit fehlschlägt, wird das System wiederherstellen.

Wenn nur die Einheit EBOD auf beide Power benötigtes Material fehlschlägt, wird das System nicht automatisch wiederhergestellt werden. Führen Sie die folgenden Schritte aus, aktivieren Sie das System und an einem ordnungsgemäßen Zustand wiederherstellen:

1. Wenn die primäre Einheit aktiviert ist, deaktivieren Sie sowohl Power und Kühlmodule (PCMs).

2. Warten Sie einige Minuten, damit das System beendet.

3. Aktivieren Sie die Anlage EBOD.

4. Nachdem die EBOD-Anlage auf befindet, aktivieren Sie die primäre Einheit.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Aktivieren Sie ein Gerät nach der primären und EBOD Einheit Verbindung geht verloren

Wenn die Verbindung zwischen dem standby Controller und dem entsprechenden EBOD Controller verloren geht, weiterhin funktioniert das Gerät. Wenn die Verbindung zwischen dem aktiven System-Controller und dem entsprechenden EBOD Controller verloren geht, Failover erfolgen soll, und das Gerät sollte weiterhin normal funktionieren.

Wenn beide Kabel serielle angefügt SCSI (SAS) entfernt werden, oder die Verbindung zwischen der EBOD Einheit und die primäre Einheit unterbrochen ist, wird das Gerät funktioniert nicht mehr. Zu diesem Zeitpunkt führen Sie die folgenden Schritte aus.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>So aktivieren Sie das Gerät nach Verbindung verloren

1. Auf die Rückseite des Geräts zugreifen.

2. Wenn die SAS-Kabel Verbindung zwischen die EBOD Einheit und die primäre Einheit fehlerhaft ist, werden alle SAS Verantwortlichkeitsbereich LED-Anzeigen auf die Anlage EBOD deaktivieren.

3. Fahren Sie sowohl Power und Kühlmodule (PCMs) auf die EBOD Einheit und die primäre Einheit.

4. Warten Sie, bis alle leuchten auf der Rückseite der beide Einheiten zu deaktivieren.

5. Setzen Sie wieder die SAS-Kabel, und stellen Sie sicher, dass es eine gute Verbindung zwischen die EBOD Einheit und die primäre Einheit ist.

6. Aktivieren Sie die Anlage EBOD ersten durch kippen beide PCM Schalter in der Stellung.

7. Stellen Sie sicher, dass die Einheit EBOD aktiviert ist, indem Sie überprüfen, dass die grüne LED aktiviert ist.

8. Aktivieren Sie die primäre Einheit.

9. Stellen Sie sicher, dass die primäre Einheit aktiviert ist, indem Sie überprüfen, dass die Controller grüne LED aktiviert ist.

10. Stellen Sie sicher, dass die EBOD Einheit Verbindung mit der primären Einheit sinnvoll, ist indem Sie überprüfen, dass die SAS Verantwortlichkeitsbereich LED (vier pro EBOD Controller) werden alle auf.

>[AZURE.IMPORTANT] Wenn die SAS-Kabel defekte sind oder die Verbindung zwischen der EBOD Einheit und die primäre Einheit nicht gut beim Einschalten des Systems ist, wird es in der Wiederherstellungsmodus wechseln. Melden Sie [sich an den Microsoft-Support](storsimple-contact-microsoft-support.md) in diesem Fall.

## <a name="turn-off-a-running-device"></a>Deaktivieren einer laufenden Gerät

Eine laufende StorSimple-Gerät ist möglicherweise beendet werden soll, wenn es verschoben wird, außer Betrieb, oder hat eine fehlerhafte Komponente, die ersetzt werden muss. Die Schritte sind unterschiedlich, je nachdem, ob das Gerät StorSimple einer 8100 oder ein Modell 8600. 8100 weist eine einzelne primäre Einheit, während das 8600 ein zwei-Einheit Gerät mit einer primären Einheit und eine Anlage EBOD ist. In diesem Abschnitt werden die Schritte zum Beenden einer laufenden Gerät.

- [Mit der primären Einheit Gerät](#8100a)

- [Gerät mit EBOD Einheit](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Mit der primären Einheit Gerät<a name="8100a"> 

Um das Gerät auf eine Weise klar strukturiertes und gesteuert beenden, können Sie es über das Azure klassischen Portal oder über die Windows PowerShell für StorSimple tun. 

>[AZURE.IMPORTANT] Nicht ausgeschaltet werden einer laufenden Gerät mithilfe der Schaltfläche ' Power ' auf der Rückseite des Geräts.
>
>Stellen Sie bevor Sie das Gerät beenden sicher, dass alle Gerätekomponenten fehlerfrei sind. Navigieren Sie im Azure klassischen-Portal zu **Geräte** > **Wartung** > **Hardware Status**, und stellen Sie sicher, dass der Status aller Komponenten grün ist. Dies gilt nur für ein fehlerfrei System. Nach unten, um das Notebook ausgeschaltet werden ist eine fehlerhafte Komponente ersetzen oder anzeigen, wird ein Fehler beim (Rot) angezeigt (gelb) Status für den entsprechenden Komponenten in den **Status der Hardware**heruntergestuft.

Nachdem Sie auf der Windows PowerShell für StorSimple oder im klassischen Azure-Portal zugreifen, folgen Sie den Schritten [fahren Sie ein Gerät StorSimple](storsimple-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Gerät mit EBOD Einheit<a name="8600a">

>[AZURE.IMPORTANT] Bevor Sie die primäre Einheit und die Einheit EBOD beenden, stellen Sie sicher, dass alle Gerätekomponenten fehlerfrei sind. Navigieren Sie im Azure klassischen-Portal zu **Geräte** > **Wartung** > **Hardware Status**, und stellen Sie sicher, dass alle fehlerfrei sind.

#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>Zum Beenden einer laufenden Gerät mit EBOD Einheit

1. Führen Sie alle Schritte für die primäre Einheit [fahren Sie ein Gerät StorSimple](storsimple-manage-device-controller.md#shut-down-a-storsimple-device) aufgeführt.

2. Nachdem Sie die primäre Einheit beendet wird, fahren den EBOD ein, indem Sie beide auszuschalten kippen und wechselt Kühlung Modul (PCM).

3. Überprüfen Sie um zu überprüfen, dass die EBOD fahren, Leuchten alle auf der Rückseite der Einheit EBOD deaktiviert sind.

>[AZURE.NOTE] Die SAS-Kabel, mit denen die Einheit EBOD Verbindung mit der primären Einheit sollte nicht bis entfernt werden, nachdem das System beendet wird.

## <a name="next-steps"></a>Nächste Schritte

[Microsoft-Support wenden](storsimple-contact-microsoft-support.md) , wenn Sie Probleme beim Einschalten oder ein Gerät StorSimple beendet.

