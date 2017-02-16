<properties
   pageTitle="Installieren Sie Update 1.2 auf Ihrem Gerät StorSimple | Microsoft Azure"
   description="Erläutert, wie Sie StorSimple 8000 Reihe Update 1.2 auf Ihrem StorSimple 8000 Reihe Gerät zu installieren."
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
   ms.date="08/22/2016"
   ms.author="alkohli" />

# <a name="install-update-12-on-your-storsimple-device"></a>Installieren Sie auf Ihrem Gerät StorSimple Update 1.2

## <a name="overview"></a>(Übersicht)

In diesem Lernprogramm wird erläutert, wie Update 1.2 auf einem Gerät StorSimple zu installieren, eine Version vor Update 1 ausgeführt wird. Das Lernprogramm behandelt auch die zusätzlichen Schritte für die Aktualisierung erforderlich, wenn ein Gateway an eine andere Daten 0 des Geräts StorSimple Schnittstelle konfiguriert ist.

Update 1.2 enthält Softwareupdates Gerät, LSI-Treiberupdates und Firmwareupdates Datenträger. Die Software LSI-Treiberupdates ohne Unterbrechung Updates und sind über das klassische Azure-Portal angewendet werden können. Der Datenträger Firmwareupdates Unterbrechung Updates werden und können nur über die Windows PowerShell-Schnittstelle des Geräts angewendet werden.

Je nachdem, welche Version wird auf Ihrem Gerät wird ausgeführt, um festzustellen, ob ein Update 1.2 angewendet werden sollen. Sie können die Softwareversion von Ihrem Gerät durch Navigieren zum Abschnitt **den ersten Blick** Ihres Geräts **Dashboard**überprüfen.

</br>

| Wenn Softwareversion ausführen...   | Was geschieht im Portal?                              |
|---------------------------------|--------------------------------------------------------------|
| Release - GA                    | Wenn Sie die Veröffentlichungsversion (GA) ausgeführt werden, wenden Sie dieses Update nicht. Nehmen Sie [an den Microsoft-Support](storsimple-contact-microsoft-support.md) , um das Gerät zu aktualisieren.|
| Aktualisieren von 0,1 auf.                      | Portal gilt Update 1.2.                                |
| Aktualisieren von 0,2                      | Portal gilt Update 1.2.                                |
| Aktualisieren von 0,3                      | Portal gilt 1.2 aktualisieren.                                |
| Update 1                        | Dieses Update ist nicht verfügbar.                           |
| Aktualisieren von 1.1                      | Dieses Update ist nicht verfügbar.                           |

</br>

> [AZURE.IMPORTANT]

> -  Möglicherweise Update 1.2 nicht angezeigt wird sofort auf, da wir eine geplante Bereitstellung von Updates führen. Suche nach Updates in wenigen Tagen erneut wird als dieses Update bald verfügbar.
> - Dieses Update enthält eine Reihe von zum Ermitteln der der Integrität Gerät im Hinblick auf Hardware Zustand und die Netzwerkkonnektivität manuelle und automatische Pre überprüft. Diese Vorabversion geprüft nur, wenn Sie die Updates aus dem Azure klassischen Portal anwenden.
> - Es empfiehlt sich, dass Sie die Software- und Treiber-Updates über das klassische Azure-Portal installieren. Sie sollten nur der Windows PowerShell-Schnittstelle des Geräts (zum Installieren von Updates) wechseln, wenn das Kontrollkästchen vor der Aktualisierung Gateway im Portal fehlschlägt. Die Updates dauert 5 bis 10 Stunden (einschließlich der Windows-Updates) installieren. Über die Windows PowerShell-Schnittstelle des Geräts müssen die Wartung Modus Updates installiert sein. Wie Wartung Modus Updates Unterbrechung Updates sind, führt dies zu nacheinander nach unten für Ihr Gerät.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-the-azure-classic-portal"></a>Installieren Sie Update 1.2 über das klassische Azure-portal

Führen Sie die folgenden Schritte aus, um Ihr Gerät zu [aktualisieren 1.2](storsimple-update1-release-notes.md)aktualisieren. Verwenden Sie dieses Verfahren nur, wenn Sie einen Gateway an 0 Datenschnittstelle auf Ihrem Gerät konfiguriert haben.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Stellen Sie sicher, dass Ihr Gerät **StorSimple 8000 Reihe Update 1.2 (6.3.9600.17584)**ausgeführt wird. Auch sollte das **Datum der letzten Aktualisierung** geändert werden. Außerdem sehen Sie, dass Wartung Modus Updates verfügbar sind (diese Meldung möglicherweise weiterhin angezeigt, bis zu 24 Stunden, nachdem Sie die Updates installiert haben).

    Wartung Modus Updates werden Unterbrechung Updates, die dazu führen, dass Gerät Ausfallzeiten und können nur über die Windows PowerShell-Benutzeroberfläche von Ihrem Gerät angewendet werden.

    ![Seite zum Warten von] (./media/storsimple-install-update-1/InstallUpdate12_10M.png "Seite zum Warten von")

13. Laden Sie die Wartung Modus Updates mit den Schritten [zum Herunterladen von Updates]( #to-download-hotfixes) aufgeführt suchen und Herunterladen von KB3063416, welche Installationen Datenträger Firmwareupdates (die anderen Updates sollte nun bereits installiert sein).

13. Folgen Sie den Schritten in [Installieren und Wartung Modus Updates überprüfen](#to-install-and-verify-maintenance-mode-hotfixes) die Wartung Modus Updates installieren.

14. Im Portal Azure klassischen navigieren Sie zu der Seite **zum Warten** und am unteren Rand der Seite, klicken Sie auf **Updates überprüfen** von Windows-Updates überprüfen, und klicken Sie auf **Updates installieren**. Sie können den Vorgang abgeschlossen, nachdem alle Updates erfolgreich installiert wurden.



## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>Installieren Sie Update 1.2 auf einem Gerät, das einen Gateway konfiguriert für die Netzwerkschnittstelle 0 nicht-Daten enthält

Nur, wenn Sie das Kontrollkästchen Gateway ein Fehler auftreten, bei dem Versuch, die Updates über das Azure klassischen Portal installieren, sollten Sie dieses Verfahren verwenden. Das Kontrollkästchen schlägt fehl, während Sie einen Gateway zu einer nicht-Daten 0 Netzwerkschnittstelle zugewiesen haben und auf Ihrem Gerät eine Version vor Update 1 ausgeführt wird. Wenn Ihr Gerät kein Gateways auf einer nicht-Daten 0 Netzwerkschnittstelle verfügt, können Sie das Gerät direkt vom Azure klassischen Portal aktualisieren. Finden Sie unter [installieren 1.2 über das klassische Azure-Portal zu aktualisieren](#install-update-1.2-via-the-azure-classic-portal).

Die Softwareversionen, die bei dieser Methode können aktualisiert werden können sind Update 0.1, Update 0,2 und 0,3-Update.


> [AZURE.IMPORTANT]
>
> - Wenn Ihr Gerät Release (GA) Version ausgeführt wird, wenden Sie sich an den [Microsoft-Support](storsimple-contact-microsoft-support.md) , um Unterstützung bei der Aktualisierung können.
> - Dieses Verfahren muss nur einmal durchgeführt werden, um Update 1.2 anzuwenden. Im klassische Azure-Portal können Sie die nachfolgenden Updates beziehen.

Wenn Ihr Gerät vor dem Update 1 Software ausgeführt wird, und es einen Gateway für einen Netzwerkadapter als Daten 0 festgelegt wurde, können Sie auf die folgenden zwei Arten Update 1.2 anwenden:

- **Option 1**: Update herunterladen und Anwenden von es mithilfe der `Start-HcsHotfix` Cmdlet aus der Windows PowerShell-Benutzeroberfläche des Geräts. Dies ist die empfohlene Methode. **Verwenden Sie diese Methode zum Update 1.2 anwenden, wenn Ihr Gerät Update 1.0 oder Update 1.1 ausgeführt wird.**

- **Option 2**: Entfernen der Gatewaykonfiguration, und installieren Sie das Update direkt vom Azure klassischen Portal.


Wenn Sie ausführliche Anweisungen für jedes dieser werden in den folgenden Abschnitten bereitgestellt.

## <a name="option-1-use-windows-powershell-for-storsimple-to-apply-update-12-as-a-hotfix"></a>Option 1: Verwenden von Windows PowerShell für StorSimple Update 1.2 als ein Update anwenden.

Sie sollten dieses Verfahren nur, wenn die Aktualisierung 0.1, 0,2, 0,3 und wenn Ihre Gateway Kontrollkästchen fehlgeschlagen ist, bei dem Versuch, installieren Updates vom klassischen Azure-Portal ausgeführt werden. Wenn Sie Software Release (GA) ausgeführt werden, melden Sie sich an [Microsoft Support Services](storsimple-contact-microsoft-support.md) , um das Gerät zu aktualisieren.

Um ein Update Update 1.2 installiert haben, müssen Sie herunterladen und installieren die folgenden Updates:

| Reihenfolge  | KB        | Beschreibung             | Aktualisieren Sie den Typ  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3063418 | Softwareupdates         |  Normale     |
| 2      | KB3043005 | LSI SAS Controller aktualisieren |  Normale     |
| 3      | KB3063416 | Festplatten-firmware           | Wartung  |

Verwenden dieses Verfahren, um das Update anzuwenden, bevor Sie Folgendes sicher:

- Beide Gerätecontroller sind online.

Führen Sie die folgenden Schritte aus, um Update 1.2 anzuwenden. **Die Updates konnte ungefähr 2 Stunden dauern (etwa 30 Minuten für Software, 30 Minuten für Treiber, 45 Minuten bei Festplatten-Firmware).**

[AZURE.INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]


## <a name="option-2-use-the-azure-classic-portal-to-apply-update-12-after-removing-the-gateway-configuration"></a>Option 2: Verwenden der Azure klassischen-Portals Update 1.2 angewendet werden, nachdem Sie die Gatewaykonfiguration entfernt

Dieses Verfahren gilt nur für StorSimple-Geräten, die eine Version der Software vor dem Update 1 und ein Gateways auf Netzwerk-Schnittstellen als Daten 0 festgelegt haben. Sie müssen die Gateway-Einstellung vor Anwenden der Aktualisierung löschen.

Das Update möglicherweise ein paar Stunden dauern. Wenn Ihre Hosts in unterschiedlichen Subnetzen sind, möglich entfernen die Gateway-Konfiguration auf den iSCSI-Schnittstellen Ausfallzeiten. Es empfiehlt sich, dass Sie Daten 0 für iSCSI-Verkehr zum Verringern der Ausfall konfigurieren.

Führen Sie die folgenden Schritte aus, um die Netzwerk-Benutzeroberfläche mit dem Gateway zu deaktivieren, und wenden Sie das Update.

[AZURE.INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu der [Version 1.2 aktualisieren](storsimple-update1-release-notes.md).
