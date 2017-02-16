<properties 
    pageTitle="Verschieben von Daten zu/von DocumentDB | Microsoft Azure" 
    description="Erfahren Sie, wie Verschieben von Daten zu/aus Azure DocumentDB Sammlung mit Azure Data Factory" 
    services="data-factory, documentdb" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="multiple" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-documentdb-using-azure-data-factory"></a>Verschieben von Daten an und von DocumentDB mit Azure Data Factory

Dieser Artikel beschreibt, wie Sie in verwenden können, die Aktivität Kopieren einer Factory Azure-Daten zum Verschieben von Daten aus einem anderen Daten Azure DocumentDB speichern und Verschieben von Daten aus DocumentDB in einem anderen Datenspeicher. In diesem Artikel wird im Artikel [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) , die eine allgemeine Übersicht über das Verschieben von Daten mit Aktivität kopieren und unterstützten Data Store Kombinationen bietet erstellt.

So kopieren Sie Daten an und von Azure DocumentDB und Azure BLOB-Speicher Sie in den folgenden Beispielen. Daten kann jedoch kopierten **direkt** aus einer der Quellen an die senken angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores) die Aktivität kopieren in Azure Data Factory verwenden.  

> [AZURE.NOTE] Kopieren von Daten aus lokale/Azure IaaS Daten speichert in Azure DocumentDB und umgekehrt mit Datenverwaltungsgateway Version 2.1 und höher unterstützt.

## <a name="sample-copy-data-from-documentdb-to-azure-blob"></a>Beispiel: Kopieren Sie Daten aus DocumentDB in Azure Blob

Im folgenden Beispiel gezeigt:

1. Eine verknüpfte Dienst vom Typ [DocumentDb](#azure-documentdb-linked-service-properties).
2. Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [DocumentDbCollectionSource](#azure-documentdb-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Im Beispiel werden die Daten in Azure DocumentDB in Azure Blob kopiert. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben.

**Azure verknüpft DocumentDB Dienst:**

    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
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

**Azure Dokument DB Eingabemethoden Dataset:**

Im Beispiel wird davon ausgegangen, dass Sie eine Sammlung namens der **Person** in einer DocumentDB Azure-Datenbank haben.
 
Festlegen von "externe": "true" und ExternalData Richtlinieninformationen der Azure-Daten Factory-Dienst, dass die Tabelle wird von externen Daten Fabrik und nicht durch eine Aktivität in der Factory Daten erzeugt angeben.

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }


**Azure Blob ausgeben Dataset:**

Daten werden in einer neuen Blob stündlich durch den Pfad für die über die entsprechenden bestimmten Datetime mit Stunde Genauigkeit Blob kopiert.

    {
      "name": "PersonBlobTableOut",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Beispiel für JSON-Dokument in der Auflistung Person in einer Datenbank DocumentDB: 

    {
      "PersonId": 2,
      "Name": {
        "First": "Jane",
        "Middle": "",
        "Last": "Doe"
      }
    }

DocumentDB unterstützt Abfragen Dokumente, die mit einer SQL wie Syntax über hierarchische JSON-Dokumente. 

Beispiel: Wählen Sie Person.PersonId, Person.Name.First AS Vorname, Person.Name.Middle als MiddleName, LastName Person.Name.Last AS Person

Die folgenden Verkaufspipeline werden Daten aus der Auflistung der Person in der Datenbank DocumentDB in einer Azure Blob kopiert. Datensätze wurden als Teil der Aktivität kopieren die Eingabe und Ausgabe angegeben.  
    
    {
      "name": "DocDbToBlobPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "DocumentDbCollectionSource",
                "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                "nestingSeparator": "."
              },
              "sink": {
                "type": "BlobSink",
                "blobWriterAddHeader": true,
                "writeBatchSize": 1000,
                "writeBatchTimeout": "00:00:59"
              }
            },
            "inputs": [
              {
                "name": "PersonDocumentDbTable"
              }
            ],
            "outputs": [
              {
                "name": "PersonBlobTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromDocDbToBlob"
          }
        ],
        "start": "2015-04-01T00:00:00Z",
        "end": "2015-04-02T00:00:00Z"
      }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-documentdb"></a>Beispiel: Kopieren Sie Daten aus Azure Blob in Azure DocumentDB

Im folgenden Beispiel gezeigt:

1. Eine verknüpfte Dienst vom Typ [DocumentDb](#azure-documentdb-linked-service-properties).
2. Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3. Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) und [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties)verwendet.


Im Beispiel kopiert Daten aus Azure Blob zu Azure DocumentDB. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben.

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

**Azure verknüpft DocumentDB Dienst:**
    
    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure Blob Eingabe-Dataset:**

    {
      "name": "PersonBlobTableIn",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "MiddleName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
          }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "fileName": "input.csv",
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Azure DocumentDB ausgeben Dataset:**

