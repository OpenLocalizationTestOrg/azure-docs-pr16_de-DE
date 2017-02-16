<properties 
    pageTitle="Verschieben von Daten aus Amazon einfache Speicherdienst mit Daten Factory | Microsoft Azure" 
    description="Informationen Sie zum Verschieben von Daten aus einer einfachen Amazon-Speicherdienst (S3) Azure Data Factory verwenden." 
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
    ms.date="10/24/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Verschieben von Daten aus Amazon einfache Speicherdienst Azure Data Factory verwenden

In diesem Artikel wird beschrieben, wie Sie die Aktivität kopieren in einer Factory Azure-Daten verwenden können, zum Verschieben von Daten aus Amazon einfache Speicherdienst (S3) zu einem anderen Daten gespeichert. In diesem Artikel basiert auf [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) Artikel, der eine allgemeine Übersicht über das Verschieben von Daten und eine Liste der unterstützten Quelle/Empfänger Datenspeicher mit Kopieren Aktivität enthält.  

Daten Factory unterstützt derzeit nur Daten von Amazon S3 für andere Datenspeicher, aber nicht zum Verschieben von Daten aus anderen Datenspeicher Amazon S3.

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Zum Kopieren von Daten aus Amazon S3 stellen Sie sicher, dass Sie unter Berechtigungen gewährt wurden:

- **S3:GetObject** und **S3:GetObjectVersion** für Amazon S3 Objekt Vorgänge
- **S3:ListBucket** und **S3:ListAllMyBuckets** (im Assistenten zum Kopieren von nur verwendet) für Amazon S3 Periode Vorgänge 

Sie können die vollständige Liste der Amazon S3 Zugriffsrechte mit Details aus [Berechtigungen zurück, der angibt, in einer Richtlinie](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html)suchen.

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit, eine Verkaufspipeline zu erstellen, die Daten von Amazon S3 kopiert besteht darin, den Assistenten zum Kopieren von Daten verwenden. Finden Sie unter [Lernprogramm: Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) für eine schnelle Exemplarische Vorgehensweise zum Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von Daten. 

Im folgende Beispiel enthält die Stichprobe JSON-Definitionen, die Sie verwenden können, erstellen Sie eine Verkaufspipeline mithilfe von [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Es wird gezeigt, wie Daten von Amazon S3 in Azure BLOB-Speicher zu kopieren. Daten können jedoch eine von der senken angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores)kopiert werden.

## <a name="sample-copy-data-from-amazon-s3-to-azure-blob"></a>Beispiel: Kopieren Sie Daten aus Amazon S3 in Azure Blob
Dieses Beispiel zeigt, wie Sie Daten aus einer S3 Amazon in einer Azure BLOB-Speicher kopieren. Daten kann jedoch kopierten **direkt** an die senken angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores) die Aktivität kopieren in Azure Data Factory verwenden.  
 
Im Beispiel weist die folgenden Daten Factory Elemente:

