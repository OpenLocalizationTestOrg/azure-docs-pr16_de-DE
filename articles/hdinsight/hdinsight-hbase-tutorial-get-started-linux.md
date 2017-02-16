<properties
    pageTitle="HBase Lernprogramm: Erste Schritte mit HBase Linux-basierten Cluster in Hadoop | Microsoft Azure"
    description="Führen Sie dieses Lernprogramm HBase den Einstieg Apache HBase mit Hadoop in HDInsight. Erstellen von Tabellen aus der Shell HBase und Abfragen können mithilfe der Struktur."
    keywords="Apache Hbase, Hbase, Hbase Shell, Hbase Lernprogramm"
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
    ms.topic="get-started-article"
    ms.date="10/19/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-linux-based-hadoop-in-hdinsight"></a>HBase Lernprogramm: Erste Schritte mit Apache HBase mit Linux-basierten Hadoop in HDInsight 

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Informationen Sie zum Erstellen eines HBase Clusters in HDInsight, HBase Tabellen erstellen und Tabellen mithilfe der Struktur Abfragen. Allgemeine HBase Informationen finden Sie unter [Übersicht über die HDInsight HBase][hdinsight-hbase-overview].

Die Informationen in diesem Dokument ist für Linux-basierte HDInsight Cluster spezifisch. Verwenden Sie Informationen auf einem Windows-basierten Cluster das tabstoppauswahlsymbol am oberen Rand der Seite um zu wechseln.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

