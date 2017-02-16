<properties 
   pageTitle="StorSimple virtuelles Gerät Update 2 | Microsoft Azure"
   description="Informationen Sie zum Erstellen, bereitstellen und Verwalten eines StorSimple virtuellen Geräts in einem Microsoft Azure-virtuellen Netzwerk. (Gilt für StorSimple Update 2)."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/23/2016"
   ms.author="alkohli" />

# <a name="deploy-and-manage-a-storsimple-virtual-device-in-azure"></a>Bereitstellen und Verwalten eines StorSimple virtuellen Geräts in Azure


##<a name="overview"></a>(Übersicht)
Das StorSimple 8000 Reihe virtuelle Gerät ist zusätzlichen Möglichkeit, die mit Ihrem Microsoft Azure StorSimple Lösung stammen. StorSimple virtuelle Gerät ausgeführt wird, klicken Sie auf einem virtuellen Computer in einem Microsoft Azure-virtuellen Netzwerk, und sie sichern und Klonen Daten aus Ihrem Hosts verwenden können. In diesem Lernprogramm beschrieben, wie Sie bereitstellen und Verwalten eines virtuellen Azure-Geräts und ist auf alle virtuellen Geräte mit Software, Version 2 Update anwendbar und unteren.


#### <a name="virtual-device-model-comparison"></a>Virtuelles Gerät Modellvergleich

Das StorSimple virtuelle Gerät steht in zwei Modelle, ein standard 8010 (ehemals 1100) und eine Premium 8020 (eingeführt in Update 2). Ein Vergleich der beiden Modelle ist unter tabellarisch angeordnet.


