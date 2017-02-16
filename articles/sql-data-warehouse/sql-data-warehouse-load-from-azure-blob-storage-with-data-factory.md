<properties
   pageTitle="Laden Sie Daten aus Azure Blob-Speicher in Azure SQL-Data Warehouse (Azure Data Factory) | Microsoft Azure"
   description="Informationen Sie zum Laden von Daten mit Azure Data Factory"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>
<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="load-data-from-azure-blob-storage-into-azure-sql-data-warehouse-azure-data-factory"></a>Laden Sie Daten aus Azure Blob-Speicher in Azure SQL-Data Warehouse (Azure Daten vorinstalliert)

> [AZURE.SELECTOR]
- [Daten Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
- [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

 In diesem Lernprogramm erfahren Sie, wie Sie eine Verkaufspipeline in Azure Data Factory zum Verschieben von Daten aus Azure-Speicher Blob in SQL Data Warehouse erstellen. Mit den folgenden Schritten können Sie:

+ Einrichten von Beispieldaten in einer Azure-Blob-Speicher.
+ Herstellen einer Verbindung Azure Data Factory mit Ressourcen.
+ Erstellen Sie eine Verkaufspipeline zum Verschieben von Daten aus Speicher Blobs in SQL Data Warehouse.

>[AZURE.VIDEO loading-azure-sql-data-warehouse-with-azure-data-factory]


## <a name="before-you-begin"></a>Vorbemerkung

Wenn Sie sich mit Azure Data Factory vertraut machen, finden Sie unter [Einführung in Azure Data Factory][].

### <a name="create-or-identify-resources"></a>Erstellen oder Identifizieren von Ressourcen

Bevor Sie beginnen in diesem Lernprogramm, müssen Sie die folgenden Ressourcen verfügen.

   + **Azure-Speicher Blob**: in diesem Lernprogramm verwendet Azure-Speicher Blob als Datenquelle für die Verkaufspipeline Azure Data Factory und daher, muss eine ist, um die Beispieldaten zu speichern. Wenn Sie eine bereits besitzen, erfahren Sie, wie Sie [Speicherplatz Konto erstellen][].

   + **SQL Data Warehouse**: in diesem Lernprogramm verschiebt die Daten aus Azure-Speicher Blob SQL Data Warehouse und in diesem Fall müssen online ein Datawarehouse haben, die mit den Beispieldaten AdventureWorksDW geladen wird. Wenn Sie noch nicht über ein Datawarehouse verfügen, erfahren Sie, wie Sie [eine Bereitstellung][Create a SQL Data Warehouse]. Wenn Sie ein Datawarehouse haben, aber es mit den Beispieldaten Bereitstellung haben, können Sie [manuell zu laden][Load sample data into SQL Data Warehouse].

   + **Azure Data Factory**: Azure Data Factory vervollständigt die tatsächliche Auslastung und daher müssen Sie eine verfügen, das Sie verwenden können, um die Daten Bewegung Verkaufspipeline zu erstellen. Wenn Sie eine bereits besitzen, erfahren Sie, wie Sie in Schritt 1 der [Erste Schritte mit Azure Data Factory (Daten Factory-Editor)][]erstellt.

   + **AZCopy**: benötigten AZCopy, um die Beispieldaten zu Ihrer Azure-Blob-Speicher von Ihrem lokalen Client zu kopieren. Installieren Anweisungen finden Sie unter der [Dokumentation AZCopy][].

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Schritt 1: Kopieren von Beispieldaten in BLOB-Speicher von Azure

Nachdem Sie die Teile bereit haben, sind Sie bereit, um Beispieldaten in Ihrer Azure-Blob-Speicher zu kopieren.

1. [Laden Sie Beispieldaten herunter][]. Diese Daten werden Ihre Beispieldaten AdventureWorksDW noch drei Jahre mit Verkaufsdaten hinzugefügt werden.

2. Verwenden Sie diesen Befehl AZCopy, um die drei Jahren von Daten in Ihrer Azure-Blob-Speicher zu kopieren.

````
AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
````


## <a name="step-2-connect-resources-to-azure-data-factory"></a>Schritt 2: Ressourcen Herstellen einer Verbindung mit Azure Data Factory

Jetzt, dass die Daten vorhanden ist, können wir die Verkaufspipeline Azure Data Factory, um die Daten aus Azure Blob-Speicher in SQL Data Warehouse verschieben erstellen.

Um anzufangen, öffnen Sie das [Azure-Portal][] , und wählen Sie Ihre Daten Factory aus dem Menü links aus.

### <a name="step-21-create-linked-service"></a>Schritt 2.1: Erstellen von verknüpften Service

Verknüpfen Sie Ihre Azure-Speicher-Konto und SQL Data Warehouse mit Ihrer Daten Factory ein.  

1. Zunächst beginnen Sie die Registrierung zu, indem Sie im Abschnitt "Verknüpfte Dienstleistungen" Ihrer Daten Factory auf, und klicken Sie dann auf 'neue Datenspeicher'. Wählen Sie einen Namen zum Registrieren der Azure-Speicher unter select Azure-Speicher als auswählen, und geben Sie Ihren Kontonamen und Kontoschlüssel ein.

2. Zum Registrieren navigieren SQL Data Warehouse Sie zu 'Autor und bereitstellen' Abschnitt auswählen 'Datenspeicher neu' und dann 'Azure SQL-Data Warehouse'. Kopieren und Einfügen in dieser Vorlage, und füllen Sie dann Ihre spezifischen Informationen.

```JSON
{
    "name": "<Linked Service Name>",
    "properties": {
        "description": "",
        "type": "AzureSqlDW",
        "typeProperties": {
             "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
         }
    }
}
```

### <a name="step-22-define-the-dataset"></a>Schritt 2.2: Definieren Sie das dataset

Nach dem Erstellen von verknüpften Diensten, haben wir die Datensätze definieren.  Hier bedeutet dies definieren die Struktur der Daten, die aus dem Speicher in Ihr Datawarehouse verschoben wird.  Weitere Informationen zum Erstellen von

1. Starten Sie dieses Verfahren durch Navigieren zum Abschnitt 'Autor und bereitstellen' Ihrer Daten Factory ein.

2. Klicken Sie auf 'Neue Dataset' und dann auf 'Azure BLOB-Speicher' zum Verknüpfen von Ihrem Storage an Ihre Daten Factory.  Sie können die unter Skript zum Definieren von Daten in Azure Blob-Speicher:

```JSON
{
    "name": "<Dataset Name>",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "<linked storage name>",
        "typeProperties": {
            "folderPath": "<containter name>",
            "fileName": "FactInternetSales.csv",
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
```


3. Wir werden nun auch unser Dataset für SQL Data Warehouse definieren.  Zunächst auf die gleiche Weise, indem Sie auf 'Neue Dataset', 'Azure SQL-Data Warehouse'.

```JSON
{
    "name": "DWDataset",
    "properties": {
        "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "FactInternetSales"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

## <a name="step-3-create-and-run-your-pipeline"></a>Schritt 3: Erstellen Sie und führen Sie der Verkaufspipeline aus

Schließlich werden wir ansetzen, und führen Sie die Verkaufspipeline in Azure Data Factory.  Dies ist der Vorgang, der die tatsächlichen Daten Bewegung abgeschlossen werden kann.  Sie können eine vollständige Ansicht Vorgänge, die Sie mit SQL Data Warehouse und Azure Data Factory abschließen können finden [hier][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].

Jetzt im Abschnitt 'Autor und bereitstellen' auf 'Weitere Befehle' und klicken auf 'Neue Verkaufspipeline'.  Nachdem Sie die Verkaufspipeline erstellt haben, können Sie mithilfe der unter Code ein, um die Daten in Ihr Datawarehouse zu übertragen:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
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
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

Starten Sie weitere Informationen anzeigen:

- [Azure Data Factory learning Pfad][].
- [SQL Azure Data Warehouse-Verbinder][]. Dies ist das Thema Core Bezug für die Verwendung von Azure Data Factory mit Azure SQL-Data Warehouse.


Diese Themen finden Sie ausführliche Informationen zu Azure Data Factory. Sie diskutieren Azure SQL-Datenbank oder HDinsight, aber die Informationen gelten auch für Azure SQL-Data Warehouse.

- [Lernprogramm: Erste Schritte mit Azure Data Factory][] Dies ist das Lernprogramm Core für die Verarbeitung von Daten mit Azure Data Factory. In diesem Lernprogramm erstellen Sie Ihre erste Verkaufspipeline, die HDInsight zum Transformieren und Analysieren von Webprotokolle monatlich verwendet. Beachten Sie in diesem Lernprogramm keine Kopie-Aktivität vorhanden ist.
- [Lernprogramm: Kopieren von Daten aus Azure-Speicher Blob mit Azure SQL-Datenbank][]. In diesem Lernprogramm erstellen Sie eine Verkaufspipeline in Azure Data Factory zum Kopieren von Daten aus Azure-Speicher Blob mit Azure SQL-Datenbank.


<!--Image references-->

<!--Article references-->
[AZCopy Dokumentation]: ../storage/storage-use-azcopy.md
[SQL Azure Datawarehouse Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Erstellen Sie ein Speicherkonto]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Erste Schritte mit Azure Data Factory (Daten Factory-Editor)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Einführung in Factory Azure-Daten]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Lernprogramm: Kopieren von Daten aus Azure-Speicher Blob mit Azure SQL-Datenbank]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Lernprogramm: Erste Schritte mit Azure Data Factory]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Pfad Azure Data Factory-Schulung]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure-portal]: https://portal.azure.com
[Herunterladen von Beispieldaten]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv
