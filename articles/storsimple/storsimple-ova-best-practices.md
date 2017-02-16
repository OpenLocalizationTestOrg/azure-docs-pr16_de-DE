<properties 
   pageTitle="Bewährte Methoden für virtuelle Array StorSimple | Microsoft Azure"
   description="Beschreibt die optimalen Methoden zum Bereitstellen und Verwalten von virtuellen StorSimple Array."
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
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-best-practices"></a>Bewährte Methoden für StorSimple Virtual Array

## <a name="overview"></a>(Übersicht)

Microsoft Azure StorSimple virtuelle Matrix ist eine integrierte Speicher-Lösung, die Verwalten von Speicheraufgaben zwischen einer lokalen virtuelles Gerät in einem Hypervisor und Microsoft Azure-Cloud-Speicher ausgeführt. StorSimple virtuelle Matrix ist eine effiziente und kostengünstiger Alternative zum physischen Array 8000-Serie. Virtuelle Arrays auf Ihrer vorhandenen Hypervisor Infrastruktur ausgeführt werden kann, unterstützt sowohl iSCSI als auch die Protokolle SMB und eignet sich gut für den remote-Standort Office Szenarien. Weitere Informationen zu den StorSimple Techniken wechseln Sie zu [Microsoft Azure StorSimple Übersicht](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx).

Dieser Artikel behandelt die bewährten Methoden bei der ersten Einrichtung, Bereitstellung und Verwaltung des Arrays virtuellen StorSimple implementiert. Bewährten bieten überprüfte Richtlinien für das Einrichten und Verwalten von Ihrer virtuelle Array. In diesem Artikel wird in Richtung IT-Administratoren gedacht bereitstellen und verwalten die virtuellen Matrizen in ihren Rechenzentren.

Es empfiehlt sich eine regelmäßige Überprüfung bewährten Methoden, um sicherzustellen, dass Ihr Gerät immer noch in Compliance ist, wenn Setup oder Vorgang illustrieren geändert wird. Sie sollten Probleme auftreten, während der Implementierung der empfohlenen Vorgehensweisen auf Ihrem virtuellen Array, [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md) , um Unterstützung.

## <a name="configuration-best-practices"></a>Bewährte Methoden für die Konfiguration 

Bewährten hervorgehen, die Richtlinien, die während der ersten Einrichtung und Bereitstellung der virtuelle Arrays berücksichtigt werden müssen. Bewährten gehören derjenigen für die Bereitstellung des virtuellen Computers, gruppenrichtlinieneinstellungen Ziehpunkt, zum Einrichten der Netzwerke, Speicherkonten konfigurieren und Erstellen von Freigaben und Datenmengen für das virtuelle Array. 

### <a name="provisioning"></a>Bereitgestellt 

StorSimple virtuelle Matrix ist nach der Bereitstellung auf dem Hypervisor (Hyper-V oder VMware) virtueller Computer (virtueller Computer) von Ihrem Servers. Beim Bereitstellen des virtuellen Computers, stellen Sie sicher, dass Ihr Host ausreichende Ressourcen reserviert sein soll kann. Weitere Informationen finden Sie auf die [benötigten minimalen Ressourcen](storsimple-ova-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements) zur Bereitstellung von einer Matrix zurück. 

Implementieren Sie die folgenden bewährten Methoden beim Bereitstellen des virtual Arrays:


|                        | Hyper-V                                                                                                                                        | VMware                                                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Typ des virtuellen Computern**   | **Generation 2** Virtueller Computer für die Verwendung mit Windows Server 2012 oder höher und ein *.vhdx* Bild. <br></br> **1 der zweiten Generation** Virtueller Computer für die Verwendung mit einem Windows Server 2008 oder höher und ein *VHD-* Abbild.                                                                                                              | Verbindung mit virtuellen Computern Version 8-11 *.vmdk* Bild verwenden.                                                                      |
| **Arbeitsspeichertyp**            | Konfigurieren Sie als **statischen Speicher**. <br></br> Verwenden Sie die Option **dynamischen Arbeitsspeicher** nicht.            |                                                    |
| **Datenträger-Datentyp**         | Bereitstellung von als **dynamisch erweitern**.<br></br> **Feste Größe** dauert sehr lange. <br></br> Verwenden Sie nicht die Option **Vergleichen** .                                                                                                                   | Verwenden Sie die Option **dünne bereitstellen** .                                                                                      |
| **Laufwerk Änderung von Daten** | Erläuterten oder verkleinern ist nicht zulässig. Vergeblich versucht, führt die lokalen Daten auf Gerät verloren gehen.                       | Erläuterten oder verkleinern ist nicht zulässig. Vergeblich versucht, führt die lokalen Daten auf Gerät verloren gehen. |

### <a name="sizing"></a>Ziehpunkt

Wenn Ihr StorSimple virtuelle Array Ziehpunkt, beachten Sie die folgenden Faktoren aus:

