<properties 
    pageTitle="Struktur Aktivität" 
    description="Erfahren Sie, wie Sie die Struktur Aktivität in einer Factory Azure-Daten verwenden können, Struktur Abfragen für eine auf – Nachfrage/Ihre eigenen HDInsight Cluster ausgeführt." 
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
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="hive-activity"></a>Struktur Aktivität
> [AZURE.SELECTOR]
[Struktur](data-factory-hive-activity.md)  
[Schwein](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Computer Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
[Daten dem Analytics U SQL](data-factory-usql-activity.md)
[benutzerdefinierten .NET](data-factory-use-custom-activities.md)

Die Struktur HDInsight Aktivität in einer Daten Factory [Verkaufspipeline](data-factory-create-pipelines.md) führt Struktur Abfragen für [Eigene](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) oder [bei Bedarf](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows, Linux-basierten HDInsight Cluster. In diesem Artikel wird im Artikel [Daten Transformationsaktivitäten](data-factory-data-transformation-activities.md) , die bietet eine allgemeine Übersicht über Datentransformation und der unterstützten Transformationsaktivitäten erstellt.

## <a name="syntax"></a>Syntax

    {
        "name": "Hive Activity",
        "description": "description",
        "type": "HDInsightHive",
        "inputs": [
          {
            "name": "input tables"
          }
        ],
        "outputs": [
          {
            "name": "output tables"
          }
        ],
        "linkedServiceName": "MyHDInsightLinkedService",
        "typeProperties": {
          "script": "Hive script",
          "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
          "defines": {
            "param1": "param1Value"
          }
        },
       "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
    }
    
## <a name="syntax-details"></a>Syntax-details

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Namen | Name der Aktivität | Ja
Beschreibung | Text zur Beschreibung, wofür die Aktivität verwendet wird | Nein
Typ | HDinsightHive | Ja
Eingaben | Durch die Struktur Aktivität verbraucht Eingaben | Nein
Ausgaben | Von der Struktur Aktivität erzeugt Ausgaben | Ja 
linkedServiceName | Verweis auf HDInsight Cluster als verknüpfte Dienst in Daten Factory registriert | Ja 
Skript | Geben Sie den Struktur Skript inline | Nein
Skriptpfad | Speichern Sie das Skript Struktur in einer Azure Blob-Speicher, und geben Sie den Pfad zu der Datei. Verwenden Sie "Skripts" oder 'ScriptPath' Eigenschaft. Beide können nicht zusammen verwendet werden. Der Dateiname beachtet werden. | Nein 
definiert | Geben Sie Parameter als Schlüssel/Wert-Paare zum Verweisen auf innerhalb des Struktur Skripts mit 'Hiveconf'  | Nein

## <a name="example"></a>Beispiel

Sehen Sie sich ein Beispiel für Spiel Protokolle Analytics, in dem Sie möchten, die Zeit, die Benutzer spielen gestartet, indem Sie Ihr Unternehmen zu identifizieren. 

Das folgende Protokoll ist ein Beispiel für Spiel Protokoll, der ein Semikolon (`,`) getrennt und enthält die folgenden Felder – ID, SessionStart, Dauer, SrcIPAddress und GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

Die **Struktur Skript** , diese Daten zu verarbeiten:

    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID       string, 
        SessionStart    string, 
        Duration        int, 
        SrcIPAddress    string, 
        GameType        string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 
    
    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (   
        ProfileID   string, 
        Duration    int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';
    
    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID

Zum Ausführen dieser Skripts Struktur in einer Verkaufspipeline Daten Factory müssen Sie die folgenden Aktionen ausführen

1. Erstellen einer verknüpften ab, um [Eigene HDInsight berechnen Cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) registrieren oder [bei Bedarf HDInsight berechnen Cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)konfigurieren. Rufen Sie uns diese verknüpfte Dienst "HDInsightLinkedService" ein.
2. Erstellen einer [verknüpften Dienst](data-factory-azure-blob-connector.md) zum Konfigurieren der Verbindungs mit Azure Blob-Speicher die Daten gehostet werden. Lassen Sie uns anrufen dieser verknüpfte Dienst "StorageLinkedService"
3. [Datasets](data-factory-create-datasets.md) zeigt die Eingabe und die Ausgabedaten erstellen. Lassen Sie uns anrufen des Eingabe-Dataset "HiveSampleIn" und dem Ergebnis Dataset "HiveSampleOut"
4. Kopieren Sie die Struktur Abfrage als Datei zu Azure BLOB-Speicher konfiguriert in Schritt #2. ist der Speicher für die Daten gehostet werden anders als Host für diese Abfragedatei, erstellen Sie eine separate Azure verknüpft Speicherdienst und in die Aktivität darauf verweisen. Verwenden Sie **ScriptPath **, um den Pfad zum Abfrage-Strukturdatei und **ScriptLinkedService** Azure-Speicher an, der die Skriptdatei enthält anzugeben. 

    > [AZURE.NOTE] Sie können das Struktur Skript Inline in der Aktivitätsdefinition auch mithilfe der Eigenschaft **Skript** bereitstellen. Wir empfehlen nicht dieser Ansatz alle Sonderzeichen in das Skript innerhalb der JSON-Dokument Anforderungen Escapezeichen eingefügt werden müssen, und kann dazu führen, dass das Debuggen von Problemen. Die bewährte Methode ist, führen Sie Schritt 4 #.
5.  Erstellen Sie eine Verkaufspipeline die Aktivität HDInsightHive. Die Aktivität Prozesse/kann die Daten.

        {
          "name": "HiveActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                  {
                    "name": "HiveSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "HiveSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
              }
            ]
          }
        }

