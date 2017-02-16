<properties
    pageTitle="HBase Lernprogramm: Erste Schritte mit HBase in Hadoop | Microsoft Azure"
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
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>



# <a name="hbase-tutorial-get-started-using-apache-hbase-with-windows-based-hadoop-in-hdinsight"></a>HBase Lernprogramm: Erste Schritte mit Apache HBase mit Windows-basiertem Hadoop in HDInsight

[AZURE.INCLUDE [hbase-selector](../../includes/hdinsight-hbase-selector.md)]

Informationen Sie zum Erstellen HBase Cluster in HDInsight, HBase Tabellen erstellen und die Tabellen mithilfe von Apache Struktur Abfragen. Allgemeine HBase Informationen finden Sie unter [Übersicht über die HDInsight HBase][hdinsight-hbase-overview].

Die Informationen in diesem Dokument ist für Windows-basiertem HDInsight Cluster spezifisch. Verwenden Sie Informationen auf einem Windows-basierten Cluster das tabstoppauswahlsymbol am oberen Rand der Seite um zu wechseln.

> [AZURE.NOTE] HBase (Version 0.98.0) auf Windows basierende HDInsight steht nur zur Verfügung, für die Verwendung mit HDInsight 3.1 Cluster (basierend auf Apache Hadoop und aus 2.4.0). Versionsinformationen finden Sie unter [Neuigkeiten in die Hadoop Cluster Versionen von HDInsight bereitgestellten?][hdinsight-versions]

## <a name="before-you-begin"></a>Vorbemerkung

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Bevor Sie dieses Lernprogramm HBase beginnen, benötigen Sie Folgendes:

- **A Microsoft Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Eine Arbeitsstationen** mit Visual Studio 2013 oder höher: Anweisungen finden Sie unter [Visual Studio installieren](http://msdn.microsoft.com/library/e2h7fzkw.aspx).

### <a name="access-control-requirements"></a>Anforderungen für Access-Steuerelement

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>Erstellen von HBase cluster

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]

**So erstellen Sie einen HBase Cluster mithilfe des Azure-Portals**

1. Melden Sie sich bei der [Azure-Portal][azure-management-portal].
2. Klicken Sie auf **neu** oder **+** in der oberen rechten Ecke, und klicken Sie dann auf **Daten + Analytics**, **HDInsight**.
3. Geben Sie die folgenden Werte aus:

    - **Cluster Name** - Geben Sie einen Namen für diesen Cluster ein.
    - **Clustertyp** – Select **HBase**.
    - **Betriebssystem Cluster** - Select **Windows**.  Zum Erstellen von Linux-basierten HBase cluster, finden Sie unter [HBase Lernprogramm: Erste Schritte mit Apache HBase mit Hadoop in HDInsight (Linux)](hdinsight-hbase-tutorial-get-started-linux.md).
    - **Version** - wählen Sie eine Version HBase.
    - **Abonnement** - wählen Sie Ihre Azure-Abonnement für das Erstellen von diesem Cluster verwendet.
    - **Ressourcengruppe** – erstellen eine neuen Azure Ressourcengruppe oder ein vorhandenes Layout auszuwählen. Weitere Informationen finden Sie unter [Azure Ressourcenmanager (Übersicht)](azure-resource-manager/resource-group-overview.md)
    - **Anmeldeinformationen** - können für Windows-basierten Cluster, die Sie eines Benutzers Cluster (bzw. HTTP-Benutzer, HTTP-Dienst Webbenutzer) und ein Benutzer Remote Desktop erstellen. Klicken Sie auf **Remotedesktop aktivieren** , um die Anmeldeinformationen für remote desktop-Benutzer hinzufügen. Im nächste Abschnitt erfordert RDP.
    - **Datenquelle** - Erstellen eines neuen Kontos mit Azure-Speicher, oder wählen Sie ein vorhandenes Azure-Speicher-Konto als Standard-Dateisystem für den Cluster verwendet werden soll. Der Konto Standardspeicherort bestimmt die Position des Speicherorts Cluster. Das Standardkonto für den Speicher und die Cluster müssen in derselben Data Center gemeinsame suchen.
    - **Knoten Preise Ebenen** - wählen Sie die Anzahl der Server für die Cluster HBase region

        > [AZURE.WARNING] Hohen Verfügbarkeit der HBase Services, müssen Sie erstellen, einen Cluster mit mindestens **drei** Knoten. Dadurch wird sichergestellt, dass, wenn einer der Knoten fällt aus, die Regionen HBase Daten auf anderen Knoten zur Verfügung stehen.

        > Wenn Sie HBase als Unterstützung dienen, immer wählen Sie 1 für die Größe der Zuordnungseinheiten aus, und löschen Sie Cluster nach jeder Verwendung, die Kosten zu verringern.

    - **Optionale Konfiguration** - virtuelle Netzwerk Azure konfigurieren, Skript-Aktionen konfigurieren, und fügen Sie zusätzlichen Speicher-Konten.

