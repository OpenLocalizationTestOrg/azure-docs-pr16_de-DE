<properties 
    pageTitle="Beispieldaten in SQLServer auf Azure | Microsoft Azure" 
    description="Beispieldaten in SQLServer auf Azure" 
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
    ms.date="09/19/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="a-nameheadingasample-data-in-sql-server-on-azure"></a><a name="heading"></a>Beispieldaten in SQL Server auf Azure


Dieses Dokument veranschaulicht, wie für die Stichprobe in SQL Server auf Azure gespeicherter Daten mit SQL oder die Python Programmiersprache. Es wird gezeigt, wie gesammelten Daten in Azure maschinellen Learning zu verschieben, indem Sie ihn in eine Datei zu speichern, Hochladen der Datei auf einer Azure Blob und anschließendes in Azure maschinellen Learning Studio lesen.

Die werden Python verwendet die [Pyodbc](https://code.google.com/p/pyodbc/) ODBC-Bibliothek, für die Verbindung mit SQL Server auf Azure und der Bibliothek [Pandas](http://pandas.pydata.org/) führen Sie die Größe die Stichprobe.

>[AZURE.NOTE] Beispiel-SQL-Code in diesem Dokument wird davon ausgegangen, dass die Daten in einer SQL Server auf Azure. Wenn es nicht der Fall ist, Näheres [Verschieben von Daten mit SQL Server auf Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) Thema Anweisungen, wie Sie Ihre Daten mit SQL Server auf Azure zu verschieben.

**Warum Beispieldaten?**
Wenn das Dataset aus, den, das Sie analysieren möchten, groß ist, ist es normalerweise eine gute Idee, die Daten, um es auf eine kleinere aber tragen und überschaubarer Größe reduzieren unten Stichproben. Dadurch erleichtert Daten Grundlegendes zu, damit arbeiten und technisch Feature. Dessen Rolle im [Team Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) besteht darin schnelle Prototypen Datenverarbeitung Funktionen und Computer learning Modelle ermöglichen.

Klicken Sie im **Menü** unten Links zu Themen, für die Stichprobe Daten aus verschiedenen Speicher-Umgebungen beschrieben. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Diese Aufgabe werden ist ein Schritt im [Team Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

##<a name="a-namesqlausing-sql"></a><a name="SQL"></a>Mithilfe von SQL

In diesem Abschnitt werden mehrere Methoden SQL zum Ausführen von einfachen Stichproben gegen die Daten in der Datenbank verwenden. Wählen Sie eine Methode basierend auf der Datengröße Ihrer und deren Verteilung an.

Zwei Elemente unten zeigen, wie Newid in SQL Server zu verwenden, um die Größe die Stichprobe ausführen. Die Methode, die Sie auswählen, hängt davon ab, wie Zufallszahl im Beispiel werden soll (Pk_id im folgenden Beispielcode wird davon ausgegangen, dass ein automatisch generiertes Primärschlüssel sein).

1. Weniger strenge Stichprobe

        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())

2. Weitere Stichprobe 

        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

TABLESAMPLE kann für Stichproben verwendet als auch werden nachfolgend gezeigt. Dies möglicherweise eine bessere Vorgehensweise, wenn Ihre Daten (vorausgesetzt, dass die Daten auf verschiedenen Seiten nicht Beziehung gesetzt werden können) groß ist und für die Abfrage in einer angemessenen Zeit abgeschlossen.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

>[AZURE.NOTE] Sie können untersuchen und Features aus dieser gesammelten Daten generieren, indem Sie es in einer neuen Tabelle speichern


###<a name="a-namesql-amlaconnecting-to-azure-machine-learning"></a><a name="sql-aml"></a>Herstellen einer Verbindung mit Learning Azure-Computern

Direkt können Sie über die Beispiele für Abfragen in den Azure maschinellen Learning [Daten importieren] [ import-data] unten Stichproben der Daten im laufenden Betrieb und bringen es in einem Versuch Azure maschinellen Learning-Modul. Screenshot mit dem Modul Reader lesen die erfassten Daten sieht folgendermaßen aus:
   
![Sql Reader][1]

##<a name="a-namepythonausing-the-python-programming-language"></a><a name="python"></a>Verwenden der Programmiersprache Python 

In diesem Abschnitt wird veranschaulicht, wie der [Pyodbc Bibliothek](https://code.google.com/p/pyodbc/) Herstellen einer ODBC mit einer SQL Server-Datenbank in Python. Die Verbindungszeichenfolge sieht wie folgt aus: (Servername, DB-Name, Benutzername und Kennwort mit der Konfiguration ersetzen):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas](http://pandas.pydata.org/) Bibliothek in Python stellt eine umfangreiche Menge von Datenstrukturen und Datenanalyse-Tools für die Bearbeitung von Daten für die Programmierung Python. Im folgenden Code liest eine Stichprobe von 0,1 % der Daten aus einer Tabelle in SQL Azure-Datenbank in eine Datentabelle Pandas:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Sie können jetzt mit der erfassten Daten in den Rahmen der Pandas Daten arbeiten. 

###<a name="a-namepython-amlaconnecting-to-azure-machine-learning"></a><a name="python-aml"></a>Herstellen einer Verbindung mit Learning Azure-Computern

Den folgende Code können Sie die unten Stichprobe Daten in einer Datei speichern, und klicken Sie auf eine Azure Blob hochladen. Die Daten im Blob können direkt gelesen werden in einem Azure maschinellen Learning Versuch mithilfe der [Daten importieren] [ import-data] Modul. Die Schritte sind wie folgt aus: 

1. Schreiben Sie den Rahmen des Pandas Daten in einer lokalen Datei

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Lokale Dateien in Azure Blob hochladen

        from azure.storage import BlobService
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

3. Lesen von Daten aus Azure Blob Azure maschinellen Learning [Importieren von Daten] mit[ import-data] Modul wie im folgenden Bildschirmfoto Bildschirm dargestellt:
 
![Blob Reader][2]

## <a name="the-team-data-science-process-in-action-example"></a>Team von Daten Wissenschaft in Aktion Beispiel

Beispiel-durchgehende Exemplarische Vorgehensweise des Prozesses Wissenschaft Daten Team einem mithilfe eines öffentlichen Datasets, finden Sie unter [Team Daten Wissenschaft Prozess in Aktion: Verwenden von SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

 [import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
