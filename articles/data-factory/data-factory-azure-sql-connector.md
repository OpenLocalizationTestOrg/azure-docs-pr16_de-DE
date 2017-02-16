<properties 
    pageTitle="Verschieben von Daten zu/aus Azure SQL-Datenbank | Microsoft Azure" 
    description="Informationen Sie zum Verschieben von Daten aus/mit Azure Data Factory Azure SQL-Datenbank." 
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
    ms.date="09/20/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-sql-database-using-azure-data-factory"></a>Verschieben von Daten an und von Azure SQL-Datenbank mit Azure Data Factory
In diesem Artikel wird beschrieben, wie Sie die Aktivität kopieren in einer Factory Azure-Daten zum Verschieben von Daten zu/aus Azure SQL-Datenbank aus dem und in einem anderen Datenspeicher verwenden können. In diesem Artikel wird im Artikel [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) , die eine allgemeine Übersicht über das Verschieben von Daten mit Aktivität kopieren und unterstützten Data Store Kombinationen bietet erstellt. 

## <a name="supported-sources-and-sinks"></a>Unterstützte Quellen und Empfängern
Finden Sie unter [unterstützte Datenspeicher](data-factory-data-movement-activities.md#supported-data-stores-and-formats) Tabelle eine Liste der Datenspeicher unterstützt als Quellen oder senken durch die Aktivität kopieren. Sie können Daten aus einem beliebigen Datenspeicher unterstützten Datenquellen mit Azure SQL-Datenbank oder aus Azure SQL-Datenbank mit einem beliebigen Datenspeicher unterstützten Empfänger verschieben.

## <a name="create-pipeline"></a>Erstellen der Verkaufspipeline
Sie können eine Verkaufspipeline mit einer Kopie Aktivität erstellen, die Daten in einer SQL Azure-Datenbank über verschiedene Tools-APIs verschoben.  

- Assistent zum Kopieren von
- Azure-portal
- Visual Studio
- Azure PowerShell
- .NET API
- REST-API

Eine schrittweise Anleitung zum Erstellen einer Verkaufspipeline mit einer Kopie Aktivität auf verschiedene Arten finden Sie unter [Kopieren Aktivität Lernprogramm](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) .   

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit, eine Verkaufspipeline zu erstellen, die Daten zu/aus Azure SQL-Datenbank kopiert besteht darin, den Assistenten zum Kopieren von Daten verwenden. Finden Sie unter [Lernprogramm: Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) für eine schnelle Exemplarische Vorgehensweise zum Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von Daten. 

In den folgenden Beispielen bieten Stichprobe JSON-Definitionen, mit denen Sie eine Verkaufspipeline mithilfe von [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Wird gezeigt, wie Daten an und von Azure SQL-Datenbank und Azure BLOB-Speicher kopieren. Daten kann jedoch kopierten **direkt** aus einer der Quellen an die senken angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores) die Aktivität kopieren in Azure Data Factory verwenden.

## <a name="sample-copy-data-from-azure-sql-database-to-azure-blob"></a>Beispiel: Kopieren von Daten aus Azure SQL-Datenbank in Azure Blob

Die gleiche definiert die folgenden Daten Factory Elemente:

1. Eine verknüpfte Dienst vom Typ [AzureSqlDatabase](#azure-sql-linked-service-properties).
2. Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureSqlTable](#azure-sql-dataset-type-properties). 
4. Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [SQLSource aus](#azure-sql-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Im Beispiel Zeit-Serie Daten kopiert (stündlich, täglich, usw.) aus einer Tabelle in SQL Azure-Datenbank in ein Blob stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben.  

**Azure SQL-verknüpfte-Dienst**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

Finden Sie im Abschnitt [Verknüpfte Azure SQL-Dienst](#azure-sql-linked-service-properties) für die Liste der Eigenschaften, die von diesem Dienst verknüpften unterstützt. 

**Azure Blob-Speicher verknüpft-Dienst**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Finden Sie im Artikel [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) für die Liste der Eigenschaften, die von diesem Dienst verknüpften unterstützt. 

**Eingabe Azure SQL-dataset**

Das Beispiel wird vorausgesetzt, Sie haben eine Tabelle "MyTable" erstellt, in SQL Azure und sie eine Spalte mit der Bezeichnung "Timestampcolumn" für die Reihe Uhrzeitdaten enthält. 

Festlegen von "externe": "true" wird dem Azure-Daten Factory-Dienst informiert, dass das Dataset externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.

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

Finden Sie im Abschnitt [SQL Azure-Typ Datensatzeigenschaften](#azure-sql-dataset-type-properties) für die Liste der Eigenschaften, die von diesem Typ Dataset unterstützt.  

**Azure Blob ausgeben dataset**

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

Finden Sie im Abschnitt [Azure Blob-Typ Datensatzeigenschaften](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) für die Liste der Eigenschaften, die von diesem Typ Dataset unterstützt.  

**Pipeline mit Aktivität kopieren**

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

Im Beispiel wird die **SqlReaderQuery** für die SQLSource aus angegeben. Die Aktivität kopieren führt diese Abfrage für die Quelle Azure SQL-Datenbank, die Daten zu erhalten. Alternativ können Sie eine gespeicherte Prozedur angeben, nach Angabe von **SqlReaderStoredProcedureName** und **StoredProcedureParameters** (wenn die gespeicherte Prozedur Parameter akzeptiert).

Wenn Sie entweder SqlReaderQuery oder SqlReaderStoredProcedureName nicht angeben, werden die Spalten im Abschnitt Struktur des Datasets JSON definiert verwendet, zum Erstellen einer Abfrage für die Azure SQL-Datenbank ausgeführt. Beispiel: `select column1, column2 from mytable`. Wenn die Definition der Dataset nicht die Struktur verfügt, werden alle Spalten aus der Tabelle ausgewählt. 


Die Liste der Eigenschaften von SQLSource aus und BlobSink unterstützt finden Sie im Abschnitt [Sql-Quelle](#sqlsource) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) . 


## <a name="sample-copy-data-from-azure-blob-to-azure-sql-database"></a>Beispiel: Kopieren von Daten aus Azure Blob mit Azure SQL-Datenbank

Im Beispiel wird die folgenden Daten Factory Elemente definiert:  

1.  Eine verknüpfte Dienst vom Typ [AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties).
4.  Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) und [SqlSink](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties)verwendet.

Die Stichprobe Kopien Time-Serie zu einer Tabelle in SQL Azure BLOB Daten (stündlich, täglich, usw.) aus Azure-Datenbank stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben. 


**Azure SQL-verknüpfte-Dienst**
    
    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

Finden Sie im Abschnitt [Verknüpfte Azure SQL-Dienst](#azure-sql-linked-service-properties) für die Liste der Eigenschaften, die von diesem Dienst verknüpften unterstützt. 

**Azure Blob-Speicher verknüpft-Dienst**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Finden Sie im Artikel [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) für die Liste der Eigenschaften, die von diesem Dienst verknüpften unterstützt.

**Eingabe Azure Blob-dataset**

Daten werden übernommen aus einer neuen Blob stündlich (Häufigkeit: Stunde, Intervall: 1). Der Ordnername Pfad und den Dateinamen für die Blob werden dynamisch ausgewertet, basierend auf der Startzeit des Segments, die verarbeitet wird. Der Pfad des verwendet Jahr, Monat und Tagesanteil der Startzeit und Dateinamen der Stundenteil des der Startzeit. "externe": "true" Einstellung informiert dem Daten Factory-Dienst, dass diese Tabelle externe Daten Fabrik und nicht durch eine Aktivität in der Factory Daten erzeugt.

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

Finden Sie im Abschnitt [Azure Blob-Typ Datensatzeigenschaften](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) für die Liste der Eigenschaften, die von diesem Typ Dataset unterstützt.

**Azure SQL-Ausgabe-dataset**

Im Beispiel werden die Daten in einer Tabelle "MyTable" in SQL Azure kopiert. Erstellen Sie die Tabelle in SQL Azure, mit die gleiche Anzahl von Spalten wie erwartet die BLOB-CSV-Datei enthalten. Neue Zeilen werden stündlich zur Tabelle hinzugefügt. 

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

Finden Sie im Abschnitt [SQL Azure-Typ Datensatzeigenschaften](#azure-sql-dataset-type-properties) für die Liste der Eigenschaften, die von diesem Typ Dataset unterstützt.

**Pipeline mit Aktivität kopieren**

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
                "type": "BlobSource",
                "blobColumnSeparators": ","
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

Die Liste der Eigenschaften von SqlSink und BlobSource unterstützt finden Sie im Abschnitt [Sql-Empfänger](#sqlsink) und [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) . 


## <a name="azure-sql-linked-service-properties"></a>Azure SQL-verknüpfte Diensteigenschaften
In den Beispielen haben Sie einen verknüpften Dienst des Typs **AzureSqlDatabase** zum Verknüpfen von einer SQL Azure-Datenbank in eine Factory Daten verwendet. Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die speziell für SQL Azure-Verknüpfte Dienst an.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Typ | Die Eigenschaft muss auf festgelegt sein: **AzureSqlDatabase** | Ja |
| connectionString | Geben Sie die Verbindung zur Instanz Azure SQL-Datenbank für die Eigenschaft ConnectionString erforderlichen Informationen an. | Ja |

> [AZURE.NOTE] Konfigurieren von [Azure SQL-Datenbank-Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) dem Datenbankserver [Azure Services auf den Server zugreifen](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure)dürfen. Wenn Sie Daten auf Azure SQL-Datenbank aus externen Azure einschließlich aus lokalen Datenquellen mit Daten Factory Gateway kopieren möchten, konfigurieren Sie darüber hinaus entsprechenden Bereich von IP-Adresse für den Computer, der Senden von Daten mit Azure SQL-Datenbank. 

## <a name="azure-sql-dataset-type-properties"></a>Azure SQL Typ Datensatzeigenschaften
In den Beispielen für haben Sie ein Dataset vom Typ **AzureSqlTable** verwendet, um eine Tabelle in einer SQL Azure-Datenbank darzustellen. 

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung finden Sie im Artikel [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinie eines Datasets JSON ähneln für alle Dataset-Typen (Azure SQL Azure Blob, Azure Table usw..). 

Im Abschnitt TypeProperties unterscheidet sich für jede Art von Dataset und enthält Informationen über den Speicherort der Daten im Datenspeicher. Im Abschnitt **TypeProperties** für das Dataset vom Typ **AzureSqlTable** weist die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Tabellenname | Name der Tabelle in der Instanz von SQL Azure-Datenbank, der auf Verknüpfte Dienst verweist. | Ja |

## <a name="azure-sql-copy-activity-type-properties"></a>Azure SQL-Kopie Aktivität Schrifteigenschaften
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten verfügbar sind finden Sie unter Artikel [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinie stehen für alle Arten von Aktivitäten.

> [AZURE.NOTE] Die Aktivität kopieren übernimmt nur eine Eingabe und die Ausgabe nur ein.

Im Abschnitt **TypeProperties** der Aktivität verfügbaren Eigenschaften variieren andererseits bei jeder Aktivität. Aktivitäten, kopieren variieren je nach den Typen von Datenquellen und senken. 

Wenn Sie Daten aus einer SQL Azure-Datenbank verschieben, legen Sie den Quelltyp in der Kopie Aktivität zu **SQLSource aus**. Auf ähnliche Weise, wenn Sie Daten in einer SQL Azure-Datenbank verschieben, legen Sie den Typ der Empfänger in der Kopie Aktivität zu **SqlSink**. Dieser Abschnitt enthält eine Liste der Eigenschaften von SQLSource aus und SqlSink unterstützt. 

### <a name="sqlsource"></a>SQLSource aus

In Aktivität kopieren bei die Quelle vom Typ **SQLSource aus**, stehen die folgenden Eigenschaften im Abschnitt **TypeProperties** :

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| sqlReaderQuery | Verwenden Sie die benutzerdefinierte Abfrage, um Daten zu lesen. | SQL-Abfragezeichenfolge. Beispiel: `select * from MyTable`.  | Nein |
| sqlReaderStoredProcedureName | Namen der gespeicherten Prozedur, die Daten aus der Quelltabelle liest. | Namen der gespeicherten Prozedur. | Nein |
| storedProcedureParameters | Parameter für die gespeicherte Prozedur. | Name/Wert-Paare. Namen und die Groß-/Kleinschreibung von Parametern müssen den Namen und die Groß-/Kleinschreibung des Parameter der gespeicherten Prozedur übereinstimmen. | Nein |

Wenn für die SQLSource aus der **SqlReaderQuery** angegeben ist, führt die Aktivität kopieren diese Abfrage für die Quelle Azure SQL-Datenbank, die Daten zu erhalten. Alternativ können Sie eine gespeicherte Prozedur angeben, nach Angabe von **SqlReaderStoredProcedureName** und **StoredProcedureParameters** (wenn die gespeicherte Prozedur Parameter akzeptiert). 

Wenn Sie entweder SqlReaderQuery oder SqlReaderStoredProcedureName nicht angeben, werden die Spalten im Abschnitt Struktur des Datasets JSON definiert zum Erstellen einer Abfrage verwendet (`select column1, column2 from mytable`) für die Azure SQL-Datenbank ausgeführt. Wenn die Definition der Dataset nicht die Struktur verfügt, werden alle Spalten aus der Tabelle ausgewählt. 

> [AZURE.NOTE] Wenn Sie **SqlReaderStoredProcedureName**verwenden, müssen Sie einen Wert für die Eigenschaft **Tabellenname** im Dataset JSON angeben. Es gibt keine Validierungen durch für diese Tabelle ausgeführt. 

### <a name="sqlsource-example"></a>Beispiel für SQLSource aus

    "source": {
        "type": "SqlSource",
        "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
        "storedProcedureParameters": {
            "stringData": { "value": "str3" },
            "id": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
        }
    }

**Die Definition der gespeicherten Prozedur:** 

    CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
    (
        @stringData varchar(20),
        @id int
    )
    AS
    SET NOCOUNT ON;
    BEGIN
         select *
         from dbo.UnitTestSrcTable
         where dbo.UnitTestSrcTable.stringData != stringData
        and dbo.UnitTestSrcTable.id != id
    END
    GO


### <a name="sqlsink"></a>SqlSink 

**SqlSink** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| writeBatchTimeout | Wartezeit für Stapel einfügen nach Abschluss des Vorgangs bevor es abläuft. | TimeSpan<br/><br/> Beispiel: "00: 30:00" (30 Minuten). | Nein | 
| writeBatchSize | Fügt Daten in der SQL-Tabelle an, wenn die Puffergröße WriteBatchSize erreicht. | Ganzzahl (Anzahl von Zeilen)| Nein (Standard: 10000)
| sqlWriterCleanupScript | Geben Sie eine Abfrage für Kopieren Aktivität auszuführende so, dass die Daten eines bestimmten Segments bereinigt werden. Weitere Informationen finden Sie unter [Wiederholbarkeit Abschnitt](#repeatability-during-copy). | Eine Abfrage-Anweisung.  | Nein |
| sliceIdentifierColumnName | Geben Sie einen Spaltennamen für Kopieren Aktivität automatisch generiert Segment Bezeichner enthält, füllen Sie die Daten eines bestimmten Segments erneut ausführen, wenn Bereinigen verwendet wird. Weitere Informationen finden Sie unter [Wiederholbarkeit Abschnitt](#repeatability-during-copy). | Spaltenname einer Spalte mit dem Datentyp des binary(32). | Nein |
| sqlWriterStoredProcedureName | Namen der gespeicherten Prozedur die Upserts (Updates/Insert) Daten in die Zieltabelle. | Namen der gespeicherten Prozedur. | Nein |
| storedProcedureParameters | Parameter für die gespeicherte Prozedur. | Name/Wert-Paare. Namen und die Groß-/Kleinschreibung von Parametern müssen den Namen und die Groß-/Kleinschreibung des Parameter der gespeicherten Prozedur übereinstimmen. | Nein | 
| sqlWriterTableType | Geben Sie einen Typ Tabellennamen in der gespeicherten Prozedur verwendet werden. Kopieren Aktivität bereitgestellt, die Daten verschoben wird in einer temporären Tabelle mit diesen Table-Datentyp. Gespeicherte Prozedurcode kann dann die Daten mit den vorhandenen Daten kopierte zusammenführen. | Ein Tabellenname Typ. | Nein |

#### <a name="sqlsink-example"></a>Beispiel für SqlSink

    "sink": {
        "type": "SqlSink",
        "writeBatchSize": 1000000,
        "writeBatchTimeout": "00:05:00",
        "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
        "sqlWriterTableType": "CopyTestTableType",
        "storedProcedureParameters": {
            "id": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" },
            "decimalData": { "value": "1", "type": "Decimal" }
        }
    }

## <a name="identity-columns-in-the-target-database"></a>Identitätsspalten in der Zieldatenbank
Dieser Abschnitt enthält ein Beispiel für das Kopieren von Daten aus einer Quelltabelle ohne eine Identitätsspalte in eine Zieltabelle mit einer Identitätsspalte. 

**Quelltabelle:** 

    create table dbo.SourceTbl
    (
           name varchar(100),
           age int
    )

**Zieltabelle:**

    create table dbo.TargetTbl
    (
           id int identity(1,1),
           name varchar(100),
           age int
    )


Beachten Sie, dass die Zieltabelle eine Identitätsspalte. 

**DSN-Dataset JSON-Datei**

    {
        "name": "SampleSource",
        "properties": {
            "type": " SqlServerTable",
            "linkedServiceName": "TestIdentitySQL",
            "typeProperties": {
                "tableName": "SourceTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }

**Ziel Dataset JSON-definition**

    {
        "name": "SampleTarget",
        "properties": {
            "structure": [
                { "name": "name" },
                { "name": "age" }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "TestIdentitySQLSource",
            "typeProperties": {
                "tableName": "TargetTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": false,
            "policy": {}
        }   
    }


Benachrichtigung, die als Ihre Tabelle Quell- und Zielwebsites anderes Schema haben (Ziel verfügt über eine zusätzliche Spalte mit Identität). In diesem Szenario müssen Sie **Struktur** -Eigenschaft in der Zielliste Dataset-Definition angegeben werden, die die Identitätsspalte einschließen nicht. 

Klicken Sie dann ordnen Sie die Spalten aus der Quelle Dataset Spalten im Ziel-Dataset. Finden Sie [Beispiele für Spalte Zuordnung](#column-mapping-samples) im Abschnitt Beispiel. 

[AZURE.INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)] 

[AZURE.INCLUDE [data-factory-sql-invoke-stored-procedure](../../includes/data-factory-sql-invoke-stored-procedure.md)]
 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-sql-server--azure-sql-database"></a>Geben Sie die Zuordnung für SQL Server & Azure SQL-Datenbank

Wie angegeben führt die [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) Artikel kopieren Aktivität automatische Konvertieren des Datentyps von Arten von Datenquellen zu ignorieren Typen mit dem folgenden Ansatz für-Schritt 2:

1. Konvertieren von Datentypen systemeigenen Quelle in .NET Typ
2. Konvertieren von .NET Typ in einer systemeigenen Empfänger Typ

Beim Verschieben von Daten zu und aus SQL Azure SQLServer, Sybase die folgenden Zuordnungen aus SQL-Typ in .NET Typ und umgekehrt verwendet werden.

Die Zuordnung ist identisch mit den SQL Server Datentypen zuordnen für ADO.NET.

| SQL Server-Datenbank-Engine Typ | .NET Framework-Typ |
| ------------------------------- | ------------------- |
| bigint | Int64 |
| Binärzahl | Byte] |
| Bit | Boolesch |
| Zeichen | Zeichenfolge, die Zeichen] |
| Datum | "DateTime" |
| "DateTime" | "DateTime" |
| datetime2 | "DateTime" |
| DateTimeOffset | DateTimeOffset |
| Dezimalzahl | Dezimalzahl |
| FILESTREAM Attribut (varbinary(max)) | Byte] |
| Frei verschieben | Double |
| Bild | Byte] | 
| Ganzzahl | Int32 | 
| Geld | Dezimalzahl |
| NCHAR | Zeichenfolge, die Zeichen] |
| ntext | Zeichenfolge, die Zeichen] |
| numerische | Dezimalzahl |
| nvarchar | Zeichenfolge, die Zeichen] |
| Real | Einzelne |
| RowVersion | Byte] |
| smalldatetime | "DateTime" |
| smallint | Int16 |
| smallmoney | Dezimalzahl | 
| sql_variant | Objekt * |
| Text | Zeichenfolge, die Zeichen] |
| Zeit | TimeSpan |
| Zeitstempel | Byte] |
| tinyint | Byte-Zeichen |
| uniqueidentifier | GUID |
| varbinary |  Byte] |
| varchar | Zeichenfolge, die Zeichen] |
| XML | XML |


[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Leistung und optimieren  
Finden Sie unter [Kopieren Aktivität Leistung und Optimieren von Leitfaden für](data-factory-copy-activity-performance.md) die Leistung Einfluss der Daten Bewegung (Kopieren Aktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren sie wichtige Faktoren lernen.