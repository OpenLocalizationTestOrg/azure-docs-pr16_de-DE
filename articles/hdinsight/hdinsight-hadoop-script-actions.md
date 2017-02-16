<properties
    pageTitle="Skripts Aktion Entwicklung mit HDInsight | Microsoft Azure"
    description="Erfahren Sie, wie Hadoop Cluster mit Skriptaktion anpassen. Skript für Aktion kann zusätzlichen ausführen in einem Cluster Hadoop Software installieren oder ändern die Konfiguration von Applications in einem Cluster installiert verwendet werden."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>Entwickeln Sie Aktion Skript Skripts für HDInsight Windows-basiertem Cluster

Informationen Sie zum Schreiben von Skriptaktion Skripts für HDInsight. Informationen zur Verwendung von Skriptaktion finden Sie unter [Anpassen HDInsight Cluster mithilfe der Aktion Skript](hdinsight-hadoop-customize-cluster.md). Im gleichen Artikel für HDInsight Linux-basierten Cluster geschrieben finden Sie unter [entwickeln Skriptaktion Skripts für HDInsight](hdinsight-hadoop-script-actions-linux.md).

> [AZURE.NOTE] Die Informationen in diesem Dokument ist für Windows-basiertem HDInsight Cluster spezifisch. Informationen zum Verwenden von Skript-Aktionen mit Windows-basierten Cluster finden Sie unter [Skript Aktion Entwicklung mit HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).


Skript für Aktion kann zusätzlichen ausführen in einem Cluster Hadoop Software installieren oder ändern die Konfiguration von Applications in einem Cluster installiert verwendet werden. Skript-Aktionen sind Skripts, die auf den Clusterknoten ausgeführt werden, wenn HDInsight Cluster bereitgestellt werden, und sie werden ausgeführt, sobald Knoten im Cluster HDInsight-Konfiguration abzuschließen. Eine Skriptaktion ausgeführt wird, klicken Sie unter System-Konto admininistratorberechtigungen und Vollzugriff für die Cluster-Knoten bietet. Mit einer Liste von Skript-Aktionen in der Reihenfolge ausgeführt werden, in dem sie angegeben sind, kann jeder Cluster bereitgestellt werden.

> [AZURE.NOTE] Wenn Sie die folgende Fehlermeldung auftreten:
>
>     System.Management.Automation.CommandNotFoundException; ExceptionMessage : The term 'Save-HDIFile' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
> Es ist, da Sie nicht die Helper Methoden enthalten.  Siehe [Helper Methoden für benutzerdefinierte Skripts](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).

## <a name="sample-scripts"></a>Beispiel für Skripts

Für Windows-Betriebssystem HDInsight Cluster erstellen, wird die Aktion Skript Azure PowerShell-Skript. Ist ein Beispiel für die folgenden Skript zum Konfigurieren der Website Konfigurationsdateien:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable to configure $ConfigFileName because it is not part of the HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

Das Skript akzeptiert vier Parameter, den Dateinamen für die Konfiguration, die Eigenschaft, die Sie ändern möchten, den Wert, den Sie festlegen möchten und eine Beschreibung an. Beispiel:

    hive-site.xml hive.metastore.client.socket.timeout 90

Diese Parameter werden den Wert hive.metastore.client.socket.timeout 90 in die Struktur site.xml Datei festlegen.  Der Standardwert ist 60 Sekunden.

Dieses Beispielskript auch [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1)finden Sie unter.

HDInsight bietet mehrere Skripts, um weitere Komponenten auf HDInsight Cluster zu installieren:

Namen | Skript
----- | -----
**Installieren von Spark** | https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. [Installieren und verwenden Sie aufzeigen auf HDInsight Cluster]finden Sie unter[hdinsight-install-spark].
**Installieren von R** | https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Finden Sie unter [Installieren und Verwenden von R auf HDInsight Cluster][hdinsight-r-scripts].
**Installieren von Solr** | https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. [Installieren und verwenden Sie Cluster Solr auf HDInsight](hdinsight-hadoop-solr-install.md)finden Sie unter.
- **Installieren von Giraph** | https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. [Installieren und verwenden Sie Cluster Giraph auf HDInsight](hdinsight-hadoop-giraph-install.md)finden Sie unter.

