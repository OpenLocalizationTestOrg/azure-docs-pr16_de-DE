<properties 
    pageTitle="Verarbeiten von Daten aus SQL Azure | Microsoft Azure" 
    description="Verarbeiten von Daten aus SQL Azure" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="a-nameheadingaprocess-data-in-sql-server-virtual-machine-on-azure"></a><a name="heading"></a>Verarbeiten von Daten in SQL Server-virtuellen Computern auf Azure

Dieses Dokument beschreibt zum Durchsuchen von Daten und Features für die in einer SQL Server virtueller Computer auf Azure gespeicherten Daten generieren. Dazu können Sie nach Daten Rechtsstreitigkeiten mit SQL oder mithilfe einer Programmiersprache wie Python.


> [AZURE.NOTE] Beispiel-SQL-Anweisungen in diesem Dokument wird davon ausgegangen, dass die Daten in SQL Server ist. Wenn sie nicht möchten, finden Sie in Cloud Daten Wissenschaft Prozessdiagramm zu erfahren Sie, wie Sie Ihre Daten mit SQL Server zu verschieben.

##<a name="a-namesqlausing-sql"></a><a name="SQL"></a>Mithilfe von SQL

Die folgenden Daten wrangling Aufgaben in diesem Abschnitt mit SQL beschrieben:

