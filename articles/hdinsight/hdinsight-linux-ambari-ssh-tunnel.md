<properties
pageTitle="Zugriff auf Ambari Web UI, Ressourcen-Manager, JobHistory, NameNode, Oozie und andere Web Benutzeroberfläche verwenden Sie SSH Tunnel"
description="Erfahren Sie, wie mit einem SSH Tunnel um Webressourcen Ihrer Linux-basierten HDInsight Knoten sicher zu durchsuchen."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="10/17/2016"
ms.author="larryfr"/>

# <a name="use-ssh-tunneling-to-access-ambari-web-ui-jobhistory-namenode-oozie-and-other-web-uis"></a>Verwenden von SSH Tunnel Ambari Web UI, JobHistory, NameNode, Oozie und andere Web Benutzeroberfläche Zugriff auf

Linux-basierten HDInsight Cluster bieten Zugriff auf Ambari Web-Benutzeroberfläche über das Internet, jedoch sind einige Features der Benutzeroberfläche nicht. Beispiel der Web-Benutzeroberfläche für andere Dienste, die durch Ambari dargestellt werden. Für die vollständige Funktionalität von der Ambari Web-Benutzeroberfläche müssen Sie einen Tunnel SSH auf den Anfang der Cluster verwenden.

## <a name="what-requires-an-ssh-tunnel"></a>Was ist einen Tunnel SSH erforderlich?

Mehrere Menüs in Ambari werden nicht vollständig ohne einen Tunnel SSH auffüllen, wie sie von Websites und Diensten, die von anderen Hadoop Services ausgeführt wird, auf dem Cluster verfügbar gemacht werden abhängig sind. Diese Websites werden häufig nicht gesichert, sodass Sie nicht sicher direkt im Internet verfügbar gemacht werden kann. In einigen Fällen wird der Dienst der Website auf einem anderen Knoten z. B. ein Zookeeper Knoten ausgeführt.

Im folgenden sind die Dienste, die Web-Benutzeroberfläche Ambari verwendet, und ohne einen Tunnel SSH nicht zugegriffen werden können:

* JobHistory,
* NameNode,
* Thread-Stapel
* Oozie Web-Benutzeroberfläche
* HBase Master und Protokolle-Benutzeroberfläche

Wenn Sie Skript-Aktionen verwenden, um Ihren Cluster anzupassen, werden alle Dienste und Dienstprogramme, die Sie installieren, die eine Web-Benutzeroberfläche verfügbar machen einen SSH Tunnel erforderlich. Wenn Sie mit einer Aktion Skript Farbton installieren, müssen Sie Zugriff auf die Website Farbton Benutzeroberfläche beispielsweise einen Tunnel SSH verwenden.

## <a name="what-is-an-ssh-tunnel"></a>Was ist ein Tunnel SSH?

[Secure Shell (SSH) Tunnel](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) leitet den Datenverkehr an einen Anschluss am lokalen Computers über eine SSH-Verbindung zu Ihrem HDInsight Cluster am Knoten, in dem die Anforderung dann gelöst ist, als wäre sie auf dem am Knoten stammte gesendet werden. Die Antwort wird dann wieder durch den Tunnel an Ihre Arbeitsstationen geleitet.

## <a name="prerequisites"></a>Erforderliche Komponenten

Wenn Sie einen Tunnel SSH für Datenverkehr im Web verwenden zu können, benötigen Sie Folgendes:

* SSH-Client. Für die Verteilung von Linux und Unix oder Macintosh OS X die `ssh` Befehl wird mit dem Betriebssystem bereitgestellt. Für Windows empfehlen wir [kitten](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

    > [AZURE.NOTE] Wenn Sie mit anderen als SSH-Client verwenden möchten `ssh` oder kitten, wenden Sie sich bitte die Dokumentation für Ihren Kunden, wie Sie einen Tunnel SSH einrichten.

* Einen Webbrowser, der Verwendung eines Proxys SOCKS konfiguriert werden können

## <a name="a-nameusesshacreate-a-tunnel-using-the-ssh-command"></a><a name="usessh"></a>Erstellen Sie einen Tunnel mit dem Befehl SSH

Verwenden Sie der folgende Befehl zum Erstellen einer SSH tunnel mithilfe der `ssh` Befehl. Ersetzen Sie __USERNAME__ mit einem Benutzer SSH für Ihren Cluster HDInsight, und Ersetzen Sie __CLUSTERNAME__ mit dem Namen der Cluster HDInsight

    ssh -C2qTnNf -D 9876 USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

Dadurch wird eine Verbindung aus, die den Datenverkehr auf lokale Port 9876 Cluster über SSH weiterleiten erstellt. Die Optionen sind:

* **D 9876** - lokalen Port, die durch den Tunnel Datenverkehr weiterleiten wird.

* **C** - alle Daten komprimiert werden, da der Web-Verkehr hauptsächlich Text ist.

* **2** - Force SSH, Protocol, Version 2 nur zu testen.

* **f** - stillen Modus.

* **T** - Pseudo tty Zuteilung deaktivieren, da wir gerade einen Port weitergeleitet werden.

* **n** - verhindern, dass das Lesen eines STDIN, da wir gerade einen Port weitergeleitet werden.

* **N** - keinen remote-Befehl nicht ausführen, da wir gerade einen Port weitergeleitet werden.

* **f** - im Hintergrund ausgeführt werden.

Wenn Sie mit einem SSH Schlüssel Cluster konfiguriert haben, können Sie benötigen verwenden die `-i` Parameter, und geben Sie den Pfad für die privaten Schlüssel SSH.

Nachdem der Befehl abgeschlossen ist, den Datenverkehr an Anschluss 9876 auf dem lokalen Computer gesendeten über weitergeleitet werden, Layer SSL (Secure Sockets) mit dem Cluster Knoten leiten und angezeigt werden, um es in diesem Fenster stammen.

## <a name="a-nameuseputtyacreate-a-tunnel-using-putty"></a><a name="useputty"></a>Erstellen Sie einen Tunnel kitten verwenden

Gehen Sie folgendermaßen vor, um eine SSH Tunnel mit kitten zu erstellen.

1. Öffnen Sie kitten, und geben Sie die Verbindungsinformationen. Wenn Sie nicht mit kitten vertraut sind, finden Sie unter [Verwenden SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md) Informationen zur gemeinsamen Nutzung, mit HDInsight.

2. Im Abschnitt **Kategorie** auf der linken Seite des Dialogfelds erweitern Sie **Verbindung** **SSH**, und wählen Sie dann auf **Tunnel**.

3. Geben Sie die folgenden Informationen im Formular **Optionen steuern SSH Port zum Weiterleiten** an:

    * **Quellport** : den Port auf dem Client, den Sie weiterleiten möchten. Beispielsweise **9876**.

    * **Ziel** - das SSH-Adresse für den Cluster Linux-basierten HDInsight. Beispielsweise **MeinCluster-ssh.azurehdinsight.net**.

    * **Dynamic** - ermöglicht dynamische SOCKS Proxy routing.

    ![Abbildung der Tunnel Optionen](./media/hdinsight-linux-ambari-ssh-tunnel/puttytunnel.png)

4. Klicken Sie auf **Hinzufügen** , um die Einstellungen hinzufügen, und klicken Sie dann auf **Öffnen** , um eine SSH-Verbindung zu öffnen.

5. Wenn Sie dazu aufgefordert werden, melden Sie sich auf dem Server. Dies Aufbau einer SSH-Sitzung, und aktivieren Sie den Tunnel.

## <a name="use-the-tunnel-from-your-browser"></a>Verwenden Sie den Tunnel im Browser

> [AZURE.NOTE] Die Schritte in diesem Abschnitt verwenden den FireFox-Browser, wie es für Linux, Unix, Macintosh OS X und Windows-Betriebssysteme kostenlos verfügbar ist. Andere moderne Browser, die über einen SOCKS-Proxyserver Support sowie funktionieren.

1. Konfigurieren Sie den Browser, um **Localhost:9876** als **v5 SOCKS** Proxy verwenden. Hier wird das Aussehen der Firefox-Einstellungen. Wenn Sie einen anderen Anschluss als 9876 verwendet haben, ändern Sie den Port nach dem von Ihnen verwendete vorkommen:

    ![Abbildung der Firefox-Einstellungen](./media/hdinsight-linux-ambari-ssh-tunnel/socks.png)

    > [AZURE.NOTE] Auswählen der **Remote-DNS-** Anfragen (DNS = Domain Name System) zum Auflösen HDInsight Cluster. Wenn dies nicht ausgewählt ist, wird DNS lokal aufgelöst werden.

2. Stellen Sie sicher, dass Datenverkehr durch den Tunnel weitergeleitet wird von einer Website wie [http://www.whatismyip.com/](http://www.whatismyip.com/) mit den Proxyeinstellungen aktiviert und deaktiviert in Firefox vising. Während die Einstellungen aktiviert sind, werden die IP-Adresse für einen Computer im Microsoft Azure Datencenter.

##<a name="verify-with-ambari-web-ui"></a>Vergewissern Sie sich beim Ambari Web-Benutzeroberfläche

Sobald der Cluster hergestellt wurde, gehen Sie folgendermaßen vor, um sicherzustellen, dass Sie Service Web Benutzeroberflächen aus dem Ambari Web zugreifen können:

1. Wechseln Sie in Ihrem Browser zu Http://headnodehost:8080. Die `headnodehost` Adresse über den Tunnel an den Cluster gesendet und in der Headnode, die Ambari ausgeführt wird, klicken Sie auf auflösen. Wenn Sie dazu aufgefordert werden, geben Sie den Administratorbenutzernamen (Administrator) und das Kennwort für Ihren Cluster aus. Sie möglicherweise ein zweites Mal nach der Ambari Web-Benutzeroberfläche aufgefordert werden. Wenn dies der Fall ist, geben Sie die Informationen erneut.
    
    > [AZURE.NOTE] Wenn die Adresse Http://headnodehost:8080 mit Cluster herstellen, werden Sie eine Verbindung herstellen direkt über den Tunnel zum am Knoten, die auf Ambari ausgeführt wird, mithilfe von HTTP und Kommunikation ist mit der SSH Tunnel gesichert. Wenn Sie über das Internet ohne Verwendung von einen Tunnel zu verbinden, ist Kommunikation mit HTTPS gesichert. Informationen zum Verbinden über das Internet mit HTTPS verwenden Sie https://CLUSTERNAME.azurehdinsight.net, wobei __CLUSTERNAME__ den Namen der Cluster.

2. Wählen Sie aus der Ambari Web-Benutzeroberfläche HDFS aus der Liste auf der linken Seite der Seite ein.

    ![Bild mit HDFS ausgewählt](./media/hdinsight-linux-ambari-ssh-tunnel/hdfsservice.png)

3. Wenn die Daten zu Dienstleistungen HDFS angezeigt wird, wählen Sie __Quicklinks__aus. Es wird eine Liste der Cluster am Knoten angezeigt. Wählen Sie eine der am Knoten, und wählen Sie anschließend __NameNode UI__.

    ![Bild mit erweiterten Menü QuickLinks](./media/hdinsight-linux-ambari-ssh-tunnel/namenodedropdown.png)

    > [AZURE.NOTE] Oder, wenn Sie eine langsame Verbindung zum Internet, der am Knoten ist ausgelastet wird möglicherweise einen Indikator warten, statt ein Menü angezeigt werden, bei der Auswahl von __Quicklinks__. Ist dies der Fall ist, warten Sie eine oder zwei Minuten für Daten vom Server empfangen werden, und wiederholen Sie dann die Liste.
    >
    > Oder, wenn Sie einen niedrigeren Auflösung Monitor, das Browserfenster nicht maximiert ist können einige Einträge im Menü __Quicklinks__ durch die rechte Seite des Bildschirms abgeschnitten werden. In diesem Fall erweitern Sie des Menüs mit der Maus, und klicken Sie dann verwenden Sie die nach-rechts-Taste auf dem Bildschirm nach rechts, um den Rest des Menüs finden Sie unter Ausführen eines Bildlaufs.

4. Eine Seite ähnlich wie die folgende sollte angezeigt:

    ![Abbildung der Benutzeroberfläche NameNode](./media/hdinsight-linux-ambari-ssh-tunnel/namenode.png)

    > [AZURE.NOTE] Beachten Sie die URL für diese Seite aus. Es sollte ähnlich wie __Http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/Cluster__sein. Dies ist den internen vollqualifizierten Domänennamen (FULLY) des Knotens verwenden und kann nicht ohne einen Tunnel SSH zugegriffen werden.

## <a name="next-steps"></a>Nächste Schritte

Jetzt, da Sie zum Erstellen und verwenden einen Tunnel SSH gelernt haben, finden Sie hier Informationen auf Überwachung und Verwaltung von den Cluster mithilfe der Ambari:

* [Verwalten von Cluster HDInsight mithilfe von Ambari](hdinsight-hadoop-manage-ambari.md)

Weitere Informationen zum Verwenden von SSH mit HDInsight finden Sie unter den folgenden:

* [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

* [Verwenden von SSH mit Linux-basierten Hadoop auf HDInsight von Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