Im Beispiel werden die Daten in einer Websitesammlung mit dem Namen "Person" kopiert.

    {
      "name": "PersonDocumentDbTableOut",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "Name.First",
            "type": "String"
          },
          {
            "name": "Name.Middle",
            "type": "String"
          },
          {
            "name": "Name.Last",
            "type": "String"
          }
        ],
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Die folgenden Verkaufspipeline kopiert Daten aus Azure Blob-Auflistung der Person in der DocumentDB. Datensätze wurden als Teil der Aktivität kopieren die Eingabe und Ausgabe angegeben. 
    
    {
      "name": "BlobToDocDbPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "DocumentDbCollectionSink",
                "nestingSeparator": ".",
                "writeBatchSize": 2,
                "writeBatchTimeout": "00:00:00"
              }
              "translator": {
                  "type": "TabularTranslator",
                  "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
              }
            },
            "inputs": [
              {
                "name": "PersonBlobTableIn"
              }
            ],
            "outputs": [
              {
                "name": "PersonDocumentDbTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromBlobToDocDb"
          }
        ],
        "start": "2015-04-14T00:00:00Z",
        "end": "2015-04-15T00:00:00Z"
      }
    }
 
Wenn die Stichprobe Blob Eingabe als 

    1,John,,Doe

Dann wird die Ausgabe JSON in DocumentDB als sein:

    {
      "Id": 1,
      "Name": {
        "First": "John",
        "Middle": null,
        "Last": "Doe"
      },
      "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
    }
    
DocumentDB ist ein NoSQL-Speicher für JSON-Dokumente, wo geschachtelte Strukturen zulässig sind. Azure Data Factory kann der Benutzer über **NestingSeparator**, Hierarchie zu kennzeichnen, also "." In diesem Beispiel. Das Trennzeichen, die Aktivität kopieren wird Generieren des Objekts "Name" mit drei untergeordneten Elementen zunächst mittleren und letzten, nach "Name.First", "Name.Middle" und "Name.Last" in die Definition der Tabelle.

## <a name="azure-documentdb-linked-service-properties"></a>Azure Verknüpfte Dienst DocumentDB Eigenschaften

Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die speziell für Dienst Azure DocumentDB verknüpft. 

| **Eigenschaft** | **Beschreibung** | **Erforderlich** |
| -------- | ----------- | --------- |
| Typ | Die Eigenschaft muss auf festgelegt sein: **DocumentDb** | Ja |
| connectionString | Geben Sie Informationen zum Verbinden mit DocumentDB Azure-Datenbank erforderlich sind. | Ja |

## <a name="azure-documentdb-dataset-type-properties"></a>Azure DocumentDB Dataset Schrifteigenschaften

