<properties
    pageTitle="Verwenden Sie Hadoop Sqoop in HDInsight Linux-basierten | Microsoft Azure"
    description="Informationen Sie zum Ausführen von Sqoop importieren und Exportieren von zwischen einer Linux-basierten Hadoop auf HDInsight Cluster und einer SQL Azure-Datenbank."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="larryfr"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight-ssh"></a>Verwenden von Sqoop mit Hadoop in HDInsight (SSH)

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Informationen Sie zum Verwenden von Sqoop zum Importieren und Exportieren von zwischen einer HDInsight Linux-basierten Cluster und Azure SQL-Datenbank oder SQL Server-Datenbank.

> [AZURE.NOTE] Die Schritte in diesem Artikel verwenden SSH für die Verbindung zu einem Cluster Linux-basierten HDInsight. Windows-Clients können Azure PowerShell und HDInsight .NET SDK Sie auch auf Linux-basierten Cluster Sqoop konzipiert. Verwenden Sie das tabstoppauswahlsymbol, um diesen Artikel zu öffnen.

##<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Hadoop A Cluster in HDInsight** und einer __Azure SQL-Datenbank__: die Schritte in diesem Dokument basieren auf den Cluster und die Datenbank, die mit dem Dokument [Cluster erstellen und SQL-Datenbank](hdinsight-use-sqoop.md#create-cluster-and-sql-database) erstellt. Wenn Sie bereits eine HDInsight Cluster und SQL-Datenbank haben, können Sie die für die in diesem Dokument verwendeten Werte ersetzen.
- **Arbeitsstationen**: Computer mit einem SSH-Client.

##<a name="install-freetds"></a>Installieren von FreeTDS

1. Verwenden Sie SSH Verbindung zum Linux-basierten HDInsight Cluster ein. Ist die Adresse beim Herstellen einer Verbindung zu verwendende `CLUSTERNAME-ssh.azurehdinsight.net` und der Port ist `22`.

    Weitere Informationen zur Verwendung von SSH Verbindung zu HDInsight finden Sie unter den folgenden Dokumenten:

    * **Linux, Unix oder OS X-Clients**: finden Sie unter [Verbinden mit einem HDInsight Linux-basierten Cluster von Linux, OS X oder Unix](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)

    * **Windows-Clients**: finden Sie unter [Verbinden zu einem HDInsight Linux-basierten Cluster von Windows](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)

3. Verwenden Sie den folgenden Befehl aus, um FreeTDS zu installieren:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

    FreeTDS wird in mehreren Schritten verwendet werden, die Verbindung zu einer SQL-Datenbank herstellen.

##<a name="create-the-table-in-sql-database"></a>Erstellen Sie die Tabelle in der SQL-Datenbank

> [AZURE.IMPORTANT] Wenn Sie eine HDInsight Cluster und SQL-Datenbank erstellt mit den Schritten in [Cluster erstellen und SQL-Datenbank](hdinsight-use-sqoop.md)verwenden, ignorieren Sie die Schritte in diesem Abschnitt, wie die Datenbank und Tabelle als Teil der Schritte in diesem Dokument erstellt wurden.

1. Verwenden Sie den folgenden Befehl aus HDInsight, die Verbindung SSH Verbindung zu den SQL-Datenbankserver und erstellen die Tabelle, die die restlichen Schritte verwendet werden:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Sie erhalten die Ausgabe ähnlich wie der folgende aus:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. Bei der `1>` dazu aufgefordert werden, geben Sie die folgenden Zeilen ein:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
        GO

    Wenn die `GO` Anweisung eingegeben wird, wird die vorherigen Anweisungen ausgewertet werden. Zunächst wird die **Mobiledata** -Tabelle erstellt und dann ein gruppierter Index wird hinzugefügt (von SQL-Datenbank erforderlich.)

    Anhand der folgenden Stellen Sie sicher, dass die Tabelle erstellt wurde:

        SELECT * FROM information_schema.tables
        GO

    Ähnlich wie der folgende Ausgabe sollte angezeigt werden:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

8. Geben Sie `exit` bei der `1>` auffordern, um Tsql zu beenden.

##<a name="sqoop-export"></a>Sqoop exportieren

3. Von der SSH-Verbindung zu HDInsight, Se den folgenden Befehl aus, um sicherzustellen, dass Sqoop sind die SQL-Datenbank sichtbar:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    Dadurch sollte eine Liste der Datenbanken, einschließlich der **Sqooptest** -Datenbank, die Sie zuvor erstellt haben, zurückgegeben.

4. Verwenden Sie den folgenden Befehl zum Exportieren von Daten aus **Hivesampletable** der Tabelle **Mobiledata** aus:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --export-dir 'wasbs:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1

    Dies weist Sqoop zum Verbinden mit SQL-Datenbank, in der Datenbank **Sqooptest** und Exportieren von Daten aus der **Wasbs: / / / Struktur/Warehouse/Hivesampletable** (physischen Dateien für die *Hivesampletable*,) in der Tabelle **Mobiledata** .

