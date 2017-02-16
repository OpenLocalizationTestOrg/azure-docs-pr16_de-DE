<properties 
    pageTitle="Verschieben von Daten zu/aus Oracle mit Daten Factory | Microsoft Azure" 
    description="Informationen Sie zum Verschieben von Daten zu/aus der Oracle-Datenbank, die lokal Azure Data Factory verwenden." 
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

# <a name="move-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Verschieben von Daten zu/aus lokalen Oracle Azure Data Factory verwenden 

Dieser Artikel beschreibt, wie Sie Daten Factory kopieren Aktivität zum Verschieben von Daten zu/aus Oracle zum Speichern von aus/mit einem anderen Daten verwenden können. In diesem Artikel wird im Artikel [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) , die eine allgemeine Übersicht über das Verschieben von Daten mit Aktivität kopieren und unterstützten Data Store Kombinationen bietet erstellt.

## <a name="installation"></a>Installation 
Für den Dienst Azure Data Factory zu Ihrer lokalen Oracle-Datenbank herstellen können müssen Sie die folgenden Komponenten installieren: 

- Datenverwaltungsgateway auf dem Computer, der die Datenbank hostet oder auf einem separaten Computer zu vermeiden, Anspruch auf Ressource mit der Datenbank. Datenverwaltungsgateway ist eine Client-Agent, der auf lokale Datenquellen und Cloud Services in eine sichere und verwaltete Möglichkeit besteht. [Verschieben von Daten zwischen lokalen und Cloud](data-factory-move-data-between-onprem-and-cloud.md) finden Sie im Artikel Details des Datenverwaltungsgateways. 
- Oracle-Datenanbieter für .NET. Diese Komponente ist in [Oracle Data Access Komponenten für Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/)enthalten. Installieren Sie die entsprechende Version (32/64-Bit) auf dem Hostcomputer, auf das Gateway installiert ist. [Oracle Datenanbieter .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) können Oracle-Datenbank 10 g Version 2 oder höher zugreifen.

    Wenn Sie "XCopy Installation" auswählen, führen Sie die Schritte im Stand. Es empfiehlt sich, dass Sie das Installationsprogramm mit Benutzeroberfläche (nicht-XCopy eine) auswählen. 
 
    Starten Sie nach der Installation von den Anbieter, den Daten Management Gateway-Hostdienst auf Ihrem Computer mithilfe von Diensten Applet (oder) Daten Management Gateway-Konfigurations-Manager erneut.  

> [AZURE.NOTE] Finden Sie unter [Behandeln von Gateway Probleme](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) Tipps zur Behebung von Verbindung/Gateway zusammenhängende Probleme. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit, eine Verkaufspipeline zu erstellen, die Daten aus/mit einer Oracle-Datenbank an die Empfänger unterstützten Datenspeicher kopiert besteht darin, den Assistenten zum Kopieren von Daten verwenden. Finden Sie unter [Lernprogramm: Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) für eine schnelle Exemplarische Vorgehensweise zum Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von Daten. 

Im folgende Beispiel enthält die Stichprobe JSON-Definitionen, die Sie verwenden können, erstellen Sie eine Verkaufspipeline mithilfe von [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Wird gezeigt, wie Sie zum Kopieren von Daten aus/mit einer Oracle-Datenbank zu/Azure BLOB-Speicher. Daten können jedoch an die senken angegebener [So](data-factory-data-movement-activities.md#supported-data-stores) verwenden die Aktivität kopieren in Azure Data Factory kopiert werden.   

## <a name="sample-copy-data-from-oracle-to-azure-blob"></a>Beispiel: Kopieren Sie Daten aus Oracle in Azure Blob
Dieses Beispiel zeigt, wie Sie Daten aus einer lokalen Oracle-Datenbank in einer Azure BLOB-Speicher kopieren. Daten kann jedoch kopierten **direkt** an die senken angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores) die Aktivität kopieren in Azure Data Factory verwenden.  
 
Im Beispiel weist die folgenden Daten Factory Elemente:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
4.  Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
5.  Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [OracleSource](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) als Quell- und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) als Empfänger verwendet.

