<properties 
   pageTitle="Verwenden von Apache Karlsruhe und Eichhörnchen in HDInsight | Microsoft Azure" 
   description="Erfahren Sie, wie Apache Karlsruhe in HDInsight verwenden und wie Sie installieren und Konfigurieren von Eichhörnchen auf Ihre Arbeitsstationen für die Verbindung zu einem Cluster HBase in HDInsight." 
   services="hdinsight" 
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
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="use-apache-phoenix-and-squirrel-with-windows-based-hbase-clusters-in-hdinsight"></a>Verwenden von Apache Karlsruhe und Eichhörnchen mit Windows-basierten HBase Cluster in HDinsight  

Erfahren Sie, wie [Apache Karlsruhe](http://phoenix.apache.org/) in HDInsight verwenden und wie Sie installieren und Konfigurieren von Eichhörnchen auf Ihre Arbeitsstationen für die Verbindung zu einem Cluster HBase in HDInsight. Weitere Informationen zu Karlsruhe finden Sie unter [Karlsruhe in 15 Minuten oder weniger](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Die Grammatik Karlsruhe finden Sie unter [Karlsruhe Grammatik](http://phoenix.apache.org/language/index.html).

>[AZURE.NOTE] Die Version Karlsruhe Informationen in HDInsight finden Sie unter [Neuigkeiten in die Hadoop Cluster Versionen von HDInsight bereitgestellten?] [hdinsight-versions].
>
> Die Informationen in diesem Dokument ist für Windows-basiertem HDInsight Cluster spezifisch. Informationen zum Verwenden von Karlsruhe auf Linux-basierten HDInsight finden Sie unter [Verwenden Apache Karlsruhe mit HBase Linux-basierten Cluster in HDinsight](hdinsight-hbase-phoenix-squirrel-linux.md).

##<a name="use-sqlline"></a>Verwenden von SQLLine
[SQLLine](http://sqlline.sourceforge.net/) ist eine Befehlszeile-Programm, mit der SQL-Anweisung auszuführen. 

###<a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie SQLLine verwenden können, müssen Sie Folgendes:

- **A HBase Cluster in HDInsight**. Informationen zum Bereitstellen von HBase cluster, finden Sie unter [Erste Schritte mit Apache HBase in HDInsight][hdinsight-hbase-get-started].
- **Verbinden mit dem HBase Cluster über remote desktop-Protokoll**. Anweisungen finden Sie unter [Verwalten von Hadoop Cluster in HDInsight mithilfe des klassischen Azure-Portal][hdinsight-manage-portal].

**Um herauszufinden, der Hostname**

1. Öffnen Sie **Hadoop Befehlszeile** vom Desktop aus.
2. Führen Sie den folgenden Befehl aus, um das DNS-Suffix erhalten:

        ipconfig

    Notieren Sie die **Verbindung-spezifische DNS-Suffix**. Beispielsweise *myhbasecluster.f5.internal.cloudapp.net*. Beim Herstellen einer zu einem Cluster HBase Verbindung müssen Sie den vollqualifizierten Domänennamen verwenden lassen zu verbinden. Jeder Cluster HDInsight verfügt über 3 lassen. Sie sind *zookeeper0*, *zookeeper1*und *zookeeper2*. Der vollqualifizierten Domänennamen werden ungefähr wie folgt *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.

**SQLLine verwenden**

1. Öffnen Sie **Hadoop Befehlszeile** vom Desktop aus.
2. Führen Sie die folgenden Befehle zum SQLLine zu öffnen:

        cd %phoenix_home%\bin
        sqlline.py [The FQDN of one of the Zookeepers]

    ![Hdinsight Hbase Karlsruhe sqlline][hdinsight-hbase-phoenix-sqlline]

    Die Befehle in der Stichprobe verwendet:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));
        
        !tables
        
        UPSERT INTO Company VALUES(1, 'Microsoft');
        
        SELECT * FROM Company;

