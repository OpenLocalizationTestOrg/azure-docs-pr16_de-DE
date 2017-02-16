<properties
   pageTitle="Erstellen von HDInsight Cluster mit Azure Lake Datenspeicher mithilfe der PowerShell | Azure"
   description="Verwenden Sie zum Erstellen und Verwenden von HDInsight Cluster mit Azure Daten Lake Azure PowerShell"
   services="data-lake-store,hdinsight" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="nitinme"/>

# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-powershell"></a>Erstellen eines HDInsight Clusters mit Lake Datenspeicher mithilfe der PowerShell Azure

> [AZURE.SELECTOR]
- [Mithilfe von Portal](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Mithilfe der PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)
- [Verwenden von Ressourcenmanager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Informationen Sie zum Azure-PowerShell verwenden, um eine HDInsight Cluster (Hadoop, HBase oder Storm) mit Zugriff auf Azure Lake Datenspeicher konfigurieren. Einige wichtige Hinweise in dieser Version:

* Als zusätzlichen Speicherkonto kann nur **für Spark Cluster (Linux), und Hadoop/Storm Cluster (Windows und Linux)**, die Lake Datenspeicher verwendet werden. Das Standardkonto-Speicher für die solche Cluster werden weiterhin Azure Speicher Blobs (WASB).

* Als Standardspeicher oder weiteren Speicherplatz kann **für HBase Cluster (Windows und Linux)**, die Lake Datenspeicher verwendet werden.

> [AZURE.NOTE] Einige wichtige Punkte beachten. 
> 
> * Option zum Erstellen von HDInsight Cluster mit Zugriff auf Lake Datenspeicher steht nur für HDInsight Versionen 3,2 und 3.4 (für Hadoop, HBase und Storm Cluster auf Windows als auch Linux). Für Spark Cluster auf Linux ist diese Option nur auf HDInsight 3.4 Cluster verfügbar.
>
> * Wie zuvor erwähnt, ist Lake Datenspeicher als Standardspeicher für bestimmte Clustertypen (HBase) und weiteren Speicherplatz für andere Clustertypen (Hadoop, Spark Storm) verfügbar. Lake Datenspeicher als Konto zusätzlicher Speicher mit wirkt sich nicht auf Leistung oder die Möglichkeit, um die Speicherung aus dem Cluster schreibgeschützt aus. In einem Szenario, in dem Lake Datenspeicher als zusätzlichen Speicher verwendet wird, sind Cluster-bezogene Dateien (z. B. Protokolle, usw.) auf den Standardspeicher Azure Bibliothek geschrieben, während die Daten, die Sie bearbeiten möchten, in einem Konto Lake Datenspeicher gespeichert werden können.


In diesem Artikel bereitstellen wir einen Hadoop Cluster mit Lake Datenspeicher als zusätzlichen Speicher.

Konfigurieren von HDInsight für die Arbeit mit Lake Datenspeicher umfasst mithilfe der PowerShell die folgenden Schritte:

* Erstellen einer Azure Lake Datenspeicher
* Richten Sie für den Zugriff auf Lake Datenspeicher rollenbasierte Authentifizierung
* Erstellen Sie mit der Authentifizierung Lake Datenspeicher HDInsight cluster
* Führen Sie einen Test-Auftrag auf dem cluster

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/pricing/free-trial/).

- **Azure PowerShell 1.0 oder größer**. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