Skript für Aktion kann aus dem Azure-Portal Azure PowerShell oder mit dem HDInsight .NET SDK bereitgestellt werden.  Weitere Informationen finden Sie unter [Anpassen HDInsight Cluster mithilfe der Aktion Skript][hdinsight-cluster-customize].

> [AZURE.NOTE] Die Stichprobe Skripts funktionieren nur mit HDInsight Clusterversion 3.1 oder höher. Weitere Informationen zum HDInsight Cluster Versionen finden Sie unter [HDInsight Cluster Versionen](hdinsight-component-versioning.md).





## <a name="helper-methods-for-custom-scripts"></a>Helper Methoden für benutzerdefinierte Skripts

Skript Aktion Helper Methoden sind Dienstprogramme, die Sie beim Erstellen von benutzerdefinierten Skripts verwenden können. Diese sind in [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1)definiert und enthalten sein können, in Ihren Skripts mithilfe der folgenden:

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module to make writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed to load HDInsightUtilities module, exiting ...";
        exit;
    }

Hier sind die Helper Methoden, die von diesem Skript bereitgestellt werden:

Helper-Methode | Beschreibung
-------------- | -----------
**Speichern-HDIFile** | Herunterladen einer Datei aus dem angegebenen URI Uniform Resource Identifier () an einem Speicherort auf dem lokalen Datenträger, der mit dem Azure-virtuellen Computer Knoten mit dem Cluster zugewiesen verknüpft ist.
**Erweitern Sie HDIZippedFile** | Extrahieren einer ZIP-Datei.
**Rufen Sie HDICmdScript** | Führen Sie ein Skript von cmd.exe ein.
**Schreiben-HDILog** | Die Ausgabe aus dem benutzerdefinierten Skript für eine Skriptaktion verwendet.
**Get-Dienste** | Erhalten Sie eine Liste der Dienste auf dem Computer ausgeführt werden, führt das Skript aus.
**Get-Dienst** | Mit dem Dienstnamen des bestimmten als Eingabe erzielen Sie detaillierte Informationen für einen bestimmten Dienst (service Name, Prozess-ID, Status usw.) auf dem Computer, in dem das Skript ausgeführt wird.
**Get-HDIServices** | Erhalten Sie eine Liste der auf dem Computer ausgeführt werden, führt das Skript HDInsight-Dienste.
**Get-HDIService** | Mit bestimmten HDInsight Dienstnamen als Eingabe, erhalten Sie detaillierte Informationen für einen bestimmten Dienst (service Name, Prozess-ID, Status usw.) auf dem Computer, in dem das Skript ausgeführt wird.
**Get-Dienstemehrere betreiben** | Erhalten Sie eine Liste der Dienste, die auf dem Computer ausgeführt werden, führt das Skript aus.
**Get-Dienst wird ausgeführt** | Überprüfen Sie, ob Sie ein bestimmter Dienst (mit Namen) auf dem Computer ausgeführt wird, führt das Skript aus.
**Get-HDIServicesRunning** | Erhalten Sie eine Liste der auf dem Computer ausgeführt werden, führt das Skript HDInsight-Dienste.
**Get-HDIServiceRunning** | Überprüfen Sie, ob ein bestimmter HDInsight-Dienst (mit Namen) auf dem Computer ausgeführt wird, führt das Skript.
**Get-HDIHadoopVersion** | Abrufen der Version von Hadoop auf dem Computer installiert, in dem das Skript ausgeführt wird.
**Test-IsHDIHeadNode** | Überprüfen Sie, ob der Computer, in dem das Skript ausgeführt wird, am Knoten ist.
**Test-IsActiveHDIHeadNode** | Überprüfen Sie, ob der Computer, in dem das Skript ausgeführt wird, ein aktiver am Knoten ist.
**Test-IsHDIDataNode** | Überprüfen Sie, ob der Computer, in dem das Skript ausgeführt wird, ein Datenknoten ist.
**Bearbeiten-HDIConfigFile** | Bearbeiten Sie die Config Dateien Struktur-site.xml, Core-site.xml, Hdfs-site.xml, Mapred-site.xml oder site.xml aus.