1. [Durchsuchen von Daten](#sql-dataexploration)
2. [Features der ersten Generation](#sql-featuregen)

###<a name="a-namesql-dataexplorationadata-exploration"></a><a name="sql-dataexploration"></a>Durchsuchen von Daten
Hier sind ein paar Stichprobe SQL-Skripts, die zum Durchsuchen von Datenspeicher in SQL Server verwendet werden können.


> [AZURE.NOTE] Für ein praktisches Beispiel können Sie das [Dataset NYC Taxi](http://www.andresmh.com/nyctaxitrips/) verwenden und finden Sie in der IPNB mit dem Titel [NYC Daten Rechtsstreitigkeiten mit IPython Notizbuch und SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) für eine End-to-End-Exemplarische Vorgehensweise.

1. Rufen Sie die Anzahl der Beobachtungen pro Tag

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Abrufen der Ebenen in einem kategorisierten Spalte

    `select  distinct <column_name> from <databasename>`

3. Die Anzahl der Ebenen in Kombination aus zwei kategorisierten Spalten abrufen 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. Abrufen der Verteilung für numerische Spalten

    `select <column_name>, count(*) from <tablename> group by <column_name>`


###<a name="a-namesql-featuregenafeature-generation"></a><a name="sql-featuregen"></a>Features der ersten Generation

In diesem Abschnitt beschreiben wir Wege zur Generierung Features, die mithilfe von SQL aus:  

1. [Zählen anhand von Features der ersten Generation](#sql-countfeature)
2. [Schachten von Features der ersten Generation](#sql-binningfeature)
3. [Die Features einführen aus einer einzelnen Spalte](#sql-featurerollout)


> [AZURE.NOTE] Nachdem Sie zusätzliche Features generieren, können Sie an die vorhandene Tabelle als Spalten hinzufügen oder erstellen eine neue Tabelle mit den zusätzlichen Funktionen und Primärschlüssel, die mit der ursprünglichen Tabelle verknüpft werden können. 

###<a name="a-namesql-countfeatureacount-based-feature-generation"></a><a name="sql-countfeature"></a>Zählen anhand von Features der ersten Generation

Dieses Dokument veranschaulicht zwei Arten generieren zählen Features. Die erste Methode verwendet Teilsummen und die zweite Methode verwendet die 'where'-Klausel. Diese können dann mit der ursprünglichen Tabelle (mit primären Key Spalten) verknüpft werden, damit zählen Features zusammen mit den Originaldaten.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

###<a name="a-namesql-binningfeatureabinning-feature-generation"></a><a name="sql-binningfeature"></a>Schachten von Features der ersten Generation

Im folgenden Beispiel wird gezeigt, wie binned Features Generieren von Schachten von (mit 5 Papierkörben) eine numerische Spalte, die als ein Feature stattdessen verwendet werden kann:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


###<a name="a-namesql-featurerolloutarolling-out-the-features-from-a-single-column"></a><a name="sql-featurerollout"></a>Die Features einführen aus einer einzelnen Spalte

In diesem Abschnitt führen wir von einer einzelnen Spalte in einer Tabelle, um zusätzliche Features generieren verteilen vor. Im Beispiel wird davon ausgegangen, dass es eine Breite oder Länge-Spalte in der Tabelle wird, aus der Sie Features generieren möchten.

Dies ist eine kurze Einführung auf Breite/Länge Standortdaten (Ressourcen aus einer Stackoverflow zugewiesen [So messen Sie die Genauigkeit der Breite und Länge?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)). Dies ist hilfreich, bevor Sie Featurizing im Feld Ort zu verstehen:

- Die Vorzeichen erfahren wir, ob wir Nord sind oder Süden, Osten oder Westen des Globus.
- Eine ungleich NULL Hunderte Ziffer erfahren wir verwenden wir Länge, nicht Breite!
- Zweistelligen Ziffer bietet eine Position auf ungefähr 1.000 Kilometer. Es gibt uns nützliche Informationen zu welchem Kontinent oder Ozean wir auf befinden.
- Die Einheiten Ziffer (ein Dezimaltrennzeichen Grad) bietet eine Position 111 Kilometer (60 Nautische Meilen, etwa 69 Meilen). Sie können uns ungefähr erkennen, welche großen Land oder Region, die wir in sind.
- Der ersten Dezimalstelle lohnt sich bis zu 11.1 km: sie können die Position des eine große Ort aus einer benachbarten große Stadt unterscheiden.
- Die zweite Dezimalstelle lohnt sich bis zu 1.1 km: sie können eine Dorf vom nächsten trennen.
- Die dritte Dezimalstelle lohnt sich bis zu 110 m, dass eine große landwirtschaftlichen Feld oder Institutionen gerade identifiziert werden kann.
- Der vierte Dezimalstelle lohnt sich bis zu 11 m, dass ein Paket Flächen identifiziert werden kann. Es ist typische Genauigkeit von einer nicht korrigierte GPS-Gerät ohne Einfluss vergleichbaren.
- Das fünfte Dezimalstelle im Wert von bis zu 1.1 m ist es Strukturen voneinander zu unterscheiden. Genauigkeit auf diesen Schwellenwert für kommerzielle GPS Einheiten kann nur mit Differenz Korrektur erzielt werden.
- Der sechsten Dezimalstelle lohnt sich bis zu 0,11 m, dass Sie dies verwenden können für das Layout von Strukturen im Detail, für das Entwerfen Landschaften, Straßen erstellen. Mehr als zufrieden zum Nachverfolgen von Glaciers und Flüsse abgestimmt sollten. Dies kann painstaking Maßnahmen mit GPS, z. B. differentially korrigierte GPS erfolgen.

Die Standortinformationen kann Featurized wie folgt berechnet, sein, Region, Ort und Ortsangaben extrahieren. Beachten Sie, dass Sie auch einen REST-Endpunkt wie Bing Maps-API verfügbar an [einen Speicherort Punkt finden](https://msdn.microsoft.com/library/ff701710.aspx) Sie die Region/Region Informationen abrufen aufrufen können.

    select 
        <location_columnname>
        ,round(<location_columnname>,0) as l1       
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

Die oben beschriebenen standortbasierte Features können weiteren generieren zählen zusätzliche Features wie zuvor beschrieben verwendet werden. 


> [AZURE.TIP] Sie können programmgesteuert die Datensätze mithilfe einer Sprache Ihrer Wahl einfügen. Möglicherweise müssen Sie die Daten in Blöcken zu schreiben Effizienz (für das Pyodbc verwenden Sie hierzu finden Sie unter [A HelloWorld Stichprobe SQLServer mit Python Zugriff auf](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)ein Beispiel) einfügen. Eine weitere Möglichkeit ist zum Einfügen von Daten in der Datenbank mit dem [Programm BCP](https://msdn.microsoft.com/library/ms162802.aspx).

###<a name="a-namesql-amlaconnecting-to-azure-machine-learning"></a><a name="sql-aml"></a>Herstellen einer Verbindung mit Learning Azure-Computern

Das Feature neu generierte kann als Spalte zu einer vorhandenen Tabelle hinzugefügt oder in einer neuen Tabelle gespeichert und mit der ursprünglichen Tabelle für maschinelle Learning beigetreten. Features generiert oder zugegriffen, wenn bereits erstellt haben, verwenden die [Daten importieren] können[ import-data] Modul in Azure Computer lernen, wie unten dargestellt:

![Azureml Leser][1] 

##<a name="a-namepythonausing-a-programming-language-like-python"></a><a name="python"></a>Verwenden einer Programmiersprache wie Python

Verwenden Python zum Durchsuchen von Daten und Features generieren, wenn die Daten in SQL Server ist ähnelt der Verarbeitung von Daten in Azure Blob Python verwenden, wie es in [Prozess Azure Blob-Daten in Sie Daten Wissenschaft Umgebung](machine-learning-data-science-process-data-blob.md)beschrieben. Die Daten aus der Datenbank in einem Pandas Daten Frame geladen werden müssen, und können dann weiter verarbeitet werden. Wir dokumentieren Sie die Verfahren zum Herstellen einer Verbindung mit der Datenbank, und laden die Daten in den Datenrahmen in diesem Abschnitt.

Im folgenden Format der Verbindungszeichenfolge kann in Verbindung mit einer SQL Server-Datenbank aus Python mit Pyodbc (Servername ersetzen, DB-Name, Benutzername und Kennwort mit Ihren Werten für bestimmten) verwendet werden:

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas Bibliothek](http://pandas.pydata.org/) in Python stellt eine umfangreiche Menge von Datenstrukturen und Datenanalyse-Tools für die Bearbeitung von Daten für die Programmierung Python. Im folgenden Code liest die Ergebnisse aus einer SQL Server-Datenbank in einen Pandas Datenrahmen zurückgegeben:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Jetzt können Sie mit dem Pandas Datenrahmen arbeiten, wie in den [Prozess Azure Blob-Daten in Sie Daten Wissenschaft Umgebung](machine-learning-data-science-process-data-blob.md)Artikel behandelt.

## <a name="azure-data-science-in-action-example"></a>Beispiel für eine Aktion Wissenschaft für Azure-Daten

Beispiel-durchgehende Exemplarische Vorgehensweise Azure Wissenschaft von Daten mithilfe einer öffentlichen Datasets finden Sie unter [Azure Daten Wissenschaft Prozess in Aktion](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