Eine vollständige Liste der Abschnitte und zum Definieren von Datasets verfügbaren Eigenschaften finden Sie im Artikel [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und ein Dataset JSON-Richtlinie ähneln für alle Dataset-Typen (Azure SQL Azure Blob, Azure Table usw..).
 
Im Abschnitt TypeProperties unterscheidet sich für jede Art von Dataset und enthält Informationen über den Speicherort der Daten im Datenspeicher. Im Abschnitt TypeProperties für das Dataset vom Typ **DocumentDbCollection** weist die folgenden Eigenschaften.

| **Eigenschaft** | **Beschreibung** | **Erforderlich** |
| -------- | ----------- | -------- |
| collectionName | Name der Sammlung DocumentDB Dokument. | Ja |


Beispiel:

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

### <a name="schema-by-data-factory"></a>Schema von Daten Factory
Für Schema frei Datenspeicher wie DocumentDB leitet der Daten Factory-Dienst das Schema in eine der folgenden Methoden:  

1.  Wenn Sie die Struktur der Daten mithilfe der **Struktur** -Eigenschaft in der Definition Dataset angeben, berücksichtigt der Daten Factory-Dienst diese Struktur wie das Schema nach. In diesem Fall wird eine Zeile keinen Wert für eine Spalte enthält, ein Nullwert dafür bereitgestellt werden.
2.  Wenn Sie die Struktur der Daten mithilfe der **Struktur** -Eigenschaft in der Definition Dataset nicht angeben, leitet der Daten Factory-Dienst das Schema mithilfe der ersten Zeile in den Daten ein. In diesem Fall werden die erste Zeile das vollständige Schema nicht enthält, im Ergebnis Kopiervorgang fehlende einige Spalten werden.

Daher ist die beste Methode Schema frei Datenquellen, die Struktur der Daten, die mithilfe der Eigenschaft **Struktur** angeben.

## <a name="azure-documentdb-copy-activity-type-properties"></a>Azure DocumentDB kopieren Aktivität Schrifteigenschaften

Eine vollständige Liste der Abschnitte und zum Definieren von Aktivitäten verfügbaren Eigenschaften finden Sie im Artikel [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinie stehen für alle Arten von Aktivitäten.
 
**Hinweis:** Die Aktivität kopieren übernimmt nur eine Eingabe und die Ausgabe nur ein.

Im Abschnitt TypeProperties der Aktivität verfügbaren Eigenschaften variieren andererseits bei jeder Aktivität und bei Aktivität kopieren sie variieren je nach den Typen von Datenquellen und senken.

Bei Aktivität kopieren Wenn Quelle vom Typ **DocumentDbCollectionSource** ist stehen die folgenden Eigenschaften im Abschnitt **TypeProperties** :

| **Eigenschaft** | **Beschreibung** | **Zulässigen Werte** | **Erforderlich** |
| ------------ | --------------- | ------------------ | ------------ |
| Abfrage | Geben Sie die Abfrage, um Daten zu lesen. | Abfragezeichenfolge von DocumentDB unterstützt. <br/><br/>Beispiel:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` | Nein <br/><br/>Wenn nicht angegeben, die SQL-Anweisung, die ausgeführt wird:`select <columns defined in structure> from mycollection` 
| nestingSeparator | Sonderzeichen, um anzugeben, dass das Dokument geschachtelt ist | Beliebiges Zeichen. <br/><br/>DocumentDB ist ein NoSQL-Speicher für JSON-Dokumente, wo geschachtelte Strukturen zulässig sind. Azure Data Factory kann der Benutzer über NestingSeparator, Hierarchie zu kennzeichnen, also "." in den Beispielen oben. Das Trennzeichen, die Aktivität kopieren wird Generieren des Objekts "Name" mit drei untergeordneten Elementen zunächst mittleren und letzten, nach "Name.First", "Name.Middle" und "Name.Last" in die Definition der Tabelle. | Nein

**DocumentDbCollectionSink** unterstützt die folgenden Eigenschaften:

| **Eigenschaft** | **Beschreibung** | **Zulässigen Werte** | **Erforderlich** |
| -------- | ----------- | -------------- | -------- |
| nestingSeparator | Sonderzeichen in dem Quellspaltennamen an, dass die verschachtelten Dokuments ist erforderlich. <br/><br/>Beispielsweise über: `Name.First` in der Ausgabe Tabelle die folgende JSON-Struktur im Dokument DocumentDB erzeugt:<br/><br/>"Name": {}<br/>  "First": "Johann"<br/>}, | Zeichen, das zum Trennen von Verschachtelungsebenen verwendet wird.<br/><br/>Ist der Standardwert `.` (Punkt). | Zeichen, das zum Trennen von Verschachtelungsebenen verwendet wird. <br/><br/>Ist der Standardwert `.` (Punkt). | Nein | 
| writeBatchSize | Anzahl der parallele Anfragen DocumentDB Dienst zum Erstellen von Dokumenten.<br/><br/>Beim Kopieren von Daten aus/DocumentDB mithilfe dieser Eigenschaft können Sie die Leistung optimieren. Sie können eine bessere Leistung erwarten, wenn Sie WriteBatchSize erhöhen, weil mehr parallelen Anfragen zu DocumentDB gesendet werden. Jedoch müssen Sie vermeiden, die begrenzungsebene auslösen kann, die Fehlermeldung: "Zins umfangreich ist anfordern".<br/><br/>Begrenzungsebene wird anhand einer Anzahl von Faktoren, einschließlich der Größe von Dokumenten, die Anzahl von Ausdrücken in Dokumenten, Indizierung Richtlinie der Ziel-Websitesammlung usw. entschieden. Für Kopiervorgänge, können Sie eine bessere Websitesammlung (z. B. S3) die am häufigsten Durchsatz verfügbar sein (2.500 anfordern Einheiten/Sekunde). | Ganze Zahl | Nein (Standard: 10000) |
| writeBatchTimeout | Wartezeit für den Vorgang abgeschlossen, bevor ein Timeout. | TimeSpan<br/><br/> Beispiel: "00: 30:00" (30 Minuten). | Nein |
 
## <a name="appendix"></a>Anlage
1. **Frage:**  
    das Kopieren Aktivität Support Aktualisieren der vorhandenen Datensätze enthält?

    **Antwort:**  
    Nein.

2. **Frage:**  
    Funktionsweise eine Wiederholung eine Kopie DocumentDB Geschäft mit bereits Datensätze kopiert?

    **Antwort:**  
    Wenn Datensätze ein Feld "ID haben" und der Kopiervorgang zum Einfügen eines Datensatzes mit der gleichen ID versucht, der Kopiervorgang löst einen Fehler aus.  
 
3. **Frage:** 
    Daten Factory unterstützt [Bereich oder Aufteilen von hashbasierten Daten](https://azure.microsoft.com/documentation/articles/documentdb-partition-data/)? 

    **Antwort:** 
    Nein. 
4. **Frage:** 
    können mehr als eine Sammlung von DocumentDB für eine Tabelle angeben?
    
    **Antwort:** 
    Nein. Zurzeit kann nur eine Auflistung angegeben werden muss.
     
## <a name="performance-and-tuning"></a>Leistung und optimieren  
Finden Sie unter [Kopieren Aktivität Leistung und Optimieren von Leitfaden für](data-factory-copy-activity-performance.md) die Leistung Einfluss der Daten Bewegung (Kopieren Aktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren sie wichtige Faktoren lernen.