## <a name="best-practices-for-script-development"></a>Bewährte Methoden für die Skriptentwicklung

Wenn Sie ein benutzerdefiniertes Skript für einen Cluster HDInsight entwickeln, gibt es verschiedene empfohlene Vorgehensweisen beachten müssen:

- Prüfen der Hadoop-version

    Nur HDInsight Version 3.1 (Hadoop 2.4) und über Support mit Skriptaktion benutzerdefinierte Komponenten auf einem Cluster installieren. In ein benutzerdefiniertes Skript müssen Sie die **Get-HDIHadoopVersion** Helper Methode zum Überprüfen der Hadoop-Version, bevor Sie mit anderen Aufgaben in der das Skript verwenden.


- Bereitstellen von unveränderliche Links zu Skriptressourcen

    Benutzer sollten sicherstellen, dass alle die Skripts und andere Elemente in die Anpassung von einem Cluster verwendete während der gesamten Dauer der Cluster verfügbar bleiben und die Versionen dieser Dateien für die Dauer unverändert bleiben. Diese Ressourcen sind erforderlich, wenn die erneute Abbildung Knoten im Cluster erforderlich ist. Die bewährte Methode besteht darin herunterladen und alles in einem Speicherkonto, die der Benutzer steuert archivieren. Dies kann das Speicher Standardkonto oder alle zusätzlichen Speicherkonten zum Zeitpunkt der Bereitstellung für einen angepassten Cluster angegeben sein.
    Spark und R angepasst Cluster Beispiele, sofern Sie in der Dokumentation, beispielsweise wir eine lokale Kopie der Ressourcen in diesem Konto Speicher vorgenommen haben: https://hdiconfigactions.blob.core.windows.net/.


- Sicherstellen Sie, dass das Anpassung Cluster Skript Idempotent ist

    Sie müssen die Knoten einer HDInsight Cluster während der Cluster Lebensdauer erneut abgebildeten werden können, die erwartet. Das Anpassung Cluster Skript wird ausgeführt, wenn ein Cluster erneut abgebildeten ist. Dieses Skript muss entworfen werden, werden, dass Idempotent, d. h., bei erneute Abbildung, das Skript sicherstellen sollten, dass der Cluster, auf die gleiche zurückgegeben wird Zustand angepasst werden, die es in wurde, einfach, nachdem Sie das Skript zum ersten Mal ausgeführt wurde, wenn der Cluster ursprünglich erstellt wurde. Wenn ein benutzerdefiniertes Skript auf eine Anwendung am D:\AppLocation installiert beispielsweise seinem ersten Ausführen und dann auf jedes nachfolgende ausführen, bei der erneute Abbildung, sollte das Skript überprüfen Sie, ob die Anwendung am Speicherort D:\AppLocation vorhanden ist, bevor Sie fortfahren mit einer anderen im Skript Schritte.


- Installieren von benutzerdefinierten Komponenten in die optimale Position

    Wenn Clusterknoten erneut abgebildeten sind, können die C:\ Ressource Laufwerk D:\ Systemlaufwerk sein erneut formatiert, was zu den Verlust von Daten und Anwendungen, die auf diesen Laufwerken installiert wurde. Dies kann auch geschehen, wenn ein Knoten Azure-virtuellen Computern (virtueller Computer), der Bestandteil der Cluster ist fällt aus und wird durch einen neuen Knoten ersetzt. Sie können die Komponenten auf dem D:\ Laufwerk oder C:\apps Speicherort auf dem Cluster installieren. Alle anderen Speicherorten auf das Laufwerk C:\ reserviert werden. Geben Sie den Speicherort für Applikationen oder Bibliotheken im Cluster Anpassung Skript installiert sein.


