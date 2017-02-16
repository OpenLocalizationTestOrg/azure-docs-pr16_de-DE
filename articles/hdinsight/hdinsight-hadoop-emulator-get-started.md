<properties
    pageTitle="Verwenden ein Sandkastens Hadoop Hadoop lernen | Microsoft Azure"
    description="Um Informationen zum Verwenden der Hadoop-Netz learning zu beginnen, können Sie eines Sandkastens Hadoop von Hortonworks auf einer Azure-virtuellen Computern einrichten. "
    keywords="Hadoop-Emulator Hadoop Sandkastenmodus"
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    authors="nitinme"
    documentationCenter=""
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="nitinme"/>

# <a name="get-started-in-the-hadoop-ecosystem-with-a-hadoop-sandbox-on-a-virtual-machine"></a>Erste Schritte in der Hadoop-Netz mit eines Sandkastens Hadoop auf einem virtuellen Computer

Informationen Sie zum Hadoop Sandkasten von Hortonworks auf einem virtuellen Computer die Hadoop-Netz lernen zu installieren. Der Sandkastenmodus bietet eine lokalen Umgebung Hadoop Hadoop Distributed Datei System (HDFS) und Senden des Auftrags lernen.

## <a name="prerequisites"></a>Erforderliche Komponenten

* [Oracle-VirtualBox](https://www.virtualbox.org/)

Nachdem Sie Hadoop vertraut sind, können Sie beginnen, Hadoop auf Azure durch Erstellen eines HDInsight Clusters verwenden. Weitere Informationen zur Seite Erste Schritte finden Sie unter [Erste Schritte mit Hadoop auf HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

## <a name="download-and-install-the-virtual-machine"></a>Herunterladen und Installieren des virtuellen Computers

1. Wählen Sie das Element __Herunterladen für VIRTUALBOX__ [http://hortonworks.com/downloads/#sandbox](http://hortonworks.com/downloads/#sandbox)für HDP 2.4 auf Hortonworks Sandkasten-. Sie werden aufgefordert, mit Hortonworks registrieren, bevor der Download beginnt.

    ![Link zum Download Hortonworks Sandkastens für VirtualBox Bild](./media/hdinsight-hadoop-emulator-get-started/download-sandbox.png)

2. Wählen Sie aus der gleichen Webseite des __VirtualBox installieren Leitfaden__ für HDP 2.4 auf Hortonworks Sandkasten-ein. Dadurch wird eine PDF-Datei mit der Installation von Anweisungen für den virtuellen Computer herunterladen.

    ![Anzeigen des Leitfadens installieren](./media/hdinsight-hadoop-emulator-get-started/view-install-guide.png)

## <a name="start-the-virtual-machine"></a>Starten des virtuellen Computers

1. Starten Sie VirtualBox Hortonworks Sandkasten, wählen Sie __Starten__, und klicken Sie dann __Normal zu starten__.

    ![Normaler start](./media/hdinsight-hadoop-emulator-get-started/normal-start.png)

2. Sobald die virtuellen Computern Boot-Vorgang abgeschlossen ist, wird es Login Anweisungen angezeigt. Öffnen Sie einen Webbrowser, und navigieren Sie zu der URL angezeigt (in der Regel Http://127.0.0.1:8888).

## <a name="set-passwords"></a>Festlegen von Kennwörtern

1. Wählen Sie im Schritt __Erste Schritte__ Seitenrand Hortonworks Sandkasten- __Ansicht erweiterte Optionen__aus. Verwenden Sie die Informationen auf dieser Seite zum Anmelden bei der Sandkastenmodus SSH verwenden. Verwenden Sie den Namen und das Kennwort ein.

    > [AZURE.NOTE] Wenn Sie kein SSH-Clients installiert haben, können Sie die webbasierten SSH am von der virtuellen Computers zu bereitgestellten __Http://localhost:4200 /__.

    Sie eine Verbindung mit SSH, zum ersten Mal werden Sie aufgefordert, das Kennwort für das Konto ändern. Geben Sie ein neues Kennwort ein, die beim Anmelden in der Zukunft über SSH verwendet wird.

2. Sobald Sie angemeldet sind, geben Sie den folgenden Befehl aus:

        ambari-admin-password-reset
    
    Wenn Sie dazu aufgefordert werden, geben Sie ein Kennwort für das Ambari Admin-Konto an. Dies wird beim Zugriff auf die Web-Benutzeroberfläche Ambari verwendet.

## <a name="use-the-hive-command"></a>Verwenden Sie den Befehl Struktur

1. Verwenden Sie den folgenden Befehl aus eine Verbindung SSH Sandkasten um die Struktur Shell zu starten:

        hive

2. Nachdem Sie die Verwaltungsshell gestartet hat, verwenden Sie folgende Tabellen anzeigen möchten, die mit der Sandkastenmodus bereitgestellt werden:

        show tables;

3. Verwenden Sie die folgenden zum Abrufen von 10 Zeilen aus der `sample_07` Tabelle:

        select * from sample_07 limit 10;

## <a name="next-steps"></a>Nächste Schritte

* [Informationen Sie zum Verwenden von Visual Studio mit der Sandkastenmodus Hortonworks](hdinsight-hadoop-emulator-visual-studio.md)
* [Lernen die Seile der Sandkasten Hortonworks](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [Hadoop-Lernprogramm - Erste Schritte mit HDP](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)