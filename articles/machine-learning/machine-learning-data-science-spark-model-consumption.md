<properties
    pageTitle="Punktzahl Spark erstellt maschinellen Learning Modelle | Microsoft Azure"
    description="So Punktzahl Learning-Modelle, die in Azure BLOB-Speicher (WASB) gespeichert wurden."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="score-spark-built-machine-learning-models"></a>Maschinelle Spark erstellt Learning Modelle Punktzahl 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

In diesem Thema beschrieben, wie Sie den Computer (ML) learning-Modelle laden, die mit Spark MLlib erstellt wurden und gehörende Kehrmatrix Azure BLOB-Speicher (WASB), und wie Sie diese mit Datasets Punktzahl, die auch in WASB gespeichert wurden. Es zeigt so vorab Verarbeitung der Eingabedaten, Features, die mit den Funktionen Indizierung und codieren im Toolkit MLlib transformieren und So erstellen Sie ein Objekt beschriftete Punkt, die als Eingabe für ML Modelle mit bewerten verwendet werden können. Zur Bewertung verwendeten Modelle beziehen Sie lineare Regression, logistische Regression, zufällige Gesamtstruktur-Modelle und Farbverlauf verstärken Struktur Modelle.


## <a name="prerequisites"></a>Erforderliche Komponenten

1. Sie benötigen ein Azure-Konto und einem HDInsight Spark Sie müssen eine HDInsight 3.4 Spark 1.6 Cluster diese exemplarische Vorgehensweise. Finden Sie die [Übersicht der Daten Wissenschaft Spark auf Azure HDInsight mit](machine-learning-data-science-spark-overview.md) Anweisungen zum erfüllen diese Anforderungen an. Dieses Thema enthält auch eine Beschreibung der hier verwendete NYC 2013 Taxi-Daten und Anweisungen zum Ausführen von Code aus einem Notizbuch Jupyter im Cluster Spark. Das **pySpark-machine-learning-data-science-spark-model-consumption.ipynb** Notizbuch, das die Codebeispielen in diesem Thema enthält steht in [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark).

2. Sie müssen auch den Computer lernen, indem Sie das [Durchsuchen von Daten und Modellierung mit Spark](machine-learning-data-science-spark-data-exploration-modeling.md) Thema hier bewertet werden Modelle erstellen.   


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
 

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Setup: Speicherpositionen, Bibliotheken und der voreingestellten Spark Kontext

Spark kann zum Lesen und schreiben zu einer Azure Speicher Blob (WASB). Damit Ihre vorhandene Daten gespeichert können es mithilfe von Spark und die Ergebnisse erneut in WASB gespeicherten verarbeitet werden.

Um Modelle oder Dateien in WASB zu speichern, muss der Pfad ordnungsgemäß angegeben werden muss. Der standardmäßige Container angefügter Spark Cluster kann mit einem Pfad, beginnend mit verwiesen werden: *"Wasb / /"*. Im folgenden Beispiel gibt die Position der Daten zu lesenden und den Pfad für das Modell Speicherverzeichnis, in dem die Ausgabe Modell gespeichert werden. 


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>Festlegen von Verzeichnispfade für Speicherorte in WASB

Modelle in gespeichert werden: "Wasb: / / / Benutzer/Remoteuser/NYCTaxi/Datenmodellen". Wenn dieser Pfad nicht richtig eingestellt ist, werden die Modelle zur Bewertung nicht geladen.

Der bewertete Ergebnisse gespeichert wurden: "Wasb: / / / Benutzer/Remoteuser/NYCTaxi/ScoredResults". Wenn der Pfad zum Ordner falsch ist, werden die Ergebnisse in diesem Ordner nicht gespeichert.   


>[AZURE.NOTE] Speicherorte der Pfad können kopiert und in den Platzhaltern in diesen Code aus der Ausgabe die letzte Zelle des Notizbuchs **machine-learning-data-science-spark-data-exploration-modeling.ipynb** eingefügt werden.   


Hier ist der Code Directory Pfade festlegen: 

    # LOCATION OF DATA TO BE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";
    
    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 
    
    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 
    
    # FILE LOCATIONS FOR THE MODELS TO BE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

**ERGEBNIS:**

DateTime.DateTime (2016, 4, 25, 23, 56, 19, 229403)


### <a name="import-libraries"></a>Importieren von Bibliotheken

Festlegen Sie Spark Kontext und importieren Sie die erforderlichen Bibliotheken mit den folgenden code

    #IMPORT LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a>Voreingestellte Spark Kontext bereitstellen und PySpark magics

