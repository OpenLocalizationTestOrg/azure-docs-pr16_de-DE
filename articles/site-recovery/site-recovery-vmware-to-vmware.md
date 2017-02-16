<properties
    pageTitle="Replikation lokale virtuelle VMware-Computer oder an einem sekundären Standort physischen Servern | Microsoft Azure"
    description="Lesen Sie diesen Artikel virtuelle VMware-Computer oder Windows/Linux physische Server auf einem sekundären Standort mit Azure Website Wiederherstellung repliziert."
    services="site-recovery"
    documentationCenter=""
    authors="nsoneji"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="nisoneji"/>


# <a name="replicate-on-premises-vmware-virtual-machines-or-physical-servers-to-a-secondary-site"></a>Lokale VMware virtuellen Computern oder physische Server auf einem sekundären Standort repliziert


## <a name="overview"></a>(Übersicht)

InMage Scout in Azure Website Wiederherstellung bietet Replikation in Echtzeit zwischen lokalen VMware Websites. InMage Scout ist in Azure Website Wiederherstellung Dienstabonnements enthalten.


## <a name="prerequisites"></a>Erforderliche Komponenten

**Azure-Konto**: benötigen Sie eine [Microsoft Azure](https://azure.microsoft.com/) -Konto. Sie können mit einer [kostenlosen Testversion](https://azure.microsoft.com/pricing/free-trial/)beginnen. [Erfahren Sie mehr](https://azure.microsoft.com/pricing/details/site-recovery/) über die Website Wiederherstellung Preise.


## <a name="step-1-create-a-vault"></a>Schritt 1: Erstellen einer Tresor
1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com)aus.
2. Klicken Sie auf Neu > Management > Sicherung und Wiederherstellung der Website (OMS). Alternativ können Sie auf Durchsuchen klicken > Wiederherstellung Services Tresor > hinzufügen.
3. Geben Sie im Feld **Name** einen Anzeigenamen ein, um den Tresor zu identifizieren. Wenn Sie mehr als ein Abonnement besitzen, wählen Sie einen davon.
4. Erstellen Sie eine neue Ressourcengruppe in **Ressourcengruppe** oder wählen Sie ein vorhandenes Layout aus. Geben Sie einen Bereich mit einer Azure führen Sie die erforderlichen Felder an. 
5.  Wählen Sie das geografische Region für den Tresor **Speicherort**. Zum Überprüfen der unterstützter Regions finden Sie unter [Azure Website Wiederherstellung Preise](https://azure.microsoft.com/pricing/details/site-recovery/).
5. Wenn Sie möchten, dass für den Schnellzugriff auf der Tresor aus dem Dashboard klicken Sie auf Pin zum Dashboard, und klicken Sie dann auf erstellen.
6. Der neue Tresor wird angezeigt, auf dem Dashboard > alle Ressourcen, und klicken Sie auf das Hauptfenster Wiederherstellung Services Depots Blade.

## <a name="step-2-configure-the-vault-and-download-inmage-scout-components"></a>Schritt 2: Konfigurieren des Tresors und InMage Scout Komponenten herunterladen
7. Der Wiederherstellung Dienste Depots Blade Ihrem Tresor auswählen und dann auf Einstellungen.
8. In den **Einstellungen** > **Erste Schritte** , klicken Sie auf **Website Wiederherstellung** > Schritt 1: **Vorbereiten Infrastruktur** > **Schutz Zielsetzung**.
9. **Schutz** Ziels Wiederherstellung Website wählen Sie aus, und wählen Sie Ja, mit VMware vSphere Hypervisor. Klicken Sie dann auf OK.
10. Klicken Sie im **Scout einrichten**auf Download zu Download InMage Scout 8.0.1 GA-Software und Registrierung-Taste. Die Setup-Dateien für alle erforderlichen Komponenten sind in der heruntergeladenen ZIP-Datei.


## <a name="step-3-install-component-updates"></a>Schritt 3: Komponentenupdates installieren

Informationen Sie zu den neuesten [Updates](#updates). Installieren Sie die Update-Dateien auf Servern in der folgenden Reihenfolge:

1. RX-Server, sofern vorhanden
2. Konfigurations-servers
3. Prozess-servers
3. Master Zielservern
4. vContinuum-servers
5. Quellserver (Windows und Linux-Server)

Installieren Sie die Updates wie folgt ein:

1. Laden Sie die ZIP-Datei [zu aktualisieren](https://aka.ms/asr-scout-update4) . Diese ZIP-Datei enthält die folgenden Dateien:

    - RX_8.0.4.0_GA_Update_4_8725872_16Sep16.TAR.gz
    - CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe
    - UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe
    - UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz
    - vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe
    - UA update4 Bits für RHEL5, OL5, OL6, SUSE 10, SUSE 11: UA_<Linux OS>_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz 
    
2. Die ZIP-Dateien zu extrahieren.<br>
3. **Für RX Server**: Kopieren **RX_8.0.4.0_GA_Update_4_8725872_16Sep16.tar.gz** auf dem Server RX und extrahieren Sie sie. Führen Sie im Ordner **extrahierten/Installieren**aus.<br>
4. **Für die Konfiguration Server-Prozess-Server**: Kopieren **CX_Windows_8.0.4.0_GA_Update_4_8725865_14Sep16.exe** des Konfigurationsserver und Prozess-Servers. Doppelklicken Sie auf diese Option, um Sie auszuführen.<br>
5. **Für die Windows-Gestaltungsvorlagen Ziel-Server**: um den einheitlichen-Agent aktualisieren möchten, kopieren Sie **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** auf dem Folienmaster Zielserver. Doppelklicken Sie darauf, um Sie auszuführen. Beachten Sie, dass der einheitliche Agent auch auf den ursprünglichen Server verfügbar ist. Sie sollten aber auch serverseitig Quelle installieren, wie weiter unten in dieser Liste angegeben ist.<br>
7. **Für den Server vContinuum**: **vCon_Windows_8.0.4.0_GA_Update_4_8921562_16Sep16.exe** auf dem Server vContinuum kopieren.  Stellen Sie sicher, dass Sie der Assistent vContinuum geschlossen haben. Doppelklicken Sie auf die Datei, um Sie auszuführen.<br>
8. **Für Linux master Ziel-Server**: um den einheitlichen Agent aktualisieren, **UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** auf dem Zielserver master kopieren und extrahieren Sie sie. Führen Sie im Ordner **extrahierten/Installieren**aus.<br>
9. **Datenquellen für Windows-Server**: Klicken Sie zum Aktualisieren des einheitlichen Agents **UA_Windows_8.0.4.0_GA_Update_4_9035261_27Sep16.exe** auf dem Quellserver zu kopieren. Doppelklicken Sie darauf, um Sie auszuführen.<br>
10. **Für Linux Quellserver**: Klicken Sie zum Aktualisieren des einheitlichen Agents entsprechenden Version UA-Datei auf dem Server Linux kopieren und extrahieren Sie sie. Führen Sie im Ordner **extrahierten/Installieren**aus.  Beispiel: RHEL 6,7 64-Bit-Server kopieren Sie **UA_RHEL6-64_8.0.4.0_GA_Update_4_9035261_26Sep16.tar.gz** auf dem Server und extrahieren Sie die Datei. Führen Sie im Ordner **extrahierten/Installieren**aus.

## <a name="step-4-set-up-replication"></a>Schritt 4: Einrichten von Replikation
1. Einrichten von Replikation zwischen der Quelle und Ziel VMware Websites.
2. Verwenden Sie für Leitfaden für die InMage Scout-Dokumentation, die heruntergeladen wird mit dem Produkt. Alternativ können Sie die Dokumentation wie folgt zugreifen:

    - [Freigeben von Notizen](https://aka.ms/asr-scout-release-notes)
    - [Kompatibilitätsmatrix](https://aka.ms/asr-scout-cm)
    - [Benutzerhandbuch](https://aka.ms/asr-scout-user-guide)
    - [RX-Benutzerhandbuch](https://aka.ms/asr-scout-rx-user-guide)
    - [Schnelle Installation guide](https://aka.ms/asr-scout-quick-install-guide)


## <a name="updates"></a>Updates

### <a name="azure-site-recovery-scout-801-update-4"></a>Azure Website Wiederherstellung Scout 8.0.1 Update 4
Scout Update 4 ist ein kumulatives Update. Es enthält alle Updates von update1 bis zum update3 und die folgenden neuen Updates und Erweiterungen.

**Neue Plattform-Unterstützung** 

- Unterstützung wurden für vCenter/vSphere 6.0, 6.1 und 6.2 hinzugefügt
- Unterstützung wurden für die folgenden Linux Betriebssysteme hinzugefügt
    - Red Hat Enterprise Linux (RHEL) 7.0, 7.1 und 7.2 
    - CentOS 7.0, 7.1 und 7.2
    - Red Hat Enterprise Linux (RHEL) 6.8
    - CentOS 6.8

>[AZURE.NOTE]
>
> RHEL/CentOS 7 64-Bit **InMage_UA_8.0.1.0_RHEL7-64_GA_06Oct2016_release.tar.gz** wird mit Basis Scout GA-Paket **InMage_Scout_Standard_8.0.1 GA.zip**verpackt. Herunterladen Sie Scout GA-Paket vom-Portal an, wie in [Schritt 1](site-recovery-vmware-to-vmware.md#Step 1: Create a vault)erwähnt.

**Fehler korrigiert und erweitert** 

- Verbesserte war(en) für folgende Linux OSes und Klonen, um zu verhindern, dass unerwünschte erneut synchronisieren Probleme behandeln.
    - Red Hat Enterprise Linux (RHEL) 6.x
    - Oracle-Linux (OL) 6.x
- Führen Sie unter Linux Zugriff auf Ordner, die Berechtigungen im Installationsverzeichnis einheitlichen Agent jetzt auf den lokalen Benutzer nur eingeschränkt sind.
- Klicken Sie auf Windows geladen Timeout Problem beim Ausgeben allgemeine verteilten Konsistenz-Lese-Zeichen auf stark verteilte Anwendungen wie SQL und Share Point Cluster.
- Hinzugefügten Log verwandte Fix in CX Basis Installationsprogramm.
- Windows-Master Ziel Basis Installer wird VMware vCLI 6.0 Downloadlink hinzugefügt.
- Weitere überprüft und Protokolle für Netzwerk Konfigurationen Änderungen hinzugefügt während der Failover und DR einen Drilldown.
- Einigen Fällen auch Aufbewahrungsrichtlinien Informationen werden nicht in die CX angegeben.  
- Für physischen Cluster ist Lautstärke Vorgang bis vContinuum-Assistenten erneut Größe weiß nicht, wenn Quelle Volume verkleinern passiert ist.
- Clusterschutz fehlgeschlagen mit Fehler "Fehler bei die Datenträgersignatur finden" Wenn Clusterdatenträger PRDM Datenträger ist.
- Cxps transport Server Absturz aufgrund von Ausnahme außerhalb des Bereichs. 
- Servernamen und die IP-Spalten werden Pushbenachrichtigungen installieren Seite des Assistenten vContinuum geändert werden kann.
- RX-API Erweiterungen
    - Bietet fünf spätesten verfügbaren allgemeine Konsistenz Punkte (nur sichergestellt, dass Kategorien).
    - Bietet Kapazität und Freigeben von Speicherplatz Details für alle geschützten Geräte.
    - Stellt Scout Treiberzustand auf Quellserver an. 
    
>[AZURE.NOTE] 
>
>- Basisversion **InMage_Scout_Standard_8.0.1_GA.zip** aktualisiert CX Basis Installer **InMage_CX_8.0.1.0_Windows_GA_26Feb2015_release.exe** und Windows-Master Ziel Basis Installer **InMage_Scout_vContinuum_MT_8.0.1.0_Windows_GA_26Feb2015_release.exe**jetzt. Verwenden Sie für alle neuen Installation neuer CX- und Windows-Master Ziel GA Bits aus.
>- Update 4 kann direkt angewendet werden, klicken Sie auf 8.0.1 GA
>- Die Konfigurationsserver und RX Updates können nicht rückgängig gemacht werden wieder, nachdem sie auf das System angewendet werden.

### <a name="azure-site-recovery-scout-801-update-3"></a>Azure Website Wiederherstellung Scout 8.0.1 Update 3
Update 3 umfasst die folgenden Updates und Erweiterungen:

- Die Konfigurationsserver und RX nicht auf der Website Wiederherstellung Tresor registrieren, wenn sie hinter dem Proxy befinden.
- Die Anzahl der Stunden, die Ziel der Wiederherstellung Punkt (RPO) nicht erfüllt ist, wird im Bericht Gesundheit nicht aktualisiert erste.
- Der Konfigurationsserver ist nicht mit RX synchronisiert werden, wenn die ESX Hardwaredetails oder Netzwerkdetails UTF-8 Zeichen enthalten sind.
- Windows Server 2008 R2 Domänencontroller fehlschlägt, nach der Wiederherstellung.
- Offlinesynchronisierung funktioniert nicht wie erwartet.
- Nach dem Failover virtuellen Computers (virtueller Computer) Replikation Paare Löschvorgang wird in der UI CX längere Zeit hängen, und Benutzer nicht das Failback durchführen oder Vorgang fortsetzen.
- Insgesamt werden Snapshot-Vorgänge, die durch den Auftrag Konsistenz fertig sind optimiert wurden zur Verringerung der Anwendung wie SQL-Clients getrennt.
- Die Leistung des Konsistenz-Tools (VACP.exe) wurde verbessert, indem Sie die arbeitsspeicherauslastung, die zum Erstellen von Momentaufnahmen auf Windows erforderlich ist.
- Die Pushbenachrichtigungen installieren stürzt ab, wenn das Kennwort größer als 16 Zeichen ist.
- vContinuum ist nicht aktivieren und für neue vCenter Anmeldeinformationen aufgefordert werden, wenn die Anmeldeinformationen nicht geändert werden.
- Unter Linux ist der Folienmaster Ziel-Cache-Manager (Cachemgr) nicht den Prozess-Server Herunterladen von Dateien aus der Replikation Paar begrenzungsebene ergibt.
- Wenn die physische Failover Cluster (MSCS) Datenträger Reihenfolge nicht auf alle Knoten identisch ist, wird Replikation nicht für einige Cluster Datenträger festgelegt.
<br/>Beachten Sie, dass der Cluster sein, um dieses Fix nutzen reprotected muss.  
- SMTP-Funktionalität funktioniert nicht wie erwartet, nachdem RX von Scout 7.1 auf Scout 8.0.1 aktualisiert wird.
- Weitere Stats wurden hinzugefügt, in der Log für den Vorgang zurücksetzen, um die Zeit zu verfolgen, die sie getroffen hat, um sie auszuführen.
- Unterstützung wurde für Linux-Betriebssysteme auf dem Server für die Datenquelle hinzugefügt:
    - Red Hat Enterprise Linux (RHEL) 6 Update 7
    - Aktualisieren von CentOS 6 7
- Die CX- und RX UI kann nun die Benachrichtigung für das Paar, anzeigen, der in Bitmap-Modus wechselt.
- Die folgenden Sicherheitsupdates wurden in RX hinzugefügt:

**Beschreibung des Problems**|**Implementierung Verfahren**
---|---
Autorisierung umgehen über Parameter manipuliert werden.|Eingeschränkter Zugriff auf Benutzer nicht verfügbar.
Websiteübergreifende Anforderung Fälschung|Implementiert des Konzepts Token-Seite, die für jede Seite zufällig generiert. <br/>Mit dieser Option werden Sie Folgendes angezeigt: <li> Es ist nur eine Anmeldung-Instanz für den gleichen Benutzer aus.</li><li>Aktualisieren der Seite funktioniert nicht – es zum Dashboard umgeleitet werden.</li>
Bösartige Datei hochladen|Eingeschränkte Dateien auf bestimmte Extensions. Zulässig sind Erweiterungen: 7Z-, Aiff, Asf, Avi, Bmp, Csv, Doc, Docx, FLA-, FLV-, Gif, Gz, Gzip, Jpeg, Jpg, Log, mid, Mov, MP3-, mp4, Mpc, Mpeg, mpg, ods Odt, Pdf, Png, ppt, Pptx, Pxd, unbedingt, Ram, Rar, Rm, Rmi, Rmvb, Rtf, Sdc, Sitd, Swf, Sxc, Sxw, Tar, Tgz, Tif, Tiff, Txt, Vsd, Wav, Wma, Wmv, Xls, Xlsx, Xml, und Zip.
Beständiger websiteübergreifende scripting | Eingabe Validierungen wird hinzugefügt.


>[AZURE.NOTE]
>
>-  Alle Website Wiederherstellung Aktualisierungen sind kumuliert. Update 3 enthält alle Updates von Update 1 und 2 aktualisieren. Update 3 kann direkt angewendet werden, klicken Sie auf 8.0.1 GA
>-  Die Konfigurationsserver und RX Updates können nicht rückgängig gemacht werden wieder, nachdem sie auf das System angewendet werden.

### <a name="azure-site-recovery-scout-801-update-2-update-03dec15"></a>Azure Website Wiederherstellung Scout 8.0.1 Update 2 (Update 03 Dez 15)

Klicken Sie unter Update 2 Problembehebungen:

- **Konfigurations-Server**: beheben für ein Problem, das das 31 Tage kostenlose Messfunktionen-Feature verhindert wie erwartet, wenn der Konfigurationsserver Website Wiederherstellung erfasst wurde.
- **Einheitliche Agent**: beheben für ein Problem in Update 1, die zur Folge des Updates nicht installiert auf dem Folienmaster Ziel-Server Wenn sie von Version 8.0 zu 8.0.1 durchgeführt wurde.


### <a name="azure-site-recovery-scout-801-update-1"></a>Azure Website Wiederherstellung Scout 8.0.1 Update 1

Update 1 umfasst die folgenden Updates und neuen Funktionen:

- 31 Tage lang kostenlosen Schutz pro Server-Instanz aus. So können Sie Funktionalität testen oder eine Prüfung des Konzepts einrichten.
    - Alle Vorgänge auf dem Server, einschließlich Failover und Failback, sind kostenlos für den ersten 31 Tagen ab dem Zeitpunkt an, die ein Server zuerst mit Website Wiederherstellung Scout geschützt ist.
    - Ab dem 32nd Tag oder höher wird jede geschützter Server mit der standard-Instanz Abschlag (Disagio) Azure Website Wiederherstellung Schutz auf einer Website im Besitz des Kunden berechnet.
    - Zu einem beliebigen Zeitpunkt ist die Anzahl der geschützten Servern, die zurzeit geladen werden auf der Seite Dashboard im Tresor Azure Website Wiederherstellung zur Verfügung.
- Unterstützung hinzugefügt 5,5 Update 2 für vSphere Line Interface (vCLI) aus.
- Unterstützung für Linux-Betriebssysteme auf dem Quellserver hinzugefügt:
    - Aktualisieren RHEL 6 von 6
    - RHEL 5 aktualisieren 11
    - CentOS 6 Update 6
    - CentOS 5 Update 11
- Fehler korrigiert mit folgenden Problemstellungen:
    - Tresor Registrierung schlägt fehl, für die Konfigurationsserver oder RX-Server.
    - Cluster Datenmengen nicht angezeigt, wie erwartet, wenn die gruppierten virtuellen Computern reprotected werden, wenn sie fortsetzen.
    - Failback schlägt fehl, wenn der Gestaltungsvorlage Ziel-Server auf einem anderen ESXi Server aus der lokalen Herstellung virtuellen Computern gehostet wird.
    - Berechtigungen für die Konfiguration werden geändert, beim upgrade auf 8.0.1, was Schutz und Vorgänge beeinflusst.
    - Der Schwellenwert für die erneute Synchronisierung wird nicht erzwungen, wie erwartet, wodurch inkonsistenten Replikationsverhalten.
    - Die RPO Einstellungen werden nicht ordnungsgemäß in der Konfiguration Server-Oberfläche angezeigt. Der Wert nicht komprimierte Daten zeigt falsch den komprimierten Wert an.
    -  Der Vorgang zum Entfernen nicht wie erwartet im Assistenten vContinuum löschen und Replikation über die Konfiguration Server-Oberfläche werden nicht gelöscht.
    -  Im Assistenten vContinuum ist der Datenträger automatisch deaktiviert, wenn Sie die **Details** in der Ansicht Datenträger während Schutz von MSCS virtuellen Computern klicken.
    - Während der physischen zu virtuellen Szenario (P2V) werden nicht erforderlichen HP-Dienste, wie z. B. CIMnotify und CqMgHost, auf manuell in virtuellen Computern Wiederherstellung verschoben. Dadurch zusätzliche Startzeit.
    - Linux virtuellen Computern Schutz schlägt fehl, wenn mehr als 26 Laufwerke auf dem Folienmaster Ziel-Server vorhanden sind.

## <a name="next-steps"></a>Nächste Schritte

Veröffentlichen Sie alle Fragen, die im [Forum Azure Wiederherstellung Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)sind.
