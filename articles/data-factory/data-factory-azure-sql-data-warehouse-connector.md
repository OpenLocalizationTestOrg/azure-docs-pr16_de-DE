<properties 
    pageTitle="Verschieben von Daten zu/aus Azure SQL-Data Warehouse | Microsoft Azure" 
    description="Informationen Sie zum Verschieben von Daten aus/mit Azure Data Factory Data Warehouse mit SQL Azure" 
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

# <a name="move-data-to-and-from-azure-sql-data-warehouse-using-azure-data-factory"></a>Verschieben von Daten an und von Azure SQL-Data Warehouse Azure Data Factory verwenden

Dieser Artikel beschreibt, wie Sie kopieren Aktivität in Azure Data Factory zum Verschieben von Daten aus dem und in Azure SQL-Data Warehouse zu/aus einem anderen Datenspeicher verwenden können. 

Sie können angeben, ob Sie PolyBase beim Laden von Daten in Azure SQL-Data Warehouse verwenden möchten. Es wird empfohlen, dass Sie PolyBase verwenden, um optimale Leistung zu erzielen, wenn Sie Daten in Azure SQL-Data Warehouse zu laden. Im Abschnitt [Verwenden PolyBase zum Laden von Daten in Azure SQL-Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) enthält Details. 


## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit, eine Verkaufspipeline zu erstellen, die Daten zu/aus Azure SQL-Data Warehouse kopiert besteht darin, den Assistenten zum Kopieren von Daten verwenden. Finden Sie unter [Lernprogramm: Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) für eine schnelle Exemplarische Vorgehensweise zum Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von Daten. 

In den folgenden Beispielen bieten Stichprobe JSON-Definitionen, mit denen Sie eine Verkaufspipeline mithilfe von [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Wird gezeigt, wie Daten an und von Azure SQL-Data Warehouse und Azure BLOB-Speicher kopieren. Daten kann jedoch kopierten **direkt** aus einer der Quellen an die senken angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores) die Aktivität kopieren in Azure Data Factory verwenden.


> [AZURE.NOTE] 
> Eine Übersicht über den Azure Data Factory-Dienst finden Sie unter [Einführung in Azure Data Factory](data-factory-introduction.md). 
> 
> In diesem Artikel finden Sie Beispiele für die JSON jedoch keine eine schrittweise Anleitung zum Erstellen einer Factory Daten. Finden Sie unter [Lernprogramm: Kopieren von Daten aus Azure Blob mit Azure SQL-Datenbank](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) eine schnelle Exemplarische Vorgehensweise mit einer schrittweisen Anleitung für die Aktivität kopieren in Azure Data Factory verwenden. 


## <a name="sample-copy-data-from-azure-sql-data-warehouse-to-azure-blob"></a>Beispiel: Kopieren von Daten aus Azure SQL-Data Warehouse in Azure Blob

Im Beispiel wird die folgenden Daten Factory Elemente definiert:

