<properties 
    pageTitle="Verschieben von Daten aus Teradata | Factory Azure-Daten" 
    description="Informationen Sie zu Teradata-Connector für den Daten Factory-Dienst, in der Sie die Daten aus Teradata-Datenbank verschieben können" 
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

# <a name="move-data-from-teradata-using-azure-data-factory"></a>Verschieben von Daten aus Azure Data Factory mit Teradata

In diesem Artikel wird beschrieben, wie Sie die Aktivität kopieren in einer Factory Azure-Daten verwenden können, zum Verschieben von Daten aus Teradata an eine andere Daten gespeichert. In diesem Artikel wird im Artikel [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) , die eine allgemeine Übersicht über das Verschieben von Daten mit Aktivität kopieren und unterstützten Data Store Kombinationen bietet erstellt.

Daten Factory unterstützt das Herstellen einer Verbindung mit einer lokalen Teradata Quellen über das Datenverwaltungsgateway. Finden Sie unter [Verschieben von Daten zwischen lokalen Orten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) Artikel lernen Datenverwaltungsgateway und eine schrittweise Anleitung zum Einrichten des Gateways. 

> [AZURE.NOTE]
Gateway ist erforderlich, auch wenn der Teradata eine Neuerung Azure gehostet wird. Sie können das Gateway auf die gleiche IaaS VM als Datenspeicher oder auf einen anderen virtuellen Computer installieren, solange des Gateways auf die Datenbank zugreifen kann. 

Daten Factory unterstützt nur Verschieben von Daten aus Teradata für andere Datenspeicher, nicht aber von anderen Datenspeicher zu Teradata.

## <a name="installation"></a>Installation 

