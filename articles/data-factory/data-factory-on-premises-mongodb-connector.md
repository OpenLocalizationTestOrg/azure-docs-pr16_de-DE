<properties 
    pageTitle="Verschieben von Daten aus Daten Factory mit MongoDB | Microsoft Azure" 
    description="Informationen Sie zum Verschieben von Daten aus Azure Data Factory mit MongoDB-Datenbank." 
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
    ms.date="08/04/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-mongodb-using-azure-data-factory"></a>Verschieben von Daten aus MongoDB Azure Data Factory verwenden

In diesem Artikel wird beschrieben, wie Sie die Aktivität kopieren in einer Factory Azure-Daten zum Verschieben von Daten aus einer lokalen MongoDB-Datenbank auf einem anderen Datenspeicher verwenden können. In diesem Artikel basiert auf den [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) Artikel bietet eine allgemeine Übersicht über das Verschieben von Daten mit Aktivität kopieren und die Quelle/Empfänger Daten Store Kombinationen, die die Aktivität kopieren unterstützt.

Factory Datendienst unterstützt das Herstellen einer Verbindung mit einer lokalen MongoDB Quellen das Datenverwaltungsgateway verwenden. Finden Sie unter [Datenverwaltungsgateway](data-factory-data-management-gateway.md) Artikel Datenverwaltungsgateway lernen und [Verschieben von Daten aus lokalen in die cloud](data-factory-move-data-between-onprem-and-cloud.md) -Artikel, um eine schrittweise Anleitung zum Einrichten von Gateways eine Verkaufspipeline Daten zum Verschieben von Daten. 

> [AZURE.NOTE] Sie müssen das Gateway zu verwenden, um die Verbindung MongoDB, auch wenn es in Azure IaaS virtuellen Computern gehostet wird. Wenn Sie versuchen, eine Verbindung mit einer Instanz von MongoDB in der Cloud gehostet wird, können Sie auch die Gatewayinstanz in die IaaS VM installieren.

Daten Factory unterstützt derzeit nur Daten von MongoDB für andere Datenspeicher, aber nicht zum Verschieben von Daten aus anderen Datenspeicher zu MongoDB.

## <a name="prerequisites"></a>Erforderliche Komponenten
Für den Dienst Azure Data Factory zu Ihrer lokalen MongoDB-Datenbank herstellen können müssen Sie die folgenden Komponenten installieren: 

- Datenverwaltungsgateway 2.0 oder höher auf dem Computer, der die Datenbank hostet oder auf einem separaten Computer, um zu vermeiden, Anspruch auf Ressource mit der Datenbank. Datenverwaltungsgateway ist eine Software, die auf lokale Datenquellen und Cloud Services in eine sichere und verwaltete Möglichkeit besteht. [Datenverwaltungsgateway](data-factory-data-management-gateway.md) finden Sie im Artikel Details des Datenverwaltungsgateways.
  
    Wenn Sie das Gateway installiert haben, wird einen Microsoft MongoDB ODBC-Treiber zum Verbinden mit MongoDB automatisch installiert. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit, eine Verkaufspipeline zu erstellen, die Daten aus einer Datenbank MongoDB an die Empfänger unterstützten Datenspeicher kopiert besteht darin, den Assistenten zum Kopieren von Daten verwenden. Finden Sie unter [Lernprogramm: Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) für eine schnelle Exemplarische Vorgehensweise zum Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von Daten. 

