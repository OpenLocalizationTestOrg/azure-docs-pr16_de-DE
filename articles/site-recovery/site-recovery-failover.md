<properties
    pageTitle="Failover in Website Wiederherstellung | Microsoft Azure" 
    description="Azure Website Wiederherstellung koordiniert die Replikation, Failover- und von virtuellen Computern und physische Server. Informationen Sie zu Failover Azure oder einem sekundären Datencenter." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="failover-in-site-recovery"></a>Failover in Website Wiederherstellung

Der Dienst Azure Website Wiederherstellung beiträgt zu Ihrer Strategie Business Continuity- und Disaster Wiederherstellung (BCDR) durch Replikation, Failover und Wiederherstellung von virtuellen Computern und physischen Servern orchestriert. Maschinen können Azure oder einem sekundären lokalen Data Center repliziert werden. Für einen schnellen Überblick lesen [Neuigkeiten Azure Website Wiederherstellung?](site-recovery-overview.md)

## <a name="overview"></a>(Übersicht)

Dieser Artikel enthält Informationen und Anweisungen zum Wiederherstellen (weiß nicht mehr, ansonsten wieder) virtuellen Computern und physischen Servern, die mit der Website Wiederherstellung geschützt sind. 

Posten Sie Kommentare oder Fragen am Ende dieses Artikels oder im [Azure Wiederherstellung Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="types-of-failover"></a>Typen von failover

Nachdem Schutz für virtuellen Computern und physischen Servern aktiviert ist, und sie sind repliziert können Sie Failovers ausgeführt wird, je nach Bedarf ein. Website Wiederherstellung unterstützt verschiedene Arten von Failover.

**Failover** | **Wann ausgeführt** | **Details** | **Prozess**
---|---|---|---
**Test-failover** | Führen Sie aus, um Ihre Strategie für die Replikation überprüfen oder einem Disaster Wiederherstellung Drilldown ausführen | Keine Daten verloren gehen oder Ausfallzeiten.<br/><br/>Keine Auswirkung auf die Replikation<br/><br/>Keine Auswirkung auf Ihre Umgebung Herstellung | Starten Sie das failover<br/><br/>Geben Sie an, wie nach Failover Testcomputern mit Netzwerken verbunden werden soll<br/><br/>Überwachen des Fortschritts der Registerkarte **Aufträge** . Testcomputern erstellt werden, und beginnen Sie in der zweiten Standort<br/><br/>Azure - Verbinden mit dem Computer im Azure-Portal<br/><br/>Sekundäre Website - Zugriff auf dem Computer in der gleichen Host und Cloud<br/><br/>Testen des und Aufräumen Sie automatisch Test Failovereinstellungen.
**Geplantes failover** | Führen Sie aus, um die Einhaltung von Vorschriften zu erfüllen<br/><br/>Führen Sie für die geplante Wartung<br/><br/>Führen Sie aus, um ein Fehler auftreten, über die Daten, die Auslastung für bekannte Ausfall – beispielsweise ein Fehler erwarteten Power oder schwere Wetterberichte ausgeführt<br/><br/>Failbacks nach Failover von primär-zum Ausführen | Keine Daten verloren<br/><br/>Ausfallzeiten fallen während der Zeit, die zum Beenden der virtuellen Computern auf dem primären und aufzurufen, klicken Sie auf den zweiten Standort benötigt.<br/><br/>Virtuellen Computern sind Einfluss wie Zielcomputern wird Quelle Autos nach Failover. | Starten Sie das failover<br/><br/>Überwachen des Fortschritts der Registerkarte **Aufträge** . Quelle Autos fahren<br/><br/>Replikat Autos sekundäre Speicherort starten<br/><br/>Azure - Verbinden mit dem Computer Replikat Azure-Portal<br/><br/>Sekundäre Website - Zugriff auf dem Computer, auf dem gleichen Host und in der gleichen Cloud<br/><br/>Das Failover Commit
**Ungeplanten failover** | Führen Sie diese Art von Failover reaktivieren Weise aus, wenn Sie ein primärer Standort nicht mehr verfügbar ist, da ein unerwarteter Vorfall, wie etwa ein Power Ausfall oder Virus Angriffen <br/><br/> Sie können Ausführen einer ungeplanten Failover durchgeführt werden kann, selbst wenn primärer Standort nicht verfügbar ist. | Abhängige Replikation Häufigkeit Einstellungen Datenverlust<br/><br/>Daten werden auf dem neuesten Stand gemäß dem letzten, die Sie synchronisiert wurde | Starten Sie das failover<br/><br/>Überwachen des Fortschritts der Registerkarte **Aufträge** . Optional versuchen Sie, fahren Sie virtuellen Computern und synchronisieren Sie die neusten Daten<br/><br/>Replikat Autos sekundäre Speicherort starten<br/><br/>Azure - Verbinden mit dem Computer Replikat Azure-Portal<br/><br/>Sekundären Zugriff auf dem Computer, auf dem gleichen Host und in der gleichen cloud<br/><br/>Das Failover Commit


Die Typen von Failovers, die unterstützt werden, abhängig von Ihrem Bereitstellungsszenario ab.

**Failover Richtung** | **Test-failover** | **Geplantes failover** | **Ungeplanten failover**
---|---|---|---
Primäre VMM Website sekundäre VMM-Website | Unterstützt | Unterstützt | Unterstützt 
Sekundäre VMM Standort primären VMM-Website | Unterstützt | Unterstützt | Unterstützt
Cloud Cloud (einzelne VMM-Server) |  Unterstützt | Unterstützt | Unterstützt
VMM Website Azure | Unterstützt | Unterstützt | Unterstützt 
Azure VMM-Website | Nicht unterstützt werden | Unterstützt | Nicht unterstützt werden 
Website Azure Hyper-V | Unterstützt | Unterstützt | Unterstützt
Azure zu Hyper-V-Website | Nicht unterstützt werden | Unterstützt | Nicht unterstützt werden
VMware Website Azure | Unterstützt (Erweiterte Szenario)<br/><br/> Nicht unterstützte (legacy-Szenario) |Nicht unterstützt werden | Unterstützt
Physische Server in Azure | Unterstützt (Erweiterte Szenario)<br/><br/> Nicht unterstützte (legacy-Szenario) | Nicht unterstützt werden | Unterstützt

## <a name="failover-and-failback"></a>Failover und failback

Sie nicht über virtuellen Computern zu einer Website sekundäre lokalen oder Azure, abhängig von der Bereitstellung. Ein Computer, der Ausfall zu Azure wird als eine Azure-virtuellen Computern erstellt. Sie können über eines einzelnen virtuellen Computers oder physischen Server oder einen Wiederherstellungsplan fehl. Wiederherstellung Pläne besteht aus einem oder mehr sortierte Gruppen, die geschützten virtuellen Computern oder physische Server enthalten. Sie sind verwendet, um Failover Koordinieren von mehreren Computern, die über, entsprechend der Gruppe fehlschlägt sie sich befinden. [Weitere Informationen finden Sie](site-recovery-create-recovery-plans.md) über Wiederherstellung Pläne. 

Nach dem Failover beendet ist und Ihrem Computer einsatzbereit an einem sekundären Standort sind Beachten Sie Folgendes:

- Wenn Sie über in Azure, fehlgeschlagen ist, nachdem die Computer Failover geschützten oder repliziert primäre oder sekundäre Speicherort nicht zur Verfügung. In Azure sind die virtuellen Computer im Speicher Geo repliziert gespeichert, die Stabilität, aber nicht auf die Replikation bereitstellt.
- Wenn Sie ein ungeplantes Failover zu einem sekundären Standort haben, nachdem die Computer Failover sekundäre Speicherort geschützt werden nicht oder repliziert.
- Wenn Sie nach den Failover Computern sekundäre Speicherort ein geplantes Failover zu einem sekundären Standort und konnten geschützt werden.
 

### <a name="failback-from-secondary-site"></a>Failback vom sekundären

Failback von einem sekundären Standort in einen primären verwendet das gleiche Verfahren als Failover vom primären auf sekundären. Nachdem das Failover von primären auf sekundären Datacenter zugesicherte und abgeschlossen ist, können Sie reverse Replikation initiieren, wenn Ihre primäre Site verfügbar ist. Umgekehrter Replikation initiiert Replikation zwischen den sekundären Standort und die primäre Delta Daten zu synchronisieren. Umgekehrter Replikation bringt den virtuellen Computern in einer geschützten Status jedoch sekundäre Datencenter ist nach wie vor der aktive Ort. Akzeptieren, um die primäre Website in der aktive Ort tätigen rufen Sie ein geplantes Failover vom sekundären zum primären, gefolgt von einem anderen reverse Replikation.

### <a name="failback-from-azure"></a>Failback aus Azure

Wenn Sie über Azure durchgeführt haben sind Ihre virtuellen Computer mithilfe der Features zur Azure Stabilität für virtuellen Computern geschützt. Führen Sie zum Umwandeln der ursprünglichen primären Standort in der aktive Ort ein geplantes Failover aus. Sie können zurück an den ursprünglichen Ort oder an einem anderen Speicherort fehl, wenn es sich bei Ihrer ursprüngliche Website nicht verfügbar ist. Rufen Sie um Replikation nach Failback zum primären Standort erneut zu starten Sie eine reverse Replikation aus.

### <a name="failover-considerations"></a>Failover Aspekte

- **IP-Adresse nach Failover**– werden standardmäßig eine fehlgeschlagene über einen Computer, eine andere IP-Adresse als der Quellcomputer müssen. Wenn Sie die gleiche IP-Adresse finden Sie unter beibehalten möchten: 
    - **Sekundären**– Wenn Sie sind weiß nicht über an einem sekundären Standort und eine IP-Adresse [Lesen](http://blogs.technet.com/b/scvmm/archive/2014/04/04/retaining-ip-address-after-failover-using-hyper-v-recovery-manager.aspx) in diesem Artikel beibehalten werden sollen. Beachten Sie, dass Sie eine öffentliche IP-Adresse beibehalten können, wenn Ihr Internetdienstanbieter es unterstützt.
    - **Azure**– Wenn Sie über in Azure weiß nicht sind können Sie angeben, dass die IP-Adresse, die Sie auf der Registerkarte **Konfigurieren** der Eigenschaften von virtuellen Computern zuweisen möchten. Sie können keine öffentliche IP-Adresse nach Failover auf Azure beibehalten. Sie können nicht RFC 1918 - Adresse Leerzeichen beibehalten, die als interne Adressen verwendet werden.
- **Teilweise Failover**– Wenn Sie über einen Teil einer Website treten anstatt Beachten Sie, dass eine gesamte Website: 
    - **Sekundären**– Wenn Sie nicht über einen Teil einer primären Website an einem sekundären Standort und wieder in der primäre Standort eine Verbindung herstellen möchten, verwenden Sie eine Standort-zu-Standort VPN-Verbindung über eine Verbindung fehlgeschlagene Applikationen auf den sekundären Standort zu Infrastrukturkomponenten auf dem primären Standort. Wenn Sie über ein ganzes Subnetz schlägt fehl, können Sie die virtuellen Computern IP-Adresse beibehalten. Wenn Sie über eine teilweise Subnetz nicht kann nicht die virtuellen Computern IP-Adresse beibehalten werden, da Subnetze zwischen verschiedenen Websites geteilt werden können.
    - **Azure**– Wenn Sie über eine Website teilweise Azure fehlschlagen und wieder in der primäre Standort eine Verbindung herstellen möchten, können ein Standort-zu-Standort VPN über eine Verbindung eine fehlgeschlagene Anwendung in Azure Infrastrukturkomponenten auf dem primären Standort. Beachten Sie, dass der gesamte Subnetz fehlschlägt, über die Sie die virtuellen Computern IP-Adresse beibehalten werden können. Wenn Sie über eine teilweise Subnetz nicht kann nicht die virtuellen Computern IP-Adresse beibehalten werden, da Subnetze zwischen verschiedenen Websites geteilt werden können.
 
- **Laufwerkbuchstaben**– Wenn Sie den Buchstaben des Laufwerks auf virtuellen Computern nach Failover beibehalten möchten legen Sie die SAN-Richtlinie für den virtuellen Computer auf **auf**. Virtuellen Computern Datenträger geschaltet automatisch online. [Weitere Informationen finden](https://technet.microsoft.com/library/gg252636.aspx).
- **Routing-Client-Anfragen**– Website Wiederherstellung funktioniert mit Azure Datenverkehr Manager die Weiterleitung von Client-Anfragen an Ihrer Anwendung nach Failover.




## <a name="run-a-test-failover"></a>Ausführen eines Failovers testen

Beim Ausführen eines Test-Failovers werden Sie aufgefordert, deren Einstellungen im Netzwerk für Replikat Testcomputern auszuwählen. Sie haben eine Reihe von Optionen.  

**Testen Sie die Option failover** | **Beschreibung** | **Failover überprüfen** | **Details**
---|---|---|---
**Treten über in Azure – ohne Netzwerk** | Wählen Sie ein Ziel Azure Netzwerk nicht | Failover Prüfungen, die virtuellen Computern Testen in Azure ordnungsgemäß gestartet wird | Alle Test-virtuellen Computern in einem Plan wiederherstellen werden in einem einzelnen Clouddienst hinzugefügt und können miteinander verbinden<br/><br/>Maschinen werden nicht nach einem Failover mit einer Azure-Netzwerk verbunden.<br/><br/>Benutzer können mit den Testcomputern mit einer öffentlichen IP-Adresse verbinden.
**Treten über in Azure – mit dem Netzwerk** | Wählen Sie ein Ziel Azure Netzwerk | Failover überprüft, ob Testcomputern mit dem Netzwerk verbunden sind | Erstellen Sie ein Azure-Netzwerk, die von Ihrem Netzwerk Azure Herstellung isoliert sowie die Infrastruktur für die repliziert virtuellen Computern erwartungsgemäß einrichten.<br/><br/>Das Subnetz des virtuellen Testcomputers basiert auf Subnetz, an dem der Fehler beim über virtuellen Computern zum Herstellen einer Verbindung mit bei einem geplanten oder ungeplanten Failover erwartet.
**Möglicher Fehler über zu einer sekundären VMM Website – ohne Netzwerk** | Wählen Sie ein virtueller Computer-Netzwerk nicht | Failover überprüft, dass Testcomputern erstellt werden.<br/><br/>Die Test-virtuellen Computern wird auf demselben Host als Host erstellt für die Replikat virtuellen Computers vorhanden sind. Es wird nicht in der Cloud eingetragen in dem Replikat virtuellen Computers gespeichert ist. | <p>Der Fehler beim über einen Computer wird nicht mit einem beliebigen Netzwerk verbunden sein.<br/><br/>Der Computer kann mit einem virtuellen Computer-Netzwerk verbunden werden, nachdem es erstellt wurde
**Möglicher Fehler über zu einer sekundären VMM Website – mit dem Netzwerk** | Wählen Sie ein vorhandenes virtueller Computer-Netzwerk ein | Failover überprüft, dass die Erstellung von virtuellen Computern | Die Test-virtuellen Computern wird auf demselben Host als Host erstellt für die Replikat virtuellen Computers vorhanden sind. Es wird nicht in der Cloud eingetragen in dem Replikat virtuellen Computers gespeichert ist.<br/><br/>Erstellen Sie ein virtueller Computer-Netzwerk, das isoliert wurde aus Ihrem Netzwerk Herstellung<br/><br/>Wenn Sie ein VLAN-basiertes Netzwerk verwenden empfehlen wir, erstellen Sie ein separates logisches Netzwerk (nicht in der Herstellung verwendet) in VMM für diesen Zweck. Diese logische Netzwerk dient zum Erstellen von virtuellen Computer Netzwerke zum Zweck der Test Failover.<br/><br/>Das logische Netzwerk sollten Sie mindestens eines der Netzwerkadapter aller Hyper-V Server hosten virtueller Computer zugeordnet werden.<br/><br/>Für VLAN logische Netzwerke sollte der Netzwerk-Websites, die Sie im logischen Netzwerk hinzufügen isoliert werden.<br/><br/>Wenn Sie ein Windows-Netzwerk-Virtualisierung – basierte logisches Netzwerk verwenden, erstellt Azure Website Wiederherstellung automatisch isoliert virtueller Computer Netzwerke aus.
**Möglicher Fehler über zu einer sekundären VMM Website – erstellen Sie ein Netzwerk** | Eine temporäre Testnetzwerk wird automatisch basierend auf der Einstellung erstellt angegebenen **Logischen Netzwerk** und der zugehörigen Netzwerk-Websites | Failover überprüft, dass die Erstellung von virtuellen Computern | Verwenden Sie diese Option aus, wenn der Wiederherstellungsplan mehr als ein virtueller Computer Netzwerk verwendet. Wenn Sie Windows-Netzwerk-Virtualisierung Netzwerken verwenden, kann diese Option automatisch virtueller Computer Netzwerken mit denselben Einstellungen (Subnetze und IP-Adresspools) im Netzwerk des virtuellen Computers Replikat erstellen. Diese Netzwerke virtueller Computer werden automatisch bereinigt nach Abschluss des Failovers testen.</p><p>Die Test-virtuellen Computern wird auf demselben Host als Host erstellt für die Replikat virtuellen Computers vorhanden sind. Es wird nicht in der Cloud eingetragen in dem Replikat virtuellen Computers gespeichert ist.

>[AZURE.NOTE] Die IP-Adresse eines virtuellen Computers während des Failovers Test angegeben ist identisch die IP-Adresse, die diese wann empfangen würde geplanten oder ungeplanten Failover (vorausgesetzt, dass die IP-Adresse im Netzwerk testen Failover verfügbar ist. ausführen Wenn die IP-Adresse im Netzwerk testen Failover verfügbar ist, virtuellen Computern eine andere IP-Adresse im Netzwerk Failover Test verfügbaren erhalten.



### <a name="run-a-test-failover-from-on-premises-to-azure"></a>Führen Sie einen Test-Failover aus lokalen in Azure

Dieses Verfahren beschreibt, wie einen Test-Failover für einen Wiederherstellungsplan ausführen. Alternativ können Sie das Failover für einen einzelnen Computer auf der Registerkarte **virtuellen Computern** ausführen.

1. Wählen Sie **Pläne Wiederherstellung** > *Recoveryplan_name*. Klicken Sie auf **Failover** > **Failover testen**.
2. Geben Sie auf der Seite **Bestätigen Test Failover** wie nach Failover Replikat Autos mit ein Azure-Netzwerk verbunden werden soll.
3. Wenn Sie sind weiß nicht über in Azure und für die Cloud-Verschlüsselung aktiviert ist, wählen Sie in **Verschlüsselungsschlüssels** das Zertifikat, das ausgestellt wurde, wenn Sie die Verschlüsselung der Daten während der Installation Anbieter aktiviert. 
4. Überwachen des Fortschritts von Failover auf der Registerkarte **Aufträge** . Sie sollten den Testcomputer Replikat Azure-Portal zu sehen.
5. Sie können Replikat Autos in Azure von Ihrem lokalen Standort initiieren eine RDP-Verbindung zu des virtuellen Computers zugreifen. Port 3389 müssen auf den Endpunkt des virtuellen Computers geöffnet werden.
5. Sobald Sie fertig sind, wenn das Failover **abgeschlossen testen** einer Phase erreicht, klicken Sie auf **Test abgeschlossen** auf Fertig stellen.
5. In **Notizen** aufzeichnen und Speichern einer beliebigen Beobachtungen des Failovers Test zugeordnet.
8. Klicken Sie auf **das Failover Test abgeschlossen ist** , um die testumgebung automatisch zu bereinigen. Nach Abschluss dieses wird das Failover testen den C**Fertig gestellt** Status angezeigt.

> [AZURE.NOTE] Wenn weiterhin ein Test-Failover für mehr als zwei Wochen wird erzwungen noch abgeschlossen werden müssen. Alle Elemente oder virtuellen Computern erstellt automatisch während des Failovers Test werden gelöscht.
  

### <a name="run-a-test-failover-from-a-primary-on-premises-site-to-a-secondary-on-premises-site"></a>Ausführen eines Failovers Testen von einer primären lokalen Website zu einer Website sekundäre lokal

Sie müssen eine Reihe von Dingen zum Ausführen eines Test Failovers, einschließlich einer Kopie der Domänencontroller und Platzieren von Test DHCP- und DNS-Servern in Ihrer testumgebung. Dies auf verschiedene Weise kann erfolgen:

- Wenn Sie einen Test System durch Verwendung von einem vorhandenen Netzwerk ausführen möchten, bereiten Sie Active Directory, DHCP und DNS in diesem Netzwerk.
- Wenn Sie einen Test System durch Verwendung von die Option virtueller Computer Netzwerke automatisch erstellen ausführen möchten, fügen Sie in der Wiederherstellungsplan, den Sie für den Test Failover verwenden, und klicken Sie dann die Infrastrukturressourcen im Netzwerk automatisch erstellte vor dem Ausführen des Failovers Test hinzufügen manuelle Schritt vor Gruppe-1 hinzu.

#### <a name="things-to-note"></a>Dinge zu beachten

- Wenn an einem sekundären Standort repliziert, der Typ des Netzwerks des Computers Replikat nicht muss entsprechen den Typ des logischen Netzwerks für die Failover-Test verwendet, aber einige Kombinationen funktioniert möglicherweise nicht. Wenn das Replikat DHCP- und VLAN-basierten Isolation verwendet, benötigen nicht im Netzwerk virtueller Computer für das Replikat einen statischen IP-Adresspool. So verwenden Windows-Netzwerk-Virtualisierung Failoververarbeitung Test funktionieren würde, da keine Adresspools verfügbar sind. Darüber hinaus funktionieren Test Failover nicht, wenn das Replikat Netzwerk keine Isolation und das Testnetzwerk Windows-Netzwerk-Virtualisierung ist. Dies ist, da das Netzwerk keine Isolation die zum Erstellen von einem Windows-Netzwerk-Virtualisierung Netzwerk erforderlich Subnetze aufweist.
- Die Möglichkeit, in welche, der Replikat virtuellen Computern mit verbunden sind, zugeordneten virtuellen Computer Netzwerken nach Failover hängt davon ab, wie das Netzwerk virtueller Computer in der VMM-Verwaltungskonsole konfiguriert ist:
    - **Virtueller Computer Netzwerk ohne Isolation oder VLAN Isolation konfiguriert**– Wenn DHCP für das Netzwerk virtueller Computer definiert ist, Replikat virtuellen Computers wird mit der VLAN-ID, die mit den Einstellungen, die angegeben werden für die Netzwerk-Website in der zugeordneten logischen Netzwerk verbunden sein. Des virtuellen Computers, dessen IP-Adresse aus den verfügbaren DHCP-Server erhalten. Sie benötigen keines statischen IP-Adresse Ressourcenpool für das Ziel virtueller Computer Netzwerk definiert. Wenn eine statische IP-Adresse Ressourcenpool für das Netzwerk virtueller Computer verwendet wird wird Replikat virtuellen Computers mit der VLAN-ID, die mit den Einstellungen, die angegeben werden für die Netzwerk-Website in der zugeordneten logischen Netzwerk verbunden sein. Des virtuellen Computers erhalten die IP-Adresse aus dem Pool für das Netzwerk virtueller Computer definiert. Wenn eine statische IP-Adresse Ressourcenpool Netzwerk virtueller Computer Ziel nicht definiert ist, tritt ein Fehler IP-Adresse Zuteilung. Die IP-Adresse Ressourcenpool sollte auf Quell- und Zielliste VMM-Servern erstellt werden, das für Schutz und Wiederherstellung verwendet werden.
    - **Virtueller Computer Netzwerk mit Windows Netzwerk-Virtualisierung**– Wenn ein Netzwerk virtueller Computer mit dieser Einstellung ein statischer Pool werden, für das Ziel virtueller Computer Netzwerk definiert sollte konfiguriert ist, unabhängig davon, ob das Quelle virtueller Computer Netzwerk konfiguriert ist mit DHCP- oder einer statischen IP-Adresse Ressourcenpool. Wenn Sie DHCP definieren, wird der Ziel VMM-Server dienen als DHCP-Server und bieten eine IP-Adresse aus dem Pool, der definiert ist für das Ziel virtueller Computer-Netzwerk. Verwenden eines statischen Pool von IP-Adressen für die Quellserver definiert ist, wird in der Zielliste VMM-Server eine IP-Adresse aus dem Pool zugewiesen werden. In beiden Fällen wird IP-Adresse Zuteilung fehl, wenn eine statische IP-Adresse Ressourcenpool nicht definiert ist.

#### <a name="run-test"></a>Test ausführen

Dieses Verfahren beschreibt, wie einen Test-Failover für einen Wiederherstellungsplan ausführen. Alternativ können Sie das Failover für einen einzelnen virtuellen Computern oder physische Server auf der Registerkarte **virtuellen Computern** ausführen.

1. Wählen Sie **Pläne Wiederherstellung** > *Recoveryplan_name*. Klicken Sie auf **Failover** > **Failover testen**.
2. Geben Sie auf der Seite **Bestätigen testen Failover** wie virtuellen Computern nach dem Test Failover mit Netzwerken verbunden sein sollen.
3. Überwachen des Fortschritts von Failover auf der Registerkarte **Aufträge** . Wenn das Failover** abgeschlossen testen** einer Phase erreicht, klicken Sie auf **Vollständige testen** , um das Test-Failover fertig zu stellen.
4. Klicken Sie auf **Notizen** aufzeichnen und Speichern einer beliebigen Beobachtungen des Failovers Test zugeordnet.
4. Stellen Sie sicher, dass die virtuellen Computer erfolgreich starten, nachdem er abgeschlossen ist.
5. Nach dem Überprüfen, dass die virtuellen Computer erfolgreich starten, schließen Sie das Failover testen, um die isoliert Umgebung zu bereinigen. Wenn Sie Automatisches Erstellen von virtuellen Computer Netzwerken ausgewählt haben, löscht Aufräumen alle Test-virtuellen Computern und Netzwerken zu testen.

> [AZURE.NOTE] Wenn weiterhin ein Test-Failover für mehr als zwei Wochen wird erzwungen noch abgeschlossen werden müssen. Alle Elemente oder virtuellen Computern erstellt automatisch während des Failovers Test werden gelöscht.


#### <a name="prepare-dhcp"></a>Vorbereiten der DHCP

Wenn die virtuellen Computern beteiligt Test Failover DHCP verwenden, Test DHCP-Server erstellt werden soll, in dem isoliert Netzwerk, das zum Zweck der Test Failover erstellt wird.


### <a name="prepare-active-directory"></a>Vorbereiten von Active Directory
Wenn einen Test-Failover für Testen der Anwendung ausführen möchten, benötigen Sie eine Kopie der Herstellung Active Directory-Umgebung in Ihrer testumgebung. Navigieren Sie in Abschnitt [Failover Aspekte für active Directory testen](site-recovery-active-directory.md#considerations-for-test-failover) , weitere Details. 


### <a name="prepare-dns"></a>Vorbereiten der DNS-Einträge

Bereiten Sie einen DNS-Server das Failover Test wie folgt vor:

- **DHCP**– Wenn virtuellen Computern DHCP verwenden, sollten die IP-Adresse des DNS-Tests auf den Test DHCP-Server aktualisiert werden. Wenn Sie einen Netzwerktyp von Windows-Netzwerk-Virtualisierung verwenden, fungiert VMM-Server als DHCP-Server. Daher sollten die IP-Adresse des DNS im Netzwerk Failover Test aktualisiert werden. In diesem Fall wird der virtuellen Computern sich an den entsprechenden DNS-Server registrieren.
- **Statische Adresse**– Wenn virtuellen Computern eine statische IP-Adresse verwenden, sollten die IP-Adresse des Servers DNS-Test in Test Failover Netzwerk aktualisiert werden. Möglicherweise müssen DNS für die IP-Adresse der Test virtuellen Computer zu aktualisieren. Sie können das folgende Beispielskript für diesen Zweck: 

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-planned-failover-primary-to-secondary"></a>Ausführen eines geplanten Failovers (primären sekundären)

 Dieses Verfahren beschreibt, wie zu ein geplantes Failover für einen Wiederherstellungsplan ausführen. Alternativ können Sie das Failover für einen einzelnen virtuellen Computer auf der Registerkarte **virtuellen Computern** ausführen.

1. Stellen Sie sicher, dass alle virtuellen Computer, über ein Fehler auftreten soll, die erste Replikation abgeschlossen haben, bevor Sie beginnen.
2. Wählen Sie **Pläne Wiederherstellung** > *Recoveryplan_name*. Klicken Sie auf **Failover** > **Failover geplant**. 
3. Wählen Sie auf der Seite **Bestätigen geplanter Failover **die Quell- und Zielwebsites-Speicherorte aus. Beachten Sie die Richtung Failover.

    - Wenn der vorherigen Failovers gearbeitet haben, wie erwartet, und alle Server virtuellen Computern befinden sich die Quell- oder Zielliste Speicherort, sind die Details des Failover Richtung nur Informationen. 
    - Virtuellen Computern auf Quell- und Zielliste Speicherorte aktiviert sind, wird die **Richtung ändern** -Schaltfläche angezeigt. Verwenden Sie diese Schaltfläche, um zu ändern, und geben Sie die Richtung, in der das Failover durchgeführt werden soll.

5. Wenn Sie sind weiß nicht über in Azure und für die Cloud-Verschlüsselung aktiviert ist, wählen Sie in **Verschlüsselungsschlüssels** das Zertifikat, das ausgestellt wurde, wenn Sie die Verschlüsselung der Daten während der Installation auf dem Server VMM Anbieter aktiviert. 
6. Bei ein geplantes Failover beginnt ist der erste Schritt zum Beenden der virtuellen Computern, um sicherzustellen, dass keine Daten verloren. Sie können den Fortschritt Failover auf der Registerkarte **Aufträge** folgen. Falls ein Fehler auftritt, in dem Failover (entweder auf einem virtuellen Computer oder in einem Skript, die in den Wiederherstellungsplan enthalten ist), das geplante Failover eines Plans Wiederherstellung beendet. Sie können das Failover erneut starten.
8. Nachdem Replikat virtuellen Computern erstellt werden können diese in ein Commit ausstehen und ein. Klicken Sie auf **Commit ausführen** , um das Failover abzuschließen. 
9. Führen Sie nach der Replikation ist Starten von virtuellen Computern vom sekundären Standort. 

## <a name="run-an-unplanned-failover"></a>Ausführen eines ungeplanten Failovers

Dieses Verfahren beschreibt das ein ungeplantes Failover für einen Wiederherstellungsplan ausführen. Alternativ können Sie das Failover für einen einzelnen virtuellen Computern oder physische Server auf der Registerkarte **virtuellen Computern** ausführen.

1. Wählen Sie **Pläne Wiederherstellung** > *Recoveryplan_name*. Klicken Sie auf **Failover** > **ungeplanten Failover**. 
3. Wählen Sie auf der Seite **Bestätigen ungeplanten Failover **die Quell- und Zielwebsites-Speicherorte aus. Beachten Sie die Richtung Failover.

    - Wenn der vorherigen Failovers gearbeitet haben, wie erwartet, und alle Server virtuellen Computern befinden sich die Quell- oder Zielliste Speicherort, sind die Details des Failover Richtung nur Informationen. 
    - Virtuellen Computern auf Quell- und Zielliste Speicherorte aktiviert sind, wird die **Richtung ändern** -Schaltfläche angezeigt. Verwenden Sie diese Schaltfläche, um zu ändern, und geben Sie die Richtung, in der das Failover durchgeführt werden soll.

4. Wenn Sie sind weiß nicht über in Azure und für die Cloud-Verschlüsselung aktiviert ist, wählen Sie in **Verschlüsselungsschlüssels** das Zertifikat, das ausgestellt wurde, wenn Sie die Verschlüsselung der Daten während der Installation auf dem Server VMM Anbieter aktiviert. 
5. Wählen Sie **fahren virtuellen Computer und die neuesten Daten synchronisieren** angegeben wird, dass die Website Wiederherstellung versuchen, fahren Sie den geschützten virtuellen Computern und die Daten zu synchronisieren, damit die neueste Version von Daten über einen Fehler beim sein wird. Wenn Sie diese Option nicht auswählen oder den keine Fehler aufgetreten werden das Failover aus den neuesten verfügbaren Wiederherstellungspunkt des virtuellen Computers.
6. Sie können den Fortschritt Failover auf der Registerkarte **Aufträge** folgen. Beachten Sie, dass auch wenn während einer ungeplanten Failover Fehler auftreten, von der Wiederherstellungsplan ausgeführt wird, bis er abgeschlossen ist.
7. Nach dem Failover befinden sich die virtuellen Computer in Zustand **Ausstehend abzuschließen** . Klicken Sie auf **Commit ausführen** , um das Failover abzuschließen.
8. Wenn Sie Replikation mit mehreren Wiederherstellungspunkten einrichten, im ändern Wiederherstellung Punkt können Sie auswählen, um einen Wiederherstellungspunkt zu verwenden, die die neuesten nicht zur Verfügung (neueste wird standardmäßig verwendet). Nachdem Sie zusätzliche commit werden Wiederherstellungspunkte entfernt.
9. Nach Abschluss der Replikation ist die virtuellen Computer starten und vom sekundären Standort ausgeführt werden. Jedoch diese geschützten oder repliziert nicht zur Verfügung. Wenn der primäre Standort erneut mit der gleichen zugrunde liegenden Infrastruktur verfügbar ist, klicken Sie auf **Reverse repliziert** , um reverse Replikation begonnen werden. Dadurch wird sichergestellt, dass alle Daten wieder auf dem primären Standort repliziert und des virtuellen Computers erneut Failoververarbeitung bereit ist. Replikation nach ein ungeplantes Failover als Aufwand in Datenübertragung budgetgerecht storniert werden. Die Übertragung verwendet dieselbe Methode, die so konfiguriert ist für die erste Replikation Einstellungen für die Cloud.

## <a name="failback-from-secondary-to-primary"></a>Failback vom sekundären zum primären

 Nach Failover vom primären auf sekundären Standort repliziert virtuellen Computern sind nicht durch Website Wiederherstellung geschützt und der zweiten Standort fungiert jetzt als der primäre. Verwenden Sie diese Verfahren wieder zur ursprünglichen primären Website fehlschlägt. Dieses Verfahren beschreibt, wie zu ein geplantes Failover für einen Wiederherstellungsplan ausführen. Alternativ können Sie das Failover für einen einzelnen virtuellen Computer auf der Registerkarte **virtuellen Computern** ausführen.

1. Wählen Sie **Pläne Wiederherstellung** > *Recoveryplan_name*. Klicken Sie auf **Failover** > **Failover geplant**.
2. Wählen Sie auf der Seite **Bestätigen geplanter Failover **die Quell- und Zielwebsites-Speicherorte aus. Beachten Sie die Richtung Failover. Wenn das Failover vom primären gearbeitet als erwartet und alle virtuellen Computern sind in den hier Informationen nur sekundäre Speicherort.
3. Wählen Sie Einstellungen Synchronisierung von **Daten**, wenn Sie wieder aus Azure weiß nicht sind:

    - **Synchronisieren von Daten vor dem Failover (nur synchronisieren Delta Änderungen)**– diese Option minimiert Ausfall virtuellen Computern an, wie es synchronisiert, ohne Sie zu beenden sie nach unten. Es geschieht Folgendes:
        - Phase 1: Wird Momentaufnahme des virtuellen Computers in Azure und in der lokalen Hyper-V-Host kopiert. Der Computer läuft in Azure weiter.
        - Phase 2: Beendet des virtuellen Computers in Azure aus, damit es keine neuen Änderungen auftreten. Die letzte Gruppe von Änderungen werden auf dem lokalen Server übertragen und der lokalen virtuellen Computern gestartet wird.
    

    - **Synchronisieren von Daten bei einem Failover nur (vollständige Download)**– verwenden Sie diese Option aus, wenn Sie längere Zeit auf Azure ausgeführt haben. Diese Option ist schneller, da wir davon ausgehen, dass die meisten der Datenträger geändert hat, und wir nicht Zeit in Prüfsumme Berechnung aufwenden möchten. Es wird einen Download des Datenträgers durchgeführt. Es ist auch nützlich, wenn auf Prem virtuellen Computers gelöscht wurden.
    
    > [AZURE.NOTE] Es empfiehlt sich, wenn Sie Azure für eine Weile ausgeführt haben, verwenden Sie diese Option (einen Monat oder mehr) oder der auf Prem virtuellen Computer wurde gelöscht. Mit dieser Option wird nicht eine Prüfsumme Berechnungen durchgeführt.
    
5. Wenn Sie sind weiß nicht über in Azure und für die Cloud-Verschlüsselung aktiviert ist, wählen Sie in **Verschlüsselungsschlüssels** das Zertifikat, das ausgestellt wurde, wenn Sie die Verschlüsselung der Daten während der Installation auf dem Server VMM Anbieter aktiviert. 
5. Standardmäßig wird der letzte Wiederherstellungspunkt verwendet, aber **Die Wiederherstellung Punkt ändern** können Sie angeben, einen anderen Wiederherstellungspunkt. 
6. Klicken Sie auf das Häkchen, um das Failback zu starten.  Sie können den Fortschritt Failover auf der Registerkarte **Aufträge** folgen. 
7. f, die Sie ausgewählt haben die Option zum Synchronisieren von Daten vor dem Failover, wenn die Ausgangsdaten Synchronisierung abgeschlossen ist, und Sie bereit sind, beenden den virtuellen Computern in Azure, klicken Sie auf **Projekte**  >  <planned failover job name> **Failover abgeschlossen**. Dies beendet Azure-Computern wird, wird die neuesten Änderungen in der lokalen virtuellen Computern übertragen, und startet sie.
8. Sie können jetzt melden Sie sich an den virtuellen Computer, um dies zu überprüfen, wie erwartet zur Verfügung. 
9. Des virtuellen Computers befindet sich ein Commit ausstehen. Klicken Sie auf **Commit ausführen** , um das Failover abzuschließen.
10. Jetzt für die Durchführung das Failback klicken Sie auf **Reverse repliziert** zum Schützen des virtuellen Computers in der primäre Standort beginnen.



## <a name="failback-to-an-alternate-location"></a>Failback an einem anderen Speicherort

Wenn Sie zwischen einer [Website von Hyper-V und Azure](site-recovery-hyper-v-site-to-azure.md) Schutz bereitgestellt haben, die Ihnen die Möglichkeit, Failback aus Azure zu lokalen ein alternativen Speicherort. Dies ist sinnvoll, wenn Sie neue lokale Hardware einrichten müssen. Hier sind, wie Sie vorgehen.

1. Wenn Sie neue Hardware einrichten installieren Sie Windows Server 2012 R2 und Hyper-V-Rolle auf dem Server.
2. Erstellen Sie einen virtuelle Netzwerk-Switch mit demselben Namen, den Sie auf dem ursprünglichen Server hatten.
3. Wählen Sie **Geschützte Elemente** -> **Gruppe "Schutz"**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> wieder fehl, und wählen Sie **Failover geplant**werden soll.
4. **Geplanter Failover bestätigen** wählen Sie **Erstellen lokalen virtuellen Computern, wenn er nicht vorhanden ist**. 
5. Wählen Sie in **Host Name** den neuen Hyper-V-Host-Server des virtuellen Computers platziert werden soll.
6. Synchronisierung von Daten wird empfohlen, dass Sie die Option **synchronisieren die Daten vor dem Failover**auswählen. Dies minimiert Ausfall virtuellen Computern, wie es synchronisiert, ohne Sie zu beenden sie nach unten. Es geschieht Folgendes:

    - Phase 1: Wird Momentaufnahme des virtuellen Computers in Azure und in der lokalen Hyper-V-Host kopiert. Der Computer läuft in Azure weiter.
    - Phase 2: Beendet des virtuellen Computers in Azure aus, damit es keine neuen Änderungen auftreten. Die letzte Gruppe von Änderungen werden auf dem lokalen Server übertragen und der lokalen virtuellen Computern gestartet wird.
    
7. Klicken Sie auf das Häkchen, um das Failover (Failback) beginnen.
8. Nachdem Sie die ursprüngliche Synchronisierung abgeschlossen ist, und Sie zum Beenden des virtuellen Computers in Azure bereit sind, klicken Sie auf **Projekte** > <planned failover job> > **Failover abgeschlossen**. Dies beendet Azure-Computern wird, wird die neuesten Änderungen in der lokalen virtuellen Computern übertragen und startet sie.
9. Sie können melden Sie sich auf den lokalen virtuellen Computern zu überprüfen, ob alles wie erwartet funktioniert. Klicken Sie dann auf **Commit** , um das Failover fertig zu stellen.
10. Klicken Sie auf **Repliziert umkehren** , um zu der lokalen virtuellen Computern schützen.

    >[AZURE.NOTE] Wenn Sie den Auftrag Failback Abbrechen, während sie im Schritt Daten Synchronisierung ist, werden der virtuellen lokalen Computer in einen Zustand beschädigt aus. Dies liegt daran Daten Synchronisierung Kopien die neuesten Daten aus Azure-virtuellen Computer zu Festplatten mit den Daten auf Prem Festplatten und Angabe bis die Synchronisierung abgeschlossen ist, den Datenträger, die möglicherweise Daten nicht in einem konsistenten Zustand. Wenn der auf-Prem virtuellen Computer gestartet wird, nachdem Daten Synchronisierung abgebrochen wird, kann es nicht gestartet. Erneutes lösen Sie Failover zum Abschließen der Synchronisierung von Daten aus aus.
 
