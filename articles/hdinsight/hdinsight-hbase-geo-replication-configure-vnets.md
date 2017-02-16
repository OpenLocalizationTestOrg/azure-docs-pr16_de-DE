<properties 
   pageTitle="Konfigurieren von VPN-Verbindung zwischen zwei virtuelle Netzwerke | Microsoft Azure" 
   description="Informationen zum Konfigurieren von VPN-Verbindungen und Auflösung des Domänennamens zwischen zwei Azure virtuelle Netzwerke und HBase Geo-Replikation konfigurieren." 
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

# <a name="configure-a-vpn-connection-between-two-azure-virtual-networks"></a>Konfigurieren einer VPN-Verbindungs zwischen zwei Azure virtuelle Netzwerke  

> [AZURE.SELECTOR]
- [Konfigurieren von VPN-Konnektivität](hdinsight-hbase-geo-replication-configure-vnets.md)
- [Konfigurieren von DNS](hdinsight-hbase-geo-replication-configure-dns.md)
- [Konfigurieren der Replikation HBase](hdinsight-hbase-geo-replication.md) 

Azure virtuelle zwischen Standorten Netzwerkkonnektivität verwendet einen VPN-Gateway, um einen sicheren Tunnel mit IPSec-/IKE bereitzustellen. Die VNets kann in anderen Abonnements und verschiedener Regionen sein. Sie können auch VNet auf VNet Kommunikation mit mehreren Standorten Konfigurationen kombinieren. Es gibt verschiedene Gründe für VNet zu VNet Connectivity ein:

- Cross Region Geo-Redundanz und Geo-Anwesenheitsstatus 
- Regionale mit mehreren Ebenen Applikationen mit signifikante Isolation Begrenzungslinie 
- Cross zwischen Organisationen Kommunikation in Azure-Abonnement

Weitere Informationen finden Sie unter [Konfigurieren einer VNet zu VNet Verbindung](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). 

Klicken Sie auf Video angezeigt:

> [AZURE.VIDEO configure-the-vpn-connectivity-between-two-azure-virtual-networks]

In diesem Lernprogramm wird ein Teil der [Serie] [ hdinsight-hbase-replication] HBase Geo-Replikation erstellen. 

- Konfigurieren eines VPN-Konnektivität zwischen zwei virtuelle Netzwerke (in diesem Lernprogramm)
- [Konfigurieren von DNS für die virtuellen Netzwerke][hdinsight-hbase-geo-replication-dns]
- [Konfigurieren der HBase Geo Replikation][hdinsight-hbase-geo-replication]

Das folgende Diagramm veranschaulicht die beiden virtuelle Netzwerke, die Sie in diesem Lernprogramm erstellen:

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


>[AZURE.NOTE] Azure Service und virtuellen Computern Names müssen eindeutig sein. Der Name in diesem Lernprogramm wird Contoso-[Name der Azure Service/virtueller Computer]-[EU / US]. Contoso-VNet-EU beträgt beispielsweise das Azure virtuelle Netzwerk in Europa North Data Center; Contoso-DNS-US ist der DNS-Server virtueller Computer im südostasiatischen US Datencenter. Sie müssen den Namen Ihrer eigenen ergibt.
 

##<a name="create-two-azure-vnets"></a>Erstellen Sie zwei Azure VNets



**So erstellen ein virtuelles Netzwerk Contoso-VNet-EU in Nordamerika-Europa bezeichnet**

1.  Melden Sie sich bei der [Azure klassischen Portal][azure-portal].
2.  Klicken Sie auf **neu**, **NETWORK SERVICES**, **virtuellen Netzwerk**, **benutzerdefinierte erstellen**.
3.  Geben Sie ein:

    - **NAME**: Contoso-VNet-EU
    - **Standort**: North Europa

        In diesem Lernprogramm verwendet North Europe und ostasiatischen US-Rechenzentren. Sie können eigene Rechenzentren auswählen.