Weitere Informationen finden Sie unter [SQLLine manuellen](http://sqlline.sourceforge.net/#manual) und [Karlsruhe Grammatik](http://phoenix.apache.org/language/index.html).


















##<a name="use-squirrel"></a>Verwenden von Eichhörnchen

[Eichhörnchen SQL Client](http://squirrel-sql.sourceforge.net/) ist ein grafisch Java-Programm, mit denen die Struktur einer JDBC-kompatible Datenbank anzeigen und Durchsuchen der Daten in Tabellen, SQL-Befehle usw. ausgeben kann. Es kann in Verbindung mit Apache Karlsruhe auf HDInsight verwendet werden.

In diesem Abschnitt wird gezeigt, wie installieren und Konfigurieren von Eichhörnchen auf Ihrem Computer für die Verbindung zu einem Cluster HBase in HDInsight über VPN erstellt wird. 

###<a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie die Schrittfolgen aus, müssen Sie die folgenden:

- Ein HBase-Cluster auf ein Azure-virtuellen Netzwerk mit einem DNS-virtuellen Computern bereitgestellt.  Anweisungen finden Sie unter [Bereitstellen von HBase Cluster virtuellen Netzwerk Azure][hdinsight-hbase-provision-vnet]. 

    >[AZURE.IMPORTANT] Sie müssen einen DNS-Server mit dem virtuellen Netzwerk installieren. Anweisungen finden Sie unter [Konfigurieren von DNS-Einträge zwischen zwei Azure virtuelle Netzwerke](hdinsight-hbase-geo-replication-configure-dns.md)

- Abrufen des HBase Cluster Cluster Verbindung-spezifische DNS-Suffixes an. Hier bekommen sie RDP in Cluster, und führen Sie dann IPConfig.  Das DNS-Suffix ähnelt:

        myhbase.b7.internal.cloudapp.net
- Herunterladen Sie und installieren Sie [Microsoft Visual Studio Express 2013 für Windows-Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) auf Ihrem Computer. Sie benötigen Makecert aus dem Paket, um Ihr Zertifikat zu erstellen.  
- Herunterladen Sie und installieren Sie [Java Runtime-Umgebung](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) auf Ihrem Computer.  Eichhörnchen SQL, Clientversion 3.0 und höher ist JRE Version 1.6 oder höher erforderlich.  


###<a name="configure-a-point-to-site-vpn-connection-to-the-azure-virtual-network"></a>Konfigurieren einer Punkt-zu-Standort VPN-Verbindung mit dem Azure-virtuellen-Netzwerk

Umfasst 3 Schritte eine Punkt-zu-Standort VPN-Verbindung zu konfigurieren:

1. [Konfigurieren eines virtuellen Netzwerks und dynamische routing-gateway](#Configure-a-virtual-network-and-a-dynamic-routing-gateway)
2. [Erstellen Sie eigener Zertifikate](#Create-your-certificates)
3. [Konfigurieren des VPN-Clients](#Configure-your-VPN-client)

Weitere Informationen finden Sie unter [Konfigurieren einer Punkt-zu-Standort VPN-Verbindung zu einem virtuellen Azure-Netzwerk](../vpn-gateway/vpn-gateway-point-to-site-create.md) .

#### <a name="configure-a-virtual-network-and-a-dynamic-routing-gateway"></a>Konfigurieren eines virtuellen Netzwerks und dynamische routing-gateway

Sie haben einen HBase Cluster in einem Azure-virtuellen Netzwerk nach der Bereitstellung zu gewährleisten (siehe die erforderlichen Komponenten für diesen Abschnitt). Im nächsten Schritt wird eine Punkt-zu-Standort-Verbindung konfigurieren.

**So konfigurieren Sie die Punkt-zu-Standort-Konnektivität**

1. Melden Sie sich bei der [Azure klassischen Portal][azure-portal].
2. Klicken Sie auf der linken Seite auf **Netzwerke**.
3. Klicken Sie auf das virtuelle Netzwerk, die Sie erstellt haben (finden Sie unter [Bereitstellen von HBase Cluster virtuellen Netzwerk Azure][hdinsight-hbase-provision-vnet]).
4. Klicken Sie oben auf **Konfigurieren** .
5. Wählen Sie im Abschnitt **Punkt-zu-Standort Connectivity** **Punkt-zu-Standort-Verbindung konfigurieren**. 
6. Konfigurieren von **Erste IP-** und **CIDR** , um den Bereich der IP-Adresse anzugeben, von dem VPN-Clients eine IP-Adresse, wenn verbunden empfangen werden. Der Bereich kann nicht überlappen keine Bereiche befindet sich auf Ihrem lokalen und Azure virtuelle Netzwerk, die, dem Sie in Verbindung hergestellt werden soll. Beispielsweise. Wenn Sie für das virtuelle Netzwerk 10.0.0.0/20 ausgewählt haben, können Sie nach dem Client Adresse Leerzeichen 10.1.0.0/24 auswählen. Die [Punkt-zu-Standort-Konnektivität] finden Sie unter[ vnet-point-to-site-connectivity] um weitere Informationen zu erhalten.
7. Klicken Sie im Abschnitt virtuelles Netzwerk Adresse Leerzeichen auf **Gateway Subnetz hinzufügen**.
7. Klicken Sie auf **Speichern** , klicken Sie auf den unteren Rand der Seite.
8. Klicken Sie auf **Ja,** um die Änderung zu bestätigen. Warten Sie, bis das System die Änderung vornimmt abgeschlossen ist, bevor Sie mit dem nächsten Schritt fortfahren.


**Erstellen eines dynamischen Weiterleitung Gateways**

1. Klicken Sie im klassischen Azure-Portal auf **DASHBOARD** vom oberen Rand der Seite.
2. Klicken Sie auf **Erstellen GATEWAY** vom unteren Rand der Seite.
3. Klicken Sie auf **Ja,** um zu bestätigen. Warten Sie, bis das Gateway erstellt wird.
4. Klicken Sie oben auf **DASHBOARD** .  Ein visuelles Diagramm des virtuellen Netzwerks werden angezeigt:

    ![Azure virtuelle Punkt-zu-Standort virtuelle Netzplandiagramm][img-vnet-diagram] 

    Das Diagramm zeigt 0 Clientverbindungen. Nachdem Sie eine Verbindung mit dem virtuellen Netzwerk haben, wird die Zahl auf eine aktualisiert werden. 

#### <a name="create-your-certificates"></a>Erstellen Sie eigener Zertifikate

Eine Möglichkeit zum Erstellen einer x 509-Zertifikat ist mit dem Certificate Creation Tool (makecert.exe), die im Lieferumfang von [Microsoft Visual Studio Express 2013 für Windows-Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx). 


**Zum Erstellen eines selbstsignierten Stamm-Zertifikats**

1. Öffnen Sie von Ihrem Computer ein Eingabeaufforderungsfenster.
2. Navigieren Sie zu dem Ordner der Visual Studio-Tools. 
3. Der folgende Befehl im folgenden Beispiel erstellen und Installieren eines Stammzertifikats einer im persönlichen Zertifikatspeicher auf Ihrem Computer und eine entsprechende CER-Datei, die Sie später klassischen Azure-Portal hochladen werden auch erstellen. 

        makecert -sky exchange -r -n "CN=HBaseVnetVPNRootCertificate" -pe -a sha1 -len 2048 -ss My "C:\Users\JohnDole\Desktop\HBaseVNetVPNRootCertificate.cer"

    Wechseln Sie zum Verzeichnis, das die CER-Datei, wobei HBaseVnetVPNRootCertificate der Name ist, die Sie für das Zertifikat verwenden möchten, gesucht werden soll. 

    Schließen Sie nicht die Befehlszeile.  Sie benötigen sie im nächsten Schritt.

    >[AZURE.NOTE] Da Sie ein Zertifikat einer Stammzertifizierungsstelle erstellt haben, von dem Client-Zertifikate generiert werden, sollten Sie diese Zertifikat und seinen privaten Schlüssel exportieren, und speichern es an einem sicheren Ort, wo es wiederhergestellt werden kann. 

**Zum Erstellen eines Clientzertifikats**

- In der gleichen Befehlszeile (es muss auf dem gleichen Computer werden, in dem Sie das Zertifikat der Stamm erstellt. Das Client-Zertifikat generiert werden muss aus dem Stammordner Zertifikat), führen Sie den folgenden Befehl aus:

        makecert.exe -n "CN=HBaseVnetVPNClientCertificate" -pe -sky exchange -m 96 -ss My -in "HBaseVnetVPNRootCertificate" -is my -a sha1

    HBaseVnetVPNRootCertificate ist der Name des Zertifikats.  Es muss der Name des Zertifikats entsprechen.  

    Sowohl die Quadratwurzel Zertifikat und das Client werden in Ihrer persönlichen Zertifikats Store auf Ihrem Computer gespeichert. Verwenden Sie zur Überprüfung certmgr.msc ein.

    ![Azure virtuelles Netzwerk Punkt-zu-Standort Vpn Zertifikat][img-certificate]

    Ein Client-Zertifikat muss auf jedem Computer installiert sein, die Sie an das virtuelle Netzwerk eine Verbindung herstellen möchten. Es empfiehlt sich, dass Sie eindeutige Client Zertifikate für jeden Computer erstellen, die Sie an das virtuelle Netzwerk eine Verbindung herstellen möchten. Verwenden Sie zum Exportieren der Client-Zertifikate certmgr.msc ein. 

**Das Zertifikat der Stamm klassischen Azure-Portal hochladen**

1. Klicken Sie auf der linken Seite auf **Netzwerk** , aus dem klassischen Azure-Portal.
2. Klicken Sie auf das virtuelle Netzwerk, wo Ihre HBase Cluster zu bereitgestellt wird.
3. Klicken Sie oben auf **Zertifikate** .
4. Klicken Sie unten auf **Hochladen** , und geben Sie die Datei im Stammverzeichnis, die Sie in der Prozedur vor dem letzten erstellt haben. Warten Sie, bis das Zertifikat importiert haben.
5. Klicken Sie oben auf **DASHBOARD** .  Das virtuelle Diagramm zeigt den Status.


#### <a name="configure-your-vpn-client"></a>Konfigurieren des VPN-Clients



**Herunterladen und Installieren des VPN-Client-Pakets**

1. Seite des virtuellen Netzwerk und im Abschnitt den ersten Blick klicken Sie entweder auf **die 64-Bit-Client-VPN-Paket herunterladen** auf dem DASHBOARD oder **Herunterladen der 32-Bit-Client-VPN-Paket** basierend auf Ihrer Arbeitsstationen OS Version.
2. Klicken Sie auf **Ausführen** , um das Paket zu installieren.
3. Sicherheit dazu aufgefordert werden klicken Sie auf **Weitere Informationen**, und klicken Sie dann auf **trotzdem ausführen**.
4. Klicken Sie auf **Ja** zweimal.

**Verbindung zum VPN**

1. Klicken Sie auf dem Desktop des Computers auf das Symbol Netzwerken auf der Taskleiste. Sie sind eine VPN-Verbindung mit Ihrem Namen virtuelles Netzwerk finden Sie unter.
2. Klicken Sie auf den Namen der VPN-Verbindung.
3. Klicken Sie auf **Verbinden**.

**So testen Sie die VPN-Verbindung und Domain namensauflösung**

- Öffnen Sie ein Eingabeaufforderungsfenster die Arbeitsstationen und Ping eine der folgenden Namen die HBase Cluster DNS-Suffix angegeben ist myhbase.b7.internal.cloudapp.net:

        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        headnode0.myhbase.b7.internal.cloudapp.net
        headnode1.myhbase.b7.internal.cloudapp.net
        workernode0.myhbase.b7.internal.cloudapp.net

###<a name="install-and-configure-squirrel-on-your-workstation"></a>Installieren und Konfigurieren von Eichhörnchen auf Ihrem Computer

**So installieren Sie Eichhörnchen**

1. Laden Sie die Eichhörnchen SQL Client JAR-Datei aus [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation)aus.
2. Die JAR-Datei öffnen/ausführen. Es ist eine der [Java Runtime-Umgebung](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html)erforderlich.
3. Klicken Sie zweimal auf **Weiter** .
4. Geben Sie einen Pfad, in dem Sie die Schreibberechtigung verfügen, und klicken Sie dann auf **Weiter**.
    >[AZURE.NOTE] Der Standardordner für die Installation ist in den Ordner c:\Programme\Microsoft Files\squirrel-Sql-3.6.  Damit in dieser Pfad zu schreiben, muss das Installationsprogramm die Administratorrechte erteilt werden. Sie können ein Eingabeaufforderungsfenster als Administrator, navigieren Sie zu der Java Papierkorb Ordner und führen Sie dann 
    >
    >     java.exe -jar [the path of the SQuirreL jar file] 
5. Klicken Sie auf **OK** , um zu bestätigen Zielverzeichnis zu erstellen.
6. Die Standardeinstellung ist die Basis und Standard-Paketen installieren.  Klicken Sie auf **Weiter**.
7. Klicken Sie zweimal auf **Weiter** , und klicken Sie dann auf **Fertig**.


**So installieren Sie den Treiber Karlsruhe**

Karlsruhe Treiber JAR-Datei befindet sich die HBase Cluster. Der Pfad lautet wie folgt basierend auf den Versionen:

    C:\apps\dist\phoenix-4.0.0.2.1.11.0-2316\phoenix-4.0.0.2.1.11.0-2316-client.jar
Sie müssen in Ihrer Arbeitsstationen unter [Eichhörnchen Installationsordner] kopieren / lib Pfad.  Die einfachste Möglichkeit besteht darin, RDP zum Cluster, und verwenden Sie dann die Datei kopieren und einfügen (STRG + C und STRG + V), um es in Ihre Arbeitsstationen zu kopieren.

**Um einen Treiber Karlsruhe Eichhörnchen hinzufügen**

1. Öffnen Sie Eichhörnchen SQL Client von Ihrem Computer aus.
2. Klicken Sie auf der Registerkarte **Treiber** auf der linken Seite.
2. Klicken Sie im Menü **Treiber** auf **Neuen Treiber**.
3. Geben Sie die folgenden Informationen ein:

    - **Name**: Karlsruhe
    - **Beispiel-URL**: jdbc:phoenix:zookeeper2.contoso-Hbase-eu.f5.internal.cloudapp.net
    - **Klassennamen**: org.apache.phoenix.jdbc.PhoenixDriver

    >[AZURE.WARNING] Benutzer alle Kleinbuchstaben in der Beispiel-URL. Können sie vollständige Zookeeper Quorum für den Fall, dass eine von ihnen unten ist.  Die Hostnamen sind zookeeper0, zookeeper1 und zookeeper2.

    ![HDInsight HBase Karlsruhe Eichhörnchen-Treiber][img-squirrel-driver]
4. Klicken Sie auf **OK**.

**Erstellen einen Alias für den Cluster HBase**

1. Eichhörnchen klicken Sie auf der Registerkarte **Aliases** auf der linken Seite.
2. Klicken Sie auf **Neuen Alias**, wählen Sie im Menü **Aliases** .
3. Geben Sie die folgenden Informationen ein:

    - **Name**: der Name des HBase Cluster oder einen beliebigen Namen, die Sie bevorzugen.
    - **Treiber**: Karlsruhe.  Dieser muss den Treibernamen übereinstimmen, die, den Sie in der letzten Prozedur erstellt haben.
    - **URL**: der URL wird aus Ihrer Treiberkonfiguration kopiert. Vergewissern Sie sich Benutzer alle Kleinbuchstaben.
    - **Benutzername**: Text sind möglich.  Da Sie hier VPN-Konnektivität verwenden, wird der Benutzername nicht gar verwendet.
    - **Kennwort**: Text sind möglich.

    ![HDInsight HBase Karlsruhe Eichhörnchen-Treiber][img-squirrel-alias]
4. Klicken Sie auf **Testen**. 
5. Klicken Sie auf **Verbinden**. Wenn sie die Verbindung herstellt, sieht wie Eichhörnchen aus:

    ![Karlsruhe HBase Eichhörnchen][img-squirrel]

**Zum Ausführen eines Tests**

1. Klicken Sie auf der Registerkarte " **SQL** " rechts neben der Registerkarte **Objekte** .
2. Kopieren Sie und fügen Sie den folgenden Code ein:

        CREATE TABLE IF NOT EXISTS us_population (state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT  CONSTRAINT my_pk PRIMARY KEY (state, city))
3. Klicken Sie auf die Schaltfläche "ausführen".

    ![Karlsruhe HBase Eichhörnchen][img-squirrel-sql]
4. Wechseln Sie wieder zur Registerkarte **Objekte** .
5. Erweitern Sie den Aliasnamen ein, und klicken Sie dann **Tabelle**.  So finden Sie unter der neuen Tabelle unter aufgeführt.
 
##<a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie die Verwendung von Apache Karlsruhe HDInsight erhalten.  Weitere Informationen finden Sie unter

- [Übersicht über die HDInsight HBase][hdinsight-hbase-overview]: HBase ist einer Apache, Open-Source NoSQL-Datenbank basierend auf Hadoop, die Direktzugriff und sicherer Konsistenz für große Mengen von unstrukturierten und semistructured Daten enthält.
- [Bereitstellung von HBase Cluster virtuellen Netzwerk Azure][hdinsight-hbase-provision-vnet]: virtuelles Netzwerk Integration, HBase Cluster mit demselben virtuellen Netzwerk als Ihre Applikationen bereitgestellt werden, damit Applikationen direkt mit HBase kommunizieren können.
- [Konfigurieren von HBase Replikation in HDInsight](hdinsight-hbase-geo-replication.md): erfahren Sie, wie HBase Replikation über zwei Azure Datencenter konfigurieren. 
- [Analysieren Sie Twitter-Grüße mit HBase in HDInsight][hbase-twitter-sentiment]: erfahren Sie, wie in Echtzeit [Grüße Analyse](http://en.wikipedia.org/wiki/Sentiment_analysis) große Datenmengen in einem Cluster Hadoop in HDInsight mithilfe von HBase ausführen.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png


 
