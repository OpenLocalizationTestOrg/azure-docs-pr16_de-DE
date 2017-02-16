<properties 
    pageTitle="Verschieben von Daten aus PostgreSQL | Factory Azure-Daten" 
    description="Informationen Sie zum Verschieben von Daten aus Azure Data Factory mit PostgreSQL-Datenbank." 
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

# <a name="move-data-from-postgresql-using-azure-data-factory"></a>Verschieben von Daten aus PostgreSQL Azure Data Factory verwenden

In diesem Artikel wird erläutert, wie Sie die Aktivität kopieren in einer Factory Azure-Daten zum Verschieben von Daten aus PostgreSQL zu einem anderen Datenspeicher verwenden können. In diesem Artikel wird im Artikel [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) , die eine allgemeine Übersicht über das Verschieben von Daten mit Aktivität kopieren und unterstützten Data Store Kombinationen bietet erstellt.

Factory Datendienst unterstützt das Herstellen einer Verbindung mit einer lokalen PostgreSQL-Datenquellen verwenden das Datenverwaltungsgateway. Finden Sie unter [Verschieben von Daten zwischen lokalen Orten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) Artikel lernen Datenverwaltungsgateway und eine schrittweise Anleitung zum Einrichten des Gateways. 

> [AZURE.NOTE]
> Gateway ist erforderlich, auch wenn die PostgreSQL-Datenbank in eine Neuerung Azure gehostet wird. Sie können das Gateway auf die gleiche IaaS VM als Datenspeicher oder auf einen anderen virtuellen Computer installieren, solange des Gateways auf die Datenbank zugreifen kann. 

Daten Factory unterstützt nur Verschieben von Daten aus PostgreSQL für andere Datenspeicher, nicht aber von anderen Datenspeicher zu PostgreSQL.

## <a name="installation"></a>Installation 

