<properties
    pageTitle="Verschieben von Daten zu und aus einem Dateisystem | Microsoft Azure"
    description="Erfahren Sie, wie Daten mithilfe von Azure Data Factory an und von einem lokalen Dateisystem verschieben."
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
    ms.date="09/01/2016"
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-an-on-premises-file-system-by-using-azure-data-factory"></a>Verschieben von Daten mithilfe von Azure Data Factory an und von einem lokalen Dateisystem

Dieser Artikel beschreibt, wie Sie zum Verschieben von Daten an und von einem lokalen Dateisystem Azure Daten Factory kopieren Aktivität verwenden können. Finden Sie unter [unterstützte Quellen und Empfängern](data-factory-data-movement-activities.md#supported-data-stores) eine Liste der Datenspeicher, die als Quellen oder senken mit dem lokalen Dateisystem verwendet werden können. In diesem Artikel wird im Artikel [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) , die eine allgemeine Übersicht über das Verschieben von Daten mit Aktivität kopieren und unterstützten Data Store Kombinationen bietet erstellt.

Daten Factory unterstützt das Herstellen einer Verbindung zu und von einem lokalen Dateisystem über Datenverwaltungsgateway. Informationen zu Datenverwaltungsgateway und für eine schrittweise Anleitung zum Einrichten des Gateways finden Sie unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md).

> [AZURE.NOTE]
> Abgesehen von Datenverwaltungsgateway müssen keine weiteren binären Dateien installiert sein, um an und von einem lokalen Dateisystem kommunizieren.
>
> Tipps zur Behebung von Problemen Verbindung, Gateway-bezogene finden Sie unter [Behandeln von Gateway Probleme](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) .

## <a name="linux-file-share"></a>Linux Dateifreigabe

Führen Sie die folgenden beiden Schritte aus, um eine Dateifreigabe Linux mit dem Dateiserver verknüpft-Dienst verwenden:

- Installieren Sie auf dem Server Linux [Samba](https://www.samba.org/) .
- Installieren Sie und konfigurieren Sie des Datenverwaltungsgateways auf einem WindowsServer. Installieren von Datenverwaltungsgateway auf einem Server Linux wird nicht unterstützt.

## <a name="copy-wizard"></a>Assistent zum Kopieren von
Die einfachste Möglichkeit, eine Verkaufspipeline zu erstellen, die Daten an und von einem lokalen Dateisystem kopiert besteht darin, verwenden den Assistenten zum Kopieren. Eine Kurzübersicht Exemplarische Vorgehensweise, finden Sie unter [Lernprogramm: Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md).

In den folgenden Beispielen bieten Stichprobe JSON-Definitionen, die Sie verwenden können, um eine Verkaufspipeline mithilfe der [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)zu erstellen. Wird gezeigt, wie Daten an und von einem lokalen Dateisystem und Azure Blob-Speicher kopieren. Allerdings können Sie Daten *direkt* aus allen Quellen an die senken mithilfe von kopieren Aktivität in Azure Data Factory [unterstützten Datenquellen und senken](data-factory-data-movement-activities.md#supported-data-stores) aufgeführt kopieren.


## <a name="sample-copy-data-from-an-on-premises-file-system-to-azure-blob-storage"></a>Beispiel: Kopieren von Daten aus einer lokalen Dateisystem in Azure Blob-Speicher

Dieses Beispiel zeigt, wie Sie Daten aus einer lokalen Dateisystem in Azure Blob-Speicher zu kopieren.

Im Beispiel weist die folgenden Daten Factory Elemente:

- Eine verknüpfte Dienst vom Typ [OnPremisesFileServer](data-factory-onprem-file-system-connector.md#onpremisesfileserver-linked-service-properties).
- Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [Dateifreigabe](data-factory-onprem-file-system-connector.md#on-premises-file-system-dataset-type-properties).
- Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Der [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [FileSystemSource](data-factory-onprem-file-system-connector.md#file-share-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Im folgende Beispiel kopiert Time-Serie Daten aus einer lokalen Dateisystem in Azure Blob-Speicher fest stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendet werden, werden in den Abschnitten nach den Beispielen beschrieben.

Als ersten Schritt richten Sie gemäß den Anweisungen im [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md)Datenverwaltungsgateway.

**Lokale Dateiserver verknüpft Dienst:**

    {
      "Name": "OnPremisesFileServerLinkedService",
      "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
          "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
          "userid": "Admin",
          "password": "123456",
          "gatewayName": "mygateway"
        }
      }
    }

Es empfiehlt sich stattdessen die Eigenschaft **EncryptedCredential** mit die Eigenschaften, die **Benutzer-ID** und **Ihr Kennwort ein** . Details zu diesen Dienst verknüpften finden Sie unter [Dateiserver verknüpft Dienst](#onpremisesfileserver-linked-service-properties) .

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

**Lokale Datei System Eingabe-Dataset:**

Daten werden von einer neuen Datei stündlich übernommen. Die Eigenschaften Ordnerpfad und Dateiname werden basierend auf der Startzeit für das Segment des bestimmt.  

Festlegen von `"external": "true"` Daten Factory informiert wurden, dass das Dataset externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.

    {
      "name": "OnpremisesFileSystemInput",
      "properties": {
        "type": " FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
        "typeProperties": {
          "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
          ]
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

**Azure Blob-Speicher ausgeben Dataset:**

Jede Stunde Daten in einer neuen Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Pfad des Ordners für das Blob wird dynamisch ausgewertet basierend auf der Startzeit des Segments, die verarbeitet wird. Der Pfad des verwendet Jahr, Monat, Tag und Stunde Teile der Startzeit.

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kopieren Sie die Aktivität:**

Der Verkaufspipeline enthält eine Kopie Aktivität, die die Eingabe- und Datasets Verwendung konfiguriert ist und stündlich ausführen geplant ist. In der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **FileSystemSource**festgelegt ist und Typ der **Empfänger** auf **BlobSink**festgelegt ist.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-06-01T18:00:00",
        "end":"2015-06-01T19:00:00",
        "description":"Pipeline for copy activity",
        "activities":[  
          {
            "name": "OnpremisesFileSystemtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "OnpremisesFileSystemInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "FileSystemSource"
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

## <a name="sample-copy-data-from-azure-sql-database-to-an-on-premises-file-system"></a>Beispiel: Kopieren Sie Daten aus Azure SQL-Datenbank auf einem lokalen Dateisystem

Im folgende Beispiel gezeigt:

- Eine verknüpfte Dienst vom Typ AzureSqlDatabase.
- Eine verknüpfte Dienst vom Typ OnPremisesFileServer.
- Eine Eingabe-Dataset vom Typ AzureSqlTable.
- Ein Dataset Ausgabe vom Typ Dateifreigabe.
- Eine Verkaufspipeline mit einer Aktivität kopieren, die SQLSource aus und FileSystemSink verwendet werden soll.

Im Beispiel kopiert Time-Serie Daten aus einer SQL Azure-Tabelle in einer lokalen Dateisystem fest stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendet werden, sind nach der Beispiele in Abschnitten beschrieben.

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

**Lokale Dateiserver verknüpft Dienst:**

    {
      "Name": "OnPremisesFileServerLinkedService",
      "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
          "host": "\\\\Contosogame-Asia.<region>.corp.<company>.com",
          "userid": "Admin",
          "password": "123456",
          "gatewayName": "mygateway"
        }
      }
    }

Es empfiehlt sich, mit der **EncryptedCredential** -Eigenschaft statt mit den Eigenschaften, die **Benutzer-ID** und **Ihr Kennwort ein** . Details zu diesen Dienst verknüpften finden Sie unter [File System verknüpft Dienst](#onpremisesfileserver-linked-service-properties) .

**Azure SQL Eingabe-Dataset:**

Im Beispiel wird davon ausgegangen, dass Sie eine Tabelle "MyTable", in SQL Azure erstellt haben und eine Spalte mit der Bezeichnung "Timestampcolumn" für die Uhrzeit-Serie Daten enthält.

Festlegen von ``“external”: ”true”`` Daten Factory informiert wurden, dass das Dataset externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.

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

**Lokale Datei System Ausgabe Dataset:**

Daten werden in einer neuen Datei stündlich kopiert. Den Ordnerpfad und den Dateinamen für die Blob werden basierend auf der Startzeit für das Segment des bestimmt.

    {
      "name": "OnpremisesFileSystemOutput",
      "properties": {
        "type": "FileShare",
        "linkedServiceName": " OnPremisesFileServerLinkedService ",
        "typeProperties": {
          "folderPath": "mysharedfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
          ]
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

**Mit einer Kopie Aktivität Pipeline:**

Der Verkaufspipeline enthält eine Kopie Aktivität, die die Eingabe- und Datasets Verwendung konfiguriert ist und stündlich ausführen geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **SQLSource aus**festgelegt ist, und der Typ der **Empfänger** auf **FileSystemSink**festgelegt ist. Die SQL-Abfrage, die für die Eigenschaft **SqlReaderQuery** angegeben ist, werden die Daten in der letzten Stunde zu kopierenden markiert.


    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-06-01T18:00:00",
        "end":"2015-06-01T20:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "AzureSQLtoOnPremisesFile",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureSQLInput"
              }
            ],
            "outputs": [
              {
                "name": "OnpremisesFileSystemOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd}\\'', WindowStart, WindowEnd)"
              },
              "sink": {
                "type": "FileSystemSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 3,
              "timeout": "01:00:00"
            }
          }
         ]
       }
    }

## <a name="on-premises-file-server-linked-service-properties"></a>Eigenschaften der lokalen Dateiserver verknüpft-Dienst

Sie können einem lokalen Dateisystem eine Fabrik Azure-Daten mit dem Dienst auf lokale Dateiserver verknüpft verknüpfen. Die folgende Tabelle enthält eine Beschreibung der JSON-Elemente, die speziell für den Dienst auf lokale Dateiserver verknüpft sind.

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Stellen Sie sicher, dass die Eigenschaft **OnPremisesFileServer**festgelegt ist. | Ja
Host | Gibt den Stammpfad des Ordners, die Sie kopieren möchten. Verwenden Sie das Escapezeichen ' \ ' für Sonderzeichen in der Zeichenfolge. Beispiele finden Sie in der [Stichprobe verknüpft Dienst und Dataset Definitionen](#sample-linked-service-and-dataset-definitions) . | Ja
Benutzer-ID | Geben Sie die ID des Benutzers, der auf dem Server zugreifen können. | Nein (falls EncryptedCredential gewünscht)
Kennwort | Geben Sie das Kennwort für den Benutzer (Benutzer-ID) an. | Nein (falls EncryptedCredential gewünscht
encryptedCredential | Geben Sie die verschlüsselten Anmeldeinformationen, die Sie aufrufen können, indem Sie das Cmdlet "New-AzureRmDataFactoryEncryptValue" ausführen. | Nein (Wenn Sie festlegen, um anzugeben, Benutzer-ID und das Kennwort im nur-Text)
gatewayName | Gibt den Namen des Gateways, die Daten Factory für die Verbindung mit dem lokalen Dateiserver verwendet werden sollen. | Ja

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für eine lokale Dateisystem-Datenquelle finden Sie unter [Festlegen der Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

### <a name="sample-linked-service-and-dataset-definitions"></a>Beispiel für verknüpfte Dienst und Dataset Definitionen
Szenario | In der Definition verknüpfte Diensts hosten | Ordnerpfad in Dataset-definition
-------- | --------------------------------- | --------------------- |
Lokalen Ordner auf dem Computer Datenverwaltungsgateway: <br/><br/>Beispiele: D:\\ \* oder D:\folder\subfolder\\* | D:\\ \\ (für Daten Management Gateway 2.0 und höher) <br/><br/> Localhost (für frühere Versionen als Daten Management Gateway 2.0) | .\\\\ oder einen Ordner\\\\Unterordner (für Daten Management Gateway 2.0 und höher) <br/><br/>D:\\ \\ oder D:\\\\Ordner\\\\Unterordner (für Gateway-Version unter 2.0)
Remote freigegebenen Ordner: <br/><br/>Beispiele: \\ \\MeinServer\\freigeben\\ \* oder \\ \\MeinServer\\freigeben\\Ordner\\Unterordner\\* | \\\\\\\\MeinServer\\\\freigeben | .\\\\ oder einen Ordner\\\\Unterordner


**So suchen Sie die Version eines Gateways**

1. Starten Sie [Daten Management Gateway-Konfigurations-Manager](data-factory-data-management-gateway.md#data-management-gateway-configuration-manager) auf Ihrem Computer.
2. Wechseln Sie zur Registerkarte **helfen** .

> [AZURE.NOTE] Es empfiehlt sich, die Sie ein [upgrade Ihrer Gateway zu Daten Management Gateway 2.0 oder höher](data-factory-data-management-gateway.md#update-data-management-gateway) auf die neuesten Features und Updates nutzen.

**Beispiel: Verwenden von Benutzername und Kennwort im nur-text**

    {
      "Name": "OnPremisesFileServerLinkedService",
      "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
          "host": "\\\\Contosogame-Asia",
          "userid": "Admin",
          "password": "123456",
          "gatewayName": "mygateway"
        }
      }
    }

**Beispiel: Verwenden von encryptedcredential**

    {
      "Name": " OnPremisesFileServerLinkedService ",
      "properties": {
        "type": "OnPremisesFileServer",
        "typeProperties": {
          "host": "D:\\",
          "encryptedCredential": "WFuIGlzIGRpc3Rpbmd1aXNoZWQsIG5vdCBvbmx5IGJ5xxxxxxxxxxxxxxxxx",
          "gatewayName": "mygateway"
        }
      }
    }

## <a name="on-premises-file-system-dataset-type-properties"></a>Lokale Datei Dataset Typ Systemeigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets verfügbar sind, finden Sie unter [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie Struktur, Verfügbarkeit und Richtlinie eines Datasets JSON ähneln für alle Dataset-Typen.

Im Abschnitt TypeProperties unterscheidet sich für jede Art von Dataset. Erläutert, wie z. B. den Speicherort und das Format der Daten in der Datenquelle. Im Abschnitt TypeProperties für das Dataset vom Typ **Dateifreigabe** weist die folgenden Eigenschaften:

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Ordnerpfad | Gibt den untergeordneten Pfad zu dem Ordner an. Verwenden Sie das Escapezeichen ' \' für Sonderzeichen in der Zeichenfolge. Beispiele finden Sie in der [Stichprobe verknüpft Dienst und Dataset Definitionen](#sample-linked-service-and-dataset-definitions) .<br/><br/>Sie können diese Eigenschaft mit **PartitionBy** Ordnerpfade auf Grundlage Segment haben kombinieren Anfangs-/Ende-Datum / Uhrzeit. | Ja
Dateiname | Geben Sie den Namen der Datei in den **Ordnerpfad** , wenn Sie die Tabelle verweisen auf eine bestimmte Datei in den Ordner soll. Wenn Sie einen beliebigen Wert für diese Eigenschaft nicht angeben, verweist die Tabelle auf alle Dateien in den Ordner.<br/><br/>Wenn FileName nicht für ein Dataset Ausgabe angegeben ist, wird der Name der generierten Datei in folgendem Format: <br/><br/>`Data.<Guid>.txt`(Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt) | Nein
partitionedBy | Sie können PartitionedBy verwenden, um einen dynamischen Ordnerpfad/Dateinamen für die Uhrzeit-Serie Daten anzugeben. Ein Beispiel ist für jede Stunde Daten parametrisierte Ordnerpfad. | Nein
Format | Die folgenden Arten von Format werden unterstützt: **TextFormat**, **AvroFormat**, **JsonFormat**, **OrcFormat**und **ParquetFormat**. Legen Sie die Eigenschaft **Typ** , klicken Sie unter Format auf einen der folgenden Werte ein. Details finden Sie unter [TextFormat zurück, der angibt](#specifying-textformat), [AvroFormat zurück, der angibt](#specifying-avroformat), [JsonFormat zurück, der angibt](#specifying-jsonformat), [OrcFormat angeben](#specifying-orcformat)und [ParquetFormat angeben](#specifying-parquetformat) . Wenn Sie die Dateien als kopieren möchten – ist zwischen Datei-basierten Stores (binäre kopieren), können Sie den Formatabschnitt beide Definitionen Eingabe- und Dataset überspringen. | Nein
fileFilter | Geben Sie einen Filter verwendet werden soll, wählen Sie eine Teilmenge der Dateien in den Ordnerpfad statt alle Dateien ein. <br/><br/>Sind die Werte zulässig: `*` (mehrere Zeichen) und `?` (einzelnes Zeichen).<br/><br/>Beispiel 1: "FileFilter": "* .log"<br/>Beispiel 2: "FileFilter": 2014 - 1-?. TXT"<br/><br/>Beachten Sie, dass die FileFilter für eine Eingabe Dateifreigabe-Dataset verfügbar ist. | Nein
| Komprimierung | Geben Sie den Typ und die Ebene der Komprimierung für die Daten ein. Unterstützte Datentypen sind **GZip** **Deflate**und **BZip2**. Unterstützte Ebenen sind **Optimal** und **am schnellsten**. Komprimierungseinstellungen werden für Daten in **AvroFormat** oder **OrcFormat**derzeit nicht unterstützt. Weitere Informationen finden Sie unter Abschnitt [Komprimierung unterstützen](#compression-support) . | Nein |


> [AZURE.NOTE] Sie können keine FileName und FileFilter gleichzeitig verwenden.

### <a name="using-partitionedby-property"></a>Verwenden die Eigenschaft partitionedBy

Wie im vorherigen Abschnitt erwähnt, können Sie eine dynamische Ordnerpfad und den Dateinamen für die Uhrzeit-Serie Daten mit PartitionedBy angeben. Sie können dazu die Daten Factory-Makros und das Systemvariablen SliceStart und SliceEnd, die für einen angegebenen Daten Segment den logischen Zeitraum angeben.

Informationen zur Zeit-Serie Datasets finden Sie unter Planung und Segmente, [Datasets erstellen](data-factory-create-datasets.md), [Planung und Ausführung](data-factory-scheduling-and-execution.md)und [Pipelines erstellen](data-factory-create-pipelines.md).

#### <a name="sample-1"></a>Beispiel 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy":
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

In diesem Beispiel {Segment} wird mit dem Wert der Daten Factory Systemvariablen SliceStart im Format (YYYYMMDDHH) ersetzt. Startzeit des Segments SliceStart verweist auf. Der Ordnerpfad unterscheidet sich für jedes Segment. Beispiel: Wikidatagateway/Wikisampledataout/2014100103 oder Wikidatagateway/Wikisampledataout/2014100104.

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

In diesem Beispiel werden Jahr, Monat, Tag und die Uhrzeit der SliceStart in separaten Variablen extrahiert, in denen die Eigenschaften Ordnerpfad und Dateiname verwendet.

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="file-share-copy-activity-type-properties"></a>Dateifreigabe Schrifteigenschaften Aktivität kopieren

**FileSystemSource** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| Rekursive | Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. | True, False (Standard)| Nein |

**FileSystemSink** unterstützt die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| copyBehavior | Definiert das Verhalten beim Kopieren, wenn die Quelle BlobSource oder Dateisystem ist. | **PreserveHierarchy:** Behält die Dateihierarchie im Zielordner an. D. h., ist der relative Pfad der Quelldatei in den Quellordner der relative Pfad der Zieldatei zum Zielordner entspricht.<br/><br/>**FlattenHierarchy:** Alle Dateien aus der Quellordner werden in die erste Ebene von Zielordner erstellt. Die Zieldateien werden mit einen Namen für die automatisch generierte erstellt.<br/><br/>**MergeFiles:** Verbindet alle Dateien aus dem Quellordner zu einer Datei an. Wenn der Name/Blob-Dateiname angegeben ist, ist der verbundenen Dateiname der angegebene Name. Andernfalls ist es eine Datei automatisch generiert. | Nein |

### <a name="recursive-and-copybehavior-examples"></a>Beispiele für rekursive und copyBehavior
In diesem Abschnitt beschreibt das sich daraus ergebende Verhalten beim Kopieren für verschiedene Kombinationen der Werte für die Eigenschaften rekursive und CopyBehavior.

Rekursive Wert | CopyBehavior Wert | Resultierende Verhalten
--------- | ------------ | --------
WAHR | preserveHierarchy | Für einen Quellordner Ordner1 mit der folgenden Struktur<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Zielordner Ordner1 wird mit derselben Struktur als Quelle erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5  
WAHR | flattenHierarchy | Für einen Quellordner Ordner1 mit der folgenden Struktur<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Das Ziel ist Ordner1 mit der folgenden Struktur erstellt: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen für Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen für Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen für File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen für File5
WAHR | mergeFiles | Für einen Quellordner Ordner1 mit der folgenden Struktur<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Das Ziel ist Ordner1 mit der folgenden Struktur erstellt: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1 + Datei2 + Datei3 + File4 + Datei 5 Inhalt in einer Datei mit einer automatisch generierten Dateinamen zusammengeführt werden.
falsch | preserveHierarchy | Für einen Quellordner Ordner1 mit der folgenden Struktur<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Zielordner Ordner1 ist mit der folgenden Struktur erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/><br/>Subfolder1 mit Datei3, File4 und File5 wird nicht übernommen.
falsch | flattenHierarchy | Für einen Quellordner Ordner1 mit der folgenden Struktur<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Zielordner Ordner1 ist mit der folgenden Struktur erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatisch generierte Namen für Datei2<br/><br/>Subfolder1 mit Datei3, File4 und File5 wird nicht übernommen.
falsch | mergeFiles | Für einen Quellordner Ordner1 mit der folgenden Struktur<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>Zielordner Ordner1 ist mit der folgenden Struktur erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei 1 + Datei2 Inhalt werden in einer Datei mit einer automatisch generierten Dateinamen zusammengeführt.<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierte Namen Datei 1<br/><br/>Subfolder1 mit Datei3, File4 und File5 wird nicht übernommen.


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Leistung und optimieren
 Wenn Sie wissen wichtige Faktoren, die das Verschieben von Daten (Kopieren Aktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren sie die Leistung auswirken, finden Sie unter [Kopieren Aktivität Leistung und Videogeräten Führungslinie](data-factory-copy-activity-performance.md).
