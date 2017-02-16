<properties 
    pageTitle="Verarbeiten von Azure Blob-Daten mit erweiterten Analytics | Microsoft Azure" 
    description="Verarbeitung von Daten in Azure Blob-Speicher." 
    services="machine-learning,storage" 
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
    ms.date="09/19/2016"
    ms.author="fashah;garye;bradsev" /> 

#<a name="a-nameheadingaprocess-azure-blob-data-with-advanced-analytics"></a><a name="heading"></a>Prozess Azure Blob-Daten mit erweiterten analytics

Dieses Dokument beschreibt Kennenlernen Daten und Generieren von Features von Daten aus Azure Blob-Speicher. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>Laden Sie die Daten in einem Pandas Datenrahmen
Müssen, um zu durchsuchen und zu einem Dataset ändern, müssen sie aus der Quelle Blob in einer lokalen Datei heruntergeladen werden, die dann in einem Rahmen der Pandas Daten geladen werden kann. Hier sind die folgenden Schritte für dieses Verfahren aus:

1. Herunterladen die Daten aus Azure BLOB-mit den folgenden Beispiel Python-Code Blob-Dienst verwenden. Ersetzen Sie die Variable im folgenden Code wird mit Ihren Werten für bestimmten an: 

        from azure.storage.blob import BlobService
        import tables
        
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))


2. Lesen Sie die Daten in einem Pandas Daten-Rahmen aus der heruntergeladenen Datei an.

        #LOCALFILE is the file path 
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Jetzt sind Sie bereit sind, untersuchen die Daten und Features für den jeweiligen Dataset generieren.


##<a name="a-nameblob-dataexplorationadata-exploration"></a><a name="blob-dataexploration"></a>Durchsuchen von Daten

Hier sind einige Beispiele für Methoden zum Durchsuchen von Daten mithilfe von Pandas aus:

1. Prüfen Sie die Anzahl der Zeilen und Spalten 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. Prüfen Sie die ersten oder letzten Zeilen im Dataset wie folgt ein:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Überprüfen Sie den Datentyp, die, den jeder Spalte importiert wurde, wie mithilfe des folgenden Codes
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. Aktivieren Sie wie folgt grundlegenden Statistiken für die Spalten in der Datengruppe zurück
 
        dataframe_blobdata.describe()
    
5. Prüfen Sie die Anzahl von Einträgen für jede Spaltenwert wie folgt

        dataframe_blobdata['<column_name>'].value_counts()

6. Zählen der Anzahl von fehlenden Werte im Vergleich zu die tatsächliche Anzahl der Einträge in jeder Spalte, die mit dem folgenden Beispielcode

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Wenn Sie fehlende Werte für eine bestimmte Spalte in den Daten verfügen, können Sie diese wie folgt löschen:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Alternativ können Sie fehlende Werte ersetzen ist mit der Funktion Modus:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Erstellen Sie ein Histogramm Zeichnungsfläche mit Variable Anzahl von Papierkörben zum Darstellen der Verteilung einer Variablen 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Schauen Sie sich Korrelationen zwischen Variablen mithilfe einer Scatterplot oder mithilfe der integrierten Korrelations-Funktion

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
    
    
##<a name="a-nameblob-featuregenafeature-generation"></a><a name="blob-featuregen"></a>Features der ersten Generation
    
Wir können Features wie folgt mit Python generieren:

###<a name="a-nameblob-countfeatureaindicator-value-based-feature-generation"></a><a name="blob-countfeature"></a>Indikator für einen Wert basiert Features der ersten Generation

Kategorisierte Features können wie folgt erstellt werden:

1. Prüfen Sie die Verteilung der Kategorieliste Spalte:
    
        dataframe_blobdata['<categorical_column>'].value_counts()

2. Generieren Sie Indikator für Werte für jede Spalte Werte

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Teilnehmen an der Indikatorspalte mit dem ursprünglichen Daten frame 
 
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. Entfernen Sie die ursprüngliche Variable selbst:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)
    
###<a name="a-nameblob-binningfeatureabinning-feature-generation"></a><a name="blob-binningfeature"></a>Schachten von Features der ersten Generation

Zum Generieren von binned Features, gehen Sie wie folgt wir:

1. Fügen Sie eine Abfolge von Spalten in einer numerischen Spalte bin hinzu.
 
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
        
2. Konvertieren Sie in eine Abfolge von Variablen vom Typ boolean Schachten von

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
    
3. Teilnehmen an schließlich die-platzhalterprodukt Variablen wieder zum ursprünglichen Daten frame

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool) 


##<a name="a-namesql-featuregenawriting-data-back-to-azure-blob-and-consuming-in-azure-machine-learning"></a><a name="sql-featuregen"></a>Schreiben von Daten zurück in Azure Blob und Azure Computer interessante Verarbeitung

Nachdem Sie die Daten untersucht haben und erstellt die erforderlichen Features, können Sie die Daten hochladen (Stichprobe oder Featurized) in eine Azure BLOB- und nutzen sie Azure Computer interessante mithilfe der folgenden Schritte: Beachten Sie, dass in der Azure maschinellen Learning Studio sowie zusätzliche Features erstellt werden können. 
1. Schreiben Sie den Datenrahmen auf lokale Dateien

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Laden Sie die Daten in Azure Blob wie folgt ein:

        from azure.storage.blob import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. Nachdem die Daten aus dem Blob mithilfe der Azure maschinellen Learning [Daten importieren] gelesen werden können[ import-data] Modul wie in der folgenden Abbildung gezeigt:
 
![Blob Reader][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