Die PySpark Kernels, die mit Jupyter-Notizbüchern bereitgestellt werden müssen einen vordefinierten Kontext. Damit Sie müssen nicht das Spark festlegen oder Struktur Kontexten explizit, bevor Sie mit der Arbeit mit der Anwendung entwickeln. Dies sind die standardmäßig für Sie verfügbar. Diese Kontexte sind:

- SC – für Spark 
- zur SqlContext - für Struktur

PySpark Kernel enthält einige vordefinierte "magics", welche sind spezielle Befehle, die Sie mit aufrufen können %. Es gibt zwei solche Befehle, die in den folgenden Codebeispielen verwendet werden.

- **% lokale** Angegeben haben, dass der Code in nachfolgenden Zeilen lokal ausgeführt wird. Code muss gültigen Python-Code sein.
- **% Sql -o<variable name>** 
- Führt eine Abfrage Struktur gegen die SqlContext. Wenn der Parameter -o übergeben wird, ist das Ergebnis der Abfrage im beibehalten der % lokalen Python Kontext als eine Pandas Dataframe.
 

Für Weitere Informationen zu den Kernels für Jupyter Notizbüchern und den vordefinierten ", die magics" diese bereitgestellt wird, finden Sie unter [Kernels für Jupyter Notizbücher mit HDInsight Spark Linux Cluster auf HDInsight verfügbar](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Aufnahme von Daten, und erstellen Sie einen Datenrahmen bereinigt

Dieser Abschnitt enthält den Code für eine Reihe von Aufgaben, die Daten bewertet werden Aufnahme erforderlich. In einer verknüpften 0,1 % Stichprobe der Taxi Geschäftsreise und Fahrpreis Datei (gespeichert als TSV-Datei), formatieren die Daten gelesen und erstellt dann einen Datenrahmen aufräumen.

Die Taxi Geschäftsreise und Fahrpreis Dateien wurden beigetreten basierend auf dem Verfahren den: [das Team Daten Wissenschaft Prozess in Aktion: HDInsight Hadoop Cluster mit](machine-learning-data-science-process-hive-walkthrough.md) Thema.

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
        
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_test_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)
    
    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ERGEBNIS:**

Über Zelle Ausführung: 46.37 Sekunden


## <a name="prepare-data-for-scoring-in-spark"></a>Vorbereiten der Daten für die Punkte in Spark 

Dieser Abschnitt listet wie indizieren, codieren und skalieren kategorisierte Funktionen, die sie zur Verwendung in MLlib überwacht Learning Algorithmen für die Einstufung und Regression vorzubereiten.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Bereitstellen von Transformation: indizieren und codieren kategorisierten Features für die Eingabe in Modelle zur Bewertung 

In diesem Abschnitt zeigt, wie indizieren kategorisierte Daten mithilfe einer `StringIndexer` und codieren Features ohne `OneHotEncoder` Eingabemethoden in die Modelle.

Die [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) codiert eine Zeichenfolgenspalte Beschriftungen, um eine Spalte mit der Bezeichnung Indizes. Die Indizes werden nach Bezeichnung Häufigkeiten sortiert. 

Der [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) ordnet eine Spalte mit der Bezeichnung Indizes eine Spalte mit binäre Vektoren, höchstens ein einzelnes Chi-Wert. Diese Codierung ermöglicht Algorithmen, die erwarten fortlaufender Werte Features wie logistische Regression, kategorisierten Features angewendet werden.
    
    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()
    
    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)
    
    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)
    
    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)
    
    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ERGEBNIS:**

Über Zelle Ausführung: 5.37 Sekunden


### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>Erstellen von RDD Objekte mit Feature Arrays für die Eingabe in Datenmodellen

Dieser Abschnitt enthält Code, der zeigt, wie kategorisierten Textdaten als Objekt RDD indizieren und eine Tastaturkürzel codiert wird, sodass es zum Schulen und Testen logistische Regression MLlib und Struktur-basierten Modelle verwendet werden kann. Die indizierten Daten werden in [Robuste verteilt Dataset (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) Objekte gespeichert. Hierbei handelt es sich um die grundlegende Abstraktion in Spark. Objekt RDD stellt eine unveränderliche, partitionierte Sammlung von Elementen, die auf parallel mit Spark betrieben werden können.

Es enthält auch Code, der wird gezeigt, wie Daten mit Skalieren der `StandardScalar` von MLlib zur Verwendung in lineare Regression mit Stochastic Farbverlauf Unterlänge (SGD), eine beliebte Algorithmus für eine Vielzahl von maschinellen Learning Modelle Schulung bereitgestellt. Der [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) wird zum Skalieren der Features, die Varianz der Maßeinheit verwendet. Featureskalierung, auch bekannt als Normalisierung von Daten wird sichergestellt, dass es sich bei Features ohne stark Datenparameter Werte werden in der Ziel-Funktion nicht angegebenen übermäßige abzuwägen. 


    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)
    
    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)
    
    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)
    
    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**ERGEBNIS:**

