<properties
   pageTitle="Bereitstellen von StorSimple Gerät Government Portal | Microsoft Azure"
   description="Beschreibt die Schritte und bewährte Methoden für die Bereitstellung der StorSimple Update 2-Gerät und im Portal Government Azure Service."
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
   ms.workload="NA"
   ms.date="08/16/2016"
   ms.author="v-sharos" />

# <a name="deploy-your-on-premises-storsimple-device-in-the-government-portal-update-2"></a>Bereitstellen von Ihrem lokalen StorSimple Gerät im Portal Regierung (Update 2)

[AZURE.INCLUDE [storsimple-version-selector-deploy-gov](../../includes/storsimple-version-selector-deploy-gov.md)]

## <a name="overview"></a>(Übersicht)

Willkommen Sie bei Microsoft Azure StorSimple Gerät Bereitstellung. Wenden Sie diesen Lernprogrammen Bereitstellung StorSimple 8000 Fakultät Update 2 Software im Portal Government Azure ausgeführt. Diese Reihe von Lernprogramme enthält eine Checkliste Konfiguration, eine Liste der Konfiguration erforderliche und detaillierte Konfigurationsschritte für Ihr Gerät StorSimple.

Die Informationen in diesen Lernprogrammen wird davon ausgegangen, dass Sie haben die Sicherheit Vorsichtsmaßnahmen überprüft und entpackt, Verkabeln und Ihrem Gerät StorSimple verbunden. Wenn Sie weiterhin diese Aufgaben erledigen müssen, beginnen Sie mit der [Sicherheit Vorsichtsmaßnahmen](storsimple-safety.md). Folgen Sie den Geräte-spezifischen-Anweisungen entpacken, bereitstellen, oder Verbinden Ihr Gerät an.

- [Entpacken, rack bereitstellen und Ihre 8100 Kabel](storsimple-8100-hardware-installation.md)
- [Entpacken, rack bereitstellen und Ihre 8600 Kabel](storsimple-8600-hardware-installation.md)

Sie benötigen Administratorrechte, um die Installation und Konfiguration abzuschließen. Es empfiehlt sich, dass die Konfiguration Checkliste überprüfen, bevor Sie beginnen. Die Bereitstellung und Konfiguration kann einige Zeit dauern.