Im Beispiel kopiert Daten aus einer Tabelle in einer lokalen Oracle-Datenbank in ein Blob stündlich. Weitere Informationen zu verschiedenen Eigenschaften, die in der Stichprobe verwendet finden Sie unter Dokumentation in Abschnitte, die in den Beispielen folgen.

**Verknüpfte Oracle-Dienst:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure Blob-Speicher verknüpft Dienst an:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Eingabe Oracle-Dataset:**

Das Beispiel wird vorausgesetzt, Sie eine Tabelle erstellt haben "MyTable" in Oracle und eine Spalte mit der Bezeichnung "Timestampcolumn" für die Reihe Uhrzeitdaten enthält. 

Festlegen von "externe": "true" werden dem Daten Factory-Dienst informiert, dass das Dataset externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.

    {
        "name": "OracleInput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
               "external": true,
            "availability": {
                "offset": "01:00:00",
                "interval": "1",
                "anchorDateTime": "2014-02-27T12:00:00",
                "frequency": "Hour"
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

Jede Stunde Daten in einer neuen Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). Der Ordnername Pfad und den Dateinamen für die Blob werden dynamisch ausgewertet, basierend auf der Startzeit des Segments, die verarbeitet wird. Der Pfad des verwendet Jahr, Monat, Tag und Stunden Teile der Startzeit.
    
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


**Pipeline mit Aktivität kopieren:**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass die Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **OracleSource** festgelegt ist, und Typ der **Empfänger** auf **BlobSink**festgelegt ist.  Die SQL-Abfrage mit **OracleReaderQuery** Eigenschaft angegeben, werden die Daten in der letzten Stunde zu kopierenden markiert.

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": " OracleInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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


Sie müssen anpassen die Abfragezeichenfolge basierend auf wie Datumsangaben in Ihrer Oracle-Datenbank konfiguriert sind. Wenn Sie die folgende Fehlermeldung angezeigt: 

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

Möglicherweise müssen Sie die Abfrage zu ändern, wie im folgenden Beispiel (mit der Funktion To_date) dargestellt:

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"

## <a name="sample-copy-data-from-azure-blob-to-oracle"></a>Beispiel: Kopieren Sie Daten aus Azure Blob zu Oracle
Dieses Beispiel zeigt, wie Sie Daten aus einer Azure BLOB-Speicher in einer lokalen Oracle-Datenbank zu kopieren. Daten kann jedoch kopierten **direkt** von jedem der Quellen angegebener [hier](data-factory-data-movement-activities.md#supported-data-stores) die Aktivität kopieren in Azure Data Factory verwenden.  
 
Im Beispiel weist die folgenden Daten Factory Elemente:

1.  Eine verknüpfte Dienst vom Typ [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
5.  Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) als [OracleSink](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) als Empfänger Datenquelle verwendet.

Die Stichprobe kopiert Daten von einem Blob in eine Tabelle in einer lokalen Oracle-Datenbank stündlich. Weitere Informationen zu verschiedenen Eigenschaften, die in der Stichprobe verwendet finden Sie unter Dokumentation in Abschnitte, die in den Beispielen folgen.

**Verknüpfte Oracle-Dienst:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure Blob-Speicher verknüpft Dienst an:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Eingabe Azure Blob-dataset**

Daten werden übernommen aus einer neuen Blob stündlich (Häufigkeit: Stunde, Intervall: 1). Der Ordnername Pfad und den Dateinamen für die Blob werden dynamisch ausgewertet, basierend auf der Startzeit des Segments, die verarbeitet wird. Der Pfad des verwendet Jahr, Monat und Tagesanteil der Startzeit und Dateinamen der Stundenteil des der Startzeit. "externe": "true" Einstellung informiert dem Daten Factory-Dienst, dass diese Tabelle externe Daten Fabrik und nicht durch eine Aktivität in der Factory Daten erzeugt.

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

**Oracle Ausgabe Dataset:**

Im Beispiel wird davon ausgegangen, dass Sie eine Tabelle "MyTable" in Oracle erstellt haben. Erstellen Sie die Tabelle in Oracle mit der gleichen Anzahl von Spalten wie erwartet die BLOB-CSV-Datei enthalten. Neue Zeilen werden stündlich zur Tabelle hinzugefügt.

    {
        "name": "OracleOutput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": "1"
            }
        }
    }


