<properties
    pageTitle="Was ist R auf HDInsight? Einführung in R Server auf HDInsight (Preview) | Microsoft Azure"
    description="Was ist R Server HDInsight (Preview) und so R Server zur Erstellung von Applications für große Analyse-Funktionen verwenden."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/17/2016"
   ms.author="jeffstok"/>


# <a name="overview-of-r-server-on-hdinsight-preview"></a>Übersicht über R Server auf HDInsight \(Vorschau\)

Mit Microsoft Azure HDInsight Premium ist Microsoft R Server jetzt als Option zur Verfügung, wenn Sie in Azure HDInsight Cluster erstellen. Diese neue Funktion bereitstellt, dass Daten Wissenschaftler, Statistiker und R Programmierer bei Bedarf Zugriff auf skalierbare, Methoden der Analytics auf HDInsight verteilt.

Cluster können werden mit den Projekten und Vorgängen zur hand Größe und unterbrochener ab, wenn sie nicht mehr benötigt werden. Teil Azure HDInsight sind, sind im Lieferumfang diese Cluster Enterprise Ebene 24/7 Support, eine Vereinbarung zum SERVICELEVEL von 99,9 % Verfügbarkeit und die Flexibilität mit anderen Komponenten in der Azure-Netz integriert werden soll.

>[AZURE.NOTE] R-Server ist nur in Verbindung mit HDInsight Premium verfügbar. HDInsight Premium ist derzeit nur für Hadoop und Spark Cluster zur Verfügung. Ja, derzeit R Server nur mit Hadoop und Spark Cluster auf HDInsight können Sie. Weitere Informationen finden Sie unter [Was die anderen Dienstebenen und Hadoop Komponenten HDInsight erhältlich sind?](hdinsight-component-versioning.md).

R-Server auf HDInsight bietet die neuesten Funktionen für R-basierten Analytics für große Datasets, die in Azure Blob-Speicher geladen werden. Da offene Quelle R R Server erstellt wird, können R-basierten Programme, die Sie erstellen nutzen keines der 8000 + offene Quelle R-Paketen als auch die Routinen in ScaleR, Microsoft big Data Analytics Paket mit R Server eingeschlossen ist.

Der Kantenknoten der Cluster Premium bietet eine geeignete Stelle zum Cluster herstellen und Ihre R Skripts ausgeführt. Mit einem Kantenknoten haben Sie die Möglichkeit, den ScaleR parallelisierten verteilten Funktionen über die Kerne des Servers Knoten Kante ausgeführt. Sie haben auch die Möglichkeit, die sie über den Knoten im Cluster ausführen, mithilfe des ScaleR Hadoop Karte verringern oder Spark Kontexten zu berechnen.