- Lokale Reservierung für Datenmengen oder Freigaben. Etwa 12 % des Leerzeichens wird auf der lokalen Ebene für jedes bereitgestellte gestufte Volume oder eine Freigabe reserviert. Grob ist 10 % des Speicherplatzes für ein lokales angeheftete Volume für Dateisystem auch reserviert.
- Snapshot Aufwand. Grob ist 15 % Speicherplatz auf der lokalen Ebene für Momentaufnahmen reserviert.
- Notwendigkeit Nachrichtenübermittlungsort wiederhergestellt. Wiederherstellen als einen neuen Vorgang ausführen zu können, sollten Ziehpunkte für den Abstand zur Wiederherstellung erforderlichen berücksichtigt werden. Wiederherstellen ist fertig, um eine Freigabe oder das Volume der gleichen Größe oder vergrößern.
- Einige Puffer sollte für alle unerwartete geometrischen zugewiesen werden.

Basierend auf die vorherigen Faktoren, können die Ziehpunkte Anforderungen durch die folgende Gleichung dargestellt werden:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`


Die folgenden Beispiele verdeutlichen, wie Sie Ihren Anforderungen entsprechend virtuelles Array Größe anpassen können.

#### <a name="example-1"></a>Beispiel 1:
Auf Ihrem virtuellen Array möchten Sie können 

- Bereitstellen einer 2 TB gestufte Volumen oder freigeben.
- Bereitstellung von 1 TB gestufte Volumen oder freigeben.
- Bereitstellen einer 300 GB lokal angeheftete Lautstärke oder freigeben.


Für die vorherige Datenmengen oder Freigaben lassen Sie uns den Platzbedarf auf der lokalen Ebene berechnet werden. 

Zunächst für jede gestufte Lautstärke/freigeben wäre lokale Reservierung gleich 12 % der Lautstärke/freigeben Größe. Für lokal angeheftete Lautstärke/freigeben ist die lokale Reservierung 10 % der Größe lokal angeheftete Volumen-Freigabe (zusätzlich zur bereitgestellte Größe). In diesem Beispiel müssen Sie

- Lokale Reservierung 240 GB (für eine 2 TB gestuft Lautstärke/freigeben)
- Lokale Reservierung 120 GB (für 1 TB gestuft Lautstärke/freigeben)
- 330 GB für lokales angeheftete Volumen oder freigeben (10 % der lokalen Reservierung wird die Größe 300 GB nach der Bereitstellung hinzu)

Der gesamte Speicherplatz auf der lokalen Ebene bisher erforderlich ist: 240 GB + 120 GB + 330 GB = 690 GB.

Zweites, benötigen wir mindestens soviel Speicherplatz auf der lokalen Ebene als die größte einzelne Reservierung an. Dieser zusätzliche Betrag wird verwendet, falls Sie über eine Momentaufnahme der Cloud wiederherstellen müssen. In diesem Beispiel wird die größte lokale Reservierung 330 GB (einschließlich Reservierung für File System), damit Sie hinzufügen möchten, dass auf 690 GB: 690 GB + 330 GB = 1020 GB.
Wenn wir nachfolgende zusätzliche stellt durchgeführt haben, können wir immer die Speicherplatz aus der vorherigen Wiederherstellung zurück.

Benötigen wir 15 % von Ihrem lokalen Speicherplatzes bisher an die lokale Momentaufnahmen, speichern, damit nur 85 %, verfügbar ist. In diesem Beispiel ist das wäre, um 1020 GB = 0.85&ast;bereitgestellte Daten Datenträger TB. Ja, der Datenträger bereitgestellte Daten wäre (1020&ast;(1/0.85)) = 1200 GB = 1,20 TB ~ 1,25 TB (Runden auf die nächste Quartile)

Unerwartete Wachstum und neue stellt der Faktor, sollten Sie Bereitstellung von einer lokalen Festplatte des um 1,25-1,5 TB.

> [AZURE.NOTE] Es empfiehlt sich auch, dass die lokale Festplatte Thin Provisioning bereitgestellt wird. Wird diese empfohlen, weil der Speicherplatz Wiederherstellen nur benötigt wird, wenn Sie Daten wiederherstellen, die älter als fünf Tage möchten. Wiederherstellung auf Elementebene können Sie zum Wiederherstellen von Daten für die letzten fünf Tage ohne Bundsteg für wiederherstellen.

#### <a name="example-2"></a>Beispiel 2: 
Auf Ihrem virtuellen Array möchten Sie können 

- Bereitstellen einer 2 TB gestuft Lautstärke
- Bereitstellen einer 300 GB lokal angehefteten Lautstärke

Abhängig von 12 % des lokalen Raum Reservierung für gestufte Datenmengen-Freigaben und 10 % für lokales angeheftete Datenmengen-Freigaben müssen wir

- Lokale Reservierung 240 GB (für 2 TB gestuft Lautstärke/freigeben)
- 330 GB für lokales angeheftete Volumen oder freigeben (den Abstand 300 GB nach der Bereitstellung 10 % der lokalen Reservierung hinzu)

Gesamter Erforderlicher Speicherplatz auf der lokalen Ebene ist: 240 GB + 330 GB = 570 GB

Der minimalen lokale benötigte Platz für Wiederherstellen 330 GB ist. 

15 % des gesamte Datenträgers wird verwendet, um die Momentaufnahmen speichern, sodass nur 0.85 verfügbar ist. Ja, um die Größe des ist (900&ast;(1/0.85)) = 1,06 TB ~ 1,25 TB (Runden auf die nächste Quartile) 

Der Faktor in einer beliebigen unerwartete Wachstum, können Sie eine lokale 1,25-1,5 TB-Datenträger bereitstellen.


### <a name="group-policy"></a>Gruppenrichtlinien

Gruppenrichtlinien sind eine Infrastruktur, die Sie bestimmte Konfigurationen für Benutzer und Computer implementieren kann. Gruppenrichtlinieneinstellungen in Gruppenrichtlinien-Objekte (Gruppenrichtlinienobjekt) aus, die mit den folgenden Active Directory-Domänendiensten (AD DS) Containern verknüpft sind enthalten sind: Websites, Domänen oder Organisations-Einheiten. 

Wenn Ihr virtuelle Array Domäne ist, können Gruppenrichtlinienobjekte darauf angewendet. Diese Gruppenrichtlinienobjekte können Anwendungen wie eine Antivirensoftware installieren, die den Vorgang des Arrays virtuellen StorSimple beeinträchtigen können.

Daher, wir empfehlen, die Sie:

-   Stellen Sie sicher, dass Ihr virtuelle Array in einem eigenen (Organisationseinheit) für Active Directory ist. 

-   Stellen Sie sicher, dass keine Gruppenrichtlinienobjekte (GPOs) auf Ihre virtuelle Array angewendet werden. Sie können die Vererbung um sicherzustellen, dass die virtuelle Matrix (untergeordnete Knoten) Gruppenrichtlinienobjekte nicht automatisch von der übergeordneten erben. Informationen um die [Vererbung](https://technet.microsoft.com/library/cc731076.aspx)zu wechseln.


### <a name="networking"></a>Netzwerke

Die Konfiguration für Ihre virtuelle Array erfolgt über die lokale Web-Benutzeroberfläche. Virtuelle Netzwerk-Schnittstellen wird über der Hypervisor aktiviert, in dem das virtuelle Array bereitgestellt wird. Verwenden Sie die Seite [Einstellungen im Netzwerk](storsimple-ova-deploy3-fs-setup.md) so konfigurieren Sie die virtuelle Netzwerk Schnittstelle IP-Adresse, Subnetz und Gateway ein.  Sie können auch die primären und sekundären DNS-Server, der Zeit und optional Proxyeinstellungen für Ihr Gerät konfigurieren. Die meisten der Netzwerkkonfiguration ist eine einmalige eingerichtet wurde. Überprüfen Sie die [StorSimple Netzwerke Anforderungen](storsimple-ova-system-requirements.md#networking-requirements) vor der Bereitstellung von virtuellen Arrays.

Wenn Sie Ihre virtuelle Array bereitstellen, empfehlen wir, führen Sie die folgenden bewährte Methoden:

-   Sicher, dass das Netzwerk, in dem das virtuelle Array immer bereitgestellt wird, die Kapazität, 5/s Internetbandbreite (oder mehr) reserviert sein soll. 

    -   Internet Bandbreite müssen, hängt von Ihrer Arbeitsbelastung Merkmale und den Zinsfuß Daten ändern.

    -   Die Daten ändern, die behandelt werden kann ist direkt proportional auf die Internetbandbreite. Beispielsweise wenn Sie eine Sicherungskopie zu erstellen kann eine 5/s Bandbreite eine Änderung Daten von ungefähr 18 GB 8 Stunden aufnehmen. Bei viermal mehr Bandbreite (20 MB/s) können Sie weitere viermal Daten ändern (72 GB) behandeln. 

-   Stellen Sie sicher, dass die Verbindung mit dem Internet immer verfügbar ist. Gelegentliche oder unzuverlässige Internet-Verbindungen mit den Geräten können in einem Verlust von Access mit Daten in der Cloud und in einer nicht unterstützten Konfiguration ergeben.

-   Wenn Sie Ihr Gerät als iSCSI-Server bereitstellen möchten: 
    -   Es empfiehlt sich, dass Sie die Option **erste IP-Adresse automatisch** (DHCP) zu deaktivieren. 
    -   Konfigurieren Sie statische IP-Adressen ein. Sie müssen einen primären und einen sekundären DNS-Server konfigurieren.

    -   Wenn mehrere Netzwerkschnittstellen auf Ihre virtuellen definieren array, nur die erste Netzwerkschnittstelle (Standardmäßig wird diese Schnittstelle **Ethernet**) die Cloud zu erreichen können. Um den Typ des Datenverkehrs zu steuern, können mehrere virtuelle Netzwerkschnittstellen auf Ihrem virtuellen Array (als iSCSI-Server konfiguriert) erstellen und verbinden diese Schnittstellen mit verschiedenen Subnetzen.

-   Um nur die Cloud Bandbreite einschränken (vom virtuelle Array verwendete), auf dem Router oder die Firewall begrenzungsebene konfigurieren. Wenn Sie in Ihrem Hypervisor begrenzungsebene definiert haben, werden sie alle Protokolle, einschließlich der iSCSI- und SMB statt nur die Cloud Bandbreite einschränken. 

-   Sicherstellen Sie, dass die Synchronisierung Zeit, für Hypervisoren aktiviert ist. Wenn Ihre virtuelle Matrix mit Hyper-V, im Hyper-V-Manager ausgewählt haben, wechseln Sie zu **Einstellungen &gt; Integration Services**, und stellen Sie sicher, dass die **Synchronisierung** aktiviert ist.

### <a name="storage-accounts"></a>Speicherkonten

StorSimple virtuelle Array können mit einem einzelnen Speicherkonto verknüpft sein. Dieses Speicherkonto könnte ein automatisch generiertes Speicherkonto ein Konto im selben Abonnement als Dienst oder ein Speicherkonto im Zusammenhang mit einem anderen Abonnement. Weitere Informationen finden Sie unter zum [Verwalten von Speicherkonten für Ihre virtuelle Matrix zurück](storsimple-ova-manage-storage-accounts.md).

Verwenden Sie folgende Aspekte für Speicherkonten Ihr virtuelle Array zugeordnet.

-   Wenn mehrere virtuelle Arrays mit einem einzelnen Speicherkonto zu verknüpfen, die in die maximale Kapazität (64 TB) für eine virtuelle Array und die maximale Größe (500 TB) für ein Speicherkonto berücksichtigt werden. Dadurch wird die Anzahl voller Größe virtuelle Matrizen zurück, die mit diesem Speicherkonto bis zu 7 verknüpft werden können.

-   Beim Erstellen eines neuen Kontos mit Speicher
    -   Es empfiehlt sich, dass Sie sie in der Region, der remote/Zweigstelle am nächsten ist erstellt haben, wo Ihre StorSimple Virtual Array bereitgestellt wird, um Wartezeiten zu minimieren.

    -   Tragen Sie beachten Sie, dass Sie ein Speicherkonto über verschiedener Regionen verschieben können. Sie können keine auch einen Dienst über Abonnements verschieben.

    -   Verwenden Sie ein, die zwischen den Datencentern Redundanz implementiert Speicherkonto. Geo-redundante Speicher (GRS), Zone redundante Speicher (ZRS) und lokal redundante Speicher (LRS) werden alle für die Verwendung mit Ihrer virtuelle Array unterstützt. Weitere Informationen über die verschiedenen Arten von Speicherkonten wechseln Sie zu [Replikation Azure-Speicher](../storage/storage-redundancy.md).


### <a name="shares-and-volumes"></a>Freigaben und Datenmengen

Sie können für Ihre virtuelle StorSimple Array Freigaben bereitstellen, wenn es als eine Dateiserver und Datenmengen, wenn als iSCSI-Server so konfiguriert, dass konfiguriert ist. Die bewährte Methoden zum Erstellen von Freigaben und Datenmengen beziehen sich auf die Größe und den Typ konfiguriert.

#### <a name="volumeshare-size"></a>Volumen/freigeben Größe

Auf Ihrem virtuellen Array können Sie Freigaben bereitstellen, wenn es als eine Dateiserver und Datenmengen, wenn als iSCSI-Server so konfiguriert, dass konfiguriert ist. Die bewährte Methoden zum Erstellen von Freigaben und Datenmengen beziehen sich auf die Größe und den Typ konfiguriert. 

Beachten Sie die folgenden bewährten Methoden beim Freigaben oder Datenmengen auf Ihrem Gerät virtuelle bereitgestellt.

-   Die Dateigröße relativ zur bereitgestellte Größe einer gestufte Freigabe können die tiering Leistung auswirken. Arbeiten mit großer Dateien ergeben in einer langsam Ebene. Bei der Arbeit mit großer Dateien, empfehlen wir, dass die größte Datei kleiner als 3 % der Größe freigeben.

-   Klicken Sie auf die virtuelle Matrix kann maximal 16 Datenmengen-Freigaben erstellt werden. Wenn lokal fixiert ist, kann die Datenmengen/Freigaben zwischen 50 GB und 2 TB sein. Wenn gestuft, muss die Datenmengen/Freigaben zwischen 500 GB bis 20 TB. 

-   Wenn Sie ein Volume erstellen, in der erwarteten Daten Verbrauch sowie zukünftiges Wachstum berücksichtigt werden. Während die Lautstärke später nicht erweitert werden kann, können Sie immer auf einen größeren Datenträger wiederherstellen.

-   Sobald die Lautstärke erstellt wurde, kann die Größe des Datenträgers auf StorSimple nicht verkleinert werden.
   
-   Beim Schreiben auf einen Datenträger gestufte auf StorSimple, wenn die Lautstärke Daten einen bestimmten Schwellenwert (relativ zu dem lokalen Leerzeichen reserviert für die Lautstärke) erreicht, die EA beschränkt. In diesem Handbuch schreiben Sie fortfahren verlangsamt die EA erheblich. Obwohl Sie eine gestufte Lautstärke über seine bereitgestellte Kapazität hinaus schreiben können (wir nicht aktiv den Benutzer von außerhalb der bereitgestellten Kapazität Schreiben beenden), wird eine Benachrichtigung für den Effekt, den Sie Abmilderung haben. Sobald Sie die Benachrichtigung angezeigt wird, wird dringend empfohlen, die Sie ergreifen Sanierungsmaßnahmen wie löschen die Lautstärke Daten oder die Lautstärke auf einen größeren Datenträger wiederherstellen (Volume-Erweiterung wird derzeit nicht unterstützt).

-   Für Disaster Wiederherstellung verwenden Fälle wie die Anzahl der zulässigen Freigaben/Datenträger 16 ist und die maximale Anzahl von Freigaben/Datenmengen, die parallel verarbeitet werden kann, auch 16 ist, verfügt die Anzahl der Freigaben/Datenträger einen Einfluss auf Ihre RPO und RTO nicht. 

#### <a name="volumeshare-type"></a>Volumen/freigeben Typ

StorSimple unterstützt zwei Volumen/freigeben anhand ihrer Verwendung: lokal angeheftet und Ebenen. Lokal angeheftete Datenmengen-Freigaben werden Linienstärke bereitgestellt werden, während die gestuften Datenmengen/Freigaben Thin Provisioning bereitgestellt werden. 

Es empfiehlt sich, dass Sie die folgenden bewährten Methoden beim StorSimple Datenmengen/Freigaben zu konfigurieren implementieren:

-   Identifizieren Sie die Lautstärke basierend auf der Auslastung, die Sie bereitstellen, bevor Sie einen Datenträger erstellen möchten. Verwenden Sie lokal angeheftete Datenmengen für Auslastung, die erfordern lokale Garantien Daten (auch bei einem Ausfall Cloud) sowie die niedrig Cloud Wartezeiten. Sie können den Lautstärke Typ nicht ändern nach dem Erstellen eines Datenträgers auf Ihrem virtuellen Array von angehefteten Sie lokal zu gestufte oder *umgekehrt*. Erstellen Sie beispielsweise lokal angeheftete Datenmengen beim Bereitstellen von SQL-Auslastung oder Auslastung Hostinganbieter virtuellen Computern (virtuellen Computern); Verwenden Sie für die Datei freigeben Auslastung gestufte Datenmengen.

-   Aktivieren Sie die Option für weniger häufig verwendeten archivierte Daten beim Umgang mit großen Dateien. Eine größere Deduplication Textbaustein Größe von 512 KB wird verwendet, wenn diese Option aktiviert ist, um die Datenübertragung in der Cloud zu beschleunigen.

#### <a name="volume-format"></a>Volumen-format

Nachdem Sie auf dem Server iSCSI StorSimple Datenmengen erstellt haben, müssen Sie Initialisierung, bereitstellen und die Datenmengen zu formatieren. Dieser Vorgang ist auf dem Host bei einer Verbindung zu Ihrem Gerät StorSimple ausgeführt. Beim Bereitstellen und die Formatierung auf dem Host StorSimple Datenmengen, werden folgende bewährte Methoden empfohlen.

-   Führen Sie eine Symbolleiste Format auf alle StorSimple Datenträger aus.

-   Beim Formatieren einer StorSimple Volumen verwenden eine Zuordnungseinheit (AUS) von 64 KB (Standardwert ist 4 KB). Die 64 KB AUS basiert auf Tests für häufige StorSimple Auslastung und aufgrund der Ergebnisse intern als erledigt.

-   Bei Verwendung des StorSimple Virtual Array als iSCSI-Server konfiguriert übergreifende Datenträger oder dynamischen Datenträger nicht als diese Datenträger verwenden oder Datenträger werden vom StorSimple nicht unterstützt.

#### <a name="share-access"></a>Freigeben des Zugriffs

Wenn Freigaben auf Ihrem Dateiserver virtuelle Array erstellen, die folgenden Richtlinien:

-   Wenn Sie eine Freigabe zu erstellen, weisen Sie eine Benutzergruppe als Administrator freigeben anstelle eines einzelnen Benutzers aus.

-   Sie können die NTFS-Berechtigungen verwalten, nachdem die Freigabe erstellt wurde, indem Sie die Freigaben über Windows Explorer bearbeiten.

#### <a name="volume-access"></a>Volumen-Zugriff

Wenn Sie die iSCSI-Datenträger auf Ihrem StorSimple virtuelle Array konfigurieren, ist es wichtig zum Steuern des Zugriffs Stelle erforderlich. Um zu bestimmen, welche Hostserver Datenmengen zugreifen können, erstellen Sie, und ordnen Sie StorSimple Datenmengen Access Steuerelement Einträge (ACRs).

Verwenden Sie die folgenden bewährten Methoden beim ACRs für StorSimple Datenmengen konfigurieren:

-   Ordnen Sie mindestens eine ACR immer einen Datenträger.

-   Definieren Sie mehrere ACRs nur in einer gruppierten Umgebung an.

-   Wenn ein Volume mehrere ACR zuordnen möchten, stellen Sie sicher, dass die Lautstärke nicht auf eine Weise verfügbar gemacht wird, wo es gleichzeitig von mehreren nicht gruppierten Host zugegriffen werden kann. Wenn Sie mehrere ACRs auf einen Datenträger zugewiesen haben, informiert eine Warnmeldung Sie zur Überprüfung Ihrer Konfigurations.

### <a name="data-security-and-encryption"></a>Daten Sicherheit und Verschlüsselung

Ihre StorSimple Virtual Array enthält Daten Sicherheit und Verschlüsselung-Features, die die Vertraulichkeit und Integrität Ihrer Daten zu gewährleisten. Wenn Sie diese Features verwenden zu können, empfiehlt es sich, dass Sie bewährten folgen: 

-   Definieren einer Cloud-Speicher Verschlüsselungsschlüssels AES-256-Verschlüsselung generieren, bevor Sie die Daten in der Cloud von Ihrem virtuelle Array gesendet wurde. Dieser Schlüssel ist nicht erforderlich, wenn die Daten zunächst verschlüsselt werden. Die Taste kann generiert und sicher mithilfe eines Systems Key-Verwaltung wie [Azure Key Tresor](../key-vault/key-vault-whatis.md)aufbewahrt werden.

-   Wenn Sie das Speicherkonto über den StorSimple-Manager-Dienst konfigurieren, stellen Sie sicher, den zum Erstellen eines Kanals für die Kommunikation zwischen Ihrem Gerät StorSimple und der Cloud SSL-Modus aktivieren.

-   Neu zu generieren die Tasten für Ihre Speicherkonten (den Zugriff auf den Azure-Speicherdienst) regelmäßig in Konto für den Zugriff auf Änderungen basierend auf die geänderte Liste von Administratoren.

-   Daten auf Ihrem virtuellen Array komprimiert und deduplizierte, bevor sie in Azure gesendet wird. Nicht empfohlen Datendeduplizierung Rollendienst auf Ihrem Windows Server-Host verwenden.


## <a name="operational-best-practices"></a>Bewährten Methoden

Betrieb bewährten Methoden werden Richtlinien, die während der Verwaltungsarbeiten oder Betrieb des Arrays virtuellen eingehalten werden sollte. Diese Methoden Deckblatt bestimmte Verwaltungsaufgaben wie Sicherungskopien, Wiederherstellen aus einer Sicherung Ausführen eines Failovers aufzeichnen, deaktivieren und löschen die Matrix, Überwachung Nutzung des Dateisystems und Gesundheit und Virus ausführen scannt auf Ihrem virtuellen Array.

### <a name="backups"></a>Sicherungskopien

Die Daten auf Ihrem virtuellen Array in der Cloud auf zwei Arten gesichert werden, ein Standardwert automatisierte tägliche Sicherung des gesamten Geräts starten um 22:30 oder über eine manuelle Sicherung von bei Bedarf. Standardmäßig wird das Gerät automatisch tägliche Cloud Momentaufnahmen aller Daten, die auf ihn erstellt. Weitere Informationen zum [Erstellen einer Sicherungskopie Ihrer StorSimple Virtual Array](storsimple-ova-backup.md)wechseln.

Die Häufigkeit und Beibehaltung der standardmäßigen Sicherungskopien zugeordnet können nicht geändert werden, aber Sie können die Uhrzeit, an der die täglichen Sicherungen jeden Tag initiiert werden, konfigurieren. Wenn Sie die Startzeit für die automatische Sicherung konfigurieren zu können, empfehlen wir, die auf:

-   Planen Sie Ihre Sicherungskopien für Zeiten aus. Zusätzliche Startzeit sollten nicht mit zahlreichen Host EA überein.

-   Rufen Sie die manuelle Sicherung bei Bedarf bei der Planung für ein Gerät Failover oder vor dem Klicken Sie im Wartungsfenster zum Schutz der Daten auf Ihrem virtuellen Array durchführen.

### <a name="restore"></a>Stellen Sie wieder her

Sie können wiederherstellen aus einer Sicherung auf zwei Arten festlegen: Wiederherstellen in ein anderes Volume oder freigeben oder Ausführen eine Elementebene Wiederherstellung (nur auf Virtuelles Array als Dateiserver konfiguriert verfügbar). Wiederherstellung auf Elementebene können Sie eine detaillierte Wiederherstellung von Dateien und Ordnern aus einer Sicherung Cloud aller Freigaben auf dem Gerät StorSimple führen. Weitere Informationen zum [Wiederherstellen aus einer Sicherung](storsimple-ova-restore.md)wechseln.

Wenn Sie eine Wiederherstellung durchführen zu können, sollten beachten Sie die folgenden Richtlinien:

-   In-Place-Wiederherstellung unterstützt Ihrer StorSimple virtuelle Matrix nicht. Dies kann jedoch leicht werden durch einen zweistufigen Prozess erreicht: Stellen Sie Speicherplatz auf dem virtuellen Arrays ab, und klicken Sie dann auf ein anderes Volume-Freigabe wiederherstellen.

-   Wenn Sie Punkte beibehalten wiederherstellen aus einem lokalen Datenträger, wird die Wiederherstellung eine umfangreiche Operation müssen. Obwohl die Lautstärke schnell online geschaltet werden kann, wird die Daten weiterhin im Hintergrund hydrated werden.

-   Der Lautstärke Typ bleibt unverändert während des Wiederherstellungsvorgangs an. Eine gestufte Lautstärke wird in ein anderes gestufte Volume wiederhergestellt, und eine lokal angeheftete Volume zu einem anderen lokal angehefteten Lautstärke.

-   Bei dem Versuch, ein Volume oder eine Freigabe aus einer Sicherung wiederherstellen festlegen, wenn der Wiederherstellungsauftrag fehlschlägt, ein Ziel-Volume oder Freigeben im Portal erstellt werden. Es ist wichtig, dass Sie diese nicht verwendete Ziel-Volume löschen oder im Portal freigeben, um alle zukünftigen Problemen im Zusammenhang dieses Elements zu minimieren.

### <a name="failover-and-disaster-recovery"></a>Failover und Disaster Wiederherstellung

Ein Gerät Failover können Sie Ihre Daten auf einem Gerät *Quelle* im Datencenter an *ein anderes Gerät, befindet sich in der gleichen oder einem anderen geographischen Standort* migrieren. Das Gerät Failover ist für das gesamte Gerät. Während des Failovers ändert die Cloud-Daten für das Quellgerät Besitzrechte an der das Gerät an.

Für das virtuelle StorSimple Array können Sie nur über in ein anderes virtuelle Array vom gleichen StorSimple Manager-Dienst verwaltet fehl. Ein Failover auf einem Gerät 8000-Serie oder einer Matrix zurück, die von einem anderen StorSimple Manager-Dienst (als die für das Quellgerät) verwaltet werden, ist nicht zulässig. Wechseln Sie zu [erforderlichen Komponenten für das Gerät Failover](storsimple-ova-failover-dr.md), um weitere Informationen zu den Failover Aspekte.

Bei der Durchführung einer Fail über für Ihre virtuelle Array Beachten Sie Folgendes beachten:

-   Bei einem geplanten Failover ist es empfiehlt es sich alle Datenmengen/Freigaben offline vor dem Initiieren des Failovers ausführen. Anweisungen Sie die Betriebssystem-spezifische machen Sie die Datenmengen/Freigaben offline auf dem Host ersten und dann offline schalten die auf Ihrem Gerät virtuelle.

-   Für eine Datei Server Wiederherstellung (DR) empfehlen wir, dass Sie das Gerät mit dieselbe Domäne als Quelle zu verknüpfen, damit die Freigabeberechtigungen automatisch aufgelöst werden. Nur das Failover an ein Gerät in der gleichen Domäne wird in dieser Version unterstützt.

-   Sobald die DR erfolgreich abgeschlossen ist, wird das Quellgerät automatisch gelöscht. Obwohl das Gerät nicht mehr verfügbar ist, ist der virtuellen Computern, die nach der Bereitstellung auf dem Host-System weiterhin Ressourcen in Anspruch. Es empfiehlt sich, dass Sie dieses virtuellen Computers aus Ihrem Hostsystem, um zu verhindern, dass alle Gebühren antizipierte löschen.

-   Führen Sie beachten Sie, dass selbst wenn das Failover nicht erfolgreich ist, wird **die Daten in der Cloud immer sicher ist**. Berücksichtigen Sie die folgenden drei Szenarien, in denen das Failover nicht erfolgreich abgeschlossen wurde:

    -   Fehler bei den ersten Schritten für das Failover z. B., wenn die Vorabversion DR-Prüfungen ausgeführt werden. In diesem Fall ist Ihr Zielgerät weiterhin verwenden können. Sie können das Failover auf demselben Zielgerät wiederholen.

    -   Während des eigentlichen Failovervorgangs ist ein Fehler aufgetreten. In diesem Fall wird das Gerät installiertes markiert. Sie müssen bereitstellen und ein anderes Ziel virtuelle Array konfigurieren und verwenden, die für Failover.

    -   Das Failover folgen, die das Quellgerät gelöscht wurde abgeschlossen wurde, aber das Gerät gibt es Probleme, und Sie auf Daten zugreifen. Die Daten weiterhin sichere befindet sich in der Cloud und durch ein anderes virtuelle Array erstellen und dann als Zielgerät verwenden, für die DR einfach abgerufen werden können.

### <a name="deactivate"></a>Deaktivieren

Wenn Sie ein StorSimple virtuelle Array deaktiviert haben, trennt Sie die Verbindung zwischen dem Gerät und dem entsprechenden StorSimple Manager-Dienst. Deaktivierung ist ein **endgültiger** Vorgang und kann nicht rückgängig gemacht werden. Eine deaktivierte Geräte kann nicht mit dem Dienst StorSimple Manager erneut registriert werden. Wechseln Sie weitere Informationen zu [deaktivieren und löschen Sie Ihre StorSimple Virtual Array](storsimple-deactivate-and-delete-device.md).

Beachten Sie die folgenden bewährten Methoden, wenn Ihre virtuelle Array können:

-   Eine Cloud Momentaufnahme aller Daten vor ein virtuelles Gerät deaktivieren. Wenn Sie ein virtuelles Array deaktiviert haben, geht die lokale Gerätedaten verloren. Erstellen einer Momentaufnahme Cloud ermöglicht Ihnen, Daten zu einem späteren Zeitpunkt wiederherzustellen.

-   Bevor Sie ein StorSimple virtuelle Array deaktiviert haben, stellen Sie sicher, beenden oder Löschen von Clients und Hosts, die auf dem Gerät abhängig sind.

-   Löschen Sie ein Gerät deaktiviert, wenn Sie nicht länger verwenden, damit es nicht Gebühren fällig.

### <a name="monitoring"></a>Für die Überwachung

Um sicherzustellen, dass Ihre StorSimple Virtual Array in einem zusammenhängenden ordnungsgemäßen Zustand ist, müssen Sie die Matrix überwachen und stellen Sie sicher, dass Sie die Informationen aus dem System, einschließlich Benachrichtigungen erhalten. Implementieren Sie die folgenden bewährten Methoden, um den allgemeinen Zustand des virtuellen Arrays überwachen:

- Konfigurieren Sie die Überwachung, um die Festplatte Verwendung Ihrer virtuelle Array Daten-Laufwerk als auch der Datenträger OS verfolgen. Wenn Hyper-V ausgeführt wird, können Sie eine Kombination aus System Center virtuellen Computern Manager (SCVMM) und System Center Operations Manager (SCOM) zum Überwachen der Virtualisierungshosts verwenden.   

- Konfigurieren von e-Mail-Benachrichtigungen auf Ihrem virtuellen Array Benachrichtigungen auf bestimmte Ebenen Verwendung zu senden.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Scannen von Applications Indexsuche und Virenschutz

StorSimple virtuelles Array können Daten aus der lokalen Ebene in der Cloud Microsoft Azure automatisch Stufen. Wenn eine Anwendung wie einer Indexsuche oder eine Virenscan Scannen auf StorSimple gespeicherten Daten verwendet wird, müssen Sie müssen Sie sicherstellen, dass die Cloud Daten nicht zugegriffen und wieder zu der lokalen Ebene herausgezogen abrufen.

Es empfiehlt sich, dass Sie die folgenden bewährten Methoden implementieren, wenn Index suchen oder Virus-Scan auf Ihrem virtuellen Array konfigurieren:

-   Deaktivieren Sie alle Vorgänge automatisch konfiguriert vollständigen Scan.

-   Konfigurieren Sie für gestufte Datenmengen die Index-suchen oder Virus-Scan-Anwendung, einen inkrementellen Scan vorzunehmen. Dies würde Scannen Sie nur die neuen Daten wahrscheinlich, die auf der lokalen Ebene. Die Daten, die in der Cloud gestuft ist ist während eines Vorgangs inkrementell nicht zugegriffen werden.

-   Stellen Sie sicher, die richtige Suchfilter und Einstellungen konfiguriert werden, damit nur die gewünschten Dateitypen gescanntes abrufen. Beispielsweise Bilddateien (JPEG, GIF und TIFF) und Technik Zeichnungen sollten nicht während der Wiederherstellung inkrementell oder vollständigen Index gescannt werden.

Bei Verwendung von Windows beim Indizierungsvorgang, führen Sie die folgenden Richtlinien:

-   Verwenden Sie die Windows-Indexer nicht für gestufte Datenmengen, wie sie große Datenmengen (TB) aus der Cloud zurückgerufen, wenn Sie der Index häufig neu erstellt werden muss. Erneutes Erstellen des Indexes werden alle Typen von Dateien, um deren Inhalt indizieren abgerufen.

-   Verwenden der Fenster beim Indizierungsvorgang für lokales angeheftete Datenmengen wie folgt nur Daten auf den lokalen Ebenen zum Erstellen des Index (die Cloud-Daten können nicht zugegriffen werden).

### <a name="byte-range-locking"></a>Byte Bereich Sperren

Applikationen können ein bestimmten Bereichs von Bytes innerhalb der Dateien sperren. Wenn Byte Bereich Sperren über die Anwendung, die in Ihrem StorSimple schreiben aktiviert ist, funktioniert klicken Sie dann Stufen auf Ihrem virtuellen Array nicht. Für gestufte um arbeiten, sollten alle Bereiche der Zugriff auf Dateien aufgehoben werden. Byte Bereich Sperren wird mit gestufte Datenmengen auf Ihrem virtuellen Array nicht unterstützt.

Empfohlene Maßnahmen vermieden Byte Bereich Sperren umfassen:

-   Deaktivieren Sie in der Anwendungslogik Sperren Byte-Bereich ein.

-   Lokal verwendet angehefteten Datenmengen (statt gestuft) für die Daten, die dieser Anwendung zugeordnet. Lokal angeheftete Datenmengen nicht in der Cloud gestuft werden.

-   Wenn Datenmengen mit lokal mit Byte Bereich Sperren aktiviert fixiert werden, kann die Lautstärke online sein, bevor die Wiederherstellung abgeschlossen ist. In diesen Fällen müssen Sie für die Wiederherstellung abgeschlossen sein warten.

## <a name="multiple-arrays"></a>Mehrere Matrizen zurück.

Mehrere virtuelle Arrays möglicherweise Konto für eine wachsende Datenmenge für Arbeit, die in der Cloud somit Einfluss auf die Leistung des Geräts Effekts konnte bereitgestellt werden müssen. In diesen Fällen empfiehlt es sich zu skalieren Geräte wächst die Arbeitsseite. Setzt ein oder mehrere Geräte in der Mitte der lokalen Daten hinzugefügt werden. Wenn Sie die Geräte hinzufügen möchten, können Sie folgende Aktionen:

-   Teilen Sie den aktuellen Datensatz.
-   Bereitstellen Sie neue Auslastung für neue Geräte.
-   Wenn mehrere virtuelle Arrays bereitstellen, wir empfehlen, die von den Lastenausgleich im Hinblick auf die Matrix auf verschiedenen Hypervisorhosts verteilen.

-  Mehrere virtuelle Arrays (Wenn als eine Dateiserver oder einem iSCSI-Server konfiguriert) können in einem Namespace Datei-System verteilt bereitgestellt werden. Die detaillierten Schritte finden Sie unter [Distributed File System Namespace Solution mit Bereitstellungshandbuch für hybriden Cloud-Speicher](https://www.microsoft.com/download/details.aspx?id=45507). Verteilte Datei Systemreplikation wird derzeit nicht zur Verwendung mit dem virtuelle Array empfohlen. 


## <a name="see-also"></a>Siehe auch
Erfahren Sie, wie [Ihre StorSimple Virtual Array verwalten](storsimple-ova-manager-service-administration.md) , über den StorSimple-Manager-Dienst.
