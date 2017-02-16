<properties
    pageTitle="Installieren von RStudio mit R Server auf HDInsight (Preview) | Microsoft Azure"
    description="So installieren Sie RStudio mit R Server auf HDInsight (Preview)."
    services="hdinsight"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/16/2016"
   ms.author="jeffstok"/>


# <a name="installing-rstudio-with-r-server-on-hdinsight-preview"></a>Installieren von RStudio mit R Server auf HDInsight (Preview)

Es stehen mehrere integrierte Entwicklung Umgebungen (IDE) für R heute, einschließlich der Microsoft zuletzt [R Tools für Visual Studio](https://www.visualstudio.com/en-us/features/rtvs-vs.aspx) (RTVS), eine Reihe von Desktop- und Server-Tools von [RStudio](https://www.rstudio.com/products/rstudio-server/)und Walwares "Ellipse"-basierten [StatET](http://www.walware.de/goto/statet)angekündigt. Zwischen die beliebtesten Linux, ist die Verwendung der [RStudio Server](https://www.rstudio.com/products/rstudio-server/) , die eine browserbasierte für die Verwendung durch remote-Clients die enthält.  Installieren von RStudio Server auf den Rand Knoten von einem Cluster HDInsight Premium bietet eine vollständige IDE Erfahrung mit R Server für die Entwicklung und Ausführung von R Skripts auf dem Cluster, und erheblich produktiver als Standard die Verwendung der Konsole R werden kann.

In diesem Artikel erfahren Sie, wie die Community (kostenlos) Version RStudio Server auf den Rand Knoten von einem Cluster zu installieren, mit einem benutzerdefinierten Skript. Falls professionell lizenzierte Programmversion von RStudio Server gewünscht, müssen Sie die Anweisungen für die Installation von [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/)befolgen.

> [AZURE.NOTE] Die Schritte in diesem Dokument erfordern einen R-Server oder HDInsight Cluster und funktionieren nicht ordnungsgemäß, wenn Sie einen HDInsight Cluster verwenden, auf dem R installiert wurde, mit der [Aktion für R Skript installieren](hdinsight-hadoop-r-scripts-linux.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

* Ein Azure HDInsight Cluster mit R Server installiert ist. Anweisungen finden Sie unter [Erste Schritte mit R Server auf HDInsight Cluster](hdinsight-hadoop-r-server-get-started.md).
* SSH-Client. Für die Verteilung von Linux und Unix oder Macintosh OS X die `ssh` Befehl wird mit dem Betriebssystem bereitgestellt. Für Windows empfehlen wir [Cygwin](http://www.redhat.com/services/custom/cygwin/) mit die [Option OpenSSH](https://www.youtube.com/watch?v=CwYSvvGaiWU)oder [kitten](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).  


## <a name="install-rstudio-on-the-cluster-using-a-custom-script"></a>Installieren von RStudio auf den Cluster über ein benutzerdefiniertes Skript

1. Identifizieren des Knotens Kante des Cluster. Folgende ist für ein HDInsight Cluster mit R Server die Benennungskonvention für am Knoten und Kante.

    * Kopf Knoten-`CLUSTERNAME-ssh.azurehdinsight.net`
    * Rand-Knoten-`R-Server.CLUSTERNAME-ssh.azurehdinsight.net` 

2. SSH in den Rand Knoten im Cluster mit dem oben genannten naming Muster. 
 
    * Wenn Sie von einem Linux-Client verbunden sind, finden Sie unter [Verbinden mit einem Cluster Linux-basierten HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster).
    * Wenn Sie sich von einem Windows-Client verbinden, finden Sie unter [Verbinden mit einem HDInsight Linux-basierten Cluster kitten verwenden](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster).

3. Nachdem Sie verbunden sind, machen Sie ein Root-Benutzer auf dem Cluster. Verwenden Sie in der Sitzung SSH den folgenden Befehl ein.

        sudo su -

4. Laden Sie das benutzerdefinierte Skript um RStudio zu installieren. Verwenden Sie den folgenden Befehl ein.

        wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/InstallRStudio.sh

5. Ändern der Berechtigungen für die benutzerdefinierte Skriptdatei, und führen Sie das Skript. Verwenden Sie die folgenden Befehle.

        chmod 755 InstallRStudio.sh
        ./InstallRStudio.sh

6. Wenn Sie ein Kennwort SSH beim Erstellen eines HDInsight Clusters mit R Server verwendet, können Sie diesen Schritt überspringen und fahren Sie mit den nächsten. Wenn Sie einen Schlüssel SSH stattdessen verwendet, um den Cluster zu erstellen, müssen Sie ein Kennwort für Ihre Benutzer SSH festlegen. Sie benötigen dieses Kennwort beim Verbinden mit RStudio. Führen Sie die folgenden Befehle. Wenn für den **aktuellen Kerberos-Kennworts**aufgefordert werden, drücken Sie die **EINGABETASTE**.  Notiz, die Sie ersetzen möchten, müssen `USERNAME` mit einem Benutzer SSH für Ihren Cluster HDInsight.

        passwd USERNAME
        Current Kerberos password:
        New password:
        Retype new password:
        Current Kerberos password:
        
    Wenn Ihr Kennwort erfolgreich festgelegt ist, sollte eine Meldung wie diese angezeigt werden.

        passwd: password updated successfully


    Beenden Sie die Sitzung SSH.

7. Erstellen Sie einen Tunnel SSH zum Cluster durch Zuordnung von `localhost:8787` im HDInsight Cluster auf dem Clientcomputer. Sie müssen einen Tunnel SSH vor dem Öffnen eines neuen Browsersitzung erstellen.

    * Öffnen Sie auf einem Linux-Client, oder klicken Sie dann einen Windows-Client mit [Cygwin](http://www.redhat.com/services/custom/cygwin/) Terminaldienste und verwenden Sie den folgenden Befehl aus.

            ssh -L localhost:8787:localhost:8787 USERNAME@R-Server.CLUSTERNAME-ssh.azurehdinsight.net
            
        Ersetzen Sie **USERNAME** mit einem Benutzer SSH für Ihren Cluster HDInsight, und Ersetzen Sie **CLUSTERNAME** mit dem Namen der Cluster HDInsight können Sie auch einen Schlüssel SSH anstelle eines Kennworts durch Hinzufügen`-i id_rsa_key`     

    * Wenn ein Windows-Client kitten dann mit

        1.  Öffnen Sie kitten, und geben Sie die Verbindungsinformationen. Wenn Sie nicht mit kitten vertraut sind, finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md) Informationen zur gemeinsamen Nutzung, mit HDInsight.
        2.  Im Abschnitt **Kategorie** auf der linken Seite des Dialogfelds erweitern Sie **Verbindung** **SSH**, und wählen Sie dann auf **Tunnel**.
        3.  Geben Sie die folgenden Informationen im Formular **Optionen steuern SSH Port zum Weiterleiten** an:

            * **Quellport** : den Port auf dem Client, den Sie weiterleiten möchten. Beispielsweise **8787**.
            * **Ziel** - das Ziel, die dem lokalen Clientcomputer zugeordnet werden müssen. Beispielsweise **Localhost:8787**.

            ![Erstellen einer SSH tunnel] (./media/hdinsight-hadoop-r-server-install-r-studio/createsshtunnel.png "Erstellen einer SSH tunnel")

        4. Klicken Sie auf **Hinzufügen** , um die Einstellungen hinzufügen, und klicken Sie dann auf **Öffnen** , um eine SSH-Verbindung zu öffnen.
        5. Wenn Sie dazu aufgefordert werden, melden Sie sich auf dem Server. Dies Aufbau einer SSH-Sitzung, und aktivieren Sie den Tunnel.

8. Öffnen Sie einen Webbrowser, und geben Sie die folgende URL basierend auf den Port für den Tunnel eingegeben.

        http://localhost:8787/ 

9. Sie werden aufgefordert, den SSH-Benutzernamen und das Kennwort für die Verbindung mit dem Cluster eingeben. Wenn Sie einen Schlüssel SSH beim Erstellen des Clusters verwendet haben, müssen Sie das Kennwort eingeben, die, das Sie in Schritt 5 erstellt haben.

    ![Verbinden mit R Studio] (./media/hdinsight-hadoop-r-server-install-r-studio/connecttostudio.png "Erstellen einer SSH tunnel")

10. Um zu testen, ob die Installation RStudio erfolgreich war, Sie können einen Testanruf ausführen Skript, R ausgeführt wird, anhand der Cluster MapReduce und Spark Aufträge. Kehren Sie zu der SSH-Verwaltungskonsole, und geben Sie die folgenden Befehle zum Herunterladen des Testskripts auf RStudio ausgeführt werden.

    * Wenn Sie einen Cluster Hadoop mit R erstellt haben, verwenden Sie diesen Befehl.
        
            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi.r

    * Wenn Sie einen Cluster Spark mit R erstellt haben, verwenden Sie diesen Befehl.

            wget http://mrsactionscripts.blob.core.windows.net/rstudio-server-community-v01/testhdi_spark.r

11. In RStudio wird das Testskript angezeigt, die, das Sie heruntergeladen haben. Klicken Sie mit der Doppelklicken auf die Datei, um es zu öffnen, markieren Sie den Inhalt der Datei, und klicken Sie dann auf **Ausführen**. Die Ausgabe im Bereich **Konsole** sollte angezeigt werden.
 
    ![Testen der installation] (./media/hdinsight-hadoop-r-server-install-r-studio/test-r-script.png "Testen der installation")

Eine weitere Möglichkeit zum Eingeben von wäre `source(testhdi.r)` oder `source(testhdi_spark.r)` , das Skript auszuführen.

## <a name="see-also"></a>Siehe auch

- [Berechnen Sie Kontextoptionen für R Server HDInsight Cluster](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure Speicheroptionen für R Server auf HDInsight premium](hdinsight-hadoop-r-server-storage.md)


 