- Sicherstellen einer hohen Verfügbarkeit der Clusterarchitektur

    HDInsight verfügt über eine Aktiv-Passiv Architektur für hohe Verfügbarkeit, in dem eine am Knoten im aktiven Modus besteht (Stelle, an der die HDInsight Dienste ausgeführt werden) und dem anderen am Knoten im standby-Modus ist (in der HDInsight Dienste nicht ausgeführt werden). Die Knoten wechseln aktiven und passiven Modi, wenn HDInsight Dienste unterbrochen werden. Wenn eine Skriptaktion So installieren Sie Dienste auf beiden am Knoten hohen Verfügbarkeit verwendet wird, beachten Sie, dass der Failovermechanismus HDInsight nicht automatisch über diese Benutzer installierte Dienste treten können. Damit Benutzer installierte Dienste auf HDInsight am Knoten, die mit hoher Verfügbarkeit erwartet werden müssen Sie haben ihre eigenen Failovermechanismus ist in Aktiv-Passiv-Modus oder im Aktiv / Aktiv-Modus ausgeführt werden.

    Ein HDInsight Skriptaktion Befehl führt auf beiden am Knoten, wenn die Leiter-Knoten Rolle als Wert in den Parameter *ClusterRoleCollection* angegeben ist. Wenn Sie ein benutzerdefiniertes Skript entwerfen, stellen Sie also sicher, dass Ihr Skript Setup bekannt sind. Sie sollten nicht ausführen Probleme, wo die gleichen Dienste installiert und Schritte auf beiden am Knoten sind und sie Anspruch auf miteinander einhandeln. Darüber hinaus Beachten Sie, dass Daten während erneute Abbildung, verloren geht, damit die Software installiert, über die Aktion Skript flexibel in Bezug auf solche Ereignisse werden muss. Arbeiten mit hoher Verfügbarkeit Daten, die in vielen Knoten verteilt ist, sollten Applikationen entworfen werden. Beachten Sie, dass bis zu 1/5-Knoten in einem Cluster wieder zur gleichen Zeit abgebildet werden können.


- Konfigurieren Sie die benutzerdefinierten Komponenten um Azure Blob-Speicher zu verwenden.

    Die benutzerdefinierten Komponenten, die Sie auf dem Clusterknoten installieren möglicherweise eine Standardkonfiguration Hadoop Distributed Datei System (HDFS) Speicher verwenden. Ändern Sie die Konfiguration, um stattdessen Azure Blob-Speicher verwenden. Auf einem Cluster erneut Bild das Dateisystem HDFS ruft formatiert und verliert Sie alle Daten, die es gespeichert ist. Mithilfe von Azure Blob-Speicher wird stattdessen sichergestellt, dass Ihre Daten aufbewahrt werden sollen.

## <a name="common-usage-patterns"></a>Gemeinsame Verwendungsmuster

Dieser Abschnitt enthält Anleitungen auf einige der gemeinsamen Verwendungsmuster, die Sie bei Auftreten schreiben ein eigenes benutzerdefinierte Skript implementieren.

### <a name="configure-environment-variables"></a>Konfigurieren der Umgebungsvariablen

In der Aktion Skriptentwicklung, werden Sie häufig der Umgebungsvariablen festlegen müssen fällt. Beispielsweise ist ein höchstwahrscheinlich Szenario auf, wenn Sie eine Binärdatei aus einer externen Website herunterladen, installieren es auf dem Cluster und fügen Sie den Speicherort, von dem sie Ihre Umgebung-Variablen 'PATH' installiert ist. Im folgende Codeausschnitt veranschaulicht die Umgebungsvariablen in das benutzerdefinierte Skript festlegen.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

