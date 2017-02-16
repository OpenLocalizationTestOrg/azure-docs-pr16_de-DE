<properties
    pageTitle="So BLOB-Speicher (Objektspeicher) von Ruby verwenden | Microsoft Azure"
    description="Speichern von unstrukturierten Daten in der Cloud mit Azure Blob-Speicher (Objektspeicher)."
    services="storage"
    documentationCenter="ruby"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>


# <a name="how-to-use-blob-storage-from-ruby"></a>Zum Verwenden von Ruby Blob-Speicher

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>(Übersicht)

Azure Blob-Speicher ist ein Dienst, der unstrukturierte Daten als Objekte/Blobs in der Cloud gespeichert sind. BLOB-Speicher kann beliebige binäre Daten, beispielsweise ein Dokument, Media-Datei oder das Anwendungsinstallationsprogramm oder zu speichern. BLOB-Speicher wird auch als Objektspeicher bezeichnet.

In diesem Handbuch wird gezeigt, wie häufige Szenarien mit Blob-Speicher ausführen werden. Die Beispiele werden mithilfe der Ruby-API geschrieben. Die Szenarios dieser einbeziehen **Hochladen, wobei, herunterladen,** und **Löschen von** blobs

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Erstellen Sie eine Anwendung Ruby

Erstellen einer Ruby Anwendung. Anweisungen finden Sie unter [Ruby auf Schienen Webanwendung eine Azure-virtuellen Computers](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)

## <a name="configure-your-application-to-access-storage"></a>Konfigurieren der Anwendungs Zugriff auf Speicher

Wenn Azure-Speicher verwenden möchten, müssen Sie das Ruby Azure-Paket, das eine Reihe von Komfort Bibliotheken enthält, die mit der Speicher REST-Dienste kommunizieren herunterladen und verwenden.

### <a name="use-rubygems-to-obtain-the-package"></a>Verwenden Sie RubyGems, um das Paket zu erhalten.

1. Verwenden Sie eine Line Schnittstelle wie **PowerShell** (Windows), **Terminal** (Mac) oder **Bash** (Unix).

2. Geben Sie im Befehlsfenster zum Installieren der Gem und Abhängigkeiten "Gem installieren Azure" ein.

### <a name="import-the-package"></a>Das Paket importieren

Verwenden einen Texteditor, fügen Sie Folgendes an den Anfang der Ruby Datei für den Speicher verwenden möchten:

    require "azure"

## <a name="setup-an-azure-storage-connection"></a>Für die Einrichtung einer Verbindungs Azure-Speicher

Das Modul Azure liest die Umgebungsvariablen **AZURE\_Speicher\_Konto** und **AZURE\_Speicher\_ACCESS_KEY** Informationen erforderlich, um eine Verbindung mit Ihrem Konto Azure-Speicher. Wenn dieser Variablen nicht festgelegt werden, müssen Sie die Kontoinformationen vor der Verwendung von **Azure::Blob::BlobService** mit den folgenden Code angeben:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"


Um diese Werte aus einer Classic oder Ressourcenmanager Speicherkonto Azure-Portal zu erhalten:

1. Melden Sie sich mit dem [Azure-Portal](https://portal.azure.com)an.
2. Navigieren Sie zu der Speicher-Konto, das Sie verwenden möchten.
3. Klicken Sie in den Einstellungen Blade auf der rechten Seite auf **Zugriffstasten**.
4. In Access Tasten Blade, das angezeigt wird, wird die Zugriffstaste 1 und 2-Zugriffstaste angezeigt. Sie können eine der folgenden verwenden. 
5. Klicken Sie auf das Kopiersymbol, um die Taste in die Zwischenablage zu kopieren. 

Um diese Werte von einem Speicherkonto klassischen im klassischen Azure-Portal zu erhalten:

1. Melden Sie sich der [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Navigieren Sie zu der Speicher-Konto, das Sie verwenden möchten.
3. Klicken Sie auf **Verwalten ZUGRIFFSTASTEN** am unteren Rand des Navigationsbereichs.
4. Im Popup-Dialogfeld sehen Sie Speicher Kontoname, Zugriffstaste primären und sekundären Zugriffstaste. Zugriffstaste können Sie entweder der primären oder der sekundären. 
5. Klicken Sie auf das Kopiersymbol, um die Taste in die Zwischenablage zu kopieren.

## <a name="create-a-container"></a>Erstellen eines Containers

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Das Objekt **Azure::Blob::BlobService** können Sie mit Containern und Blobs arbeiten. Verwenden Sie zum Erstellen eines Containers der **Erstellen\_container()** Methode.

Im folgenden Code wird erstellt einen Container oder drucken Sie den Fehler, wenn eine vorhanden ist.

    azure_blob_service = Azure::Blob::BlobService.new
    begin
      container = azure_blob_service.create_container("test-container")
    rescue
      puts $!
    end

Wenn Sie die Dateien im Container öffentlich machen möchten, können Sie die Berechtigungen des Containers festlegen.

Sie können einfach ändern, die <strong>Erstellen\_container()</strong> Anruf übergeben der **: öffentlichen\_Access\_Ebene** Option:

    container = azure_blob_service.create_container("test-container",
      :public_access_level => "<public access level>")


Gültige Werte für die **: öffentlichen\_Access\_Ebene** Option sind:

* **Blob:** Gibt die vollständigen öffentlichen Lesezugriff für Container und Blob-Daten an. Clients können Blobs innerhalb des Containers über anonyme Anforderung auflisten, aber Sie können nicht aufgelistet werden Container innerhalb des Speicherkontos.

* **Container:** Gibt die öffentlichen Lesezugriff für Blobs an. BLOB-Daten in diesem Container können über anonyme Anforderung gelesen werden, aber Daten Container sind nicht verfügbar. Clients können nicht innerhalb des Containers über anonyme Anforderung Blobs aufgelistet werden.

Alternativ können Sie mithilfe die öffentlichen Zugriffsebene eines Containers ändern **festlegen\_Container\_acl()** Methode, um die öffentliche Zugriffsebene anzugeben.

Im folgenden Code wird die öffentliche Zugriffsebene **Container**Änderungen:

    azure_blob_service.set_container_acl('test-container', "container")

## <a name="upload-a-blob-into-a-container"></a>Hochladen eines BLOBs in einen container

Verwenden Sie zum Hochladen von Inhalten in ein Blob die **Erstellen\_blockieren\_blob()** Methode, um die Blob Erstellen einer Datei oder eine Zeichenfolge als den Inhalt des Blob verwenden.

Mit dem folgende Code lädt die Datei **test.png** als neue Blob mit dem Namen "Bild-Blob" im Container hoch.

    content = File.open("test.png", "rb") { |file| file.read }
    blob = azure_blob_service.create_block_blob(container.name,
      "image-blob", content)
    puts blob.name

## <a name="list-the-blobs-in-a-container"></a>Liste der Blobs in einem container

Verwenden Sie **list_containers()** Methode, um die Liste der Container.
Um die Blobs innerhalb eines Containers aufzulisten, verwenden Sie **Liste\_blobs()** Methode.

Dadurch wird die Urls der alle Blobs in allen Containern für das Konto ausgegeben.

    containers = azure_blob_service.list_containers()
    containers.each do |container|
      blobs = azure_blob_service.list_blobs(container.name)
      blobs.each do |blob|
        puts blob.name
      end
    end

## <a name="download-blobs"></a>Herunterladen von blobs

Um Blobs herunterladen möchten, verwenden Sie die **abrufen\_blob()** Methode, um den Inhalt abzurufen.

Im folgenden Code wird veranschaulicht, mit **abrufen\_blob()** des Inhalts "Bild-Blob" herunterladen und in einer lokalen Datei zu schreiben.

    blob, content = azure_blob_service.get_blob(container.name,"image-blob")
    File.open("download.png","wb") {|f| f.write(content)}

## <a name="delete-a-blob"></a>Löschen eines BLOBs
Verwenden Sie zum Löschen eines BLOBs schließlich die **Löschen\_blob()** Methode. Im folgenden Code wird veranschaulicht, wie einen Blob löschen.

    azure_blob_service.delete_blob(container.name, "image-blob")

## <a name="next-steps"></a>Nächste Schritte

Folgen Sie diesen Links, um komplexere Aufgaben zu lernen:

- [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure SDK für Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) Repository auf GitHub
- [Übertragen von Daten mit den AzCopy Befehlszeilenprogramms](storage-use-azcopy.md)