Über Zelle Ausführung: 11.72 Sekunden


## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a>Mit dem Logistic Regressionsmodell Punktzahl und speichern Sie die Ausgabe an BLOB-

Der Code in diesem Abschnitt zeigt so Laden ein Logistic Regression-Modell, das in Azure Blob-Speicher gespeichert wurde, und verwenden sie vorhersagen, und zwar unabhängig davon, ob ein Tipp auf einer Reise Taxi bezahlt wird, erzielen es mit standardisierten Klassifizierung Kennzahlen, und klicken Sie dann speichern und die Ergebnisse auf Blob-Speicher darstellen. Die bewertete Ergebnisse werden im RDD Objekte gespeichert. 


    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))
    
    ## SAVE SCORED RESULTS (RDD) TO BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**ERGEBNIS:**

Über Zelle Ausführung: 19.22 Sekunden


## <a name="score-a-linear-regression-model"></a>Bewerten eines Modells lineare Regression

[LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) verwendet, um eine lineare Regressionsmodell mit Stochastic Farbverlauf Unterlänge (SGD) zur Optimierung den Betrag der gezahlten Tipp Vorhersagen Schulen. 

Der Code in diesem Abschnitt veranschaulicht, wie eine lineare Regressionsmodell aus Azure Blob-Speicher laden, Punktzahl skalierte Variablen verwenden und das Ergebnis zurück in den Blob zu speichern.

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel
    
    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**ERGEBNIS:**

Über Zelle Ausführung: 16.63 Sekunden


## <a name="score-classification-and-regression-random-forest-models"></a>Klassifizierung und Regression zufällige Gesamtstruktur-Modelle Punktzahl

Der Code in diesem Abschnitt zeigt, wie die gespeicherte Einstufung laden und Regression zufällige Gesamtstruktur Modelle in Azure Blob-Speicher gespeichert, deren Leistung mit standard Klassifizierung und Measures Regression Punktzahl und das Ergebnis zurück zur Blob-Speicher zu speichern.

[Verteilte Gesamtstrukturen](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) sind Anzüge der Entscheidungsstruktur an.  Diese kombinieren viele Entscheidungsbäume, um das Risiko von Overfitting zu verringern. Zufallszahl Gesamtstrukturen können behandeln kategorisierten Features erweitern, um die Einstellung multiclass Klassifizierung, Featureskalierung nicht erforderlich und können Nichtlinearität erfassen und Interaktionen bereitstellen. Zufallszahl Gesamtstrukturen sind eine des Computers die meisten erfolgreichen learning Modelle für die Einstufung und Regression.

