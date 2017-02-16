<properties
pageTitle="Indizierung Azure Blob-Speicher mit Azure suchen"
description="Erfahren Sie, wie Azure BLOB-Speicher indiziert und Extrahieren von Text aus Dokumenten mit Azure-Suche"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="10/16/2016"
ms.author="eugenesh" />

# <a name="indexing-documents-in-azure-blob-storage-with-azure-search"></a>Indizieren Dokumenten in Azure Blob-Speicher mit Azure suchen

Dieser Artikel beschreibt, wie Sie Azure suchen zum Indizieren von Dokumenten (z. B. PDF-Dateien, Microsoft Office-Dokumente und mehrere anderer gängiger Formate) in Azure Blob Storage gespeichert. Der neue Suche Azure Blob Indexer können dieses Verfahren, schnell und nahtlose. 

> [AZURE.IMPORTANT] Diese Funktion ist derzeit in der Vorschau. Es ist nur in die REST-API mit Version **2015-02-28-Vorschau**verfügbar. Bitte denken Sie daran, eine Vorschau APIs dienen zum Testen und bewerten, und nicht in der Herstellung Umgebungen verwendet werden.

## <a name="supported-document-formats"></a>Unterstützte Dokumentformate

Der Blob Indexer kann zum Extrahieren von Text aus den folgenden Dokumentformaten:

- PDF-DATEI
- Microsoft Office-Formate: DOCX/DOC, XLSX/XLS, PPTX/PPT, MSG (Outlook-e-Mails)  
- HTML-CODE
- XML
- ZIP
- EML
- Nur-Text-Dateien  
- JSON (Details finden Sie unter [Indizierung JSON-Blobs](search-howto-index-json-blobs.md) )
- CSV (Details finden Sie unter [Blobs Indizierung CSV](search-howto-index-csv-blobs.md) )

## <a name="setting-up-blob-indexing"></a>Einrichten von BLOB-Indizierung

Sie können eine mithilfe von Azure BLOB-Speicher Indexer festlegen:

