<properties 
    pageTitle="Verschieben von Daten aus DB2 | Factory Azure-Daten" 
    description="Weitere Informationen über das Verschieben von Daten aus Azure Data Factory mit DB2-Datenbank" 
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
    ms.date="09/06/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-db2-using-azure-data-factory"></a>Verschieben von Daten aus DB2 Azure Data Factory verwenden
In diesem Artikel wird beschrieben, wie Sie die Aktivität kopieren in einer Factory Azure-Daten verwenden können, zum Verschieben von Daten aus DB2 an eine andere Daten gespeichert. In diesem Artikel wird im Artikel [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) , die eine allgemeine Übersicht über das Verschieben von Daten mit Aktivität kopieren und unterstützten Data Store Kombinationen bietet erstellt.

Daten Factory unterstützt das Herstellen einer Verbindung mit einer lokalen DB2 Quellen das Datenverwaltungsgateway verwenden. Finden Sie unter [Verschieben von Daten zwischen lokalen Orten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) Artikel lernen Datenverwaltungsgateway und eine schrittweise Anleitung zum Einrichten des Gateways. 

> [AZURE.NOTE]
Verwenden des Gateways Verbindung zu DB2, auch wenn es in Azure IaaS virtuellen Computern gehostet wird. Wenn Sie versuchen, eine Verbindung mit einer Instanz von DB2 in der Cloud gehostet wird, können Sie auch die Gatewayinstanz in die IaaS VM installieren.

Daten Factory unterstützt derzeit nur Verschieben von Daten aus DB2 für andere Datenspeicher, nicht aber von anderen Datenspeicher zu DB2. 

## <a name="installation"></a>Installation 

Das Datenverwaltungsgateway bietet einen integrierten DB2-Treiber, der das Folgendes unterstützt: 

- SQLAM 9 / 10 / 11
- DB2 für LUW (Linux, Unix, Windows)
- DB2 für Z/OS und DB2 für ich (als QuickInfos / 400)

Daher müssen Sie nicht mehr die Treiber beim Kopieren von Daten aus DB2 manuell zu installieren.

> [AZURE.NOTE] Finden Sie unter [Behandeln von Gateway Probleme](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) Tipps zur Behebung von Verbindung/Gateway zusammenhängende Probleme. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit, eine Verkaufspipeline zu erstellen, die Daten aus einer DB2-Datenbank kopiert besteht darin, den Assistenten zum Kopieren von Daten verwenden. Finden Sie unter [Lernprogramm: Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) für eine schnelle Exemplarische Vorgehensweise zum Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von Daten. 

In den folgenden Beispielen bieten Stichprobe JSON-Definitionen, mit denen Sie eine Verkaufspipeline mithilfe von [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)erstellen. Wird gezeigt, wie Daten aus DB2-Datenbank und Azure BLOB-Speicher kopieren. Daten können jedoch an die senken angegebener [So](data-factory-data-movement-activities.md#supported-data-stores) verwenden die Aktivität kopieren in Azure Data Factory kopiert werden.

## <a name="sample-copy-data-from-db2-to-azure-blob"></a>Beispiel: Kopieren Sie Daten aus DB2 in Azure Blob

Dieses Beispiel zeigt, wie Sie Daten aus einer lokalen DB2-Datenbank in einer Azure BLOB-Speicher kopieren. Daten kann jedoch kopierten **direkt** an die senken angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores) die Aktivität kopieren in Azure Data Factory verwenden.  
 