4. Klicken Sie auf **Erstellen**.

>[AZURE.NOTE] Nach dem Löschen eines HBase Clusters können Sie einen anderen HBase Cluster mithilfe des gleichen Standardkontos Speicher und die standardmäßige Blob-Container erstellen. Der neue Cluster übernehmen HBase Tabellen, die Sie in der ursprünglichen Cluster erstellt haben. Es wird empfohlen, HBase Tabellen deaktivieren, bevor Sie den Cluster löschen, um Inkonsistenzen zu vermeiden.

## <a name="create-tables-and-insert-data"></a>Erstellen von Tabellen und Einfügen von Daten

Zurzeit stehen zwei-Wege-HBase Zugriff auf. Dieser Abschnitt enthält die Verwaltungsshell HBase verwenden. Im nächste Abschnitt behandelt .NET SDK verwenden.

Für die meisten Personen werden Daten im Tabellenformat angezeigt:

![Hdinsight Hbase Tabellendaten][img-hbase-sample-data-tabular]

In HBase, die eine Implementierung von BigTable wird, sieht die gleichen Daten wie aus:

![Hdinsight Hbase Bigtable Daten][img-hbase-sample-data-bigtable]

Es werden weitere sinnvoll sein, nachdem Sie die nächsten Schritte ausgeführt haben.  

**So verwenden Sie die Verwaltungsshell HBase**

1. Verwenden Sie für die Verbindung zum Cluster HBase in HDInsight RDP ein. Die RDP Anweisungen finden Sie unter [Verwalten von Hadoop Cluster in Verwenden des Portals Azure HDInsight][hdinsight-manage-portal].
2. Innerhalb der RDP-Sitzung klicken Sie auf die **Befehlszeile Hadoop** Verknüpfung auf dem Desktop befindet.
3. Öffnen Sie die Verwaltungsshell HBase an:

        cd %HBASE_HOME%\bin
        hbase shell

4. Erstellen eines HBase mit zwei Spalte Familien:

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

    Sehen Sie die gleichen Ergebnisse wie mithilfe des Befehls Scan, da es nur eine Zeile ist.

    Weitere Informationen über das Schema der Hbase finden Sie unter [Einführung in HBase Schema Design][hbase-schema]. Weitere HBase Befehle finden Sie unter [Referenzhandbuchs Apache HBase][hbase-quick-start].


6. Beenden der shell

        exit

**Zum Laden von Daten in der Tabelle der Kontakte HBase gruppenweise**

