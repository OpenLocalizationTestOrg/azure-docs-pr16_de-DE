<properties
   pageTitle="Aktualisieren von Ihrem Gerät StorSimple | Microsoft Azure"
   description="Erläutert, wie Sie das StorSimple Update-Feature zu verwenden, um normale und Wartung Modus Updates und Updates installieren."
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
   ms.date="06/28/2016"
   ms.author="v-sharos" />

# <a name="update-your-storsimple-8000-series-device"></a>Aktualisieren von Ihrem Gerät StorSimple 8000-Serie

## <a name="overview"></a>(Übersicht)

Die Funktionen für StorSimple Updates können Sie einfach Ihr Gerät StorSimple auf dem neuesten Stand halten. Je nach dem Update können Sie Updates auf das Gerät über das klassische Azure-Portal oder über die Windows PowerShell-Schnittstelle anwenden. In diesem Lernprogramm beschrieben, die Aktualisierungstypen, jede von ihnen zu installieren.

Sie können zwei Arten von Geräteupdates anwenden: 

- Reguläre (oder normalen Modus) Updates
- Wartung Modus updates

Sie können regelmäßige Updates über die Azure klassischen Portal oder Windows PowerShell installieren. jedoch müssen Sie Windows PowerShell verwenden, Wartung Modus Updates installieren. 

Jedes Typs Update ist separat unten beschrieben.

### <a name="regular-updates"></a>Regelmäßige updates

Regelmäßige Updates werden ohne Unterbrechung Updates, die installiert werden können, wenn das Gerät im normalen Modus ist. Diese Updates werden über Microsoft Update-Website auf jedem Gerätecontroller angewendet. 

> [AZURE.IMPORTANT] Ein Controller Failover kann während des Aktualisierungsvorgangs auftreten. Jedoch wirkt nicht System Verfügbarkeit oder Vorgang sich dies.

