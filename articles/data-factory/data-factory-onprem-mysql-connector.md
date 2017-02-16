<properties 
    pageTitle="Verschieben von Daten aus MySQL | Factory Azure-Daten" 
    description="Informationen Sie zum Verschieben von Daten aus Azure Data Factory mit MySQL-Datenbank." 
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

# <a name="move-data-from-mysql-using-azure-data-factory"></a>Verschieben von Daten aus MySQL Azure Data Factory verwenden

In diesem Artikel wird beschrieben, wie Sie die Aktivität kopieren in einer Factory Azure-Daten verwenden können, zum Verschieben von Daten aus MySQL an eine andere Daten gespeichert. In diesem Artikel wird im Artikel [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) , die eine allgemeine Übersicht über das Verschieben von Daten mit Aktivität kopieren und unterstützten Data Store Kombinationen bietet erstellt.

Factory Datendienst unterstützt das Herstellen einer Verbindung mit einer lokalen MySQL-Datenquellen verwenden das Datenverwaltungsgateway. Finden Sie unter [Verschieben von Daten zwischen lokalen Orten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) Artikel lernen Datenverwaltungsgateway und eine schrittweise Anleitung zum Einrichten des Gateways. 

> [AZURE.NOTE] Gateway ist erforderlich, auch wenn die MySQL-Datenbank in eine IaaS Azure-virtuellen Computern (virtueller Computer) gehostet wird. Sie können das Gateway des gleichen virtuellen Computers als Datenspeicher oder auf einen anderen virtuellen Computer installieren, solange des Gateways auf die Datenbank zugreifen kann.  

Daten Factory unterstützt aktuell nur Verschieben von Daten aus MySQL für andere Datenspeicher, aber nicht zum Verschieben von Daten aus anderen Datenspeicher in MySQL.