- [Azure-portal](https://ms.portal.azure.com)
- Azure Suche [REST-API](https://msdn.microsoft.com/library/azure/dn946891.aspx)
- Azure Suche .NET SDK [Version 2.0-Vorschau](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) 

> [AZURE.NOTE] Einige Features (z. B. Feld Zuordnungen) sind noch nicht im Portal verfügbar und programmgesteuert verwendet werden müssen. 

In diesem Artikel richten wir Indexer die REST-API verwenden, indem Sie die folgenden drei Schritte: Erstellen einer Datenquelle, erstellen Sie einen Index, Indexer konfigurieren.

### <a name="step-1-create-a-data-source"></a>Schritt 1: Erstellen einer Datenquelle

Eine Datenquelle gibt an, welche Daten Sie indizieren, Anmeldeinformationen für den Zugriff auf die Daten und Richtlinien für den effizienten Identifizieren von Änderungen in den Daten (neue, geänderte oder gelöschte Zeilen) auf. Eine Datenquelle kann von mehrere Indexer in der gleichen Suchdienst verwendet werden.

BLOB-Indizierung zu können, muss die Datenquelle die folgenden erforderlichen Eigenschaften verfügen: 

- **Name** ist der eindeutige Name der Datenquelle innerhalb der Suchdienst an. 

- **Typ** muss `azureblob`. 

- **Anmeldeinformationen** stellt die Verbindungszeichenfolge für Speicher-Konto als das `credentials.connectionString` Parameter. Sie können die Verbindungszeichenfolge aus der Azure-Portal abrufen, durch Navigieren zu dem gewünschten Speicher Konto Blade > **Einstellungen** > **Tasten** und verwenden Sie den Wert "Primäre Verbindungszeichenfolge" oder "Sekundäre Verbindungszeichenfolge". 

- **Container** gibt einen Container in Ihrem Speicherkonto an. Standardmäßig können alle Blobs innerhalb des Containers abgerufen werden. Wenn Sie nur Index Blobs in einem bestimmten virtuellen Verzeichnis möchten, können Sie dieses Verzeichnis unter Verwendung des Parameters optional **Abfrage** angeben. 

Das folgende Beispiel veranschaulicht die DSN-Datei:

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
    }   

Weitere Informationen zur Datenquelle erstellen-API finden Sie unter [Datenquelle erstellen](search-api-indexers-2015-02-28-preview.md#create-data-source).

### <a name="step-2-create-an-index"></a>Schritt 2: Erstellen eines Indexes 

Index gibt die Felder in einem Dokument, Attributen und andere Konstrukte, die die Suche modellieren auftreten.  

Für BLOB-Indizierung, achten Sie darauf, dass Ihr Index eine durchsuchbare hat `content` zum Speichern von Blob-Feld.

    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-target-index",
        "fields": [
            { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
        ]
    }

Weitere Informationen über die Index-API erstellen finden Sie unter [Create Index](https://msdn.microsoft.com/library/dn798941.aspx)

### <a name="step-3-create-an-indexer"></a>Schritt 3: Erstellen eines Indexers 

Ein Indexer verbindet Datenquellen mit Ziel Suchindizes und Zeitplaninformationen bietet, damit Sie die Daten aktualisieren automatisieren können. Nachdem Sie der Index und die Datenquelle erstellt wurden, ist es relativ einfach, einen Indexer erstellen, der auf die Datenquelle und einen Zielindex verweist. Beispiel:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "blob-indexer",
      "dataSourceName" : "blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Mit diesem Indexer wird ausgeführt, alle zwei Stunden (Zeitplanintervall ist auf "PT2H" eingestellt). Klicken Sie zum Ausführen eines Indexers alle 30 Minuten, das Intervall zum "PT30M" festlegen. Geschätzte unterstützten Intervall beträgt 5 Minuten. Zeitplan optional – Wenn nicht angegeben ist, ein Indexer wird nur einmal ausgeführt, wenn erstellt. Sie können jedoch eine Indexer bei Bedarf zu einem beliebigen Zeitpunkt ausführen.   

Weitere Informationen zu den Indexer-API erstellen checken Sie [Indexer erstellen](search-api-indexers-2015-02-28-preview.md#create-indexer)aus.


## <a name="document-extraction-process"></a>Extraktion Dokument

Azure Suche indiziert jedes Dokument (Blob) wie folgt aus:

- Wird das gesamte Textinhalt des Dokuments in einer Zeichenfolgenfeld mit dem Namen extrahiert `content`. Wir unterstützen nicht aktuell zum Extrahieren des mehrere Dokumente eines einzelnen BLOBs:
    - Zum Beispiel ist eine CSV-Datei als ein einzelnes Dokument indiziert. Wenn Sie jede Zeile in eine CSV-Datei als ein einzelnes Dokument zu behandeln müssen, stimmen Sie für [diese UserVoice Vorschlag](https://feedback.azure.com/forums/263029-azure-search/suggestions/13865325-please-treat-each-line-in-a-csv-file-as-a-separate).
    - Ein Dokument verknüpften oder eingebetteten (beispielsweise ein ZIP-Archiv oder ein Word-Dokument mit eingebetteten Outlook e-Mail-Nachrichten mit Anlagen) wird auch als ein einzelnes Dokument indiziert.

- Falls vorhanden, sind benutzerdefinierter Metadateneigenschaften auf die Blob vorhanden wörtlich extrahiert. Die Metadateneigenschaften können auch zum Steuern von bestimmter Aspekte des Prozesses Extraktion Dokument – Weitere Informationen finden [Mithilfe von benutzerdefinierten Metadaten zum Steuerelement Dokument Extraktion](#CustomMetadataControl) verwendet werden.

- Standard Blob-Metadaten-Eigenschaften werden in die folgenden Felder extrahiert:

    - **Metadaten\_Speicher\_Namen** (Edm.String) – den Dateinamen des Blob. Angenommen, wenn Sie eine Blob-/my-container/my-folder/subfolder/resume.pdf haben, ist der Wert dieses Felds `resume.pdf`.

    - **Metadaten\_Speicher\_Pfad** (Edm.String) – die vollständige URI des Blob, einschließlich des Speicherkontos. Beispielsweise`https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`

    - **Metadaten\_Speicher\_Inhalt\_Typ** (Edm.String) – Inhaltstyp gemäß den Code, die Sie zum Hochladen von des BLOBs verwendet. Beispielsweise `application/octet-stream`.

    - **Metadaten\_Speicher\_letzten\_geändert** (Edm.DateTimeOffset) – der letzten Änderung für das Blob Zeitstempel. Azure Suche anhand dieser Zeitstempel geänderte Blobs, um zu vermeiden, erneute Indizierung alles nach der anfänglichen Indizierung identifiziert.

    - **Metadaten\_Speicher\_Größe** (Int64) - BLOB-Größe in Byte.

    - **Metadaten\_Speicher\_Inhalt\_md5** (Edm.String) – MD5 Hashs für den Blob-Inhalt, falls vorhanden.

- Speziell für jedes Dokumentformat Metadaten-Eigenschaften werden in der aufgeführten [hier](#ContentSpecificMetadata)extrahiert.

Sie benötigen keine Felder für alle oben genannten Eigenschaften im Suchindex definieren – nur die Eigenschaften für eine Anwendung benötigte erfassen. 

> [AZURE.NOTE] Häufig werden die Feldnamen in Ihrer vorhandenen Index von den Feldnamen generiert während der Extraktion Dokument unterscheiden. **Feld Zuordnungen** können Sie die Namen der Suche Azure bereitgestellt werden, um die Feldnamen im Suchindex zuordnen. Beispiel für ein Feld wird angezeigt, die Zuordnungen unter verwenden. 

## <a name="picking-the-document-key-field-and-dealing-with-different-field-names"></a>Das Feld mit dem Dokument auswählen und Umgang mit anderen Feldnamen

In Azure suchen identifiziert die Taste Dokument ein Dokument eindeutig. Jeder Suchindex müssen genau ein Feld vom Typ Edm.String. Das Feld mit das ist erforderlich für jedes Dokument, das auf den Index hinzugefügt wird (tatsächlich nur Feld erforderlich ist).  
   
Sie sollten sorgfältig überlegen, welches Feld extrahierten sollte das Feld mit dem für das Kontaktregister zuordnen. Die geeignet sind:

- **Metadaten\_Speicher\_Namen** – dies möglicherweise eine geeignete Kandidat, aber beachten Sie, dass 1) die Namen nicht eindeutig sein können, da Blobs mit demselben Namen in unterschiedlichen Ordnern und 2) den Namen stehen Ihnen möglicherweise enthält Zeichen, die im Dokument Tasten, z. B. Striche ungültig sind. Ungültige Zeichen nachdenken, mithilfe der `base64Encode` [Feld Zuordnung (Funktion)](search-indexer-field-mappings.md#base64EncodeFunction) – Wenn Sie dies tun, denken Sie daran, Dokument Tasten codieren, wenn sie in-API weitergegeben werden beispielsweise Nachschlagen Anrufe. (Z. B. in .NET die [UrlTokenEncode Methode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx) für diesen Zweck können Sie).

- **Metadaten\_Speicher\_Pfad** – verwenden den vollständigen Pfad Eindeutigkeit gewährleistet, aber der Pfad auf jeden Fall enthält `/` Zeichen, die [in einem Dokumentschlüssel ungültig](https://msdn.microsoft.com/library/azure/dn857353.aspx)sind.  Wie oben, haben Sie die Möglichkeit, die mithilfe von Tasten Codierung der `base64Encode` [(Funktion)](search-indexer-field-mappings.md#base64EncodeFunction).

- Wenn keine der oben genannten Optionen für sich arbeiten, können Sie eine benutzerdefinierte Metadateneigenschaft, die Blobs hinzufügen. Diese Option ist, jedoch alle Blobs die Metadateneigenschaft hinzuzufügende Prozesses Upload Blob erforderlich. Da der Schlüssel eine Eigenschaft erforderlich ist, tritt alle Blobs, die die Eigenschaft besitzen indiziert werden sollen.

> [AZURE.IMPORTANT] Wenn es keine explizite Zuordnung für das Feld mit dem im Index, Azure Suche automatisch verwendet `metadata_storage_path` als Schlüssel und Base-64 codiert Schlüsselwerte (die zweite Option oben).

In diesem Beispiel wählen Sie uns die `metadata_storage_name` -Feld als die Dokument-Taste. Nehmen wir auch an Ihren Index weist ein Feld mit einem mit dem Namen `key` und einem Feld `fileSize` für die Dokumentgröße zu speichern. Um Elemente nach oben, nach Bedarf zu verbinden, geben Sie die folgenden Feld Zuordnungen beim Erstellen oder aktualisieren Ihre Indexer an:

    "fieldMappings" : [
      { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
      { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
    ]

Um dies zu bringen alles zusammen, hier, wie Sie Feld Zuordnungen hinzufügen und Base-64-Codierung von Schlüsseln für einen vorhandenen Indexer aktivieren:

    PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "dataSourceName" : " blob-datasource ",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "fieldMappings" : [
        { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key", "mappingFunction" : { "name" : "base64Encode" } },
        { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
      ]
    }

> [AZURE.NOTE] Weitere Informationen zum Feld Zuordnungen finden Sie [in diesem Artikel](search-indexer-field-mappings.md)aus.

## <a name="incremental-indexing-and-deletion-detection"></a>Inkrementell indizieren und Löschvorgang Erkennung

Beim Einrichten einer Blob-Indexer für einen Zeitplan ausgeführt, sie erneut nur die geänderten Blobs gemäß des BLOBs des indiziert `LastModified` Zeitstempel.

> [AZURE.NOTE] Verfügen Sie nicht über eine Änderung Erkennung Richtlinie angeben – inkrementell Indizierung automatisch für Sie aktiviert ist.

Gehen Sie zum Löschen von Dokumenten zu unterstützen, "Weiche löschen" vor. Wenn Sie die Blobs sofortiges löschen, werden nicht entsprechende Dokumente aus dem Suchindex entfernt. Verwenden Sie stattdessen die folgenden Schritte aus:  

1. Fügen Sie eine benutzerdefinierte Metadateneigenschaft für dieses Blob Azure suchen an, dass es logisch gelöscht wird
2. Konfigurieren einer weiche Löschvorgang Erkennung Richtlinie für die Datenquelle
3. Nachdem der Indexer das Blob (dargestellt durch den Indizierungsstatus API) verarbeitet hat, können Sie physisch das Blob löschen

Beispielsweise die folgende Richtlinie betrachtet einen Blob gelöscht werden soll, wenn sie eine Metadateneigenschaft hat `IsDeleted` mit dem Wert `true`:

    PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "my-container", "query" : "my-folder" },
        "dataDeletionDetectionPolicy" : {
            "@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",   
            "softDeleteColumnName" : "IsDeleted",
            "softDeleteMarkerValue" : "true"
        }
    }   

<a name="ContentSpecificMetadata"></a>
## <a name="content-type-specific-metadata-properties"></a>Inhaltsmetadaten Typ-spezifische Eigenschaften

In der folgenden Tabelle werden für jede Dokumentformat erledigt Verarbeitung zusammengefasst, und beschreibt die Metadateneigenschaften von Azure Suche extrahierten.

Dokumentformat / Inhaltstyp | Inhaltstyp bestimmte Metadaten-Eigenschaften | Von Verarbeitungsdetails
-------------------------------|-------------------------------------------|-------------------
HTML-Code (`text/html`) | `metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` | Entfernen von HTML-Markup und Extrahieren von text
PDF (`application/pdf`) | `metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title`| Extrahieren von Text, einschließlich der eingebetteten Dokumente (außer Bilder)
DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` | Extrahieren von Text, einschließlich der eingebetteten Dokumente
Dokument (Anwendung/Msword) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` | Extrahieren von Text, einschließlich der eingebetteten Dokumente
XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` | Extrahieren von Text, einschließlich der eingebetteten Dokumente
XLS (Anwendung/vnd.ms-excel) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` | Extrahieren von Text, einschließlich der eingebetteten Dokumente
PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` | Extrahieren von Text, einschließlich der eingebetteten Dokumente
PPT (Anwendung/vnd.ms-Powerpoint) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` | Extrahieren von Text, einschließlich der eingebetteten Dokumente
MSG (Anwendung/vnd.ms-Outlook) | `metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_message_bcc`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` | Extrahieren von Text, einschließlich Anhänge
ZIP (Anwendung/Zip) | `metadata_content_type` | Extrahieren von Text aus allen Dokumenten im Archiv
XML (Anwendung/Xml) | `metadata_content_type`</br>`metadata_content_encoding`</br> | Entfernen von XML-Markup und Extrahieren von text
JSON (Anwendung/Json) | `metadata_content_type`</br>`metadata_content_encoding` | Extrahieren von text<br/>Hinweis: Wenn Sie mehrere Felder des Dokuments aus einer JSON-Blob extrahieren müssen, finden Sie unter [Indizierung JSON-Blobs](search-howto-index-json-blobs.md) Details
EML (Nachricht/rfc822) | `metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` | Extrahieren von Text, einschließlich Anhänge
Nur-Text (Text/nur) | `metadata_content_type`</br>`metadata_content_encoding`</br> | 

<a name="CustomMetadataControl"></a>
## <a name="using-custom-metadata-to-control-document-extraction"></a>Verwenden von benutzerdefinierten Metadaten zum Steuerelement Dokument Extraktion

Sie können ein Blob steuern bestimmte Aspekte des Prozesses Extraktion von BLOB-Indizierung und Dokument Metadateneigenschaften hinzufügen. Derzeit werden die folgenden Eigenschaften unterstützt:

Eigenschaftsname | Immobilienwert | Erläuterung
--------------|----------------|------------
AzureSearch_Skip | "true" | Weist den Blob-Indexer können Sie das Blob vollständig zu überspringen; Extraktion von weder Metadaten noch Inhalt ist übermitteln. Dies ist sinnvoll, wenn Sie bestimmte Inhaltstypen überspringen möchten, oder ein bestimmter Blob wiederholt schlägt fehl, und der Indizierung unterbrochen.
AzureSearch_SkipContent | "true" | Weist den Blob-Indexer können Sie nur Index die Metadaten und extrahieren Inhalt des Blob überspringen. Dies ist nützlich, wenn der BLOB-Inhalt nicht interessant ist, aber Sie weiterhin die Metadaten, die das Blob angefügt indizieren möchten.

<a name="IndexerParametersConfigurationControl"></a>
## <a name="using-indexer-parameters-to-control-document-extraction"></a>Verwenden von Indexerparameter Dokument Extraktion steuern

Mehrere Indexer Konfiguration Parameter zum Steuern der blobs und welche Teile eines BLOBs Inhalt und Metadaten verfügbar sind, werden indiziert. 

### <a name="index-only-the-blobs-with-specific-file-extensions"></a>Indizieren Sie nur die Blobs mit bestimmten Dateierweiterungen

Sie können einen index nur die Blobs mit der Dateinamenerweiterungen, die Sie angeben, indem Sie mit der `indexedFileNameExtensions` Konfiguration Indexerparameter. Der Wert ist eine Zeichenfolge mit einer durch Trennzeichen getrennte Liste von Dateierweiterungen (mit einer führenden Punkt). Beispielsweise an Index nur die. PDF und. Blobs DOCX, folgendermaßen vor: 

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
    }

### <a name="exclude-blobs-with-specific-file-extensions-from-indexing"></a>Ausschließen von Blobs mit bestimmten Dateierweiterungen Indizierung

Sie können mithilfe von Indizierung Blobs mit bestimmten Dateinamenerweiterungen Ausschließen der `excludedFileNameExtensions` Konfigurationsparameter. Der Wert ist eine Zeichenfolge mit einer durch Trennzeichen getrennte Liste von Dateierweiterungen (mit einer führenden Punkt). Angenommen, um alle Blobs solchen mit einem außer Index der. PNG und. JPEG-Erweiterungen folgendermaßen vor: 

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
    }

Wenn beide `indexedFileNameExtensions` und `excludedFileNameExtensions` Parameter vorhanden sind, werden zuerst untersucht Azure-Suche `indexedFileNameExtensions`, klicken Sie dann auf `excludedFileNameExtensions`. Das heißt, wenn die Erweiterung in beiden Listen vorhanden ist, es aus Indizierung ausgeschlossen werden sollen. 

### <a name="index-storage-metadata-only"></a>Nur Index Speicher-Metadaten

Sie können nur die Speichermetadaten für indizieren und vollständig zu überspringen, das Dokument Extraktion Prozess mithilfe der `indexStorageMetadataOnly` Konfigurationseigenschaft. Dies ist nützlich, wenn Sie nicht, dass des Inhalts des Dokuments erforderlich noch Sie die Inhaltsmetadaten Typ-spezifische Eigenschaften benötigen. Legen Sie hierzu die `indexStorageMetadataOnly` Eigenschaft zu `true`: 

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "indexStorageMetadataOnly" : true } }
    }

### <a name="index-both-storage-and-content-type-metadata-but-skip-content-extraction"></a>Indizieren Sie sowohl Speicher und Inhaltstyp Metadaten, aber überspringen Sie Extraktion von Inhalten

Wenn Sie alle Metadaten zu extrahieren, aber überspringen Extraktion von Inhalt für alle Blobs müssen, können Sie dieses Verhalten mithilfe der Konfigurations Indexer anstatt hinzufügen anfordern `AzureSearch_SkipContent` Metadaten zu den einzelnen BLOB-einzeln. Legen Sie hierzu die `skipContent` Konfiguration Indexereigenschaft zu `true`: 

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      ... other parts of indexer definition
      "parameters" : { "configuration" : { "skipContent" : true } }
    }

## <a name="help-us-make-azure-search-better"></a>Helfen Sie uns Azure-Suche zu verbessern.

Wenn Sie das Feature Serviceanfragen oder mit Vorschlägen zur Verbesserung der haben, wenden Sie sich bitte erreichen Sie uns auf unsere [UserVoice Website](https://feedback.azure.com/forums/263029-azure-search/).
