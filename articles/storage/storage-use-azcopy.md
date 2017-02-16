<properties
    pageTitle="Kopieren oder Verschieben von Daten auf Speicher mit AzCopy | Microsoft Azure"
    description="Verwenden Sie das Programm AzCopy zu verschieben oder Kopieren von Daten in den oder aus dem Blob, Tabelle und der Inhalt der Datei ein. Kopieren Sie Daten in Azure-Speicher aus lokalen Dateien, oder Kopieren von Daten innerhalb oder zwischen Speicherkonten. Einfache Migration von Daten zu Azure-Speicher ein."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="micurd"/>

# <a name="transfer-data-with-the-azcopy-command-line-utility"></a>Übertragen von Daten mit den AzCopy Befehlszeilenprogramms

## <a name="overview"></a>(Übersicht)

AzCopy ist ein Windows-Befehlszeilenprogramm zum Kopieren von Daten an und von Microsoft Azure Blob, Datei, und Tabellenspeicher, verwenden einfache Befehle mit optimale Leistung entwickelt. Sie können Daten von einem Objekt in ein anderes kopieren, oder zwischen Speicherkonten in Ihr Speicherkonto.

> [AZURE.NOTE] Mit diesem Leitfaden wird davon ausgegangen, dass Sie bereits mit [Azure-Speicher](https://azure.microsoft.com/services/storage/)vertraut sind. Wenn dies nicht der Fall ist, lesen die [Einführung in Azure-Speicher](storage-introduction.md) Dokumentation hilfreich sein wird. Vor allem müssen Sie [Speicher-Konto](storage-create-storage-account.md#create-a-storage-account) erstellen und Verwenden von AzCopy.

## <a name="download-and-install-azcopy"></a>Herunterladen und Installieren von AzCopy

### <a name="windows"></a>Windows

Laden Sie die [neueste Version von AzCopy](http://aka.ms/downloadazcopy).

### <a name="maclinux"></a>Mac/Linux

AzCopy ist nicht verfügbar für Mac/Linux OSs. Azure CLI ist jedoch eine geeignete Alternative zum Kopieren von Daten an und von Azure-Speicher. Gelesen Sie weitere Informationen [mithilfe der Azure CLI mit Azure-Speicher](storage-azure-cli.md) .

## <a name="writing-your-first-azcopy-command"></a>Schreiben den ersten Befehl ein AzCopy

Die grundlegende Syntax für AzCopy Befehle lautet:

    AzCopy /Source:<source> /Dest:<destination> [Options]

Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zum Installationsverzeichnis auf Ihrem Computer - AzCopy, in dem die `AzCopy.exe` ausführbare befindet. Falls gewünscht, können Sie den Speicherort der AzCopy auf Ihrem Systempfad hinzufügen. Standardmäßig AzCopy installiert ist `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` oder `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

Einer Vielzahl von Szenarien zum Kopieren von Daten aus Microsoft Azure Blobs, Dateien und Tabellen, und führen Sie die folgenden Beispiele vor. Finden Sie im Abschnitt [AzCopy Parameter](#azcopy-parameters) eine ausführliche Erläuterung der Parameter in jeder Stichprobe verwendet.

## <a name="blob-download"></a>BLOB: herunterladen

### <a name="download-single-blob"></a>Einzelnes Blob herunterladen

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"

Beachten Sie, dass bei den Ordner `C:\myfolder` nicht vorhanden ist, erstellt, und Laden Sie AzCopy `abc.txt ` in den neuen Ordner.

### <a name="download-single-blob-from-secondary-region"></a>Einzelnes Blob von sekundären Region herunterladen

    AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Beachten Sie, dass Sie Lesezugriff Geo redundante Speicher aktiviert haben, müssen.

### <a name="download-all-blobs"></a>Alle Blobs herunterladen

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S

Angenommen Sie, die folgenden Blobs in der angegebenen Container befinden:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Nach dem Downloadvorgang Verzeichnis `C:\myfolder` werden, enthalten die folgenden Dateien:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Wenn Sie keine Option angeben `/S`, werden keine Blobs heruntergeladen.

### <a name="download-blobs-with-specified-prefix"></a>Herunterladen von Blobs mit angegebenen Präfix

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S

Angenommen Sie, die folgenden Blobs in der angegebenen Container befinden. Alle Blobs beginnend mit dem Präfix `a` wird heruntergeladen:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Nach dem Downloadvorgang, den Ordner `C:\myfolder` werden, enthalten die folgenden Dateien:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Das Präfix gilt für das virtuelle Verzeichnis, bildet bei den ersten Teil der Blob-Name. Im Beispiel oben dargestellten entspricht virtuelle Verzeichnis nicht das angegebene Präfix,, damit sie nicht heruntergeladen wird. Darüber hinaus, wenn die Option `\S` nicht angegeben ist, AzCopy werden keine Blobs herunterladen.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>Legen Sie die Uhrzeit der letzten Änderung der exportierten Dateien in der Quelle Blobs identisch sein

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT

Sie können auch Blobs aus des Herunterladens basierend auf deren Uhrzeit der letzten Änderung ausschließen. Wenn Sie Blobs ausschließen, deren der letzten Zeit Änderung, möchten beträgt beispielsweise-Version als der Zieldatei hinzufügen der `/XN` Option:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN

Oder Blobs ausgeschlossen, deren der letzten Zeit Änderung, werden soll, ist die gleichen oder älter sind als der Zieldatei hinzufügen der `/XO` Option:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO

## <a name="blob-upload"></a>BLOB: hochladen

### <a name="upload-single-file"></a>Hochladen einer Datei

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"

Wenn der Zielcontainer angegebenen nicht vorhanden ist, AzCopy erstellt, und Laden Sie die Datei hinein.

### <a name="upload-single-file-to-virtual-directory"></a>Hochladen einer Datei auf virtuelle Verzeichnis

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt

Wenn das angegebene virtuelle Verzeichnis nicht vorhanden ist, wird AzCopy hochladen die Datei, um das virtuelle Verzeichnis in deren Namen aufnehmen möchten (*z.*B. `vd/abc.txt` im Beispiel oben).

### <a name="upload-all-files"></a>Alle Dateien hochladen

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S

Angeben der Option `/S` uploads den Inhalt des angegebenen Verzeichnisses BLOB-Speicher rekursiv, was bedeutet, dass alle Unterordner und ihre Dateien sowie hochgeladen werden. Beispielsweise wird davon ausgegangen, die folgenden Dateien befinden sich im Ordner `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Nach dem Hochladen beginnen wird der Container die folgenden Dateien enthalten:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Wenn Sie keine Option angeben `/S`, AzCopy, rekursiv nicht hochgeladen werden. Nach dem Hochladen beginnen wird der Container die folgenden Dateien enthalten:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a>Hochladen von Dateien, die bestimmten Muster entsprechen.

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S

Angenommen, die folgenden Dateien befinden sich im Ordner `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Nach dem Hochladen beginnen wird der Container die folgenden Dateien enthalten:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Wenn Sie keine Option angeben `/S`, AzCopy, Blobs, die in einem virtuellen Verzeichnis gespeichert sind, nicht nur hochgeladen werden:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Geben Sie den MIME-Inhaltstyp eines Blob Ziel

Standardmäßig wird AzCopy der Inhaltstyp eines Blob Ziel, `application/octet-stream`. Ab Version 3.1.0, Sie können explizit angeben den Inhaltstyp über die Option `/SetContentType:[content-type]`. Diese Syntax wird der Inhaltstyp für alle Blobs in einen Vorgang hochladen.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4

Wenn Sie angeben `/SetContentType` ohne einen Wert, legen Sie dann AzCopy wird jedes Blob oder der Datei Inhaltstyp entsprechend ihrer Erweiterung.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType

## <a name="blob-copy"></a>BLOB: Kopieren

### <a name="copy-single-blob-within-storage-account"></a>Kopieren Sie die einzelnes Blob in Speicher-Konto

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt

Beim Kopieren eines BLOBs innerhalb eines Kontos Speicher wird ein [serverseitigen](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) Kopiervorgang ausgeführt.

### <a name="copy-single-blob-across-storage-accounts"></a>Kopieren Sie einzelnes Blob über Speicher-Konten

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Beim Kopieren eines BLOBs über Speicherkonten hinweg wird ein [serverseitigen](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) Kopiervorgang ausgeführt.

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Kopieren Sie einzelnes Blob aus sekundäre Region primäre Region

    AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Beachten Sie, dass Sie Lesezugriff Geo redundante Speicher aktiviert haben, müssen.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Kopieren Sie einzelne Blob und der Momentaufnahmen über Speicher-Konten

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot

Nach dem Kopiervorgang wird der Zielcontainer der Blob und der Momentaufnahmen enthalten. Vorausgesetzt, dass das Blob im obigen Beispiel zwei Momentaufnahmen hat, der Container gehören die folgenden Blob und Momentaufnahmen:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Kopieren Sie Blobs synchron über Speicher-Konten

AzCopy standardmäßig kopiert Daten zwischen zwei Speicher Endpunkten asynchrone aus. Daher wird der Kopiervorgang ausgeführt, Hintergrund in freie Bandbreite, die keine Vereinbarung zum SERVICELEVEL werden wie schnelles enthält ein Blob kopiert werden und AzCopy überprüft regelmäßig den Status kopieren, bis das Kopieren abgeschlossen oder fehlgeschlagen ist.

Die `/SyncCopy` Option wird sichergestellt, dass der Kopiervorgang gleichmäßige Geschwindigkeit abruft. AzCopy führt die synchroner Kopie herunterladen die Blobs aus der angegebenen Quelle in den lokalen Speicher kopieren, und klicken Sie dann auf das Ziel des Blob-Speicher Dateien hochladen aus.

    AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy

`/SyncCopy`Möglicherweise sollten Sie den Ausgangs-zusätzliche Kosten im Vergleich zu asynchrone Kopie generieren, wird empfohlen, diese Option eine Azure-virtuellen Computer verwenden, der in derselben Region als Quelle Speicherkonto Ausgang Kosten zu vermeiden.

## <a name="file-download"></a>Datei: herunterladen

### <a name="download-single-file"></a>Herunterladen einer Datei

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Die angegebene Quelle ist ein Azure Dateifreigabe, und geben Sie entweder den genauen Dateinamen (*z. B.* `abc.txt`) zu einer Datei herunterladen, oder geben Sie die Option `/S` alle Dateien in der Freigabe rekursiv herunterladen. Bei dem Versuch, eine Dateimuster und die Option angeben `/S` zusammen zu einem Fehler führt.

### <a name="download-all-files"></a>Alle Dateien herunterladen

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S

Beachten Sie, dass keine leeren Ordner werden nicht heruntergeladen werden.

## <a name="file-upload"></a>Datei: hochladen

### <a name="upload-single-file"></a>Hochladen einer Datei

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt

### <a name="upload-all-files"></a>Alle Dateien hochladen

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S

Beachten Sie, dass keine leeren Ordner nicht hochgeladen werden.

### <a name="upload-files-matching-specified-pattern"></a>Hochladen von Dateien, die bestimmten Muster entsprechen.

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S

## <a name="file-copy"></a>Datei: Kopieren

### <a name="copy-across-file-shares"></a>Kopieren von Dateifreigaben

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S

### <a name="copy-from-file-share-to-blob"></a>Kopieren von Dateifreigabe zu BLOB-

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S

Beachten Sie, dass asynchrone Kopieren von Dateispeicher in Seite Blob nicht unterstützt wird.

### <a name="copy-from-blob-to-file-share"></a>Kopieren von Blob auf Dateifreigabe

    AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S

### <a name="synchronously-copy-files"></a>Kopieren Sie die Dateien synchron

Sie können angeben, die `/SyncCopy` option, um Daten aus den Dateispeicher Dateispeicher, vom Dateispeicherort zu Blob-Speicher und aus den Dateispeicher Blob-Speicher synchron zu kopieren, AzCopy macht's, indem Sie die Quelldaten in den lokalen Speicher herunterladen und wieder zu Ziel hochladen.

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

Beim Kopieren von Dateispeicher in Blob-Speicher des Standardtyps für Blob blockieren Blob, Benutzer kann angeben Option `/BlobType:page` Ziel BLOB-Typ ändern.

Beachten Sie, dass `/SyncCopy` möglicherweise zusätzliche Kosten vergleichen von asynchrone Kopie Ausgang generieren, wird empfohlen, diese Option auf dem Azure-virtuellen Computer verwenden, der in derselben Region als Quelle Speicher-Konto zu vermeiden, sollten Sie den Ausgangs-Kosten ist.

## <a name="table-export"></a>Tabelle: Exportieren

### <a name="export-table"></a>Tabelle exportieren

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key

AzCopy schreibt eine Manifestdatei in den angegebenen Zielordner. Die Manifestdatei wird in den Importvorgang verwendet, suchen Sie die erforderlichen Datendateien und Ausführen der datenüberprüfung. Die Manifestdatei verwendet die folgende Benennungskonvention standardmäßig an:

    <account name>_<table name>_<timestamp>.manifest

Benutzer kann auch die Option angeben `/Manifest:<manifest file name>` den Manifestdateinamen festlegen.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest

### <a name="split-export-into-multiple-files"></a>Teilen Sie exportieren in mehrere Dateien

    AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100

AzCopy verwendet einen *Index Lautstärke* in den Dateinamen der geteilten Daten, mehrere Dateien zu unterscheiden. Der Lautstärke Index besteht aus zwei Teilen, einen *Index der wichtigsten Bereich* und *Teilen Sie die Datei Index*. Beide Indizes sind nullbasiert.

Der Index der wichtigsten Bereich werden 0, wenn Benutzer keine Option angibt `/PKRS`.

Nehmen Sie beispielsweise an AzCopy generiert zwei Datendateien, nachdem der Benutzer die Option gibt an `/SplitSize`. Die resultierenden Daten Dateinamen möglicherweise:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Notiz, die der minimale möglichen Wert für die Option `/SplitSize` beträgt 32MB. Wenn das angegebene Ziel Blob-Speicher ist, AzCopy teilen die Datendatei aus, dessen Größe beim Erreichen das Größenlimit Blob (200GB), unabhängig davon, ob option wird `/SplitSize` vom Benutzer angegeben wurde.

### <a name="export-table-to-json-or-csv-data-file-format"></a>Exportieren der Tabelle zu JSON oder CSV-Format für Datendateien

AzCopy standardmäßig exportiert Tabellen in JSON-Datendateien. Sie können angeben, dass die Option `/PayloadFormat:JSON|CSV` zum Exportieren von Tabellen als JSON oder CSV-Format.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV

Wenn Sie das Format der CSV-Nutzlast angeben, generieren AzCopy auch eine Schemadatei mit der Erweiterung `.schema.csv` für jede Datendatei.

### <a name="export-table-entities-concurrently"></a>Exportieren Sie die Tabelle Personen gleichzeitig

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"

AzCopy beginnt gleichzeitige Vorgänge, um Personen zu exportieren, wenn der Benutzer die Option angibt `/PKRS`. Jede Operation exportiert eine Partition Key Bereich.

Beachten Sie, dass die Anzahl der gleichzeitigen Operationen auch durch die Option gesteuert wird `/NC`. AzCopy verwendet die Anzahl der Prozessoren Core als den Standardwert `/NC` beim Kopieren der Tabelle Einheiten, auch wenn `/NC` wurde nicht angegeben. Wenn der Benutzer die Option angibt `/PKRS`, AzCopy verwendet den kleineren der beiden Werte – Partition Key Bereiche im Vergleich zu implizit oder explizit festgelegten gleichzeitige Vorgänge - zu ermitteln Sie die Anzahl der gleichzeitigen Operationen zu beginnen. Geben Sie weitere Informationen hierzu `AzCopy /?:NC` in der Befehlszeile.

### <a name="export-table-to-blob"></a>Exportieren von BLOB-Tabelle

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2

AzCopy generiert eine JSON-Datendatei in den Container Blob mit Benennungskonvention folgen:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

Die generierte JSON-Datendatei resultiert Nutzlast Format minimale Metadaten aus. Details zu diesem Nutzlast Format finden Sie unter [Nutzlast Format für Operationen der Tabelle](http://msdn.microsoft.com/library/azure/dn535600.aspx).

Beachten Sie, dass beim Exportieren von Tabellen zu Blobs AzCopy Laden Sie die Tabelle Personen auf lokale temporäre Datendateien und Laden Sie diese Elemente in der Blob wird. Diese temporäre Dateien werden in den Ordner Journal Datei, mit dem standardmäßigen Pfad eingefügt "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", können Sie angeben, Option/z [Journal-Dateiordner] so ändern Sie die Erfassung Datei Ordnerspeicherort und somit Speicherort für die temporäre Datendateien ändern. Ist die temporäre Datendateien Größe durch Ihre Tabelle-Einheiten und der Größe, die Sie mit der Option /SplitSize angegeben haben entschieden, obwohl die temporären Datendatei im lokalen Festplatte sofort gelöscht wird, wenn es in der Blob hochgeladen wurde, stellen Sie sicher, dass Sie ausreichenden lokalen Speicherplatz diese temporäre Dateien gespeichert werden, bevor sie gelöscht werden müssen.

## <a name="table-import"></a>Tabelle: Importieren

### <a name="import-table"></a>Tabelle importieren

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace

Die Option `/EntityOperation` gibt an, wie Elemente in die Tabelle einfügen. Mögliche Werte sind:

- `InsertOrSkip`: Überspringt eine vorhandene Entität oder fügt ein neues Element aus, wenn sie in der Tabelle nicht vorhanden ist.
- `InsertOrMerge`: Führt eine vorhandene Entität oder fügt ein neues Element aus, wenn sie in der Tabelle nicht vorhanden ist.
- `InsertOrReplace`: Ersetzt eine vorhandene Entität oder fügt ein neues Element aus, wenn sie in der Tabelle nicht vorhanden ist.

Beachten Sie, dass Sie die Option angeben können `/PKRS` im Szenario importieren. Im Gegensatz zu dem Exportieren Szenario, in dem Sie Option angeben müssen `/PKRS` um gleichzeitige Vorgänge zu beginnen, AzCopy wird standardmäßig beim Starten von gleichzeitige Vorgänge Sie eine Tabelle importieren. Die Standardanzahl von gleichzeitige Vorgänge gestartet ist gleich der Anzahl der Prozessoren Core; Sie können jedoch unterschiedlich viele gleichzeitig mit der Option angeben `/NC`. Geben Sie weitere Informationen hierzu `AzCopy /?:NC` in der Befehlszeile.

Beachten Sie, dass AzCopy unterstützt nur für JSON, nicht CSV importieren. AzCopy nicht unterstützt Tabelle Importe von Benutzern erstellte JSON und Manifestdateien. Beide Dateien müssen aus einer AzCopy Tabelle exportieren stammen. Zur Vermeidung von Fehlern bitte die exportierte JSON ändern oder nicht Manifestdatei.

### <a name="import-entities-to-table-using-blobs"></a>Importieren Sie die Elemente aus, um die Tabelle mit blobs

Angenommen, ein Containers Blob enthält folgenden: A JSON-Datei, die eine Tabelle Azure und den zugehörigen Manifestdatei darstellt.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

Sie können Elemente in einer Tabelle mithilfe der Manifestdatei im Container Blob importieren den folgenden Befehl ausführen:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"

## <a name="other-azcopy-features"></a>Weitere Features AzCopy

### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Nur kopieren Sie Daten, die in das Ziel nicht vorhanden ist

Die `/XO` und `/XN` Parameter ermöglichen es Ihnen, die älter oder neuer Quellressourcen ausschließen Hilfethemas kopiert. Wenn Sie nur Quellressourcen, die in das Ziel nicht vorhanden ist, kopieren möchten, können Sie den Befehl AzCopy in beiden Parameter angeben:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Hinweis: Dies ist nicht unterstützt, wenn die Quelle oder das Ziel um eine Tabelle handelt.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Verwenden Sie eine Antwortdatei Befehlszeilenparameter angeben.

    AzCopy /@:"C:\responsefiles\copyoperation.txt"

Sie können alle AzCopy Befehlszeilenparameter in einer Antwortdatei einbeziehen. AzCopy verarbeitet die Parameter in der Datei als wäre sie in der Befehlszeile eingegeben worden Durchführung einer direkten Ersetzung mit dem Inhalt der Datei.

Eine Antwortdatei namens Zielformatvorlagen `copyoperation.txt`, die die folgenden Zeilen enthält. Jeder AzCopy Parameter kann in einer einzigen Zeile angegeben werden muss

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

oder auf separaten Zeilen:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy schlägt fehl, wenn Sie den Parameter auf zwei Zeilen aufgeteilt wie hier dargestellt für die `/sourcekey` Parameter:

    http://myaccount.blob.core.windows.net/mycontainer
    C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Verwenden Sie mehrerer Antwortdateien Befehlszeilenparameter angeben.

Eine Antwortdatei namens Zielformatvorlagen `source.txt` , ein Containers Quelle angibt:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

Und eine Antwortdatei namens `dest.txt` , die einen Zielordner im Dateisystem gibt:

    /Dest:C:\myfolder

Und eine Antwortdatei namens `options.txt` gibt, die die Optionen für AzCopy:

    /S /Y

Um AzCopy mit diesen Antwortdateien aufzurufen, die im Verzeichnis befinden `C:\responsefiles`, verwenden Sie diesen Befehl:

    AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   

AzCopy verarbeitet dieser Befehl nur aus, als hätten Sie alle die einzelnen Parameter in der Befehlszeile enthalten:

    AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

### <a name="specify-a-shared-access-signature-sas"></a>Angeben einer freigegebenen Access-Signatur (SAS)

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt

Sie können auch eine SAS für den Container URI angeben:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S

### <a name="journal-file-folder"></a>Journal Dateiordner

Jedes Mal, wenn Sie einen Befehl zum AzCopy, aus überprüft, ob eine Journaldatei im Standardordner vorhanden ist, oder gibt an, ob es sich in einem Ordner befindet, die Sie über diese Option angegeben haben. Wenn die Journal-Datei nicht in keinem der beiden Speicherorte vorhanden ist, wird den Vorgang als neue AzCopy, und generiert eine neue Journaldatei.

Wenn die Journal-Datei vorhanden ist, überprüft AzCopy, ob die Befehlszeile, die eingegeben werden die Befehlszeile in der Journaldatei entspricht. Wenn der Befehl Zeilenende entsprechen, Lebensläufen AzCopy den unvollständigen Vorgang aus. Wenn sie nicht übereinstimmen, werden Sie aufgefordert, die Journal-Datei, um einen neuen Vorgang zu starten, oder um den aktuellen Vorgang abzubrechen entweder zu überschreiben.

Wenn Sie den Standardspeicherort für die Journaldatei verwenden möchten:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z

Wenn Sie die Option auslassen `/Z`, oder geben Sie die Option `/Z` ohne den Ordnerpfad wie oben gezeigt, AzCopy erstellt die Journal-Datei in den Standardspeicherort, also `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Wenn die Journal-Datei bereits vorhanden ist, wird AzCopy den Vorgang basierend auf der Journaldatei fortgesetzt.

Wenn Sie einen benutzerdefinierten Speicherort für die Journaldatei angeben möchten:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\

In diesem Beispiel erstellt die Journal-Datei aus, wenn es nicht bereits vorhanden ist. Wenn sie vorhanden ist, wird AzCopy den Vorgang basierend auf der Journaldatei fortgesetzt.

Wenn Sie einen AzCopy Vorgang fortsetzen möchten:

    AzCopy /Z:C:\journalfolder\

In diesem Beispiel Lebensläufen den letzten Vorgang, die zum Abschließen fehlgeschlagen ist.

### <a name="generate-a-log-file"></a>Erstellen einer Protokolldatei

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V

Wenn Sie die Option angeben `/V` ohne Angabe einen Dateipfad zu ausführlichen Protokoll, klicken Sie dann AzCopy erstellt die Protokolldatei am Standardspeicherort, also `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Andernfalls können Sie eine Protokolldatei in einem benutzerdefinierten Pfad erstellen:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log

Beachten Sie, dass, wenn Sie angeben, dass einen relativen Pfad folgen Option `/V`, wie `/V:test/azcopy1.log`, und klicken Sie dann das ausführliche Protokoll im aktuell geöffneten Verzeichnis in einen Unterordner namens erstellt wird `test`.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Geben Sie die Anzahl der gleichzeitigen Operationen zu starten

Option `/NC` gibt die Anzahl der parallele Kopiervorgänge an. Standardmäßig startet AzCopy eine bestimmte Anzahl von gleichzeitige Vorgänge, um die Daten Übertragungsdurchsatz zu erhöhen. Für Tabellenvorgänge ist die Anzahl der gleichzeitigen Operationen gleich der Anzahl der Prozessoren, die Sie installiert haben. Für Vorgänge Blob und die Datei, die Anzahl der gleichzeitigen Operationen ist gleich 8 Mal die Anzahl der Prozessoren, die Sie installiert haben. Wenn Sie über ein Netzwerk mit niedriger Bandbreite AzCopy ausgeführt werden, können Sie eine niedrigere Zahl für/NC Fehlers aufgrund Ressource induzierten Risikos zu vermeiden angeben.

### <a name="run-azcopy-against-azure-storage-emulator"></a>Führen Sie AzCopy für Azure Speicheremulator

Sie können AzCopy für den [Azure Speicheremulator](storage-use-emulator.md) für Blobs ausführen:

    AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S

und Tabellen:

    AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table

## <a name="azcopy-parameters"></a>AzCopy Parameter

Parameter für AzCopy werden im folgenden beschrieben. Sie können auch einen der folgenden Befehle über die Befehlszeile Hilfe eingeben, sich mit AzCopy:

- Ausführliche Hilfe zum Befehlszeilenoptionen für AzCopy:`AzCopy /?`
- Ausführliche Hilfe mit einem beliebigen AzCopy Parameter:`AzCopy /?:SourceKey`
- Beispiele für die Befehlszeile:`AzCopy /?:Samples`

### <a name="sourcesource"></a>/ Datenquelle: "Datenquelle"

Gibt die Quelldaten aus dem kopiert. Die Quelle kann ein Verzeichnis des Dateisystems, eines Containers Blob, Blob virtuelle Verzeichnis, eine Dateifreigabe Speicher, Speicher Dateiverzeichnis oder einer Azure Table sein.

**Anwendbar:** BLOBs, Dateien, Tabellen

### <a name="destdestination"></a>/ Ziel: "Ziel"

Gibt das Ziel zu kopieren. Das Ziel kann ein Verzeichnis des Dateisystems, eines Containers Blob, Blob virtuelle Verzeichnis, eine Dateifreigabe Speicher, Speicher Dateiverzeichnis oder einer Azure Table sein.

**Anwendbar:** BLOBs, Dateien, Tabellen

### <a name="patternfile-pattern"></a>/ Muster: "Datei-Muster"

Gibt eine Dateimuster, das zeigt an, welche Dateien zu kopieren. Das Verhalten des Parameters /Pattern wird durch den Speicherort der Quelldaten und dem Vorhandensein von rekursive Modus die Option bestimmt. Rekursive Modus wird über die Option/s angegeben.

Ist die angegebene Quelle ein Verzeichnis im Dateisystem, und klicken Sie dann standard Platzhaltern gültig sind, und das Dateimuster zur Verfügung gestellt werden mit Dateien im Systemverzeichnis abgeglichen. Wenn die Option/s angegeben wird, klicken Sie dann AzCopy auch dem angegebenen Muster mit allen Dateien im Unterordner unterhalb des Verzeichnisses entspricht.

Ist die angegebene Quelle einer Blob-Container oder virtuelle Verzeichnis, werden Platzhalterzeichen nicht angewendet. Wenn die Option/s angegeben wird, klicken Sie dann AzCopy das Muster für die angegebene Datei als Blob Präfix interpretiert. Wenn die Option/s nicht angegeben ist, klicken Sie dann AzCopy mit dem Dateimuster anhand genauen Blob-Namen übereinstimmt.

Wenn die angegebene Quelle ein Azure Dateifreigabe ist, und klicken Sie dann entweder Geben Sie den genauen Dateinamen, (z. B. abc.txt) Wenn Sie eine einzelne Datei kopieren, oder geben Sie die Option/s zum Kopieren aller Dateien in die rekursiv freigeben. Bei dem Versuch, eine Dateimuster und die Option angeben führt/s zusammen zu einem Fehler.

AzCopy verwendet die Groß-/Kleinschreibung entsprechen, wenn die/Source ein Blob-Container oder Blob virtuelle Verzeichnis ist und Groß-/Kleinschreibung nicht übereinstimmenden in allen anderen Fällen verwendet.

Das standardmäßige Dateimuster verwendet, wenn keine Dateimuster angegeben ist, ist *.* für einen Speicherort im Dateisystem oder ein leeres Präfix für ein Azure-Speicherort. Angeben von mehreren Datei Mustern wird nicht unterstützt.

**Anwendbar:** BLOBs, Dateien

### <a name="destkeystorage-key"></a>/ DestKey: "Speicher-Key"

Gibt den Speicher kontoschlüssel für die Ressource Ziel.

**Anwendbar:** BLOBs, Dateien, Tabellen

### <a name="destsassas-token"></a>/ DestSAS: "Sas-Token"

Gibt eine freigegebene Access Signatur (SAS) mit Berechtigungen zum Lesen und Schreiben für das Ziel an, (falls zutreffend). In Klammern Sie die SAS in doppelte Anführungszeichen ein, wie sie möglicherweise Befehlszeile Sonderzeichen enthält.

Ist die Ressource Ziel einer Blob-Container, eine Dateifreigabe oder eine Tabelle, können Sie diese Option, gefolgt von dem Token SAS entweder angeben, oder Sie können die SAS als Teil der Ziel-Blob-Container, Dateifreigabe oder Tabelle URI, ohne diese Option angeben.

Wenn Quell- und beide Blobs sind, muss das Ziel-Blob in dem Speicherkonto als die Quelle BLOB-befinden.

**Anwendbar:** BLOBs, Dateien, Tabellen

### <a name="sourcekeystorage-key"></a>/ SourceKey: "Speicher-Key"

Gibt den Speicher kontoschlüssel für die Ressource Quelle.

**Anwendbar:** BLOBs, Dateien, Tabellen

### <a name="sourcesassas-token"></a>/ SourceSAS: "Sas-Token"

Gibt eine freigegebene Access-Signatur mit Berechtigungen zum Lesen und Liste für die Quelle (falls zutreffend) an. In Klammern Sie die SAS in doppelte Anführungszeichen ein, wie sie möglicherweise Befehlszeile Sonderzeichen enthält.

Wenn die Quelle Ressource ein Blob-Container ist und weder einen Schlüssel noch eine SAS bereitgestellt wird, wird der Blob-Container über anonymer Zugriff gelesen werden.

Ist die Quelle einer Dateifreigabe oder die Tabelle, muss ein Schlüssel oder einem SAS bereitgestellt werden.

**Anwendbar:** BLOBs, Dateien, Tabellen

### <a name="s"></a>/ S

Gibt rekursive Modus für Kopiervorgänge. Im Modus rekursive kopiert AzCopy alle Blobs oder Dateien, die das angegebene Dateimuster, einschließlich der Empfänger in Unterordnern entsprechen.

**Anwendbar:** BLOBs, Dateien

### <a name="blobtypeblock--page--append"></a>/ BlobType: "blockieren" | "Seite" | "Anfügen"

Gibt an, ob das Ziel Blob ein Blob blockieren, einer Seitenblob oder ein Blob anfügen. Diese Option gilt nur, wenn Sie einen Blob hochladen. Andernfalls wird ein Fehler ausgelöst. Wenn das Ziel ein Blob ist und diese Option nicht, wird standardmäßig angegeben erstellt AzCopy einen Blob blockieren aus.

**Anwendbar:** BLOBs

### <a name="checkmd5"></a>/ CheckMD5

MD5-Hash für heruntergeladene Daten berechnet und überprüft, ob der MD5-Hash in dem Blob gespeichert oder Content-MD5 des Dateieigenschaft entspricht den berechneten Hash. Das Kontrollkästchen MD5 ist standardmäßig deaktiviert, daher müssen Sie diese Option, um das Kontrollkästchen MD5 ausführen, wenn das Herunterladen von Daten.

Beachten Sie, dass Azure-Speicher sichergestellt ist nicht, dass der MD5-Hash für die Blob oder der Datei gespeichert, auf dem neuesten Stand ist. Es ist Kunden Zuständigkeit MD5 aktualisiert, wenn die Blob oder einer Datei geändert wird.

AzCopy wird immer die Content-MD5-Eigenschaft für ein Azure Blob oder einer Datei nach dem Hochladen der Datei auf den Dienst.  

**Anwendbar:** BLOBs, Dateien

### <a name="snapshot"></a>/ Snapshot

Gibt an, ob Momentaufnahmen übertragen. Diese Option gilt nur, wenn die Quelle ein Blob ist.

Die übertragenen Blob Momentaufnahmen werden in diesem Format umbenannt: .extension Blob-Name (Snapshot-Zeit)

Standardmäßig werden Momentaufnahmen nicht kopiert.

**Anwendbar:** BLOBs

### <a name="vverbose-log-file"></a>/ V [ausführliche--Protokolldatei]

Ausgaben ausführlichen Statusnachrichten in einer Protokolldatei.

Standardmäßig ist die ausführliche Protokolldatei namens AzCopyVerbose.log in `%LocalAppData%\Microsoft\Azure\AzCopy`. Wenn Sie eine vorhandene Dateispeicherort für diese Option angeben, wird das ausführliche Protokoll in dieser Datei angefügt.  

**Anwendbar:** BLOBs, Dateien, Tabellen

### <a name="zjournal-file-folder"></a>/ Z [Journal-Dateiordner]

Gibt einen Dateiordner Journal für einen Vorgang fortsetzen.

AzCopy unterstützt immer fortsetzen, wenn ein Vorgang unterbrochen wurde.

Wenn diese Option nicht angegeben, oder es, ohne einen Ordnerpfad angegeben wird, erstellt AzCopy die Journal-Datei am Standardspeicherort dann also % LocalAppData%\Microsoft\Azure\AzCopy.

Jedes Mal, wenn Sie einen Befehl zum AzCopy, aus überprüft, ob eine Journaldatei im Standardordner vorhanden ist, oder gibt an, ob es sich in einem Ordner befindet, die Sie über diese Option angegeben haben. Wenn die Journal-Datei nicht in keinem der beiden Speicherorte vorhanden ist, wird den Vorgang als neue AzCopy, und generiert eine neue Journaldatei.

Wenn die Journal-Datei vorhanden ist, überprüft AzCopy, ob die Befehlszeile, die eingegeben werden die Befehlszeile in der Journaldatei entspricht. Wenn der Befehl Zeilenende entsprechen, Lebensläufen AzCopy den unvollständigen Vorgang aus. Wenn sie nicht übereinstimmen, werden Sie aufgefordert, die Journal-Datei, um einen neuen Vorgang zu starten, oder um den aktuellen Vorgang abzubrechen entweder zu überschreiben.

Die Journal-Datei wird nach dem erfolgreichen Abschluss des Vorgangs gelöscht.

Beachten Sie, dass einen Vorgang aus einer Journaldatei fortsetzen nach einer früheren Version von AzCopy erstellt wird nicht unterstützt.

**Anwendbar:** BLOBs, Dateien, Tabellen

### <a name="parameter-file"></a>/@:"parameter-file"

Gibt eine Datei, die Parameter enthält. AzCopy verarbeitet die Parameter in der Datei aus, als wäre sie in der Befehlszeile eingegeben worden.

Sie können in einer Antwortdatei mehrere Parameter angeben, in einer einzigen Zeile oder jeden Parameter in einer eigenen Zeile angeben. Beachten Sie, dass keine einzelner Parameter kann nicht über mehrere Zeilen erstrecken.

Antwortdateien können Linien Kommentare enthalten, die mit dem Nummernzeichen (#) beginnen.

Sie können mehrere Antwortdateien angeben. Beachten Sie aber, dass AzCopy geschachtelte Antwortdateien nicht unterstützt.

**Anwendbar:** BLOBs, Dateien, Tabellen

### <a name="y"></a>/ Y

Unterdrückt alle AzCopy Bestätigung Fragen.

**Anwendbar:** BLOBs, Dateien, Tabellen

### <a name="l"></a>/ L

Gibt einen Eintrag-Vorgang an. Es werden keine Daten kopiert.

AzCopy interpretiert wird, die mit dieser Option als eine Simulation zum Ausführen der Befehlszeile ohne diese option/l und zählen, wie viele Objekte kopiert werden, können Sie angeben, Option/v zur gleichen Zeit zum Überprüfen der Objekte in das ausführliche Protokoll kopiert werden.

Das Verhalten dieser Option wird auch durch den Speicherort der Quelldaten und dem Vorhandensein von die rekursive Modus/s und Datei Muster-Option /Pattern bestimmt.

AzCopy erfordert eine Berechtigung für Listen- und Lesen dieses Speicherorts der Datenquelle bei Verwendung dieser Option.

**Anwendbar:** BLOBs, Dateien

### <a name="mt"></a>/ MT

Legt die heruntergeladene Datei letzten Änderung Zeit identisch sein, wie die Quelle Blob oder der Datei an.

**Anwendbar:** BLOBs, Dateien

### <a name="xn"></a>/ XN

Schließt eine neuere Quelle Ressource an. Die Ressource werden nicht gleich ist die Uhrzeit der letzten Änderung der Quelle kopiert oder höher als Ziel.

**Anwendbar:** BLOBs, Dateien

### <a name="xo"></a>/ XO

Schließt eine ältere Quelle Ressource an. Die Ressource nicht kopiert werden, wenn die Uhrzeit der letzten Änderung der Quelle identisch ist oder älter als Ziel.

**Anwendbar:** BLOBs, Dateien

### <a name="a"></a>/ A

Uploads nur Dateien, die das Attribut Archiv festgelegt haben.

**Anwendbar:** BLOBs, Dateien

### <a name="iarashcnetoi"></a>/ IA: [RASHCNETOI]

Uploads nur Dateien mit keines der angegebenen Attribute festlegen.

Verfügbare Attribute umfassen:

- R = Schreibgeschützte Dateien
- A = Datenbank nutzbar Dateien für Archivierung
- S = Systemdateien
- H = Versteckte Dateien
- C = komprimierte Dateien
- N = normale Dateien
- E = verschlüsselte Dateien
- T = temporäre Dateien
- O = Offline-Dateien
- Kann ich Dateien nicht indiziert =

**Anwendbar:** BLOBs, Dateien

### <a name="xarashcnetoi"></a>/ XA: [RASHCNETOI]

Schließt Dateien, die keines der angegebenen Attribute festlegen aufweisen.

Verfügbare Attribute umfassen:

- R = Schreibgeschützte Dateien
- A = Datenbank nutzbar Dateien für Archivierung
- S = Systemdateien
- H = Versteckte Dateien
- C = komprimierte Dateien
- N = normale Dateien
- E = verschlüsselte Dateien
- T = temporäre Dateien
- O = Offline-Dateien
- Kann ich Dateien nicht indiziert =

**Anwendbar:** BLOBs, Dateien

### <a name="delimiterdelimiter"></a>/ Trennzeichen: "Trennzeichen"

Zeigt an, das zum Trennen von virtueller Verzeichnissen in einem BLOB-Trennzeichen.

Standardmäßig verwendet AzCopy / als Trennzeichen. Unterstützt AzCopy jedoch mit beliebigen allgemeine Zeichen (z. B. @, # oder %) als Trennzeichen. Wenn Sie eine der folgenden Sonderzeichen in der Befehlszeile enthalten müssen, setzen Sie den Dateinamen in doppelte Anführungszeichen.

Diese Option gilt nur für Blobs herunterladen.

**Anwendbar:** BLOBs

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: "Zahl-von-gleichzeitige-Vorgänge"

Gibt die Anzahl der gleichzeitige Vorgänge an.

AzCopy standardmäßig beginnt eine bestimmte Anzahl von gleichzeitige Vorgänge, um die Daten Übertragungsdurchsatz zu erhöhen. Beachten Sie, dass viele gleichzeitige Vorgänge in einer Umgebung mit niedriger Bandbreite möglicherweise die Verbindung überlastet und verhindern, dass die Vorgänge vollständig abgeschlossen. Schränken Sie gleichzeitige Vorgänge basierend auf der tatsächlichen verfügbaren Bandbreite an.

Die obere Grenze für gleichzeitige Vorgänge ist 512.

**Anwendbar:** BLOBs, Dateien, Tabellen

### <a name="sourcetypeblob--table"></a>/ SourceType: "Blob" | "Tabelle"

Gibt an, dass die `source` Ressource ist ein Blob in der Ausführung im Speicheremulator lokale Entwicklung-Umgebung zur Verfügung.

**Anwendbar:** BLOBs, Tabellen

### <a name="desttypeblob--table"></a>/ DestType: "Blob" | "Tabelle"

Gibt an, dass die `destination` Ressource ist ein Blob in der Ausführung im Speicheremulator lokale Entwicklung-Umgebung zur Verfügung.

**Anwendbar:** BLOBs, Tabellen

### <a name="pkrskey1key2key3"></a>/ PKRS: "Schlüssel1 Schlüssel2 # #key3 #..."

Teilt Partition Key Bereich zum Exportieren von Tabellendaten parallel, wodurch sich die Geschwindigkeit des Exportvorgangs erhöht aktivieren.

Wenn diese Option nicht angegeben ist, verwendet AzCopy einen einzelnen Thread um zu exportierenden Tabelle Einheiten. Wenn der Benutzer /PKRS gibt an, z. B.: "aa #bb", und klicken Sie dann auf AzCopy beginnt drei gleichzeitige Vorgänge.

Jede Operation exportiert eine der drei Partition Key Bereiche, wie unten dargestellt:

  [ersten Partitionsschlüssel, aa)

  [aa, bb)

  [letzten Partitionsschlüssel bb]

**Anwendbar:** Tabellen

### <a name="splitsizefile-size"></a>/ SplitSize: "Dateigröße"

Gibt an, die exportierte Datei Teilen Größe in MB, der minimale zulässige Wert 32 ist.

Wenn diese Option nicht angegeben ist, wird AzCopy Tabellendaten in Datei exportieren.

Wenn die Daten der Tabelle werden in ein Blob exportiert, und die exportierte Dateigröße die 200 GB für BLOB-Größe erreicht, wird AzCopy die exportierte Datei teilen, auch wenn diese Option nicht angegeben ist.

**Anwendbar:** Tabellen

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: "InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"

Gibt das Verhalten von Tabelle Daten importieren an.

- InsertOrSkip – überspringt eine vorhandene Entität oder fügt ein neues Element aus, wenn sie in der Tabelle nicht vorhanden ist.

- InsertOrMerge - führt eine vorhandene Entität oder fügt ein neues Element aus, wenn sie in der Tabelle nicht vorhanden ist.

- InsertOrReplace – ersetzt eine vorhandene Entität oder fügt ein neues Element aus, wenn sie in der Tabelle nicht vorhanden ist.

**Anwendbar:** Tabellen

### <a name="manifestmanifest-file"></a>/ Manifest: "Manifest-Datei"

Gibt die Manifestdatei an, für die Tabelle exportieren und Importieren von Vorgang.

Diese Option ist optional während des Exportvorgangs, AzCopy wird eine Manifestdatei mit den vordefinierten Namen generieren, wenn diese Option nicht angegeben wird.

Diese Option ist während des Importvorgangs zum Auffinden von Datendateien erforderlich.

**Anwendbar:** Tabellen

### <a name="synccopy"></a>/ SyncCopy

Gibt an, ob Blobs oder Dateien zwischen zwei Azure-Speicher Endpunkten synchron zu kopieren.

AzCopy standardmäßig verwendet serverseitige asynchrone kopieren. Geben Sie diese Option, um synchroner kopieren, sondern die Blobs oder Dateien in den lokalen Speicher downloads und diese dann auf Azure-Speicher hochgeladen an.

Sie können diese Option verwenden, beim Kopieren von Dateien in Blob-Speicher innerhalb Dateispeicher oder von BLOB-Speicher Dateispeicher oder umgekehrt.

**Anwendbar:** BLOBs, Dateien

### <a name="setcontenttypecontent-type"></a>/ SetContentType: "Inhaltstyp"

Gibt den MIME-Inhaltstyp für die Ziel-Blobs oder Dateien an.

AzCopy legt den Inhaltstyp für ein Blob oder eine Datei auf Application/Octet-Stream standardmäßig an. Den Inhaltstyp für alle Blobs oder Dateien können Sie festlegen, indem Sie einen Wert für diese Option explizit angeben.

Wenn Sie diese Option ohne einen Wert angeben, wird die AzCopy jedes Blob oder der Datei Inhaltstyp entsprechend ihrer Erweiterung festgelegt.

**Anwendbar:** BLOBs, Dateien

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: "JSON" | "CSV"

Gibt das Format der exportierten Datendatei Tabelle.

Wenn diese Option nicht angegeben ist, exportiert AzCopy standardmäßig Tabelle Datendatei im JSON-Format.

**Anwendbar:** Tabellen

## <a name="known-issues-and-best-practices"></a>Bekannte Probleme und bewährte Methoden

### <a name="limit-concurrent-writes-while-copying-data"></a>Beschränken Sie die gleichzeitige Schreibt beim Kopieren von Daten

Wenn Sie Blobs oder Dateien mit AzCopy kopieren, lassen Sie beachten Sie, dass eine andere Anwendung Daten ändert, während Sie ihn kopieren. Falls möglich, stellen Sie sicher, dass die Daten, die Sie kopieren, werden während der Kopiervorgang nicht geändert wird. Beim Kopieren einer virtuellen Festplatte mit einer Azure-virtuellen Computern verknüpft ist, stellen Sie beispielsweise sicher, dass aktuell keine anderen Programme auf der virtuellen Festplatte schreiben. Eine gute Möglichkeit, dies zu tun ist durch leasing der Ressource kopiert werden. Alternativ können Sie erstellen zuerst eine Momentaufnahme der virtuellen Festplatte und kopieren Sie dann den Snapshot.

Wenn Sie nicht, dass andere Programme verhindert von Blobs oder Dateien schreiben, während sie kopiert werden, lassen Sie beachten Sie, dass die Zeit, die der Auftrag endet, indem Sie die kopierten Ressourcen nicht mehr vollständige Unstimmigkeit mit den Quellressourcen auftritt.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Eine Instanz von AzCopy auf einem Computer ausführen.
AzCopy soll die Nutzung von Ihrem Computer Ressourcen zum Beschleunigen der Datenübertragung maximieren, sollten Sie nur eine Instanz von AzCopy auf einem Computer ausführen, und geben Sie die Option `/NC` Wenn Sie mehr gleichzeitige Vorgänge benötigen. Geben Sie weitere Informationen hierzu `AzCopy /?:NC` in der Befehlszeile.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>Aktivieren von FIPS-kompatible MD5-Algorithmus für AzCopy Wenn Sie "verwenden FIPS kompatiblen Algorithmen für Verschlüsselung, hashing und Signatur".
AzCopy standardmäßig .NET MD5 Implementierung verwendet, um die MD5 zu berechnen, wenn Objekte kopieren, aber es gibt einige Sicherheit Anforderungen, die AzCopy FIPS kompatibel MD5 Einstellung aktivieren benötigen.

Sie können eine App erstellen `AzCopy.exe.config` Eigenschaft `AzureStorageUseV1MD5` und Aside mit AzCopy.exe ablegen.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

Für die Eigenschaft "AzureStorageUseV1MD5" • True - wird der Standardwert, AzCopy .NET MD5 Implementierung verwendet.
• Falsch – AzCopy wird FIPS kompatibel MD5-Algorithmus verwendet.

Beachten Sie, dass FIPS-kompatible Algorithmen auf Ihrem Windows-Computer standardmäßig deaktiviert ist, können Sie geben Sie im Fenster Ausführen secpol.msc und aktivieren Sie diesen Schalter, bei der Einstellung Sicherheit -> lokaler Richtlinie-Sicherheit > Optionen -> Systemkryptografie: FIPS-kompatiblen Algorithmus für Verschlüsselung, hashing und Signatur.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure-Speicher und AzCopy finden Sie in den folgenden Ressourcen.

### <a name="azure-storage-documentation"></a>Azure-Speicher-Dokumentation:

- [Einführung in Azure-Speicher](storage-introduction.md)
- [So verwenden Sie BLOB-Speicher von .NET](storage-dotnet-how-to-use-blobs.md)
- [So verwenden Sie Dateispeicher von .NET](storage-dotnet-how-to-use-files.md)
- [Zum Verwenden von .NET Tabellenspeicher](storage-dotnet-how-to-use-tables.md)
- [So erstellen, verwalten oder Löschen eines Kontos Speicher](storage-create-storage-account.md)

### <a name="azure-storage-blog-posts"></a>Azure Speicher von Blogbeiträgen:
- [Einführung in Azure-Speicher Datenvorschau Bewegung Bibliothek](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
- [AzCopy: Einführung in die synchroner kopieren und angepasste Inhaltstyp](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
- [AzCopy: Ankündigung, allgemeine Verfügbarkeit von AzCopy 3.0 plus Preview-Version von AzCopy 4.0 mit Tabellen und Datei support](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
- [AzCopy: Für umfangreiche kopieren Szenarien optimiert](http://go.microsoft.com/fwlink/?LinkId=507682)
- [AzCopy: Unterstützung für Lesezugriff Geo redundante Speicher](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
- [AzCopy: Übertragen von Daten mit erneut wiederherzustellenden Modus und SAS token](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
- [AzCopy: Mithilfe von kopieren-Blob Cross-Konto](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
- [AzCopy: Upload/Download-Dateien für Azure Blobs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)
