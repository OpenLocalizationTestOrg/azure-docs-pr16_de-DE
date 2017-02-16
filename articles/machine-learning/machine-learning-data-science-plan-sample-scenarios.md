<properties
    pageTitle="Szenarien für das erweiterte Analytics Prozess und Technologie Azure-Computern interessante | Microsoft Azure"
    description="Wählen Sie die entsprechenden Szenarios hierfür erweiterte Vorhersageanalytik mit dem Team Daten Wissenschaft Prozess aus."
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
    ms.author="bradsev" />


# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a>Szenarien für erweiterte Analytics Azure Computer interessante

In diesem Artikel werden die Vielzahl von Datenquellen für die Stichprobe und Ziel Szenarien, die von der [Teamwebsite Daten Wissenschaft Prozess (TDSP)](data-science-process-overview.md)behandelt werden können. Die TDSP bietet einen systematischen Ansatz für Teams zum Erstellen von intelligenter Clientanwendungen gemeinsames Bearbeiten. Die hier vorgestellten Szenarien veranschaulichen Optionen in der Workflow für die Verarbeitung von Daten, die die Datenmerkmale, Quellspeicherort und Ziel Repositorys in Azure abhängig sind.

Die **Entscheidungsstruktur** für die Beispielszenarien auswählen, die für Ihre Daten und Ziel geeignet ist, wird im letzten Abschnitt dargestellt.