Für das Datenverwaltungsgateway ist die Verbindung zu den PostgreSQL-Datenbank herstellen müssen Sie den [Ngpsql-Datenanbieter für PostgreSQL](http://go.microsoft.com/fwlink/?linkid=282716) auf demselben System wie das Datenverwaltungsgateway zu installieren.

> [AZURE.NOTE] Finden Sie unter [Behandeln von Gateway Probleme](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) Tipps zur Behebung von Verbindung/Gateway zusammenhängende Probleme. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit, eine Verkaufspipeline zu erstellen, die Daten aus einer PostgreSQL-Datenbank an die Empfänger unterstützten Datenspeicher kopiert besteht darin, den Assistenten zum Kopieren von Daten verwenden. Finden Sie unter [Lernprogramm: Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) für eine schnelle Exemplarische Vorgehensweise zum Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von Daten. 

Im folgende Beispiel enthält die Stichprobe JSON-Definitionen, die Sie verwenden können, erstellen Sie eine Verkaufspipeline mithilfe von [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Wird gezeigt, wie Daten aus PostgreSQL-Datenbank in Azure BLOB-Speicher zu kopieren. Daten können jedoch an die senken angegebener [So](data-factory-data-movement-activities.md#supported-data-stores) verwenden die Aktivität kopieren in Azure Data Factory kopiert werden.   

## <a name="sample-copy-data-from-postgresql-to-azure-blob"></a>Beispiel: Kopieren Sie Daten aus PostgreSQL in Azure Blob

Dieses Beispiel zeigt, wie Sie Daten aus einer PostgreSQL-Datenbank in einer Azure BLOB-Speicher kopieren. Daten kann jedoch kopierten **direkt** an die senken angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores) die Aktivität kopieren in Azure Data Factory verwenden.  
 
Im Beispiel weist die folgenden Daten Factory Elemente:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#postgresql-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [RelationalTable](data-factory-onprem-postgresql-connector.md#postgresql-dataset-type-properties).
4.  Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Der [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [RelationalSource](data-factory-onprem-postgresql-connector.md#postgresql-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet. 

Im Beispiel kopiert Daten aus einem Abfrageergebnis in PostgreSQL-Datenbank in eine Blob stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben. 

Richten Sie als ersten Schritt das datenverwaltungsgateway ein. Die Hinweise finden Sie im Artikel [Verschieben von Daten zwischen lokalen Orten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) .

**Verknüpfte PostgreSQL-Dienst:**

    {
        "name": "OnPremPostgreSqlLinkedService",
        "properties": {
            "type": "OnPremisesPostgreSql",
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
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
      }
    }

**Eingabe PostgreSQL-Dataset:**

Das Beispiel wird vorausgesetzt, Sie eine Tabelle erstellt haben "MyTable" in den PostgreSQL und eine Spalte namens "Timestamp" für die Reihe Uhrzeitdaten enthält.

Festlegen von "externe": WAHR informiert dem Daten Factory-Dienst, dass das Dataset externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.

    {
        "name": "PostgreSqlDataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremPostgreSqlLinkedService",
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

Jede Stunde Daten in einer neuen Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Der Ordnername Pfad und den Dateinamen für die Blob werden dynamisch ausgewertet, basierend auf der Startzeit des Segments, die verarbeitet wird. Der Pfad des verwendet Jahr, Monat, Tag und Stunden Teile der Startzeit.

    {
        "name": "AzureBlobPostgreSqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/postgresql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


**Kopieren Sie die Aktivität:**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass die Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **RelationalSource** festgelegt ist, und Typ der **Empfänger** auf **BlobSink**festgelegt ist. Die SQL-Abfrage für die Eigenschaft **Abfrage** angegeben, werden die Daten aus der Tabelle public.usstates in der PostgreSQL-Datenbank markiert.
    
    {
        "name": "CopyPostgreSqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from \"public\".\"usstates\""
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "PostgreSqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobPostgreSqlDataSet"
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
                    "name": "PostgreSqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="postgresql-linked-service-properties"></a>Eigenschaften der verknüpft PostgreSQL-Dienst

Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die speziell für verknüpfte PostgreSQL-Dienst an.

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Die Eigenschaft muss auf festgelegt sein: **OnPremisesPostgreSql** | Ja
Server | Name des PostgreSQL-Servers. | Ja 
Datenbank | Name der PostgreSQL-Datenbank. | Ja 
Schema | Name des Schemas in der Datenbank. Der Name des Schemas beachtet werden. | Nein 
authenticationType | Art der Authentifizierung verwendet, um zu den PostgreSQL-Datenbank herstellen. Mögliche Werte sind: anonym, Basic und Windows. | Ja 
Benutzername | Geben Sie Benutzernamen an, wenn Sie Basic oder Windows-Authentifizierung verwenden. | Nein 
Kennwort | Geben Sie das Kennwort für das Benutzerkonto, das Sie für den Benutzernamen angegeben haben. | Nein 
gatewayName | Name des Gateways, das der Daten Factory-Dienst verwendet werden sollen, die Verbindung zu der lokalen PostgreSQL-Datenbank herstellen. | Ja 

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für eine lokale PostgreSQL-Datenquelle finden Sie unter [Festlegen der Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="postgresql-dataset-type-properties"></a>PostgreSQL-Typ Datensatzeigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung finden Sie im Artikel [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinie eines Datasets JSON ähneln für alle Dataset-Typen (Azure SQL Azure Blob, Azure Table usw..).

Im Abschnitt TypeProperties unterscheidet sich für jede Art von Dataset und enthält Informationen über den Speicherort der Daten im Datenspeicher. Im Abschnitt TypeProperties für Dataset vom Typ **RelationalTable** (enthält PostgreSQL-Dataset) weist die folgenden Eigenschaften:

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Tabellenname | Name der Tabelle in der PostgreSQL-Datenbank-Instanz, der auf Verknüpfte Dienst verweist. Der Tabellenname beachtet werden. | Nein (wenn die **Abfrage** mit **RelationalSource** angegeben ist) 

## <a name="postgresql-copy-activity-type-properties"></a>PostgreSQL kopieren Aktivität Schrifteigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten verfügbar sind finden Sie unter Artikel [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinie stehen für alle Arten von Aktivitäten. 

Im Abschnitt TypeProperties der Aktivität verfügbaren Eigenschaften variieren andererseits bei jeder Aktivität. Aktivitäten, kopieren variieren je nach den Typen von Datenquellen und senken.

Wenn Quelle vom Typ **RelationalSource** ist (enthält PostgreSQL) stehen die folgenden Eigenschaften im Abschnitt TypeProperties:

Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich
-------- | ----------- | -------------- | --------
Abfrage | Verwenden Sie die benutzerdefinierte Abfrage, um Daten zu lesen. | SQL-Abfragezeichenfolge. Beispiel: "Abfrage": "Wählen Sie * aus \"MySchema\". \"MyTable\"". | Nein (sofern **Tabellenname** **Datasets** angegeben ist)

> [AZURE.NOTE] Schema und Tabellennamen Groß-/Kleinschreibung beachtet werden und sie haben in eingeschlossen sein "" (doppelte Anführungszeichen) in der Abfrage.  

**Beispiel:**

 "Abfrage": "Wählen Sie * aus \"MySchema\". \"MyTable\""

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-postgresql"></a>Für die PostgreSQL-Zuordnung

Wie angegeben führt die [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) Artikel kopieren Aktivität automatische Konvertieren des Datentyps von Arten von Datenquellen zu ignorieren Typen mit dem folgenden Ansatz für-Schritt 2:

1. Konvertieren von Datentypen systemeigenen Quelle in .NET Typ
1. Konvertieren von .NET Typ in einer systemeigenen Empfänger Typ

Beim Verschieben von Daten in PostgreSQL, werden die folgenden Zuordnungen aus PostgreSQL-Typ in .NET Typ verwendet.

PostgreSQL-Datenbank-Datentyp |  PostgresSQL Aliase | .NET Framework-Typ
------------------------ | -------------------- | ---------------------
abstime |  | "DateTime"
bigint | int8 | Int64
bigserial | serial8 | Int64
Bit [(n)] |  | Byte [], Zeichenfolge
Bit mit wechselnder [(n)] | VARBIT | Byte [], Zeichenfolge
Boolesch | bool | Boolesch
Feld |  | Byte [], Zeichenfolge
bytea |  | Byte [], Zeichenfolge
Zeichen [(n)] | Zeichen [(n)] | Zeichenfolge
Zeichen, die mit wechselnder [(n)] | Varchar [(n)] | Zeichenfolge
CID |  | Zeichenfolge
CIDR | | Zeichenfolge
Kreis | | Byte [], Zeichenfolge
Datum | | "DateTime"
DateRange | | Zeichenfolge
doppelter Genauigkeit| FLOAT8 | Double
Internet | | Byte [], Zeichenfolge
intarry | | Zeichenfolge
int4range | | Zeichenfolge
int8range | | Zeichenfolge
ganze Zahl | Int, int4 | Int32
Intervall [Felder] [(p)] | | TimeSpan
JSON | | Zeichenfolge
jsonb | | Byte]
Zeile | | Byte [], Zeichenfolge
lseg | | Byte [], Zeichenfolge
macaddr | | Byte [], Zeichenfolge
Geld | | Dezimalzahl
numerischen [(p, s)] | Decimal [(p, s)] | Dezimalzahl
numrange | | Zeichenfolge
OID | | Int32
Pfad | | Byte [], Zeichenfolge
pg_lsn | | Int64
Zeigen Sie auf | | Byte [], Zeichenfolge
Polygon | | Byte [], Zeichenfolge
Real | FLOAT4 | Einzelne
smallint | int2 | Int16
smallserial | serial2 | Int16
serielle | serial4 | Int32
Text | | Zeichenfolge


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Leistung und optimieren  
Finden Sie unter [Kopieren Aktivität Leistung und Optimieren von Leitfaden für](data-factory-copy-activity-performance.md) die Leistung Einfluss der Daten Bewegung (Kopieren Aktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren sie wichtige Faktoren lernen.