Diese Anweisung legt die Umgebungsvariable **MDS_RUNNER_CUSTOM_CLUSTER** auf den Wert "True" und auch den Bereich diese Variable maschinellen organisationsweite sein. Manchmal ist es wichtig, den gewünschten Bereich – Computer oder Benutzer Umgebungsvariablen festgelegt werden. Finden Sie [hier] [ 1] für Weitere Informationen zum Einrichten der Umgebungsvariablen.

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Zugriff auf Speicherorte benutzerdefinierte Skripts gespeichert sind

Skripts verwendet, um einen Cluster anpassen müssen entweder in das Standardkonto-Speicher für den Cluster oder in einer öffentlichen schreibgeschützten Container auf ein anderes Speicherkonto verwenden sein. Wenn Ihr Skript an anderen Ressourcen greift auf müssen diese in einer öffentlich zugänglichen werden (mindestens öffentlichen schreibgeschützt). Sie möchten beispielsweise Zugriff auf eine Datei, und speichern sie mithilfe des Befehls SaveFile-HDI.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

In diesem Beispiel müssen Sie sicherstellen, dass der Container 'Somecontainer' in 'Somestorageaccount' Speicher-Konto öffentlich zugänglich ist. Andernfalls wird das Skript eine 'Nicht gefunden' Ausnahme auslösen und ein Fehler auftreten.

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a>Übergeben von Parametern an das Cmdlet AzureRmHDInsightScriptAction hinzufügen

Um mehrere Parameter hinzufügen AzureRmHDInsightScriptAction Cmdlet übergeben, müssen Sie zum Formatieren des Werts Zeichenfolge, um alle Parameter für das Skript enthalten. Beispiel:

    "-CertifcateUri wasbs:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

oder

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Ausnahme bei fehlerhaften Clusterbereitstellung

Der präzise benachrichtigt zu werden soll die Fakultät, die Anpassung cluster konnte nicht wie erwartet, es ist wichtig, eine Ausnahme auslösen und die Clustererstellung fehl. Sie möchten beispielsweise eine Datei zu verarbeiten, sofern vorhanden und helfen den Fehler Fall, in dem die Datei nicht vorhanden ist. Dadurch wird sichergestellt, dass das Skript ordnungsgemäß beendet und der Status der Cluster ordnungsgemäß bekannt ist. Im folgende Codeausschnitt bietet ein Beispiel dazu:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

In diesem Codeausschnitt Wenn die Datei nicht vorhanden ist, bewirken es, dass in einem Zustand, in dem das Skript tatsächlich beendet ordnungsgemäß nach dem Drucken der Fehlermeldung und Cluster erreicht laufenden Zustand unter der Voraussetzung, dass sie "erfolgreich" Cluster Anpassungsprozess durchgeführt. Wenn Sie genau das Verhalten, die Anpassung im Wesentlichen cluster benachrichtigt werden möchten erwartungsgemäß aufgrund einer fehlenden Datei nicht erfolgreich war, ist es besser geeignet ist, eine Ausnahme auslösen und der Cluster Anpassung Schritt fehl. Dazu müssen Sie den folgende Beispiel Codeausschnitt verwenden.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Checkliste für die Bereitstellung von einer Skriptaktion
Hier sind die Schritte, die wir erstellen, wenn Sie diese Skripts bereitstellen vorbereiten:

1. Setzen Sie die Dateien, die die benutzerdefinierte Skripts an einem Ort enthalten, die während der Bereitstellung von dem Clusterknoten zugegriffen werden kann. Dies kann einen der standardmäßigen oder weiteren Speicherplatz Konten zum Zeitpunkt der Clusterbereitstellung oder einem beliebigen anderen öffentlich zugänglichen Speichercontainer angegeben sein.
2. Hinzufügen von Prüfungen in Skripts, um sicherzustellen, dass er Idempotently, ausführen, so, dass das Skript mehrmals auf demselben Knoten ausgeführt werden kann.
3. Verwenden Sie das **Schreiben-Ausgabe** Azure-PowerShell-Cmdlet STDOUT als auch STDERR Drucken aus. Verwenden Sie keine **Schreiben-Host**.
4. Verwenden Sie einen Ordner für temporäre Dateien, wie z. B. $env: TEMP, so behalten Sie die heruntergeladene Datei verwendet wird, indem Sie die Skripts und dann bereinigen sie nach Skripts ausgeführt haben.
5. Installieren Sie benutzerdefiniertes Software nur auf D:\ oder C:\apps. Anderen Speicherorten auf Laufwerk C: sollte nicht verwendet werden, da diese reserviert sind. Beachten Sie, dass es sich bei der Installation von Dateien auf Laufwerk C: außerhalb des Ordners C:\apps Setupfehler während des Knotens erneut Bilder führen kann.
6. Den Fall, dass OS Ebene Einstellungen oder Hadoop Service Konfigurationsdateien geändert wurden, sollten Sie HDInsight-Dienste neu zu starten, damit er von keine Einstellungen OS Ebene, wie etwa die Umgebungsvariablen festlegen in den Skripts auswählen können.