- **Windows SDK**. Sie können ihn [hier](https://dev.windows.com/en-us/downloads)installieren. Sie verwenden, um ein Sicherheitszertifikat zu erstellen.

- **Azure Active Directory-Dienst Tilgungsanteile**. Die Schritte in diesem Lernprogramm Bereitstellen von Anweisungen zum Erstellen einer Dienst Tilgungsanteile in Azure Active Directory. Jedoch müssen Sie Administrator Azure AD-werden sollen, erstellen einen Dienst Tilgungsanteile sein. Wenn Sie ein Azure AD-Administrator sind, können Sie diese Voraussetzung überspringen und Fortsetzen des Lernprogramms.
    
    **Wenn Sie kein Azure AD-Administrator sind**, werden Sie nicht die erforderlichen Schritte zum Erstellen einer Dienst Tilgungsanteile ausführen sein. In diesem Fall muss Ihre Azure AD-Administrator zuerst ein Dienst Tilgungsanteile erstellen, vor der Erstellung eines HDInsight Clusters mit Lake Datenspeicher. Darüber hinaus muss die Tilgungsanteile Dienst erstellt werden mit einem Zertifikat, wie bei [einen Dienst Hauptbenutzer mit Zertifikat erstellen](../resource-group-authenticate-service-principal.md#create-service-principal-with-certificate)beschrieben. 


## <a name="create-an-azure-data-lake-store"></a>Erstellen einer Azure Lake Datenspeicher

Wie folgt vor, um eine Lake Datenspeicher zu erstellen.

1. Öffnen Sie auf dem Desktop ein neues Azure PowerShell-Fenster zu, und geben Sie den folgenden Codeausschnitt. Wenn Sie aufgefordert werden, melden Sie sich, stellen Sie sicher, dass Sie in einem von Ihrem Admininistrators/Besitzer anmelden:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    >[AZURE.NOTE] Wenn Sie eine ähnliche Fehlermeldung `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` den Anbieter für Ressourcen Lake Datenspeicher registrieren, es ist möglich, dass Ihre Subsrcription nicht auf weißer Liste für Azure Lake Datenspeicher ist. Stellen Sie sicher, dass Sie Ihre Azure-Abonnement für public Preview-Version Lake Datenspeicher aktivieren, indem Sie diese [Anweisungen](data-lake-store-get-started-portal.md#signup)folgen.

3. Ein Konto Azure Lake Datenspeicher ist eine Azure Ressourcengruppe zugeordnet. Erstellen einer Ressourcengruppe Azure zunächst.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Erstellen einer Ressourcengruppe Azure] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateResourceGroup.png "Erstellen einer Ressourcengruppe Azure")

2. Erstellen Sie ein Azure Lake Datenspeicher-Konto an. Den Namen des Kontos, die, den Sie angeben, darf nur Kleinbuchstaben und Zahlen enthalten.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Erstellen einer Azure Daten dem Konto] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateADLAcc.png "Erstellen einer Azure Daten dem Konto")

3. Stellen Sie sicher, dass das Konto erfolgreich erstellt wurde.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Die Ausgabe für diesen sollte **erfüllt**werden.

4. Hochladen Sie einige Beispieldaten auf Azure Daten Lake. Wir verwenden diese später in diesem Artikel zur Überprüfung, dass die Daten aus einem HDInsight Cluster verfügbar sind. Wenn Sie einige Beispieldaten hochladen suchen, können Sie den Ordner **Krankenwagen Daten** aus dem [Azure Daten dem Git Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)abrufen.


        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Richten Sie für den Zugriff auf Lake Datenspeicher rollenbasierte Authentifizierung

Jede Azure-Abonnement ist ein Azure Active Directory zugeordnet. Benutzer und Dienste, die Zugriff auf Ressourcen des Abonnements mit dem klassischen Azure-Portal oder Azure Ressourcenmanager API müssen zuerst mit dieser Azure Active Directory authentifizieren. Bei Azure-Abonnements und-Diensten Zugriff durch die geeignete Rolle für eine Ressource Azure zuweisen.  Für Dienste identifiziert einen Dienst Tilgungsanteile den Dienst in Azure Active Directory (AAD). In diesem Abschnitt wird veranschaulicht, wie einen Anwendungsdienst, wie HDInsight, Zugriff auf eine Azure Ressource (das Konto Azure Lake Datenspeicher, die Sie zuvor erstellt haben) erteilen, indem Sie einen Dienst Tilgungsanteile für die Anwendung erstellen und Zuweisen von Rollen zu, die über Azure PowerShell.

