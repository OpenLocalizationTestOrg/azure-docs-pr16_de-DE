<properties 
    pageTitle="Kopieren von Daten zu/aus Azure BLOB-Speicher | Factory Azure-Daten" 
    description="Informationen Sie zum Kopieren von BLOB-Daten in Azure Data Factory. Verwenden Sie unseren Beispiel: So kopieren Sie Daten an und von Azure BLOB-Speicher und Azure SQL-Datenbank." 
    keywords="BLOB-Daten zu Azure Blob kopieren"
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
    ms.date="09/27/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-blob-using-azure-data-factory"></a>Verschieben von Daten an und von Azure Blob Azure Data Factory verwenden
In diesem Artikel wird erläutert, wie die Aktivität kopieren in Azure Data Factory zu verwenden, um die Daten an und von Azure Blob verschieben, indem Sie als Quelle BLOB-Daten aus einem anderen Datenspeicher. In diesem Artikel wird im Artikel [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) , die eine allgemeine Übersicht über das Verschieben von Daten mit der Aktivität kopieren und die unterstützten Data Store Kombinationen bietet erstellt.

## <a name="supported-sources-and-sinks"></a>Unterstützte Quellen und Empfängern
Finden Sie unter [unterstützte Datenspeicher](data-factory-data-movement-activities.md#supported-data-stores-and-formats) Tabelle eine Liste der Datenspeicher unterstützt als Quellen oder senken durch die Aktivität kopieren. Sie können Daten aus einem beliebigen Datenspeicher unterstützten Quelle zu Azure BLOB-Speicher oder aus Azure BLOB-Speicher auf einen beliebigen Datenspeicher unterstützten Empfänger verschieben.

Die Aktivität kopieren unterstützt das Kopieren von Daten aus dem und in allgemeine Azure-Speicher Firmen und Hotspot/kalt Blob-Speicher. Anfügen von der Aktivität unterstützt Textblocks, lesen oder Seite blobs, aber nur Blobs blockieren schreiben unterstützt. 

## <a name="create-pipeline"></a>Erstellen der Verkaufspipeline
Sie können eine Verkaufspipeline mit einer Kopie Aktivität erstellen, die Daten in einer Azure BLOB-Speicher über verschiedene Tools-APIs verschoben.  

- Assistent zum Kopieren von
- Azure-portal
- Visual Studio
- Azure PowerShell
- .NET API
- REST-API

Finden Sie unter [Kopieren Aktivität Lernprogramm](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) für eine schrittweise Anleitung zum Erstellen von einer Verkaufspipeline mit einer Kopie Aktivität auf verschiedene Arten.

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit, eine Verkaufspipeline zu erstellen, die Daten zu/aus Azure BLOB-Speicher kopiert besteht darin, den Assistenten zum Kopieren von Daten verwenden. Finden Sie unter [Lernprogramm: Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) für eine schnelle Exemplarische Vorgehensweise zum Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von Daten. 

In den folgenden Beispielen bieten Stichprobe JSON-Definitionen, mit denen Sie eine Verkaufspipeline mithilfe von [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Wird gezeigt, wie Daten an und von Azure BLOB-Speicher und Azure SQL-Datenbank zu kopieren. Daten kann jedoch kopierten **direkt** aus einer der Quellen an die senken angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores) die Aktivität kopieren in Azure Data Factory verwenden.

## <a name="sample-copy-data-from-azure-blob-to-azure-sql"></a>Beispiel: Kopieren Sie Daten aus Azure Blob in SQL Azure
 

Im folgende Beispiel gezeigt:

1.  Eine verknüpfte Dienst vom Typ [AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](#azure-storage-linked-service-properties).
3.  Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](#azure-blob-dataset-type-properties).
4.  Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties).
4.  Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit einer Aktivität kopieren, die [BlobSource](#azure-blob-copy-activity-type-properties) und [SqlSink](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties)verwendet.

Die Stichprobe Kopien Time-Serie zu einer SQL Azure BLOB Daten aus einer Azure-Tabelle stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben. 

**Azure SQL-verknüpfte Dienst:**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

**Azure verknüpft Speicherdienst:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure Data Factory unterstützt zwei Arten von Diensten Azure-Speicher verknüpft: **AzureStorage** und **AzureStorageSas**. Der ersten Phase Geben Sie die Verbindungszeichenfolge, die die kontoschlüssel enthält und für die höher ein, die Sie angeben des URIs freigegeben Access Signatur (SAS). Siehe Abschnitt " [Verknüpfte Services](#linked-services) " Details.  

**Azure Blob Eingabe-Dataset:**

Daten werden übernommen aus einer neuen Blob stündlich (Häufigkeit: Stunde, Intervall: 1). Der Ordnername Pfad und den Dateinamen für die Blob werden dynamisch ausgewertet, basierend auf der Startzeit des Segments, die verarbeitet wird. Der Pfad des verwendet Jahr, Monat und Tagesanteil der Startzeit und Dateinamen der Stundenteil des der Startzeit. "externe": "true" Einstellung informiert Daten Factory, dass die Tabelle externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
          "fileName": "{Hour}.csv",
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
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

**SQL Azure ausgeben Dataset:**

Die Kopien Beispieldaten zu einer Tabelle mit der Bezeichnung "MyTable" in einer SQL Azure-Datenbank. Erstellen Sie die Tabelle in der SQL Azure-Datenbank mit der gleichen Anzahl von Spalten wie erwartet die BLOB-CSV-Datei enthalten. Neue Zeilen werden stündlich zur Tabelle hinzugefügt.

    {
      "name": "AzureSqlOutput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Mit einer Kopie Aktivität Pipeline:**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass die Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **BlobSource** festgelegt ist, und Typ der **Empfänger** auf **SqlSink**festgelegt ist. 

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureSqlOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
          ]
       }
    }