[Spark.mllib](http://spark.apache.org/mllib/) unterstützt zufällige Gesamtstrukturen für binäre und multiclass Klassifizierung und Regression, fortlaufender und kategorisierten Features verwenden. 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES 
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**ERGEBNIS:**

Über Zelle Ausführung: 31.07 Sekunden


## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Klassifizierung und Regression eines Farbverlaufs verstärken Struktur Modelle Punktzahl

Der Code in diesem Abschnitt veranschaulicht die Klassifizierung und Regression eines Farbverlaufs verstärken Struktur Modelle aus Azure Blob-Speicher laden, deren Leistung mit standard Klassifizierung und Measures Regression Punktzahl und das Ergebnis zurück zur Blob-Speicher zu speichern. 

**Spark.mllib** unterstützt GBTs für binäre Klassifizierung und Regression, fortlaufender und kategorisierten Features verwenden. 

[Farbverlauf steigern Strukturen](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) sind Anzüge der Entscheidungsstruktur an. GBTs Schulen Entscheidungsstruktur wiederholt auf eine Funktion Verlust zu minimieren. GBTs können kategorisierte Features behandelt, Featureskalierung nicht erforderlich und können Nichtlinearität erfassen und Interaktionen bereitstellen. Sie können auch eine Einstellung Multiclass-Klassifizierung verwendet werden.


    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    #LOAD AND SCORE THE MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 
    
**ERGEBNIS:**

Über Zelle Ausführung: 14.6 Sekunden


## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>Bereinigen von Objekten aus dem Speicher und Drucken bewertet Dateispeicherorte

    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH TO SCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


**ERGEBNIS:**

LogisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt

LinearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949

RandomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt

RandomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt



## <a name="consume-spark-models-through-a-web-interface"></a>Nutzen Sie Spark Modelle über eine Web-Oberfläche

Spark stellt ein Verfahren zum Stapelverarbeitung oder interaktive Abfragen über eine REST-Oberfläche mit eine Komponente namens Livius Remote zu übermitteln. Livius ist standardmäßig für Ihren Cluster HDInsight Spark aktiviert. Weitere Informationen zu Livius finden Sie unter: [übermitteln Spark Aufträge mit Remote Livius](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md). 

Livius können Sie Remote Absenden ein Auftrags, die Stapel der Ergebnisse eine Datei, die in einer Azure Blob gespeichert und dann die Ergebnisse in einer anderen Blob schreibt. Hierzu hochladen Sie das Skript Python aus  
[Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) , um das Blob des Spark Cluster. Ein Tool wie **Microsoft Azure-Speicher-Explorer** oder **AzCopy** können Sie um das Skript in den Cluster Blob zu kopieren. In diesem Fall wir das Skript ***wasb:///example/python/ConsumeGBNYCReg.py***geladen.   


>[AZURE.NOTE] Die Tastenkombinationen, die Sie auf das Portal für das Speicherkonto zugeordnet Spark Cluster können gefunden werden müssen. 


Nachdem in dieses Verzeichnis hochgeladen wird, wird dieses Skript im Cluster Spark in einem Kontext verteilt ausgeführt. Er lädt das Modell und Vorhersagen von Dateien basierend auf dem Modell ausgeführt.  

Sie können dieses Skript Remote aufrufen, durch eine einfache HTTPS/REST Anforderung auf Livius.  Hier ist ein Curl-Befehl zum Erstellen der HTTP-Anforderung zum Python-Skript Remote aufrufen. Ersetzen Sie durch die entsprechenden Werte für Ihren Cluster Spark CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME aus.


    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

Eine beliebige Sprache können auf dem remote-System Sie um den Auftrag Spark bis Livius zu starten, indem Sie eine einfache HTTPS-Anruf mit Standardauthentifizierung.   


>[AZURE.NOTE] Es wäre geeignete zu die Bibliothek Python Anfragen verwenden, wenn diese HTTP-anrufen, aber es ist zurzeit nicht installiert standardmäßig in Azure-Funktionen. Damit ältere HTTP-Bibliotheken stattdessen verwendet werden.   


So sieht der Python-Code für den Anruf HTTP aus:

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64
    
    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'
    
    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}
    
    # SPECIFY THE PYTHON SCRIPT TO RUN ON THE SPARK CLUSTER
    # IN THE FILE PARAMETER OF THE JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


Sie können auch diese Python Code [Azure-Funktionen](https://azure.microsoft.com/documentation/services/functions/) zum Senden einer Spark des Auftrags auslösen Bewertungen der Lesbarkeit ein Blob auf verschiedene Ereignisse wie eine Zeitmessung, erstellen oder Aktualisieren eines Blob basieren, hinzufügen. 

Auf Wunsch einen Code kostenlosen Client-Benutzeroberfläche verwenden Sie die [Azure Logik Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) zum Aufrufen des Spark Stapels Bewertung durch eine Aktion HTTP im **Logik Apps Designer** definieren und Festlegen der beiden Parameter. 

- Aus Azure-Portal, erstellen Sie eine neue Logik App, indem Sie **+ neu** -> **Web + Mobile** -> **Logik App**. 
- Um, um die **Logik Apps-Designer**zu öffnen, geben Sie den Namen der App Logik und Planen der App-Dienst aus.
- Wählen Sie eine HTTP-Aktion aus, und geben Sie die Parameter in der folgenden Abbildung dargestellt:

![](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)


## <a name="whats-next"></a>Wie geht's weiter? 

**Übergreifende Überprüfung und Hyperparameter ziehen**: finden Sie unter [Durchsuchen von Daten und Modellierung mit Spark erweiterte](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) auf wie Modelle werden können gelernt mit übergreifende Überprüfung und hyper-Parameter ziehen.
