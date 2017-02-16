<properties 
    pageTitle="Schwein Aktivität" 
    description="Erfahren Sie, wie Sie die Aktivität Schwein in einer Factory Azure-Daten verwenden können, eine auf – Nachfrage/Ihre eigenen HDInsight Cluster Schwein Skripts auszuführen." 
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
    ms.date="08/31/2016" 
    ms.author="shlo"/>

# <a name="pig-activity"></a>Schwein Aktivität
> [AZURE.SELECTOR]
[Struktur](data-factory-hive-activity.md)  
[Schwein](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Computer Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
[Daten dem Analytics U SQL](data-factory-usql-activity.md)
[benutzerdefinierten .NET](data-factory-use-custom-activities.md)

Die Aktivität HDInsight Schwein in einer Daten Factory [Verkaufspipeline](data-factory-create-pipelines.md) führt Schwein Abfragen für [Eigene](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) oder [bei Bedarf](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows, Linux-basierten HDInsight Cluster an. In diesem Artikel wird im Artikel [Daten Transformationsaktivitäten](data-factory-data-transformation-activities.md) , die bietet eine allgemeine Übersicht über Datentransformation und der unterstützten Transformationsaktivitäten erstellt.

## <a name="syntax"></a>Syntax

    {
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "Pig Activity",
                "description": "description",
                "type": "HDInsightPig",
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
                    "script": "Pig script",
                    "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                    "defines": {
                        "param1": "param1Value"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
            }
        ]
      }
    }

## <a name="syntax-details"></a>Syntax-details

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Namen | Name der Aktivität | Ja
Beschreibung | Text zur Beschreibung, wofür die Aktivität verwendet wird | Nein
Typ | HDinsightPig | Ja
Eingaben | Eine oder mehrere Eingaben durch die Aktivität Schwein verbraucht | Nein
Ausgaben | Eine oder mehrere Ausgaben von der Aktivität Schwein erzeugt | Ja
linkedServiceName | Verweis auf HDInsight Cluster als verknüpfte Dienst in Daten Factory registriert | Ja
Skript | Geben Sie den Schwein Skript inline | Nein
Skriptpfad | Geben Sie den Pfad zu der Datei, und speichern Sie das Skript Schwein in einer Azure Blob-Speicher. Verwenden Sie "Skripts" oder 'ScriptPath' Eigenschaft. Beide können nicht zusammen verwendet werden. Der Dateiname beachtet werden. | Nein
definiert | Geben Sie Parameter als Schlüssel/Wert-Paare zum Verweisen auf innerhalb des Schwein Skripts | Nein

## <a name="example"></a>Beispiel

Sehen Sie sich ein Beispiel für Spiel Protokolle Analytics, in dem Sie möchten, die Zeit, Spieler spielen gestartet, indem Sie Ihr Unternehmen zu identifizieren.
 
Das folgende Beispielprotokoll Spiel ist ein Semikolon (;) getrennt-Datei. Sie enthält die folgenden Felder – ID, SessionStart, Dauer, SrcIPAddress und GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

Das **Skript Schwein** , diese Daten zu verarbeiten:

    PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    
    GroupProfile = Group PigSampleIn all;
    
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    
    Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');

Zum Ausführen dieser Skripts Schwein in einer Daten Factory Verkaufspipeline folgendermaßen Sie vor:

1. Erstellen einer verknüpften ab, um [Eigene HDInsight berechnen Cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) registrieren oder [bei Bedarf HDInsight berechnen Cluster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)konfigurieren. Rufen Sie uns diese verknüpfte Dienst **HDInsightLinkedService**.
2.  Erstellen einer [verknüpften Dienst](data-factory-azure-blob-connector.md) zum Konfigurieren der Verbindungs mit Azure Blob-Speicher die Daten gehostet werden. Rufen Sie uns diese verknüpfte Dienst **StorageLinkedService**.
3.  [Datasets](data-factory-create-datasets.md) zeigt die Eingabe und die Ausgabedaten erstellen. Lassen Sie uns anrufen des Eingabe-Dataset **PigSampleIn** und die Ausgabe Dataset **PigSampleOut**.
4.  Kopieren Sie die Abfrage Schwein in einer Datei den in Schritt 2 # konfiguriert Azure BLOB-Speicher. Ist der Azure-Speicher, der die Daten hostet der abweicht, der die Abfragedatei hostet, erstellen Sie eine separate Azure verknüpft Speicherdienst aus. Schlagen Sie in der verknüpfte Dienst in der Aktivitätskonfiguration. Verwenden Sie **ScriptPath **, um den Pfad zur Skriptdatei Schwein und **ScriptLinkedService**anzugeben. 
    
    > [AZURE.NOTE] Sie können auch den Schwein Skript Inline in der Aktivitätsdefinition mithilfe der Eigenschaft **Skript** bereitstellen. Allerdings empfehlen nicht dieser Ansatz alle Sonderzeichen in das Skript muss mit Escapezeichen versehen werden und kann dazu führen, dass das Debuggen von Problemen. Die bewährte Methode ist, führen Sie Schritt 4 #.
5. Erstellen der Verkaufspipeline die Aktivität HDInsightPig. Diese Aktivität verarbeitet die eingegebenen Daten durch Ausführen Schwein Skripts auf HDInsight Cluster an.

        {
          "name": "PigActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                  {
                    "name": "PigSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "PigSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
              }
            ]
          }
        } 
6. Bereitstellen der Verkaufspipeline an. Finden Sie im Artikel [Erstellen Pipelines](data-factory-create-pipelines.md) Details. 
7. Überwachen der Verkaufspipeline die Überwachung und Verwaltung Datenansichten von Factory verwenden. Finden Sie unter [Überwachen und Verwalten von Daten Factory Pipelines](data-factory-monitor-manage-pipelines.md) Artikel Details.

## <a name="specifying-parameters-for-a-pig-script"></a>Angeben von Parametern für ein Schwein-Skript 

Im folgenden Beispiel: Spiel Protokolle täglich in Azure BLOB-Speicher aufgenommen und in einem Ordner partitionierten basierend auf Datum und Uhrzeit gespeichert sind. Das Skript Schwein parametrisieren und den Eingabewerte Ordnerspeicherort dynamisch während der Laufzeit übergeben und erzeugt auch die Ausgabe mit Datum und Uhrzeit aufgeteilt werden soll.
 
Wenn parametrisierte Schwein Skript verwenden möchten, führen Sie folgende Schritte aus:

- Definieren Sie die Parameter in **definiert**.

        {
            "name": "PigActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "PigActivitySample",
                    "type": "HDInsightPig",
                    "inputs": [
                        {
                            "name": "PigSampleIn"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "PigSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0: %M}/dayno={0: %d}/',SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    }
                }
            ]
          }
        }  
      
- Finden Sie in das Skript Schwein in der Parameter '**$parameterName**' mit, wie im folgenden Beispiel gezeigt:

        PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);   
        GroupProfile = Group PigSampleIn all;       
        PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);      
        Store PigSampleOut into '$Output' USING PigStorage (','); 


## <a name="see-also"></a>Siehe auch
- [Struktur Aktivität](data-factory-hive-activity.md)
- [MapReduce Aktivität](data-factory-map-reduce.md)
- [Hadoop Streaming Aktivität](data-factory-hadoop-streaming-activity.md)
- [Rufen Sie Spark-Programme](data-factory-spark.md)
- [Rufen Sie R Skripts](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)


