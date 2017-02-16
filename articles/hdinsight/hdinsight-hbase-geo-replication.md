<properties 
   pageTitle="Konfigurieren von HBase Replikation zwischen zwei Datencenter | Microsoft Azure" 
   description="Informationen Sie zum Konfigurieren der HBase Replikation über zwei Data Centers, und Informationen zu den Fällen verwenden Cluster Replikation." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="07/25/2016"
   ms.author="jgao"/>

# <a name="configure-hbase-geo-replication-in-hdinsight"></a>Konfigurieren der HBase Geo-Replikation HDInsight

> [AZURE.SELECTOR]
- [Konfigurieren von VPN-Konnektivität](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [Konfigurieren von DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [Konfigurieren der Replikation HBase](hdinsight-hbase-geo-replication.md) 
 
Informationen Sie zum Konfigurieren der HBase Replikation über zwei Data Centers. Einige verwenden Fällen für Clusterreplikation einbeziehen:

- Sichern und Disaster Wiederherstellung
- Datenaggregation
- Geografischen Daten einer Zufallsvariablen zurück
- Online-Daten Aufnahme in Kombination mit Analytics offline-Daten

Clusterreplikation verwendet eine Methode Quelle-Pushbenachrichtigungen. Ein HBase Cluster kann einer Quelle, eines Ziels, oder beide Rollen gleichzeitig erfüllen kann. Die Replikation ist asynchrone, und das Ziel der Replikation ist tatsächlichen Konsistenz. Wenn die Quelle ein bearbeiten zu einer Spalte Familie mit Replikation aktiviert empfängt, wird dieser bearbeiten an alle Ziel Cluster weitergegeben. Wenn Sie Daten aus einem Cluster zu einem anderen repliziert werden, werden die Quelle Cluster und alle Cluster, die bereits die Daten verwendet haben Replikation Schleifen nicht verloren geht verfolgt. Weitere Informationen werden In diesem Lernprogramm Sie eine Quelle-Ziel Replikation konfigurieren.  Andere Clustertopologien finden Sie unter [Apache HBase Referenzhandbuchs](http://hbase.apache.org/book.html#_cluster_replication).

Dies ist der dritte Teil der Serie aus:

- [Konfigurieren eines VPN-Konnektivität zwischen zwei virtuelle Netzwerke][hdinsight-hbase-replication-vnet]
- [Konfigurieren von DNS für die virtuellen Netzwerke][hdinsight-hbase-replication-dns]
- Konfigurieren der HBase Geo Replikation (in diesem Lernprogramm)

Das folgende Diagramm veranschaulicht die beiden virtuelle Netzwerke und die Sie erstellt, in [Konfigurieren eines VPN-Konnektivität zwischen zwei virtuelle Netzwerke haben] Netzwerkkonnektivität[ hdinsight-hbase-geo-replication-vnet] und [Konfigurieren von DNS für die virtuellen Netzwerke][hdinsight-hbase-replication-dns]: 

![HDInsight HBase Replikation virtuelle Netzplandiagramm][img-vnet-diagram]

## <a name="a-idprerequisitesaprerequisites"></a><a id="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Eine Arbeitsstationen mit Azure PowerShell**.

    Um PowerShell-Skripts ausführen zu können, müssen Sie Azure PowerShell als Administrator ausführen und legen die Ausführungsrichtlinie auf *RemoteSigned*. Mithilfe des Cmdlets Set-ExecutionPolicy finden Sie unter.
    
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Zwei Azure virtuelles Netzwerk VPN-Konnektivität und DNS konfiguriert**.  Anweisungen finden Sie unter [Konfigurieren einer VPN-Verbindung zwischen zwei Azure virtuelle Netzwerke][hdinsight-hbase-replication-vnet], und [Konfigurieren von DNS-Einträge zwischen zwei Azure virtuelle Netzwerke][hdinsight-hbase-replication-dns].


    Stellen Sie bevor PowerShell-Skripts sicher, dass Sie das folgende Cmdlet mit Ihrem Azure-Abonnement verbunden sind:

        Add-AzureAccount

    Wenn Sie mehrere Azure-Abonnements verfügen, verwenden Sie das folgende Cmdlet aus, um das aktuelle Abonnement festlegen:

        Select-AzureSubscription <AzureSubscriptionName>



## <a name="provision-hbase-clusters-in-hdinsight"></a>Bereitstellen von HBase Cluster in HDInsight

In [Konfigurieren einer VPN-Verbindung zwischen zwei Azure virtuelle Netzwerke][hdinsight-hbase-replication-vnet], Sie haben ein virtuelles Netzwerk in einer Europa Datacenter, und ein virtuelles Netzwerk in einem US Data Center erstellt. Das zwei virtuelle Netzwerk über VPN verbunden sind. In dieser Sitzung werden Sie einen Cluster HBase in den einzelnen virtuellen Netzwerken bereitstellen. Weiter unten in diesem Lernprogramm werden Sie beide HBase Zuordnungseinheiten andere HBase Cluster repliziert vornehmen.

Das klassische Azure-Portal unterstützt keine provisioning HDInsight Cluster mit benutzerdefinierten Konfigurationsoptionen. Legen Sie beispielsweise *hbase.replication* auf *true*. Wenn Sie den Wert in der Konfigurationsdatei festlegen, nachdem ein Cluster bereitgestellt wird, verlieren Sie die Einstellung nach einem neuen Image Cluster versehen werden ist. Weitere Informationen finden Sie unter [Bereitstellen von Hadoop Cluster in HDInsight][hdinsight-provision]. Eine der Optionen zur Bereitstellung von HDInsight Cluster mit benutzerdefinierten Optionen ist Azure PowerShell verwenden.


**Bereitstellen einer HBase Cluster in Contoso-VNet-EU** 

1. Öffnen Sie von Ihrem Computer Windows PowerShell ISE aus.
2. Legen Sie die Variablen am Anfang des Skripts, und führen Sie das Skript.

        # create hbase cluster with replication enabled
        
        $azureSubscriptionName = "[AzureSubscriptionName]"
        
        $hbaseClusterName = "Contoso-HBase-EU" # This is the HBase cluster name to be used.
        $hbaseClusterSize = 1   # You must provision a cluster that contains at least 3 nodes for high availability of the HBase services.
        $hadoopUserLogin = "[HadoopUserName]"
        $hadoopUserpw = "[HadoopUserPassword]"
        
        $vNetName = "Contoso-VNet-EU"  # This name must match your Europe virtual network name.
        $subNetName = 'Subnet-1' # This name must match your Europe virtual network subnet name.  The default name is "Subnet-1".
        
        $storageAccountName = 'ContosoStoreEU'  # The script will create this storage account for you.  The storage account name doesn't support hyphens. 
        $storageAccountName = $storageAccountName.ToLower() #Storage account name must be in lower case.
        $blobContainerName = $hbaseClusterName.ToLower()  #Use the cluster name as the default container name.
        
        #connect to your Azure subscription
        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName
        
        # Create a storage account used by the HBase cluster
        $location = Get-AzureVNetSite -VNetName $vNetName | %{$_.Location} # use the virtual network location
        New-AzureStorageAccount -StorageAccountName $storageAccountName -Location $location
        
        # Create a blob container used by the HBase cluster
        $storageAccountKey = Get-AzureStorageKey -StorageAccountName $storageAccountName | %{$_.Primary}
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $blobContainerName -Context $storageContext
        
        # Create provision configuration object
        $hbaseConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHBaseConfiguration'
            
        $hbaseConfigValues.Configuration = @{ "hbase.replication"="true" } #this modifies the hbase-site.xml file
        
        # retrieve vnet id based on vnetname
        $vNetID = Get-AzureVNetSite -VNetName $vNetName | %{$_.id}
        
        $config = New-AzureHDInsightClusterConfig `
                        -ClusterSizeInNodes $hbaseClusterSize `
                        -ClusterType HBase `
                        -VirtualNetworkId $vNetID `
                        -SubnetName $subNetName `
                    | Set-AzureHDInsightDefaultStorage `
                        -StorageAccountName $storageAccountName `
                        -StorageAccountKey $storageAccountKey `
                        -StorageContainerName $blobContainerName `
                    | Add-AzureHDInsightConfigValues `
                        -HBase $hbaseConfigValues
        
        # provision HDInsight cluster
        $hadoopUserPassword = ConvertTo-SecureString -String $hadoopUserpw -AsPlainText -Force     
        $credential = New-Object System.Management.Automation.PSCredential($hadoopUserLogin,$hadoopUserPassword)
        
        $config | New-AzureHDInsightCluster -Name $hbaseClusterName -Location $location -Credential $credential



**Bereitstellen einer HBase Cluster in Contoso-VNet-US** 

- Verwenden Sie das gleiche Skript mit den folgenden Werten aus:

        $hbaseClusterName = "Contoso-HBase-US" # This is the HBase cluster name to be used.
        $vNetName = "Contoso-VNet-US"  # This name must match your Europe virtual network name.
        $storageAccountName = 'ContosoStoreUS'  

    Da Sie bereits mit Ihrem Azure-Konto verbunden haben, müssen Sie nicht mehr die folgenden Comlets ausführen:

        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName




## <a name="configure-dns-conditional-forwarder"></a>Konfigurieren von DNS-bedingte Weiterleitung

In [Konfigurieren von DNS für die virtuellen Netzwerke][hdinsight-hbase-replication-dns], Sie haben die DNS-Server für die beiden Netzwerke konfiguriert. Die HBase Cluster haben andere Domänensuffixe. Daher müssen Sie zusätzliche bedingte DNS-Weiterleitung konfigurieren.

Um bedingte Weiterleitung konfigurieren zu können, müssen Sie die Domänensuffixe zwei HBase Zuordnungseinheiten wissen. 

**Um die Domänensuffixe zwei HBase Zuordnungseinheiten zu finden.**

1. RDP in **Contoso-HBase-EU**.  Anweisungen finden Sie unter [Verwalten von Hadoop Cluster in HDInsight mithilfe des klassischen Azure-Portal][hdinsight-manage-portal]. Es ist tatsächlich headnode0 im Cluster.
2. Öffnen Sie ein Windows PowerShell-Konsole oder ein Eingabeaufforderungsfenster.
3. Führen Sie **Ipconfig**, und notieren Sie sich **Verbindung-spezifische DNS-Suffix**.
4. Bitte nicht die RDP-Sitzung zu schließen.  Sie benötigen sie später Auflösung des Domänennamens zu testen.
5. Wiederholen Sie diese Schritte, um die **Verbindung-spezifische DNS-Suffix** von **Contoso-HBase-US**herauszufinden.


**So konfigurieren Sie die DNS-Weiterleitung**
 
1.  RDP in **Contoso-DNS-EU**. 
2.  Klicken Sie auf die Windows-Taste auf der unteren linken Ecke.
2.  Klicken Sie auf **Verwaltung**.
3.  Klicken Sie auf **DNS**.
4.  Erweitern Sie im linken Bereich **DSN**, **Contoso-DNS-EU**aus.
5.  Mit der rechten Maustaste **Bedingte Weiterleitung**, und klicken Sie dann auf **Neue bedingte Weiterleitung**. 
5.  Geben Sie die folgenden Informationen ein:
    - **DNS-Domäne**: Geben Sie das DNS-Suffix von Contoso-HBase-US. Beispiel: Contoso-HBase-US.f5.internal.cloudapp.net.
    - **IP-Adressen der master-Server**: 10.2.0.4, also den Contoso-DNS-US des IP-Adresse eingeben. Überprüfen Sie die IP-Adresse ein. Ihr DNS-Server kann eine andere IP-Adresse haben.
6.  Drücken Sie die **EINGABETASTE**, und klicken Sie dann auf **OK**.  Nun werden Sie die Contoso-DNS-US des IP-Adresse von Contoso-DNS-EU auflösen.
7.  Wiederholen Sie die Schritte aus, um eine bedingte DNS-Weiterleitung der DNS-Dienst auf den Contoso-DNS-US-virtuellen Computern mit den folgenden Werten hinzuzufügen:
    - **DNS-Domäne**: Geben Sie das DNS-Suffix der Contoso-HBase-EU. 
    - **IP-Adressen der master-Server**: 10.2.0.4, also den Contoso-DNS-EU IP-Adresse eingeben.

**So testen Sie die Auflösung des Domänennamens**

1. Wechseln Sie zum Fenster Contoso-HBase-EU RDP.
2. Öffnen Sie ein Eingabeaufforderungsfenster.
3. Führen Sie den Pingbefehl:

        ping headnode0.[DNS suffix of Contoso-HBase-US]

    Das Protokoll ICM ist Worker Knoten HBase Zuordnungseinheiten aktiviert

4. Schließen Sie die RDP-Sitzung nicht. Sie können weiterhin später im Lernprogramm benötigen.
5. Wiederholen Sie dieselben Schritte aus, um die headnode0 der Contoso-HBase-EU von Contoso-HBase-US Pingen aus.

>[AZURE.IMPORTANT] DNS-Einträge müssen arbeiten, bevor Sie mit den nächsten Schritten fort wechseln können.

## <a name="enable-replication-between-hbase-tables"></a>Aktivieren der Replikation zwischen HBase Tabellen

Sie können nun eine HBase Beispieltabelle Replikation aktivieren und Testen Sie es dann mit einiger Daten. Sie verwenden die Beispieltabelle weist zwei Spalte Familien: persönlich und Office. 

Stellen Sie in diesem Lernprogramm Europe HBase Cluster als die Quelle Cluster, zu US-HBase als Zielcluster.

Erstellen Sie HBase Tabellen mit den gleichen Namen und die Spalte Familien auf Quell- und Ziel-Cluster, damit der Zielcluster weiß, wo die Daten gespeichert, die sie empfangen werden. Weitere Informationen zur Verwendung der HBase-Verwaltungsshell finden Sie unter [Erste Schritte mit Apache HBase in HDInsight][hdinsight-hbase-get-started].

**So erstellen Sie eine Tabelle HBase auf Contoso-HBase-EU**

1. Wechseln Sie zum Fenster RDP **Contoso-HBase-EU** .
2. Klicken Sie auf den Desktop **Hadoop Befehlszeile**.
2. Ändern Sie den Ordner im Stammverzeichnis HBase aus:

        cd %HBASE_HOME%\bin
3. Öffnen Sie die Verwaltungsshell HBase an:

        hbase shell
4. Erstellen einer Tabelle HBase:

        create 'Contacts', 'Personal', 'Office'
5. Schließen Sie entweder die RDP-Sitzung noch im Fenster Hadoop Befehlszeile nicht. Diese werden immer noch später im Lernprogramm benötigt werden.
    
**So erstellen Sie eine Tabelle HBase auf Contoso-HBase-US**

- Wiederholen Sie diese Schritte zum Erstellen der gleichen Tabelle auf Contoso-HBase-US an.


**Contoso-HBase-US als Peer Replikation hinzufügen**

1. Wechseln Sie zum Fenster RDP **Contso-HBase_EU** .
2. Fügen Sie aus der HBase Shell-Fenster die Zielcluster (Contoso-HBase-US) beispielsweise als Peer, hinzu:

        add_peer '1', 'zookeeper0.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper1.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper2.contoso-hbase-us.d4.internal.cloudapp.net:2181:/hbase'

    Im Beispiel ist das Domänensuffix *Contoso-Hbase-us.d4.internal.cloudapp.net*. Sie müssen entsprechend Ihrer Domäne Suffixes uns HBase Cluster aktualisiert. Stellen Sie sicher, dass es keine Leerzeichen zwischen den Hostnamen ist.

**So konfigurieren Sie jede Spalte Familie auf Quelle Cluster repliziert werden**

1. Konfigurieren Sie über die HBase Shell-Fenster der **Contso-HBase-EU** RDP Sitzung jede Spalte Familie repliziert werden:

        disable 'Contacts'
        alter 'Contacts', {NAME => 'Personal', REPLICATION_SCOPE => '1'}
        alter 'Contacts', {NAME => 'Office', REPLICATION_SCOPE => '1'}
        enable 'Contacts'

**Zum Hochladen von Daten in der Tabelle HBase**

Eine Beispiel-Datendatei wurde hochgeladen zu einem öffentlichen Azure Blob-Container mit dem folgenden URL:

        wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

Den Inhalt der Datei:

        8396    Calvin Raji 230-555-0191    5415 San Gabriel Dr.
        16600   Karen Wu    646-555-0113    9265 La Paz
        4324    Karl Xie    508-555-0163    4912 La Vuelta
        16891   Jonathan Jackson    674-555-0110    40 Ellis St.
        3273    Miguel Miller   397-555-0155    6696 Anchor Drive
        3588    Osarumwense Agbonile    592-555-0152    1873 Lion Circle
        10272   Julia Lee   870-555-0110    3148 Rose Street
        4868    Jose Hayes  599-555-0171    793 Crawford Street
        4761    Caleb Alexander 670-555-0141    4775 Kentucky Dr.
        16443   Terry Chander   998-555-0171    771 Northridge Drive

Die gleichen Datendatei in Ihren Cluster HBase hochladen können und importieren Sie die Daten von dort aus.

1. Wechseln Sie zum Fenster RDP **Contoso-HBase-EU** .
2. Klicken Sie auf den Desktop **Hadoop Befehlszeile**.
3. Ändern Sie den Ordner im Stammverzeichnis HBase aus:

        cd %HBASE_HOME%\bin

4. Laden Sie die Daten ein:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:HomePhone, Office:Address" -Dimporttsv.bulk.output=/tmpOutput Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /tmpOutput Contacts


## <a name="verify-that-data-replication-is-taking-place"></a>Stellen Sie sicher, dass die Datenreplikation stattfindet

Sie können überprüfen, dass die Replikation stattfindet durch Scannen die Tabellen aus beide Cluster mit den folgenden HBase Shell-Befehlen:

        Scan 'Contacts'


## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt HBase Replikation über zwei Datencenter konfigurieren. Weitere Informationen zu HDInsight und HBase Informationen finden Sie unter:

- [HDInsight Service-Seite](https://azure.microsoft.com/services/hdinsight/)
- [HDInsight Dokumentation](https://azure.microsoft.com/documentation/services/hdinsight/)
- [Erste Schritte mit Apache HBase in HDInsight][hdinsight-hbase-get-started]
- [HDInsight HBase (Übersicht)][hdinsight-hbase-overview]
- [Bereitstellen von HBase Cluster virtuellen Netzwerk Azure][hdinsight-hbase-provision-vnet]
- [Analysieren Sie in Echtzeit Twitter-Grüße mit HBase][hdinsight-hbase-twitter-sentiment]
- [Analysieren von Sensordaten mit Storm und HBase in HDInsight (Hadoop)][hdinsight-sensor-data]

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: powershell-install-configure.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-hbase-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
