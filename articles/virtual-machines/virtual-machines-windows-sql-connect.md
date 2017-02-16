<properties
    pageTitle="Verbinden mit einer SQL Server virtuellen Computern (Ressourcenmanager) | Microsoft Azure"
    description="Erfahren Sie, wie die Verbindung mit SQL Server auf einem virtuellen Computer in Azure ausgeführt. In diesem Thema wird das Bereitstellungsmodell klassischen verwendet. Die folgenden Szenarien variieren je nach der Netzwerkkonfiguration und den Speicherort des Clients."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a>Verbinden Sie mit einer SQL Server virtuellen Computern auf Azure (Ressourcen-Manager)

> [AZURE.SELECTOR]
- [Ressourcenmanager](virtual-machines-windows-sql-connect.md)
- [Klassische](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>(Übersicht)

In diesem Thema beschrieben, wie die Verbindung zu Ihrer SQL Server-Instanz für eine Azure-virtuellen Computern ausgeführt wird. Einige [allgemeine Konnektivität Szenarien](#connection-scenarios) behandelt, und klicken Sie dann enthält die [detaillierten Schritte für das Konfigurieren von SQL Server-Konnektivität auf eine Azure-virtuellen Computer](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell. Zum Anzeigen der klassischen Version der in diesem Artikel finden Sie unter [Verbinden zu einer SQL Server virtuellen Computern auf klassische Azure](virtual-machines-windows-classic-sql-connect.md).

Wenn Sie lieber eine vollständige Verwendung der Funktion bereitgestellt und Verbindungsproblemen fungieren sollen, finden Sie in [einer SQL Server virtuellen Computern auf Azure bereitgestellt](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Verbindungsszenarien

Die Möglichkeit, die ein Client mit SQL Server auf einem virtuellen Computer her unterscheidet sich je nach der Position des Clients und den Computer/Netzwerkkonfiguration. Diese Szenarios umfassen:

- [Verbinden Sie mit SQL Server über das internet](#connect-to-sql-server-over-the-internet)
- [Verbinden Sie mit SQL Server in der gleichen virtuellen Netzwerk](#connect-to-sql-server-in-the-same-virtual-network)

### <a name="connect-to-sql-server-over-the-internet"></a>Verbinden Sie mit SQLServer über das Internet

Wenn Sie zu Ihrer SQL Server-Datenbank-Engine aus dem Internet verbinden möchten, es gibt mehrere Schritte erforderlich, z. B. Konfigurieren der Firewall, SQL-Authentifizierung aktivieren und Konfigurieren von Ihrem Netzwerk-Sicherheitsgruppe müssen Sie eine Sicherheitsgruppe Netzwerk Regel TCP-Datenverkehr auf Port 1433 zulässt verfügen.

Wenn Sie im Portal verwenden, um ein Bild der SQL Server-virtuellen Computern mit Ressourcenmanager bereitzustellen, sind diese Schritte für Sie erledigt Wenn Sie die SQL-Konnektivität Option **öffentlichen** auswählen:

![Öffentliche SQL Connectivity Option während der Bereitstellung](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Wenn dies nicht eine während bereitgestellt wurde, können Sie manuell SQL Server und Ihre virtuellen Computer konfigurieren anhand der [Schritte in diesem Artikel zum manuellen Konfigurieren von Konnektivität](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

>[AZURE.NOTE] Das Bild virtuellen Computern für SQL Server Express Edition wird das Protokoll TCP/IP nicht automatisch aktiviert. Für Express-Edition müssen Sie SQL Server-Konfigurations-Manager [das Protokoll TCP/IP manuell](#configure-sql-server-to-listen-on-the-tcp-protocol) aktivieren nach dem Erstellen des virtuellen Computer verwenden.

Nachdem dies erfolgt ist, kann alle Client mit Zugriff auf das Internet zur SQL Server-Instanz entweder die öffentliche IP-Adresse des virtuellen Computers oder die DNS-Bezeichnung zugewiesen an diese IP-Adresse angeben verbinden. Ist der SQL Server-Port 1433, brauchen Sie dies in der Verbindungszeichenfolge angeben.

    "Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Obwohl dies Konnektivität für Clients über das Internet ermöglicht, bedeutet aber nicht, dass jede Person mit SQL Server herstellen kann. Benötigen außerhalb Clients für den richtigen Benutzernamen und das Kennwort ein. Zur Erhöhung der Sicherheit können Sie den bekannten Anschluss 1433 vermeiden. Wenn Sie SQL Server zum Abhören Ports 1500 konfiguriert und geeignete Firewall und Netzwerk Sicherheit Gruppe Regeln hergestellt, konnte Sie beispielsweise, indem Sie die Nummer des Ports an den Servernamen wie im folgenden Beispiel anfügen verbinden:

    "Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

>[AZURE.NOTE] Es ist wichtig zu beachten, dass, wenn Sie dieses Verfahren zum Kommunizieren mit SQL Server, alle ausgehende Daten aus Azure Datencenter normalen [Preisen für ausgehende Datenübertragung](https://azure.microsoft.com/pricing/details/data-transfers/)unterliegt verwenden.

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Verbinden Sie mit SQL Server in der gleichen virtuellen Netzwerk

[Virtuelle Netzwerk](../virtual-network/virtual-networks-overview.md) ermöglicht zusätzliche Szenarios an. Sie können virtuellen Computern in der gleichen virtuellen Netzwerk herstellen, selbst wenn diese virtuellen Computern in anderen Ressourcengruppen vorhanden. Und Sie können mit einem [Website-zu-Standort VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md)eine Hybrid Architektur, die virtuelle Computer mit dem lokalen Netzwerken und Computern verbindet, erstellen.

Virtuelle Netzwerke können Sie auch Ihre Azure-virtuellen Computern zu einer Domäne hinzufügen. Dies ist die einzige Möglichkeit zum Verwenden der Windows-Authentifizierung für SQL Server. Die anderen Verbindungsszenarios erfordern SQL-Authentifizierung mit Benutzernamen und Kennwörter an.

Wenn Sie im Portal verwenden, um ein Bild der SQL Server-virtuellen Computern mit Ressourcenmanager bereitzustellen, sind die geeignete Firewall-Regeln für die Kommunikation über das virtuelle Netzwerk einrichten Wenn Sie die SQL-Konnektivität Option **als "Privat"** auswählen. Wenn dies nicht eine während bereitgestellt wurde, können Sie manuell SQL Server und Ihre virtuellen Computer konfigurieren anhand der [Schritte in diesem Artikel zum manuellen Konfigurieren von Konnektivität](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm). Aber wenn Sie planen, einer Domäne-Umgebung und die Windows-Authentifizierung konfigurieren, müssen Sie nicht die Schritte in diesem Artikel verwenden, um SQL-Authentifizierung und Benutzernamen zu konfigurieren. Sie auch muss nicht Netzwerk-Sicherheitsgruppe Regeln für den Zugriff über das Internet konfigurieren.

>[AZURE.NOTE] Das Bild virtuellen Computern für SQL Server Express Edition wird das Protokoll TCP/IP nicht automatisch aktiviert. Für Express-Edition müssen Sie SQL Server-Konfigurations-Manager [das Protokoll TCP/IP manuell](#configure-sql-server-to-listen-on-the-tcp-protocol) aktivieren nach dem Erstellen des virtuellen Computer verwenden.

Unter der Voraussetzung, dass Sie DNS-Einträge in Ihrem Netzwerk virtuelle konfiguriert haben, können Sie durch Angabe der SQL Server Computername in der Verbindungszeichenfolge zu Ihrer SQL Server-Instanz verbinden. Im folgende Beispiel wird davon ausgegangen, dass die Windows-Authentifizierung auch konfiguriert wurde und der Benutzer Zugriff auf die SQL Server-Instanz gewährt wurde.

    "Server=mysqlvm;Integrated Security=true"

Beachten Sie, dass in diesem Szenario die IP-Adresse für den virtuellen Computer auch angeben können.

## <a name="steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm"></a>Schritte zum manuellen Konfigurieren von SQL Server-Konnektivität auf eine Azure-virtuellen Computer

Manuelles setup-Verbindung zu SQL Server-Instanz und dann optional Herstellen einer Verbindung über das Internet mithilfe von SQL Server Management Studio (SSMS) führen Sie die folgenden Schritten vor. Beachten Sie, dass viele dieser Schritte für Sie fertig sind, wenn Sie die entsprechenden SQL Server-Konnektivitätsoptionen im Portal auswählen.

Bevor Sie mit der SQL Server-Instanz aus einem anderen virtuellen Computers oder im Internet verbinden können, müssen Sie die folgenden Aufgaben abgeschlossen haben, wie in den Abschnitten beschrieben, die folgen:

- [Öffnen Sie in der Windows-Firewall TCP ports](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [Konfigurieren von SQL Server, um das TCP-Protokoll Abhören](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [Konfigurieren von SQL Server für gemischten Authentifizierungsmodus](#configure-sql-server-for-mixed-mode-authentication)
- [Erstellen von SQL Server-Authentifizierung Benutzernamen](#create-sql-server-authentication-logins)
- [Konfigurieren einer Sicherheitsgruppe Netzwerk eingehenden Regel](#configure-a-network-security-group-inbound-rule-for-the-vm)
- [Konfigurieren eines DNS-Bezeichnung für die öffentliche IP-Adresse](#configure-a-dns-label-for-the-public-ip-address)
- [Verbinden Sie mit der Datenbank-Engine von einem anderen computer](#connect-to-the-database-engine-from-another-computer)

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager-nsg-rule.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Nächste Schritte

Bereitstellen von Anweisungen zusammen mit den Schritten Connectivity finden Sie unter [Bereitstellung eines SQL Server virtuellen Computers auf Azure](virtual-machines-windows-portal-sql-server-provision.md).

[Durchsuchen der Learning Path](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) für SQL Server auf Azure-virtuellen Computern.

Weitere Themen im Zusammenhang mit dem SQL Server in Azure-virtuellen Computern ausgeführt wird finden Sie unter [SQL Server auf Azure virtuellen Computern](virtual-machines-windows-sql-server-iaas-overview.md).