## <a name="sample-copy-data-from-azure-sql-to-azure-blob"></a>Beispiel: Kopieren Daten aus Azure SQL Azure Blob
Im folgende Beispiel gezeigt:

1.  Eine verknüpfte Dienst vom Typ [AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](#azure-storage-linked-service-properties).
3.  Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties).
4.  Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](#azure-blob-dataset-type-properties).
4.  Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [SQLSource aus](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties) und [BlobSink](#azure-blob-copy-activity-type-properties)verwendet.


Im Beispiel kopiert Time-Serie Daten aus einer SQL Azure-Tabelle in eine Azure Blob stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben. 

**Azure SQL-verknüpfte Dienst:**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

**Azure verknüpft Speicherdienst:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure Data Factory unterstützt zwei Arten von Diensten Azure-Speicher verknüpft: **AzureStorage** und **AzureStorageSas**. Der ersten Phase Geben Sie die Verbindungszeichenfolge, die die kontoschlüssel enthält und für die höher ein, die Sie angeben des URIs freigegeben Access Signatur (SAS). Siehe Abschnitt " [Verknüpfte Services](#linked-services) " Details.  


**Azure SQL Eingabe-Dataset:**

Das Beispiel wird vorausgesetzt, Sie haben eine Tabelle "MyTable" erstellt, in SQL Azure und sie eine Spalte mit der Bezeichnung "Timestampcolumn" für die Reihe Uhrzeitdaten enthält. 

Festlegen von "externe": "true" informiert wurden Daten Factory-Dienst, dass die Tabelle externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.

    {
      "name": "AzureSqlInput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }


**Azure Blob ausgeben Dataset:**

Jede Stunde Daten in einer neuen Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Pfad des Ordners für das Blob wird dynamisch ausgewertet basierend auf der Startzeit des Segments, die verarbeitet wird. Der Pfad des verwendet Jahr, Monat, Tag und Stunden Teile der Startzeit.

    
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
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
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Pipeline mit der Aktivität kopieren:**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass die Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **SQLSource aus** festgelegt ist, und Typ der **Empfänger** auf **BlobSink**festgelegt ist. Die SQL-Abfrage für die Eigenschaft **SqlReaderQuery** angegeben, werden die Daten in der letzten Stunde zu kopierenden markiert.


    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2014-06-01T18:00:00",
            "end":"2014-06-01T19:00:00",
            "description":"pipeline for copy activity",
            "activities":[  
                {
                    "name": "AzureSQLtoBlob",
                    "description": "copy activity",
                    "type": "Copy",
                    "inputs": [
                      {
                        "name": "AzureSQLInput"
                      }
                    ],
                    "outputs": [
                      {
                        "name": "AzureBlobOutput"
                      }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 0,
                        "timeout": "01:00:00"
                    }
                }
             ]
        }
    }

## <a name="linked-services"></a>Verknüpfte Services
In den Beispielen haben Sie einen verknüpften Dienst des Typs **AzureStorage** so verknüpfen Sie ein Azure-Speicher-Konto mit einer Factory Daten verwendet. Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die speziell für Azure verknüpft Speicherdienst.

Es gibt zwei Arten von verknüpften Diensten, die Sie verwenden können, um-Azure Blob-Speicher mit einer Factory Azure-Daten zu verknüpfen. Sie sind: **AzureStorage** verknüpft, Dienste und **AzureStorageSas** verknüpft. Der Azure verknüpft Speicherdienst bietet die Factory Daten mit globaler Zugriff auf den Azure-Speicher. Während der Azure-Speicher SAS (Access-Signatur freigegeben) verknüpft bietet die Factory Daten mit Access beschränkt/Uhrzeit-Grenze um den Azure-Speicher. Es gibt keine anderen Unterschiede zwischen diese beiden verknüpften Dienste. Wählen Sie aus der verknüpften Dienst, der Ihren Bedürfnissen entspricht. Die folgenden Abschnitte enthalten weitere Details auf diese beiden verknüpften Dienste.

[AZURE.INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="azure-blob-dataset-type-properties"></a>Azure BLOB-Typ Datensatzeigenschaften
In den Beispielen haben Sie ein Dataset vom Typ **AzureBlob** verwendet, um einen Blob-Container und einen Ordner in einer Azure Blob-Speicher darzustellen. 

Eine vollständige Liste der JSON-Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung finden Sie im Artikel [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinie eines Datasets JSON ähneln für alle Dataset-Typen (Azure SQL Azure Blob, Azure Table usw..).

Im Abschnitt **TypeProperties** unterscheidet sich für jede Art von Dataset und enthält Informationen über den Speicherort, formatieren usw., der die Daten in der Datenquelle. Im Abschnitt TypeProperties für Dataset vom Typ **AzureBlob** Dataset weist die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| Ordnerpfad | Der Pfad zum Container und Ordner in den Blob-Speicher. Beispiel: Myblobcontainer\myblobfolder\ | Ja |
| Dateiname | Name des Blob. FileName ist optional und Groß-/Kleinschreibung beachtet.<br/><br/>Wenn Sie einen Dateinamen angeben, funktioniert die Aktivität (einschließlich kopieren) auf die bestimmte Blob.<br/><br/>Wenn FileName nicht angegeben ist, enthält kopieren alle Blobs in den Ordnerpfad für die Eingabe-Dataset an.<br/><br/>Wenn FileName nicht für ein Dataset Ausgabe angegeben ist, der Namen der generierten Datei in den folgenden wäre dieses Format: Daten. <Guid>txt (zum Beispiel:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | Nein |
| partitionedBy | PartitionedBy ist eine optionale Eigenschaft. Sie können es verwenden, um eine dynamische Ordnerpfad und den Dateinamen für die Reihe Zeitdaten anzugeben. Beispielsweise kann Ordnerpfad für jede Stunde Daten parametrisierte werden. Finden Sie im [Abschnitt verwenden PartitionedBy-Eigenschaft](#using-partitionedBy-property) Details und Beispielen aus. | Nein
| Format | Die folgenden Arten von Format werden unterstützt: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat**, **ParquetFormat**. Legen Sie die Eigenschaft **Typ** , klicken Sie unter Format auf einen der folgenden Werte ein. Finden Sie Details Abschnitten [TextFormat zurück, der angibt](#specifying-textformat), [AvroFormat zurück, der angibt](#specifying-avroformat), [JsonFormat zurück, der angibt](#specifying-jsonformat), [OrcFormat angeben](#specifying-orcformat)und [ParquetFormat angeben](#specifying-parquetformat) . Wenn Sie die Dateien als kopieren möchten – ist zwischen Datei-basierten Stores (binäre kopieren), können Sie den Formatabschnitt beide Definitionen Eingabe- und Dataset überspringen.| Nein
| Komprimierung | Geben Sie den Typ und die Ebene der Komprimierung für die Daten ein. Unterstützte Datentypen sind: **GZip**, **Deflate**, und **BZip2** und unterstützten Ebenen sind: **Optimal** und **am schnellsten**. Weitere Informationen finden Sie unter [Unterstützung der Komprimierung](#compression-support) Abschnitt. Aktuell, werden die komprimierungseinstellungen Daten in **AvroFormat**, **OrcFormat**oder **ParquetFormat**nicht unterstützt. Für diese Formate verwendet Daten Factory den Komprimierungscodec in den Metadaten aus, um die Daten zu lesen. Jedoch wählt Daten Factory beim Schreiben einer Datei in diesen Formaten den Standard-Komprimierung-Code für das entsprechende Format. SNAPPY für ParquetFormat und ZLIB für OrcFormat. | Nein |

### <a name="using-partitionedby-property"></a>Verwenden die Eigenschaft partitionedBy
Wie im vorherigen Abschnitt erwähnt, Sie können angeben, eine dynamische Ordnerpfad und den Dateinamen für die Reihe Zeitdaten mit Abschnitt **PartitionedBy** , Daten Factory Makros und das Systemvariablen: SliceStart und SliceEnd, die Start- und Endzeiten für einen angegebenen Daten Segment angeben.

Finden Sie unter [Daten Factory Systemvariablen](data-factory-scheduling-and-execution.md#data-factory-system-variables) und [Daten Factory Funktionen Bezug](data-factory-scheduling-and-execution.md#data-factory-functions-reference) zu erfahren Sie mehr über Data Factory Systemvariablen und Funktionen, die Sie im Abschnitt PartitionedBy verwenden können.   

Weitere Informationen zu Zeit Reihe Datasets Planung und Segmente, finden Sie unter [Datasets erstellen](data-factory-create-datasets.md) und [Planung & Ausführung](data-factory-scheduling-and-execution.md) Artikel.

#### <a name="sample-1"></a>Beispiel 1

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

In diesem Beispiel {Segment} wird mit dem angegebenen Wert Daten Factory System Variablen SliceStart im Format (YYYYMMDDHH) ersetzt. Die SliceStart bezieht sich zur Startzeit des Segments. Der Ordnerpfad unterscheidet sich für jedes Segment. Beispiel: Wikidatagateway/Wikisampledataout/2014100103 oder Wikidatagateway/Wikisampledataout/2014100104

#### <a name="sample-2"></a>Beispiel 2

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


## <a name="azure-blob-copy-activity-type-properties"></a>Azure Blob kopieren Aktivität Schrifteigenschaften  
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten verfügbar sind finden Sie unter Artikel [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Datasets und Richtlinien zur Verfügung stehen für alle Arten von Aktivitäten.

Im Abschnitt TypeProperties der Aktivität verfügbaren Eigenschaften variieren andererseits bei jeder Aktivität. Aktivitäten, kopieren variieren je nach den Typen von Datenquellen und senken

Wenn Sie Daten aus einer Azure BLOB-Speicher verschieben, legen Sie den Quelltyp in der Kopie Aktivität zu **BlobSource**. Auf ähnliche Weise, wenn Sie Daten auf einer Azure BLOB-Speicher verschieben, legen Sie den Typ der Empfänger in der Kopie Aktivität zu **BlobSink**. Dieser Abschnitt enthält eine Liste der Eigenschaften von BlobSource und BlobSink unterstützt. 

**BlobSource** unterstützt die folgenden Eigenschaften im Abschnitt **TypeProperties** an:

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- | 
| Rekursive | Gibt an, ob die Daten rekursiv aus den Sub-Ordnern oder nur aus dem angegebenen Ordner gelesen werden. | True (Standardwert), False | Nein | 

**BlobSink** unterstützt Abschnitt **TypeProperties** folgende Eigenschaften:

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| copyBehavior | Definiert das Verhalten beim Kopieren, wenn die Quelle BlobSource oder Dateisystem ist. | **PreserveHierarchy:** behält die Hierarchie im Zielordner. Der relative Pfad der Quelldatei in Quellordner entspricht der relative Pfad der Zieldatei zum Zielordner.<br/><br/>**FlattenHierarchy:** alle Dateien im Ordner "Datenquelle" in die erste Ebene von Zielordner sind. Die Zieldateien haben Namen automatisch generiert. <br/><br/>**MergeFiles: (Standard)** werden alle Dateien aus dem Quellordner zu einer Datei zusammengeführt. Wenn der Name der Datei/Blob angegeben wird, wäre der verbundenen Dateiname den angegebenen Namen; Andernfalls wäre automatisch generierten Dateinamen. | Nein |

**BlobSource** unterstützt auch diese beiden Eigenschaften für Abwärtskompatibilität. 

- **TreatEmptyAsNull**: Gibt an, ob null oder eine leere Zeichenfolge als null-Wert zu behandeln.
- **SkipHeaderLineCount** - gibt an, wie viele Zeilen übersprungen werden müssen. Es gilt nur beim Eingabe-Dataset TextFormat verwendet wird.

Entsprechend unterstützt **BlobSink** die folgende Eigenschaft für Abwärtskompatibilität.

- **BlobWriterAddHeader**: Gibt an, ob eine Kopfzeile der Spaltendefinitionen beim Schreiben in einem Dataset Ausgabe hinzufügen. 

Datasets unterstützt jetzt die folgenden Eigenschaften, die die gleiche Funktionalität implementieren: **TreatEmptyAsNull**, **SkipLineCount**, **FirstRowAsHeader**.

Die folgende Tabelle enthält Anleitungen zur Verwendung von neuen Dataset Eigenschaften anstelle diese Blob Quelle/Empfänger Eigenschaften. 

| Kopieren Sie die Eigenschaft Aktivität | DataSet-Eigenschaft |
| :---------------------- | :---------------- | 
| Klicken Sie auf BlobSource skipHeaderLineCount | SkipLineCount und FirstRowAsHeader. Zeilen werden zuerst übersprungen, und klicken Sie dann die erste Zeile als Header gelesen wird. |
| Klicken Sie auf BlobSource treatEmptyAsNull | Klicken Sie auf Eingabe-Dataset treatEmptyAsNull |
| Klicken Sie auf BlobSink blobWriterAddHeader | Klicken Sie auf die Ausgabe Dataset firstRowAsHeader | 

Siehe Abschnitt ausführliche Informationen zu diesen Eigenschaften zu [TextFormat angeben](#specifying-textformat) .    

### <a name="recursive-and-copybehavior-examples"></a>Beispiele für rekursive und copyBehavior
In diesem Abschnitt beschreibt das sich daraus ergebende Verhalten beim Kopieren für unterschiedliche Kombinationen aus rekursive und CopyBehavior Werte. 

Rekursive | copyBehavior | Resultierende Verhalten
--------- | ------------ | --------
WAHR | preserveHierarchy | Für einen Quellordner Ordner1 mit der folgenden Struktur: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Zielordner Ordner1 wird mit derselben Struktur als Quelle erstellt.<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.  
WAHR | flattenHierarchy | Für einen Quellordner Ordner1 mit der folgenden Struktur: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Das Ziel ist Ordner1 mit der folgenden Struktur erstellt: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen für Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen für Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen für File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen für File5
WAHR | mergeFiles | Für einen Quellordner Ordner1 mit der folgenden Struktur: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Das Ziel ist Ordner1 mit der folgenden Struktur erstellt: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1 + Datei2 + Datei3 + File4 + Datei 5 Inhalt in einer Datei mit automatisch generierten Dateinamen zusammengeführt werden
falsch | preserveHierarchy | Für einen Quellordner Ordner1 mit der folgenden Struktur: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Zielordner Ordner1 wird mit der folgenden Struktur erstellt.<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/><br/><br/>Subfolder1 mit Datei3, File4 und File5 werden nicht übernommen.
falsch | flattenHierarchy | Für einen Quellordner Ordner1 mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Zielordner Ordner1 wird mit der folgenden Struktur erstellt.<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen für Datei2<br/><br/><br/>Subfolder1 mit Datei3, File4 und File5 werden nicht übernommen.
falsch | mergeFiles | Für einen Quellordner Ordner1 mit der folgenden Struktur:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Zielordner Ordner1 wird mit der folgenden Struktur erstellt.<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1 + Datei2 Inhalt werden in eine Datei mit automatisch generierten Namen zusammengeführt. automatisch generierte Namen Datei 1<br/><br/>Subfolder1 mit Datei3, File4 und File5 werden nicht übernommen.

  


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Leistung und optimieren  
Finden Sie unter [Kopieren Aktivität Leistung und Optimieren von Leitfaden für](data-factory-copy-activity-performance.md) die Leistung Einfluss der Daten Bewegung (Kopieren Aktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren sie wichtige Faktoren lernen.