**Pipeline mit Aktivität kopieren:**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass die Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **BlobSource** festgelegt ist, und der Typ der **Empfänger** auf **OracleSink**festgelegt ist.  

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "OracleOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "OracleSink"
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


## <a name="oracle-linked-service-properties"></a>Eigenschaften der verknüpft Oracle-Dienst

Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die speziell für verknüpfte Oracle-Dienst an. 

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Typ | Die Eigenschaft muss auf festgelegt sein: **OnPremisesOracle** | Ja
connectionString | Geben Sie Informationen zum Verbinden mit der Oracle-Datenbank-Instanz für die Eigenschaft ConnectionString erforderlich. | Ja 
gatewayName | Name des Gateways, die in Verbindung mit dem lokalen Oracle-Server verwendet wird | Ja

Ausführliche Informationen zum Festlegen von Anmeldeinformationen für eine lokale Oracle-Datenquelle finden Sie unter [Festlegen der Anmeldeinformationen und Sicherheit](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .
## <a name="oracle-dataset-type-properties"></a>Oracle-Typ Datensatzeigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung finden Sie im Artikel [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinie eines Datasets JSON ähneln für alle Dataset-Typen (Oracle, Azure Blob, Azure Table usw.).
 
Im Abschnitt TypeProperties unterscheidet sich für jede Art von Dataset und enthält Informationen über den Speicherort der Daten im Datenspeicher. Im Abschnitt TypeProperties für das Dataset vom Typ OracleTable weist die folgenden Eigenschaften:

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Tabellenname | Name der Tabelle in der Oracle-Datenbank, die auf der Dienst verknüpfte verweist. | Nein (Wenn **OracleReaderQuery** der **OracleSource** angegeben ist)

## <a name="oracle-copy-activity-type-properties"></a>Oracle kopieren Aktivität Schrifteigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten verfügbar sind finden Sie unter Artikel [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinie stehen für alle Arten von Aktivitäten. 

> [AZURE.NOTE]
Die Aktivität kopieren übernimmt nur eine Eingabe und die Ausgabe nur ein.

Im Abschnitt TypeProperties der Aktivität verfügbaren Eigenschaften variieren andererseits bei jeder Aktivität. Aktivitäten, kopieren variieren je nach den Typen von Datenquellen und senken.

### <a name="oraclesource"></a>OracleSource
In Aktivität kopieren Wenn die Quelle vom Typ **OracleSource** ist stehen die folgenden Eigenschaften im Abschnitt **TypeProperties** :

Eigenschaft | Beschreibung |Zulässigen Werte | Erforderlich
-------- | ----------- | ------------- | --------
oracleReaderQuery | Verwenden Sie die benutzerdefinierte Abfrage, um Daten zu lesen. | SQL-Abfragezeichenfolge. Beispiel: Wählen Sie *from MyTable <br/> <br/>nicht angegeben, die SQL-Anweisung, die ausgeführt wird: Wählen Sie* from MyTable | Nein (sofern **Tabellenname** **Datasets** angegeben ist)

### <a name="oraclesink"></a>OracleSink
**OracleSink** unterstützt die folgenden Eigenschaften:

Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich
-------- | ----------- | -------------- | --------
writeBatchTimeout | Wartezeit für Stapel einfügen nach Abschluss des Vorgangs bevor es abläuft. | TimeSpan<br/><br/> Beispiel: 00:30:00 (30 Minuten). | Nein
writeBatchSize | Fügt Daten in der SQL-Tabelle an, wenn die Puffergröße WriteBatchSize erreicht.   | Ganzzahl (Anzahl von Zeilen)| Nein (Standard: 10000)  
sqlWriterCleanupScript | Geben Sie eine Abfrage für Kopieren Aktivität auszuführende so, dass die Daten eines bestimmten Segments bereinigt werden. | Eine Abfrage-Anweisung. | Nein
sliceIdentifierColumnName | Geben Sie die Spaltennamen für Kopieren Aktivität automatisch generiert Segment Bezeichner enthält, füllen Sie die Daten eines bestimmten Segments erneut ausführen, wenn Bereinigen verwendet wird. | Spaltenname einer Spalte mit dem Datentyp des binary(32). | Nein


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-oracle"></a>Für Oracle-Zuordnung

Wie angegeben führt die [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) Artikel kopieren Aktivität automatische Konvertieren des Datentyps von Arten von Datenquellen zu ignorieren Typen mit dem folgenden Ansatz für-Schritt 2:

1. Konvertieren von Datentypen systemeigenen Quelle in .NET Typ
2. Konvertieren von .NET Typ in einer systemeigenen Empfänger Typ

Beim Verschieben von Daten aus Oracle werden die folgenden Zuordnungen aus Oracle-Datentyp in .NET Typ und umgekehrt verwendet.

Oracle-Datentyp | .NET Framework-Datentyp
---------------- | ------------------------
BFILE | Byte]
BLOB | Byte]
ZEICHEN | Zeichenfolge
CLOB | Zeichenfolge
DATUM | "DateTime"
FREI VERSCHIEBEN | Dezimalzahl
GANZE ZAHL | Dezimalzahl
INTERVALL JAHR ZUM MONAT | Int32
INTERVALL TAG ZWEITEN | TimeSpan
LANGE | Zeichenfolge
LANGE UNFORMATIERTEN | Byte]
NCHAR | Zeichenfolge
NCLOB | Zeichenfolge
ZAHL | Dezimalzahl
NVARCHAR2 | Zeichenfolge
UNFORMATIERTE | Byte]
ROWID | Zeichenfolge
ZEITSTEMPEL | "DateTime"
TIMESTAMP MIT LOKALEN ZEITZONE | "DateTime"
TIMESTAMP MIT (BILDSCHIRMDRUCK) | "DateTime"
GANZE ZAHL OHNE VORZEICHEN | Zahl
VARCHAR2 | Zeichenfolge
XML | Zeichenfolge

