<properties
    pageTitle="Analysieren von Twitter-Daten mit Apache Struktur auf HDInsight | Microsoft Azure"
    description="Erfahren Sie, wie Python Tweets gespeichert, die bestimmte Schlüsselwörter enthalten, und verwenden Sie dann Struktur und Hadoop auf HDInsight zum Umwandeln der unformatierten TWitter-Daten in eine durchsuchbare strukturtabelle verwenden."
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
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="larryfr"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Analysieren von Twitter-Daten, die mithilfe der Struktur in HDInsight

In diesem Dokument erhalten Sie Tweets mithilfe einer Twitter streaming-API, und dann verwenden Apache Struktur auf einem Linux-basierten HDInsight Cluster an den JSON-Prozess Daten formatiert. Das Ergebnis wird eine Liste der Twitter-Benutzer sein, die die meisten Tweets gesendet hat, die ein bestimmtes Wort enthalten.

> [AZURE.NOTE] Während die einzelnen Bestandteile dieses Dokuments verwendet werden können mit Windows-basierten HDInsight Cluster (beispielsweise Python), basieren auf einem HDInsight Linux-basierten Cluster mit vielen Schritten. Bestimmte Schritte zu einem Windows-basierten Cluster finden Sie unter [Analysieren Twitter-Daten mit Struktur in HDInsight](hdinsight-analyze-twitter-data.md).

###<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- Ein __Linux-basierten Azure HDInsight Cluster__. Informationen zum Erstellen von eines Clusters finden Sie unter [Erste Schritte mit Linux-basierten HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) schrittweise Anleitungen zum Erstellen eines Clusters.

- Eine __SSH-Client__. Weitere Informationen zum Verwenden von SSH mit Linux-basierten HDInsight finden Sie unter den folgenden Artikeln:

    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

