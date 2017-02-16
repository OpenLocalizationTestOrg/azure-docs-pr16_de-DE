<properties
    pageTitle="Premium Speicher: Leistungsstarke Storage für Azure-virtuellen Computern Auslastung | Microsoft Azure"
    description="Premium-Speicher bietet leistungsfähige und niedrig Wartezeiten Datenträger Unterstützung für I/O-Auslastung Auslastung auf Azure virtuellen Computern ausgeführt. Unterstützung von Azure DS-Serie, DSv2-Serie und GS-Serie virtuellen Computern Premium-Speicher."
    services="storage"
    documentationCenter=""
    authors="yuemlu"
    manager="aungoo-msft"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/28/2016"
    ms.author="yuemlu"/>


# <a name="premium-storage-high-performance-storage-for-azure-virtual-machine-workloads"></a>Premium Speicher: Leistungsstarke Storage für Auslastung Azure-virtuellen Computern

## <a name="overview"></a>(Übersicht)

Azure Premium-Speicher bietet leistungsfähige und niedrig Wartezeiten Datenträger Unterstützung für virtuelle Maschinen, die ich/O-Auslastung Auslastung. Datenträger virtuellen Computern (virtueller Computer), mit denen Storage Premium speichern Daten auf einfarbige Zustand Laufwerken (SSDs). Sie können Ihrer Anwendung virtueller Computer Datenträger in Azure Premium Speicher nutzen der Geschwindigkeit und Leistung von diese Datenträger ausführen migrieren.

Ein Azure-virtuellen Computer unterstützt mehrere Premium-Datenträger, anfügen, sodass Ihre Applikationen bis zu 64 TB Speicherplatz pro virtueller Computer haben, können. Mit Premium-Speicher erreichen Ihrer Anwendung 80.000 IOPS (e/a-Vorgänge pro Sekunde) pro virtueller Computer und 2000 MB pro zweiten Datenträgerdurchsatz pro virtueller Computer mit sehr niedrig Wartezeiten für Vorgänge finden Sie hier.

Mit Premium-Speicher bietet Azure der Möglichkeit, wirklich heben Sie-und-UMSCHALT Ihre anspruchsvolle Enterprise-Anwendungen wie, ANGEHÖREN, Dynamics CRM, Exchange Server, SharePoint-Farmen und SAP-Business-Suite aus, in der Cloud. Sie können einer Vielzahl von Leistung Datenbanken Auslastung wie SQL Server, Oracle, MongoDB, MySQL, Redis, ausführen, die konsistent hohe Leistung und niedrige Wartezeit Premium Speichermenge erfordern.

>[AZURE.NOTE] Es empfiehlt sich, alle virtuellen Computern Datenträger mit hoher IOPS zu Azure Premium Speicher für die optimale Leistung für eine Anwendung Anforderung migrieren. Wenn der Datenträger keine hohe IOPS erforderlich sind, können Sie Kosten beschränken, indem Sie es in Standard-Speicher, der virtuellen Computern Datenträgerdaten auf Festplatten (Festplatten) anstelle von SSDs gespeichert verwalten.