Im folgende Beispiel enthält die Stichprobe JSON-Definitionen, die Sie verwenden können, erstellen Sie eine Verkaufspipeline mithilfe von [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Wird gezeigt, wie Daten aus Datenbank MongoDB in Azure BLOB-Speicher zu kopieren. Daten können jedoch an die senken angegebener [So](data-factory-data-movement-activities.md#supported-data-stores) verwenden die Aktivität kopieren in Azure Data Factory kopiert werden.

## <a name="sample-copy-data-from-mongodb-to-azure-blob"></a>Beispiel: Kopieren Sie Daten aus MongoDB in Azure Blob
Dieses Beispiel zeigt, wie Sie Daten aus einer lokalen MongoDB-Datenbank in einer Azure BLOB-Speicher zu kopieren. Daten kann jedoch kopierten **direkt** an die senken angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores) die Aktivität kopieren in Azure Data Factory verwenden.  
 
Im Beispiel weist die folgenden Daten Factory Elemente:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesMongoDb](#linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [MongoDbCollection](#dataset-type-properties).
4.  Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [MongoDbSource](#copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Im Beispiel kopiert Daten aus einem Abfrageergebnis in MongoDB Datenbank in eine Blob stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben. 

Richten Sie als ersten Schritt das datenverwaltungsgateway gemäß den Anweisungen im [Datenverwaltungsgateway](data-factory-data-management-gateway.md) -Artikel aus. 

**MongoDB verknüpft-Dienst**

    { 
        "name": "OnPremisesMongoDbLinkedService", 
        "properties": 
        { 
            "type": "OnPremisesMongoDb", 
            "typeProperties": 
            { 
                "authenticationType": "<Basic or Anonymous>", 
                "server": "< The IP address or host name of the MongoDB server >",  
                "port": "<The number of the TCP port that the MongoDB server uses to listen for client connections.>", 
                "username": "<username>", 
                "password": "<password>",
               "authSource": "< The database that you want to use to check your credentials for authentication. >",
                "databaseName": "<database name>",
                "gatewayName": "<mygateway>"
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

**Eingabe MongoDB-dataset** Festlegen von "externe": "true" werden dem Daten Factory-Dienst informiert, dass die Tabelle externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.
    
    {
         "name":  "MongoDbInputDataset",
        "properties": { 
            "type": "MongoDbCollection", 
            "linkedServiceName": "OnPremisesMongoDbLinkedService", 
            "typeProperties": { 
                "collectionName": "<Collection name>"   
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
                "folderPath": "mycontainer/frommongodb/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                            "format": "%M"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%d"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%H"
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

Der Verkaufspipeline enthält eine Kopie Aktivitäten, die darauf konfiguriert ist, verwenden Sie die oben angegebenen Eingabe und Datasets ausgeben und stündlich ausführen geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **MongoDbSource** festgelegt ist, und Typ der **Empfänger** auf **BlobSink**festgelegt ist. Die SQL-Abfrage für die Eigenschaft **Abfrage** angegebenen wählt die Daten in der letzten Stunde zu kopieren.
    
    {
        "name": "CopyMongoDBToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "MongoDbSource",
                            "query": "$$Text.Format('select * from  MyTable where LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MongoDbInputDataset"
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
                    "name": "MongoDBToAzureBlob"
                }
            ],
            "start": "2016-06-01T18:00:00Z",
            "end": "2016-06-01T19:00:00Z"
        }
    }


## <a name="linked-service-properties"></a>Verknüpfte Diensteigenschaften

Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die speziell für Dienst **OnPremisesMongoDB** verknüpft.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| Typ | Die Eigenschaft muss auf festgelegt sein: **OnPremisesMongoDb** | Ja |
| Server | IP-Adresse oder Host Name des Servers MongoDB. | Ja | 
| Port | TCP-, die vom Server MongoDB, Clientverbindungen zu überwachen. | Optional, Standardwert: 27017 |
| authenticationType | Einfacher oder anonymer. | Ja | 
| Benutzername | Benutzerkonto MongoDB Zugriff auf. | Ja (Wenn Standardauthentifizierung verwendet wird).|
| Kennwort | Kennwort für den Benutzer. |   Ja (Wenn Standardauthentifizierung verwendet wird). | 
| authSource | Name der MongoDB Datenbank, die Sie verwenden, um Ihre Anmeldeinformationen für die Authentifizierung prüfen möchten. | Optional (Wenn Standardauthentifizierung verwendet wird). Standard: verwendet das Admin-Konto und die Datenbank mithilfe der Eigenschaft Datenbankname angegeben. |  
| Datenbankname | Name der MongoDB Datenbank, die Sie zugreifen möchten. | Ja |
| gatewayName | Name des Gateways, das die Datenspeicher greift auf. | Ja | 
| encryptedCredential | Anmeldeinformationen, die von einem Gateway verschlüsselt werden. | Optional |


Ausführliche Informationen zum Festlegen von Anmeldeinformationen für eine lokale MongoDB-Datenquelle finden Sie unter [Festlegen der Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="dataset-type-properties"></a>Typ Datensatzeigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung finden Sie im Artikel [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinie eines Datasets JSON ähneln für alle Dataset-Typen (Azure SQL Azure Blob, Azure Table usw..).

Im Abschnitt **TypeProperties** unterscheidet sich für jede Art von Dataset und enthält Informationen über den Speicherort der Daten im Datenspeicher. Im Abschnitt TypeProperties für Dataset vom Typ **MongoDbCollection** weist die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| collectionName | Name der Sammlung in MongoDB Datenbank. | Ja | 

## <a name="copy-activity-type-properties"></a>Kopieren Sie die Schrifteigenschaften Aktivität

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten verfügbar sind finden Sie unter Artikel [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinie stehen für alle Arten von Aktivitäten. 

Im Abschnitt **TypeProperties** der Aktivität verfügbaren Eigenschaften variieren andererseits bei jeder Aktivität. Aktivitäten, kopieren variieren je nach den Typen von Datenquellen und senken.

Wenn die Quelle vom Typ **MongoDbSource** ist stehen die folgenden Eigenschaften im Abschnitt TypeProperties:

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| Abfrage | Verwenden Sie die benutzerdefinierte Abfrage, um Daten zu lesen. | SQL-92-Abfragezeichenfolge. Beispiel: Wählen Sie * from MyTable. | Nein (sofern **CollectionName** **Datasets** angegeben ist) | 

## <a name="schema-by-data-factory"></a>Schema von Daten Factory
Azure Data Factory-Dienst leitet Schema aus einer Sammlung MongoDB mithilfe der neuesten 100 Dokumente in der Auflistung ab. Wenn diese 100 Dokumente vollständigen Schema nicht enthalten, können einige Spalten während der Kopiervorgang ignoriert werden. 

## <a name="type-mapping-for-mongodb"></a>Für MongoDB-Zuordnung

Wie in den [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) Artikel erwähnt, führt kopieren Aktivität automatische Konvertieren des Datentyps von Arten von Datenquellen zu ignorieren Typen mit dem folgenden Ansatz für-Schritt 2:

1. Konvertieren von Datentypen systemeigenen Quelle in .NET Typ
2. Konvertieren von .NET Typ in einer systemeigenen Empfänger Typ

Beim Verschieben von Daten in MongoDB werden die folgenden Zuordnungen in .NET Dateitypen MongoDB Arten verwendet.

| MongoDB Typ | .NET Framework-Typ |
| ------------------- | ------------------- | 
| Binäre | Byte] |
| Boolesch | Boolesch |
| Datum | "DateTime" |
| NumberDouble | Double |
| NumberInt | Int32 |
| NumberLong | Int64 |
| ObjectID | Zeichenfolge |
| Zeichenfolge | Zeichenfolge |
| UUID | GUID | 
| Objekt | Renormalisiert auf reduzieren Spalten mit "_" als Trennzeichen für geschachtelte |

> [AZURE.NOTE]  
> Informationen zur Unterstützung von Arrays virtuellen Tabellen mit benutzerfreundlicheren Sie [Unterstützung für komplexe Typen verwenden virtueller Tabellen](#support-for-complex-types-using-virtual-tables) Abschnitt weiter unten. 

Die folgenden MongoDB Datentypen werden derzeit nicht unterstützt: DBPointer, JavaScript-, Max/Min wichtiger, reguläre Ausdrücke, Symbol, Zeitstempel undefiniert

## <a name="support-for-complex-types-using-virtual-tables"></a>Unterstützung für komplexe Typen verwenden virtueller Tabellen
Azure Data Factory verwendet einen integrierten ODBC-Treiber zum Herstellen einer Verbindung mit und kopieren Sie Daten aus Ihrer Datenbank MongoDB. Für komplexe Typen wie etwa Arrays oder Objekte mit unterschiedlichen Typen in Dokumenten normalisiert erneut der Treiber Daten in den entsprechenden virtuelle Tabellen. Eine Tabelle solche Spalten enthält, wird der Treiber insbesondere in den folgenden virtuellen Tabellen generiert:

-   Eine **Basis Tabelle**, die die gleichen Daten wie realen Tischs eine Ausnahme bilden jedoch die komplexe Typspalten enthält. Die base Tabelle verwendet denselben Namen als realen Tischs, die ihn darstellt.
-   Eine **virtuelle Tabelle** für jede Spalte komplexen Typ, die erweitert die geschachtelten Daten. Virtuellen Tabellen sind benannte unter Verwendung des realen Tischs, ein Trennzeichen "_" und den Namen der Matrix oder des Objekts.

Virtuelle Tabellen beziehen sich auf die Daten in der eigentlichen Tabelle, den Zugriff auf die denormalisierten Daten-Treiber aktivieren. Siehe Abschnitt "Beispiel" unter Details. Sie können den Inhalt der MongoDB Arrays zugreifen, indem Sie Abfragen und die Teilnahme an der virtuellen Tabellen.

Sie können mithilfe des [Assistenten zum Kopieren von](data-factory-data-movement-activities.md#data-factory-copy-wizard) intuitiv die Liste der Tabellen in MongoDB Datenbank einschließlich virtuellen Tabellen anzuzeigen und Anzeigen einer Vorschau die Daten in der. Sie können auch erstellen eine Abfrage in den Assistenten zum Kopieren und überprüfen, um das Ergebnis anzuzeigen.

### <a name="example"></a>Beispiel

"ExampleTable" unter beträgt beispielsweise eine MongoDB-Tabelle mit einer Spalte mit der ein Array von Objekten in jeder Zelle – Rechnungen sowie eine Spalte mit der ein Array von skalaren Typen – Bewertungen aus. 

_id | Kundenname | Rechnungen | Dienstalter | Bewertungen
--- | ------------- | -------- | ------------- | -------
1111 | ABC | [{Invoice_id: "123" Element: "Toaster", den Preis: "456" Rabatt: "0,2"}, {Invoice_id: "124" Element: "Ofen", den Preis: "1235", Rabatt: "0,2"}] | Silber | [5,6]
2222 | XYZ | [{Invoice_id: "135" Element: "Kühlschrank", den Preis: "12543" Rabatt: "0,0"}] | Gold | [1,2]

Der Treiber würde mehrere virtuelle Tabellen zum Darstellen dieser einzelne Tabelle generieren. Die erste virtuelle Tabelle ist die Basis-Tabelle mit dem Namen "ExampleTable" abgebildet. Die base Tabelle enthält alle Daten der ursprünglichen Tabelle, aber die Daten aus den Arrays fehlt und ist in den virtuellen Tabellen erweitert.

_id | Kundenname | Dienstalter
--- | ------------- | -------------
1111 | ABC | Silber
2222 | XYZ | Gold

Die folgende Tabelle enthält die virtuellen Tabellen, die die ursprüngliche Arrays im Beispiel darstellen. In diesen Tabellen enthalten Folgendes: 

- Ein Verweis auf die ursprüngliche primären Schlüsselspalte entspricht, auf die Zeile des ursprünglichen Arrays (über die Spalte _id) zurück
- Angabe der die Position der Daten innerhalb des ursprünglichen Arrays 
- Die erweiterten Daten für jedes Element innerhalb des Arrays

Tabelle "ExampleTable_Invoices":

_id | ExampleTable_Invoices_dim1_idx | invoice_id | Element | Kurs | Rabatt
--- | -------------- | ---------- | ---- | ----- | --------
1111 | 0 | 123 | Toaster | 456 | 0,2
1111 | 1 | 124 | Ofen | 1235 | 0,2
2222 | 0 | 135 | Kühlschrank | 12543 | 0.0

Tabelle "ExampleTable_Ratings":

_id | ExampleTable_Ratings_dim1_idx | ExampleTable_Ratings
--- | ------------- | -------------
1111 | 0 | 5
1111 | 1 | 6
2222 | 0 | 1
2222 | 1 | 2

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Leistung und optimieren  
Finden Sie unter [Kopieren Aktivität Leistung und Optimieren von Leitfaden für](data-factory-copy-activity-performance.md) die Leistung Einfluss der Daten Bewegung (Kopieren Aktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren sie wichtige Faktoren lernen.

## <a name="next-steps"></a>Nächste Schritte
Finden Sie unter [Verschieben von Daten zwischen lokalen und Cloud](data-factory-move-data-between-onprem-and-cloud.md) -Artikel für eine schrittweise Anleitung zum Erstellen einer Verkaufspipeline Daten, die Daten aus einem lokalen Datenspeicher in einen Azure Datenspeicher verschoben. 