Um Active Directory-Authentifizierung für Azure Daten See eingerichtet haben, müssen Sie die folgenden Aufgaben ausführen.

* Erstellen eines selbstsignierten Zertifikats
* Erstellen Sie eine Anwendung in Azure Active Directory und einem Dienst Tilgungsanteile

### <a name="create-a-self-signed-certificate"></a>Erstellen eines selbstsignierten Zertifikats

Stellen Sie sicher, dass Sie [Windows SDK](https://dev.windows.com/en-us/downloads) , bevor Sie mit den Schritten in diesem Abschnitt installiert haben. Sie müssen auch ein Verzeichnis, z. B. **C:\mycertdir**, erstellt haben, wo das Zertifikat erstellt werden soll.

1. Aus dem Fenster PowerShell, navigieren Sie zu dem Speicherort, auf dem Windows SDK installiert (in der Regel `C:\Program Files (x86)\Windows Kits\10\bin\x86` und Verwenden der [MakeCert] [ makecert] Programm, mit der ein selbst signiertes Zertifikat und einem privaten Schlüssel erstellen. Verwenden Sie die folgenden Befehle.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir
        $startDate = (Get-Date).ToString('MM/dd/yyyy')
        $endDate = (Get-Date).AddDays(365).ToString('MM/dd/yyyy')

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -b $startDate -e $endDate -r -len 2048

    Sie werden aufgefordert, das Kennwort für private Schlüssel eingeben. Nachdem der Befehl erfolgreich ausgeführt wird, sollte eine **CertFile.cer** und **mykey.pvk** im Zertifikatsverzeichnis die angegebene angezeigt werden.

4. Verwenden Sie die [Pvk2Pfx] [ pvk2pfx] Programm, mit die erstellten MakeCert .pvk und CER-Dateien in einer PFX-Datei konvertieren. Führen Sie den folgenden Befehl ein.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Wenn Sie aufgefordert werden Geben Sie das Kennwort für private Schlüssel Sie zuvor angegeben haben. Der Wert, den Sie für den Parameter **-Bestellung** angeben, ist das Kennwort, das die PFX-Datei zugeordnet ist. Nach dem erfolgreichen des Befehls Abschluss sollte auch eine CertFile.pfx im Zertifikatsverzeichnis angezeigt werden, die Sie angegeben haben.

###  <a name="create-an-azure-active-directory-and-a-service-principal"></a>Erstellen einer Azure Active Directory und einem Dienst Tilgungsanteile

In diesem Abschnitt führen Sie die Schritte zum Erstellen eines Diensts Hauptbenutzer für eine Anwendung Azure Active Directory, Zuweisen einer Rolle zu den wichtigsten Dienst und authentifizieren als die der Tilgungsanteile können, indem Sie ein Zertifikat aus. Führen Sie die folgenden Befehle zum Erstellen einer Anwendung in Azure Active Directory.

1. Fügen Sie die folgenden Cmdlets im Fenster PowerShell-Konsole aus. Stellen Sie sicher, dass der Wert für die **DisplayName -** Eigenschaft angegebenen eindeutig ist. Darüber hinaus die Werte für **HomePage -** und **- IdentiferUris** Platzhalter Fehlerwerte und nicht überprüft werden.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
                    -DisplayName "HDIADL" `
                    -HomePage "https://contoso.com" `
                    -IdentifierUris "https://mycontoso.com" `
                    -KeyValue $credential  `
                    -KeyType "AsymmetricX509Cert"  `
                    -KeyUsage "Verify"  `
                    -StartDate $startDate  `
                    -EndDate $endDate

        $applicationId = $application.ApplicationId

2. Erstellen einer Dienst Tilgungsanteile, die mit der Anwendung-ID.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id

3. Erteilen Sie den Hauptbenutzer--Zugriff auf die Lake Datenspeicher Datei/temporärer Ordner, den Sie aus dem Cluster HDInsight zugreifen. Der nachstehenden Codeausschnitt bietet Zugriff auf der Stammebene der Lake Datenspeicher-Konto an.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All

    Aufgefordert werden Geben Sie **Y** , um zu bestätigen.

## <a name="create-an-hdinsight-cluster-with-authentication-to-data-lake-store"></a>Erstellen eines HDInsight Clusters mit der Authentifizierung Lake Datenspeicher

In diesem Abschnitt erstellen wir einen HDInsight Hadoop Cluster. In dieser Version müssen HDInsight Cluster und Lake Datenspeicher an derselben Stelle (ostasiatische US 2).

1. Beginnen Sie mit die Abonnement Mandanten-ID abrufen Sie benötigen, die später.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. In dieser Version kann für einen Cluster Hadoop Lake Datenspeicher nur als einer weiteren Speicherplatz für den Cluster verwendet werden. Der Standardspeicher bleibt die Blobs Azure-Speicher (WASB). Ja, wird der Speicherkonto und Speichercontainer für den Cluster erforderlich zuerst erstellen.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName | %{ $_.Key1 }
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext

3. Den Cluster HDInsight zu erstellen. Verwenden Sie die folgenden Cmdlets.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $rdpCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.2" -RdpCredential $rdpCredentials -RdpAccessExpiry (Get-Date).AddDays(14) -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Nach dem erfolgreichen des Cmdlets Abschluss sollte eine Ausgabe wie folgt angezeigt:

        Name                      : hdiadlcluster
        Id                        : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/hdiadlgroup/providers/Mi
                                    crosoft.HDInsight/clusters/hdiadlcluster
        Location                  : East US 2
        ClusterVersion            : 3.2.7.707
        OperatingSystemType       : Windows
        ClusterState              : Running
        ClusterType               : Hadoop
        CoresUsed                 : 16
        HttpEndpoint              : hdiadlcluster.azurehdinsight.net
        Error                     :
        DefaultStorageAccount     :
        DefaultStorageContainer   :
        ResourceGroup             : hdiadlgroup
        AdditionalStorageAccounts :

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>HDInsight Cluster Lake Datenspeicher verwenden Testaufträge ausgeführt

Nachdem Sie einen HDInsight Cluster konfiguriert haben, können Sie Einzelvorgänge Test ausführen, auf dem Cluster zu testen, ob der Cluster HDInsight Lake Datenspeicher zugreifen kann. Hierzu werden wir einen Stichprobe Struktur Auftrag ausführen, der erstellt eine Tabelle mit den Beispieldaten, die Sie zuvor in Ihren Lake Datenspeicher hochgeladen.

### <a name="for-a-linux-cluster"></a>Für einen Linux cluster

In diesem Abschnitt werden Sie SSH in den Cluster und Ausführen der Abfrage Struktur Stichprobe. Windows bietet keinen integrierten SSH-Client. Es empfiehlt sich, mit **kitten**, die von [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)heruntergeladen werden kann.

Weitere Informationen zur Verwendung von kitten finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. Nachdem die Verbindung hergestellt wurde, starten Sie die Struktur CLI mit dem folgenden Befehl:

        hive

2. Geben Sie die CLI mithilfe der folgenden Aussagen zum Erstellen einer neuen Tabelle mit dem Namen **Fahrzeuge** mithilfe der Beispieldaten im Datenspeicher Lake aus:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Sie sollte eine Ausgabe ähnlich der folgenden angezeigt:

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


### <a name="for-a-windows-cluster"></a>Bei einem Windows-cluster

Verwenden Sie die folgenden Cmdlets zum Ausführen der Abfrage Struktur. In dieser Abfrage wir Erstellen einer Tabelle aus den Daten im Datenspeicher Sees und führen Sie dann auf die erstellte Tabelle eine Auswahlabfrage.

    $queryString = "DROP TABLE vehicles;" + "CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://$dataLakeStoreName.azuredatalakestore.net:443/';" + "SELECT * FROM vehicles LIMIT 10;"

    $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString

    $hiveJob = Start-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $httpCredentials

    Wait-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $httpCredentials

Dies kann die folgende Ausgabe sind. **ExitValue** von 0 in der Ausgabe schlägt vor, dass der Auftrag erfolgreich abgeschlossen.

    Cluster         : hdiadlcluster.
    HttpEndpoint    : hdiadlcluster.azurehdinsight.net
    State           : SUCCEEDED
    JobId           : job_1445386885331_0012
    ParentId        :
    PercentComplete :
    ExitValue       : 0
    User            : admin
    Callback        :
    Completed       : done

Rufen Sie die Ausgabe können Sie von den Auftrag ab, indem Sie das folgende Cmdlet aus:

    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $hiveJob.JobId -DefaultContainer $containerName -DefaultStorageAccountName $storageAccountName -DefaultStorageAccountKey $storageAccountKey -ClusterCredential $httpCredentials

Die Auftragsausgabe sieht folgendermaßen aus:

    1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
    1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
    1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
    1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
    1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
    1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
    1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
    1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
    1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
    1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


## <a name="access-data-lake-store-using-hdfs-commands"></a>Access Lake Datenspeicher HDFS Befehle verwenden

Nachdem Sie HDInsight Cluster um Lake Datenspeicher verwenden konfiguriert haben, können Sie die HDFS Shell-Befehle, auf den Speicher zuzugreifen.

### <a name="for-a-linux-cluster"></a>Für einen Linux cluster

In diesem Abschnitt Sie SSH zum Cluster wird, und führen Sie die Befehle HDFS. Windows bietet keinen integrierten SSH-Client. Es empfiehlt sich, mit **kitten**, die von [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)heruntergeladen werden kann.

Weitere Informationen zur Verwendung von kitten finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Nachdem die Verbindung hergestellt wurde, verwenden Sie den folgenden Befehl ein HDFS Dateisystem die Dateien im Datenspeicher Lake aufgelistet.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

Dies sollte der Datei aufgeführt, die Sie zuvor in die Datenspeicher Lake hochgeladen.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

Sie können auch die `hdfs dfs -put` Befehl aus, um mit dem Lake Datenspeicher einige Dateien hochladen, und verwenden Sie dann `hdfs dfs -ls` zu überprüfen, ob die Dateien erfolgreich hochgeladen wurden.


### <a name="for-a-windows-cluster"></a>Bei einem Windows-cluster

1. Melden Sie sich auf das neue [Azure-Portal](https://portal.azure.com)an.

2. Klicken Sie auf **Durchsuchen**, klicken Sie auf **HDInsight Cluster**, und klicken Sie dann auf HDInsight Cluster, den Sie erstellt haben.

3. Klicken Sie auf **Remote Desktop**in das Blade Cluster, und klicken Sie dann in das Blade **Remotedesktop** auf **Verbinden**.

    ![Remote in HDI cluster] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.HDI.PS.Remote.Desktop.png "Erstellen einer Ressourcengruppe Azure")

    Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen, die Sie für den remote desktop Benutzer bereitgestellt.

4. In der remote-Sitzung starten von Windows PowerShell und verwenden Sie die HDFS Dateisystem-Befehle, die Dateien im Datenspeicher Lake Azure aufgelistet.

        hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

    Dies sollte der Datei aufgeführt, die Sie zuvor in die Datenspeicher Lake hochgeladen.

        15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
        Found 1 items
        -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/vehicle1_09142014.csv

    Sie können auch die `hdfs dfs -put` Befehl aus, um mit dem Lake Datenspeicher einige Dateien hochladen, und verwenden Sie dann `hdfs dfs -ls` zu überprüfen, ob die Dateien erfolgreich hochgeladen wurden.

## <a name="see-also"></a>Siehe auch

* [Portal: Erstellen eines HDInsight Clusters um Lake Datenspeicher verwenden](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