>[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Die folgenden Abschnitten bietet ein Beispielszenario. Für jedes Szenario einer möglichen Daten Wissenschaft oder erweiterte Analytics Fluss und unterstützen von Azure Ressourcen aufgeführt sind.

>[AZURE.NOTE]**Für alle der folgenden Szenarien, müssen Sie:**
><br/>
>* [Erstellen Sie ein Speicherkonto](../storage/storage-create-storage-account.md)
><br/>
>* [Erstellen eines Arbeitsbereichs Azure maschinellen Schulung](machine-learning-create-workspace.md)


## <a name="a-namesmalllocalascenario-1-small-to-medium-tabular-dataset-in-a-local-files"></a><a name="smalllocal"></a>Szenario \#1: kleine und mittlere tabellarischen Dataset in einer lokalen Dateien

![Kleine und mittlere lokale Dateien][1]

#### <a name="additional-azure-resources-none"></a>Zusätzliche Azure Ressourcen: keine

1.  Melden Sie sich bei der [Azure-Computern Learning Studio](https://studio.azureml.net/).

2.  Hochladen eines Datasets an.

3.  Erstellen einer Azure maschinellen Learning experimentieren Fluss hochgeladene Dataset(s) angefangen.

## <a name="a-namesmalllocalprocessascenario-2-small-to-medium-dataset-of-local-files-that-require-processing"></a><a name="smalllocalprocess"></a>Szenario \#2: kleine und mittlere Dataset lokale Dateien, die bearbeitet werden muss

![Kleine und mittlere lokale Dateien mit Verarbeitung][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Zusätzliche Azure Ressourcen: Azure-virtuellen Computern (IPython Notizbuch Server)

1.  Erstellen einer Azure-virtuellen Computern ausgeführt IPython Notizbuch an.

2.  Hochladen von Daten zu einem Container Azure-Speicher.

3.  Vor der Verarbeitung und Aufräumen von Daten in IPython Notizbuch, den Zugriff auf Daten aus Azure-Speichercontainer.

4.  Transformieren Sie Daten bereinigt, tabellarischen Form.

5.  Speichern Sie umgewandelte Daten in Azure Blobs.

6.  Melden Sie sich bei der [Azure-Computern Learning Studio](https://studio.azureml.net/).

7.  Lesen die Daten aus Azure Blobs mithilfe der [Daten importieren] [ import-data] Modul.

8. Erstellen einer Azure maschinellen Learning experimentieren Fluss Motor angesaugten Dataset(s) angefangen.

## <a name="a-namelargelocalascenario-3-large-dataset-of-local-files-targeting-azure-blobs"></a><a name="largelocal"></a>Szenario \#3: großen Dataset lokale Dateien, Azure Blobs verwendet

![Große lokale Dateien][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a>Zusätzliche Azure Ressourcen: Azure-virtuellen Computern (IPython Notizbuch Server)

1.  Erstellen einer Azure-virtuellen Computern ausgeführt IPython Notizbuch an.

2.  Hochladen von Daten zu einem Container Azure-Speicher.

3.  Vor der Verarbeitung und Aufräumen von Daten in IPython Notizbuch, den Zugriff auf Daten aus Azure Blobs.

4.  Transformieren von Daten bereinigt, Tabellenformat, falls erforderlich.

5.  Durchsuchen von Daten und Features nach Bedarf erstellen.

6.  Extrahieren einer Stichprobe kleinen bis mittleren Daten an.

7.  Speichern der erfassten Daten in Azure Blobs an.

8. Melden Sie sich bei der [Azure-Computern Learning Studio](https://studio.azureml.net/).

9. Lesen die Daten aus Azure Blobs mithilfe der [Daten importieren] [ import-data] Modul.

10. Erstellen von Azure maschinellen Learning experimentieren Fluss Motor angesaugten Dataset(s) ab.


## <a name="a-namesmalllocaltodbascenario-4-small-to-medium-dataset-of-local-files-targeting-sql-server-in-an-azure-virtal-machine"></a><a name="smalllocaltodb"></a>Szenario \#4: kleine und mittlere Dataset lokale Dateien, SQL Server auf einem Azure Virtal Computer verwendet

![Kleine und mittlere lokale Dateien in Azure SQL-DB][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Zusätzliche Azure Ressourcen: Azure-virtuellen Computern (SQL Server / IPython Notizbuch Server)

1.  Erstellen einer Azure-virtuellen Computern ausgeführt wird, SQL Server + IPython Notizbuch an.

2.  Hochladen von Daten zu einem Container Azure-Speicher.

3.  Vor der Verarbeitung und Aufräumen von Daten in Azure-Speichercontainer IPython Notizbuch verwenden.

4.  Transformieren von Daten bereinigt, Tabellenformat, falls erforderlich.

5.  Speichern von Daten auf virtuellen Computer-lokale Dateien (Notizbuch IPython virtuellen Computers ausgeführt wird, lokalen Laufwerke verweisen auf virtuellen Computer Laufwerke).

6.  Laden Sie die Daten mit einer Azure-virtuellen Computers ausgeführten SQL Server-Datenbank.

    Option \#1: Verwenden von SQL Server Management Studio.

    - Melden Sie sich SQLServer virtueller Computer
    - Führen Sie SQL Server Management Studio.
    - Erstellen Sie Tabellen Datenbank und Ziel ein.
    - Verwenden Sie eine der das gleichzeitige Importieren Methoden, um die Daten aus lokalen virtuellen Computer Dateien zu laden.

    Option \#2: verwenden IPython Notebook – nicht empfehlenswert für Mittel- und größere Datasets<!-- -->    
    - Verwenden Sie SQL Server auf virtuellen Computer Zugriff auf ODBC-Verbindungszeichenfolge.
    - Erstellen Sie Tabellen Datenbank und Ziel ein.
    - Verwenden Sie eine der das gleichzeitige Importieren Methoden, um die Daten aus lokalen virtuellen Computer Dateien zu laden.

7.  Durchsuchen von Daten, die Features nach Bedarf erstellen. Beachten Sie, dass die Features nicht in den Datenbanktabellen materialisiert werden müssen. Beachten Sie nur die erforderliche Abfrage, um diese zu erstellen.

8. Entscheiden Sie auf einer Stichprobenumfang Daten, falls erforderlich, und/oder gewünscht.

9. Melden Sie sich bei der [Azure-Computern Learning Studio](https://studio.azureml.net/).

10. Lesen die Daten direkt vom [Importieren von Daten] mit SQL Server[ import-data] Modul. Fügen Sie die erforderliche Abfrage die Felder extrahiert und Funktionen erstellt aufgenommene Daten direkt in das [Importieren von Daten] nach Bedarf[ import-data] Abfrage.

11. Erstellen von Azure maschinellen Learning experimentieren Fluss Motor angesaugten Dataset(s) ab.

## <a name="a-namelargelocaltodbascenario-5-large-dataset-in-a-local-files-target-sql-server-in-azure-vm"></a><a name="largelocaltodb"></a>Szenario \#5: großes Dataset in eine lokale Dateien, adressieren SQL Server auf virtuellen Azure-Computer

![Große lokale Dateien an in Azure SQL-DB][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Zusätzliche Azure Ressourcen: Azure-virtuellen Computern (SQL Server / IPython Notizbuch Server)

1.  Erstellen einer Azure-virtuellen Computern mit SQL Server und IPython Notizbuch an.

2.  Hochladen von Daten zu einem Container Azure-Speicher.

3.  (Optional) Daten vor der Verarbeitung und Aufräumen.

    ein.  Vor der Verarbeitung und Aufräumen von Daten in IPython Notizbuch, den Zugriff auf Daten aus Azure Blobs.

    b.  Transformieren von Daten bereinigt, Tabellenformat, falls erforderlich.

    c.  Speichern von Daten auf virtuellen Computer-lokale Dateien (Notizbuch IPython virtuellen Computers ausgeführt wird, lokalen Laufwerke verweisen auf virtuellen Computer Laufwerke).

4.  Laden Sie die Daten mit einer Azure-virtuellen Computers ausgeführten SQL Server-Datenbank.

    ein.  Melden Sie sich mit SQLServer virtueller Computer.

    b.  Wenn Daten nicht bereits gespeichert, laden Sie Datendateien aus Azure-Speichercontainer auf Ordner Lokale-virtueller Computer.

    c.  Führen Sie SQL Server Management Studio.

    d.  Erstellen Sie Tabellen Datenbank und Ziel ein.

    e.  Verwenden Sie eine der das gleichzeitige Importieren Methoden, um die Daten zu laden.

    f.  Wenn die Tabelle Verknüpfungen erforderlich sind, Erstellen von Indizes, Verknüpfungen zu beschleunigen.

     > [AZURE.NOTE] Schnelleres Laden von Größen großer Datenmengen empfiehlt es sich, dass Sie partitionierte Tabellen erstellen und Massen importieren Sie die Daten parallel. Weitere Informationen finden Sie unter [Parallele Daten importieren auf SQL-Datenbanktabellen aufgeteilt](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).

5.  Durchsuchen von Daten, die Features nach Bedarf erstellen. Beachten Sie, dass die Features nicht in den Datenbanktabellen materialisiert werden müssen. Beachten Sie nur die erforderliche Abfrage, um diese zu erstellen.

6.  Entscheiden Sie auf einer Stichprobenumfang Daten, falls erforderlich, und/oder gewünscht.

7.  Melden Sie sich bei der [Azure-Computern Learning Studio](https://studio.azureml.net/).

8. Lesen die Daten direkt vom [Importieren von Daten] mit SQL Server[ import-data] Modul. Fügen Sie die erforderliche Abfrage die Felder extrahiert und Funktionen erstellt aufgenommene Daten direkt in das [Importieren von Daten] nach Bedarf[ import-data] Abfrage.

9. Einfache Azure maschinellen Learning experimentieren Fluss hochgeladene Dataset angefangen

## <a name="a-namelargedbtodbascenario-6-large-dataset-in-a-sql-server-database-on-prem-targeting-sql-server-in-an-azure-virtual-machine"></a><a name="largedbtodb"></a>Szenario \#6: großen Dataset in einer SQL Server-Datenbank auf-Prem, verwendet, SQL Server in einer Azure-virtuellen Computern

![Große SQL DB auf-Prem zu in Azure SQL-DB][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Zusätzliche Azure Ressourcen: Azure-virtuellen Computern (SQL Server / IPython Notizbuch Server)

1.  Erstellen einer Azure-virtuellen Computern mit SQL Server und IPython Notizbuch an.

2.  Verwenden Sie eine der Daten exportieren Methoden, um die Daten aus SQL Server in Sicherungsdateien zu exportieren.

    > [AZURE.NOTE] Wenn Sie alle Daten in der Datenbank auf Prem alternative Methode zum Verschieben der vollständigen Datenbank auf die SQL Server-Instanz in Azure (schneller) verschieben möchten. Überspringen Sie die Schritte zum Exportieren von Daten, erstellen die Datenbank, Last-Import der Daten in die Zieldatenbank und führen Sie die alternative Methode.

3.  Hochladen von Sicherungsdateien Azure-Speicher Container.

4.  Laden Sie die Daten in einer SQL Server-Datenbank auf einer Azure-virtuellen Computern ausgeführt.

    ein.  Melden Sie sich an den SQLServer virtueller Computer.

    b.  Herunterladen von Datendateien in den Ordner Lokale-virtuellen Computer aus einem Container Azure-Speicher.

    c.  Führen Sie SQL Server Management Studio.

    d.  Erstellen Sie Tabellen Datenbank und Ziel ein.

    e.  Verwenden Sie eine der das gleichzeitige Importieren Methoden, um die Daten zu laden.

    f.  Wenn die Tabelle Verknüpfungen erforderlich sind, Erstellen von Indizes, Verknüpfungen zu beschleunigen.

    > [AZURE.NOTE] Importieren Sie für schnelleres Laden von Daten große Auswahl an Papiergrößen, Erstellen von partitionierten Tabellen und Massen die Daten parallel aus. Weitere Informationen finden Sie unter [Parallele Daten importieren auf SQL-Datenbanktabellen aufgeteilt](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).

5.  Durchsuchen von Daten, die Features nach Bedarf erstellen. Beachten Sie, dass die Features nicht in den Datenbanktabellen materialisiert werden müssen. Beachten Sie nur die erforderliche Abfrage, um diese zu erstellen.

6.  Entscheiden Sie auf einer Stichprobenumfang Daten, falls erforderlich, und/oder gewünscht.

7.  Melden Sie sich bei der [Azure-Computern Learning Studio](https://studio.azureml.net/).

8. Lesen die Daten direkt vom [Importieren von Daten] mit SQL Server[ import-data] Modul. Fügen Sie die erforderliche Abfrage die Felder extrahiert und Funktionen erstellt aufgenommene Daten direkt in das [Importieren von Daten] nach Bedarf[ import-data] Abfrage.

9. Einfache Azure maschinellen Learning experimentieren Fluss hochgeladene Dataset angefangen.

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a>Alternative Methode, um eine vollständige Datenbank aus einer lokalen SQL Server mit Azure SQL-Datenbank zu kopieren.

![Lokale DB trennen und Anfügen an in Azure SQL-DB][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a>Zusätzliche Azure Ressourcen: Azure-virtuellen Computern (SQL Server / IPython Notizbuch Server)

Wenn Sie die gesamte SQL Server-Datenbank in der SQL Server virtueller Computer repliziert, sollten Sie kopieren eine Datenbank von einem Speicherort-Server zu einem anderen, unter der Voraussetzung, dass die Datenbank vorübergehend offline verfügbar gemacht werden kann. Aktion in SQL Server Management Studio Objekt-Explorer, oder verwenden die entsprechenden Transact-SQL-Befehle.

1. Trennen Sie die Datenbank am Quellspeicherort an. Weitere Informationen finden Sie unter [Trennen einer Datenbank](https://technet.microsoft.com/library/ms191491(v=sql.110).aspx).
2. Kopieren Sie in Windows-Explorer oder Windows-Befehlszeile den getrennten Datenbankdatei oder Dateien und Protokolldatei oder Dateien in den Zielort des SQL Server virtuellen Computers in Azure ein.
3. Schließen Sie die kopierten Dateien an Ziel SQL Server-Instanz ein. Weitere Informationen finden Sie unter [Anfügen einer Datenbank](https://technet.microsoft.com/library/ms190209(v=sql.110).aspx).

[Verschieben einer Datenbank mithilfe von trennen und anfügen (Transact-SQL)](https://technet.microsoft.com/library/ms187858(v=sql.110).aspx)

## <a name="a-namelargedbtohiveascenario-7-big-data-in-local-files-target-hive-database-in-azure-hdinsight-hadoop-clusters"></a><a name="largedbtohive"></a>Szenario \#7: Big Data in lokalen Dateien adressieren Struktur Datenbank in Azure HDInsight Hadoop Cluster

![Große Daten in lokales Ziel Struktur][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a>Zusätzliche Azure Ressourcen: Azure HDInsight Hadoop Cluster und Azure-virtuellen Computern (IPython Notizbuch Server)

1.  Erstellen einer Azure-virtuellen Computern IPython Notizbuch Server ausgeführt.

2.  Erstellen eines Azure HDInsight Hadoop Clusters an.

3.  (Optional) Daten vor der Verarbeitung und Aufräumen.

    ein.  Vor der Verarbeitung und Aufräumen von Daten in IPython Notizbuch, den Zugriff auf Daten aus Azure Blobs.

    b.  Transformieren von Daten bereinigt, Tabellenformat, falls erforderlich.

    c.  Speichern von Daten auf virtuellen Computer-lokale Dateien (Notizbuch IPython virtuellen Computers ausgeführt wird, lokalen Laufwerke verweisen auf virtuellen Computer Laufwerke).

4.  Hochladen von Daten in den standardmäßigen Container Hadoop Cluster im Schritt 2 ausgewählt haben.

5.  Laden Sie die Daten, die Struktur der Datenbank in Azure HDInsight Hadoop Cluster.

    ein.  Melden Sie sich bei der am Knoten im Cluster Hadoop

    b.  Öffnen Sie die Befehlszeile Hadoop.

    c.  Geben Sie im Stammverzeichnis der Struktur, Befehl `cd %hive_home%\bin` in Hadoop Befehlszeile.

    d.  Führen Sie die Struktur Abfragen zum Erstellen von Datenbanken und Tabellen, und Laden Sie der Daten aus Blob-Speicher in die Struktur Tabellen.

    > [AZURE.NOTE] Wenn die Daten groß ist, können Benutzer die strukturtabelle mit Partitionen erstellen. Benutzer können dann mithilfe einer `for` Schleife in der Hadoop Befehlszeile auf dem am Knoten zum Laden von Daten in die strukturtabelle von Partition aufgeteilt.

6.  Durchsuchen von Daten und Features nach Bedarf in Hadoop Befehlszeile erstellen. Beachten Sie, dass die Features nicht in den Datenbanktabellen materialisiert werden müssen. Beachten Sie nur die erforderliche Abfrage, um diese zu erstellen.

    ein.  Melden Sie sich bei der am Knoten im Cluster Hadoop

    b.  Öffnen Sie die Befehlszeile Hadoop.

    c.  Geben Sie im Stammverzeichnis der Struktur, Befehl `cd %hive_home%\bin` in Hadoop Befehlszeile.

    d.  Führen Sie die Struktur Abfragen in Hadoop Befehlszeile, auf dem am Knoten im Cluster Hadoop Untersuchen der Daten und Features nach Bedarf erstellen.

7.  Falls erforderlich, und/oder gewünscht, Beispieldaten Sie die in Azure maschinellen Learning Studio passt.

8.  Melden Sie sich bei der [Azure-Computern Learning Studio](https://studio.azureml.net/).

9. Lesen die Daten direkt aus der `Hive Queries` mithilfe der [Daten importieren] [ import-data] Modul. Fügen Sie die erforderliche Abfrage die Felder extrahiert und Funktionen erstellt aufgenommene Daten direkt in das [Importieren von Daten] nach Bedarf[ import-data] Abfrage.

10. Einfache Azure maschinellen Learning experimentieren Fluss hochgeladene Dataset angefangen.

## <a name="a-namedecisiontreeadecision-tree-for-scenario-selection"></a><a name="decisiontree"></a>Entscheidungsstruktur für Szenario Auswahl
------------------------

Das folgende Diagramm enthält eine Übersicht über den oben beschriebenen Szenarien und die erweiterte Analytics Prozess und Technologie getroffene, die Sie zu den einzelnen die detaillierte Szenarien ausführen. Notiz, die Datenverarbeitung, damit arbeiten, Feature technisch und werden möglicherweise in eine oder mehrere Methode/Umgebung – an der Quelle, mittlere, und/oder Ziel Umgebungen – platzieren und wiederholt Bedarf fortgesetzt werden kann. Das Diagramm nur dient als eine Abbildung der einige der möglichen Zahlungen und bietet eine vollständige Enumeration nicht.

![DS Prozess Exemplarische Vorgehensweise Beispielszenarien][8]

### <a name="advanced-analytics-in-action-examples"></a>Erweiterte Analytics in Aktion Beispiele

End-to-End-Azure maschinellen Learning Vorgehensweisen, die die erweiterte Analytics Prozess und Sie können öffentliche Datensets-Technologie einsetzen, finden Sie unter:


* [Team Daten Wissenschaft Prozess in Aktion: Verwenden von SQL Server](machine-learning-data-science-process-sql-walkthrough.md).
* [Team Daten Wissenschaft Prozess in Aktion: HDInsight Hadoop Cluster mit](machine-learning-data-science-process-hive-walkthrough.md).


[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