- Eine verknüpfte Dienst vom Typ [AwsAccessKey](#linked-service-properties).
- Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AmazonS3](#dataset-type-properties).
- Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [FileSystemSource](#copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Im Beispiel kopiert Daten der Amazon S3 in eine Azure Blob stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben. 

**Dienst Amazon S3 verknüpft**

    {
        "name": "AmazonS3LinkedService",
        "properties": {
            "type": "AwsAccessKey",
            "typeProperties": {
                "accessKeyId": "<access key id>",
                "secretAccessKey": "<secret access key>"
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

**Eingabe S3 Amazon-dataset**

Festlegen von **"externe": WAHR** informiert Sie dem Daten Factory-Dienst, dass das Dataset externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird. Klicken Sie auf eine Eingabe-Dataset, die nicht von einer Aktivität in der Verkaufspipeline erzeugt wurde, legen Sie diese Eigenschaft auf True.

    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }



**Azure Blob ausgeben dataset**

Jede Stunde Daten in einer neuen Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Pfad des Ordners für das Blob wird dynamisch ausgewertet basierend auf der Startzeit des Segments, die verarbeitet wird. Der Pfad des verwendet Jahr, Monat, Tag und Stunden Teile der Startzeit.

    {
        "name": "AzureBlobOutputDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass die Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **FileSystemSource** festgelegt ist, und Typ der **Empfänger** auf **BlobSink**festgelegt ist. 
    
    {
        "name": "CopyAmazonS3ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "FileSystemSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonS3InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutputDataSet"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "AmazonS3ToBlob"
                }
            ],
            "start": "2014-08-08T18:00:00Z",
            "end": "2014-08-08T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Verknüpfte Diensteigenschaften

Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die bestimmte Amazon S3 (**AwsAccessKey**) verknüpft Dienst.

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------- | ------- |  
| accessKeyID | ID des geheimen Zugriffstaste. | Zeichenfolge | Ja |
| secretAccessKey | Geheimnis Zugriffstaste selbst. | Verschlüsselte geheimen Zeichenfolge | Ja | 


## <a name="dataset-type-properties"></a>Typ Datensatzeigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung finden Sie im Artikel [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinie ähneln für alle Dataset-Typen (Azure SQL Azure Blob, Azure Table usw..).

Im Abschnitt **TypeProperties** unterscheidet sich für jede Art von Dataset und enthält Informationen über den Speicherort der Daten im Datenspeicher. Im Abschnitt TypeProperties für Dataset vom Typ **AmazonS3** (enthält Amazon S3 Dataset) weist die folgenden Eigenschaften

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------- | ------ | 
| bucketName | Der Name des S3 Periode. | Zeichenfolge | Ja |
| Schlüssel | Die S3 Objekt-Taste. | Zeichenfolge | Nein | 
| Präfix | Präfix für die S3 Objekt-Taste. Objekte, deren Schlüssel mit diesem Präfix beginnen, ausgewählt sind. Gilt nur beim leer ist. | Zeichenfolge | Nein | 
| Version | Die Version des Objekts S3, wenn S3 versionsverwaltung aktiviert ist. | Zeichenfolge | Nein |  
| Format | Die folgenden Arten von Format werden unterstützt: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat**, **ParquetFormat**. Legen Sie die Eigenschaft **Typ** , klicken Sie unter Format auf einen der folgenden Werte ein. Finden Sie Details Abschnitten [TextFormat zurück, der angibt](#specifying-textformat), [AvroFormat zurück, der angibt](#specifying-avroformat), [JsonFormat zurück, der angibt](#specifying-jsonformat), [OrcFormat angeben](#specifying-orcformat)und [ParquetFormat angeben](#specifying-parquetformat) . Wenn Sie die Dateien als kopieren möchten – ist zwischen Datei-basierten Stores (binäre kopieren), können Sie den Formatabschnitt beide Definitionen Eingabe- und Dataset überspringen.| Nein
| Komprimierung | Geben Sie den Typ und die Ebene der Komprimierung für die Daten ein. Unterstützte Datentypen sind: **GZip**, **Deflate**, und **BZip2** und unterstützten Ebenen sind: **Optimal** und **am schnellsten**. Die komprimierungseinstellungen werden für Daten in **AvroFormat** oder **OrcFormat**derzeit nicht unterstützt. Weitere Informationen finden Sie unter [Unterstützung der Komprimierung](#compression-support) Abschnitt.  | Nein |

> [AZURE.NOTE] BucketName + Taste gibt den Speicherort des Objekts S3, wobei Zelle ist der Stammcontainer für S3 Objekte und Schlüssel S3 Objekt den vollständigen Pfad, ein.

### <a name="sample-dataset-with-prefix"></a>Beispiel für Dataset mit Präfix

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "prefix": "testFolder/test",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }

### <a name="sample-data-set-with-version"></a>Beispiel für Datengruppe zurück (mit Version)

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "version": "WBeMIxQkJczm0CJajYkHf0_k6LhBmkcL",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


### <a name="dynamic-paths-for-s3"></a>Dynamische Pfade für S3

In der Stichprobe verwenden wir feste Werte für die Schlüssel und BucketName Eigenschaften im Dataset Amazon S3. 

    "key": "testFolder/test.orc",
    "bucketName": "testbucket",

Lassen Sie die Taste und BucketName dynamisch zur Laufzeit mithilfe von Systemvariablen wie SliceStart berechnen von Daten Factory ein.

    "key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
    "bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"

Sie können auch für die Eigenschaft Präfix eines Datasets Amazon S3 vornehmen. Eine Liste der unterstützten Funktionen und Variablen finden Sie unter [Data Factory-Funktionen und Systemvariablen](data-factory-functions-variables.md) . 


[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]


## <a name="copy-activity-type-properties"></a>Kopieren Sie die Schrifteigenschaften Aktivität

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten verfügbar sind finden Sie unter Artikel [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien zur Verfügung stehen für alle Arten von Aktivitäten. 

Im Abschnitt **TypeProperties** der Aktivität verfügbaren Eigenschaften variieren andererseits bei jeder Aktivität. Aktivitäten, kopieren variieren je nach den Typen von Datenquellen und senken.

Wenn Quelle in der Kopie Aktivität vom Typ **FileSystemSource** ist (der Amazon S3 enthält), stehen die folgenden Eigenschaften im Abschnitt TypeProperties:

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- | 
| Rekursive | Gibt an, ob rekursiv S3 Liste Objekte im Verzeichnis. | Wahrheitswert | Nein | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Leistung und optimieren  
Finden Sie unter [Kopieren Aktivität Leistung und Optimieren von Leitfaden für](data-factory-copy-activity-performance.md) die Leistung Einfluss der Daten Bewegung (Kopieren Aktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren sie wichtige Faktoren lernen.

## <a name="next-steps"></a>Nächste Schritte
Finden Sie unter den folgenden Artikeln: 
- [Lernprogramm kopieren Aktivität](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) für eine schrittweise Anleitung zum Erstellen einer Verkaufspipeline mit einer Aktivität kopieren. 
