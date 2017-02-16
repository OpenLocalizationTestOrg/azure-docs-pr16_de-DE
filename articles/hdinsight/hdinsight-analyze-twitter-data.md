<properties
    pageTitle="Analysieren von Twitter-Daten mit Hadoop in HDInsight | Microsoft Azure"
    description="Erfahren Sie, wie Struktur Twitter Datenanalyse auf Hadoop in HDInsight finden Sie die Häufigkeit der Verwendung eines bestimmten Worts verwenden."
    services="hdinsight"
    documentationCenter=""
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

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Analysieren von Twitter-Daten, die mithilfe der Struktur in HDInsight

Websites für soziale Netzwerke gehören zu den wichtigsten steuernde erzwingt für große Daten Annahme. Von Websites wie Twitter bereitgestellten öffentlichen APIs sind eine hilfreiche Datenquelle für Analyse und beliebte Trends. In diesem Lernprogramm Sie Tweets mithilfe einer Twitter streaming-API abrufen, und verwenden Sie dann Apache Struktur Azure HDInsight beim Abrufen einer Liste der Twitter-Benutzer, die die meisten Tweets gesendet hat, die ein bestimmtes Wort enthalten.

> [AZURE.NOTE] Die Schritte in diesem Dokument erfordern einen Windows-basiertem HDInsight Cluster. Bestimmte Schritte zu einem Linux-basierten Cluster finden Sie unter [Analysieren Twitter-Daten mit Struktur in HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).


##<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Eine Arbeitsstationen** mit Azure PowerShell installiert und konfiguriert. 

    Wenn Sie Windows PowerShell-Skripts ausführen möchten, müssen Sie Azure PowerShell als Administrator ausführen und die Ausführungsrichtlinie auf *RemoteSigned*festgelegt. [Ausführen von Windows PowerShell-Skripts]finden Sie unter[powershell-script].

    Stellen Sie bevor Windows PowerShell-Skripts sicher, dass Sie Ihr Abonnement Azure verbunden sind, mithilfe von das folgende Cmdlet aus:

        Login-AzureRmAccount

    Wenn Sie mehrere Azure-Abonnements verfügen, verwenden Sie das folgende Cmdlet aus, um das aktuelle Abonnement festlegen:

        Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Ein Azure HDInsight Cluster**. Anweisungen Cluster bereitgestellt, finden Sie unter [Erste Schritte mit HDInsight] [ hdinsight-get-started] oder [Bereitstellen von HDInsight Cluster] [hdinsight-provision]. Sie benötigen den Clusternamen später im Lernprogramm.

Die folgende Tabelle listet die Dateien in diesem Lernprogramm verwendet:

Dateien|Beschreibung
---|---
/Tutorials/Twitter/Data/TWEETS.txt|Die Quelldaten für das Projekt Struktur.
/Tutorials/Twitter/Output|Die Ausgabeordner für die Struktur. Der Struktur Auftrag Ausgabe Standarddateinamen ist **000000_0**.
Tutorials/Twitter/Twitter.hql|Die Skriptdatei HiveQL.
/Tutorials/Twitter/Jobstatus|Der Status Hadoop.


##<a name="get-twitter-feed"></a>Get-Twitter-Feeds

In diesem Lernprogramm verwenden Sie das [streaming APIs Twitter][twitter-streaming-api]. Die spezielle Twitter-API Sie verwenden streaming ist [Status/Filter][twitter-statuses-filter].

>[AZURE.NOTE] Eine Datei mit 10.000 Tweets und die Struktur Skriptdatei (im nächsten Abschnitt behandelt) haben in einem Öffentlichen Blob Container hochgeladen wurde. Wenn Sie die hochgeladenen Dateien verwenden möchten, können Sie diesen Abschnitt überspringen.

