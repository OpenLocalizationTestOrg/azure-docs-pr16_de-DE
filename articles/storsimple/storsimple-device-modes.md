<properties 
   pageTitle="Ändern Sie den StorSimple Gerätemodus | Microsoft Azure"
   description="Beschreibt die StorSimple Gerät Modi und erläutert, wie Sie Windows PowerShell für StorSimple verwenden, um den Modus zu ändern."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-device-mode-on-your-storsimple-device"></a>Ändern Sie den Gerätemodus auf Ihrem Gerät StorSimple

Dieser Artikel enthält eine kurze Beschreibung der verschiedenen Modi in denen Ihr Gerät StorSimple verwendet werden kann. Ihr Gerät StorSimple kann Funktion in drei Modi: Normal, Wartung und Wiederherstellung. 

Nach dem Lesen dieses Artikels beherrschen Sie Folgendes:

- Welche die StorSimple Gerät Modi sind
- Wie Sie herausfinden, welche Modus das Gerät StorSimple befindet sich in
- Ändern von normalen Wartungsmodus und *umgekehrt*


Die oben aufgeführten Verwaltungsaufgaben können nur über die Windows PowerShell-Benutzeroberfläche von Ihrem Gerät StorSimple ausgeführt werden.

## <a name="about-storsimple-device-modes"></a>Informationen zum StorSimple Gerät Modi

Ihr Gerät StorSimple kann im Normal, Wartung oder Wiederherstellung Modus ausgeführt werden. Alle drei Modi wird nachfolgend kurz beschrieben.

### <a name="normal-mode"></a>Normalen Modus

Dies wird als den normalen Betrieb Modus für ein vollständig konfigurierten StorSimple Gerät definiert. Ihr Gerät sollten standardmäßig im normalen Modus.

### <a name="maintenance-mode"></a>Wartungsmodus

Manchmal kann das Gerät StorSimple in Wartungsmodus platziert werden müssen. In diesem Modus können Sie Wartung auf dem Gerät ausführen und wie Zusammenhang mit der Datenträger Unterbrechung Updates installieren.

Sie können das System in Wartungsmodus nur über die Windows PowerShell für StorSimple einfügen. In diesem Modus werden alle e/a-Anfragen angehalten. Dienste wie permanenten Arbeitsspeicher (NVRAM) oder der Cluster-Dienst werden auch beendet. Sowohl die Controller werden neu gestartet, wenn Sie diesen Modus beenden, oder geben Sie. Wenn Sie den Wartungsmodus zu beenden, werden alle Dienste fortgesetzt werden und fehlerfrei sollten. Dies kann einige Minuten dauern.

>[AZURE.NOTE]**Wartungsmodus wird nur auf einem Gerät ordnungsgemäß funktionsfähig unterstützt. Es wird nicht auf einem Gerät, in denen eine oder beide der Controller nicht funktionieren, unterstützt.**
</br>

### <a name="recovery-mode"></a>Wiederherstellungsmodus

Wiederherstellungsmodus kann als "Windows Abgesicherter Modus mit Netzwerk-Support" beschrieben werden. Wiederherstellungsmodus aktiviert wird, das Microsoft-Support-Team und ermöglicht es ihnen, die auf dem System Diagnose durchführen. Das primäre Ziel der Wiederherstellungsmodus besteht darin die Systemprotokolle abzurufen.

Wenn auf Ihrem System in Wiederherstellungsmodus wechselt, sollten Sie für die nächsten Schritte Microsoft-Support wenden. Wechseln Sie weitere Informationen zu [Microsoft-Support wenden](storsimple-contact-microsoft-support.md).

>[AZURE.NOTE] **Sie können keine Wiederherstellungsmodus das Gerät versehen. Wenn das Gerät in einem schlechten Zustand ist, versucht Wiederherstellungsmodus können Sie das Gerät in einem Zustand zu gelangen, in dem Microsoft-Support-Mitarbeiter es überprüft werden können.**

## <a name="determine-storsimple-device-mode"></a>Bestimmen StorSimple Gerätemodus

#### <a name="to-determine-the-current-device-mode"></a>Um den aktuellen Gerätemodus zu bestimmen.

1. Melden Sie sich an das Gerät serielle Konsole gemäß die Anweisungen in [Verwenden kitten Verbindung zu der seriellen Gerät-Konsole](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)an.

2. Prüfen Sie die Nachricht Banner serielle Konsole im Menü des Geräts aus. Diese Meldung zeigt explizit an, ob das Gerät im Modus Wartung oder Wiederherstellung ist. Wenn die Nachricht nicht in Verbindung mit den Systemmodus Gruppenrichtlinien bestimmte Informationen enthält, ist das Gerät im normalen Modus.

## <a name="change-the-storsimple-device-mode"></a>Ändern Sie den StorSimple Gerät-Modus 

Sie können das Gerät StorSimple in Wartungsmodus (aus dem normalen Modus) platzieren, zum Ausführen von Wartung oder Wartung Modus Updates installieren. Führen Sie die folgenden Verfahren zum eingeben oder Wartungsmodus beenden.

> [AZURE.IMPORTANT] Vor dem Eingeben der Wartungsmodus, stellen Sie sicher, dass beide Gerätecontroller fehlerfrei sind, indem Sie auf den **Status der Hardware** auf der Seite **Wartung** in der klassischen Azure-Portal zugreifen. Wenn Sie einen oder beide der Controller nicht fehlerfrei sind, wenden Sie sich an den Microsoft-Support für den nächsten Schritten fort. Wechseln Sie weitere Informationen zu [Microsoft-Support wenden](storsimple-contact-microsoft-support.md).

#### <a name="to-enter-maintenance-mode"></a>Wartungsmodus eingeben

1. Melden Sie sich an das Gerät serielle Konsole gemäß die Anweisungen in [Verwenden kitten Verbindung zu der seriellen Gerät-Konsole](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)an.

2. Wählen Sie im Menü seriellen Konsole Option 1, **Melden Sie sich mit Vollzugriff**aus. Wenn Sie dazu aufgefordert werden, geben Sie das **Gerät Administratorkennwort**ein. Das standardmäßige Kennwort lautet: `Password1`.

3. Geben Sie an der Befehlszeile 

    `Enter-HcsMaintenanceMode`

4. Eine Warnung angezeigt, die besagt, dass die Wartungsmodus unterbrochen werden alle e/a-Anfragen und trennt die Verbindung zum klassischen Azure-Portal wird und Sie werden zur Bestätigung aufgefordert wird angezeigt. Geben Sie **Y** Wartungsmodus eingeben.

5. Beide Controller werden neu gestartet. Wenn der Neustart abgeschlossen ist, wird eine Meldung angezeigt, die angibt, dass das Gerät Wartungsmodus ist.


#### <a name="to-exit-maintenance-mode"></a>Zum Beenden des Wartungsmodus

1. Melden Sie sich an die serielle Konsole Gerät an. Vergewissern Sie sich aus der Banner-Nachricht, die Ihrem Gerät Wartungsmodus ist.

2. Geben Sie an der Befehlszeile ein:

    `Exit-HcsMaintenanceMode`

3. Eine Warnung angezeigt und eine bestätigungsmeldung werden angezeigt. Geben Sie **Y** , um die Wartungsmodus zu beenden.

4. Beide Controller werden neu gestartet. Wenn der Neustart abgeschlossen ist, wird eine Meldung angezeigt, die angibt, dass das Gerät im normalen Modus ist.


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie auf Ihrem Gerät StorSimple [Normal und Wartung Modus Updates anwenden](storsimple-update-device.md) .