4.  Geben Sie ein:

    - **DNS-SERVER**: (lassen Sie ihn leer) 
    
        Sie benötigen einen eigenen DNS-Server für die Auflösung von Namen in virtuelle Netzwerke. Weitere Informationen darüber, wann verwenden Azure bereitgestellten namensauflösung und Ihre eigenen DNS-Server verwenden finden Sie unter [Name Auflösung (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md). So konfigurieren Sie namensauflösung zwischen VNets Anweisungen finden Sie unter [Konfigurieren von DNS-Einträge zwischen zwei Azure virtuelle Netzwerke][hdinsight-hbase-dns].
  
    - **Konfigurieren eines Punkt-zu-Standort VPN**: (deaktiviert)

        Für dieses Szenario nicht Punkt-zu-Standort gelten.

    - **Konfigurieren einer Standort-zu-Standort VPN**: (deaktiviert)
    
        Konfigurieren Sie die Website-zu-Standort VPN-Verbindung mit dem Azure virtuellen Netzwerk im südostasiatischen US Datencenter.
5.  Geben Sie ein:

    -   **Adresse Leerzeichen Felder starten IP**: 10.1.0.0
    -   **Adresse Leerzeichen CIDR**: / 16
    -   **Starten der Subnetz-1 IP**: 10.1.0.0
    -   **CIDR Subnetz-1**: / 24

    Die Adresse Leerzeichen kann nicht mit dem virtuelle US-Netzwerk überschneiden.  

**So erstellen ein virtuelles Netzwerk Contoso-VNet-EU in Europa-"Westen" bezeichnet**

- Wiederholen Sie den letzten Vorgang mit den folgenden Werten aus:

    - **NAME**: Contoso-VNet-US
    - **Standort**: ostasiatischen US
     
    - **DNS-SERVER**: (lassen Sie ihn leer)
    - **Konfigurieren eines Punkt-zu-Standort VPN**: (deaktiviert)
    - **Konfigurieren einer Standort-zu-Standort VPN**: (deaktiviert)
     
    - **Adresse Leerzeichen Felder starten IP**: 10.2.0.0
    - **Adresse Leerzeichen CIDR**: / 16
    - **Starten der Subnetz-1 IP**: 10.2.0.0
    - **CIDR Subnetz-1**: / 24

















##<a name="configure-a-vpn-connection-between-the-two-vnets"></a>Konfigurieren einer VPN-Verbindungs zwischen den beiden VNets

###<a name="create-local-networks"></a>Erstellen von lokalen Netzwerken

Beim Erstellen eines VNet an VNet-Konfiguration, müssen Sie jede VNet um miteinander erkennen, wie ein lokales Netzwerk-Website konfigurieren. In diesem Abschnitt werden Sie jede VNet als ein lokales Netzwerk konfigurieren. Die lokale Netzwerke freigeben die gleichen IP-Adresse Leerzeichen mit den entsprechenden VNet.

![Konfigurieren von Azure VPN Standorten Konfigurations - Azure lokalen Netzwerken][img-vnet-lnet-diagram]


**So erstellen ein lokales Netzwerk namens Contoso-LNet-EU entsprechen den Contoso-VNet-EU-Netzwerk Adresse Abstand**

1. Klicken Sie im klassischen Azure-Portal auf **neu**, **NETWORK SERVICES**, **Virtuelle Netzwerk**, **Lokalen Netzwerk hinzufügen**.
3. Geben Sie ein:

    - **NAME**: Contoso-LNet-EU
    - **VPN-Gerät IP-Adresse**: 192.168.0.1 (diese Adresse später aktualisiert wird)

        In der Regel, verwenden Sie die tatsächliche externe IP-Adresse für ein VPN-Gerät. Für VNet VNet Konfigurationen verwenden Sie die Option VPN Gateway IP-Adresse. Vorausgesetzt, dass Sie nicht die VPN-Gateways für die zwei VNets noch erstellt haben, geben Sie eine Arbitary IP-Adresse ein, und Sie wieder zum beheben.
4.  Geben Sie ein:

    - **Adresse Leerzeichen Felder starten IP:** 10.1.0.0
    - **Adresse Leerzeichen CIDR:** /16
    
    Dies muss genau auf den Bereich entsprechen, die Sie zuvor für Contoso-VNet-EU angegeben haben.

**So erstellen ein lokales Netzwerk namens Contoso-LNet-US entsprechen den Contoso-VNet-US-Netzwerk Adresse Abstand**

- Wiederholen Sie den letzten Vorgang mit den folgenden Parametern aus:

    - **NAME**: Contoso-LNet-US
    - **VPN-Gerät IP-Adresse**: 192.168.0.1 (diese Adresse später aktualisiert wird)
     
    - **Adresse Leerzeichen Felder starten IP**: 10.2.0.0
    - **Adresse Leerzeichen CIDR**: / 16


###<a name="create-vpn-gateways"></a>Erstellen von VPN-gateways

Es gibt zwei Teilen in dieser Konfiguration aus. Zuerst konfigurieren Sie eine VNet Standorten Verbindung zu einem lokalen Netzwerk, und klicken Sie dann erstellen Sie eine dynamische Weiterleitung VPN. VNet zu VNet ist Azure VPN-Gateways mit dynamischen Weiterleitung VPN erforderlich. Azure statische routing VPN werden nicht unterstützt.

**So konfigurieren Sie die Contoso-VNet-EU Standorten Verbindung zu Contoso-LNet-US**

1.  Vom klassischen Azure-Portal klicken Sie im linken Bereich auf **Netzwerke** ,
2.  Klicken Sie auf **Contoso-VNet-EU**.
3.  Klicken Sie auf der Registerkarte **CONFIGUE** .
4.  Überprüfen Sie **mit dem lokalen Netzwerk verbinden**.
5.  Wählen Sie im **Lokalen Netzwerk** **Contoso-LNet-US**ein.
6.  Klicken Sie auf **Gateway-Subnetz hinzufügen** im Abschnitt Leerzeichen Adresse virtuelles Netzwerk.
7.  Klicken Sie auf **Speichern**.
8.  Klicken Sie auf **OK** , um zu bestätigen.


**So erstellen einen VPN-Gateway für Contoso-VNet-EU**

1.  Klicken Sie im klassischen Azure-Portal auf die Registerkarte **DASHBOARD** .
4.  Klicken Sie auf **GATEWAY zu erstellen** , klicken Sie auf den unteren Rand der Seite, und klicken Sie dann auf **Dynamisches Routing**.
5.  Klicken Sie auf **Ja,** um zu bestätigen. Beachten Sie die Grafik Gateways auf der Seite ändert sich in Gelb und besagt Gateway erstellen. In der Regel nimmt etwa 15 Minuten für das Gateway zu erstellen.

    Wenn der gatewaystatus zum Herstellen einer Verbindung verwandelt hat, werden die IP-Adresse für jedes Gateway im Dashboard angezeigt. Notieren Sie sich die IP-Adresse, die entspricht zu jeder VNet, sorgen, nicht, um diese zu mischen nach oben. Hierbei handelt es sich um die IP-Adressen, die verwendet werden, wenn Sie Ihre IP-Adressen Platzhalter für das Gerät VPN in lokalen Netzwerken bearbeiten.

6.  Erstellen Sie eine Kopie des **GATEWAY IP-Adresse**ein. Sie verwenden sie so konfigurieren Sie die Option VPN Gateway IP-Adresse für Contoso-VNet-EU im nächsten Abschnitt.

**So erstellen einen VPN-Gateway für Contoso-VNet-EU**

- Wiederholen Sie die letzten beiden Schritte aus, um die Contoso-VNet-US-Standorten Konnektivität Contoso-LNet-EU, und das Erstellen eines Gateways VPN für Contoso-Vnet-US konfigurieren. Wenn Sie fertig sind, haben Sie die Option VPN Gateway IP-Adresse für Contoso-VNet-US.


### <a name="set-the-vpn-device-ip-addresses-for-local-networks"></a>Legen Sie die Option VPN Gerät IP-Adressen für lokale Netzwerke
Im letzten Abschnitt erstellen Sie einen VPN-Gateway für jede der VNets aus. Sie haben die IP-Adressen der Gateways VPN erhalten haben. Jetzt können Sie zurückkehren lokales Netzwerk VPN-Gerät IP-Adressen konfigurieren.

**So konfigurieren Sie die Option VPN Gerät IP-Adresse für Contoso-LNet-EU** 

1.  Vom klassischen Azure-Portal klicken Sie im linken Bereich auf **Netzwerke** .
2.  Klicken Sie oben auf **Lokale Netzwerke** .
3.  Klicken Sie auf **Contoso-LNet-EU**, und klicken Sie dann unten auf **Bearbeiten** .
4.  Aktualisieren Sie **VPN-Gerät IP-Adresse**ein.  Dies ist die Adresse, die Sie von der Registerkarte ' DASHBOARD ' Contoso-VNET-EU zu gelangen.
5.  Klicken Sie mit der rechten Maustaste auf.
6.  Klicken Sie auf die Schaltfläche überprüfen.

**So konfigurieren Sie die Option VPN Gerät IP-Adresse für Contoso-LNet-US** 

- Wiederholen der letzten Schritte aus, um die Option VPN Gerät IP-Adresse für Contoso-LNet-US konfigurieren.

###<a name="set-vnet-gateway-keys"></a>Legen Sie VNet Gateway Schlüssel

Der Gateways Vnet mithilfe eines freigegebenes Keys authentifiziert Verbindungen zwischen den virtuellen Netzwerken. Der Schlüssel kann nicht aus dem klassischen Azure-Portal konfiguriert werden. Sie müssen PowerShell oder .NET SDK verwenden.

**Zum Festlegen der Schlüssel**

1. Öffnen Sie von Ihrem Computer **Windows PowerShell ISE** oder der Windows PowerShell-Konsole aus.
2. Aktualisieren Sie die Parameter in diesem Skript folgen, und führen Sie es aus:

        Add-AuzreAccount
        Select-AzureSubscription -[AzureSubscriptionName]
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-EU -LocalNetworkSiteName Contoso-LNet-US -SharedKey A1b2C3D4
        Set-AzureVNetGatewayKey -VNetName ContosoVNet-US -LocalNetworkSiteName Contoso-LNet-EU -SharedKey A1b2C3D4 


##<a name="check-the-vpn-connection"></a>Überprüfen Sie die Option VPN-Verbindung 

Ohne alle virtuellen Computern, auf die VNets bereitgestellt können virtuelle visuellen Netzplandiagramm der VNet Dashboardseite im klassischen Azure-Portal Sie den Verbindungsstatus prüfen:

![HDInsight HBase Replikation virtuelles Netzwerk VPN-Verbindungsstatus][img-vpn-status]
  



##<a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie so konfigurieren Sie eine VPN-Verbindung zwischen zwei Azure virtuelle Netzwerke erhalten. Die anderen zwei Artikeln der Reihe behandeln:

- [Konfigurieren von DNS zwischen zwei Azure virtuelle Netzwerke][hdinsight-hbase-geo-replication-dns]
- [Konfigurieren der HBase Geo Replikation][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com

[powershell-install]: ../install-configure-powershell.md



[hdinsight-hbase-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-dns]: hdinsight-hbase-geo-replication-configure-dns.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-diagram.png
[img-vnet-lnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-lnet-diagram.png
[img-vpn-status]: ./media/hdinsight-hbase-geo-replication-configure-vnets/hdinsight-hbase-vpn-status.png 
