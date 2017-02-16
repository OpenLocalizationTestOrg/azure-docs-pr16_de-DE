<properties
    pageTitle="Verschieben von Daten an und von SQL Server | Factory Azure-Daten"
    description="Informationen Sie zum Verschieben von Daten in SQL Server-Datenbank, die lokal ist oder in einer Azure virtueller Computer Azure Data Factory verwenden."
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
    ms.date="08/31/2016"
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-sql-server-on-premises-or-on-iaas-azure-vm-using-azure-data-factory"></a>Verschieben von Daten zu und aus SQL Server lokal oder auf IaaS (Azure virtueller Computer) mit Azure Data Factory
In diesem Artikel wird erläutert, wie Sie die Aktivität kopieren zum Verschieben von Daten aus dem und in SQL Server in einem anderen Datenspeicher verwenden können. In diesem Artikel basiert auf den [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) Artikel eine allgemeine Übersicht über das Verschieben von Daten, und Datenspeicher als Quellen und Empfängern unterstützt bietet.

## <a name="supported-sources-and-sinks"></a>Unterstützte Quellen und Empfängern
Finden Sie unter [unterstützte Datenspeicher](data-factory-data-movement-activities.md#supported-data-stores-and-formats) Tabelle eine Liste der Datenspeicher unterstützt als Quellen oder senken durch die Aktivität kopieren. Sie können Daten aus einem beliebigen Datenspeicher unterstützte Datenquelle in SQL Server oder SQL Server auf einen beliebigen Datenspeicher unterstützten Empfänger verschieben.

## <a name="create-pipeline"></a>Erstellen der Verkaufspipeline
Sie können eine Verkaufspipeline mit einer Kopie Aktivität erstellen, die Daten in einer lokalen SQL Server-Datenbank über verschiedene Tools-APIs verschoben.  

- Assistent zum Kopieren von
- Azure-portal
- Visual Studio
- Azure PowerShell
- .NET API
- REST-API

Finden Sie unter [Kopieren Aktivität Lernprogramm](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) für eine schrittweise Anleitung zum Erstellen von einer Verkaufspipeline mit einer Kopie Aktivität auf verschiedene Arten.

## <a name="enabling-connectivity"></a>Aktivieren der Konnektivität

Konzepte und Schritte zum Herstellen einer Verbindung mit SQL Server als Host lokal oder in Azure IaaS (Infrastructure-as-a-Service) virtuellen Computern erforderlich sind gleich. In beiden Fällen müssen Sie Datenverwaltungsgateway für die Verbindung zu verwenden.

Finden Sie unter [Verschieben von Daten zwischen lokalen Orten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) Artikel lernen Datenverwaltungsgateway und eine schrittweise Anleitung zum Einrichten des Gateways. Einrichten von einem Gatewayinstanz ist eine Vorbedingung für das Herstellen einer Verbindung mit SQL Server.

Während Sie als SQL Server für eine bessere Leistung Gateways auf dem gleichen lokalen Computer oder eine Instanz der Cloud virtueller Computer installieren können, empfehlen wir, dass Sie auf separaten Computern installiert haben. Das Gateway und SQL Server auf separaten Computern Probleme Reduzierung von Ressourcenkonflikten.


## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit, eine Verkaufspipeline zu erstellen, die Daten aus einer SQL Server-Datenbank an die Empfänger unterstützten Datenspeicher kopiert besteht darin, den Assistenten zum Kopieren von Daten verwenden. Finden Sie unter [Lernprogramm: Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) für eine schnelle Exemplarische Vorgehensweise zum Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von Daten. 

Im folgende Beispiel enthält die Stichprobe JSON-Definitionen, die Sie verwenden können, erstellen Sie eine Verkaufspipeline mithilfe von [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). So kopieren Sie Daten an und von SQL Server und Azure BLOB-Speicher Sie in den folgenden Beispielen. Daten kann jedoch kopierten **direkt** aus einer der Quellen an die senken angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores) die Aktivität kopieren in Azure Data Factory verwenden.     

## <a name="sample-copy-data-from-sql-server-to-azure-blob"></a>Beispiel: Kopieren von Daten aus SQL Server in Azure Blob