Die Modelle oder Vorhersagen, das Ergebnis von Analysen für heruntergeladen werden kann verwenden lokalen. Sie können auch an anderer Stelle in Azure, z. B. über eine [Azure maschinellen Learning Studio](http://studio.azureml.net) [Webdienst](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md)operationalized werden.

## <a name="get-started-with-r-on-hdinsight"></a>Erste Schritte mit R auf HDInsight

Wenn in einem Cluster HDInsight R Server einbeziehen möchten, müssen Sie entweder eine Hadoop oder Spark Cluster mit die Option Premium erstellen, beim Erstellen eines Clusters mithilfe des Azure-Portals. Beide Cluster verwenden die gleiche Konfiguration, die R Server auf die Daten von einem Cluster, und eine Kantenknoten als Startseite für Zone für R serverbasierte Analytics enthält. Für eine ausführliche exemplarische Vorgehensweise zum Erstellen eines Clusters finden Sie unter [Erste Schritte mit R Server auf HDInsight](hdinsight-hadoop-r-server-get-started.md) .

## <a name="learn-about-data-storage-options"></a>Erfahren Sie mehr über die Datenspeicheroptionen

Standardspeicher für HDInsight Cluster ist BLOB-Speicher mit dem HDFS-Dateisystem zu einem Container Blob zugeordnet. Dies stellt sicher, dass jeden Daten auf dem Cluster-Speicher hochgeladen, oder um Cluster-Speicher im Verlauf der Analyse, geschrieben beständigen besteht. Das Programm [AzCopy](../storage/storage-use-azcopy.md) können Sie Daten an und von den Blob kopieren.

Zusätzlich zur Verwendung von Blob-Speicher, müssen Sie die Option der Verwendung von [Azure Daten dem Speicher](https://azure.microsoft.com/services/data-lake-store/) mit Ihren Cluster. Wenn Sie Daten Lake verwenden, können Sie sowohl Blob-Speicher und Daten Lake HDFS Storage verwenden.

Sie können auch [Azure-Dateien](../storage/storage-how-to-use-files-linux.md) als Speicheroption für verwenden, klicken Sie auf den Rand Knoten verwenden. Azure-Dateien können Sie eine Dateifreigabe bereitstellen, die in Azure-Speicher im Dateisystem Linux erstellt wurde. Weitere Informationen über die Datenspeicheroptionen für R Server HDInsight Cluster finden Sie unter [Speicheroptionen für R Server auf HDInsight Cluster](hdinsight-hadoop-r-server-storage.md).

## <a name="access-r-server-on-the-cluster"></a>Access-R-Server im cluster

Nachdem Sie einen Cluster mit R Server erstellt haben, können Sie mit der R-Verwaltungskonsole auf den Rand Knoten der Cluster erfolgt über SSH/kitten verbinden. Sie können auch über einen Browser herstellen, wenn Sie festlegen, dass die optionale RStudio Server IDE auf den Rand Knoten installieren. Weitere Informationen zum Installieren von RStudio Server finden Sie unter [Installieren von RStudio Server auf HDInsight Cluster](hdinsight-hadoop-r-server-install-r-studio.md).   

## <a name="develop-and-run-r-scripts"></a>Entwickeln und Ausführen von R

Die R Skripts, die Sie erstellen und ausführen können die Pakete 8000 + offene Quelle R sowie die Routinen parallelisierten und verteilten in der Bibliothek ScaleR verwenden. Im Allgemeinen wird Skript, in R Server, klicken Sie auf den Rand Knoten ausgeführt wird, innerhalb der Interpreter R auf diesem Knoten ausgeführt. Eine Ausnahme ist, dass diese Schritte aus, die eine ScaleR Aufrufen mit einem Kontext berechnen auf Hadoop Karte reduzieren (RxHadoopMR) oder Spark (RxSpark) festgelegt ist, der Funktion.

In diesen Fällen führt die Funktion verteilt über diese Daten (Vorgang) Knoten im Cluster, die die angesprochenen Daten zugeordnet sind. Weitere Informationen zu den verschiedenen berechnen Kontextoptionen finden Sie unter [Kontextoptionen für R Server auf HDInsight Premium zu berechnen](hdinsight-hadoop-r-server-compute-contexts.md).

## <a name="operationalize-a-model"></a>Prozessen umsetzen eines Modells

Wenn Ihre Datenmodelle abgeschlossen ist, können Sie das Modell, um Vorhersagen für neue Daten sowohl in Azure und lokale Prozessen umsetzen. Dieses Verfahren wird als bewerten bezeichnet. Hier sind einige Beispiele für ein.

### <a name="score-in-hdinsight"></a>Punktzahl in HDInsight

Um der Punktzahl in HDInsight, Schreiben Sie eine R-Funktion, die Anrufe Modell um Vorhersagen für eine neue Datendatei zu machen, die Sie bei Ihrem Speicherkonto geladen haben. Speichern Sie dann die Vorhersagen wieder auf das Speicherkonto ein. Sie können den routinemäßigen bei Bedarf auf den Rand Knoten Ihren Cluster oder mithilfe eines geplanten Auftrags ausführen.  

### <a name="score-in-azure-machine-learning"></a>Punktzahl interessante Azure-Computern

Das open-Source Azure maschinellen Learning R Paket bekannt als [AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) mit der um der Punktzahl mithilfe eines Webdiensts Azure maschinellen Schulung, Ihr Modell als Azure Webdienst veröffentlicht. Zur Vereinfachung ist dieses Paket auf den Rand Knoten vorinstalliert. Als Nächstes verwenden Sie Funktionen zur maschinellen interessante zum Erstellen einer Benutzeroberfläche für den Webdienst, und rufen Sie dann den Webdienst zur Bewertung nach Bedarf.

Wenn Sie diese Option auswählen, müssen Sie alle ScaleR Modellobjekte in gleichwertiger Open Source Modellobjekte zur Verwendung mit dem Webdienst konvertieren. Dies kann mithilfe von ScaleR Koersion Funktionen, wie `as.randomForest()` für Ensemble-basierten Modelle.


### <a name="score-on-premises"></a>Punktzahl lokal

Zum lokalen nach dem Erstellen des Modells zu erreichen, können Sie das Modell im R serialisieren, herunterladen, Entserialisierung es und verwenden sie dann zur Bewertung der neuer Daten. Sie können neue Daten mithilfe des oben beschriebenen [Punkte zählen in HDInsight](#scoring-in-hdinsight) Ansatzes oder mithilfe eines [DeployR](https://deployr.revolutionanalytics.com/)Faktor.

## <a name="maintain-the-cluster"></a>Verwalten des Clusters

### <a name="install-and-maintain-r-packages"></a>Installieren und Verwalten von R-Paketen

Die meisten der R Pakete, mit denen Sie auf den Rand Knoten müssen, da die meisten der Skripts R dorthin ausgeführt werden. Um zusätzliche R Pakete auf den Rand Knoten installiert haben, können Sie die übliche `install.packages()` Methode in R.

In den meisten Fällen müssen Sie keine zusätzliche R Pakete auf den Datenknoten installieren, wenn Sie nur Routinen aus der Bibliothek ScaleR über den Cluster verwenden. Jedoch müssen Sie zusätzliche Pakete für Unterstützung von **RxExec** oder **RxDataStep** Ausführung auf den Datenknoten gegebenenfalls.

In diesen Fällen müssen zusätzliche Pakete durch die Verwendung von einer Skriptaktion angegeben werden, nachdem Sie den Cluster erstellt haben. Weitere Informationen finden Sie unter [Erstellen eines HDInsight Clusters mit R Server](hdinsight-hadoop-r-server-get-started.md).   

### <a name="change-hadoop-map-reduce-memory-settings"></a>Ändern der Einstellungen für Hadoop Karte verringern Arbeitsspeicher

Ein Cluster kann geändert werden, um die Größe des Arbeitsspeichers zu ändern, R-Server verfügbar ist, bei der Ausführung eines Auftrags Karte zu verringern. Um einen Cluster zu ändern, verwenden Sie die Apache Ambari-Benutzeroberfläche, die über das Azure Portals Blade für Ihren Cluster verfügbar ist. Informationen dazu, wie die Ambari UI für Ihren Cluster zugreifen finden Sie unter [Verwalten von HDInsight Cluster mithilfe der Web-Benutzeroberfläche Ambari](hdinsight-hadoop-manage-ambari.md).

Es ist auch möglich, die Speichermenge ändern, R-Server verfügbar ist, mit Hadoop Schalter in den Anruf an die **RxHadoopMR** wie folgt:

    hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### <a name="scale-your-cluster"></a>Ihren Cluster skalieren

Ein vorhandener Cluster kann über das Portal nach oben oder unten skaliert werden. Durch Skalieren, können Sie die zusätzliche Kapazität, die Sie für größere von Verarbeitungsaufgaben benötigen möglicherweise erhalten, oder Sie skalieren wieder einen Cluster, wenn er nicht verwendet wird. Informationen dazu, wie einen Cluster Skalieren finden Sie unter [Verwalten von HDInsight Cluster](hdinsight-administer-use-portal-linux.md).

### <a name="maintain-the-system"></a>Verwalten des Systems

Wartung erfolgt auf der zugrunde liegenden Linux virtuellen Computern in einem Cluster HDInsight tagsüber OS Patches und andere Updates anwenden. In der Regel ist Wartung um 3:30 Uhr (basierend auf der lokalen Zeit für den virtuellen Computer) jeder Montag und Donnerstag abgeschlossen. Updates werden so ausgeführt, dass sie mehr als ein Viertel der Cluster jeweils beeinträchtigen nicht.  

Da die am Knoten redundant sind, und nicht alle Datenknoten hängen davon ab, möglicherweise Aufträge, die während dieses Zeitraums ausgeführt werden verlangsamt. Sie sollten jedoch weiterhin bis zum Abschluss, ausgeführt. Benutzerdefinierte Software oder lokaler Daten, die Sie besitzen werden, sofern die Schwerwiegender Fehler auftritt, die Cluster erstellen muss über diese Ereignisse Wartung beibehalten.

## <a name="learn-about-ide-options-for-r-server-on-an-hdinsight-cluster"></a>Erfahren Sie mehr über IDE-Optionen für R Server auf einem Cluster HDInsight

Des Knotens Linux Kante eines HDInsight Premium Cluster ist die Startseite für Zone für die Analyse-basierten R. Nach dem Herstellen einer Verbindung mit dem Cluster, können Sie die Benutzeroberfläche der Konsole auf R Server durch Eingabe von **R** an der Linux Befehlszeile starten. Verwendung der Benutzeroberfläche der Konsole wurde verbessert, wenn Sie einen Text-Editor für die Entwicklung von R Skript in ein anderes Fenster ausführen und Ausschneiden und Einfügen von Abschnitten des Skripts in der Konsole R Bedarf.

Eine komplexere Tool für die Entwicklung von Ihr Skript R ist der R-basierten IDE für die Verwendung auf dem Desktop, wie Microsoft kürzlich [R Tools für Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS) angekündigt. Dies ist eine Reihe von Desktop- und Server-Tools aus [RStudio](https://www.rstudio.com/products/rstudio-server/). Sie können auch Walwares "Ellipse"-basierten [StatET](http://www.walware.de/goto/statet)verwenden.

Eine weitere Möglichkeit besteht darin eine IDE auf den Linux Kantenknoten selbst zu installieren.  Ein häufig wird [RStudio Server](https://www.rstudio.com/products/rstudio-server/), mit dem eine browserbasierte für die Verwendung durch remote-Clients die enthält. Installieren von RStudio Server auf den Rand Knoten von einem Cluster HDInsight Premium bietet die vollständige IDE für die Entwicklung und Ausführung von R Skripts mit R Server im Cluster. Es kann wesentlich produktiver als die Konsole R sein.  Wenn Sie RStudio Server verwenden möchten, finden Sie unter [Installieren von RStudio Server auf HDInsight Cluster](hdinsight-hadoop-r-server-install-r-studio.md).

## <a name="learn-about-pricing"></a>Erfahren Sie mehr über die Preise

Die Gebühren, die eine HDInsight Premium Cluster mit R Server zugeordnet sind sind entsprechend den Gebühren für die standardmäßige HDInsight Cluster strukturiert. Das Ändern der Größe von der zugrunde liegenden virtuellen Computern über den Namen, Daten und Kante Knoten, durch das Hinzufügen einer Anhub Core-Stunden-Format für Premium basieren. Weitere Informationen zu HDInsight Premium Preise, einschließlich Preise während Public Preview-Version, und die Verfügbarkeit von 30 Tage kostenlose Testversion, finden Sie unter [HDInsight Preise](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="next-steps"></a>Nächste Schritte

Führen Sie die folgenden Links, um weitere Informationen zur Verwendung von R Server mit HDInsight Cluster.

- [Erste Schritte mit R Server auf HDInsight](hdinsight-hadoop-r-server-get-started.md)

- [Hinzufügen von RStudio Server zu HDInsight Premium](hdinsight-hadoop-r-server-install-r-studio.md)

- [Berechnen von Kontextoptionen für R Server HDInsight (Preview)](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure Speicheroptionen für R Server auf HDInsight Premium](hdinsight-hadoop-r-server-storage.md)
