<properties 
    pageTitle="Erstellen von Datasets in Azure Data Factory | Microsoft Azure" 
    description="Erfahren Sie, wie Datasets in Azure Data Factory mit Beispiele zu erstellen, die Eigenschaften wie Bereich.verschieben und AnchorDateTime verwenden."
    keywords="Erstellen Sie Dataset, Dataset Beispiel, versetzt Beispiel"
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="shlo"/>

# <a name="datasets-in-azure-data-factory"></a>Datasets in Factory Azure-Daten
In diesem Artikel beschrieben Datasets in Azure Data Factory und enthält Beispiele wie Offset, AnchorDateTime und Offset/Formatvorlage Datenbanken.

Wenn Sie ein Dataset erstellen, erstellen Sie einen Zeiger auf die Daten, die Sie bearbeiten möchten. Daten verarbeitet (Eingabe/Ausgabe) in einer Aktivität und eine Aktivität in einer Verkaufspipeline enthalten ist. Eingabe-Dataset die Eingabe für eine Aktivität in der Verkaufspipeline und ein Dataset Ausgabe darstellt die Ausgabe für die Aktivität.

Datasets identifizieren Sie Daten in anderen Datenspeicher wie Tabellen, Dateien, Ordner und Dokumente. Nachdem Sie ein Dataset erstellt haben, können Sie es mit Aktivitäten in einem Verkaufspipeline verwenden. Beispielsweise kann ein Dataset ein Dataset/Ausgang von einer Kopie oder eine HDInsightHive Aktivität sein. Azure-Portal bietet Ihnen eine visuelle Layout aller Pipelines und Daten Eingaben und Ausgaben. Auf einen Blick sehen Sie alle Beziehungen und Abhängigkeiten von Ihrer Rohrleitungen für alle Datenquellen identisch, damit Sie immer wissen, woher Daten stammen, und wo geht's.

In Azure Data Factory können Sie Daten aus einem Dataset mithilfe von kopieren Aktivität in einer Verkaufspipeline abrufen.

> [AZURE.NOTE] Wenn Sie neu bei Azure Data Factory sind, finden Sie unter [Einführung in Azure Data Factory](data-factory-introduction.md) einen Überblick über die Daten Factory Azure Service. [Erstellen Ihrer erste Daten Factory](data-factory-build-your-first-pipeline.md) ein Lernprogramm zum Erstellen Ihrer ersten Daten Factory finden Sie unter. Die folgenden zwei Artikel angeben, dass Sie Hintergrundinformationen zum besseren Verständnis der in diesem Artikel erforderlichen. 

## <a name="define-datasets"></a>Definieren von datasets
Ein Dataset in Azure Data Factory ist wie folgt definiert: 


    {
        "name": "<name of dataset>",
        "properties": {
            "type": "<type of dataset: AzureBlob, AzureSql etc...>",
            "external": <boolean flag to indicate external data. only for input datasets>,
            "linkedServiceName": "<Name of the linked service that refers to a data store.>",
            "structure": [
                {
                    "name": "<Name of the column>",
                    "type": "<Name of the type>"
                }
            ],
            "typeProperties": {
                "<type specific property>": "<value>",
                "<type specific property 2>": "<value 2>",
            },
            "availability": {
                "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
                "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
            },
           "policy": 
            {      
            }
        }
    }

Die folgende Tabelle beschreibt die Eigenschaften in der obigen JSON:   