6.  Bereitstellen der Verkaufspipeline an. Finden Sie im Artikel [Erstellen Pipelines](data-factory-create-pipelines.md) Details. 
7.  Überwachen der Verkaufspipeline die Überwachung und Verwaltung Datenansichten von Factory verwenden. Finden Sie unter [Überwachen und Verwalten von Daten Factory Pipelines](data-factory-monitor-manage-pipelines.md) Artikel Details. 


## <a name="specifying-parameters-for-a-hive-script"></a>Angeben von Parametern für eine Struktur-Skript  
In diesem Beispiel Spiel Protokolle täglich in Azure BLOB-Speicher aufgenommen werden und in einem Ordner mit Datum und Uhrzeit aufgeteilt gespeichert sind. Das Skript Struktur parametrisieren und den Eingabewerte Ordnerspeicherort dynamisch während der Laufzeit übergeben und erzeugt auch die Ausgabe mit Datum und Uhrzeit aufgeteilt werden soll.

Gehen Sie folgendermaßen vor, um parametrisierte Struktur Skript verwenden

- Definieren Sie die Parameter in **definiert**.

        {
            "name": "HiveActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "HiveActivitySample",
                    "type": "HDInsightHive",
                    "inputs": [
                        {
                            "name": "HiveSampleIn"
                          }
                    ],
                    "outputs": [
                        {
                            "name": "HiveSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        }
                    }
                }
            ]
          }
        }

- Finden Sie in das Skript Struktur in den Parameter **${Hiveconf:parameterName}**verwenden. 

        DROP TABLE IF EXISTS HiveSampleIn; 
        CREATE EXTERNAL TABLE HiveSampleIn 
        (
            ProfileID   string, 
            SessionStart    string, 
            Duration    int, 
            SrcIPAddress    string, 
            GameType    string
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 
        
        DROP TABLE IF EXISTS HiveSampleOut; 
        CREATE EXTERNAL TABLE HiveSampleOut 
        (
            ProfileID   string, 
            Duration    int
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';
        
        INSERT OVERWRITE TABLE HiveSampleOut
        Select 
            ProfileID,
            SUM(Duration)
        FROM HiveSampleIn Group by ProfileID


## <a name="see-also"></a>Siehe auch
- [Schwein Aktivität](data-factory-pig-activity.md)
- [MapReduce Aktivität](data-factory-map-reduce.md)
- [Hadoop Streaming Aktivität](data-factory-hadoop-streaming-activity.md)
- [Rufen Sie Spark-Programme](data-factory-spark.md)
- [Rufen Sie R Skripts](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)









