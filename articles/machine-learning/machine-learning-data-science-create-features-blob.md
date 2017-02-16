<properties
    pageTitle="Erstellen von Features für Azure Blob-Speicherdaten mithilfe von Panda | Microsoft Azure"
    description="Informationen zum Erstellen von Features für die Daten, die in Azure Blob-Container mit dem Panda Python-Paket gespeichert ist."
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
    ms.author="bradsev;garye" />

#<a name="create-features-for-azure-blob-storage-data-using-panda"></a>Erstellen von Features für Azure Blob-Speicherdaten mithilfe von Panda

Dieses Dokument wird gezeigt, wie Features für die Daten zu erstellen, die in Azure Blob-Container mit dem [Pandas](http://pandas.pydata.org/) Python-Paket gespeichert ist. Nach der Gliederung wie die Daten in einem Panda Datenrahmen laden, wird gezeigt, wie kategorisierten Features Verwenden von Python Skripts mit Indikatorwerte und Schachten von Features generieren.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Diese **Menü** enthält Links zu Themen, die zum Erstellen von Features für die Daten in verschiedenen Umgebungen zu beschreiben. Dieser Vorgang ist ein Schritt in die [Teamwebsite Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Artikel wird vorausgesetzt, dass Sie ein Azure Blob-Speicher-Konto erstellt haben und Ihre Daten dort gespeichert haben. Wenn Sie die Anweisungen zum Einrichten eines Kontos benötigen, finden Sie unter [Erstellen eines Kontos Azure-Speicher](../storage/storage-create-storage-account.md#create-a-storage-account)


## <a name="load-the-data-into-a-pandas-data-frame"></a>Laden Sie die Daten in einem Pandas Datenrahmen
Damit gehen Sie wie folgt untersuchen und ein Dataset zu bearbeiten, muss es aus der Quelle Blob in einer lokalen Datei heruntergeladen werden, die dann in einem Rahmen der Pandas Daten geladen werden kann. Hier sind die folgenden Schritte für dieses Verfahren aus:

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

##<a name="a-nameblob-featuregenafeature-generation"></a><a name="blob-featuregen"></a>Features der ersten Generation

In den beiden nächsten Abschnitten zeigen, wie kategorisierten mit Indikator Werten und Schachten von Features, die mithilfe von Python Skripts generieren.

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

3. Nun können die Daten aus dem Blob des Moduls Azure maschinellen Learning [Importieren von Daten](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) verwenden, wie in der nachstehenden Abbildung gezeigt gelesen werden:

![Blob Reader](./media/machine-learning-data-science-process-data-blob/reader_blob.png)