Im Beispiel weist die folgenden Daten Factory Elemente:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesDb2](data-factory-onprem-db2-connector.md#db2-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3.  Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [RelationalTable](data-factory-onprem-db2-connector.md#db2-dataset-type-properties).
4.  Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
5.  Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [RelationalSource](data-factory-onprem-db2-connector.md#db2-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet. 

Im Beispiel kopiert Daten aus einem Abfrageergebnis in einer DB2-Datenbank in einer Azure Blob stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben. 

Als ersten Schritt installieren und Konfigurieren eines datenverwaltungsgateways. Hinweise finden Sie im Artikel [Verschieben von Daten zwischen lokalen Orten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) .

**Verknüpfte DB2-Dienst:**

    {
        "name": "OnPremDb2LinkedService",
        "properties": {
            "type": "OnPremisesDb2",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
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

**Eingabe DB2-Dataset:**

Das Beispiel wird vorausgesetzt, Sie eine Tabelle erstellt haben "MyTable" in DB2 und eine Spalte namens "Timestamp" für die Reihe Uhrzeitdaten enthält.

Festlegen von "externe": WAHR informiert dem Daten Factory-Dienst, dass dieses Dataset externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird. Beachten Sie, dass der **Typ** **RelationalTable**festgelegt ist. 

    {
        "name": "Db2DataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremDb2LinkedService",
            "typeProperties": {},
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
        "name": "AzureBlobDb2DataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

**Pipeline mit Aktivität kopieren:**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass die Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **RelationalSource** festgelegt ist, und Typ der **Empfänger** auf **BlobSink**festgelegt ist. Die SQL-Abfrage für die Eigenschaft **Abfrage** angegeben, werden die Daten aus der Tabelle Orders markiert.

    {
        "name": "CopyDb2ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from \"Orders\""
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Db2DataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobDb2DataSet"
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
                    "name": "Db2ToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="db2-linked-service-properties"></a>Eigenschaften der verknüpft DB2-Dienst

Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die speziell für verknüpfte DB2-Dienst an. 

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| Typ | Die Eigenschaft muss auf festgelegt sein: **OnPremisesDB2** | Ja |
| Server | Name des DB2-Servers. | Ja |
| Datenbank | Name der DB2-Datenbank. | Ja |
| Schema | Name des Schemas in der Datenbank. Der Name des Schemas beachtet werden. | Nein |
| authenticationType | Art der Authentifizierung, die zum Verbinden mit der DB2-Datenbank. Mögliche Werte sind: anonym, Basic und Windows. | Ja |
| Benutzername | Geben Sie Benutzernamen an, wenn Sie Basic oder Windows-Authentifizierung verwenden. | Nein |
| Kennwort | Geben Sie das Kennwort für das Benutzerkonto, das Sie für den Benutzernamen angegeben haben. | Nein |
| gatewayName | Name des Gateways, das der Daten Factory-Dienst verwendet werden sollen, die Verbindung zur lokalen DB2-Datenbank. | Ja |

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für eine lokale DB2-Datenquelle finden Sie unter [Festlegen der Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) . 


## <a name="db2-dataset-type-properties"></a>DB2 Typ Datensatzeigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung finden Sie im Artikel [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinie eines Datasets JSON ähneln für alle Dataset-Typen (Azure SQL Azure Blob, Azure Table usw..).

Im Abschnitt TypeProperties unterscheidet sich für jede Art von Dataset und enthält Informationen über den Speicherort der Daten im Datenspeicher. Im Abschnitt TypeProperties für Dataset vom Typ RelationalTable (enthält DB2 Dataset) weist die folgenden Eigenschaften.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| Tabellenname | Name der Tabelle in der DB2-Datenbank-Instanz, der auf Verknüpfte Dienst verweist. Der Tabellenname beachtet werden. | Nein (wenn die **Abfrage** mit **RelationalSource** angegeben ist) |

## <a name="db2-copy-activity-type-properties"></a>DB2 kopieren Aktivität Schrifteigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten verfügbar sind finden Sie unter Artikel [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinien zur Verfügung stehen für alle Arten von Aktivitäten. 

Im Abschnitt TypeProperties der Aktivität verfügbaren Eigenschaften variieren andererseits bei jeder Aktivität. Aktivitäten, kopieren variieren je nach den Typen von Datenquellen und senken.

Aktivitäten, kopieren Wenn Quelle vom Typ **RelationalSource** ist (enthält DB2) stehen die folgenden Eigenschaften im Abschnitt TypeProperties:


| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------- | -------------- |
| Abfrage | Verwenden Sie die benutzerdefinierte Abfrage, um Daten zu lesen. | SQL-Abfragezeichenfolge. Beispiel: `"query": "select * from "MySchema"."MyTable""`. | Nein (sofern **Tabellenname** **Datasets** angegeben ist)|

> [AZURE.NOTE] Schema und Tabellennamen Groß-/Kleinschreibung. Setzen Sie die Namen in "" (doppelte Anführungszeichen) in der Abfrage.  

**Beispiel:**

    "query": "select * from "DB2ADMIN"."Customers""


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-db2"></a>Für DB2-Zuordnung
Wie in den [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) Artikel erwähnt, führt die Aktivität kopieren automatische Konvertieren des Datentyps von Arten von Datenquellen zu ignorieren Typen mit dem folgenden Ansatz für-Schritt 2:

1. Konvertieren von Datentypen systemeigenen Quelle in .NET Typ
2. Konvertieren von .NET Typ in einer systemeigenen Empfänger Typ

Beim Verschieben von Daten in DB2, werden die folgenden Zuordnungen in .NET Typ von DB2 Typ verwendet.

DB2-Datenbank-Datentyp | .NET Framework-Typ 
----------------- | ------------------- 
SmallInt | Int16
Ganze Zahl | Int32
BigInt | Int64
Real | Einzelne
Double | Double
Frei verschieben | Double
Dezimalzahl | Dezimalzahl
DecimalFloat | Dezimalzahl
Numerische | Dezimalzahl
Datum | "DateTime"
Zeit | TimeSpan
Zeitstempel | "DateTime"
XML | Byte]
Zeichen | Zeichenfolge
VarChar | Zeichenfolge
LongVarChar | Zeichenfolge
DB2DynArray | Zeichenfolge
Binäre | Byte]
VarBinary | Byte]
LongVarBinary | Byte]
Grafik | Zeichenfolge
VarGraphic | Zeichenfolge
LongVarGraphic | Zeichenfolge
CLOB | Zeichenfolge
BLOB | Byte]
DbClob | Zeichenfolge
SmallInt | Int16
Ganze Zahl | Int32
BigInt | Int64
Real | Einzelne
Double | Double
Frei verschieben | Double
Dezimalzahl | Dezimalzahl
DecimalFloat | Dezimalzahl
Numerische | Dezimalzahl
Datum | "DateTime"
Zeit | TimeSpan
Zeitstempel | "DateTime"
XML | Byte]
Zeichen | Zeichenfolge


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Leistung und optimieren  
Finden Sie unter [Kopieren Aktivität Leistung und Optimieren von Leitfaden für](data-factory-copy-activity-performance.md) die Leistung Einfluss der Daten Bewegung (Kopieren Aktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren sie wichtige Faktoren lernen.


