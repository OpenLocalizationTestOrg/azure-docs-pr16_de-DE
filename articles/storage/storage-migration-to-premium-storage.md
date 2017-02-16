<properties
    pageTitle="Migrieren zu Azure Premium Speicher | Microsoft Azure"
    description="Migrieren Sie die vorhandenen virtuellen Computer zu Azure Premium Speicher. Premium-Speicher bietet leistungsfähige und niedrig Wartezeiten Datenträger Unterstützung für I/O-Auslastung Auslastung auf Azure virtuellen Computern ausgeführt."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="yuemlu"/>


# <a name="migrating-to-azure-premium-storage"></a>Migrieren zu Premium Azure-Speicher

## <a name="overview"></a>(Übersicht)

Azure Premium-Speicher bietet leistungsfähige und niedrig Wartezeiten Datenträger Unterstützung für virtuellen Computern ich/O-Auslastung Auslastung ausgeführt werden. Vorteile der Geschwindigkeit und Leistung von diese Datenträger dauert nach Ihrer Anwendung virtueller Computer Datenträger zum Azure Premium Speicher migrieren.

Dieses Handbuch dient damit neue Benutzer Azure Premium Speicher besser vorbereiten einen Übergang von ihr aktuelles System zum Premium Speicher vornehmen. Des Leitfadens Adressen drei wichtigsten Komponenten in diesem Prozess: 

  - [Planen der Migrations zu Premium-Speicher](#plan-the-migration-to-premium-storage)
  - [Bereiten Sie vor und kopieren Sie virtuelle Festplatten (virtuelle Festplatten) Premium-Speicher](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
  - [Erstellen von Azure virtuellen Computern mit Premium-Speicher](#create-azure-virtual-machine-using-premium-storage)

Sie können Migrieren von virtuellen Computern von anderen Plattformen zu Azure Premium Speicher oder Migrieren der vorhandenen Azure-virtuellen Computern standardmäßigen Speicher auf Premium-Speicher. In diesem Handbuch werden die Schritte für beide folgenden beiden Szenarien behandelt. Führen Sie die Schritte im entsprechenden Abschnitt abhängig von Ihrem Szenario angegeben.

>[AZURE.NOTE] Finden Sie eine Übersicht über die Features und Preise Premium-Speicher im Speicher Premium: [Leistungsstarke Storage für Azure-virtuellen Computern Auslastung](storage-premium-storage.md). Es empfiehlt sich, alle virtuellen Computern Datenträger mit hoher IOPS zu Azure Premium Speicher für die optimale Leistung für eine Anwendung Anforderung migrieren. Wenn der Datenträger keine hohe IOPS erforderlich sind, können Sie Kosten beschränken, indem Sie es in Standard-Speicher, der virtuellen Computern Datenträgerdaten auf Festplatten (Festplatten) anstelle von SSDs gespeichert verwalten.

Zum Abschluss der Migration vollständig möglicherweise zusätzliche Aktionen vor und nach der Schritte in diesem Handbuch erforderlich. Beispiele: virtuelle Netzwerke oder Endpunkte konfigurieren oder Ändern der Code innerhalb der Anwendung selbst die langweilig in Ihrer Anwendung erfordern. Diese Aktionen stehen nur in jede Anwendung, und schließen Sie sie zusammen mit den Schritten in diesem Leitfaden für den vollständigen Übergang zu Premium Speicher als nahtlose wie möglich.


## <a name="a-nameplan-the-migration-to-premium-storageaplan-for-the-migration-to-premium-storage"></a><a name="plan-the-migration-to-premium-storage"></a>Planen der Migrations zu Premium-Speicher

In diesem Abschnitt wird sichergestellt, dass Sie bereit sind, führen Sie die Migrationsschritte in diesem Artikel, und hilft Ihnen die optimale Entscheidung virtueller Computer und dem Datenträger Typen vornehmen.

### <a name="prerequisites"></a>Erforderliche Komponenten
- Sie benötigen ein Azure-Abonnement. Wenn Sie eine besitzen, können Sie ein Abonnement für einen Monat [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) erstellen oder finden Sie weitere Optionen auf [Azure Preise](https://azure.microsoft.com/pricing/) .
- Wenn PowerShell-Cmdlets ausführen möchten, benötigen Sie das Microsoft Azure PowerShell-Modul. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) für die Installation Punkt und Installation Anweisungen.
- Wenn Sie beabsichtigen, Azure-virtuellen Computern ausgeführt Premium Speicher verwenden, müssen Sie die in virtuellen Computern Premium Speicher verwenden. Sie können sowohl Standard- und Premium Speicher Datenträger mit Premium Speicher in virtuellen Computern verwenden. Premium-Datenträger stehen in der Zukunft weitere virtueller Computer Arten zur Verfügung. Weitere Informationen zum aller verfügbaren Azure-virtuellen Computer Datenträger Arten und Größen finden Sie unter [Größen für virtuellen Computern](../virtual-machines/virtual-machines-windows-sizes.md) und [Größen für Cloud-Dienste](../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Aspekte

Ein Azure-virtuellen Computer unterstützt mehrere Premium-Datenträger anfügen, damit Ihre Applikationen bis zu 64 TB Speicherplatz pro virtueller Computer haben, können. Mit Premium-Speicher erreichen Ihrer Anwendung 80.000 IOPS (e/a-Vorgänge pro Sekunde) pro virtueller Computer und 2000 MB pro zweiten Datenträgerdurchsatz pro virtueller Computer mit sehr niedrig Wartezeiten für Vorgänge finden Sie hier. Sie haben die Optionen in einer Vielzahl von virtuellen Computern und Festplatten. In diesem Abschnitt besteht darin, die Ihnen dabei helfen, eine Option zu finden, die Ihre Arbeitsbelastung am besten passt.

#### <a name="vm-sizes"></a>Virtueller Computer Größen
Spezifikationen Größe Azure-virtuellen Computer werden in [Größen für virtuellen Computern](../virtual-machines/virtual-machines-windows-sizes.md)aufgeführt. Überprüfen Sie die Leistungsmerkmale virtuellen Computern, die Arbeiten mit Premium-Speicher, und wählen Sie das am besten geeignete virtueller Speicher, das Ihre Arbeitsbelastung am besten passt. Stellen Sie sicher, dass Ihre virtuellen Computers zu den Datenverkehr Datenträger drive ausreichende Bandbreite vorhanden ist.


#### <a name="disk-sizes"></a>Datenträgergrößen
Es gibt drei Typen von Datenträger, die mit Ihrem virtuellen Computer verwendet werden können, und jeder hat bestimmte IOPs und Durchsatz Grenzwerte. Berücksichtigen Sie diese Grenzwerte beim Auswählen des Typs der Datenträger für Ihre virtuellen Computer auf den Anforderungen der Anwendung im Hinblick auf Kapazität, Leistung und Skalierbarkeit basierend und Höchstwert lädt.

|Premium Speicher Datenträgertyp|P10|P20|P30|
|:---:|:---:|:---:|:---:|
|Größe des Datenträger|128 GB|512 GB|1024 GB (1 TB)|
|IOPS pro Datenträger|500|2300|5000|
|Durchsatz pro Datenträger|100 MB pro Sekunde|150 MB pro Sekunde|200 MB pro Sekunde|

Abhängig von Ihrer Arbeitsbelastung festzustellen Sie, ob zusätzliche Datenträger für Ihre virtuellen Computer erforderlich sind. Sie können Ihre virtuellen Computer mehrere permanente Daten Datenträger zuordnen. Bei Bedarf können Sie auf dem Datenträger zum Steigern der Kapazität und Leistung von der Lautstärke verteilen. (Finden Sie unter Was Festplattenstriping ist [hier](storage-premium-storage-performance.md#disk-striping).) Wenn Sie Premium Daten Datenträger mit [Speicher Leerzeichen]versehen[4], sollten Sie es mit einer Spalte für jedes Laufwerk, das verwendet wird, konfigurieren. Andernfalls möglicherweise die allgemeine Leistung von der aufgeteilten Lautstärke erwarteten aufgrund ungleiche Verteilung der Verkehr über die Laufwerke vorangehen. Das Programm *Mdadm* können Sie für Linux virtuellen Computern um gleich zu erzielen. Finden Sie im Artikel [Konfigurieren der Software RAID auf Linux](../virtual-machines/virtual-machines-linux-configure-raid.md) Details.

#### <a name="storage-account-scalability-targets"></a>Speicher Konto Skalierbarkeit Ziele
Premium Speicherkonten haben die folgenden Skalierbarkeit Ziele sowie die [Leistung und Skalierbarkeit der Azure-Speicher](storage-scalability-targets.md). Ihren Anforderungen für die Anwendung, die größer sind als der Skalierbarkeit Ziele eines einzelnen Speicher-Kontos, erstellen Sie die Anwendung mit mehreren Speicherkonten, und Partitionieren von Daten über diese Speicherkonten hinweg.

|Total Kapazität|Gesamte Bandbreite für ein Speicherkonto lokal redundante|
|:--|:---|
|Datenträger Kapazität: 35TB<br />Snapshot-Kapazität: 10 TB|Bis zu 50 GB/s pro Sekunde für eingehende + ausgehend|

Weitere Informationen Premium Speicher Spezifikationen checken Sie [Skalierbarkeit und Leistung bei Verwendung von Premium-Speicher aus](storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

#### <a name="disk-caching-policy"></a>Datenträger Zwischenspeichern Richtlinie
Standardmäßig ist die Richtlinie Zwischenspeichern Datenträger *Schreibgeschützt* für alle Premium Daten Datenträger und *Lese-und Schreibzugriff* für den Premium Betriebssystem Datenträger angefügter den virtuellen Computer. Diese Einstellung Konfiguration wird empfohlen, um die optimale Leistung für IOs Ihrer Anwendung zu erzielen. Deaktivieren Sie für Datenträger überladene schreiben oder nur Schreiben von Daten (wie etwa SQL Server-Protokolldateien) Zwischenspeichern auf der Festplatte, damit Sie eine bessere Leistung der Anwendung erreichen können. Die Einstellungen des Caches für vorhandene Daten Datenträger können über [Azure-Portal](https://portal.azure.com) oder den *HostCaching -* Parameter des Cmdlets *Set-AzureDataDisk* aktualisiert werden.

#### <a name="location"></a>Speicherort
Wählen Sie einen Speicherort, wo Azure Premium Speicher verfügbar ist. Aktuelle Informationen über die verfügbaren Speicherorte finden Sie unter [Azure Services nach Region](https://azure.microsoft.com/regions/#services) . Virtuelle Computer befindet sich in derselben Region als das Konto Speicher, dass Stores Datenträger für den virtuellen Computer viel bessere Leistung erhält als bei in separaten Regionen.

#### <a name="other-azure-vm-configuration-settings"></a>Azure-virtuellen Computer her
Beim Erstellen einer Azure-virtuellen Computer werden Sie aufgefordert, um bestimmte virtueller Computer zu konfigurieren. Beachten Sie, dass einige Einstellungen für die Gültigkeitsdauer von dem virtuellen Computer behoben werden, während Sie andere später hinzufügen oder ändern können. Überprüfen Sie diese Einstellungen der Azure-virtuellen Computer-Konfiguration, und stellen Sie sicher, dass diese entsprechend Ihren Anforderungen Arbeitsbelastung ordnungsgemäß konfiguriert sind.

### <a name="optimization"></a>Optimierung

[Azure Premium Speicher: Entwurf für eine hohe Leistung](storage-premium-storage-performance.md) bietet Richtlinien zum Erstellen von leistungsfähige Clientanwendungen Azure Premium Speicher verwenden. Sie können die Richtlinien zusammen mit der Leistung bewährte Methoden anwendbar Technologien, die von der Anwendung verwendeten folgen.


## <a name="a-nameprepare-and-copy-virtual-hard-disks-vhds-to-premium-storageaprepare-and-copy-virtual-hard-disks-vhds-to-premium-storage"></a><a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Bereiten Sie vor und kopieren Sie virtuellen Festplatten (virtuelle Festplatten) Premium-Speicher

Im folgende Abschnitt bietet Richtlinien für virtuelle Festplatten aus Ihrer virtuellen Computer vorbereiten und Kopieren von virtuellen Festplatten in Azure-Speicher.

- [Szenario 1: "Ich bin vorhandene Azure-virtuellen Computern zu Azure Premium-Speicher migrieren".](#scenario1)
- [Szenario 2: "Ich bin virtuellen Computern von anderen Plattformen zu Azure Premium-Speicher migrieren".](#scenario2)

### <a name="prerequisites"></a>Erforderliche Komponenten

Um die virtuellen Festplatten zur Migration vorzubereiten, müssen Sie:

- Ein Azure-Abonnement, Speicher-Konto und eines Containers in diesem Storage-Konto, das Sie Ihre virtuelle Festplatte kopieren können. Beachten Sie, dass das Ziel-Speicher-Konto ein Konto Standard oder Premium Speicher je nach Ihrer Anforderung werden kann.
- Ein Tool an die virtuelle Festplatte generalize, wenn Sie beabsichtigen, mehrere Instanzen von virtuellen Computer daraus zu erstellen. Sysprep für Windows oder virtueller Pufferzeiger-Sysprep für Ubuntu.
- Ein Tool zum Hochladen der Datei virtuelle Festplatte mit dem Speicher-Konto. Finden Sie unter [Übertragen von Daten mit den AzCopy Befehlszeilenprogramm](storage-use-azcopy.md) , oder verwenden Sie eine [Azure-Speicher-Explorer](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx). Dieses Handbuch beschreibt Ihre virtuelle Festplatte mithilfe des Tools AzCopy kopieren.

> [AZURE.NOTE] Falls gewünscht synchroner kopieren Kopieroption mit AzCopy, um optimale Leistung zu Ihrer virtuelle Festplatte durch eines der folgenden Tools aus einer Azure-virtuellen Computer, der in derselben Region als die Ziel-Speicher-Konto wird ausgeführt. Wenn Sie eine virtuelle Festplatte aus einer Azure-virtuellen Computer in einem anderen Bereich kopieren, kann die Leistung verlangsamt werden.
>
> Für eine große Datenmenge über eingeschränkter Bandbreite kopieren, bietet [den Azure Import/Export-ab, um Daten zu Blob-Speicher übertragen](storage-import-export-service.md); So können Sie Ihre Daten durch Versand Festplatten zu einer Azure Datacenter übertragen. Den Azure Import/Export-Dienst können Sie um Daten in einem standardmäßigen Speicher-Konto zu kopieren. Sobald die Daten in Ihrem standardmäßigen Speicher-Konto ist, können Sie die Daten zu Ihrem Premium Speicherkonto übertragen der [Blob-API kopieren](https://msdn.microsoft.com/library/azure/dd894037.aspx) oder AzCopy verwenden.
>
> Beachten Sie, dass Microsoft Azure nur feste Größe virtuelle Festplatte Dateien unterstützt. VHDX Dateien oder dynamischen virtuellen Festplatten werden nicht unterstützt. Wenn Sie eine dynamische virtuelle Festplatte verfügen, können Sie es auf feste Größe mithilfe des Cmdlets [Konvertieren-virtuelle Festplatte](http://technet.microsoft.com/library/hh848454.aspx) konvertieren.

### <a name="a-namescenario1ascenario-1-i-am-migrating-existing-azure-vms-to-azure-premium-storage"></a><a name="scenario1"></a>Szenario 1: "Ich bin vorhandene Azure-virtuellen Computern zu Azure Premium-Speicher migrieren".

Wenn Sie vorhandene Azure-virtuellen Computern migrieren, beenden Sie den virtuellen Computer, bereiten Sie virtueller Festplatten pro den Typ des virtuellen Festplatte werden sollen vor, und kopieren Sie die virtuelle Festplatte mit AzCopy oder PowerShell.

Der virtuellen Computer muss vollständig nach unten zum Migrieren von eines übersichtlichen Zustands werden. Bis zum Abschluss der Migrations werden eine Ausfallzeit vor.

#### <a name="step-1-prepare-vhds-for-migration"></a>Schritt 1. Vorbereiten virtueller Festplatten für die migration

Wenn Sie vorhandene Azure-virtuellen Computern zu Premium Speicher migrieren, kann der virtuelle Festplatte haben:

- Ein Bild GRG-Betriebssystem
- Einen eindeutigen Betriebssystem Datenträger
- Einen Datenträger

Im folgenden durchgehen wir diese 3 Szenarios für Ihre virtuelle Festplatte vorbereiten.

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a>Verwenden einer GRG Betriebssystem virtuellen mehrerer Instanzen von virtuellen Computer erstellen

Wenn Sie eine virtuelle Festplatte hochladen, die zum Erstellen von mehreren generischer Azure-virtuellen Computer Instanzen verwendet werden, müssen Sie zuerst virtuellen Festplatte mithilfe einer Systemvorbereitungsprogramm verallgemeinern. Dies gilt eine virtuelle, der lokalen Festplatte oder in der Cloud. Sysprep entfernt alle Computer-spezifische Informationen von der virtuellen Festplatte an.

>[AZURE.IMPORTANT] Eine Momentaufnahme oder Ihrer virtuellen Computer sichern, bevor Sie es verallgemeinern. Ausführen von Sysprep wird beendet und die Instanz virtueller Computer freigeben. Führen Sie die folgenden Schritte aus um Sysprep eine virtuelle Festplatte Windows-Betriebssystem aus. Beachten Sie, dass Ausführen des Befehls Sysprep Sie zum Beenden des virtuellen Computers erforderlich ist. Weitere Informationen zu Sysprep finden Sie unter [Übersicht über die Sysprep](http://technet.microsoft.com/library/hh825209.aspx) oder [Technische Referenz zu Sysprep](http://technet.microsoft.com/library/cc766049.aspx).

1. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator an.
2. Geben Sie den folgenden Befehl aus, um Sysprep zu öffnen:

        %windir%\system32\sysprep\sysprep.exe

3. Im System Vorbereitung Tool select System geben Out-of-Box-Experience (OOBE), aktivieren Sie das Kontrollkästchen verallgemeinern, wählen Sie **war(en)**und klicken Sie dann auf **OK**, wie in der nachstehenden Abbildung gezeigt. Sysprep wird das Betriebssystem generalize, und fahren Sie das System.

    ![][1]

Verwenden Sie für eine VM Ubuntu virtueller Pufferzeiger-Sysprep identisch zu erzielen. [Virtueller Pufferzeiger-Sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) Weitere Informationen hierzu finden Sie unter. Siehe auch einige der geöffneten Quelle [Linux Server-Bereitstellung von Software](http://www.cyberciti.biz/tips/server-provisioning-software.html) für andere Betriebssysteme Linux.

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a>Verwenden Sie zum Erstellen einer einzelnen Instanz der virtueller Computer eine eindeutige Betriebssystem virtuelle Festplatte

Wenn Sie eine Anwendung Ausführen des virtuellen Computers die die Computer bestimmte Daten erforderlich ist haben, führen Sie die virtuelle Festplatte keine Generalisierung. Eine virtuelle nicht GRG Festplatte kann zum Erstellen einer eindeutigen Instanz von Azure virtueller Computer verwendet werden. Beispielsweise, wenn Sie auf der virtuellen Festplatte Domain Controller verfügen, wird Sysprep ausführen nicht als Domänencontroller wirksam erleichtern. Überprüfen Sie die Anwendungen für Ihre virtuellen Computer und den Einfluss der Sysprep auf diese ausführen, bevor Sie die virtuelle Festplatte verallgemeinern ausgeführt.

##### <a name="register-data-disk-vhd"></a>Register Daten Datenträger virtuelle Festplatte

Wenn Sie Daten Datenträger in Azure zu migrierende haben, müssen Sie sicherstellen, dass die virtuellen Computern, die diese Daten Datenträger verwenden ausgeschaltet werden.

Folgen Sie den Schritten unter virtuelle Festplatte auf Azure Premium Speicher kopieren und als Datenträger bereitgestellte Daten zu registrieren.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Schritt 2. Erstellen Sie das Ziel für Ihre virtuelle Festplatte

Erstellen Sie ein Speicherkonto für die Verwaltung Ihrer virtuellen Festplatten. Berücksichtigen Sie die folgenden Punkte, beim Planen, wo Ihre virtuellen Festplatten gespeichert:

- Das Ziel Premium Speicher-Konto.
- Der Speicherort für das Konto muss Premium Speicher in Azure-virtuellen Computern identisch sein, die in der letzten Phase erstellt werden. Sie können ein Neukunde Speicher oder Plan mit dem gleichen Speicherkonto entsprechend Ihren Anforderungen kopieren.
- Kopieren Sie und speichern Sie den Schlüssel Speicher Konto das Ziel-Speicher-Konto für die nächste Phase.

Für Daten Datenträger Sie können auch einige Datenträger Daten in einem standardmäßigen Speicher-Konto (z. B. Datenträger, die besser Speicher aufweisen) beibehalten, aber es wird dringend empfohlen Sie alle Daten für die Herstellung Arbeitsbelastung mit Premium Speicher verschieben.

#### <a name="a-namecopy-vhd-with-azcopy-or-powershellastep-3-copy-vhd-with-azcopy-or-powershell"></a><a name="copy-vhd-with-azcopy-or-powershell"></a>Schritt 3. Kopieren Sie die virtuelle Festplatte mit AzCopy oder PowerShell

Sie müssen Ihren Container Pfad und Speicher kontoschlüssel Verarbeitungszeit diese beiden Optionen zu suchen. Container Pfad und Speicher kontoschlüssel finden Sie im **Portal Azure** > **Speicher**. Die URL des Containers werden wie "https://myaccount.blob.core.windows.net/mycontainer/".

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>Option 1: Kopieren einer virtuellen Festplatte mit AzCopy (asynchrone Kopie)

Verwenden AzCopy, können Sie einfach die virtuelle Festplatte über das Internet hochladen. Je nach der Größe der virtuellen Festplatten kann dies Zeit dauern. Denken Sie daran, aktivieren Sie das Konto eingehende/Ausgang Speichergrenzwerte bei Verwendung dieser Option werden soll. Details finden Sie unter [Leistung und Skalierbarkeit der Azure-Speicher](storage-scalability-targets.md) .

1. Herunterladen und Installieren von AzCopy hier: [neueste Version von AzCopy](http://aka.ms/downloadazcopy)
2. Öffnen Sie Azure PowerShell zu, und wechseln Sie zu dem Ordner, in dem AzCopy installiert ist.
3. Verwenden Sie den folgenden Befehl aus, um die Datei virtuelle Festplatte aus "Quelle" an "Ziel" zu kopieren.

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Beispiel:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd

    Es folgen einige Beschreibungen der Parameter in den Befehl AzCopy verwendet:

 - * */Source: * &lt;Quelle&gt;:*** Speicherort des Ordners oder der Container Speicher-URL, die die virtuelle Festplatte enthält.
 - * */SourceKey: * &lt;Quelle kontoschlüssel&gt;:*** Speicher kontoschlüssel des Kontos Speicher Quelle.
 - * */Dest: * &lt;Ziel&gt;:*** Speicher Container-URL, um die virtuelle Festplatte zu kopieren.
 - * */DestKey: * &lt;Ziel kontoschlüssel&gt;:*** Speicher kontoschlüssel für das Ziel-Speicher-Konto.
 - * *-Block: * &lt;Dateiname&gt;:*** geben den Dateinamen an der virtuellen Festplatte zu kopieren.

Details zur Verwendung von AzCopy Tool finden Sie unter [Übertragen von Daten mit den AzCopy Befehlszeilenprogramm](storage-use-azcopy.md).

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>Option 2: Kopieren einer virtuellen Festplatte mit PowerShell (synchronisierte Kopie)

Sie können auch die virtuelle Festplatte Datei mithilfe des PowerShell-Cmdlets Start-AzureStorageBlobCopy kopieren. Verwenden Sie den folgenden Befehl auf Azure PowerShell virtuelle Festplatte zu kopieren. Ersetzen Sie die Werte in <> mit entsprechenden Werten aus der Quell- und Zielfelder Speicher-Konto an. Wenn Sie diesen Befehl verwenden zu können, müssen Sie einen Container namens virtuelle Festplatten in Ihrem Ziel-Speicher-Konto verfügen. Wenn der Container nicht vorhanden ist, erstellen Sie vor dem Ausführen des Befehls.

    $sourceBlobUri = <source-vhd-uri>

    $sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

    $destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

    Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext

Beispiel:

    C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

    C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

    C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

    C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext

### <a name="a-namescenario2ascenario-2-i-am-migrating-vms-from-other-platforms-to-azure-premium-storage"></a><a name="scenario2"></a>Szenario 2: "Ich bin virtuellen Computern von anderen Plattformen zu Azure Premium-Speicher migrieren".

Wenn Sie virtuelle Festplatte von nicht - Azure Cloud-Speicher in Azure migrieren, müssen Sie zuerst die virtuelle Festplatte in einem lokalen Verzeichnis exportieren. Haben Sie den vollständige Quellpfad des lokalen Verzeichnisses virtuelle Festplatte praktischen gespeichert ist, und dann verwenden Sie AzCopy, um das Bild zu Azure-Speicher hochzuladen.

#### <a name="step-1-export-vhd-to-a-local-directory"></a>Schritt 1. Exportieren virtuelle Festplatte in einem lokalen Verzeichnis

##### <a name="copy-a-vhd-from-aws"></a>Kopieren Sie eine virtuelle Festplatte aus AWS

1. Wenn Sie AWS verwenden, exportieren Sie die EC2 Instanz in eine virtuelle Festplatte in eine Zelle Amazon S3. Befolgen Sie die Schritte in der Amazon-Dokumentation für Amazon EC2 Instanzen exportieren, installieren Sie das Tool der Amazon EC2 line Interface (CLI), und führen Sie den Befehl erstellen-Instanz-Export-Vorgang in der EC2 Instanz in eine virtuelle Festplatte Datei exportieren. Achten Sie darauf, dass **virtuelle Festplatte** für den Datenträger & #95; Bild & #95 verwendet werden soll. FORMAT-Variable beim Ausführen des Befehls **erstellen-Instanz-Export-Vorgang** . Die exportierte Datei virtuelle Festplatte wird in der Zelle Amazon S3 gespeichert, die Sie während dieses Prozesses zu bestimmen.

        aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
        --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX

2. Herunterladen der Datei virtuelle Festplatte aus der Zelle S3 an. Wählen Sie die virtuelle Festplatte-Datei, klicken Sie dann die **Aktionen** > **herunterladen**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Kopieren Sie eine virtuelle Festplatte aus anderen nicht Azure cloud

Wenn Sie virtuelle Festplatte von nicht - Azure Cloud-Speicher in Azure migrieren, müssen Sie zuerst die virtuelle Festplatte in einem lokalen Verzeichnis exportieren. Kopieren Sie den vollständige Quellpfad des lokalen Verzeichnisses virtuelle Festplatte gespeichert ist.

##### <a name="copy-a-vhd-from-on-premise"></a>Kopieren Sie eine virtuelle Festplatte aus lokal

Wenn Sie virtuelle Festplatte aus einer lokalen Umgebung migrieren, benötigen Sie den vollständige Quellpfad virtuelle Festplatte gespeichert ist. Der Quellpfad könnte eine Server-Speicherort oder einer Datei freigeben.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Schritt 2. Erstellen Sie das Ziel für Ihre virtuelle Festplatte

Erstellen Sie ein Speicherkonto für die Verwaltung Ihrer virtuellen Festplatten. Berücksichtigen Sie die folgenden Punkte, beim Planen, wo Ihre virtuellen Festplatten gespeichert:

- Das Ziel-Speicher-Konto konnte standard oder Premium Speicher je nach Ihrer Anwendung Anforderung sein.
- Die Region Speicher Konto muss Premium Speicher in Azure-virtuellen Computern identisch sein, die in der letzten Phase erstellt werden. Sie können ein Neukunde Speicher oder Plan mit dem gleichen Speicherkonto entsprechend Ihren Anforderungen kopieren.
- Kopieren Sie und speichern Sie den Schlüssel Speicher Konto das Ziel-Speicher-Konto für die nächste Phase.

Es wird dringend empfohlen verschieben alle Daten für die Herstellung Arbeitsbelastung Premium Speicher verwenden.

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a>Schritt 3. Hochladen der virtuellen Festplatte zu Azure-Speicher

Jetzt, da Sie Ihre virtuelle Festplatte im lokalen Verzeichnis verfügen, können Sie die VHD-Datei zu Azure-Speicher hochladen AzCopy oder AzurePowerShell verwenden. Beide Optionen stehen hier zur Verfügung:

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a>Option 1: Hochladen die VHD-Datei mithilfe von Azure PowerShell-Add-AzureVhd

    Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>

Beispiel für <Uri> möglicherweise **_"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"_**. Beispiel für <FileInfo> möglicherweise **_"C:\path\to\upload.vhd"_**.

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a>Option 2: Verwenden von AzCopy VHD-Datei hochladen

Verwenden AzCopy, können Sie einfach die virtuelle Festplatte über das Internet hochladen. Je nach der Größe der virtuellen Festplatten kann dies Zeit dauern. Denken Sie daran, aktivieren Sie das Konto eingehende/Ausgang Speichergrenzwerte bei Verwendung dieser Option werden soll. Details finden Sie unter [Leistung und Skalierbarkeit der Azure-Speicher](storage-scalability-targets.md) .

1. Herunterladen und Installieren von AzCopy hier: [neueste Version von AzCopy](http://aka.ms/downloadazcopy)
2. Öffnen Sie Azure PowerShell zu, und wechseln Sie zu dem Ordner, in dem AzCopy installiert ist.
3. Verwenden Sie den folgenden Befehl aus, um die Datei virtuelle Festplatte aus "Quelle" an "Ziel" zu kopieren.

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Beispiel:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd

    Es folgen einige Beschreibungen der Parameter in den Befehl AzCopy verwendet:

 - * */Source: * &lt;Quelle&gt;:*** Speicherort des Ordners oder der Container Speicher-URL, die die virtuelle Festplatte enthält.
 - * */SourceKey: * &lt;Quelle kontoschlüssel&gt;:*** Speicher kontoschlüssel des Kontos Speicher Quelle.
 - * */Dest: * &lt;Ziel&gt;:*** Speicher Container-URL, um die virtuelle Festplatte zu kopieren.
 - * */DestKey: * &lt;Ziel kontoschlüssel&gt;:*** Speicher kontoschlüssel für das Ziel-Speicher-Konto.
 - **/BlobType: Seite:** Gibt an, ob das Ziel einer Seitenblob ist.
 - * *-Block: * &lt;Dateiname&gt;:*** geben den Dateinamen an der virtuellen Festplatte zu kopieren.

Details zur Verwendung von AzCopy Tool finden Sie unter [Übertragen von Daten mit den AzCopy Befehlszeilenprogramm](storage-use-azcopy.md).

##### <a name="other-options-for-uploading-a-vhd"></a>Weitere Optionen für das Hochladen einer virtuellen Festplatte

Außerdem können Sie eine virtuelle Festplatte mit Ihrem Speicherkonto mithilfe einer der folgenden Arten:

- [Azure-Speicher kopieren Blob-API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure-Speicher Explorer hochladen Blobs](https://azurestorageexplorer.codeplex.com/)
- [Speicher Import/Export-Dienst REST-API-Referenz](https://msdn.microsoft.com/library/dn529096.aspx)

>[AZURE.NOTE] Wir empfehlen Import/Export-Dienst verwenden, wenn eine geschätzte hochladen Zeit mehr als sieben Tage ist. [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) können Sie die Zeit von Größe und durchstellen Dateneinheit schätzen.
>
> Import/Export kann verwendet werden, um mit einem standardmäßigen Speicher-Konto zu kopieren. Sie benötigen Premium Speicher-Konto mithilfe von Tools wie AzCopy standardmäßigen Speicher zu kopieren.


## <a name="a-namecreate-azure-virtual-machine-using-premium-storageacreate-azure-vms-using-premium-storage"></a><a name="create-azure-virtual-machine-using-premium-storage"></a>Erstellen von Azure-virtuellen Computern mit Premium-Speicher

Nachdem die virtuelle Festplatte hochgeladen oder mit dem Speicherkonto des gewünschten kopiert haben, folgen Sie den Anweisungen in diesem Abschnitt, um die virtuelle Festplatte als Bild OS oder OS Datenträger abhängig von Ihrem Szenario registrieren, und erstellen Sie dann eine Instanz der virtueller Computer es ein. Der Datenträger Daten virtuelle Festplatte kann an den virtuellen Computer angefügt werden, nachdem es erstellt wurde. Am Ende dieses Abschnitts ist ein Beispiel für die Migration – Skript bereitgestellt. Dieses einfache Skript entspricht nicht alle Szenarien. Möglicherweise müssen Sie das Skript mit Ihrer jeweiligen Szenario entsprechend zu aktualisieren. Um herauszufinden ob dieses Skript zu Ihrem Szenario gilt, finden Sie unter [A Beispiel Migrations-Skript](#a-sample-migration-script).

### <a name="checklist"></a>Checkliste

1.  Warten Sie, bis alle Kopieren virtuelle Festplatte Datenträger ist abgeschlossen.
2.  Sicherstellen Sie, dass Premium Speicher in der Region, zu dem Sie migrieren, verfügbar ist.
3.  Entscheiden Sie die neue Datenreihe virtueller Computer, die Sie verwenden möchten. Sie sollten eine Premium-Speicher in, und die Größe Abhängigkeit die Verfügbarkeit in der Region, und entsprechend Ihren Anforderungen.
4.  Entscheiden Sie die genaue Größe virtueller Computer, die Sie verwenden möchten. Virtueller Speicher muss ausreicht, um die Anzahl der Daten Datenträger unterstützen, stehen Ihnen, werden. Z. B. Wenn Sie 4 Daten Datenträger verfügen, müssen der virtuellen Computer 2 oder mehr Kerne. Erwägen Sie auch, Verarbeitung Power, Arbeitsspeicher und Netzwerk-Bandbreite benötigt.
5.  Erstellen Sie ein Konto Premium Speicher in den Zielbereich. Dies ist das Konto ein, das für den neuen virtuellen Computer soll verwendet werden.
6.  Haben Sie die aktuellen virtuellen Computer Details praktischen, einschließlich der Liste der Laufwerke und entsprechende virtuelle Festplatte Blobs.

Bereiten Sie Ihrer Anwendung für Ausfallzeiten vor. Um eine übersichtliche Migration durchzuführen, müssen Sie alle die Verarbeitung im aktuellen System beenden. Nur dann können Sie es in konsistenten Zustand abzurufen, die Sie auf die neue Plattform migrieren können. Ausfallzeiten Dauer wird die Menge der Daten im Laufwerke migrieren abhängig sind.

>[AZURE.NOTE] Wenn Sie eine Azure Ressourcenmanager VM aus einem speziellen virtuellen Festplatte Festplatten erstellen, finden Sie in [dieser Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) für die Bereitstellung von Ressourcenmanager VM vorhandenen Datenträger verwenden.

### <a name="register-your-vhd"></a>Registrieren der virtuellen Festplatte

Erstellen ein virtuellen Computers von OS virtuellen Festplatte oder einen Datenträger Daten einen neuen virtuellen Computer installieren, müssen Sie diese registrieren. Führen Sie abhängig von Ihrer virtuelle Festplattes Szenario zutreffenden Schritte aus.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>GRG-Betriebssystem virtuelle Festplatte mehrerer Instanzen von Azure-virtuellen Computer erstellen

Nachdem GRG OS Bild virtuelle Festplatte mit dem Speicherkonto hochgeladen wurde, registrieren Sie es als **Bild der Azure-virtuellen Computer** , damit Sie eine oder mehrere Instanzen von virtuellen Computer daraus erstellen können. Verwenden Sie die folgenden PowerShell-Cmdlets, um Ihre virtuelle Festplatte als Bild Azure-virtuellen Computer OS registrieren. Geben Sie vollständigen Container URL in die virtuelle Festplatte kopiert wurde.

    Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows

Kopieren Sie und speichern Sie den Namen der diese neue Azure-virtuellen Computer Abbildung. Im Beispiel oben ist es *OSImageName*.

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Eindeutige Betriebssystem virtuelle Festplatte zum Erstellen einer einzelnen Instanz von Azure virtueller Computer

Nachdem die eindeutigen OS virtuelle Festplatte mit dem Speicherkonto hochgeladen wurde, als Registrieren einer **Azure OS Datenträger** , damit Sie eine Instanz der virtueller Computer daraus erstellen können. Verwenden Sie diese PowerShell-Cmdlets, um Ihre virtuelle Festplatte als eine Azure OS Datenträger registrieren. Geben Sie vollständigen Container URL in die virtuelle Festplatte kopiert wurde.

    Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"

Kopieren und den Namen der neuen Azure OS Datenträger speichern. Im Beispiel oben ist es *OSDisk*aus.

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a>Daten Datenträger virtuelle Festplatte neue Instanzen der Azure-virtuellen Computer zugeordnet werden soll

Nachdem der Datenträger Daten virtuelle Festplatte Speicher Konto hochgeladen wurde, als Registrieren einer Azure-Daten Datenträger, damit es für Ihre neue DS Serie DSv2 Reihe oder Instanz GS Reihe Azure virtueller Computer verbunden werden kann.

Verwenden Sie diese PowerShell-Cmdlets, um Ihre virtuelle Festplatte als Datenträger eine Azure-Daten zu registrieren. Geben Sie vollständigen Container URL in die virtuelle Festplatte kopiert wurde.

    Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"

Kopieren Sie und speichern Sie den Namen des neuen Laufwerks Azure Daten. Im Beispiel oben ist es *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Erstellen eines Premium Speicher-fähigen virtuellen Computers

Nachdem das OS Bild oder OS Datenträger registriert sind, erstellen Sie eine neue DS-Serie, DSv2-Serie oder GS-Serie virtueller Computer. Sie werden Betriebssystemabbilds oder Name des Betriebssystems Datenträger, die Sie registriert verwenden. Wählen Sie den virtuellen Computer von der Ebene Premium-Speicher. Im folgenden Beispiel verwenden wir die *Standard_DS2* virtueller Speicher.

>[AZURE.NOTE] Aktualisieren Sie die Datenträgergröße, um sicherzustellen, dass sie Ihre Kapazität und Leistung Anforderungen und die Größen Azure verfügbarer übereinstimmt.

Führen Sie die Schritt-für-Schritt-PowerShell-Cmdlets unten, um den neuen virtuellen Computer erstellen. Legen Sie zunächst die allgemeinen Parameter ein:

    $serviceName = "yourVM"
    $location = "location-name" (e.g., West US)
    $vmSize ="Standard_DS2"
    $adminUser = "youradmin"
    $adminPassword = "yourpassword"
    $vmName ="yourVM"
    $vmSize = "Standard_DS2"

Erstellen Sie zuerst einen Cloud-Dienst, in dem Sie Ihre neue virtuelle Computer hosten.

    New-AzureService -ServiceName $serviceName -Location $location

Erstellen Sie dann abhängig von Ihrem Szenario, die Instanz Azure virtueller Computer aus dem Bild OS oder OS Datenträger, die Sie registriert.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>GRG-Betriebssystem virtuelle Festplatte mehrerer Instanzen von Azure-virtuellen Computer erstellen

Erstellen Sie die eine oder mehrere neuen DS Serie Azure-virtuellen Computer Instanzen mithilfe des **Azure OS Bild** , die Sie registriert. Angegeben Sie diese OS Bildnamen in der Konfiguration virtueller Computer die Erstellung virtueller neuen Computer wie unten dargestellt.

    $OSImage = Get-AzureVMImage –ImageName "OSImageName"

    $vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

    Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

    New-AzureVM -ServiceName $serviceName -VM $vm

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Eindeutige Betriebssystem virtuelle Festplatte zum Erstellen einer einzelnen Instanz von Azure virtueller Computer

Erstellen Sie eine neue DS Serie Azure-virtuellen Computer-Instanz mithilfe der **Azure OS Datenträger** , die Sie registriert. Geben Sie diese OS Datenträger Namen in der Konfiguration virtueller Computer, wenn Sie den neuen virtuellen Computer erstellen, wie unten dargestellt.

    $OSDisk = Get-AzureDisk –DiskName "OSDisk"

    $vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

    New-AzureVM -ServiceName $serviceName –VM $vm

Geben Sie die anderen Azure-virtuellen Computer Informationen, wie eine Cloud-Dienst, Region, Speicher-Konto, Verfügbarkeit festlegen und Richtlinie Zwischenspeichern. Beachten Sie, dass die Instanz virtueller Computer am selben Standort mit zugeordneten Betriebssystem oder Daten Datenträger, stehen muss, damit das ausgewählte Cloud Service "," Region "und" Speicher-Konto in am selben Speicherort wie die zugrunde liegenden virtuellen Festplatten der betreffenden Datenträger sein muss.

### <a name="attach-data-disk"></a>Anfügen von Daten Datenträger

Wenn Sie Daten Datenträger virtueller Festplatten registriert haben, verbinden Sie sie schließlich mit dem neuen Premium Speicher in Azure virtuellen Computer.

Formular mit folgenden PowerShell-Cmdlet Daten Datenträger anfügen, um den neuen virtuellen Computer, und geben Sie die Richtlinie Zwischenspeichern. Im folgenden Beispiel wird die Richtlinie für Zwischenspeichern auf *schreibgeschützt*festgelegt.

    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

    Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

    Update-AzureVM  -VM $vm

>[AZURE.NOTE] Möglicherweise zusätzliche Schritte benötigt werden, um eine Anwendung unterstützen, ist nicht in diesem Handbuch behandelt.

### <a name="checking-and-plan-backup"></a>Aktivieren und Planen Sie die Sicherung

Nachdem Sie der neue virtuellen Computer ausgeführt wird, Zugriff darauf mit demselben Benutzernamen und Kennwort als ursprünglichen virtuellen ist, und stellen Sie sicher, dass, dass alles wie erwartet funktioniert. Die Einstellungen der aufgeteilte Datenträger, einschließlich würde auf dem neuen virtuellen Computer vorhanden sein.

Der letzte Schritt darin ist Sicherung Planung und Wartungszeitplan für den neuen virtuellen Computer auf Grundlage der Anwendung Anforderungen.

### <a name="a-namea-sample-migration-scriptaa-sample-migration-script"></a><a name="a-sample-migration-script"></a>Ein Beispiel Migrations-Skript

Wenn Sie mehrere virtuelle Computer migrieren haben, wird Automatisierung über PowerShell-Skripts hilfreich sein. Es folgt ein Beispiel-Skript, die Automatisierung die Migration eines virtuellen Computers. Beachten Sie, dass sich unterhalb Skript ist nur ein Beispiel, und es gibt einige Annahmen über die aktuellen virtuellen Computer Datenträger. Möglicherweise müssen Sie das Skript mit Ihrer jeweiligen Szenario entsprechend zu aktualisieren.

Annahmen sind:

- Sie sind klassische Azure-virtuellen Computern erstellen.
- Die Quelle OS-Datenträger und die Quelle Daten Datenträger befinden sich in derselben Speicherkonto und die gleichen Container. Wenn die OS-Datenträger und Datenträger Daten nicht an derselben Stelle sind, können Sie zum Kopieren von virtuellen Festplatten über Speicherkonten und Container AzCopy oder Azure PowerShell verwenden. Schlagen Sie in den vorherigen Schritt: [Kopieren virtuelle Festplatte mit AzCopy oder PowerShell](#copy-vhd-with-azcopy-or-powershell). Bearbeiten dieses Skript, um Ihrem Szenario entsprechen ist eine andere, aber wir empfehlen, AzCopy oder PowerShell verwenden, da es einfacher und schneller ist.

Nachfolgend finden Sie das Automatisierungsskript. Ersetzen von Text durch Ihre Daten, und aktualisieren Sie das Skript mit Ihrer jeweiligen Szenario entsprechen.

>[AZURE.NOTE] Verwenden das vorhandene Skript wird die Netzwerkkonfiguration Ihrer Datenquelle virtueller Computer nicht beibehalten. Folgende Schritte sind Abstände-Config Netzwerken Einstellungen auf Ihre migrierten virtuellen Computern erforderlich.

    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED “AS IS” WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location “Southeast Asia”

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you’d like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the sourve vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more custimization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location

#### <a name="a-nameoptimizationaoptimization"></a><a name="optimization"></a>Optimierung

Ihrer aktuelle virtuellen Computer Konfiguration möglicherweise angepasst werden speziell für Standard Datenträger gut arbeiten. Z. B., um die Leistung zu verbessern, mithilfe von vielen Datenträger in einem aufgeteilten Datenträger. Beispielsweise anstelle von 4 Datenträger separat Premium Speichermenge, um die Kosten zu optimieren, indem Sie eine einzelne Festplatte Probleme möglicherweise. Optimierungen wie diese müssen auf Basis von Fall behandelt werden und für die benutzerdefinierte Schritte erforderlich, nach der Migration. Beachten Sie auch, dieses Verfahren gut funktionieren möglicherweise nicht für Datenbanken und Anwendungen, die in der Einrichtung definiert das Layout des Datenträgers abhängig sind.

##### <a name="preparation"></a>Vorbereitung

1.  Führen Sie die einfachen Migration wie aus dem vorherigen Abschnitt beschrieben. Optimierungen werden nach der Migration des neuen virtuellen Computers ausgeführt werden.
2.  Definieren Sie die neuen Datenträger Schriftgrade für die optimierte Konfiguration erforderlich.
3.  Ermitteln Sie Zuordnung aktuelle Datenträger/Datenträger den neuen Datenträger Spezifikationen an.

##### <a name="execution-steps"></a>Ausführungsschritte

1.  Erstellen Sie neue Datenträger mit der richtigen Größe des virtuellen Computers Premium-Speicher.
2.  Melden Sie sich an die virtuellen Computer, und kopieren Sie die Daten aus dem aktuellen Volume auf die neue Festplatte, die die Lautstärke zugeordnet ist. Dies gilt für alle aktuellen Datenmengen, die auf einer neuen Festplatte zuordnen müssen.
3.  Als Nächstes ändern Sie die Anwendungseinstellungen zu den neuen Laufwerken zu wechseln, und trennen Sie die alten Datenmengen.

Zur Optimierung der Anwendungs für eine bessere Leistung der Datenträger, Näheres [Leistung der Anwendung optimieren](storage-premium-storage-performance.md#optimizing-application-performance).

### <a name="application-migrations"></a>Migration von Anwendung

Datenbanken und anderen komplexen Clientanwendungen möglicherweise spezielle Schritte erfordern, durch die Anwendung-Anbieter für die Migration definiert. Finden Sie in der Dokumentation zur jeweiligen Anwendung. Z. B. in der Regel durch Sicherung Datenbanken migriert werden können und wiederherstellen.

## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter den folgenden Ressourcen für bestimmte Szenarien Migrieren von virtuellen Computern:

- [Migrieren von Azure-virtuellen Computern zwischen Speicherkonten](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
- [Erstellen und Hochladen einer Windows Server virtuellen in Azure.](../virtual-machines/virtual-machines-windows-classic-createupload-vhd.md)
- [Erstellen und Hochladen einer virtuellen Festplatte, die das Betriebssystem Linux enthält.](../virtual-machines/virtual-machines-linux-classic-create-upload-vhd.md)
- [Migrieren von virtuellen Computern von Amazon AWS in Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Darüber hinaus finden Sie weitere Informationen zur Azure-Speicher und Azure-virtuellen Computern die folgenden Ressourcen:

- [Azure-Speicher](https://azure.microsoft.com/documentation/services/storage/)
- [Azure-virtuellen Computern](https://azure.microsoft.com/documentation/services/virtual-machines/)
- [Premium Speicher: Leistungsstarke Storage für Auslastung Azure-virtuellen Computern](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