- __Python__ und [pip](https://pypi.python.org/pypi/pip)

##<a name="get-a-twitter-feed"></a>Abrufen einer Twitter-Feeds

Twitter können Sie die [Daten für jedes Tweet](https://dev.twitter.com/docs/platform-objects/tweets) als JavaScript Object Notation (JSON) Dokument über eine REST-API abrufen. [OAuth](http://oauth.net) ist für die Authentifizierung an die API erforderlich. Sie müssen auch eine _Twitter-Anwendung_ erstellen, die Einstellungen verwendet, um die-API zugreifen enthält.

###<a name="create-a-twitter-application"></a>Erstellen Sie eine Twitter-Anwendung

1. Melden Sie sich mit einem Webbrowser zu [https://apps.twitter.com/](https://apps.twitter.com/)an. Wenn Sie einen Twitter-Konto besitzen, klicken Sie auf den Link **Jetzt registrieren** .
2. Klicken Sie auf **neue App erstellen**.
3. Geben Sie **Name**, **Beschreibung**, **Website**aus. Sie können eine URL für das Feld **Website** zusammensetzt. Die folgende Tabelle zeigt einige Beispielwerte zu verwenden:

  	| Feld | Wert |
  	|:----- |:----- |
  	| Namen  | MyHDInsightApp |
  	| Beschreibung | MyHDInsightApp |
  	| Website | http://www.myhdinsightapp.com |
    
4. Überprüfen Sie **Ja, ich stimme zu**, und klicken Sie dann auf **Erstellen Ihrer Twitter-Anwendung**.
5. Klicken Sie auf der Registerkarte **Berechtigungen** . Die Standardberechtigung ist **schreibgeschützt**. Dies ist ausreichend für dieses Lernprogramm.
6. Klicken Sie auf der Registerkarte **Tasten und Access Token** .
7. Klicken Sie auf **Erstellen Meine Access Token**.
8. Klicken Sie in der oberen rechten Ecke der Seite auf **OAuth testen** .
9. Notieren Sie sich **Consumer Schlüssel**, **Consumer geheim**, **Access Token**und **Access token geheim**. Sie werden die Werte später benötigen.

>[AZURE.NOTE] Wenn Sie den Befehl Curl in Windows verwenden, verwenden Sie doppelte Anführungszeichen statt einzelnen Angebote für die Optionswerte.

###<a name="download-tweets"></a>Tweets herunterladen

Der folgende Python Code wird 10.000 Tweets aus Twitter herunterladen und diese in einer Datei namens __tweets.txt__speichern.

> [AZURE.NOTE] Da Python bereits installiert ist, werden die folgenden Schritte auf dem Cluster HDInsight ausgeführt.

1. Verbinden Sie mit dem HDInsight Cluster SSH verwenden:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Wenn Sie ein Kennwort zum Sichern Ihrer SSH Benutzerkontos verwendet haben, werden Sie aufgefordert, es einzugeben. Wenn Sie einen öffentlichen Schlüssel verwendet haben, müssen Sie möglicherweise verwenden Sie die `-i` Parameter, um den passenden privaten Schlüssel anzugeben. Beispielsweise `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
        
    Weitere Informationen zum Verwenden von SSH mit Linux-basierten HDInsight finden Sie unter den folgenden Artikeln:
    
    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows)
    
2. Standardmäßig ist das __Pip__ -Programm auf dem HDInsight am Knoten nicht installiert. Verwenden Sie die folgenden um zu installieren, und aktualisieren Sie dieses Programm:

        sudo apt-get install python-pip
        sudo pip install --upgrade pip

3. Der Code Tweets herunterladen basiert auf [Tweepy](http://www.tweepy.org/) und [Statusanzeige](https://pypi.python.org/pypi/progressbar/2.2). Um diese zu installieren, verwenden Sie den folgenden Befehl aus:

        sudo apt-get install python-dev libffi-dev libssl-dev
        sudo apt-get remove python-openssl
        sudo pip install tweepy progressbar pyOpenSSL requests[security]
        
    > [AZURE.NOTE] Die Bits zum Entfernen von Python-Openssl Installieren von Python-Entwickler, Libffi-Entwickler, Libssl-Entwickler, PyOpenSSL und Besprechungsanfragen [Sicherheit] besteht darin, eine Warnung InsecurePlatform beim Herstellen einer Verbindung mit Twitter über SSL aus Python zu vermeiden.
    >
    > Tweepy v3.2.0 wird verwendet, um [ein Fehler](https://github.com/tweepy/tweepy/issues/576) zu vermeiden, die bei der Verarbeitung von Tweets auftreten können.

4. Verwenden Sie den folgenden Befehl, um eine neue Datei namens __gettweets.py__zu erstellen:

        nano gettweets.py

5. Verwenden Sie die folgenden als den Inhalt der Datei __gettweets.py__ . Ersetzen Sie den Platzhalterinformationen für __Consumer\_geheim__, __Consumer\_Schlüssel__, __Access /\_token__, und __Access\_token\_geheim__ mit den Informationen aus der Twitter-Anwendung.

        #!/usr/bin/python

        from tweepy import Stream, OAuthHandler
        from tweepy.streaming import StreamListener
        from progressbar import ProgressBar, Percentage, Bar
        import json
        import sys

        #Twitter app information
        consumer_secret='Your consumer secret'
        consumer_key='Your consumer key'
        access_token='Your access token'
        access_token_secret='Your access token secret'

        #The number of tweets we want to get
        max_tweets=10000

        #Create the listener class that will receive and save tweets
        class listener(StreamListener):
            #On init, set the counter to zero and create a progress bar
            def __init__(self, api=None):
                self.num_tweets = 0
                self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

            #When data is received, do this
            def on_data(self, data):
                #Append the tweet to the 'tweets.txt' file
                with open('tweets.txt', 'a') as tweet_file:
                    tweet_file.write(data)
                    #Increment the number of tweets
                    self.num_tweets += 1
                    #Check to see if we have hit max_tweets and exit if so
                    if self.num_tweets >= max_tweets:
                        self.pbar.finish()
                        sys.exit(0)
                    else:
                        #increment the progress bar
                        self.pbar.update(self.num_tweets)
                return True

            #Handle any errors that may occur
            def on_error(self, status):
                print status

        #Get the OAuth token
        auth = OAuthHandler(consumer_key, consumer_secret)
        auth.set_access_token(access_token, access_token_secret)
        #Use the listener class for stream processing
        twitterStream = Stream(auth, listener())
        #Filter for these topics
        twitterStream.filter(track=["azure","cloud","hdinsight"])

6. Verwenden Sie __STRG + X__und __Y__ zum Speichern der Datei ein.

7. Verwenden Sie den folgenden Befehl aus, führen Sie die Datei, und Laden Sie Tweets:

        python gettweets.py

    Eine Statusanzeige soll angezeigt werden, und bis zu 100 % zu zählen, wie die Tweets heruntergeladen und in der Datei gespeichert werden.

    > [AZURE.NOTE] Wenn sie für die Statusanzeige zur nächsten Folie sehr viel Zeit in Anspruch nehmen, sollten Sie der Filters zum Nachverfolgen von beliebte Themen ändern; Wenn es zahlreiche Tweets zum Thema, dem Sie filtern möchten gibt, können Sie die 10000 Tweets erforderlich sehr schnell ermitteln.

###<a name="upload-the-data"></a>Hochladen der Daten

Zum Hochladen der Daten auf WASB (distributed File System untersuchten HDInsight) verwenden Sie die folgenden Befehle:

    hdfs dfs -mkdir -p /tutorials/twitter/data
    hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt

Hiermit werden die Daten in einem Speicherort, den alle Knoten im Cluster zugreifen können.

##<a name="run-the-hiveql-job"></a>Führen Sie den Auftrag HiveQL


1. Verwenden Sie den folgenden Befehl, um eine Datei mit HiveQL Anweisungen zu erstellen:

        nano twitter.hql
    
    Verwenden Sie die folgenden als den Inhalt der Datei ein:

        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        -- Drop table, if it exists
        DROP TABLE tweets_raw;
        -- Create it, pointing toward the tweets logged from Twitter
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
        -- Drop and recreate the destination table
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
        -- Select tweets from the imported data, parse the JSON,
        -- and insert into the tweets table
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
        
        
3. Drücken Sie __STRG + X__, und drücken Sie dann __Y__ , um die Datei zu speichern.

4. Verwenden Sie den folgenden Befehl aus, um die in der Datei enthaltenen HiveQL auszuführen:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i twitter.hql
        
    Dies wird die Verwaltungsshell Struktur laden, führen Sie die HiveQL in der Datei __twitter.hql__ und schließlich zurückzugeben eines `jdbc:hive2//localhost:10001/>` auffordern.
    
5. Verwenden Sie folgende von Beeline dazu aufgefordert werden um sicherzustellen, dass Sie Daten aus der __Tweets__ Tabelle erstellt, die HiveQL in der Datei __twitter.hql__ auswählen können:
        
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;

    Dadurch wird maximal 10 Tweets zurückgegeben, die das Wort __Azure__ im Nachrichtentext enthalten.

##<a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben wir gesehen, wie eine unstrukturierte JSON-Dataset in eine strukturierte strukturtabelle, Abfragen, untersuchen und Analysieren von Daten aus Twitter mithilfe von HDInsight auf Azure transformieren. Weitere Informationen finden Sie unter:

- [Erste Schritte mit HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Analysieren Sie HDInsight mit Verzögerung Flugdaten](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
