<properties 
    pageTitle="Untersuchen von Daten in Azure Blob-Speicher mit Pandas | Microsoft Azure" 
    description="Informationen zum Durchsuchen von Daten, die in Azure Blob-Container mit Pandas gespeichert ist." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-azure-blob-storage-with-pandas"></a>Untersuchen von Daten in Azure Blob-Speicher mit Pandas

Dieses Dokument beschreibt, wie Sie Daten untersuchen, die in Azure Blob-Container mit [Pandas](http://pandas.pydata.org/) Python-Paket gespeichert ist.

Die folgenden **Menü** Links zu Themen, für die Verwendung von Tools zum Durchsuchen von Daten aus verschiedenen Speicher-Umgebungen beschrieben. Dieser Vorgang ist einen Schritt im [Prozess Wissenschaft]().

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


## <a name="prerequisites"></a>Erforderliche Komponenten
In diesem Artikel wird vorausgesetzt, dass Sie:

* Erstellt ein Azure-Speicher-Konto an. Wenn Sie die Anweisungen benötigen, finden Sie unter [Erstellen eines Kontos Azure-Speicher](../storage/storage-create-storage-account.md#create-a-storage-account)
* Die Daten in einer Azure Blob-Speicher-Konto gespeichert. Wenn Sie die Anweisungen benötigen, finden Sie unter [Verschieben von Daten an und von Azure-Speicher](../storage/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>Laden Sie die Daten in einer Pandas DataFrame
Zum Durchsuchen, und bearbeiten ein Dataset, müssen sie zuerst aus der Quelle Blob in einer lokalen Datei, heruntergeladen werden, die dann in eine Pandas DataFrame geladen werden kann. Hier sind die folgenden Schritte für dieses Verfahren aus:

1. Herunterladen die Daten aus Azure BLOB-mit den folgenden Python Codebeispiel Blob-Dienst verwenden. Ersetzen Sie die Variable in den folgenden Code mit Ihren Werten für bestimmten an: 

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

##<a name="a-nameblob-dataexplorationaexamples-of-data-exploration-using-pandas"></a><a name="blob-dataexploration"></a>Beispiele für Durchsuchen von Daten mithilfe von Pandas

Hier sind einige Beispiele für Methoden zum Durchsuchen von Daten mithilfe von Pandas aus:

1. Prüfen Sie die **Anzahl von Zeilen und Spalten** 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. Die erste oder letzte **Zeilen** in das folgende Dataset **Prüfen** :

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Überprüfen Sie den **Datentyp** , die jeder Spalte importiert wurde, wie mithilfe des folgenden Codes
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. Überprüfen Sie die **grundlegenden Stats** für die Spalten in der Datenmenge wie folgt
 
        dataframe_blobdata.describe()
    
5. Prüfen Sie die Anzahl von Einträgen für jede Spaltenwert wie folgt

        dataframe_blobdata['<column_name>'].value_counts()

6. **Zählen der Anzahl der fehlenden Werte** im Vergleich zu die tatsächliche Anzahl der Einträge in jeder Spalte, die mit dem folgenden Beispielcode

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Wenn Sie für eine bestimmte Spalte **fehlende Werte** in den Daten haben, können Sie diese wie folgt entfernen:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Alternativ können Sie fehlende Werte ersetzen ist mit der Funktion Modus:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Erstellen Sie ein **Histogramm** Zeichnungsfläche mit Variable Anzahl von Papierkörben zum Darstellen der Verteilung einer Variablen 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Schauen Sie sich **Korrelationen** zwischen Variablen mithilfe einer Scatterplot oder mithilfe der integrierten Korrelations-Funktion

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
