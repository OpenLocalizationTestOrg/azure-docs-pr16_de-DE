<properties 
    pageTitle="Verschieben von Daten aus lokalen HDFS | Factory Azure-Daten" 
    description="Informationen Sie zum Verschieben von Daten aus lokalen HDFS Azure Data Factory verwenden." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Verschieben von Daten aus lokalen HDFS Azure Data Factory verwenden
In diesem Artikel wird beschrieben, wie Sie die Aktivität kopieren in einer Factory Azure-Daten zum Verschieben von Daten aus einer lokalen HDFS zu einem anderen Datenspeicher verwenden können. In diesem Artikel basiert auf Artikel [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) , der eine allgemeine Übersicht über das Verschieben von Daten mit Aktivität kopieren und unterstützten Data Store Kombinationen bietet.

Daten Factory unterstützt derzeit nur verschieben Daten aus einer lokalen HDFS für andere Datenspeicher, aber nicht zum Verschieben von Daten aus anderen Datenspeicher in einer lokalen HDFS.


## <a name="enabling-connectivity"></a>Aktivieren der Konnektivität
Factory Datendienst unterstützt das Herstellen einer Verbindung mit einer lokalen HDFS das Datenverwaltungsgateway verwenden. Finden Sie unter [Verschieben von Daten zwischen lokalen Orten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) Artikel lernen Datenverwaltungsgateway und eine schrittweise Anleitung zum Einrichten des Gateways. Verwenden des Gateways Verbindung zum HDFS, auch wenn es eine Neuerung Azure gehostet wird. 

Während der Installation von Gateway auf demselben lokalen Computer oder den Azure-virtuellen Computer als die HDFS empfehlen wir, dass Sie des Gateways auf einem separaten Computer/Azure IaaS virtueller Computer installieren. Gateways auf einem separaten Computer Probleme verringert Ressource Probleme und die Leistung verbessert. Bei der Installation des Gateways auf einem separaten Computer sollten der Computer auf den Computer mit der HDFS zugreifen. 


## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit, eine Verkaufspipeline zu erstellen, die Daten aus lokalen HDFS kopiert besteht darin, den Assistenten zum Kopieren von Daten verwenden. Finden Sie unter [Lernprogramm: Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) für eine schnelle Exemplarische Vorgehensweise zum Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von Daten. 

