<properties 
    pageTitle="Erforschen von Daten in SQL Server virtuellen Computern Azure | Microsoft Azure" 
    description="Informationen zum Durchsuchen von Daten, die in einer SQL Server virtueller Computer auf Azure gespeichert ist." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Erforschen von Daten in SQL Server virtuellen Computern Azure


Dieses Dokument beschreibt, wie Sie Daten untersuchen, die in einer SQL Server virtueller Computer auf Azure gespeichert ist. Dazu können Sie nach Daten Rechtsstreitigkeiten mit SQL oder mithilfe einer Programmiersprache wie Python.

Die folgenden **Menü** Links zu Themen, für die Verwendung von Tools zum Durchsuchen von Daten aus verschiedenen Speicher-Umgebungen beschrieben. Dieser Vorgang ist ein Schritt in Cortana Analytics Prozess (Linienende).

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


> [AZURE.NOTE] Beispiel-SQL-Anweisungen in diesem Dokument wird davon ausgegangen, dass die Daten in SQL Server ist. Wenn sie nicht ist, finden Sie in Cloud Daten Wissenschaft Prozessdiagramm zu erfahren Sie, wie Sie Ihre Daten mit SQL Server zu verschieben.



## <a name="a-namesql-dataexplorationaexplore-sql-data-with-sql-scripts"></a><a name="sql-dataexploration"></a>Durchsuchen von SQL-Daten mit SQL-Skripts

Hier sind ein paar Stichprobe SQL-Skripts, die zum Durchsuchen von Datenspeicher in SQL Server verwendet werden können.

1. Rufen Sie die Anzahl der Beobachtungen pro Tag

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Abrufen der Ebenen in einem kategorisierten Spalte

    `select  distinct <column_name> from <databasename>`

3. Die Anzahl der Ebenen in Kombination aus zwei kategorisierten Spalten abrufen 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. Abrufen der Verteilung für numerische Spalten

    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [AZURE.NOTE] Für ein praktisches Beispiel können Sie das [Dataset NYC Taxi](http://www.andresmh.com/nyctaxitrips/) verwenden und finden Sie in der IPNB mit dem Titel [NYC Daten Rechtsstreitigkeiten mit IPython Notizbuch und SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) für eine End-to-End-Exemplarische Vorgehensweise.

##<a name="a-namepythonaexplore-sql-data-with-python"></a><a name="python"></a>Durchsuchen von SQL-Daten mit Python

Verwenden Python zum Durchsuchen von Daten und Features generieren, wenn die Daten in SQL Server ist ähnelt für die Verarbeitung von Daten in Azure Blob mit Python, wie es in [Prozess Azure Blob-Daten in Ihrer Daten Wissenschaft Umgebung](machine-learning-data-science-process-data-blob.md)beschrieben. Die Daten aus der Datenbank in einer Pandas DataFrame geladen werden müssen, und können dann weiter verarbeitet werden. Wir dokumentieren Sie die Verfahren zum Herstellen einer Verbindung mit der Datenbank, und laden die Daten in der DataFrame in diesem Abschnitt.

Im folgenden Format der Verbindungszeichenfolge kann in Verbindung mit einer SQL Server-Datenbank aus Python mit Pyodbc (Servername ersetzen, DB-Name, Benutzername und Kennwort mit Ihren Werten für bestimmten) verwendet werden:

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas Bibliothek](http://pandas.pydata.org/) in Python stellt eine umfangreiche Menge von Datenstrukturen und Datenanalyse-Tools für die Bearbeitung von Daten für die Programmierung Python. Im folgende Code liest die Ergebnisse aus einer SQL Server-Datenbank in einen Pandas Datenrahmen zurückgegeben:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Jetzt können Sie mit der Pandas DataFrame arbeiten, wie im Thema [Prozess Azure Blob-Daten in Ihrer Daten Wissenschaft Umgebung](machine-learning-data-science-process-data-blob.md)behandelt.

## <a name="cortana-analytics-process-in-action-example"></a>Cortana Analytics Prozess in Aktion Beispiel

Beispiel-durchgehende Exemplarische Vorgehensweise des Prozesses Analytics Cortana mithilfe eines öffentlichen Datasets finden Sie unter [das Team Daten Wissenschaft Prozess in Aktion: Verwenden von SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

 