5. Nach Abschluss des Befehls Verbindung mit der Datenbank mithilfe der TSQL anhand der folgenden:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Nachdem die Verbindung hergestellt wurde, mit der folgenden Aussagen stellen Sie sicher, dass die Daten in der Tabelle **Mobiledata** exportiert wurde:

        SELECT * FROM mobiledata
        GO

    Es sollte eine Auflistung von Daten in der Tabelle angezeigt. Typ `exit` um Tsql zu beenden.

##<a name="sqoop-import"></a>Sqoop importieren

1. Verwenden Sie die folgenden zum Importieren von Daten aus der Tabelle **Mobiledata** in SQL-Datenbank, zu der **Wasbs: / / / Lernprogramme/Usesqoop/Importeddata** Verzeichnis auf HDInsight:

        sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

    Die importierten Daten, Felder, die durch ein Tabstoppzeichen getrennt werden müssen, und die Zeilen werden durch Zeichen für eine neue Zeile abgeschlossen sein.

2. Nach Abschluss des Importvorgangs hat, verwenden Sie den folgenden Befehl zur Liste, die Daten in das neue Verzeichnis aus:

        hadoop fs -text wasbs:///tutorials/usesqoop/importeddata/part-m-00000

##<a name="using-sql-server"></a>Verwenden von SQLServer

Sie können auch Sqoop verwenden, importieren und Exportieren von Daten aus SQL Server, in der Mitte der Daten oder auf einem virtuellen Computer in Azure gehostet wird. Die Unterschiede zwischen der Verwendung von SQL-Datenbank und SQL Server sind:

* HDInsight sowohl in der SQL Server muss im selben Azure-virtuellen Netzwerk

    > [AZURE.NOTE] HDInsight unterstützt nur standortbasierte virtuelle Netzwerke, und es funktioniert derzeit nicht mit Zugehörigkeit Gruppe basierenden virtuellen Netzwerken.

    Wenn Sie in Ihrem Datencenter SQL Server verwenden, müssen Sie das virtuelle Netzwerk als *zwischen Standorten* oder *Punkt-zu-Standort*konfigurieren.

    > [AZURE.NOTE] Für **Punkt-zu-Standort** virtuelle Netzwerke muss SQL Server den VPN-Client-konfigurationsanwendung aus, ausgeführt werden die aus dem **Dashboard** der Azure virtuelle Netzwerkkonfiguration zur Verfügung steht.

    Weitere Informationen zu Azure-virtuellen Netzwerk finden Sie unter [Virtuelle Network (Übersicht)](../virtual-network/virtual-networks-overview.md).

* SQL Server muss konfiguriert sein, um die SQL-Authentifizierung zulässig. Weitere Informationen finden Sie unter [Auswählen eines Authentifizierungsmodus](https://msdn.microsoft.com/ms144284.aspx)

* Möglicherweise müssen Sie SQL Server zum Annehmen der remote-Verbindungen zu konfigurieren. Weitere Informationen [zum Herstellen einer Verbindung mit der SQL Server-Datenbank-Engine Problembehandlung](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) finden

* Sie müssen die **Sqooptest** -Datenbank in SQL Server Verwendung eines Programms, wie etwa **SQL Server Management Studio** oder **Tsql** erstellen – die Schritte zur Verwendung der CLI Azure funktionieren nur für SQL Azure-Datenbank

    Die TSQL Anweisungen zum Erstellen einer Tabelle **Mobiledata** sind ähnlich wie die für die SQL-Datenbank, mit Ausnahme von Erstellen einer Clusterd Index – Dies ist nicht für SQL Server erforderlich:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])

* Beim Herstellen einer Verbindung mit SQL Server aus HDInsight, müssen Sie möglicherweise die IP-Adresse der SQL Server verwenden, es sei denn, Sie eines DNS Domain Name System () zum Lösen von Namen in das virtuelle Azure-Netzwerk konfiguriert haben. Beispiel:

        sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

##<a name="limitations"></a>Einschränkungen

* Massen Exportieren - mit Linux-basierten HDInsight, der Sqoop Verbinder verwendet, um Daten in Microsoft SQL Server oder SQL Azure-Datenbank exportieren unterstützt derzeit nicht Massen eingefügt.

* Batchverarbeitung von-mit Linux-basierten HDInsight bei Verwendung der `-batch` wechseln, wenn die Durchführung fügt, Sqoop führt mehrere fügt anstelle der Batchvorgängen einfügen.

##<a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie gelernt Sqoop verwenden. Weitere Informationen finden Sie unter:

- [Verwenden von Oozie mit HDInsight][hdinsight-use-oozie]: Verwenden Sie Sqoop Aktion in einem Workflow Oozie.
- [Analysieren Flugdaten Verzögerung mit HDInsight][hdinsight-analyze-flight-data]: verzögern Daten analysieren Flight Struktur verwenden, und verwenden Sie Sqoop, Daten in einer SQL Azure-Datenbank exportieren.
- [Hochladen von Daten mit HDInsight][hdinsight-upload-data]: Suchen nach anderen Methoden zum Hochladen von Daten in HDInsight/Azure Blob-Speicher.



[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