In den folgenden Beispielen bieten Stichprobe JSON-Definitionen, mit denen Sie eine Verkaufspipeline mithilfe von [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Wird gezeigt, wie Daten aus einer lokalen HDFS in einer Azure BLOB-Speicher zu kopieren. Daten können jedoch an die senken angegebener [So](data-factory-data-movement-activities.md#supported-data-stores) verwenden die Aktivität kopieren in Azure Data Factory kopiert werden.

## <a name="sample-copy-data-from-on-premises-hdfs-to-azure-blob"></a>Beispiel: Kopieren Sie Daten aus lokalen HDFS in Azure Blob

Dieses Beispiel zeigt, wie Sie Daten aus einer lokalen HDFS in Azure BLOB-Speicher kopieren. Daten kann jedoch kopierten **direkt** an die senken angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores) die Aktivität kopieren in Azure Data Factory verwenden.  
 
Im Beispiel weist die folgenden Daten Factory Elemente:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesHdfs](#hdfs-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [Dateifreigabe](#hdfs-dataset-type-properties).
4.  Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [FileSystemSource](#hdfs-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Im Beispiel kopiert Daten aus einer lokalen HDFS in eine Azure Blob stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben. 

Richten Sie als ersten Schritt das datenverwaltungsgateway aus. Die Anweisungen im Artikel [Verschieben von Daten zwischen lokalen Orten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) . 

**HDFS verknüpft service** In diesem Beispiel wird die Windows-Authentifizierung. Finden Sie unter [HDFS verknüpft Dienst](#hdfs-linked-service-properties) Abschnitt für unterschiedliche Arten von Authentifizierung, die Sie verwenden können. 

    {
        "name": "HDFSLinkedService",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }

**Azure verknüpft Speicherdienst**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**HDFS Eingabemethoden dataset** Dieses Dataset bezieht sich auf den Ordner HDFS DataTransfer/UnitTest /. Der Verkaufspipeline kopiert alle Dateien in diesem Ordner zum Ziel ein. 

Festlegen von "externe": "true" werden dem Daten Factory-Dienst informiert, dass das Dataset externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.
    
    {
        "name": "InputDataset",
        "properties": {
            "type": "FileShare",
            "linkedServiceName": "HDFSLinkedService",
            "typeProperties": {
                "folderPath": "DataTransfer/UnitTest/"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval":  1
            }
        }
    }




**Azure Blob ausgeben dataset**

Jede Stunde Daten in einer neuen Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Pfad des Ordners für das Blob wird dynamisch ausgewertet basierend auf der Startzeit des Segments, die verarbeitet wird. Der Pfad des verwendet Jahr, Monat, Tag und Stunden Teile der Startzeit.

    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Pipeline mit Aktivität kopieren**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass diese Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **FileSystemSource** festgelegt ist, und Typ der **Empfänger** auf **BlobSink**festgelegt ist. Die SQL-Abfrage für die Eigenschaft **Abfrage** angegebenen wählt die Daten in der letzten Stunde zu kopieren.
    
    {
        "name": "pipeline",
        "properties":
        {
            "activities":
            [
                {
                    "name": "HdfsToBlobCopy",
                    "inputs": [ {"name": "InputDataset"} ],
                    "outputs": [ {"name": "OutputDataset"} ],
                    "type": "Copy",
                    "typeProperties":
                    {
                        "source":
                        {
                            "type": "FileSystemSource"
                        },
                        "sink":
                        {
                            "type": "BlobSink"
                        }
                    },
                    "policy":
                    {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "00:05:00"
                    }
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="hdfs-linked-service-properties"></a>Verknüpfte Dienst HDFS Eigenschaften

Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die speziell für HDFS Service verknüpft.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| Typ | Die Eigenschaft muss auf festgelegt sein: **Hdfs** | Ja | 
| URL | URL der HDFS | Ja |
| encryptedCredential | [Neu-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) Ausgabe der Access-Anmeldeinformationen. | Nein |
| Benutzername | Benutzernamen für Windows-Authentifizierung. | Ja (für Windows-Authentifizierung)
| Kennwort | Kennwort für die Windows-Authentifizierung. | Ja (für Windows-Authentifizierung)
| authenticationType | Windows, oder anonymer. | Ja |
| gatewayName | Name des Gateways, das der Daten Factory-Dienst in Verbindung mit der HDFS verwendet werden sollen. | Ja |   

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für lokale HDFS finden Sie unter [Festlegen der Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

### <a name="using-anonymous-authentication"></a>Anonyme Authentifizierung

    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Anonymous",
                "userName": "hadoop",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }


### <a name="using-windows-authentication"></a>Mithilfe der Windows-Authentifizierung
    
    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }
 


## <a name="hdfs-dataset-type-properties"></a>HDFS Typ Datensatzeigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung finden Sie im Artikel [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinie eines Datasets JSON ähneln für alle Dataset-Typen (Azure SQL Azure Blob, Azure Table usw..).

Im Abschnitt **TypeProperties** unterscheidet sich für jede Art von Dataset und enthält Informationen über den Speicherort der Daten im Datenspeicher. Im Abschnitt TypeProperties für Dataset vom Typ **Dateifreigabe** (enthält HDFS Dataset) weist die folgenden Eigenschaften

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Ordnerpfad | Der Pfad zu dem Ordner. Beispiel:`myfolder`<br/><br/>Verwenden Sie Escapezeichen ' \ ' für Sonderzeichen in der Zeichenfolge. Beispiel: Geben Sie für Folder\subfolder, Ordner\\\\Unterordner, und geben Sie für d:\samplefolder, d:\\\\Beispielordner.<br/><br/>Sie können diese Eigenschaft mit **PartitionBy** Ordnerpfade auf Grundlage Segment haben kombinieren Anfangs-/Ende-Datum / Uhrzeit. | Ja
Dateiname | Geben Sie den Namen der Datei in den **Ordnerpfad** , wenn Sie die Tabelle, um zu einer bestimmten Datei im Ordner verweisen möchten. Wenn Sie einen beliebigen Wert für diese Eigenschaft nicht angeben, verweist die Tabelle auf alle Dateien in den Ordner.<br/><br/>Wenn FileName nicht für ein Dataset Ausgabe angegeben ist, der Namen der generierten Datei in den folgenden wäre dieses Format: <br/><br/>Daten. <Guid>txt (zum Beispiel:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | Nein
partitionedBy | PartitionedBy kann verwendet werden, um eine dynamische Ordnerpfad, Dateinamen für die Reihe Zeitdaten anzugeben. Beispiel: Ordnerpfad parametrisierte für jede Stunde Daten. | Nein
fileFilter | Geben Sie einen Filter verwendet werden soll, wählen Sie eine Teilmenge der Dateien in den Ordnerpfad statt alle Dateien ein. <br/><br/>Sind die Werte zulässig: `*` (mehrere Zeichen) und `?` (einzelnes Zeichen).<br/><br/>Beispiele für die 1:`"fileFilter": "*.log"`<br/>Beispiel 2:`"fileFilter": 2014-1-?.txt"`<br/><br/>**Hinweis**: FileFilter gilt für eine Eingabe Dateifreigabe-Dataset | Nein
| Format | Die folgenden Arten von Format werden unterstützt: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat**und **ParquetFormat**. Legen Sie die Eigenschaft **Typ** , klicken Sie unter Format auf einen der folgenden Werte ein. Finden Sie Details Abschnitten [TextFormat zurück, der angibt](#specifying-textformat), [AvroFormat zurück, der angibt](#specifying-avroformat), [JsonFormat zurück, der angibt](#specifying-jsonformat), [OrcFormat angeben](#specifying-orcformat)und [ParquetFormat angeben](#specifying-parquetformat) . Wenn Sie die Dateien als kopieren möchten – ist zwischen Datei-basierten Stores (binäre kopieren), können Sie den Formatabschnitt beide Definitionen Eingabe- und Dataset überspringen. | Nein 
| Komprimierung | Geben Sie den Typ und die Ebene der Komprimierung für die Daten ein. Unterstützte Datentypen sind: **GZip**, **Deflate**, und **BZip2** und unterstützten Ebenen sind: **Optimal** und **am schnellsten**. Komprimierungseinstellungen werden für Daten in **AvroFormat** oder **OrcFormat**derzeit nicht unterstützt. Weitere Informationen finden Sie unter [Unterstützung der Komprimierung](#compression-support) Abschnitt.  | Nein |



> [AZURE.NOTE] FileName und FileFilter können nicht gleichzeitig verwendet werden.


### <a name="using-partionedby-property"></a>Verwenden die Eigenschaft partionedBy

Wie im vorherigen Abschnitt erwähnt, können Sie eine dynamische Ordnerpfad, Dateinamen für die Reihe Zeitdaten mit PartitionedBy angeben. Sie können dazu die Daten Factory-Makros und Systemvariablen SliceStart, SliceEnd, die für einen angegebenen Daten Segment den logischen Zeitraum angeben. 

Weitere Informationen zu Zeit Reihe Datasets Planung und Segmente, finden Sie unter [Datasets erstellen](data-factory-create-datasets.md), [Planung & Ausführung](data-factory-scheduling-and-execution.md)und [Erstellen von Pipelines](data-factory-create-pipelines.md) Artikel. 

#### <a name="sample-1"></a>Beispiel 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

In diesem Beispiel {Segment} wird mit dem angegebenen Wert Daten Factory System Variablen SliceStart im Format (YYYYMMDDHH) ersetzt. Die SliceStart bezieht sich zur Startzeit des Segments. Der Ordnerpfad unterscheidet sich für jedes Segment. Beispiel: Wikidatagateway/Wikisampledataout/2014100103 oder Wikidatagateway/Wikisampledataout/2014100104.

#### <a name="sample-2"></a>Beispiel 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

In diesem Beispiel werden Jahr, Monat, Tag und die Uhrzeit der SliceStart in separaten Variablen extrahiert, die von den Ordnerpfad und den Dateinamen Eigenschaften verwendet werden.

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="hdfs-copy-activity-type-properties"></a>HDFS kopieren Aktivität Schrifteigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten verfügbar sind finden Sie unter Artikel [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien zur Verfügung stehen für alle Arten von Aktivitäten. 

Im Abschnitt TypeProperties der Aktivität verfügbaren Eigenschaften variieren andererseits bei jeder Aktivität. Aktivitäten, kopieren variieren je nach den Typen von Datenquellen und senken.

Aktivitäten, kopieren Wenn vom Typ **FileSystemSource** Quelle ist stehen die folgenden Eigenschaften im Abschnitt TypeProperties:

**FileSystemSource** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| Rekursive | Gibt an, ob die Daten rekursiv aus den Sub-Ordnern oder nur aus dem angegebenen Ordner gelesen werden. | True, False (Standard)| Nein |

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Leistung und optimieren  
Finden Sie unter [Kopieren Aktivität Leistung und Optimieren von Leitfaden für](data-factory-copy-activity-performance.md) die Leistung Einfluss der Daten Bewegung (Kopieren Aktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren sie wichtige Faktoren lernen.

