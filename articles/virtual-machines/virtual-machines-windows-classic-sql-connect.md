<properties
    pageTitle="Verbinden mit einer SQL Server virtuellen Computern (klassisch) | Microsoft Azure"
    description="Erfahren Sie, wie die Verbindung mit SQL Server auf einem virtuellen Computer in Azure ausgeführt. In diesem Thema wird das Bereitstellungsmodell klassischen verwendet. Die folgenden Szenarien variieren je nach der Netzwerkkonfiguration und den Speicherort des Clients."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Verbinden Sie mit einer SQL Server virtuellen Computern auf Azure (klassische Bereitstellung)

> [AZURE.SELECTOR]
- [Ressourcenmanager](virtual-machines-windows-sql-connect.md)
- [Klassische](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>(Übersicht)

In diesem Thema beschrieben, wie die Verbindung zu Ihrer SQL Server-Instanz für eine Azure-virtuellen Computern ausgeführt wird. Einige [allgemeine Konnektivität Szenarien](#connection-scenarios) behandelt, und klicken Sie dann enthält die [detaillierten Schritte für das Konfigurieren von SQL Server-Konnektivität auf eine Azure-virtuellen Computer](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Wenn Sie Ressourcenmanager virtuelle Computer verwenden, finden Sie unter [Verbinden zu einer SQL Server virtuellen Computern auf Azure Ressourcenmanager verwenden](virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Verbindungsszenarien

Die Möglichkeit, die ein Client mit SQL Server auf einem virtuellen Computer her unterscheidet sich je nach der Position des Clients und den Computer/Netzwerkkonfiguration. Diese Szenarios umfassen:

- [Verbinden Sie mit SQL Server in der gleichen Cloud-Dienst](#connect-to-sql-server-in-the-same-cloud-service)
- [Verbinden Sie mit SQL Server über das internet](#connect-to-sql-server-over-the-internet)
- [Verbinden Sie mit SQL Server in der gleichen virtuellen Netzwerk](#connect-to-sql-server-in-the-same-virtual-network)

>[AZURE.NOTE] Bevor Sie mit einer der folgenden Methoden verbinden, müssen Sie die [Schritte in diesem Artikel zum Konfigurieren der Konnektivität](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)befolgen.

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Verbinden Sie mit SQL Server in der gleichen Cloud-Dienst

Mehrere virtuelle Computer können in der gleichen Cloud-Dienst erstellt werden. Dieses Szenario virtuellen Computern finden Sie unter [Herstellen einer virtuellen Computern mit einem virtuellen Netzwerk oder Cloud-Dienst](virtual-machines-windows-classic-connect-vms.md#connect-vms-in-a-standalone-cloud-service). Dieses Szenario ist, wenn ein Client auf einem virtuellen Computern versucht, die Verbindung mit SQL Server auf einem anderen virtuellen Computern in der gleichen Cloud-Dienst ausgeführt.

In diesem Szenario können Sie mithilfe der **Namen** (auch als **Computernamen** oder **Hostname** im Portal dargestellt) verbinden. Dies ist der Name, den Sie während der Erstellung der für den virtuellen Computer bereitgestellt. Beispielsweise wenn Sie den Namen Ihrer SQL VM **Mysqlvm**, konnte ein Client virtueller Computer in der gleichen Cloud-Dienst verwenden die folgende Verbindungszeichenfolge Verbindung:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Verbinden Sie mit SQLServer über das Internet

Wenn Sie zu Ihrer SQL Server-Datenbank-Engine aus dem Internet verbinden möchten, müssen Sie einen Endpunkt virtuellen Computern für eingehende TCP-Kommunikation zu erstellen. Dieser Konfigurationsschritt Azure weist eingehenden Verkehr auf TCP Port um einen TCP-, die den virtuellen Computer zugegriffen werden kann.

Wenn Sie über das Internet verbunden ist, müssen Sie die DNS-Name des virtuellen Computers und die virtuellen Computer Endpunkt Port-Nummer (weiter unten in diesem Artikel konfiguriert) verwenden. Klicken Sie zum Suchen der DNS-Name, navigieren Sie zu der Azure-Portal, und wählen Sie **virtuellen Computern (klassische)**. Wählen Sie dann Ihre virtuellen Computern aus. Der **DNS-Name** wird im Abschnitt **Übersicht** angezeigt.

Betrachten Sie beispielsweise eine klassische virtuellen Computern, die mit dem Namen **Mysqlvm** mit dem Namen DNS- **mysqlvm7777.cloudapp.net** und einen Endpunkt virtueller Computer **57500**aus. Einem ordnungsgemäß konfigurierten Connectivity vorausgesetzt können, die folgenden Verbindungszeichenfolge konnte verwendet werden, um des virtuellen Computers von überall darauf zugreifen im Internet:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Obwohl dies Konnektivität für Clients über das Internet ermöglicht, bedeutet aber nicht, dass jede Person mit SQL Server herstellen kann. Benötigen außerhalb Clients für den richtigen Benutzernamen und das Kennwort ein. Zur Erhöhung der Sicherheit verwenden Sie nicht den bekannten Anschluss 1433 für den öffentlichen virtuellen Computern Endpunkt fest. Und falls möglich, sollten Sie eine ACL hinzufügen gestatten Sie für Ihre Endpunkt Verkehr auf den Clients nur eingeschränkt. Hinweise zur Verwendung von ACLs mit Endpunkten sind finden Sie unter [Verwalten der ACL für einen Endpunkt](virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

>[AZURE.NOTE] Es ist wichtig zu beachten, dass, wenn Sie dieses Verfahren zum Kommunizieren mit SQL Server, alle ausgehende Daten aus Azure Datencenter normalen [Preisen für ausgehende Datenübertragung](https://azure.microsoft.com/pricing/details/data-transfers/)unterliegt verwenden.

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Verbinden Sie mit SQL Server in der gleichen virtuellen Netzwerk

[Virtuelle Netzwerk](../virtual-network/virtual-networks-overview.md) ermöglicht zusätzliche Szenarios an. Sie können in der gleichen virtuellen Netzwerk virtuellen Computern herstellen, selbst wenn diese virtuellen Computern in anderen Cloud Services vorhanden. Und Sie können mit einem [Website-zu-Standort VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)eine Hybrid Architektur, die virtuelle Computer mit dem lokalen Netzwerken und Computern verbindet, erstellen.

Virtuelle Netzwerke können Sie auch Ihre Azure-virtuellen Computern zu einer Domäne hinzufügen. Dies ist die einzige Möglichkeit zum Verwenden der Windows-Authentifizierung für SQL Server. Die anderen Verbindungsszenarios erfordern SQL-Authentifizierung mit Benutzernamen und Kennwörter an.

Wenn Sie eine Domäne Umgebung und die Windows-Authentifizierung konfigurieren möchten, müssen Sie nicht die Schritte in diesem Artikel verwenden, um den öffentlichen Endpunkt oder SQL-Authentifizierung und Benutzernamen zu konfigurieren. In diesem Szenario können Sie zu Ihrer SQL Server-Instanz verbinden, indem Sie den Namen des SQL Server virtuellen Computers in der Verbindungszeichenfolge angeben. Im folgende Beispiel wird davon ausgegangen, dass die Windows-Authentifizierung auch konfiguriert wurde und der Benutzer Zugriff auf die SQL Server-Instanz gewährt wurde.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Schritte zum Konfigurieren von SQL Server-Konnektivität auf eine Azure-virtuellen Computer

Herstellen einer Verbindung mit SQL Server-Instanz über das Internet mithilfe von SQL Server Management Studio (SSMS) führen Sie die folgenden Schritten vor. Zugänglich Ihre SQL Server-virtuellen Computern für die Anwendung, die sowohl lokalen ausgeführt und in Azure gilt jedoch dieselben Schritte aus.

Bevor Sie mit der SQL Server-Instanz aus einem anderen virtuellen Computers oder im Internet verbinden können, müssen Sie die folgenden Aufgaben abgeschlossen haben, wie in den Abschnitten beschrieben, die folgen:

- [Erstellen Sie einen TCP-Endpunkt des virtuellen Computers](#create-a-tcp-endpoint-for-the-virtual-machine)
- [Öffnen Sie in der Windows-Firewall TCP ports](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [Konfigurieren von SQL Server, um das TCP-Protokoll Abhören](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Konfigurieren von SQL Server für gemischten Authentifizierungsmodus](#configure-sql-server-for-mixed-mode-authentication)
- [Erstellen von SQL Server-Authentifizierung Benutzernamen](#create-sql-server-authentication-logins)
- [Bestimmen Sie den DNS-Namen des virtuellen Computers](#determine-the-dns-name-of-the-virtual-machine)
- [Verbinden Sie mit der Datenbank-Engine von einem anderen computer](#connect-to-the-database-engine-from-another-computer)

Der Verbindungspfad wird durch das folgende Diagramm zusammengefasst:

![Herstellen einer Verbindung mit einer SQL Server-virtuellen Computern](../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic Steps](../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie auch planen, AlwaysOn Verfügbarkeit Gruppen für hohe Verfügbarkeit und Wiederherstellung verwenden, sollten Sie eine Zuhörer implementieren. Datenbank-Clients stellen eine Verbindung mit dem Zuhörer statt direkt in eine SQL Server-Instanzen. Die Zuhörer leitet Clients an den Primärschlüssel in der Gruppe Verfügbarkeit. Weitere Informationen finden Sie unter [Konfigurieren einer ILB Zuhörer für AlwaysOn Verfügbarkeit von Gruppen in Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Es ist wichtig, alle best Practices für die Sicherheit für SQL Server ausgeführt wird, klicken Sie auf eine Azure-virtuellen Computern überprüfen. Weitere Informationen finden Sie unter [Sicherheitsaspekte für SQL Server in Azure virtuellen Computern](virtual-machines-windows-sql-security.md).

[Durchsuchen der Learning Path](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) für SQL Server auf Azure-virtuellen Computern. 

Weitere Themen im Zusammenhang mit dem SQL Server in Azure-virtuellen Computern ausgeführt wird finden Sie unter [SQL Server auf Azure virtuellen Computern](virtual-machines-windows-sql-server-iaas-overview.md).
