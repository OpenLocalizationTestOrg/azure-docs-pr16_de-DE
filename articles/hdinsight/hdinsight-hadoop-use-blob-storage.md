<properties
    pageTitle="Abfragen von Daten aus HDFS kompatiblen Blob-Speicher | Microsoft Azure"
    description="HDInsight verwendet Azure Blob-Speicher als große Datenspeicher für HDFS. Informationen Sie zum Abfragen von Daten aus dem Blob-Speicher, und speichern die Ergebnisse Ihrer Analyse."
    keywords="BLOB-Speicher, Hdfs, strukturierte Daten, unstrukturierten Daten"
    services="hdinsight,storage"
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
    ms.topic="get-started-article"
    ms.date="09/06/2016"
    ms.author="jgao"/>


# <a name="use-hdfs-compatible-azure-blob-storage-with-hadoop-in-hdinsight"></a>Verwenden von HDFS kompatiblen Azure Blob-Speicher mit Hadoop in HDInsight

Informationen Sie zum Verwenden kostengünstiger Azure Blob-Speicher mit HDInsight, Azure-Speicher-Konto und BLOB-Speichercontainer erstellen und Adressieren Sie die Daten in.

Azure Blob-Speicher ist eine robuste, allgemeine Speicher-Lösung, die sich mit HDInsight nahtlos. Über eine Oberfläche Hadoop distributed Datei System (HDFS) kann sämtlicher Komponenten HDInsight direkt auf strukturierte oder unstrukturierte Daten im BLOB-Speicher ausgeführt werden.

Speichern von Daten im BLOB-Speicher, können Sie problemlos die HDInsight Cluster löschen, die für die Berechnung verwendet werden, ohne dass Benutzerdaten verloren gehen.

> [AZURE.IMPORTANT] HDInsight unterstützt nur Blobs blockieren aus. Nicht-Supportseite oder Blobs anfügen.

Informationen zum Erstellen eines Clusters HDInsight finden Sie unter [Erste Schritte mit HDInsight] [ hdinsight-get-started] oder [HDInsight erstellen Cluster][hdinsight-creation].


## <a name="hdinsight-storage-architecture"></a>HDInsight Speicherarchitektur
Das folgende Diagramm enthält eine abstrakte Ansicht der Architektur Speicher HDInsight:

![Hadoop Cluster mithilfe der HDFS-API zugreifen, und speichern strukturierte und unstrukturierte Daten im BLOB-Speicher.] (./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Speicher Architektur")

HDInsight ermöglicht den Zugriff auf distributed File System, das lokal mit den Datenverarbeitungsknoten verbunden ist. Dieses Dateisystem kann mithilfe des vollqualifizierten URIS zugegriffen werden:

    hdfs://<namenodehost>/<path>

Darüber hinaus bietet HDInsight die Möglichkeit, die Access-Daten, die in Azure Blob Storage gespeichert sind. Die Syntax lautet:

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

> [AZURE.NOTE] In früheren Versionen von HDInsight als 3.0 `asv://` verwendet wurde, statt `wasb://`. `asv://`sollte nicht verwendet werden mit HDInsight Cluster 3.0 oder höher, da es zu einem Fehler führt.

Hadoop unterstützt Konzept eines Standard-Dateisystem. Das Standard-Dateisystem impliziert ein Standardschema und Zertifizierungsstelle. Sie können auch relative Pfade Problembehebung verwendet werden. Während der Erstellungsprozess HDInsight, ein Konto Azure-Speicher und einer bestimmten Azure Blob-Speicher wird Container über dieses Konto als Standard-Dateisystem festgelegt.

Zusätzlich zu diesem Speicherkonto können Sie zusätzlichen Speicherkonten aus der gleichen Azure-Abonnements oder von anderen Azure-Abonnements hinzufügen, während der Erstellungsprozess oder ein Cluster erstellt wurde. Anweisungen zum Hinzufügen von zusätzlichen Speicher-Konten finden Sie unter [Erstellen von HDInsight Cluster][hdinsight-creation].

- **Container in die Speicherkonten, die mit einem Cluster verbunden sind:** Da den Kontonamen und die Taste während der Erstellung der Cluster zugeordnet sind, haben Sie Vollzugriff auf die Blobs in diesen Containern.

- **Öffentlichen Container oder öffentliche Blobs im Speicherkonten, die nicht zu einem Cluster verbunden sind:** Sie berechtigt schreibgeschützt, die Blobs in den Container.

    > [AZURE.NOTE]
        > Öffentliche Container ermöglichen Ihnen, eine Liste der alle Blobs aufzurufen, die im Container stehen und Container Metadaten zu erhalten. Öffentliche Blobs ermöglichen es Ihnen, die Blobs nur zugreifen, wenn Sie den genauen URL kennen. Weitere Informationen finden Sie unter <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">Einschränken des Zugriffs auf Container und Blobs</a>.

- **Private Container im Speicherkonten, die nicht zu einem Cluster verbunden sind:** Sie können nicht die Blobs in den Containern zugreifen, es sei denn, Sie beim Übermitteln der Aufträge WebHCat Speicher-Konto definieren. Dies wird später in diesem Artikel erläutert.


Die Speicherkonten, die in den Erstellungsprozess und ihre Schlüssel definiert sind, werden in %HADOOP_HOME%/conf/core-site.xml auf den Clusterknoten gespeichert. Das Standardverhalten der HDInsight besteht darin, die in der Datei Core-site.xml definierte Speicherkonten verwenden. Es wird nicht empfohlen, um die Core site.xml Datei zu bearbeiten, da der Cluster am node(master) einem neuen Image versehen oder zu einem beliebigen Zeitpunkt migriert werden können, und alle Änderungen an diesen Dateien verloren.

Mehrere WebHCat Aufträge, einschließlich Struktur, MapReduce, Hadoop streaming und Schwein, können eine Beschreibung der Speicherkonten und Metadaten mit ihnen ausführen. (Dies funktioniert derzeit, für Schwein mit Speicherkonten, aber nicht für Metadaten.) In diesem Artikel im Abschnitt [Zugriff Blobs mithilfe der PowerShell Azure](#powershell) ist vorhanden ein Beispiel für dieses Feature. Weitere Informationen finden Sie unter [Verwenden eines HDInsight Cluster mit alternativen Speicher-Konten und Metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

BLOB-Speicher kann für strukturierte und unstrukturierte Daten verwendet werden. BLOB-Speichercontainer Daten als Schlüssel/Wert-Paare speichern, und es gibt keine Verzeichnishierarchie. Jedoch kann Schrägstrich (/) innerhalb der Name des Schlüssels verwendet werden, um zu gestalten angezeigt, wenn während eine Datei in eine Verzeichnisstruktur gespeichert ist. Beispielsweise möglicherweise eine Blob-Taste *input/log1.txt*. Keine tatsächlichen *Eingabewerte* Verzeichnis vorhanden ist, aber auf dem Schrägstrich im Namen Key ein, sie die Darstellung der Pfad für eine Datei hat.

###<a name="a-idbenefitsabenefits-of-blob-storage"></a><a id="benefits"></a>Vorteile der Blob-Speicher
Die Leistungskosten impliziten nicht gemeinsame angezeigte berechnen Cluster und Speicherressourcen wird verringert durch die Art der Computecluster in der Nähe der Speicher Konto Ressourcen innerhalb der Azure Region, wo das schnelle Netzwerk erleichtert sehr effizient für die berechnen Knoten Zugriff auf die Daten in Azure Blob-Speicher erstellt werden.

Es gibt mehrere Vorteile, speichern die Daten in Azure Blob-Speicher statt HDFS zugeordnet:

* **Daten wiederverwenden und Freigabe:** Die Daten in HDFS befindet sich innerhalb der berechnungscluster. Nur die Programme, die auf die berechnungscluster zugreifen können die Daten mithilfe von HDFS-APIs verwenden. Die Daten in Azure Blob-Speicher zugegriffen werden können, über die HDFS-APIs oder über die [REST-APIs von BLOB-Speicher][blob-storage-restAPI]. Auf diese Weise kann eine größere Anzahl von Tools und Anwendungen (einschließlich anderer HDInsight Cluster) verwendet werden, um zu erstellen, und nutzen die Daten.
* **Archivieren von Daten:** Speichern von Daten in Azure Blob-Speicher ermöglicht die HDInsight Cluster für Berechnung verwendet, um sicher gelöscht werden, ohne dass Benutzerdaten verloren gehen.
* **Daten Speicherkosten:** Speichern von Daten in DFS für langfristig ist teurer als das Speichern der Daten in Azure Blob-Speicher, da die Kosten von einem berechnungscluster höher als die Kosten für einen Azure Blob-Speichercontainer sind. Darüber hinaus, da die Daten nicht für jede berechnen Cluster Generation neu geladen werden müssen, sind Sie auch Kosten Laden von Daten speichern.
* **Flexible Skalierung:** Obwohl HDFS mit einem skalierten Dateisystem bereitstellt, wird die Skalierung durch die Anzahl der Knoten bestimmt, die Sie für Ihren Cluster zu erstellen. Ändern des Maßstabs kann ein komplizierter Vorgang als sich auf die Skalierung der Funktionen, die Sie automatisch in Azure Blob-Speicher abrufen elastisch verlassen werden.
* **Geo-Replikation:** Ihre Azure BLOB-Speichercontainer können Geo repliziert werden. Obwohl Sie geografischen Wiederherstellung und Datenredundanz Dadurch werden, ein Failover zu dem Speicherort Geo repliziert stark wirkt sich auf die Leistung und können sie zusätzliche Kosten entstehen. Damit wird unsere empfohlen, wählen Sie die Geo-Replikation sorgfältig und nur, wenn der Wert der Daten im Wert zusätzliche Kosten ist.

Bestimmte MapReduce Aufträge und Pakete möglicherweise Zwischenergebnisse erstellen, die Sie nicht wirklich in Azure Blob Storage gespeichert werden sollen. In diesem Fall können Sie zum Speichern der Daten in der lokalen HDFS auszuwählen. Tatsächlich verwendet HDInsight DFS für eine Reihe von diese Zwischenergebnisse Struktur Einzelvorgänge und andere Prozesse an.

> [AZURE.NOTE] Die meisten HDFS Befehle (z. B. <b>!</b>, <b>CopyFromLocal</b> und <b>Mkdir</b>) funktionieren weiterhin wie erwartet. Nur die Befehle, die speziell für die systemeigene HDFS Implementierung (die als DFS bezeichnet wird), z. B. <b>Fschk</b> und <b>Dfsadmin</b>, sind, werden in Azure Blob-Speicher anderes Verhalten angezeigt.

## <a name="create-blob-containers"></a>Blob-Container erstellen

Wenn Blobs verwenden möchten, erstellen Sie zuerst ein [Konto Azure-Speicher][azure-storage-create]. Als Teil Geben Sie einen Azure-Bereich, in der die Objekte gespeichert werden, die mit diesem Konto erstellten. Cluster und Speicher-Konto müssen in der gleichen Region gehostet werden. Die Struktur Metastore SQL Server-Datenbank und Oozie Metastore SQL Server-Datenbank müssen auch im selben Bereich befinden.

Wo sie sich befindet, gehört jedes Blob erstellten zu einem Container in Ihr Konto Azure-Speicher. Diese Container möglicherweise eine vorhandene Blob, die außerhalb von HDInsight erstellt wurde, oder es möglicherweise einen Container, der für einen HDInsight Cluster erstellt wird.


Der standardmäßige Blob Container speichert Cluster bestimmte Informationen wie Historie und Protokolle. Freigeben Sie nicht mehrere HDInsight Cluster einen standardmäßige Blob Container. Dies möglicherweise Historie beschädigter und Cluster verantwortlich sind. Es wird empfohlen, verwenden einen anderen Container für jeden Cluster, und setzen freigegebene Daten auf einem angegebenen Bereitstellung von aller relevanten Cluster statt des Standardkontos für den Speicher verknüpfte Speicherkonto. Weitere Informationen zum Konfigurieren von verknüpften Speicherkonten finden Sie unter [Erstellen von HDInsight Cluster][hdinsight-creation]. Jedoch können Sie einen standardmäßige Speichercontainer wiederverwenden, nachdem der ursprüngliche HDInsight Cluster gelöscht wurde. HBase Cluster können Sie das Schema der HBase tatsächlich beibehalten und Daten durch Erstellen eines neuen HBase Clusters mithilfe des standardmäßige BLOB-Speicher Containers, der von einem HBase Cluster verwendet wird, die gelöscht wurden.


### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

Beim Erstellen eines HDInsight Clusters aus dem Portal haben Sie die Optionen zum Verwenden eines vorhandenen Speicher-Kontos oder erstellen ein neues Speicherkonto aus:

![Hdinsight Hadoop Erstellung der Datenquelle](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

###<a name="using-azure-cli"></a>Verwenden von Azure CLI

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

Wenn Sie [installiert und die Azure CLI konfiguriert](../xplat-cli-install.md)haben, kann mit dem folgende Befehl ein Speicher-Konto und Container verwendet werden.

    azure storage account create <storageaccountname> --type LRS

> [AZURE.NOTE] Die `--type` Parameter zeigt an, wie das Speicherkonto repliziert werden sollen. Weitere Informationen finden Sie unter [Azure Speicherreplikation](../storage/storage-redundancy.md). Verwenden Sie keine ZRS wie ZRS Seitenblob, einer Datei, Tabelle oder Warteschlange unterstützt.

Sie werden aufgefordert, die geografische Region angeben, der in das Speicherkonto gespeichert werden. Erstellen Sie das Speicherkonto in derselben Region, die Sie Ihren Cluster HDInsight erstellen möchten.

Nachdem das Speicherkonto erstellt wurde, verwenden Sie den folgenden Befehl zum Abrufen von Schlüsseln im Speicher-Konto aus:

    azure storage account keys list <storageaccountname>

Wenn Sie einen Container erstellen möchten, verwenden Sie den folgenden Befehl aus:

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

### <a name="using-azure-powershell"></a>Mithilfe der PowerShell Azure

Wenn Sie [installiert und konfiguriert Azure PowerShell][powershell-install], können Sie Folgendes aus der Azure-PowerShell-Eingabeaufforderung zum Erstellen einer Speicher-Konto und Container verwenden:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"
    
    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"
    
    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID
    
    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location
    
    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS 
    
    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

## <a name="address-files-in-blob-storage"></a>Adresse Dateien im BLOB-Speicher

Das URI-Schema für den Zugriff auf Dateien im BLOB-Speicher aus HDInsight lautet:

    wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>


Das URI-Schema bietet unverschlüsselter Zugriff (mit den *Wasb:* Präfix) und SSL-Verschlüsselung (mit *Wasbs*). Es wird empfohlen, wobei *Wasbs* soweit möglich, selbst wenn der Zugriff auf Daten, die innerhalb der gleichen Region in Azure verfügbar ist.

Die &lt;BlobStorageContainerName&gt; identifiziert den Namen des Containers in Azure Blob-Speicher.
Die &lt;StorageAccountName&gt; identifiziert den Kontonamen Azure-Speicher. Ein vollqualifizierten Domänennamen (FULLY) ist erforderlich.

Wenn weder &lt;BlobStorageContainerName&gt; noch &lt;StorageAccountName&gt; angegeben wurde, wird das Standard-Dateisystem verwendet. Für die Dateien im Standarddateisystem können Sie ein relativer Pfad oder ein absoluter Pfad. Beispielsweise kann die *Hadoop-Mapreduce-examples.jar* -Datei, die im Lieferumfang HDInsight Cluster verwiesen werden mithilfe einer der folgenden Aktionen aus:

    wasbs://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
    wasbs:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [AZURE.NOTE] Der Dateiname ist <i>Hadoop-examples.jar</i> in HDInsight Versionen 2.1 und 1.6 Cluster.


Die &lt;Pfad&gt; der Pfadnamen der Datei oder ein Verzeichnis HDFS ist. Da Container in Azure Blob-Speicher einfach Schlüsselwert Stores sind, gibt es keine WAHR hierarchische Dateisystem. Innerhalb eines Blob-Taste auf ein Schrägstrich (/) wird als Trennzeichen Directory interpretiert. Beträgt beispielsweise der Namen Blob *Hadoop-Mapreduce-examples.jar* aus:

    example/jars/hadoop-mapreduce-examples.jar

> [AZURE.NOTE] Bei der Arbeit mit Blobs außerhalb HDInsight die meisten Dienstprogramme nicht kennen das Format WASB und stattdessen wie erwartet eine Pfadformat grundlegende `example/jars/hadoop-mapreduce-examples.jar`.

## <a name="access-blobs-using-azure-cli"></a>Access-Blobs Azure CLI verwenden

Verwenden Sie den folgenden Befehl aus, um die Blob-bezogene Befehle auflisten:

    azure storage blob

**Beispiel für Azure CLI zum Hochladen einer Datei**

    azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Beispiel für Azure CLI verwenden, um eine Datei nicht herunterladen**

    azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>

**Beispiel für Azure CLI zum Löschen einer Datei**

    azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Beispiel für Azure CLI verwenden, um Dateien**

    azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="access-blobs-using-azure-powershell"></a>Access-Blobs mithilfe der PowerShell Azure

> [AZURE.NOTE] Die Befehle in diesem Abschnitt bieten ein einfaches Beispiel mithilfe der PowerShell mit Access-Daten in Blobs gespeichert. Eine weitere mit vollständigen Funktionen ausgestatteten Beispiel, bei dem für die Arbeit mit HDInsight angepasst wird, finden Sie unter den [HDInsight Tools](https://github.com/Blackmist/hdinsight-tools).

Verwenden Sie den folgenden Befehl aus, um die Blob-Cmdlets für die Liste:

    Get-Command *blob*

![Liste der Blob-bezogene PowerShell-Cmdlets.][img-hdi-powershell-blobcommands]

###<a name="upload-files"></a>Hochladen von Dateien

[Hochladen von Daten mit HDInsight]finden Sie unter[hdinsight-upload-data].

###<a name="download-files"></a>Herunterladen von Dateien

Die folgenden Script downloads blockieren Blob in den aktuellen Ordner an. Bevor Sie das Skript ausführen, wechseln Sie zu einem Ordner, in dem Sie Schreibberechtigung besitzen.

    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<AzureStorageAccountName>"   # The storage account used for the default file system specified at creation.
    $containerName = "<BlobStorageContainerName>"  # The default file system container has the same name as the cluster.
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.
    
    # Use Add-AzureAccount if you haven't connected to your Azure subscription
    Login-AzureRmAccount 
    Select-AzureRmSubscription -SubscriptionID "<Your Azure Subscription ID>"
    
    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    
    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force
    
    Write-Host "List the downloaded file ..." -ForegroundColor Green
    cat "./$blob"

Den Namen der Ressource Gruppe und den Clusternamen bereitstellen, können Sie den folgenden Code verwenden:

    $resourceGroupName = "<AzureResourceGroupName>"
    $clusterName = "<HDInsightClusterName>"
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.
    
    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 
    
    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force

###<a name="delete-files"></a>Löschen von Dateien


    Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob

###<a name="list-files"></a>Von Listendateien

    Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"

###<a name="run-hive-queries-using-an-undefined-storage-account"></a>Ausführen von Struktur Abfragen mit einem Speicherkonto nicht definierten

Dieses Beispiel zeigt, wie Sie einen Ordner aus Speicher-Konto auf, die nicht während der Erstellungsprozess definiert ist.
$clusterName = "<HDInsightClusterName>"

    $undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
    $undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

    $undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

    Use-AzureRmHDInsightCluster $clusterName

    $defines = @{}
    $defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

    Invoke-AzureRmHDInsightHiveJob -Defines $defines -Query "dfs -ls wasbs://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel zur Verwendung von HDFS kompatiblen Azure Blob-Speicher mit HDInsight gelernt und gelernten Azure Blob-Speicher eine grundlegende Komponente des HDInsight ist. So können Sie skalierbare, Archivierung langfristiges Daten Acquisition Lösungen mit Azure Blob-Speicher erstellen und Verwenden von HDInsight zum Aufheben der Sperre innerhalb der gespeicherten strukturierten und unstrukturierten Daten.

Weitere Informationen finden Sie unter:

* [Erste Schritte mit Azure HDInsight][hdinsight-get-started]
* [Hochladen von Daten mit HDInsight][hdinsight-upload-data]
* [Verwenden Sie die Struktur mit HDInsight][hdinsight-use-hive]
* [Schwein mit HDInsight verwenden][hdinsight-use-pig]
* [Verwenden von Azure-Speicher freigegeben Access Signaturen zum Einschränken des Zugriffs auf Daten mit HDInsight][hdinsight-use-sas]

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: ../powershell-install-configure.md
[hdinsight-creation]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: ../storage/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  
