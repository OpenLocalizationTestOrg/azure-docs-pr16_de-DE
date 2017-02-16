<properties 
   pageTitle="Konfigurieren von DNS zwischen zwei Azure virtuelle Netzwerke | Microsoft Azure" 
   description="Informationen zum Konfigurieren von VPN-Verbindungen und Auflösung des Domänennamens zwischen zwei virtuelle Netzwerke und HBase Geo-Replikation konfigurieren." 
   services="hdinsight,virtual-network" 
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
   ms.date="06/28/2016"
   ms.author="jgao"/>

# <a name="configure-dns-between-two-azure-virtual-networks"></a>Konfigurieren von DNS zwischen zwei Azure virtuelle Netzwerke

> [AZURE.SELECTOR]
- [Konfigurieren von VPN-Konnektivität](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [Konfigurieren von DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [Konfigurieren der Replikation HBase](hdinsight-hbase-geo-replication.md) 


Informationen Sie zum Hinzufügen und Konfigurieren von DNS-Server mit Azure virtuellen Netzwerken innerhalb und über die virtuellen Netzwerke mit einer namensauflösung von zu behandeln.

In diesem Lernprogramm ist des zweiten Teils der [Reihe] [ hdinsight-hbase-geo-replication] HBase Geo-Replikation zu erstellen:

- [Konfigurieren eines VPN-Konnektivität zwischen zwei virtuelle Netzwerke][hdinsight-hbase-geo-replication-vnet]
- Konfigurieren von DNS für die virtuellen Netzwerke (in diesem Lernprogramm)
- [Konfigurieren der HBase Geo Replikation][hdinsight-hbase-geo-replication]


Das folgende Diagramm veranschaulicht die beiden virtuelle Netzwerke, die Sie in [Konfigurieren eines VPN-Konnektivität zwischen zwei virtuelle Netzwerke erstellt][hdinsight-hbase-geo-replication-vnet]:

![HDInsight HBase Replikation virtuelle Netzplandiagramm][img-vnet-diagram]

##<a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie dieses Lernprogramm beginnen, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion Azure abrufen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Eine Arbeitsstationen mit Azure PowerShell**.

    Stellen Sie bevor PowerShell-Skripts sicher, dass Sie das folgende Cmdlet mit Ihrem Azure-Abonnement verbunden sind:

        Add-AzureAccount

    Wenn Sie mehrere Azure-Abonnements verfügen, verwenden Sie das folgende Cmdlet aus, um das aktuelle Abonnement festlegen:

        Select-AzureSubscription <AzureSubscriptionName>
        
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Zwei Azure virtuelles Netzwerk mit VPN-Konnektivität**.  Anweisungen finden Sie unter [Konfigurieren einer VPN-Verbindung zwischen zwei Azure virtuelle Netzwerke][hdinsight-hbase-geo-replication-vnet].

>[AZURE.NOTE] Azure Service und virtuellen Computern Names müssen eindeutig sein. Der Name in diesem Lernprogramm wird Contoso-[Name der Azure Service/virtueller Computer]-[EU / US]. Contoso-VNet-EU beträgt beispielsweise das Azure virtuelle Netzwerk in Europa North Data Center; Contoso-DNS-US ist der DNS-Server virtueller Computer in ostasiatischen US Data Center. Sie müssen den Namen Ihrer eigenen ergibt.
 
 
##<a name="create-azure-virtual-machines-to-be-used-as-dns-servers"></a>Erstellen von Azure-virtuellen Computern als DNS-Server verwendet werden soll

**Erstellen eines virtuellen Computers innerhalb Contoso VNet EU, mit dem Namen Contoso-DNS-EU**

1.  Klicken Sie auf **neu** **zu berechnen**, **virtuellen Computern**, **FROM-Katalog**.
2.  Wählen Sie **Windows Server 2012 R2 Datacenter**aus.
3.  Geben Sie ein:
    - **NAME des virtuellen Computers**: Contoso-DNS-EU
    - **Neuer Benutzername**: 
    - **Neues Kennwort**: 
4.  Geben Sie ein:
    - **CLOUD-Dienst**: erstellen einen neuen Cloud-Dienst
    - **REGION/Gruppe Zugehörigkeit/virtuellen Netzwerk**: (Select Contoso-VNet-EU)
    - **Virtuellen NETZWERKSUBNETS**: Subnetz-1
    - **Speicher-Konto**: Verwenden Sie ein automatisch generiertes Speicherkonto
    
        Der Name der Cloud-Dienst wird der Name des virtuellen Computers identisch sein. In diesem Fall ist den Contoso-DNS-EU. Für nachfolgende virtuellen Computern kann ich mit den gleichen Clouddienst auswählen.  Alle virtuellen Computern unter demselben Cloud-Dienst Freigeben der gleichen virtuellen Netzwerk und Domänensuffix an.

        Das Speicherkonto dient zum Speichern von virtuellen Computern Bilddatei. 
    - **ENDPUNKTE**: (einen Bildlauf nach unten, und wählen Sie **DNS**) 

Nach der Erstellung des virtuellen Computers herausfinden der internen IP-Adresse und externe IP-Adresse.

1.  Klicken Sie auf der Name des virtuellen Computers, **Contoso-DNS-EU**.
2.  Klicken Sie auf **DashBoard**.
3.  Notieren Sie sich ein:
    - ÖFFENTLICHE VIRTUELLE IP-ADRESSE
    - INTERNE IP-ADRESSE


**Erstellen eines virtuellen Computers innerhalb der Contoso-VNet-US, mit dem Namen Contoso-DNS-US** 

- Wiederholen Sie dasselbe Verfahren, das zum Erstellen eines virtuellen Computers mit den folgenden Werten aus:
    - Der NAME des virtuellen Computers: Contoso-DNS-US
    - REGION/Gruppe Zugehörigkeit/virtuellen Netzwerk: Wählen Sie Contoso-VNET-US
    - VIRTUELLE NETZWERKSUBNETS: Subnetz-1
    - SPEICHERKONTO: Verwenden Sie ein Speicherkonto automatisch generierte
    - ENDPUNKTE: (DNS auswählen)

##<a name="set-static-ip-addresses-for-the-two-virtual-machines"></a>Festlegen von statischen IP-Adressen für die zwei virtuellen Computern

DNS-Server erfordert statische IP-Adressen.  Dieser Schritt kann nicht vom klassischen Azure-Portal erfolgen. Verwenden Sie PowerShell Azure.

**So konfigurieren Sie statische IP-Adresse für den zwei virtuellen Computern**

1. Öffnen Sie Windows PowerShell ISE.
2. Führen Sie die folgenden Cmdlets.  

        Add-AzureAccount
        Select-AzureSubscription [YourAzureSubscriptionName]
        
        Get-AzureVM -ServiceName Contoso-DNS-EU -Name Contoso-DNS-EU | Set-AzureStaticVNetIP -IPAddress 10.1.0.4 | Update-AzureVM
        Get-AzureVM -ServiceName Contoso-DNS-US -Name Contoso-DNS-US | Set-AzureStaticVNetIP -IPAddress 10.2.0.4 | Update-AzureVM 

    Dienstname ist der Name der Cloud-Dienst an. Da der DNS-Server den ersten virtuellen Computer der Cloud-Dienst ist, ist der Name der Cloud-Dienst der Name des virtuellen Computers identisch.

    Sie müssen möglicherweise ServiceName und Name den Namen übereinstimmen, die Sie aktualisieren.


##<a name="add-the-dns-server-role-the-two-virtual-machines"></a>Fügen Sie den DNS-Server Rolle der beiden virtuellen Computern

**Um die DNS-Server-Rolle für Contoso-DNS-EU hinzuzufügen**

1.  Klicken Sie im klassischen Azure-Portal auf der linken Seite auf **virtuellen Computern** . 
2.  Klicken Sie auf **Contoso-DNS-EU**.
3.  Klicken Sie oben auf **DASHBOARD** .
4.  Klicken Sie auf **Verbinden** von unten, und folgen Sie die Anweisungen, um die Verbindung des virtuellen Computers über RDP.
2.  Klicken Sie innerhalb der RDP-Sitzung auf die Windows-Schaltfläche auf der unteren linken Ecke, um den Startbildschirm zu öffnen.
3.  Klicken Sie auf die Kachel der **Server-Manager** .
4.  Klicken Sie auf **Hinzufügen, Rollen und Features**.
5.  Klicken Sie auf **Weiter**
6.  Wählen Sie **rollenbasierte oder featurebasierten Installation**, und klicken Sie dann auf **Weiter**.
7.  Wählen Sie Ihre DNS-virtuellen Computern (es muss bereits hervorgehoben werden) aus, und klicken Sie dann auf **Weiter**.
8.  Überprüfen Sie die **DNS-Server**.
9.  Klicken Sie auf **Features hinzufügen**, und klicken Sie dann auf **Weiter**.
10. Klicken Sie dreimal auf **Weiter** , und klicken Sie dann auf **Installieren**. 

**Um die DNS-Server-Rolle für Contoso-DNS-US hinzuzufügen**

- Wiederholen Sie die Schritte zum Hinzufügen von DNS-Rolle zu **Contoso-DNS-US**aus.

##<a name="assign-dns-servers-to-the-virtual-networks"></a>Zuweisen von DNS-Server mit virtuellen Netzwerken

**Um die beiden DNS-Server zu registrieren.**

1.  Klicken Sie im klassischen Azure-Portal auf **neu**, **NETWORK SERVICES**, **Virtuelles Netzwerk** **DNS-SERVER registrieren**.
2.  Geben Sie ein:
    - **NAME**: Contoso-DNS-EU
    - **DNS-SERVER IP-Adresse**: 10.1.0.4 – die IP-Adresse muss mit der DNS-Server virtuellen Computern IP-Adresse.
     
3.  Wiederholen Sie den Vorgang zum Registrieren Contoso-DNS-US mit den folgenden Einstellungen aus:
    - **NAME**: Contoso-DNS-US
    - **DNS-SERVER IP-Adresse**: 10.2.0.4

**Die beiden DNS-Server die beiden virtuelle Netzwerke zuweisen**

1.  Klicken Sie im linken Bereich in der klassischen Portal **Netzwerken** auf.
2.  Klicken Sie auf **Contoso-VNet-EU**.
3.  Klicken Sie auf **Konfigurieren**.
4.  Wählen Sie **Contoso-DNS-EU** im Abschnitt **DNS-Servers** ein.
5.  Klicken Sie auf **Speichern** , klicken Sie auf den unteren Rand der Seite, und klicken Sie auf **Ja,** um zu bestätigen.
6.  Wiederholen Sie den Vorgang, um den **Contoso-DNS-US** -DNS-Server mit dem virtuellen **Contoso-VNet-US** -Netzwerk zuweisen.

Alle virtuellen Computern, die mit der virtuellen Netzwerken bereitgestellt wurden muss neu gestartet werden, um die DNS-Server-Konfiguration zu aktualisieren.

**Die virtuellen Computer neu gestartet.**

1. Klicken Sie im klassischen Azure-Portal auf der linken Seite auf **virtuellen Computern** .
2. Klicken Sie auf **Contoso-DNS-EU**.
3. Klicken Sie oben auf **Dashboard** .
4. Klicken Sie unten auf **neu starten** .
5. Wiederholen Sie dieselben Schritte aus, um **Contoso-DNS-US**neu zu starten.


##<a name="configure-dns-conditional-forwarders"></a>Konfigurieren von DNS-bedingte Weiterleitung

Der DNS-Server in jeder virtuelle Netzwerk kann DNS-Namen innerhalb dieses virtuellen Netzwerks nur aufgelöst werden. Sie müssen eine bedingte Weiterleitung verweisen auf dem Peer-DNS-Server für die Auflösung von Namen in das virtuelle Peer-Netzwerk konfigurieren.

Um bedingte Weiterleitung konfigurieren zu können, müssen Sie die Domänensuffixe zwei DNS-Server kennen. Die DNS-Suffixe können abhängig von der Cloud Services-Konfiguration unterscheiden, die Sie verwendet werden, wenn Sie die virtuellen Computer erstellt haben. Für jede DNS-Suffix in das virtuelle Netzwerk verwendet werden müssen Sie eine bedingte Weiterleitung hinzufügen. 

**Um die Domänensuffixe zwei DNS-Server zu finden.**

1. RDP in **Contoso-DNS-EU**.
2. Öffnen Sie Windows PowerShell-Konsole, oder im Eingabeaufforderungsfenster.
3. Führen Sie **Ipconfig**, und notieren Sie sich **Verbindung-spezifische DNS-Suffix**.
4. Schließen Sie die Sitzung RDP nicht, Sie werden immer noch benötigen. 
5. Wiederholen Sie diese Schritte, um die **Verbindung-spezifische DNS-Suffix** von **Contoso-DNS-US**herauszufinden.


**So konfigurieren Sie die DNS-Weiterleitung**
 
1.  Klicken Sie von der RDP-Sitzung zu **Contoso-DNS-EU**auf die Windows-Taste auf der unteren linken Ecke.
2.  Klicken Sie auf **Verwaltung**.
3.  Klicken Sie auf **DNS**.
4.  Erweitern Sie im linken Bereich **DSN**, **Contoso-DNS-EU**aus.
5.  Geben Sie die folgenden Informationen ein:
    - **DNS-Domäne**: Geben Sie das DNS-Suffix der Contoso-DNS-USA. Beispiel: Contoso-DNS-US.b5.internal.cloudapp.net.
    - **IP-Adressen der master-Server**: 10.2.0.4, also den Contoso-DNS-US des IP-Adresse eingeben.
6.  Drücken Sie die **EINGABETASTE**, und klicken Sie dann auf **OK**.  Nun werden Sie die Contoso-DNS-US des IP-Adresse von Contoso-DNS-EU auflösen.
7.  Wiederholen Sie die Schritte zum Hinzufügen einer DNS-Weiterleitung an den DNS-Dienst auf den Contoso-DNS-US-virtuellen Computern mit den folgenden Werten aus:
    - **DNS-Domäne**: Geben Sie das DNS-Suffix der Contoso-DNS-EU. 
    - **IP-Adressen der master-Server**: 10.2.0.4, also den Contoso-DNS-EU IP-Adresse eingeben.

##<a name="test-the-name-resolution-across-the-virtual-networks"></a>Testen Sie die Auflösung über die virtuellen Netzwerke

Nun können Sie über die virtuellen Netzwerke Hostname-Auflösung testen. Ping wird standardmäßig von Firewall blockiert.  Nslookup können Sie den DNS-Server virtuellen Computern (Sie müssen vollqualifizierten Domänennamen verwenden) in den Peer-Netzwerken beheben.  


##<a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie gelernt namensauflösung über virtuelle Netzwerke mit VPN-Verbindungen zu konfigurieren. Die anderen zwei Artikeln der Reihe behandeln:

- [Konfigurieren einer VPN-Verbindungs zwischen zwei Azure virtuelle Netzwerke][hdinsight-hbase-geo-replication-vnet]
- [Konfigurieren der HBase Geo Replikation][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[powershell-install]: powershell-install-configure.md

[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-DNS/HDInsight.HBase.VPN.diagram.png 