[TWEETS Daten](https://dev.twitter.com/docs/platform-objects/tweets) wird im Format JavaScript Object Notation (JSON) gespeichert, die eine komplexe geschachtelte Struktur enthält. Statt viele Codezeilen mithilfe einer herkömmlichen Programmiersprache schreiben, können transformiert diese geschachtelte Struktur in eine strukturtabelle, damit sie nach einer SQL Structured Query Language () abgefragt werden kann – wie Sprache namens HiveQL.

Twitter verwendet OAuth autorisierten Zugriff auf seine API bereit. OAuth ist ein Protokoll zur Authentifizierung, die Benutzern Genehmigen von Applications in deren Auftrag fungieren, ohne die Freigabe ihres Kennworts ermöglicht. Weitere Informationen kann vom Hueniverse am [oauth.net](http://oauth.net/) oder die hervorragende [Einführung in OAuth](http://hueniverse.com/oauth/) gefunden werden.

Im erste Schritt OAuth verwenden besteht im Erstellen einer neuen Anwendungs auf der Website Twitter-Entwickler.

**So erstellen Sie eine Twitter-Anwendung**

1. Melden Sie sich bei [https://apps.twitter.com/](https://apps.twitter.com/). Wenn Sie einen Twitter-Konto besitzen, klicken Sie auf den Link **Jetzt registrieren** .
2. Klicken Sie auf **neue App erstellen**.
3. Geben Sie **Name**, **Beschreibung**, **Website**aus. Sie können eine URL für das Feld **Website** zusammensetzt. Die folgende Tabelle zeigt einige Beispielwerte zu verwenden:

    Feld|Wert
    ---|---
    Namen|MyHDInsightApp
    Beschreibung|MyHDInsightApp
    Website|http://www.myhdinsightapp.com

4. Überprüfen Sie **Ja, ich stimme zu**, und klicken Sie dann auf **Erstellen Ihrer Twitter-Anwendung**.
5. Klicken Sie auf der Registerkarte **Berechtigungen** . Die Standardberechtigung ist **schreibgeschützt**. Dies ist ausreichend für dieses Lernprogramm.
6. Klicken Sie auf der Registerkarte **Tasten und Access Token** .
7. Klicken Sie auf **Erstellen Meine Access Token**.
8. Klicken Sie in der oberen rechten Ecke der Seite auf **OAuth testen** .
9. Notieren Sie sich **Consumer Schlüssel**, **Consumer geheim**, **Access Token**und **Access token geheim**. Sie benötigen die Werte später im Lernprogramm.

In diesem Lernprogramm verwenden Sie Windows PowerShell, um den Anruf Webdienst zu gestalten. Eine .NET C#-Beispiel, finden Sie unter [Analysieren in Echtzeit Twitter-Grüße mit HBase in HDInsight][hdinsight-hbase-twitter-sentiment]. Das andere beliebte Tool auf Web Service-Aufrufe ist [*Aufrollen*][curl]. Von [hier]Curl heruntergeladen werden kann[curl-download].

>[AZURE.NOTE] Wenn Sie den Befehl Curl in Windows verwenden, verwenden Sie doppelte Anführungszeichen statt einzelnen Angebote für die Optionswerte.

**Tweets abrufen**

1. Öffnen der Windows PowerShell Integrated Scripting-Umgebung (ISE). (Geben Sie auf der Windows 8-Startbildschirm **PowerShell_ISE** und klicken Sie dann auf **Windows PowerShell ISE**. [Starten Sie Windows PowerShell unter Windows 8 und Windows]finden Sie unter[powershell-start].)

2. Kopieren Sie das folgende Skript in den Skriptbereich:

        #region - variables and constants
        $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

        # Enter the OAuth information for your Twitter application
        $oauth_consumer_key = "<TwitterAppConsumerKey>";
        $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
        $oauth_token = "<TwitterAppAccessToken>";
        $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

        $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

        $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
        $track = [System.Uri]::EscapeDataString($trackString);
        $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
        #endregion

        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        Login-AzureRmAccount
        #endregion

        #region - Create a block blob object for writing tweets into Blob storage
        Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $containerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

        Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
        Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

        Write-Host "Create block blob object ..." -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($containerName)
        $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
        #end region

        # region - Format OAuth strings
        Write-Host "Format oauth strings ..." -ForegroundColor Green
        $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
        $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
        $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

        $signature = "POST&";
        $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
        $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
        $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
        $signature += [System.Uri]::EscapeDataString("track=" + $track);

        $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

        $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
        $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
        $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

        $oauth_authorization = 'OAuth ';
        $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
        $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
        $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
        $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
        $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
        $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
        $oauth_authorization += 'oauth_version="1.0"';

        $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
        #endregion

        #region - Read tweets
        Write-Host "Create HTTP web request ..." -ForegroundColor Green
        [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
        $request.Method = "POST";
        $request.Headers.Add("Authorization", $oauth_authorization);
        $request.ContentType = "application/x-www-form-urlencoded";
        $body = $request.GetRequestStream();

        $body.write($post_body, 0, $post_body.length);
        $body.flush();
        $body.close();
        $response = $request.GetResponse() ;

        Write-Host "Start stream reading ..." -ForegroundColor Green

        Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream

        $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

        $inrec = $sReader.ReadLine()
        $count = 0
        while (($inrec -ne $null) -and ($count -le $lineMax))
        {
            if ($inrec -ne "")
            {
                Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

                $writeStream.WriteLine($inrec)
                $count ++
            }

            $inrec=$sReader.ReadLine()
        }
        #endregion

        #region - Write tweets to Blob storage
        Write-Host "Write to the destination blob ..." -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $destBlob.UploadFromStream($memStream)

        $sReader.close()
        #endregion

        Write-Host "Completed!" -ForegroundColor Green

3. Festlegen der ersten fünf bis acht Variablen im Skript:


    Variable|Beschreibung
    ---|---
    $clusterName|Dies ist der Name des HDInsight Cluster, in dem Sie die Anwendung ausführen möchten.
    $oauth_consumer_key|Dies ist der Twitter-Anwendung **Consumer-Taste** , die Sie zuvor notiert bei der Erstellung der Twitter-Anwendungs.
    $oauth_consumer_secret|Dies ist der Twitter-Anwendung **Consumer geheim** , die Sie zuvor notiert.
    $oauth_token|Dies ist der Twitter-Anwendung **Access Token** , die Sie zuvor notiert.
    $oauth_token_secret|Dies ist der Twitter-Anwendung **Access token geheim** , die Sie zuvor notiert.
    $destBlobName|Dies ist der Name der Ausgabe Blob. Der Standardwert ist **tutorials/twitter/data/tweets.txt**. Wenn Sie den Standardwert ändern, müssen Sie die Windows PowerShell-Skripts entsprechend zu aktualisieren.
    $trackString|Der Webdienst wird im Zusammenhang mit dieser Schlüsselwörter Tweets zurückgeben. Der Standardwert ist **Azure, Cloud, HDInsight**. Wenn Sie den Standardwert ändern, werden Sie entsprechend der Windows PowerShell-Skripts aktualisieren.
    $lineMax|Der Wert bestimmt, wie viele Tweets das Skript gelesen werden. Es dauert ungefähr drei Minuten zum Lesen von 100 Tweets. Sie können eine größere Anzahl festlegen, doch dauert es mehr Zeit zum Herunterladen.

5. Drücken Sie **F5** , um das Skript auszuführen. Wenn eine dieses Problem zu umgehen, Probleme auftreten wählen Sie alle Zeilen aus, und drücken Sie dann **F8**.
6. Die gilt "Abgeschlossen!" angezeigt. am Ende der Ausgabe. Fehlermeldungen werden in Rot angezeigt werden.

Wie eine Prozedur Überprüfung können Sie die Ausgabedatei, **/tutorials/twitter/data/tweets.txt**, auf Ihre Azure Blob-Speicher mithilfe einer Azure-Speicher-Explorer oder Azure PowerShell überprüfen. Ein Beispiel für Windows PowerShell-Skript für die Auflistung von Dateien, finden Sie unter [verwenden Blob-Speicher mit HDInsight][hdinsight-storage-powershell].



##<a name="create-hiveql-script"></a>Erstellen Sie HiveQL-Skript

Verwenden Azure PowerShell, können Sie mehrere HiveQL Anweisungen eine nacheinander ausführen oder verpacken die HiveQL-Anweisung in eine Skriptdatei. In diesem Lernprogramm erstellen Sie ein HiveQL Skript. Die Skriptdatei muss auf Azure Blob-Speicher hochgeladen werden. Im nächsten Abschnitt führen Sie die Skriptdatei mithilfe der PowerShell Azure.

>[AZURE.NOTE] In einem Öffentlichen Blob Container haben die Struktur Skriptdatei und eine Datei mit 10.000 Tweets hochgeladen wurde. Wenn Sie die hochgeladenen Dateien verwenden möchten, können Sie diesen Abschnitt überspringen.

Das Skript HiveQL werden die folgenden Schritte aus:

1. **Legen Sie die Tabelle Tweets_raw** im Fall der Tabelle ist bereits vorhanden.
2. **Erstellen Sie die strukturtabelle Tweets_raw**. Diese temporäre strukturierten strukturtabelle enthält die Daten für eine weitere extrahieren, Transformieren und laden (ETL) Verarbeitung. Informationen zu Partitionen, finden Sie unter [Lernprogramm Struktur][apache-hive-tutorial].  
3. **Laden von Daten** aus der Quellordner /tutorials/twitter/data. Große Tweets Dataset in verschachtelten JSON-Format hat jetzt in eine temporäre Struktur Tabellenstruktur transformiert wurden.
3. **Legen Sie die Tabelle Tweets** im Fall der Tabelle ist bereits vorhanden.
4. **Erstellen der Tabelle Tweets**. Bevor Sie mithilfe der Struktur gegen das Dataset Tweets Abfragen können, müssen Sie ein anderes ETL-Prozess ausführen. Diese ETL-Prozess definiert eine ausführlichere Tabellenschema für die Daten, die in der Tabelle "Twitter_raw" gespeichert sind.  
5. **Einfügen einer Tabelle überschreiben**. Dieses komplexe Struktur Skript wird durch den Cluster Hadoop deaktivieren eine Reihe von lange MapReduce Aufträge Starten eines. Abhängig von Ihrer Dataset und die Größe für den Cluster kann dies etwa 10 Minuten dauern.
6. **Einfügen Verzeichnis zu überschreiben**. Ausführen einer Abfrage, und das Dataset in eine Datei ausgeben. Diese Abfrage zurückgegeben werden kann eine Liste der Twitter-Benutzer, die meisten Tweets gesendet hat, die das Wort enthalten "Azure".

**Erstellen Sie ein Skript Struktur und zum Azure hochladen**

1. Öffnen Sie Windows PowerShell ISE.
2. Kopieren Sie das folgende Skript in den Skriptbereich:

        #region - variables and constants
        $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
        $subscriptionID = "<Azure Subscription ID>"
        
        $sourceDataPath = "/tutorials/twitter/data"
        $outputPath = "/tutorials/twitter/output"
        $hqlScriptFile = "tutorials/twitter/twitter.hql"
        
        $hqlStatements = @"
        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        
        DROP TABLE tweets_raw;
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '$sourceDataPath';
        
        DROP TABLE tweets;
        CREATE TABLE tweets
        (
            id BIGINT,
            created_at STRING,
            created_at_date STRING,
            created_at_year STRING,
            created_at_month STRING,
            created_at_day STRING,
            created_at_time STRING,
            in_reply_to_user_id_str STRING,
            text STRING,
            contributors STRING,
            retweeted STRING,
            truncated STRING,
            coordinates STRING,
            source STRING,
            retweet_count INT,
            url STRING,
            hashtags array<STRING>,
            user_mentions array<STRING>,
            first_hashtag STRING,
            first_user_mention STRING,
            screen_name STRING,
            name STRING,
            followers_count INT,
            listed_count INT,
            friends_count INT,
            lang STRING,
            user_location STRING,
            time_zone STRING,
            profile_image_url STRING,
            json_response STRING
        );
        
        FROM tweets_raw
        INSERT OVERWRITE TABLE tweets
        SELECT
            cast(get_json_object(json_response, '$.id_str') as BIGINT),
            get_json_object(json_response, '$.created_at'),
            concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
            substr (get_json_object(json_response, '$.created_at'),27,4)),
            substr (get_json_object(json_response, '$.created_at'),27,4),
            case substr (get_json_object(json_response, '$.created_at'),5,3)
                when "Jan" then "01"
                when "Feb" then "02"
                when "Mar" then "03"
                when "Apr" then "04"
                when "May" then "05"
                when "Jun" then "06"
                when "Jul" then "07"
                when "Aug" then "08"
                when "Sep" then "09"
                when "Oct" then "10"
                when "Nov" then "11"
                when "Dec" then "12" end,
            substr (get_json_object(json_response, '$.created_at'),9,2),
            substr (get_json_object(json_response, '$.created_at'),12,8),
            get_json_object(json_response, '$.in_reply_to_user_id_str'),
            get_json_object(json_response, '$.text'),
            get_json_object(json_response, '$.contributors'),
            get_json_object(json_response, '$.retweeted'),
            get_json_object(json_response, '$.truncated'),
            get_json_object(json_response, '$.coordinates'),
            get_json_object(json_response, '$.source'),
            cast (get_json_object(json_response, '$.retweet_count') as INT),
            get_json_object(json_response, '$.entities.display_url'),
            array(
                trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
            array(
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            get_json_object(json_response, '$.user.screen_name'),
            get_json_object(json_response, '$.user.name'),
            cast (get_json_object(json_response, '$.user.followers_count') as INT),
            cast (get_json_object(json_response, '$.user.listed_count') as INT),
            cast (get_json_object(json_response, '$.user.friends_count') as INT),
            get_json_object(json_response, '$.user.lang'),
            get_json_object(json_response, '$.user.location'),
            get_json_object(json_response, '$.user.time_zone'),
            get_json_object(json_response, '$.user.profile_image_url'),
            json_response
        WHERE (length(json_response) > 500);
        
        INSERT OVERWRITE DIRECTORY '$outputPath'
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;
        "@
        #endregion
        
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        
        Try{
            Get-AzureRmSubscription
        }
        Catch{
            Login-AzureRmAccount
        }
        
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
        
        #endregion
        
        #region - Create a block blob object for writing the Hive script file
        Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $defaultBlobContainerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
        
        Write-Host "Define the connection string ..." -ForegroundColor Green
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
        
        Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
        $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)
        
        Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream
        $writeStream.Writeline($hqlStatements)
        #endregion
        
        #region - Write the Hive script file to Blob storage
        Write-Host "Write to the destination blob ... " -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $hqlScriptBlob.UploadFromStream($memStream)
        #endregion
        
        Write-Host "Completed!" -ForegroundColor Green

        

4. Legen Sie die ersten beiden Variablen im Skript aus:

    Variable|Beschreibung
    ---|---
    $clusterName|Geben Sie den Namen der HDInsight Cluster, in dem Sie die Anwendung ausführen möchten.
    $subscriptionID|Geben Sie Ihre Azure-Abonnement-ID an.
    $sourceDataPath|Die Position von Azure BLOB-Speicher, in dem die Struktur Abfragen die Daten gelesen werden. Sie brauchen diese Variable zu ändern.
    $outputPath|Die Stelle, an der die Struktur Abfragen der Ergebnisse Ausgabe Azure Blob-Speicherort. Sie brauchen diese Variable zu ändern.
    $hqlScriptFile|Die Position und den Dateinamen der Skriptdatei HiveQL. Sie brauchen diese Variable zu ändern.

5. Drücken Sie **F5** , um das Skript auszuführen. Wenn eine dieses Problem zu umgehen, Probleme auftreten wählen Sie alle Zeilen aus, und drücken Sie dann **F8**.
6. Die gilt "Abgeschlossen!" angezeigt. am Ende der Ausgabe. Fehlermeldungen werden in Rot angezeigt werden.

Wie eine Prozedur Überprüfung können Sie die Ausgabedatei, **/tutorials/twitter/twitter.hql**, auf Ihre Azure Blob-Speicher mithilfe einer Azure-Speicher-Explorer oder Azure PowerShell überprüfen. Ein Beispiel für Windows PowerShell-Skript für die Auflistung von Dateien, finden Sie unter [verwenden Blob-Speicher mit HDInsight][hdinsight-storage-powershell].  


##<a name="process-twitter-data-by-using-hive"></a>Verarbeiten von Daten mithilfe von Struktur Twitter

Sie haben die vorbereitenden abgeschlossen. Nun können Sie das Skript Struktur aufrufen und die Ergebnisse überprüfen.

### <a name="submit-a-hive-job"></a>Senden eines Auftrags Struktur

Verwenden Sie das folgende Windows PowerShell-Skript zum Ausführen des Skripts Struktur. Sie benötigen, um die erste Variable festzulegen.

>[AZURE.NOTE] Zum Verwenden der Tweets und das HiveQL-Skript, das Sie hochgeladen in den letzten beiden Abschnitten, legen Sie $hqlScriptFile auf "/ tutorials/twitter/twitter.hql". Wenn diejenigen, die für Sie zu einer öffentlichen Blob hochgeladen wurden verwenden möchten, legen Sie $hqlScriptFile auf "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    $httpUserName = "admin"
    $httpUserPassword = "<HDInsight Cluster HTTP User Password>"
    
    #use one of the following
    $hqlScriptFile = "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
    $hqlScriptFile = "/tutorials/twitter/twitter.hql"
    
    $statusFolder = "/tutorials/twitter/jobstatus"
    #endregion
    
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    
    #region - Invoke Hive
    Write-Host "Invoke Hive ... " -ForegroundColor Green
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential 
    $response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable
    
    Write-Host "Display the standard error log ... " -ForegroundColor Green
    $jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
    #endregion

### <a name="check-the-results"></a>Überprüfen Sie die Ergebnisse

Verwenden Sie das folgende Windows PowerShell-Skript, um die Struktur Auftragsausgabe überprüfen. Sie müssen die ersten beiden Variablen festlegen.

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    
    $blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
    #engregion
    
    #region - Create an Azure storage context object
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
    
    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey  
    #endregion
    
    #region - Download blob and display blob
    Write-Host "Download the blob ..." -ForegroundColor Green
    cd $HOME
    Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force
    
    Write-Host "Display the output ..." -ForegroundColor Green
    Write-Host "==================================" -ForegroundColor Green
    cat "./$blob"
    Write-Host "==================================" -ForegroundColor Green
    #end region

> [AZURE.NOTE] Die strukturtabelle wird als Feldtrennzeichen \001 verwendet. Das Trennzeichen wird nicht in der Ausgabe angezeigt.

Nachdem die Analyseergebnisse in Azure Blob-Speicher erteilt wurden, können Sie Exportieren von Daten in einer SQL Azure Database-SQL Server, exportieren Sie die Daten nach Excel mithilfe von Power Query oder Verbinden Ihrer Anwendung mit den Daten mithilfe der Struktur ODBC-Treiber. Weitere Informationen finden Sie unter [Verwenden von Sqoop mit HDInsight][hdinsight-use-sqoop], [Analysieren Verzögerung Flugdaten mit HDInsight][hdinsight-analyze-flight-delay-data], [Verbinden von Excel mit HDInsight mithilfe von Power Query][hdinsight-power-query], und [Verbinden von Excel mit HDInsight mithilfe von Microsoft Struktur ODBC-Treiber][hdinsight-hive-odbc].

##<a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben wir gesehen, wie eine unstrukturierte JSON-Dataset in eine strukturierte strukturtabelle, Abfragen, untersuchen und Analysieren von Daten aus Twitter mithilfe von HDInsight auf Azure transformieren. Weitere Informationen finden Sie unter:

- [Erste Schritte mit HDInsight][hdinsight-get-started]
- [Analysieren Sie in Echtzeit Twitter-Grüße mit HBase in HDInsight][hdinsight-hbase-twitter-sentiment]
- [Analysieren Sie HDInsight mit Verzögerung Flugdaten][hdinsight-analyze-flight-delay-data]
- [Herstellen einer Verbindung mit Power Query HDInsight mit Excel][hdinsight-power-query]
- [Herstellen einer Verbindung mit dem Microsoft-Struktur ODBC-Treiber HDInsight mit Excel][hdinsight-hive-odbc]
- [Verwenden Sie Sqoop mit HDInsight][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
