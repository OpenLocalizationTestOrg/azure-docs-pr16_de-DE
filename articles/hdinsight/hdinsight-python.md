<properties
    pageTitle="Verwenden von Python mit Struktur und Schwein in HDInsight | Microsoft Azure"
    description="Erfahren Sie, wie Python Benutzer benutzerdefinierte Funktionen (UDFs) von Struktur und Schwein in HDInsight, den Hadoop Technologie Stapel auf Azure verwenden."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/07/2016" 
    ms.author="larryfr"/>

#<a name="use-python-with-hive-and-pig-in-hdinsight"></a>Verwenden von Python mit Struktur und Schwein in HDInsight

Struktur und Schwein eignen sich hervorragend zum Arbeiten mit Daten in HDInsight, doch manchmal benötigen Sie einen Weitere allgemeine Zweck Sprache. Sowohl die Struktur Schwein können Sie Benutzer benutzerdefinierte Funktionen (UDFs) erstellen mithilfe einer Vielzahl von Sprachen. In diesem Artikel erfahren Sie, wie Python UDFs von Struktur und Schwein verwendet werden.

##<a name="requirements"></a>Anforderungen

* Ein HDInsight Cluster (Windows oder Linux-basierten)

* Einen Text-editor

    > [AZURE.IMPORTANT] Wenn Sie einen HDInsight Linux-basierten Server verwenden, aber die Python-Dateien auf einem Windows-Client erstellen, müssen Sie einen Editor verwenden verwendet, LF als eine Zeile Enddatum. Wenn Sie nicht sicher sind, ob Editor Zeilenvorschub- oder CRLF verwendet wird, finden Sie im Abschnitt [Problembehandlung](#troubleshooting) Schritte zum Entfernen des Dienstprogramme auf dem HDInsight Cluster mit Wagenrücklauf-Zeichens.
    
##<a name="a-namepythonapython-on-hdinsight"></a><a name="python"></a>Python auf HDInsight

Python2.7 wird standardmäßig auf HDInsight 3.0 und höher Cluster installiert. Struktur kann mit dieser Version von Python für Stream Verarbeitung verwendet werden (Daten zwischen Struktur und Python mit STDOUT/STANDARDEINGABE übergeben werden).

HDInsight enthält darüber hinaus Jython, die in Java geschriebenen Python Implementierung handelt. Schwein weiß, wie ohne Streaming, neu zu sortieren, damit ist es empfehlenswert beim Schwein mit Jython sprechen. Allerdings können Sie auch normale Python (C Python), mit Schwein ebenfalls verwenden.

##<a name="a-namehivepythonahive-and-python"></a><a name="hivepython"></a>Struktur und Python

Python können verwendet werden, als UDFs aus "durch den HiveQL **TRANSFORMIEREN** Anweisung. Die folgenden HiveQL ruft beispielsweise ein Python-Skript in der Datei **streaming.py** gespeichert.

**Linux-basierten HDInsight**

    add file wasbs:///streaming.py;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
      USING 'python streaming.py' AS
      (clientid string, phoneLable string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;

**Windows-basierten HDInsight**

    add file wasbs:///streaming.py;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
      USING 'D:\Python27\python.exe streaming.py' AS
      (clientid string, phoneLable string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;

> [AZURE.NOTE] Klicken Sie auf Windows basierende HDInsight Cluster muss die **USING** -Klausel den vollständigen Pfad zum python.exe angeben. Dies ist immer `D:\Python27\python.exe`.

Hier ist, was bedeutet, dass in diesem Beispiel:

1. Die **Datei hinzufügen** -Anweisung am Anfang der Datei wird dem verteilten Cache, die Datei **streaming.py** hinzugefügt, sodass darauf zugreifen, indem Sie alle Knoten im Cluster.

2. Die TRANSFORMATION **wählen... Verwenden von** Anweisung wählt Daten aus der **Hivesampletable**, und übergibt Clientid, Devicemake und Devicemodel an das Skript **streaming.py** .

3. Die **AS** -Klausel beschrieben, die von **streaming.py** zurückgegebenen Felder

<a name="streamingpy"></a>Hier sind die vom HiveQL Beispiel verwendete **streaming.py** -Datei ein.

    #!/usr/bin/env python

    import sys
    import string
    import hashlib

    while True:
      line = sys.stdin.readline()
      if not line:
        break

      line = string.strip(line, "\n ")
      clientid, devicemake, devicemodel = string.split(line, "\t")
      phone_label = devicemake + ' ' + devicemodel
      print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])

Da wir streaming verwenden, muss dieses Skript wie folgt vorgehen:

1. Lesen von Daten aus STDIN. Dies geschieht mithilfe von `sys.stdin.readline()` in diesem Beispiel.

2. Ein Zeichen für den Zeilenvorschub entfernt mit `string.strip(line, "\n ")`, da wir gerade die Textdaten und nicht am Ende der Zeile Indikator möchten.

2. Wenn Stream Verarbeitung ausführen, enthält eine einzelne Zeile alle Werte mit einem Tabstoppzeichen zwischen den einzelnen Werten aus. Also `string.split(line, "\t")` können verwendet werden, um die Eingabe auf jeder Registerkarte, die nur die Felder zurückgeben teilen.

3. Nach Abschluss der Verarbeitung muss die Ausgabe an STDOUT als einzelne Zeile mit einer Registerkarte zwischen jedes Feld geschrieben werden. Dies geschieht mithilfe von `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.

4. Tritt auf, das ganze innerhalb einer `while` Schleife, bis keine wiederholt werden soll, die `line` gelesen wurde, an welcher Stelle `break` Ausgänge der Schleife und das Skript beendet wird.

Darüber hinaus verkettet das Skript nur die Eingabewerte für `devicemake` und `devicemodel`, und einen Hash verketteten Wert berechnet. Ziemlich einfach, aber es werden die Grundlagen der Funktionsweise alle Python-Skript aus Struktur aufgerufen: Endloswiedergabe, Eingabemethoden lesen, bis es ist nicht mehr, Teilen jede Zeile der Eingabe auf den Registerkarten Prozess, schreiben eine einzelne Textzeile Ausgabe Tabstopp-getrennt.

Finden Sie unter [die Beispielen ausgeführt](#running) , wie in diesem Beispiel für Ihren Cluster HDInsight ausgeführt.

##<a name="a-namepigpythonapig-and-python"></a><a name="pigpython"></a>Schwein und Python

Ein Skript Python kann als UDFs aus Schwein über die **generieren** -Anweisung verwendet werden. Es gibt zwei Möglichkeiten, dies zu erreichen. Mithilfe von Jython (Python implementiert die Java virtuellen Computern) und C Python (reguläre Python). 

Der primäre Unterschied zwischen diesen sind, dass Jython ausgeführt wird, klicken Sie auf die JVM und systembedingt von Schwein (auch auf die JVM ausgeführt.) aufgerufen werden können C Python umfasst eine externe (geschrieben in c) Damit die Daten aus auf die JVM Schwein ist an gesendet, der das Skript in einem Prozess Python ausgeführt und dann die Ausgabe der wieder in Schwein gesendet wird.

Um festzustellen, ob Schwein Jython oder C Python verwendet, um das Skript auszuführen, verwenden Sie __Registrieren__ verweisen auf das Skript Python von Schwein Lateinisch. Dies weist Schwein, welche Interpreter verwenden und welche Alias für das Skript erstellen. In den folgenden Beispielen registrieren Skripts mit Schwein als __Myfuncs__:

* __Jython verwenden__:`register '/path/to/pig_python.py' using jython as myfuncs;`
* __C Python verwenden__:`register '/path/to/pig_python.py' using streaming_python as myfuncs;`

> [AZURE.IMPORTANT] Bei der Verwendung von Jython kann der Pfad zu der Datei Pig_jython entweder einen lokalen Pfad oder einer WASB: / / Pfad. Allerdings müssen bei der Verwendung von C Python Sie eine Datei auf dem lokalen Dateisystem des Knotens verwiesen werden, die Sie verwenden, um den Auftrag Schwein übermitteln.

Einmal vorhanden ist ältere Registrierung, die Schwein Lateinisch in diesem Beispiel für beide identisch:

    LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
    LOG = FILTER LOGS by LINE is not null;
    DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
    DUMP DETAILS;

Hier ist, was bedeutet, dass in diesem Beispiel:

1. Die erste Zeile lädt die Daten Beispieldatei **sample.log** in **Protokolle**. Da diese Protokolldatei ein konsistentes Schema aufweist, werden außerdem jeden Datensatz (**Zeile** in diesem Fall) als eine **Chararray**definiert. Chararray ist im Wesentlichen eine Zeichenfolge an.

2. Alle null-Werte, speichern das Ergebnis des Vorgangs in **LOG**filtert die nächste Zeile.

3. Als Nächstes werden durchläuft mit den Datensätzen in **LOG** und verwendet **generieren** , die im Python/Jython Skript als **Myfuncs**geladen enthaltene **Create_structure** -Methode aufzurufen.  **Zeile** wird der aktuelle Datensatz an die Funktion zu übergebenden verwendet.

4. Schließlich werden die Ausgaben mit dem Befehl **AUSGEBEN** STDOUT ausgegeben. Es wird nur das Ergebnis sofort anzuzeigen, nach Abschluss des Vorgangs. in einem tatsächlichen Skript würden Sie normalerweise **STORE** die Daten in einer neuen Datei.

Die tatsächliche Python-Skriptdatei ähnelt auch zwischen C Python und Jython, der einzige Unterschied, dass Sie importierender müssen __Schwein\_Util__ bei Verwendung von C Python. Hier finden Sie die __Schwein\_python.py__ Skript:

<a name="streamingpy"></a>

    # Uncomment the following if using C Python
    #from pig_util import outputSchema

    @outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
    def create_structure(input):
    if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
    date, time, classname, level, detail = input.split(' ', 4)
    return date, time, classname, level, detail

> [AZURE.NOTE] 'Pig_util' nicht etwas, die Sie installieren müssen. Es wird automatisch für das Skript zur Verfügung.

Denken Sie daran, dass wir zuvor nur, die **Linie** als eine Chararray Eingabemethoden definiert, da es keine konsistentes Schema für die Eingabe wurde? Unterstützt das Python-Skript ist zum Umwandeln der Daten in ein konsistentes Schema für die Ausgabe. Es funktioniert wie folgt:

1. Die **@outputSchema** -Anweisung definiert das Format der Daten, die an Schwein zurückgegeben werden. In diesem Fall ist es eine **Sammlung von Daten**, die ein Schwein-Datentyp ist. Die Sammlung enthält die folgenden Felder, die alle Chararray (Zeichenfolgen) sind:

    * Datum - das Datum, das der Eintrag erstellt wurde
    * Zeit – die Zeit, die der Eintrag erstellt wurde
    * Objektname - wurde für der Klassennamen den Eintrag erstellt.
    * Level - die Ebene
    * Detail - ausführliche Details für den Vertrieb

2. Als Nächstes definiert die **Qualität create_structure(input)** die Funktion, der Schwein Zeilenelementen zu übergeben wird.

3. Die Beispieldaten, **sample.log**, entspricht hauptsächlich Datum, Uhrzeit, Objektname, Ebene und Details Schema, wir zurückgeben möchten. Jedoch auch ein paar Zeilen, die beginnen mit der Zeichenfolge '*java.lang.Exception*', die geändert werden, um das Schema entsprechen müssen, enthält. Die **Wenn** -Anweisung für diejenigen überprüft, und klicken Sie dann Rendererdiensts die Eingabedaten, um die Zeichenfolge '*java.lang.Exception*' bis zum Ende, schalten die Daten in Zeile mit unseren erwarteten Ergebnis Schema zu verschieben.

4. Als Nächstes wird der Befehl **Teilen** verwendet, um die Daten in den ersten vier Leerzeichen aufzuteilen. Das Ergebnis fünf Werte, die in **Datum**, **Uhrzeit**, **Objektname**, **Ebene**und **Details**zugewiesen werden.

5. Schließlich werden die Werte in Schwein zurückgegeben.

Wenn die Daten an Schwein zurückgegeben werden, müssen sie ein konsistentes Schema, im Sinne der **@outputSchema** Anweisung.

##<a name="a-namerunningarunning-the-examples"></a><a name="running"></a>Die Beispiele ausgeführt

Wenn Sie einen Linux-basierten HDInsight Cluster arbeiten, verwenden Sie **SSH** Schritte. Wenn Sie ein Windows-basiertes HDInsight Cluster und einem Windows-Client verwenden, führen Sie die **PowerShell** Schritte.

###<a name="ssh"></a>SSH

Weitere Informationen zur Verwendung von SSH finden Sie unter <a href="../hdinsight-hadoop-linux-use-ssh-unix/" target="_blank">Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix, oder OS X</a> oder <a href="../hdinsight-hadoop-linux-use-ssh-windows/" target="_blank">Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Windows</a>.

1. Erstellen Sie lokale Kopien von Dateien auf Ihrem Entwicklungscomputer mit der Python Beispiele [streaming.py](#streamingpy) und [pig_python.py](#jythonpy).

2. Verwenden Sie `scp` um die Dateien in Ihren Cluster HDInsight zu kopieren. Beispielsweise würde die folgenden die Dateien kopieren zu einem Cluster mit dem Namen **MeinCluster**.

        scp streaming.py pig_python.py myuser@mycluster-ssh.azurehdinsight.net:

3. Verwenden Sie die Verbindung zum Cluster SSH ein. Beispielsweise würde Folgendes zu einem Cluster mit dem Namen **MeinCluster** als Benutzer **Myuser**verbinden.

        ssh myuser@mycluster-ssh.azurehdinsight.net

4. Fügen Sie die zuvor in die WASB Speicherplatz für den Cluster hochgeladen Python-Dateien aus der Sitzung SSH hinzu.

        hdfs dfs -put streaming.py /streaming.py
        hdfs dfs -put pig_python.py /pig_python.py

Nach dem Hochladen der Dateien, gehen Sie folgendermaßen vor, um die Struktur und Schwein Aufgaben auszuführen.

####<a name="hive"></a>Struktur

1. Verwenden der `hive` Befehl aus, um die Struktur Shell starten. Sie Auftreten einer `hive>` auffordern, nachdem die Verwaltungsshell geladen wurde.

2. Geben Sie Folgendes an der `hive>` auffordern.

        add file wasbs:///streaming.py;
        SELECT TRANSFORM (clientid, devicemake, devicemodel)
          USING 'python streaming.py' AS
          (clientid string, phoneLabel string, phoneHash string)
        FROM hivesampletable
        ORDER BY clientid LIMIT 50;

3. Nach der Eingabe der letzten Zeile, sollte den Auftrag zu starten. Schließlich wird die Ausgabe ähnlich wie der folgende zurückgegeben.

        100041  RIM 9650    d476f3687700442549a83fac4560c51c
        100041  RIM 9650    d476f3687700442549a83fac4560c51c
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
        100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9

####<a name="pig"></a>Schwein

1. Verwenden der `pig` Befehl aus, um die Verwaltungsshell zu starten. Sie Auftreten einer `grunt>` auffordern, nachdem die Verwaltungsshell geladen wurde.

2. Geben Sie die folgenden Aussagen bei der `grunt>` auffordern, das mit dem Jython Interpreter Python-Skript ausgeführt.

        Register wasbs:///pig_python.py using jython as myfuncs;
        LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
        DUMP DETAILS;

3. Nachdem Sie die folgende Zeile eingegeben haben, sollten den Auftrag zu starten. Schließlich wird die Ausgabe ähnlich wie der folgende zurückgegeben.

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. Verwenden Sie `quit` die Verwaltungsshell mühselige beenden, und verwenden Sie dann die folgenden zum Bearbeiten der Datei pig_python.py auf dem lokalen Dateisystem:

    Nano pig_python.py

5. Kommentieren Sie einmal im Editor die folgende Zeile durch das Entfernen der `#` Zeichen ab dem Anfang der Zeile:

        #from pig_util import outputSchema

    Nachdem die Änderung vorgenommen wurde, verwenden Sie STRG + X, um den Editor zu verlassen. Wählen Sie Y, und klicken Sie dann die EINGABETASTE, um die Änderungen zu speichern.

6. Verwenden der `pig` Befehl aus, um die Verwaltungsshell erneut zu starten. Nachdem Sie sich befinden die `grunt>` dazu aufgefordert werden, verwenden Sie die folgenden, das mit dem C Python Interpreter Python-Skript ausgeführt.

        Register 'pig_python.py' using streaming_python as myfuncs;
        LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
        DUMP DETAILS;

    Sobald dieses Projekt abgeschlossen ist, sollten Sie das gleiche Ergebnis sehen, als wenn Sie das Skript mithilfe von Jython ausgeführt haben.

###<a name="powershell"></a>PowerShell

Diese Schritte ausführen, Azure PowerShell. Wenn dies nicht bereits installiert und auf Ihrem Entwicklungscomputer konfiguriert ist, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) vor mithilfe der folgenden Schritte.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. Erstellen Sie lokale Kopien von Dateien auf Ihrem Entwicklungscomputer mit der Python Beispiele [streaming.py](#streamingpy) und [pig_python.py](#jythonpy).

2. Verwenden Sie das folgende PowerShell-Skript zum Hochladen der **streaming.py** und **schweinchen\_python.py** Dateien auf dem Server. Ersetzen Sie den Namen Ihrer Azure HDInsight Cluster und den Pfad für die **streaming.py** und **schweinchen\_python.py** Dateien auf die ersten drei Zeilen des Skripts.

        $clusterName = YourHDIClusterName
        $pathToStreamingFile = "C:\path\to\streaming.py"
        $pathToJythonFile = "C:\path\to\pig_python.py"

        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
        
        Set-AzureStorageBlobContent `
            -File $pathToStreamingFile `
            -Blob "streaming.py" `
            -Container $container `
            -Context $context
        
        Set-AzureStorageBlobContent `
            -File $pathToJythonFile `
            -Blob "pig_python.py" `
            -Container $container `
            -Context $context

    Dieses Skript ruft Informationen für Ihren Cluster HDInsight ab, und klicken Sie dann das Konto und Schlüssel für das Standardkonto für den Speicher extrahiert und lädt die Dateien werden im Stammordner des Containers hoch.

    > [AZURE.NOTE] Andere Methoden zum Hochladen der Skripts finden Sie im Dokument [Hochladen von Daten für Hadoop Aufträge in HDInsight](hdinsight-upload-data.md) .

Nach dem Hochladen der Dateien, verwenden Sie die folgenden PowerShell Skripts, um die Einzelvorgänge zu starten. Bei Auftragsabschluss, sollte die Ausgabe in der PowerShell-Konsole geschrieben werden.

####<a name="hive"></a>Struktur

Das folgende Skript wird das Skript __streaming.py__ ausgeführt. Bevor er ausgeführt wird, werden sie für die Kontoinformationen HTTPs/Administrator für Ihren Cluster HDInsight aufgefordert.

    # Replace 'YourHDIClusterName' with the name of your cluster
    $clusterName = YourHDIClusterName
    $creds=Get-Credential
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
    
    # If using a Windows-based HDInsight cluster, change the USING statement to:
    # "USING 'D:\Python27\python.exe streaming.py' AS " +
    $HiveQuery = "add file wasbs:///streaming.py;" +
                 "SELECT TRANSFORM (clientid, devicemake, devicemodel) " +
                   "USING 'python streaming.py' AS " +
                   "(clientid string, phoneLabel string, phoneHash string) " +
                 "FROM hivesampletable " +
                 "ORDER BY clientid LIMIT 50;"

    $jobDefinition = New-AzureRmHDInsightHiveJobDefinition `
        -Query $HiveQuery

    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for the Hive job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -JobId $job.JobId `
        -ClusterName $clusterName `
        -HttpCredential $creds
    # Uncomment the following to see stderr output
    # Get-AzureRmHDInsightJobOutput `
    #   -Clustername $clusterName `
    #   -JobId $job.JobId `
    #   -DefaultContainer $container `
    #   -DefaultStorageAccountName $storageAccountName `
    #   -DefaultStorageAccountKey $storageAccountKey `
    #   -HttpCredential $creds `
    #   -DisplayOutputType StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds

Die Ausgabe für den Auftrag **Struktur** sollte etwa wie folgt angezeigt:

    100041  RIM 9650    d476f3687700442549a83fac4560c51c
    100041  RIM 9650    d476f3687700442549a83fac4560c51c
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9
    100042  Apple iPhone 4.2.x  375ad9a0ddc4351536804f1d5d0ea9b9

####<a name="pig-jython"></a>Schwein (Jython)

Im folgenden wird das __pig_python.py__ -Skript, mit dem Jython Interpreter verwendet. Bevor er ausgeführt wird, werden sie für die HTTPs/Admin-Informationen für den Cluster HDInsight aufgefordert.

> [AZURE.NOTE] Wenn ein Projekt mithilfe der PowerShell Remote gesendet werden, ist nicht möglich, C Python als der Interpreter verwenden.

    # Replace 'YourHDIClusterName' with the name of your cluster
    $clusterName = YourHDIClusterName

    $creds = Get-Credential
    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
    
    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey
            
    $PigQuery = "Register wasbs:///jython.py using jython as myfuncs;" +
                "LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);" +
                "LOG = FILTER LOGS by LINE is not null;" +
                "DETAILS = foreach LOG generate myfuncs.create_structure(LINE);" +
                "DUMP DETAILS;"

    $jobDefinition = New-AzureRmHDInsightPigJobDefinition -Query $PigQuery

    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
        
    Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -Job $job.JobId `
        -ClusterName $clusterName `
        -HttpCredential $creds
    # Uncomment the following to see stderr output
    # Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -DefaultContainer $container `
        -DefaultStorageAccountName $storageAccountName `
        -DefaultStorageAccountKey $storageAccountKey `
        -HttpCredential $creds

Die Ausgabe für den Auftrag **Schwein** sollte ähnlich wie die folgende angezeigt:

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

##<a name="a-nametroubleshootingatroubleshooting"></a><a name="troubleshooting"></a>Behandlung von Problemen

###<a name="errors-when-running-jobs"></a>Fehler beim Ausführen der Aufträge

Wenn Sie den Auftrag Struktur ausführen, können Sie einen ähnlich wie der folgende Fehler auftreten:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.
    
Dieses Problem möglicherweise durch die Zeilenenden in der Datei streaming.py verursacht werden. Viele Windows-Editoren Standard bei der Verwendung von CRLF als die Zeile beenden, aber Linux Applications normalerweise erwarten LF.

Wenn Sie einen Editor, der Zeilenenden LF kann nicht erstellt oder nicht sicher sind verwenden, welche Zeilenenden verwendet werden, verwenden Sie die folgenden Aussagen PowerShell So entfernen Sie die Zeilenumbruchzeichen vor dem Hochladen der Datei auf HDInsight:

    $original_file ='c:\path\to\streaming.py'
    $text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
    [IO.File]::WriteAllText($original_file, $text)

###<a name="powershell-scripts"></a>PowerShell-Skripts

Beide Beispiel PowerShell Skripts verwendet, um die Beispiele auszuführen enthalten eine kommentierte Zeile, die Ausgabe zurück, für das Projekt angezeigt werden. Wenn Sie nicht das erwartete Ergebnis für das Projekt angezeigt werden, kommentieren Sie die folgende Zeile und festzustellen Sie, ob die Fehlerinformationen auf ein Problem hinweist.

    # Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Die Fehlerinformationen (), und das Ergebnis des Projekts (STDOUT), werden auch dem standardmäßigen Blob Container für Ihre Cluster an folgenden Speicherorten protokolliert.

Für dieses Projekt...|Schauen Sie sich diese Dateien im Container blob
---|---
Struktur|/ HivePython/stderr<p>/ HivePython/stdout
Schwein|/ PigPython/stderr<p>/ PigPython/stdout

##<a name="a-namenextanext-steps"></a><a name="next"></a>Nächste Schritte

Wenn Sie Python Module laden, die standardmäßig zur Verfügung gestellt werden nicht benötigen, finden Sie unter [Gewusst wie: Bereitstellen ein Moduls mit Azure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx) ein Beispiel dazu.

Schwein für andere Methoden verwenden, die Struktur, und Weitere Informationen zum Verwenden von MapReduce, finden Sie hier.

* [Verwenden Sie die Struktur mit HDInsight](hdinsight-use-hive.md)

* [Schwein mit HDInsight verwenden](hdinsight-use-pig.md)

* [Verwenden von MapReduce mit HDInsight](hdinsight-use-mapreduce.md)