- Details zum regelmäßige Updates über das klassische Azure-Portal installieren finden Sie unter [reguläre Updates über die Azure klassischen portal(#install-regular-updates-via-the-azure-classic-portal) installieren.

- Sie können auch regelmäßige Updates über Windows PowerShell für StorSimple installieren. Details finden Sie unter [Installieren von regulären Updates über Windows PowerShell für StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).

### <a name="maintenance-mode-updates"></a>Wartung Modus updates

Wartung Modus Updates werden wie Datenträger Firmwareupdates Unterbrechung Updates. Diese Updates erfordern das Gerät Wartungsmodus abgelegt werden. Details finden Sie unter [Schritt2: Geben Sie Wartung Modus](#step2). Sie können das Azure klassische Portal Wartung Modus Updates installieren nutzen. In diesem Fall müssen Sie Windows PowerShell für StorSimple verwenden. 

Details zum Wartung Modus Updates installieren finden Sie unter [Installieren von Wartung Modus Updates über Windows PowerShell für StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [AZURE.IMPORTANT] Wartung Modus Updates müssen separat auf jeder Controller angewendet werden. 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a>Über das klassische Azure-Portal regelmäßige Updates installieren

Im klassische Azure-Portal können Sie Updates auf Ihrem Gerät StorSimple anzuwenden.

[AZURE.INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>Installieren Sie regelmäßig Updates über Windows PowerShell für StorSimple

Alternativ können Sie Windows PowerShell für StorSimple normale (normalen Modus) Updates installieren.

> [AZURE.IMPORTANT] Obwohl Sie regelmäßig mithilfe von Windows PowerShell für StorSimple Updates installieren können, wird dringend empfohlen, dass Sie über das Azure klassischen Portal regelmäßige Updates installieren. Beginnend mit Update 1, werden vor dem Prüfungen vor der Installation von Updates auf dem Portal ausgeführt werden. Diese Vorabversion überprüft werden haben Vorrang vor Fehlern und vergewissern Sie sich eine gleichmäßigere zur Verfügung steht. 

[AZURE.INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Installieren von Wartung Modus Updates über Windows PowerShell für StorSimple

Sie verwenden Windows PowerShell für StorSimple Wartung Modus Updates auf Ihr Gerät StorSimple zuzuweisen. In diesem Modus werden alle e/a-Anfragen angehalten. Dienste wie permanenten Arbeitsspeicher (NVRAM) oder der Cluster-Dienst werden auch beendet. Beide Controller werden neu gestartet, wenn Sie eingeben oder diesen Modus beenden. Wenn Sie in diesem Modus beenden, werden alle Dienste fortgesetzt werden und fehlerfrei sollten. (Dies kann einige Minuten dauern.)

Wenn Sie Wartung Modus Updates beziehen müssen, erhalten Sie eine Benachrichtigung über das Azure klassischen Portal, dass Sie über Updates verfügen, die installiert werden müssen. Diese Warnung wird Anweisungen für die Verwendung von Windows PowerShell für StorSimple, die Updates installieren enthalten. Nachdem Sie Ihr Gerät aktualisieren möchten, verwenden Sie dasselbe Verfahren, das das Gerät in normalen Modus wechseln. Schrittweise Anweisungen finden Sie unter [Schritt 4: Wartung Beenden des Modus](#step4).

> [AZURE.IMPORTANT] 
> 
> - Bevor Sie die Wartungsmodus eingeben, stellen Sie sicher, dass beide Gerätecontroller fehlerfrei sind, indem Sie den **Status der Hardware** auf der Seite **Wartung** in der klassischen Azure-Portal. Wenn der Controller nicht fehlerfrei ist, wenden Sie sich an den Microsoft-Support für den nächsten Schritten fort. Wechseln Sie weitere Informationen zu Microsoft-Support wenden. 
> - Wenn Sie im Modus Wartung sind, müssen Sie das Update zuerst auf einen Controller und klicken Sie dann auf dem anderen Controller anwenden.

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a>Schritt 1: Herstellen einer Verbindung die serielle Konsole mit<a name="step1">

Verwenden Sie zuerst eine Anwendung wie kitten, um die serielle Konsole zuzugreifen. Das folgende Verfahren erläutert, wie Sie die kitten Verbindung zu der seriellen Konsole verwenden.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>Schritt 2: Geben Sie Wartung-Modus<a name="step2">

Nachdem Sie auf die Verwaltungskonsole zugreifen, überprüfen Sie, ob Updates zu installieren, und geben Sie Wartung-Modus, um sie zu installieren.

[AZURE.INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>Schritt 3: Installieren Sie Ihre Aktualisierungen<a name="step3">

Installieren Sie dann Ihre Aktualisierungen an.

[AZURE.INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]
 
### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>Schritt 4: Wartung Beenden des Modus<a name="step4">

Beenden Sie schließlich Wartungsmodus.

[AZURE.INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>Installieren von Updates über Windows PowerShell für StorSimple

Im Gegensatz zu Updates für Microsoft Azure StorSimple sind die Updates aus einem freigegebenen Ordner installiert. Es gibt zwei Arten von Updates, wie bei Updates: 

- Normale Updates 
- Wartung-Modus-Updates  

Die folgenden Verfahren erläutert, wie Windows PowerShell für StorSimple verwenden, um normale und Wartung Modus Updates installieren.

[AZURE.INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[AZURE.INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a>Was geschieht mit Updates, wenn Sie eine Fabrik Zurücksetzen des Geräts durchführen?

Wenn Sie ein Gerät zu Factory Einstellungen zurückgesetzt wird, werden alle Updates verloren. Nachdem das Gerät Factory-zurücksetzen registriert und konfiguriert ist, müssen Sie manuell installieren von Updates über die Azure klassischen Portal und/oder Windows PowerShell für StorSimple. Weitere Informationen zum Factory zurücksetzen, finden Sie unter [Gerät auf Factory-Standardeinstellungen zurücksetzen](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum [Verwenden von Windows PowerShell für StorSimple auf Ihrem Gerät StorSimple verwalten](storsimple-windows-powershell-administration.md).
- Weitere Informationen zum [Verwenden des Diensts StorSimple Manager zum Verwalten von Ihrem Geräts StorSimple](storsimple-manager-service-administration.md).