| Eigenschaft | Beschreibung | Erforderlich | Standard |
| -------- | ----------- | -------- | ------- |
| Namen | Name des Datasets. Benennungskonventionen finden Sie unter [Azure Daten Factory - Regeln benennen](data-factory-naming-rules.md) . | Ja | NV |
| Typ | Typ des Datasets. Geben Sie einen der Typen von Azure Daten Factory unterstützt (zum Beispiel: AzureBlob, AzureSqlTable). <br/><br/>Details finden Sie unter [Typ Dataset](#Type) . | Ja | NV |
| Struktur | Schema des dataset<br/><br/>Details finden Sie unter [Dataset Struktur](#Structure) Abschnitt. | Nein. | NV |
| typeProperties | Eigenschaften der gewählten Typs entspricht. Siehe Abschnitt " [Dataset Typ](#Type) " Details auf unterstützten Datentypen und ihre Eigenschaften. | Ja | NV |
| externe | Boolesche kennzeichnen Sie angeben, ob ein Dataset durch eine Daten Factory Verkaufspipeline oder nicht explizit erstellt wird.  | Nein | falsch | 
| Verfügbarkeit | Definiert das Verarbeitungsfenster oder Slices aufteilendes Modell für die Herstellung Dataset. <br/><br/>Details finden Sie unter [Dataset Verfügbarkeit](#Availability) Abschnitt. <br/><br/>Ausführliche Informationen zum Modell, [Planung und Ausführung](data-factory-scheduling-and-execution.md) finden Sie im Artikel segmentieren Dataset. | Ja | NV
| Richtlinie | Definiert die Kriterien oder die Bedingung, die die Segmente des Dataset erfüllt werden müssen. <br/><br/>Details finden Sie unter [Dataset Richtlinie](#Policy) Abschnitt. | Nein | NV |

## <a name="dataset-example"></a>Beispiel für DataSet
Im folgenden Beispiel stellt das Dataset eine Tabelle mit dem Namen **MyTable** in einer **SQL Azure-Datenbank**. 

    {
        "name": "DatasetSample",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": 
            {
                "tableName": "MyTable"
            },
            "availability": 
            {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

Beachten Sie die folgenden Punkte: 

- Typ wird auf AzureSqlTable festgelegt.
- Tabellenname Eigenschaft (spezifisch AzureSqlTable Typ) wird auf MyTable festgelegt.
- LinkedServiceName bezieht sich auf eine verknüpfte Dienst vom Typ AzureSqlDatabase. Finden Sie unter der Definition der folgende verknüpfte Dienst. 
- Verfügbarkeit Häufigkeit zum Tag und Intervall 1, was bedeutet, dass das Segment täglich erzeugt wird festgelegt ist.  

AzureSqlLinkedService ist wie folgt definiert:

    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "",
            "typeProperties": {
                "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
            }
        }
    }

In der obigen JSON: 

- Typ wird auf AzureSqlDatabase festgelegt.
- Eigenschaft ConnectionString gibt Informationen zum Herstellen einer SQL Azure-Datenbank an.  


Wie Sie sehen können, definiert der verknüpfte Dienst herstellen einer Verbindung mit einer SQL Azure-Datenbank. Das Dataset definiert, welche Tabelle als eine Ausgang für die Aktivität in einer Verkaufspipeline verwendet wird. Abschnitt Aktivität in der [Verkaufspipeline](data-factory-create-pipelines.md) JSON gibt an, ob das Dataset als eine Eingabe- oder Dataset verwendet wird.


> [AZURE.IMPORTANT] Es sei denn, ein Dataset von Azure Daten Factory erstellt werden, sollten sie als **externe**markiert. Diese Einstellung gilt im Allgemeinen Eingaben der ersten Aktivität in einer Verkaufspipeline.   

## <a name="a-nametypea-dataset-type"></a><a name="Type"></a>Typ "DataSet"
Die unterstützten Datenquellen und Dataset-Typen ausgerichtet werden. Finden Sie im Artikel [Daten Bewegung Aktivitäten](data-factory-data-movement-activities.md#supported-data-stores) Informationen zu Datentypen und Konfiguration von Datasets referenzierte Themen. Beispielsweise, wenn Sie Daten aus einer SQL Azure-Datenbank verwenden, klicken Sie auf Azure SQL-Datenbank in der Liste der unterstützten Datenspeicher, detaillierte Informationen anzuzeigen.  

## <a name="a-namestructureadataset-structure"></a><a name="Structure"></a>DataSet-Struktur
Im Abschnitt **Struktur** definiert das Schema des Datasets. Sie enthält eine Auflistung von Namen und Datentypen der Spalten aus.  Im folgenden Beispiel wurde das Dataset drei Spalten Slicetimestamp, Projektname und Pageviews und sind vom Typ: Zeichenfolge, Zeichenfolge und Dezimaltrennzeichen Hilfethemas.

    structure:  
    [ 
        { "name": "slicetimestamp", "type": "String"},
        { "name": "projectname", "type": "String"},
        { "name": "pageviews", "type": "Decimal"}
    ]

## <a name="a-nameavailabilitya-dataset-availability"></a><a name="Availability"></a>Verfügbarkeit von DataSet
In einem Dataset im Abschnitt **Verfügbarkeit** der definiert das Verarbeitungsfenster (stündlich, täglich, wöchentlich usw.) oder das Slices aufteilendes Modell für das Dataset. Weitere Details im Dataset segmentieren und Abhängigkeit Modell finden Sie unter [Planung und Ausführung](data-factory-scheduling-and-execution.md) diesem Artikel. 

Im folgenden Abschnitt der Verfügbarkeit gibt an, dass das Ausgabe Dataset erzeugten stündlich (oder) Eingabe Dataset stündlich verfügbar ist:

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 1   
    }

Die folgende Tabelle beschreibt die Eigenschaften, die Sie im Abschnitt Verfügbarkeit der verwenden können: 

| Eigenschaft | Beschreibung | Erforderlich | Standard |
| -------- | ----------- | -------- | ------- |
| Häufigkeit | Gibt die Zeiteinheit für Dataset Segment Herstellung.<br/><br/>**Unterstützte Häufigkeit**: Minute, Stunde, Tag, Woche, Monat | Ja | NV |
| Intervall | Gibt einen Multiplikator für Häufigkeit<br/><br/>"X Häufigkeitsintervall" bestimmt, wie oft das Segment erzeugt wird.<br/><br/>Wenn Sie das Dataset zu stündlich Slices aufgeteilt werden benötigen, legen Sie **Häufigkeit** auf und **Intervall** **1** **Stunde**.<br/><br/>**Hinweis:** Wenn Sie die Häufigkeit als Minute angeben, empfehlen wir, dass Sie das Intervall auf nicht weniger als 15 festlegen | Ja | NV |
| Formatvorlage | Gibt an, ob das Segment am Anfang/Ende des Intervalls erstellt werden muss.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Wenn Häufigkeit auf Month festgelegt wird, und Formatvorlage auf EndOfInterval festgelegt ist, wird das Segment am letzten Tag des Monats erstellt. Wenn die Formatvorlage auf StartOfInterval festgelegt ist, wird das Segment am ersten Tag des Monats erstellt.<br/><br/>Wenn Häufigkeit auf Tag festgelegt ist, und Formatvorlage auf EndOfInterval festgelegt ist, wird das Segment in der letzten Stunde des Tages erstellt.<br/><br/>Wenn Häufigkeit auf Stunde festgelegt ist, und Formatvorlage auf EndOfInterval festgelegt ist, wird das Segment am Ende der Stunde erstellt. Beispielsweise wird das Segment für ein Segment für 1 – 2 Uhr Zeitraum, um 14 Uhr erstellt. | Nein | EndOfInterval |
| anchorDateTime | Definiert die absolute Position Zeitpunkt von Scheduler verwendet, um das Dataset Segment Grenzen zu berechnen. <br/><br/>**Hinweis:** Wenn die AnchorDateTime Datumsteilen aufweist, die feiner unterteilt als die Häufigkeit sind, und klicken Sie dann die feiner Teile ignoriert werden. <br/><br/>Wenn das **Intervall** **stündlich** wird beispielsweise (Häufigkeit: Stunde und jedes Intervall: 1) und der **AnchorDateTime** enthält, **Minuten und Sekunden**, und klicken Sie dann die **Minuten und Sekunden** Teile der AnchorDateTime werden ignoriert. | Nein | 01/01/0001 |
| Versatz | Diese Zeitspanne, an dem das Start- und Enddatum der alle Dataset Segmente verschoben werden. <br/><br/>**Hinweis:** Wenn AnchorDateTime und den Versatz angegeben sind, ist das Ergebnis der kombinierte UMSCHALT aus. | Nein | NV |

### <a name="offset-example"></a>Bereich.verschieben Beispiel

Täglich slices, beginnen bei 6 Uhr anstelle der standardmäßigen Mitternacht verstrichen sind. 

    "availability":
    {
        "frequency": "Day",
        "interval": 1,
        "offset": "06:00:00"
    }

Die **Häufigkeit** ist zum **Tag** und **Intervall** **1** (einmal täglich) festgelegt ist: Wenn Sie das Segment am 6 Uhr statt der standardmäßigen gleichzeitig produziert werden soll: 00 Uhr. Denken Sie daran, dass diesmal UTC-Zeit ist. 

## <a name="anchordatetime-example"></a>Beispiel für anchorDateTime

**Beispiel:** 23 Stunden Dataset Segmente auf 2007 starten-04-19T08:00:00

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 23, 
        "anchorDateTime":"2007-04-19T08:00:00"  
    }

## <a name="offsetstyle-example"></a>Beispiel für einen Abstand/Stil

Wenn Sie Dataset monatlichen auf bestimmte Datum und Uhrzeit benötigen (Nehmen Sie an auf 3rd des Monats um 8:00 Uhr), verwenden Sie die Kategorie **Offset** legen Sie das Datum und Uhrzeit, es ausgeführt werden soll. 

    {
      "name": "MyDataset",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "availability": {
          "frequency": "Month",
          "interval": 1,
          "offset": "3.08:10:00",
          "style": "StartOfInterval"
        }
      }
    }


## <a name="a-namepolicyadataset-policy"></a><a name="Policy"></a>DataSet Richtlinie

Im Abschnitt **Richtlinie** in Dataset Definition definiert die Kriterien oder die Bedingung, die die Segmente des Dataset erfüllt werden müssen.

### <a name="validation-policies"></a>Überprüfung Richtlinien

| Name der Richtlinie | Beschreibung | Angewendet | Erforderlich | Standard |
| ----------- | ----------- | ---------- | -------- | ------- |
| minimumSizeMB | Überprüft, dass die Daten in einer **Azure BLOB-** die Mindestgröße (in MB) erfüllt. | Azure Blob | Nein | NV |
|minimumRows | Überprüft, ob die Daten in einer **SQL Azure-Datenbank** oder einer **Azure Table** die minimale Anzahl von Zeilen enthält. | <ul><li>SQL Azure-Datenbank</li><li>Azure Table</li></ul> | Nein | NV

#### <a name="examples"></a>Beispiele

**MinimumSizeMB:**

    "policy":
    
    {
        "validation":
        {
            "minimumSizeMB": 10.0
        }
    }

**minimumRows**

    "policy":
    {
        "validation":
        {
            "minimumRows": 100
        }
    }

### <a name="external-datasets"></a>Externe datasets

Externe Datasets sind diejenigen, die nicht von einer laufenden Verkaufspipeline in die Factory Daten erzeugt werden. Wenn das Dataset als **externe**markiert ist, möglicherweise die Richtlinie **ExternalData** definiert werden, um das Verhalten der Verfügbarkeit Segment Dataset beeinflussen. 

Es sei denn, ein Dataset von Azure Daten Factory erstellt werden, sollten sie als **externe**markiert. Diese Einstellung gilt die Eingaben der ersten Aktivität in einer Verkaufspipeline im Allgemeinen aus, es sei denn, Aktivität oder Verkaufspipeline verketten verwendet wird. 

| Namen | Beschreibung | Erforderlich | Standardwert  |
| ---- | ----------- | -------- | -------------- |
| dataDelay | Zeit, die Überprüfung auf die Verfügbarkeit der externen Daten für das angegebene Segment verzögern. Wenn die Daten stündlich verfügbar sein soll, kann beispielsweise das Kontrollkästchen Prüfen steht die externen Daten und die entsprechenden Segments wird sofort verzögert werden mithilfe von DataDelay.<br/><br/>Gilt nur für die aktuelle Zeit.  Angenommen, wenn es 1:00 Uhr sofort ist und dieser Wert 10 Minuten ist, beginnt die Überprüfung um 13:10.<br/><br/>Diese Einstellung wirkt sich nicht Segmente in der Vergangenheit (Segmente mit Segment Endzeit + DataDelay < jetzt) ohne Verzögerung verarbeitet werden.<br/><br/>Zeit größer ist als 23:59, die Stunden müssen im day.hours:minutes:seconds Format angegeben. Beispielsweise verwenden Sie 24 Stunden, nicht, um 24:00:00; Verwenden Sie stattdessen 1.00:00:00. Wenn Sie 24:00:00 verwenden, wird es als 24 Tage (24.00:00:00) behandelt. Geben Sie für 1 Tag und 4 Stunden 1:04:00:00 an. | Nein | 0 |
| retryInterval | Wiederholen Sie die Wartezeit zwischen einem Fehler und dem nächsten Versuch aus. Gilt für präsentieren Zeit. Wenn der vorherigen Fehler beim versuchen, warten wir dies lange nach dem letzten Versuch. <br/><br/>Es ist 1:00 Uhr sofort beginnen wir die erste testen. Ist die Dauer, die die erste Überprüfung 1 Minute und der Vorgang fehlgeschlagen ist, wird die nächste Wiederholung bei 1:00 + 1 min (Dauer) + 1min (Wiederholungsintervalls) = 1:02 Uhr. <br/><br/>Für Segmente in der Vergangenheit erfolgt keine Verzögerung. Die Wiederholung wird sofort ausgeführt. | Nein | 00:01:00 (1 Minute) | 
| "RetryTimeout" | Das Timeout für jeden Versuch "Wiederholen".<br/><br/>Wenn diese Eigenschaft auf 10 Minuten festgelegt ist, muss die Überprüfung innerhalb von 10 Minuten abgeschlossen sein soll. Wenn es mehr als 10 Minuten zum Durchführen der Überprüfung dauert, das die "Wiederholen" Timeout.<br/><br/>Wenn alle Versuche für die Überprüfung Zeit überschreitet, wird das Segment als TimedOut markiert. | Nein | 00:10:00 (10 Minuten) |
| maximumRetry | Die Anzahl, wie oft die Verfügbarkeit von externen Daten überprüfen. Der zulässige Höchstwert ist 10. | Nein | 3 | 

## <a name="scoped-datasets"></a>Suchbegriffs datasets
Sie können Datasets erstellen, die auf einer Verkaufspipeline beschränkt sind, mithilfe der Eigenschaft **Datasets** . Diese Datasets kann nur von Aktivitäten innerhalb dieser Verkaufspipeline aber nicht von Aktivitäten in anderen Rohrleitungen verwendet werden. Im folgenden Beispiel wird eine Verkaufspipeline mit zwei Datasets - InputDataset-Rdc und OutputDataset-Rdc - innerhalb der Verkaufspipeline verwendet werden:  

> [AZURE.IMPORTANT] Suchbegriffs Datasets werden nur mit einmaligen Rohrleitungen (**PipelineMode** auf **OneTime**festgelegt) unterstützt. Details finden Sie unter [Onetime Verkaufspipeline](data-factory-scheduling-and-execution.md#onetime-pipeline) .

    {
        "name": "CopyPipeline-rdc",
        "properties": {
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": false
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "InputDataset-rdc"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset-rdc"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1,
                        "style": "StartOfInterval"
                    },
                    "name": "CopyActivity-0"
                }
            ],
            "start": "2016-02-28T00:00:00Z",
            "end": "2016-02-28T00:00:00Z",
            "isPaused": false,
            "pipelineMode": "OneTime",
            "expirationTime": "15.00:00:00",
            "datasets": [
                {
                    "name": "InputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "InputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/input",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": true,
                        "policy": {}
                    }
                },
                {
                    "name": "OutputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "OutputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/output",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": false,
                        "policy": {}
                    }
                }
            ]
        }
    }