## <a name="debug-custom-scripts"></a>Benutzerdefinierte Skripts Debuggen

Das Skript Fehlerprotokolle werden zusammen mit anderen Ausgabe, in der Speicher Standardkonto gespeichert, die Sie für den Cluster bei seiner Erstellung angegeben haben. Die Protokolle befinden sich in einer Tabelle mit dem Namen *u < \cluster-name-fragment >< \time-stamp > Setuplog*. Hierbei handelt es sich um zusammengefasster Protokolle, die Datensätze aller Knoten (am und Worker Knoten) aufweisen, auf denen das Skript im Cluster ausgeführt wird.
Überprüfen Sie die Protokolle auf einfache Weise besteht darin, HDInsight Tools für Visual Studio verwenden. Installieren die Tools, finden Sie unter [Erste Schritte mit Visual Studio Hadoop-Tools für HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#install-hdinsight-tools-for-visual-studio)

**So überprüfen Sie das Protokoll mit Visual Studio**

1. Öffnen Sie Visual Studio.
2. Klicken Sie auf **Ansicht**, und klicken Sie dann auf **Server-Explorer**.
3. Mit der rechten Maustaste in "Azure", klicken Sie auf Verbindung herstellen mit **Microsoft Azure-Abonnements**, und geben Sie Ihre Anmeldeinformationen.
4. Erweitern Sie **Speicher**das Azure-Speicher-Konto als Standard-Dateisystem verwendet, erweitern Sie **Tabellen**, und doppelklicken Sie dann auf den Namen der Tabelle.


Sie können auch remote in die Clusterknoten in der beide finden Sie unter STDOUT und STDERR für benutzerdefinierte Skripts. Die Protokolle auf den einzelnen Knoten sind nur auf diesen Knoten spezifisch und **C:\HDInsightLogs\DeploymentAgent.log**angemeldet sind. Diese Protokolldateien aufzeichnen alle Ausgaben benutzerdefinierter Skripts. Ein Beispiel für Log Codeausschnitt für eine Spark Skriptaktion sieht wie folgt aus:

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


In diesem Protokoll ist es, dass die Spark Skriptaktion des virtuellen Computers mit dem Namen HEADNODE0 ausgeführt wurde, und dass keine Ausnahmen, während der Ausführung ausgelöst wurden löschen.

Den Fall, dass ein Fehler bei der abfrageausführung auftritt, wird die Ausgabe, beschreibt es auch in dieser Protokolldatei enthalten sein. Die Informationen in diesen Protokollen bereitgestellten sollten dabei Debuggen von Skript-Problemen, die auftreten können hilfreich sein.


## <a name="see-also"></a>Siehe auch

- [Anpassen von HDInsight Cluster mithilfe der Aktion Skript][hdinsight-cluster-customize]
- [Installieren und Verwenden von Spark auf HDInsight Cluster][hdinsight-install-spark]
- [Installieren und Verwenden von R auf HDInsight Cluster][hdinsight-r-scripts]
- [Installieren und verwenden Sie Cluster Solr auf HDInsight](hdinsight-hadoop-solr-install.md).
- [Installieren und verwenden Sie Cluster Giraph auf HDInsight](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