Um mit Azure Premium Speicher anzufangen, finden Sie auf Seite [Erste Schritte kostenlos](https://azure.microsoft.com/pricing/free-trial/) . Informationen zum Migrieren von Ihrer vorhandenen virtuellen Computern zu Premium Speicher finden Sie unter [Migrieren zu Azure Premium Storage](storage-migration-to-premium-storage.md).

>[AZURE.NOTE] Premium Speicher wird derzeit in einigen Regionen unterstützt. Sie können die Liste der verfügbaren Regionen [Azure](https://azure.microsoft.com/regions/#services)-Dienste nach Region suchen.

## <a name="premium-storage-features"></a>Premium Speicher-Features

**Premium-Datenträger**: Azure Premium Speicher unterstützt virtueller Computer Datenträger Premium Speicher angefügt werden können unterstützt Azure-virtuellen Computern (DS, DSv2, GS oder Fs Reihen). Bei Verwendung von Premium Speicher Sie haben die Wahl der drei Datenträgergrößen nämlich P10 (128GiB), P20 (512GiB) und P30 (1024GiB), jede mit einem eigenen Performance-Spezifikationen. Je nach Ihrer Anwendung Anforderung fügen Sie eine oder mehrere der folgenden Laufwerke zu Ihrem Premium Storage virtueller Computer unterstützt. Im folgenden Abschnitt [Premium Speicher Skalierbarkeit](#premium-storage-scalability-and-performance-targets) und Leistungsziele wird die Angaben ausführlicher beschrieben.

**Die Seitenblob Premium**: Premium Speicher unterstützt Azure Seitenblobs, die zum beständigen Datenträger für Azure virtuellen Computern (virtuellen Computern) halten verwendet werden. Derzeit unterstützt Premium speichern nicht Azure Block Blobs, Azure anfügen Blobs, Azure-Dateien, Azure Tabellen oder Azure Warteschlangen. Ein anderes Objekt in einem Konto Premium Speicher platziert werden auf einer Seite Blob, und es wird in einer der unterstützten bereitgestellten Größen ausrichten. Daher ist Premium Speicher Konto nicht zum Speichern von sehr Blobs vorgesehen.

**Premium Speicher Konto**: zum Verwenden von Premium Speicher, müssen Sie ein Konto Premium Speicher erstellen. Wenn Sie das [Azure-Portal](https://portal.azure.com)zu verwenden lieber, können Sie ein Konto Premium Speicher durch Festlegen von "lokal redundante Speicher (LRS)" und "Premium" Leistung Ebene als die Replikationsoption erstellen. Sie können auch ein Konto Premium Speicher erstellen, indem Sie den Typ als "Premium_LRS" mit der Version 2014-02-14 [Speicher REST-API](http://msdn.microsoft.com//library/azure/dd179355.aspx) angeben oder höher; der [Dienst Verwaltung REST-API](http://msdn.microsoft.com/library/azure/ee460799.aspx) Version 2014-10-01 oder spätere (klassische Bereitstellungen); [Azure Speicher Ressource Anbieter REST-API-Referenz](http://msdn.microsoft.com/library/azure/mt163683.aspx) (Ressourcenmanager Bereitstellungen); und [Azure PowerShell](../powershell-install-configure.md) Version 0.8.10 oder höher. Informationen Sie zu Premium Konto Speichergrenzwerte auf [Premium Speicher Skalierbarkeit und Leistungsziele](#premium-storage-scalability-and-performance-targets)im folgenden Abschnitt.

**Lokal redundante Speicherung Premium**: A Premium Speicher Konto nur lokal redundante Speicher (LRS) als die Replikationsoption unterstützt und drei Kopien der Daten in einem einzigen Bereich behält. Aspekte hinsichtlich Geo Replikation bei der Premium Speicherung finden Sie im Abschnitt " [Momentaufnahmen und Blob kopieren](#snapshots-and-copy-blob) " in diesem Artikel.

Azure verwendet das Speicherkonto als Container für Ihr Betriebssystem (BS) und Daten Datenträger. Wenn Sie einer Azure DS, DSv2, GS oder Fs virtueller Computer erstellen, und wählen Sie ein Konto Azure Premium Speicher, Ihr Betriebssystem und Datenträger mit Daten in diesem Storage-Konto gespeichert.

Sie können Premium Speicher für Datenträger in eine von zwei Arten verwenden:
- Erstellen Sie zuerst ein neues Premium Speicher-Konto an. Wählen Sie das Premium Speicher-Konto in den Einstellungen der Speicher Konfiguration als Nächstes beim Erstellen einer neuen DS DSv2, GS oder Fs virtueller Computer. ODER,
- Beim Erstellen einer neuen DS DSv2, GS, Fs virtueller Computer erstellen ein neues Premium Speicherkonto in den Einstellungen für Speicher-Konfiguration oder Erstellen eines Premium-Speicher Standardkonto Azure-Portal lassen.

Schrittweise Anweisungen finden Sie unter [Schnellstart](#quick-start) im Abschnitt weiter unten in diesem Artikel.

>[AZURE.NOTE] Um einen benutzerdefinierten Domänennamen kann ein Premium Speicher-Konto zugeordnet werden.

## <a name="premium-storage-supported-vms"></a>Premium Speicher unterstützt virtuellen Computern

Premium Speicher unterstützt DS-Serie, DSv2-Serie, GS-Serie und Fs-Serie Azure-virtuellen Computern (virtuelle Computer). Sie können mit Premium-Speicher von virtuellen Computern unterstützt sowohl Standard- und Premium-Datenträger verwenden. Doch Sie können Datenträger Premium mit virtueller Computer Datenreihe, die nicht Premium Speicher kompatibel sind.

Informationen zur Verfügung Azure-virtuellen Computer Arten und Größen für virtuelle Windows-Computer finden Sie unter [Windows virtueller Computer Größen](../virtual-machines/virtual-machines-windows-sizes.md). Informationen zu virtuellen Computer Arten und Größen für Linux virtuellen Computern finden Sie unter [Linux virtueller Computer Größen](../virtual-machines/virtual-machines-linux-sizes.md).

Es folgen einige der Features von DS, DSv2, GS und EA Reihe virtuellen Computern:

**Cloud-Dienst**: DS-Serie virtuellen Computern hinzugefügt werden können, auf einen Clouddienst, die nur DS-Serie virtuellen Computern enthält. Fügen Sie keine DS-Serie virtuellen Computern in einer vorhandenen Cloud-Service, die DS-Serie virtuellen Computern enthält. Sie können einen neuen Cloud-Dienst Ausführen von nur DS-Serie virtuellen Computern Ihrer vorhandenen virtuellen Festplatten migrieren. Wenn die gleiche virtuelle IP-Adresse für den neuen Clouddienst beibehalten, auf dem Ihre DS-Serie virtuellen Computern gehostet werden soll, verwenden Sie die [Reservierte IP-Adressen](../virtual-network/virtual-networks-instance-level-public-ip.md). Ausführen von nur G-Serie virtuellen Computern einer vorhandenen Cloud-Dienst können GS-Serie virtuellen Computern hinzugefügt werden.

**Betriebssystem-Laufwerk**: der Premium Speicher unterstützt Azure-virtuellen Computern kann einen Betriebssystem (BS) Datenträger auf einem Standard-Speicher-Konto oder auf einem Konto Premium Speicher gehostet Verwendung konfiguriert werden. Wir empfehlen Premium Speicher mit OS Datenträger für optimale benutzerfreundlichkeit basiert.

**Daten Datenträger**: Sie können beide Premium und Standard-Datenträger in denselben Premium Speicher unterstützt virtueller Computer. Mit Premium-Speicher, können Sie eine Premium-Speicher bereitgestellt virtueller Computer unterstützt und mehrere permanente Daten Datenträger an den virtuellen Computer anzufügen. Bei Bedarf können Sie auf dem Datenträger zum Steigern der Kapazität und Leistung von der Lautstärke verteilen.

> [AZURE.NOTE] Wenn Sie Premium Daten Datenträger mit [Speicher Leerzeichen](http://technet.microsoft.com/library/hh831739.aspx)versehen, sollten Sie es mit einer Spalte für jeden Datenträger konfigurieren, die verwendet wird. Andernfalls möglicherweise Gesamtsystemleistung des Datenträgers aufgeteilten erwarteten aufgrund ungleiche Verteilung der Verkehr über die Laufwerke vorangehen. Standardmäßig kann die Server-Manager-Benutzeroberfläche (UI) Setup Spalten bis zu 8 Festplatten. Aber wenn Sie mehr als 8 Datenträger haben, müssen Sie PowerShell verwenden, um die Lautstärke zu erstellen, und geben Sie die Anzahl der Spalten auch manuell. Andernfalls wird der Server-Manager-UI weiterhin 8 Spalten verwenden, obwohl Sie weitere Datenträger verfügen. Wenn Sie in einer einzelnen Streifen 32 Datenträger haben, sollten Sie beispielsweise 32 Spalten angeben. Sie können den Parameter *NumberOfColumns* des [Neu VirtualDisk](http://technet.microsoft.com/library/hh848643.aspx) PowerShell-Cmdlets verwenden, um die Anzahl der Spalten, die das virtuelle Laufwerk untersuchten anzugeben. Weitere Informationen finden Sie unter [Speicher Leerzeichen – Übersicht](http://technet.microsoft.com/library/hh831739.aspx) und [Häufig gestellte Fragen zu Speicher Leerzeichen](http://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).

**Cache**: Premium Speicher unterstützte virtuelle Computer haben eine einzigartige Zwischenspeichern-Funktion mit dem erhalten Sie hohe Durchsatz und Wartezeit, die zugrunde liegenden Premium Speicher Datenträger Leistung überschreiten. Sie können die Richtlinie auf die Premium-Datenträger als schreibgeschützt, lesen/schreiben oder keine Zwischenspeichern Datenträger konfigurieren. Der Standard-Datenträger Zwischenspeichern Richtlinie ist schreibgeschützt für alle Datenträger mit Daten und Lesen/Schreiben für das Betriebssystem von Festplatten. Verwenden Sie die richtigen Konfiguration Einstellung, um optimale Leistung für eine Anwendung zu erzielen. Beispielsweise für gelesen beanspruchen oder finden Sie hier nur Daten Datenträger, z. B. SQL Server-Datendateien, legen Sie Datenträger Zwischenspeichern Richtlinie "Schreibgeschützt". Legen Sie für Schreiben fett oder Schreiben nur Daten Datenträger, wie etwa SQL Server-Protokolldateien Datenträger Zwischenspeichern Richtlinie auf "Keine" aus. Weitere Informationen zum Optimieren Ihrer Design mit Premium von Speicher in [Entwerfen, um die Leistung mit Premium-Speicher](storage-premium-storage-performance.md).

**Analytics**: zum Analysieren der Leistung von virtuellen Computern Datenträger auf Premium Speicher Konten verwenden, können Sie die Azure-virtuellen Computer Diagnose Azure-Portal aktivieren. Details finden Sie unter [Microsoft Azure virtuellen Computern Überwachung mit Azure Diagnose Erweiterung](https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/) . Basiert Betriebssystem verwenden, um die Leistung der Datenträger finden Sie unter Tools, wie etwa [Windows Performance Monitor](https://technet.microsoft.com/library/cc749249.aspx) für Windows-virtuellen Computern und [IOSTAT](http://linux.die.net/man/1/iostat) für Linux virtuelle Computer ein.

**Virtueller Computer Maßstab Grenzwerte und Leistung**: jeder Premium Speicher unterstützte virtueller Speicher weist Maßstab Grenzwerte und Leistung Spezifikation für IOPS, die Bandbreite und die Anzahl der Datenträger, die pro virtuellen Computer angefügt werden kann. Wenn mit Premium-Datenträger mit Premium Speicher virtuellen Computern unterstützt, stellen Sie sicher, dass es steht ausreichend IOPS und Bandbreite Ihrer virtuellen Computers, um den Datenverkehr Datenträger versorgen.
Beispielsweise hat einen STANDARD_DS1 virtuellen 32 MB pro zweiten dedizierte Bandbreite für Premium Speicher Datenträger Datenverkehr verfügbar. Der Datenträger für die Speicherung einer P10 Premium kann 100 MB pro zweiten Bandbreite bereitstellen. Wenn Sie ein Datenträger P10 Premium Speicher mit diesem virtuellen Computer angefügt wurden, kann er nur bis zu 32 MB pro Sekunde, aber nicht bis zu 100 MB pro Sekunde eingefügt werden, die der Datenträger P10 bereitstellen kann.

Aktuell der größte virtuellen Computer auf DS-Serie ist Standard_DS15_v2, und es kann bis zu 960 MB pro Sekunde über alle Laufwerke bereitstellen. Der größte virtuellen Computer auf GS-Serie ist Standard_GS5, und es kann bis zu 2000 MB pro Sekunde mitteilen, auf alle Festplatten verteilt.
Beachten Sie, dass diese Grenzwerte für den Datenverkehr Datenträger alleine gelten, einschließlich nicht-Cache-Treffer und -Netzwerkverkehr. Es steht eine separate Bandbreite Netzwerkverkehr virtueller Computer, also die dedizierte Bandbreite für Premium-Datenträger abweicht.

Die aktuellste Informationen auf maximale IOPS und Durchsatz (Bandbreite) für die Speicherung Premium unterstützte virtuellen Computern, finden Sie unter [Größen virtuellen Windows-Computer](../virtual-machines/virtual-machines-windows-sizes.md) oder [Linux virtueller Computer Schriftgrade](../virtual-machines/virtual-machines-linux-sizes.md).

Weitere Informationen zu den Datenträger Premium und deren IOPs und Durchsatz Grenzwerte finden Sie unter der Tabelle im Abschnitt [Premium Speicher Skalierbarkeit und Leistungsziele](#premium-storage-scalability-and-performance-targets) in diesem Artikel.

## <a name="premium-storage-scalability-and-performance-targets"></a>Premium Speicher Skalierbarkeit und Leistungsziele

In diesem Abschnitt werden die Skalierbarkeit und Leistung Ziele beschrieben, die Sie berücksichtigen müssen, wenn Premium Speicher verwenden.

### <a name="premium-storage-account-limits"></a>Premium Konto Speichergrenzwerte

Premium Speicherkonten haben Skalierbarkeit Ziele folgen:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tbody>
<tr>
    <td><strong>Total Kapazität</strong></td>
    <td><strong>Gesamte Bandbreite für ein Speicherkonto lokal redundante</strong></td>
</tr>
<tr>
    <td>
    <ul>
       <li type=round>Datenträger Kapazität: 35 TB</li>
       <li type=round>Snapshot-Kapazität: 10 TB</li>
    </ul>
    </td>
    <td>Bis zu 50 GB/s pro Sekunde für eingehende + ausgehend</td>
</tr>
</tbody>
</table>

- Eingehende bezieht sich auf alle Daten (Anfragen) mit einem Speicherkonto gesendet werden.
- Ausgehende bezieht sich auf alle Daten (Antworten) von einem Speicherkonto empfangen werden.

Weitere Informationen finden Sie unter [Leistung und Skalierbarkeit der Azure-Speicher](storage-scalability-targets.md).

Die Anforderungen Ihrer Anwendung, die größer sind als der Skalierbarkeit Ziele eines einzelnen Speicher-Kontos, erstellen Sie die Anwendung mit mehreren Speicherkonten, und Partitionieren von Daten über diese Speicherkonten hinweg. Beispielsweise, wenn Sie 51 TB (TB) Datenträger über eine Reihe von virtuellen Computern anfügen möchten, verteilen Sie diese auf zwei Speicherkonten da 35 TB den Grenzwert für ein einzelnes Premium Speicher-Konto ist. Stellen Sie sicher, dass ein einzelnes Premium Speicher Konto nie mehr als 35 TB bereitgestellte Datenträger verfügt.

### <a name="premium-storage-disks-limits"></a>Premium Speicher Festplatten Beschränkungen

Wenn Sie einen Datenträger mit einem Premium Speicher-Konto bereitstellen, abhängig wie viel Eingabe-/Ausgabe-Vorgänge pro Sekunde (IOPS) und Durchsatz (Bandbreite) Bezugsarten können die Größe des Datenträgers. Zurzeit stehen drei Arten von Premium-Datenträger: P10 P20 und P30. Jeweils weist bestimmte Grenzwerte für IOPS und Durchsatz in der folgenden Tabelle angegeben sind:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tbody>
<tr>
    <td><strong>Premium Speicher Datenträgertyp</strong></td>
    <td><strong>P10</strong></td>
    <td><strong>P20</strong></td>
    <td><strong>P30</strong></td>
</tr>
<tr>
    <td><strong>Größe des Datenträger</strong></td>
    <td>128 giB</td>
    <td>512 giB</td>
    <td>1024 giB (1 TB)</td>
</tr>
<tr>
    <td><strong>IOPS pro Datenträger</strong></td>
    <td>500</td>
    <td>2300</td>
    <td>5000</td>
</tr>
<tr>
    <td><strong>Durchsatz pro Datenträger</strong></td>
    <td>100 MB pro Sekunde </td>
    <td>150 MB pro Sekunde </td>
    <td>200 MB pro Sekunde </td>
</tr>
</tbody>
</table>

> [AZURE.NOTE] Stellen Sie sicher, dass Ihre virtuellen Computers, um den Datenverkehr Datenträger zu steuern, wie im Abschnitt [Speicher Premium virtuellen Computern unterstützt](#ds-dsv2-and-gs-series-vms) zuvor in diesem Artikel erläutert ausreichende Bandbreite vorhanden ist. Andernfalls wird Ihre Datenträgerdurchsatz und IOPS eingeschränkt werden, um Werte basierend auf die Speichergrenze in der obigen Tabelle angegeben ist, anstatt die Grenzwerte virtueller Computer zu verringern.  

Hier sind einige wichtige Aspekte, die Sie hinsichtlich Premium Speicher Skalierbarkeit und Leistung Ziele kennen müssen:

- **Nach der Bereitstellung von Kapazität und Performance**: Wenn Sie einen Premium Speicher Datenträger, im Gegensatz zu den standardmäßigen Speicher bereitstellen wird sichergestellt Kapazität, IOPS und Durchsatz für diesen Datenträger. Angenommen, bei der Erstellung einer P30 Datenträger, Azure Vorschriften 1024 GB Speicherkapazität für das zweite 5000 IOPS und 200 MB pro Durchsatz für diesen Datenträger. Die Anwendung kann alle oder einen Teil der Kapazität und Leistung verwenden.

- **Größe des Datenträger**: Azure ordnet die Größe des (aufgerundet) auf die nächste Premium Speicher die Option als in der Tabelle angegeben. Beispielsweise einen Datenträger Größe 100 GiB wird als eine Option P10 klassifiziert und bis zu 500 EA Einheiten pro Sekunde und mit bis zu 100 MB Datendurchsatz pro Sekunde ausführen kann. Auf ähnliche Weise einen Datenträger Größe 400 GiB wird als eine Option P20 klassifiziert und kann bis zu 2300 EA Einheiten pro zweite und bis zu 150 MB Datendurchsatz pro Sekunde ausführen.

    > [AZURE.NOTE] Sie können die Größe der vorhandenen Festplatten problemlos erhöhen. Beispielsweise, wenn Sie vergrößern möchten die Größe des eine 30 GB Datenträger 128GB oder 1 TB. Oder, wenn Sie Ihre P20 Datenträger auf einen Datenträger P30 konvertiert werden, da Sie mehr Kapazität oder mehr IOPS und Durchsatz benötigen möchten. Sie können den Datenträger mithilfe von "Update-AzureDisk" PowerShell-Cmdlet mit erweitern "-ResizedSizeInGB" Eigenschaft. Für diese Aktion muss Datenträger aus der virtuellen Computers oder der virtuellen Computer muss abgebrochen werden getrennt werden soll.

- **EA-Größe**: Maßeinheit für den ein-/Ausgabe (e/a) ist 256 KB. Wenn die übertragenen Daten weniger als 256 KB ist, gilt die e/a-Einheit. Die e/a-größere werden als mehrere i/OS Größe berücksichtigt 256 KB. 1100 KB e/a wird beispielsweise als fünf e/a-Einheiten gezählt.

- **Durchsatz**: der Durchsatzgrenze umfasst schreibt auf dem Datenträger als auch liest aus diesem Datenträger, die nicht aus dem Cache bereitgestellt werden. Angenommen, hat ein Datenträger P10 100 MB Datendurchsatz pro Sekunde pro Datenträger. Einige Beispiele für gültige Durchsatz für den Datenträger P10 sind,
<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tbody>
<tr>
    <td><strong>Max Throughput pro P10 Datenträger</strong></td>
    <td><strong>Nicht-Cache liest Datenträger</strong></td>
    <td><strong>Nicht-Cache schreibt auf einem Datenträger</strong></td>
</tr>
<tr>
    <td>100 MB pro Sekunde</td>
    <td>100 MB pro Sekunde</td>
    <td>0</td>
</tr>
<tr>
    <td>100 MB pro Sekunde</td>
    <td>0</td>
    <td>100 MB pro Sekunde</td>
</tr>
<tr>
    <td>100 MB pro Sekunde </td>
    <td>60 MB pro Sekunde </td>
    <td>40 MB pro Sekunde </td>
</tr>
</tbody>
</table>

- **Cache-Treffer**:-Cache-Treffer werden nicht durch die zugewiesenen IOPS/Durchsatz des Datenträgers beschränkt. Beispielsweise, wenn Sie einen Datenträger mit schreibgeschützten Cache Einstellung auf ein Premium-Speicher virtueller Computer unterstützt, liest, die aus dem Cache bereitgestellt werden, sind nicht unterliegen Premium Speichergrenzwerte Datenträger. Daher könnten Sie sehr hoch erhalten Durchsatz von einem Datenträger ist die Arbeitsbelastung überwiegend liest. Beachten Sie, dass separate IOPS Cache unterliegt / Durchsatz Ebene virtueller Computer anhand der Größe virtueller Computer beschränkt. DS-Serie virtuellen Computern haben ungefähr 4000 IOPS und 33 MB/s pro Core für Cache und lokale SSD IOs. GS-Serie virtuellen Computern haben maximal 5000 IOPS und 50 MB/s pro Core für Cache und lokale SSD IOs.

## <a name="throttling"></a>Begrenzungsebene
Möglicherweise werden angezeigt, wenn die Anwendung IOPS oder Durchsatz die zugewiesenen Grenzwerte für einen Datenträger Premium Speicher überschreiten oder Ihren Datenträger insgesamt Verkehr über alle Laufwerke des virtuellen Computers Datenträger Bandbreite verfügbar für den virtuellen Computer überschreitet begrenzungsebene. Um zu vermeiden, begrenzungsebene, empfehlen wir, dass Sie die Anzahl der ausstehenden e/a-Anforderung von Datenträger basierend auf den Zielgruppen Skalierbarkeit und Leistung für den Datenträger, den haben Sie nach der Bereitstellung und basierend auf den verfügbare Datenträgerbandbreite für den virtuellen Computer, einschränken.  

Ihrer Anwendung kann die niedrigste Wartezeit erreichen, wenn es entworfen wird, um begrenzungsebene zu vermeiden. Andererseits, wenn die Anzahl der ausstehenden e/a-Anfragen für den Datenträger zu klein ist, kann keine Ihrer Anwendung nutzen die maximale IOPS und Durchsatz Ebenen, die auf den Datenträger verfügbar sind.

Die folgenden Beispiele veranschaulichen Drosselung Ebenen zu berechnen. Alle Berechnungen basieren auf e/a-Größe von 256 KB:

### <a name="example-1"></a>Beispiel 1:
Die Anwendung weist 495 e/a-Einheiten 16 KB Größe in eine Sekunde auf einem Datenträger P10 fertig. Diese werden als 495 e/a-Einheiten pro Sekunde (IOPS) berücksichtigt werden. Wenn Sie eine 2 MB e/a in der gleichen zweiten versuchen, ist die Summe der i/o-Einheiten gleich 495 + 8. Dies liegt daran 2 MB e/a in einer anderen Einheit 2048 KB / 256 KB = 8 e/a-ausgegeben wird, wenn die Größe der e/a-256 KB ist. Da die Summe der 495 + 8 den 500 IOPS Grenzwert für den Datenträger überschreitet, tritt begrenzungsebene.

### <a name="example-2"></a>Beispiel 2:
Die Anwendung weist 400 e/a-Einheiten 256 KB Größe auf einem Datenträger P10 fertig. Die gesamte Bandbreite verbraucht ist (400 * 256) / 1024 = 100 MB / s. Ein Datenträger P10 verfügt über Durchsatzgrenze von 100 MB pro Sekunde. Wenn die Anwendung versucht, weitere e/a in die zweite ausführen, wird es gedrosselt, da sie die zugewiesene Obergrenze überschreitet.

### <a name="example-3"></a>Beispiel 3:
Sie haben einen virtuellen DS4 mit zwei verbundenen P30 Datenträger aus. Jeder P30 Datenträger kann 200 MB Datendurchsatz pro Sekunde. Allerdings hat einen virtuellen DS4 einer gesamten Bandbreite Speicherkapazität 256 MB pro Sekunde. Sie können keine also, die angefügten Datenträger für den maximalen Durchsatz dieses DS4 virtuellen Computers gleichzeitig steuern. Um dieses Verhalten zu beheben, können Datenverkehr 200 MB pro Sekunde auf einem Datenträger und 56 MB pro Sekunde auf dem anderen Datenträger haftbar. Wenn die Summe der Festplatte wechselt über 256 MB pro Sekunde Verkehr, erhält der Datenverkehr Datenträger gedrosselt.

>[AZURE.NOTE] Wenn der Datenträger Datenverkehr hauptsächlich kleine e/a-Größen besteht, ist es anzunehmen, dass eine Anwendung die Beschränkung IOPS vor der Durchsatzgrenze erreicht wird. Andererseits, wenn der Datenverkehr Datenträger hauptsächlich großen e/a-Größen besteht, ist es äußerst wahrscheinlich, dass die Anwendung der Durchsatzgrenze anstelle der Grenzwert IOPS erreicht wird. Sie können Ihrer Anwendung IOPS und Durchsatz Kapazität mithilfe von optimale e/a-Größen und auch durch Einschränken der Anzahl der anstehende e/a-Anfragen für Datenträger maximieren.

Informationen zum Erstellen eines Konzepts für hohe Leistung analysiert finden Sie unter mit Premium-Speicher im Artikel, [Entwurf, um die Leistung mit Premium-Speicher](storage-premium-storage-performance.md).

## <a name="snapshots-and-copy-blob"></a>Momentaufnahmen und Blob kopieren
Sie können eine Momentaufnahme für Premium-Speicher auf die gleiche Weise erstellen, wie Sie bei Verwendung von Standard-Speicher eine Momentaufnahme erstellen. Da Premium Speicher nur lokal redundante Speicher (LRS) als die Replikationsoption unterstützt, wird empfohlen, dass Sie Momentaufnahmen erstellen und kopieren Sie diese Momentaufnahmen mit einem Speicherkonto Geo redundante standard. Weitere Informationen finden Sie unter [Speicheroptionen für Redundanz Azure](storage-redundancy.md).

Wenn Sie ein Datenträger eines virtuellen Computers zugeordnet ist, sind bestimmte Vorgänge API auf der Seitenblob Sichern des Datenträgers nicht zulässig. Angenommen, eine [Kopie Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx) -Operation für die Blob nicht möglich, solange der Datenträger eines virtuellen Computers zugeordnet ist. In diesem Fall erstellen Sie zuerst eine Momentaufnahme der betreffende Blob mithilfe der Methode [Blob Snapshot](http://msdn.microsoft.com/library/azure/ee691971.aspx) REST-API, und führen Sie dann auf [Kopieren Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx) der Momentaufnahme zum Kopieren des angefügten Datenträgers. Alternativ können Sie trennen Sie den Datenträger, und führen Sie dann auf alle notwendigen Vorgänge von der zugrunde liegenden Blob.

Folgende Grenzwerte gelten für Premium Speicher Blob Momentaufnahmen:
<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tbody>
<tr>
    <td><strong>Speichergrenzwert Premium</strong></td>
    <td><strong>Wert</strong></td>
</tr>
<tr>
    <td>Max. Anzahl von Momentaufnahmen pro blob</td>
    <td>100</td>
</tr>
<tr>
    <td>Konto Speicherkapazität für Momentaufnahmen (Momentaufnahmen nur Daten umfasst und enthält keine Daten in Basis Blob)</td>
    <td>10 TB</td>
</tr>
<tr>
    <td>Min. Zeit zwischen aufeinander folgenden Momentaufnahmen</td>
    <td>10 Minuten</td>
</tr>
</tbody>
</table>

Wenn die Momentaufnahmen Geo redundanten Kopien beibehalten möchten, können Sie Momentaufnahmen von einem Konto Premium Speicher mit einem Speicherkonto Geo redundanten standard-kopieren mithilfe von AzCopy oder kopieren Blob. Weitere Informationen finden Sie unter [Übertragen von Daten mit dem AzCopy Befehlszeilenprogramm](storage-use-azcopy.md) und [Blob kopieren](http://msdn.microsoft.com/library/azure/dd894037.aspx).

Ausführliche Informationen zum Durchführen von Vorgängen REST Seitenblobs im Speicher Premium-Konten finden Sie unter [Ausführen von BLOB-Dienst Operationen mit Azure Premium Speicher](http://go.microsoft.com/fwlink/?LinkId=521969) in der MSDN-Bibliothek.

## <a name="using-linux-vms-with-premium-storage"></a>Verwenden von Linux virtuellen Computern mit Premium-Speicher
Näheres wichtige Anweisungen unter zum Konfigurieren Ihrer Linux virtuellen Computern Premium Speichermenge:

- Für alle Premium-Datenträger mit Cache Einstellung als entweder "schreibgeschützt" oder "Keine" müssen Sie "Hindernisse" deaktivieren, während Sie das Dateisystem laden, um die Ziele Skalierbarkeit für Premium-Speicher zu erreichen. Nicht benötigen Hindernisse für dieses Szenario, da die schreibt in Premium Speicher gesicherten Datenträger dauerhaften für diese Einstellungen des Caches sind. Wenn die Anfrage schreiben erfolgreich abgeschlossen ist, hat Daten in den beständigen Speicher geschrieben wurde. Verwenden Sie die folgenden Methoden zum Deaktivieren von "Hindernisse" abhängig von Ihrem Dateisystem:
    - Wenn Sie **ReiserFS**verwenden, deaktivieren Sie mit der Option bereitstellen Hindernisse "Hindernis = keine" (verwenden Sie zum Aktivieren der Hindernisse, "Hindernis Löschvorgang =")
    - Wenn Sie **ext3/ext4**verwenden, deaktivieren Sie über die Option bereitstellen Hindernisse "Hindernis = 0" (verwenden Sie zum Aktivieren der Hindernisse, "Hindernis = 1")
    - Wenn Sie **XFS**verwenden, deaktivieren Sie mit der Option bereitstellen "Nobarrier" Hindernisse (zum Aktivieren der Hindernisse, verwenden Sie die Option "Hindernis")

- Für Premium-Datenträger mit Cache "Lesen/Schreiben" festlegen sollte Hindernisse für Zuverlässigkeit der schreibt aktiviert sein.
- Für die Lautstärke Bezeichnungen nach dem Neustart des virtuellen Computer beibehalten werden müssen Sie mit der UUID verweisen auf die Laufwerke/etc/fstab aktualisieren. Lesen Sie auch [How to fügen Sie einen Datenträger zu einem Linux virtuellen Computern](../virtual-machines/virtual-machines-linux-classic-attach-disk.md)

Im folgenden werden die Linux-Versionen, die wir überprüft mit Premium-Speicher. Es empfiehlt sich, dass Sie Ihre virtuellen Computer auf mindestens eine dieser Versionen (oder höher) aktualisieren für eine bessere Leistung und Stabilität mit Premium-Speicher. Darüber hinaus erfordern einige Versionen der neuesten LIS (Linux Integration Services Version 4.0 für Microsoft Azure). Führen Sie den Link unter für herunterladen und installieren. Wir weiterhin die weiteren Bilder zu der Liste als führen wir Sie zusätzliche Validierungen hinzufügen. Bitte beachten Sie, unsere Validierungen angezeigt wurden Leistung ist für diese Bilder unterschiedlich, und auch das hängt Arbeitsbelastung Merkmale und Einstellungen, klicken Sie auf die Bilder. Verschiedene Bilder werden für unterschiedliche Arten von Arbeitsbelastung optimiert.
<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tbody>
<tr>
    <td><strong>Verteilung</strong></td>
    <td><strong>Version</strong></td>
    <td><strong>Unterstützte Kernel</strong></td>
    <td><strong>Details</strong></td>
</tr>
<tr>
    <td rowspan="2"><strong>Ubuntu</strong></td>
    <td>12.04</td>
    <td>3.2.0-75.110+</td>
    <td>Ubuntu-12_04_5-LTS-amd64-Server-20150119-en-US-30GB</td>
</tr>
<tr>
    <td>14.04 +</td>
    <td>3.13.0-44.73+</td>
    <td>Ubuntu-14_04_1-LTS-amd64-Server-20150123-en-US-30GB</td>
</tr>
<tr>
    <td><strong>Debian</strong></td>
    <td>7.x, 8.x</td>
    <td>3.16.7-ckt4-1+</td>
    <td> </td>
</tr>
<tr>
    <td rowspan="2"><strong>SUSE</strong></td>
    <td>SLES 12</td>
    <td>3.12.36-38.1+</td>
    <td>SuSE-Sles-12-Priorität-v20150213<br>SuSE-Sles-12-v20150213</td>
</tr>
<tr>
    <td>SLES 11 SP4</td>
    <td>3.0.101-0.63.1+</td>
    <td> </td>
</tr>
<tr>
    <td><strong>CoreOS</strong></td>
    <td>584.0.0+</td>
    <td>3.18.4+</td>
    <td>CoreOS 584.0.0</td>
</tr>
<tr>
    <td rowspan="2"><strong>CentOS</strong></td>
    <td>6.5, 6.6, 6,7 7.0</td>
    <td></td>
    <td>
        <a href="http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409">LIS4 erforderlich</a> <br/>
        *Siehe Hinweis unten*
    </td>
</tr>
<tr>
    <td>7.1 +</td>
    <td>3.10.0-229.1.2.el7+</td>
    <td>
        <a href="http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409">Empfohlene LIS4</a> <br/>
        *Siehe Hinweis unten*
    </td>
</tr>
<tr>
    <td><strong>RHEL</strong></td>
    <td>6.8 +, 7.2 +</td>
    <td> </td>
    <td></td>
</tr>
<tr>
    <td rowspan="3"><strong>Oracle</strong></td>
    <td>6.8 +, 7.2 +</td>
    <td> </td>
    <td> UEK4 oder RHCK </td>

</tr>
<tr>
    <td>7.0-7.1</td>
    <td> </td>
    <td>UEK4 oder RHCK mit<a href="http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409">LIS 4.1 +</a></td>
</tr>
<tr>
    <td>6.4-6,7</td>
    <td></td>
    <td>UEK4 oder RHCK mit<a href="http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409">LIS 4.1 +</a></td>
</tr>
</tbody>
</table>


### <a name="lis-drivers-for-openlogic-centos"></a>LIS-Treiber für Openlogic CentOS

Ausführen von OpenLogic CentOS virtuellen Computern Kunden sollte den folgenden Befehl zum Installieren der neuesten Treiber ausführen:

    sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
    sudo yum install microsoft-hyper-v

Ein Neustart wird dann aktivieren Sie die neuen Treiber erforderlich sein.

## <a name="pricing-and-billing"></a>Preise und Abrechnung
Bei der Premium Speicherung Abrechnung unter folgenden Aspekten Ursachen zurückzuführen:
- Premium Speicher Datenträger / BLOB-Größe
- Premium Speicher Momentaufnahmen
- Ausgehende Datenübertragung

**Premium Speicher Datenträger / BLOB-Größe**: Abrechnung für die bereitgestellte Größe des Datenträgers/Blob ein Datenträger/Blob-Speicher Premium abhängt. Azure ordnet die bereitgestellte Größe (aufgerundet) die nächste Premium Speicher Datenträger Option in der Tabelle wird im Abschnitt [Skalierbarkeit und Leistung bei Verwendung von Premium Speicher](#premium-storage-scalability-and-performance-targets) angegeben sind. Alle Objekte in einem Konto Premium Speicher auf einen der Karte werden die nach der Bereitstellung Größen und werden in Rechnung gestellt werden entsprechend der unterstützt. Daher vermeiden Sie Premium Speicher-Konto zum Speichern von sehr Blobs. Abrechnung für alle bereitgestellte Datenträger/Blob ist anteilig stündlich mithilfe des monatlichen Preis für das Angebot Premium-Speicher. Wenn Sie einen Datenträger P10 bereitgestellt und dann nach 20 Stunden gelöscht, werden Sie beispielsweise Abrechnung für das Angebot P10 20 Stunden anteilig. Dies ist unabhängig von der Menge der tatsächlichen Daten auf den Datenträger geschrieben oder der IOPS-Durchsatz verwendet.

**Premium Speicher Momentaufnahmen**: Momentaufnahmen Premium Speichermenge für die zusätzliche Kapazität verwendet werden, indem Sie die Momentaufnahmen in Rechnung gestellt werden. Informationen zu Momentaufnahmen finden Sie unter [Erstellen einer Momentaufnahme eines Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).

**Ausgehende Datenübertragung**: Rechnung für die Verwendung der Bandbreite [ausgehende Datenübertragung](https://azure.microsoft.com/pricing/details/data-transfers/) (Daten aus Azure Data Center vertraut) zu tätigen.

Ausführliche Informationen zu Preisen für Premium-Speicher, Premium Speicher unterstützte virtuellen Computern finden Sie unter:

- [Preise Azure-Speicher](https://azure.microsoft.com/pricing/details/storage/)
- [Preise von virtuellen Computern](https://azure.microsoft.com/pricing/details/virtual-machines/)

## <a name="backup"></a>Sicherung
Virtuelle Maschinen mit Premium Speicher können mit Azure Sicherung gesichert werden. [Weitere Informationen hierzu](../backup/backup-azure-vms-first-look-arm.md).

## <a name="quick-start"></a>Schnellstart

## <a name="create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk"></a>Erstellen Sie und verwenden Sie ein Konto Premium Speicherplatz für einen Datenträger virtuellen Computern

In diesem Abschnitt werden wir die folgenden Szenarien mit Azure-Portal Azure PowerShell und Azure CLI zeigen:

- So erstellen Sie ein Konto Premium-Speicher.
- So erstellen Sie einen virtuellen Computer und des virtuellen Computers bei Verwendung von Premium Speicher einen Datenträger zuordnen.
- Informationen zum Ändern der Datenträger Zwischenspeichern Richtlinie für einen Datenträger, einen virtuellen Computer angefügt.

### <a name="create-an-azure-virtual-machine-using-premium-storage-via-the-azure-portal"></a>Erstellen einer Azure-virtuellen Computern mit Premium-Speicher über das Azure-portal

#### <a name="i-create-a-premium-storage-account-in-azure-portal"></a>Ich. Erstellen Sie ein Konto Premium Speicher Azure-Portal

In diesem Abschnitt veranschaulicht, wie ein Premium Speicher Konto über das Azure-Portal zu erstellen.

1.  Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com)aus. Schauen Sie sich [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) Angebot, wenn Sie nicht über ein Abonnement noch verfügen.

2. Wählen Sie im Menü Hub **neu** -> **Daten + Speicher** -> **Speicher-Konto**.

3. Geben Sie einen Namen für Ihr Speicherkonto aus.

    > [AZURE.NOTE] Speicher Kontonamen müssen zwischen 3 und 24 Zeichen lang sein und darf Zahlen und nur Kleinbuchstaben enthalten.
    >  
    > Ihren Kontonamen Speicher muss innerhalb Azure eindeutig sein. Azure-Portal wird angegeben, wenn Sie den Namen des Speicher-Kontos an, die, den Sie auswählen, die bereits verwendet wird.

4. Geben Sie das Modell zur Bereitstellung von zu verwendenden: **Ressourcenmanager** oder **klassische**. **Ressourcen-Manager** ist das Modell empfohlene Bereitstellung. Weitere Informationen finden Sie unter [Grundlegendes zu Ressourcenmanager und klassischen Bereitstellung](../resource-manager-deployment-model.md).

5. Geben Sie die Ebene Leistung für das Speicherkonto als **Premium**.

6. **Lokal redundanten Speicher (LRS)** ist die Replikationsoption nur verfügbar mit Premium-Speicher. Weitere Informationen zu Replikationsoptionen Azure-Speicher finden Sie unter [Replikation Azure-Speicher](storage-redundancy.md).

7. Wählen Sie das Abonnement aus, in dem das neue Speicherkonto erstellt werden soll.

8. Geben Sie eine neue Ressourcengruppe, oder wählen Sie eine vorhandene Ressourcengruppe aus. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht Azure Ressourcenmanager](../azure-resource-manager/resource-group-overview.md).

9. Wählen Sie die geografische Position für Ihr Speicherkonto aus. Sie können überprüfen, ob es sich bei Premium Speicher ausgewählten Standort zur Verfügung anhand [Azure Services nach Region](https://azure.microsoft.com/regions/#services).

10. Klicken Sie auf **Erstellen** , um das Speicherkonto zu erstellen.

#### <a name="ii-create-an-azure-virtual-machine-via-azure-portal"></a>II. Erstellen einer Azure-virtuellen Computern über Azure-portal

Sie müssen eine Premium-Speicher erstellen unterstützt virtueller Computer, um Premium-Speicher verwenden können. Führen Sie die Schritte in [Erstellen einem Windows Azure-Portal Computer](../virtual-machines/virtual-machines-windows-hero-tutorial.md) zum Erstellen eines neuen DS, DSv2, GS oder Fs virtuellen Computers aus.

#### <a name="iii-attach-a-premium-storage-data-disk-via-azure-portal"></a>III. Fügen Sie einen Datenträger Speicher Premium über Azure-portal

1. Suchen Sie den neuen oder vorhandenen DS, DSv2, GS, oder Fs virtueller Computer Azure-Portal.
2. In den virtuellen Computer **Alle Einstellungen**wechseln Sie zu **Datenträger** aus, und klicken Sie auf **Neue anfügen**.
3. Geben Sie den Namen der Festplatte Daten, und wählen Sie den **Typ** als **Premium**aus. Wählen Sie die gewünschte **Größe** und **Host Zwischenspeichern** Einstellung aus.

    ![Premium Datenträger][Image1]

[So fügen Sie einen Datenträger Azure-Portal](../virtual-machines/virtual-machines-windows-attach-disk-portal.md)Schritten ausführlichere finden Sie unter.

#### <a name="iv-change-disk-caching-policy-via-azure-portal"></a>IV. Ändern der Datenträger Zwischenspeichern Richtlinie über Azure-portal

1. Suchen Sie den neuen oder vorhandenen DS, DSv2, GS, oder Fs virtueller Computer Azure-Portal.
2. Klicken Sie unter Alle Einstellungen virtueller Computer in Datenträger wechseln Sie, und klicken Sie auf dem Datenträger, die, den Sie ändern möchten.
3. Ändern Sie die Option aus, um den gewünschten Wert, Zwischenspeichern Host keine oder schreibgeschützt oder Lesen/Schreiben

>[AZURE.WARNING] Ändern die Einstellung Cache eines Azure-Datenträgers trennt und wieder angefügt wird den Zieldatenträger. Es ist der Datenträger Betriebssystem der virtuellen Computer neu gestartet wurde. Beenden Sie alle Anwendungen/Dienste, die von diesem Unterbrechung vor dem Ändern der Datenträger-Cache-Einstellung betroffen werden könnten.

### <a name="create-an-azure-virtual-machine-using-premium-storage-via-azure-powershell"></a>Erstellen einer Azure mit Premium-Speicher über PowerShell Azure-virtuellen Computern

#### <a name="i-create-a-premium-storage-account-in-azure-powershell"></a>Ich. Erstellen Sie ein Konto Premium Speicher in Azure PowerShell

In diesem Beispiel PowerShell zeigt so erstellen ein neues Premium Speicher-Konto, und fügen Sie einen Datenträger, für der Verwendung dieses Kontos zu einer neuen Azure-virtuellen Computern an.

1. Richten Sie Ihrer Umgebung PowerShell gemäß die Anweisungen zum [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)versehen ein.
2. Starten Sie die PowerShell-Konsole, Herstellen einer Verbindung Ihres Abonnements mit, und führen Sie das folgende PowerShell-Cmdlet Console-Fenster. Wie in der folgenden Anweisung PowerShell gesehen, müssen Sie beim Erstellen eines Kontos Premium Speicher **Type** -Parameter als **Premium_LRS** angeben.

        New-AzureStorageAccount -StorageAccountName "yourpremiumaccount" -Location "West US" -Type "Premium_LRS"

#### <a name="ii-create-an-azure-virtual-machine-via-azure-powershell"></a>II. Erstellen einer Azure-virtuellen Computern über Azure PowerShell

Erstellen eines neuen DS-Serie virtuellen Computers als Nächstes und angeben, dass die gewünschte Premium Speicher durch Ausführen der folgenden PowerShell-Cmdlets im Fenster Konsole. Sie können einen GS-Serie virtuellen Computer mit denselben Schritten erstellen. Geben Sie die entsprechenden virtueller Speicher Befehle aus. Für z. B. Standard_GS2:

        $storageAccount = "yourpremiumaccount"
        $adminName = "youradmin"
        $adminPassword = "yourpassword"
        $vmName ="yourVM"
        $location = "West US"
        $imageName = "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201409.01-en.us-127GB.vhd"
        $vmSize ="Standard_DS2"
        $OSDiskPath = "https://" + $storageAccount + ".blob.core.windows.net/vhds/" + $vmName + "_OS_PIO.vhd"
        $vm = New-AzureVMConfig -Name $vmName -ImageName $imageName -InstanceSize $vmSize -MediaLocation $OSDiskPath
        Add-AzureProvisioningConfig -Windows -VM $vm -AdminUsername $adminName -Password $adminPassword
        New-AzureVM -ServiceName $vmName -VMs $VM -Location $location

#### <a name="iii-attach-a-premium-storage-data-disk-via-azure-powershell"></a>III. Fügen Sie einen Datenträger Speicher Premium über Azure PowerShell

Sie bei Bedarf mehr Speicherplatz für Ihre virtuellen Computer, fügen Sie einen neuen Datenträger zu einer vorhandenen unterstützt Premium Speicher virtueller Computer, nachdem sie erstellt wurde, indem Sie die folgenden PowerShell-Cmdlets im Fenster Konsole ausführen:

        $storageAccount = "yourpremiumaccount"
        $vmName ="yourVM"
        $vm = Get-AzureVM -ServiceName $vmName -Name $vmName
        $LunNo = 1
        $path = "http://" + $storageAccount + ".blob.core.windows.net/vhds/" + "myDataDisk_" + $LunNo + "_PIO.vhd"
        $label = "Disk " + $LunNo
        Add-AzureDataDisk -CreateNew -MediaLocation $path -DiskSizeInGB 128 -DiskLabel $label -LUN $LunNo -HostCaching ReadOnly -VM $vm | Update-AzureVm

#### <a name="iv-change-disk-caching-policy-via-azure-powershell"></a>IV. Ändern Sie die Richtlinie über Azure PowerShell Zwischenspeichern Datenträger

Klicken Sie zum Aktualisieren des Datenträgers Zwischenspeichern Richtlinie Beachten Sie die LUN-Nummer des Datenträgers Daten angefügt. Führen Sie den folgenden Befehl zum Aktualisieren von Daten Datenträger auf LUN-Nummer 2, um schreibgeschützten angeschlossen.

        Get-AzureVM "myservice" -name "MyVM" | Set-AzureDataDisk -LUN 2 -HostCaching ReadOnly | Update-AzureVM

>[AZURE.WARNING] Ändern die Einstellung Cache eines Azure-Datenträgers trennt und wieder angefügt wird den Zieldatenträger. Es ist der Datenträger Betriebssystem der virtuellen Computer neu gestartet wurde. Beenden Sie alle Anwendungen/Dienste, die von diesem Unterbrechung vor dem Ändern der Datenträger-Cache-Einstellung betroffen werden könnten.

### <a name="create-an-azure-virtual-machine-using-premium-storage-via-the-azure-command-line-interface"></a>Erstellen einer Azure-virtuellen Computern Premium Speicher über die Azure Line Benutzeroberfläche verwenden

Die [Azure Line Interface](../xplat-cli-install.md)(CLI Azure) bietet eine bietet eine Reihe von open Source Plattform-Befehle für die Arbeit mit Azure-Plattform. Den folgenden Beispielen wird veranschaulicht, wie Azure CLI verwenden (Version 0.8.14 und höher) erstellen Sie ein Konto Premium Speicher, einen neuen virtuellen Computer, und fügen Sie einen neuen Datenträger von einem Konto Premium-Speicher.

#### <a name="i-create-a-premium-storage-account-via-azure-cli"></a>Ich. Erstellen Sie ein Konto Premium Speicher über Azure CLI

````
azure storage account create "premiumtestaccount" -l "west us" --type PLRS
````

#### <a name="ii-create-a-ds-series-virtual-machine-via-azure-cli"></a>II. Erstellen eines DS-Serie virtuellen Computers über Azure CLI

    azure vm create -z "Standard_DS2" -l "west us" -e 22 "premium-test-vm"
        "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_10-amd64-server-20150202-en-us-30GB" -u "myusername" -p "passwd@123"

Anzeigen von Informationen zu den virtuellen Computern

    azure vm show premium-test-vm

#### <a name="iii-attach-a-new-premium-data-disk-via-azure-cli"></a>III. Fügen Sie einen neuen Datenträger über Azure CLI premium

    azure vm disk attach-new premium-test-vm 20 https://premiumstorageaccount.blob.core.windows.net/vhd-store/data1.vhd

Anzeigen von Informationen zu den neuen Daten Datenträger

    azure vm disk show premium-test-vm-premium-test-vm-0-201502210429470316

#### <a name="iv-change-disk-caching-policy"></a>IV. Ändern Datenträger Zwischenspeichern Richtlinie

Wenn die Cacherichtlinie auf einem Ihrer Datenträger mit Azure CLI ändern möchten, führen Sie den folgenden Befehl aus:

        $ azure vm disk attach -h ReadOnly <VM-Name> <Disk-Name>

Beachten Sie, dass die Option für das Zwischenspeichern Richtlinie können schreibgeschützt, keine, oder lesen/schreiben. Weitere Optionen finden Sie in der Hilfe, indem Sie den folgenden Befehl ausführen:

        azure vm disk attach --help

>[AZURE.WARNING] Ändern die Einstellung Cache eines Azure-Datenträgers trennt und verbindet wieder auf den Zieldatenträger. Es ist der Datenträger Betriebssystem der virtuellen Computer neu gestartet wurde. Beenden Sie alle Anwendungen/Dienste, die von diesem Unterbrechung vor dem Ändern der Datenträger-Cache-Einstellung betroffen werden könnten.

## <a name="faqs"></a>Häufig gestellte Fragen

1. **Können sowohl Premium Anfügen und unterstützt Standarddaten Datenträger in einem Speicher Premium virtueller Computer?**

    Ja. Sie können in eine Reihe Premium Speicher unterstützt virtueller Computer sowohl Premium und Standarddaten Datenträger anfügen.

2. **Können sowohl Premium und Standarddaten Datenträger in eine Reihe "D", "Dv2", "G" oder "W" virtuellen Computer werden angefügt?**

    Nein. Sie können alle virtuellen Computern, die nicht sind, dass Premium Speicher Reihe unterstützt, nur einen Datenträger Standarddaten zuordnen.

3. **Wenn ich einen Datenträger Premium aus eine vorhandene virtuelle, die 80 GB Größe wurde erstellen, wie viel kostet, die mir?**

    Ein Datenträger Premium 80 GB virtuelle Festplatte Dokumentvorlagen werden als die nächste verfügbare Premium Datenträgergröße, einen Datenträger P10 behandelt. Sie werden gemäß der Datenträger P10 Preise in Rechnung gestellt.

4. **Gibt es Transaktionskosten Premium Speicher verwenden?**

    Es gibt eine Fixkosten für jeden Datenträgergröße, die mit einer bestimmten Anzahl von IOPS und Durchsatz bereitgestellte stammt. Nur andere Kosten werden ausgehende Bandbreite und Momentaufnahmen Kapazität, falls zutreffend. Weitere Informationen hierzu finden Sie unter [Azure Speicher Preise](https://azure.microsoft.com/pricing/details/storage/) .

5. **Wo kann ich Boot Diagnose Meine Investition ohne Finanzierungskosten Premium Speicher unterstützt virtuellen Computer speichern?**

    Erstellen eines standard-Speicher-Kontos zum Speichern der Boot Diagnose Ihrer Premium Speicher unterstützt Reihe virtueller Computer.

6. **Wie viele IOPS und Durchsatz kann ich aus dem Datenträgercache erhalten?**

    Die kombinierte Grenzwerte für Cache und lokale SSD einer Investition ohne Finanzierungskosten DS sind 4000 IOPS pro Core und 33 MB pro Sekunde pro Core. GS-Serie bietet 5000 IOPS pro Core und 50 MB pro Sekunde pro Core.

7. **Was ist, dass die lokale SSD in einer Premium-Speicher unterstützt Reihe virtueller Computer?**

    Das lokale SSD ist eine temporäre Speicher, der mit einer Datenreihe Premium Speicher unterstützt virtueller Computer enthalten ist. Es ist keine zusätzliche für diese temporäre Speicherung Kosten. Es wird empfohlen, dass Sie nicht verwenden dieser temporäre Speicher oder eine lokale SSD für Ihre Anwendungsdaten speichern, wie es in Azure BLOB-Speicher nicht beibehalten werden.

8. **Kann ich mein Speicherkonto standard-mit einem Konto Premium Speicher konvertieren?**

    Nein. Es ist nicht möglich, standard-Speicherkonto Premium Speicher Konto oder umgekehrt konvertieren. Sie müssen ein neues Speicherkonto mit den gewünschten Typ erstellen und Kopieren von Daten mit neuen Speicherkonto, falls zutreffend.

9. **Wie kann ich meine D Serie virtueller Computer in einer Serie DS virtueller Computer konvertieren?**

    Lizenzinformationen finden Sie die Migrationshandbuch [Migrieren nach Azure Premium Speicher](storage-migration-to-premium-storage.md) Verschieben Ihrer Arbeitsbelastung aus einer Reihe D virtueller Computer mit standardmäßigen Speicher-Konto in eine Reihe DS virtueller Computer Premium Speicher-Konto verwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Azure Premium Speicher finden Sie in den folgenden Artikeln.

### <a name="design-and-implement-with-azure-premium-storage"></a>Entwerfen und implementieren mit Azure Premium Storage

- [Entwurf, um die Leistung mit Premium-Speicher](storage-premium-storage-performance.md)
- [Verwenden von Blob-Dienstvorgänge mit Azure Premium-Speicher](http://go.microsoft.com/fwlink/?LinkId=521969)

### <a name="operational-guidance"></a>Betrieb Anleitungen

- [Migrieren zu Premium Azure-Speicher](storage-migration-to-premium-storage.md)

### <a name="blog-posts"></a>Von Blogbeiträgen

- [In der Regel verfügbar Azure Premium-Speicher](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/)
- [Ankündigung der GS-Serie: Hinzufügen von Premium Speicher Unterstützung zu den größten virtuellen Computern, in der öffentlichen Cloud](https://azure.microsoft.com/blog/azure-has-the-most-powerful-vms-in-the-public-cloud/)

[Image1]: ./media/storage-premium-storage/Azure_attach_premium_disk.png