> [AZURE.NOTE] Klicken Sie auf der Website Microsoft Azure veröffentlichte Informationen zur Bereitstellung der StorSimple gilt für StorSimple 8000 Reihe Geräte nur. Vollständige Informationen über die Reihe 7000 Geräte, wechseln Sie zu: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). Informationen zur Bereitstellung von 7000 Serie finden Sie im [StorSimple System Schnellstarthandbuch](http://onlinehelp.storsimple.com/111_Appliance/).

## <a name="deployment-steps"></a>Vor der Bereitstellung

Führen Sie die erforderlichen Schritte zum Konfigurieren von Ihrem Geräts StorSimple, und verbinden Sie es mit dem Dienst StorSimple-Manager. Zusätzlich zu den erforderlichen Schritten gibt es optionale Schritte und Verfahren, die Sie möglicherweise während der Bereitstellung ausführen müssen. Die Bereitstellung schrittweise Anweisungen zeigen an, bei der Durchführung der einzelnen Schritte optional sollten.


| Schritt                                                                                   | Beschreibung                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ERFORDERLICHE KOMPONENTEN**                                                                      | Diese müssen in Vorbereitung für die Bereitstellung anstehende abgeschlossen sein.                                                                                        |
|[Checkliste für die Bereitstellung](#deployment-configuration-checklist)                                                     | Verwenden Sie diese Prüfliste zum Sammeln und Aufzeichnen von Informationen vor und während der Bereitstellung.                                                                       |
| [Voraussetzungen für die Bereitstellung](#deployment-prerequsites)                                                               | Diese überprüfen, dass die Umgebung für die Bereitstellung bereit ist.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **SCHRITTWEISE BEREITSTELLUNG**                                                                   | Diese Schritte sind erforderlich, auf Ihrem Gerät StorSimple Herstellung bereitstellen.                                                                                      |
| [Schritt 1: Erstellen eines neuen Dienstes](#step-1-create-a-new-service)                                                         | Cloud-Verwaltung und Speicherplatz für Ihr Gerät StorSimple einrichten. *Überspringen Sie diesen Schritt, wenn Sie einen vorhandenen Dienst für andere Geräte StorSimple haben*.              |
| [Schritt 2: Abrufen der Dienst Registrierungsschlüssel](#step-2-get-the-service-registration-key)                                               | Verwenden Sie diesen Schlüssel zu registrieren, und verbinden Sie Ihr Gerät StorSimple mit der Verwaltungsdienst ein.                                                                         |
| [Schritt 3: Konfigurieren Sie, und melden Sie sich das Gerät über Windows PowerShell für StorSimple](#step 3-configure-and-register-the-device-through-windows-powershell-for-storsimple) | Schließen Sie das Gerät mit Ihrem Netzwerk konfigurieren und registrieren mit Azure, um die Verwendung des Verwaltungsdienst Einrichtung abzuschließen.                                            |
| [Schritt 4: Schließen Sie die geräteeinrichtung minimalen](#step-4-complete-the-minimum-device-setup) </br>Optional: Aktualisieren Sie Ihr Gerät StorSimple. | Verwenden Sie den Verwaltungsdienst Geräte-Einrichtung abzuschließen, und aktivieren sie Speicher bereitzustellen.                                                                      |
| [Schritt 5: Erstellen eines Containers Lautstärke](#step-5-create-a-volume-container)                                                      | Erstellen eines Containers zum Bereitstellen von Datenmengen an. Volumen Container weist Speicher-Konto, Bandbreite und Verschlüsselung Einstellungen für alle darin enthaltenen Datenträger.    |
| [Schritt 6: Erstellen Sie einen Datenträger](#step-6-create-a-volume)                                                               | Bereitstellen von Speicher Datenträger auf dem Gerät StorSimple für Ihre Server.                                                                                        |
| [Schritt 7: Bereitstellen, Initialisierung und formatieren Sie einen Datenträger](#step-7-mount-initialize-and-format-a-volume) </br>Optional: Konfigurieren von MPIO.            | Verbinden Sie Ihre Server mit den iSCSI-Speicher zur Verfügung gestellt, indem Sie das Gerät an. Konfigurieren Sie optional MPIO, um sicherzustellen, dass Ihre Server Link, Netzwerk- und Fehler bei der Benutzeroberfläche tolerieren können.                                                                                                                                                              |
| [Schritt 8: Nehmen Sie eine Sicherung](#step-8-take-a-backup)                                                                  | Richten Sie Ihre Sicherung Richtlinie zum Schutz Ihrer Daten                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **ANDERE VERFAHREN**                                                                   | Möglicherweise müssen Sie auf diese Verfahren verweisen, wie Sie Ihre Lösung bereitstellen.                                                                                    |
| [Konfigurieren eines neuen Speicher-Kontos für den Dienst](#configure-a-new-storage-account-for-the-service)                                      |                                                                                                                                                               |
| [Verwenden von kitten Verbindung zu der seriellen Gerät-Konsole](#use-putty-to-connect-to-the-device-serial-console)                                    |                                                                                                                                                               |
| [Suchen nach und Anwenden von updates](#scan-for-and-apply-updates)                                                   |                                                                                                                                                               |
| [Abrufen der IQN eines Windows Server-Hosts](#get-the-iqn-of-a-windows-server-host)                                                   |                                                                                                                                                               |
| [Erstellen Sie eine manuelle Sicherung](#create-a-manual-backup)                                                                 |
| [Konfigurieren von MPIO](#configure-mpio)                                                                          |

## <a name="deployment-configuration-checklist"></a>Checkliste für die Bereitstellung

Bevor Sie Ihr Gerät StorSimple bereitstellen, müssen Sie zum Sammeln von Informationen zum Konfigurieren der Software auf Ihrem Gerät. Einige dieser Informationen im Voraus vorbereiten hilft bei den Prozess der Bereitstellung des StorSimple Geräts in Ihrer Umgebung zu optimieren. Herunterladen Sie und verwenden Sie diese Prüfliste, um die Konfigurationsdetails Beachten Sie, wie Sie auf Ihrem Gerät bereitstellen.  

[Checkliste für die Bereitstellung StorSimple herunterladen](http://www.microsoft.com/download/details.aspx?id=49159)  

## <a name="deployment-prerequisites"></a>Voraussetzungen für die Bereitstellung

In den folgenden Abschnitten erläutern die Konfiguration erforderlichen Komponenten für Ihren Manager StorSimple-Dienst und Ihrem Gerät StorSimple.

### <a name="for-the-storsimple-manager-service"></a>Für den Dienst StorSimple-Manager

Bevor Sie beginnen, stellen Sie Folgendes sicher:

- Sie müssen mit Anmeldeinformationen für den Zugriff auf Ihr Microsoft-Konto.

- Sie haben Ihre Microsoft Azure-Speicher-Konto mit Anmeldeinformationen für den Zugriff.

- Ihr Microsoft Azure-Abonnement ist für den Dienst StorSimple Manager aktiviert. Ihr Abonnement sollte bis [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/)erworben werden.

- Sie haben Zugriff auf Terminalemulationssoftware wie kitten.

### <a name="for-the-device-in-the-datacenter"></a>Für das Gerät im Datencenter

Bevor Sie das Gerät Folgendes sicher:

- Ihr Gerät vollständig entpackt, bereitgestellt auf einer den Shapes für Gestelle und vollständig für Power, Netzwerk- und seriellen Zugriff verbunden sind, wie in beschrieben:

    -  [Entpacken, rack bereitstellen und Ihrem Gerät 8100 Kabel](storsimple-8100-hardware-installation.md)
    -  [Entpacken, rack bereitstellen und Ihrem Gerät 8600 Kabel](storsimple-8600-hardware-installation.md)


### <a name="for-the-network-in-the-datacenter"></a>Für das Netzwerk im Datencenter

Bevor Sie beginnen, stellen Sie Folgendes sicher:

- Die Ports in der Firewall Datacenter um für iSCSI ermöglichen und den Datenverkehr in die [Anforderungen für Ihr Gerät StorSimple Networking](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)beschriebenen cloud geöffnet.

## <a name="step-by-step-deployment"></a>Schrittweise Bereitstellung

Verwenden Sie die folgenden Schritte auf Ihrem Gerät StorSimple im Datencenter bereitstellen.

## <a name="step-1-create-a-new-service"></a>Schritt 1: Erstellen eines neuen Dienstes

Ein StorSimple-Manager-Dienst kann mehrere StorSimple Geräte verwalten. Führen Sie die folgenden Schritte aus, um eine neue Instanz des Diensts StorSimple Manager zu erstellen.

[AZURE.INCLUDE [storsimple-create-new-service-gov](../../includes/storsimple-create-new-service-gov.md)]

> [AZURE.IMPORTANT] Wenn Sie die automatische Erstellung eines Speicher-Kontos mit Ihrem Dienst nicht aktiviert haben, müssen Sie mindestens ein Speicherkonto zu erstellen, nachdem Sie einen Dienst erfolgreich erstellt haben. Dieses Speicherkonto wird beim Erstellen eines Containers Lautstärke verwendet.
>
> * Wenn Sie ein Speicherkonto nicht automatisch erstellt haben, wechseln Sie ausführliche Anweisungen zum [Konfigurieren eines neuen Speicherkonto für den Dienst](#configure-a-new-storage-account-for-the-service) .
> * Wenn Sie die automatische Erstellung eines Kontos Speicher aktiviert haben, wechseln Sie zu [Schritt2: Abrufen der Dienst Registrierungsschlüssel](#step-2-get-the-service-registration-key).

## <a name="step-2-get-the-service-registration-key"></a>Schritt 2: Abrufen der Dienst Registrierungsschlüssel

Nachdem der StorSimple Manager-Dienst ausgeführt wird, müssen Sie den Dienst Registrierungsschlüssel abrufen. Dieser Schlüssel wird verwendet, um zu registrieren, und verbinden Sie Ihr Gerät StorSimple zum Dienst.

Führen Sie die folgenden Schritte aus, im Portal Government.

[AZURE.INCLUDE [storsimple-get-service-registration-key-gov](../../includes/storsimple-get-service-registration-key-gov.md)]


## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Schritt 3: Konfigurieren Sie, und melden Sie sich das Gerät über Windows PowerShell für StorSimple

Verwenden von Windows PowerShell für StorSimple die erste Einrichtung Ihres Geräts StorSimple ausführen, wie im folgenden Verfahren erläutert. Sie müssen mit Terminalemulationssoftware diesen Schritt abgeschlossen haben. Weitere Informationen finden Sie unter [Verwenden kitten Verbindung zu der seriellen Gerät-Konsole](#use-putty-to-connect-to-the-device-serial-console).

[AZURE.INCLUDE [storsimple-configure-and-register-device-gov](../../includes/storsimple-configure-and-register-device-gov-u2.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Schritt 4: Schließen Sie minimalen Geräte-Einrichtung ab

Für die Gerätekonfiguration der minimalen von Ihrem Gerät StorSimple müssen Sie:

- Einrichten des sekundären DNS-Servers.
- Aktivieren Sie iSCSI auf mindestens ein Netzwerk-Benutzeroberfläche an.
- Sowohl die Controller feste IP-Adressen zuweisen.

Führen Sie die folgenden Schritte aus, im Portal Government zum Abschließen der minimalen Gerät.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>Schritt 5: Erstellen eines Containers Lautstärke

Volumen Container weist Speicher-Konto, Bandbreite und Verschlüsselung Einstellungen für alle darin enthaltenen Datenträger. Sie benötigen einen Container Lautstärke zu erstellen, bevor Sie beginnen können Datenmengen auf Ihrem Gerät StorSimple bereitgestellt.

Führen Sie die folgenden Schritte aus, im Portal Government Erstellen eines Containers Lautstärke.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Schritt 6: Erstellen Sie einen Datenträger

Nach dem Erstellen eines Containers Volume können Sie ein Volume Speicherplatz auf dem Gerät StorSimple für Ihre Server bereitstellen. Führen Sie die folgenden Schritte aus, im Portal Government zum Erstellen eines Datenträgers.

> [AZURE.IMPORTANT] Azure StorSimple kann nur Speicherdefizite Datenmengen erstellen.  Sie können keine erstellt vollständig bereitgestellte oder teilweise bereitgestellte Datenmengen auf einem Azure StorSimple System.

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume-u2.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Schritt 7: Bereitstellen, Initialisierung und formatieren Sie einen Datenträger

Führen Sie diese Schritte aus, auf Ihrem Windows Server-Host.

> [AZURE.IMPORTANT]

> - Für die hohe Verfügbarkeit Ihrer Lösung StorSimple empfehlen wir, dass Sie auf den Hostservern (optional) vor dem Konfigurieren von iSCSI MPIO konfigurieren. MPIO-Konfiguration auf Hostservern wird Stellen Sie sicher, dass die Server ein Link, Netzwerk- oder Benutzeroberflächen-Fehler tolerieren können.

> - Anweisungen MPIO und iSCSI-Installation und Konfiguration von Windows Server-Host wechseln Sie zu [Konfigurieren MPIO für Ihr Gerät StorSimple](storsimple-configure-mpio-windows-server.md). Diese werden ebenfalls sind die Schritte zum Bereitstellen, Initialisierung und StorSimple Datenmengen zu formatieren.

> - Anweisungen MPIO und iSCSI-Installation und Konfiguration auf einem Linux Host wechseln Sie zu [Konfigurieren MPIO für Ihren StorSimple Linux-host](storsimple-configure-mpio-on-linux.md)

Wenn Sie doch nicht MPIO konfigurieren möchten, führen Sie die folgenden Schritte zum Bereitstellen, Initialisierung und formatieren Ihre StorSimple Datenmengen auf einem Windows Server-Host.

[AZURE.INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Schritt 8: Nehmen Sie eine Sicherung

Sicherungskopien bieten Point-in-Time-Schutz der Datenträger und verbessern wiederherstellungsmöglichkeiten und minimiert die Wiederherstellungszeiten. Sie können zwei Arten von Sicherung ausführen, auf Ihrem Gerät StorSimple: lokale Momentaufnahmen und Cloud Momentaufnahmen. Jeder dieser Sicherung Typen kann **geplant** oder **manuell**sein.

Führen Sie die folgenden Schritte im Portal Government, um eine geplante Sicherung zu erstellen.

[AZURE.INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Sie können einen manuellen zu einem beliebigen Zeitpunkt Sicherung ausführen. Verfahren finden Sie unter [erstellen eine manuelle Sicherung](#create-a-manual-backup).

## <a name="configure-a-new-storage-account-for-the-service"></a>Konfigurieren eines neuen Speicher-Kontos für den Dienst

Dies ist ein optionaler Schritt, den Sie müssen nur durchführen, wenn Sie die automatische Erstellung eines Speicher-Kontos mit Ihrem Dienst nicht aktiviert haben. Erstellen eines StorSimple Lautstärke Containers ist ein Microsoft Azure-Speicher-Konto erforderlich.

Wenn Sie ein Konto Azure-Speicher in einem anderen Bereich erstellen müssen, finden Sie unter [Zu Azure Speicherkonten](../storage/storage-create-storage-account.md) eine schrittweise Anleitung.

Führen Sie die folgenden Schritte aus, im Portal Government auf der Seite **StorSimple Manager-Dienst** .

[AZURE.INCLUDE [storsimple-configure-new-storage-account-u1](../../includes/storsimple-configure-new-storage-account-u1.md)]


## <a name="use-putty-to-connect-to-the-device-serial-console"></a>Verwenden von kitten Verbindung zu der seriellen Gerät-Konsole

Für eine Verbindung mit Windows PowerShell für StorSimple, müssen Sie Terminalemulationssoftware wie kitten verwenden. Sie können kitten beim Zugriff auf des Geräts direkt über die serielle Konsole oder indem Sie eine Telnetsitzung von Remotecomputern aus.

[AZURE.INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Suchen nach und Anwenden von updates

Aktualisieren von Ihrem Gerät kann mehrere Stunden dauern. Führen Sie die folgenden Schritte aus, um für Scannen und Anwenden von Updates auf Ihrem Gerät.

<!--If you have a gateway configured on a network interface other than Data 0, you will need to disable Data 2 and Data 3 network interfaces before installing the update. Go to **Devices > Configure** and disable Data 2 and Data 3 interfaces. You should re-enable these interfaces after the device is updated.-->

#### <a name="to-update-your-device"></a>Aktualisieren von Ihrem Gerät

1.  Klicken Sie auf der Seite Geräte für **Schnellstart** auf **Geräte**. Wählen Sie das physische Gerät aus, klicken Sie auf **Wartung** , und klicken Sie dann auf **Updates überprüfen**.  
2.  Ein Auftrag zum Suchen nach verfügbaren Updates wird erstellt. Wenn Updates verfügbar sind, wird die **Updates Scannen** **Updates installieren**. Klicken Sie auf **Updates installieren**.
3.  Ein Aktualisierungsvorgang wird erstellt. Überwachen des Status Ihrer Update durch Navigieren auf **Projekte**an.

    > [AZURE.NOTE] Wenn die Aktualisierung gestartet wird, wird sofort der Status als 50 % angezeigt. Der Status wird auf 100 %, wenn die Aktualisierung abgeschlossen ist. Es gibt keine in Echtzeit Status für den Aktualisierungsvorgang.

4.  Nachdem das Gerät erfolgreich aktualisiert wird, aktivieren Sie Netzwerk-Schnittstellen Daten 2 und 3 von Daten, wenn diese deaktiviert wurden.

## <a name="get-the-iqn-of-a-windows-server-host"></a>Abrufen der IQN eines Windows Server-Hosts

Gehen Sie folgendermaßen vor, um die iSCSI erhalten qualifizierte Namen (IQN) von einem Windows-Host, Windows Server® 2012 ausgeführt wird.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Erstellen Sie eine manuelle Sicherung

Führen Sie die folgenden Schritte aus, im Portal Government zum Erstellen einer bei Bedarf manuellen Sicherung für ein einzelnes Volume auf Ihrem Gerät StorSimple.

[AZURE.INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup-gov.md)]

## <a name="configure-mpio"></a>Konfigurieren von MPIO

Mehreren Pfaden e/a (MPIO) ist ein optionales Feature und ist standardmäßig nicht unter Windows Server installiert. Es sollte als ein Feature über den Server-Manager installiert werden. MPIO Installation Anweisungen finden Sie unter [Konfigurieren von MPIO für Ihr Gerät StorSimple](storsimple-configure-mpio-windows-server.md).

MPIO Installation Anweisungen für ein StorSimple Gerät mit einem Linux-Server verbunden sind finden Sie unter [Konfigurieren von MPIO für Ihre Linux-Server](storsimple-configure-mpio-on-linux.md).

> [AZURE.NOTE] MPIO ist auf einem StorSimple virtuelle Gerät in Azure nicht unterstützt.

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren eines [virtuellen Gerät](storsimple-virtual-device-u2.md)an.

- Verwenden Sie den [StorSimple-Manager-Dienst](https://msdn.microsoft.com/library/azure/dn772396.aspx) auf Ihrem Gerät StorSimple verwalten.