##<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm HBase beginnen, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- [Secure Shell(SSH)](hdinsight-hadoop-linux-use-ssh-unix.md). 
- [Aufrollen](http://curl.haxx.se/download.html).

### <a name="access-control-requirements"></a>Anforderungen für Access-Steuerelement

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>Erstellen von HBase cluster

Das folgende Verfahren wird eine Ressourcenmanager Azure-Vorlage zum Erstellen einer Version 3.4 HBase Linux-basierten Cluster und abhängige Standard Speicher Azure-Konto verwendet. Die Parameter in der Prozedur und andere Methoden zur Erstellung Cluster verwendete finden Sie unter [Erstellen von Linux-basierten Hadoop Cluster in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Klicken Sie auf die folgende Abbildung, um die Vorlage im Azure-Portal zu öffnen. Die Vorlage befindet sich in einem öffentlichen Blob-Container. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Geben Sie aus dem Blade **benutzerdefinierte Bereitstellung** Folgendes ein:

    - **Abonnements**: Wählen Sie Ihre Azure-Abonnement, die zum Erstellen des Clusters verwendet wird.
    - **Ressourcengruppe**: Erstellen einer neuen Gruppe von Azure Ressourcenverwaltung oder ein vorhandenes zu verwenden.
    - **Standort**: Geben Sie den Speicherort der Ressourcengruppe. 
    - **ClusterName**: Geben Sie einen Namen für den HBase Cluster, die Sie erstellen.
    - **Cluster-Benutzernamen und Ihr Kennwort**: der Standard-Anmeldename ist **Admin**.
    - **SSH-Benutzernamen und Ihr Kennwort**: der Standard-Benutzername ist **Sshuser**.  Sie können ihn umbenennen.
     
    Andere Parameter sind optional.  
    
    Jeder Cluster verfügt über eine Azure BLOB-Speicher Konto Abhängigkeit. Nach dem Löschen von eines Clusters behält die Daten im Speicherkonto. Cluster Speicher Name des Standardkontos ist der Clustername mit "Store" angefügt. Es ist hartcodierte im Abschnitt Variablen Vorlage aus.
        
3. Wählen Sie **ich stimme zu, die allgemeinen Geschäftsbedingungen oben erwähnten**aus, und klicken Sie dann auf **kaufen**. So erstellen Sie einen Cluster dauert ungefähr 20 Minuten.


>[AZURE.NOTE] Nach dem Löschen eines HBase Clusters können Sie einen anderen HBase Cluster mithilfe des gleichen standardmäßige Blob-Containers erstellen. Der neue Cluster übernehmen HBase Tabellen, die Sie in der ursprünglichen Cluster erstellt haben. Es wird empfohlen, HBase Tabellen deaktivieren, bevor Sie den Cluster löschen, um Inkonsistenzen zu vermeiden.

## <a name="create-tables-and-insert-data"></a>Erstellen von Tabellen und Einfügen von Daten

SSH können Sie eine Verbindung mit HBase Cluster und dann HBase Shell HBase Tabellen erstellen, fügen Sie Daten und Abfragen von Daten verwenden. Informationen zur Verwendung von SSH finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix, oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md) und [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
 

Für die meisten Personen werden Daten im Tabellenformat angezeigt:

![HDInsight HBase Tabellendaten][img-hbase-sample-data-tabular]

In HBase, die eine Implementierung von BigTable wird, sieht die gleichen Daten wie aus:

![HDInsight HBase Bigtable Daten][img-hbase-sample-data-bigtable]

Es werden weitere sinnvoll, nachdem Sie die nächsten Schritte ausgeführt haben.  


**So verwenden Sie die Verwaltungsshell HBase**

1. Führen Sie von SSH den folgenden Befehl aus:

        hbase shell

4. Erstellen eines HBase mit zwei Spalten Familien:

        create 'Contacts', 'Personal', 'Office'
        list
5. Fügen Sie Daten ein:

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![Hdinsight Hadoop Hbase shell][img-hbase-shell]

6. Abrufen einer Zeile

        get 'Contacts', '1000'

    Sie sehen die gleichen Ergebnisse wie mithilfe des Befehls Scan, da es nur eine Zeile ist.

    Weitere Informationen über das Schema der HBase finden Sie unter [Einführung in HBase Schema Design][hbase-schema]. Weitere HBase Befehle finden Sie unter [Referenzhandbuchs Apache HBase][hbase-quick-start].

6. Beenden der shell

        exit



**Zum Laden von Daten in der Tabelle der Kontakte HBase gruppenweise**

HBase bietet mehrere Methoden zum Laden von Daten in Tabellen.  Weitere Informationen finden Sie unter [Massen zu laden](http://hbase.apache.org/book.html#arch.bulk.load).


Eine Beispiel-Datendatei zu einem Container Blob für den öffentlichen hochgeladen wurde *wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  Der Inhalt der Datendatei lautet:

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

Sie können eine Textdatei erstellen und Hochladen der Datei auf Ihr Speicherkonto, wenn Sie möchten. Die Anweisungen finden Sie unter [Upload von Daten für Hadoop Aufträge in HDInsight][hdinsight-upload-data].

> [AZURE.NOTE] Dieses Verfahren verwendet die Kontakte HBase Tabelle, die Sie in der letzten Prozedur erstellt haben.

1. Von SSH, führen Sie den folgenden Befehl zum Umwandeln der Datendatei StoreFiles und Store am ein relativer Pfad durch Dimporttsv.bulk.output angegeben:.  Wenn Sie in der HBase-Verwaltungsshell sind, verwenden Sie den Befehl Beenden um zu beenden.

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Führen Sie zum Hochladen der Daten aus /example/data/storeDataFileOutput in der Tabelle HBase den folgenden Befehl ein:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Sie können die Verwaltungsshell HBase öffnen und mithilfe des Befehls Scan den Inhalt der Liste.



## <a name="use-hive-to-query-hbase"></a>Mit der Abfrage HBase Struktur

Sie können Daten in Tabellen HBase mithilfe der Struktur Abfragen. In diesem Abschnitt erstellt eine strukturtabelle, die in der Tabelle HBase ordnet und zum Abfragen von Daten in der Tabelle HBase wird verwendet.

1. **Kitten**öffnen, und Verbinden mit dem Cluster.  Finden Sie die Anweisungen im vorangehenden Verfahren aus.
2. Öffnen Sie die Verwaltungsshell Struktur.

       hive
3. Führen Sie die folgende HiveQL Skript einer Strukturtabelle erstellen, die in der Tabelle HBase zugeordnet ist. Stellen Sie sicher, dass Sie die Beispieltabelle optimiert zuvor in diesem Lernprogramm verwenden die Verwaltungsshell HBase aus, bevor Sie diese Anweisung ausführen erstellt haben.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

2. Führen Sie das folgende HiveQL-Skript, um die Daten in der Tabelle HBase Abfrage:

        SELECT count(*) FROM hbasecontacts;

## <a name="use-hbase-rest-apis-using-curl"></a>Verwenden Sie HBase REST-APIs Curl verwenden

> [AZURE.NOTE] Wenn Curl oder einer beliebigen anderen REST-Kommunikation mit WebHCat verwenden zu können, müssen Sie die Anfragen können, indem Sie den Benutzernamen und das Kennwort für den HDInsight Clusteradministrator authentifizieren. Sie müssen den Clusternamen auch als Teil der der URI Uniform Resource Identifier () verwendet, um die Anforderungen an den Server gesendeten verwenden.
>
> Die Befehle in diesem Abschnitt ersetzen Sie **Benutzernamen** für den Benutzer mit dem Cluster authentifiziert, und Ersetzen Sie **PASSWORD** durch das Kennwort für das Benutzerkonto. Ersetzen Sie **CLUSTERNAME** mit dem Namen der Cluster aus.
>
> Die REST-API wird über [Standardauthentifizierung](http://en.wikipedia.org/wiki/Basic_access_authentication)gesichert. Stellen Sie Besprechungsanfragen immer, mithilfe von Secure HTTP (HTTPS), um sicherzustellen, dass Ihre Anmeldeinformationen ein, auf dem Server eine sichere Verbindung gesendet werden.

1. Verwenden Sie den folgenden Befehl von einer Befehlszeile aus um sicherzustellen, dass Sie auf Ihren Cluster HDInsight zugreifen können:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status

    Sie erhalten eine Antwort ähnlich wie der folgende aus:

        {"status":"ok","version":"v1"}

    Die Parameter in dieser Befehl verwendet werden wie folgt aus:

    * **-u** - Benutzername und Kennwort zum Authentifizieren der Anforderung verwendet.
    * **-G** - gibt an, dass dies eine GET-Anforderung ist.

2. Verwenden Sie den folgenden Befehl aus, um die vorhandenen HBase Tabellen finden Sie:

        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/hbaserest/

3. Verwenden Sie den folgenden Befehl zum Erstellen einer neuen HBase-Tabelle mit zwei Spalte Familien:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
        -v

    Das Schema wird in das JSon-Format bereitgestellt.

4. Verwenden Sie den folgenden Befehl zum Einfügen von Daten aus:

        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"Row\":{\"key\":\"MTAwMA==\",\"Cell\":{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}}}" \
        -v

    Sie müssen base64 codiert werden die Werte in der Befehlszeilenoption -d angegeben.  In der Exmaple:

    - MTAwMA ==: 1000
    - UGVyc29uYWw6TmFtZQ ==: persönlich: Name
    - Sm9obiBEb2xl: Johann Dole

    [False-Zeile-Taste](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) können Sie mehrere (gespeicherten) Wert einfügen.

5. Verwenden Sie den folgenden Befehl, um eine Zeile zu erhalten:

        curl -u <UserName>:<Password> \
        -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
        -H "Accept: application/json" \
        -v

Weitere Informationen zu den HBase Rest finden Sie unter [Apache HBase Referenzhandbuchs](https://hbase.apache.org/book.html#_rest).

## <a name="check-cluster-status"></a>Überprüfen des Status eines cluster

HBase in HDInsight im Lieferumfang von Web-Benutzeroberfläche für die Überwachung Cluster. Verwenden der Web-Benutzeroberfläche, können Sie Statistiken oder Informationen zu Regionen anfordern.

SSH kann auch auf lokale Anfragen, z. B. Webanfragen, mit dem HDInsight Cluster tunnel verwendet werden. Die Anfrage wird dann zur angeforderten Ressource weitergeleitet werden, als ob es auf dem HDInsight Cluster am Knoten stammen. Weitere Informationen finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md#tunnel).

**Herstellen einer Sitzung Tunnel SSH**

1. **Kitten**zu öffnen.  
2. Wenn Sie einen SSH Schlüssel bereitgestellt, wenn Sie Ihr Benutzerkonto beim Erstellen der erstellt haben, müssen Sie die folgende Schritte aus, um den privaten Schlüssel zu verwenden, wenn Sie mit dem Cluster authentifizieren auszuwählen ausführen:

    Klicken Sie in der **Kategorie**erweitern Sie **Verbindung** **SSH**, und wählen Sie **Authentifizierung**aus. Schließlich, klicken Sie auf **Durchsuchen** , und wählen Sie die .ppk-Datei, die Ihre privaten Schlüssel enthält.

3. Klicken Sie in der **Kategorie** **Sitzung**auf.
4. Geben Sie die folgenden Werte aus der Standardoptionen für Ihres Bildschirms PuTTY Sitzung:

    - **Host Name**: die SSH-Adresse des Felds für den HDInsight-Server in der Hostname (oder IP-Adresse). Die SSH-Adresse Ihres Clusternamens dann ist **-ssh.azurehdinsight.net**. Beispielsweise *MeinCluster-ssh.azurehdinsight.net*.
    - **Port**: 22. Die ssh Port die primäre Headnode ist 22.  
5. Im Abschnitt **Kategorie** auf der linken Seite des Dialogfelds **Verbindung**zu erweitern, Erweitern von **SSH**und klicken Sie dann auf **Tunnel**.
6. Geben Sie die folgenden Informationen zu den Optionen steuern SSH Port Weiterleitung Formular an:

    - **Quellport** : den Port auf dem Client, den Sie weiterleiten möchten. Beispielsweise 9876.
    - **Dynamic** - ermöglicht dynamische SOCKS Proxy routing.
7. Klicken Sie auf **Hinzufügen** , um die Einstellungen hinzufügen.
8. Klicken Sie auf am unteren Rand des Dialogfelds **Öffnen** , um eine SSH-Verbindung zu öffnen.
9. Wenn Sie dazu aufgefordert werden, melden Sie sich an den Server mit einem SSH-Konto an. Dies Aufbau einer SSH-Sitzung, und aktivieren Sie den Tunnel.

**Um den vollqualifizierten Domänennamen für die Verwendung von Ambari lassen zu finden.**

1. Navigieren Sie zu https://<ClusterName>.azurehdinsight.net/.
2. Geben Sie Ihre Anmeldeinformationen Cluster zweimal auf.
3. Klicken Sie im Menü links auf **Zookeeper**.
4. Klicken Sie auf einen der drei **ZooKeeper Server** Links aus der Liste Zusammenfassung.
5. **Hostname**zu kopieren. Zk0-CLUSTERNAME.xxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net.

**Zum Konfigurieren einer Clientprogramm (Firefox), und aktivieren Sie Cluster status**

1. Firefox zu öffnen.
2. Klicken Sie auf die Schaltfläche **Menü öffnen** .
3. Klicken Sie auf **Optionen**.
4. Klicken Sie auf **Erweitert**, klicken Sie auf **Netzwerk**, und klicken Sie dann auf **Einstellungen**.
5. Wählen Sie **Manueller Proxy-Konfiguration**aus.
6. Geben Sie die folgenden Werte aus:

    - **Host Socks**: Localhost
    - **Port**: Verwenden Sie den gleichen Anschluss, die Sie in der kitten SSH Tunnel konfiguriert.  Beispielsweise 9876.
    - **SOCKS v5**: (aktiviert)
    - **Remote-DNS-**: (aktiviert)
7. Klicken Sie auf **OK** , um die Änderungen zu speichern.
8. Navigieren Sie zu http://&lt;der FQDN der eine ZooKeeper >: 60010/Master-Status.

In einem Cluster hohen Verfügbarkeit finden Sie eine Verknüpfung mit dem aktuellen aktiven HBase master Knoten, die der Web-Benutzeroberfläche gehostet wird.

##<a name="delete-the-cluster"></a>Löschen des Clusters

Es wird empfohlen, HBase Tabellen deaktivieren, bevor Sie den Cluster löschen, um Inkonsistenzen zu vermeiden.

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm HBase für HDInsight haben Sie zum Erstellen eines HBase Clusters und zum Erstellen von Tabellen und Anzeigen von Daten in diesen Tabellen aus der Shell HBase. Sie haben gelernt wie Sie eine Struktur Abfrage auf Daten in Tabellen HBase verwenden und wie die APIs C HBase REST zum Erstellen einer Tabelle HBase und Abrufen von Daten aus der Tabelle verwendet.

Weitere Informationen finden Sie unter:

- [Übersicht über die HDInsight HBase][hdinsight-hbase-overview]: HBase ist einer Apache, Open-Source NoSQL-Datenbank basierend auf Hadoop, die Direktzugriff und sicherer Konsistenz für große Mengen von unstrukturierten und semistructured Daten enthält.


[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