| Gerätemodell          | 8010<sup>1</sup>                                                                     | 8020                                                                                                                               |
|-----------------------|---------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **Maximale Kapazität**      | 30 TB                                                                     | 64 TB                                                                                                                                |
| **Azure-virtuellen Computer**              | Standard_A3 (4 Kernen, 7 GB Arbeitsspeicher)                                                                      | Standard_DS3 (4 Kernen, 14 GB Arbeitsspeicher)                                                                                                                          |
| **Versionskompatibilität** | Versionen, die vorab Update 2 oder höher                                             | Ausführen von Update 2 oder höhere Versionen                                                                                                  |
| **Verfügbarkeit von Region**   | Alle Azure Regionen                                                         | Azure Regionen, die Premium-Speicher unterstützen<br></br>Eine Liste der Gebiete finden Sie unter [unterstützte Regionen für 8020](#supported-regions-for-8020) |
| **Speichertyp**          | Standard Azure-Speicher verwendet für lokale Datenträger<br></br> Informationen zum [Erstellen eines Standard-Speicher-Kontos]() | Azure Premium-Speicher verwendet für lokale Datenträger<sup>2</sup> <br></br>Informationen zum [Erstellen eines Kontos Premium-Speicher](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)                                                               |
| **Arbeitsbelastung Anleitungen**     | Element Ebene Abrufen von Dateien aus Sicherungskopien                                              | Cloud-Entwickler, und Testen Sie Szenarien, Niedrig Wartezeit, höhere Leistung Auslastung <br></br>Sekundäre Gerät für die Wiederherstellung                                                                                            |
 
<sup>1</sup> *früher bekannt als der 1100*.

<sup>2</sup> *sowohl in der 8010 8020 verwenden Standard Azure-Speicher für die Cloud-Leiste. Besteht der Unterschied nur in der lokalen Ebene in das Gerät*.

#### <a name="supported-regions-for-8020"></a>Unterstützte Regionen für 8020

Die Premium Speicher Regionen, die zurzeit für 8020 unterstützt werden, sind unter tabellarisch angeordnet. Diese Liste wird kontinuierlich aktualisiert werden, sobald Premium Speicher in weitere Regionen zur Verfügung stehen. 

| S. Nein.                                                  | Derzeit unterstützt in Regionen. |
|---------------------------------------------------------|--------------------------------|
| 1                                                       | USA – zentral                     |
| 2                                                       |  Ostasiatische US                       |
| 3                                                       |  Ostasiatische USA 2                     |
| 4                                                       | Westen US                        |
| 5                                                       | North Europa                   |
| 6                                                       | Westen Europa                    |
| 7                                                       | Oder Asien                 |
| 8                                                       | Japan OST                     |
| 9                                                       | Japan "Westen"                     |
| 10                                                      | Australien OST                 |
| 11                                                      | Australien oder *           |
| 12                                                      | Ostasien *                     |
| 13                                                      | Süd zentralen US *              |

* Premium Speicher wurde in den folgenden Geos zuletzt gestartet.

In diesem Artikel werden die einzelnen Schritte beim Bereitstellen von einem StorSimple virtuelle Gerät in Azure. Nach dem Lesen dieses Artikels werden Sie folgende Aufgaben ausführen:

- Grundlegendes zu Unterschiede zwischen das virtuelle Gerät das physische Gerät aus.

- Lage, erstellen und Konfigurieren des virtuellen Geräts.

- Verbinden Sie mit dem virtuellen Gerät.

- Informationen Sie zum Arbeiten mit dem virtuelle Gerät.

In diesem Lernprogramm gilt für alle StorSimple virtuelle Geräte Ausführen von Update 2 und höher. 

## <a name="how-the-virtual-device-differs-from-the-physical-device"></a>Wie unterscheidet sich das virtuelle Gerät aus dem physischen Gerät

Das StorSimple virtuelle Gerät ist eine reine Software-Version von StorSimple, die für einen einzelnen Knoten in einer Microsoft Azure virtuellen Computern ausgeführt wird. Das virtuelle Gerät unterstützt Wiederherstellungssituationen, in denen Ihre physische Gerät steht nicht zur Verfügung und eignet sich für die Verwendung in Elementebene Abruf von Sicherungskopien, lokalen Wiederherstellung und Szenarien für die Entwicklung und Testen der Cloud.

#### <a name="differences-from-the-physical-device"></a>Vom Gerät physisch Unterschiede

Die folgende Tabelle zeigt einige wichtigsten Unterschiede zwischen dem StorSimple virtuelle Gerät und das StorSimple physische Gerät.

|                             | Physische Gerät                                          | Virtuelles Gerät                                                                            |
|-----------------------------|----------------------------------------------------------|-------------------------------------------------------------------------------------------|
| **Speicherort**                    | Befindet sich im Datencenter.                               | Azure wird ausgeführt.                                                                            |
| **Netzwerk-Schnittstellen**          | Verfügt über sechs Netzwerkschnittstellen: Daten 0 bis 5 Daten.                  | Nur eine Schnittstelle hat: Daten 0.                                        |
| **Registrierung**                | Während der Konfigurationsschritt erfasst.                | Die Registrierung ist einen eigenen Vorgang an.                                                          |
| **Dienst Daten Verschlüsselungsschlüssels** | Auf dem Gerät physisch neu zu generieren Sie, und aktualisieren Sie das virtuelle Gerät mit dem neuen Schlüssel.           | Kann nicht aus dem virtuelle Gerät neu erstellen. |


## <a name="prerequisites-for-the-virtual-device"></a>Erforderliche Komponenten für das virtuelle Gerät

In den folgenden Abschnitten erläutern die Konfiguration erforderlichen Komponenten für Ihr StorSimple virtuelle Gerät. Überprüfen Sie vor der Bereitstellung von einem virtuellen Gerät aus, die [Sicherheitsaspekte der Verwendung von eines virtuellen Geräts](storsimple-security.md#storsimple-virtual-device-security)aus.

#### <a name="azure-requirements"></a>Azure Anforderungen

Bevor Sie das virtuelle Gerät bereitstellen, müssen Sie die folgenden vorbereitenden in Ihrer Umgebung Azure:

- Für das virtuelle Gerät, das [Konfigurieren eines virtuellen Netzwerks auf Azure](../virtual-network/virtual-networks-create-vnet-classic-portal.md). Wenn Premium Speicher verwenden zu können, müssen Sie ein virtuelles Netzwerk in einem Azure Region erstellen, die die Premium-Speicher unterstützt. Weitere Informationen zu [Regionen, die derzeit für 8020 unterstützt werden](#supported-regions-for-8020).
- Es wird empfohlen, verwenden Sie den DNS-Standardserver von Azure bereitgestellt werden, anstatt einen eigenen DNS-Server anzugeben. Wenn Ihre DNS-Servername ungültig ist oder der DNS-Server nicht ordnungsgemäß IP-Adressen auflösen kann, wird die Erstellung des virtuellen Geräts fehl.
- Punkt-zu-Standort und zwischen Standorten sind optional, aber nicht erforderlich. Wenn Sie möchten, können Sie diese Optionen für komplexere Szenarios konfigurieren. 
- Sie können [Azure-virtuellen Computern](../virtual-machines/virtual-machines-linux-about.md) (Host Servers) in das virtuelle Netzwerk erstellen, mit denen die Datenmengen, die virtuelle Gerät bereitgestellt werden können. Diese Server müssen die folgenden Anforderungen erfüllt:                            
    - Windows oder Linux virtuellen Computern mit iSCSI-Initiatorsoftware installiert sein können.
    - Im gleichen virtuellen Netzwerk als virtuelles Gerät ausgeführt werden.
    - Verbindung mit den iSCSI-Ziel des virtuellen Geräts durch die interne IP-Adresse des virtuellen Geräts hergestellt werden.

- Stellen Sie sicher, dass Sie Unterstützung für iSCSI und Cloud Verkehr auf demselben virtuellen Netzwerk konfiguriert haben.


#### <a name="storsimple-requirements"></a>StorSimple Anforderungen

Nehmen Sie die folgenden Updates zu Ihrem Dienst Azure StorSimple vor dem Erstellen von eines virtuellen Geräts:


- Hinzufügen von [Einträgen, Access-Steuerelement](storsimple-manage-acrs.md) für den virtuellen Computern, die Host-Servern für Ihr Gerät virtuelle werden sollen.

- Verwenden Sie ein [Speicherkonto](storsimple-manage-storage-accounts.md#add-a-storage-account) in derselben Region als virtuelles Gerät. Leistung Speicherkonten in unterschiedlichen Regionen möglicherweise beeinträchtigt werden. Sie können ein Konto Standard oder Premium Speicher mit virtuelles Gerät verwenden. Weitere Informationen zum Erstellen eines [Standard-Speicher-Konto]() oder einem [Premium Speicher-Konto](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)

- Verwenden Sie für virtuelles Gerät Erstellung von verwendet für Ihre Daten einer anderen Speicherkonto. Verwendung des Kontos Speicher möglicherweise beeinträchtigt Leistung.

Stellen Sie sicher, dass Sie die folgende Informationen verfügen, bevor Sie beginnen:

- Ihr Azure klassischen Portalseite Konto mit Anmeldeinformationen für den Zugriff.

- Eine Kopie des Verschlüsselungsschlüssels Dienst Daten auf Ihrem Gerät physisch.


## <a name="create-and-configure-the-virtual-device"></a>Erstellen und Konfigurieren des virtuellen Geräts

Stellen Sie bevor Sie diese Verfahren ausführen sicher, dass Sie die [erforderlichen Komponenten für das virtuelle Gerät](#prerequisites-for-the-virtual-device)erfüllt haben. 

Nachdem ein virtuelles Netzwerk erstellt, einen StorSimple Manager-Dienst konfiguriert und Ihre physische StorSimple Gerät Dienst registriert haben, können Sie die folgenden Schritte zum Erstellen und Konfigurieren eines StorSimple virtuellen Geräts verwenden. 

### <a name="step-1-create-a-virtual-device"></a>Schritt 1: Erstellen eines virtuellen Geräts

Führen Sie die folgenden Schritte aus, um die StorSimple virtuelles Gerät zu erstellen.

[AZURE.INCLUDE [Create a virtual device](../../includes/storsimple-create-virtual-device-u2.md)]

Wenn die Erstellung des virtuellen Geräts in diesem Schritt fehlschlägt, verfügen Sie möglicherweise nicht mit dem Internet Connectivity. Weitere Informationen finden Sie unter [Behandeln von Problemen mit Internet-Konnektivitätsfehlern](#troubleshoot-internet-connectivity-errors) beim Creatig ein virtuelles Gerät.


### <a name="step-2-configure-and-register-the-virtual-device"></a>Schritt 2: Konfigurieren Sie und registrieren Sie virtuelle Gerät

Stellen Sie bevor Sie beginnen sicher, dass Sie eine Kopie des Verschlüsselungsschlüssels Dienst Daten besitzen. Der Dienst Datenschlüssel erstellt wurde, wenn Sie Ihre erste StorSimple Gerät so konfiguriert ist, und Sie angewiesen wurden, ihn an einem sicheren Ort zu speichern. Wenn Sie nicht über eine Kopie des Verschlüsselungsschlüssels Dienst Daten verfügen, müssen Sie für die Unterstützung von Microsoft Support wenden.

Gehen Sie folgendermaßen zum Konfigurieren und Ihre StorSimple virtuelle Gerät zu registrieren.
[AZURE.INCLUDE [Configure and register a virtual device](../../includes/storsimple-configure-register-virtual-device.md)]

### <a name="step-3-optional-modify-the-device-configuration-settings"></a>Schritt 3: (Optional) ändern die Konfiguration des Audiogeräts

Im folgende Abschnitt beschreibt die Konfiguration des Audiogeräts erforderlich für die StorSimple virtuelles Gerät verwenden soll CHAP, StorSimple Snapshot-Manager oder das Gerät Administratorkennwort ändern.

#### <a name="configure-the-chap-initiator"></a>Konfigurieren des Initiators CHAP

Für diesen Parameter enthält die Anmeldeinformationen, die Ihre virtuelle Gerät (Ziel) aus der Initiatoren (Server) erwartet, die die Datenmengen zugreifen möchten. Die Initiatoren bietet einen CHAP-Benutzernamen und ein Kennwort CHAP, sich mit Ihrem Gerät während diese Authentifizierung zu identifizieren. Die detaillierten Schritte finden Sie unter [Konfigurieren von CHAP für Ihr Gerät](storsimple-configure-chap.md#unidirectional-or-one-way-authentication).

#### <a name="configure-the-chap-target"></a>Konfigurieren des CHAP-Ziels

Für diesen Parameter enthält die Anmeldeinformationen, die Ihre virtuelle Gerät verwendet, wenn ein Initiator CHAP aktiviert gegenseitig oder bidirektionale Authentifizierung anfordert. Virtuelle mithilfe Ihres Geräts wird eine umgekehrte CHAP-Benutzernamen und das Kennwort umgekehrte CHAP um sich an den Initiator während dieser Authentifizierung zu identifizieren. Beachten Sie, dass CHAP Target Globale Standardeinstellungen sind. Wenn diese angewendet werden, werden alle Datenträger, die mit dem virtuelle Speichergerät verbunden CHAP-Authentifizierung verwenden. Die detaillierten Schritte finden Sie unter [Konfigurieren von CHAP für Ihr Gerät](storsimple-configure-chap.md#bidirectional-or-mutual-authentication).

#### <a name="configure-the-storsimple-snapshot-manager-password"></a>Legen Sie das Kennwort StorSimple Snapshot-Manager

StorSimple Snapshot-Manager-Software befindet sich auf Ihrem Windows-Server und können Administratoren verwalten Ihres Geräts StorSimple in Form eines lokalen Sicherungen und Momentaufnahmen cloud.

>[AZURE.NOTE] Für das virtuelle Gerät wird Ihr Windows-Host eine Azure-virtuellen Computern.

Wenn Sie ein Gerät StorSimple Snapshot-Manager konfigurieren, werden Sie aufgefordert, den StorSimple Gerät IP-Adresse und das Kennwort, das Speichergerät authentifizieren bereitzustellen. Die detaillierten Schritte finden Sie unter [Konfigurieren StorSimple Snapshot-Manager Kennwort](storsimple-change-passwords.md#change-the-storsimple-snapshot-manager-password).

#### <a name="change-the-device-administrator-password"></a>Ändern Sie das Kennwort des Administrators 

Wenn Sie die Windows PowerShell-Benutzeroberfläche zum Zugreifen auf des virtuellen Geräts verwenden, müssen Sie ein Gerät Administratorkennwort einzugeben. Für die Sicherheit Ihrer Daten müssen Sie dieses Kennwort ändern, bevor das virtuelle Gerät verwendet werden kann. Die detaillierten Schritte finden Sie unter [Konfigurieren Gerät Administratorkennwort](storsimple-change-passwords.md#change-the-device-administrator-password).

## <a name="connect-remotely-to-the-virtual-device"></a>Herstellen einer Verbindung virtuelles Gerät mit Remote
Remotezugriff auf Ihrem virtuellen Gerät über die Windows PowerShell-Schnittstelle ist nicht standardmäßig aktiviert. Sie müssen zuerst Aktivieren der remote-Verwaltung auf dem Gerät virtuelle, und aktivieren sie dann auf dem Client, der Zugriff auf Ihre virtuellen Gerät verwendet wird.

Der Verbindung Remote zweistufigen Prozess wird unter detailliert beschrieben.

### <a name="step-1-configure-remote-management"></a>Schritt 1: Konfigurieren von remote-Verwaltung

Führen Sie die folgenden Schritte aus, um remote-Management für Ihre StorSimple virtuelles Gerät zu konfigurieren.

[AZURE.INCLUDE [Configure remote management via HTTP for virtual device](../../includes/storsimple-configure-remote-management-http-device.md)]

### <a name="step-2-remotely-access-the-virtual-device"></a>Schritt 2: Remotezugriff auf das virtuelle Gerät

Nachdem Sie remote-Verwaltung auf der Seite StorSimple Gerät Konfiguration aktiviert haben, können Sie Windows PowerShell Remote aus einem anderen virtuellen Computern innerhalb der gleichen virtuellen Netzwerk eine Verbindung zu dem virtuellen Gerät verwenden; Beispielsweise können Sie vom Host virtueller Computer verbinden, die Sie konfiguriert und iSCSI Verbindung verwendet wird. In den meisten Installationen wird bereits geöffnet einen öffentlichen Endpunkt Ihres Hosts virtueller Computer zugreifen, die Sie für den Zugriff auf das virtuelle Gerät verwenden können.

>[AZURE.WARNING] **Zur Verbesserung der Sicherheit wird dringend empfohlen, dass Sie an die Endpunkte Verbinden mit HTTPS verwenden, und löschen Sie die Endpunkte, nachdem Sie Ihre remote PowerShell-Sitzung beendet haben.**

Befolgen Sie die Verfahren in der [Herstellen einer Verbindung mit Ihrem Gerät StorSimple Remote](storsimple-remote-connect.md) Remote für Ihr Gerät virtuelle einrichten.

## <a name="connect-directly-to-the-virtual-device"></a>Schließen Sie direkt an das virtuelle Gerät

Sie können auch direkt auf das virtuelle Gerät verbinden. Wenn Sie direkt auf das virtuelle Gerät von einem anderen Computer außerhalb des virtuellen Netzwerks oder außerhalb der Microsoft Azure-Umgebung verbinden möchten, müssen Sie zusätzliche Endpunkte erstellen, wie im folgenden Verfahren beschrieben. 

Führen Sie die folgenden Schritte aus, um einen öffentlichen Endpunkt auf dem virtuelle Gerät zu erstellen.

[AZURE.INCLUDE [Create public endpoints on a virtual device](../../includes/storsimple-create-public-endpoints-virtual-device.md)]

Es empfiehlt sich, dass Sie von einem anderen virtuellen Computern innerhalb der gleichen virtuellen Netzwerk verbinden, da dieser praktischen die Anzahl der öffentlichen Endpunkte in Ihrem Netzwerk virtuelle minimiert. Wenn Sie diese Methode verwenden, werden Sie einfach Herstellen einer Verbindung des virtuellen Computers über eine Sitzung Remote Desktop mit und dann konfigurieren Sie diesen virtuellen Computern für die Verwendung wie andere Windows-Client auf einem lokalen Netzwerk. Sie müssen nicht die öffentlichen Port-Nummer angefügt werden, weil der Port bereits bekannt ist.

## <a name="work-with-the-storsimple-virtual-device"></a>Arbeiten Sie mit dem StorSimple virtuelle Gerät

Jetzt, da Sie erstellt und das StorSimple virtuelle Gerät konfiguriert haben, sind Sie bereit sind, damit arbeiten kann. Sie können mit Lautstärke Container, Datenmengen und zusätzliche Richtlinien auf einem virtuellen Gerät arbeiten wie auf einem physischen StorSimple Gerät; der einzige Unterschied ist, dass Sie benötigen, um sicherzustellen, dass Sie das Gerät virtuelle Geräte in der Liste auswählen. [Verwenden Sie den Dienst StorSimple-Manager, um ein virtuelles Gerät zu verwalten](storsimple-manager-service-administration.md) , eine schrittweise Anleitung für die verschiedenen Verwaltungsaufgaben für die virtuelle Gerät verweisen.

Den folgenden Abschnitten werden einige der Unterschiede bei der Arbeit mit dem Gerät virtuelle auftretenden.

### <a name="maintain-a-storsimple-virtual-device"></a>Verwalten von einem virtuellen StorSimple-Gerät

Da es sich um eine reine Software-Gerät ist, ist Wartung für das virtuelle Gerät minimalen im Vergleich zu Wartung für das physische Gerät. Sie haben die folgenden Optionen aus:

- **Softwareupdates** – Sie können das Datum der letzten Aktualisierung die Software, in Kombination mit aktualisieren Statusnachrichten. Sie können die Schaltfläche **Updates überprüfen** am unteren Rand der Seite verwenden, um einen manuellen Scan ausführen, wenn Sie nach neuen Updates suchen möchten. Wenn Updates verfügbar sind, klicken Sie auf **Updates installieren** , zu installieren. Da nur eine einzige Oberfläche auf das virtuelle Gerät vorhanden ist, bedeutet dies, dass es wird eine geringe dienstunterbrechung sein, wenn Updates angewendet werden. Das virtuelle Gerät wird beenden und neu starten (falls erforderlich), um alle Updates anzuwenden, die freigegeben wurden. Für eine schrittweise Anleitung wechseln Sie zu [Ihrem Gerät zu aktualisieren](storsimple-update-device.md#install-regular-updates-via-the-azure-classic-portal).
- **Unterstützung für Paket** – Sie können erstellen und Hochladen eines Pakets Support zum Behandeln von Problemen mit Ihrem Gerät virtuelle Microsoft-Support-Hilfe. Eine schrittweise Anleitung finden Sie unter [Erstellen und verwalten ein Support-Pakets](storsimple-create-manage-support-package.md).

### <a name="storage-accounts-for-a-virtual-device"></a>Speicherkonten für ein virtuelles Gerät

Speicherkonten werden für die Verwendung der StorSimple Manager-Dienst, der virtuelle Gerät und das physische Gerät erstellt. Wenn Sie Ihre Speicherkonten erstellen, wird empfohlen, dass Sie in den Anzeigenamen einen Regionsbezeichner verwenden, um sicherzustellen, dass der Bereich innerhalb aller Systemkomponenten konsistent ist. Für ein virtuelles Gerät ist es wichtig, dass alle Komponenten in der gleichen Region, um Leistungsprobleme zu vermeiden.

Eine schrittweise Anleitung finden Sie unter [Hinzufügen eines Kontos Speicher](storsimple-manage-storage-accounts.md#add-a-storage-account).

### <a name="deactivate-a-storsimple-virtual-device"></a>Deaktivieren eines virtuellen StorSimple-Geräts

Deaktivieren von einem virtuellen Gerät löscht den virtuellen Computer und den Ressourcen erstellt, wenn sie nach der Bereitstellung wurde. Nachdem das virtuelle Gerät deaktiviert ist, können sie den ursprünglichen Zustand wiederhergestellt werden. Bevor Sie das virtuelle Gerät deaktiviert haben, stellen Sie sicher, beenden oder Löschen von Clients und Hosts, die sie abhängig sind.

Deaktivieren eines virtuellen Geräts angegeben, ergibt sich die folgenden Aktionen aus:

- Das virtuelle Gerät wird entfernt.

- Der Datenträger OS und Daten Datenträger für das virtuelle Gerät erstellt werden entfernt.

- Die gehosteten Dienst und virtuelle Netzwerk während der Vorbereitung erstellten aufbewahrt werden. Wenn Sie diese nicht verwenden, sollten Sie diese manuell löschen.

- Cloud Momentaufnahmen für das virtuelle Gerät erstellt werden beibehalten.

Eine schrittweise Anleitung, wechseln Sie zu [deaktivieren und Löschen von Ihrem Gerät StorSimple](storsimple-deactivate-and-delete-device.md).

Sobald das virtuelle Gerät als deaktiviert auf der Seite StorSimple-Manager angezeigt wird, können Sie das virtuelle Gerät aus Geräte in der Liste auf der Seite **Geräte** löschen.


### <a name="start-stop-and-restart-a-virtual-device"></a>Starten, beenden und starten Sie ein virtuelles Gerät neu
Im Gegensatz zu StorSimple physischen Geräts ist keine Power auf oder eine Schaltfläche auf einem StorSimple virtuelle Gerät Pushbenachrichtigungen ausschalten. Jedoch möglicherweise Anlässe, wo Sie beenden, und starten das Gerät virtuelle müssen. Beispielsweise möglicherweise einige Updates erfordern, dass der virtuellen Computer neu gestartet werden, um den Aktualisierungsvorgang abzuschließen. Die einfachste Möglichkeit, starten, beenden und Neustart ein virtuelles Gerät besteht darin, die virtuellen Computern-Verwaltungskonsole verwenden.

Wenn Sie die Verwaltungskonsole betrachten, ist der Status virtuelles Gerät auch **ausgeführt** , da es standardmäßig gestartet wird, nachdem sie erstellt wurde. Sie können beginnen, beenden und neu starten ein virtuellen Computers zu einem beliebigen Zeitpunkt.

[AZURE.INCLUDE [Stop and restart virtual device](../../includes/storsimple-stop-restart-virtual-device.md)]

### <a name="reset-to-factory-defaults"></a>Klicken Sie auf Standardwerte für das Zurücksetzen

Wenn Sie sich entscheiden, dass Sie nur mit Ihrem Gerät virtuelle neu beginnen möchten, einfach deaktivieren Sie und löschen Sie ihn, und klicken Sie dann erstellen Sie einen neuen. Genau wie bei Ihrem Gerät physische zurückgesetzt wird wird das neue virtuelle Gerät alle Updates sind nicht installiert; Stellen Sie daher sollten Sie prüfen nach Updates suchen, bevor Sie es verwenden.


## <a name="fail-over-to-the-virtual-device"></a>Möglicher Fehler über am virtuellen Gerät

Wiederherstellung (DR) ist eines der wichtigsten Szenarien, denen für das StorSimple virtuelle Gerät entwickelt wurde. In diesem Szenario ist möglicherweise die physische StorSimple Gerät oder die gesamte Datacenter nicht verfügbar. Glücklicherweise können Sie ein virtuelles Gerät Operationen in einem anderen Speicherort wiederherstellen aus. Während DR die Lautstärke Container vom Quellgerät Besitz ändern und am virtuellen Gerät übertragen werden. Die Komponenten für DR sind, dass das virtuelle Gerät erstellt und konfiguriert wurde, alle Datenträger innerhalb des Containers Lautstärke offline getroffen wurden und der Lautstärke Container eine zugeordnete weist Cloud Momentaufnahme.

>[AZURE.NOTE] 
>
> - Beachten Sie bei ein virtuelles Gerät als sekundäre Gerät für DR verwenden zu können, beachten Sie, dass die 8010 30 TB Standard-Speicher und 8020 64 TB Premium-Speicher hat. Höhere Kapazität 8020 virtuelles Gerät möglicherweise geeigneter für ein DR-Szenario.
> - Sie können sich nicht Failover oder Klonen auf einem Gerät Update 2 auf einem Gerät 1 vor dem Update unter ausgeführt. Sie können jedoch nicht über ein Gerät mit Update 2 auf ein Gerät unter Update 1 (1.1 oder 1.2)

Eine schrittweise Anleitung wechseln Sie zu [Failover zu einem virtuellen Gerät](storsimple-device-failover-disaster-recovery.md#fail-over-to-a-storsimple-virtual-device).
 

## <a name="shut-down-or-delete-the-virtual-device"></a>Fahren Sie oder löschen Sie das virtuelle Gerät

Wenn Sie zuvor konfiguriert und verwendet eine StorSimple virtuelles Gerät aber jetzt berechnen Gebühren für ihre Verwendung antizipierte beenden möchten, können Sie die virtuelle Gerät zu deaktivieren. Das virtuelle Gerät beendet wird deren Datenlaufwerke im Speicher oder Betriebssystem gelöscht. Hält sie Gebühren für Ihr Abonnement antizipierte, aber weiterhin Speicher Gebühren für den Datenträger OS und Daten.

Wenn Sie ausgeschaltet werden virtuelle oder löschen, wird es auf der Seite Geräte des Diensts StorSimple Manager als **Offline** angezeigt. Sie können auswählen, deaktivieren oder löschen Sie das Gerät aus, wenn Sie auch die Sicherungskopien erstellt, das virtuelle Gerät löschen möchten. Weitere Informationen finden Sie unter [deaktivieren und Löschen eines StorSimple Gerät](storsimple-deactivate-and-delete-device.md).

[AZURE.INCLUDE [Shut down a virtual device](../../includes/storsimple-shutdown-virtual-device.md)]

[AZURE.INCLUDE [Delete a virtual device](../../includes/storsimple-delete-virtual-device.md)]

   
## <a name="troubleshoot-internet-connectivity-errors"></a>Behandeln von Problemen mit Internet Connectivity-Fehler 

Während der Erstellung eines virtuellen Geräts Wenn es keine Verbindung mit dem Internet besteht, fehl Schritt beim Erstellen des. Gehen Sie folgendermaßen zur Problembehandlung, ob der Fehler aufgrund von Internet Connectivity, in der klassischen Azure-Portal:

1. Erstellen Sie eine Windows Server 2012-virtuellen Computern in Azure. Dieses virtuellen Computers sollten die gleichen Speicherkonto, VNet und Subnetz wie von Ihrem Gerät virtuelle verwendet. Wenn Sie bereits einen vorhandenen Windows Server Host in Azure mit Speicher-Konto, Vnet und Subnetz verfügen, können Sie es auch im Internet Verbindungsproblemen verwenden.
2. Remote Log in den virtuellen Computern, die im vorherigen Schritt erstellt haben. 
3. Öffnen Sie ein Eingabeaufforderungsfenster innerhalb des virtuellen Computers (Gewinn + R, und geben Sie dann ein `cmd`).
4. Führen Sie die folgenden Cmd aufgefordert werden.

    `nslookup windows.net`

5. Wenn `nslookup` schlägt fehl, und klicken Sie dann Internet Connectivity Fehler verhindert, dass das virtuelle Gerät zum Dienst StorSimple Manager registrieren. 
6. Nehmen Sie die erforderlichen Änderungen vor, mit Ihrem Netzwerk virtuelle, um sicherzustellen, dass das virtuelle Gerät auf Azure Websites wie "Windows" zugreifen kann.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie der Dienst StorSimple Manager zum Verwalten von eines virtuellen Geräts zu [verwenden](storsimple-manager-service-administration.md).
 
- Grundlegendes zum [Wiederherstellen einer StorSimple Lautstärke aus einer Sicherung festlegen](storsimple-restore-from-backup-set.md). 

