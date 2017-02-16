<properties 
    pageTitle="Beispieldaten in Azure BLOB-Speicher | Microsoft Azure" 
    description="Beispieldaten in Azure BLOB-Speicher" 
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

#<a name="a-nameheadingasample-data-in-azure-blob-storage"></a><a name="heading"></a>Beispieldaten in Azure BLOB-Speicher


Dieses Dokument behandelt werden Daten von programmgesteuert herunterladen und dann Stichproben mit Verfahren in Python geschrieben in Azure Blob-Speicher gespeichert.

**Warum Beispieldaten?**
Wenn das Dataset aus, den, das Sie analysieren möchten, groß ist, ist es normalerweise eine gute Idee, die Daten, um es auf eine kleinere aber tragen und überschaubarer Größe reduzieren unten Stichproben. Dadurch erleichtert Daten Grundlegendes zu, damit arbeiten und technisch Feature. Seine Rolle im Cortana Analytics Prozess besteht darin schnelle Prototypen Datenverarbeitung Funktionen und Computer learning Modelle ermöglichen.

Klicken Sie im **Menü** unten Links zu Themen, für die Stichprobe Daten aus verschiedenen Speicher-Umgebungen beschrieben. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Diese Aufgabe werden ist ein Schritt im [Team Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="download-and-down-sample-data"></a>Herunterladen und Down-Beispieldaten
1. Herunterladen von Daten aus Azure Blob-Speicher mithilfe des Diensts Blob aus den folgenden Beispiel Python Code: 

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

2. Lesen von Daten in einem Pandas Daten-Rahmen aus der Datei, die über heruntergeladen.

        import pandas as pd

        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. Unten Stichproben der Daten mithilfe der `numpy`des `random.choice` wie folgt:

        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

Jetzt können Sie mit den oben angegebenen Datenrahmen der 1 % Stichproben eingegangen und Features der ersten Generation arbeiten.

##<a name="a-nameheadingaupload-data-and-read-it-into-azure-machine-learning"></a><a name="heading"></a>Hochladen von Daten, und Lesen Sie es in Azure maschinellen Schulung

Den folgende Code können Sie unten Stichproben der Daten, und diese direkt in Azure ML verwenden:

1. Schreiben Sie den Datenrahmen in einer lokalen Datei

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Hochladen der lokalen Datei in einer Azure Blob mit den folgenden Code:

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
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. Lesen Sie die Daten aus dem Azure Blob Azure ML [Importieren von Daten](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) verwenden, wie in der folgenden Abbildung dargestellt:
 
![Blob Reader](./media/machine-learning-data-science-sample-data-blob/reader_blob.png)

 