1. Eine verknüpfte Dienst vom Typ [AzureSqlDW](#azure-sql-data-warehouse-linked-service-properties).
2. Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureSqlDWTable](#azure-sql-data-warehouse-dataset-type-properties). 
4. Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [SqlDWSource](#azure-sql-data-warehouse-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Im Beispiel Kopiervorgang Time-Serie (stündlich, täglich usw.) aus einer Tabelle in Azure SQL-Data Warehouse-Datenbank in ein Blob stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben.

**Azure SQL-Data Warehouse verknüpft Dienst an:**

    {
      "name": "AzureSqlDWLinkedService",
      "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

**Azure Blob-Speicher verknüpft Dienst an:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure SQL-Data Warehouse Eingabemethoden Dataset:**

Das Beispiel wird vorausgesetzt, Sie eine Tabelle erstellt haben "MyTable" in Azure SQL-Data Warehouse und eine Spalte mit der Bezeichnung "Timestampcolumn" für die Reihe Zeitdaten enthält.
 
Festlegen von "externe": "true" werden dem Daten Factory-Dienst informiert, dass das Dataset externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.

    {
      "name": "AzureSqlDWInput",
      "properties": {
        "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
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
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


**Pipeline mit Aktivität kopieren:**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass die Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **SqlDWSource** festgelegt ist, und Typ der **Empfänger** auf **BlobSink**festgelegt ist. Die SQL-Abfrage für die Eigenschaft **SqlReaderQuery** angegeben, werden die Daten in der letzten Stunde zu kopierenden markiert.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "AzureSQLDWtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureSqlDWInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "SqlDWSource",
                "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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

> [AZURE.NOTE] Im Beispiel wird die **SqlReaderQuery** für die SqlDWSource angegeben. Die Aktivität kopieren führt diese Abfrage für die Azure SQL-Data Warehouse Datenquelle verfügen, um die Daten zu erhalten.
>  
> Alternativ können Sie eine gespeicherte Prozedur angeben, nach Angabe von **SqlReaderStoredProcedureName** und **StoredProcedureParameters** (wenn die gespeicherte Prozedur Parameter akzeptiert).
>  
> Wenn Sie entweder SqlReaderQuery oder SqlReaderStoredProcedureName nicht angeben, werden die Spalten im Abschnitt Struktur des Datasets JSON definiert verwendet, zum Erstellen von Abfragen (select column1, column2 aus Mytable), um für SQL Azure Data Warehouse auszuführen. Wenn die Definition der Dataset nicht die Struktur verfügt, werden alle Spalten aus der Tabelle ausgewählt.

## <a name="sample-copy-data-from-azure-blob-to-azure-sql-data-warehouse"></a>Beispiel: Kopieren Sie Daten aus Azure Blob in Azure SQL-Data Warehouse

Im Beispiel wird die folgenden Daten Factory Elemente definiert:

1.  Eine verknüpfte Dienst vom Typ [AzureSqlDW](#azure-sql-data-warehouse-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureSqlDWTable](#azure-sql-data-warehouse-dataset-type-properties).
4.  Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) und [SqlDWSink](#azure-sql-data-warehouse-copy-activity-type-properties)verwendet.


Die Stichprobe Kopien Time-Serie zu einer Tabelle in Azure SQL-Data Warehouse Daten (stündlich, täglich usw.) aus Azure blob-Datenbank stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben. 

**Azure SQL-Data Warehouse verknüpft Dienst an:**

    {
      "name": "AzureSqlDWLinkedService",
      "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

**Azure Blob-Speicher verknüpft Dienst an:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure Blob Eingabe-Dataset:**

Daten werden übernommen aus einer neuen Blob stündlich (Häufigkeit: Stunde, Intervall: 1). Der Ordnername Pfad und den Dateinamen für die Blob werden dynamisch ausgewertet, basierend auf der Startzeit des Segments, die verarbeitet wird. Der Pfad des verwendet Jahr, Monat und Tagesanteil der Startzeit und Dateinamen der Stundenteil des der Startzeit. "externe": "true" Einstellung informiert dem Daten Factory-Dienst, dass diese Tabelle externe Daten Fabrik und nicht durch eine Aktivität in der Factory Daten erzeugt.

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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

**Azure SQL-Data Warehouse ausgeben Dataset:**

Im Beispiel werden die Daten in einer Tabelle mit dem Namen "MyTable" Azure SQL-Data Warehouse kopiert. Erstellen Sie die Tabelle in Azure SQL-Data Warehouse mit der gleichen Anzahl von Spalten wie erwartet die BLOB-CSV-Datei enthalten. Neue Zeilen werden stündlich zur Tabelle hinzugefügt. 

    {
      "name": "AzureSqlDWOutput",
      "properties": {
        "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Verkaufspipeline mit Aktivität kopieren**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass die Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **BlobSource** festgelegt ist, und Typ der **Empfänger** auf **SqlDWSink**festgelegt ist.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoSQLDW",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureSqlDWOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource",
                "blobColumnSeparators": ","
              },
              "sink": {
                "type": "SqlDWSink"
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

Eine exemplarische Vorgehensweise finden Sie unter [Laden von Daten mit Azure Data Factory](../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) in der Dokumentation Azure SQL-Data Warehouse.

## <a name="azure-sql-data-warehouse-linked-service-properties"></a>Azure SQL Data Warehouse verknüpft Diensteigenschaften

Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die speziell für verknüpfte Data Warehouse mit SQL Azure-Dienst an. 

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Die Eigenschaft muss auf festgelegt sein: **AzureSqlDW** | Ja
**connectionString** | Geben Sie die Verbindung zur Azure SQL-Data Warehouse Instanz für die Eigenschaft ConnectionString erforderlichen Informationen an. | Ja

> [AZURE.IMPORTANT] Konfigurieren von [Azure SQL-Datenbank-Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) und dem Datenbankserver [Azure Services auf den Server zugreifen](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure)dürfen. Wenn Sie Daten auf Azure SQL-Data Warehouse aus externen Azure einschließlich aus lokalen Datenquellen mit Daten Factory Gateway kopieren möchten, konfigurieren Sie darüber hinaus entsprechenden IP-Adressbereichs für den Computer, der die Daten in Azure SQL-Data Warehouse sendet. 

## <a name="azure-sql-data-warehouse-dataset-type-properties"></a>Azure SQL-Data Warehouse Typ Datensatzeigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung finden Sie im Artikel [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinie eines Datasets JSON ähneln für alle Dataset-Typen (Azure SQL Azure Blob, Azure Table usw..). 

Im Abschnitt TypeProperties unterscheidet sich für jede Art von Dataset und enthält Informationen über den Speicherort der Daten im Datenspeicher. Im Abschnitt **TypeProperties** für das Dataset vom Typ **AzureSqlDWTable** weist die folgenden Eigenschaften.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Tabellenname | Name der Tabelle in der Azure SQL-Data Warehouse-Datenbank, der auf der Dienst verknüpfte verweist. | Ja |

## <a name="azure-sql-data-warehouse-copy-activity-type-properties"></a>Azure SQL-Data Warehouse kopieren Aktivität Schrifteigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten verfügbar sind finden Sie unter Artikel [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinie stehen für alle Arten von Aktivitäten.

> [AZURE.NOTE]
Die Aktivität kopieren übernimmt nur eine Eingabe und die Ausgabe nur ein.

Im Abschnitt TypeProperties der Aktivität verfügbaren Eigenschaften variieren andererseits bei jeder Aktivität. Aktivitäten, kopieren variieren je nach den Typen von Datenquellen und senken.

### <a name="sqldwsource"></a>SqlDWSource

Wenn Quelle vom Typ **SqlDWSource**ist, sind die folgenden Eigenschaften im Abschnitt **TypeProperties** verfügbar:

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| sqlReaderQuery | Verwenden Sie die benutzerdefinierte Abfrage, um Daten zu lesen. | SQL-Abfragezeichenfolge. Beispiel: Wählen Sie * from MyTable. | Nein |
| sqlReaderStoredProcedureName | Namen der gespeicherten Prozedur, die Daten aus der Quelltabelle liest. | Namen der gespeicherten Prozedur. | Nein |
| storedProcedureParameters | Parameter für die gespeicherte Prozedur. | Name/Wert-Paare. Namen und die Groß-/Kleinschreibung von Parametern müssen den Namen und die Groß-/Kleinschreibung des Parameter der gespeicherten Prozedur übereinstimmen. | Nein |

Wenn die **SqlReaderQuery** für die SqlDWSource angegeben ist, führt die Aktivität kopieren diese Abfrage für die Azure SQL-Data Warehouse Datenquelle verfügen, um die Daten zu erhalten. 

Alternativ können Sie eine gespeicherte Prozedur angeben, nach Angabe von **SqlReaderStoredProcedureName** und **StoredProcedureParameters** (wenn die gespeicherte Prozedur Parameter akzeptiert). 

Wenn Sie entweder SqlReaderQuery oder SqlReaderStoredProcedureName nicht angeben, werden die Spalten im Abschnitt Struktur des Datasets JSON definiert verwendet, zum Erstellen von Abfragen für SQL Azure Data Warehouse ausführen. Beispiel: `select column1, column2 from mytable`. Wenn die Definition der Dataset nicht die Struktur verfügt, werden alle Spalten aus der Tabelle ausgewählt.

#### <a name="sqldwsource-example"></a>Beispiel für SqlDWSource

    "source": {
        "type": "SqlDWSource",
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
 

### <a name="sqldwsink"></a>SqlDWSink
**SqlDWSink** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| writeBatchSize | Daten in der SQL-Tabelle eingefügt werden soll, wenn die Puffergröße WriteBatchSize erreicht | Ganzzahl (Anzahl von Zeilen) | Nein (Standard: 10000) |
| writeBatchTimeout | Wartezeit für Stapel einfügen nach Abschluss des Vorgangs bevor es abläuft. | TimeSpan<br/><br/> Beispiel: "00: 30:00" (30 Minuten). | Nein | 
| sqlWriterCleanupScript | Geben Sie eine Abfrage für Kopieren Aktivität auszuführende so, dass die Daten eines bestimmten Segments bereinigt werden. Weitere Informationen finden Sie unter [Wiederholbarkeit Abschnitt](#repeatability-during-copy). | Eine Abfrage-Anweisung.  | Nein |
| allowPolyBase | Gibt an, ob Sie PolyBase (falls zutreffend) statt BULKINSERT Verfahren verwenden, um Daten in Azure SQL-Data Warehouse zu laden. <br/><br/>Zurzeit können Sie nur **Azure BLOB-** Dataset mit **Format** so **TextFormat** als Quelle Dataset eingestellt. <br/><br/>Siehe Abschnitt für Einschränkungen und Details zu [PolyBase verwenden, um Daten in Azure SQL-Data Warehouse zu laden](#use-polybase-to-load-data-into-azure-sql-data-warehouse) . | WAHR <br/>Falsch (Standard) | Nein |  
| polyBaseSettings | Eine Gruppe von Eigenschaften, die sein können angegeben, wenn die Eigenschaft **AllowPolybase** auf **true**festgelegt ist. | &nbsp; | Nein |  
| rejectValue | Gibt die Zahl oder einen Prozentsatz von Zeilen, die abgelehnt werden können, bevor die Abfrage schlägt fehl. <br/><br/>Weitere Informationen zu den PolyBases ablehnen Optionen im Abschnitt **Argumente** Themas [Externe Tabelle erstellen (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) . | 0 (Standard), 1, 2... | Nein |  
| rejectType | Gibt an, ob die Option RejectValue als literaler Wert oder als Prozentsatz angegeben ist. | Wert (Standardwert), Prozentsatz | Nein |   
| rejectSampleValue | Ermittelt die Anzahl der Zeilen, die abgerufen werden, bevor die PolyBase den Prozentsatz der abgelehnten Zeilen neu berechnet. | 1, 2... | Ja, wenn **RejectType** **Prozentsatz** ist |  
| useTypeDefault | Gibt an, wie in der fehlenden Werte in durch Trennzeichen getrennte Textdateien PolyBase Daten aus der Textdatei abruft behandelt.<br/><br/>Weitere Informationen zu dieser Eigenschaft aus dem Abschnitt Argumente in [Erstellen externer-DATEIFORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx). | True, False (Standard) | Nein | 


#### <a name="sqldwsink-example"></a>Beispiel für SqlDWSink


    "sink": {
        "type": "SqlDWSink",
        "writeBatchSize": 1000000,
        "writeBatchTimeout": "00:05:00"
    }

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Verwenden Sie zum Laden von Daten in Azure SQL-Data Warehouse PolyBase
Verwendung von **PolyBase** ist eine effiziente Möglichkeit große Datenmenge in Azure SQL-Data Warehouse mit hohen Durchsatz zu laden.  Sie können einen großen Gewinn in den Durchsatz mithilfe von PolyBase anstelle der standardmäßigen BULKINSERT Verfahren anzeigen.

Legen Sie die Eigenschaft **AllowPolyBase** auf **true** ein, wie im folgenden Beispiel für Azure Daten Factory PolyBase verwendet, Kopieren von Daten in Azure SQL-Data Warehouse dargestellt. Wenn Sie AllowPolyBase true festlegen, können Sie PolyBase bestimmte Eigenschaften unter Verwendung der **PolyBaseSettings** Eigenschaftsgruppe angeben. finden Sie im Abschnitt [SqlDWSink](#SqlDWSink) Details über gemeinsame Eigenschaften, die mit PolyBaseSettings verwendet werden können.   


    "sink": {
        "type": "SqlDWSink",
        "allowPolyBase": true,
        "polyBaseSettings":
        {
            "rejectType": "percentage",
            "rejectValue": 10.0,
            "rejectSampleValue": 100,
            "useTypeDefault": true 
        }

    }

### <a name="direct-copy-using-polybase"></a>Direkte Kopie PolyBase verwenden
Wenn die Quelldaten die in diesem Abschnitt beschriebenen Kriterien erfüllt, können Sie direkt zu PolyBase mit Data Warehouse mit SQL Azure aus der Quelle Datenspeicher kopieren. Andernfalls können Sie [mithilfe von kopieren bereitgestellt PolyBase](#staged-copy-using-polybase)verwenden.

Wenn Sie nicht die Anforderungen erfüllt sind, Azure Data Factory überprüft die Einstellungen, und greift automatisch auf die BULKINSERT Verfahren für das Verschieben von Daten zurück.

1.  **Verknüpfte Datenquelle Dienst** ist vom Typ: **Azure-Speicher** , und es ist nicht so konfiguriert, dass Authentifizierung SAS (Access-Signatur freigegeben) verwenden. Weitere Informationen finden Sie unter [Azure-Speicher verknüpft Dienst](data-factory-azure-blob-connector.md#azure-storage-linked-service) .  
2. **Eingabe-Dataset** ist vom Typ: **Azure Blob** und den Formattyp unter Typeigenschaften ist **OrcFormat** oder **TextFormat** mit den folgenden Konfigurationen:
    1. **RowDelimiter** muss **\n**. 
    2. **NullValue** festgelegt ist, auf **leere Zeichenfolge** (""). 
    3. **EncodingName** ist auf **Utf-8**festgelegt ist der **Standardwert, damit nicht es in einen anderen Wert festlegen** . 
    4. **EscapeChar** und **QuoteChar** werden nicht angegeben. 
    5. **Komprimierung** ist nicht **BZIP2**.
     
            "typeProperties": {
                "folderPath": "<blobpath>",
                "format": {
                    "type": "TextFormat",     
                    "columnDelimiter": "<any delimiter>", 
                    "rowDelimiter": "\n",       
                    "nullValue": "",           
                    "encodingName": "utf-8"    
                },
                "compression": {  
                    "type": "GZip",  
                    "level": "Optimal"  
                }  
            },
3.  Es gibt keine **SkipHeaderLineCount** Einstellung unter **BlobSource** für die Aktivität kopieren in der Verkaufspipeline. 
4.  Es gibt keine **SliceIdentifierColumnName** Einstellung unter **SqlDWSink** für die Aktivität kopieren in der Verkaufspipeline. (PolyBase wird sichergestellt, dass alle Daten aktualisiert oder in einer einzelnen Instanz wird nichts aktualisiert. Um **Wiederholbarkeit**zu erreichen, können Sie **SqlWriterCleanupScript**.
5.  Es gibt keine **ColumnMapping** verwendet wird, in der Kopie in zugeordneten Aktivität. 

### <a name="staged-copy-using-polybase"></a>Gestaffelter mit PolyBase kopieren
Wenn die Quelldaten nicht die im vorherigen Abschnitt eingeführt werden Kriterien entspricht, können Sie kopieren von Daten über eine Zwischenzeit staging Azure Blob-Speicher aktivieren. In diesem Fall führt Azure Data Factory Transformationen auf die Daten zu PolyBase Daten Format Anforderungen entsprechen, und verwenden Sie PolyBase zum Laden von Daten in SQL Data Warehouse. Details zum Kopieren von Daten über eine staging Azure Blob im Allgemeinen Funktionsweise finden Sie unter [Kopieren bereitgestellt](data-factory-copy-activity-performance.md#staged-copy) .

> [AZURE.IMPORTANT] Wenn Sie kopieren möchten, Daten aus einer Datenquelle auf Prem in Azure SQL-Data Warehouse mit PolyBase speichern und staging, installieren das 8 JRE (Java Runtime-Umgebung) auf Ihrem Gatewaycomputer, das verwendet wird, um die Quelldaten in Klein-oder transformieren. Ein 64-Bit-Gateway erfordert 64-Bit-JRE und ein 32-Bit-Gateway 32-Bit-JRE. Laden Sie die entsprechende Version von [Java-Downloads Speicherort](http://go.microsoft.com/fwlink/?LinkId=808605).

Um dieses Feature verwenden zu können, erstellen Sie eine [Azure-Speicher verknüpft Dienst](data-factory-azure-blob-connector.md#azure-storage-linked-service) , die mit dem Azure-Speicher-Konto mit den zwischenzeitlichen Blob-Speicher, und klicken Sie dann die Eigenschaften **EnableStaging** und **StagingSettings** für die Aktivität kopieren angeben verweist, wie im folgenden Code dargestellt:

    "activities":[  
    {
        "name": "Sample copy activity from SQL Server to SQL Data Warehouse via PolyBase",
        "type": "Copy",
        "inputs": [{ "name": "OnpremisesSQLServerInput" }],
        "outputs": [{ "name": "AzureSQLDWOutput" }],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlDwSink",
                "allowPolyBase": true
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": "MyStagingBlob"
            }
        }
    }
    ]


### <a name="best-practices-when-using-polybase"></a>Bewährte Methoden beim Verwenden von PolyBase

#### <a name="row-size-limitation"></a>Größenbeschränkungen Zeile
Zeilen mit mehr als 32 KB unterstützt Polybase nicht. Beim Laden der einer Tabelle mit Zeilen größer als 32 KB würde der folgende Fehler führen: 

    Type=System.Data.SqlClient.SqlException,Message=107093;Row size exceeds the defined Maximum DMS row size: [35328 bytes] is larger than the limit of [32768 bytes],Source=.Net SqlClient

Wenn Sie die Quelldaten mit Zeilen größer als 32 KB Größe haben, möchten Sie möglicherweise die Quelltabellen in mehreren kleinen Netzwerken vertikal teilen, in denen die größte Zeilengröße jeder von ihnen nicht die Beschränkung überschreiten. Kleineren Tabellen mit PolyBase geladen werden können und Azure SQL-Data Warehouse zusammen eingefügt.

#### <a name="tablename-in-azure-sql-data-warehouse"></a>Tabellenname in Azure SQL-Data Warehouse
Die folgende Tabelle enthält Beispiele zum Angeben der **Tabellenname** -Eigenschaft in Dataset JSON für verschiedene Kombinationen aus Schema und den Tabellennamen an.

| DB Schema | Tabellenname | Tabellenname JSON-Eigenschaft |
| --------- | -----------| ----------------------- | 
| dbo | MyTable | MyTable oder Dbo. MyTable oder [Dbo]. Beispiel: [myDatabase] |
| dbo1 | MyTable | dbo1. MyTable oder [dbo1]. Beispiel: [myDatabase] |
| dbo | My.Table | [My.Table] oder [Dbo]. [My.Table] |
| dbo1 | My.Table | [dbo1]. [My.Table] |

Wenn den folgenden Fehler angezeigt wird, könnte es ein Problem mit dem Wert, den Sie für die Eigenschaft Tabellenname angegeben haben. Finden Sie in der Tabelle für die ordnungsgemäße Werte für die Tabellenname JSON-Eigenschaft angeben.  

    Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider

#### <a name="columns-with-default-values"></a>Spalten mit Standardwerten
PolyBase-Funktion in Daten Factory akzeptiert derzeit nur die gleiche Anzahl von Spalten wie in der Zieltabelle. Angenommen Sie, Sie haben eine Tabelle mit vier Spalten und einer von ihnen mit einem Standardwert definiert ist. Die Eingabedaten sollten immer noch vier Spalten enthalten. Bereitstellen einer Spalte 3 Eingabe-Dataset ergibt eine Fehlermeldung angezeigt, die folgende Meldung angezeigt:

    All columns of the table must be specified in the INSERT BULK statement.

NULL-Wert ist eine spezielle Form des Standardwerts. Wenn die Spalte auf NULL festgelegt ist, die Eingabedaten (in Blob) für diese Spalte möglicherweise leere (kann nicht aus der Eingabe-Dataset fehlt werden). PolyBase fügt NULL für diese in SQL Azure Data Warehouse.  


[AZURE.INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)] 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-azure-sql-data-warehouse"></a>Geben Sie die Zuordnung für Azure SQL-Data Warehouse

Wie in den [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) Artikel erwähnt, führt kopieren Aktivität automatische Konvertieren des Datentyps von automatischen Konvertieren des Datentyps von Arten von Datenquellen zu ignorieren Typen mit dem folgenden Ansatz für-Schritt 2:

1. Konvertieren von Datentypen systemeigenen Quelle in .NET Typ
2. Konvertieren von .NET Typ in einer systemeigenen Empfänger Typ

Beim Verschieben von Daten zu und aus SQL Azure SQLServer, Sybase die folgenden Zuordnungen aus SQL-Typ in .NET Typ und umgekehrt verwendet werden.

Die Zuordnung ist gleich der [SQL Server Datentypen zuordnen für ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx).

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