Im folgende Beispiel gezeigt:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesSqlServer](data-factory-sqlserver-connector.md#sql-server-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [SqlServerTable](data-factory-sqlserver-connector.md#sql-server-dataset-type-properties).
4.  Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Der [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [SQLSource aus](data-factory-sqlserver-connector.md#sql-server-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Im Beispiel kopiert Time-Serie Daten aus einer SQL Server-Tabelle in eine Azure Blob fest stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben.

Richten Sie als ersten Schritt das datenverwaltungsgateway ein. Die Hinweise finden Sie im Artikel [Verschieben von Daten zwischen lokalen Orten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) .

**Verknüpft von SQL Server-Dienst**

    {
      "Name": "SqlServerLinkedService",
      "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
          "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
          "gatewayName": "<gatewayname>"
        }
      }
    }

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

**Eingabe SQL Server-dataset**

Das Beispiel wird vorausgesetzt, Sie eine Tabelle erstellt haben "MyTable" in SQL Server und eine Spalte mit der Bezeichnung "Timestampcolumn" für die Reihe Uhrzeitdaten enthält. Sie können über mehrere Tabellen in derselben Datenbank mit einem einzelnen Dataset Abfragen, aber des Datasets Tabellenname TypeProperty muss eine einzelne Tabelle verwendet werden.

Festlegen von "externe": "true" teilt mit Daten Factory-Dienst, dass das Dataset externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.

    {
      "name": "SqlServerInput",
      "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlServerLinkedService",
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

**Azure Blob ausgeben dataset**

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

**Pipeline mit Aktivität kopieren**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass diese Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **SQLSource aus** festgelegt ist, und Typ der **Empfänger** auf **BlobSink**festgelegt ist. Die SQL-Abfrage für die Eigenschaft **SqlReaderQuery** angegeben, werden die Daten in der letzten Stunde zu kopierenden markiert.


    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "SqlServertoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": " SqlServerInput"
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


In diesem Beispiel wird die **SqlReaderQuery** für die SQLSource aus angegeben. Die Aktivität kopieren führt diese Abfrage in der SQL Server-Datenbank Datenquelle verfügen, um die Daten zu erhalten. Alternativ können Sie eine gespeicherte Prozedur angeben, nach Angabe von **SqlReaderStoredProcedureName** und **StoredProcedureParameters** (wenn die gespeicherte Prozedur Parameter akzeptiert). Die SqlReaderQuery kann mehrere Tabellen in der Datenbank des Eingabe-Dataset optimiert verwiesen werden. Es ist nicht nur der Tabelle festlegen als des Datasets Tabellenname TypeProperty begrenzt.


Wenn Sie keine SqlReaderQuery oder SqlReaderStoredProcedureName angeben, die Spalten im Abschnitt Struktur definiert dienen zum Erstellen einer Auswahlabfrage, die für die SQL Server-Datenbank ausgeführt. Wenn die Definition der Dataset nicht die Struktur verfügt, werden alle Spalten aus der Tabelle ausgewählt.


Die Liste der Eigenschaften von SQLSource aus und BlobSink unterstützt finden Sie im Abschnitt [Sql-Quelle](#sqlsource) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) .

## <a name="sample-copy-data-from-azure-blob-to-sql-server"></a>Beispiel: Daten aus Azure Blob in SQL Server

Im folgende Beispiel gezeigt:

1.  Verknüpfte Dienst vom Typ [OnPremisesSqlServer](data-factory-sqlserver-connector.md#sql-server-linked-service-properties).
2.  Verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [SqlServerTable](data-factory-sqlserver-connector.md#sql-server-dataset-type-properties).
4.  Der [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) und [SqlSink](data-factory-sqlserver-connector.md#sql-server-copy-activity-type-properties)verwendet.

Die Stichprobe Kopien Time-Serie zu einem SQL Server Daten aus einer Azure blob Tabelle pro Stunde an. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben.

**Verknüpft von SQL Server-Dienst**

    {
      "Name": "SqlServerLinkedService",
      "properties": {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
          "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
          "gatewayName": "<gatewayname>"
        }
      }
    }

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

**Eingabe Azure Blob-dataset**

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

**SQL Server Ausgabe dataset**

Im Beispiel werden die Daten in einer Tabelle "MyTable" in SQL Server kopiert. Erstellen Sie die Tabelle in SQL Server mit der gleichen Anzahl von Spalten wie erwartet die BLOB-CSV-Datei enthalten. Neue Zeilen werden stündlich zur Tabelle hinzugefügt.

    {
      "name": "SqlServerOutput",
      "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlServerLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Pipeline mit Aktivität kopieren**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass diese Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **BlobSource** festgelegt ist, und Typ der **Empfänger** auf **SqlSink**festgelegt ist.

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
                "name": " SqlServerOutput "
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

## <a name="sql-server-linked-service-properties"></a>Eigenschaften von SQL Server verknüpft
In den Beispielen haben Sie einen verknüpften Dienst des Typs **OnPremisesSqlServer** zum Verknüpfen von einer lokalen SQL Server-Datenbank in eine Factory Daten verwendet. Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die speziell für lokale verknüpft von SQL Server-Dienst an.

Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die speziell für SQL Server verknüpft-Dienst an.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Typ | Die Typeigenschaft auf festgelegt sein sollte: **OnPremisesSqlServer**. | Ja |
| connectionString | Geben Sie ConnectionString Informationen, die Verbindung zu einer lokalen SQL Server-Datenbank mithilfe der SQL-Authentifizierung oder Windows-Authentifizierung erforderlich. | Ja |
| gatewayName | Name des Gateways, das der Daten Factory-Dienst verwendet werden sollen, die Verbindung zu einer lokalen SQL Server-Datenbank herstellen. | Ja |
| Benutzername | Geben Sie Benutzernamen an, wenn Sie die Windows-Authentifizierung verwenden. Beispiel: **Domänenname\\Benutzername**. | Nein |
| Kennwort | Geben Sie das Kennwort für das Benutzerkonto, das Sie für den Benutzernamen angegeben haben. | Nein |

Sie können Verschlüsseln der Anmeldeinformationen mithilfe des Cmdlets **New-AzureRmDataFactoryEncryptValue** und diese in der Verbindungszeichenfolge verwenden, wie im folgenden Beispiel (**EncryptedCredential** Eigenschaft) dargestellt:  

    "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",

### <a name="samples"></a>Beispiele

**JSON für die Verwendung der SQL-Authentifizierung**

    {
        "name": "MyOnPremisesSQLDB",
        "properties":
        {
            "type": "OnPremisesSqlLinkedService",
            "typeProperties": {
                "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
                "gatewayName": "<gateway name>"
            }
        }
    }

**JSON für die Verwendung von Windows-Authentifizierung**

Wenn Benutzername und Kennwort angegeben sind, werden Sie von Gateway Identitätswechsel das angegebene Benutzerkonto für die Verbindung zu einer lokalen SQL Server-Datenbank verwendet. Andernfalls verbindet Gateways auf dem SQL Server direkt mit den Sicherheitskontext des Gateways (deren Startkonto).

    {
         "Name": " MyOnPremisesSQLDB",
         "Properties":
         {
             "type": "OnPremisesSqlLinkedService",
             "typeProperties": {
                "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
                "username": "<domain\\username>",
                "password": "<password>",
                "gatewayName": "<gateway name>"
            }
         }
    }

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für eine SQL Server-Datenquelle finden Sie unter [Festlegen der Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="sql-server-dataset-type-properties"></a>SQL Server-Typ Datensatzeigenschaften
In den Beispielen haben Sie ein Dataset vom Typ **SqlServerTable** Darstellung eine Tabelle in einer SQL Server-Datenbank verwendet.  

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung finden Sie im Artikel [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinie eines Datasets JSON ähneln für alle Dataset-Typen (SQL Server, Azure Blob, Azure Table usw..).

Im Abschnitt TypeProperties unterscheidet sich für jede Art von Dataset und enthält Informationen über den Speicherort der Daten im Datenspeicher. Im Abschnitt **TypeProperties** für das Dataset vom Typ **SqlServerTable** weist die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Tabellenname | Name der Tabelle in der Instanz von SQL Server-Datenbank, der auf Verknüpfte Dienst verweist. | Ja |

## <a name="sql-server-copy-activity-type-properties"></a>SQL Server kopieren Aktivität Schrifteigenschaften
Wenn Sie Daten aus einer SQL Server-Datenbank verschieben, legen Sie den Quelltyp in der Kopie Aktivität zu **SQLSource aus**. Auf ähnliche Weise, wenn Sie Daten in einer SQL Server-Datenbank verschieben, legen Sie den Typ der Empfänger in der Kopie Aktivität zu **SqlSink**. Dieser Abschnitt enthält eine Liste der Eigenschaften von SQLSource aus und SqlSink unterstützt. 

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten verfügbar sind finden Sie unter Artikel [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien zur Verfügung stehen für alle Arten von Aktivitäten.

> [AZURE.NOTE] Die Aktivität kopieren übernimmt nur eine Eingabe und die Ausgabe nur ein.

Im Abschnitt TypeProperties der Aktivität verfügbaren Eigenschaften variieren andererseits bei jeder Aktivität. Aktivitäten, kopieren variieren je nach den Typen von Datenquellen und senken.

### <a name="sqlsource"></a>SQLSource aus

Wenn Quelle in einer Aktivität Kopie vom Typ **SQLSource aus**ist, sind die folgenden Eigenschaften im Abschnitt **TypeProperties** verfügbar:

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| sqlReaderQuery | Verwenden Sie die benutzerdefinierte Abfrage, um Daten zu lesen. | SQL-Abfragezeichenfolge. Beispiel: Wählen Sie * from MyTable. Möglicherweise mehrere Tabellen aus der Datenbank des Eingabe-Dataset optimiert verwiesen werden. Wenn nicht angegeben, die SQL-Anweisung, die ausgeführt wird: select, from MyTable. | Nein |
| sqlReaderStoredProcedureName | Namen der gespeicherten Prozedur, die Daten aus der Quelltabelle liest. | Namen der gespeicherten Prozedur. | Nein |
| storedProcedureParameters | Parameter für die gespeicherte Prozedur. | Name/Wert-Paare. Namen und die Groß-/Kleinschreibung von Parametern müssen den Namen und die Groß-/Kleinschreibung des Parameter der gespeicherten Prozedur übereinstimmen. | Nein |

Wenn für die SQLSource aus der **SqlReaderQuery** angegeben ist, führt die Aktivität kopieren diese Abfrage für die SQL Server-Datenbank Datenquelle verfügen, um die Daten zu erhalten.

Alternativ können Sie eine gespeicherte Prozedur angeben, nach Angabe von **SqlReaderStoredProcedureName** und **StoredProcedureParameters** (wenn die gespeicherte Prozedur Parameter akzeptiert).

Wenn Sie entweder SqlReaderQuery oder SqlReaderStoredProcedureName nicht angeben, werden die Spalten im Abschnitt Struktur definiert verwendet, zum Erstellen einer Auswahlabfrage, die für die SQL Server-Datenbank ausgeführt. Wenn die Definition der Dataset nicht die Struktur verfügt, werden alle Spalten aus der Tabelle ausgewählt.

> [AZURE.NOTE] Wenn Sie **SqlReaderStoredProcedureName**verwenden, müssen Sie einen Wert für die Eigenschaft **Tabellenname** im Dataset JSON angeben. Es gibt keine Validierungen durch für diese Tabelle ausgeführt.

### <a name="sqlsink"></a>SqlSink

**SqlSink** unterstützt die folgenden Eigenschaften:


| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| writeBatchTimeout | Wartezeit für Stapel einfügen nach Abschluss des Vorgangs bevor es abläuft. | TimeSpan<br/><br/> Beispiel: "00: 30:00" (30 Minuten). | Nein |
| writeBatchSize | Fügt Daten in der SQL-Tabelle an, wenn die Puffergröße WriteBatchSize erreicht. | Ganzzahl (Anzahl von Zeilen) | Nein (Standard: 10000)
| sqlWriterCleanupScript | Geben Sie die Abfrage für Kopieren Aktivität auszuführende so, dass die Daten eines bestimmten Segments bereinigt werden. Weitere Informationen finden Sie unter [Wiederholbarkeit](#repeatability-during-copy) Abschnitt. | Eine Abfrage-Anweisung.  | Nein |
| sliceIdentifierColumnName | Geben Sie die Spaltennamen für Kopieren Aktivität automatisch generiert Segment Bezeichner enthält, füllen Sie die Daten eines bestimmten Segments erneut ausführen, wenn Bereinigen verwendet wird. Weitere Informationen finden Sie unter [Wiederholbarkeit](#repeatability-during-copy) Abschnitt. | Spaltenname einer Spalte mit dem Datentyp des binary(32). | Nein |
| sqlWriterStoredProcedureName | Namen der gespeicherten Prozedur die Upserts (Updates/Insert) Daten in die Zieltabelle. | Namen der gespeicherten Prozedur. | Nein |
| storedProcedureParameters | Parameter für die gespeicherte Prozedur. | Name/Wert-Paare. Namen und die Groß-/Kleinschreibung von Parametern müssen den Namen und die Groß-/Kleinschreibung des Parameter der gespeicherten Prozedur übereinstimmen. | Nein |
| sqlWriterTableType | Geben Sie Tabellennamen geben an, in der gespeicherten Prozedur verwendet werden soll. Kopieren Aktivität bereitgestellt, die Daten verschoben wird in einer temporären Tabelle mit diesen Table-Datentyp. Gespeicherte Prozedurcode kann dann die Daten mit den vorhandenen Daten kopierte zusammenführen. | Ein Tabellenname Typ. | Nein |

## <a name="troubleshooting-connection-issues"></a>Behandeln von Verbindungsproblemen

1. Konfigurieren der SQL Server um remote-Verbindungen zu übernehmen. Starten Sie **SQL Server Management Studio**, mit der rechten Maustaste **Server**, und klicken Sie auf **Eigenschaften**. Wählen Sie **Verbindungen** aus der Liste aus, und aktivieren Sie **remote-Verbindungen zulassen auf dem Server**.

    ![Aktivieren des remote-Verbindungen](.\media\data-factory-sqlserver-connector\AllowRemoteConnections.png)

    Detaillierten Schritte finden Sie unter [Konfigurieren der RAS Server-Konfigurations-Option](https://msdn.microsoft.com/library/ms191464.aspx) .
2. **SQL Server-Konfigurations-Manager**zu starten. Erweitern Sie **SQL Server-Netzwerkkonfiguration** für die gewünschte Instanz aus, und wählen Sie **Protokolle für MSSQLSERVER**aus. Klicken Sie im rechten Bereich Protokolle sollte angezeigt werden. Aktivieren Sie TCP/IP, indem Sie mit der rechten Maustaste **TCP/IP** , und klicken Sie auf **Aktivieren**.

    ![Aktivieren von TCP/IP](.\media\data-factory-sqlserver-connector\EnableTCPProptocol.png)

    Details und alternative Methoden für die Aktivierung der TCP/IP Protokoll finden Sie unter [Aktivieren oder deaktivieren ein Netzwerk-Server-Protokoll](https://msdn.microsoft.com/library/ms191294.aspx) .
3. Doppelklicken Sie im selben Fenster auf **TCP/IP** zum Fenster **TCP/IP** starten.
4. Wechseln Sie zur Registerkarte **IP-Adressen** . Führen Sie einen Bildlauf nach unten bis zum Abschnitt **IPAll** finden Sie unter. Notieren Sie sich den **TCP- **(der Standardwert liegt **1433**).
5. Erstellen Sie eine **Regel für die Windows-Firewall** auf dem Computer eingehenden Datenverkehr über diesen Anschluss zulassen.  
6. **Überprüfen der Verbindung**: Verwenden Sie zum Verbinden mit den vollqualifizierten Namen mit SQL Server, SQL Server Management Studio aus einem anderen Computer. Beispiel: "<machine>. <domain>. corp.<company>.com, 1433. "

    > [AZURE.IMPORTANT]
    > Ausführliche Informationen finden Sie unter [Ports und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#port-and-security-considerations) .
    >   
    > Finden Sie unter [Behandeln von Gateway Probleme](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) Tipps zur Behebung von Verbindung/Gateway zusammenhängende Probleme. 

## <a name="identity-columns-in-the-target-database"></a>Identitätsspalten in der Zieldatenbank
Dieser Abschnitt enthält ein Beispiel, in dem Daten aus einer Quelltabelle mit keine Identitätsspalte in eine Zieltabelle mit einer Identitätsspalte kopiert.

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
            "published": false,
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
            "published": false,
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


### <a name="type-mapping-for-sql-server--azure-sql"></a>Für SQLServer und SQL Azure-Zuordnung

Wie in den [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) Artikel erwähnt, führt die Aktivität kopieren automatische Konvertieren des Datentyps von Arten von Datenquellen zu ignorieren Typen mit dem folgenden Ansatz für-Schritt 2:

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