## <a name="troubleshooting-tips"></a>Tipps zur Problembehandlung

**Problem:** Die folgende **Fehlermeldung**angezeigt: Kopieren Aktivität erfüllt bei ungültigen Parametern: UnknownParameterName, detaillierte Meldung: kann nicht gefunden werden die angeforderten .net Framework-Datenanbieter. Möglicherweise ist er nicht installiert".  

**Mögliche Gründe:**

1. .NET Framework-Datenanbieter für Oracle wurde nicht installiert.
2. .NET Framework-Datenanbieter für Oracle in .NET Framework 2.0 installiert wurde, und in die .NET Framework 4.0-Ordner nicht gefunden wird. 

**Mit einer Auflösung von/dieses Problem zu umgehen:**

1. Wenn Sie den .NET-Anbieter für Oracle, [Installieren Sie es](http://www.oracle.com/technetwork/topics/dotnet/downloads/) noch nicht installiert, und wiederholen das Szenario. 
2. Wenn Sie die Fehlermeldung erhalten, auch nach der Installation des Anbieters, führen Sie die folgenden Schritte aus: 
    1. Maschinelle Config von .NET 2.0 aus dem Ordner öffnen: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
    2. Suche für **Oracle-Datenanbieter für .NET**, und Sie können einen Eintrag zu finden, wie in der folgenden Beispiel Unwn in den folgenden dargestellt werden sollen (Beispiel) heben Sie die Markierungder **system.data** -> **DbProviderFactories**:
            “<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
2.  Kopieren Sie diesen Eintrag zu der Datei "Machine.config" im folgenden Ordner Version 4.0: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config, und ändern Sie die Version 4.xxx.x.x.
3.  Installieren Sie "< (ODP.NET installierte Pfad > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" im globalen Assemblycache (GAC) durch Ausführen `gacutil /i [provider path]`.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


## <a name="performance-and-tuning"></a>Leistung und optimieren  
Finden Sie unter [Kopieren Aktivität Leistung und Optimieren von Leitfaden für](data-factory-copy-activity-performance.md) die Leistung Einfluss der Daten Bewegung (Kopieren Aktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren sie wichtige Faktoren lernen.
