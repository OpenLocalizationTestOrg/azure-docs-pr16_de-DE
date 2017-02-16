<properties 
    pageTitle="Verschieben von Daten in/aus Azure Tabelle | Microsoft Azure" 
    description="Informationen Sie zum Verschieben von Daten zu/aus Azure Table Storage Azure Data Factory verwenden." 
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
    ms.date="09/13/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Verschieben von Daten an und von Azure-Tabelle mit Azure Data Factory

In diesem Artikel wird beschrieben, wie Sie die Aktivität kopieren in einer Factory Azure-Daten zum Verschieben von Daten zu/aus der Tabelle Azure aus dem und in einem anderen Datenspeicher verwenden können. In diesem Artikel basiert auf den [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) Artikel eine allgemeine Übersicht über das Verschieben von Daten und unterstützten Data Store Kombinationen mit Aktivität kopieren bietet.

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit, eine Verkaufspipeline zu erstellen, die Daten zu/aus Azure Table Storage kopiert besteht darin, den Assistenten zum Kopieren von Daten verwenden. Finden Sie unter [Lernprogramm: Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) für eine schnelle Exemplarische Vorgehensweise zum Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von Daten. 

In den folgenden Beispielen bieten Stichprobe JSON-Definitionen, mit denen Sie eine Verkaufspipeline mithilfe von [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Wird gezeigt, wie Daten an und von Azure Table Storage und Azure Blob-Datenbank zu kopieren. Daten kann jedoch kopierten **direkt** aus einer der Quellen an die senken angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores) die Aktivität kopieren in Azure Data Factory verwenden.

## <a name="sample-copy-data-from-azure-table-to-azure-blob"></a>Beispiel: Kopieren Sie Daten aus Azure-Tabelle in Azure Blob

Im folgende Beispiel gezeigt:

1.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (für sowohl Tabelle & Blob verwendet).
2.  Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [Azure Table](#azure-table-dataset-type-properties).
3.  Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
3.  Der [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [AzureTableSource](#azure-table-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet. 

Im Beispiel kopiert Daten, die Standardpartition in einer Tabelle Azure Blob stündlich gehören. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben.

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

**Azure Tabelle Eingabe-Dataset:**

Im Beispiel wird davon ausgegangen, dass Sie eine Tabelle "MyTable" in Azure-Tabelle erstellt haben.
 
Festlegen von "externe": "true" werden dem Daten Factory-Dienst informiert, dass das Dataset externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.

    {
      "name": "AzureTableInput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
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

**Pipeline mit der Aktivität kopieren:**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass die Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **AzureTableSource** festgelegt ist, und Typ der **Empfänger** auf **BlobSink**festgelegt ist. Die SQL-Abfrage mit **AzureTableSourceQuery** Eigenschaft angegebenen wählt die Daten aus dem standardmäßigen-Abschnitt stündlich um zu kopieren.

    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2014-06-01T18:00:00",
            "end":"2014-06-01T19:00:00",
            "description":"pipeline for copy activity",
            "activities":[  
                {
                    "name": "AzureTabletoBlob",
                    "description": "copy activity",
                    "type": "Copy",
                    "inputs": [
                        {
                            "name": "AzureTableInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "AzureTableSource",
                            "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
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

## <a name="sample-copy-data-from-azure-blob-to-azure-table"></a>Beispiel: Kopieren Sie Daten aus Azure Blob Azure-Tabelle

Im folgende Beispiel gezeigt:

1.  Eine verknüpfte Dienst des Typs [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (für sowohl Tabelle & Blob verwendet)
3.  Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [Azure Table](#azure-table-dataset-type-properties). 
4.  Der [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) und [AzureTableSink](#azure-table-copy-activity-type-properties)verwendet. 


Die Stichprobe Kopien Time-Serie Daten aus einer Azure in einer Azure BLOB-Tabelle stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben.

**Azure-Speicher (für Azure Tabelle & Blob) verknüpft Dienst:**

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

Daten werden übernommen aus einer neuen Blob stündlich (Häufigkeit: Stunde, Intervall: 1). Der Ordnername Pfad und den Dateinamen für die Blob werden dynamisch ausgewertet, basierend auf der Startzeit des Segments, die verarbeitet wird. Der Pfad des verwendet Jahr, Monat und Tagesanteil der Startzeit und Dateinamen der Stundenteil des der Startzeit. "externe": "true" Einstellung informiert dem Daten Factory-Dienst, dass das Dataset externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.
    
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

**Azure Table ausgeben Dataset:**

Im Beispiel werden Daten in einer Tabelle "MyTable" in Azure Tabelle kopiert. Erstellen Sie eine Azure Table mit der gleichen Anzahl von Spalten wie erwartet die BLOB-CSV-Datei enthalten. Neue Zeilen werden stündlich zur Tabelle hinzugefügt. 

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Pipeline mit der Aktivität kopieren:**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass die Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **BlobSource** festgelegt ist, und Typ der **Empfänger** auf **AzureTableSink**festgelegt ist. 


    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureTableOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "AzureTableSink",
                "writeBatchSize": 100,
                "writeBatchTimeout": "01:00:00"
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
Es gibt zwei Arten von verknüpften Diensten, die Sie verwenden können, um-Azure Blob-Speicher mit einer Factory Azure-Daten zu verknüpfen. Sie sind: **AzureStorage** verknüpft, Dienste und **AzureStorageSas** verknüpft. Der Azure verknüpft Speicherdienst stellt die Factory Daten mit globaler Zugriff auf den Azure-Speicher. Während der Azure-Speicher SAS (Access-Signatur freigegeben) verknüpft bietet die Factory Daten mit Access beschränkt/Uhrzeit-Grenze um den Azure-Speicher. Es gibt keine anderen Unterschiede zwischen diese beiden verknüpften Dienste. Wählen Sie aus der verknüpften Dienst, der Ihren Bedürfnissen entspricht. Die folgenden Abschnitte enthalten weitere Details auf diese beiden verknüpften Dienste.

[AZURE.INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="azure-table-dataset-type-properties"></a>Azure Tabelle Typ Datensatzeigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung finden Sie im Artikel [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinie eines Datasets JSON ähneln für alle Dataset-Typen (Azure SQL Azure Blob, Azure Table usw..).

Im Abschnitt TypeProperties unterscheidet sich für jede Art von Dataset und enthält Informationen über den Speicherort der Daten im Datenspeicher. Im Abschnitt **TypeProperties** für das Dataset vom Typ **Azure Table** weist die folgenden Eigenschaften.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Tabellenname | Name der Tabelle in der Tabelle Azure-Datenbank-Instanz, der auf Verknüpfte Dienst verweist. | Ja. Wenn ein Tabellenname ohne eine AzureTableSourceQuery angegeben wird, werden alle Datensätze aus der Tabelle in das Ziel kopiert. Wenn eine AzureTableSourceQuery ebenfalls angegeben ist, werden Datensätze aus der Tabelle, die die Abfrage erfüllt an das Ziel kopiert. |

### <a name="schema-by-data-factory"></a>Schema von Daten Factory
Für Schema frei Datenspeicher wie z. B. Azure Table leitet der Daten Factory-Dienst das Schema in eine der folgenden Methoden:

1.  Wenn Sie die Struktur der Daten mithilfe der **Struktur** -Eigenschaft in der Definition Dataset angeben, berücksichtigt der Daten Factory-Dienst diese Struktur wie das Schema nach. In diesem Fall wird eine Zeile keinen Wert für eine Spalte enthält, ein Nullwert dafür bereitgestellt.
2. Wenn Sie die Struktur der Daten nicht mithilfe der **Struktur** -Eigenschaft in der Definition Dataset angeben, leitet Daten Factory das Schema mithilfe der ersten Zeile in den Daten ein. In diesem Fall werden die erste Zeile das vollständige Schema nicht enthält, einige Spalten im Ergebnis Kopiervorgang verpasst.

Daher ist die beste Methode Schema frei Datenquellen, die Struktur der Daten, die mithilfe der Eigenschaft **Struktur** angeben.

## <a name="azure-table-copy-activity-type-properties"></a>Azure Tabelle kopieren Aktivität Schrifteigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten verfügbar sind finden Sie unter Artikel [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Datasets und Richtlinien zur Verfügung stehen für alle Arten von Aktivitäten. 

Im Abschnitt TypeProperties der Aktivität verfügbaren Eigenschaften variieren andererseits bei jeder Aktivität. Aktivitäten, kopieren variieren je nach den Typen von Datenquellen und senken.

**AzureTableSource** unterstützt die folgenden Eigenschaften im Abschnitt TypeProperties an:

Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich
-------- | ----------- | -------------- | -------- 
azureTableSourceQuery | Verwenden Sie die benutzerdefinierte Abfrage, um Daten zu lesen. | Azure Tabelle Abfragezeichenfolge. Beispiele für den im nächsten Abschnitt. | Nein. Wenn ein Tabellenname ohne eine AzureTableSourceQuery angegeben wird, werden alle Datensätze aus der Tabelle in das Ziel kopiert. Wenn eine AzureTableSourceQuery ebenfalls angegeben ist, werden Datensätze aus der Tabelle, die die Abfrage erfüllt an das Ziel kopiert.
azureTableSourceIgnoreTableNotFound | Geben Sie an, ob schluckt Ausnahme der Tabelle nicht vorhanden. | WAHR<br/>FALSCH | Nein |

### <a name="azuretablesourcequery-examples"></a>Beispiele für azureTableSourceQuery

Wenn Azure Tabellenspalte Zeichenfolgentyp ist: 

    azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"

Wenn Azure-Tabellenspalte vom Typ "DateTime" ist: 

    "azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"


**AzureTableSink** unterstützt die folgenden Eigenschaften im Abschnitt TypeProperties an:


Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich  
-------- | ----------- | -------------- | -------- 
azureTableDefaultPartitionKeyValue | Partition Key Standardwert, die durch den Empfänger verwendet werden kann. | Ein Zeichenfolgenwert. | Nein 
azureTablePartitionKeyName | Geben Sie an der Spalte, deren Werte als Partition Schlüssel verwendet werden. Wenn nicht angegeben, wird als der Partitionsschlüssel AzureTableDefaultPartitionKeyValue verwendet. | Ein Spaltenname. | Nein |
azureTableRowKeyName | Geben Sie an der Spalte, deren Spaltenwerte als Zeilenschlüssel verwendet werden. Wenn nicht angegeben, verwenden Sie eine GUID für jede Zeile an. | Ein Spaltenname. | Nein  
azureTableInsertType | Der Modus zum Einfügen von Daten in Azure Table.<br/><br/>Diese Eigenschaft steuert, ob die vorhandene Zeilen in der Ausgabetabelle mit übereinstimmenden Partition und Zeile Schlüsseln deren Werte ersetzt oder zusammengeführt haben. <br/><br/>Weitere Informationen zur Funktionsweise von diese Einstellungen (Zusammenführen und ersetzen), finden Sie unter [Einfügen oder Zusammenführen juristische](https://msdn.microsoft.com/library/azure/hh452241.aspx) und [Einfügen oder juristische ersetzen](https://msdn.microsoft.com/library/azure/hh452242.aspx) . <br/><br> Diese Einstellung gilt Ebene der Zeile der Tabellenebene, und weder Option löscht Zeilen in der Ausgabetabelle, die nicht in der Eingabe vorhanden sind. | Zusammenführen (Standard)<br/>Ersetzen | Nein 
writeBatchSize | Fügt Daten in der Tabelle Azure an, wenn die WriteBatchSize oder WriteBatchTimeout erreicht wird. | Ganzzahl (Anzahl von Zeilen)| Nein (Standard: 10000) 
writeBatchTimeout | Daten in der Azure Table eingefügt werden soll, wenn die WriteBatchSize oder WriteBatchTimeout erreicht wird | TimeSpan<br/><br/>Beispiel: "00: 20:00" (20 Minuten) | Nein (Standard auf Speicher Client Standardtimeout Wert 90 Sekunden)

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Ordnen Sie eine Quellspalte einer Zielspalte den Übersetzer JSON-Eigenschaft verwenden, bevor Sie die Zielspalte als der AzureTablePartitionKeyName verwenden können.

Im folgenden Beispiel wird die Quellspalte DivisionID in der Zielspalte zugeordnet: DivisionID.  

    "translator": {
        "type": "TabularTranslator",
        "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
    } 

Die DivisionID ist als der Partitionsschlüssel angegeben haben. 

    "sink": {
        "type": "AzureTableSink",
        "azureTablePartitionKeyName": "DivisionID",
        "writeBatchSize": 100,
        "writeBatchTimeout": "01:00:00"
    }


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-azure-table"></a>Für Azure Table-Zuordnung

Wie in den [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) Artikel erwähnt, führt Kopieren von Aktivität automatische Konvertieren des Datentyps von Arten von Datenquellen zu Typen mit den folgenden zwei Ansatz ignorieren.

1. Konvertieren von Datentypen systemeigenen Quelle in .NET Typ
2. Konvertieren von .NET Typ in einer systemeigenen Empfänger Typ

Beim Verschieben von Daten zu und Azure-Tabelle werden die folgenden [Zuordnungen von Tabelle Azure Service definiert](https://msdn.microsoft.com/library/azure/dd179338.aspx) von Azure Tabelle OData-Typen in .NET Typ und umgekehrt verwendet. 

| OData-Datentyp | .NET Typ | Details |
| --------------- | --------- | ------- |
| Edm.Binary | Byte] | Ein Array von Bytes bis zu 64 KB sein. |
| Edm.Boolean | bool | Der boolesche Wert. |
| Edm.DateTime | "DateTime" | Ein 64-Bit-Wert als Coordinated Universal Time (UTC). Der unterstützte DateTime-Bereich beginnt, von 00:00 Uhr, Januar 1, 1601 z. zurück. (FRÜHESTE), UTC. Der Bereich endet am 31. Dezember 9999. |
| Edm.Double | Doppelte | Eine 64-Bit-Gleitkommawert. |
| Edm.Guid | GUID | Eine 128-Bit-globally unique Identifier. |
| Edm.Int32 | Int32 oder int | Eine 32-Bit-Ganzzahl. |
| Int64 | Int64 oder long | Eine 64-Bit-Ganzzahl. |
| Edm.String | Zeichenfolge | Ein Wert UTF-16-codierte. Zeichenfolgenwerte können bis zu 64 KB sein. |

### <a name="type-conversion-sample"></a>Typ Konvertierung Stichprobe

Im folgende Beispiel wird zum Kopieren von Daten aus einer Azure Blob Azure-Tabelle mit Konvertieren des Datentyps. 

Nehmen Sie an das Dataset Blob im CSV-Format ist und umfasst drei Spalten. Eine von ihnen ist eine Datetime-Spalte mit einem benutzerdefinierten Datetime-Format mit abgekürzten Französisch Namen für den Tag der Woche aus. 

Definieren des Blob-Quelle Datasets wie folgt zusammen mit Definitionen für die Spalten an.
    
    {
        "name": " AzureBlobInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1,
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

Die Zuordnung von Azure Tabelle OData-Typ für .NET Typ ergeben, würden Sie die Tabelle in Azure-Tabelle mit dem folgenden Schema definieren. 

**Azure Table-Schema:**

Spaltenname | Typ
----------- | --------
Benutzer-ID | Int64
Namen | Edm.String 
LastLoginDate | Edm.DateTime

Als Nächstes definieren Sie wie folgt das Dataset Azure-Tabelle. Sie müssen nicht mit der Information "Struktur" im Abschnitt angeben, da die Informationen im zugrunde liegenden Datenspeicher bereits angegeben ist.

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

In diesem Fall Daten Factory automatisch Konvertierungen einschließlich Datetime-Feld mit dem benutzerdefinierten Datetime-Format verwenden die Kultur "Französisch-Französisch" beim Verschieben von Daten von Blob Azure Tabelle eingeben.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Leistung und optimieren  
Um wichtige Faktoren die Leistung Einfluss der Daten Bewegung (Kopieren Aktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren sie wissen möchten, finden Sie unter [Kopieren Aktivität Leistung und Leitfaden optimieren](data-factory-copy-activity-performance.md).