## <a name="installation"></a>Installation 
Für das Datenverwaltungsgateway ist die Verbindung zu der MySQL-Datenbank herstellen müssen Sie [MySQL Verbinder/Netto 6.6.5 für Microsoft Windows](http://go.microsoft.com/fwlink/?LinkId=278885) auf demselben System wie das Datenverwaltungsgateway zu installieren.

> [AZURE.NOTE] Finden Sie unter [Behandeln von Gateway Probleme](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) Tipps zur Behebung von Verbindung/Gateway zusammenhängende Probleme. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit, eine Verkaufspipeline zu erstellen, die Daten aus einer MySQL-Datenbank an die Empfänger unterstützten Datenspeicher kopiert besteht darin, den Assistenten zum Kopieren von Daten verwenden. Finden Sie unter [Lernprogramm: Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) für eine schnelle Exemplarische Vorgehensweise zum Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von Daten. 

Im folgende Beispiel stellt Stichproben JSON-Definitionen, die Sie verwenden können, um eine Verkaufspipeline zu erstellen. Umfassende Informationen mit einer schrittweisen Anleitung finden Sie unter [Verschieben von Daten zwischen lokalen Orten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) Artikel. Daten können an allen der senken angegebener [So](data-factory-data-movement-activities.md#supported-data-stores) verwenden die Aktivität kopieren in Azure Data Factory kopiert werden.   

## <a name="sample-copy-data-from-mysql-to-azure-blob"></a>Beispiel: Kopieren Sie Daten aus MySQL in Azure Blob
Dieses Beispiel zeigt, wie Sie Daten aus einer lokalen MySQL-Datenbank in einer Azure BLOB-Speicher zu kopieren. Daten kann jedoch kopierten **direkt** an die senken angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores) die Aktivität kopieren in Azure Data Factory verwenden.  

> [AZURE.IMPORTANT] Dieses Beispiel stellt JSON-Codeausschnitte. Eine schrittweise Anleitung zum Erstellen der Factory Daten enthält nicht. [Verschieben von Daten zwischen lokalen Orten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) -Artikel, um eine schrittweise Anleitung finden Sie unter. 
 
Im Beispiel weist die folgenden Daten Factory Elemente:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesMySql](data-factory-onprem-mysql-connector.md#mysql-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [RelationalTable](data-factory-onprem-mysql-connector.md#mysql-dataset-type-properties).
4.  Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [RelationalSource](data-factory-onprem-mysql-connector.md#mysql-copy-activity-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

Im Beispiel kopiert Daten aus einem Abfrageergebnis in MySQL-Datenbank in ein Blob stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben. 

Richten Sie als ersten Schritt das datenverwaltungsgateway ein. Die Hinweise finden Sie im Artikel [Verschieben von Daten zwischen lokalen Orten und Cloud](data-factory-move-data-between-onprem-and-cloud.md) . 

**Verknüpfte MySQL-Dienst**

    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
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

**Eingabe MySQL-dataset**

Das Beispiel wird vorausgesetzt, Sie eine Tabelle erstellt haben "MyTable" in MySQL und eine Spalte mit der Bezeichnung "Timestampcolumn" für die Reihe Uhrzeitdaten enthält.

Festlegen von "externe": "true" werden dem Daten Factory-Dienst informiert, dass die Tabelle externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.
    
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
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



**Azure Blob ausgeben dataset**

Jede Stunde Daten in einer neuen Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Pfad des Ordners für das Blob wird dynamisch ausgewertet basierend auf der Startzeit des Segments, die verarbeitet wird. Der Pfad des verwendet Jahr, Monat, Tag und Stunden Teile der Startzeit.

    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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



**Pipeline mit Aktivität kopieren**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass die Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **RelationalSource** festgelegt ist, und Typ der **Empfänger** auf **BlobSink**festgelegt ist. Die SQL-Abfrage für die Eigenschaft **Abfrage** angegebenen wählt die Daten in der letzten Stunde zu kopieren.
    
    {
        "name": "CopyMySqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="mysql-linked-service-properties"></a>Eigenschaften der verknüpft MySQL-Dienst

Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die speziell für verknüpfte MySQL-Dienst an.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| Typ | Die Eigenschaft muss auf festgelegt sein: **OnPremisesMySql** | Ja |
| Server | Name der MySQL-Server. | Ja |
| Datenbank | Name der MySQL-Datenbank. | Ja | 
| Schema  | Name des Schemas in der Datenbank. | Nein | 
| authenticationType | Art der Authentifizierung, die zum Verbinden mit der MySQL-Datenbank. Mögliche Werte sind: anonym, Basic und Windows. | Ja | 
| Benutzername | Geben Sie Benutzernamen an, wenn Sie Basic oder Windows-Authentifizierung verwenden. | Nein | 
| Kennwort | Geben Sie das Kennwort für das Benutzerkonto, das Sie für den Benutzernamen angegeben haben. | Nein | 
| gatewayName | Name des Gateways, das der Daten Factory-Dienst verwendet werden sollen, die Verbindung zu der lokalen MySQL-Datenbank herstellen. | Ja |

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für eine lokale MySQL-Datenquelle finden Sie unter [Festlegen der Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="mysql-dataset-type-properties"></a>MySQL-Typ Datensatzeigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung finden Sie im Artikel [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinie eines Datasets JSON ähneln für alle Dataset-Typen (Azure SQL Azure Blob, Azure Table usw..).

Im Abschnitt **TypeProperties** unterscheidet sich für jede Art von Dataset und enthält Informationen über den Speicherort der Daten im Datenspeicher. Im Abschnitt TypeProperties für Dataset vom Typ **RelationalTable** (enthält MySQL-Dataset) weist die folgenden Eigenschaften

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Tabellenname | Name der Tabelle in der MySQL-Datenbank-Instanz, der auf Verknüpfte Dienst verweist. | Nein (wenn die **Abfrage** mit **RelationalSource** angegeben ist) | 

## <a name="mysql-copy-activity-type-properties"></a>MySQL kopieren Aktivität Schrifteigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten verfügbar sind finden Sie unter Artikel [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen sind Richtlinien für alle Arten von Aktivitäten zur Verfügung stehen. 

Im Abschnitt **TypeProperties** der Aktivität verfügbaren Eigenschaften variieren andererseits bei jeder Aktivität. Aktivitäten, kopieren variieren je nach den Typen von Datenquellen und senken.

Wenn Quelle in der Kopie Aktivität vom Typ **RelationalSource** ist (enthält MySQL) stehen die folgenden Eigenschaften im Abschnitt TypeProperties:

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| Abfrage | Verwenden Sie die benutzerdefinierte Abfrage, um Daten zu lesen. | SQL-Abfragezeichenfolge. Beispiel: Wählen Sie * from MyTable. | Nein (sofern **Tabellenname** **Datasets** angegeben ist) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-mysql"></a>Für MySQL-Zuordnung

Wie in den [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) Artikel erwähnt, führt kopieren Aktivität automatische Konvertieren des Datentyps von Arten von Datenquellen zu Typen mit den folgenden zwei Ansatz ignorieren:

1. Konvertieren von Datentypen systemeigenen Quelle in .NET Typ
2. Konvertieren von .NET Typ in einer systemeigenen Empfänger Typ

Beim Verschieben von Daten in MySQL, werden die folgenden Zuordnungen aus MySQL-Datentypen in .NET Dateitypen verwendet.

| MySQL-Datenbank-Datentyp | .NET Framework-Typ |
| ------------------- | ------------------- | 
| unsigned bigint | Dezimalzahl |
| bigint | Int64 |
| Bit | Dezimalzahl |
| BLOB | Byte] |
| bool |  Boolesch | 
| Zeichen | Zeichenfolge |
| Datum | "DateTime" |
| "DateTime" | "DateTime" |
| Dezimalzahl | Dezimalzahl |
| doppelter Genauigkeit | Double |
| Doppelte | Double |
| Aufzählung | Zeichenfolge |
| frei verschieben | Einzelne |
| Ganzzahl ohne Vorzeichen | Int64 |
| Ganzzahl | Int32 |
| ganze Zahl ohne Vorzeichen | Int64 |
| ganze Zahl | Int32 | 
| lange varbinary | Byte] |
| lange varchar | Zeichenfolge |
| longblob | Byte] |
| LONGTEXT | Zeichenfolge | 
| mediumblob |  Byte] | 
| Mediumint ohne Vorzeichen | Int64 |
| mediumint | Int32 | 
| mediumtext | Zeichenfolge |
| numerische | Dezimalzahl |
| Real |  Double |
| Festlegen | Zeichenfolge |
| unsigned smallint | Int32 |
| smallint | Int16 |
| Text | Zeichenfolge |
| Zeit | TimeSpan |
| Zeitstempel | "DateTime" |
| tinyblob | Byte] |
| Tinyint ohne Vorzeichen |  Int16 | 
| tinyint | Int16 |
| tinytext | Zeichenfolge | 
| varchar | Zeichenfolge | 
| Jahr | Ganzzahl | 


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Leistung und optimieren  
Finden Sie unter [Kopieren Aktivität Leistung und Optimieren von Leitfaden für](data-factory-copy-activity-performance.md) die Leistung Einfluss der Daten Bewegung (Kopieren Aktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren sie wichtige Faktoren lernen.

