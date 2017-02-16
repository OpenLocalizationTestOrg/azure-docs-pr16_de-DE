<properties 
   pageTitle="Bereitstellen von StorSimple Gerät Government Portal | Microsoft Azure"
   description="Beschreibt die Schritte und bewährte Methoden für die Bereitstellung der StorSimple Update 1 Gerät und im Portal Government Azure Service."
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
   ms.date="06/17/2016"
   ms.author="v-sharos" />

# <a name="deploy-your-on-premises-storsimple-device-in-the-government-portal"></a>Bereitstellen von Ihrem lokalen StorSimple Gerät im Portal Government

[AZURE.INCLUDE [storsimple-version-selector-deploy-gov](../../includes/storsimple-version-selector-deploy-gov.md)]

## <a name="overview"></a>(Übersicht)

Willkommen Sie bei Microsoft Azure StorSimple Gerät Bereitstellung. Wenden Sie diesen Lernprogrammen Bereitstellung StorSimple 8000 Fakultät Software Update 1 im Portal Government Azure ausgeführt. Diese Reihe von Lernprogramme beschrieben, wie Sie Ihr Gerät StorSimple konfigurieren und einer Checkliste Konfiguration, Konfiguration erforderliche und detaillierte Konfiguration enthält Schritte.

Die Informationen in diesen Lernprogrammen wird davon ausgegangen, dass Sie haben die Sicherheit Vorsichtsmaßnahmen überprüft und entpackt, Verkabeln und Ihrem Gerät StorSimple verbunden. Wenn Sie weiterhin diese Aufgaben erledigen müssen, beginnen Sie mit der [Sicherheit Vorsichtsmaßnahmen](storsimple-safety.md). Je nach Gerätemodell, Sie können dann entpacken, den Shapes für Gestelle bereitstellen und Kabel, indem Sie wie im folgenden beschrieben:

- [Entpacken, rack bereitstellen und Ihre 8100 Kabel](storsimple-8100-hardware-installation.md)
- [Entpacken, rack bereitstellen und Ihre 8600 Kabel](storsimple-8600-hardware-installation.md)

Sie benötigen Administratorrechte, um die Installation und Konfiguration abzuschließen. Es empfiehlt sich, dass die Konfiguration Checkliste überprüfen, bevor Sie beginnen. Die Bereitstellung und Konfiguration kann einige Zeit dauern.