Für das Datenverwaltungsgateway ist die Verbindung zu den Teradata-Datenbank herstellen müssen Sie den [Datenanbieter für .NET für Teradata](http://go.microsoft.com/fwlink/?LinkId=278886) auf demselben System wie das Datenverwaltungsgateway zu installieren.

> [AZURE.NOTE] Finden Sie unter [Behandeln von Gateway Probleme](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) Tipps zur Behebung von Verbindung/Gateway zusammenhängende Probleme. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit, eine Verkaufspipeline zu erstellen, die Daten aus Teradata an die Empfänger unterstützten Datenspeicher kopiert besteht darin, den Assistenten zum Kopieren von Daten verwenden. Finden Sie unter [Lernprogramm: Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) für eine schnelle Exemplarische Vorgehensweise zum Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von Daten. 

Im folgende Beispiel enthält die Stichprobe JSON-Definitionen, die Sie verwenden können, erstellen Sie eine Verkaufspipeline mithilfe von [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Wird gezeigt, wie Daten aus Teradata in Azure BLOB-Speicher zu kopieren. Daten können jedoch an die senken angegebener [So](data-factory-data-movement-activities.md#supported-data-stores) verwenden die Aktivität kopieren in Azure Data Factory kopiert werden.   

### <a name="sample-copy-data-from-teradata-to-azure-blob"></a>Beispiel: Kopieren Sie Daten aus Teradata in Azure Blob

Dieses Beispiel zeigt, wie Sie Daten aus einer Teradata-Datenbank in einer Azure BLOB-Speicher kopieren. Daten kann jedoch kopierten **direkt** an die senken angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores) die Aktivität kopieren in Azure Data Factory verwenden.  
 
Im Beispiel weist die folgenden Daten Factory Elemente:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesTeradata](data-factory-onprem-teradata-connector.md#teradata-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [RelationalTable](data-factory-onprem-teradata-connector.md#teradata-dataset-type-properties).
4.  Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
4.  Der [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [RelationalSource](data-factory-onprem-teradata-connector.md#teradata-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Im Beispiel kopiert Daten aus einem Abfrageergebnis in Teradata-Datenbank in eine Blob stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben. 

Richten Sie als ersten Schritt das datenverwaltungsgateway ein. Die Hinweise finden Sie im Artikel [Verschieben von Daten zwischen lokalen Orten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) .

**Verknüpfte Teradata-Dienst:**

    {
        "name": "OnPremTeradataLinkedService",
        "properties": {
            "type": "OnPremisesTeradata",
            "typeProperties": {
                "server": "<server>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }

**Azure Blob-Speicher verknüpft Dienst an:**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorageLinkedService",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
            }
        }
    }


**Eingabe Teradata-Dataset:**

Das Beispiel wird vorausgesetzt, Sie eine Tabelle erstellt haben "MyTable" in Teradata und eine Spalte namens "Timestamp" für die Reihe Uhrzeitdaten enthält.

Festlegen von "externe": WAHR informiert dem Daten Factory-Dienst, dass die Tabelle externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.

    {
        "name": "TeradataDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremTeradataLinkedService",
            "typeProperties": {
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
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
        "name": "AzureBlobTeradataDataSet",
        "properties": {
            "published": false,
            "location": {
                "type": "AzureBlobLocation",
                "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                ],
                "linkedServiceName": "AzureStorageLinkedService"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Pipeline mit Aktivität kopieren:**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass die Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **RelationalSource** festgelegt ist, und Typ der **Empfänger** auf **BlobSink**festgelegt ist. Die SQL-Abfrage für die Eigenschaft **Abfrage** angegebenen wählt die Daten in der letzten Stunde zu kopieren.

    {
        "name": "CopyTeradataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "TeradataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobTeradataDataSet"
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
                    "name": "TeradataToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z",
            "isPaused": false
        }
    }


## <a name="teradata-linked-service-properties"></a>Eigenschaften der verknüpft Teradata-Dienst

Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die speziell für verknüpfte Teradata-Dienst an. 

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Die Eigenschaft muss auf festgelegt sein: **OnPremisesTeradata** | Ja
Server | Name des Teradata-Servers. | Ja
authenticationType | Art der Authentifizierung verwendet, um zu der Teradata-Datenbank herstellen. Mögliche Werte sind: anonym, Basic und Windows. | Ja
Benutzername | Geben Sie Benutzernamen an, wenn Sie Basic oder Windows-Authentifizierung verwenden. | Nein 
Kennwort | Geben Sie das Kennwort für das Benutzerkonto, das Sie für den Benutzernamen angegeben haben. | Nein 
gatewayName | Name des Gateways, das der Daten Factory-Dienst verwendet werden sollen, die Verbindung zu der lokalen Teradata-Datenbank herstellen. | Ja

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für eine lokale Teradata-Datenquelle finden Sie unter [Festlegen der Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="teradata-dataset-type-properties"></a>Teradata Typ Datensatzeigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung finden Sie im Artikel [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinie eines Datasets JSON ähneln für alle Dataset-Typen (Azure SQL Azure Blob, Azure Table usw..).

Im Abschnitt **TypeProperties** unterscheidet sich für jede Art von Dataset und enthält Informationen über den Speicherort der Daten im Datenspeicher. Derzeit keine Typeigenschaften für das Dataset Teradata unterstützt vorhanden sind. 


## <a name="teradata-copy-activity-type-properties"></a>Teradata kopieren Aktivität Schrifteigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten verfügbar sind finden Sie unter Artikel [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien zur Verfügung stehen für alle Arten von Aktivitäten. 

Im Abschnitt TypeProperties der Aktivität verfügbaren Eigenschaften variieren andererseits bei jeder Aktivität. Aktivitäten, kopieren variieren je nach den Typen von Datenquellen und senken.

Wenn Quelle vom Typ **RelationalSource** ist (enthält Teradata) stehen die folgenden Eigenschaften im Abschnitt **TypeProperties** :

Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich
-------- | ----------- | -------------- | --------
Abfrage | Verwenden Sie die benutzerdefinierte Abfrage, um Daten zu lesen. | SQL-Abfragezeichenfolge. Beispiel: Wählen Sie * from MyTable. | Ja

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-teradata"></a>Für Teradata-Zuordnung

Wie in den [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) Artikel erwähnt, führt die Aktivität kopieren automatische Konvertieren des Datentyps von Arten von Datenquellen zu ignorieren Typen mit dem folgenden Ansatz für-Schritt 2:

1. Konvertieren von Datentypen systemeigenen Quelle in .NET Typ
2. Konvertieren von .NET Typ in einer systemeigenen Empfänger Typ

Beim Verschieben von Daten in Teradata, werden die folgenden Zuordnungen in .NET Typ von Teradata-Typ verwendet.

Teradata-Datenbank-Datentyp | .NET Framework-Typ
----------------- | ---------------------------
Zeichen | Zeichenfolge
CLOB | Zeichenfolge
Grafik | Zeichenfolge
VarChar | Zeichenfolge
VarGraphic | Zeichenfolge
BLOB | Byte]
Byte-Zeichen | Byte]
VarByte | Byte]
BigInt | Int64
ByteInt | Int16
Dezimalzahl | Dezimalzahl
Double | Double
Ganze Zahl | Int32
Zahl | Double
SmallInt | Int16
Datum | "DateTime"
Zeit | TimeSpan
Zeit mit (Bildschirmdruck) | Zeichenfolge
Zeitstempel | "DateTime"
Timestamp mit (Bildschirmdruck) | DateTimeOffset
Intervall Tag | TimeSpan
Intervall Tag Stunde | TimeSpan
Intervall Tag in Minute um | TimeSpan
Intervall Tag zweiten | TimeSpan
Intervall Stunde | TimeSpan
Intervall Stunde, Minute | TimeSpan
Intervall Stunde zweiten | TimeSpan
Intervall Minute | TimeSpan
Intervall Minute und Sekunde | TimeSpan
Intervall zweiten | TimeSpan
Intervall Jahr | Zeichenfolge
Intervall Jahr zum Monat | Zeichenfolge
Intervall Monat | Zeichenfolge
Period(Date) | Zeichenfolge
Period(Time) | Zeichenfolge
Punkt (Zeit und Zeitzone) | Zeichenfolge
Period(timestamp) | Zeichenfolge
Punkt (Zeitstempel mit Zeitzone) | Zeichenfolge
XML | Zeichenfolge

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Leistung und optimieren  
Finden Sie unter [Kopieren Aktivität Leistung und Optimieren von Leitfaden für](data-factory-copy-activity-performance.md) die Leistung Einfluss der Daten Bewegung (Kopieren Aktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren sie wichtige Faktoren lernen.