HBase bietet mehrere Methoden zum Laden von Daten in Tabellen. Weitere Informationen finden Sie unter [Massen zu laden](http://hbase.apache.org/book.html#arch.bulk.load).


Eine Beispiel-Datendatei zu einem Container Blob für den öffentlichen hochgeladen wurde wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt. Der Inhalt der Datendatei lautet:

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

1. Innerhalb der RDP-Sitzung klicken Sie auf die **Befehlszeile Hadoop** Verknüpfung auf dem Desktop befindet.
2. Verzeichnis zu ändern:

        cd %HBASE_HOME%\bin

3. Führen Sie den folgenden Befehl zum Umwandeln der Datendatei StoreFiles und Store am ein relativer Pfad durch Dimporttsv.bulk.output angegeben:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. Führen Sie zum Hochladen der Daten aus /example/data/storeDataFileOutput in der Tabelle HBase den folgenden Befehl ein:

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. Sie können die Verwaltungsshell HBase öffnen und mithilfe des Befehls Scan den Inhalt der Liste.



## <a name="use-hive-to-query-hbase-tables"></a>Mit der Struktur HBase Abfragetabellen

Sie können Daten aus der HBase mithilfe der Struktur Abfragen. In diesem Abschnitt erstellt eine strukturtabelle, die in der Tabelle HBase ordnet und zum Abfragen von Daten in der Tabelle HBase wird verwendet.

**So öffnen Sie das Cluster-dashboard**

1. Navigieren Sie zu **https://<HDInsight Cluster Name>.azurehdinsight.net/**.
5. Geben Sie den Hadoop Konto Benutzername und das Kennwort ein. Der Standard-Benutzername ist **Admin** und das Kennwort ist, was Sie beim Erstellen der eingegeben haben. Eine neue Browserregisterkarte wird geöffnet.
6. Klicken Sie auf **Struktur Editor** am oberen Rand der Seite. Die Struktur-Editor sieht wie folgt aus:

    ![HDInsight Cluster Dashboard.][img-hdinsight-hbase-hive-editor]

**Zum Ausführen der Struktur Abfragen**

1. Geben Sie das folgende HiveQL Skript in Editor Struktur, und klicken Sie auf **Absenden** , um eine Strukturtabelle zu erstellen, die in der Tabelle HBase zugeordnet ist. Stellen Sie sicher, dass die Beispieltabelle optimiert zuvor in diesem Lernprogramm verwenden die Verwaltungsshell HBase aus, bevor Sie diese Anweisung ausführen erstellt.

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

    Warten Sie, bis **die statusaktualisierungen **abgeschlossen**** .

2. Geben Sie das folgende HiveQL Skript in Editor Struktur, und klicken Sie dann auf **Senden**. Die Struktur Abfrage Abfragen, die Daten in der Tabelle HBase:

        SELECT count(*) FROM hbasecontacts;

4. Um die Ergebnisse der Abfrage Struktur abzurufen, klicken Sie auf den Link im Fenster **Projekt-Sitzung** **Details anzeigen** , wenn das Projekt abgeschlossen ist. Da Sie einen Eintrag in der Tabelle HBase investieren werden nur eine Position Ausgabedatei vor.




**Um die Ausgabedatei zu navigieren.**

1. Klicken Sie in der Abfrage-Konsole auf **Datei-Browser**.
2. Klicken Sie auf das Azure-Speicher-Konto, das als Standard-Dateisystem für den HBase Cluster verwendet wird.
3. Klicken Sie auf den Namen der HBase Cluster. Der standardmäßige Azure-Speicher Konto Container wird der Clustername verwendet.
4. Klicken Sie auf **Benutzer**, und klicken Sie dann auf **Administrator**. (Dies ist der Benutzername Hadoop.)
6. Klicken Sie auf den Namen der Position mit der Uhrzeit der **Letzten Änderung** , die die Uhrzeit entspricht, bei der Ausführung der Abfrage Struktur auswählen.
4. Klicken Sie auf **Stdout**. Speichern Sie die Datei, und öffnen Sie die Datei mit Editor. Es wird eine Ausgabedatei vorhanden sein.

    ![HDInsight HBase Struktur-Editor Datei-Browser][img-hdinsight-hbase-file-browser]

## <a name="use-the-net-hbase-rest-api-client-library"></a>Verwenden Sie die REST-API von .NET HBase Client-Bibliothek

Sie müssen die Bibliothek HBase REST-API Client für .NET von GitHub herunterladen und erstellen Sie das Projekt aus, damit Sie HBase .NET SDK verwenden können. Das folgende Verfahren enthält die Anweisungen für diese Aufgabe.

1. Erstellen Sie eine neue C#-Visual Studio Windows Desktop.
2. Öffnen Sie die NuGet-Paket-Manager-Konsole, indem Sie auf **Extras** > **NuGet Package Manager** > **Paket-Manager-Konsole**.
3. Führen Sie den folgenden Befehl aus NuGet in der Verwaltungskonsole:

        Install-Package Microsoft.HBase.Client

5. Fügen Sie am oberen Rand der Datei die folgenden Aussagen zur **Verwendung von** hinzu:

        using Microsoft.HBase.Client;
        using org.apache.hadoop.hbase.rest.protobuf.generated;

6. Ersetzen Sie die **Main** -Funktion durch Folgendes ein:

        static void Main(string[] args)
        {
            string clusterURL = "https://<yourHBaseClusterName>.azurehdinsight.net";
            string hadoopUsername= "<yourHadoopUsername>";
            string hadoopUserPassword = "<yourHadoopUserPassword>";

            string hbaseTableName = "sampleHbaseTable";

            // Create a new instance of an HBase client.
            ClusterCredentials creds = new ClusterCredentials(new Uri(clusterURL), hadoopUsername, hadoopUserPassword);
            HBaseClient hbaseClient = new HBaseClient(creds);

            // Retrieve the cluster version.
            var version = hbaseClient.GetVersion();
            Console.WriteLine("The HBase cluster version is " + version);

            // Create a new HBase table.
            TableSchema testTableSchema = new TableSchema();
            testTableSchema.name = hbaseTableName;
            testTableSchema.columns.Add(new ColumnSchema() { name = "d" });
            testTableSchema.columns.Add(new ColumnSchema() { name = "f" });
            hbaseClient.CreateTable(testTableSchema);

            // Insert data into the HBase table.
            string testKey = "content";
            string testValue = "the force is strong in this column";
            CellSet cellSet = new CellSet();
            CellSet.Row cellSetRow = new CellSet.Row { key = Encoding.UTF8.GetBytes(testKey) };
            cellSet.rows.Add(cellSetRow);

            Cell value = new Cell { column = Encoding.UTF8.GetBytes("d:starwars"), data = Encoding.UTF8.GetBytes(testValue) };
            cellSetRow.values.Add(value);
            hbaseClient.StoreCells(hbaseTableName, cellSet);

            // Retrieve a cell by its key.
            cellSet = hbaseClient.GetCells(hbaseTableName, testKey);
            Console.WriteLine("The data with the key '" + testKey + "' is: " + Encoding.UTF8.GetString(cellSet.rows[0].values[0].data));
            // with the previous insert, it should yield: "the force is strong in this column"

            //Scan over rows in a table. Assume the table has integer keys and you want data between keys 25 and 35.
            Scanner scanSettings = new Scanner()
            {
                batch = 10,
                startRow = BitConverter.GetBytes(25),
                endRow = BitConverter.GetBytes(35)
            };

            ScannerInformation scannerInfo = hbaseClient.CreateScanner(hbaseTableName, scanSettings);
            CellSet next = null;
            Console.WriteLine("Scan results");

            while ((next = hbaseClient.ScannerGetNext(scannerInfo)) != null)
            {
                foreach (CellSet.Row row in next.rows)
                {
                    Console.WriteLine(row.key + " : " + Encoding.UTF8.GetString(row.values[0].data));
                }
            }

            Console.WriteLine("Press ENTER to continue ...");
            Console.ReadLine();
        }

7. Setzen Sie die ersten drei Variablen in der **Main** -Funktion.
8. Drücken Sie **F5** , um die Anwendung ausführen.

## <a name="check-cluster-status"></a>Überprüfen des Status eines cluster

HBase in HDInsight im Lieferumfang von Web-Benutzeroberfläche für die Überwachung Cluster. Verwenden der Web-Benutzeroberfläche, können Sie Statistiken oder Informationen zu Regionen anfordern.

Klicken Sie zum Öffnen der Web-Benutzeroberfläche Sie müssen RDP in Cluster, und klicken Sie dann klicken Sie auf die Verknüpfung HMaster Informationen Web-Benutzeroberfläche auf dem Desktop oder verwenden Sie den folgenden URL in einem Webbrowser:

    http://zookeeper[0-2]:60010/master-status

In einem Cluster hohen Verfügbarkeit finden Sie eine Verknüpfung mit dem aktuellen aktiven HBase master Knoten aus, die der Web-Benutzeroberfläche gehostet wird.

##<a name="delete-the-cluster"></a>Löschen des Clusters
Es wird empfohlen, HBase Tabellen deaktivieren, bevor Sie den Cluster löschen, um Inkonsistenzen zu vermeiden.
[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="whats-next"></a>Wie geht's weiter?
In diesem Lernprogramm HBase für HDInsight haben Sie zum Erstellen eines HBase Clusters und zum Erstellen von Tabellen und Anzeigen von Daten in diesen Tabellen aus der Shell HBase. Sie haben auch gelernt, wie eine Struktur Abfrage verwenden, klicken Sie auf Daten in Tabellen HBase und wie die APIs C HBase REST zum Erstellen einer Tabelle HBase und Abrufen von Daten aus der Tabelle verwendet.

Weitere Informationen finden Sie unter:

- [Übersicht über die HDInsight HBase][hdinsight-hbase-overview].
HBase handelt es sich um eine Apache, Open-Source NoSQL-Datenbank basierend auf Hadoop, die Direktzugriff und sicherer Konsistenz für große Mengen von unstrukturierten und semistructured Daten enthält.
- [Erstellen HBase Cluster virtuellen Netzwerk Azure][hdinsight-hbase-provision-vnet].
Virtuelle Netzwerk-Integration können HBase Cluster, damit Applikationen direkt mit HBase kommunizieren können mit dem gleichen virtuellen Netzwerk als Ihrer Anwendung bereitgestellt werden.
- [Konfigurieren von HBase Replikation in HDInsight](hdinsight-hbase-geo-replication.md). Informationen Sie zum Konfigurieren der HBase Replikation über zwei Azure Datencenter.
- [Analysieren Sie Twitter-Grüße mit HBase in HDInsight][hbase-twitter-sentiment].
Erfahren Sie, wie in Echtzeit [Grüße Analyse](http://en.wikipedia.org/wiki/Sentiment_analysis) große Datenmengen in einem Cluster Hadoop in HDInsight mithilfe von HBase ausführen.

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
[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-bigtable.png
