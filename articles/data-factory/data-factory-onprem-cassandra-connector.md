<properties 
    pageTitle="Verschieben von Daten aus mit Daten Factory Cassandra | Microsoft Azure" 
    description="Informationen Sie zum Verschieben von Daten aus einer lokalen Cassandra Datenbank Azure Data Factory verwenden." 
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
    ms.date="09/07/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Verschieben von Daten aus einer lokalen Cassandra-Datenbank mit Azure Data Factory
In diesem Artikel wird beschrieben, wie Sie die Aktivität kopieren in einer Factory Azure-Daten verwenden können, zum Kopieren von Daten aus einer lokalen Cassandra-Datenbank in eine beliebige Datenspeicher unter Empfänger Spalte im Abschnitt [Quellen unterstützt und senken](data-factory-data-movement-activities.md#supported-data-stores) aufgeführt. In diesem Artikel wird im Artikel [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md) , die eine allgemeine Übersicht über das Verschieben von Daten mit Aktivität kopieren und unterstützten Data Store Kombinationen bietet erstellt.

Daten Factory unterstützt derzeit nur Verschieben von Daten aus einer Datenbank Cassandra [Empfänger Datenspeicher](data-factory-data-movement-activities.md#supported-data-stores)unterstützt, aber nicht Verschieben von Daten aus anderen Datenspeicher zu einer Datenbank Cassandra.

## <a name="prerequisites"></a>Erforderliche Komponenten
Für den Dienst Azure Data Factory zu Ihrer lokalen Cassandra-Datenbank herstellen können müssen Sie Folgendes installieren: 

- Datenverwaltungsgateway 2.0 oder höher auf dem Computer, der die Datenbank hostet oder auf einem separaten Computer, um zu vermeiden, Anspruch auf Ressource mit der Datenbank. Datenverwaltungsgateway ist eine Software, die auf lokale Datenquellen und Cloud Services in eine sichere und verwaltete Möglichkeit besteht. [Verschieben von Daten zwischen lokalen und Cloud](data-factory-move-data-between-onprem-and-cloud.md) finden Sie im Artikel Details des Datenverwaltungsgateways.
  
    Wenn Sie das Gateway installiert haben, wird einen Microsoft Cassandra ODBC-Treiber zum Verbinden mit Cassandra Datenbank automatisch installiert. 

> [AZURE.NOTE] Finden Sie unter [Behandeln von Gateway Probleme](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) Tipps zur Behebung von Verbindung/Gateway zusammenhängende Probleme. 

## <a name="copy-data-wizard"></a>Assistent zum Kopieren von Daten
Die einfachste Möglichkeit, eine Verkaufspipeline zu erstellen, die Daten aus einer Datenbank Cassandra an die Empfänger unterstützten Datenspeicher kopiert besteht darin, den Assistenten zum Kopieren von Daten verwenden. Finden Sie unter [Lernprogramm: Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von](data-factory-copy-data-wizard-tutorial.md) für eine schnelle Exemplarische Vorgehensweise zum Erstellen einer Verkaufspipeline mithilfe des Assistenten zum Kopieren von Daten. 

Im folgende Beispiel enthält die Stichprobe JSON-Definitionen, die Sie verwenden können, erstellen Sie eine Verkaufspipeline mithilfe von [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md) oder [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Wird gezeigt, wie Daten aus Datenbank Cassandra in Azure BLOB-Speicher zu kopieren. Daten können jedoch an die senken angegebener [So](data-factory-data-movement-activities.md#supported-data-stores) verwenden die Aktivität kopieren in Azure Data Factory kopiert werden.   


## <a name="sample-copy-data-from-cassandra-to-blob"></a>Beispiel: Daten aus Cassandra in Blob kopieren
Im Beispiel kopiert Daten aus einer Datenbank Cassandra in eine Azure Blob stündlich. Die JSON-Eigenschaften, die in diesen Beispielen verwendete werden in den Beispielen folgen Abschnitten beschrieben. Daten können direkt an die im Artikel [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md#supported-data-stores) angegeben ist, verwenden Sie die Aktivität kopieren in Azure Data Factory senken kopiert werden. 

- Eine verknüpfte Dienst vom Typ [OnPremisesCassandra](#onpremisescassandra-linked-service-properties).
- Eine verknüpfte Dienst vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Eine Eingabe- [Dataset](data-factory-create-datasets.md) vom Typ [CassandraTable](#cassandratable-properties).
- Eine Ausgabe [Dataset](data-factory-create-datasets.md) vom Typ [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Eine [Verkaufspipeline](data-factory-create-pipelines.md) mit Aktivität kopieren, die [CassandraSource](#cassandrasource-type-properties) und [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties)verwendet.

**Cassandra verknüpft-Dienst**

In diesem Beispiel wird den Dienst **Cassandra** verknüpft. Siehe [Cassandra verknüpft Dienst](#onpremisescassandra-linked-service-properties) Abschnitt für die Eigenschaften, die von diesem Dienst verknüpften unterstützt.  

    {
        "name": "CassandraLinkedService",
        "properties":
        {
            "type": "OnPremisesCassandra",
            "typeProperties":
            {
                "authenticationType": "Basic",
                "host": "mycassandraserver",
                "port": 9042,
                "username": "user",
                "password": "password",
                "gatewayName": "mygateway"
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

**Cassandra Eingabe-dataset**

    {
        "name": "CassandraInput",
        "properties": {
            "linkedServiceName": "CassandraLinkedService",
            "type": "CassandraTable",
            "typeProperties": {
                "tableName": "mytable",
                "keySpace": "mykeyspace" 
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

**Externe** auf **true** festlegen informiert dem Daten Factory-Dienst an, dass das Dataset externe Daten Fabrik Wert und nicht durch eine Aktivität in der Factory Daten erstellt wird.

**Azure Blob ausgeben dataset**

Jede Stunde Daten in einer neuen Blob geschrieben (Häufigkeit: Stunde, Intervall: 1). 

    {
        "name": "AzureBlobOutput",
        "properties":
        {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties":
            {
                "folderPath": "adfgetstarted/fromcassandra"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Pipeline mit Aktivität kopieren**

Der Verkaufspipeline enthält eine Kopie-Aktivität, ist so konfiguriert, dass die Eingabe- und Datasets verwenden und stündlich Ausführung geplant ist. Klicken Sie in der Verkaufspipeline JSON-Definition der Typ der **Quelle** auf **CassandraSource** festgelegt ist, und Typ der **Empfänger** auf **BlobSink**festgelegt ist. 

Die Liste der Eigenschaften von der RelationalSource unterstützt finden Sie unter [RelationalSource Schrifteigenschaften](#cassandrasource-type-properties) . 
    
    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2016-06-01T18:00:00",
            "end":"2016-06-01T19:00:00",
            "description":"pipeline with copy activity",
            "activities":[  
            {
                "name": "CassandraToAzureBlob",
                "description": "Copy from Cassandra to an Azure blob",
                "type": "Copy",
                "inputs": [
                {
                    "name": "CassandraInput"
                }
                ],
                "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "CassandraSource",
                        "query": "select id, firstname, lastname from mykeyspace.mytable"
        
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
## <a name="onpremisescassandra-linked-service-properties"></a>Eigenschaften des OnPremisesCassandra verknüpft

Die folgende Tabelle enthält eine Beschreibung für den JSON-Elemente, die speziell für Dienst Cassandra verknüpft.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- | 
| Typ | Die Eigenschaft muss auf festgelegt sein: **OnPremisesCassandra** | Ja |
| Host | Eine oder mehrere IP-Adressen oder Hostnamen Cassandra Server.<br/><br/>Geben Sie eine durch Trennzeichen getrennte Liste von IP-Adressen oder Hostnamen in Verbindung mit allen Servern gleichzeitig an. | Ja | 
| Port | Die TCP-, die vom Server Cassandra, Clientverbindungen zu überwachen. | Nein, Standardwert: 9042 |
| authenticationType | Einfacher oder anonymer | Ja |
| Benutzername |Geben Sie Benutzernamen für das Benutzerkonto. | Ja, wenn AuthenticationType Basic festgelegt ist. |
| Kennwort | Geben Sie das Kennwort für das Benutzerkonto an.  | Ja, wenn AuthenticationType Basic festgelegt ist. |
| gatewayName | Der Name des Gateways, die Verbindung zur lokalen Cassandra Datenbank verwendet wird. | Ja |
| encryptedCredential | Anmeldeinformationen, die vom Gateway verschlüsselt werden. | Nein | 

## <a name="cassandratable-properties"></a>CassandraTable Eigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften, die zum Definieren von Datasets zur Verfügung finden Sie im Artikel [Datasets erstellen](data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinie eines Datasets JSON ähneln für alle Dataset-Typen (Azure SQL Azure Blob, Azure Table usw..).

Im Abschnitt **TypeProperties** unterscheidet sich für jede Art von Dataset und enthält Informationen über den Speicherort der Daten im Datenspeicher. Im Abschnitt TypeProperties für Dataset vom Typ **CassandraTable** weist die folgenden Eigenschaften

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Schlüssellänge zur Verfügung | Name des Schema in Cassandra Datenbank oder Schlüssellänge zur Verfügung. | Ja (wenn die **Abfrage** für **CassandraSource** nicht definiert wurde). |
| Tabellenname | Name der Tabelle in der Datenbank Cassandra. | Ja (wenn die **Abfrage** für **CassandraSource** nicht definiert wurde). |


## <a name="cassandrasource-type-properties"></a>CassandraSource Typeigenschaften
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten verfügbar sind finden Sie unter Artikel [Pipelines erstellen](data-factory-create-pipelines.md) . Eigenschaften wie Name, Beschreibung, Eingabe- und Tabellen und Richtlinie stehen für alle Arten von Aktivitäten. 

Im Abschnitt TypeProperties der Aktivität verfügbaren Eigenschaften variieren andererseits bei jeder Aktivität. Aktivitäten, kopieren variieren je nach den Typen von Datenquellen und senken.

Wenn Quelle vom Typ **CassandraSource**ist, sind die folgenden Eigenschaften im Abschnitt TypeProperties verfügbar:

| Eigenschaft | Beschreibung | Zulässigen Werte | Erforderlich |
| -------- | ----------- | -------------- | -------- |
| Abfrage | Verwenden Sie die benutzerdefinierte Abfrage, um Daten zu lesen. | SQL-92-Abfrage oder CQL Abfrage. Finden Sie unter [CQL verweisen](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Geben Sie bei der Verwendung von SQL-Abfrage **Schlüssellänge zur Verfügung name.table Namen** , um die Darstellung der Tabelle, die Sie abfragen möchten. | Nein (wenn der Tabellenname und Schlüssellänge zur Verfügung, auf Dataset definiert sind).  |
| consistencyLevel | Die Konsistenz-Ebene gibt an, wie viele Replikate zu einer Besprechungsanfrage gelesen reagieren müssen, bevor Daten an die Clientanwendung zurückgegeben. Cassandra überprüft die angegebene Anzahl von Replikaten für Daten zur Ausführung der Anforderung finden Sie hier. | EINE, ZWEI, DREI, QUORUMS FINDEN SIE UNTER ALL, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE. Details finden Sie unter [Konfigurieren von Datenkonsistenz](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) . | Nein. Standardwert ist ein. |  


### <a name="type-mapping-for-cassandra"></a>Für Cassandra-Zuordnung
Cassandra Typ | .NET Grundlage Typ
--------------- | ---------------
ASCII | Zeichenfolge 
BIGINT | Int64
BLOB | Byte]
BOOLESCH | Boolesch
DEZIMALZAHL | Dezimalzahl
DOUBLE | Double
FREI VERSCHIEBEN | Einzelne
INTERNET | Zeichenfolge
GANZZAHL | Int32
TEXT | Zeichenfolge
ZEITSTEMPEL | "DateTime"
TIMEUUID | GUID
UUID | GUID
VARCHAR | Zeichenfolge
VARINT | Dezimalzahl

> [AZURE.NOTE]  
> Für die Websitesammlung finden Typen (Karte, festlegen, Liste usw.), zum [Arbeiten mit Cassandra Websitesammlung Typen virtuelle Tabelle verwenden](#work-with-collections-using-virtual-table) . 
> 
> Benutzerdefinierte Typen werden nicht unterstützt.
> 
> Die Länge der Zeichenfolge und Binärspalte Länge darf nicht größer als 4000 sein. 

## <a name="work-with-collections-using-virtual-table"></a>Arbeiten Sie mit Sammlungen mit virtuelle Tabelle
Azure Data Factory verwendet einen integrierten ODBC-Treiber zum Herstellen einer Verbindung mit und kopieren Sie Daten aus Ihrer Datenbank Cassandra. Für Websitesammlung Typen einschließlich Karte sowie festlegen und Liste normalisiert erneut der Treiber die Daten in den entsprechenden virtuelle Tabellen. Eine Tabelle eine Auflistung der Spalten enthält, wird der Treiber insbesondere in den folgenden virtuellen Tabellen generiert:

-   Eine **Basis Tabelle**, die die gleichen Daten wie realen Tischs eine Ausnahme bilden jedoch die Websitesammlung Spalten enthält. Die base Tabelle verwendet denselben Namen als realen Tischs, die ihn darstellt.
-   Eine **virtuelle Tabelle** für jede Websitesammlung-Spalte, die erweitert die geschachtelten Daten. Virtuellen Tabellen, die Websitesammlungen darstellen sind benannte unter Verwendung des realen Tischs, ein Trennzeichen "_vt_" und den Namen der Spalte.

Virtuelle Tabellen beziehen sich auf die Daten in der eigentlichen Tabelle, den Zugriff auf die denormalisierten Daten-Treiber aktivieren. Siehe Abschnitt "Beispiel" Details. Sie können den Inhalt der Cassandra Websitesammlungen zugreifen, indem Sie Abfragen und die Teilnahme an der virtuellen Tabellen.

Sie können nutzen Sie den [Assistenten zum Kopieren](data-factory-data-movement-activities.md#data-factory-copy-wizard) , um intuitiv die Liste der Tabellen in Cassandra Datenbank einschließlich virtuellen Tabellen anzuzeigen und Anzeigen einer Vorschau die Daten in der. Sie können auch erstellen eine Abfrage in den Assistenten zum Kopieren und überprüfen, um das Ergebnis anzuzeigen.

### <a name="example"></a>Beispiel
Die folgenden "ExampleTable" beträgt beispielsweise eine Cassandra Datenbanktabelle mit einer Ganzzahl primären Schlüsselspalte mit dem Namen "Pk_int", mit der Bezeichnung Value Textspalte, einer Listenspalte, eine Spalte Karte und eine Menge Spalte (mit dem Namen "StringSet").

pk_int | Wert | Liste | Karte |   StringSet
------ | ----- | ---- | --- | --------
1 | Beispiel für "Wert 1" | ["1", "2", "3"]  | {"S1": "a", "S2": "b"} | {"A", "B", "C"}
3 | Beispiel für "Wert 3" | ["100", "101", "102", "105"] | {"S1": "t"} | {"A", "E"}

Der Treiber würde mehrere virtuelle Tabellen zum Darstellen dieser einzelne Tabelle generieren. Die Fremdschlüssel Key Spalten in den virtuellen Tabellen verweisen auf Spalten für die Primärschlüssel in der Tabelle Real- und anzugeben, welche real Tabellenzeile die virtuelle Tabellenzeile entspricht. 

Die erste virtuelle Tabelle ist, dass die base Tabelle mit dem Namen "ExampleTable" in der folgenden Tabelle angezeigt wird. Die base Tabelle enthält die gleichen Daten wie die ursprüngliche Datenbanktabelle eine Ausnahme bilden jedoch die Sammlungen, die in dieser Tabelle ausgelassen und in anderen Tabellen virtuellen erweitert werden.

pk_int | Wert
------ | -----
1 | Beispiel für "Wert 1"
3 | Beispiel für "Wert 3"

Die folgende Tabelle enthält die virtuellen Tabellen, die die Daten aus der Liste, die Karte und StringSet Spalten renormalize. Spalten mit Namen, die mit "_index" oder "_key" enden, geben Sie die Position der Daten in der ursprünglichen Liste oder Zuordnung an. Spalten mit Namen, die mit "_value" enden, enthalten die erweiterten Daten aus der Sammlung.

#### <a name="table-exampletablevtlist"></a>Tabelle "ExampleTable_vt_List":
pk_int | List_index | List_value
------ | ---------- | ----------
1 | 0 | 1
1 | 1 | 2
1 | 2 | 3
3 | 0 | 100
3 | 1 | 101
3 | 2 | 102
3 | 3 | 103

#### <a name="table-exampletablevtmap"></a>Tabelle "ExampleTable_vt_Map":
pk_int | Map_key | Map_value
------ | ------- | ---------
1 | S1 | A
1 | S2 | b
3 | S1 | t

#### <a name="table-exampletablevtstringset"></a>Tabelle "ExampleTable_vt_StringSet":
pk_int | StringSet_value
------ | ---------------
1 | A
1 | B
1 | C
3 | A
3 | E





[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]
[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Leistung und optimieren  
Finden Sie unter [Kopieren Aktivität Leistung und Optimieren von Leitfaden für](data-factory-copy-activity-performance.md) die Leistung Einfluss der Daten Bewegung (Kopieren Aktivität) in Azure Data Factory und verschiedene Methoden zum Optimieren sie wichtige Faktoren lernen.