> [AZURE.NOTE] Klicken Sie auf der Website Microsoft Azure veröffentlichte Informationen zur Bereitstellung der StorSimple gilt für StorSimple 8000 Reihe Geräte nur. Vollständige Informationen über die Reihe 7000 Geräte, wechseln Sie zu: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). Informationen zur Bereitstellung von 7000 Serie finden Sie im [StorSimple System Schnellstarthandbuch](http://onlinehelp.storsimple.com/111_Appliance/).

## <a name="deployment-steps"></a>Vor der Bereitstellung

Führen Sie die erforderlichen Schritte zum Konfigurieren von Ihrem Geräts StorSimple, und verbinden Sie es mit dem Dienst StorSimple-Manager. Zusätzlich zu den erforderlichen Schritten gibt es optionale Schritte und Verfahren, die Sie möglicherweise während der Bereitstellung müssen. Die Bereitstellung schrittweise Anweisungen zeigen an, bei der Durchführung der einzelnen Schritte optional sollten.


| Schritt                                                                                   | Beschreibung                                                                                                                                                   |
|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ERFORDERLICHE KOMPONENTEN**                                                                      | Diese müssen in Vorbereitung für die Bereitstellung anstehende abgeschlossen sein.                                                                                        |
| Checkliste für die Bereitstellung.                                                     | Verwenden Sie diese Prüfliste zum Sammeln und Aufzeichnen von Informationen vor und während der Bereitstellung.                                                                       |
| Bereitstellung erforderliche Komponenten.                                                               | Diese überprüfen die Umgebung für die Bereitstellung bereit ist.                                                                                                     |
|                                                                                        |                                                                                                                                                               |
| **SCHRITTWEISE BEREITSTELLUNG**                                                                   | Diese Schritte sind erforderlich, auf Ihrem Gerät StorSimple Herstellung bereitstellen.                                                                                      |
| Schritt 1: Erstellen eines neuen Dienstes.                                                         | Einrichten von Cloud Management und Speicherplatz für Ihr Gerät StorSimple. Überspringen Sie diesen Schritt, wenn Sie einen vorhandenen Dienst für andere Geräte StorSimple haben.                |
| Schritt 2: Abrufen der Dienst-Registrierungsschlüssel.                                               | Verwenden Sie diesen Schlüssel zu registrieren, und verbinden Ihr Gerät StorSimple mit der Verwaltungsdienst ein.                                                                         |
| Schritt 3: Konfigurieren und das Gerät über Windows PowerShell für StorSimple registrieren.    | Schließen Sie das Gerät mit Ihrem Netzwerk konfigurieren und registrieren mit Azure, um die Verwendung des Verwaltungsdienst Einrichtung abzuschließen.                                            |
| Schritt 4: Schließen Sie minimalen Geräte-Einrichtung ab</br>Optional: Aktualisieren Sie Ihr Gerät StorSimple.      | Verwenden Sie den Verwaltungsdienst Geräte-Einrichtung abzuschließen, und aktivieren sie Speicher bereitzustellen.                                                                      |
| Schritt 5: Erstellen eines Containers Lautstärke.                                                      | Erstellen eines Containers zum Bereitstellen von Datenmengen an. Volumen Container weist Speicher-Konto, Bandbreite und Verschlüsselung Einstellungen für alle darin enthaltenen Datenträger.    |
| Schritt 6: Erstellen einer Lautstärke.                                                                | Bereitstellen von Speicher Datenträger auf dem Gerät StorSimple für Ihre Server.                                                                                        |
| Schritt 7: Bereitstellen, Initialisierung und ein Volume formatieren.</br>Optional: Konfigurieren von MPIO.            | Verbinden Sie Ihre Server mit den iSCSI-Speicher zur Verfügung gestellt, indem Sie das Gerät an. Konfigurieren Sie bei Bedarf MPIO, um sicherzustellen, dass Ihre Server Link, Netzwerk- und Fehler bei der Benutzeroberfläche tolerieren können.                                                                                                                                                              |
| Schritt 8: Erstellen Sie eine Sicherungskopie.                                                                  | Richten Sie Ihre Sicherung Richtlinie zum Schutz Ihrer Daten                                                                                                                 |
|                                                                                        |                                                                                                                                                               |
| **ANDERE VERFAHREN**                                                                   | Möglicherweise müssen Sie auf diese Verfahren verweisen, wie Sie Ihre Lösung bereitstellen.                                                                                        |
| Konfigurieren eines neuen Speicher-Kontos für den Dienst an.                                      |                                                                                                                                                               |
| Verwenden Sie kitten Verbindung zu der seriellen Gerät-Konsole.                                    |                                                                                                                                                               |
| Suchen Sie nach Updates und anwenden.                                                   |                                                                                                                                                               |
| Abrufen der IQN eines Windows Server-Hosts an.                                                   |                                                                                                                                                               |
| Erstellen Sie eine manuelle Sicherung.                                                                 | 
| Konfigurieren von MPIO.                                                                          |


## <a name="deployment-configuration-checklist"></a>Checkliste für die Bereitstellung

Die folgende Bereitstellung Konfiguration Checkliste beschreibt die Informationen, die Sie vor, und wie Sie die Software auf Ihrem Gerät StorSimple konfigurieren sammeln müssen. Einige dieser Informationen im Voraus vorbereiten hilft bei den Prozess der Bereitstellung des StorSimple Geräts in Ihrer Umgebung zu optimieren. Verwenden Sie diese Prüfliste auch nach unten die Konfigurationsdetails Beachten Sie, wie Sie auf Ihrem Gerät bereitstellen.

| Phase                                  | Parameter                                         | Details                                                                                                                                                                | Werte |
|----------------------------------------|---------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------|
| **Kabel Ihrem Gerät**                      | Serieller Zugriff                                     | Ursprüngliche Gerätekonfiguration                                                                  | Ja/Nein |
|   |   |  |  |
| **Konfigurieren und registrieren Gerät**          | Daten 0 Netzwerkeinstellungen                           | Daten 0 IP-Adresse:</br>Subnetzmaske:</br>Gateway:</br>Primärer DNS-Server:</br>Primärer NTP-Server:</br>Web-Proxyserver IP/FQDN (optional):</br>Web Proxy-Port:|        |
|                                        | Administratorkennwort Gerät                     | Kennwort muss zwischen 8 und 15 Zeichen, Kleinbuchstaben, Großbuchstaben, numerischen und Sonderzeichen enthält. |        |
|                                        | Kennwort-Manager StorSimple-Snapshot              | Kennwort muss 14 oder 15 Zeichen, Kleinbuchstaben, Großbuchstaben, numerischen und Sonderzeichen enthält.|        |
|                                        | Dienst-Registrierungsschlüssel                          | Dieser Schlüssel wird vom Azure-Portal generiert.    |        |
|                                        | Dienst Datenschlüssel                       | Wenn das Gerät mit der Verwaltungsdienst über die Windows PowerShell für StorSimple registriert ist, wird dieser Schlüssel erstellt. Kopieren Sie diesen Schlüssel, und speichern Sie sie an einem sicheren Ort.|  |
|   |   |  |  |
| **Setup abgeschlossen minimalen Gerät**          | Anzeigenamen für Ihr Gerät                     | Dies ist einen beschreibenden Namen für das Gerät aus. |        |
|                                        | Zeitzone                                          | Ihr Gerät wird dieser Zeitzone für alle geplanten Vorgängen verwendet werden.  |        |
|                                        | Sekundäre DNS-server                              | Dies ist eine erforderliche Konfiguration.                                  |        |
|                                        | Netzwerk-Benutzeroberfläche: Daten 0 Controller feste IP-Adressen                                     | Diese IP Adressen sollte mit dem Internet weitergeleitet werden.</br>Controller 0 geometrisch-IP-Adresse:</br>Controller 1 geometrisch-IP-Adresse:|
|   |   |  |  |
| **Zusätzliche Netzwerkeinstellungen für Benutzeroberflächen**  | Netzwerk-Benutzeroberfläche: 1-Daten</br>Wenn iSCSI aktiviert, nicht Konfigurieren des Gateways.      | Zweck: Cloud/iSCSI/nicht verwendet</br>IP-Adresse:</br>Subnetzmaske:</br>Gateway:|
|                                        | Netzwerk-Benutzeroberfläche: Daten 2</br>Wenn iSCSI aktiviert, nicht Konfigurieren des Gateways.      | Zweck: Cloud/iSCSI/nicht verwendet</br>IP-Adresse:</br>Subnetzmaske:</br>Gateway:|
|                                        | Netzwerk-Benutzeroberfläche: Daten 3</br>Wenn iSCSI aktiviert, nicht Konfigurieren des Gateways.      | Zweck: Cloud/iSCSI/nicht verwendet</br>IP-Adresse:</br>Subnetzmaske:</br>Gateway:|
|                                        | Netzwerk-Benutzeroberfläche: Daten 4</br>Wenn iSCSI aktiviert, nicht Konfigurieren des Gateways.      | Zweck: Cloud/iSCSI/nicht verwendet</br>IP-Adresse:</br>Subnetzmaske:</br>Gateway:|
|                                        | Netzwerk-Benutzeroberfläche: Daten 5</br>Wenn iSCSI aktiviert, nicht Konfigurieren des Gateways.      | Zweck: Cloud/iSCSI/nicht verwendet</br>IP-Adresse:</br>Subnetzmaske:</br>Gateway:|
|   |   |  |  |
| **Erstellen eines Containers Lautstärke**                      | Volumen Containername:                            | Name für den container                                                                                                                                                 |        |
|                                        | Azure-Speicherkonto:                            | Speicher Konto Name und Access-Taste, um diesen Container Lautstärke zuordnen                                                                                              |        |
|                                        | Cloud-Speicher Verschlüsselungsschlüssels:                     | Verschlüsselungsschlüssels für Speicher in den einzelnen Containern                                                                                                                           |        |
|   |   |  |  |
| **Erstellen Sie einen Datenträger**                        | Ausführliche Informationen zu jedem volume                           | Volumename:                                                                                                                                                           |        |
|                                        |                                                   | Größe:                                                                                                                                                                  |        |
|                                        |                                                   | Verwendung Type:                                                                                                                                                            |        |
|                                        |                                                   | Name der ACR:                                                                                                                                                              |        |
|                                        |                                                   | Zusätzliche Standardrichtlinie:                                                                                                                                                 |        |
|   |   |  |  |
| **Bereitstellen, Initialisierung und formatieren Sie einen Datenträger** | Details für jeden Hostserver Herstellen einer Verbindung mit dem Speicher | Windows Server-Name:                                                                                                                                                   |        |
|                                        |                                                   | WindowsServer IQN:                                                                                                                                                    |        |
|                                        |                                                   | Windows Server Lautstärke Name:                                                                                                                                                   |        |
|                                        |                                                   | NTFS bereitstellen Punkt/Laufwerk Buchstaben:                                                                                                                                      |        |


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

[AZURE.INCLUDE [storsimple-configure-and-register-device-gov](../../includes/storsimple-configure-and-register-device-gov.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Schritt 4: Schließen Sie minimalen Geräte-Einrichtung ab

Für die Gerätekonfiguration der minimalen von Ihrem Gerät StorSimple müssen Sie: 

- Einrichten des sekundären DNS-Servers.
- Aktivieren Sie iSCSI auf mindestens ein Netzwerk-Benutzeroberfläche an.
- Sowohl die Controller feste IP-Adressen zuweisen.

Führen Sie die folgenden Schritte aus, im Portal Government zum Abschließen der minimalen Gerät.

[AZURE.INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup-u1.md)]

## <a name="step-5-create-a-volume-container"></a>Schritt 5: Erstellen eines Containers Lautstärke

Volumen Container weist Speicher-Konto, Bandbreite und Verschlüsselung Einstellungen für alle darin enthaltenen Datenträger. Sie benötigen einen Container Lautstärke zu erstellen, bevor Sie beginnen können Datenmengen auf Ihrem Gerät StorSimple bereitgestellt. 

Führen Sie die folgenden Schritte im Portal Government Erstellen eines Containers Lautstärke ein.

[AZURE.INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Schritt 6: Erstellen Sie einen Datenträger

Nach dem Erstellen eines Containers Volume können Sie ein Volume Speicherplatz auf dem Gerät StorSimple für Ihre Server bereitstellen. Führen Sie die folgenden Schritte aus, im Portal Government zum Erstellen eines Datenträgers.

> [AZURE.IMPORTANT] Azure StorSimple kann nur Speicherdefizite Datenmengen erstellen.  Sie können keine erstellt vollständig bereitgestellte oder teilweise bereitgestellte Datenmengen auf einem Azure StorSimple System. 

[AZURE.INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume.md)]

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

Wenn Sie ein Konto Azure-Speicher in einem anderen Bereich erstellen müssen, finden Sie unter [Informationen zu Azure Speicherkonten](../storage/storage-create-storage-account.md) eine schrittweise Anleitung.

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

> [AZURE.NOTE] MPIO ist auf einem StorSimple virtuelle Gerät nicht unterstützt. 

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren eines [virtuellen Gerät](storsimple-virtual-device-u2.md)an.

- Verwenden Sie den [StorSimple-Manager-Dienst](https://msdn.microsoft.com/library/azure/dn772396.aspx) auf Ihrem Gerät StorSimple verwalten.
 
